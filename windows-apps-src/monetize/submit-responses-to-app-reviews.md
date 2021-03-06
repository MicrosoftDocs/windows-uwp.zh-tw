---
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: 使用 Microsoft Store 評論 API 中的此方法，針對您 app 的評論提交回應。
title: 提交評論的回應
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store 服務, Microsoft Store 評論 API, 附加元件下載數
ms.localizationpriority: medium
ms.openlocfilehash: 9cf62f5f619eba0431f1398a391eef03b47bb13d
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984634"
---
# <a name="submit-responses-to-reviews"></a>提交評論的回應


使用 Microsoft Store 評論 API 中的此方法，以程式設計方式回應您 app 的評論。 當您呼叫這個方法時，您必須指定您想要回應的評論的識別碼。 評論識別碼是在 Microsoft Store 分析 API [取得 app 評論](get-app-reviews.md)方法的回應資料中，以及[評論報告](../publish/reviews-report.md)的[離線下載](../publish/download-analytic-reports.md)中。

當客戶提交評論時，他們可以選擇不接收評論的回應。 如果您嘗試回應到客戶選擇不收到回應的評論，這個方法的回應主體會指出回應嘗試已失敗。 呼叫這個方法之前，您可以使用[取得應用程式評論的回應資訊](get-response-info-for-app-reviews.md)方法，選擇判斷是否允許您回應特定評論。

> [!NOTE]
> 除了使用這個方法以程式設計方式回應評論，您也可以 [使用合作夥伴中心](../publish/respond-to-customer-reviews.md)來回應評論。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 評論 API 的所有[先決條件](respond-to-reviews-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。
* 取得您想要回應之評論的識別碼。 評論識別碼是在 Microsoft Store 分析 API [取得 app 評論](get-app-reviews.md)方法的回應資料中，以及[評論報告](../publish/reviews-report.md)的[離線下載](../publish/download-analytic-reports.md)中。

## <a name="request"></a>要求

### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

這個方法沒有任何要求參數。


### <a name="request-body"></a>Request body

要求主體包含下列值。

| 值        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------|
| 回應 | array | 物件陣列，包含您想要提交的回應資料。 如需有關每個物件中資料的詳細資訊，請參閱下表。 |


*Responses* 陣列中的每個物件包含下列值。

| 值        | 類型   | 描述           |  必要  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | 字串 |  您想要回應評論之 App 的「Store 識別碼」。 Store 識別碼可在合作夥伴中心的 [ [應用程式識別] 頁面](../publish/view-app-identity-details.md) 上取得。 舉例來說，Store 識別碼可以是「9WZDNCRFJ3Q8」。   |  是  |
| ReviewId | 字串 |  您想要回應評論的識別碼 (這是 GUID)。 評論識別碼是在 Microsoft Store 分析 API [取得 app 評論](get-app-reviews.md)方法的回應資料中，以及[評論報告](../publish/reviews-report.md)的[離線下載](../publish/download-analytic-reports.md)中。   |  是  |
| ResponseText | 字串 | 您想要提交的回應。 您的回應必須依照[這些指導方針](../publish/respond-to-customer-reviews.md#guidelines-for-responses)。   |  是  |
| SupportEmail | 字串 | 您 app 的支援電子郵件地址，客戶可以用來直接連絡您。 這必須是有效的電子郵件地址。     |  是  |
| IsPublic | 布林值 |  如果您指定 **true**，您的回應會顯示在應用程式的 Store 清單中，緊接在客戶的評論正下方，而且所有客戶都可以看到。 如果您指定 **false** ，而使用者未選擇不接受接收電子郵件回應，則您的回應將會透過電子郵件傳送給客戶，而在您應用程式的 Store 清單中，其他客戶將看不到它。 如果您指定 **false** ，而使用者選擇不接收電子郵件回應，則會傳回錯誤。   |  是  |


### <a name="request-example"></a>要求範例

以下範例示範如何使用此方法，針對幾個評論提交回應。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "Responses": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "ResponseText": "Thank you for pointing out this bug. I fixed it and published an update, you should have the fix soon",
      "SupportEmail": "support@contoso.com",
      "IsPublic": true
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": false
    }
  ]
}
```

## <a name="response"></a>回應

### <a name="response-body"></a>回應本文

| 值        | 類型   | 描述            |
|---------------|--------|---------------------|
| 結果 | array | 物件陣列，包含您所送出每個回覆的相關資料。 如需有關每個物件中資料的詳細資訊，請參閱下表。  |


*Result* 陣列中的每個物件包含下列值。

| 值        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | 字串 |  您已回應評論之 App 的「Store 識別碼」。 舉例來說，Store 識別碼可以是「9WZDNCRFJ3Q8」。   |
| ReviewId | 字串 |  您已回應之評論的識別碼。 這是 GUID。   |
| 成功 | 字串 | 值 **true** 指出已成功傳送您的回應。 值 **false** 表示您的回應未成功。    |
| FailureReason | 字串 | 如果 **Successful** 是 **false**，這個值包含失敗原因。 如果 **Successful** 是 **true**，這個值是空的。      |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Result": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "Successful": "true",
      "FailureReason": ""
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "Successful": "false",
      "FailureReason": "No Permission"
    }
  ]
}
```

## <a name="related-topics"></a>相關主題

* [使用合作夥伴中心回應客戶評論](../publish/respond-to-customer-reviews.md)
* [使用 Microsoft Store 服務回應評論](respond-to-reviews-using-windows-store-services.md)
* [取得應用程式評論的回應資訊](get-response-info-for-app-reviews.md)
* [取得應用程式評論](get-app-reviews.md)
