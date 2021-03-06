---
description: 本節顯示如何製作、封裝及使用您應用程式的字串、影像和檔案資源。
title: 應用程式資源和資源管理系統
label: Intro
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 3d6b23284c24de4b611301668c52894b57ed2500
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031801"
---
# <a name="app-resources-and-the-resource-management-system"></a>應用程式資源和資源管理系統


本節顯示如何製作、封裝及使用您應用程式的字串、影像和檔案資源。 例如，您可能會封裝檔案以及包含遊戲層級定義的休閒遊戲，並且載入執行階段的檔案。 我們也會向您顯示單獨維護您的應用程式邏輯的資源，如何可讓您輕鬆針對不同地區設定、裝置顯示、協助工具設定以及其他使用者和電腦內容，當地語系化及自訂您的應用程式。 諸如字串和影像的資源通常需要存在於多個語言、縮放比例和對比變體中。 對於這類資源，您有[資源管理系統](resource-management-system.md)的支援。

應用程式資源有兩種類型。
- 檔案資源是在磁碟上儲存為檔案的資源。 檔案資源可以包含點陣圖影像、XAML、XML、HTML 或任何其他類型的資料。
- 內嵌的資源是內嵌在一些包含資源檔案的資源。 最常見的範例是內嵌於資源檔案中的字串資源 (.resw 或 .resjson)。

如需有關將您的應用程式當地語系化的價值主張的詳細資訊，請參閱[全球化和當地語系化](../design/globalizing/globalizing-portal.md)。

| 發行項 | 描述 |
|---------|-------------|
| [資源管理系統](resource-management-system.md) | 建置期間，資源管理系統會建立所有不同變體 (使用您的應用程式封裝) 的資源的索引。 在執行階段，系統會偵測生效的使用者和電腦設定，並載入這些設定的最佳相符項的資源。 |
| [資源管理系統如何比對和選擇資源](how-rms-matches-and-chooses-resources.md) | 當要求資源時，也許有幾個某程度符合目前資源內容的候選項目。 資源管理系統將會分析所有的候選項目，並判斷要傳回的最佳候選項目。 本主題將詳細說明該程序，並且提供範例。 |
| [資源管理系統如何比對語言標記](how-rms-matches-lang-tags.md) | 上一個主題 ([資源管理系統如何比對和選擇資源](how-rms-matches-and-chooses-resources.md)) 主要綜觀限定詞比對。 本主題著重於語言標記比對的細節。 |
| [針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md) | 本主題說明資源限定詞的一般概念、其使用方式，以及每個限定詞名稱的用途。 |
| [當地語系化您的 UI 及應用程式套件資訊清單中的字串](localize-strings-ui-manifest.md) | 如果希望應用程式支援不同的顯示語言，而且您的程式碼、XAML 標記或應用程式套件資訊清單也含有字串常值時，請將這些字串移入資源檔案 (.resw)。 您可以接著針對應用程式支援的每一種語言建立該資源檔案的翻譯複本。 |
| [載入針對縮放比例、佈景主題、高對比及其他設定量身打造的影像和資產](images-tailored-for-scale-theme-contrast.md) | 您的應用程式可以載入包含針對顯示縮放比例、佈景主題、高對比及其他執行階段內容量身打造之影像的影像資源檔案。 |
| [URI 配置](uri-schemes.md) | 有幾個可供您用來參考您的應用程式套件、您的應用程式資料的資料夾或雲端之檔案的 URI (統一資源識別項) 配置。 您也可以使用 URI 配置參考從您應用程式的檔案資源 (.resw) 載入的字串。 |
| [指定您的應用程式使用的預設資源](specify-default-resources-installed.md) | 如果您的應用程式沒有符合客戶裝置特定設定的資源，則會使用應用程式的預設資源。 本主題說明如何指定這些預設資源的內容。 |
| [將資源建置到您的應用程式套件，而不是資源套件](build-resources-into-app-package.md) | 某些類型的應用程式 (多語系字典、翻譯工具等) 需要覆寫應用程式套件組合的預設行為，並將資源建置到應用程式套件中，而不是讓這些資源分散在不同的資源套件中。 本主題說明如何執行這個動作。 |
| [套件資源索引 (PRI) API 和自訂建置系統](pri-apis-custom-build-systems.md) | 您可以使用[套件資源索引 (PRI) API](/windows/desktop/menurc/pri-indexing-reference)，開發適用於您的 UWP 應用程式資源的自訂建置系統。 建置系統可以依據 UWP 應用程式所需的任何複雜層級，建立套件資源索引 (PRI) 檔案並對這些檔案進行版本控制和傾印 (為 XML)。 |
| [使用 MakePri.exe 來手動編譯資源](compile-resources-manually-with-makepri.md) | MakePri.exe 是一個命令列工具，您可以用來建立和傾印 PRI 檔案。 其做為 MSBuild 的一部分整合到 Microsoft Visual Studio 中，但用於手動建立套件或自訂組建系統也很有用。 |
| [在舊版應用程式或遊戲中使用 Windows 10 資源管理系統](using-mrt-for-converted-desktop-apps-and-games.md) | 將您的 .NET 或 Win32 應用程式或遊戲封裝成 .msix 或 .appx 套件，即可利用「資源管理系統」載入為執行階段內容量身打造的應用程式資源。 這個深入主題說明技術。 |

另請參閱[對語言、縮放比例及高對比的磚與快顯通知支援](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)。
