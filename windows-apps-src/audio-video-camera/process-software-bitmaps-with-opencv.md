---
ms.assetid: ''
description: 本文說明如何使用 Open Source Computer Vision Library (OpenCV) 搭配 SoftwareBitmap 類別。
title: 使用 OpenCV 處理點陣圖
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, opencv, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: a917c4efc8da8fbdabbdc753aacf23724ae17055
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363801"
---
# <a name="process-bitmaps-with-opencv"></a>使用 OpenCV 處理點陣圖

本文說明如何使用 **[SoftwareBitmap](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** 類別，此類別是由許多不同的 Windows 執行階段 api 用來代表影像，並提供開放原始碼電腦視覺程式庫 (OpenCV) ，此為開放原始碼、原生程式碼程式庫，可提供各式各樣的影像處理演算法。 

本文中的範例將逐步引導您建立可從 UWP 應用程式使用的機器碼 Windows 執行階段元件，包括使用 c # 建立的應用程式。 此協助程式元件將會公開單一方法 **Blur**，此方法會使用 OpenCV 的模糊影像處理功能。 此元件會實作私用方法以取得指向基礎影像資料緩衝區的指標，可供 OpenCV 程式庫直接使用，讓您更容易延伸協助程式元件來實作其他 OpenCV 處理功能。 

* 如需使用 **SoftwareBitmap** 的指示，請參閱[建立、編輯和儲存點陣圖影像](imaging.md)。 
* 若要瞭解如何使用 OpenCV 程式庫，請移至 [https://opencv.org](https://opencv.org) 。
* 若要了解如何使用本文所顯示的 OpenCV 協助程式元件來搭配 **[MediaFrameReader](/uwp/api/windows.media.capture.frames.mediaframereader)**，實作取自相機之畫面的即時影像處理，請參閱[使用 OpenCV 搭配 MediaFrameReader](use-opencv-with-mediaframereader.md)。
* 如需實作一些不同效果的完整程式碼範例，請參閱 Windows 通用範例 GitHub 存放庫中的[相機畫面 + OpenCV 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) (英文)。

> [!NOTE] 
> 本文詳細說明的 OpenCVHelper 元件所用技術，需要我們將進行處理的影像資料位於 CPU 記憶體中，而不是 GPU 記憶體中。 因此，對於可讓您要求影像的記憶體位置的 API，例如 **[MediaCapture](/uwp/api/windows.media.capture.mediacapture)** 類別，您應指定 CPU 記憶體。

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>建立 OpenCV interop 的 helper Windows 執行階段元件

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. 在您的方案中新增原生程式碼 Windows 執行階段元件專案

1. 在 \[方案總管\] 中的方案上按一下滑鼠右鍵，然後選取 **\[新增\] -> \[新增專案\]**，新增專案到 Visual Studio 中的方案。 
2. 在 **\[Visual C++\]** 類別下，選取 **\[Windows 執行階段元件 (通用 Windows)\]**。 對於此範例，將專案命名為「OpenCVBridge」，然後按一下 **\[確定\]**。 
3. 在 **\[新增 Windows 通用專案\]** 對話方塊中，選取您應用程式的目標和最小作業系統版本，然後按一下 **\[確定\]**。
4. 在 \[方案總管\] 中自動產生的檔案 Class1.cpp 上按一下滑鼠右鍵，然後選取 **\[移除\]**，確認對話方塊出現時，選擇 **\[刪除\]**。 接著刪除 Class1.h 標頭檔。
5. 在 OpenCVBridge 專案圖示上按一下滑鼠右鍵，然後選取 **\[新增\] -> \[類別\]**。在 **\[新增類別\]** 對話方塊中，在 **\[類別名稱\]** 欄位中輸入「OpenCVHelper」，然後按一下 **\[確定\]**。 我們將在稍後的步驟新增程式碼到所建立的類別檔案中。

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. 將 OpenCV NuGet 套件新增到您的元件專案

1. 在 \[方案總管\] 中，以滑鼠右鍵按一下 OpenCVBridge 專案，然後選取 **\[管理 NuGet 套件...\]**
2. 當 NuGet 封裝管理員對話方塊開啟時，請選取 [ **流覽** ] 索引標籤，然後在搜尋方塊中輸入 "OpenCV"。
3. 選取「OpenCV.Win.Core」，然後按一下 **\[安裝\]**。 在 **\[預覽\]** 對話方塊中，按一下 **\[確定\]**。
4. 使用相同的程序，安裝「OpenCV.Win.ImgProc」套件。

>[!NOTE]
>OpenCV 和 OpenCV 不會定期更新，且不會通過存放區合規性檢查，因此這些套件僅供實驗之用。

### <a name="3-implement-the-opencvhelper-class"></a>3. 實作 OpenCVHelper 類別

將下列程式碼貼到 OpenCVHelper.h 標頭檔中。 此程式碼包含我們安裝的 *Core* 和 *ImgProc* 套件的 OpenCV 標頭檔，並宣告將顯示在下列步驟中的三個方法。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h" id="SnippetOpenCVHelperHeader":::

刪除 OpenCVHelper.cpp 檔案的現有內容，然後新增下列 include 指示詞。 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperInclude":::

在 include 指示詞後面，新增下列 **using** 指示詞。 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperUsing":::

接下來，新增方法 **GetPointerToPixelData** 至 OpenCVHelper.cpp。 此方法接受 **[SoftwareBitmap](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** 並透過一系列轉換取得像素資料的 COM 介面表示法，我們可以透過此方法以 **char** 陣列取得指向基礎影像資料緩衝區的指標。 

首先，透過呼叫 **[LockBuffer](/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)** 取得包含像素資料的 **[BitmapBuffer](/uwp/api/windows.graphics.imaging.bitmapbuffer)**，要求讀取/寫入緩衝區以讓 OpenCV 程式庫可以修改像素資料。  呼叫 **[CreateReference](/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** 以取得 **[IMemoryBufferReference](/uwp/api/windows.foundation.imemorybufferreference)** 物件。 接下來，將 **IMemoryBufferByteAccess** 介面轉換成 **IInspectable** (這是所有 Windows 執行階段類別的基底介面)，並呼叫 **[QueryInterface](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))** 以取得 **[IMemoryBufferByteAccess](/previous-versions/mt297505(v=vs.85))** COM 介面，可讓我們透過 **char** 陣列取得像素資料緩衝區。 最後，呼叫 **[IMemoryBufferByteAccess::GetBuffer](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer)** 填入 **char** 陣列。 如果此方法中的任一轉換步驟失敗，方法會傳回 **false**，指出無法繼續進一步處理。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperGetPointerToPixelData":::

接下來，新增方法 **TryConvert**，如下所示。 此方法接受 **SoftwareBitmap**，並嘗試將它轉換成 **Mat** 物件，矩陣物件 OpenCV 用來代表影像資料緩衝區。 此方法會呼叫上述定義的 **GetPointerToPixelData** 方法以取得代表像素資料緩衝區的 **char**陣列。 如果此作業成功，則會呼叫 **Mat** 類別的建構函式，傳遞從來源 **SoftwareBitmap** 物件取得的像素寬度與高度。 

> [!NOTE] 
> 此範例指定 CV_8UC4 常數做為所建立 **Mat** 物件的像素格式。 這表示傳遞到此方法的 **SoftwareBitmap** 必須具有預乘 Alpha 之 **[BGRA8](/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** 的 **[BitmapPixelFormat](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** 屬性值 (CV_8UC4 的對等項目)，才能用於此範例。

方法會傳回所建立 **Mat** 物件的淺層複製，以便對 **SoftwareBitmap** 參考的相同資料像素資料緩衝區進行進一步處理，而非對此緩衝區複本進行處理。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperTryConvert":::

最後，此範例協助程式類別會實作單一影像處理方法 **Blur**，只使用上述定義的 **TryConvert** 方法來擷取代表來源點陣圖的 **Mat** 物件和模糊作業的目標，接著從 OpenCV ImgProc 程式庫呼叫 **blur** 方法。 **blur** 的另一個參數以 X 和 Y 方向指定模糊效果的大小。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperBlur":::


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>使用協助程式元件的簡單 SoftwareBitmap OpenCV 範例
現在已經建立 OpenCVBridge 元件，我們可以建立簡單 C# 應用程式，來使用 OpenCV **blur** 方法修改 **SoftwareBitmap**。 若要從 UWP 應用程式存取 Windows 執行階段元件，您必須先新增元件的參考。 在 \[方案總管\] 中，以滑鼠右鍵按一下 UWP app 專案下方的 **\[參考\]** 節點，然後選取 **\[加入參考...\]**。在 \[參考管理員\] 對話方塊中，選取 **\[專案\] -> \[方案\]**。 核取 OpenCVBridge 專案旁邊的方塊，並按一下 **\[確定\]**。

以下的範例程式碼可讓使用者選取影像檔，接著使用 **[BitmapDecoder](/uwp/api/windows.graphics.imaging.bitmapencoder)** 建立影像的 **SoftwareBitmap** 表示法。 如需使用 **SoftwareBitmap** 的詳細資訊，請參閱[建立、編輯和儲存點陣圖影像](./imaging.md)。

如本文先前所述，**OpenCVHelper** 類別需要所有提供的 **SoftwareBitmap** 影像使用具有預乘 Alpha 值的 BGRA8 像素格式來進行編碼，因此如果影像尚未使用此格式，範例程式碼會呼叫 **[Convert](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** 來將影像轉換到所需的格式。

接下來，會建立 **SoftwareBitmap** 以做為模糊作業的目標。 將使用輸入影像屬性做為建構函式的引數，來建立具備相符格式的點陣圖。

會建立 **OpenCVHelper** 的新執行個體，並呼叫 **Blur** 方法來傳遞來源和目標點陣圖。 最後，建立 **SoftwareBitmapSource** 以將輸出影像指派給 XAML **Image** 控制項。

此範例程式碼會使用下列命名空間中的 Api，以及預設專案範本所包含的命名空間。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVMainPageUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVBlur":::

## <a name="related-topics"></a>相關主題

* [BitmapEncoder 選項參考](bitmapencoder-options-reference.md)
* [影像中繼資料](image-metadata.md)
 

 
