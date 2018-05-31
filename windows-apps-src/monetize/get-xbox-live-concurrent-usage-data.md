---
author: mcleanbyron
description: 在 Microsoft Store 分析 API 中使用此方法取得 Xbox Live 並行使用資料。
title: 取得 Xbox Live 並行使用資料
ms.author: mcleans
ms.date: 04/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、Microsoft Store 服務、Microsoft Store 分析 API、Xbox Live 分析、並行使用
ms.localizationpriority: medium
ms.openlocfilehash: b739c9ac3ce9fe4501ecaa1071df4fd3901484ab
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816243"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>取得 Xbox Live 並行使用資料


在 Microsoft Store 分析 API 中使用此方法可以取得有關在指定時間範圍內每分鐘、每小時或每天玩您的 [已啟用 Xbox Live 遊戲](../xbox-live/index.md) 的平均客戶數量的即時使用量資料 (延遲 5-15 分鐘)。 「Windows 開發人員中心」儀表板中的 [Xbox 分析報告](../publish/xbox-analytics-report.md)也有提供這項資訊。

> [!IMPORTANT]
> 此方法目前只支援由 [Microsoft 合作夥伴](../xbox-live/developer-program-overview.md#microsoft-partners) 發行或透過 [ID@Xbox 程式](../xbox-live/developer-program-overview.md#id) 提交的支援 Xbox Live 的遊戲。 它不會傳回透過 [Xbox Live 創作者計畫](../xbox-live/developer-program-overview.md#xbox-live-creators-program) 提交的遊戲的資料。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數


| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取 Xbox Live 並行使用資料之遊戲的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字串 | 指定要擷取 Xbox Live 分析資料類型的字串。 對於此方法，請指定 **並行**值。  |  是  |
| startDate | 日期 | 要擷取並行使用資料之日期範圍的開始日期。 有關預設行為，請參閱 *aggregationLevel* 說明。 |  否  |
| endDate | 日期 | 要擷取並行使用資料之日期範圍的結束日期。 有關預設行為，請參閱 *aggregationLevel* 說明。 |  否  |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：**分鐘**、**小時** 或  **天**。 如果沒有指定，則預設為 **天**。 <p/><p/>如果您不指定 *startDate* 或 *endDate*，回應主體預設如下： <ul><li>**分鐘**：可用資料的最後 60 個記錄。</li><li>**小時**：可用資料的最後 24 個記錄。</li><li>**天**：可用資料的最後 7 個記錄。</li></ul><p/>下列彙總層級對可以傳回的記錄數量有大小限制。 如果要求的時間範圍太大，記錄將會被截斷。 <ul><li>**分鐘**：高達 1440 個記錄 (24 小時的資料)。</li><li>**小時**：高達 720 個記錄 (30 天的資料)。</li><li>**天**：高達 60 個記錄 (60 天的資料)。</li></ul>  |  否  |


### <a name="request-example"></a>要求的範例

以下範例示範了用於已啟用 Xbox Live 遊戲取得並行使用資料的要求。 此要求會擷取 2018 年 2 月 1 日至 2018 年 2 月 2 日期間的每筆記錄資料。 將 *applicationId* 值以您遊戲的 Store 識別碼取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

回應主體包含物件的陣列，每個物件包含一組指定的分鐘、小時或天的並行使用資料。 每個物件包含下列值。

| 數值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 計數      | 數字  | 在指定的分鐘、小時或天內玩已啟用 Xbox Live 的客戶的平均數量。 <p/><p/>**注意**&nbsp;&nbsp; 值為 0 表示在指定的時間間隔內沒有並行使用者，或者在指定的時間間隔內為遊戲收集並行使用者資料時發生錯誤。 |
| 日期  | 字串 | 並行使用資料發生期間其詳列了分鐘、小時或天的日期。  |
| SeriesName | 字串    | 這總有 **UserConcurrency** 的值。 |


### <a name="response-example"></a>回應範例

下列範例示範使用於此要求的一個 JSON 回應主體範例，係依照記錄作資料彙整。

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得 Xbox Live 分析資料](get-xbox-live-analytics.md)
* [取得 Xbox Live 成就資料](get-xbox-live-achievements-data.md)
* [取得 Xbox Live 健康情況資料](get-xbox-live-health-data.md)
* [取得 Xbox Live 遊戲中心資料](get-xbox-live-game-hub-data.md)
* [取得 Xbox Live 俱樂部資料](get-xbox-live-club-data.md)
* [取得 Xbox Live 多人遊戲資料](get-xbox-live-multiplayer-data.md)