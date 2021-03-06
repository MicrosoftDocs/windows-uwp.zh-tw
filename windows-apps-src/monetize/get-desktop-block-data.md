---
description: 使用此 REST URI 可在指定的日期範圍和其他選擇性篩選準則下，取得桌面應用程式的區塊資料。
title: 取得傳統型應用程式的升級區塊
ms.date: 07/11/2018
ms.topic: article
keywords: windows 10、桌面應用程式區塊、Windows 傳統型應用程式計畫
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 65cb156d313f398517e174b12520411d5586e22c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158552"
---
# <a name="get-upgrade-blocks-for-your-desktop-application"></a>取得傳統型應用程式的升級區塊

使用此 REST URI 可取得您的桌面應用程式封鎖執行 Windows 10 升級之 Windows 10 裝置的相關資訊。 您只能針對已新增至 [Windows 桌面應用](/windows/desktop/appxpkg/windows-desktop-application-program)程式的桌面應用程式使用此 URI。 這項資訊也可在合作夥伴中心的桌面應用程式的 [ [應用程式區塊] 報表](/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) 中取得。

若要在桌面應用程式中取得特定可執行檔的裝置區塊詳細資料，請參閱 [取得桌面應用程式的升級區塊詳細](get-desktop-block-data-details.md)資料。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits```|


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  說明      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您要用來取得區塊資料之桌面應用程式的產品識別碼。 若要取得傳統型應用程式的產品識別碼，請 [在合作夥伴中心 (中開啟桌面應用程式的任何分析報表](/windows/desktop/appxpkg/windows-desktop-application-program) ，例如「 **區塊」報表**) ，並從 URL 取出產品識別碼。 |  是  |
| startDate | date | 要取出之區塊資料的日期範圍內的開始日期。 預設值為目前日期前的90天。 |  否  |
| endDate | date | 要取出之區塊資料的日期範圍中的結束日期。 預設值是目前的日期。 |  否  |
| top | int | 在要求中傳回的資料列數目。 如果未指定，最大值和預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | int | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter | 字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *篩選* 參數中的字串值必須由單引號括住。 您可以在回應本文中指定下列欄位：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>建築</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>檔案名</strong></li><li><strong>市場</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>名稱</strong></li><li><strong>targetOs</strong></li></ul> | 否   |
| orderby | 字串 | 排序每個區塊之結果資料值的語句。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列來自回應本文欄位的其中一個︰<p/><ul><li><strong>applicationVersion</strong></li><li><strong>建築</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>檔案名</strong></li><li><strong>市場</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>名稱</strong></li><li><strong>targetOs</strong></li><li><strong>deviceCount</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設值為 <strong>asc</strong>。</p><p>以下是範例 <em>orderby</em> 字串： <em>orderby = date，市</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以在回應本文中指定下列欄位：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>建築</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>檔案名</strong></li><li><strong>市場</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>名稱</strong></li><li><strong>deviceCount</strong></li></ul></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範一些取得桌面應用程式區塊資料的要求。 將 *applicationId* 值取代為桌面應用程式的產品識別碼。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應本文

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 包含匯總區塊資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下表。 |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數設定為10000，但查詢有超過10000個數據列的資料列，就會傳回此值。 |
| TotalCount | int    | 查詢之資料結果的資料列總數。 |


*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 說明                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字串 | 您已抓取區塊資料的桌面應用程式產品識別碼。 |
| date                | 字串 | 與區塊點擊值相關聯的日期。 |
| productName         | 字串 | 從相關聯的可執行檔中繼資料衍生的傳統型應用程式的顯示名稱。 |
| fileName            | 字串 | 已封鎖的可執行檔。 |
| applicationVersion  | 字串 | 已封鎖的應用程式可執行檔版本。 |
| osVersion           | 字串 | 下列其中一個字串指定桌面應用程式目前執行所在的作業系統版本：<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>   |
| osRelease           | 字串 | 下列其中一個字串指定作業系統版本或試驗環形 (為桌面應用程式目前執行所在) 作業系統版本內的 subpopulation。<p/><p>適用於 Windows 10：</p><ul><li><strong>版本1507</strong></li><li><strong>版本 1511</strong></li><li><strong>版本1607</strong></li><li><strong>版本1703</strong></li><li><strong>版本1709</strong></li><li><strong>發行預覽</strong></li><li><strong>測試人員 - 快</strong></li><li><strong>測試人員 - 慢</strong></li></ul><p/><p>若是 Windows Server 1709：</p><ul><li><strong>Rtm</strong></li></ul><p>若是 Windows Server 2016：</p><ul><li><strong>版本1607</strong></li></ul><p>適用於 Windows 8.1：</p><ul><li><strong>Update 1</strong></li></ul><p>適用於 Windows 7：</p><ul><li><strong>Service Pack 1</strong></li></ul><p>如果 OS 版本或正式發行前小眾測試通道不明，此欄位會有<strong>不明</strong>值。</p> |
| market              | 字串 | 桌面應用程式被封鎖的市場的 ISO 3166 國家/地區代碼。 |
| deviceType          | 字串 | 下列其中一個字串，指定封鎖桌面應用程式的裝置類型：<p/><ul><li><strong>Pc</strong></li><li><strong>Server</strong></li><li><strong>平板電腦</strong></li><li><strong>Unknown</strong></li></ul> |
| blockType            | 字串 | 下列其中一個字串指定在裝置上找到的區塊類型：<p/><ul><li><strong>可能的 Sediment</strong></li><li><strong>暫存 Sediment</strong></li><li><strong>執行時間通知</strong></li></ul><p/><p/>如需這些區塊類型以及它們對開發人員和使用者之意義的詳細資訊，請參閱 [應用程式區塊報表](/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report)的描述。  |
| 架構        | 字串 | 區塊所在裝置的架構： <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | 字串 | 下列其中一個字串，指定已封鎖桌面應用程式執行所在的 Windows 10 OS 版本： <p/><ul><li><strong>版本1709</strong></li><li><strong>版本1803</strong></li></ul> |
| deviceCount         | number | 在指定的匯總層級具有區塊的相異裝置數目。 |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
     "applicationId": "10238467886765136388",
     "date": "2018-06-03",
     "productName": "Contoso Demo",
     "fileName": "contosodemo.exe",
     "applicationVersion": "2.2.2.0",
     "osVersion": "Windows 8.1",
     "osRelease": "Update 1",
     "market": "ZA",
     "deviceType": "All",
     "blockType": "Runtime Notification",
     "architecture": "X86",
     "targetOs": "RS4",
     "deviceCount": 120
    }
  ],
  "@nextLink": "desktop/blockhits?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>相關主題

* [Windows 桌面應用程式](/windows/desktop/appxpkg/windows-desktop-application-program)
* [取得傳統型應用程式的升級區塊詳細資料](get-desktop-block-data-details.md)