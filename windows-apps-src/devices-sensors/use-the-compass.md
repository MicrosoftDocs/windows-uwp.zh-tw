---
ms.assetid: 5B30E32F-27E0-4656-A834-391A559AC8BC
title: 使用指南針
description: 瞭解如何使用通用 Windows 平臺 (UWP) 羅盤 API 來判斷導覽應用程式中目前的標題。
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4ddd5ddddb31cf93977cb0d5bd9c16e916c4126b
ms.sourcegitcommit: e81227399ba0f286e74e4977d757237829440a2e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96310196"
---
# <a name="use-the-compass"></a>使用指南針


**重要 API**

-   [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
-   [**指南針**](/uwp/api/Windows.Devices.Sensors.Compass)

**範例**

-   如需更完整的實作，請參閱[指南針範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass)。

了解如何使用指南針來判斷目前朝向何方。

應用程式可以根據磁極或正北方來擷取目前所朝向的方向。 導航應用程式會使用指南針來判斷裝置所朝向的方向，然後據此設定地圖方位。

## <a name="prerequisites"></a>Prerequisites

您應該熟悉 Extensible Application Markup Language (XAML)、Microsoft Visual C# 及事件。

您使用的裝置或模擬器必須支援指南針。

## <a name="create-a-simple-compass-app"></a>建立簡單的指南針應用程式

本節分為兩個子區段。 第一個子區段會引導您完成從頭開始建立簡單指南針應用程式所需的步驟。 接下來的子區段會說明您剛建立的應用程式。

### <a name="instructions"></a>指示

-   從 **\[Visual C#\]** 專案範本中選擇 **\[空白應用程式 (通用 Windows)\]** 來建立一個新專案。

-   開啟專案的 MainPage.xaml.cs 檔案，然後以下列程式碼取代現有的程式碼。

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the compass


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Compass _compass; // Our app' s compass object

            // This event handler writes the current compass reading to
            // the textblocks on the app' s main page.

            private async void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
            {
               await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    CompassReading reading = e.Reading;
                    txtMagnetic.Text = String.Format("{0,5:0.00}", reading.HeadingMagneticNorth);
                    if (reading.HeadingTrueNorth.HasValue)
                        txtNorth.Text = String.Format("{0,5:0.00}", reading.HeadingTrueNorth);
                    else
                        txtNorth.Text = "No reading.";
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
               _compass = Compass.GetDefault(); // Get the default compass object

                // Assign an event handler for the compass reading-changed event
                if (_compass != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _compass.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _compass.ReportInterval = reportInterval;
                    _compass.ReadingChanged += new TypedEventHandler<Compass, CompassReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

您需要將之前程式碼片段中的命名空間重新命名為您專案的名稱。 例如，如果您已建立名為 **CompassCS** 的專案，則應該將 `namespace App1` 取代為 `namespace CompassCS`。

-   開啟 MainPage.xaml 檔案，並以下列 XML 取代原始內容。

```xml
    <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
            <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
            <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>

         </Grid>
    </Page>
```

您需要將之前程式碼片段中的第一個部分的類別名稱，換成 app 的命名空間。 例如，如果您已建立名為 **CompassCS** 的專案，則應該將 `x:Class="App1.MainPage"` 取代為 `x:Class="CompassCS.MainPage"`。 您也應該將 `xmlns:local="using:App1"` 取代為 `xmlns:local="using:CompassCS"`。

-   按 F5 或選取 [ **Debug**  >  **開始調試** 程式]，以建立、部署和執行應用程式。

App 開始執行之後，您就可以移動裝置或使用模擬器工具來變更指南針值。

-   返回 Visual Studio 並按 Shift + F5 或選取 [ **Debug** 停止錯] 以停止應用程式，以停止應用程式  >  **Stop Debugging** 。

### <a name="explanation"></a>說明

前面的範例示範了如何只需要撰寫簡短的程式碼，就可以整合 app 中的指南針輸入。

App 會與 **MainPage** 方法中的預設指南針建立連線。

```csharp
_compass = Compass.GetDefault(); // Get the default compass object
```

App 會在 **MainPage** 方法內建立報告間隔。 這段程式碼會擷取裝置所支援的最短間隔，並和所要求的 16 毫秒間隔 (重新整理的速率大約是 60-Hz) 比較。 如果支援的最短間隔大於要求的間隔，程式碼會將該值設定為最小值。 否則，就會將該值設定為要求的間隔。

```csharp
uint minReportInterval = _compass.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_compass.ReportInterval = reportInterval;
```

會在 **ReadingChanged** 方法中擷取新的指南針資料。 每次感應器驅動程式收到感應器的新資料時，都會使用這個事件處理常式將值傳送給 app。 應用程式會用下行程式碼登錄這個事件處理常式。

```csharp
_compass.ReadingChanged += new TypedEventHandler<Compass,
CompassReadingChangedEventArgs>(ReadingChanged);
```

這些新的值會寫入專案 XAML 中的 TextBlock。

```xml
 <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
 <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
 <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>
```
 

 
