---
title: 轉換應用程式服務，以便與其主控應用程式在相同處理序中執行
description: 將在個別背景處理序中執行的 app 服務程式碼，轉換成和您 app 服務提供者在相同處理序內執行的程式碼。
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10、uwp、app service
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
ms.localizationpriority: medium
ms.openlocfilehash: d1bdd8b72cafe3dd719f7ee1733e13e1531f8c7e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162712"
---
# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>轉換應用程式服務，以便與其主控應用程式在相同處理序中執行

[AppServiceConnection](/uwp/api/windows.applicationmodel.appservice.appserviceconnection) 可讓另一個應用程式喚醒您在背景中的 app，並開始與其直接通訊。

引進同處理序的應用程式服務後，兩個執行中的前景應用程式可以透過應用程式服務連線直接通訊。 應用程式服務現在可以和前景應用程式在相同的處理序中執行，讓 App 之間更容易通訊，而且不再需要將服務程式碼分成不同的專案。

需要兩處變更，才能將跨處理序模型的應用程式服務轉變成同處理序模型。 第一處是變更資訊清單。

> ```xml
> <Package
>    ...
>   <Applications>
>       <Application Id=...
>           ...
>           EntryPoint="...">
>           <Extensions>
>               <uap:Extension Category="windows.appService">
>                   <uap:AppService Name="InProcessAppService" />
>               </uap:Extension>
>           </Extensions>
>           ...
>       </Application>
>   </Applications>
> ```

`EntryPoint`從元素移除屬性， `<Extension>` 因為現在[OnBackgroundActivated ( # B1](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)是在叫用 app service 時所使用的進入點。

第二處變更是，將服務邏輯從其個別的背景工作專案移入可從 **OnBackgroundActivated()** 呼叫的方法中。

現在可以您的應用程式可以直接執行應用程式服務。 例如，在 App.xaml.cs 中：

[!NOTE] 下列程式碼與範例 1 (跨進程服務) 所提供的程式碼不同。 下列程式碼僅供說明之用，不應作為範例 2 (內含式服務) 的一部分。  若要繼續進行範例 1 (跨進程服務) 至範例 2 (同進程服務) 繼續使用提供的範例1的程式碼，而不是以下的說明程式碼。

``` cs
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
...

sealed partial class App : Application
{
  private AppServiceConnection _appServiceConnection;
  private BackgroundTaskDeferral _appServiceDeferral;

  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
      _appServiceDeferral = taskInstance.GetDeferral();
      taskInstance.Canceled += OnAppServicesCanceled;
      _appServiceConnection = appService.AppServiceConnection;
      _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
      _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
  }

  private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
  {
      AppServiceDeferral messageDeferral = args.GetDeferral();
      ValueSet message = args.Request.Message;
      string text = message["Request"] as string;

      if ("Value" == text)
      {
          ValueSet returnMessage = new ValueSet();
          returnMessage.Add("Response", "True");
          await args.Request.SendResponseAsync(returnMessage);
      }
      messageDeferral.Complete();
  }

  private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
  {
      _appServiceDeferral.Complete();
  }

  private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
  {
      _appServiceDeferral.Complete();
  }
}
```

在上述程式碼中，`OnBackgroundActivated` 方法負責處理應用程式服務啟用。 已登錄透過 [AppServiceConnection](/uwp/api/windows.applicationmodel.appservice.appserviceconnection) 通訊時所需的所有事件，並已儲存工作延遲物件，以便在應用程式間的通訊完成時，標示為完成。

當 app 收到要求，然後讀取提供的 [ValueSet](/uwp/api/windows.foundation.collections.valueset)，查看是否會出現 `Key` 和 `Value` 字串。 如果有的話，則應用程式服務會將一組 `Response` 和 `True` 字串值傳回 **AppServiceConnection** 另一端的 app。

在[建立和使用應用程式服務](./how-to-create-and-consume-an-app-service.md?f=255&MSPPError=-2147217396)，深入了解連線並與其他app 通訊。