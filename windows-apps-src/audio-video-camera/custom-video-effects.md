---
description: 本文章明如何建立能實作 IBasicVideoEffect 介面以允許您為視訊串流建立自訂效果的 Windows 執行階段元件。
MS-HAID: dev\_audio\_vid\_camera.custom\_video\_effects
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 自訂視訊效果
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 40a6bd32-a756-400f-ba34-2c5f507262c0
ms.localizationpriority: medium
ms.openlocfilehash: 119d444f073c4f668dcdc63fc118ed408eca8e32
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030491"
---
# <a name="custom-video-effects"></a>自訂視訊效果




本文明如何建立能實作 [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) 介面以為視訊串流建立自訂效果的 Windows 執行階段元件。 自訂效果可以搭配數個不同的 Windows 執行階段 API 使用，其中包括能提供裝置相機存取的 [MediaCapture](/uwp/api/Windows.Media.Capture.MediaCapture)，以及能允許您由媒體剪輯建立複雜組合的 [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition)。

## <a name="add-a-custom-effect-to-your-app"></a>將自訂效果新增到您的 App


自訂視訊效果是在實作 [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) 介面之類別中定義。 此類別不能直接包含在您 App 的專案中。 您必須改為使用 Windows 執行階段元件來裝載您的視訊效果類別。

**為您的視訊效果新增 Windows 執行階段元件**

1.  在 Microsoft Visual Studio 中，開啟您的方案，移至 **[檔案** ] 功能表，然後選取 [ **加入 &gt; 新專案** ]。
2.  選取 [ **通用 Windows) ] 專案類型 (Windows 執行階段元件** 。
3.  在此範例中，請將專案命名為 *VideoEffectComponent* 。 此名稱將會由稍後的程式碼所參考。
4.  按一下 [確定]。
5.  專案範本會建立名為 Class1.cs 的類別。 在 **方案總管** 中，以滑鼠右鍵按一下 Class1.cs 的圖示，然後選取 [ **重新命名** ]。
6.  將檔案重新命名為 *ExampleVideoEffect.cs* 。 Visual Studio 將會顯示提示，詢問您是否要將所有參照更新為新的名稱。 按一下 [是]  。
7.  開啟 **ExampleVideoEffect.cs** ，並更新類別定義以執行 [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) 介面。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetImplementIBasicVideoEffect":::


您必須在您的效果類別檔案中包含下列命名空間，以存取本文章的範例中所使用的所有類型。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetEffectUsing":::


## <a name="implement-the-ibasicvideoeffect-interface-using-software-processing"></a>使用軟體處理實作 IBasicVideoEffect 介面


您的視訊效果必須實作 [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) 介面的所有方法和屬性。 本節將引導您完成此介面使用軟體處理的簡單實作。

### <a name="close-method"></a>Close 方法

系統將會在效果應該關閉時，呼叫您類別上的 [**Close**](/uwp/api/windows.media.effects.ibasicvideoeffect.close) 方法。 您應該使用此方法來處置您已建立的任何資源。 方法的引數為 [**MediaEffectClosedReason**](/uwp/api/Windows.Media.Effects.MediaEffectClosedReason)，可讓您知道效果是否正常關閉、是否有發生錯誤，或是效果是否不支援所需的編碼格式。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetClose":::


### <a name="discardqueuedframes-method"></a>DiscardQueuedFrames 方法

在您的效果應該重設時，便會呼叫 [**DiscardQueuedFrames**](/uwp/api/windows.media.effects.ibasicvideoeffect.discardqueuedframes) 方法。 此情況的其中一個典型案例是您的效果會儲存先前已處理的畫面，以用於處理目前的畫面之上。 呼叫此方法時，您應該處置先前已儲存的畫面組合。 除了針對累計的視訊畫面之外，此方法也可以用來重設任何與先前畫面相關的狀態。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetDiscardQueuedFrames":::



### <a name="isreadonly-property"></a>IsReadOnly 屬性

[**IsReadOnly**](/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly) 屬性能讓系統知道您的效果是否會寫入至效果的輸出。 如果您的 App 並不會修改視訊畫面 (例如僅針對視訊畫面執行分析的效果)，您應該將此屬性設定為 true，這將能使系統有效率地為您將畫面輸入複製到畫面輸出。

> [!TIP]
> 當 [**IsReadOnly**](/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly) 屬性設定為 true 時，系統會在呼叫 [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) 之前將輸入畫面複製到輸出畫面。 將 **IsReadOnly** 屬性設定為 true 並不會限制您在 **ProcessFrame** 中寫入至效果的輸出畫面。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetIsReadOnly":::

### <a name="setencodingproperties-method"></a>SetEncodingProperties 方法

系統會在您的效果上呼叫 [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) 來讓您知道效果正在運作之視訊串流的編碼屬性。 此方法也能夠提供用於硬體轉譯之 Direct3D 裝置的參照。 此裝置的使用方式將會在本文章稍後的硬體處理範例中提供。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSetEncodingProperties":::


### <a name="supportedencodingproperties-property"></a>SupportedEncodingProperties 屬性

系統將會檢查 [**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties) 屬性來判斷您效果所支援的編碼屬性。 請注意，如果您效果的取用者無法使用您所指定的屬性來編碼視訊，它將會在您的效果上呼叫 [**Close**](/uwp/api/windows.media.effects.ibasicvideoeffect.close) 並將您的效果從視訊管線中移除。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSupportedEncodingProperties":::


> [!NOTE] 
> 如果您從 **SupportedEncodingProperties** 傳回 [**VideoEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties) 物件的空白清單，系統預設將會使用 ARGB32 編碼。

 

### <a name="supportedmemorytypes-property"></a>SupportedMemoryTypes 屬性

系統會檢查 [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) 屬性以判斷您的效果會存取軟體記憶體中的視訊畫面格，或是硬體 (GPU) 記憶體中的視訊畫面格。 如果您回傳 [**MediaMemoryTypes.Cpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes)，您的效果將會被傳遞包含 [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 物件中之影像資料的輸入和輸出畫面格。 如果您回傳 **MediaMemoryTypes.Gpu** ，您的效果將會被傳遞包含 [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 物件中之影像資料的輸入和輸出畫面格。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSupportedMemoryTypes":::


> [!NOTE]
> 如果您指定 [**MediaMemoryTypes.GpuAndCpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes)，系統將會使用 GPU 或系統記憶體，視哪一項針對管線較有效率而定。 使用此值時，您必須檢查 [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) 方法，以查看傳遞至方法的 [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 或 [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 是否包含資料，並據此處理畫面。

 

### <a name="timeindependent-property"></a>TimeIndependent 屬性

[**TimeIndependent**](/uwp/api/windows.media.effects.ibasicvideoeffect.timeindependent) 屬性能讓系統知道您的效果不需要統一計時。 當設定為 true 時，系統將可以使用能增強效果效能的最佳化功能。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetTimeIndependent":::

### <a name="setproperties-method"></a>SetProperties 方法

[**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) 方法允許使用您效果的 App 調整效果參數。 屬性將會以包含屬性名稱和值的 [**IPropertySet**](/uwp/api/Windows.Foundation.Collections.IPropertySet) 對應進行傳遞。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSetProperties":::


此簡單範例會根據指定值使每個視訊畫面格中的像素變暗。 將會宣告屬性，並使用 TryGetValue 來取得由呼叫 App 所設定的值。 如果沒有設定任何值，將會使用 0.5 的預設值。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetFadeValue":::


### <a name="processframe-method"></a>ProcessFrame 方法

[**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) 方法可以讓您的效果修改視訊的影像資料。 此方法會於每個畫面格呼叫一次，並會被傳遞 [**ProcessVideoFrameContext**](/uwp/api/Windows.Media.Effects.ProcessVideoFrameContext) 物件。 此物件包含輸入 [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) 物件 (該物件包含要處理的傳入畫面格)，以及輸出 **VideoFrame** 物件 (您將會針對該物件寫入會傳遞至剩餘視訊管線的影像資料)。 每個 **VideoFrame** 物件皆擁有 [**SoftwareBitmap**](/uwp/api/windows.media.videoframe.softwarebitmap) 屬性及 [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface) 屬性，並由您從 [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) 屬性回傳的值來決定會使用上述哪一個參數。

此範例顯示使用軟體處理之 **ProcessFrame** 方法的簡單實作。 如需使用 [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 物件的詳細資訊，請參閱 [影像處理](imaging.md)。 本文稍後將提供使用硬體處理之 **ProcessFrame** 實作的範例。

存取 **SoftwareBitmap** 的資料緩衝區需要 COM interop，因此您應該將 **System.Runtime.InteropServices** 命名空間包含在您的效果類別檔案中。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetCOMUsing":::


將下列程式碼新增到您效果的命名空間內，以匯入存取影像緩衝區的介面。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetCOMImport":::


> [!NOTE]
> 因為此技術會存取原生、未受管理的影像緩衝區，您必須將您的專案設定為允許不安全的程式碼。
> 1.  在方案總管中，以滑鼠右鍵按一下 VideoEffectComponent 專案，然後選取 [ **屬性** ]。
> 2.  選取 [組建]  索引標籤。
> 3.  選取 [ **允許 unsafe 程式碼** ] 核取方塊。

 

現在您可以新增 **ProcessFrame** 方法實作。 首先，此方法會同時從輸入和輸出軟體點陣圖取得 [**BitmapBuffer**](/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) 物件。 請注意，輸出畫面格會針對寫入開啟，而輸入畫面格則會針對讀取開啟。 接下來，將會透過呼叫 [**CreateReference**](/uwp/api/windows.graphics.imaging.bitmapbuffer.createreference) 來為每個緩衝區取得 [**IMemoryBufferReference**](/uwp/api/Windows.Foundation.IMemoryBufferReference)。 然後，透過將 **IMemoryBufferReference** 物件轉型為 **IMemoryByteAccess** (於上方定義的 COM interop 介面)，然後呼叫 **GetBuffer** ，來取得實際的資料緩衝區。

現在您已取得資料緩衝區，您可以從輸入緩衝區讀取，並寫入至輸出緩衝區。 緩衝區的配置是透過呼叫 [**GetPlaneDescription**](/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription) 取得，這將能提供緩衝區寬度、Stride 及初始位移的資訊。 「每一像素位元數」是由先前透過 [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) 方法所設定的編碼屬性所決定。 緩衝區格式資訊是用來找出每個像素之緩衝區的索引。 來源緩衝區的像素值會被複製到目標緩衝區中，其色彩值會被乘以針對此效果定義的 FadeValue 屬性，來以指定的量將它們變暗。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetProcessFrameSoftwareBitmap":::


## <a name="implement-the-ibasicvideoeffect-interface-using-hardware-processing"></a>使用硬體處理實作 IBasicVideoEffect 介面


使用硬體 (GPU) 處理建立自訂視訊效果，與上述使用軟體處理的方式幾乎相同。 本節將說明使用硬體處理之效果的一些差異。 此範例使用 Win2D Windows 執行階段 API。 如需使用 Win2D 的詳細資訊，請參閱 [Win2D 文件](https://microsoft.github.io/Win2D/html/Introduction.htm)。

使用下列步驟，將 Win2D NuGet 套件新增到您依照本文開頭的 **將自訂效果新增到您的 App** 一節建立的專案。

**將 Win2D NuGet 套件新增到您的效果專案**

1.  在 **方案總管** 中，以滑鼠右鍵按一下 **VideoEffectComponent** 專案，然後選取 [ **管理 NuGet 套件** ]。
2.  在視窗頂端，選取 [ **流覽** ] 索引標籤。
3.  在搜尋方塊中輸入 **Win2D** 。
4.  選取 [ **Win2D** ]，然後在右窗格中選取 [ **安裝** ]。
5.  [ **評論變更** ] 對話方塊會顯示要安裝的套件。 按一下 [確定]。
6.  接受套件授權。

除了包含在基本專案設定中的命名空間之外，您將需要包含下列由 Win2D 提供的命名空間。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetUsingWin2D":::


由於此效果會使用 GPU 記憶體來在影像資料上運作，您應該要從 [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) 參數回傳 [**MediaMemoryTypes.Gpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSupportedMemoryTypesWin2D":::


使用 [**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties) 屬性來設定您效果會支援的編碼屬性。 使用 Win2D 時，您必須使用 ARGB32 編碼。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSupportedEncodingPropertiesWin2D":::


使用 [**SetEncodingProperties**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) 方法來從傳遞到方法中的 [**IDirect3DDevice**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DDevice) 建立新的 Win2D **CanvasDevice** 物件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSetEncodingPropertiesWin2D":::


[**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) 實作和先前的軟體處理範例相同。 此範例使用 **BlurAmount** 參數來設定 Win2D 模糊效果。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSetPropertiesWin2D":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetBlurAmountWin2D":::


最後一個步驟為實作實際處理影像資料的 [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) 方法。

透過使用 Win2D API，將會從輸入畫面格的 [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface) 參數建立 **CanvasBitmap** 。 **CanvasRenderTarget** 會從輸出畫面格的 **Direct3DSurface** 建立，而 **CanvasDrawingSession** 將會從此轉譯目標建立。 新的 Win2D **GaussianBlurEffect** 將會使用效果透過 [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) 公開的 **BlurAmount** 參數進行初始化。 最後，將會呼叫 **CanvasDrawingSession.DrawImage** 方法來使用模糊效果將輸入點陣圖繪製到轉譯目標上。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetProcessFrameWin2D":::


## <a name="adding-your-custom-effect-to-your-app"></a>將您的自訂效果新增到您的 App


如果要從您的 App 使用您的視訊效果，您必須將針對效果專案的參照新增到您的 App。

1.  在 [方案總管] 中，於您的專案下方，以滑鼠右鍵按一下 **\[參考\]** ，然後選取 **\[加入參考\]** 。
2.  展開 **\[專案\]** 索引標籤，選取 **\[方案\]** ，然後選取您效果專案名稱的核取方塊。 針對此範例，該名稱為 *VideoEffectComponent* 。
3.  按一下 [確定]。

### <a name="add-your-custom-effect-to-a-camera-video-stream"></a>將您的自訂效果新增到相機視訊串流

您可以遵循[簡單的相機預覽存取](simple-camera-preview-access.md)文章中的步驟，來從相機設定簡單的預覽串流。 遵循那些步驟將能提供您初始化的 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 物件，該物件可以用來存取相機的視訊串流。

如果要將您的自訂視訊效果新增到相機串流，請先建立新的 [**VideoEffectDefinition**](/uwp/api/Windows.Media.Effects.VideoEffectDefinition) 物件，並傳遞您效果的命名空間和類別名稱。 接著，請呼叫 **MediaCapture** 物件的 [**AddVideoEffect**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) 方法，以將您的效果新增到指定的串流。 此範例使用 [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) 值來指定要將該效果新增到預覽串流。 如果您的 App 支援視訊擷取，您也應該使用 **MediaStreamType.VideoRecord** 來將該效果新增到擷取串流。 **AddVideoEffect** 會傳回代表您自訂效果的 [**IMediaExtension**](/uwp/api/Windows.Media.IMediaExtension) 物件。 您可以使用 SetProperties 方法來為您的效果設定組態。

新增效果之後，將會呼叫 [**StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync) 以開始預覽串流。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs" id="SnippetAddVideoEffectAsync":::



### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>將您的自訂效果新增到 MediaComposition 中的短片

如需從視訊短片建立媒體組合的一般指導方針，請參閱[媒體組合和編輯](media-compositions-and-editing.md)。 下列程式碼片段示範具有自訂視訊效果之簡單媒體組合的建立方式。 透過呼叫 [**CreateFromFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromfileasync) 來建立 [**MediaClip**](/uwp/api/Windows.Media.Editing.MediaClip) 物件，傳遞使用者透過 [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 所選取的視訊檔案，該短片則新增到新的 [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition)。 接著將會建立新的 [**VideoEffectDefinition**](/uwp/api/Windows.Media.Effects.VideoEffectDefinition) 物件，並將您效果的命名空間和類別名稱傳遞給建構函式。 最後，將效果定義新增到 **MediaClip** 物件的 [**VideoEffectDefinitions**](/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) 集合。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs" id="SnippetAddEffectToComposition":::


## <a name="related-topics"></a>相關主題
* [簡單的相機預覽存取](simple-camera-preview-access.md)
* [媒體組合和編輯](media-compositions-and-editing.md)
* [Win2D 文件](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [媒體播放](media-playback.md)
