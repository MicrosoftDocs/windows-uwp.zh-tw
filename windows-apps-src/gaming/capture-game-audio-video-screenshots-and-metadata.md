---
ms.assetid: ''
description: 瞭解如何捕獲遊戲的影片、音訊和螢幕擷取畫面，以及如何提交系統在已捕獲和廣播媒體中內嵌的中繼資料。
title: 擷取遊戲音訊、影片、螢幕擷取畫面和中繼資料
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, 遊戲, 擷取, 音訊, 影片, 元資料
ms.localizationpriority: medium
ms.openlocfilehash: ea01139d4945d1e1b7e9a49078cd93725388e946
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364141"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>擷取遊戲音訊、影片、螢幕擷取畫面和中繼資料
本文說明如何擷取遊戲影片、音訊和螢幕擷取畫面，以及如何提交系統將會嵌入所擷取和廣播的媒體中的元資料，讓您的 App 和其他人可建立與遊戲活動同步的動態遊戲體驗。 

有兩種不同方式可在 UWP app 中擷取遊戲。 使用者可以使用內建的系統 UI 起始擷取。 使用這項技術所捕捉的媒體會內嵌到 Microsoft 遊戲生態系統中，您可以透過第一方的體驗（例如 Xbox 應用程式）來查看和共用，而且不能直接用於您的應用程式或使用者。 本文的第一節顯示如何啟用和停用系統實作的 App 擷取，以及如何在 App 擷取開始或停止時接收通知。

擷取媒體的其他方式是使用 **[Windows.Media.AppRecording](/uwp/api/windows.media.apprecording)** 命名空間的 API。 如果在裝置上啟用擷取，您的 App 可以開始擷取遊戲，然後在經過一段時間後，您可以停止擷取，此時媒體會寫入檔案。 如果使用者已啟用歷史擷取，您也可以錄製已經發生的遊戲，只要指定過去的開始時間和要錄製的持續時間即可。 這兩種技術都會產生您的 App 和使用者可以存取的影片檔案，並根據您選擇的位置儲存檔案。 本文的中間章節會帶領您實作這些案例。

**[Windows.Media.Capture](/uwp/api/windows.media.capture)** 命名空間提供 API，用於建立描述將擷取或廣播之遊戲的元資料。 這可包含文字或數值，還有識別每個資料項目的文字標籤。 元資料可以代表單一時刻發生的「事件」，例如在使用者完成賽車遊戲的一圈時，或者也可以代表持續一段時間的「狀態」，例如目前使用者正在玩的遊戲中的地圖。 元資料是寫入快取中，而快取則是由系統為您的 App 進行配置和管理。 元資料是內嵌到廣播串流及擷取的影片檔案中，同時包括內建的系統擷取或自訂的 App 擷取技術。 本文的最後一節顯示如何撰寫遊戲元資料。

> [!NOTE] 
> 因為遊戲元資料可內嵌到可能透過網路共用的媒體檔案，使用者無法加以控制，所以不應在元資料中包含可識別個人的資訊或其他潛在的敏感資料。


## <a name="enable-and-disable-system-app-capture"></a>啟用和停用系統 App 擷取
系統 App 擷取是由使用者使用內建的系統 UI 起始 這些檔案是由 Windows 遊戲生態系統所內嵌，並不適用于您的應用程式或使用者，除了透過第一方體驗（如 Xbox 應用程式）之外。 您的 App 可以停用和啟用系統起始的 App 擷取，讓您得以防止使用者擷取特定內容或遊戲。 

若要啟用或停用系統 App 擷取，只要呼叫靜態方法 **[AppCapture.SetAllowedAsync](/uwp/api/windows.media.capture.appcapture.setallowedasync)** 並傳送 **false** 來停用擷取或傳送 **true** 啟用擷取。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetSetAppCaptureAllowed":::


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>當系統 App 擷取開始和停止時收到通知
若要在系統 App 擷取開始或結束時收到通知，請先透過呼叫 Factory 方法 **[GetForCurrentView](/uwp/api/windows.media.capture.appcapture.GetForCurrentView)** 來取得 **[AppCapture](/uwp/api/windows.media.capture.appcapture)** 類別的執行個體。 接著，註冊 **[CapturingChanged](/uwp/api/windows.media.capture.appcapture.CapturingChanged)** 事件的處理常式。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRegisterCapturingChanged":::

在 **CapturingChanged** 事件的處理常式中，您可以檢查 **[IsCapturingAudio](/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** 和 **[IsCapturingVideo](/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** 屬性，以判斷是否分別擷取音訊或影片。 若要指出目前的擷取狀態，您可以更新您的 App 的 UI。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnCapturingChanged":::

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>將 UWP 的 Windows 桌面延伸新增到您的 App
直接從您的 App 錄製音訊和影片的 API 和擷取螢幕擷取畫面的 API，可在 **[Windows.Media.AppRecording](/uwp/api/windows.media.apprecording)** 命名空間中找到，並未包含在通用 API 協定中。 若要存取 API，您必須利用下列步驟將 UWP 的 Windows 桌面延伸的參考新增到您的 App。

1. 在 Visual Studio 中，請在 **\[方案總管\]** 中展開 UWP 專案，以滑鼠右鍵按一下 **\[參考\]**，然後選取 **\[加入參考...\]**。 
2. 展開 **\[通用 Windows\]** 節點，然後選取 **\[延伸\]**。
3. 在延伸清單中，核取符合您專案的目標組建的 **\[UWP 的 Windows 桌面延伸\]** 項目旁的核取方塊。 若是 App 的廣播功能，版本必須是 1709 或以上。
4. 按一下 [確定]  。

## <a name="get-an-instance-of-apprecordingmanager"></a>取得 AppRecordingManager 的執行個體
**[AppRecordingManager](/uwp/api/windows.media.apprecording.apprecordingmanager)** 類別是您將用來管理 App 錄製的中央 API。 透過呼叫 Factory 方法 **[GetDefault](/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)** 取得的此類別的執行個體。 使用 **Windows.Media.AppRecording** 命名空間中的任何 API 之前，應該檢查其是否存在於目前的裝置上。 API 在執行 Windows 10 版本 1709 之前的作業系統版本的裝置上無法使用。 與其檢查特定的作業系統版本，不如使用 **[ApiInformation.IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 方法來查詢 *Windows.Media.AppBroadcasting.AppRecordingContract* 1.0 版。 如果有此協定，則可在裝置上錄製 API。 本文中的範例程式碼會檢查一次 API，然後在進行後續作業之前檢查 **AppRecordingManager** 是否為 null。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetGetAppRecordingManager":::

## <a name="determine-if-your-app-can-currently-record"></a>判斷您的 App 目前是否可以錄製
有幾個原因您的 App 可能目前無法擷取音訊或影片，包括目前的裝置是否不符合錄製的硬體需求，或者其他 App 目前正在廣播中。 在起始錄製之前，您可以檢查您的 App 是否目前無法錄製。 呼叫 **AppRecordingManager** 物件的 **[GetStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** 方法，然後檢查傳回之 **[AppRecordingStatus](/uwp/api/windows.media.apprecording.apprecordingstatus)** 物件的 **[CanRecord](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** 屬性。 如果 **CanRecord** 傳回 **false**，表示您的應用程式目前無法錄製，您可以查看 **[詳細資料](/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** 屬性來判斷原因。 根據原因而定，您可能想要對使用者顯示狀態，或顯示啟用 App 錄製的指示。



:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCanRecord":::

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>手動開始和停止錄製您的 App 到檔案中

確認您的 App 可以錄製之後，即可透過呼叫 **AppRecordingManager** 物件的 **[StartRecordingToFileAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** 方法開始新的錄製。

在下列範例中，當非同步工作失敗時，第一個 **then** 會封鎖執行。 第二個 **then** 會封鎖嘗試存取工作的結果，如果結果為 null，則工作已完成。 在這兩個案例中，如下所示，會呼叫 **OnRecordingComplete** 協助程式方法來處理結果。 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartRecordToFile":::

當錄製作業完成時，檢查傳回之 **[AppRecordingResult](/uwp/api/windows.media.apprecording.apprecordingresult)** 物件的 **[Succeeded](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** 屬性，以判斷錄製作業是否成功。 若成功，您可以檢查 **[IsFileTruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** 屬性來判斷系統是否基於儲存空間的原因被迫截斷所擷取的檔案。 您可以檢查 **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** 屬性，以了解錄製檔案的實際持續時間，如果檔案被截斷，可能比錄製作業的持續時間較短。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnRecordingComplete":::

下列範例顯示先前範例中開始和停止錄製作業的一些基本程式碼。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallStartRecordToFile":::

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetFinishRecordToFile":::

## <a name="record-a-historical-time-span-to-a-file"></a>錄製歷史時間範圍到檔案
如果使用者已在系統設定中為您的 App 啟用歷史錄製，您可以錄製之前已玩過遊戲的時間範圍。 本文中的上一個範例顯示如何確認您的 App 目前是否可以錄製遊戲。 還有其他檢查可以判斷是否啟用歷史擷取。 同樣地，呼叫 **[GetStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)**，並檢查傳回之 **AppRecordingStatus** 物件的 **[CanRecordTimeSpan](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** 屬性。 此範例也傳回 **AppRecordingStatus** 的 **[HistoricalBufferDuration](/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** 屬性，將用來判斷錄製作業的有效開始時間。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCanRecordTimeSpan":::

若要擷取歷史時間範圍，您必須指定錄製的開始時間和持續時間。 開始時間是以 **[DateTime](/uwp/api/windows.foundation.datetime)** 結構提供。 開始時間必須是在目前的時間之前，歷史錄製緩衝的長度之內。 就此範例而言，擷取緩衝長度是檢查的一部分，如上一個程式碼範例所示，可查看歷史錄製是否啟用。 歷史錄製的持續時間是以 **[TimeSpan](/uwp/api/windows.foundation.timespan)** 結構提供，也應該等於或小於歷史緩衝的持續時間。 一旦您確定您想要的開始時間和持續時間後，呼叫 **[RecordTimeSpanToFileAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** 以開始錄製作業。

如同手動開始和停止錄製，當歷史錄製完成時，您可以檢查傳回之 **[AppRecordingResult](/uwp/api/windows.media.apprecording.apprecordingresult)** 物件的 **[Succeeded](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** 屬性來判斷錄製作業是否成功，並且可以檢查 **[IsFileTruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** 和 **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** 屬性，了解所錄製檔案的實際持續時間，檔案若被截斷，可能比所要求時間範圍的持續時間來得短。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRecordTimeSpanToFile":::

下列範例顯示先前範例中起始歷史錄製作業的一些基本程式碼。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallRecordTimeSpanToFile":::

## <a name="save-screenshot-images-to-files"></a>儲存螢幕擷取畫面的影像到檔案
您的 App 可以起始螢幕擷取畫面的擷取，將 App 視窗的目前內容儲存到一個影像檔或到不同影像編碼的多個影像檔。 若要指定您想要使用的影像編碼，請建立一份字串清單，其中每個字串代表一種影像類型。 **[ImageEncodingSubtypes](/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** 的屬性為每一種支援的影像類型 (例如 **MediaEncodingSubtypes.Png** 或 **MediaEncodingSubtypes.JpegXr**) 提供正確的字串。

透過呼叫 **AppRecordingManager** 物件的 **[SaveScreenshotToFilesAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** 方法，來起始螢幕擷取。 此方法的第一個參數是 **StorageFolder**，影像檔案將會儲存在其中。 第二個參數是檔案名稱前置詞，系統將為每種儲存的影像類型附加副檔名，例如「.png」。

如果要擷取的目前視窗顯示 HDR 內容，**SaveScreenshotToFilesAsync** 的第三個參數是系統為了能夠執行正確的色彩空間轉換所必要的。 如果有 HDR 內容，此參數應該設定為 **AppRecordingSaveScreenshotOption.HdrContentVisible**。 否則，請使用 **AppRecordingSaveScreenshotOption.None**。 此方法的最後一個參數，是應該擷取之螢幕的影像格式清單。

當非同步呼叫 **SaveScreenshotToFilesAsync** 完成時，它會傳回 **[AppRecordingSavedScreenshotInfo](/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** 物件，其為每個儲存的影像提供 **StorageFile** 和表示影像類型的相關 **MediaEncodingSubtypes** 值。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetSaveScreenShotToFiles":::

下列範例顯示先前範例中起始螢幕擷取畫面作業的一些基本程式碼。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallSaveScreenShotToFiles":::

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>新增適用於系統和應用程式起始之擷取的遊戲元資料
本文的下列章節描述如何提供元資料，讓系統嵌入所擷取或廣播的遊戲的 MP4 串流。 元資料可嵌入使用內建的系統 UI 擷取的媒體和 App 使用 **AppRecordingManager** 擷取的媒體。 這個元資料可在媒體播放期間由您的 App 和其他 App 擷取，以便提供與所擷取或廣播的遊戲同步的內容感知體驗。

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>取得 AppCaptureMetadataWriter 的執行個體
管理 App 擷取元資料的主要類別是 **[AppCaptureMetadataWriter](/uwp/api/windows.media.capture.appcapturemetadatawriter)**。 初始化此類別的執行個體之前，請使用 **[ApiInformation.IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 方法查詢 *Windows.Media.Capture.AppCaptureMetadataContract* 版本 1.0，以確認 API 可在目前的裝置上使用。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetGetMetadataWriter":::

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>為您的 App 將元資料寫入系統快取
每個元資料項目都有字串標籤，識別元資料項目、相關聯的資料值，其可能是字串、整數或雙精確度值，以及來自 **[AppCaptureMetadataPriority](/uwp/api/windows.media.capture.appcapturemetadatapriority)** 列舉的值，其指出資料項目的相關優先順序。 元資料項目可能被視為「事件」，即發生在單一時間點，或保持一個值一段時間的「狀態」。 元資料是寫入記憶體中，而記憶體則是由系統為您的 App 進行配置和管理。 系統會對元資料記憶體快取強制執行大小限制，並且在達到上限時，根據每個寫入的元資料項目的優先順序清除資料。 本文的下一個章節將顯示如何管理您的 App 元資料記憶體配置。

一般的 App 可以選擇在擷取工作階段的開頭寫入一些元資料，以提供後續的資料一些內容。 對於本案例，建議使用瞬間的「事件」資料。 此範例呼叫 **[AddStringEvent](/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**、**[AddDoubleEvent](/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)** 和 **[AddInt32Event](/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)**，來為每一種資料類型設定瞬間值。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartSession":::

使用持續一段時間的「狀態」的常見案例，是去追蹤目前玩家還在其中的遊戲地圖。 這個範例呼叫 **[StartStringState](/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** 來設定狀態值。 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartMap":::

呼叫 **[StopState](/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** 以錄製已經結束的特定狀態。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetEndMap":::

您可以透過使用現有的狀態標籤設定新值，來覆寫狀態。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetLevelUp":::

您可以藉由呼叫 **[StopAllStates](/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)** 來結束所有打開的狀態。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRaceComplete":::

### <a name="manage-metadata-cache-storage-limit"></a>管理元資料快取儲存空間的限制
您使用 **AppCaptureMetadataWriter** 撰寫的元資料，會由系統快取直到其寫入相關聯的媒體串流。 系統會定義每個 App 元資料快取的大小限制。 一旦達到快取的大小限制，系統會開始清除快取的元資料。 系統會先刪除以 **[AppCaptureMetadataPriority](/uwp/api/windows.media.capture.appcapturemetadatapriority)** 撰寫的中繼資料，再刪除具有 AppCaptureMetadataPriority 的中繼資料。 **[重要](/uwp/api/windows.media.capture.appcapturemetadatapriority)** 優先順序。

任何時候，您都可以透過呼叫 **[RemainingStorageBytesAvailable](/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)** 來查看您的 App 元資料快取中可用的位元組數。 您可以選擇設定您自己的 App 定義的閾值，之後您可以選擇減少要寫入快取的元資料量。 下列範例顯示此模式的簡單實作。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCheckMetadataStorage":::

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetComboExecuted":::

### <a name="receive-notifications-when-the-system-purges-metadata"></a>當系統清除元資料時收到通知
您可以註冊，以在系統開始清除應用程式的中繼資料時收到通知，方法是註冊 **[MetadataPurged](/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)** 事件的處理常式。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRegisterMetadataPurged":::

在 **MetadataPurged** 事件的處理常式中，您可以透過結束較低優先順序的狀態，在元資料快取中清出一些空間，您可以實作 App 定義的邏輯，來減少寫入快取中的元資料量，或者您可以什麼都不做，讓系統繼續根據所撰寫的優先順序清除快取。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnMetadataPurged":::

## <a name="related-topics"></a>相關主題

* [遊戲](index.md)
 

 
