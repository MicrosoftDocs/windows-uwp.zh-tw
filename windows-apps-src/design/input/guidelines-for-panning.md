---
description: 移動瀏覽或捲動可讓使用者在單一檢視內進行瀏覽，以顯示無法容納在檢視區中的檢視內容。 檢視範例包括電腦的資料夾結構、文件庫或相簿。
title: 移動瀏覽
ms.assetid: b419f538-c7fb-4e7c-9547-5fb2494c0b71
label: Panning
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8e1208f023a28621c3f98536f77d36a962343bb
ms.sourcegitcommit: 4fffc66fac18fc4c80281e2a4afa9c4f2e1f7551
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/11/2020
ms.locfileid: "94513717"
---
# <a name="guidelines-for-panning"></a>移動瀏覽的指導方針


移動瀏覽或捲動可讓使用者在單一檢視內進行瀏覽，以顯示無法容納在檢視區中的檢視內容。 檢視範例包括電腦的資料夾結構、文件庫或相簿。

> **重要 API** ： [**Windows.UI.Input**](/uwp/api/Windows.UI.Input)、 [**Windows.UI.Xaml.Input**](/uwp/api/Windows.UI.Xaml.Input)


## <a name="dos-and-donts"></a>可行與禁止事項


**移動瀏覽指標和捲軸**

-   確保可以移動瀏覽/捲動，才將內容載入到應用程式中。

-   顯示移動瀏覽指示器和捲軸來提供位置和尺寸提示。若您要提供自訂瀏覽功能，請隱藏他們。

    **備註**  與標準捲軸不同，移動瀏覽指標僅為提供資訊。 這些指標不對輸入裝置顯示，而且完全不能以任何方式操作。

     

**單軸移動瀏覽 (一維溢位)**

-   對延長超過一個檢視區界限 (垂直或水平) 的內容區域，使用單軸移動瀏覽。

    -   若為項目的一維清單，請使用垂直移動瀏覽。
    -   若為格線中的項目，請使用水平移動瀏覽。
-   如果必須讓使用者在貼齊點之間移動瀏覽並停止時，請不要利用單軸移動瀏覽來使用強制貼齊點。 強制貼齊點會保證使用者停止在貼齊點上。 請改用鄰近性貼齊點。

**任意方向移動瀏覽 (二維溢位)**

-   對延長超過兩個檢視區界限 (垂直和水平) 的內容區域，使用雙軸移動瀏覽。

    -   若是使用者可能會往多個方向移動的無結構內容，請覆寫預設的柵欄行為並使用任意方向移動瀏覽。
-   任意方向移動瀏覽通常適用於影像或地圖內。

**分頁檢視**

-   當內容是由離散的元素所組成或您想要顯示整個元素時，請使用強制貼齊點。 這可能包含一本書或雜誌的頁面、一個項目資料欄或個別的影像。

    -   貼齊點應被放置在每個邏輯界限上。
    -   每個元素都應該調整大小或縮放以符合檢視範圍。

**邏輯和關鍵點**

-   如果內容中有使用者可能停住的關鍵點或邏輯位置，請使用鄰近性貼齊點。 例如，區段標頭。

-   如果定義了最大和最小大小限制或界限，可以在使用者達到或超出這些界限時，使用視覺化回饋作為顯示。

**鏈結內嵌或巢狀的內容**

-   對於文字與格線內容，請使用單軸移動瀏覽 (通常為水平) 與欄配置。 在這些情況下，內容通常會從資料行自然地包裝和流出至資料行，並讓使用者體驗在 Windows 應用程式之間保持一致且可供探索。

-   不要使用內嵌的可移動瀏覽區域來顯示文字或項目清單。 因為只有在區域內偵測到輸入接觸點時，才會顯示移動瀏覽指標與捲軸，所以這並非直覺式或可探索的使用者經驗。

-   如果兩個可移動瀏覽區域的移動瀏覽方向相同，請勿將兩者鏈結，或將其中一個區域置於另一個區域內 (如下所示)。 當達到子區域的界限時，這可能會導致在父區域發生非預期的移動瀏覽。 請考慮讓移動瀏覽軸成垂直。

    ![示範與其容器朝同方向捲動之內嵌可移動瀏覽區域的影像。](images/scrolling-embedded3.png)

## <a name="additional-usage-guidance"></a>其他用法指導方針

利用觸控進行移動瀏覽 (搭配單指或多指使用撥動或滑動手勢) 就像是使用滑鼠捲動。 移動瀏覽互動更像是旋轉滑鼠滾輪或滑動捲動方塊，而不是按一下捲軸。 除非在 API 中進行區別，或受到某些裝置特定 Windows UI 的要求，我們都將這兩種互動稱為移動瀏覽。

> <div id="main">
> <strong>Windows 10 Fall Creators Update 行為變更</strong> 使用中的畫筆預設會在 Windows 應用程式中進行滾動/移動 (例如觸控、觸控板和被動畫筆) ，而不是文字選取。  
> 如果您的應用程式需仰賴先前的行為，則可以覆寫手寫筆捲動並還原至先前的行為。 如需詳細資料，請參閱針對 <a href="/uwp/api/windows.ui.xaml.controls.scrollviewer">ScrollViewer 類別</a> \(英文\) 的 API 參考主題。
> </div>

根據輸入裝置，使用者會透過下列其中一種方式，在可移動瀏覽的區域內移動瀏覽：

-   使用滑鼠、觸控板或主動式畫筆/手寫筆按一下捲動箭號、拖曳捲動方塊，或在捲軸內按一下。
-   使用滑鼠的滾輪按鈕來模擬拖曳捲動方塊的動作。
-   延伸的按鈕 (XBUTTON1 和 XBUTTON2)，如果滑鼠支援。
-   使用鍵盤方向鍵來模擬拖曳捲動方塊的動作，或頁面鍵來模擬在捲軸內按一下的動作。
-   使用觸控、觸控板或被動式畫筆/手寫筆往想要的方向滑動，或將手指往想要的方向撥動。

滑動需要將手指慢慢朝移動瀏覽方向移動。 這會產生一對一關係，內容會依手指移動的速度和距離移動瀏覽。 撥動 (需要快速滑動並提起手指) 會將下列物理作用套用到移動瀏覽動畫：

-   減速 (慣性)：提起手指會讓移動瀏覽開始減速。 這類似於在光滑的表面上滑動到停止的現象。
-   吸力：減速時如果到達貼齊點或內容區域界限，移動瀏覽動力會產生輕微的反彈效果。

**移動瀏覽的類型**

Windows 8 支援三種移動瀏覽類型：

-   單軸 - 僅支援一個方向的移動瀏覽 (水平或垂直)。
-   柵欄 - 支援所有方向的移動瀏覽。 不過，一旦使用者超出某個特定方向的距離閾值，移動瀏覽就會受限於該軸。
-   任意方向 - 支援所有方向的移動瀏覽。

**移動瀏覽 UI**

移動瀏覽的互動經驗對輸入裝置都是唯一的，但仍然能提供類似的功能。

**Pannable 區域** Pannable 區域行為是在設計階段透過階層式樣式表 (CSS) ，在設計階段使用 JavaScript 開發人員公開給 Windows 應用程式。

根據偵測到的輸入裝置，提供兩種移動瀏覽顯示模式：

-   觸控時為移動瀏覽指標。
-   使用其他輸入裝置 (包括滑鼠、觸控板、鍵盤以及手寫筆) 時為捲軸。

**備註**  移動瀏覽指標僅在觸控觸碰點位於可移動瀏覽的區域時，才會顯示。 同理，只有當滑鼠游標、畫筆/手寫筆游標或鍵盤焦點是在可捲動的區域內時，才會顯示捲軸。

 

**移動瀏覽指標** 移動瀏覽指標類似於捲軸中的捲動方塊。 它們指示了顯示內容佔可移動瀏覽總區域的比例，以及顯示內容在可移動瀏覽區域中的相對位置。

下圖顯示兩個長度不同的可移動瀏覽區域及其移動瀏覽指標。

![顯示兩個長度不同的可移動瀏覽區域及其移動瀏覽指標的影像。](images/scrolling-indicators.png)

**移動流覽行為** 
**貼齊點** 移動觸控手勢時，移動觸控手勢時，會將慣性行為引入互動。 利用慣性作用，內容會繼續移動瀏覽，直到達到某個距離閾值，而不用使用者直接輸入。 使用貼齊點修改這種慣性行為。

貼齊點會指定應用程式內容的邏輯停止點。 貼齊點的作用就像是供使用者使用的分頁機制，將在大型可移動瀏覽區域的過度滑動或撥動減至最低程度。 使用它們即可處理不精確的使用者輸入，確保檢視區中可以顯示特定內容子集或關鍵資訊。

貼齊點有兩種類型：

-   鄰近性 - 提起手指後，如果慣性作用讓貼齊點停在距離閾值範圍內，就會選取該貼齊點。 移動瀏覽仍然可以在鄰近性貼齊點之間停止。
-   強制 - 選取的貼齊點為提起手指之前，在最後一個越過之貼齊點之前或之後的那個貼齊點 (取決於手勢的方向和速度)。 移動瀏覽必須在強制貼齊點上停止。

對於一些模擬分頁內容或具有項目邏輯群組而可以被動態重新分組以納入檢視區或顯示範圍中的應用程式 (例如網頁瀏覽器和相簿) 來說，移動瀏覽貼齊點相當有用。

下圖顯示移動瀏覽至特定點並放開時，如何造成內容自動移動瀏覽至某個邏輯位置。

:::row:::
   :::column:::
      ![顯示可移動瀏覽區域的影像。](images/ux-panning-snap1.png)

      撥動以移動瀏覽。
   :::column-end:::
   :::column:::
      ![顯示可移動瀏覽區域被移動瀏覽至左側的影像。](images/ux-panning-snap2.png)

      提起手指。
   :::column-end:::
   :::column:::
      ![顯示已在某個邏輯貼齊點停止移動瀏覽之可移動瀏覽區域的影像。](images/ux-panning-snap3.png)

      可移動瀏覽區域會在貼齊點停止，而不是提起手指的位置。
   :::column-end:::
:::row-end:::

**柵欄** 內容可以寬於和高於顯示裝置的維度及解析度。 基於這個理由，二維移動瀏覽 (水平和垂直) 通常是必需的。 柵欄可改進這些案例中的使用者經驗，透過沿著動作方向的軸線 (垂直或水平) 強調移動瀏覽來完成。

下圖示範柵欄的概念。

![畫面中有柵欄限制移動瀏覽範圍的圖表](images/ux-panning-rails.png)

**鏈結內嵌或巢狀的內容**

使用者達到元素 (巢狀於其他可縮放或可捲動元素內) 的縮放或捲動限制後，您可以指定父項元素是否應繼續執行由其子元素開始的縮放或捲動作業。 這稱為縮放或捲動「鏈結」。

對於在包含一或多個單軸或任意方向移動瀏覽區域的單軸內容區域內所進行的移動瀏覽 (當觸控接觸是發生在這些子區域的其中一個之內)，可以使用鏈結。 當達到子區域某個特定方向的移動瀏覽界限時，便會啟動父區域同一方向的移動瀏覽。

當一個可移動瀏覽區域巢狀於另一個可移動瀏覽區域內時，在容器與內嵌的內容之間指定足夠的空間便相當重要。 在下圖中，一個可移動瀏覽區域被置於另一個可移動瀏覽區域內，各自成垂直方向。 每個區域中都有充分的空間可供使用者移動瀏覽。

![示範內嵌的可移動瀏覽區域的影像。](images/scrolling-embedded.png)

如果空間不足 (如下圖所示)，內嵌的可移動瀏覽區域便可能干擾容器中的移動瀏覽，並導致在一或多個可移動瀏覽區域中發生非預期的移動瀏覽。

![示範內嵌的可移動瀏覽區域邊框間距不足的影像。](images/ux-panning-embedded-wrong.png)

對於在個別影像或地圖內支援無限制移動瀏覽，同時在相簿內 (上一個或下一個影像) 或詳細資料區域內支援單軸移動瀏覽的應用程式 (如相簿或地圖應用程式)，這個指導方針也很實用。 在提供對應任意方向移動瀏覽影像或地圖之詳細資料或選項區域的應用程式中，我們建議頁面配置應該先從詳細資料和選項區域開始，因為影像或地圖的無限制移動瀏覽區域可能會干擾針對詳細資料區域的移動瀏覽。

## <a name="related-articles"></a>相關文章

- [自訂使用者互動](../layout/index.md)
- [最佳化 ListView 與 GridView](../../debug-test-perf/optimize-gridview-and-listview.md)
- [鍵盤協助工具](../accessibility/keyboard-accessibility.md)

**範例**
- [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)

**封存範例**
- [輸入：XAML 使用者輸入事件範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [輸入：裝置功能範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [輸入：觸控點擊測試範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 滾動、移動流覽和縮放範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [輸入：簡化的筆跡範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [輸入：操作和手勢範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX 觸控輸入範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
