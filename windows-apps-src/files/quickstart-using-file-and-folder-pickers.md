---
ms.assetid: F87DBE2F-77DB-4573-8172-29E11ABEFD34
title: 使用選擇器開啟檔案和資料夾
description: 讓使用者與選擇器互動以存取檔案和資料夾。 您可以使用 FileOpenPicker 和 FileSavePicker 類別來存取檔案，使用 FolderPicker 來存取資料夾。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8a438c5c0fa0e3dbe932c393291afe05b28b3e4a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175422"
---
# <a name="open-files-and-folders-with-a-picker"></a>使用選擇器開啟檔案和資料夾

**重要 API**

-   [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) \(英文\)
-   [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker) \(英文\)
-   [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) \(英文\)

讓使用者與選擇器互動以存取檔案和資料夾。 您可以使用 [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 和 [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) 類別來存取檔案，使用 [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker) 來存取資料夾。

> [!NOTE]
> 如需完整範例，請參閱[檔案選擇器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker) \(英文\)。

## <a name="prerequisites"></a>必要條件


-   **了解通用 Windows 平台 (UWP) 應用程式的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md)。

-   **位置的存取權限**

    請參閱[檔案存取權限](file-access-permissions.md)。

## <a name="file-picker-ui"></a>檔案選擇器 UI


檔案選擇器會顯示資訊以引導使用者，並且在開啟或儲存檔案時提供一致的體驗。

資訊包括：

-   目前的位置
-   使用者選擇的項目
-   使用者可以瀏覽的位置樹狀結構。 這些位置包括檔案系統位置 (例如 [音樂] 或 [下載] 資料夾)，以及實作檔案選擇器協定 (例如相機、相片與 Microsoft OneDrive) 的 app。

電子郵件 app 可能會顯示檔案選擇器，讓使用者可以挑選附件。

![有兩個選取要開啟之檔案的檔案選擇器。](images/picker-multifile-600px.png)

## <a name="how-pickers-work"></a>選擇器的運作方式


使用選擇器，您的 app 可以存取、瀏覽和儲存使用者系統上的檔案和資料夾。 您的 app 會以 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 和 [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) 物件的形式接收這些挑中的項目，您稍後可以進行操作。

選擇器使用統合的單一介面，讓使用者得以從檔案系統或其他 app 挑選檔案和資料夾。 從其他 app 挑選的檔案就像從檔案系統挑選的檔案一樣：它們也會以 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 物件傳回。 一般而言，您的 app 操作它們的方式與操作其他物件一樣。 其他 app 只要透過參與檔案選擇器協定就可以提供檔案。 如果您想要 app 提供檔案、儲存位置或檔案更新給其他 app，請參閱[與檔案選擇器協定整合](/previous-versions/windows/apps/hh465192(v=win.10))。

例如，您可以在 app 中呼叫檔案選擇器，讓您的使用者能夠開啟檔案。 這會讓您的 app 成為呼叫 app。 檔案選擇器與系統及其他 app 互動，以便讓使用者瀏覽和挑選檔案。 使用者選擇檔案時，檔案選擇器會將這個檔案傳回到您的 app。 以下是使用者從提供的 app (例如 OneDrive) 選擇檔案時的程序。

![圖表中顯示的程序是一個 app 透過另一個 app 開啟檔案，並將檔案選擇器當作兩個 app 之間的介面使用。](images/app-to-app-diagram-600px.png)

## <a name="pick-a-single-file-complete-code-listing"></a>挑選單一檔案：完整程式碼清單


```cs
var picker = new Windows.Storage.Pickers.FileOpenPicker();
picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
picker.FileTypeFilter.Add(".jpg");
picker.FileTypeFilter.Add(".jpeg");
picker.FileTypeFilter.Add(".png");

Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();
if (file != null)
{
    // Application now has read/write access to the picked file
    this.textBlock.Text = "Picked photo: " + file.Name;
}
else
{
    this.textBlock.Text = "Operation cancelled.";
}
```

## <a name="pick-a-single-file-step-by-step"></a>挑選單一檔案：逐步說明


呼叫檔案選擇器牽涉到建立和自訂檔案選擇器物件，然後顯示檔案選擇器供使用者挑選一或多個項目。

1.  **建立和自訂 FileOpenPicker**

    ```cs
    var picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
    picker.FileTypeFilter.Add(".jpg");
    picker.FileTypeFilter.Add(".jpeg");
    picker.FileTypeFilter.Add(".png");
    ```
    在與您的使用者和 app 相關的檔案選擇器物件上設定屬性。

    這個範例會藉由設定下列三個屬性，在使用者可以挑選的便利位置建立豐富的視覺化圖片：[**ViewMode**](/uwp/api/windows.storage.pickers.fileopenpicker.viewmode) \(英文\)、[**SuggestedStartLocation**](/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) \(英文\) 及 [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) \(英文\)。

    -   將 [**ViewMode**](/uwp/api/windows.storage.pickers.fileopenpicker.viewmode) \(英文\) 設定成 [**PickerViewMode**](/uwp/api/Windows.Storage.Pickers.PickerViewMode) \(英文\) **Thumbnail** 列舉值會在檔案選擇器中使用圖片縮圖來表示檔案，來建立豐富的視覺顯示。 執行這個動作來挑選如圖片或影片的視覺檔案。 否則，請使用 [**PickerViewMode.List**](/uwp/api/Windows.Storage.Pickers.PickerViewMode)。 具有**附加圖片或影片**和**附加文件**功能的假設電子郵件 app 會針對功能適當設定 **ViewMode**，再顯示檔案選擇器。

    -   使用 [**PickerLocationId.PicturesLibrary**](/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) 將 [**SuggestedStartLocation**](/uwp/api/Windows.Storage.Pickers.PickerLocationId) 設定為 [圖片]，讓使用者一開始就在可以找到圖片的位置。 將 **SuggestedStartLocation** 設定為所挑選檔案類型的適當位置，例如，音樂、圖片、影片或文件。 使用者可以從開始位置瀏覽到其他位置。

    -   使用 [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) 以指定檔案類型，讓使用者聚焦在挑選相關的檔案。 若要使用新的項目取代 **FileTypeFilter** 中的檔案類型，請使用 [**ReplaceAll**](/uwp/api/windows.storage.pickers.fileextensionvector.replaceall) (而不是 [**Add**](/uwp/api/windows.storage.pickers.fileextensionvector.append))。

2.  **顯示 FileOpenPicker**

    - **開啟單一檔案**

        ```cs
        Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();
        if (file != null)
        {
            // Application now has read/write access to the picked file
            this.textBlock.Text = "Picked photo: " + file.Name;
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
        ```

    - **開啟多個檔案**  

        ```cs
        var files = await picker.PickMultipleFilesAsync();
        if (files.Count > 0)
        {
            StringBuilder output = new StringBuilder("Picked files:\n");
    
            // Application now has read/write access to the picked file(s)
            foreach (Windows.Storage.StorageFile file in files)
            {
                output.Append(file.Name + "\n");
            }
            this.textBlock.Text = output.ToString();
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
        ```

## <a name="pick-a-folder-complete-code-listing"></a>挑選資料夾：完整程式碼清單


```cs
var folderPicker = new Windows.Storage.Pickers.FolderPicker();
folderPicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.Desktop;
folderPicker.FileTypeFilter.Add("*");

Windows.Storage.StorageFolder folder = await folderPicker.PickSingleFolderAsync();
if (folder != null)
{
    // Application now has read/write access to all contents in the picked folder
    // (including other sub-folder contents)
    Windows.Storage.AccessCache.StorageApplicationPermissions.
    FutureAccessList.AddOrReplace("PickedFolderToken", folder);
    this.textBlock.Text = "Picked folder: " + folder.Name;
}
else
{
    this.textBlock.Text = "Operation cancelled.";
}
```

> [!TIP]
> 每當您的應用程式透過選擇器來存取檔案或資料夾時，將該項目新增到應用程式的 [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) \(英文\) 或 [**MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist) \(英文\) 來加以追蹤。 若要深入了解如何使用這些清單，請參閱[如何追蹤最近使用的檔案和資料夾](how-to-track-recently-used-files-and-folders.md)。