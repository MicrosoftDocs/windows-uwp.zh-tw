---
description: 了解如何使用具有自動調整大小、版面配置面板、視覺狀態和個別 UI 定義的 XAML 彈性版面配置系統來建立回應式 UI。
title: 使用 XAML 的回應式版面配置
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: contperf-fy21q2
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: e2bc29acc63a5363891ad78b873db6c8311ac970
ms.sourcegitcommit: 06d59b59a95aad009acb947a0dac7432116bdb60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/16/2021
ms.locfileid: "100544648"
---
# <a name="responsive-layouts-with-xaml"></a>使用 XAML 的回應式版面配置

XAML 版面配置系統提供自動調整元素大小、版面配置面板、視覺狀態等功能，來協助您建立回應式 UI。 有了回應式版面配置，您就可以讓應用程式在不同應用程式視窗尺寸、解析度、像素密度與方向的螢幕上看起來更美觀。 您也可以使用 XAML 調整位置、調整大小、顯示/隱藏、取代，或重新設計您應用程式的 UI，如同[回應式設計技術](responsive-design.md)中所討論。 以下，我們討論如何搭配 XAML 實作回應式版面配置。

## <a name="fluid-layouts-with-properties-and-panels"></a>具有屬性與面板的流暢版面配置

回應式版面配置的基礎在於適當地使用 XAML 版面配置屬性和面板，在流暢方式中進行內容的重新置放、調整大小及自動重排。

XAML 版面配置系統支援靜態與流暢版面配置。 在靜態配置中，您提供控制項明確的像素大小與位置。 當使用者變更裝置的解析度或方向時，UI 不會變更。 靜態版面配置在不同的硬體規格、畫面大小中會遭到裁剪。 相反的，流暢的版面配置可縮小、放大和自動重排，以回應裝置上的可用視覺空間。

實際上，可以使用靜態與流暢元素的組合來建立 UI。 您仍然會在某些地方使用靜態元素與值，但請確定整體 UI 是否回應不同的解析度、螢幕大小及檢視。

我們將在此處討論如何使用 XAML 屬性和版面配置面板，建立流暢版面配置。

### <a name="layout-properties"></a>版面配置屬性
版面配置屬性控制元素的大小與位置。 若要建立流暢版面配置，請針對元素使用自動或等比例調整大小，並視需要允許版面配置面板放置其子系。

以下是一些常見的版面配置屬性，以及如何加以使用來建立流暢版面配置。

**Height 與 Width**

[**Height**](/uwp/api/windows.ui.xaml.frameworkelement.height) \(英文\) 與 [**Width**](/uwp/api/windows.ui.xaml.frameworkelement.width) \(英文\) 屬性指定元素的大小。 您可以使用以有效像素衡量的固定值，或者可以使用自動或等比例調整大小。

自動調整大小會調整 UI 元素大小，以符合其的內容或父容器。 您也可以使用自動調整大小搭配方格的列與欄。 若要使用自動調整大小，請將 UI 元素的 Height 和/或 Width 設定為 **Auto**。

> [!NOTE]
> 元素是否會調整大小以符合其內容或容器，取決於父容器如何處理調整其子項的大小。 如需詳細資訊，請參閱此文章後續內容中的[版面配置面板](#layout-panels)。

等比例調整大小 (亦稱為「星號調整」  ) 會按照權重比例，將可用的空間分配給方格的列和欄。 在 XAML 中，星號值的表示方法為 \* (針對加權星號調整則為 *n*\*)。 例如，若要在 2 欄的版面配置中，將某一欄的寬度設定為第二欄的 5 倍，請使用 "5\*" 與 "\*" 來表示 [**ColumnDefinition**](/uwp/api/Windows.UI.Xaml.Controls.ColumnDefinition) \(英文\) 元素中的 [**Width**](/uwp/api/windows.ui.xaml.controls.columndefinition.width) \(英文\) 屬性。

這個範例會在具有 4 欄的 [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) 中結合固定、自動和等比例調整大小。

| Column | 調整大小 | 描述 |
| ------ | ------ | ----------- |
Column_1 | **Auto** | 會調整欄的大小以容納其內容。
Column_2 | * | 計算 Auto 欄之後，這個欄會分配到一部分的剩餘寬度。 Column_2 會是 Column_4 的一半寬度。
Column_3 | **44** | 此欄寬度為 44 個像素。
Column_4 | **2**\* | 計算 Auto 欄之後，這個欄會分配到一部分的剩餘寬度。 Column_4 會是 Column_2 的兩倍寬度。

預設欄的寬度是 "*"，因此不需要為第二欄明確設定這個值。

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its content." FontSize="24"/>
</Grid>
```

在 Visual Studio XAML 設計工具中，結果看起來就像這樣。

![Visual Studio 設計工具中的 4 欄格線](images/xaml-layout-grid-in-designer.png)

若要在執行階段取得元素的大小，請使用唯讀 [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) \(英文\) 與 [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) \(英文\) 屬性，而不是 Height 與 Width。

**大小限制**

在 UI 中使用自動調整大小時，仍然需要設置元素大小的限制。 您可以設定 [**MinWidth**](/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) 和 [**MinHeight**](/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](/uwp/api/windows.ui.xaml.frameworkelement.maxheight) 屬性，來指定限制元素大小的值，同時允許流暢的調整大小。

在 Grid 中，MinWidth/MaxWidth 也可以與欄定義搭配使用，而 MinHeight/MaxHeight 可以與列定義搭配使用。

**對齊**

使用 [**HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) 和 [**VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) 屬性，來指定元素應該如何放置於其父容器內。
- 適用於 **HorizontalAlignment** 的值為 **Left**、**Center**、**Right** 和 **Stretch**。
- 適用於 **VerticalAlignment** 的值為 **Top**、**Center**、**Bottom** 和 **Stretch**。

利用 **Stretch** 對齊方式，元素將可填滿父容器中提供給它們的所有空間。 Stretch 是這兩個對齊屬性的預設值。 不過，某些控制項 (像是 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)) 會在其預設樣式中覆寫這個值。
任何可含有子元素的元素都能以獨特的方式來處理 HorizontalAlignment 和 VerticalAlignment 屬性的 Stretch 值。 例如，使用放置於 Grid 中之預設 Stretch 值的元素，會向兩邊延伸以填滿包含它的儲存格。 放置於 Canvas 中的相同元素會調整大小以符合其內容。 如需每個面板如何處理 Stretch 值的詳細資訊，請參閱[版面配置面板](layout-panels.md)文章。

如需詳細資訊，請參閱 [對齊方式、邊界及邊框間距](alignment-margin-padding.md)文章，以及 [**HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) 和 [**VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) 參考頁面。

**可見度**

您可以將元素的 [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) \(英文\) 屬性設定為其中一個 [**Visibility** 列舉](/uwp/api/Windows.UI.Xaml.Visibility) \(英文\) 值，以顯示或隱藏元素：**Visible** 或 **Collapsed**。 當元素是 Collapsed 時，它不會佔用 UI 版面配置中的任何空間。

您可以在程式碼或視覺狀態中變更元素的 Visibility 屬性。 當元素的 Visibility 變更時，其所有子元素也會變更。 您可以藉由顯示某一個面板，同時摺疊另一個面板，來取代 UI 的區段。

> [!Tip]
> 當您在 UI 中具有預設是 **Collapsed** 的元素時，仍會在啟動期間建立物件，即使其不會顯示也一樣。 您可以延遲載入這些元素，直到藉由將 **x:DeferLoadStrategy attribute** 屬性設為 "Lazy" 來顯示它們為止。 這可以提升啟動效能。 如需詳細資訊，請參閱 [x:DeferLoadStrategy 屬性](../../xaml-platform/x-deferloadstrategy-attribute.md)。

### <a name="style-resources"></a>樣式資源

您不需要在控制項上個別設定每個屬性值。 將屬性值群組到 [**Style**](/uwp/api/Windows.UI.Xaml.Style) 資源中並將 Style 套用到控制項，通常更有效率。 這尤其適用於當您需要將相同屬性值套用到許多控制項的情況。 如需有關使用樣式的詳細資訊，請參閱[設定控制項的樣式](../controls-and-patterns/xaml-styles.md)。

### <a name="layout-panels"></a>版面配置面板

若要定位視覺物件，您必須將它們放在 Panel 或其他容器物件中。 XAML 架構提供各種 Panel 類別 (例如 [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas)、[**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid)、[**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) 和 [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel)) 做為容器，您可以在其中放置和排列 UI 元素。

選擇版面配置面板的最重要考量是面板如何放置它的子元素以及調整其大小。 您也需要考慮重疊的子元素如何彼此交疊。

以下是在 XAML 架構中提供的面板控制項的主要功能比較。

面板控制項 | 說明
--------------|------------
[**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) \(英文\) | **Canvas** 不支援流暢的 UI；您可以完全控制放置子元素及調整其大小的各方面設定。 您通常會在特殊的情況下使用它，例如，建立圖形或定義較大型調適型 UI 的小型靜態區域。 您可以使用程式碼或視覺狀態，在執行階段重新置放元素。<li>元素是使用 Canvas.Top 與 Canvas.Left 附加屬性以絕對位置的方式來放置。</li><li>圖層可以使用 Canvas.ZIndex 附加屬性明確指定。</li><li>適用於 HorizontalAlignment/VerticalAlignment 的 Stretch 值都會遭到忽略。 如果沒有明確設定元素的大小，它就會調整其大小來符合它的內容。</li><li>如果子內容大於面板，就不會以視覺化方式進行剪裁。 </li><li>子內容不會受限於面板的範圍內。</li>
[**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) \(英文\) | **Grid** 支援流暢地調整子元素大小。 您可以使用程式碼或視覺狀態，重新置放和自動重排元素。<li>元素是使用 Grid.Row 與 Grid.Column 附加屬性，以列和欄形式來排列。</li><li>您可以使用 Grid.RowSpan 與 Grid.ColumnSpan 附加屬性，讓元素橫跨多個列與欄。</li><li>系統會採用適用於 HorizontalAlignment/VerticalAlignment 的 Stretch 值。 如果沒有明確設定元素的大小，它會向兩邊延伸以填滿方格儲存格中的可用空間。</li><li>如果子內容大於面板，就會以視覺化方式進行剪裁。</li><li>內容大小受限於面板的範圍，因此可捲動的內容會視需要顯示捲軸。</li>
[**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) \(英文\) | <li>元素是以相較於面板的邊緣或中心，以及彼此相對的關係來排列。</li><li>元素是使用各種不同的附加屬性來放置，這些屬性可控制面板對齊方式、同層級對齊方式及同層級位置。 </li><li>除非用來對齊的 RelativePanel 附加屬性會造成向兩邊延伸 (例如，元素會向面板的左右邊緣對齊)，否則 HorizontalAlignment/VerticalAlignment 的 Stretch 值會遭到忽略。 如果沒有明確設定元素的大小且它不會向兩邊延伸，則它會調整大小來符合其內容。</li><li>如果子內容大於面板，就會以視覺化方式進行剪裁。</li><li>內容大小受限於面板的範圍，因此可捲動的內容會視需要顯示捲軸。</li>
[**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) \(英文\) |<li>元素以垂直或水平方式堆疊到單行中。</li><li>適用於 HorizontalAlignment/VerticalAlignment 的 Stretch 值會以與 Orientation 屬性相反的方向來採用。 如果沒有明確設定元素的大小，它會向兩邊延伸以填滿可用的寬度 (或高度，如果 Orientation 是 Horizontal)。 利用 Orientation 屬性指定的方向，元素會調整大小來符合其內容。</li><li>如果子內容大於面板，就會以視覺化方式進行剪裁。</li><li>內容大小不會以 Orientation 屬性指定的方向受限於面板的範圍內，因此，可捲動內容向兩邊延伸的範圍會超過面板的範圍且不會顯示捲軸。 您必須明確限制子內容的高度 (或寬度)，讓它能夠顯示捲軸。</li>
[**VariableSizedWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) \(英文\) |<li>在列或欄中排列的元素，達到 MaximumRowsOrColumns 值時會自動換行到新列或新欄。</li><li>Orientation 屬性會指定以列或欄排列元素。</li><li>您可以使用 VariableSizedWrapGrid.RowSpan 與 VariableSizedWrapGrid.ColumnSpan 附加屬性，讓元素橫跨多個列與欄。</li><li>適用於 HorizontalAlignment/VerticalAlignment 的 Stretch 值都會遭到忽略。 元素的大小是由 ItemHeight 與 ItemWidth 屬性所指定。 如果未設定這些屬性，則會從第一個資料格的大小取得它們的值。</li><li>如果子內容大於面板，就會以視覺化方式進行剪裁。</li><li>內容大小受限於面板的範圍，因此可捲動的內容會視需要顯示捲軸。</li>

如需這些面板的詳細資訊和範例，請參閱[版面配置面板](layout-panels.md)。 另請參閱[回應技術範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlResponsiveTechniques)。

版面配置面板可讓您將 UI 組織成控制項的邏輯群組。 將它們與適當的屬性設定搭配使用時，您可以取得自動調整大小、重新置放及自動重排 UI 元素的一些支援。 不過，大部分的 UI 版面配置需要在視窗大小有大幅變更時進一步修改。 為此，您可以使用視覺狀態。

## <a name="adaptive-layouts-with-visual-states-and-state-triggers"></a>使用視覺效果狀態和狀態觸發程序的彈性版面配置
使用視覺效果狀態，根據視窗大小或其他變更，大幅更改您的 UI。

當應用程式視窗放大或縮小的範圍超過一定數量時，您可能想要更改版面配置屬性，以重新置放、調整大小、自動重排或取代 UI 的區段。 您可以為 UI 定義不同的視覺狀態，並在視窗寬度或視窗長度超出指定閾值時套用它們。

[  **VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) 會定義在其處於特殊狀態時要套用到元素的屬性值。 您會在 [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) 中群組視覺狀態，在符合特定條件時套用適當的 VisualState。 [ **AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 提供一種簡單的方法，來設定在 XAML 套用狀態的閾值 (也稱為「中斷點」)。 或者，您可以在程式碼中呼叫 [**VisualStateManager.GoToState**](/uwp/api/windows.ui.xaml.visualstatemanager.gotostate) 方法，來套用視覺狀態。 這兩種方式的範例會在下一節中顯示。

### <a name="set-visual-states-in-code"></a>在程式碼中設定視覺狀態

若要從程式碼套用視覺狀態，您可以呼叫 [**VisualStateManager.GoToState**](/uwp/api/windows.ui.xaml.visualstatemanager.gotostate) 方法。 例如，若要在 app 視窗為特定大小時套用某個狀態，請處理 [**SizeChanged**](/uwp/api/windows.ui.xaml.window.sizechanged) 事件並呼叫 **GoToState** 以套用適當的狀態。

此處的 [**VisualStateGroup**](/uwp/api/Windows.UI.Xaml.VisualStateGroup) \(英文\) 包含兩個 VisualState 定義。 第一個是 `DefaultState`，是空的。 套用時，即會套用 XAML 頁面中定義的值。 第二個是 `WideState`，它會將 [**SplitView**](/uwp/api/Windows.UI.Xaml.Controls.SplitView) 的 [**DisplayMode**](/uwp/api/windows.ui.xaml.controls.splitview.displaymode) 屬性變更為 **Inline** 並開啟窗格。 如果視窗寬度比 640 個有效像素來得大，即會在 SizeChanged 事件處理常式中套用此狀態。

> [!NOTE]
> Windows 不會針對您的應用程式提供一個偵測執行您應用程式之特定裝置的方法。 它能夠告知您執行應用程式的裝置系列 (行動、桌面等)、實際解析度，以及應用程式可用的螢幕空間量 (應用程式的視窗大小)。 我們建議為[螢幕大小與中斷點](screen-sizes-and-breakpoints-for-responsive-design.md)定義視覺狀態。

```xaml
<Page ...
    SizeChanged="CurrentWindow_SizeChanged">
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="DefaultState">
                        <Storyboard>
                        </Storyboard>
                    </VisualState>

                <VisualState x:Name="WideState">
                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.DisplayMode"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0">
                                <DiscreteObjectKeyFrame.Value>
                                    <SplitViewDisplayMode>Inline</SplitViewDisplayMode>
                                </DiscreteObjectKeyFrame.Value>
                            </DiscreteObjectKeyFrame>
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.IsPaneOpen"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

```csharp
private void CurrentWindow_SizeChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
{
    if (e.Size.Width > 640)
        VisualStateManager.GoToState(this, "WideState", false);
    else
        VisualStateManager.GoToState(this, "DefaultState", false);
}
```

```cppwinrt
// YourPage.h
void CurrentWindow_SizeChanged(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::SizeChangedEventArgs const& e);

// YourPage.cpp
void YourPage::CurrentWindow_SizeChanged(IInspectable const& sender, SizeChangedEventArgs const& e)
{
    if (e.NewSize.Width > 640)
        VisualStateManager::GoToState(*this, "WideState", false);
    else
        VisualStateManager::GoToState(*this, "DefaultState", false);
}

```

### <a name="set-visual-states-in-xaml-markup"></a>在 XAML 標記中設定視覺狀態

在 Windows 10 之前，VisualState 定義需要 [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) 物件來進行屬性變更，而且您必須在程式碼中呼叫 **GoToState** 來套用狀態。 如同先前範例所示。 您仍然會看到許多範例使用此語法，或者您現在可能具有會用到它的程式碼。

從 Windows 10 開始，您可以使用此處所示的簡化 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 語法，而且可以在 XAML 標記中使用 [**StateTrigger**](/uwp/api/Windows.UI.Xaml.StateTrigger) 來套用狀態。 您使用狀態觸發程序來建立簡單的規則，這些規則會自動觸發視覺狀態變更以回應應用程式事件。

這個範例與上一個範例相同，但會使用簡化的 **Setter** 語法 (而不是 Storyboard) 來定義屬性變更。 此外，它不會呼叫 GoToState，而是改用內建的 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 狀態觸發程序來套用狀態。 當您使用狀態觸發程序時，不需要定義空的 `DefaultState`。 如果不再符合狀態觸發程序的條件，即會自動重新套用預設設定。

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <!-- VisualState to be triggered when the
                             window width is >=640 effective pixels. -->
                        <AdaptiveTrigger MinWindowWidth="640" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="mySplitView.DisplayMode" Value="Inline"/>
                        <Setter Target="mySplitView.IsPaneOpen" Value="True"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

> [!Important]
> 在先前的範例中，已在 **Grid** 元素上設定 VisualStateManager.VisualStateGroups 附加屬性。 使用 StateTrigger 時，請一律確保會將 VisualStateGroups 附加到根目錄的第一個子項，讓觸發程序能夠自動生效 (此處的 **Grid** 是根 **Page** 元素的第一個子項)。

### <a name="attached-property-syntax"></a>附加屬性語法

在 VisualState 中，您通常會設定控制項屬性的值，或者為包含該控制項之面板的其中一個附加屬性設定值。 設定附加屬性時，請使用括號將附加屬性名稱括起來。

這個範例示範如何在名為 `myTextBox` 的 TextBox 上，設定 [**RelativePanel.AlignHorizontalCenterWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanelproperty) 附加屬性。 第一個 XAML 會使用 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 語法，而第二個會使用 **Setter** 語法。

```xaml
<!-- Set an attached property using ObjectAnimationUsingKeyFrames. -->
<ObjectAnimationUsingKeyFrames
    Storyboard.TargetProperty="(RelativePanel.AlignHorizontalCenterWithPanel)"
    Storyboard.TargetName="myTextBox">
    <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
</ObjectAnimationUsingKeyFrames>

<!-- Set an attached property using Setter. -->
<Setter Target="myTextBox.(RelativePanel.AlignHorizontalCenterWithPanel)" Value="True"/>
```

### <a name="custom-state-triggers"></a>自訂狀態觸發程序

您可以擴充 [**StateTrigger**](/uwp/api/Windows.UI.Xaml.StateTrigger) 類別，針對各種案例建立自訂觸發程序。 例如，可以建立 StateTrigger，根據輸入類型來觸發不同狀態，然後在輸入類型為觸控時，增加控制項四周的邊界。 或是建立 StateTrigger，以根據應用程式執行所在的裝置系列來套用不同的狀態。 如需如何建置自訂觸發程序並使用它們從單一 XAML 檢視中建立最佳化 UI 體驗的範例，請參閱[狀態觸發程序範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers)。

### <a name="visual-states-and-styles"></a>視覺狀態與樣式

您可以在視覺狀態中使用 Style 資源，將一組屬性變更套用到多個控制項。 如需有關使用樣式的詳細資訊，請參閱[設定控制項的樣式](../controls-and-patterns/xaml-styles.md)。

在這個來自狀態觸發程序範例的簡化 XAML 中，Style 資源被套用到 Button，以調整滑鼠或觸控輸入的大小和邊界。 如需自訂狀態觸發程序的完整程式碼和定義，請參閱[狀態觸發程序範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers)。

```xaml
<Page ... >
    <Page.Resources>
        <!-- Styles to be used for mouse vs. touch/pen hit targets -->
        <Style x:Key="MouseStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="5" />
            <Setter Property="Height" Value="20" />
            <Setter Property="Width" Value="20" />
        </Style>
        <Style x:Key="TouchPenStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="15" />
            <Setter Property="Height" Value="40" />
            <Setter Property="Width" Value="40" />
        </Style>
    </Page.Resources>

    <RelativePanel>
        <!-- ... -->
        <Button Content="Color Palette Button" x:Name="MenuButton">
            <Button.Flyout>
                <Flyout Placement="Bottom">
                    <RelativePanel>
                        <Rectangle Name="BlueRect" Fill="Blue"/>
                        <Rectangle Name="GreenRect" Fill="Green" RelativePanel.RightOf="BlueRect" />
                        <!-- ... -->
                    </RelativePanel>
                </Flyout>
            </Button.Flyout>
        </Button>
        <!-- ... -->
    </RelativePanel>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="InputTypeStates">
            <!-- Second set of VisualStates for building responsive UI optimized for input type.
                 Take a look at InputTypeTrigger.cs class in CustomTriggers folder to see how this is implemented. -->
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- This trigger indicates that this VisualState is to be applied when MenuButton is invoked using a mouse. -->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Mouse" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource MouseStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource MouseStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- Multiple trigger statements can be declared in the following way to imply OR usage.
                         For example, the following statements indicate that this VisualState is to be applied when MenuButton is invoked using Touch OR Pen.-->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Touch" />
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Pen" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Page>
```

## <a name="related-topics"></a>相關主題

- [教學課程：建立調適型版面配置](../basics/xaml-basics-adaptive-layout.md)
- [回應技術範例 (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlResponsiveTechniques) \(英文\)
- [狀態觸發程序範例 (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers) \(英文\)
- [量身打造的多個檢視範例 (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlTailoredMultipleViews) \(英文\)
