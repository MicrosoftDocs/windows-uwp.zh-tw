---
title: 處理應用程式啟用
description: 了解如何透過覆寫 OnLaunched 方法來處理應用程式啟用。
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 0c3aa86fd8eee3724e092799eea6d34f0b9d453b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164992"
---
# <a name="handle-app-activation"></a>處理應用程式啟用

瞭解如何藉由覆寫 [**>onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 方法來處理應用程式啟用。

## <a name="override-the-launch-handler"></a>覆寫啟動處理常式

當應用程式啟動時，如果有任何原因，系統會傳送 [**CoreApplicationView**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 事件。 如需啟用類型的清單，請參閱 [**ActivationKind**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) 列舉。

[**Windows.UI.Xaml.Application**](/uwp/api/Windows.UI.Xaml.Application) 類別定義了您可以覆寫來處理不同啟用類型的方法。 數種啟用類型有您可以覆寫的特定方法。 如需其他啟用類型，請覆寫 [**OnActivated**](/uwp/api/windows.ui.xaml.application.onactivated) 方法。

定義應用程式的類別。

```xml
<Application
    x:Class="AppName.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

覆寫 [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 方法。 不論使用者何時啟動 app 都會呼叫這個方法。 [**LaunchActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) 參數包含 app 之前的狀態以及啟用引數。

> [!NOTE]
> 在 Windows 中，從 [開始] 磚或應用程式清單啟動暫停的應用程式不會呼叫這個方法。

```csharp
using System;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

namespace AppName
{
   public partial class App
   {
      async protected override void OnLaunched(LaunchActivatedEventArgs args)
      {
         EnsurePageCreatedAndActivate();
      }

      // Creates the MainPage if it isn't already created.  Also activates
      // the window so it takes foreground and input focus.
      private MainPage EnsurePageCreatedAndActivate()
      {
         if (Window.Current.Content == null)
         {
             Window.Current.Content = new MainPage();
         }

         Window.Current.Activate();
         return Window.Current.Content as MainPage;
      }
   }
}
```

```vb
Class App
   Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
      Window.Current.Content = New MainPage()
      Window.Current.Activate()
   End Sub
End Class
```

```cppwinrt
...
#include "MainPage.h"
#include "winrt/Windows.ApplicationModel.Activation.h"
#include "winrt/Windows.UI.Xaml.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
...
using namespace winrt;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    /// <summary>
    /// Invoked when the application is launched normally by the end user.  Other entry points
    /// will be used such as when the application is launched to open a specific file.
    /// </summary>
    /// <param name="e">Details about the launch request and process.</param>
    void OnLaunched(LaunchActivatedEventArgs const& e)
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
        }

        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active
        if (rootFrame == nullptr)
        {
            // Create a Frame to act as the navigation context and associate it with
            // a SuspensionManager key
            rootFrame = Frame();

            rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

            if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
            {
                // Restore the saved session state only when appropriate, scheduling the
                // final launch steps after the restore is complete
            }

            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Place the frame in the current Window
                Window::Current().Content(rootFrame);
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
        else
        {
            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
    }
};
```

```cpp
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;
void App::OnLaunched(LaunchActivatedEventArgs^ args)
{
   EnsurePageCreatedAndActivate();
}

// Creates the MainPage if it isn't already created.  Also activates
// the window so it takes foreground and input focus.
void App::EnsurePageCreatedAndActivate()
{
    if (_mainPage == nullptr)
    {
        // Save the MainPage for use if we get activated later
        _mainPage = ref new MainPage();
    }
    Window::Current->Content = _mainPage;
    Window::Current->Activate();
}
```

## <a name="restore-application-data-if-app-was-suspended-then-terminated"></a>在 app 暫停然後終止時還原應用程式資料

當使用者切換到已終止的 app 時，系統會傳送 [**Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 事件，並將 [**Kind**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind) 設定成 **Launch**，以及將 [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) 設定成 **Terminated** 或 **ClosedByUser**。 App 必須載入它已儲存的應用程式資料，並且重新整理已顯示的內容。

```csharp
async protected override void OnLaunched(LaunchActivatedEventArgs args)
{
   if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
       args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

```vb
Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
   Dim restoreState As Boolean = False

   Select Case args.PreviousExecutionState
      Case ApplicationExecutionState.Terminated
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case ApplicationExecutionState.ClosedByUser
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case Else
         ' TODO: Populate the UI with defaults
   End Select

   Window.Current.Content = New MainPage(restoreState)
   Window.Current.Activate()
End Sub
```

```cppwinrt
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs const& e)
{
    if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated ||
        e.PreviousExecutionState() == ApplicationExecutionState::ClosedByUser)
    {
        // Populate the UI with the previously saved application data.
    }
    else
    {
        // Populate the UI with defaults.
    }
    ...
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
{
   if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
       args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

如果 [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) 的值是 **NotRunning**，app 將無法成功儲存它的應用程式資料，而且 app 必須從頭開始，如同它剛被初始啟動一般。

## <a name="remarks"></a>備註

> [!NOTE]
> 如果目前的視窗中已有設定的內容，App 便可以略過初始化程序。 您可以檢查 [**LaunchActivatedEventArgs TileId**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.tileid) 屬性，以判斷應用程式是從主要或次要磚啟動，並根據該資訊來決定您是否應該提出全新或繼續的應用程式體驗。

## <a name="important-apis"></a>重要 API
* [ApplicationModel。啟用](/uwp/api/Windows.ApplicationModel.Activation)
* [Windows.UI.Xaml.Application](/uwp/api/Windows.UI.Xaml.Application)

## <a name="related-topics"></a>相關主題
* [處理應用程式暫停](suspend-an-app.md)
* [處理應用程式繼續執行](resume-an-app.md)
* [App 暫停和繼續執行的指導方針](./index.md)
* [應用程式生命週期](app-lifecycle.md)