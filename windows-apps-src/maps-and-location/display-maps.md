---
title: 顯示 2D、3D 和 Streetside 檢視的地圖
description: 您可以在稱為地圖*座次卡*的淺色可關閉視窗中顯示地圖，亦可在全功能地圖控制項中顯示地圖。
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10、uwp、地圖、位置、地圖控制項、地圖檢視
ms.localizationpriority: medium
ms.openlocfilehash: 1979aa2b2e99a585a122e4835bb6920b9d342cd7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162642"
---
# <a name="display-maps-with-2d-3d-and-streetside-views"></a>顯示 2D、3D 和 Streetside 檢視的地圖

您可以在一個叫地圖*placecard*的淺色可關閉視窗中顯示一張地圖，或者在一個完整精選地圖控制項中顯示。

下載 [地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) 以嘗試本指南中描述的一些功能。

<a id="placecard" />

## <a name="display-map-in-a-placecard"></a>在座次卡中顯示地圖
您可以在輕量型彈出式視窗中向使用者顯示地圖，該視窗在一個 UI 元件或是使用者觸碰的應用程式區域的上方、下方或側面。 地圖可以顯示與應用程式中的資訊相關的城市或地址。  

這個座次卡顯示西雅圖市。

![座次卡顯示西雅圖市](images/placecard-city.png)

這裡有可讓西雅圖顯示在按鈕下方座次卡的代碼。

```csharp
private void Seattle_Click(object sender, RoutedEventArgs e)
{
    Geopoint seattlePoint = new Geopoint
        (new BasicGeoposition { Latitude = 47.6062, Longitude = -122.3321 });

    PlaceInfo spaceNeedlePlace = PlaceInfo.Create(seattlePoint);

    FrameworkElement targetElement = (FrameworkElement)sender;

    GeneralTransform generalTransform =
        targetElement.TransformToVisual((FrameworkElement)targetElement.Parent);

    Rect rectangle = generalTransform.TransformBounds(new Rect(new Point
        (targetElement.Margin.Left, targetElement.Margin.Top), targetElement.RenderSize));

    spaceNeedlePlace.Show(rectangle, Windows.UI.Popups.Placement.Below);
}
```

這個座次卡顯示了西雅圖太空針塔的位置。

![座次卡顯示太空針塔的位置](images/placecard-needle.png)

以下是可讓太空針塔顯示在座次卡按鈕下方的代碼。

```csharp
private void SpaceNeedle_Click(object sender, RoutedEventArgs e)
{
    Geopoint spaceNeedlePoint = new Geopoint
        (new BasicGeoposition { Latitude = 47.6205, Longitude = -122.3493 });

    PlaceInfoCreateOptions options = new PlaceInfoCreateOptions();

    options.DisplayAddress = "400 Broad St, Seattle, WA 98109";
    options.DisplayName = "Seattle Space Needle";

    PlaceInfo spaceNeedlePlace =  PlaceInfo.Create(spaceNeedlePoint, options);

    FrameworkElement targetElement = (FrameworkElement)sender;

    GeneralTransform generalTransform =
        targetElement.TransformToVisual((FrameworkElement)targetElement.Parent);

    Rect rectangle = generalTransform.TransformBounds(new Rect(new Point
        (targetElement.Margin.Left, targetElement.Margin.Top), targetElement.RenderSize));

    spaceNeedlePlace.Show(rectangle, Windows.UI.Popups.Placement.Below);
}
```

<a id="map-control" />

## <a name="display-map-in-a-control"></a>在控制項中顯示地圖

在您的應用程式中使用地圖控制項顯示豐富且可自訂的地圖資料。 地圖控制項可顯示地圖、空照圖、3D 圖、檢視、方向、搜尋結果及交通路況。 您可以在地圖上顯示使用者的位置、方向，以及感興趣的地點。 地圖也可以顯示空照圖 3D 檢視、Streetside 檢視、交通路況、大眾運輸和當地企業。

當您想要 app 內有地圖時，請使用地圖控制項讓使用者能夠檢視 app 專屬或一般地理資訊。 在 app 中擁有地圖控制項，表示使用者不需要離開您的 app 便可取得該資訊。

> [!NOTE]
>如果您不介意使用者離開您的應用程式，請考慮使用 Windows 地圖 app 來提供該資訊。 您的 app 可以啟動 Windows 地圖 app 來顯示特定的地圖、方向以及搜尋結果。 如需詳細資訊，請參閱[啟動 Windows 地圖應用程式](../launch-resume/launch-maps-app.md)。

### <a name="add-a-map-control-to-your-app"></a>將地圖控制項新增到您的應用程式。

藉由新增 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)，在 XAML 頁面上顯示地圖。 若要使用 **MapControl**，您必須在 XAML 頁面或您的程式碼中宣告 [**Windows.UI.Xaml.Controls.Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps) 命名空間。 如果您從工具箱拖曳控制項，會自動新增此命名空間宣告。 如果您是手動將 **MapControl** 新增到 XAML 頁面，就必須在頁面頂端手動新增命名空間宣告。

下列範例顯示基本的地圖控制項，並設定讓地圖在除了接受觸控輸入之外，還能顯示縮放和傾斜控制項。

```xml
<Page
    x:Class="MapsAndLocation1.DisplayMaps"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MapsAndLocation1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:Maps="using:Windows.UI.Xaml.Controls.Maps"
    mc:Ignorable="d">

 <Grid x:Name="pageGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Maps:MapControl
       x:Name="MapControl1"            
       ZoomInteractionMode="GestureAndControl"
       TiltInteractionMode="GestureAndControl"   
       MapServiceToken="EnterYourAuthenticationKeyHere"/>

 </Grid>
</Page>
```

如果您是在程式碼中新增地圖控制項，就必須在程式碼檔案頂端手動宣告命名空間。

```csharp
using Windows.UI.Xaml.Controls.Maps;
...

// Add the MapControl and the specify maps authentication key.
MapControl MapControl2 = new MapControl();
MapControl2.ZoomInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.TiltInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.MapServiceToken = "EnterYourAuthenticationKeyHere";
pageGrid.Children.Add(MapControl2);
```

### <a name="get-and-set-a-maps-authentication-key"></a>取得及設定地圖驗證金鑰

您必須先指定地圖驗證金鑰做為 [**MapServiceToken**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 屬性的值，才能使用 [**MapControl**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken) 和地圖服務。 請在先前的範例中，使用您從 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)取得的金鑰來取代 `EnterYourAuthenticationKeyHere`。 除非您指定地圖驗證金鑰，否則「**警告: 未指定 MapServiceToken**」文字會持續顯示在的控制項下方。 如需有關取得及設定地圖驗證金鑰的詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

## <a name="set-the-location-of-a-map"></a>設定地圖的位置
將地圖指向您想要的任何位置或使用使用者的目前位置。  

### <a name="set-a-starting-location-for-the-map"></a>設定地圖的開始位置

藉由在您的程式碼中指定 [**MapControl**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) 的 [**Center**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 屬性，或在您的 XAML 標記中繫結該屬性，即可設定要在地圖上顯示的位置。 下列範例會顯示以西雅圖市為中心的地圖。

> [!NOTE]
> 由於字串無法轉換成[**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint)，因此您無法在 XAML 標記中指定[**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center)屬性的值，除非您使用資料繫結。 (這項限制也適用於 [**MapControl.Location**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.setlocation) 附加屬性。)

 
```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Specify a known location.
   BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };
   Geopoint cityCenter = new Geopoint(cityPosition);

   // Set the map location.
   MapControl1.Center = cityCenter;
   MapControl1.ZoomLevel = 12;
   MapControl1.LandmarksVisible = true;
}
```

![地圖控制項的範例。](images/displaymapsexample1.png)

### <a name="set-the-current-location-of-the-map"></a>設定地圖的目前位置

您的 app 必須先呼叫 [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 方法，才能存取使用者的位置。 此時，您的 app 必須在前景，且 **RequestAccessAsync** 必須是從 UI 執行緒呼叫。 在使用者授與您的 app 存取其位置的權限之前，您的 app 將無法存取位置資料。

藉由使用 [**Geolocator**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) 類別的 [**GetGeopositionAsync**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 方法，即可取得裝置的目前位置 (如果位置可以使用)。 若要取得對應的 [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint)，您可以使用地理位置坐標的 [**Point**](/uwp/api/windows.devices.geolocation.geocoordinate.point) 屬性。 如需詳細資訊，請參閱[取得目前位置](get-location.md)。

```csharp
// Set your current location.
var accessStatus = await Geolocator.RequestAccessAsync();
switch (accessStatus)
{
   case GeolocationAccessStatus.Allowed:

      // Get the current location.
      Geolocator geolocator = new Geolocator();
      Geoposition pos = await geolocator.GetGeopositionAsync();
      Geopoint myLocation = pos.Coordinate.Point;

      // Set the map location.
      MapControl1.Center = myLocation;
      MapControl1.ZoomLevel = 12;
      MapControl1.LandmarksVisible = true;
      break;

   case GeolocationAccessStatus.Denied:
      // Handle the case  if access to location is denied.
      break;

   case GeolocationAccessStatus.Unspecified:
      // Handle the case if  an unspecified error occurs.
      break;
}
```

在地圖上顯示您的裝置位置時，請考慮顯示圖形，並根據位置資料的正確性設定縮放比例。 如需詳細資訊，請參閱[定位感知應用程式的指導方針](./guidelines-and-checklist-for-detecting-location.md)。

### <a name="change-the-location-of-the-map"></a>變更地圖的位置。

若要變更顯示於 2D 地圖的位置，您可以呼叫 [**TrySetViewAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetviewasync) 方法的其中一個多載。 使用該方法來為 [**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center)、[**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel)、[**Heading**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.heading) 和 [**Pitch**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pitch) 指定新的值。 您也可以提供 [**MapAnimationKind**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapAnimationKind) 列舉中的常數，以指定要在檢視變更時使用的選用動畫。

若要變更 3D 地圖的位置，您可以改用 [**TrySetSceneAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetsceneasync) 方法。 如需詳細資訊，請參閱[顯示空照圖 3D 檢視](#3Dviews)。

呼叫 [**TrySetViewBoundsAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetviewboundsasync) 方法，在地圖上顯示 [**GeoboundingBox**](/uwp/api/Windows.Devices.Geolocation.GeoboundingBox) 的內容。 例如，您可以使用此方法在地圖上顯示一條路線或路線的一部分。 如需詳細資訊，請參閱[在地圖上顯示路線和路線指引](routes-and-directions.md)。

## <a name="change-the-appearance-of-a-map"></a>變更地圖的外觀。

若要自訂地圖的外觀及操作，請將地圖控制項的 [**StyleSheet**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.StyleSheet) 屬性設定為任何現有的 [**MapStyleSheet**](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) 物件。

```csharp
myMap.StyleSheet = MapStyleSheet.RoadDark();
```

![深色樣式地圖](images/style-dark.png)

您也可以使用 JSON 來定義自訂樣式，接著使用該 JSON 來建立 [**MapStyleSheet**](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) 物件。

您可以使用 [ [地圖樣式表單編輯器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) ] 應用程式，以互動方式建立樣式表單 JSON。

```csharp
myMap.StyleSheet = MapStyleSheet.ParseFromJson(@"
    {
        ""version"": ""1.0"",
        ""settings"": {
            ""landColor"": ""#FFFFFF"",
            ""spaceColor"": ""#000000""
        },
        ""elements"": {
            ""mapElement"": {
                ""labelColor"": ""#000000"",
                ""labelOutlineColor"": ""#FFFFFF""
            },
            ""water"": {
                ""fillColor"": ""#DDDDDD""
            },
            ""area"": {
                ""fillColor"": ""#EEEEEE""
            },
            ""political"": {
                ""borderStrokeColor"": ""#CCCCCC"",
                ""borderOutlineColor"": ""#00000000""
            }
        }
    }
");
```

![自訂樣式地圖](images/style-custom.png)

如需完整的 JSON 項目參考，請參閱[地圖樣式表參考](elements-of-map-style-sheet.md)。

您可以從現有樣式表開始，然後使用 JSON 覆寫您想要的任何項目。 此範例從現有的樣式開始，並使用 JSON 變更水域的色彩。

```csharp
 MapStyleSheet \customSheet = MapStyleSheet.ParseFromJson(@"
    {
        ""version"": ""1.0"",
        ""elements"": {
            ""water"": {
                ""fillColor"": ""#DDDDDD""
            }
        }
    }
");

MapStyleSheet builtInSheet = MapStyleSheet.RoadDark();

myMap.StyleSheet = MapStyleSheet.Combine(new List<MapStyleSheet> { builtInSheet, customSheet });
```

![結合樣式地圖](images/style-combined.png)

>[!NOTE]
>您在第二個樣式表中定義的樣式會覆寫第一個樣式中的樣式。

## <a name="set-orientation-and-perspective"></a>設定方向和透視

放大、縮小、旋轉和傾斜地圖的相機，以取得您想要的效果的恰好角度。 請試試這些屬性。

-   設定 [**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) 屬性，將地圖的**中心**設定為某個地理點。
-   將 [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) 屬性設定為 1 到 20 之間的值，以設定地圖的**縮放層級**。
-   設定 [**Heading**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.heading) 屬性以設定地圖的**旋轉**，其中 0 或 360 度 = 北、90 = 東、180 = 南、270 = 西。
-   將 [**DesiredPitch**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.desiredpitch) 屬性設定為 0 到 65 度之間的值，以設定地圖的**傾斜**。

## <a name="show-and-hide-map-features"></a>顯示或隱藏地圖功能

設定下列 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 屬性的值，以顯示或隱藏地圖功能，例如道路和地標。

* 啟用或停用 [**LandmarksVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.landmarksvisible) 屬性，在地圖上顯示**建築物與地標**。

  > [!NOTE]
  > 您可以顯示或隱藏建築物，但不能阻止它們顯示 3D。  

* 啟用或停用 [**PedestrianFeaturesVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pedestrianfeaturesvisible) 屬性，在地圖上顯示**行人功能**，例如公共樓梯。
* 啟用或停用 [**TrafficFlowVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trafficflowvisible) 屬性，在地圖上顯示**交通**。
* 將 [**WatermarkMode**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.watermarkmode) 屬性設定成其中一個 [**MapWatermarkMode**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapWatermarkMode) 常數，以指定是否要在地圖上顯示**浮水印**。
* 將 [**MapRouteView**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView) 新增到地圖控制項的 [**Routes**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes) 集合，以在地圖上顯示**開車或步行路線**。 如需詳細資訊和範例，請參閱[在地圖上顯示路線和路線指引](routes-and-directions.md)。

如需有關如何在 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 中顯示圖釘、圖形及 XAML 控制項的資訊，請參閱[在地圖上顯示興趣點 (POI)](display-poi.md)。

## <a name="display-streetside-views"></a>顯示 Streetside 檢視


Streetside 檢視是位置的街景透視，它會出現在地圖控制項上方。

![地圖控制項的 Streetside 檢視範例。](images/onlystreetside-730width.png)

考慮「走進」Streetside 檢視的經驗，這不同於原先在地圖控制項中顯示的地圖。 例如，變更 Streetside 檢視中的位置並不會變更 Streetside 檢視「底下」的地圖位置或外觀。 關閉 Streetside 檢視之後 (方法是按一下控制項右上角的 **X**)，原始的地圖會保持不變。

顯示 Streetside 檢視：

1.  檢查 [**IsStreetsideSupported**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.isstreetsidesupported) 以判斷裝置是否支援 Streetside 檢視。
2.  如果支援 Streetside 檢視，請呼叫 [**FindNearbyAsync**](/uwp/api/Windows.UI.Xaml.Controls.Maps.StreetsidePanorama) 以在指定位置附近建立 [**StreetsidePanorama**](/uwp/api/windows.ui.xaml.controls.maps.streetsidepanorama.findnearbyasync)。
3.  檢查 [**StreetsidePanorama**](/uwp/api/Windows.UI.Xaml.Controls.Maps.StreetsidePanorama) 是否不是 Null，以判斷是否有找到附近的全景。
4.  如果有找到附近的全景，請為地圖控制項的 [**CustomExperience**](/uwp/api/windows.ui.xaml.controls.maps.streetsideexperience) 屬性建立 [**StreetsideExperience**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.customexperience)。

本範例說明如何顯示類似先前影像的 Streetside 檢視。

**注意**   如果地圖控制項的大小太小，就不會出現總覽圖。

 

```csharp
private async void showStreetsideView()
{
   // Check if Streetside is supported.
   if (MapControl1.IsStreetsideSupported)
   {
      // Find a panorama near Avenue Gustave Eiffel.
      BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 48.858, Longitude = 2.295};
      Geopoint cityCenter = new Geopoint(cityPosition);
      StreetsidePanorama panoramaNearCity = await StreetsidePanorama.FindNearbyAsync(cityCenter);

      // Set the Streetside view if a panorama exists.
      if (panoramaNearCity != null)
      {
         // Create the Streetside view.
         StreetsideExperience ssView = new StreetsideExperience(panoramaNearCity);
         ssView.OverviewMapVisible = true;
         MapControl1.CustomExperience = ssView;
      }
   }
   else
   {
      // If Streetside is not supported
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "Streetside is not supported",
         Content ="\nStreetside views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();            
   }
}
```

<a id="3Dviews" />
## <a name="display-aerial-3d-views"></a>顯示空照圖 3D 檢視


藉由使用 [**MapScene**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapScene) 類別，即可指定地圖的 3D 透視。 地圖場景代表在地圖中顯示的 3D 檢視。 [**MapCamera**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapCamera) 類別代表顯示這類檢視的相機位置。

![地圖場景位置之 MapCamera 位置的圖表](images/mapcontrol-techdiagram.png)

若要讓地圖表面上的建築物和其他功能以 3D 方式顯現，請將地圖控制項的 [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) 屬性設定為 [**MapStyle.Aerial3DWithRoads**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle)。 以下是使用 **Aerial3DWithRoads** 樣式的 3D 檢視範例。

![3D 地圖檢視的範例。](images/only3d-730width.png)

顯示 3D 檢視

1.  檢查 [**Is3DSupported**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.is3dsupported) 以判斷裝置是否支援 3D 檢視。
2.  如果支援 3D 檢視，請將地圖控制項的 [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) 屬性設定為 [**MapStyle.Aerial3DWithRoads**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle)。
3.  使用眾多 **CreateFrom** 方法的其中之一 (例如 [**CreateFromLocationAndRadius**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapScene) 和 [**CreateFromCamera**](/uwp/api/windows.ui.xaml.controls.maps.mapscene.createfromlocationandradius)) 來建立 [**MapScene**](/uwp/api/windows.ui.xaml.controls.maps.mapscene.createfromcamera) 物件。
4.  呼叫 [**TrySetSceneAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetsceneasync) 以顯示 3D 檢視。 您也可以提供 [**MapAnimationKind**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapAnimationKind) 列舉中的常數，以指定要在檢視變更時使用的選用動畫。

本範例說明如何顯示 3D 檢視。

```csharp
private async void display3DLocation()
{
   if (MapControl1.Is3DSupported)
   {
      // Set the aerial 3D view.
      MapControl1.Style = MapStyle.Aerial3DWithRoads;

      // Specify the location.
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 43.773251, Longitude = 11.255474};
      Geopoint hwPoint = new Geopoint(hwGeoposition);

      // Create the map scene.
      MapScene hwScene = MapScene.CreateFromLocationAndRadius(hwPoint,
                                                                           80, /* show this many meters around */
                                                                           0, /* looking at it to the North*/
                                                                           60 /* degrees pitch */);
      // Set the 3D view with animation.
      await MapControl1.TrySetSceneAsync(hwScene,MapAnimationKind.Bow);
   }
   else
   {
      // If 3D views are not supported, display dialog.
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "3D is not supported",
         Content = "\n3D views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();
   }
}
```

## <a name="get-info-about-locations"></a>取得有關位置的相關資訊


藉由呼叫下列 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 方法，即可取得地圖上位置的相關資訊。

-   [**TryGetLocationFromOffset**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.getlocationfromoffset) 方法-取得對應至地圖控制項之視口中指定點的地理位置。
-   [**GetOffsetFromLocation**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.getoffsetfromlocation) 方法 - 取得與指定的地理位置對應的地圖控制項檢視區中的點。
-   [**IsLocationInView**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.islocationinview) 方法 - 決定目前是否能在地圖控制項的檢視區中看見指定的地理位置。
-   [**FindMapElementsAtOffset**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.findmapelementsatoffset) 方法 - 取得地圖上位於地圖控制項檢視區中指定點的元素。

## <a name="handle-interaction-and-changes"></a>處理互動及變更


藉由處理下列 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 事件，即可處理使用者在地圖上的輸入手勢。 請檢查 [**MapInputEventArgs**](/uwp/api/windows.ui.xaml.controls.maps.mapinputeventargs.location) 之 [**Location**](/uwp/api/windows.ui.xaml.controls.maps.mapinputeventargs.position) 和 [**Position**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapInputEventArgs) 屬性的值，以取得手勢發生所在的地圖上地理位置及檢視區中實體位置的相關資訊。

-   [**MapTapped**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.maptapped)
-   [**MapDoubleTapped**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapdoubletapped)
-   [**MapHolding**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapholding)

藉由處理控制項的 [**LoadingStatusChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.loadingstatuschanged) 事件，即可判斷地圖是正在載入，還是已完全載入。

請處理下列 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 事件，以處理使用者或應用程式變更地圖設定時所發生的變更。 [地圖的指導方針]()

-   [**CenterChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.centerchanged)
-   [**HeadingChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.headingchanged)
-   [**PitchChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pitchchanged)
-   [**ZoomLevelChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevelchanged)

## <a name="best-practice-recommendations"></a>最佳做法建議

-   使用充裕的螢幕空間 (或整個螢幕) 來顯示地圖，讓使用者不需要過度平移和縮放即可檢視地理資訊。

-   如果地圖只用來呈現靜態的參考檢視，使用較小的地圖可能更適合。 如果您使用較小的靜態地圖，請根據可用性設定尺寸—設為較小的尺寸以保留足夠的螢幕實際可用空間，但尺寸也要夠大以保持清晰易讀。

-   使用 [**map elements**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapelementsproperty) 在地圖場景中內嵌感興趣的地點；任何額外的資訊都可以透過與地圖場景重疊的暫時性 UI 方式顯示。

## <a name="related-topics"></a>相關主題

* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [UWP 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [取得目前的位置](get-location.md)
* [定位感知應用程式的設計指導方針](./guidelines-and-checklist-for-detecting-location.md)
* [地圖的設計指導方針]()
* [Build 2015 影片：跨手機、平板電腦和電腦運用 Windows app 中的地圖與位置功能](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 車流量應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)