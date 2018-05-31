---
author: Jwmsft
description: 使用樹狀檢視範例程式碼建立可展開的樹狀結構。
title: 樹狀檢視
label: Tree view
template: detail.hbs
ms.author: jimwalk
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.openlocfilehash: c92d0c6517456f180dcc84b60cbc6ca3a53ea282
ms.sourcegitcommit: 346b5c9298a6e9e78acf05944bfe13624ea7062e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/05/2018
ms.locfileid: "1707143"
---
# <a name="treeview"></a>TreeView

> [!IMPORTANT]
> 本文說明尚未發佈但可能在正式發行前大幅度修改的功能。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

XAML TreeView 控制項啟用提供包含巢狀項目的展開及摺疊節點的階層式清單。 此控制項可以用來說明 UI 中的資料夾結構或巢狀關聯性。

> **重要 API**：[TreeView 類別](/uwp/api/windows.ui.xaml.controls.treeview)、[TreeViewNode 類別](/uwp/api/windows.ui.xaml.controls.treeviewnode)

TreeView API 支援下列功能：

- N 層巢狀結構
- 展開/摺疊節點
- 選取單一節點或多個節點

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

- 當項目有巢狀清單項目時，以及如果向對等項目與節點協助說明項目的階層式關係很重要時，請使用樹狀檢視。

- 如果將項目的巢狀關係以醒目提示方式顯示不是優先考量，請避免使用樹狀檢視。 對於大部分深入案例來說，適合使用一般清單檢視

## <a name="treeview-ui"></a>TreeView UI

樹狀檢視使用縮排和圖示的組合來表示資料夾/父節點和非資料夾/子節點之間的巢狀關係。 摺疊的節點使用＞形箭號指向右方，而展開的節點使用＞形箭號指向下方。

![TreeView 中的＞形箭號圖示](images/treeview_chevron.png)

您可以在樹檢視項目資料範本中包含圖示來表示節點。 如果這樣做，您應該只對代表常值資料夾 (例如磁碟上的資料夾結構) 的節點使用資料夾圖示。

![同在 TreeView 中的＞形箭號和資料夾圖示](images/treeview_chevron_folder.png)

## <a name="create-a-tree-view"></a>建立樹狀檢視

若要建立樹檢視，請使用 [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 控制項和 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) 物件階層。 您可以將一個或多個根節點新增至 TreeView 控制項的 RootNodes 集合，來建立節點階層。 然後，每個 TreeViewNode 都可以有更多新增至其 Children 集合的節點。 您可以將樹狀檢視節點巢狀化到您需要的任何深度。

以下是使用 XAML 宣告的簡單樹狀檢視範例。 您通常會在程式碼中新增節點，但我們在此顯示 XAML 階層，因為這對視覺化展示如何建立節點階層可能會有幫助。

```xaml
<TreeView>
    <TreeView.RootNodes>
        <TreeViewNode Content="Flavors" IsExpanded="True">
            <TreeViewNode.Children>
                <TreeViewNode Content="Vanilla"/>
                <TreeViewNode Content="Strawberry"/>
                <TreeViewNode Content="Chocolate"/>
            </TreeViewNode.Children>
        </TreeViewNode>
    </TreeView.RootNodes>
</TreeView>
```

在大部分情況下，樹狀檢視會顯示資料來源的資料，因此您通常在 XAML 中宣告根 TreeView 控制項，而在程式碼中新增 TreeViewNode 物件。

此樹狀檢視與先前使用 XAML 建立的樹狀檢視相同，但節點是使用程式碼所建立。

```xaml
<TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    TreeViewNode rootNode = new TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

這些 API 可用來管理樹狀檢視的資料階層。

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | 樹狀檢視可以有一或多個根節點。 將 TreeViewNode 物件新增至 RootNodes 集合以建立根節點。 根節點的 **Parent** 永遠為 **null**。 根節點的 **Depth** 為 0。 |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | 將 TreeViewNode 物件新增至父節點的 Children 集合以建立您的節點階層。 節點是其 **Children** 集合中所有節點的 **Parent**。 |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | 如果節點有具現化的子系，則為 **true**。 **false** 表示空的資料夾或一個項目。 |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | 如果您正在節點展開時填滿節點，請使用此屬性。 請參閱本文稍後的＜_當節點正在展開時填滿節點_＞。 |
| [Depth](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | 表示子節點距離根節點有多遠。 |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | 取得擁有此節點所屬 **Children** 集合的 TreeViewNode。 |

樹狀檢視使用 **HasChildren** 和 **HasUnrealizedChildren** 屬性來判斷展開/摺疊圖示是否已顯示。 如果任一屬性為 **true**，則圖示已顯示，否則未顯示。

## <a name="tree-view-node-content"></a>樹狀檢視節點內容

您可以將樹狀檢視所表示的資料項目儲在其 [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) 屬性中。

在前一個範例中，內容是簡單的字串值。 在這裡，樹狀檢視節點表示使用者的 [圖片] 資料夾，因此圖片媒體櫃 [StorageFolder](/uwp/api/windows.storage.storagefolder) 已指派給節點的 Content 屬性。

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
TreeViewNode pictureNode = new TreeViewNode();
pictureNode.Content = picturesFolder;
```

您可以提供 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) 來指定資料項目在樹狀檢視中的顯示方式。

> [!NOTE]
> 在 Windows 10 版本 1803 中，如果您的內容不是字串，就必須重新建立 TreeView 控制項的範本並指定自訂 ItemTemplate。 如需詳細資訊，請參閱本文結尾的完整範例。

## <a name="interacting-with-a-tree-view"></a>與樹狀檢視互動

您可以設定樹狀檢視，讓使用者以多種不同方式與之互動：

- 展開或摺疊節點
- 單一或多重選取項目
- 按一下以叫用項目

### <a name="expandcollapse"></a>展開/摺疊

任何有子系的樹狀檢視節點永遠都可以藉由按一下展開/摺疊字符來展開或摺疊。 您也可以透過程式設計方式展開或摺疊節點，並在節點狀態變更時做出回應。

#### <a name="expandcollapse-a-node-programmatically"></a>以程式設計方式展開/摺疊節點

有 2 種方式可以在您的程式碼中展開或摺疊樹狀檢視節點。

- [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 類別具有 [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) 和 [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand) 方法。 呼叫這些方法時，您會傳入您要展開或摺疊的 TreeViewNode。

- 每個 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) 都有 [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded) 屬性。 您可以使用此屬性來檢查節點的狀態，或加以設定來變更狀態。 您也可以在 XAML 中設定此屬性，以設定節點的初始狀態。

### <a name="fill-a-node-when-its-expanding"></a>當節點正在展開時填滿節點

您可能需要在樹狀檢視中顯示大量節點，或者您無法事先知道其中會有多少節點。 TreeView 控制項並未虛擬化，因此您可以透過在每個節點展開時填滿該節點，或在其摺疊時移除子節點這樣的方式來管理資源。

處理 [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand) 事件，並在節點正在展開時，使用 [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) 屬性將子系新增至該節點。 HasUnrealizedChildren 屬性會指出節點是否需要加以填滿，或是否已填入其 Children 集合。 請務必記住，TreeViewNode 不會設定這個值，您必須在應用程式的程式碼中管理該值。

以下是這些使用中 API 的範例。 請參閱本文結尾的完整範例程式碼，以了解相關內容，包括「FillTreeNode」的實作。

```csharp
private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

這不是必要動作，但您可能會想要另外再處理 [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) 事件，並在關閉父節點時移除子節點。 如果樹狀檢視有許多節點，或是節點資料使用大量資源，這點就會相當重要。 相對於在關閉的節點上保留子節點，您應該考量每次在開啟節點時填滿節點的效能影響。 最佳選項取決於您的應用程式。

以下是 Collapsed 事件的處理常式範例。

```csharp
private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

### <a name="invoking-an-item"></a>叫用項目

使用者可以叫用動作 (將項目當做按鈕一樣處理)，而不選取項目。 您可以處理 [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) 事件來回應此使用者互動。

> [!NOTE]
> 與具有 [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 屬性的 ListView 不同，樹狀檢視永遠會啟用對項目的叫用。 您仍然可以選擇是否要處理事件。

**[TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs) 類別**

ItemInvoked 事件引數可讓您存取叫用的項目。 [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) 屬性含有已叫用的節點。 您可以將其轉換為 TreeViewNode，並從 TreeViewNode.Content 屬性取得資料項目。

以下是 ItemInvoked 事件處理常式的範例。 資料項目是 [IStorageItem](/uwp/api/windows.storage.istorageitem)，而此範例只是顯示一些有關檔案和樹狀的資訊。 此外，如果節點是資料夾節點，還會同時展開或摺疊節點。 如果不是，節點只會在按一下＞形箭號時展開或摺疊。

```csharp
private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

### <a name="item-selection"></a>項目選取

TreeView 控制項同時支援單一選取和多重選取。 預設會關閉節點選取，但您可以設定 [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) 屬性來允許選取節點。 [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) 值會是 **None**、**Single** 和 **Multiple**。

啟用選項時，每個樹狀檢視節點旁邊會顯示核取方塊，並反白顯示選取的項目。 使用者可以使用核取方塊來選取或取消選取項目。按一下項目仍會導致叫用該項目。

選取的節點會新增至樹狀檢視的 [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) 集合。 您可以呼叫 [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) 方法來選取樹狀檢視中的所有節點。

> [!NOTE]
> 如果您呼叫 **SelectAll**，則不論 SelectionMode 為何，都會選取所有已具現化的節點。 為了提供一致的使用者體驗，如果 SelectionMode 為 **Multiple**，您應該只呼叫 SelectAll。

#### <a name="selection-and-realizedunrealized-nodes"></a>選取項目以及具現化/未具現化節點

如果樹狀檢視有未具現化的節點，這些節點不會納入選取項目。 以下是一些需要在選取項目與未具現化節點方面注意的事項。

- 如果使用者選取父節點，也會選取該父節點下所有的具現化子節點。 同樣的，如果選取所有的子節點，父節點也會變成已選取狀態。
- SelectAll 方法只會將具現化節點新增至 SelectedNodes 集合。
- 如果選取包含未具現化子節點的父節點，則會在子節點具現化時選取這些子節點。

## <a name="code-examples"></a>程式碼範例

### <a name="tree-view-with-selection-enabled"></a>已啟用選取的樹狀檢視

此範例顯示如何使用 XAML 建立簡單樹狀檢視結構。 樹狀檢視依類型排列，顯示可供使用者選擇的冰淇淋口味及配料。 已啟用多重選取，當使用者按一下按鈕時，主應用程式 UI 會顯示 SelectedItems。

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView x:Name="DessertTree" SelectionMode="Multiple">
                <TreeView.RootNodes>
                    <TreeViewNode Content="Flavors" IsExpanded="True">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Vanilla"/>
                            <TreeViewNode Content="Strawberry"/>
                            <TreeViewNode Content="Chocolate"/>
                        </TreeViewNode.Children>
                    </TreeViewNode>

                    <TreeViewNode Content="Toppings">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Candy">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Chocolate"/>
                                    <TreeViewNode Content="Mint"/>
                                    <TreeViewNode Content="Sprinkles"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Fruits">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Mango"/>
                                    <TreeViewNode Content="Peach"/>
                                    <TreeViewNode Content="Kiwi"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Berries">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Strawberry"/>
                                    <TreeViewNode Content="Blueberry"/>
                                    <TreeViewNode Content="Blackberry"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                        </TreeViewNode.Children>
                    </TreeViewNode>
                </TreeView.RootNodes>
            </TreeView>
        </SplitView.Pane>

        <StackPanel Grid.Column="1" Margin="12,0">
            <Button Content="Select all" Click="SelectAllButton_Click"/>
            <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
            <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
            <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="ToppingList"/>
        </StackPanel>
    </SplitView>
</Grid>
```

```csharp
private void OrderButton_Click(object sender, RoutedEventArgs e)
{
    FlavorList.Text = string.Empty;
    ToppingList.Text = string.Empty;

    foreach (TreeViewNode node in DessertTree.SelectedNodes)
    {
        if (node.Parent.Content?.ToString() == "Flavors")
        {
            FlavorList.Text += node.Content + "; ";
        }
        else if (node.HasChildren == false)
        {
            ToppingList.Text += node.Content + "; ";
        }
    }
}

private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (DessertTree.SelectionMode == TreeViewSelectionMode.Multiple)
    {
        DessertTree.SelectAll();
    }
}
```

### <a name="pictures-and-music-library-tree-view"></a>圖片及音樂媒體櫃樹狀檢視

此範例示範如何建立顯示使用者 [圖片] 及 [音樂] 媒體櫃內容和結構的樹狀檢視。 無法事先知道項目的數量，因此會在每個節點展開時填滿該節點，並在其摺疊時加以清空。

自訂項目範本會用來顯示資料項目，而這些項目的類型為 [IStorageItem](/uwp/api/windows.storage.istorageitem)。

> [!IMPORTANT]
> 此範例中的程式碼需要 picturesLibrary 和 musicLibrary 功能。 如需檔案存取的詳細資訊，請參閱[檔案存取權限](../../files/file-access-permissions.md)、[列舉和查詢檔案及資料夾](../../files/quickstart-listing-files-and-folders.md)以及[音樂、圖片及影片媒體櫃中的檔案和資料夾](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。

```xaml
<Page
    x:Class="TreeViewApp1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewApp1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock
                    Text="{Binding Content.DisplayName}"
                    HorizontalAlignment="Left"
                    VerticalAlignment="Center"
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <Style TargetType="TreeView">
            <Setter Property="IsTabStop" Value="False" />
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="TreeView">
                        <TreeViewList x:Name="ListControl"
                                      ItemTemplate="{StaticResource TreeViewItemDataTemplate}"
                                      ItemContainerStyle="{StaticResource TreeViewItemStyle}"
                                      CanDragItems="True"
                                      AllowDrop="True"
                                      CanReorderItems="True">
                            <TreeViewList.ItemContainerTransitions>
                                <TransitionCollection>
                                    <ContentThemeTransition />
                                    <ReorderThemeTransition />
                                    <EntranceThemeTransition IsStaggeringEnabled="False" />
                                </TransitionCollection>
                            </TreeViewList.ItemContainerTransitions>
                        </TreeViewList>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();
    InitializeTreeView();
}

private void InitializeTreeView()
{
    // A TreeView can have more than 1 root node. The Pictures library
    // and the Music library will each be a root node in the tree.
    // Get Pictures library.
    StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
    TreeViewNode pictureNode = new TreeViewNode();
    pictureNode.Content = picturesFolder;
    pictureNode.IsExpanded = true;
    pictureNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(pictureNode);
    FillTreeNode(pictureNode);

    // Get Music library.
    StorageFolder musicFolder = KnownFolders.MusicLibrary;
    TreeViewNode musicNode = new TreeViewNode();
    musicNode.Content = musicFolder;
    musicNode.IsExpanded = true;
    musicNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(musicNode);
    FillTreeNode(musicNode);
}

private async void FillTreeNode(TreeViewNode node)
{
    // Get the contents of the folder represented by the current tree node.
    // Add each item as a new child node of the node that's being expanded.

    // Only process the node if it's a folder and has unrealized children.
    StorageFolder folder = null;
    if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
    {
        folder = node.Content as StorageFolder;
    }
    else
    {
        // The node isn't a folder, or it's already been filled.
        return;
    }

    IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

    if (itemsList.Count == 0)
    {
        // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
        // that the chevron appears, but don't try to process children that aren't there.
        return;
    }

    foreach (var item in itemsList)
    {
        var newNode = new TreeViewNode();
        newNode.Content = item;

        if (item is StorageFolder)
        {
            // If the item is a folder, set HasUnrealizedChildren to true. 
            // This makes the collapsed chevron show up.
            newNode.HasUnrealizedChildren = true;
        }
        else
        {
            // Item is StorageFile. No processing needed for this scenario.
        }
        node.Children.Add(newNode);
    }
    // Children were just added to this node, so set HasUnrealizedChildren to false.
    node.HasUnrealizedChildren = false;
}

private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}

private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}

private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}

private void RefreshButton_Click(object sender, RoutedEventArgs e)
{
    sampleTreeView.RootNodes.Clear();
    InitializeTreeView();
}
```

## <a name="related-articles"></a>相關文章

- [TreeView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView 和 GridView](listview-and-gridview.md)