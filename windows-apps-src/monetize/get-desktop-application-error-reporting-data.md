---
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得傳統型應用程式的彙總錯誤報告資料。
title: 取得傳統型應用程式的錯誤報告資料
ms.date: 09/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 錯誤, 傳統型應用程式
ms.localizationpriority: medium
ms.openlocfilehash: bb9c0bd3162f603bd9965a98c5e75bad5aae9c54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167592"
---
# <a name="get-error-reporting-data-for-your-desktop-application"></a>取得傳統型應用程式的錯誤報告資料

在 Microsoft Store 分析 API 使用此方法，取得傳統型應用程式之彙總錯誤報告資料，而您已將其加入到 [Windows 傳統型應用程式](/windows/desktop/appxpkg/windows-desktop-application-program)。 此方法只能捕獲過去30天內發生的錯誤。 合作夥伴中心中的桌面應用程式 [健康情況報告](/windows/desktop/appxpkg/windows-desktop-application-program) 也提供這項資訊。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits``` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  說明      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取其錯誤報告資料的傳統型應用程式的產品識別碼。 若要取得傳統型應用程式的產品識別碼，請 [在合作夥伴中心 (中開啟桌面應用程式的任何分析報表](/windows/desktop/appxpkg/windows-desktop-application-program) ，例如 **健康情況報表**) ，並從 URL 取出產品識別碼。 |  是  |
| startDate | date | 要擷取錯誤報告資料之日期範圍的開始日期，格式為 ```mm/dd/yyyy```。 預設值是目前的日期。<p/><p/>**注意：** &nbsp; &nbsp;此方法只能捕獲過去30天內發生的錯誤。  |  否  |
| endDate | date | 要擷取錯誤報告資料之日期範圍的結束日期，格式為 ```mm/dd/yyyy```。 預設值是目前的日期。   |  否  |
| top | int | 在要求中傳回的資料列數目。 如果未指定，最大值和預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | int | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter |字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *篩選* 參數中的字串值必須由單引號括住。 您可以在回應本文中指定下列欄位：<p/><ul><li><strong>檔案名</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>象徵</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>市場</strong></li><li><strong>deviceType</strong></li><li><strong>名稱</strong></li><li><strong>date</strong></li></ul> | 否   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：**day**、**week** 或 **month**。 如果沒有指定，則預設為 **天**。 如果您指定 **week** 或 **month**，*failureName* 和 *failureHash* 值將會被限制在 1000 個值區。<p/>  | 否 |
| orderby | 字串 | 將結果資料值排序的陳述式。 語法為 *orderby=field [order],field [order],...*，其中 *field* 參數可以是下列其中一個字串︰<ul><li><strong>檔案名</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>象徵</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>市場</strong></li><li><strong>deviceType</strong></li><li><strong>名稱</strong></li><li><strong>date</strong></li></ul>*order* 參數為選擇性，並可以是 **asc** 或 **desc**，以指定每個欄位的遞增或遞減順序。 預設值為 **asc**。</p><p>以下是範例 *orderby* 字串： *orderby = date，市*</p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li>**failureName**</li><li>**failureHash**</li><li>**象徵**</li><li>**osVersion**</li><li>**eventType**</li><li>**市場**</li><li>**deviceType**</li></ul><p>傳回的資料列將包含 *groupby* 參數中指定的欄位，以及下列項目：</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**eventCount**</li></ul><p>*groupby* 參數可以搭配 *aggregationLevel* 參數使用。 例如： * &amp; Groupby = failureName、市場 &amp; aggregationLevel = 周*</p></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範取得錯誤報告資料的數個要求。 將 *applicationId* 值取代為您傳統型應用程式的產品識別碼。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=1/1/2018&endDate=2/1/2018&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應本文

| 值      | 類型    | 描述     |
|------------|---------|--------------|
| 值      | array   | 內含彙總錯誤報告資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[錯誤數值](#error-values)一節。     |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的錯誤，就會傳回此值。 |
| TotalCount | integer | 查詢之資料結果的資料列總數。     |


### <a name="error-values"></a>錯誤值

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 說明        |
|-----------------|---------|---------------------|
| date            | 字串  | 錯誤資料之日期範圍中的第一個日期，格式為 ```yyyy-mm-dd```。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一個較長的日期範圍，此值便會是該日期範圍的第一個日期。 對於指定 **hour** 的 *aggregationLevel* 值，這個值也包括 ```hh:mm:ss``` 格式的時間。  |
| applicationId   | 字串  | 您已擷取錯誤資料的傳統型應用程式的產品識別碼值。    |
| productName | 字串  | 從相關聯的可執行檔中繼資料衍生的傳統型應用程式的顯示名稱。   |
| appName | 字串  |  TBD  |
| fileName | 字串  | 傳統型應用程式的可執行檔名稱。   |
| failureName     | 字串  | 失敗的名稱，由四個部分組成：一個或多個問題類別、例外狀況/錯誤檢查碼、失敗發生位置映像名稱，以及相關函式名稱。  |
| failureHash     | 字串  | 錯誤的唯一識別碼。   |
| 符號          | 字串  | 指派給此錯誤的符號。 |
| osBuild       | 字串  | 發生錯誤之 OS 的四個部分組建號碼。  |
| osVersion       | 字串  | 下列其中一個字串，指定傳統型應用程式安裝所在的作業系統版本：<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>   |   
| osRelease | 字串  | 下列其中一個字串指定的 OS 版本或正式發行前小眾測試頻道 (為 OS 版本內的次群族) 上發生的錯誤。<p/><p>適用於 Windows 10：</p><ul><li><strong>版本1507</strong></li><li><strong>版本 1511</strong></li><li><strong>版本1607</strong></li><li><strong>版本1703</strong></li><li><strong>版本1709</strong></li><li><strong>版本1803</strong></li><li><strong>發行預覽</strong></li><li><strong>測試人員 - 快</strong></li><li><strong>測試人員 - 慢</strong></li></ul><p/><p>若是 Windows Server 1709：</p><ul><li><strong>Rtm</strong></li></ul><p>若是 Windows Server 2016：</p><ul><li><strong>版本1607</strong></li></ul><p>適用於 Windows 8.1：</p><ul><li><strong>Update 1</strong></li></ul><p>適用於 Windows 7：</p><ul><li><strong>Service Pack 1</strong></li></ul><p>如果 OS 版本或正式發行前小眾測試通道不明，此欄位會有<strong>不明</strong>值。</p> |
| eventType       | 字串  | 其中一個下列字串，可指出錯誤事件的類型：<ul><li>**崩潰**</li><li>**hang**</li><li>**記憶**</li><li>**jse**</li></ul>       |
| market          | 字串  | 裝置市場的 ISO 3166 國家/地區碼。   |
| deviceType      | 字串  | 下列其中一個字串，指定發生錯誤的裝置類型：<p/><ul><li><strong>Pc</strong></li><li><strong>Server</strong></li><li><strong>平板電腦</strong></li><li><strong>Unknown</strong></li></ul>    |
| applicationVersion     | 字串  |   發生錯誤之應用程式可執行的版本。    |
| eventCount      | number | 針對指定的彙總層級被歸類到此錯誤的事件數目。      |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2018-02-01",
      "applicationId": "10238467886765136388",
      "productName": "Contoso Demo",
      "appName": "Contoso Demo",
      "fileName": "contosodemo.exe",
      "failureName": "SVCHOSTGROUP_localservice_IN_PAGE_ERROR_c0000006_hardware_disk!Unknown",
      "failureHash": "11242ef3-ebd8-d525-838d-b5497b225695",
      "symbol": "hardware_disk!Unknown",
      "osBuild": "10.0.15063.850",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "applicationVersion": "2.2.2.0",
      "eventCount": 0.0012422360248447205
    }
  ],
  "@nextLink": "desktop/failurehits?applicationId=10238467886765136388&aggregationLevel=week&startDate=2018/02/01&endDate2018/02/08&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>相關主題

* [健康情況報告](../publish/health-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)
* [取得傳統型應用程式中錯誤的堆疊追蹤](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [下載傳統型應用程式中錯誤的 CAB 檔案](download-the-cab-file-for-an-error-in-your-desktop-application.md)