---
description: 使用「Microsoft Store 提交 API」中的這個方法，來取得套件正式發行前小眾測試版提交的套件推出資訊。
title: 取得正式發行前小眾測試版提交的推出資訊
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 套件推出, 正式發行前小眾測試版提交
ms.assetid: 397f1b99-2be7-4f65-bcf1-9433a3d496ad
ms.localizationpriority: medium
ms.openlocfilehash: 5f8e39650c5b0d9e9124ed671613e7a2f8e77ed2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171412"
---
# <a name="get-rollout-info-for-a-flight-submission"></a>取得正式發行前小眾測試版提交的推出資訊


使用「Microsoft Store 提交 API」中的這個方法，來取得套件正式發行前小眾測試版提交的[套件推出](../publish/gradual-package-rollout.md)資訊。 如需使用 Microsoft Store 提交 API 建立套件正式發行前小眾測試版提交程序的詳細資訊，請參閱[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。
* 為您的其中一個應用程式建立套件航班提交。 您可以在合作夥伴中心中進行這項作業，也可以使用 [建立封裝航班提交](create-a-flight-submission.md) 方法來達到此目的。

## <a name="request"></a>要求

這個方法的語法如下。 如需使用方式範例及標頭和要求參數的描述，請參閱下列各節。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout   ` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 說明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 App 的「Store 識別碼」，此 App 包含您想要取得其套件推出資訊的套件正式發行前小眾測試版提交。 如需有關 Store 識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](../publish/view-app-identity-details.md)。  |
| flightId | 字串 | 必要。 套件正式發行前小眾測試版的識別碼，此正式發行前小眾測試版包含您想要取得其套件推出資訊的提交。 識別碼可從[建立套件正式發行前小眾測試版](create-a-flight.md)和[取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)要求的回應資料中取得。 針對在合作夥伴中心中建立的航班，此識別碼也可在合作夥伴中心中航班頁面的 URL 中取得。    |
| submissionId | 字串 | 必要。 要取得其套件推出資訊之提交的識別碼。 在[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)要求的回應資料中有提供此識別碼。 針對合作夥伴中心中建立的提交，此識別碼也可在 [提交] 頁面的 URL 中找到合作夥伴中心。   |


### <a name="request-body"></a>Request body

不提供此方法的要求主體。

### <a name="request-example"></a>要求範例

下列範例示範如何取得套件正式發行前小眾測試版提交的套件推出資訊。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/packagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例針對已啟用漸進式套件推出功能的套件正式發行前小眾測試版提交，示範成功呼叫這個方法時的 JSON 回應主體。 如需有關回應主體中各個值的詳細資訊，請參閱[套件推出資源](manage-flight-submissions.md#package-rollout-object)。

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

如果套件正式發行前小眾測試版提交未啟用漸進式套件推出功能，則會傳回以下回應主體。

```json
{
    "isPackageRollout": false,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutNotStarted",
    "fallbackSubmissionId": "0"
}
```

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到套件正式發行前小眾測試版。 |
| 409  | 封裝航班提交不屬於指定的封裝航班，或應用程式使用 [Microsoft Store 提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作夥伴中心功能。 |   


## <a name="related-topics"></a>相關主題

* [漸進式套件推出](../publish/gradual-package-rollout.md)
* [使用 Microsoft Store 提交 API 管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)
* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)