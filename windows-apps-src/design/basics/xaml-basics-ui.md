---
title: 建立使用者介面教學課程
description: 遵循此教學課程，以了解如何使用 Visual Studio 中的 XAML 工具來建立影像編輯程式的基本 UI。
keywords: XAML, UWP, 開始使用
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c1754edd6901b5de778b40294078b16c03dc0dda
ms.sourcegitcommit: 40b890c7b862f333879887cc22faff560c49eae6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97598819"
---
# <a name="tutorial-create-a-user-interface"></a>教學課程：建立使用者介面

在本教學課程中，您將透過下列方式了解如何建立影像編輯程式的基本 UI：

+ 使用 Visual studio 中的 XAML 工具 (例如 XAML 設計工具、工具箱、XAML 編輯器、[屬性]  面板和文件大綱) 將控制項和內容新增至您的 UI。
+ 使用一些最常見的 XAML 配置面板，例如 `RelativePanel`、`Grid` 和 `StackPanel`。

影像編輯程式有兩個頁面。 「主頁面」  會顯示影像中心檢視，以及一些關於每個影像檔案的資訊。

![主頁面](images/xaml-basics/mainpage.png)

「詳細資料頁面」  會在選取單一相片之後，顯示該相片。 飛出視窗編輯功能表可用來變更、重新命名和儲存相片。

![詳細資料頁面](images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>必要條件

+ Visual Studio 2019：[下載 Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (此為免費的社群版本)。
+ Windows 10 SDK (10.0.17763.0 或更新版本)：[下載最新 Windows SDK (免費)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10，版本 1809 或更新版本

## <a name="part-0-get-the-starter-code-from-github"></a>第 0 部分：從 GitHub 取得起始程式碼

針對此教學課程，您將從簡化版的 PhotoLab 範例開始著手。

1. 移至 GitHub 範例頁面：[https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)。
2. 下一步，您將需要複製或下載範例。 選取 [複製或下載]  按鈕。 子功能表會出現。
    ![PhotoLab 範例的 GitHub 頁面上的複製或下載功能表](images/xaml-basics/clone-repo.png)

    **如果您不熟悉 GitHub：**

    a. 選取 [下載 ZIP]  ，然後在本機儲存此檔案。 這個下載的 .zip 檔案，包含您所需要的所有專案檔案。

    b. 將檔案解壓縮。 使用 [檔案總管] 瀏覽至您下載的 .zip 檔案，以滑鼠右鍵按一下檔案，然後選取 [解壓縮全部...]。

    c. 瀏覽至您範例的本機複製，並移至 `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface` 目錄。

    **如果您熟悉 GitHub：**

    a. 在本機複製存放庫中的主要分支。

    b. 瀏覽至 `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface` 目錄。

3. 按兩下 `Photolab.sln`，在 Visual Studio 中開啟解決方案。

## <a name="part-1-add-a-textblock-control-by-using-xaml-designer"></a>第 1 部分：使用 XAML 設計工具來新增 TextBlock 控制項

Visual Studio 提供幾項工具，讓您建立 XAML UI 更輕鬆。

+ 將控制項從 [工具箱] 拖曳到 [XAML 設計工具] 的設計介面上，即可在執行應用程式之前，先了解其外觀。
+ [屬性] 面板可讓您檢視和設定設計工具使用中控制項的所有屬性。
+ [文件大綱] 會顯示 UI 的 XAML 視覺化樹狀結構的父系/子系結構。
+ [XAML 編輯器] 可讓您直接輸入和修改 XAML 標記。

以下是已標示工具的 Visual Studio UI。

![Visual Studio 版面配置](images/xaml-basics/visual-studio-tools.png)

這些工具都會讓您建立 UI 更輕鬆，因此本教學課程會說明所有的工具。 一開始會使用 XAML 設計工具來新增控制項。

若要使用 XAML 設計工具來新增控制項：

1. 在方案總管中按兩下 **MainPage.xaml** 將它開啟。 此步驟會顯示未加入任何 UI 項目的應用程式主頁面。

2. 繼續進行之前，您需要對 Visual studio 進行一些調整：

    + 確認 [方案平台] 已設定為 x86 或 x64，而非 ARM。
    + 將主頁面 XAML 設計工具設定為顯示 13.3 吋桌面預覽。

    您應該會在視窗頂端附近看見這兩設定，如這裡所示。

    ![Visual Studio 設定](images/xaml-basics/layout-vs-settings.png)

    您現在可以執行應用程式，但看到的內容不多。 我們來新增一些 UI 元素，讓事情變得更加有趣。

3. 在工具箱中，展開 [通用 XAML 控制項]  ，並尋找 [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) 控制項。 將 `TextBlock` 控制項拖曳到設計介面上，並放置在頁面左上角附近。

    `TextBlock` 控制項會新增至頁面，設計工具會根據它對您所要版面配置的最佳猜測設定一些屬性。 `TextBlock` 控制項周圍出現藍色反白，表示它現在是使用中物件。 留意設計工具新增的邊界及其他設定。

    您的 XAML 看起來會像下面這樣。 如果格式不完全一樣，不用擔心。 這裡只是簡化了一些，讓您更容易看懂。

    ```xaml
    <TextBlock HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    在下一個步驟中，您將會更新這些值。

4. 在 [屬性] 面板中，將 `TextBlock` 控制項的 [名稱] 值變更為 **TitleTextBlock**。 (請確定 `TextBlock` 控制項仍為使用中物件)。

5. 在 [通用]  下方，將 [文字]  值變更為 **Collection**。

    ![TextBlock 屬性](images/xaml-basics/text-block-properties.png)

    在 XAML 編輯器中，XAML 目前看起來會像這樣。

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="Collection"
               VerticalAlignment="Top"/>
    ```

6. 若要放置 `TextBlock` 控制項，您必須先移除 Visual studio 所新增的屬性值。 在 [文件大綱] 中，以滑鼠右鍵按一下 [TitleTextBlock]  ，然後選取 [配置]   > [全部重設]  。

    ![文件大綱](images/xaml-basics/doc-outline-reset.png)

7. 在 [屬性]  面板中，在搜尋方塊中輸入 margin`Margin`，即可輕鬆找到 Margin`Margin` 屬性。 將左邊界和下邊界設定為 24。

    ![TextBlock 邊界](images/xaml-basics/margins.png)

    邊界提供元素在頁面上最基本的定位。 對微調版面配置非常有用，但應該避免使用那些像 Visual Studio 所新增的大邊界值。 大邊界值會讓 UI 很難適應各種不同的螢幕大小。

    如需詳細資訊，請參閱[對齊、邊界及邊框間距](../layout/alignment-margin-padding.md)。

8. 在 [文件大綱] 中，以滑鼠右鍵按一下 **TitleTextBlock**，然後選取 [編輯樣式]   > [套用資源]   > [TitleTextBlockStyle]  。 此步驟會對標題文字套用系統定義樣式。

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. 在 [屬性] 面板的搜尋方塊中輸入 **textwrapping**，即可輕鬆找到 `TextWrapping` 屬性。 選取 `TextWrapping` 屬性的 _屬性標記_ 以開啟其功能表。 (屬性標記是小方塊符號，位於每個屬性值右邊。 屬性標記是黑色的，表示屬性設定為非預設值)。在 [屬性] 功能表上，選取 [重設] 來重設 `TextWrapping` 屬性。

    Visual Studio 會新增此屬性，但這已經在您套用的樣式中設定，因此這裡不需要該屬性。

您已將 UI 的第一個部分新增至您的應用程式。 立即執行應用程式，看看會是什麼樣子。

> [!NOTE]
> 在本教學課程的這一部分，您已藉由拖曳方式新增了控制項。 您也可以在 [工具箱] 中按兩下控制項來新增一個控制項。 試試看，了解 Visual Studio 產生的 XAML 有何不同。

## <a name="part-2-add-a-gridview-control-by-using-the-xaml-editor"></a>第 2 部分：使用 XAML 編輯器來新增 GridView 控制項

在第 1 部分中，您已嘗試使用 Visual Studio 提供的 XAML 設計工具以及一些其他工具。 您在這裡將會使用 XAML 編輯器直接處理 XAML 標記。 當您更加熟悉 XAML 時，可能會發現這是更有效率的工作方式。

首先，您會將根配置 [Grid](/uwp/api/windows.ui.xaml.controls.grid) 取代為 [RelativePanel](/uwp/api/windows.ui.xaml.controls.relativepanel)。 `RelativePanel` 可讓您更輕鬆地相對於面板或 UI 的其他部分重新排列 UI 區塊。 您會在 [XAML 調適型配置](xaml-basics-adaptive-layout.md)教學課程了解其實用性。

然後再新增 [GridView](/uwp/api/windows.ui.xaml.controls.gridview) 控制項來顯示您的資料。

若要使用 XAML 編輯器來新增控制項：

1. 在 XAML 編輯器中，將根 `Grid` 變更為 `RelativePanel`。

    **之前**

    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <TextBlock x:Name="TitleTextBlock"
                     Text="Collection"
                     Margin="24,0,0,24"
                     Style="{StaticResource TitleTextBlockStyle}"/>
    </Grid>
    ```

    **之後**

    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
    </RelativePanel>
    ```

    如需有關使用 `RelativePanel` 之配置的詳細資訊，請參閱[版面配置面板](../layout/layout-panels.md#relativepanel)。

2. 在 `TextBlock` 元素下方，新增名為 **ImageGridView** 的 `GridView` 控制項。 設定 `RelativePanel` _附加屬性_，以將控制項放置在標題文字下方，並讓它伸展橫跨整個螢幕寬度。

    **新增此 XAML**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **在 TextBlock 之後**

    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>

        <!-- Add the GridView control here. -->

    </RelativePanel>
    ```

    如需有關 Panel 附加屬性的詳細資訊，請參閱[版面配置面板](../layout/layout-panels.md)。

3. 為了讓 `GridView` 控制項顯示任何內容，您必須為其提供要顯示的資料集合。 開啟 **MainPage.xaml.cs** 並尋找 `GetItemsAsync` 方法。 這個方法會填入名為 **Images** 的集合，這是我們已在 **MainPage** 中新增的屬性。

    在 `GetItemsAsync` 方法的結尾，加入這行程式碼。

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    這會將 `GridView` 控制項的 [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性設定為應用程式的 **Images** 集合。 此步驟也會提供一些資料供 `GridView` 顯示。

這執行應用程式的適當位置，可確保一切運作正常。 看起來應該會像這樣。

![應用程式 UI 檢查點 1](images/xaml-basics/layout-0.png)

您會發現應用程式並沒有顯示影像。 根據預設，它會顯示集合中資料類型的 `ToString` 值。 接下來，您要建立資料範本來定義資料的顯示方式。

> [!NOTE]
> 您可以使用[版面配置面板](../layout/layout-panels.md#relativepanel)文章中的 `RelativePanel` 來深入了解版面配置。 看看一些不同的版面配置，然後在 `RelativePanel` 和 `TextBlock` 上設定 `GridView` 附加屬性，實驗一下這些版面配置。

## <a name="part-3-add-a-datatemplate-object-to-display-your-data"></a>第 3 部分：新增 DataTemplate 物件以顯示您的資料

現在您將會建立告知 `GridView` 控制項如何顯示資料的 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) 物件。 如需資料範本完整說明，請參閱[項目容器與範本](../controls-and-patterns/item-containers-templates.md)。

您暫時只要新增預留位置來協助建立所需的版面配置。 在 [XAML 資料繫結](../../data-binding/xaml-basics-data-binding.md)教學課程中，您將會以 `ImageFileInfo` 類別中的實際資料取代這些預留位置。 如果現在想要了解資料物件的樣貌，可以開啟 **ImageFileInfo.cs** 檔案來查看。

若要將資料範本新增至方格檢視：

1. 開啟 **MainPage.xaml**。

2. 若要顯示評分，請使用 [Windows UI 程式庫](/windows/apps/winui/) (WinUI) NuGet 套件中的 [RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)。 新增指定 WinUI 控制項命名空間的 XAML 命名空間參考。 將這放在 `Page` 標籤開頭，緊接其他 `xmlns:` 項目之後。

    **新增此 XAML**

    ```xaml
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    ```

    **在最後一個 `xmlns:` 項目之後**

    ```xaml
    <Page x:Name="page"
      x:Class="PhotoLab.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:PhotoLab"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
      mc:Ignorable="d"
      NavigationCacheMode="Enabled">
    ```

    如需 XAML 命名空間的詳細資訊，請參閱 [XAML 命名空間與命名空間對應](../../xaml-platform/xaml-namespaces-and-namespace-mapping.md)。

3. 在 [文件大綱] 中，以滑鼠右鍵按一下 [ImageGridView]  。 在捷徑功能表中，選取 [編輯其他範本]   > [編輯產生的項目 (ItemTemplate)]   > [建立空白]  。 [建立資源]  對話方塊隨即開啟。

4. 在對話方塊中，將 [名稱 (索引碼)]  值變更為 **ImageGridView_DefaultItemTemplate**，然後選取 [確定]  。

    當您選取 [確定]  時，將會發生幾件事：

    + `DataTemplate` 物件會新增至 **MainPage.xaml** 的 `Page.Resources` 區段。

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    + `DataTemplate` 控制項的 `GridView` 屬性設定為 [DataTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 資源。

       ```xaml
           <GridView x:Name="ImageGridView"
                     Margin="0,0,0,8"
                     RelativePanel.AlignLeftWithPanel="True"
                     RelativePanel.AlignRightWithPanel="True"
                     RelativePanel.Below="TitleTextBlock"
                     ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
       ```

5. 在 **ImageGridView_DefaultItemTemplate** 資源中，為根 `Grid` 指定值為 **300** 的高度和寬度，以及值為的 **8** 的邊界。 然後新增兩列，並第二列的高度設定為 **Auto**。

    **之前**

    ```xaml
    <Grid/>
    ```

    **之後**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
    </Grid>
    ```

    如需有關 `Grid` 版面配置的詳細資訊，請參閱[版面配置面板](../layout/layout-panels.md#grid)。

6. 將控制項新增至 `Grid` 版面配置。

    a. 將 [Image](/uwp/api/windows.ui.xaml.controls.image) 控制項新增至方格；根據預設，其會放在第一個方格資料列中 (第 0 列)。 這是顯示影像的位置。 但您會暫時使用應用程式的 Microsoft Store 標誌做為預留位置。

    b. 新增 `TextBlock` 控制來顯示影像的名稱、檔案類型和尺寸。 為此，您使用 `StackPanel` 控制項來排列文字區塊。 使用 `Grid.Row` 附加屬性，將最外層的 `StackPanel` 放在第二個資料列 (第 1 列) 中。

    如需有關 `StackPanel` 版面配置的詳細資訊，請參閱[版面配置面板](../layout/layout-panels.md#stackpanel)。

    c. 將 `RatingControl` 新增至外部 (垂直) `StackPanel` 控制項。 將它放置在內部 (水平) `StackPanel` 控制項之後。

    **最終範本**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Image x:Name="ItemImage"
               Source="Assets/StoreLogo.png"
               Stretch="Uniform" />

        <StackPanel Orientation="Vertical"
                    Grid.Row="1">
            <TextBlock Text="ImageTitle"
                       HorizontalAlignment="Center"
                       Style="{StaticResource SubtitleTextBlockStyle}" />
            <StackPanel Orientation="Horizontal"
                        HorizontalAlignment="Center">
                <TextBlock Text="ImageFileType"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}" />
                <TextBlock Text="ImageDimensions"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}"
                           Margin="8,0,0,0" />
            </StackPanel>

            <muxc:RatingControl Value="3" IsReadOnly="True"/>
        </StackPanel>
    </Grid>
    ```

立即執行應用程式，查看包含您剛建立之項目範本的 `GridView` 控制項。 接下來，您將變更背景色彩，並在方格項目之間新增一些空間。

![執行中的集合應用程式螢幕擷取畫面，其中顯示項目範本。](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>第 4 部分：修改項目容器樣式

項目的控制項範本包含顯示狀態的視覺效果，例如選取、指標暫留及焦點。 這些視覺效果都是在資料範本的上方或下方進行轉譯。 這裡將修改控制項範本的 `Background` 及 `Margin` 屬性，為 `GridView` 項目提供灰色背景。

若要修改項目容器：

1. 在 [文件大綱] 中，以滑鼠右鍵按一下 [ImageGridView]  。 在捷徑功能表中，選取 [編輯其他範本]   > [編輯產生的項目容器 (ItemContainerStyle)]   > [編輯複本]  。 [建立資源]  對話方塊隨即開啟。

2. 在對話方塊中，將 [名稱 (索引碼)]  值變更為 **ImageGridView_DefaultItemContainerStyle**，然後選取 [確定]  。

    預設樣式複本會新增至 XAML 的 **Page.Resources** 區段。

    ```xaml
    <Style x:Key="ImageGridView_DefaultItemContainerStyle" TargetType="GridViewItem">
        <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
        <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}"/>
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
        <Setter Property="Foreground" Value="{ThemeResource GridViewItemForeground}"/>
        <Setter Property="TabNavigation" Value="Local"/>
        <Setter Property="IsHoldingEnabled" Value="True"/>
        <Setter Property="HorizontalContentAlignment" Value="Center"/>
        <Setter Property="VerticalContentAlignment" Value="Center"/>
        <Setter Property="Margin" Value="0,0,4,4"/>
        <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
        <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
        <Setter Property="AllowDrop" Value="False"/>
        <Setter Property="UseSystemFocusVisuals" Value="{StaticResource UseSystemFocusVisuals}"/>
        <Setter Property="FocusVisualMargin" Value="-2"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="GridViewItem">
                <!-- XAML removed for clarity
                    <ListViewItemPresenter ... />
                -->   
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    ```

    `GridViewItem` 預設樣式會設定許多屬性。 不過，在您的範本副本中，您只需要保留想要修改的屬性。

    與上一個步驟相同，**GridView** 控制項的 **ItemContainerStyle** 屬性會設定為新的 **Style** 資源。

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

3. 刪除 `Background` 和 `Margin` 以外的所有 `Setter` 元素。

4. 將 `Background` 屬性的值變更為 `Gray`。

    **之前**

    ```xaml
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
    ```

    **之後**

    ```xaml
        <Setter Property="Background" Value="Gray"/>
    ```

5. 將 `Margin` 屬性的值變更為 `8`。

    **之前**

    ```xaml
        <Setter Property="Margin" Value="0,0,4,4"/>
    ```

    **之後**

    ```xaml
        <Setter Property="Margin" Value="8"/>
    ```

執行應用程式，看看目前會是什麼樣子。 調整應用程式視窗大小。 `GridView` 控制項負責重新排列影像，但在某些寬度下，應用程式視窗右側會留下許多空間。 如果影像已置中，就比較好看。 您接下來會處理這個問題。

![執行中的集合應用程式螢幕擷取畫面，其中顯示項目範本，且應用程式視窗的右邊有額外空間。](images/xaml-basics/layout-2.png)

> [!Note]
> 如果您想要實驗，請嘗試將 `Background` 和 `Margin` 屬性設定為不同值，看看會有什麼效果。

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>第 5 部分：將一些最後的調整套用至版面配置

若要在頁面上置中影像，您必須調整頁面中 `Grid` 控制項的對齊方式。 或者，您需要調整 `GridView` 中 Images 的對應嗎？ 這會有什麼影響？ 我們來看看。

如需對齊方式的詳細資訊，請參閱[對齊、邊界及邊框間距](../layout/alignment-margin-padding.md)

(您可能在此步驟，嘗試將 `Background` 的 `GridView` 屬性設定為您最愛的色彩。 這將讓您更清楚了解版面配置發生的狀況)。

若要修改影像的對齊方式：

1. 在 `GridView` 中，將 [HorizontalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) 屬性設定為 `Center`。

    **之前**

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
    ```

    **之後**

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  HorizontalAlignment="Center"/>
    ```

2. 執行應用程式，並調整視窗大小。 向下捲動以查看更多的影像。

    影像已置中，看起來比較好。 不過，捲軸是對齊 `GridView` 控制項的邊緣，而不是視窗的邊緣。 為了修正這個問題，您要在頁面上將 `GridView` 的影像置中，而不是將 `GridView` 置中。 這要多費一點事，但最後會變得較好看。

3. 移除上一個步驟中的 `HorizontalAlignment` 設定。

4. 在 [文件大綱] 中，以滑鼠右鍵按一下 [ImageGridView]  。 在捷徑功能表中，選取 [編輯其他範本]   > [編輯項目的配置 (ItemsPanel)]   > [編輯複本]  。 [建立資源]  對話方塊隨即開啟。

5. 在對話方塊中，將 [名稱 (索引碼)]  值變更為 **ImageGridView_ItemsPanelTemplate**，然後選取 [確定]  。

    預設 **ItemsPanelTemplate** 的複本會新增至 XAML 的 **Page.Resources** 區段 (像之前一樣，`GridView` 會更新以參考此資源)。

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    就像您使用了各種面板來配置應用程式的控制項一樣，`GridView` 也有內部面板管理其項目配置。 既然您可以存取此面板 ([ItemsWrapGrid](/uwp/api/windows.ui.xaml.controls.itemswrapgrid))，就可以修改屬性來變更 `GridView` 控制項內部項目的版面配置。

6. 在 `ItemsWrapGrid`中，將 `HorizontalAlignment` 屬性設定為 `Center`。

    **之前**

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    **之後**

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal"
                       HorizontalAlignment="Center"/>
    </ItemsPanelTemplate>
    ```

7. 執行應用程式，並重新調整視窗大小。 向下捲動以查看更多的影像。

![應用程式 UI 檢查點 4](images/xaml-basics/layout-3.png)

捲軸現在已對齊視窗邊緣。 做得太好了！ 您已為您的應用程式建立基本 UI。

## <a name="go-further"></a>更進一步

現在您已建立基本 UI，接著請了解其他教學課程。 這些教學課程同樣以 PhotoLab 範例為基礎。

* 在 [XAML 資料繫結教學課程](../../data-binding/xaml-basics-data-binding.md)中新增真正的影像和資料。
* 在 [XAML 調整型配置教學課程](xaml-basics-adaptive-layout.md)中讓 UI 適應不同的螢幕大小。


## <a name="get-the-final-version-of-the-photolab-sample"></a>取得最終版本 PhotoLab 範例

本教學課程不會一直建置到完整的相片編輯應用程式。 因此請務必查看[最終版本](https://github.com/Microsoft/Windows-appsample-photo-lab)來了解各項功能，例如自訂動畫和彈性配置。
