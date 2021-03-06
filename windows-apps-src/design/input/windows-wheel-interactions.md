---
description: 瞭解 Surface Dial 以及如何使用它來為 Windows 和 Windows 應用程式啟用吸引人且獨特的使用者互動體驗。
title: Surface Dial 互動
label: Surface Dial interactions
template: detail.hbs
keywords: Surface Dial、Windows 滾輪、RadialController、Radial 控制器、使用者互動、輸入
ms.date: 09/24/2020
ms.topic: article
ms.assetid: e7deb1d6-feeb-471e-9a83-26386d1aaf37
ms.localizationpriority: medium
ms.openlocfilehash: e5b55f35e59779d5fb8dfd01caa843be5bcfa6a3
ms.sourcegitcommit: 40b890c7b862f333879887cc22faff560c49eae6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97598889"
---
# <a name="surface-dial-interactions"></a>Surface Dial 互動

![Surface Dial 與 Surface Studio 的影像](images/windows-wheel/dial-pen-studio-600px.png)  
*Surface Dial、Surface Studio 和手寫筆* (可在 [Microsoft 網上商店](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116)購買)。

## <a name="overview"></a>概觀

Windows 滾輪裝置，例如 Surface Dial，是一種新的輸入裝置，可以針對 Windows 和 Windows 應用程式提供極具吸引力且獨特的使用者互動體驗。 

> [!IMPORTANT]
> 在本主題中，我們特別說明 Surface Dial 互動，但此資訊適用於所有 Windows 滾輪裝置。 

:::row:::
   :::column:::
      <iframe src="https://www.youtube-nocookie.com/embed/WMklcdzcNcU" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe>

      *Surface Dial 應用程式合作夥伴*
   :::column-end:::
   :::column:::
      <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe>

      *適用於開發人員的 Surface Dial*
   :::column-end:::
:::row-end:::

使用以 *旋轉* 動作為依據的外型規格 (或手勢) 時，Surface Dial 會作為輔助的多重強制回應輸入裝置，以便從主要裝置補充輸入。 在大部分情況下，使用者是以慣用手執行工作 (例如以手寫筆寫字)，同時以非慣用手操作這類裝置。 其設計的目的並不是為了精確的指標輸入 (例如觸控、手寫筆或滑鼠)。 

Surface Dial 也支援 *按下和按住* 動作和 *點擊* 動作。 長按有單一的功能︰顯示命令的功能表。 如果功能表使用中，旋轉並按一下輸入是由功能表處理。 否則，輸入就會傳遞到您的應用程式進行處理。 

**就像搭配所有 Windows 輸入裝置一樣，您可以自訂並量身打造 Surface Dial 的互動體驗，以符合您應用程式中的功能。**

> [!TIP]
> Surface Dial 與新的 Surface Studio 一起使用，可以提供更獨特的使用者體驗。  
>
>除了上述預設的長按功能表體驗之外，Surface Dial 也可以直接放置在 Surface Studio 的螢幕上。 這會產生特殊的「螢幕上」功能表。 
>
>系統會偵測 Surface Dial 的接觸位置和界限，使用此資訊處理裝置的遮蔽範圍，並沿著 Dial 外圍迴旋顯示較大版本的功能表。 您的應用程式也可以使用這個相同的資訊，針對裝置是否存在及其預期的使用方式 (例如使用者的手與手臂的放置位置) 打造 UI。

:::row:::
   :::column:::
      **Surface Dial 移開螢幕時的功能表**

      ![螢幕擷取畫面： Surface Dial 的螢幕擷取畫面。](images/windows-wheel/surface-dial-menu-offscreen.png)
   :::column-end:::
   :::column:::
      **Surface Dial 位於螢幕上的功能表**

      ![螢幕擷取畫面：螢幕上的 [Surface Dial] 功能表。](images/windows-wheel/surface-dial-menu-onscreen.png)
   :::column-end:::
:::row-end:::

## <a name="system-integration"></a>系統整合

Surface Dial 與 Windows 緊密整合，而且支援功能表上的一組內建工具︰系統音量、捲動、放大/縮小，以及復原/重做。

這一組內建工具可配合目前的系統內容而包含︰
- 使用者在 Windows 桌面時的系統亮度工具
- 播放媒體時的上一首/下一首工具

除了這個一般的平台支援，Surface Dial 也能與 Windows Ink 平台控制項 ([**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 與 [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar)) 緊密整合。

![Surface Dial 與 Surface 手寫筆](images/windows-wheel/dial-and-pen-400px.png)  
*Surface Dial 與 Surface 手寫筆*

與 Surface Dial 搭配使用時，這些控制項可以有其他功能來修改筆跡屬性，以及控制筆跡工具列的尺規樣板。

當您在使用筆跡工具列的筆跡應用程式中開啟 Surface Dial 功能表時，功能表現在所含的工具可以控制手寫筆類型與筆刷粗細。 尺規啟用時，功能表中就會新增對應的工具，讓裝置控制尺規的位置與角度。

![搭配 Windows Ink 工具列之手寫筆選取工具的 Surface Dial 功能表](images/windows-wheel/surface-dial-menu-inktoolbar-pen.png)  
*搭配 Windows Ink 工具列之手寫筆選取工具的 Surface Dial 功能表*

![搭配 Windows Ink 工具列之筆劃大小工具的 Surface Dial 功能表](images/windows-wheel/surface-dial-menu-inktoolbar-strokesize.png)  
*搭配 Windows Ink 工具列之筆劃大小工具的 Surface Dial 功能表*

![搭配 Windows Ink 工具列之尺規工具的 Surface Dial 功能表](images/windows-wheel/surface-dial-menu-inktoolbar-ruler.png)  
*搭配 Windows Ink 工具列之尺規工具的 Surface Dial 功能表*

## <a name="user-customization"></a>使用者自訂項目

使用者可以透過 **Windows 設定 ->裝置 -> 滾輪** 頁面自訂自己的 Dial 體驗的某些層面，包含預設工具、震動 (或觸覺回饋) 以及寫字的手 (或慣用手)。 

自訂 Surface Dial 使用者體驗時，一定要確定使用者可以使用並啟用特定的功能或行為。

## <a name="custom-tools"></a>自訂工具

我們將在下面討論在 Surface Dial 功能表上自訂顯示工具的 UX 與開發人員指導方針。

### <a name="ux-guidance-for-custom-tools"></a>自訂工具的 UX 指導方針

**確定您的工具對應到目前的情境** 如果工具的功能以及 Surface Dial 互動的運作方式清楚而直覺，就可以協助使用者快速了解並聚焦在自己的工作上。

**盡可能減少應用程式工具的數目**  
Surface Dial 功能表的空間可容納七個項目。 如果有八個以上的項目，使用者就必須旋轉 Dial 才能在溢出的飛出視窗中查看有哪些工具可用，如此一來功能表就很難瀏覽，工具就很難探索和選取。

我們建議為您的應用程式或應用程式情境提供單一的自訂工具。 這樣您就可以根據使用者正在進行的動作設定該工具，使用者不需要啟用 Surface Dial 功能表並選取工具。 

**動態更新工具集合**  
因為 Surface Dial 功能表項目不支援停用的狀態，所以您應該根據使用者的情境 (目前的檢視或具有焦點的視窗) 動態新增和移除工具 (包括內建的預設工具)。 如果某個工具與目前的活動不相關或重複，就移除該工具。

> [!IMPORTANT]
> 當您在功能表中新增項目時，請確定當中還沒有該項目。

**不要移除內建的系統音量設定工具**  
使用者通常都會需要音量控制。 使用者在使用您的應用程式時可能會聆聽音樂，因此必須一律可從 Surface Dial 功能表存取音量和下一首工具。 (媒體播放時，下一首工具會自動新增到功能表中)。

**讓功能表的組織方式保持一致**  
這可以協助使用者在使用您的應用程式時探索和了解可以使用哪些工具，並且在切換工具時協助提昇效率。

**提供與內建圖示一致的高品質圖示**  
圖示可以傳達專業及優點，並且獲得使用者的信任。
- 提供高品質 64 x 64 像素的 PNG 影像 (最小支援 44 x 44)
- 確保背景是透明的
- 圖示應該填滿大部分的影像
- 白色圖示要有一個黑色外框可在高對比模式中顯示

:::row:::
   :::column:::
      ![具有 Alpha 背景之圖示的螢幕擷取畫面。](images/windows-wheel/surface-dial-menu-icon1.png)

      *使用 alpha 背景的圖示*
   :::column-end:::
   :::column:::
      ![以預設主題顯示于滾輪功能表上之圖示的螢幕擷取畫面。](images/windows-wheel/surface-dial-menu-icon2.png)

      *滾輪功能表上使用預設佈景主題所顯示的圖示*
   :::column-end:::
   :::column:::
      ![具有高對比白色主題之滾輪功能表上顯示圖示的螢幕擷取畫面。](images/windows-wheel/surface-dial-menu-icon3.png)

      *滾輪功能表上使用高對比白色佈景主題所顯示的圖示*
   :::column-end:::
:::row-end:::

**使用簡潔和清楚描述的名稱**  
工具名稱會與工具圖示一起顯示在工具功能表中，螢幕助讀程式也會使用此名稱。 
- 名稱應該要簡短到可以放入滾輪功能表的中央圓形內
- 名稱應該要清楚地識別主要動作 (可以暗示補充動作)︰
  - 捲軸指出兩個旋轉方向的效果
  - 復原指定主要動作，但是使用者可以推斷和輕鬆探索重做 (補充動作)

### <a name="developer-guidance"></a>開發人員指引

您可以透過一組完整的 [Windows 執行階段 API](/uwp/api/Windows.UI.Input.RadialController) 自訂 Surface Dial 體驗，補充您應用程式的功能。 

如先前所述，預設的 Surface Dial 功能表會預先填入一組內建工具，涵蓋廣泛的基本系統功能 (系統音量、系統亮度、捲動、縮放、復原，以及當系統偵測到播放音訊或視訊時的媒體控制項)。 不過，這些預設工具可能無法提供您的應用程式所需的功能。 

在下列各節中，我們將說明如何在 Surface Dial 功能表中新增自訂工具，並指定要顯示哪些內建工具。

從 [RadialController 自訂](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-radialcontroller-customization.zip)下載此範例的更健全版本。

**新增自訂工具**

在此範例中，我們要新增一個基本的自訂工具，將旋轉和按一下事件的輸入資料傳遞到一些 XAML UI 控制項。

1. 首先，我們在 XAML 中宣告我們的 UI (只有一個滑桿和切換按鈕)。

   ![星形控制器範例的螢幕擷取畫面，其中的水準滑杆設為左邊。](images/windows-wheel/surface-dial-snippet-customtool1.png)  
   *範例應用程式 UI*

    ```Xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
          <TextBlock x:Name="Header"
            Text="RadialController customization sample"
            VerticalAlignment="Center"
            Style="{ThemeResource HeaderTextBlockStyle}"
            Margin="10,0,0,0" />
      </StackPanel>
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center"
        Grid.Row="1">
          <!-- Slider for rotation input -->
          <Slider x:Name="RotationSlider"
            Width="300"
            HorizontalAlignment="Left"/>
          <!-- Switch for click input -->
          <ToggleSwitch x:Name="ButtonToggle"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    ```

2. 然後，在程式碼後置中，我們將自訂工具新增到 Surface Dial 功能表，並宣告 [**RadialController**](/uwp/api/Windows.UI.Input.RadialController) 輸入處理常式。 

   我們呼叫 [**CreateForCurrentView**](/uwp/api/Windows.UI.Input.RadialController) 取得 Surface Dial (myController) 的 [**RadialController**](/uwp/api/windows.ui.input.radialcontroller.createforcurrentview) 物件的參考。

   接著我們呼叫 [**RadialControllerMenuItem.CreateFromIcon**](/uwp/api/Windows.UI.Input.RadialControllerMenuItem) 建立 [**RadialControllerMenuItem**](/uwp/api/windows.ui.input.radialcontrollermenuitem.createfromicon) (myItem) 的執行個體。 

   接下來，我們將該項目附加到功能表項目的集合。

   我們宣告 [**RadialController**](/uwp/api/windows.ui.input.radialcontroller.buttonclicked) 物件的輸入事件處理常式 ([**ButtonClicked**](/uwp/api/windows.ui.input.radialcontroller.rotationchanged) 和 [**RotationChanged**](/uwp/api/Windows.UI.Input.RadialController))。

   最後，我們定義事件處理常式。

    ```csharp
    public sealed partial class MainPage : Page
    {
        RadialController myController;

        public MainPage()
        {
            this.InitializeComponent();
            // Create a reference to the RadialController.
            myController = RadialController.CreateForCurrentView();

            // Create an icon for the custom tool.
            RandomAccessStreamReference icon =
              RandomAccessStreamReference.CreateFromUri(
                new Uri("ms-appx:///Assets/StoreLogo.png"));

            // Create a menu item for the custom tool.
            RadialControllerMenuItem myItem =
              RadialControllerMenuItem.CreateFromIcon("Sample", icon);

            // Add the custom tool to the RadialController menu.
            myController.Menu.Items.Add(myItem);

            // Declare input handlers for the RadialController.
            myController.ButtonClicked += MyController_ButtonClicked;
            myController.RotationChanged += MyController_RotationChanged;
        }

        // Handler for rotation input from the RadialController.
        private void MyController_RotationChanged(RadialController sender,
          RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees > 100)
            {
                RotationSlider.Value = 100;
                return;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < 0)
            {
                RotationSlider.Value = 0;
                return;
            }
            RotationSlider.Value += args.RotationDeltaInDegrees;
        }

        // Handler for click input from the RadialController.
        private void MyController_ButtonClicked(RadialController sender,
          RadialControllerButtonClickedEventArgs args)
        {
            ButtonToggle.IsOn = !ButtonToggle.IsOn;
        }
    }
    ```

當我們執行應用程式時，我們使用 Surface Dial 與其進行互動。 首先，我們長按以開啟功能表，然後選取我們自訂的工具。 自訂工具啟用之後，可以旋轉 Dial 調整滑桿控制項，而且可以按一下 Dial 切換開關。

![星形控制器範例的螢幕擷取畫面，其中的水準滑杆設為中間。](images/windows-wheel/surface-dial-snippet-customtool2.png)  
*啟用 Surface Dial 自訂工具的範例應用程式 UI*

**指定內建的工具**

您可以使用 [**RadialControllerConfiguration**](/uwp/api/Windows.UI.Input.RadialControllerConfiguration) 類別來自訂您應用程式的內建功能表項目集合。

例如，如果您的應用程式不會有任何捲動或縮放區域，而且不需要復原/重做功能，就可以從功能表移除這些工具。 這樣就能在功能表上騰出空間，新增您應用程式的自訂工具。 

> [!IMPORTANT] 
> Surface Dial 功能表必須至少要有一個功能表項目。 如果您在新增自訂工具之前移除了所有預設工具，預設工具會還原，您的工具則會附加到預設集合。

根據設計指導方針，我們不建議移除媒體控制項工具 (音量與上一首/下一首)，因為使用者經常會播放背景音樂，同時執行其他工作。

以下，我們將說明如何設定 Surface Dial 功能表，只放入音量和下一首/上一首的媒體控制項。

```csharp
public MainPage()
{
  ...
  //Remove a subset of the default system tools
  RadialControllerConfiguration myConfiguration = 
  RadialControllerConfiguration.GetForCurrentView();
  myConfiguration.SetDefaultMenuItems(new[] 
  {
    RadialControllerSystemMenuItemKind.Volume,
      RadialControllerSystemMenuItemKind.NextPreviousTrack
  });
}
```

## <a name="custom-interactions"></a>自訂互動

如先前所述，Surface Dial 支援三種手勢 (長按、旋轉、按一下)，分別有對應的預設互動。 

請確定根據這些手勢的任何自訂互動，對選取的動作或工具是有意義的。 

> [!NOTE]
> 互動體驗取決於 Surface Dial 功能表的狀態。 如果功能表使用中，就會處理輸入，否則您的應用程式會接手。

### <a name="press-and-hold"></a>長按

這個手勢會啟用並顯示 Surface Dial 功能表，沒有任何App 功能這個手勢相關聯。 

此功能表預設會顯示在使用者的螢幕中央。 不過，使用者可以抓取它，然後移動到任何位置。

> [!NOTE]
> 如果將 Surface Dial 放在 Surface Studio 的螢幕上，就會在 Surface Dial 放在螢幕上的位置中央顯示功能表。

### <a name="rotate"></a>Rotate

Surface Dial 主要是設計來支援涉及順暢、增量調整類比值或控制項的互動旋轉。

裝置可以順時針方向和逆時針方向旋轉，也可以提供觸覺回饋以指出不連續的距離。

> [!NOTE]
> 使用者可以在 **Windows 設定 ->裝置 -> 滾輪** 頁面中停用觸覺回饋。

#### <a name="ux-guidance-for-custom-interactions"></a>自訂互動的 UX 指導方針

**具連續或高旋轉靈敏度的工具應該停用觸覺回饋**

觸覺回饋要符合使用中工具的旋轉靈敏度。 我們建議具連續或高旋轉靈敏度的工具停用觸覺回饋，因為使用者可能會感到不舒服。 

**慣用手應該不會影響以旋轉為基礎的互動**

Surface Dial 無法偵測到您正在使用哪隻手，但是使用者可以在 **Windows 設定 ->裝置 -> 手寫筆與 Windows Ink** 中設定寫字的手 (或慣用手)。

**所有旋轉互動應考慮地區設定**

讓您的互動提供並可配合地區設定以及由右至左配置，提高客戶的滿意度。

Dial 功能表上的內建工具和命令遵循下列指導方針進行以旋轉為基礎的互動︰

:::row:::
   :::column:::
      Left

      Up

      外 
   :::column-end:::
   :::column span="2":::
      ![Surface Dial 的影像](images/windows-wheel/surface-dial-rotate.png)
   :::column-end:::
   :::column:::
      Right

      Down

      位於
   :::column-end:::
:::row-end:::

| 概念方向 | 對應到 Surface Dial | 順時針方向旋轉 | 逆時針方向旋轉 |
| --- | --- | --- | --- |
| 水平 | 以 Surface Dial 的頂端為基準對應向左和向右 | Right | Left |
| Vertical | 以 Surface Dial 的左側為基準對應向上和向下 | Down | Up |
| Z 軸 | 向內 (或接近) 對應到向上/向右<br/>向外 (或離開) 對應到向左/向下 | 位於 | 外 |

#### <a name="developer-guidance"></a>開發人員指引

當使用者旋轉裝置，會根據相對於旋轉方向的差異 ([**RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees**](/uwp/api/windows.ui.input.radialcontroller.rotationchanged)) 引發 [**RadialController.RotationChanged**](/uwp/api/windows.ui.input.radialcontrollerrotationchangedeventargs.rotationdeltaindegrees) 事件。 使用 [**RadialController.RotationResolutionInDegrees**](/uwp/api/windows.ui.input.radialcontroller.rotationresolutionindegrees) 屬性可以設定資料的靈敏度 (或解析度)。

> [!NOTE]
> 根據預設，裝置最少要旋轉 10 度，旋轉的輸入事件才會傳遞至 [**RadialController**](/uwp/api/Windows.UI.Input.RadialController) 物件。 每個輸入事件都會導致裝置震動。

一般而言，我們建議當旋轉解析度設定為少於 5 度時停用觸覺回饋。 這可以為連續的互動提供更順暢的體驗。 

您可以設定 [**RadialController.UseAutomaticHapticFeedback**](/uwp/api/windows.ui.input.radialcontroller.useautomatichapticfeedback) 屬性，啟用和停用自訂工具的觸覺回饋。

> [!NOTE]
> 您不能覆寫系統工具 (例如音量控制) 的觸覺回饋行為。 這些工具的觸覺回饋，只能由使用者從滾輪設定頁面停用。

以下是如何自訂旋轉資料解析度，以及啟用或停用觸覺回饋的範例。

```csharp
private void MyController_ButtonClicked(RadialController sender, 
  RadialControllerButtonClickedEventArgs args)
{
  ButtonToggle.IsOn = !ButtonToggle.IsOn;

  if(ButtonToggle.IsOn)
  {
    //high resolution mode
    RotationSlider.LargeChange = 1;
    myController.UseAutomaticHapticFeedback = false;
    myController.RotationResolutionInDegrees = 1;
  }
  else
  {
    //low resolution mode
    RotationSlider.LargeChange = 10;
    myController.UseAutomaticHapticFeedback = true;
    myController.RotationResolutionInDegrees = 10;
  }
}
```

### <a name="click"></a>按一下

按一下 Surface Dial 類似按一下滑鼠左鍵 (裝置的旋轉狀態不會影響此動作)。

#### <a name="ux-guidance"></a>UX 指導方針

**如果使用者無法輕鬆地從結果恢復，請不要對應動作，或對這個手勢執行命令**

您的應用程式根據使用者按一下 Surface Dial 所採取的任何動作都必須是可以還原的。 請務必讓使用者可以輕鬆地將應用程式周遊回堆疊，並還原先前的應用程式狀態。

二進位操作 (例如靜音/取消靜音或顯示/隱藏) 使用按一下手勢可以提供良好的使用者體驗。

**按一下 Surface Dial 不應啟用或停用強制回應工具**

有些應用程式/工具模式可能會與依賴旋轉的互動發生衝突，或停用這類互動。 Windows Ink 工具列中尺規之類的工具，應該透過其他 UI 能供性開啟或關閉 (Ink 工具列提供內建的 [**ToggleButton**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton) 控制項)。

對於強制回應工具，請將使用中的 Surface Dial 功能表項目對應到目標工具或先前選取的功能表項目。

#### <a name="developer-guidance"></a>開發人員指引

按一下 Surface Dial 時，會引發 [**RadialController.ButtonClicked**](/uwp/api/windows.ui.input.radialcontroller.buttonclicked) 事件。 [**RadialControllerButtonClickedEventArgs**](/uwp/api/Windows.UI.Input.RadialControllerButtonClickedEventArgs) 包括 [**Contact**](/uwp/api/windows.ui.input.radialcontrollerbuttonclickedeventargs.contact) 屬性，其中包含 Surface Dial 在 Surface Studio 螢幕上接觸的位置以及週框區域。 如果 Surface Dial 沒有接觸螢幕，這個屬性為 null。 

### <a name="on-screen"></a>螢幕上

如先前所述，Surface Dial 可以搭配 Surface Studio 使用，在特殊的螢幕上模式顯示 Surface Dial 功能表。 

處於這個模式時，可以與您的應用程式更進一步整合並自訂您的 Dial 互動體驗。 同時使用 Surface Dial 與 Surface Studio 才能獲得的獨特體驗包括下列範例︰

- 根據 Surface Dial 的位置顯示與內容相關的工具 (例如調色盤)，讓這些工具更容易尋找及使用
- 根據 Surface Dial 放置的 UI 設定可使用的工具
- 根據 Surface Dial 的位置放大螢幕區域
- 根據螢幕位置進行獨特的遊戲互動

#### <a name="ux-guidance-for-on-screen-interactions"></a>螢幕互動的 UX 指導方針

**在螢幕上偵測到 Surface Dial 時，應用程式應該要回應**

視覺化回饋有助於告訴使用者您的應用程式已偵測到裝置放在 Surface Studio 的螢幕上。

**根據裝置的位置調整與 Surface Dial 相關的 UI**

取決於使用者放置裝置的位置，裝置 (和使用者的身體) 可能會遮蓋重要的 UI。

**根據使用者的互動調整與 Surface Dial 相關的 UI**

除了硬體遮蔽，使用者在使用裝置時，手和手臂可能會遮蓋到部分螢幕。

被遮蔽的區域取決於正用哪隻手使用裝置。 由於裝置主要是搭配非慣用手使用而設計，所以應該針對使用者指定的相反手調整與 Surface Dial 相關的 UI (**Windows 設定 > 裝置 > 手寫筆與 Windows Ink > 選擇您用來寫字的那隻手** 設定)。

**互動應該回應 Surface Dial 的位置而不是移動**

裝置是設計來固定在螢幕上而不是滑動，因為它不是精確的指標裝置。 因此，我們希望使用者較常提起和放置 Surface Dial 而不是在螢幕上拖曳。

**使用螢幕位置判斷使用者意圖**

根據 UI 內容設定可使用的工具 (例如接近控制項、畫布或視窗)，減少執行工作所需的步驟可以增進使用者體驗。

#### <a name="developer-guidance"></a>開發人員指引

將 Surface Dial 放在 Surface Studio 的數位板表面時，會引發 [**RadialController.ScreenContactStarted**](/uwp/api/windows.ui.input.radialcontroller.screencontactstarted) 事件並將接觸資訊 ([**RadialControllerScreenContactStartedEventArgs.Contact**](/uwp/api/windows.ui.input.radialcontrollerscreencontactstartedeventargs.contact)) 提供給您的應用程式。

同樣地，如果在接觸 Surface Studio 的數位板表面時按一下 Surface Dial，會引發 [**RadialController.ButtonClicked**](/uwp/api/windows.ui.input.radialcontroller.buttonclicked) 事件並將接觸資訊 ([**RadialControllerButtonClickedEventArgs.Contact**](/uwp/api/windows.ui.input.radialcontrollerbuttonclickedeventargs.contact)) 提供給您的應用程式。 

接觸資訊 ([**RadialControllerScreenContact**](/uwp/api/Windows.UI.Input.RadialControllerScreenContact)) 包含 Surface Dial 中心在應用程式座標空間中的 X/Y 座標 ([**RadialControllerScreenContact.Position**](/uwp/api/windows.ui.input.radialcontrollerscreencontact.position))，以及週框 ([**RadialControllerScreenContact.Bounds**](/uwp/api/windows.ui.input.radialcontrollerscreencontact.bounds))，單位為裝置獨立畫素 (DIP)。 這項資訊對於提供情境給可使用工具，以及提供與裝置相關的視覺化回饋給使用者時很有用。

在下列範例中，我們建立了一個有四個不同區段的基本應用程式，每一個區段都包含一個滑桿和一個切換開關。 我們接著會使用 Surface Dial 在螢幕上的位置指定 Surface Dial 要控制哪一組滑桿和切換開關。

1. 首先，我們在 XAML 中宣告我們的 UI (四個區段，每個區段都有一個滑桿和切換按鈕)。

   ![星形控制器範例的螢幕擷取畫面，其中四個水準滑杆設為左邊。](images/windows-wheel/surface-dial-snippet-customtool3.png)  
   *範例應用程式 UI*

   ```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
  <Grid.RowDefinitions>
    <RowDefinition Height="Auto"/>
    <RowDefinition Height="*"/>
  </Grid.RowDefinitions>
  <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
    <TextBlock x:Name="Header"
      Text="RadialController customization sample"
      VerticalAlignment="Center"
      Style="{ThemeResource HeaderTextBlockStyle}"
      Margin="10,0,0,0" />
  </StackPanel>
  <Grid Grid.Row="1" x:Name="RootGrid">
    <Grid.RowDefinitions>
      <RowDefinition Height="*"/>
      <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
      <ColumnDefinition Width="*"/>
      <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <Grid x:Name="Grid0"
      Grid.Row="0"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider0"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle0"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid1"
      Grid.Row="0"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider1"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle1"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid2"
      Grid.Row="1"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider2"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle2"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid3"
      Grid.Row="1"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider3"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle3"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
  </Grid>
</Grid>
   ```

2. 以下程式碼後置包含針對 Surface Dial 螢幕位置定義的處理常式。

```csharp
Slider ActiveSlider;
ToggleSwitch ActiveSwitch;
Grid ActiveGrid;

public MainPage()
{
  ...

  myController.ScreenContactStarted += 
    MyController_ScreenContactStarted;
  myController.ScreenContactContinued += 
    MyController_ScreenContactContinued;
  myController.ScreenContactEnded += 
    MyController_ScreenContactEnded;
  myController.ControlLost += MyController_ControlLost;

  //Set initial grid for Surface Dial input.
  ActiveGrid = Grid0;
  ActiveSlider = RotationSlider0;
  ActiveSwitch = ButtonToggle0;
}

private void MyController_ScreenContactStarted(RadialController sender, 
  RadialControllerScreenContactStartedEventArgs args)
{
  //find grid at contact location, update visuals, selection
  ActivateGridAtLocation(args.Contact.Position);
}

private void MyController_ScreenContactContinued(RadialController sender, 
  RadialControllerScreenContactContinuedEventArgs args)
{
  //if a new grid is under contact location, update visuals, selection
  if (!VisualTreeHelper.FindElementsInHostCoordinates(
    args.Contact.Position, RootGrid).Contains(ActiveGrid))
  {
    ActiveGrid.Background = new 
      SolidColorBrush(Windows.UI.Colors.White);
    ActivateGridAtLocation(args.Contact.Position);
  }
}

private void MyController_ScreenContactEnded(RadialController sender, object args)
{
  //return grid color to normal when contact leaves screen
  ActiveGrid.Background = new 
  SolidColorBrush(Windows.UI.Colors.White);
}

private void MyController_ControlLost(RadialController sender, object args)
{
  //return grid color to normal when focus lost
  ActiveGrid.Background = new 
    SolidColorBrush(Windows.UI.Colors.White);
}

private void ActivateGridAtLocation(Point Location)
{
  var elementsAtContactLocation = 
    VisualTreeHelper.FindElementsInHostCoordinates(Location, 
      RootGrid);

  foreach (UIElement element in elementsAtContactLocation)
  {
    if (element as Grid == Grid0)
    {
      ActiveSlider = RotationSlider0;
      ActiveSwitch = ButtonToggle0;
      ActiveGrid = Grid0;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid1)
    {
      ActiveSlider = RotationSlider1;
      ActiveSwitch = ButtonToggle1;
      ActiveGrid = Grid1;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid2)
    {
      ActiveSlider = RotationSlider2;
      ActiveSwitch = ButtonToggle2;
      ActiveGrid = Grid2;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid3)
    {
      ActiveSlider = RotationSlider3;
      ActiveSwitch = ButtonToggle3;
      ActiveGrid = Grid3;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
  }
}
```

當我們執行應用程式時，我們使用 Surface Dial 與其進行互動。 首先，我們將裝置放在 Surface Studio 螢幕上，應用程式會偵測到並與右下角的區段關聯 (參閱影像)。 然後，我們長按 Surface Dial 以開啟功能表，然後選取我們自訂的工具。 自訂工具啟用之後，可以旋轉 Surface Dial 調整滑桿控制項，而且可以按一下 Surface Dial 切換開關。

![星形控制器範例的螢幕擷取畫面，其中四個水準滑杆設為左方，而第四個控制器已反白顯示。](images/windows-wheel/surface-dial-snippet-customtool4.png)  
*啟用 Surface Dial 自訂工具的範例應用程式 UI*

## <a name="summary"></a>摘要

本主題提供 Surface Dial 輸入裝置的概觀，以及搭配 Surface Studio 使用時，如何針對移開螢幕時的案例和放上螢幕時的案例自訂使用者體驗的 UX 與開發人員指導方針。

請將您的問題、建議和意見反應傳送給 [radialcontroller@microsoft.com](mailto:radialcontroller@microsoft.com) 。

## <a name="related-articles"></a>相關文章

[教學課程：支援 Windows 應用程式中) 的 Surface Dial (和其他輪子裝置](radialcontroller-walkthrough.md)

### <a name="api-reference"></a>API 參考資料

- [**RadialController** 類別](/uwp/api/Windows.UI.Input.RadialController)
- [**RadialControllerButtonClickedEventArgs** 類別](/uwp/api/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**RadialControllerConfiguration** 類別](/uwp/api/Windows.UI.Input.RadialControllerConfiguration) 
- [**RadialControllerControlAcquiredEventArgs** 類別](/uwp/api/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**RadialControllerMenu** 類別](/uwp/api/Windows.UI.Input.RadialControllerMenu) 
- [**RadialControllerMenuItem** 類別](/uwp/api/Windows.UI.Input.RadialControllerMenuItem) 
- [**RadialControllerRotationChangedEventArgs** 類別](/uwp/api/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**RadialControllerScreenContact** 類別](/uwp/api/Windows.UI.Input.RadialControllerScreenContact) 
- [**RadialControllerScreenContactContinuedEventArgs** 類別](/uwp/api/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**RadialControllerScreenContactStartedEventArgs** 類別](/uwp/api/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [**RadialControllerMenuKnownIcon** 列舉](/uwp/api/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [**RadialControllerSystemMenuItemKind** 列舉](/uwp/api/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>範例

#### <a name="topic-samples"></a>主題範例

[RadialController 自訂](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-radialcontroller-customization.zip)

#### <a name="other-samples"></a>其他範例

[著色書籍範例](https://github.com/Microsoft/Windows-appsample-coloringbook)

[開始教學課程：支援 Windows 應用程式中) 的 Surface Dial (和其他輪子裝置](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-RadialController)

[通用 Windows 平台範例 (C# 和 C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)

[Windows 傳統桌面範例](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)
