---
author: mcleanbyron
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: "在 Windows 市集促銷 API 中使用此方法，管理適用於廣告行銷活動的廣告素材。"
title: "管理廣告行銷活動的廣告素材"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, Windows 市集促銷 API, 廣告行銷活動"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 1e0755134a47b6acfb48f735ea56c4aa3c46be14
ms.lasthandoff: 02/08/2017

---

# <a name="manage-creatives-for-ad-campaigns"></a>管理廣告行銷活動的廣告素材

在 Windows 市集促銷 API 使用這些方法，上傳您自己的自訂廣告素以用於廣告行銷活動，或取得現有的廣告素材。 廣告素材可能會與一個或多個廣告播送行相關聯，甚至跨廣告行銷活動，但前提是它永遠代表相同 app。

如需有關廣告素材和廣告行銷活動、播送行及目標設定檔之間關聯性的詳細資訊，請參閱[使用 Windows 市集服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)。

## <a name="prerequisites"></a>必要條件

若要使用這些方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集促銷交 API 的所有[先決條件](run-ad-campaigns-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這些方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這些方法具有下列 URI。

| 方法類型 | 要求 URI     |  描述  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  建立新廣告素材。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  取得 *creativeId* 指定的廣告素材。  |

>**注意**&nbsp;&nbsp;此 API 目前不支援 PUT 方法。

<span/> 
### <a name="header"></a>標頭

| 標頭        | 類型   | 描述         |
|---------------|--------|---------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |
| 追蹤識別碼   | GUID   | 選用。 追蹤呼叫流程的識別碼。                                  |


<span/>
### <a name="request-body"></a>要求本文

POST 方法需要 JSON 要求本文，以及[廣告素材](#creative)物件之必要欄位。

<span/>
### <a name="request-examples"></a>要求範例

以下範例示範如何呼叫 POST 方法，以建立廣告素材。 在此範例中，為了簡短而縮短 *content* 值。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

以下範例示範如何呼叫 GET 方法，以擷取廣告素材。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>
## <a name="response"></a>回應

下列方法傳回 JSON 回應本文及[廣告素材](#creative)物件，其包含所建立或擷取之廣告素材的相關資訊。 以下為範例示範這些方法的回應本文。 在此範例中，為了簡短而縮短 *content* 值。

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```

<span id="creative"/>
## <a name="creative-object"></a>廣告素材物件

下列方法的要求和回應本文包含下列欄位。 下表顯示的欄位為唯讀 (亦即們無法在 PUT 方法中變更)，而且僅 POST 方法之要求本文所需的欄位。

| 欄位        | 類型   |  描述      |  唯讀  | 預設值  |  POST 所需 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整數   |  廣告素材的識別碼。     |   是    |      |    否   |       
|  名稱   |  字串   |   廣告素材的名稱。    |    否   |      |  是     |       
|  內容   |  字串   |  廣告素材影像的內容，含 Base64 編碼格式。     |  否     |      |   是    |       
|  高度   |  整數   |   廣告素材的高度。    |    否    |      |   是    |       
|  寬度   |  整數   |  廣告素材的寬度。     |  否    |     |    是   |       
|  landingUrl   |  字串   |  廣告素材的登陸 URL (此值必須為有效的 URI)。     |  否    |     |   是    |       
|  格式   |  字串   |   廣告格式。 目前唯一支援的值為 **Banner**。    |   否    |  橫幅   |  否     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   提供廣告素材的屬性。     |   否    |      |   是    |       
|  storeProductId   |  字串   |   此廣告行銷活動所關聯之應用程式的[市集識別碼](in-app-purchases-and-trials.md#store-ids) 。 例如產品市集識別碼為 9nblggh42cfd。    |   否    |    |  否     |   |  

<span id="image-attributes"/>
## <a name="imageattributes-object"></a>ImageAttributes 物件

| 欄位        | 類型   |  描述      |  唯讀  | 預設值  | POST 所需 |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   字串  |   如 PNG 或 JPG 之類的圖檔副檔名。    |    否   |      |   是    |       |


## <a name="related-topics"></a>相關主題

* [使用 Windows 市集服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md)
* [管理廣告行銷活動](manage-ad-campaigns.md)
* [管理廣告行銷活動的廣告播送行](manage-delivery-lines-for-ad-campaigns.md)
* [管理廣告行銷活動的目標設定檔](manage-targeting-profiles-for-ad-campaigns.md)
* [取得廣告行銷活動績效資料](get-ad-campaign-performance-data.md)
