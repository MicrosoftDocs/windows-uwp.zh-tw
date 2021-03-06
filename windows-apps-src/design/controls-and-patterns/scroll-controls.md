---
description: 移動瀏覽和捲動可讓使用者到達超出螢幕界限的內容。
title: 捲動檢視器控制項
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: Scrollbars
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: Abarlow, pagildea
design-contact: ksulliv
dev-contact: regisb
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f0bd0fa72587d83e9dca28b18688f1c667033070
ms.sourcegitcommit: c5fdcc0779d4b657669948a4eda32ca3ccc7889b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2021
ms.locfileid: "102784849"
---
# <a name="scroll-viewer-controls"></a>捲動檢視器控制項



當您要顯示的 UI 內容多到超過區域所能容納的範圍時，請使用捲動檢視器控制項。

> **重要 API**：[ScrollViewer 類別](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) \(英文\)、[ScrollBar 類別](/uwp/api/windows.ui.xaml.controls.primitives.scrollbar) \(英文\)

捲動檢視器可讓內容延伸到檢視區 (可見區域) 的界限外。 使用者能透過觸控、滑鼠滾輪、鍵盤或遊戲控制器操縱捲動檢視器表面，或是使用滑鼠或手寫筆游標與捲動檢視器的捲軸互動，以觸達此內容。 此圖顯示捲動檢視器控制項的幾個範例。

![說明標準捲軸控制項的螢幕擷取畫面](images/ScrollBar_Standard.jpg)

視情況而定，捲動檢視器的捲軸使用兩種不同的視覺效果，如下圖所示：移動瀏覽指示器 (左) 和傳統捲軸 (右)。

![標準捲軸和移動瀏覽指示器控制項外觀的範例](images/SCROLLBAR.png)

捲動檢視器會感知使用者的輸入方式，並用它來判斷要顯示哪一個視覺效果。

* 例如，在沒有直接操縱捲軸的情況下透過觸控區域時，移動瀏覽指示器會出現，並顯示目前的捲動位置。
* 當滑鼠或手寫筆游標移至移動瀏覽指示器上方時，它會變換成傳統捲軸。  拖曳捲軸捲動方塊可操縱捲動區域。

<!--
<div class="microsoft-internal-note">
See complete redlines in [UNI]http://uni/DesignDepot.FrontEnd/#/ProductNav/3378/0/dv/?t=Windows|Controls|ScrollControls&f=RS2
</div>
-->

![捲軸運作情形](images/conscious-scroll.gif)

> [!NOTE]
> 看得見捲軸時，捲軸會在 ScrollViewer 內的內容上面顯示為 16px 的重疊。 為了確保良好 UX 設計，您應該確定此重疊不會遮蔽任何互動式內容。 此外，如果您不希望有 UX 重疊，請在檢視區邊緣保留 16px 的邊框間距，來為捲軸讓出空間。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡以<a href="xamlcontrolsgallery:/item/ScrollViewer">開啟應用程式並查看 ScrollViewer 的運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-scroll-viewer"></a>建立捲動檢視器

若要將垂直捲動功能新增至頁面，請將頁面內容包裝在捲動檢視器中。

```xaml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1">

    <ScrollViewer>
        <StackPanel>
            <TextBlock Text="My Page Title" Style="{StaticResource TitleTextBlockStyle}"/>
            <!-- more page content -->
        </StackPanel>
    </ScrollViewer>
</Page>
```

此 XAML 說明如何啟用水平捲動，將影像放在捲動檢視器中，並啟用縮放功能。

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10"
              HorizontalScrollMode="Enabled" HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

## <a name="scrollviewer-in-a-control-template"></a>控制項範本中的 ScrollViewer

對於 ScrollViewer 控制項而言，當做其他控制項的複合部分存在很獨特。 只有在主控制項的配置空間限制小於展開的內容大小時，ScrollViewer 組件以及用於支援的 [ScrollContentPresenter](/uwp/api/Windows.UI.Xaml.Controls.ScrollContentPresenter) 類別才會將檢視區連同捲軸一起顯示。 這種情況通常發生在清單中，因此 [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) 和 [GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView) 範本一律包含 ScrollViewer。 [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 和 [RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 也在其範本中包含一個 ScrollViewer。

當 **ScrollViewer** 部分存在於控制項中時，主控制項對於讓內容捲動的某些輸入事件以及操作，通常都有內建的事件處理方式。 例如，GridView 會解譯撥動手勢，這會導致水平捲動內容。 主控制項所收到的輸入事件與原始操作都會被視為由控制項處理，因此將不會引發較低層級的事件 (例如 [PointerPressed](/uwp/api/windows.ui.xaml.uielement.pointerpressed))，也不會產生任何父容器。 您可以透過覆寫事件的控制項類別 **On**_Event_ 虛擬方法，或透過重製控制項的範本，來變更部分內建的控制項處理。 但在任一種情況下，重現通常就在那裡的原始預設行為，讓控制項以預期的方式回應事件和使用者的輸入動作與手勢並不容易。 因此，您應該考慮您是否真的需要引發該輸入事件。 您可能會想要調查是否有不是由控制項處理的其他輸入事件或手勢，並在您的 app 中使用那些輸入事件或手勢，或控制互動設計。

為了能夠讓包含 ScrollViewer 的控制項影響來自 ScrollViewer 組件內的部分行為和屬性，ScrollViewer 會定義一些可以在樣式中設定，並在範本繫結中使用的 XAML 附加屬性。 如需有關附加屬性的詳細資訊，請參閱[附加屬性概觀](../../xaml-platform/attached-properties-overview.md)。

**ScrollViewer XAML 附加屬性**

ScrollViewer 會定義下列 XAML 附加屬性︰

- [ScrollViewer.BringIntoViewOnFocusChange](/uwp/api/windows.ui.xaml.controls.scrollviewer.bringintoviewonfocuschange) \(英文\)
- [ScrollViewer.HorizontalScrollBarVisibility](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) \(英文\)
- [ScrollViewer.HorizontalScrollMode](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) \(英文\)
- [ScrollViewer.IsDeferredScrollingEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.isdeferredscrollingenabled) \(英文\)
- [ScrollViewer.IsHorizontalRailEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.ishorizontalrailenabled) \(英文\)
- [ScrollViewer.IsHorizontalScrollChainingEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.ishorizontalscrollchainingenabled) \(英文\)
- [ScrollViewer.IsScrollInertiaEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.isscrollinertiaenabled) \(英文\)
- [ScrollViewer.IsVerticalRailEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.isverticalrailenabled) \(英文\)
- [ScrollViewer.IsVerticalScrollChainingEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.isverticalscrollchainingenabled) \(英文\)
- [ScrollViewer.IsZoomChainingEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled) \(英文\)
- [ScrollViewer.IsZoomInertiaEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled) \(英文\)
- [ScrollViewer.VerticalScrollBarVisibility](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibilityproperty) \(英文\)
- [ScrollViewer.VerticalScrollMode](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) \(英文\)
- [ScrollViewer.ZoomMode](/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode) \(英文\)

這些 XAML 附加屬性適用於 ScrollViewer 是隱含的情況，例如當 ScrollViewer 存在於 ListView 或 GridView 的預設範本中，而且您希望能夠在不存取範本組件的情況下，影響控制項的捲動行為時。

例如，以下是讓 ListView 的內建捲動檢視器永遠顯示垂直捲軸的方式。

```xaml
<ListView ScrollViewer.VerticalScrollBarVisibility="Visible"/>
```

對於 ScrollViewer 在 XAML 中是明確的情況下 (如範例程式碼中所示)，您不需要使用附加屬性語法。 只要使用屬性語法，例如 `<ScrollViewer VerticalScrollBarVisibility="Visible"/>`。


## <a name="dos-and-donts"></a>可行與禁止事項

- 可以的話，請採用垂直捲動的設計，而不是水平捲動。
- 對延長超過一個檢視區界限 (垂直或水平) 的內容區域，使用單軸移動瀏覽。 對延長超過兩個檢視區界限 (垂直和水平) 的內容區域，使用雙軸移動瀏覽。
- 在清單檢視、方格檢視、下拉式方塊、清單方塊、文字輸入方塊及中樞控制項中，使用內建的捲動功能。 如果要一次全部顯示的項目數量太多，請使用上述控制項，讓使用者能夠在項目清單水平或垂直捲動。
- 如果您希望使用者能夠在大型區域雙向移動瀏覽，還能予以縮放，例如，您想讓使用者在完整大小的影像 (而不是根據畫面來調整影像大小) 移動瀏覽和縮放，請將影像放在捲動檢視器內。
- 如果使用者將捲動很冗長的文字訊息，請將捲動檢視器設定為只能垂直捲動。
- 使用捲動檢視器以只限包含一個物件。 請注意，這一個物件可以是配置面板，其本身可以包含數目不拘的物件。
- 如果您需要在可捲動的檢視中處理 [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) 的指標事件 (例如 ScrollViewer 或 ListView)，則必須藉由呼叫 [UIElement.CancelDirectmanipulation()](/uwp/api/windows.ui.xaml.uielement.canceldirectmanipulations)，明確停用對於此檢視元素上操作事件的支援。 若要在檢視中重新啟用操作事件，請呼叫 [UIElement.TryStartDirectManipulation()](/uwp/api/windows.ui.xaml.uielement.trystartdirectmanipulation)。



## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-topics"></a>相關主題

**適用於開發人員 (XAML)**

* [ScrollViewer 類別](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) \(英文\)
