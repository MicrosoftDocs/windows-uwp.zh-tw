---
title: 啟動 Windows 地圖應用程式
description: 瞭解如何使用 bingmaps、ms 磁片磁碟機到、ms 逐步解說和 ms 設定 URI 配置，從您的應用程式啟動 Windows 地圖應用程式。
ms.assetid: E363490A-C886-4D92-9A64-52E3C24F1D98
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e020972a8dff0b0721fd2c5726999a7896d359c4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167902"
---
# <a name="launch-the-windows-maps-app"></a>啟動 Windows 地圖應用程式




了解如何從您的應用程式啟動 Windows 地圖應用程式。 本主題說明 **bingmaps：**、 **ms 磁片磁碟機至：**、ms 逐步解說 **：** 和 **MS 設定：** 統一資源識別項 (URI) 配置。 使用這些 URI 配置，可針對特定的地圖、方向和搜尋結果啟動 Windows 地圖應用程式，或者從設定應用程式下載 Windows 地圖離線地圖。

**提示** 若要深入了解如何從您的應用程式啟動 Windows 地圖應用程式，請從 GitHub 的 [Windows-universal-samples 儲存機制](https://github.com/Microsoft/Windows-universal-samples)下載[通用 Windows 平台 (UWP) 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)。

## <a name="introducing-uris"></a>URI 簡介

URI 配置可讓您按一下超連結 (或在 app 中以程式設計方式) 開啟 app。 就像您可以使用 **mailto:** 建立新的電子郵件，或使用 **http:** 開啟網頁瀏覽器一樣，您可以使用 **bingmaps:**、**ms-drive-to:** 和 **ms-walk-to:** 來開啟 Windows 地圖 app。

-   **bingmaps:** URI 可提供位置、搜尋結果、方向及交通的地圖。
-   **ms-drive-to:** URI 可提供從您目前的所在位置出發的轉向建議導航行駛路線指引。
-   **ms-walk-to:** URI 可提供從您目前的所在位置出發的轉向建議導航步行路線指引。

例如，下列 URI 會開啟 Windows 地圖 app，並顯示以紐約市為中心的地圖。

```xml
<bingmaps:?cp=40.726966~-74.006076>
```

![以紐約市為中心的地圖。](images/mapnyc.png)

以下是此 URI 配置的描述：

**bingmaps:?query**

在此 URI 配置中，*query* 是一系列的「參數名稱/值」組：

**&param1 = value1&param2 = value2 .。。**

如需完整的可用參數清單，請參閱 [bingmaps:](#bingmaps-param-reference)、[ms-drive-to:](#ms-drive-to-param-reference) 和 [ms-walk-to:](#ms-walk-to-param-reference) 參數參考。 本主題稍後也提供相關範例。

## <a name="launch-a-uri-from-your-app"></a>從您的 app 啟動 URI


若要從您的 app 啟動 Windows 地圖 app，請使用 **bingmaps:**、**ms-drive-to:** 或 **ms-walk-to:** URI 呼叫 [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) 方法。 下列範例會啟動與前一個範例中相同的 URI。 如需關於透過 URI 啟動 app 的詳細資訊，請參閱[啟動 URI 的預設 app](launch-default-app.md)。

```cs
// Center on New York City
var uriNewYork = new Uri(@"bingmaps:?cp=40.726966~-74.006076");

// Launch the Windows Maps app
var launcherOptions = new Windows.System.LauncherOptions();
launcherOptions.TargetApplicationPackageFamilyName = "Microsoft.WindowsMaps_8wekyb3d8bbwe";
var success = await Windows.System.Launcher.LaunchUriAsync(uriNewYork, launcherOptions);
```

在這個範例中，會使用 [**LauncherOptions**](/uwp/api/Windows.System.LauncherOptions) 類別確保 Windows 地圖 app 可啟動。

## <a name="display-known-locations"></a>顯示已知位置

有許多選項，可控制要顯示的對應的部分。 您可以將 *cp* (中心點) 參數搭配 *rad* (半徑) 或 *lvl* (縮放層級) 參數使用以顯示位置，並選擇為放大所要靠近的距離。 當您使用 *cp* 參數時，也可以指定 *hdg* (方向) 與 *pit* (上下移動) 來控制要往哪一個方向看。 另一個方法就是使用 *bb* (週框方塊) 參數提供所要顯示區域的最東、西、南、北方座標。

若要控制檢視類型，請使用 *sty* (樣式) 與 *ss* (街景) 參數。 *sty* 參數可讓您在道路與空照圖檢視之間切換。 *ss* 參數會將地圖放入街景檢視中。 如需這些與其他參數的詳細資訊，請參閱 [bingmaps: 參數參考](#bingmaps-param-reference)。


| URI 範例                                                                 | 結果                                                                                                                                                                                        |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?                                                                 | 開啟地圖 app。                                                                                                                                                                            |
| bingmaps:?cp=40.726966~-74.006076                                          | 顯示以紐約市為中心的地圖。                                                                                                                                                    |
| bingmaps:?cp=40.726966~-74.006076&amp;lvl=10                                   | 顯示縮放比例 10 以紐約市為中心的地圖。                                                                                                                            |
| bingmaps：？ bb = 39.719 \_ -74.52 ~ 41.71 \_ -73。5                                   | 顯示紐約市的地圖，這是 **bb** 引數中指定的區域。                                                                                                           |
| bingmaps：？ bb = 39.719 \_ -74.52 ~ 41.71 \_ -73.5&cp = 47 ~-122                        | 顯示紐約市地圖，這是週框方塊引數中指定的區域。 會略過以 **cp** 引數指定的西雅圖中心點，因為指定了 *bb*。 |
| bingmaps：？ collection = point. 36.116584 \_ -115.176753 \_ Caesars% 20Palace&lvl = 16 | 將縮放比例設定為 16 來顯示含有 Caesar Palace (拉斯維加斯) 地點名稱的地圖。                                                                                                 |
| bingmaps：？ collection = point. 40.726966 \_ -74.006076 \_ Some% 255FBusiness        | 顯示名為「部分商務 (的地圖， \_ 在拉斯維加斯的) 。                                                                                                                               |
| bingmaps:?cp=40.726966~-74.006076&trfc=1&sty=a                             | 顯示具有「交通」資訊和「空照圖」地圖樣式的紐約市地圖。                                                                                                                          |
| bingmaps:?cp=47.6204~-122.3491&sty=3d                                      | 顯示太空針塔的 3D 檢視。                                                                                                                                                        |
| bingmaps:?cp=47.6204~-122.3491&sty=3d&rad=200&pit=75&amp;amp;hdg=165               | 顯示半徑為 200 公尺、上下移動為 75 度、朝向為 165 度的太空針塔 3D 檢視。                                                                             |
| bingmaps:?cp=47.6204~-122.3491&ss=1                                        | 顯示太空針塔的街景檢視。                                                                                                                                                |


## <a name="display-search-results"></a>顯示搜尋結果

當使用 *q* 參數搜尋位置時，我們建議儘可能使用具體的字詞，並使用 *cp*、*bb* 或 *where* 參數來指定搜尋位置。 如果您未指定搜尋位置，且使用者目前的位置無法使用，則搜尋不會傳回有意義的結果。 搜尋結果會顯示在最適合的地圖檢視中。 如需這些與其他參數的詳細資訊，請參閱 [bingmaps: 參數參考](#bingmaps-param-reference)。


| URI 範例                                                    | 結果                                                                            |
|---------------------------------------------------------------|------------------------------------------------------------------------------------|
| bingmaps:?q=1600%20Pennsylvania%20Ave,%20Washington,%20DC     | 顯示地圖，並搜尋華盛頓特區白宮的地址。 |
| bingmaps:?q=coffee&where=Seattle                              | 搜尋西雅圖市的咖啡廳。                                                    |
| bingmaps:?cp=40.726966~-74.006076&where=New%20York            | 搜尋靠近指定中心點的紐約。                             |
| bingmaps：？ bb = 39.719 \_ -74.52 ~ 41.71 \_ -73.5&q = 比薩              | 搜尋指定週框方塊 (亦即紐約市) 中的比薩店。      |

 
## <a name="display-multiple-points"></a>顯示多個點


使用 *collection* 參數可在地圖上顯示一組自訂的點。 如果有多個點，則會顯示點清單。 一個集合可以有多達 25 個點，並會依提供的順序列出。 集合的優先順序高於搜尋與路線指引要求。 如需關於此參數與其他參數的詳細資訊，請參閱[bingmaps: 參數參考](#bingmaps-param-reference)。

| URI 範例 | 結果                                                                                                                   |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| bingmaps：？ collection = point. 36.116584 \_ -115.176753 \_ Caesars% 20Palace                                                                                                | 搜尋拉斯維加斯的 Caesar's Palace，然後以最佳的地圖檢視在地圖上顯示結果。                         |
| bingmaps：？ collection = point. 36.116584 \_ -115.176753 \_ Caesars% 20Palace&lvl = 16                                                                                         | 將縮放比例設定為 16 來顯示位於拉斯維加斯名為 Caesars Palace 的圖釘。                                               |
| bingmaps：？ collection = point. 36.116584 \_ -115.176753 \_ Caesars% 20Palace ~ point. 36.113126 \_ -115.175188 a \_ % 20Bellagio&lvl = 16&cp = 36.114902 ~-115.176669                   | 將縮放比例設定為 16 來顯示位於拉斯維加斯名為 Caesars Palace 和名為 The Bellagio 的圖釘。              |
| bingmaps：？ collection = point. 40.726966 \_ -74.006076 \_ 假% 255FBusiness% 255Fwith% 255FUnderscore                                                                        | 顯示紐約，其圖釘名為假 \_ 企業 \_ 加 \_ 底線。                                                  |
| bingmaps：？ collection = name。飯店% 20List ~ point. 36.116584 \_ -115.176753 \_ Caesars% 20Palace ~ point. 36.113126 \_ -115.175188 a \_ % 20Bellagio&lvl = 16&cp = 36.114902 ~-115.176669 | 將縮放比例設定為 16 來顯示名為 Hotel List 的清單，以及兩個代表位於拉斯維加斯之 Caesars Palace 和 The Bellagio 的圖釘。 |

 

## <a name="display-directions-and-traffic"></a>顯示路線指引和交通狀況


您可以使用 *rtp* 參數顯示兩個點之間的路線；這些點可以是地址或緯度和經度座標。 使用 *trfc* 參數可顯示交通資訊。 若要指定路線類型 (開車、步行或運輸工具)，請使用 *mode* 參數。 若未指定 *mode*，則會以使用者偏好的交通模式提供路線指引。 如需這些參數與其他參數的詳細資訊，請參閱 [bingmaps: 參數參考](#bingmaps-param-reference)。

![路線指引範例](images/windowsmapgcdirections.png)

| URI 範例                                                                                                              | 結果                                                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps：？ rtp = pos. 44.9160 \_ -110.4158 ~ pos. 45.0475 \_ -109.4187                                                             | 顯示點對點的路線地圖。 由於未指定 *mode*，因此會以使用者的交通喜好設定模式提供路線指引。 |
| bingmaps:?cp=43.0332~-87.9167&amp;trfc=1                                                                                    | 顯示以威斯康辛州密爾瓦基市為中心和交通的地圖。                                                                                                        |
| bingmaps：？ rtp = adr。一個 Microsoft 的方式，Redmond，WA 98052 ~ pos. 39.0731 \_ -108.7238                                           | 顯示從指定地址到指定位置的路線地圖。                                                                            |
| bingmaps：？ rtp = adr. 1% 20Microsoft% 20Way，% 20Redmond，% 20WA，%2098052 ~ pos. 36.1223-111.9495% 20Canyon% 20northern \_ \_ % 20rim | 顯示從 1 Microsoft Way, Redmond, WA, 98052 到大峽谷北緣的路線。                                                                |
| bingmaps:?rtp=adr.Davenport, CA~adr.Yosemite Village                                                                    | 顯示從指定位置到指定地標的駕駛路線地圖。                                                                   |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=d                      | 顯示從加州山景城到加州舊金山國際機場的駕駛路線。                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=w                      | 顯示從加州山景城到加州舊金山國際機場的步行路線。                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=t                      | 顯示從加州山景城到加州舊金山國際機場的運輸工具路線。                                                                  |

## <a name="display-turn-by-turn-directions"></a>顯示轉向建議導航路線指引


**ms-drive-to:** 和 **ms-walk-to:** URI 配置可讓您直接啟動至轉向建議導航路線檢視。 這些 URI 配置只能提供從使用者目前所在位置出發的路線指引。 如果您必須提供不包含使用者目前所在位置的兩點之間的路線指引，請使用上一節所說明的 **bingmaps:** URI 配置。 如需這些 URI 配置的詳細資訊，請參閱 [ms-drive-to:](#ms-drive-to-param-reference) 和 [ms-walk-to:](#ms-walk-to-param-reference) 參數參考。

> **重要** 當 **ms-drive-to:** 或 **ms-walk-to:** URI 配置啟動時，地圖應用程式會檢查裝置是否曾經修正 GPS 位置。 如果有，地圖 app 就會前往轉向建議導航路線指引。 如果還未修正，app 將會顯示路線概觀，如[顯示路線指引和交通狀況](#display-directions-and-traffic)中所述。

![轉向建議導航路線指引範例](images/windowsmapsappdirections.png)

| URI 範例                                                                                                | 結果                                                                                       |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| ms-drive-to:?destination.latitude=47.680504&destination.longitude=-122.328262&amp;amp;destination.name=Green Lake | 顯示一個地圖，內含從您目前的所在位置到 Green Lake 的詳細行駛路線。 |
| ms-walk-to:?destination.latitude=47.680504&destination.longitude=-122.328262&amp;amp;destination.name=Green Lake  | 顯示一個地圖，內含從您目前的所在位置到 Green Lake 的詳細步行路線。 |


## <a name="download-offline-maps"></a>下載離線地圖

**ms-settings:** URI 配置可讓您直接啟動到設定應用程式中的特定頁面。 雖然 **ms-settings:** URI 配置不會啟動到地圖應用程式，但是允許您直接啟動到設定應用程式中的 [離線地圖] 頁面，並且顯示確認對話方塊來下載地圖應用程式所使用的離線地圖。 URI 配置接受緯度和經度所指定的值，並自動判斷是否有包含該點之地區可用的離線地圖。  如果緯度和經度剛好落在多個下載地區內，確認對話方塊會讓使用者挑選要下載其中哪一個區域。 如果包含該點的地區沒有離線地圖，則設定應用程式中的 [離線地圖] 頁面會顯示錯誤對話方塊。

| URI 範例  | 結果 |
|-------------|---------|
| ms-settings:maps-downloadmaps?latlong=47.6,-122.3 | 將設定應用程式開啟到 [離線地圖] 頁面，並顯示確認對話方塊來下載包含所指定經緯度點之地區的地圖。 |

<span id="bingmaps-param-reference"/>

## <a name="bingmaps-parameter-reference"></a>bingmaps: 參數參考

此表格中每個參數的語法是使用擴充巴克斯格式 (Augmented Backus–Naur Form, ABNF) 來示範的。

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">定義</th>
<th align="left">ABNF 定義和範例</th>
<th align="left">詳細資料</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><b>Cp</b></p></td>
<td align="left"><p>中心點</p></td>
<td align="left"><p>cp = "cp=" cpval</p>
<p>cpval = degreeslat "~" degreeslon</p>
<p>degreeslat = ["-"] 1*3DIGIT ["."1*7DIGIT]</p>
<p>degreeslon = ["-"] 1*2DIGIT ["." 1*7DIGIT]</p>
<p>範例：</p>
<p>cp=40.726966~-74.006076</p></td>
<td align="left"><p>這兩個值都必須以十進位數表示，並以波狀符號 (<b>~</b>) 分隔。</p>
<p>有效的經度值介於 -180 (含) 到 +180 (含)。</p>
<p>有效的緯度值介於 -90 (含) 到 +90 (含)。</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>Bb</b></p></td>
<td align="left"><p>週框方塊</p></td>
<td align="left"><p>bb = "bb=" southlatitude "_" westlongitude "~" northlatitude "_" eastlongitude</p>
<p>southlatitude = degreeslat</p>
<p>northlatitude = degreeslat</p>
<p>westlongitude = degreeslon</p>
<p>eastlongitude = degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>範例：</p>
<p>bb=39.719_-74.52~41.71_-73.5</p></td>
<td align="left"><p>矩形區域，指定以小數點表示的周框方塊，使用波狀符號 (<b>~</b>) 將左下角與右上角分隔開來。 每個週框方塊的經緯度會以底線 (<b>_</b>) 分隔。</p>
<p>有效的經度值介於 -180 (含) 到 +180 (含)。</p>
<p>有效的緯度值介於 -90 (含) 到 +90 (含)。</p><p>提供週框方塊時，會忽略 cp 和 lvl 參數。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>where</b></p></td>
<td align="left"><p>Location</p></td>
<td align="left"><p>where = "where=" whereval</p>
<p>whereval = 1 *( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "*" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>
<p>範例：</p>
<p>where=1600%20Pennsylvania%20Ave,%20Washington,%20DC</p></td>
<td align="left"><p>特定位置、地標或地點的搜尋字詞。</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>問</b></p></td>
<td align="left"><p>查詢字詞</p></td>
<td align="left"><p>q = "q="</p>
<p>whereval</p>
<p>範例：</p>
<p>q=mexican%20restaurants</p></td>
<td align="left"><p>當地商店或商店類別的搜尋字詞。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>lvl</b></p></td>
<td align="left"><p>縮放比例</p></td>
<td align="left"><p>lvl = "lvl=" 1<i>2DIGIT ["." 1</i>2DIGIT]</p>
<p>範例：</p>
<p>lvl=10.50</p></td>
<td align="left"><p>定義地圖檢視的縮放比例。 有效值為 1-20，其中 1 是縮到最小。</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>豬圈</b></p></td>
<td align="left"><p>樣式</p></td>
<td align="left"><p>sty = "sty=" ("a" / "r"/"3d")</p>
<p>範例：</p>
<p>sty=a</p></td>
<td align="left"><p>定義地圖樣式。 此參數的有效值包括：</p>
<ul>
<li><b>a</b>：顯示地圖的空照圖檢視。</li>
<li><b>r</b>：顯示地圖的道路圖檢視。</li>
<li><b>3d</b>：顯示地圖的立體檢視。 與 <b>cp</b> 參數搭配使用，還可以選擇性地搭配 <b>rad</b> 參數。</li>
</ul>
<p>在 Windows 10 中，空照圖檢視和 3D 檢視樣式相同。</p>
<div class="alert">
<b>注意</b>   省略<b>sty</b>參數會產生與 sty = r 相同的結果。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>Rad</b></p></td>
<td align="left"><p>半徑</p></td>
<td align="left"><p>rad = "rad=" 1*8DIGIT</p>
<p>範例：</p>
<p>rad=1000</p></td>
<td align="left"><p>指定所需地圖檢視的圓形區域。 半徑值以公尺為單位。</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>pit</b></p></td>
<td align="left"><p>音調</p></td>
<td align="left"><p>pit = "pit=" pitch</p>
<p>範例：</p>
<p>pit=60</p></td>
<td align="left"><p>指出檢視地圖的角度；90 會遠眺地平線 (最大)，0 會筆直俯瞰 (最小)。</p><p>有效的上下移動值介於 0 (含) 到 90 (含)。</td>
</tr>
<tr class="odd">
<td align="left"><p><b>hdg</b></p></td>
<td align="left"><p>朝向</p></td>
<td align="left"><p>hdg = "hdg=" heading</p>
<p>範例：</p>
<p>hdg=180</p></td>
<td align="left"><p>以度為單位指出地圖所朝的方向；0 或 360 = 北、90 = 東、180 = 南、270 = 西。</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>ss</b></p></td>
<td align="left"><p>街景</p></td>
<td align="left"><p>ss = "ss=" BIT</p>
<p>範例：</p>
<p>ss=1</p></td>
<td align="left"><p>指出當 <code>ss=1</code> 時就顯示街景圖。 省略 <b>ss</b> 參數會產生與 <code>ss=0</code> 相同的結果。 與 <b>cp</b> 參數搭配使用來指定街景檢視的位置。</p>
<div class="alert">
<b>注意</b>   街道層級的影像無法在所有區域中使用。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>trfc</b></p></td>
<td align="left"><p>交通流量</p></td>
<td align="left"><p>trfc = "trfc=" BIT</p>
<p>範例：</p>
<p>trfc=1</p></td>
<td align="left"><p>指定是否要在地圖上包含交通資訊。 省略 trfc 參數會產生與 <code>trfc=0</code> 相同的結果。</p>
<div class="alert">
<b>注意</b>   所有區域都無法使用流量資料。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p><b>rtp</b></p></td>
<td align="left"><p>路由</p></td>
<td align="left"><p>rtp = "rtp=" (waypoint "~" [waypoint]) / ("~" waypoint)</p>
<p>waypoint = ("pos." point ) / ("adr." whereval)</p>
<p>point = "point." pointval ["_" title]</p>
<p>pointval = degreeslat "" degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>title = whereval</p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>


<p>範例：</p>
<p>rtp=adr.Mountain%20View,%20CA~adr.SFO</p>
<p>rtp=adr.One%20Microsoft%20Way,%20Redmond,%20WA~pos.45.23423_-122.1232 _My%20Picnic%20Spot</p></td>
<td align="left"><p>定義要在地圖上繪製之路線的起點和終點，並以波狀符號 (<b>~</b>) 分隔。 每個導航點都是由使用緯度、經度和選擇性標題或地址識別碼的位置來定義。</p>
<p>完整的路線會正好包含兩個導航點。 例如，<code>rtp="A"~"B"</code> 會定義具有兩個導航點的路線。</p>
<p>也可以接受指定不完整的路線。 例如，您可以使用 <code>rtp="A"~</code> 僅定義路線的起點。 在此情況下，顯示路線指引輸入時，<b>\[從\]</b> 欄位中會有所提供的導航點，而 <b>\[到\]</b> 欄位則為焦點所在。</p>
<p>如果只指定路線的終點，如同<code>rtp=~"B"</code>，則在顯示路線指引面板時，<b>\[到\]</b> 欄位中會有提供的導航點。 如果有正確的目前位置，將會在具有焦點的 <b>\[從\]</b> 欄位中預先填入目前所在位置。</p>
<p>提供的路線不完整時，不會繪製任何路線圖。</p>
<p>與 <b>mode</b> 參數搭配使用可指定交通模式 (開車、運輸工具或步行)。 若未指定 <b>mode</b>，則會以使用者的交通喜好設定模式提供路線指引。</p>
<div class="alert">
<b>注意</b>   如果位置是由<b>pos</b>參數值所指定，則可以將標題用於位置。 系統將顯示標題，而不是緯度和經度。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>mode</b></p></td>
<td align="left"><p>交通模式</p></td>
<td align="left"><p>mode = "mode=" ("d" / "t" / "w")</p>
<p>範例：</p>
<p>mode=d</p></td>
<td align="left"><p>定義交通模式。 此參數的有效值包括：</p>
<ul>
<li><b>d</b>：顯示行駛路線的路線概觀</li>
<li><b>t</b>：顯示大眾運輸路線的路線概觀</li>
<li><b>w</b>：顯示步行路線的路線概觀</li>
</ul>
<p>與 <b>rtp</b> 參數搭配使用，提供交通路線指引。 若未指定 <b>mode</b>，則會以使用者的交通喜好設定模式提供路線指引。 <b>mode</b> 可以與 no route 參數一起提供，來輸入該模式從目前位置的路線指引輸入。</p></td>
</tr>

<tr class="even">
<td align="left"><p><b>收集</b></p></td>
<td align="left"><p>集合</p></td>
<td align="left"><p>collection = "collection="(name"~"/)point["~"point]</p>
<p>name = "name." whereval </p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?") </p>
<p>point = "point." pointval ["_" title] </p>
<p>pointval = degreeslat "" degreeslon </p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT] </p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT] </p>
<p>title = whereval</p>


<p>範例：</p>
<p>collection=name.My%20Trip%20Stops~point.36.116584_-115.176753_Las%20Vegas~point.37.8268_-122.4798_Golden%20Gate%20Bridge</p></td>
<td align="left"><p>要新增至地圖和清單中的點集合。 使用 name 參數，可以指定點集合。 使用緯度、經度和選擇性標題指定一個點。</p>
<p>以波狀符號 () 分隔名稱和多個點 <b>~</b> 。</p>
<p>如果您指定的項目包含波狀符號，請務必要將波狀符號以 <code>%7E</code> 編碼。 如果沒有與「中心點」與「縮放比例」參數搭配使用，集合將會提供最適當的地圖檢視。</p>

<p><b>重要</b> 如果您所指定的項目包含底線，請確定將底線以 %255F 雙重編碼。</p></td>
</tr>
</tbody>
</table>

  
<span id="ms-drive-to-param-reference"/>

## <a name="ms-drive-to-parameter-reference"></a>ms-drive-to: 參數參考


啟動轉向建議駕駛路線要求的 URI 不需要進行編碼，並且具有下列格式。

> **注意** 您並未在此 URI 配置中指定起點。 起點一律會假設為目前的位置。 如果您需要指定目前所在位置以外的起點，請參閱[顯示路線和交通狀況](#display-directions-and-traffic)。

 

| 參數 | 定義 | 範例 | 詳細資料 |
|------------|-----------|---------|---------|
| **destination.latitude** | 目的地緯度 | 範例：destination.latitude=47.6451413797194 | 目的地的緯度。 有效的緯度值介於 -90 (含) 到 +90 (含)。 |
| **destination.longitude** | 目的地經度 | 範例：destination.longitude=-122.141964733601 | 目的地的經度。 有效的經度值介於 -180 (含) 到 +180 (含)。 |
| **destination.name** | 目的地的名稱 | 範例：destination.name=Redmond, WA | 目的地的名稱。 您不需要編碼 **destination.name** 值。 |

 
<span id="ms-walk-to-param-reference"/>

## <a name="ms-walk-to-parameter-reference"></a>ms-walk-to: 參數參考


啟動轉向建議步行路線要求的 URI 不需要進行編碼，並且具有下列格式。

> **注意** 您並未在此 URI 配置中指定起點。 起點一律會假設為目前的位置。 如果您需要指定目前所在位置以外的起點，請參閱[顯示路線和交通狀況](#display-directions-and-traffic)。
 

| 參數 | 定義 | 範例 | 詳細資料 |
|-----------|------------|---------|----------|
| **destination.latitude** | 目的地緯度 | 範例：destination.latitude=47.6451413797194 | 目的地的緯度。 有效的緯度值介於 -90 (含) 到 +90 (含)。 |
| **destination.longitude** | 目的地經度 | 範例：destination.longitude=-122.141964733601 | 目的地的經度。 有效的經度值介於 -180 (含) 到 +180 (含)。 |
| **destination.name** | 目的地的名稱 | 範例：destination.name=Redmond, WA | 目的地的名稱。 您不需要編碼 **destination.name** 值。 |

## <a name="ms-settings-parameter-reference"></a>ms-settings: 參數參考

**ms-settings:** URI 配置的地圖應用程式特定參數的語法定義如下。 **maps-downloadmaps** 是與 **ms-settings:** URI 一起指定，格式為 **ms-settings:maps-downloadmaps?**，以指示離線地圖設定頁面。 

| 參數 | 定義 | 範例 | 詳細資料 |
|-----------|------------|---------|----------|
| **latlong** | 定義離線地圖區域的點。 | 範例：latlong=47.6,-122.3 | 地理點是以逗號分隔的緯度和經度來指定。 有效的緯度值介於 -90 (含) 到 +90 (含)。 有效的經度值介於 -180 (含) 到 +180 (含)。 |