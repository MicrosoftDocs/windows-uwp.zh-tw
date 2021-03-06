---
description: 在 Microsoft Store 分析 API 中使用這個方法，即可取得 UWP 應用程式的匯總取得資料，以及透過 Xbox Developer 入口網站所內嵌的 Xbox One 遊戲 (XDP) 並可在 XDP 分析儀表板中取得。
title: 取得遊戲和應用程式的銷售資料
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, 廣告網路, 應用程式中繼資料
ms.localizationpriority: medium
ms.openlocfilehash: 371f855e644a990191cd8f8b9365553bc2df9693
ms.sourcegitcommit: 368753aea2792984857f6a57a22daed1035f1a33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349704"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>取得遊戲和應用程式的銷售資料 
在 Microsoft Store 分析 API 中使用這個方法，即可取得 UWP 應用程式的匯總取得資料，以及透過 Xbox Developer 入口網站所內嵌的 Xbox One 遊戲 (XDP) 並可在 XDP 分析儀表板中取得。 

> [!NOTE]
> 此 API 不會在10月 2016 1 日之前提供每日匯總資料。 

## <a name="prerequisites"></a>Prerequisites
若要使用這個方法，您必須先進行下列動作： 

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md)。 
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。 

## <a name="request"></a>要求
### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI |
| --- | --- |
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>要求標頭

| 標頭 | 類型 | 描述 |
| --- | --- | --- |
| 授權 | 字串 | 必要。 以持有人形式 Azure AD 存取權杖 `<token>` 。 |

### <a name="request-parameters"></a>要求參數

| 參數 | 類型 | 說明 | 必要 |
| --- | --- | --- | --- |
| applicationId | 字串 | 您正在擷取下載數資料之 Xbox One 遊戲的產品識別碼。 若要取得遊戲的產品識別碼，請在 XDP 分析程式中流覽至您的遊戲，然後從 URL 取出產品識別碼。 或者，如果您從「合作夥伴中心分析」報告下載您的收購資料，則會將產品識別碼包含在 tsv 檔案中。  | 是 |
| startDate | date | 要擷取下載數資料之日期範圍的開始日期。 預設值是目前的日期。  | 否 |
| endDate | date | 要擷取下載數資料之日期範圍的結束日期。 預設值是目前的日期。  | 否 |
| filter | 字串 | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 篩選 參數中的字串值必須由單引號括住。 例如，*filter=market eq 'US' and gender eq 'm'*。  <br/> 您可以在回應本文中指定下列欄位： <ul><li>**acquisitionType**</li><li>**年齡**</li><li>**storeClient**</li><li>**gender**</li><li>**市場**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | 否 |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：**day**、**week** 或 **month**。 如果沒有指定，則預設為 **天**。  | 否 |
| orderby | 字串 | 對每個下載數的結果資料值做出排序的陳述式。 語法為 *orderby = field [order]，field [order],...**Field* 參數可以是下列其中一個字串： <ul><li>**date**</li><li>**acquisitionType**</li><li>**年齡**</li><li>**storeClient**</li><li>**gender**</li><li>**市場**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> *order* 參數為選擇性，並可以是 **asc** 或 **desc**，以指定每個欄位的遞增或遞減順序。 預設值為 **asc**。 以下是範例 *orderby* 字串： *orderby = date，市*  | 否 |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位： <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**gender**</li><li>**市場**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 傳回的資料列將包含 *groupby* 參數中指定的欄位，以及下列項目： <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> *groupby* 參數可以搭配 aggregationLevel 參數使用。 例如： *&groupby = >agegroup，市場&aggregationLevel = 周*  | 否 |

### <a name="request-example"></a>要求範例
下列範例示範數個取得 Xbox One 遊戲範下載數資料的要求。 將 *applicationId* 值取代為遊戲的產品識別碼。  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>回應

### <a name="response-body"></a>回應本文
| 值 | 類型 | 描述 |
| --- | --- | --- |
| 值 | array | 內含遊戲的彙總下載數資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的＜[下載數數值](#acquisition-values)＞一節。 |
| TotalCount | integer | 查詢之資料結果的資料列總數。 |

### <a name="acquisition-values"></a>下載數數值 
*Value* 陣列中的元素包含下列值。 

| 值 | 類型 | 說明 |
| --- | --- | --- |
| date | 字串 | 下載數資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId | 字串 | 您正在擷取下載數資料之 Xbox One 遊戲的產品識別碼。 |
| applicationName | 字串 | 遊戲的顯示名稱。 |
| acquisitionType | 字串 | 其中一個下列字串，可指出收購類型：  <ul><li>**免費**</li><li>**試用版**</li><li>**已支付**</li><li>**促銷代碼**</li><li>**Iap**</li><li>**訂用帳戶 Iap**</li><li>**私人對象**</li><li>**預先訂購**</li><li>**Xbox Game Pass** (或 **Game Pass** 如果在 2018 年 3 月 23 之前查詢資料)</li><li>**磁碟**</li><li>**預付碼**</li><li>**依預付單計費**</li><li>**取消的前置順序**</li><li>**失敗的前置順序**</li></ul> |
| age | 字串 | 下列其中一個字串，這些字串表示進行下載的使用者的年齡層： <ul><li>**小於13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**大於55**</li><li>**Unknown**</li></ul> |
| deviceType | 字串 | 以下字串之一指定完成收購的裝置類型： <ul><li>**Pc**</li><li>**電話**</li><li>**主控台-Xbox One**</li><li>**主控台-Xbox 系列 X**</li><li>**IoT**</li><li>**伺服器**</li><li>**平板電腦**</li><li>**全息影像**</li><li>**Unknown**</li></ul> |
| gender | 字串 | 下列其中一個字串，這些字串詳列進行收購的使用者的性別： <ul><li>**m**</li><li>**f**</li><li>**Unknown**</li></ul> |
| market | 字串 | 發生下載之市場的 ISO 3166 國家/地區碼。 |
| osVersion | 字串 | 發生下載的 OS 版本。 適用於這種方法，這個值總是 **Windows 10**。 |
| paymentInstrumentType | 字串 | 下列其中一個字串，這些字串表示用於採集的付款指示： <ul><li>**信用卡**</li><li>**直接轉帳卡**</li><li>**推斷購買**</li><li>**MS 餘額**</li><li>**行動電信業者**</li><li>**線上銀行轉帳**</li><li>**PayPal**</li><li>**分段交易**</li><li>**預付碼兌換**</li><li>**已支付零金額**</li><li>**eWallet**</li><li>**Unknown**</li></ul> |
| sandboxId | 字串 | 為遊戲建立的沙箱 ID。 這可以是 **零售** 或私用的沙箱識別碼值。 |
| storeClient | 字串 | 下列其中一個表示收購發生的 Microsoft Store 版本之字串： <ul><li>**Windows Phone 市集 (用戶端)**</li><li>**Microsoft Store (用戶端)** (或 **Microsoft Store (用戶端)** 如果在 2018 年 3 月 23 之前查詢資料) </li><li>**Microsoft Store (網頁)** (或 **Microsoft Store (網頁)** 如果在 2018 年 3 月 23 之前查詢資料) </li><li>**Volume purchase by organizations**</li><li>**其他**</li></ul> |
| xboxTitleId | 字串 | 由 Xbox 開發人員入口網站 (XDP) 所指派用於已啟用 Xbox Live 遊戲的Xbox Live 標題 ID (以十六進位值表示)。 |
| acquisitionQuantity | 數目 | 指定彙總層級期間發生的下載數目。 |
| purchasePriceUSDAmount | 數目 | 由客戶為該收購的付費金額係按當月匯率換算為美元。 |
| purchaseTaxUSDAmount | 數目 | 適用於收購的稅額轉換為美元。 |
| localCurrencyCode | 字串 | 以合作夥伴中心帳戶的國家/地區為基礎的本地貨幣代碼。  |
| xboxProductId | 字串 | XDP 產品的 Xbox 產品識別碼（如果適用）。  |
| availabilityId | 字串 | XDP 產品的可用性識別碼（如果適用）。  |
| skuId | 字串 | XDP 產品的 SKU 識別碼（如果適用）。  |
| skuDisplayName  | 字串 | XDP 中的產品 SKU 顯示名稱（如果適用）。  |
| xboxParentProductId | 字串 | XDP 產品的 Xbox 父產品識別碼（如果適用）。  |
| parentProductName | 字串 | XDP 產品的父產品名稱（如果適用）。  |
| productTypeName | 字串 | XDP 產品的產品類型名稱（如果適用）。  |
| purchaseTaxType | 字串 | 從 XDP 購買產品的稅務類型（如果適用）。  |
| purchasePriceLocalAmount | 數目 | 從 XDP 購買產品的當地費用（如果適用）。  |
| purchaseTaxLocalAmount | 數目 | 從 XDP 購買產品的稅金區域金額（如果適用）。  |

### <a name="response-example"></a>回應範例
下列範例針對此要求示範範例 JSON 回應主體。 

```JSON
{ 
    "Value": [ 
        { 
            "date": "2019-01-15T01:00:00.0000000Z", 
            "applicationId": "9WZDNCRFHXHT", 
            "applicationName": null, 
            "acquisitionType": "Paid", 
            "age": null, 
            "deviceType": "Phone", 
            "gender": null, 
            "market": "US", 
            "osVersion": "Windows 10", 
            "paymentInstrumentType": null, 
            "sandboxId": "RETAIL", 
            "storeClient": "Microsoft Store (client)", 
            "xboxTitleId": null, 
            "localCurrencyCode": "USD", 
            "xboxProductId": null, 
            "availabilityId": "B42LRTSZ2MCJ", 
            "skuId": "0010", 
            "skuDisplayName": null, 
            "xboxParentProductId": null, 
            "parentProductName": null, 
            "productTypeName": "Game", 
            "purchaseTaxType": "TaxesNotIncluded", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 3.08, 
            "purchasePriceLocalAmount": 3.08, 
            "purchaseTaxUSDAmount": 0.09, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null,
    
    "TotalCount": 12221 
} 
```

 
