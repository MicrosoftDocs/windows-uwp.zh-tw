---
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: SystemMediaTransportControls 類別可讓您的 App 使用內建於 Windows 的系統媒體傳輸控制項，以及更新控制項顯示您 App 目前正在播放之媒體的相關中繼資料。
title: 系統媒體傳輸控制項的手動控制項
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d592b516db32c2602c8b51d82f3ea56c037e5164
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363781"
---
# <a name="manual-control-of-the-system-media-transport-controls"></a>系統媒體傳輸控制項的手動控制項


從 Windows 10 版本 1607 開始，使用 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 類別來播放媒體的 UWP App，預設將會自動與系統媒體傳輸控制項 (SMTC) 整合。 針對大部分的案例，這是與 SMTC 互動的建議方式。 如需自訂 SMTC 與 **MediaPlayer** 預設整合的詳細資訊，請參閱[與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)。

在某幾個案例中，您可能需要實作 SMTC 的手動控制項。 這包括使用 [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController) 來控制一或多個媒體播放程式之播放的案例。 或是會使用多個媒體播放程式，但針對 App 只想要單一 SMTC 執行個體的案例。 如果您是使用 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 來播放媒體，便必須手動控制 SMTC。

## <a name="set-up-transport-controls"></a>設定傳輸控制項
如果您是使用 **MediaPlayer** 來播放媒體，您可以透過存取 [**MediaPlayer.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) 屬性來取得 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 類別的執行個體。 如果您要手動控制 SMTC，您應該停用由 **MediaPlayer** 所提供的自動整合，方法是將 [**CommandManager.IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 屬性設定為 false。

> [!NOTE] 
> 如果您透過將 [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 設定為 false 來停用 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 的 [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager)，它將會破壞 **MediaPlayer** 和由 **MediaPlayerElement** 所提供的 [**TransportControls**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) 之間的連結，使內建傳輸控制項無法繼續自動控制播放器的播放。 您必須改為實作自己的控制項以控制 **MediaPlayer**。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetInitSMTCMediaPlayer":::

您也可以透過呼叫 [**GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview) 以取得 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 的執行個體。 如果您是使用 **MediaElement** 來播放媒體，您必須透過此方法來取得該物件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetInitSMTCMediaElement":::

啟用您的 App 將會使用的按鈕，方法是設定 **SystemMediaTransportControls** 物件的相對應「is enabled」屬性，例如 [**IsPlayEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled)、[**IsPauseEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled)、[**IsNextEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isnextenabled) 及 [**IsPreviousEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispreviousenabled)。 請參閱 **SystemMediaTransportControls** 參考文件，以取得可用控制項的完整清單。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetEnableContols":::

註冊 [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 事件的處理常式，以在使用者按下按鈕時接收通知。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetRegisterButtonPressed":::

## <a name="handle-system-media-transport-controls-button-presses"></a>處理系統媒體傳輸控制項按鈕的按下動作

當某個啟用的按鈕被按下時，系統傳輸控制項會引發 [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 事件。 傳入事件處理常式的 [**SystemMediaTransportControlsButtonPressedEventArgs**](/uwp/api/Windows.Media.SystemMediaTransportControlsButtonPressedEventArgs) 的 [**Button**](/uwp/api/windows.media.systemmediatransportcontrolsbuttonpressedeventargs.button) 屬性，是 [**SystemMediaTransportControlsButton**](/uwp/api/Windows.Media.SystemMediaTransportControlsButton) 列舉的成員，該列舉表示已按下哪個已啟用的按鈕。

若要從 [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 事件處理常式更新 UI 執行緒上的物件 (如 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 物件)，您必須透過 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 為呼叫進行封送處理。 這是因為 **ButtonPressed** 事件處理常式不是從 UI 執行緒呼叫，因此如果您嘗試直接修改 UI 將會擲回例外狀況。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsButtonPressed":::

## <a name="update-the-system-media-transport-controls-with-the-current-media-status"></a>以目前的媒體狀態更新系統媒體傳輸控制項

當媒體狀態已變更時，您應該通知 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls)，以便讓系統可以更新控制項以反映目前狀態。 若要這樣做，請從 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 的 [**CurrentStateChanged**](/uwp/api/windows.ui.xaml.controls.mediaelement.currentstatechanged) 事件 (當媒體狀態變更時引發)，將 [**PlaybackStatus**](/uwp/api/windows.media.systemmediatransportcontrols.playbackstatus) 屬性設為適當的 [**MediaPlaybackStatus**](/uwp/api/Windows.Media.MediaPlaybackStatus) 值。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsStateChange":::

## <a name="update-the-system-media-transport-controls-with-media-info-and-thumbnails"></a>以媒體資訊和縮圖更新系統媒體傳輸控制項

您可以使用 [**SystemMediaTransportControlsDisplayUpdater**](/uwp/api/Windows.Media.SystemMediaTransportControlsDisplayUpdater) 類別來更新傳輸控制項顯示的媒體資訊，例如，目前播放媒體項目的歌曲標題或專輯封面。 使用 [**SystemMediaTransportControls.DisplayUpdater**](/uwp/api/windows.media.systemmediatransportcontrols.displayupdater) 屬性取得這個類別的執行個體。 對於典型的案例，傳遞中繼資料的建議方式是呼叫 [**CopyFromFileAsync**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.copyfromfileasync)，傳入目前播放媒體檔案。 顯示更新程式會自動從檔案擷取中繼資料和縮圖影像。

呼叫 [**Update**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.update) 會造成系統媒體傳輸控制項使用新的中繼資料與縮圖更新它的 UI。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsUpdater":::

如果您的案例需要，您可以手動更新系統媒體傳輸控制項顯示的中繼資料，方法是設定 [**DisplayUpdater**](/uwp/api/windows.media.systemmediatransportcontrols.displayupdater) 類別公開之 [**MusicProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.musicproperties)、[**ImageProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.imageproperties) 或 [**VideoProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.videoproperties) 物件的值。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SystemMediaTransportControlsUpdaterManual":::

> [!Note]
> 應用程式應該設定 [SystemMediaTransportControlsDisplayUpdater](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.type#Windows_Media_SystemMediaTransportControlsDisplayUpdater_Type
) 屬性的值，即使它們未提供系統媒體傳輸控制項所顯示的其他媒體中繼資料。 此值可協助系統正確處理您的媒體內容，包括防止螢幕保護裝置在播放期間啟用。


## <a name="update-the-system-media-transport-controls-timeline-properties"></a>更新系統媒體傳輸控制項時間軸屬性

系統傳輸控制項會顯示目前播放媒體項目之時間軸的相關資訊，包括媒體項目的目前播放位置、開始時間及結束時間。 若要更新系統傳輸控制項時間軸屬性，請建立新的 [**SystemMediaTransportControlsTimelineProperties**](/uwp/api/Windows.Media.SystemMediaTransportControlsTimelineProperties) 物件。 設定物件的屬性以反映正在播放的媒體項目的目前狀態。 呼叫 [**SystemMediaTransportControls.UpdateTimelineProperties**](/uwp/api/windows.media.systemmediatransportcontrols.updatetimelineproperties) 讓控制項更新時間軸。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetUpdateTimelineProperties":::

-   您必須提供 [**StartTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.starttime)、[**EndTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.endtime) 和 [**Position**](/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested) 的值，系統控制項才能為您正在播放的項目顯示時間軸。

-   [**MinSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) 和 [**MaxSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime) 可讓您指定使用者可以搜尋的時間軸的範圍。 典型的案例是讓內容提供者在他們的媒體中包含廣告中斷。

    您必須設定 [**MinSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) 和 [**MaxSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime)，才會引發 [**PositionChangeRequest**](/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested)。

-   建議您讓系統控制項與您的媒體播放保持同步，方法是在播放期間大約每 5 秒更新這些屬性，以及每當播放的狀態變更時更新，例如暫停或搜尋新的位置。

## <a name="respond-to-player-property-changes"></a>回應播放程式屬性變更

有一組系統傳輸控制項屬性與媒體播放程式本身的目前狀態相關，而不是與正在播放的媒體項目狀態相關。 每個屬性與使用者調整相關聯控制項時所引發的事件相符。 這些屬性與事件包括：

| 屬性                                                                  | 事件                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmode) | [**AutoRepeatModeChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmodechangerequested) |
| [**PlaybackRate**](/uwp/api/windows.media.systemmediatransportcontrols.playbackrate)     | [**PlaybackRateChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested)     |
| [**ShuffleEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabled) | [**ShuffleEnabledChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabledchangerequested) |

 
若要處理使用者與其中一個控制項的互動，請先註冊相關聯事件的處理常式。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetRegisterPlaybackChangedHandler":::

在事件處理常式中，請先確定要求的值是在有效且預期的範圍內。 如果是的話，設定 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 的對應屬性，然後設定 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 物件的對應屬性。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetPlaybackChangedHandler":::

-   為了引發其中一個播放程式屬性事件，您必須設定屬性的初始值。 例如，[**PlaybackRateChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested) 不會引發，直到您設定 [**PlaybackRate**](/uwp/api/windows.media.systemmediatransportcontrols.playbackrate) 屬性的值至少一次之後。

## <a name="use-the-system-media-transport-controls-for-background-audio"></a>針對背景音訊使用系統媒體傳輸控制項

如果您沒有使用由 **MediaPlayer** 所提供的自動 SMTC 整合，您必須手動與 SMTC 整合以啟用背景音訊。 您的 App 至少必須將 [**IsPlayEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled) 和 [**IsPauseEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled) 設定成 true 來啟用播放和暫停按鈕。 您的 App 還必須處理 [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 事件。 如果您的 App 沒有符合這些需求，音訊播放將會在 App 移至背景時停止。

針對背景音訊使用新的單處理程序模型的 App，應該能透過呼叫 [**GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview) 取得 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 的執行個體。 針對背景音訊使用舊版的雙處理程序模型 的 App，必須使用 [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) 以取得從背景處理程序存取 SMTC 的權限。

如需有關在背景播放音訊的詳細資訊，請參閱[在背景播放媒體](background-audio.md)。

## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md) 
* [系統媒體傳輸範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 
