---
title: 與遠端應用程式服務通訊
description: 使用專案 Rome 透過在遠端裝置上執行的應用程式服務交換訊息。
ms.assetid: a0261e7a-5706-4f9a-b79c-46a3c81b136f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp，連接的裝置，遠端系統，羅馬，專案羅馬，背景工作，app service
ms.localizationpriority: medium
ms.openlocfilehash: 779205a47b85cf9f9a0aec9db910b97995dc2cd8
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363911"
---
# <a name="communicate-with-a-remote-app-service"></a>與遠端應用程式服務通訊

除了使用 URI 在遠端裝置上啟動應用程式之外，您也可以在遠端裝置上執行 *應用程式服務* 並與之通訊。 任何 Windows 裝置都可用來做為用戶端或主機裝置。 這讓您不需將 app 帶到前景，就能以幾乎數目不拘的方式來與連接的裝置互動。

## <a name="set-up-the-app-service-on-the-host-device"></a>設定主機裝置上的 App 服務
您必須已經在遠端裝置上安裝 app 服務的提供者，才能在該裝置上執行該 app 服務。 本指南會使用 [Windows 通用範例儲存機制](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)上的 CSharp 版[隨機數字產生器應用程式服務](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)。 如需如何撰寫您自己的 app 服務的指示，請參閱[建立和取用 App 服務](how-to-create-and-consume-an-app-service.md)。

不論您正在使用現成的應用程式服務或要撰寫自己的，都需要進行一些編輯，才能讓服務與遠端系統相容。 在 Visual Studio 中，移至應用程式服務提供者的專案 (在範例中名為「AppServicesProvider」)，然後選取其 _Package.appxmanifest_ 檔案。 以滑鼠右鍵按一下並選取 [ **View Code** ]，以查看檔案的完整內容。 在主要**應用程式**專案 (內建立**延伸**模組專案，如果已經存在) ，則尋找它。 然後建立 **延伸** 模組，將專案定義為 app service，並參考其父專案。

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap3:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

在 **AppService** 元素旁邊，新增 **SupportsRemoteSystems** 屬性：

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

若要使用此 **uap3** 命名空間中的專案，您必須在資訊清單檔的頂端加入命名空間定義（如果尚未存在）。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

然後，建立您的 app service 提供者專案，並將其部署至主機裝置 (s) 。

## <a name="target-the-app-service-from-the-client-device"></a>以用戶端裝置的 App 服務為目標
要從中呼叫遠端 app 服務的裝置，需要有具備遠端系統功能的 app。 這可加入至在主機裝置上提供 app 服務的相同 app (在這種情況下，您會在這兩個裝置上安裝相同的 app)，或在完全不同的 app 中加以實作。

本節的程式碼需要下列 **using** 陳述式，才能依現況執行：

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetUsings":::


您必須先將 [**AppServiceConnection**](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) 物件具現化，就如同您在本機呼叫應用程式服務一樣。 [建立和取用 App 服務](how-to-create-and-consume-an-app-service.md)中提供更多關於此處理序的詳細資料。 在這個範例中，當成目標的 app 服務是隨機數字產生器服務。

> [!NOTE]
> 其中假設您已經在會呼叫下列方法的程式碼內，利用一些方法取得 [RemoteSystem](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) 物件。 如需這要如何設定的指示，請參閱[啟動遠端應用程式](launch-a-remote-app.md)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetAppService":::

接下來，為預定的遠端裝置建立 [**RemoteSystemConnectionRequest**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) 物件。 然後用它針對該裝置開啟 **AppServiceConnection**。 請注意，為求簡潔，下列範例已大幅簡化錯誤處理和報告。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetRemoteConnection":::

此時，您應該針對遠端電腦上的應用程式服務開啟連線。

## <a name="exchange-service-specific-messages-over-the-remote-connection"></a>透過遠端連線交換服務特定訊息

接下來，您能以 [**ValueSet**](/uwp/api/windows.foundation.collections.valueset) 物件的形式，對服務傳送訊息和接收和接收服務的訊息 (如需詳細資訊，請參閱[建立和取用應用程式服務](how-to-create-and-consume-an-app-service.md))。 隨機數字產生器服務會利用索引鍵 `"minvalue"` 與 `"maxvalue"`，取得兩個整數做為輸入，隨機選取其範圍內的整數，然後以索引鍵 `"Result"` 將值傳回呼叫的處理序。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetSendMessage":::

現在您已連線到目標主機裝置上的 app 服務、在該裝置上執行操作，然後接收到送至用戶端裝置的回應資料。

## <a name="related-topics"></a>相關主題

[ (Project 羅馬的連線應用程式和裝置) 總覽](connected-apps-and-devices.md)  
[啟動遠端 App](launch-a-remote-app.md)  
[建立和使用應用程式服務](how-to-create-and-consume-an-app-service.md)  
[遠端系統 API 參考](/uwp/api/Windows.System.RemoteSystems)  
[遠端系統範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
