---
title: 取得使用者的位置
description: 尋找使用者的位置並回應位置變更。 存取使用者的位置是由 \[設定\] app 中的隱私權設定所管理。 本主題也示範如何檢查您的應用程式是否具備存取使用者位置的權限。
ms.assetid: 24DC9A41-8CC1-48B0-BC6D-24BF571AFCC8
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, 地圖, 位置, 定位功能
ms.localizationpriority: medium
ms.openlocfilehash: 8a60b5003310fdba046b624e61007761ef5e0f20
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297761"
---
# <a name="get-the-users-location"></a>取得使用者的位置

> [!NOTE]
> [**隨 mapcontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 和地圖服務會 Requite 稱為 [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken)的地圖服務驗證金鑰。 如需有關取得及設定地圖驗證金鑰的詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

尋找使用者的位置並回應位置變更。 存取使用者的位置是由 \[設定\] app 中的隱私權設定所管理。 本主題也示範如何檢查您的應用程式是否具備存取使用者位置的權限。

**提示**：若要深入了解如何在您的 app 中存取使用者的位置，請從 GitHub 的 [Windows-universal-samples 存放庫](https://github.com/Microsoft/Windows-universal-samples)下載下列範例。

-   [通用 Windows 平台 (UWP) 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="enable-the-location-capability"></a>啟用位置功能


1.  在 \[**方案總管**\] 中按兩下 **package.appxmanifest**，然後選取 \[**功能**\] 索引標籤。
2.  在 **\[功能\]** 清單中，選取 **\[位置\]** 的方塊。 這會將 `location` 裝置功能新增至封裝資訊清單檔案中。

```XML
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="get-the-current-location"></a>取得目前的位置


本節說明如何使用 [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) 命名空間中的 API 來偵測使用者的地理位置。

### <a name="step-1-request-access-to-the-users-location"></a>步驟 1：要求使用者位置的存取權

除非您的應用程式具有粗略的位置功能 (請參閱附注) ，您必須先使用 [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 方法要求存取使用者的位置，然後再嘗試存取該位置。 您必須從 UI 執行緒呼叫 **RequestAccessAsync** 方法，而且您的 app 必須在前景中。 在使用者將權限授與您的 app 之後，您的 app 才能存取使用者的位置資訊。\*

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```



[**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 方法會提示使用者提供可存取其位置的權限。 只會提示使用者一次 (每一 app)。 在使用者第一次授與或拒絕權限之後，這個方法就不會再顯示權限提示。 為了協助使用者在出現過提示之後變更位置權限，建議您提供一個位置設定連結，如本主題稍後所示範。

>注意：粗略位置功能可讓您的應用程式取得刻意模糊的 (不精確) 位置，而不需要取得使用者的明確許可權 (全系統位置切換必須保持 **開啟**，但) 。 若要瞭解如何在您的應用程式中使用粗略位置，請參閱[**Geolocator**](/uwp/api/windows.devices.geolocation.geolocator)類別中的[**AllowFallbackToConsentlessPositions**](/uwp/api/windows.devices.geolocation.geolocator.allowfallbacktoconsentlesspositions)方法。

### <a name="step-2-get-the-users-location-and-register-for-changes-in-location-permissions"></a>步驟 2：取得使用者的位置並登錄位置權限的變更

[**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) 方法會執行目前位置的單次讀取。 在這裡，**switch** 陳述式是與 **accessStatus** (來自先前的範例) 搭配使用，只有在獲允許存取使用者位置的情況下才有作用。 如果獲允許存取使用者的位置，程式碼就會建立 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 物件、登錄位置權限的變更，以及要求使用者的位置。

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);

        // If DesiredAccuracy or DesiredAccuracyInMeters are not set (or value is 0), DesiredAccuracy.Default is used.
        Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = _desireAccuracyInMetersValue };

        // Subscribe to the StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;

        // Carry out the operation.
        Geoposition pos = await geolocator.GetGeopositionAsync();

        UpdateLocationData(pos);
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        UpdateLocationData(null);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        UpdateLocationData(null);
        break;
}
```

### <a name="step-3-handle-changes-in-location-permissions"></a>步驟 3：處理位置權限的變更

[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 物件會觸發 [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) 事件，以指出使用者的位置設定已變更。 該事件會透過引數的 **Status** 屬性 (類型為 [**PositionStatus**](/uwp/api/Windows.Devices.Geolocation.PositionStatus)) 傳遞對應的狀態。 請注意，此方法並不是從 UI 執行緒呼叫，且 [**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 物件會叫用 UI 變更。

```csharp
using Windows.UI.Core;
...
async private void OnStatusChanged(Geolocator sender, StatusChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Show the location setting message only if status is disabled.
        LocationDisabledMessage.Visibility = Visibility.Collapsed;

        switch (e.Status)
        {
            case PositionStatus.Ready:
                // Location platform is providing valid data.
                ScenarioOutput_Status.Text = "Ready";
                _rootPage.NotifyUser("Location platform is ready.", NotifyType.StatusMessage);
                break;

            case PositionStatus.Initializing:
                // Location platform is attempting to acquire a fix.
                ScenarioOutput_Status.Text = "Initializing";
                _rootPage.NotifyUser("Location platform is attempting to obtain a position.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NoData:
                // Location platform could not obtain location data.
                ScenarioOutput_Status.Text = "No data";
                _rootPage.NotifyUser("Not able to determine the location.", NotifyType.ErrorMessage);
                break;

            case PositionStatus.Disabled:
                // The permission to access location data is denied by the user or other policies.
                ScenarioOutput_Status.Text = "Disabled";
                _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

                // Show message to the user to go to location settings.
                LocationDisabledMessage.Visibility = Visibility.Visible;

                // Clear any cached location data.
                UpdateLocationData(null);
                break;

            case PositionStatus.NotInitialized:
                // The location platform is not initialized. This indicates that the application
                // has not made a request for location data.
                ScenarioOutput_Status.Text = "Not initialized";
                _rootPage.NotifyUser("No request for location is made yet.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NotAvailable:
                // The location platform is not available on this version of the OS.
                ScenarioOutput_Status.Text = "Not available";
                _rootPage.NotifyUser("Location is not available on this version of the OS.", NotifyType.ErrorMessage);
                break;

            default:
                ScenarioOutput_Status.Text = "Unknown";
                _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
                break;
        }
    });
}
```

## <a name="respond-to-location-updates"></a>回應位置更新


本節說明如何使用 [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 事件，以接收一段時間後的使用者位置更新。 由於使用者可以隨時撤銷對位置的存取權，因此請務必如上一節所示，呼叫 [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 並使用 [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) 事件。

本節假設您已經啟用位置功能，並已從您的前景 app UI 執行緒呼叫 [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync)。

### <a name="step-1-define-the-report-interval-and-register-for-location-updates"></a>步驟 1：定義報告擷取和登錄位置更新

在這個範例中，**switch** 陳述式是與 **accessStatus** (來自先前的範例) 搭配使用，只有在獲允許存取使用者位置的情況下才有作用。 如果獲允許存取使用者的位置，程式碼就會建立 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 物件、指定追蹤類型，以及登錄位置更新。

[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 物件可以根據位置的變更 (距離型追蹤) 或時間的變更 (定期型追蹤) 來觸發 [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 事件。

-   針對距離型追蹤，請設定 [**MovementThreshold**](/uwp/api/windows.devices.geolocation.geolocator.movementthreshold) 屬性。
-   針對定期型追蹤，請設定 [**ReportInterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval) 屬性。

如果兩個屬性都未設定，將會每隔 1 秒 (對等於 `ReportInterval = 1000`) 傳回一次位置。 這裡使用的是 2 秒 (`ReportInterval = 2000`) 的報告間隔。
```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();

switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        // Create Geolocator and define perodic-based tracking (2 second interval).
        _geolocator = new Geolocator { ReportInterval = 2000 };

        // Subscribe to the PositionChanged event to get location updates.
        _geolocator.PositionChanged += OnPositionChanged;

        // Subscribe to StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;

        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        StartTrackingButton.IsEnabled = false;
        StopTrackingButton.IsEnabled = true;
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecificed error!", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        break;
}
```

### <a name="step-2-handle-location-updates"></a>步驟 2：處理位置更新

[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 物件會觸發 [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 事件，以指出使用者的位置已變更或時間已推移 (視您的設定而定)。 該事件會透過引數的 **Position** 屬性 (類型為 [**Geoposition**](/uwp/api/Windows.Devices.Geolocation.Geoposition)) 傳遞對應的位置。 在這個範例中，此方法不是從 UI 執行緒呼叫，且 [**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 物件會叫用 UI 變更。

```csharp
using Windows.UI.Core;
...
async private void OnPositionChanged(Geolocator sender, PositionChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        UpdateLocationData(e.Position);
    });
}
```

## <a name="change-the-location-privacy-settings"></a>變更位置隱私權設定


如果位置隱私權設定不允許您的 app 存取使用者的位置，建議您提供一個可連到 \[**設定**\] app 中 \[**位置隱私權設定**\] 的便利連結。 在這個範例中，是使用「超連結」控制項來瀏覽至 `ms-settings:privacy-location` URI。

```xml
<!--Set Visibility to Visible when access to location is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

或者，您的 app 也可以呼叫 [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) 方法，以從程式碼啟動 \[**設定**\] app。 如需詳細資訊，請參閱[啟動 Windows 設定 app](../launch-resume/launch-settings-app.md)。

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="troubleshoot-your-app"></a>針對您的 app 進行疑難排解


必須先在裝置上啟用 \[**位置**\]，您的 app 才能存取使用者的位置。 在 \[**設定**\] app 中，確認已開啟下列 \[**位置隱私權設定**\]：

-   已將 **此裝置的位置** 設為 **開啟** \(不適用於 Windows 10 行動裝置版\)
-   已將定位服務設定的 \[**位置**\] 設為 \[**開啟**\]
-   在 \[**選擇可以使用您的位置的應用程式**\] 底下，將您的 app 設為 \[**開啟**\]

## <a name="related-topics"></a>相關主題

* [UWP 地理位置範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [地理柵欄的設計指導方針](./guidelines-for-geofencing.md)
* [定位感知應用程式的設計指導方針](./guidelines-and-checklist-for-detecting-location.md)
