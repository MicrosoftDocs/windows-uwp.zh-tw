---
title: 背景工作的指導方針
description: 查看在您的應用程式中開發和執行同進程和跨進程背景工作的詳細指導方針。
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp、背景工作
ms.localizationpriority: medium
ms.openlocfilehash: b73568c5fb4bae6392051fedcd6ca3dea078a98d
ms.sourcegitcommit: 4491da3f509b1126601990a816c6eb301d35ecc6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "95416615"
---
# <a name="guidelines-for-background-tasks"></a>背景工作的指導方針


確保您的應用程式符合執行背景工作的需求。

## <a name="background-task-guidance"></a>背景工作指引

在開發背景工作時，以及在發佈應用程式之前，請考慮下列指引。

如果您使用背景工作在背景播放媒體，請參閱[在背景播放媒體](../audio-video-camera/background-audio.md)，以了解有關 Windows 10 版本 1607 中讓此操作更容易進行的改進功能詳細資訊。

**同處理序與跨處理序背景工作︰** Windows 10 (版本 1607) 引進的 [同處理序背景工作](create-and-register-an-inproc-background-task.md)可讓您在與前景應用程式相同的處理序中執行背景程式碼。 決定要使用同處理序或跨處理序的背景工作時，請考慮下列因素︰

|考量 | 影響 |
|--------------|--------|
|恢復功能   | 如果您的背景處理序在另一個處理序中執行，背景處理序毀損並不會關閉您的前景應用程式。 此外，如果背景活動執行超過執行時間限制，即使在您的應用程式內，也可以終止。 當前景和背景處理序不需要彼此通訊 (因為同處理序背景工作的主要優點之一，就是處理程間不需要通訊) 時，最好選擇將背景工作和前景應用程式的工作分開。 |
|簡潔    | 同處理序背景工作不需要跨處理序通訊，撰寫上也比較不複雜。  |
|可用的觸發程序 | 同處理序背景工作不支援下列觸發程序︰[DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) 和 **IoTStartupTask**。 |
|VoIP | 同處理序背景工作不支援在應用程式內啟用 VoIP 背景工作。 |  

**觸發程序執行個體的數目限制：** App 可以註冊某些觸發程序的執行個體數目有限制。 App 只能針對其每個執行個體註冊 [ApplicationTrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)、[MediaProcessingTrigger](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) 和 [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) 一次。 如果 App 超過此限制，註冊將會擲回例外狀況。

**CPU 配額：** 背景工作受限於其依據觸發程序類型所取得的實際執行使用時間量。 大部分的觸發程序有 30 秒的實際執行使用限制，而部分觸發程序擁有最多 10 分鐘的執行時間，以完成需要大量運算資源的工作。 背景工作應該要輕量，才能延長電池使用時間並為前景應用程式提供較佳的使用者體驗。 請參閱[使用背景工作支援應用程式](support-your-app-with-background-tasks.md)，以了解套用至背景工作的資源限制。

**管理背景工作：** 您的應用程式應該取得已登錄的背景工作清單、登錄進度與完成處理常式，並適當地處理這些事件。 您的背景工作類別應該報告進度、取消和完成。 如需詳細資訊，請參閱[處理已取消的背景工作](handle-a-cancelled-background-task.md)和[監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)。

**使用 [BackgroundTaskDeferral](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral)：** 如果背景工作類別執行非同步程式碼，請務必使用延遲。 否則，當 [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法在同進程背景工作) 的情況下傳回 (或 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) 方法時，您的背景工作可能會提前終止。 如需詳細資訊，請參閱[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)。

或者，要求一個延遲並使用 **async/await** 來完成非同步方法呼叫。 請在 **await** 方法呼叫後關閉延遲。

**更新應用程式資訊清單：**  對於在處理程序外執行的背景工作，請在應用程式資訊清單中宣告各個背景工作，連同其所使用的觸發程序類型。 否則您的應用程式將無法在執行階段註冊背景工作。

如果您有多個背景工作，請考慮是否應該先執行相同的主機程序，或分散到不同的主機處理程序。 如果您擔心一個背景工作失敗會擔擱其他背景工作，請分敗主機處理程序。  使用資訊清單設計工具中的 **資源群組** 項目，將背景工作分組到不同的主機的處理程序。 

若要設定 **資源群組**，請開啟 Package.appxmanifest 設計工具，選擇 **/[宣告/]**，並新增 **/[應用程式服務/]** 宣告：

![資源群組設定](images/resourcegroup.png)

請參閱[應用程式架構參考](/uwp/schemas/appxpackage/uapmanifestschema/element-application)以取得有關資源群組設定的詳細資訊。

與前景應用程式在相同處理序中執行的背景工作，不需要在應用程式資訊清單中宣告自己。 如需在資訊清單中宣告在跨處理序中執行的背景工作的詳細資訊，請參閱[在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)。

**準備應用程式更新︰** 如果要更新您的應用程式，請建立並註冊 **ServicingComplete** 背景工作 (請參閱 [SystemTriggerType](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)) 來取消註冊舊版應用程式的背景工作，並註冊新版的背景工作。 除了在前景內執行的內容以外，此時也很適合執行可能需要的應用程式更新。

**執行背景工作的要求：**

> **重要**  自 Windows 10 起，應用程式不再需要將位於鎖定畫面作為先決條件，也可執行背景工作。

通用 Windows 平台 (UWP) 應用程式可以在不釘選到鎖定畫面上的情況下，執行所有支援的工作類型。 不過，應用程式必須呼叫 [**GetAccessState**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) ，並檢查應用程式在背景中不會被拒絕執行。 確定 **GetAccessStatus** 不會傳回其中一個已拒絕的 [**BackgroundAccessStatus**](/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) 列舉。 例如，如果使用者已在裝置的設定中明確拒絕您應用程式的背景工作許可權，則此方法會傳回 **BackgroundAccessStatus DeniedByUser。**

如果您的應用程式在背景中被拒絕執行，您的應用程式應該呼叫 [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) ，並確保在註冊背景工作之前，不會拒絕回應。

如需使用者的背景活動與省電模式選項的詳細資訊，請參閱[最佳化背景活動](../debug-test-perf/optimize-background-activity.md)。 
## <a name="background-task-checklist"></a>背景工作檢查清單

*同時適用於同處理序和跨處理序的背景工作*

-   將背景工作與正確的觸發程序關聯。
-   新增條件以協助確認背景工作順利執行。
-   處理背景工作進度、完成和取消。
-   於應用程式啟動期間，重新登錄背景工作。 這可確保它們在第一次啟動應用程式時進行註冊。 它也提供了一種方式，偵測使用者是否 (在事件註冊失敗時) 已停用應用程式的背景執行功能。
-   檢查背景工作登錄是否有錯誤。 在適當狀況下，嘗試以不同的參數值再次登錄背景工作。
-   針對桌上型電腦以外的所有裝置系列，如果裝置的記憶體變成不足，背景工作可能就會終止。 如果沒有顯示記憶體不足的例外狀況，或是應用程式沒有處理該狀況，背景工作將會在沒有警告也沒有引發 OnCanceled 事件的情況下終止。 這有助於確保前景應用程式的使用者體驗。 您的背景工作應該要設計成能夠處理這種情況。

*僅適用於跨處理序背景工作*

-   在 Windows 執行階段元件中建立您的背景工作。
-   請勿顯示背景工作快顯通知、磚以及徽章更新以外的 UI。
-   在 [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法中，針對每個非同步方法呼叫要求延遲，並在方法完成時關閉它們。 或者，搭配 **async/await** 使用單一延遲。
-   使用永久性存放裝置在背景工作和應用程式之間分享資料。
-   在應用程式資訊清單中宣告每個背景工作，以及它所使用的觸發程序類型。 確認進入點與觸發程序類型是否正確。
-   除非您要使用應該與應用程式在相同內容中執行的觸發程序 (例如 [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger))，否則請勿在資訊清單中指定 Executable 元素。

*僅適用於同處理序背景工作*

- 取消工作時，請確認 `BackgroundActivated` 事件處理常式在取消發生或終止整個處理序之前結束。
-   撰寫短期背景工作。 大部分的背景工作限制為30秒的時鐘用量。


*應避免事項*
- 透過 COM 或 RPC 將處理序間通訊的使用降至最低。
-   您嘗試進行通訊的處理常式可能不會處於執行中狀態，因此可能會造成停止回應。
-   您可以花很長的時間來加速跨進程的通訊，而且會根據分配給執行背景工作的時間來計算。
- 不要依賴背景工作的使用者互動。


## <a name="related-topics"></a>相關主題

* [建立及註冊同處理序的背景工作](create-and-register-an-inproc-background-task.md)
* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [建立並註冊 winmain COM 背景工作](create-and-register-a-winmain-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [在背景播放媒體](../audio-video-camera/background-audio.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [註冊背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 UWP 應用程式觸發暫停、繼續和背景事件 (偵錯時)](/previous-versions/hh974425(v=vs.110))

 

 
