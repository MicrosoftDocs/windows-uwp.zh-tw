---
description: 使用對齊、邊界、邊框間距屬性來排列頁面上元素的版面配置。
title: 版面配置的對齊、邊界及邊框間距
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f2782118b2ed35578ac48f2996839ceefcf5b71b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034891"
---
# <a name="alignment-margin-padding"></a>對齊、邊界、邊框間距

UWP 應用程式中，大部分的使用者介面 (UI) 元素是從 [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) 類別繼承。 每個 FrameworkElement 皆有影響版面配置行為的維度、邊界、邊框間距屬性。 下列指導方針提供有關如何使用這些版面配置屬性，確定您應用程式的 UI 可辨識且在任何操作概觀中皆容易使用。

## <a name="dimensions-height-width"></a>維度 (Height、Width)
適當的大小調整可確保所有的內容清楚可辨識。 使用者不必捲動或縮放即可解讀主要內容。

![顯示維度的圖](images/dimensions.svg)

- [**Height**](/uwp/api/windows.ui.xaml.frameworkelement.height) 和 [**Width**](/uwp/api/windows.ui.xaml.frameworkelement.width) 指定元素的大小。 預設值在數學上來說是 NaN (不是數字)。 您可以設定以 [有效像素](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)衡量的固定值，或者可以使用 **Auto** 或適用於可變式行為的 [等比例調整大小](layout-panels.md#grid)。

- [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) 和 [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 是唯讀屬性，在執行階段提供元素的大小。 如果可變式版面配置擴大或縮小，則這些值會在 [**SizeChanged**](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged)事件中變更。 請注意， [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) 不會變更 ActualHeight 和 ActualWidth 值。

- [**MinWidth**](/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) 和 [**MinHeight**](/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](/uwp/api/windows.ui.xaml.frameworkelement.maxheight) 指定的值會在允許可變式調整大小時限制元素大小。

- [**FontSize**](/uwp/api/windows.ui.xaml.controls.textblock.fontsize) 和其他文字屬性控制了文字元素的版面配置大小。 文字元素沒有有明確宣告維度，但其實已經計算 ActualWidth 和 ActualHeight。 

## <a name="alignment"></a>對應項目
對齊讓您的 UI 外觀看起來整潔、有組織，且平衡，也可以用來建立視覺階層與關聯性。

![顯示對齊方式的圖](images/alignment.svg)

- [**HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) 和 [**VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) 指定元素應該如何放置於其父容器內。
    - 適用於 **HorizontalAlignment** 的值為 **Left** 、 **Center** 、 **Right** 和 **Stretch** 。
    - 適用於 **VerticalAlignment** 的值為 **Top** 、 **Center** 、 **Bottom** 和 **Stretch** 。

- **Stretch** 是這兩個屬性的預設，且元素將填滿父容器中提供給它們的所有空間。 Height 和 Width 的實際數字會取消一個 Stretch 值，而這個值將反而做為 Center 值。 某些控制項 (像是 Button) 會在其預設樣式中覆寫預設 Stretch 值。

- [**HorizontalContentAlignment**](/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) 和 [**VerticalContentAlignment**](/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment) 指定如何將子元素放置於一個容器內。

- 對齊可能會影響配置面板中的裁剪。 例如，使用 `HorizontalAlignment="Left"`，如果內容大於 ActualWidth，則剪裁元素的右側。

- 文字元素使用 [**TextAlignment**](/uwp/api/windows.ui.xaml.textalignment)屬性。 一般而言，我們建議使用靠左對齊 (預設值)。 如需設定文字樣式的詳細資訊，請參閱[印刷樣式](../style/typography.md)。

## <a name="margin-and-padding"></a>邊界及邊框間距
邊界和邊框間距屬性讓 UI 看起來不會過於雜亂或太疏鬆，也可以讓使用特定輸入方式 (像是手寫筆、觸控) 更容易。 以下圖示容器與其內容的邊界和邊框間距。

![XAML 邊界和邊框間距的圖](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>邊界
[**Margin**](/uwp/api/windows.ui.xaml.frameworkelement.margin) 控制元素周圍的空白空間。 邊界不會在 ActualHeight 和 ActualWidth 中增加像素，而且在點擊測試與來源輸入事件中不會被視為元素的一部分。

- 邊界的值可以統一或不同。 使用 `Margin="20"` 時，會對左、上、右、下邊界上的元素一致套用 20 像素的邊界。 使用 `Margin="0,10,5,25"`，會依對左、上、右、下的順序套用值。 

- 邊界可以累加。 若兩個元素同時指定一致的邊界為 10 個像素，且皆為方向不拘的相鄰對等元素，則元素之間的距離會是 20 個像素。

- 允許使用負值邊界。 不過，使用負值邊界經常會造成裁剪或過度繪製對等元素，因此負值邊界並不是常用的技術。

- 最後才限制邊界值，因此使用邊界時請謹慎小心，因為容器可裁剪或限制元素。 邊界值可能會是造成元素無法轉譯的原因；套用邊界，元素的維度可限制為 0。

### <a name="padding"></a>邊框間距
[**Padding**](/uwp/api/windows.ui.xaml.frameworkelement.padding) 控制元素的內部框線及其子內容或元素之間的空間。 正數的 Padding 值會減少元素的內容區域。 

不同於邊界，邊框間距不是 FrameworkElement 的屬性。 有幾個類別，每一個皆定義其自身的邊框間距屬性：

-   [**Control.Padding**](/uwp/api/windows.ui.xaml.controls.control.padding)：會繼承到所有 [**Control**](/uwp/api/windows.ui.xaml.controls) 衍生類別。 不是所有控制項都有內容，因此對於這些控制項而言，設定此屬性沒有作用。 如果控制項有邊框，將會在該邊框內套用邊框間距。
-   [**Border.Padding**](/uwp/api/windows.ui.xaml.controls.border.padding)：定義 [**BorderThickness**](/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[**BorderBrush**](/uwp/api/windows.ui.xaml.controls.border.borderbrush) 所建立的矩形線條與 [**Child**](/uwp/api/windows.ui.xaml.controls.border.child) 元素之間的空間。
-   [**ItemsPresenter.Padding**](/uwp/api/windows.ui.xaml.controls.itemspresenter.padding)：作用在項目控制項中的項目外觀，在每個項目周圍放上指定的邊框間距。
-   [**TextBlock.Padding**](/uwp/api/windows.ui.xaml.controls.textblock.padding) 與 [**RichTextBlock.Padding**](/uwp/api/windows.ui.xaml.controls.richtextblock.padding)：展開文字元素的文字周圍的周框方塊。 這些文字元素沒有 **Background** ，可能不容易看出。 基於這個原因，改為在 [**Block**](/uwp/api/windows.ui.xaml.documents.block)容器上使用 [**Margin**](/uwp/api/windows.ui.xaml.documents.block.margin) 設定。

不論上述哪一種情況，元素也具有邊界屬性。 若同時套用邊界與邊框間距，兩者會相加：外部容器與任何內部內容之間的外觀距離等於邊界加上邊框間距。

### <a name="example"></a>範例
讓我們看一下 Margin 和 Padding 在實際控制項上的效果。 以下是 Grid 內部的 TextBox，且預設的 Margin 和 Padding 值為 0。

![邊界及邊框間距為 0 的 TextBox](images/xaml-layout-textbox-no-margins-padding.svg)

以下是 TextBox 上含有 Margin 和 Padding 值的相同 TextBox 和 Grid，如這個 XAML 中所示。

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="16,24"/>
</Grid>
```

![含有正數邊界及邊框間距值的 TextBox](images/xaml-layout-textbox-with-margins-padding.svg)


## <a name="style-resources"></a>樣式資源
您不需要在控制項上個別設定每個屬性值。 將屬性值群組到 [**Style**](/uwp/api/Windows.UI.Xaml.Style) 資源中並將 Style 套用到控制項，通常更有效率。 這尤其適用於當您需要將相同屬性值套用到許多控制項的情況。 如需有關使用樣式的詳細資訊，請參閱 [XAML 樣式](../controls-and-patterns/xaml-styles.md)。

## <a name="general-recommendations"></a>一般建議
- 只將測量值套用到特定按鍵元素，其他元素則使用可變式配置行為。 視窗寬度變更時，這可提供給[回應式 UI](responsive-design.md)。

- 如果您使用測量值， **所有維度、邊界、邊框間距皆應以 4 個有效像素 (epx) 遞增** 。 當 UWP 使用[有效像素與縮放比例](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) 讓您的應用程式在所有裝置與螢幕尺寸上清楚可辨，它是以 4 的倍數縮放 UI 元素。 藉由符合整數像素，使用以 4 遞增的值，呈現最佳轉譯。

- 針對小的視窗寬度 (小於 640 像素)，建議使用 12 epx 裝訂邊，較大的視窗寬度，建議使用 24 epx 裝訂邊。

![建議的裝訂邊](images/12-gutter.svg)

## <a name="related-topics"></a>相關主題
* [**FrameworkElement.Height**](/uwp/api/windows.ui.xaml.frameworkelement.height)
* [**FrameworkElement.Width**](/uwp/api/windows.ui.xaml.frameworkelement.width)
* [**FrameworkElement.HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [**FrameworkElement.VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [**FrameworkElement.Margin**](/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [**Control.Padding**](/uwp/api/windows.ui.xaml.controls.control.padding)
