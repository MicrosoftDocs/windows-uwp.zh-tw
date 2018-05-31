---
author: mijacobs
title: 回應式設計的螢幕大小與中斷點
description: 除了最佳化 UI，我們建議針對幾個稱為中斷點的重要寬度類別，設計許多跨 Windows 10 生態系統的裝置。
ms.author: mijacobs
ms.date: 08/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0d84530c1a7c3795c566495c1eae121691b0766a
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
ms.locfileid: "1688934"
---
#  <a name="screen-sizes-and-breakpoints"></a>螢幕大小與中斷點

UWP app 可在執行 Windows 10，包括手機、平板電腦、桌面、電視，以及更多的任何裝置上執行。 我們建議使用跨 Windows 10 生態系統的大量裝置目標與螢幕大小，而不是最佳化您每個裝置的 UI，來設計幾個重要的寬度類別 (也稱為「中斷點」) ： 
- 小型 (小於 640px) 
- 中型 (641px 到 1007px)
- 大型 (1008px 和更大的)

> [!TIP]
> 當針對特定中斷點進行設計時，請針對您應用程式，而不是螢幕大小，可以使用的螢幕空間量 (應用程式的視窗) 進行設計。 當應用程式執行全螢幕時，應用程式視窗會與螢幕大小相同，但當應用程式不是全螢幕時，視窗則小於螢幕。

## <a name="breakpoints"></a>中斷點
下表描述不同大小類別和中斷點。

![回應式設計中斷點](images/rsp-design/rspd-breakpoints.png)

<table>
<thead>
<tr class="header">
<th align="left">大小類別</th>
<th align="left">中斷點</th>
<th align="left">螢幕大小 (對角線)</th>
<th align="left">裝置</th>
<th align="left">視窗大小</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">小型</td>
<td style="vertical-align:top;">640px 或更少</td>
<td style="vertical-align:top;">4&quot; 到 6&quot;</td>
<td style="vertical-align:top;">手機</td>
<td style="vertical-align:top;">320x569、360x640、480x854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">中型</td>
<td style="vertical-align:top;">641px 到 1007px</td>
<td style="vertical-align:top;">7&quot; 到 12&quot;</td>
<td style="vertical-align:top;">平板手機、平板電腦、電視</td>
<td style="vertical-align:top;">960x540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">大型</td>
<td style="vertical-align:top;">1008px 或更大像素</td>
<td style="vertical-align:top;">13&quot; 及更大</td>
<td style="vertical-align:top;">電腦、膝上型電腦、Surface Hub</td>
<td style="vertical-align:top;">1024x640、1366x768、1920x1080</td>
</tr>
</tbody>
</table>

## <a name="general-recommendations"></a>一般建議

### <a name="small"></a>小型
- 設定左右視窗邊界為 12px，以在應用程式視窗左右邊緣建立視覺區隔。
- 將[應用程式列](../controls-and-patterns/app-bars.md) 固定在視窗底部以改善存取性。
- 一次使用 1 個欄位/區域。
- 使用圖示來表示搜尋 (不顯示搜尋方塊)。
- 將[瀏覽窗格](../controls-and-patterns/navigationview.md)以重疊模式放置，以節省螢幕空間。
- 如果您使用[主要詳細資料模式](../controls-and-patterns/master-details.md)，請使用堆疊展示模式，以節省螢幕空間。

### <a name="medium"></a>中型
- 設定左右視窗邊界為 24px，以在應用程式視窗左右邊緣建立視覺區隔。
- 將[應用程式列](../controls-and-patterns/app-bars.md) 之類的命令元素放置在應用程式視窗的頂端。
- 使用最多 2 個欄位/區域。
- 顯示搜尋方塊。
- 將[瀏覽窗格](../controls-and-patterns/navigationview.md)以窄條模式放置，以便讓圖示帶狀線永遠顯示。
- 請考慮進一步自訂[電視體驗](http://go.microsoft.com/fwlink/?LinkId=760736)。

### <a name="large"></a>大型
- 設定左右視窗邊界為 24px，以在應用程式視窗左右邊緣建立視覺區隔。
- 將[應用程式列](../controls-and-patterns/app-bars.md)之類的命令元素放置在應用程式視窗的頂端。
- 使用最多 3 個欄位/區域。
- 顯示搜尋方塊。
- 將[瀏覽窗格](../controls-and-patterns/navigationview.md)以停駐模式放置，以便讓它永遠顯示。

>[!TIP] 
> 透過 [**Continuum 手機版**](http://go.microsoft.com/fwlink/p/?LinkID=699431)，使用者可以將相容的 Windows 10 行動裝置連接到監視器、滑鼠和鍵盤，讓手機可以像膝上型電腦一樣運作。 針對特定中斷點進行設計時，請記住這項新功能- 行動電話不會永遠留在大小類別中。

## <a name="effective-pixels-and-scale-factor"></a>有效像素與縮放比例

UWP app 會自動調整您的 UI 以確保應用程式在所有 Windows 10 裝置上可辨識。 Windows 會根據 DPI (每英吋點數) 以及裝置的檢視距離，自動縮放每個螢幕的比例。 使用者可以移至 **\[設定\]** > **\[顯示\]** > **\[縮放與版面配置\]** 設定頁面，覆寫預設值。 