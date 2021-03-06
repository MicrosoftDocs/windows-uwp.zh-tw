---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: 本文示範如何使用手動裝置控制項來啟用美化的視訊擷取案例，包括 HDR 視訊和曝光優先順序。
title: 視訊擷取的手動相機控制項
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ac3a286a8e3961b66a8fd0e4cf20fa7665b6b081
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364011"
---
# <a name="manual-camera-controls-for-video-capture"></a>視訊擷取的手動相機控制項



本文示範如何使用手動裝置控制項來啟用美化的視訊擷取案例，包括 HDR 視訊和曝光優先順序。

本文中討論的視訊裝置控制項全都會使用相同的模式新增到您的 app。 首先，檢查 app 目前執行所在的裝置是否支援此控制項。 如果支援此控制項，則為控制項設定所需的模式。 通常，如果目前的裝置不支援特定的控制項，您應該停用或隱藏可讓使用者啟用該功能的 UI 元素。

此文章中討論的所有裝置控制項 API 都是 [**Windows.Media.Devices**](/uwp/api/Windows.Media.Devices) 命名空間的成員。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVideoControllersUsing":::

> [!NOTE] 
> 本文是以[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 我們建議您先熟悉該文章中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文中的程式碼假設您的 app 已有正確初始化的 MediaCapture 執行個體。

## <a name="hdr-video"></a>HDR 視訊

高動態範圍 (HDR) 視訊功能可將 HDR 處理套用至擷取裝置的視訊資料流。 選取 [**HdrVideoControl.Supported**](/uwp/api/windows.media.devices.hdrvideocontrol.supported) 屬性，以判斷是否支援 HDR 視訊。

HDR 視訊控制項支援開啟、關閉和自動三種模式，這表示裝置會動態判斷 HDR 視訊處理是否可改進媒體擷取，若是可以，則會啟用 HDR 視訊。 若要判斷目前的裝置是否支援特定的模式，請查看 [**HdrVideoControl.SupportedModes**](/uwp/api/windows.media.devices.hdrvideocontrol.supportedmodes) 集合是否包含所需的模式。

將 [**HdrVideoControl.Mode**](/uwp/api/windows.media.devices.hdrvideocontrol.mode) 設定為所需的模式，以啟用或停用 HDR 視訊處理。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSetHdrVideoMode":::

## <a name="exposure-priority"></a>曝光優先順序

啟用 [**ExposurePriorityVideoControl**](/uwp/api/Windows.Media.Devices.ExposurePriorityVideoControl) 後，會評估來自擷取裝置的視訊框架，以判斷視訊是否擷取光線不足的場景。 若是如此，此控制項會降低所擷取視訊的框架速度，以增加每個框架的曝光時間並改善所擷取視訊的視覺品質。

檢查 [**ExposurePriorityVideoControl.Supported**](/uwp/api/windows.media.devices.exposurepriorityvideocontrol.supported) 屬性，以判斷目前的裝置是否支援曝光優先順序控制項。

將 [**ExposurePriorityVideoControl.Enabled**](/uwp/api/windows.media.devices.exposurepriorityvideocontrol.enabled) 設定為所需的模式，以啟用或停用曝光優先順序控制項。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetEnableExposurePriority":::

## <a name="temporal-denoising"></a>時態性去雜訊
從 Windows 10 版本 1803 開始，您可以在支援時態性去雜訊的裝置上為影片啟用此功能。 這項功能可即時融合多個相鄰畫面的影像資料，製作較少視覺雜訊的視訊畫面。

[**VideoTemporalDenoisingControl**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol) 可讓您的應用程式判斷目前裝置是否支援時態性去雜訊，而如果支援，是支援哪種去雜訊模式。 可用的去雜訊模式為 [**Off**](/uwp/api/windows.media.devices.videotemporaldenoisingmode)、[**On**](/uwp/api/windows.media.devices.videotemporaldenoisingmode) 和 [**Auto**](/uwp/api/windows.media.devices.videotemporaldenoisingmode)。一部裝置可能無法支援所有模式，但必須支援 **Auto** 或 **On** 及 **Off**。

下列範例使用簡單的 UI 來提供選項按鈕，讓使用者在去雜訊模式之間切換。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetDenoiseXAML":::

在下列方法中，會檢查 [**VideoTemporalDenoisingControl.Supported**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) 屬性，以查看目前的裝置是否支援時態性去雜訊。 如果支援，接著檢查以確定支援的是 **Off** 和 **Auto** 或 **On**，以便顯示相應的選項按鈕。 接著，如果支援 **Auto** 和 **On** 方法則顯示這些按鈕。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetUpdateDenoiseCapabilities":::

在選項按鈕的 **Checked** 事件處理常式中，透過設定 [**VideoTemporalDenoisingControl.Mode**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode) 屬性來檢查按鈕的名稱並設定對應模式。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseButtonChecked":::

### <a name="disabling-temporal-denoising-while-processing-frames"></a>處理畫面時停用時態性去雜訊
使用時態性去雜訊處理過的影片，看起來會更賞心悅目。 不過，因為時態性去雜訊會影響影像一致性並降低畫面細節，在畫面上執行影像處理 (例如註冊或光學字元辨識) 的應用程式可能會想在啟用影像處理時，以程式設計方式停用時態性去雜訊。

下列範例會判斷支援哪種去雜訊模式，並將這項資訊儲存在某些類別變數中。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseFrameReaderVars":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseCapabilitiesForFrameProcessing":::

當應用程式啟用畫面處理時，它會將支援的去雜訊模式設定為 **Off**，讓畫面處理程序可以使用尚未去雜訊的原始畫面。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEnableFrameProcessing":::

當應用程式停用畫面處理時，它會視所支援的模式，將去雜訊模式設定為 **On** 或 **Auto**。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDisableFrameProcessing":::

如需取得用於影像處理之視訊框架的詳細資訊，請參閱[使用 MediaFrameReader 處理媒體畫面](process-media-frames-with-mediaframereader.md)。

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [使用 MediaFrameReader 處理媒體畫面](process-media-frames-with-mediaframereader.md)
*  [**VideoTemporalDenoisingControl**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 
