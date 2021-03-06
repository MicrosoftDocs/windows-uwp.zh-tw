---
description: 將預設 InkToolbar 新增至 Windows 應用程式筆跡應用程式、將自訂畫筆按鈕新增至 InkToolbar，然後將自訂畫筆按鈕系結至自訂畫筆定義。
title: 將 InkToolbar 新增至 Windows 應用程式
label: Add an InkToolbar to a Windows app
template: detail.hbs
keywords: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, InkToolbar, Universal Windows Platform, UWP, user interaction, input, Windows 筆跡, 通用 Windows 平台, 使用者互動, 輸入
ms.date: 09/24/2020
ms.topic: article
ms.assetid: d888f75f-c2a0-4134-81db-907b5e24fcc5
ms.localizationpriority: medium
ms.openlocfilehash: 78585f9734131531db5cfa429770ed8351459d8f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030201"
---
# <a name="add-an-inktoolbar-to-a-windows-app"></a>將 InkToolbar 新增至 Windows 應用程式



有兩個不同的控制項可加速 Windows 應用程式中的筆跡： [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) 和 [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar)。

[**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) 控制項提供基本的 Windows 筆跡功能。 使用此控制項將手寫筆輸入轉譯為筆墨筆劃 (使用色彩與粗細的預設設定) 或擦去筆劃。

> 如需 InkCanvas 的執行詳細資料，請參閱 [Windows 應用程式中的畫筆和手寫筆互動](pen-and-stylus-interactions.md)。

做為完全透明的重疊，InkCanvas 不會提供任何用於設定筆墨筆劃屬性的內建 UI。 如果您想要變更預設的手寫筆跡體驗、讓使用者設定筆墨筆劃的屬性，以及支援其他自訂的手寫筆跡功能，您有兩個選項︰

- 在程式碼後置中，使用繫結至 InkCanvas 的基礎 [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) 物件。

  InkPresenter API 支援大量的手寫筆跡體驗自訂項目。 如需詳細資訊，請參閱 [Windows 應用程式中的畫筆和手寫筆互動](pen-and-stylus-interactions.md)。

- 將 [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) 繫結至 InkCanvas。 根據預設，InkToolbar 會提供可自訂並可擴充的按鈕集合，以用於啟用與筆墨相關的功能 (例如筆觸大小、筆跡色彩及筆尖形狀)。

  我們會在本主題中討論 InkToolbar。

> **重要 api** ： [**InkCanvas 類別**](/uwp/api/windows.ui.xaml.controls.inkcanvas)、 [**InkToolbar 類別**](/uwp/api/windows.ui.xaml.controls.inktoolbar)、 [**InkPresenter 類別、類別**](/uwp/api/windows.ui.input.inking.inkpresenter)、 [**輸入**](/uwp/api/Windows.UI.Input.Inking)筆墨

## <a name="default-inktoolbar"></a>預設 InkToolbar

根據預設， [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) 包含可用於繪圖、清除、反白顯示，以及顯示樣板 (尺規或量角器) 的按鈕。 根據功能而定，其他設定和命令 (例如筆跡色彩、筆劃粗細、清除所有筆跡) 將會在飛出視窗中提供。

![InkToolbar](./images/ink/ink-tools-invoked-toolbar-small.png)  
*預設 Windows Ink 工具列*

若要新增預設的 [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) 到手寫筆跡應用程式，只要將它放在與 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) 同一個頁面上並關聯兩個控制項即可。

1. 在 MainPage.xaml 中，針對手寫筆跡表面宣告容器物件 (如這個範例中，我們使用 [格線] 控制項)。
2. 將 InkCanvas 物件宣告為容器的子系。 (InkCanvas 大小是繼承自容器。)
3. 宣告 InkToolbar 並使用 TargetInkCanvas 屬性將它繫結至 InkCanvas。

> [!NOTE]
> 請確定 InkToolbar 是在 InkCanvas 之後宣告。 如果不是，InkCanvas 重疊會將 InkToolbar 轉譯為無法存取。

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## <a name="basic-customization"></a>基本自訂項目

在本節中，我們會討論一些基本的 Windows Ink 工具列自訂案例。

### <a name="specify-location-and-orientation"></a>指定位置和方向

當您將筆跡工具列新增到您的應用程式時，您可以接受工具列的預設位置和方向，或是根據您應用程式或使用者的需要設定他們。

**XAML**

透過其 [VerticalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.VerticalAlignment)、[HorizontalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment) 和 [Orientation](/uwp/api/windows.ui.xaml.controls.inktoolbar?branch=rs3.Orientation) 屬性明確指定工具列的位置和方向。

| 預設 | 明確 |
| --- | --- |
| ![預設筆跡工具列位置和方向](./images/ink/location-default-small.png) | ![明確筆跡工具列位置和方向](./images/ink/location-explicit-small.png) |
| *Windows Ink 工具列預設位置和方向* | *Windows Ink 工具列明確位置和方向* |

以下是在 XAML 中明確設定筆跡工具列位置和方向的程式碼。
```xaml
<InkToolbar x:Name="inkToolbar" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Right" 
    Orientation="Vertical" 
    TargetInkCanvas="{x:Bind inkCanvas}" />
```

**根據使用者喜好設定或裝置狀態初始化**

在某些案例中，您可能會想要根據使用者喜好設定或裝置狀態設定筆跡工具列的位置和方向。 下列範例示範如何根據透過 **\[設定\] > \[裝置\] > \[手寫筆與 Windows Ink\] > \[手寫筆\] > \[選擇您用來書寫的手\]** 指定的左手或右手書寫喜好設定，來設定筆跡工具列的位置和方向。

![主控手設定](./images/ink/location-handedness-setting.png)  
*主控手設定*

您可以透過 Windows.UI.ViewManagement 的 HandPreference 屬性查詢此設定，然後根據傳回的值設定 [HorizontalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment)。 在此範例中，我們會為慣用左手的人將工具列設在應用程式的左側，為慣用右手的人將工具列設在右側。

**從 [筆墨工具列位置和方向範例下載此範例 (基本)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)**

```csharp
public MainPage()
{
    this.InitializeComponent();

    Windows.UI.ViewManagement.UISettings settings = 
        new Windows.UI.ViewManagement.UISettings();
    HorizontalAlignment alignment = 
        (settings.HandPreference == 
            Windows.UI.ViewManagement.HandPreference.LeftHanded) ? 
            HorizontalAlignment.Left : HorizontalAlignment.Right;
    inkToolbar.HorizontalAlignment = alignment;
}
```

**根據使用者或裝置狀態動態調整**

您也可以使用繫結來追蹤根據使用者喜好設定、裝置設定或裝置狀態變更而產生的 UI 更新。 在以下的範例中，我們會展開先前的範例，並示範如何使用繫結、一個 ViewMOdel 物件和 [NotifyPropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged) 介面根據裝置的方向動態調整筆跡工具列的位置。 

**從 [筆墨工具列位置和方向範例下載此範例 (動態)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)**

1. 首先，讓我們先新增 ViewModel。
    1. 將新資料夾新增到您的專案，然後命名為 **ViewModels** 。
    1. 在 ViewModels 資料夾中新增新的類別 (在此範例中，我們命名為 **InkToolbarSnippetHostViewModel.cs** )。
        > [!NOTE] 
        > 我們使用[單一模式](/previous-versions/msp-n-p/ff650849(v=pandp.10)) (英文)，因為我們在應用程式的使用期間只需要一個此類型的物件

    1. 將 `using System.ComponentModel` 新增到檔案。
    1. 新增名為 **instance** 的靜態成員變數，以及名為 **Instance** 的靜態唯讀屬性。 將建構函式設為私密 (private)，確保此類別只能透過 Instance 屬性存取。   
        > [!NOTE] 
        > 此類型繼承 [INotifyPropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged) 介面，用於通知用戶端 (通常是繫結用戶端) 屬性已變更。 我們會使用這個處理裝置方向的變更 (我們會展開此程式碼並在之後的步驟中解釋)。  

        ```csharp
        using System.ComponentModel;

        namespace locationandorientation.ViewModels
        {
            public class InkToolbarSnippetHostViewModel : INotifyPropertyChanged
            {
                private static InkToolbarSnippetHostViewModel instance;
                
                public static InkToolbarSnippetHostViewModel Instance
                {
                    get
                    {
                        if (null == instance)
                        {
                            instance = new InkToolbarSnippetHostViewModel();
                        }
                        return instance;
                    }
                }
            }

            private InkToolbarSnippetHostViewModel() { }
        }
        ```

    1. 將兩個 bool 屬性新增到 InkToolbarSnippetHostViewModel 類別： **LeftHandedLayout** (與先前的僅 XAML 範例中具備相同功能) 和 **PortraitLayout** (裝置的方向)。
        >[!NOTE] 
        > PortraitLayout 可進行設定，並包含 [PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件的定義。

        ```csharp
        public bool LeftHandedLayout
        {
            get
            {
                bool leftHandedLayout = false;
                Windows.UI.ViewManagement.UISettings settings =
                    new Windows.UI.ViewManagement.UISettings();
                leftHandedLayout = (settings.HandPreference ==
                    Windows.UI.ViewManagement.HandPreference.LeftHanded);
                return leftHandedLayout;
            }
        }

        public bool portraitLayout = false;
        public bool PortraitLayout
        {
            get
            {
                Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                    Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
                portraitLayout = 
                    (winOrientation == 
                        Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
                return portraitLayout;
            }
            set
            {
                if (value.Equals(portraitLayout)) return;
                portraitLayout = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
            }
        }
        ```

1. 現在，讓我們為專案新增幾個轉換器類別。 每個類別都包含一個 Convert 物件，該物件會傳回對齊值 ([HorizontalAlignment](/uwp/api/windows.ui.xaml.horizontalalignment) 或 [VerticalAlignment](/uwp/api/windows.ui.xaml.verticalalignment))。
    1. 將新資料夾新增到您的專案，然後命名為 **Converters** 。
    1. 將兩個新類別新增到 Converters 資料夾 (在此範例中，我們命名為 **HorizontalAlignmentFromHandednessConverter.cs** 和 **VerticalAlignmentFromAppViewConverter.cs** )。
    1. 為每個檔案新增 `using Windows.UI.Xaml` 和 `using Windows.UI.Xaml.Data` 命名空間。
    1. 將每個類別都變更為 `public`，指定其實作 [IValueConverter](/uwp/api/windows.ui.xaml.data.ivalueconverter) 介面。
    1. 為每個檔案新增 [Convert](/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) 和 [ConvertBack](/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) 方法，如下所示 (我們先不實作 ConvertBack 方法)。
        - HorizontalAlignmentFromHandednessConverter 會為慣用右手的使用者將筆跡工具列的位置設在應用程式的右側，並為慣用左手的使用者設在應用程式的左側。
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class HorizontalAlignmentFromHandednessConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool leftHanded = (bool)value;
                    HorizontalAlignment alignment = HorizontalAlignment.Right;
                    if (leftHanded)
                    {
                        alignment = HorizontalAlignment.Left;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

        - VerticalAlignmentFromAppViewConverter 會為直式方向將筆跡工具列設在應用程式的中央，並為橫向設在應用程式的頂部 (雖然我們意圖改善可用性，但這僅只是為了展示用途的任意選擇)。
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class VerticalAlignmentFromAppViewConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool portraitOrientation = (bool)value;
                    VerticalAlignment alignment = VerticalAlignment.Top;
                    if (portraitOrientation)
                    {
                        alignment = VerticalAlignment.Center;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1. 現在，開啟 MainPage.xaml.cs 檔案。
    1. 新增 `using using locationandorientation.ViewModels` 至命名空間清單，以將我們的 ViewModel 相關聯。
    1. 新增 `using Windows.UI.ViewManagement` 至命名空間清單，以允許接聽裝置方向的變更。
    1. 新增 [WindowSizeChangedEventHandler](/uwp/api/windows.ui.xaml.windowsizechangedeventhandler) 程式碼。
    1. 將視圖的 [DataCoNtext](/uwp/api/Windows.UI.Xaml.FrameworkElement.DataContext) 設定為 InkToolbarSnippetHostViewModel 類別的單一實例。 
    ```csharp
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;

    using locationandorientation.ViewModels;
    using Windows.UI.ViewManagement;

    namespace locationandorientation
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();

                Window.Current.SizeChanged += (sender, args) =>
                {
                    ApplicationView currentView = ApplicationView.GetForCurrentView();

                    if (currentView.Orientation == ApplicationViewOrientation.Landscape)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = false;
                    }
                    else if (currentView.Orientation == ApplicationViewOrientation.Portrait)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = true;
                    }
                };

                DataContext = InkToolbarSnippetHostViewModel.Instance;
            }
        }
    }
    ```

1. 接著，開啟 MainPage .xaml 檔案。
    1. 加入至專案，以系結 `xmlns:converters="using:locationandorientation.Converters"` `Page` 至我們的轉換器。
        ```xaml
        <Page
        x:Class="locationandorientation.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:locationandorientation"
        xmlns:converters="using:locationandorientation.Converters"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        ```

    1. 加入專案 `PageResources` ，並指定對我們的轉換器的參考。
        ```xaml
        <Page.Resources>
            <converters:HorizontalAlignmentFromHandednessConverter x:Key="HorizontalAlignmentConverter"/>
            <converters:VerticalAlignmentFromAppViewConverter x:Key="VerticalAlignmentConverter"/>
        </Page.Resources>
        ```

    1. 加入 InkCanvas 和 InkToolbar 元素，並系結 InkToolbar 的 VerticalAlignment 和 >horizontalalignment 屬性。
        ```xaml
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="{Binding PortraitLayout, Converter={StaticResource VerticalAlignmentConverter} }" 
                    HorizontalAlignment="{Binding LeftHandedLayout, Converter={StaticResource HorizontalAlignmentConverter} }" 
                    Orientation="Vertical" 
                    TargetInkCanvas="{x:Bind inkCanvas}" />
        ```

1. 返回 InkToolbarSnippetHostViewModel.cs 檔案，將我們的 `PortraitLayout` 和 `LeftHandedLayout` bool 屬性加入至 `InkToolbarSnippetHostViewModel` 類別，並 `PortraitLayout` 在屬性值變更時支援重新系結。 
    ```csharp
    public bool LeftHandedLayout
    {
        get
        {
            bool leftHandedLayout = false;
            Windows.UI.ViewManagement.UISettings settings =
                new Windows.UI.ViewManagement.UISettings();
            leftHandedLayout = (settings.HandPreference ==
                Windows.UI.ViewManagement.HandPreference.LeftHanded);
            return leftHandedLayout;
        }
    }

    public bool portraitLayout = false;
    public bool PortraitLayout
    {
        get
        {
            Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            portraitLayout = 
                (winOrientation == 
                    Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
            return portraitLayout;
        }
        set
        {
            if (value.Equals(portraitLayout)) return;
            portraitLayout = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
        }
    }

    #region INotifyPropertyChanged Members

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }

    #endregion
    ```

您現在應該有一個筆跡應用程式，可適應使用者的主要偏好喜好設定，並動態地回應使用者裝置的方向。

### <a name="specify-the-selected-button"></a>指定選取的按鈕  
![在初始化時選取的鉛筆按鈕](./images/ink/ink-tools-default-toolbar.png)  
*包含在初始化時所選取鉛筆按鈕的 Windows Ink 工具列*

根據預設，當您的應用程式啟動時或是工具列初始化時，會選取第一個 (或最左邊) 按鈕。 在預設 Windows Ink 工具列中，這是鋼珠筆按鈕。

因為架構定義了內建按鈕的順序，根據預設，第一個按鈕可能不是您想要啟用的畫筆或工具。

您可以覆寫這個預設行為，並在工具列上指定選取的按鈕。

這個範例中，我們透過選取的鉛筆按鈕和啟用的鉛筆 (而不是鋼珠筆) 將預設工具列初始化。

1. 針對上一個範例的 InkCanvas 和 InkToolbar 使用 XAML 宣告。
2. 在程式碼後置中，針對 [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar) 物件的 [Loaded](/uwp/api/windows.ui.xaml.frameworkelement.loaded) 事件設定處理常式。

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. 在 [Loaded](/uwp/api/windows.ui.xaml.frameworkelement.loaded) 事件的處理常式中：
    1. 取得內建 [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) 的參考。

    在 [GetToolButton](/uwp/api/windows.ui.xaml.controls.inktoolbar.gettoolbutton) 方法中傳遞 [InkToolbarTool.Pencil](/uwp/api/windows.ui.xaml.controls.inktoolbartool) 物件會傳回 [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) 的 [InkToolbarToolButton](/uwp/api/windows.ui.xaml.controls.inktoolbartoolbutton) 物件。

    2. 將 [ActiveTool](/uwp/api/windows.ui.xaml.controls.inktoolbar.activetool) 設為在上一個步驟中傳回的物件。

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### <a name="specify-the-built-in-buttons"></a>指定內建的按鈕

![初始化時包含的特定按鈕](./images/ink/ink-tools-specific.png)  
*初始化時包含的特定按鈕*

如前所述，Windows Ink 工具列會包含一組預設的內建按鈕。 這些按鈕會以下列順序顯示 (由左至右)：

- [InkToolbarBallpointPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton)
- [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton)
- [InkToolbarHighlighterButton](/uwp/api/windows.ui.xaml.controls.inktoolbarhighlighterbutton)
- [InkToolbarEraserButton](/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton)
- [InkToolbarRulerButton](/uwp/api/windows.ui.xaml.controls.inktoolbarrulerbutton)

這個範例中，我們僅會初始化內建鋼珠筆、鉛筆以及橡皮擦按鈕。

您可以使用 XAML 或程式碼後置這麼做。

**XAML**

修改第一個範例中 InkCanvas 和 InkToolbar 的 XAML 宣告。
- 新增 [InitialControls](/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols) 屬性，並將其值設定為 "[None](/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols)"。 這會清除預設的內建按鈕集合。
- 新增您的應用程式所需的特定 InkToolbar 按鈕。 現在，我們僅新增 [InkToolbarBallpointPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton)、[InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) 和 [InkToolbarEraserButton](/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton)。
> [!NOTE]
> 按鈕會依照架構所定義的順序 (而非此處指定的順序) 新增至工具列。

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**程式碼後置**
1. 針對第一個範例的 InkCanvas 和 InkToolbar 使用 XAML 宣告。

  ```xaml
  <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
          <RowDefinition Height="Auto"/>
          <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
          <TextBlock x:Name="Header"
                     Text="Basic ink sample"
                     Style="{ThemeResource HeaderTextBlockStyle}"
                     Margin="10,0,0,0" />
      </StackPanel>
      <Grid Grid.Row="1">
          <Image Source="Assets\StoreLogo.png" />
          <InkCanvas x:Name="inkCanvas" />
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. 在程式碼後置中，針對 [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar) 物件的 [Loading](/uwp/api/windows.ui.xaml.frameworkelement.loading) 事件設定處理常式。

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. 將 [InitialControls](/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols) 設定為 "[None](/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols)"。
4. 建立應用程式所需按鈕的物件參考。 現在，我們僅新增 [InkToolbarBallpointPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton)、[InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) 和 [InkToolbarEraserButton](/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton)。
  > [!NOTE]
  > 按鈕會依照架構所定義的順序 (而非此處指定的順序) 新增至工具列。

5. 將按鈕[新增](/uwp/api/windows.ui.xaml.dependencyobjectcollection.add)到 InkToolbar。

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## <a name="custom-buttons-and-inking-features"></a>自訂按鈕和手寫筆跡功能

您可以自訂和延伸透過 InkToolbar 所提供按鈕 (以及相關聯的手寫筆跡功能) 的集合。

InkToolbar 包含兩個不同群組的按鈕類型︰

1. 包含內建繪圖、清除以及醒目提示按鈕的「工具」按鈕群組。 在此處新增自訂的畫筆與工具。
> **Note** &nbsp; 注意 &nbsp;特徵選取是互斥的。

2. 包含內建尺規按鈕的「切換」按鈕群組。 在此處新增自訂的切換。
> **Note** &nbsp; 注意 &nbsp;功能不會互斥，而且可以與其他使用中的工具同時使用。

根據您的應用程式和所需的手寫筆跡功能，您可以將下列任何按鈕 (繫結到您自訂的筆跡功能) 新增到 InkToolbar：

- 自訂畫筆 – 適用於由主控應用程式定義的筆跡調色盤和筆尖屬性 (例如圖形、旋轉和大小) 的畫筆。
- 自訂工具 – 由主控應用程式定義的非畫筆工具。
- 自訂切換 – 將應用程式定義功能的狀態設定為開啟或關閉。 開啟時，功能會與使用中的工具搭配使用。

> **Note** &nbsp; 注意 &nbsp;您無法變更內建按鈕的顯示順序。 預設的顯示順序是：鋼珠筆、鉛筆、螢光筆、橡皮擦和尺規。 自訂畫筆會附加到最後一個預設畫筆，自訂工具按鈕會新增到最後一個畫筆按鈕與橡皮擦按鈕之間，而自訂切換按鈕會新增到尺規按鈕之後。 (自訂按鈕會以指定的順序新增。)

### <a name="custom-pen"></a>自訂畫筆

您可以在您定義筆跡調色盤和畫筆祕訣屬性 (例如形狀、旋轉和大小) 的位置，建立自訂的畫筆 (透過自訂的畫筆按鈕啟用)。

![自訂書法筆按鈕](./images/ink/ink-tools-custompen.png)  
*自訂書法筆按鈕*

這個範例中，我們會定義具有寬筆尖的自訂畫筆，支援基本的書法筆墨筆劃。 我們也會自訂按鈕飛出視窗上所顯示調色盤中的筆刷集合。

**程式碼後置**

首先，我們換定義我們的自訂畫筆並在程式碼後置中指定繪圖屬性。 稍後我們會從 XAML 參考此自訂畫筆。

1. 在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [加入] &gt; [新增項目]。
2. 在 [Visual C#] -&gt; [程式碼] 底下，新增新的類別檔案，並命名為 CalligraphicPen.cs。
3. 在 Calligraphic.cs 中，使用下列項目取代預設 using 區塊：
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. 指定 CalligraphicPen 類別是衍生自 [InkToolbarCustomPen](/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen)。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. 覆寫 [CreateInkDrawingAttributesCore](/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore) 以指定您自己的筆刷和筆觸大小。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. 建立 [InkDrawingAttributes](/uwp/api/windows.ui.input.inking.inkdrawingattributes) 物件，並設定[筆尖形狀](/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentip)、[筆尖旋轉](/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentiptransform)、[筆觸大小](/uwp/api/windows.ui.input.inking.inkdrawingattributes.size)和[筆跡色彩](/uwp/api/windows.ui.input.inking.inkdrawingattributes.color)。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

接下來，我們會在 MainPage.xaml 中新增對自訂畫筆的必要參考。

1. 我們會宣告一個本機頁面資源字典，它會建立對 CalligraphicPen.cs 中所定義自訂畫筆 (`CalligraphicPen`) 的參考，以及自訂畫筆 (`CalligraphicPenPalette`) 支援的[筆刷集合](/uwp/api/Windows.UI.Xaml.Media.BrushCollection)。
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. 然後，我們會使用子系 [InkToolbarCustomPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarcustompenbutton) 元素新增 InkToolbar。

  自訂畫筆按鈕包含兩個在頁面資源中宣告的靜態資源參考︰`CalligraphicPen` 和 `CalligraphicPenPalette`。

  我們也會指定筆觸大小滑桿的範圍 ([MinStrokeWidth](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth)、[MaxStrokeWidth](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth) 和 [SelectedStrokeWidth](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty))、選取的筆刷 ([SelectedBrushIndex](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex))，以及自訂畫筆按鈕的圖示 ([SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon))。
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

### <a name="custom-toggle"></a>自訂切換

您可以建立自訂的切換開關 (透過自訂的切換按鈕啟用)，將應用程式定義功能的狀態設定為開啟或關閉。 開啟時，功能會與使用中的工具搭配使用。

在此範例中，我們會定義自訂的切換按鈕，利用觸控輸入使用手寫筆跡 (觸控筆跡預設未啟用)。

> [!NOTE]  
> 如果您需要支援使用觸控的手寫筆跡，我們建議您使用此範例中指定的圖示與工具提示，使用 CustomToggleButton 加以啟用。

觸控輸入通常是用來直接操作物件或應用程式 UI。 為了示範觸控筆跡啟用時的行為差異，我們在 ScrollViewer 容器內放置 InkCanvas，並設定 ScrollViewer 的尺寸小於 InkCanvas。 

當應用程式啟動時，只支援畫筆筆跡，觸控則用來移動瀏覽或縮放筆跡表面。 觸控筆跡啟用時，就無法透過觸控輸入移動瀏覽或縮放筆跡表面。

> [!NOTE]
> 請參閱 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)和 [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) UX 指導方針的筆墨 [控制項](../controls-and-patterns/inking-controls.md)。 下列是與此範例相關的建議︰
> - [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar)和筆跡一般都是透過使用中的畫筆獲得最佳體驗。 不過，如果您的應用程式要求，也可支援使用滑鼠與觸控的手寫筆跡。 
> - 如果支援使用觸控輸入的手寫筆跡，建議您針對切換按鈕 (包含「觸控書寫」工具提示) 使用 "Segoe MLD2 Assets" 字型的"ED5F" 圖示。 

**XAML**

1. 首先，我們宣告 [**InkToolbarCustomToggleButton**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton) 項目 (toggleButton)，包含指定事件處理常式 (Toggle_Custom) 的 Click 事件接聽程式。

```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel Grid.Row="0" 
                x:Name="HeaderPanel" 
                Orientation="Horizontal">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10" />
    </StackPanel>

    <ScrollViewer Grid.Row="1" 
                  HorizontalScrollBarVisibility="Auto" 
                  VerticalScrollBarVisibility="Auto">
        
        <Grid HorizontalAlignment="Left" VerticalAlignment="Top">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            
            <InkToolbar Grid.Row="0" 
                        Margin="10"
                        x:Name="inkToolbar" 
                        VerticalAlignment="Top"
                        TargetInkCanvas="{x:Bind inkCanvas}">
                <InkToolbarCustomToggleButton 
                x:Name="toggleButton" 
                Click="CustomToggle_Click" 
                ToolTipService.ToolTip="Touch Writing">
                    <SymbolIcon Symbol="{x:Bind TouchWritingIcon}"/>
                </InkToolbarCustomToggleButton>
            </InkToolbar>
            
            <ScrollViewer Grid.Row="1" 
                          Height="500"
                          Width="500"
                          x:Name="scrollViewer" 
                          ZoomMode="Enabled" 
                          MinZoomFactor=".1" 
                          VerticalScrollMode="Enabled" 
                          VerticalScrollBarVisibility="Auto" 
                          HorizontalScrollMode="Enabled" 
                          HorizontalScrollBarVisibility="Auto">
                
                <Grid x:Name="outputGrid" 
                      Height="1000"
                      Width="1000"
                      Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}">
                    <InkCanvas x:Name="inkCanvas"/>
                </Grid>
                
            </ScrollViewer>
        </Grid>
    </ScrollViewer>
</Grid>
```

**程式碼後置**

2. 在先前的程式碼片段中，我們為觸控筆跡 (toggleButton) 在自訂的切換按鈕上宣告了 Click 事件接聽程式和處理常式 (Toggle_Custom)。 這個處理常式只會透過 InkPresenter 的 InputDeviceTypes 屬性切換 CoreInputDeviceTypes.Touch 的支援。

   此外，我們還使用 SymbolIcon 元素指定按鈕的圖示，並使用 {x:Bind} 標記延伸將它繫結到程式碼後置檔案 (TouchWritingIcon) 中定義的欄位。

   下列程式碼片段包含 Click 事件處理常式和 TouchWritingIcon 的定義。

```csharp 
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomToggle : Page
    {
        Symbol TouchWritingIcon = (Symbol)0xED5F;

        public MainPage_AddCustomToggle()
        {
            this.InitializeComponent();
        }

        // Handler for the custom toggle button that enables touch inking.
        private void CustomToggle_Click(object sender, RoutedEventArgs e)
        {
            if (toggleButton.IsChecked == true)
            {
                inkCanvas.InkPresenter.InputDeviceTypes |= CoreInputDeviceTypes.Touch;
            }
            else
            {
                inkCanvas.InkPresenter.InputDeviceTypes &= ~CoreInputDeviceTypes.Touch;
            }
        }
    }
}
```

### <a name="custom-tool"></a>自訂工具

您可以建立自訂工具按鈕來叫用您的應用程式所定義的非手寫筆工具。

根據預設， [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) 會將所有輸入處理為筆墨筆劃或清除筆劃。 這包括透過次要硬體能供性所修改的輸入，例如畫筆筆身按鈕、滑鼠右鍵按鈕或類似按鈕。 不過， [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) 可以設定為不處理特定的輸入，接著傳遞至您的應用程式進行自訂的處理。

在此範例中，我們定義自訂的工具按鈕，選取時，讓後續的筆觸進行處理，並轉譯為選取套索 (虛線) 而不是筆跡。 選取區域界限內所有的筆墨筆劃都設定為 [**Selected**](/uwp/api/windows.ui.input.inking.inkstroke.selected)。

> [!NOTE]
> 請參閱＜筆跡控制項＞了解 InkCanvas 與 InkToolbar 兩者的 UX 指導方針。 下列是與此範例相關的建議︰
> - 如果提供筆劃選取項目，建議您針對工具按鈕 (包含「選取工具」工具提示) 使用 "Segoe MLD2 Assets" 字型的 "EF20" 圖示。 
 
**XAML**

1. 首先，我們宣告 [**InkToolbarCustomToolButton**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) 項目 (customToolButton)，包含指定已設定筆劃選取項目之事件處理常式 (customToolButton_Click) 的 Click 事件接聽程式。 (我們也已經新增一組用於複製、剪下並貼上筆劃選取項目的按鈕)。

2. 我們也新增 Canvas 元素用於繪製我們選取的筆劃。 使用不同的一層來繪製選取筆劃，確保 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 及其內容保持原貌。 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
    </StackPanel>
    <StackPanel x:Name="ToolPanel" Orientation="Horizontal" Grid.Row="1">
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="Top" 
                    TargetInkCanvas="{x:Bind inkCanvas}">
            <InkToolbarCustomToolButton 
                x:Name="customToolButton" 
                Click="customToolButton_Click" 
                ToolTipService.ToolTip="Selection tool">
                <SymbolIcon Symbol="{x:Bind SelectIcon}"/>
            </InkToolbarCustomToolButton>
        </InkToolbar>
        <Button x:Name="cutButton" 
                Content="Cut" 
                Click="cutButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="copyButton" 
                Content="Copy"  
                Click="copyButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="pasteButton" 
                Content="Paste"  
                Click="pasteButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
    </StackPanel>
    <Grid Grid.Row="2" x:Name="outputGrid" 
              Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}" 
              Height="Auto">
        <!-- Canvas for displaying selection UI. -->
        <Canvas x:Name="selectionCanvas"/>
        <!-- Canvas for displaying ink. -->
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

**程式碼後置**

2. 然後我們會在 MainPage.xaml.cs 程式碼後置檔案中處理 [**InkToolbarCustomToolButton**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) 的 Click 事件。

   這個處理常式會設定 [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) 將未處理的輸入傳遞到應用程式。 

   如需此程式碼的詳細步驟，請參閱 [Windows 應用程式中的畫筆互動和 Windows Ink](pen-and-stylus-interactions.md)的傳遞輸入（advanced 處理）一節。

   此外，我們還使用 SymbolIcon 元素指定按鈕的圖示，並使用 {x:Bind} 標記延伸將它繫結到程式碼後置檔案 (SelectIcon) 中定義的欄位。

   下列程式碼片段包含 Click 事件處理常式和 SelectIcon 的定義。

```csharp
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomTool : Page
    {
        // Icon for custom selection tool button.
        Symbol SelectIcon = (Symbol)0xEF20;

        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;

        public MainPage_AddCustomTool()
        {
            this.InitializeComponent();

            // Listen for new ink or erase strokes to clean up selection UI.
            inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
                StrokeInput_StrokeStarted;
            inkCanvas.InkPresenter.StrokesErased +=
                InkPresenter_StrokesErased;
        }

        private void customToolButton_Click(object sender, RoutedEventArgs e)
        {
            // By default, the InkPresenter processes input modified by 
            // a secondary affordance (pen barrel button, right mouse 
            // button, or similar) as ink.
            // To pass through modified input to the app for custom processing 
            // on the app UI thread instead of the background ink thread, set 
            // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
            inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
                InkInputRightDragAction.LeaveUnprocessed;

            // Listen for unprocessed pointer events from modified input.
            // The input is used to provide selection functionality.
            inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
                UnprocessedInput_PointerPressed;
            inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
                UnprocessedInput_PointerMoved;
            inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
                UnprocessedInput_PointerReleased;
        }

        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
            InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
            ClearSelection();
        }

        private void InkPresenter_StrokesErased(
            InkPresenter sender, InkStrokesErasedEventArgs args)
        {
            ClearSelection();
        }

        private void cutButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
            inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            ClearSelection();
        }

        private void copyButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        }

        private void pasteButton_Click(object sender, RoutedEventArgs e)
        {
            if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
            {
                inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                    new Point(0, 0));
            }
            else
            {
                // Cannot paste from clipboard.
            }
        }

        // Clean up selection UI.
        private void ClearSelection()
        {
            var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
            foreach (var stroke in strokes)
            {
                stroke.Selected = false;
            }
            ClearBoundingRect();
        }

        private void ClearBoundingRect()
        {
            if (selectionCanvas.Children.Any())
            {
                selectionCanvas.Children.Clear();
                boundingRect = Rect.Empty;
            }
        }

        // Handle unprocessed pointer events from modifed input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Initialize a selection lasso.
            lasso = new Polyline()
            {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                StrokeThickness = 1,
                StrokeDashArray = new DoubleCollection() { 5, 2 },
            };

            lasso.Points.Add(args.CurrentPoint.RawPosition);

            selectionCanvas.Children.Add(lasso);
        }

        private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
        }

        private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add the final point to the Polyline object and 
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas 
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
                inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                    lasso.Points);

            DrawBoundingRect();
        }

        // Draw a bounding rectangle, on the selection canvas, encompassing 
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
            // Clear all existing content from the selection canvas.
            selectionCanvas.Children.Clear();

            // Draw a bounding rectangle only if there are ink strokes 
            // within the lasso area.
            if (!((boundingRect.Width == 0) ||
                (boundingRect.Height == 0) ||
                boundingRect.IsEmpty))
            {
                var rectangle = new Rectangle()
                {
                    Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                    StrokeThickness = 1,
                    StrokeDashArray = new DoubleCollection() { 5, 2 },
                    Width = boundingRect.Width,
                    Height = boundingRect.Height
                };

                Canvas.SetLeft(rectangle, boundingRect.X);
                Canvas.SetTop(rectangle, boundingRect.Y);

                selectionCanvas.Children.Add(rectangle);
            }
        }
    }
}
```



### <a name="custom-ink-rendering"></a>轉譯自訂的筆跡

根據預設，筆墨輸入是在低延遲背景執行緒上處理，並在其繪製期間轉譯為「濕潤」狀態。 完成筆劃 (拿起畫筆或手指，或是放開滑鼠按鈕) 時，即會在 UI 執行緒上處理該筆劃，並以「烘乾」狀態轉譯到 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 層級 (在應用程式內容上方，並取代濕潤的筆墨)。

筆跡平台可讓您覆寫這個行為，並以自訂乾筆跡輸入完整自訂筆跡體驗。

如需自訂乾燥的詳細資訊，請參閱 [Windows 應用程式中的畫筆互動和 Windows Ink](./pen-and-stylus-interactions.md#custom-ink-rendering)。

> [!NOTE]
> 自訂乾燥與 [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> 如果您的 app 使用自訂的乾燥實作覆寫 [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) 的預設筆跡轉譯行為，InkToolbar 就不會再有轉譯的筆墨筆觸，InkToolbar 的內建清除命令也無法如預期般運作。 若要提供清除功能，就必須處理所有指標事件、對每一個筆劃執行點擊測試，並且覆寫內建的「清除所有筆跡」命令。

## <a name="related-articles"></a>相關文章

- [畫筆和手寫筆互動](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>主題範例

- [筆跡工具列位置和方向範例 (基本)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
- [筆跡工具列位置和方向範例 (動態)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)

### <a name="other-samples"></a>其他範例

- [簡單的筆跡範例 (C#/C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [複雜的筆跡範例 (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [筆跡範例 (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [開始教學課程：在您的 Windows 應用程式中支援筆墨](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [著色本範例](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [家庭記事本範例](https://github.com/Microsoft/Windows-appsample-familynotes)
