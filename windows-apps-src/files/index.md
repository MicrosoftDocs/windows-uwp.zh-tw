---
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: 檔案、資料夾和媒體櫃
description: 了解如何讀取和寫入應用程式設定、檔案和資料夾選擇器，以及特殊的沙箱式位置，例如影片/音樂媒體櫃。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 714185f943e5bb3d417128011684fcc367fd7f20
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175412"
---
 # <a name="files-folders-and-libraries"></a>檔案、資料夾和媒體櫃


您可以使用 [Windows.Storage](/uwp/api/Windows.Storage)、[Windows.Storage.Streams](/uwp/api/Windows.Storage.Streams) 和 [Windows.Storage.Pickers](/uwp/api/Windows.Storage.Pickers) 命名空間中的 API，以在檔案中讀取與寫入文字和其他資料格式，以及管理檔案和資料夾。 在本節中，您也將了解讀取和寫入 app 設定、檔案和資料夾選擇器，並了解特殊的沙箱式位置，例如影片/音樂媒體櫃。

| 主題 | 描述  |
|-------|--------------|
| [列舉和查詢檔案和資料夾](quickstart-listing-files-and-folders.md) | 存取位於資料夾、媒體櫃、裝置或網路位置中的檔案和資料夾。 您也可以建構檔案和資料夾查詢，來查詢位置中的檔案和資料夾。 |
| [建立、寫入和讀取檔案](quickstart-reading-and-writing-files.md) | 使用 [StorageFile](/uwp/api/Windows.Storage.StorageFile) 物件讀取和寫入檔案。 |
| [寫入檔案的最佳做法](best-practices-for-writing-to-files.md) | 了解使用 [FileIO](/uwp/api/windows.storage.fileio) 和 [PathIO](/uwp/api/windows.storage.pathio) 類別的各種檔案寫入方法的最佳做法。 |
| [取得檔案屬性](quickstart-getting-file-properties.md) | 取得由 [StorageFile](/uwp/api/Windows.Storage.StorageFile) 物件所表示檔案的屬性 (最上層、基本及延伸)。 |
| [使用選擇器開啟檔案和資料夾](quickstart-using-file-and-folder-pickers.md) | 讓使用者與選擇器互動以存取檔案和資料夾。 您可以使用 [FolderPicker](/uwp/api/Windows.Storage.Pickers.FolderPicker) 來存取資料夾。 |
| [使用選擇器儲存檔案](quickstart-save-a-file-with-a-picker.md) | 使用 [FileSavePicker](/uwp/api/Windows.Storage.Pickers.FileSavePicker)，讓使用者指定想要您的 app 儲存檔案的名稱和位置。 |
| [存取 HomeGroup 內容](quickstart-accessing-homegroup-content.md) | 存取儲存在使用者 HomeGroup 資料夾中的內容，包括圖片、音樂及視訊。 |
| [判斷 Microsoft OneDrive 檔案的可用性](quickstart-determining-availability-of-microsoft-onedrive-files.md) | 使用 [StorageFile.IsAvailable](/uwp/api/windows.storage.storagefile.isavailable) 屬性判斷 Microsoft OneDrive 檔案是否可供使用。 |
| [音樂、圖片及影片媒體櫃中的檔案和資料夾](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | 將現有的音樂、圖片或影片資料夾新增到對應的媒體櫃中。 您也可以從媒體櫃中移除資料夾、取得媒體櫃中的資料夾清單，以及尋找已儲存的相片、音樂和影片。 |
| [追蹤最近使用的檔案和資料夾](how-to-track-recently-used-files-and-folders.md) | 將使用者經常存取的檔案新增到您應用程式的最近使用清單 (MRU) 中，以追蹤這些檔案。 平台會根據項目上次存取的時間來排序項目，並在達到清單的 25 個項目數限制時移除最舊的項目，為您管理 MRU。 所有 app 都有自己的 MRU。 |
| [追蹤在背景中的檔案系統變更](change-tracking-filesystem.md) | 追蹤檔案系統變更，即使應用程式未執行時亦然。|
| [存取 SD 記憶卡](access-the-sd-card.md) | 您可以在選用的 microSD 記憶卡上儲存和存取非必要的資料，尤其是內部儲存空間有限的低價行動裝置。 |
| [檔案存取權限](file-access-permissions.md) | App 預設可以存取特定的檔案系統位置。 應用程式也可以透過檔案選擇器或宣告功能，以存取其他位置。 |
| [快速存取 UWP 中的檔案屬性](fast-file-properties.md) | 有效收集程式庫的檔案和其屬性清單以便用於 UWP 應用程式。 |

## <a name="related-samples"></a>相關範例
[資料夾列舉範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FolderEnumeration)

[檔案存取範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)

[檔案選擇器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker)
 

 