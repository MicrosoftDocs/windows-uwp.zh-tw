---
title: 偵錯背景工作
description: 了解如何偵錯背景工作，包括 Windows 事件記錄檔中的背景工作啟用和偵錯追蹤。
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp、背景工作
ms.localizationpriority: medium
ms.openlocfilehash: 5696d3c5ffb28ee8dc6ebd51e678894ee78ae420
ms.sourcegitcommit: e39b569626804d2ce4246353ac2c03a916dc9737
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192978"
---
# <a name="debug-a-background-task"></a>偵錯背景工作


**重要 API**
-   [Windows.ApplicationModel.Background](/uwp/api/Windows.ApplicationModel.Background)

了解如何偵錯背景工作，包括 Windows 事件記錄檔中的背景工作啟用和偵錯追蹤。

## <a name="debugging-out-of-process-vs-in-process-background-tasks"></a>對跨處理序與同處理序背景工作進行偵錯
本主題主要用來說明在與主控 App 不同處理序中執行的背景工作。 如果您正在對同處理序背景進行偵錯，您將不會有個別的背景工作專案，並可以在 **OnBackgroundActivated()** (同處理序背景程式碼執行所在的位置) 設定中斷點，請參閱下方位於[手動觸發背景工作以偵錯背景工作程式碼](#trigger-background-tasks-manually-to-debug-background-task-code)中的步驟 2，以取得如何觸發背景程式碼執行的指示。

## <a name="make-sure-the-background-task-project-is-set-up-correctly"></a>請確定已正確設定背景工作專案

本主題假設您已經有需要對其背景工作偵錯的 App。 下列項目適用於在跨處理序中執行的背景工作，且不適用於同處理序背景工作。

-   在 C# 與 C++ 中，確定主要專案參照背景工作專案。 如果此參照未就緒，背景工作將不會包含在應用程式套件中。
-   在 c # 和 c + + 中，請確定背景工作專案的 **輸出類型** 為「Windows 執行階段元件」。
-   背景類別必須在套件資訊清單的進入點屬性中進行宣告。

## <a name="trigger-background-tasks-manually-to-debug-background-task-code"></a>手動觸發背景工作以偵錯背景工作程式碼

您可以透過 Microsoft Visual Studio 手動觸發背景工作。 然後就可以逐步檢查程式碼並進行偵錯。

1.  在 C# 中，請在背景類別的 Run 方法中設置中斷點 (若為同處理序背景工作，請在 App.OnBackgroundActivated() 設置中斷點)，和/或使用 [**System.Diagnostics**](/dotnet/api/system.diagnostics) 來撰寫偵錯輸出。

    在 C++ 中，請在背景類別的 Run 類別中設置中斷點 (若為同處理序背景工作，請在 App.OnBackgroundActivated() 設置中斷點)，和/或使用 [**OutputDebugString**](/windows/desktop/api/debugapi/nf-debugapi-outputdebugstringw) 來撰寫偵錯輸出。

2.  在偵錯工具中執行您的應用程式，然後使用 [ **生命週期事件** ] 工具列觸發背景工作。 這個下拉式清單會顯示可由 Visual Studio 啟用的背景工作名稱。

    > [!NOTE]
    > 在 Visual Studio 中，預設不會顯示 [生命週期事件] 工具列選項。 若要顯示這些選項，請以滑鼠右鍵按一下 Visual Studio 中的目前工具列，並確定已啟用 [ **偵錯工具位置** ] 選項。

    若要能夠運作，背景工作必須已經註冊且必須仍在等候觸發程序。 例如，如果背景工作是以一次性的 TimeTrigger 註冊，且該觸發程序已經引發，則透過 Visual Studio 啟動工作將不會有作用。

    > [!Note]
    > 無法以這種方式將使用下列觸發程序的背景啟用：[**ApplicationTrigger**](/uwp/api/windows.applicationmodel.background.applicationtrigger)、[**MediaProcessingTrigger**](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger)、[**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)、[**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)，以及觸發程序類型為 [**SmsReceived**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 使用 [**SystemTrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) 的背景工作。  
    > **ApplicationTrigger** 與 **MediaProcessingTrigger** 可以使用 `trigger.RequestAsync()` 在程式碼中手動發送訊號。

    ![偵錯背景工作](images/debugging-activation.png)

3.  當背景工作啟用時，偵錯工具會連結到工作，並在 VS 顯示偵錯輸出。

## <a name="debug-background-task-activation"></a>偵錯背景工作啟用

> [!NOTE]
> 本節適用於在跨處理序中執行的背景工作，且不適用於同處理序背景工作。

背景工作啟用仰賴下列三要件：

-   背景工作類別的名稱與命名空間
-   在套件資訊清單中指定的進入點屬性
-   您的 app 在註冊背景工作時指定的進入點

1.  使用 Visual Studio 記下背景工作的進入點：

    -   在 C# 與 C++ 中，記下背景工作專案中所指定的背景工作類別的名稱與命名空間。

2.  使用資訊清單設計工具來檢查已在套件資訊清單中正確宣告背景工作：

    -   在 C# 與 C++ 中，進入點屬性必須符合類別名稱後的背景工作命名空間。 例如：RuntimeComponent1.MyBackgroundTask。
    -   也必須指定與工作搭配使用的所有觸發程序類型。
    -   除非您使用 [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 或 [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)，否則「切勿」指定可執行檔。

3.  僅限 Windows。 查看 Windows 使用的進入點來啟動背景工作、啟用偵錯追蹤，以及使用 Windows 事件記錄檔。

    如果您遵照此程序但事件日誌顯示背景工作發生錯誤進入點或觸發程序，表示您的 app 未正確註冊背景工作。 如需此工作的協助，請參閱[註冊背景工作](register-a-background-task.md)。

    1.  移至 [開始] 畫面並搜尋 eventvwr.exe，開啟事件檢視器。
    2.  **Application and Services Logs**  - &gt; **Microsoft**  - &gt; **Windows**  - &gt; **BackgroundTaskInfrastructure**在 [事件檢視器] 中，移至 [應用程式和服務記錄 Microsoft Windows BackgroundTaskInfrastructure]。
    3.  在 [動作] 窗格中 **，選取 [**  - &gt; **顯示分析和偵錯工具記錄**檔]，以啟用診斷記錄。
    4.  選取 **診斷記錄** ，然後按一下 [ **啟用記錄**]。
    5.  現在嘗試使用應用程式再次註冊並啟動背景工作。
    6.  檢視診斷記錄檔，以取得詳細的錯誤資訊。 當中包含為背景工作註冊的進入點。

![背景工作偵錯資訊的事件檢視器](images/event-viewer.png)

## <a name="background-tasks-and-visual-studio-package-deployment"></a>背景工作和 Visual Studio 套件部署

如果使用 Visual Studio 部署使用背景工作的應用程式，且資訊清單設計工具中指定的 (主要和/或次要) 版本接著更新，則使用 Visual Studio 重新部署應用程式可能會導致應用程式的背景工作停止。 這可以透過下列方式來修正：

-   執行與套件一起產生的指令碼，使用 Windows PowerShell (而非 Visual Studio) 來部署已更新的應用程式。
-   如果您已經使用 Visual Studio 部署應用程式，且其背景工作現在已停止，請重新開機或登出/登入，以讓應用程式的背景工作再次運作。
-   您可以選取 [永遠重新安裝我的封裝] 偵錯選項，避免在 C# 專案中發生這個狀況。
-   等到應用程式準備好進行最終部署，以遞增套件版本 (在) 偵錯工具時不會變更它。

## <a name="remarks"></a>備註

-   確認您的 app 會先檢查現有背景工作註冊，以免再次註冊背景工作。 多次登錄相同背景工作會在每次觸發背景工作時多次執行該背景工作，因而造成無法預期的結果。
-   如果背景工作需要鎖定畫面存取，請確定先將 app 置於鎖定畫面後，再嘗試偵錯背景工作。 如需為具有鎖定畫面功能的 App 指定資訊清單選項的詳細資訊，請參閱[在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)。
-   背景工作註冊參數是在註冊之時進行驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 請確認您的 App 能夠妥善處理背景工作註冊失敗的狀況；反之，如果 App 需依賴有效的驗證物件，則在嘗試註冊工作之後，可能會當機。

如需使用 VS 來偵測背景工作的詳細資訊，請參閱 [如何在 UWP 應用程式中觸發暫止、繼續及背景事件](/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)。

## <a name="related-topics"></a>相關主題

* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [建立及註冊同處理序的背景工作](create-and-register-an-inproc-background-task.md)
* [註冊背景工作](register-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [如何在 UWP App 中觸發暫停、繼續和背景事件](/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)
* [使用 Visual Studio 程式碼分析，分析 UWP 應用程式的程式碼品質](/visualstudio/test/analyze-the-code-quality-of-store-apps-using-visual-studio-static-code-analysis?view=vs-2015)

 

 