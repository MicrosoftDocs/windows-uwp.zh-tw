---
ms.assetid: B5D915E4-4280-422C-BA0E-D574C534410B
description: 本文說明如何使用 SceneAnalysisEffect 和FaceDetectionEffect 分析媒體擷取預覽串流的內容。
title: 分析相機畫面的效果
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 56f828268980eb7ccc63a84729365bb5d17a609c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363751"
---
# <a name="effects-for-analyzing-camera-frames"></a>分析相機畫面的效果



本文說明如何使用 [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 和 [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) 來分析 media capture preview 資料流程的內容。

## <a name="scene-analysis-effect"></a>場景分析效果

[**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 會分析媒體擷取預覽串流的視訊框架，並且建議處理選項以改善擷取結果。 目前效果支援偵測擷取是否已使用高動態範圍 (HDR) 處理獲得改善。

如果效果建議使用 HDR，您可以透過下列方式進行：

-   使用 [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 類別，使用 Windows 內建 HDR 處理演算法來擷取相片。 如需詳細資訊，請參閱[高動態範圍 (HDR) 相片擷取](high-dynamic-range-hdr-photo-capture.md)。

-   使用 [**HdrVideoControl**](/uwp/api/Windows.Media.Devices.HdrVideoControl)，以使用 Windows 內建 HDR 處理演算法來擷取視訊。 如需詳細資訊，請參閱[視訊擷取的擷取裝置控制項](capture-device-controls-for-video-capture.md)。

-   使用 [**VariablePhotoSequenceControl**](/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) 擷取一系列的畫面，您可以接著使用自訂的 HDR 實作進行組合。 如需詳細資訊，請參閱[可變相片序列](variable-photo-sequence.md)。

### <a name="scene-analysis-namespaces"></a>場景分析命名空間

若要使用場景分析，您的 app 除了基本媒體擷取所需的命名空間之外，還必須包含下列命名空間。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSceneAnalysisUsing":::

### <a name="initialize-the-scene-analysis-effect-and-add-it-to-the-preview-stream"></a>初始化場景分析效果並將它新增至預覽串流

視訊效果是使用以下兩個 API 進行實作：效果定義，提供擷取裝置初始化效果所需的設定，以及效果執行個體，它可以用來控制效果。 因為您可能會想要從您的程式碼中的多個位置存取效果執行個體，所以您通常應該宣告成員變數來保存物件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareSceneAnalysisEffect":::

在您的 app 中初始化 **MediaCapture** 物件之後，建立 [**SceneAnalysisEffectDefinition**](/uwp/api/Windows.Media.Core.SceneAnalysisEffectDefinition) 的新執行個體。

使用擷取裝置註冊效果，方法是在您的 **MediaCapture** 物件上呼叫 [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync)，提供 **SceneAnalysisEffectDefinition** 並且指定 [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) 以表示效果應該套用至影片預覽串流，而不是擷取串流。 **AddVideoEffectAsync** 會傳回新增之效果的執行個體。 因為這個方法可以用於多個效果類型，所以您必須將傳回的執行個體轉換為 [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 物件。

若要接收場景分析的結果，您必須註冊 [**SceneAnalyzed**](/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed) 事件的處理常式。

目前場景分析效果僅包含高動態範圍分析程式。 將效果的 [**HighDynamicRangeControl.Enabled**](/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) 設為 true 以啟用 HDR 分析。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateSceneAnalysisEffectAsync":::

### <a name="implement-the-sceneanalyzed-event-handler"></a>實作 SceneAnalyzed 事件處理常式

場景分析的結果會在 **SceneAnalyzed** 事件處理常式中傳回。 傳入處理常式中的 [**SceneAnalyzedEventArgs**](/uwp/api/Windows.Media.Core.SceneAnalyzedEventArgs) 物件有 [**SceneAnalysisEffectFrame**](/uwp/api/Windows.Media.Core.SceneAnalysisEffectFrame) 物件，該物件有 [**HighDynamicRangeOutput**](/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) 物件。 高動態範圍輸出的 [**Certainty**](/uwp/api/windows.media.core.highdynamicrangeoutput.certainty) 屬性提供一個介於 0 到 1.0 的值，其中 0 表示 HDR 處理不會協助改善擷取結果，1.0 表示 HDR 處理會協助。 您可以決定您想要使用 HDR 的閾值點，或對使用者顯示結果並讓使用者決定。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSceneAnalyzed":::

傳入處理常式的 [**HighDynamicRangeOutput**](/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) 物件也有 [**FrameControllers**](/uwp/api/windows.media.core.highdynamicrangeoutput.framecontrollers) 屬性，包含建議的框架控制項，以針對 HDR 處理擷取可變相片序列。 如需詳細資訊，請參閱[可變相片序列](variable-photo-sequence.md)。

### <a name="clean-up-the-scene-analysis-effect"></a>清除場景分析效果

當您的 app 完成擷取時，在處置 **MediaCapture** 物件之前，您應該將效果的 [**HighDynamicRangeAnalyzer.Enabled**](/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) 屬性設為 false，以停用場景分析效果，並且取消註冊您的 [**SceneAnalyzed**](/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed) 事件處理常式。 呼叫 [**MediaCapture.ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync)，指定影片預覽串流，因為那是要新增效果的串流。 最後，將您的成員變數設為 null。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpSceneAnalysisEffectAsync":::

## <a name="face-detection-effect"></a>臉部偵測效果

[**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) 會識別媒體擷取預覽串流中的臉部位置。 效果可以讓您只要在預覽串流中偵測到臉部時，就會收到通知，並且在預覽框架內針對每個偵測到的臉部提供界限方塊。 在支援的裝置中，臉部偵測效果也會提供增強的曝光度，並且將焦點放在場景中最重要的臉部。

### <a name="face-detection-namespaces"></a>臉部偵測命名空間

若要使用臉部偵測，您的 app 除了基本媒體擷取所需的命名空間之外，還必須包含下列命名空間。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFaceDetectionUsing":::

### <a name="initialize-the-face-detection-effect-and-add-it-to-the-preview-stream"></a>初始化臉部偵測效果並將它新增至預覽串流

視訊效果是使用以下兩個 API 進行實作：效果定義，提供擷取裝置初始化效果所需的設定，以及效果執行個體，它可以用來控制效果。 因為您可能會想要從您的程式碼中的多個位置存取效果執行個體，所以您通常應該宣告成員變數來保存物件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareFaceDetectionEffect":::

在您的 app 中初始化 **MediaCapture** 物件之後，建立 [**FaceDetectionEffectDefinition**](/uwp/api/Windows.Media.Core.FaceDetectionEffectDefinition) 的新執行個體。 設定 [**DetectionMode**](/uwp/api/windows.media.core.facedetectioneffectdefinition.detectionmode) 屬性以設定優先順序，讓臉部偵測更快速或者準確度更高。 設定 [**SynchronousDetectionEnabled**](/uwp/api/windows.media.core.facedetectioneffectdefinition.synchronousdetectionenabled) 以指定傳入框架不因為等待臉部偵測完成而延遲，因為這可能會導致不穩定的預覽經驗。

使用擷取裝置註冊效果，方法是在您的 **MediaCapture** 物件上呼叫 [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync)，提供 **FaceDetectionEffectDefinition** 並且指定 [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) 以表示效果應該套用至影片預覽串流，而不是擷取串流。 **AddVideoEffectAsync** 會傳回新增之效果的執行個體。 因為這個方法可以用於多個效果類型，所以您必須將傳回的執行個體轉換為 [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) 物件。

藉由設定 [**FaceDetectionEffect.Enabled**](/uwp/api/windows.media.core.facedetectioneffect.enabled) 屬性，即可啟用或停用這些效果。 藉由設定 [**FaceDetectionEffect.DesiredDetectionInterval**](/uwp/api/windows.media.core.facedetectioneffect.desireddetectioninterval) 屬性，調整效果分析框架的頻率。 媒體擷取正在進行時可以調整這兩個屬性。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateFaceDetectionEffectAsync":::

### <a name="receive-notifications-when-faces-are-detected"></a>偵測到臉部時接收通知

如果您想要在偵測到臉部時執行某些動作，例如於影片預覽中在偵測到的臉部周圍繪製方塊，您可以註冊 [**FaceDetected**](/uwp/api/windows.media.core.facedetectioneffect.facedetected) 事件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterFaceDetectionHandler":::

在事件處理常式中，您可以取得框架中偵測到的所有臉部的清單，方法是存取 [**FaceDetectedEventArgs**](/uwp/api/Windows.Media.Core.FaceDetectedEventArgs) 的 [**FaceDetectionEffectFrame.DetectedFaces**](/uwp/api/windows.media.core.facedetectioneffectframe.detectedfaces) 屬性。 [**FaceBox**](/uwp/api/windows.media.faceanalysis.detectedface.facebox) 屬性是 [**BitmapBounds**](/uwp/api/Windows.Graphics.Imaging.BitmapBounds) 結構，以預覽串流維度相對的單位來描述包含所偵測臉部的矩形。 若要檢視會將預覽串流座標轉換成畫面座標的範例程式碼，請參閱[臉部偵測 UWP 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFaceDetected":::

### <a name="clean-up-the-face-detection-effect"></a>清除臉部偵測效果

當您的 app 完成擷取時，在處置 **MediaCapture** 物件之前，您應該使用 [**FaceDetectionEffect.Enabled**](/uwp/api/windows.media.core.facedetectioneffect.enabled) 以停用臉部偵測效果，並且取消註冊您的 [**FaceDetected**](/uwp/api/windows.media.core.facedetectioneffect.facedetected) 事件處理常式 (如果您之前已經註冊一個事件處理常式)。 呼叫 [**MediaCapture.ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync)，指定影片預覽串流，因為那是要新增效果的串流。 最後，將您的成員變數設為 null。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpFaceDetectionEffectAsync":::

### <a name="check-for-focus-and-exposure-support-for-detected-faces"></a>檢查偵測到的臉部的對焦和曝光支援

並非所有裝置都有可以根據偵測到的臉部調整其焦點和曝光的擷取裝置。 因為臉部偵測會耗用裝置資源，所以您可能只想要在可以使用功能以增強擷取的裝置上啟用臉部偵測。 若要查看臉部型擷取最佳化是否可用，取得適用於您的初始化 [MediaCapture](./index.md) 的 [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController)，然後取得影片裝置控制器的 [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl)。 檢查以查看 [**MaxRegions**](/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) 是否支援至少一個區域。 然後檢查以查看 [**AutoExposureSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autoexposuresupported) 或 [**AutoFocusSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) 是否為 true。 如果符合這些條件，則裝置可以利用臉部偵測來增強擷取。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetAreFaceFocusAndExposureSupported":::

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
