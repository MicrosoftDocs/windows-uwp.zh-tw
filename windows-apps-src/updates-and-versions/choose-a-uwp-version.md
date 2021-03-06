---
title: 選擇 UWP 版本
description: 在 Microsoft Visual Studio 中撰寫 UWP app 時，您可以選擇要以哪個版本為目標。 了解不同 UWP 版本之間的差異，以及如何在新的及現有的專案中設定您的選項。
ms.date: 5/12/2019
ms.topic: article
keywords: windows 10, uwp, 版本, 組建, windows, 選擇, 更新
ms.assetid: a8b7830f-4929-44c6-90be-91f38be5f364
ms.localizationpriority: medium
f1_keywords:
- UniversalProjects.TargetPlatformWizardPicker
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 1987ad1267b3a48c0488e2d0af985f316097d3e2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173802"
---
# <a name="choose-a-uwp-version"></a>選擇 UWP 版本

每一版的 Windows 10 都為 UWP 平台帶來了全新與改進的功能。 在 Microsoft Visual Studio 中建立 UWP 應用程式時，您可以選擇要以哪個版本為目標。 使用 [.NET Standard 2.0](/dotnet/standard/net-standard) 的專案必須擁有組建 16299 或更新版本的**最小版本**。

> [!WARNING]
> 在新版 Visual Studio 中建立的任何 UWP 專案，都無法在 Visual Studio 2015 中開啟。

下表說明可用的 Windows 10 版本。 請注意，此表格僅適用於建置只有 Windows 10 支援的 UWP 應用程式。 您無法為舊版 Windows 開發 UWP 應用程式，且必須已[安裝適當的 SDK 組建](https://developer.microsoft.com/windows/downloads#_blank)，才能以該版本為目標。

| 版本 | 說明 |
| --- | --- |
| 組建 19041 (版本 2004) | 這是 2020 年 5 月發行的最新 Windows 10 版本。 此版本的重點功能包括： </br> \* **WSL2：** Windows 子系統 Linux 版已使用新的架構模型進行更新，現在會在 Windows 上執行實際的 Linux 核心。 請在[關於 WSL2](/windows/wsl/wsl2-about) 中深入了解。 </br> \*  **MSIX：** Windows 中的新功能可進一步支援新式 MSIX 應用程式封裝格式，包括能夠建立包含服務的套件、建立託管的應用程式，以及在非封裝應用程式中包含需要套件識別資料的功能。 在 [MSIX 文件](/windows/msix/overview)中深入了解。 </br> 如需這些功能及此版本 Windows 中所新增之其他多項功能的詳細資訊，請造訪[開發人員中心](https://developer.microsoft.com/windows/windows-10-for-developers)，或是參閱更深入的 [Windows 10 中適用於開發人員的新增功能](../whats-new/windows-10-build-19041.md)頁面
| 組建 18362 (版本 1903) | 此版本的 Windows 10 於 2019 年 4 月發行。 此版本的一些重點功能包括： </br> \* **XAML Islands:** Windows 10 現在可讓您在非 UWP 傳統型應用程式中使用 UWP 控制項。 如果您正在開發的 WPF、Windows Forms 或 C++Win32，[請查看如何將最新的 Windows 10 UI 功能新增至現有的應用程式](/windows/apps/desktop/modernize/xaml-islands)。 </br> \* **Windows 子系統 Linux 版：** 您現在可以直接從 Windows 存取 Linux 檔案，並使用數個新的命令列選項。 請參閱[關於 WSL](/windows/wsl/about.md) 的最新資訊。 </br> 如需這些功能及此版本 Windows 中所新增之其他多項功能的相關資訊，請造訪[組建 18362 中的新功能](../whats-new/windows-10-build-18362.md)
| 組建 17763 (版本 1809) | 此版本的 Windows 10 於 2018 年 11 月發行。 **請注意，您「必須」使用 Visual Studio 2017 或 Visual Studio 2019，才能以這個版本的 Windows 為目標。** 此版本的一些重點功能包括： </br> \* **Windows Machine Learning：** Windows Machine Learning 已正式推出，可為尖端機器學習模型提供更快速的評估和支援。 若要深入了解此平台，請參閱 [Windows Machine Learning](/windows/ai/)。 </br> \* **Fluent Design：** Windows 10 已新增一些新功能，例如功能表列、命令列飛出視窗和 XAML 屬性動畫。 請至 [Fluent Design 概觀](/windows/apps/fluent-design-system)查看最新消息。 </br> 如需這些功能及此版本 Windows 中所新增之其他多項功能的相關資訊，請造訪 [組建 17763 中的新功能](../whats-new/windows-10-build-17763.md)
| 組建 17134 (版本 1803) | 此版本的 Windows 10 於 2018 年 4 月發行。 **請注意，您「必須」使用 Visual Studio 2017 或 Visual Studio 2019，才能以這個版本的 Windows 為目標。** 此版本的一些重點功能包括： </br> \* **Fluent Design：** Windows 10 中已新增樹狀檢視、拖動重新整理及瀏覽檢視等新功能。 請至 [Fluent Design 概觀](/windows/apps/fluent-design-system)查看最新消息。 </br> \* **主控台 UWP 應用程式：** 您現在可以撰寫執行於主控台視窗 (例如 DOS 或 PowerShell 主控台視窗) 中的 C++ /WinRT 或 /CX UWP 主控台應用程式。 </br> 如需這些功能及此版本 Windows 中所新增之其他多項功能的相關資訊，請造訪[組建 17134 中的新功能](../whats-new/windows-10-build-17134.md)
| 組建 16299 (Fall Creators Update，版本 1709) | 此版本的 Windows 10 於 2017 年 10 月發行。 **請注意，您「必須」使用 Visual Studio 2017 或 Visual Studio 2019，才能以這個版本的 Windows 為目標。** 此版本的一些重點功能包括： </br> \* **.NET Standard 2.0：** 包含大量增加的 .NET API 並將您最愛的 NuGet 套件和第三方程式庫納入 .NET Standard。 請至[這裡](/dotnet/standard/net-standard)查看更多詳細資料並瀏覽文件。 請注意，您必須將**最小版本**設定為組建 16299，才能存取這些新的 API。 </br> \* **Fluent Design：** 使用光線、深度、透視以及移動來增強您的應用程式，並協助使用者專注於重要的 UI 元素。 </br> \* **條件式 XAML：** 輕鬆設定屬性，並根據執行階段是否存在 API 來起始物件，讓您的應用程式在各裝置和版本間順暢執行。 </br> 如需這些功能及此版本 Windows 中所新增之其他多項功能的相關資訊，請造訪 [適用於開發人員的 Windows 10 之新功能](../whats-new/windows-10-build-16299.md)
| 組建 15063 (Creators Update，版本 1703) | 此版本的 Windows 10 於 2017 年 3 月發行。 **請注意，您「必須」使用 Visual Studio 2017 或 Visual Studio 2019，才能以這個版本的 Windows 為目標。** 此版本的一些重點功能包括：  </br> \* **筆跡分析：** Windows Ink 現在可以將筆墨筆劃分類為書寫或繪圖筆劃，並辨識文字、形狀和基本配置結構。 </br> \* **Windows.Ui.Composition API：** 輕鬆地結合並套用您的應用程式之間的動畫。 </br> \* **即時編輯：** 在應用程式執行時同時編輯 XAML，並即時看見所套用的變更。 </br> 如需這些功能及此版本 Windows 中所新增之其他多項功能的相關資訊，請造訪[組建 15063 中的新功能](../whats-new/windows-10-build-15063.md)  |
| 組建 14393 (年度更新版，版本 1607) | 此版本的 Windows 10 於 2016 年 7 月發行。 此版本的一些重點功能包括： </br> \* **Windows Ink：** 新的 InkCanvas 和 InkToolbar 控制項。 </br> \* **Cortana API：** 使用新的 Cortana 動作來將 Cortana 支援與您應用程式的特定功能整合。 </br> \* **Windows Hello：** Microsoft Edge 現在支援 Windows Hello，提供網頁開發人員存取生物識別驗證。 </br> 如需這些功能及此版本 Windows 中所新增之其他多項功能的相關資訊，請造訪[組建 14393 中的新功能](../whats-new/windows-10-build-14393.md)  |
| 組建 10586 (11 月更新，版本 1511) | 此版本的 Windows 10 發行於 2015 年 11 月。 重點功能包括為 Microsoft Edge 和提供者 API 中的影片通訊推出 ORTC (物件即時通訊) API，讓 App 可以使用 Windows Hello 臉部驗證。 [於此組建推出之功能的詳細資訊。](../whats-new/windows-10-build-10586.md) |
| Build 10240 (Windows 10，版本 1507) | 這是 2015 年 7 月發行的初始 Windows 10 版本。 [於此組建推出之功能的詳細資訊。](../whats-new/windows-10-build-10240.md) |

強烈建議新的開發人員與針對一般大眾撰寫程式碼的開發人員一律使用最新的 Windows 組建 (19041)。 撰寫企業應用程式的開發人員應強烈考慮支援較舊的 [最小版本]。

## <a name="whats-different-in-each-uwp-version"></a>每個 UWP 版本有何差異？

每個 Windows 10 後繼版本都可使用適用於 UWP 的新 API 和已變更的 API。 如需有關哪個版本中新增了哪些功能的特定資訊，請參閱 [Windows 10 提供給開發人員的新功能](../whats-new/windows-10-build-19041.md)。

如需列舉出所有裝置系列及其版本和所有 API 協定及其版本的參考主題，請參閱[裝置系列](/uwp/extension-sdks/)和 [API 協定](/uwp/extension-sdks/)。

## <a name="net-api-availability-in-uwp-versions"></a>在 UWP 版本的 .NET API 可用性

UWP 支援有限的 .NET API 子集，不論您專案的**目標版本**或**最低版本**為何都可使用。 [此頁面提供有關可用類型的詳細資訊](/dotnet/api/index?view=dotnet-uwp-10.0)。

如果您想要建立可重複使用的跨平台程式庫，則 UWP 支援 .NET Standard。 [.NET Standard 文件](/dotnet/standard/net-standard)提供哪些 UWP 版本支援 .NET Standard 的資訊。

如果您正在開發傳統型應用程式，請改為參數 [.NET Framework 版本和相依性](/dotnet/framework/migration-guide/versions-and-dependencies)，以取得 .NET Framework 可用性的詳細資訊。

## <a name="choose-which-version-to-use-for-your-app"></a>選擇要針對您 App 使用的版本

在 Visual Studio 中的 [新增通用 Windows 專案] 對話方塊中，您可以為 [目標版本] 和 [最小版本] 選擇版本。 此外，您可以在應用程式其 [屬性] 的 [應用程式] 區段中，變更 UWP 應用程式的 [目標版本] 和 [最小版本]。

* **目標版本**。 您的應用程式打算在其上執行的 Windows 10 版本。 這會設定您專案檔中的 *TargetPlatformVersion* 設定。 它也會決定您應用程式套件資訊清單中 *TargetDeviceFamily@MaxVersionTested* 屬性的值。 您選擇的值會指定您專案的目標 UWP 平台版本，也因此會決定您 App 可用的一組 API，所以我們建議您選擇可能的最新版本。 如需有關您應用程式套件資訊清單的詳細資訊，以及一些有關手動設定 TargetDeviceFamily 指導方針，請參閱 [TargetDeviceFamily](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)。
* **最小版本**。 支援應用程式基本功能所需的最舊 Windows 10 版本。 這會設定您專案檔中的 *TargetPlatformMinVersion* 設定。 它也會決定您應用程式套件資訊清單中 *TargetDeviceFamily@MinVersion* 屬性的值。 您選擇的值會指定您專案可搭配運作的 UWP 平台最小版本。

請注意，您宣告的是您的應用程式可以在從 [最小版本] 到 [目標版本] 範圍內的任何 Windows 版本上運作。 如果這兩者是相同版本，您就不需要採取任何特別的動作。 如果兩者不同，則以下是一些需要注意的事項。

* 在您的程式碼中，您可以自由地 (也就是沒有條件檢查) 呼叫存在於 [最小版本] 所指定版本中的任何 API。
* 確定您是在執行 [最小版本] 的裝置上測試程式碼，以確保它無需僅在 [目標版本] 中提供的 API 也可正常運作。
* [目標版本] 的值可用來識別所有用來編譯您專案的參考 (協定 winmds)。 但這些參考將可讓您在編譯您的程式碼時，使用不一定存在於您所宣告支援之裝置上的 API 呼叫 (透過 [最小版本]) 來進行編譯。 因此，呼叫在 [最小版本] 之後引進的 API 時，將需要透過調適型程式碼來呼叫。 如需調適型程式碼的相關詳細資訊，請參閱[版本調適型程式碼](../debug-test-perf/version-adaptive-code.md)。