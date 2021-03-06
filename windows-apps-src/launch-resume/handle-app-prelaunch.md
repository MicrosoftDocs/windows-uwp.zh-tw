---
title: 處理應用程式預先啟動
description: 了解如何透過覆寫 OnLaunched 方法及呼叫 CoreApplication.EnablePrelaunch(true) 來處理應用程式預先啟動。
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 219ca73115d5605f1e1483f2af224a13c28791ff
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164862"
---
# <a name="handle-app-prelaunch"></a>處理應用程式預先啟動

了解如何透過覆寫 [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 方法來處理 App 預先啟動。

## <a name="introduction"></a>簡介

當可用的系統資源允許時，會透過在背景主動啟動使用者最常使用的應用程式，來改善電腦裝置系列裝置上 UWP 應用程式的啟動效能。 預先啟動的 App 在啟動不久後就會立即進入暫停狀態。 然後，當使用者叫用 App 時，App 會從暫停狀態切換到執行中狀態來繼續執行，這比 App 冷啟動快。 使用者所體驗到的就是 App 啟動非常快速。

在 Windows 10 之前，App 並未自動利用預先啟動。 在 Windows 10 版本 1511 中，通用 Windows 平台 (UWP) app 已成為預先啟動的候選項目。 在 Windows 10 版本 1607 中，您必須透過呼叫 [CoreApplication.EnablePrelaunch(true)](/uwp/api/windows.applicationmodel.core.coreapplication.enableprelaunch)，才能加入預先啟動行為。 在 `OnLaunched()` 內靠近進行 `if (e.PrelaunchActivated == false)` 檢查的位置就是放置這個呼叫的好地方。

是否會預先啟動 App，取決於系統資源。 如果系統感到資源壓力，就不會預先啟動應用程式。

某些類型的 App 可能需要變更其啟動行為，才能搭配預先啟動正常運作。 例如，在啟動時播放音樂的 App、假設使用者在場並在應用程式啟動時顯示精美視覺效果的遊戲、在啟動期間變更使用者線上可見度的訊息中心 App，皆可以識別 App 何時已預先啟動並可變更其啟動行為，如下列各節所述。

XAML 專案 (C#、VB、C++) 及 WinJS 的預設範本皆納入了 Visual Studio 2015 Update 3 中的預先啟動。

## <a name="prelaunch-and-the-app-lifecycle"></a>預先啟動與 App 週期

應用程式在預先啟動之後，將會進入暫停狀態。 (請參閱[處理 app 暫停](suspend-an-app.md))。

## <a name="detect-and-handle-prelaunch"></a>偵測和處理預先啟動

App 在啟動期間會收到 [**LaunchActivatedEventArgs.PrelaunchActivated**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.prelaunchactivated) 旗標。 使用此旗標來執行程式碼，該程式碼只應在使用者明確啟動應用程式時執行，如下列 [**>onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched)修改所示。

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // CoreApplication.EnablePrelaunch was introduced in Windows 10 version 1607
    bool canEnablePrelaunch = Windows.Foundation.Metadata.ApiInformation.IsMethodPresent("Windows.ApplicationModel.Core.CoreApplication", "EnablePrelaunch");

    // NOTE: Only enable this code if you are targeting a version of Windows 10 prior to version 1607
    // and you want to opt-out of prelaunch.
    // In Windows 10 version 1511, all UWP apps were candidates for prelaunch.
    // Starting in Windows 10 version 1607, the app must opt-in to be prelaunched.
    //if ( !canEnablePrelaunch && e.PrelaunchActivated == true)
    //{
    //    return;
    //}

    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        // On Windows 10 version 1607 or later, this code signals that this app wants to participate in prelaunch
        if (canEnablePrelaunch)
        {
            TryEnablePrelaunch();
        }

        // TODO: This is not a prelaunch activation. Perform operations which
        // assume that the user explicitly launched the app such as updating
        // the online presence of the user on a social network, updating a
        // what's new feed, etc.

        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();
    }
}

/// <summary>
/// Encapsulates the call to CoreApplication.EnablePrelaunch() so that the JIT
/// won't encounter that call (and prevent the app from running when it doesn't
/// find it), unless this method gets called. This method should only
/// be called when the caller determines that we are running on a system that
/// supports CoreApplication.EnablePrelaunch().
/// </summary>
private void TryEnablePrelaunch()
{
    Windows.ApplicationModel.Core.CoreApplication.EnablePrelaunch(true);
}
```

請注意 `TryEnablePrelaunch()` 上述函數。 將呼叫帶出至此函式的原因 `CoreApplication.EnablePrelaunch()` 是，當呼叫方法時，JIT (即時編譯) 將會嘗試編譯整個方法。 如果您的應用程式是在不支援的 Windows 10 版本上執行 `CoreApplication.EnablePrelaunch()` ，則 JIT 將會失敗。 藉由將呼叫的方法，只會在應用程式判斷平臺所支援的情況 `CoreApplication.EnablePrelaunch()` 下呼叫，我們就可以避免這個問題。

上述範例中也有一些程式碼，如果您的應用程式在 Windows 10 1511 版上執行時需要退出預先啟動，您可以取消批註。 在1511版中，所有 UWP 應用程式都會自動進入預先啟動，這可能不適合您的應用程式。

## <a name="use-the-visibilitychanged-event"></a>使用 VisibilityChanged 事件

使用者看不到預先啟動所啟動的應用程式。 當使用者切換到它們時，才會顯示。 您可能會想要將某些操作延遲到應用程式主視窗顯示之後才進行。 例如，如果您的 App 會從摘要中顯示最新動向項目清單，您可以在 [**VisibilityChanged**](/uwp/api/windows.ui.xaml.window.visibilitychanged) 事件期間更新該清單，而不用使用預先啟動 App 時所建置的清單，因為等到使用者啟用 App 時該清單可能已過時。 下列程式碼會處理 **MainPage** 的 **VisibilityChanged** 事件：

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than
        // when it is prelaunched, such as building a what's new feed
    }
}
```

## <a name="directx-games-guidance"></a>DirectX 遊戲指導方針

DirectX 遊戲通常不應該啟用預先啟動，因為許多 DirectX 遊戲是在系統能夠偵測到預先啟動之前就進行其初始化。 從 Windows 1607 (年度版) 開始，預設將不會預先啟用您的遊戲。  如果您確實希望您的遊戲利用預先啟動，請呼叫 [CoreApplication.EnablePrelaunch(true)](/uwp/api/windows.applicationmodel.core.coreapplication.enableprelaunch)。

如果您的遊戲是以舊版 Windows 10 為目標，則您可以處理預先啟動條件來結束應用程式：

```cppwinrt
void ViewProvider::OnActivated(CoreApplicationView const& /* appView */, Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Launch)
    {
        auto launchArgs{ args.as<Windows::ApplicationModel::Activation::LaunchActivatedEventArgs>()};
        if (launchArgs.PrelaunchActivated())
        {
            // Opt-out of Prelaunch.
            CoreApplication::Exit();
        }
    }
}

void ViewProvider::Initialize(CoreApplicationView const & appView)
{
    appView.Activated({ this, &App::OnActivated });
}
```

```cpp
void ViewProvider::OnActivated(CoreApplicationView^ appView,IActivatedEventArgs^ args)
{
    if (args->Kind == ActivationKind::Launch)
    {
        auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args);
        if (launchArgs->PrelaunchActivated)
        {
            // Opt-out of Prelaunch
            CoreApplication::Exit();
            return;
        }
    }
}
```

## <a name="winjs-app-guidance"></a>WinJS 應用程式指導方針

如果您的 WinJS 應用程式是以舊版 Windows 10 為目標，則您可以在 [onactivated](/previous-versions/windows/apps/br212679(v=win.10)) 處理常式中處理預先啟動條件：

```javascript
    app.onactivated = function (args) {
        if (!args.detail.prelaunchActivated) {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }
    }
```

## <a name="general-guidance"></a>一般指導方針

-   App 不應該在預先啟動期間執行長時間執行的操作，因為如果無法快速暫停 App，App 就會終止。
-   當預先啟動應用程式時，應用程式不應初始化 [**Application.OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 的音訊播放，因為將會看不到應用程式，所以無法知道為什麼會播放音訊。
-   假設使用者可以看見應用程式，或假設應用程式是由使用者明確啟動，那麼應用程式在啟動期間不應該執行任何操作。 由於應用程式現在不需明確的使用者動作即可在背景中啟動，因此開發人員應該考量隱私權、使用者體驗及可能的效能影響。
    -   隱私權考量舉例：社交應用程式應該在何時將使用者狀態變更為線上。 它應該等到使用者切換到應用程式，而不是在預先啟動應用程式時變更狀態。
    -   使用者體驗考量舉例：如果您的應用程式 (例如遊戲) 會在啟動時顯示一個開場片段，則您可以將開場片段延遲到使用者切換到該應用程式之後才顯示。
    -   效能影響舉例：您可以等到使用者切換到 App 才擷取當前的天氣資訊，而不是在預先啟動 App 時就載入該資訊，然後在 App 變成可見時，需要再次載入該資訊以確保該資訊是當前的。
-   如果您的 App 會在啟動時清除其動態磚，請將此操作延遲到可見度變更事件發生之後才進行。
-   您 App 的遙測應該要能區分一般磚啟用和預先啟動啟用，以便讓您能夠在問題發生時縮小狀況範圍。
-   如果您有 Microsoft Visual Studio 2015 Update 1 和 Windows 10 版本 1511，您可以在 Visual Studio 2015 中選擇 **\[偵錯\]** &gt; **\[其他偵錯目標\]** &gt; **\[偵錯 Windows 通用應用程式預先啟動\]**，來模擬 App 的預先啟動。

## <a name="related-topics"></a>相關主題

* [應用程式生命週期](app-lifecycle.md)
* [CoreApplication.EnablePrelaunch](/uwp/api/windows.applicationmodel.core.coreapplication.enableprelaunch)