---
description: 您可以透過程式設計方式，將自己的 App 主要磚釘選到 [開始]，就像您可以釘選次要磚一樣。 您也可以檢查其目前是否已釘選。
title: 主要磚 API
label: Primary tile API's
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp, StartScreenManager, pin primary tile, primary tile apis, check if tile pinned, live tile, 釘選主要磚, 主要磚 api, 檢查是否釘選, 動態磚
ms.localizationpriority: medium
ms.openlocfilehash: 83cf11d80ffcd03148cbe5e784aaad5836357796
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029691"
---
# <a name="primary-tile-apis"></a>主要磚 API
 

主要磚 API 可讓您查看您的應用程式目前是否釘選在 \[開始\] 上，以及要求釘選您應用程式的主要磚。

> [!IMPORTANT]
> **需要 Creators Update** ：您的目標必須是 SDK 15063 並執行組建 15063 或更新版本，才能使用主要磚 API。

> **重要 API** ： [**StartScreenManager 類別**](/uwp/api/windows.ui.startscreen.startscreenmanager)、 [ContainsAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)、 [RequestAddAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)


## <a name="when-to-use-primary-tile-apis"></a>使用主要磚 API 的時機

您大費周章設計了提供絕佳體驗的 App 主要磚，而您現在有機會可以要求使用者將它釘選到 [開始]。 但我們深入了解程式碼之前，以下是一些在設計您的體驗時要注意的事項：

* **務必** 在 App 中，以明確的「釘選動態磚」動作邀請來製作不具破壞性且可輕鬆關閉的 UX。
* **務必** 在要求使用者釘選 App 動態磚之前，清楚說明其真實意義。
* **請勿** 要求使用者釘選 App 的磚，如果該磚已釘選或裝置不支援此磚的話 (詳細資訊後述)。
* **請勿** 重複要求使用者釘選 App 的磚 (這樣可能會困擾使用者)。
* **請勿** 在沒有明確使用者互動情況下，或在應用程式已最小化/未開啟時呼叫釘選 API。


## <a name="checking-whether-the-apis-exist"></a>檢查 API 是否存在

如果您的 App 支援舊版 Windows 10，則必須檢查是否有這些主要磚 API 可用。 請使用 ApiInformation 進行這項檢查。 如果沒有磚主要 API 可用，請避免執行任何 API 呼叫。

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.StartScreen.StartScreenManager"))
{
    // Primary tile API's supported!
}
else
{
    // Older version of Windows, no primary tile API's
}
```


## <a name="check-if-start-supports-your-app"></a>檢查 [開始] 是否支援您的 App

視目前的 [開始] 功能表和 App 的類型而定，有可能不支援釘選您的 App 到開始畫面。 僅限電腦版和行動裝置版支援釘選主要磚到開始畫面。 因此，在顯示任何釘選 UI 或執行任何釘選程式碼之前，必須先檢查您的 App 是否也支援目前的開始畫面。 如果不支援，請勿提示使用者釘選磚。

```csharp
// Get your own app list entry
// (which is always the first app list entry assuming you are not a multi-app package)
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if Start supports your app
bool isSupported = StartScreenManager.GetDefault().SupportsAppListEntry(entry);
```


## <a name="check-whether-youre-currently-pinned"></a>檢查您的磚目前是否已釘選

若要了解主要磚目前是否已釘選到 [開始]，請使用 [ContainsAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) 方法。

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if your app is currently pinned
bool isPinned = await StartScreenManager.GetDefault().ContainsAppListEntryAsync(entry);
```


##  <a name="pin-your-primary-tile"></a>釘選主要磚

如果主要磚目前未釘選，但 [開始] 支援畫面此磚時，您可能需要向使用者顯示提示，告知他們可以釘選您的主要磚。

> [!NOTE]
> 您必須在 App 處於前景時從 UI 執行緒呼叫此 API，而且只能在刻意要求使用者釘選主要磚之後 (例如，在使用者針對關於釘選主要磚的提示按一下 [是] 之後) 呼叫此 API。

如果使用者按一下按鈕來釘選主要磚，您可以接著呼叫 [RequestAddAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) 方法以要求將磚釘選到 [開始]。 這樣會顯示對話方塊，要求使用者確認他們想要將您的磚釘選到 [開始]。

此時將傳回表示您的磚現在是否已釘選到 [開始] 的布林值。 如果您的磚尚未釘選，將會立即傳回 true，而不會向使用者顯示對話方塊。 如果使用者按一下對話方塊中的 [否]，或系統不支援釘選您的磚到 [開始] 時，將會傳回 false。 否則，若使用者按了一下 [是] 且磚已釘選時，API 將會傳回 true。

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// And pin it to Start
bool isPinned = await StartScreenManager.GetDefault().RequestAddAppListEntryAsync(entry);
```


## <a name="resources"></a>資源

* [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-pin-primary-tile)
* [釘選到工作列](../pin-to-taskbar.md)
* [磚、徽章及通知](index.md)
* [調適型磚文件](create-adaptive-tiles.md)
