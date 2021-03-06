---
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: 此文章討論如何使用相機設定檔來探索和管理不同視訊擷取裝置的功能。 這其中包括如下的工作：選取支援特定解析度或畫面播放速率的設定檔、選取支援可同時存取多台相機的設定檔，以及選取支援 HDR 的設定檔。
title: 使用相機設定檔探索並選取相機功能
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f42b58794b62753ff325cb4fc23202e5a48047d4
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364021"
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>使用相機設定檔探索並選取相機功能



此文章討論如何使用相機設定檔來探索和管理不同視訊擷取裝置的功能。 這其中包括如下的工作：選取支援特定解析度或畫面播放速率的設定檔、選取支援可同時存取多台相機的設定檔，以及選取支援 HDR 的設定檔。

> [!NOTE] 
> 本文是以[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 建議您先熟悉該文中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文中的程式碼假設您的 app 已有正確初始化的 MediaCapture 執行個體。

 

## <a name="about-camera-profiles"></a>關於相機設定檔

不同裝置上的相機支援不同的功能，包括支援的擷取解析度、視訊擷取的畫面播放速率，以及是否支援 HDR 或可變畫面播放速率擷取。 通用 Windows 平台 (UWP) 媒體擷取架構將這組功能存放在 [**MediaCaptureVideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription) 中。 以 [**MediaCaptureVideoProfile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) 物件表示的相機設定檔有三個媒體描述集合；一個用於相片擷取、一個用於視訊擷取，另一個用於視訊預覽。

在初始化 [MediaCapture](./index.md) 物件之前，您可以在目前裝置上查詢擷取裝置，以查看支援哪些設定檔。 當您選取支援的設定檔時，您知道此擷取裝置支援設定檔的媒體描述中的所有功能。 這樣就不需要試用與錯誤方法來判斷特定裝置上支援哪些功能組合。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetBasicInitExample":::

本文中的程式碼範例透過探索支援各種功能的相機設定檔來取代此最小初始化，然後會使用這些相機設定檔來初始化媒體擷取裝置。

## <a name="find-a-video-device-that-supports-camera-profiles"></a>尋找可支援相機設定檔的視訊裝置

搜尋支援的相機設定檔之前，您應該尋找可支援使用相機設定檔的擷取裝置。 以下範例中定義的 **GetVideoProfileSupportedDeviceIdAsync** 協助程式方法使用 [**DeviceInformaion.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 方法，擷取所有可用的視訊擷取裝置清單。 它會循環顯示清單中的所有裝置，並對每個裝置呼叫靜態方法 ([**IsVideoProfileSupported**](/uwp/api/windows.media.capture.mediacapture.isvideoprofilesupported))，以查看是否支援視訊設定檔。 此外，每個裝置的 [**EnclosureLocation.Panel**](/uwp/api/windows.devices.enumeration.enclosurelocation.panel) 屬性可讓您指定您希望相機在裝置的正面或背面。

如果在指定的面板上找到支援相機設定檔的裝置，則會傳回包含裝置識別碼字串的 [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) 值。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetVideoProfileSupportedDeviceIdAsync":::

如果 **GetVideoProfileSupportedDeviceIdAsync** 協助程式方法傳回的裝置識別碼為 null 或空字串，表示指定的面板上沒有任何支援相機設定檔的裝置。 在此情況下，您應該在不使用設定檔的情況下初始化媒體擷取裝置。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetDeviceWithProfileSupport":::

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>根據支援的解析度和畫面播放速率選取設定檔

若要選取具有特定功能 (例如有達到特定解析度和畫面播放速率的能力) 的設定檔，您應該先呼叫上面定義的協助程式方法，以取得支援使用相機設定檔的擷取裝置識別碼。

建立新的 [**MediaCaptureInitializationSettings**](/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings) 物件，並傳入選取的裝置識別碼。 接著，呼叫靜態方法 [**MediaCapture.FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles)，以取得裝置支援的所有相機設定檔清單。

這個範例使用 Linq 查詢方法 (包含在 using**System.Linq** 命名空間中) 來選取包含 [**SupportedRecordMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) 物件的設定檔，其中 [**Width**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.width), [**Height**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.height) 和 [**FrameRate**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.framerate) 屬性符合要求的值。 如果找到相符的值，則 **MediaCaptureInitializationSettings** 的 [**VideoProfile**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videoprofile) 和 [**RecordMediaDescription**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.recordmediadescription) 會設定為 Linq 查詢所傳回之匿名類型中的值。 如果找不到相符的值，則會使用預設設定檔。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindWVGA30FPSProfile":::

以您所需的相機設定檔填入 **MediaCaptureInitializationSettings** 後，您只需在媒體擷取物件上呼叫 [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) 來將它設為所需的設定檔即可。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetInitCaptureWithProfile":::

## <a name="use-media-frame-source-groups-to-get-profiles"></a>使用媒體畫面來源群組取得設定檔

從 Windows 10 版本 1803 開始，在初始化 **MediaCapture** 物件前，您可以使用 [**MediaFrameSourceGroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup) 類別來取得具有特殊功能的相機設定檔。 畫面來源群組可讓裝置製造商將一組感應器或擷取功能表示為單一虛擬裝置。 這樣可以啟用計算攝影案例，例如一起使用景深和彩色相機，但也可以用來選取簡單擷取案例的相機設定檔。 如需使用 **MediaFrameSourceGroup** 的詳細資訊，請參閱[使用 MediaFrameReader 處理媒體畫面](process-media-frames-with-mediaframereader.md)。

以下範例方法示範如何使用 **MediaFrameSourceGroup** 物件來尋找支援已知視訊設定檔 (例如支援 HDR 或可變相片序列) 的相機設定檔。 首先，呼叫 [**MediaFrameSourceGroup.FindAllAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) 以取得目前裝置上可用的所有媒體畫面來源群組清單。 循環顯示每個來源群組並呼叫 [**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) 以取得支援所指定設定檔之目前來源群組的所有視訊設定檔清單，在此例中是 HDR 含 WCG 相片。 如果找到符合條件的設定檔，則建立新的 **MediaCaptureInitializationSettings** 物件，並將 **VideoProfile** 設定為所選設定檔以及將 **VideoDeviceId** 設定為目前媒體畫面來源群組的 **Id** 屬性。 因此，舉例來說，您可以傳遞 **KnownVideoProfile.HdrWithWcgVideo** 值到此方法以取得支援 HDR 影片的媒體擷取設定。 傳遞 **KnownVideoProfile.VariablePhotoSequence** 以取得支援可變相片序列的設定。

 :::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindKnownVideoProfile":::

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video-legacy-technique"></a>使用已知的設定檔來尋找支援 HDR 視訊的設定檔 (舊版技術)

> [!NOTE] 
> 這一節中所述的 API 從 Windows 10 版本 1803 起已過時。 請參閱上一節，**使用媒體畫面來源群組取得設定檔**。

像其他案例一樣，開始選取支援 HDR 的設定檔。 建立 **MediaCaptureInitializationSettings** 和字串來保存 CAPTURE 裝置識別碼。 新增布林值變數，以追蹤是否支援 HDR 視訊。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetHdrProfileSetup":::

使用上面定義的 **GetVideoProfileSupportedDeviceIdAsync** 協助程式方法，取得可支援相機設定檔之擷取裝置的裝置識別碼。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindDeviceHDR":::

靜態方法 [**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) 會傳回由指定的裝置支援並依指定的 [**KnownVideoProfile**](/uwp/api/Windows.Media.Capture.KnownVideoProfile) 值分類的相機設定檔。 在這個案例中，指定 **VideoRecording** 值可將傳回的相機設定檔限制為支援視訊錄製的設定檔。

循環顯示傳回的相機設定檔清單。 對於每個相機設定檔，在設定檔檢查中循環顯示每個 [**VideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription)，以查看 [**IsHdrVideoSupported**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.ishdrvideosupported) 屬性是否為 true。 找到合適的媒體描述後，請中斷迴圈並將設定檔和描述物件指派給 **MediaCaptureInitializationSettings** 物件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindHDRProfile":::

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>判斷裝置是否支援同時相片和視訊擷取

許多裝置都支援同時擷取相片和視訊。 若要判斷擷取裝置是否支援此功能，請呼叫 [**MediaCapture.FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) 以取得裝置所支援的所有相機設定檔。 使用連結查詢來尋找至少有一個 [**SupportedPhotoMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedphotomediadescription) 項目和一個 [**SupportedRecordMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) 項目的設定檔，這表示此設定檔可支援同時擷取。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetPhotoAndVideoSupport":::

您可以修改此查詢，以尋找支援特定解析度或其他功能 (同時視訊錄製除外) 的設定檔。 您也可以使用 [**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) 並指定 [**BalancedVideoAndPhoto**](/uwp/api/Windows.Media.Capture.KnownVideoProfile) 值來擷取支援同時擷取的設定檔，但查詢所有設定檔會提供更完整的結果。

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
