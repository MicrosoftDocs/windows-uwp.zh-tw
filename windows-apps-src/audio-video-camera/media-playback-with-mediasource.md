---
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: 本文示範如何使用 MediaSource 提供一個常見的方法來參考和播放來自不同來源 (例如本機或遠端檔案) 的媒體，並且公開一個常見的模型來存取媒體資料 (不論使用什麼基礎媒體格式)。
title: 媒體項目、播放清單與曲目
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1f428bb8beb4bb933387a77e5a74819016a4c64
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363881"
---
# <a name="media-items-playlists-and-tracks"></a>媒體項目、播放清單與曲目


 本文示範如何使用 [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) 類別提供一個常見的方法來參考和播放來自不同來源 (例如本機或遠端檔案) 的媒體，並且公開一個常見的模型來存取媒體資料 (不論使用什麼基礎媒體格式)。 [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 類別延伸了 **MediaSource** 的功能，可讓您針對包含在媒體項目中的多個音訊、視訊及中繼資料播放軌進行管理和選取。 [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 可讓您從一或多個媒體播放項目建立播放清單。


## <a name="create-and-play-a-mediasource"></a>建立和播放 MediaSource

呼叫類別所公開的其中一個 Factory 方法以建立新的 **MediaSource** 執行個體：

-   [**CreateFromAdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.createfromadaptivemediasource)
-   [**CreateFromIMediaSource**](/uwp/api/windows.media.core.mediasource.createfromimediasource)
-   [**CreateFromMediaStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommediastreamsource)
-   [**CreateFromMseStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommsestreamsource)
-   [**CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile)
-   [**CreateFromStream**](/uwp/api/windows.media.core.mediasource.createfromstream)
-   [**CreateFromStreamReference**](/uwp/api/windows.media.core.mediasource.createfromstreamreference)
-   [**CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri)
-   [**CreateFromDownloadOperation**](/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

建立 **MediaSource** 之後，透過設定 [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) 屬性，即可使用 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 進行播放。 從 Windows 10 版本 1607 開始，您可以透過呼叫 [**SetMediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer) 來指派 **MediaPlayer** 到 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)，以在 XAML 頁面中轉譯媒體播放程式內容。 這是優於使用 **MediaElement** 的慣用方法。 如需使用 **MediaPlayer** 的詳細資訊，請參閱[**使用 MediaPlayer 播放音訊和視訊**](play-audio-and-video-with-mediaplayer.md)。

下列範例示範如何使用 **MediaSource** 來播放 **MediaPlayer** 中使用者選取的媒體檔案。

您將需要包含 [**Windows.Media.Core**](/uwp/api/Windows.Media.Core) 和 [**Windows.Media.Playback**](/uwp/api/Windows.Media.Playback) 命名空間，才能完成這個案例。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetUsing":::

宣告類型為 **MediaSource** 的變數。 在本文的範例中，是將媒體來源宣告為類別成員，以便能夠從多個位置存取它。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaSource":::

宣告一個變數以儲存 **MediaPlayer** 物件，另外，如果您想以 XAML 轉譯媒體內容，請在您的頁面新增一個 **MediaPlayerElement** 控制項。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlayer":::

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetMediaPlayerElement":::

若要允許使用者挑選要播放的媒體檔案，請使用 [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)。 使用從選擇器的 [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) 方法傳回的 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 物件，藉由呼叫 [**MediaSource.CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile)，初始化一個新的 MediaObject。 最後，藉由呼叫 [**SetPlaybackSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.setplaybacksource) 方法，將媒體來源設定為 **MediaElement** 的播放來源。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaSource":::

根據預設，設定媒體來源後，**MediaPlayer** 不會自動開始播放。 您可以透過呼叫 [**Play**](/uwp/api/windows.media.playback.mediaplayer.play)，以手動方式開始播放。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlay":::

您也可以將 **MediaPlayer** 的 [**AutoPlay**](/uwp/api/windows.media.playback.mediaplayer.autoplay) 屬性設為 true，以告知玩家媒體來源設定後即會開始播放。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAutoPlay":::

### <a name="create-a-mediasource-from-a-downloadoperation"></a>從 DownloadOperation 建立 MediaSource
從 Windows 版本 1803 開始，您可以從 **DownloadOperation** 建立 **MediaSource** 物件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetCreateMediaSourceFromDownload":::

請注意，雖然您可以從下載建立 **MediaSource** 而不啟動它或將其 **IsRandomAccessRequired** 屬性設定為 true，但您仍必須執行這兩項作業，才能嘗試將 **MediaSource** 連接到 **MediaPlayer** 或 **MediaPlayerElement** 以進行播放。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetStartDownload":::


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>使用 MediaPlaybackItem 處理多個音訊、視訊及中繼資料曲目

使用 [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) 來進行播放相當便利，因為它提供一個可從不同種類的來源播放媒體的常見方法，但是從 **MediaSource** 建立 [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 則可執行更進階的動作。 這包括能夠存取和管理媒體項目的多個音訊、視訊及資料曲目。

宣告變數來儲存您的 **MediaPlaybackItem**。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlaybackItem":::

呼叫建構函式並傳入已初始化的 **MediaSource** 物件來建立 **MediaPlaybackItem**。

如果您的 app 支援一個媒體播放項目中有多個音訊、視訊或資料播放軌，請為 [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged)、[**VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged) 或 [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged) 事件註冊事件處理常式。

最後，將 **MediaElement** 或 **MediaPlayer** 的播放來源設定為您的 **MediaPlaybackItem**。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaPlaybackItem":::

> [!NOTE] 
> 一個 **MediaSource** 只能與一個 **MediaPlaybackItem** 關聯。 從來源建立 **MediaPlaybackItem** 之後，嘗試從相同的來源建立另一個播放項目將會導致錯誤。 此外，從媒體來源建立 **MediaPlaybackItem** 之後，您就不能將 **MediaSource** 物件直接設定為 **MediaPlayer** 的來源物件，而是應該改用 **MediaPlaybackItem**。

將包含多個視訊曲目的 **MediaPlaybackItem** 指派為播放來源之後，將會引發 [**VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged) 事件，並且如果因項目變更而導致視訊曲目清單發生變更，就會再次引發該事件。 這個事件的處理常式可讓您更新 UI，以允許使用者在可用的播放軌之間切換。 這個範例使用 [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 來顯示可用的視訊播放軌。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetVideoComboBox":::

請在 **VideoTracksChanged** 處理常式中，循環處理播放項目之 [**VideoTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.videotracks) 清單中的所有播放軌。 針對每個播放軌，將會建立一個新的 [**ComboBoxItem**](/uwp/api/Windows.UI.Xaml.Controls.ComboBoxItem)。 如果播放軌尚未擁有標籤，將會從播放軌索引產生標籤。 下拉式方塊項目的 [**Tag**](/uwp/api/windows.ui.xaml.frameworkelement.tag) 屬性是設定為播放軌索引，以便稍後可供識別。 最後，該項目會新增到下拉式方塊中。 請注意，這些操作是在 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 呼叫內執行，因為所有 UI 變更都必須在 UI 執行緒上進行，而這個事件是在不同的執行緒上引發。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetVideoTracksChanged":::

在下拉式方塊的 [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 處理常式中，會從所選項目的 **Tag** 屬性抓取播放軌索引。 設定媒體播放項目之 [**VideoTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.videotracks) 清單的 [**SelectedIndex**](/uwp/api/windows.media.playback.mediaplaybackvideotracklist.selectedindex) 屬性會使 **MediaElement** 或 **MediaPlayer** 將使用中的視訊播放軌切換到指定的索引。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetVideoTracksSelectionChanged":::

管理含有多個音軌的媒體項目與管理含有視訊播放軌的媒體項目方式完全相同。 請處理 [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged)，以使用在播放項目的 [**AudioTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks) 清單中找到的音軌更新您的 UI。 當使用者選取音軌時，設定 **AudioTracks** 清單的 [**SelectedIndex**](/uwp/api/windows.media.playback.mediaplaybackaudiotracklist.selectedindex) 屬性以使 **MediaElement** 或 **MediaPlayer** 將使用中的音軌切換到指定的索引。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetAudioComboBox":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksChanged":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksSelectionChanged":::

除了音訊和視訊之外，**MediaPlaybackItem** 物件可能會包含零個或多個 [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack) 物件。 定時中繼資料播放軌可以包含字幕或輔助字幕文字，也可以包含您 app 專屬的自訂資料。 定時中繼資料播放軌包含由繼承 [**IMediaCue**](/uwp/api/Windows.Media.Core.IMediaCue) 的物件 (例如 [**DataCue**](/uwp/api/Windows.Media.Core.DataCue) 或 [**TimedTextCue**](/uwp/api/Windows.Media.Core.TimedTextCue)) 所代表的提示清單。 每個提示都有一個開始時間和持續時間，用來決定何時啟用提示及要持續多久。

與音軌和視訊播放軌類似，透過處理 **MediaPlaybackItem** 的 [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged) 事件，即可探索媒體項目的定時中繼資料播放軌。 不過，使用定時中繼資料播放軌時，使用者可能會想要一次啟用多個中繼資料播放軌。 此外，視您的 app 情況而定，您可能會想要自動啟用或停用中繼資料播放軌，而不需要使用者介入。 為了方便說明，這個範例會為媒體項目中的每個中繼資料播放軌新增 [**ToggleButton**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton)，以允許使用者啟用和停用播放軌。每個按鈕的 **Tag** 屬性都設定為相關中繼資料播放軌的索引，以便在切換按鈕時能夠加以識別。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetMetaStackPanel":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedMetadataTrackschanged":::

由於一次可以有多個中繼資料播放軌處於使用中，因此您不是直接設定中繼資料播放軌清單的使用中索引。 而是改為呼叫 **MediaPlaybackItem** 物件的 [**SetPresentationMode**](/previous-versions/windows/dn986977(v=win.10)) 方法，藉此傳入您想要切換之播放軌的索引，然後從 [**TimedMetadataTrackPresentationMode**](/uwp/api/Windows.Media.Playback.TimedMetadataTrackPresentationMode) 列舉提供值。 您選擇的呈現模式取決於您 app 的實作。 在這個範例中，中繼資料播放軌在啟用時會設定為 **PlatformPresented**。 就文字型播放軌而言，這表示系統會自動顯示播放軌中的文字提示。當切換按鈕被關閉時，呈現模式會設定成 **Disabled**，這表示不會顯示任何文字，也不會引發任何提示事件。 本文稍後會討論提示事件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetToggleChecked":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetToggleUnchecked":::

當您處理中繼資料曲目時，您可以透過存取 [**Cues**](/uwp/api/windows.media.core.timedmetadatatrack.cues) 或 [**ActiveCues**](/uwp/api/windows.media.core.timedmetadatatrack.activecues) 屬性來存取曲目內的提示集。 這樣做可以更新 UI 以顯示媒體項目的提示位置。

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>處理開啟媒體項目時不支援的轉碼器和不明的錯誤
從 Windows 10 版本 1607 開始，您可以檢查執行您的應用程式所在的裝置是否支援或部分支援播放媒體項目需要的轉碼器。 在 **MediaPlaybackItem** 曲目變更事件 (例如 [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged)) 的事件處理常式中，先檢查曲目變更是否為插入新曲目。如果是，您可以透過使用在 **IVectorChangedEventArgs.Index** 參數中傳入的索引與適當的 **MediaPlaybackItem** 參數曲目集合 (例如 [**AudioTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks) 集合)，以取得插入中曲目的參照。

取得已插入曲目的參照後，請檢查該曲目的 [**SupportInfo**](/uwp/api/windows.media.core.audiotrack.supportinfo) 屬性的 [**DecoderStatus**](/uwp/api/windows.media.core.audiotracksupportinfo.decoderstatus)。 如果值為 [**FullySupported**](/uwp/api/Windows.Media.Core.MediaDecoderStatus)，則播放曲目需要的適當轉碼器會顯示在裝置上。 如果值為 [**Degraded**](/uwp/api/Windows.Media.Core.MediaDecoderStatus)，系統雖然可以播放曲目，但播放品質會降低。 例如，5.1 的音訊曲目可能會改播放為雙通道立體聲。 如果是這種情況，您可能想更新您的 UI 以提醒使用者播放品質降低。 如果值為 [**UnsupportedSubtype**](/uwp/api/Windows.Media.Core.MediaDecoderStatus) 或 [**UnsupportedEncoderProperties**](/uwp/api/Windows.Media.Core.MediaDecoderStatus)，則無法使用裝置上目前的轉碼器來播放曲目。 您可能想要對使用者發出警示並略過播放的項目，或是實作 UI 以供使用者下載正確的轉碼器。 曲目的 [**GetEncodingProperties**](/uwp/api/windows.media.core.audiotrack.getencodingproperties) 方法可用來判斷播放所需的轉碼器。

最後，您可以註冊曲目的 [**OpenFailed**](/uwp/api/windows.media.core.audiotrack.openfailed) 事件，如果裝置支援播放該曲目，但因為管線中不明的錯誤而無法開啟，則會引發此事件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksChanged_CodecCheck":::

在 [**OpenFailed**](/uwp/api/windows.media.core.audiotrack.openfailed) 事件處理常式中，您可以檢查 **MediaSource** 狀態是否為不明，如果是，您可以程式設計方式選取其他曲目進行播放、讓使用者能選擇其他曲目或中止播放。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetOpenFailed":::

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>設定系統媒體傳輸控制項使用的顯示屬性
從 Windows 10 版本 1607 開始，[**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 中播放的媒體預設會自動整合到系統媒體傳輸控制項 (SMTC)。 您可以更新 **MediaPlaybackItem** 的顯示屬性，以指定 SMTC 將顯示的中繼資料。 透過呼叫 [**GetDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties)，以取得代表項目顯示屬性的物件。 透過設定 [**Type**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type) 屬性，以設定播放項目是音樂或視訊。 接著設定物件的 [**VideoProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties) 或 [**MusicProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties) 屬性。 呼叫 [**ApplyDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties) 以將項目的屬性更新為您提供的值。 一般而言，App 會以動態方式擷取 Web 服務中的值，但下列範例會示範此程序搭配硬式編碼值。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetSetVideoProperties":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetSetMusicProperties":::

## <a name="add-external-timed-text-with-timedtextsource"></a>使用 TimedTextSource 新增外部定時文字

在某些情況下，您可能會有包含與媒體項目關聯之定時文字的外部檔案，例如包含不同地區設定之字幕的個別檔案。 使用 [**TimedTextSource**](/uwp/api/Windows.Media.Core.TimedTextSource) 類別以從串流或 URI 載入外部定時文字檔案。

這個範例使用 **Dictionary** 集合來儲存媒體項目的定時文字來源清單，其中會使用來源 URI 和 **TimedTextSource** 物件做為「機碼/值」組，以在解析播放軌之後識別播放軌。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSourceMap":::

呼叫 [**CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri) 來為每個外部定時文字檔案建立一個新的 **TimedTextSource**。 在 **Dictionary** 中為定時文字來源新增一個項目。 為 [**TimedTextSource.Resolved**](/uwp/api/windows.media.core.timedtextsource.resolved) 事件新增一個處理常式，以處理在順利載入項目之後，項目無法載入或設定其他屬性的情況。

將您所有的 **TimedTextSource** 物件新增到 [**ExternalTimedTextSources**](/uwp/api/windows.media.core.mediasource.externaltimedtextsources) 集合，以向 **MediaSource** 註冊這些物件。 請注意，外部定時文字來源會直接新增到 **MediaSource**，而不是新增到從來源建立的 **MediaPlaybackItem**。 若要更新您的 UI 以反映外部文字播放軌，請依照本文先前所述的方式註冊並處理 **TimedMetadataTracksChanged** 事件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSource":::

在 [**TimedTextSource.Resolved**](/uwp/api/windows.media.core.timedtextsource.resolved) 事件的處理常式中，檢查傳遞給處理常式之 [**TimedTextSourceResolveResultEventArgs**](/uwp/api/Windows.Media.Core.TimedTextSourceResolveResultEventArgs) 的 **Error** 屬性，以判斷嘗試載入定時文字資料時是否發生錯誤。 如果已順利解析項目，您可以使用這個處理常式來更新已解析之播放軌的其他屬性。這個範例會根據先前儲存在 **Dictionary** 中的 URI，為每個播放軌新增一個標籤。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSourceResolved":::

## <a name="add-additional-metadata-tracks"></a>新增其他中繼資料播放軌

您可以在程式碼中動態建立自訂的中繼資料播放軌，然後將它們與媒體來源建立關聯。 您建立的播放軌可以包含字幕或輔助字幕文字，也可包含您的專屬 App 資料。

呼叫建構函式並指定識別碼、語言識別碼及來自 [**TimedMetadataKind**](/uwp/api/Windows.Media.Core.TimedMetadataKind) 列舉的值，以建立新的 [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack)。 註冊 [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.cueentered) 和 [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.cueexited) 事件的處理常式。 當已達到提示的開始時間，以及當提示的持續時間已過期時，將會分別引發這些事件。

為您建立的中繼資料播放軌建立適用其類型的新提示物件，並設定播放軌的識別碼、開始時間及持續時間。這個範例會建立一個資料播放軌，因此會產生一組 [**DataCue**](/uwp/api/Windows.Media.Core.DataCue) 物件，並為每個提示提供一個包含 app 特定資料的緩衝區。 若要註冊新播放軌，請將它新增到 **MediaSource** 物件的 [**ExternalTimedMetadataTracks**](/uwp/api/windows.media.core.mediasource.externaltimedmetadatatracks) 集合。

從 Windows 10 版本 1703 開始，**DataCue.Properties**屬性公開可用來以索引鍵/資料配對儲存自訂屬性的[**PropertySet**](/uwp/api/windows.foundation.collections.propertyset)，而這些配對可以在**CueEntered**和**CueExited**事件中進行擷取。  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAddDataTrack":::

只要相關播放軌的呈現模式為 **ApplicationPresented**、**Hidden** 或 **PlatformPresented**，當達到提示的開始時間時，便會引發 **CueEntered** 事件。 如果播放軌的呈現模式為 **Disabled** 時，則不會為中繼資料播放軌引發提示事件。 這個範例只會將與提示關聯的自訂資料輸出到偵錯視窗。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDataCueEntered":::

這個範例會藉由在建立播放軌時指定 **TimedMetadataKind.Caption**，並使用 [**TimedTextCue**](/uwp/api/Windows.Media.Core.TimedTextCue) 物件將提示新增到播放軌，來新增自訂文字播放軌。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAddTextTrack":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTextCueEntered":::

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>使用 MediaPlaybackList 播放媒體項目清單

[**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 可讓您建立媒體項目 (由 **MediaPlaybackItem** 物件代表) 的播放清單。

**注意**   [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList)中的專案會使用 gapless 播放來呈現。 系統會使用 MP3 或 AAC 編碼檔案中所提供的中繼資料，以判斷無間斷播放所需的延遲或間隔補償。 如果 MP3 或 AAC 編碼檔案未提供此中繼資料，則系統會啟發式地判斷延遲或間隔。 針對不失真的格式 (例如 PCM、FLAC 或 ALAC)，系統不會採取任何動作，因為這些編碼器不會導致延遲或間隔。

若要開始，請宣告變數來儲存您的 **MediaPlaybackList**。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlaybackList":::

使用本文先前所述的相同程序，為您想要新增到清單中的每個媒體項目建立 **MediaPlaybackItem**。 將您的 **MediaPlaybackList** 物件初始化，然後將媒體播放項目新增到此物件中。 註冊 [**CurrentItemChanged**](/uwp/api/windows.media.playback.mediaplaybacklist.currentitemchanged) 事件的處理常式。 這個事件可讓您更新 UI 以反映目前播放的媒體項目。 您也可以註冊[ItemOpened](/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened)事件以在成功開啟清單中的項目時引發，以及註冊[ItemFailed](/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed)事件以在無法開放清單中的項目時引發。

從 Windows 10 版本 1703 開始，您可以指定**MediaPlaybackList**中系統在播放後將持續開啟的**MediaPlaybackItem**物件數目上限，方法是設定[MaxPlayedItemsToKeepOpen](/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen)屬性。 如果持續開啟**MediaPlaybackItem**，則會在使用者因不需要重新載入項目而切換至該項目時立即開始播放項目。 但持續開啟項目也會增加應用程式的記憶體耗用，因此您應該在設定此值時考慮回應性與記憶體使用之間的平衡。 

若要啟用清單的播放，請將**MediaPlayer**的播放來源設定為**MediaPlaybackList**。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaPlaybackList":::

在 **CurrentItemChanged** 事件處理常式中，更新您的 UI 以反映目前正在播放的項目 (藉由使用傳遞給事件之 [**CurrentMediaPlaybackItemChangedEventArgs**](/uwp/api/Windows.Media.Playback.CurrentMediaPlaybackItemChangedEventArgs) 物件的 [**NewItem**](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.newitem) 屬性即可抓取此項目)。 請記住，如果您是從這個事件更新 UI，您應該在對 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 的呼叫中執行此操作，以便在 UI 執行緒上進行更新。

從 Windows 10 版本 1703 開始，您可以檢查[CurrentMediaPlaybackItemChangedEventArgs.Reason](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason)屬性來取得值，指出項目變更原因，例如以程式設計方式設計應用程式切換項目、先前播放的項目達到其結尾，或發生錯誤。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetMediaPlaybackListItemChanged":::


呼叫 [**MovePrevious**](/uwp/api/windows.media.playback.mediaplaybacklist.moveprevious) 或 [**MoveNext**](/uwp/api/windows.media.playback.mediaplaybacklist.movenext)，以使媒體播放器播放您 **MediaPlaybackList** 中的上一個或下一個項目。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPrevButton":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetNextButton":::

設定 [**ShuffleEnabled**](/uwp/api/windows.media.playback.mediaplaybacklist.shuffleenabled) 屬性，以指定媒體播放器是否應該以隨機順序播放您清單中的項目。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetShuffleButton":::

設定 [**AutoRepeatEnabled**](/uwp/api/windows.media.playback.mediaplaybacklist.autorepeatenabled) 屬性，以指定媒體播放器是否應該循環播放您清單中的項目。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetRepeatButton":::


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>處理播放清單中失敗的媒體項目
當清單中的項目無法開啟時，會引發 [**ItemFailed**](/uwp/api/windows.media.playback.mediaplaybacklist.itemfailed) 事件。 可以的話，傳入處理常式的 [**MediaPlaybackItemError**](/uwp/api/Windows.Media.Playback.MediaPlaybackItemError) 物件的 [**ErrorCode**](/uwp/api/windows.media.playback.mediaplaybackitemerror.errorcode) 屬性會列舉失敗的特定原因，包括網路錯誤、解碼錯誤或加密錯誤。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetItemFailed":::

### <a name="disable-playback-of-items-in-a-playback-list"></a>停用播放清單中項目的播放
從 Windows 10 版本 1703 開始，您可以停用**MediaPlaybackItemList**中一或多個項目的播放，方法是將[MediaPlaybackItem](/uwp/api/Windows.Media.Playback.MediaPlaybackItem)的[IsDisabledInPlaybackList](/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList)屬性設定為 false。 

這項功能的一般案例是針對播放從網際網路串流處理之音樂的應用程式。 應用程式可以接聽裝置的網路連線狀態變更，然後停用未完全下載之項目的播放。 在下列範例中，會註冊處理常式的[NetworkInformation.NetworkStatusChanged](/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged)事件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterNetworkStatusChanged":::

在**NetworkStatusChanged**的處理常式中，確認[GetInternetConnectionProfile](/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile)是否傳回空值，指出未連接網路。 如果是這種情況，請循環執行播放清單中的所有項目，而且如果項目的[TotalDownloadProgress](/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress)小於 1，表示尚未完全下載項目，請停用項目。 如果啟用網路連線，請循環執行播放清單中的所有項目，並啟用每個項目。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetNetworkStatusChanged":::

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>使用 MediaBinder 延遲繫結播放清單中項目的媒體內容
在先前的範例中，會從檔案、URL 或資料流建立**MediaSource**，之後，則會建立**MediaPlaybackItem**，並將其新增至**MediaPlaybackList**。 在某些情況下，例如，如果使用者正在付費檢視內容，您可能會想要延遲擷取**MediaSource**的內容，直到準備好實際播放播放清單中的項目。 若要實作此案例，請建立[**MediaBinder**](/uwp/api/Windows.Media.Core.MediaBinder)類別的執行個體。 將[**Token**](/uwp/api/Windows.Media.Core.MediaBinder.Token)屬性設定為應用程式定義的字串，以識別您想要延遲擷取的內容，然後註冊[**Binding**](/uwp/api/Windows.Media.Core.MediaBinder.Binding)事件的處理常式。 接著，呼叫[**MediaSource.CreateFromMediaBinder**](/uwp/api/windows.media.core.mediasource.createfrommediabinder)，以從**Binder**建立**MediaSource**。 然後，從**MediaSource**建立**MediaPlaybackItem**，並如常將它新增至播放清單。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetInitMediaBinder":::

當系統判斷需要擷取與**MediaBinder**相關的內容時，會引發**Binding**事件。 在這個事件的處理常式中，您可以從傳入事件的[**MediaBindingEventArgs**](/uwp/api/windows.media.core.mediabindingeventargs)中擷取**MediaBinder**執行個體。 擷取您為**Token**屬性指定的字串，並使用它來判斷應該擷取的內容。 **MediaBindingEventArgs**提供方法，以透過數種不同的呈現來設定繫結的內容，包括[**SetStorageFile**](/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile)、[**SetStream**](/uwp/api/windows.media.core.mediabindingeventargs.setstream)、[**SetStreamReference**](/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference)和[**SetUri**](/uwp/api/windows.media.core.mediabindingeventargs.seturi)。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetBinderBinding":::

請注意，如果您要在**Binding**事件處理常式中執行非同步作業，例如 Web 要求，則應該呼叫[**MediaBindingEventArgs.GetDeferral**](/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral)方法，指示系統等候作業完成，再繼續進行。 在作業完成之後呼叫[**Deferral.Complete**](/uwp/api/windows.foundation.deferral.Complete)，指示系統繼續進行。

從 Windows 10 版本 1703 開始，您可以將[**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource)提供為繫結的內容，方法是呼叫 [**SetAdaptiveMediaSource**](/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)。 如需在應用程式中使用彈性資料流的詳細資訊，請參閱[彈性資料流](adaptive-streaming.md)。



## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)
* [與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)
* [在背景播放媒體](background-audio.md)
