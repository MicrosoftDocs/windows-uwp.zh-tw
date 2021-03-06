---
title: 建立及註冊同處理序的背景工作
description: 建立和註冊與前景應用程式在相同處理序中執行的同處理序工作。
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10、uwp、背景工作
ms.assetid: d99de93b-e33b-45a9-b19f-31417f1e9354
ms.localizationpriority: medium
ms.openlocfilehash: 489de52a3c592bac9d715b679470b84c2af7e621
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155992"
---
# <a name="create-and-register-an-in-process-background-task"></a>建立及註冊同處理序的背景工作

**重要 API**

-   [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

本主題示範如何建立和註冊與您的 App 在相同處理序中執行的背景工作。

實作同處理序背景工作比實作跨處理序背景工作簡單。 不過，前者較不具彈性。 如果在同處理序背景工作中執行的程式碼損毀，將會關閉您的 App。 另請注意，[DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger)、[DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) 及 **IoTStartupTask** 無法與同處理序模型搭配使用。 此外，也不可能在應用程式內啟用 VoIP 背景工作。 使用跨處理序背景工作模型仍可支援這些觸發程序和工作。

請注意，如果背景活動在執行時超過執行時間限制，則即使它是在 App 的前景處理序內執行，也可能被終止。 就某些用途而言，將工作分散到在個別處理程序中執行的背景工作中仍然相當有用。 對於不需要與前景應用程式進行通訊的工作來說，將背景工作保持為與前景應用程式分開的工作可能是最佳選項。

## <a name="fundamentals"></a>基礎

同處理序模型藉由提供 App 進入前景或背景時的改良式通知，增強應用程式週期。 應用程式物件針對這些轉換提供兩個新的事件：[**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 和 [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)。 這些事件適用的應用程式週期取決於應用程式的可見度狀態。如需了解這些事件及它們如何影響應用程式週情，請參閱 [App 週期](app-lifecycle.md)。

大致說來，您將處理 **EnteredBackground** 事件來執行將在您 App 於背景中執行時執行的程式碼，以及處理 **LeavingBackground** 以在您 App 已移至前景時能夠得知。

## <a name="register-your-background-task-trigger"></a>註冊背景工作觸發程序

註冊同處理序背景活動的方式與註冊跨處理序背景活動的方式非常相似。 所有背景觸發程序都是先使用 [BackgroundTaskBuilder](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder?f=255&MSPPError=-2147217396) 進行註冊。 建立器可讓您在一個地方設定所有必要的值，讓您輕輕鬆鬆就可以登錄背景工作︰

> [!div class="tabbedCodeSnippets"]
> ```cs
> var builder = new BackgroundTaskBuilder();
> builder.Name = "My Background Trigger";
> builder.SetTrigger(new TimeTrigger(15, true));
> // Do not set builder.TaskEntryPoint for in-process background tasks
> // Here we register the task and work will start based on the time trigger.
> BackgroundTaskRegistration task = builder.Register();
> ```

> [!NOTE]
> 通用 Windows app 必須先呼叫 [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)，才能登錄任何背景觸發程序類型。
> 為了確保您的通用 Windows app 會在您發行更新之後繼續正常執行，您必須呼叫 [**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)，然後在 app 於更新後啟動時呼叫 [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

針對同處理序背景活動，您將不設定 `TaskEntryPoint.`。將它留白會啟用預設進入點，這是 Application 物件上一個受保護的新方法，稱為 [OnBackgroundActivated()](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)。

登錄觸發程序之後，它就會根據 [SetTrigger](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.settrigger) 方法中所設定的觸發程序類型引發。 在上述範例中，使用的是 [TimeTrigger](/uwp/api/windows.applicationmodel.background.timetrigger)，會在登錄它之後 15 分鐘引發。

## <a name="add-a-condition-to-control-when-your-task-will-run-optional"></a>新增條件來控制工作執行時機 (選擇性)

您可以新增條件來控制觸發程序事件發生後工作的執行時機。 例如，如果您希望等到使用者在場時才執行工作，可以使用 **UserPresent** 條件。 如需可用條件的清單，請參閱 [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)。

下列範例程式碼會指派條件，要求必須要有使用者：

> [!div class="tabbedCodeSnippets"]
> ```cs
> builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
> ```

## <a name="place-your-background-activity-code-in-onbackgroundactivated"></a>將您的背景活動程式碼放在 OnBackgroundActivated() 中

將您的背景活動程式碼放在 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) 中，以在引發時回應背景觸發程式。 **OnBackgroundActivated** 可以視同 [IBackgroundTask.Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396)。 方法具有 [BackgroundActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.backgroundactivatedeventargs) 參數，其中包含 **Run** 方法提供的所有專案。 例如，在 App.xaml.cs 中：

``` cs
using Windows.ApplicationModel.Background;

...

sealed partial class App : Application
{
  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      DoYourBackgroundWork(taskInstance);  
  }
}
```

如需更豐富的 **OnBackgroundActivated** 範例，請參閱 [轉換 app service 以在其主機應用程式的相同進程中執行](convert-app-service-in-process.md)。

## <a name="handle-background-task-progress-and-completion"></a>處理背景工作進度和完成

監視工作進度和完成的方式與監視多處理程序背景工作的方式相同 (請參閱[監視背景工作進度和完成](monitor-background-task-progress-and-completion.md))，但是您可能會發現透過在 App 中使用變數來追蹤進度或完成狀態更加容易。 這是讓背景活動在與 App 相同的處理程序中執行的其中一個優點。

## <a name="handle-background-task-cancellation"></a>處理背景工作取消

取消同處理序背景工作的方式與取消跨處理序背景工作的方式相同 (請參閱[處理已取消的背景工作](handle-a-cancelled-background-task.md))。 請注意，在發生取消之前，您的 **BackgroundActivated** 事件處理常式必須先結束，否則系統會終止整個處理程序。 如果您的前景應用程式在您取消背景工作時意外關閉，請確認您的處理常式是否在發生取消之前已結束。

## <a name="the-manifest"></a>資訊清單

與跨處理序背景工作不同，您不需要將背景工作資訊新增到套件資訊清單中，即可執行同處理序背景工作。

## <a name="summary-and-next-steps"></a>摘要和後續步驟

您現在應該對如何撰寫同處理序背景工作有基本的了解。

請參閱下列 API 參考的相關主題、背景工作概念指引，以及撰寫使用背景工作之 app 的更詳細說明。

## <a name="related-topics"></a>相關主題

**詳細的背景工作教學主題**

* [將跨處理序背景工作轉換成同處理序背景工作](convert-out-of-process-background-task.md)
* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [在背景播放媒體](../audio-video-camera/background-audio.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [註冊背景工作](register-a-background-task.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)

**背景工作指引**

* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 UWP 應用程式觸發暫停、繼續和背景事件 (偵錯時)](/previous-versions/hh974425(v=vs.110))

**背景工作 API 參考**

* [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)