---
ms.assetid: 1A69A388-B1CC-4D2C-886B-EA07E6E60252
description: 使用 Microsoft Store 提交 API 中的這個方法，刪除現有的套件正式發行前小眾測試版提交。
title: 刪除套件正式發行前小眾測試版提交
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 正式發行前小眾測試版提交, 刪除, 套件正式發行前小眾測試版
ms.localizationpriority: medium
ms.openlocfilehash: 9cfac0958e5f4b4175fafe1d22a42b2b6eae08c2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171652"
---
# <a name="delete-a-package-flight-submission"></a>刪除套件正式發行前小眾測試版提交

使用 Microsoft Store 提交 API 中的這個方法，刪除現有的套件正式發行前小眾測試版提交。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| 刪除    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/flights/{flightId}/submissions/{submissionId}` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 說明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 包含您想要刪除套件正式發行前小眾測試版提交之 App 的 Store 識別碼。 如需有關 Store 識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](../publish/view-app-identity-details.md)。  |
| flightId | 字串 | 必要。 包含要刪除提交之套件正式發行前小眾測試版的識別碼。 識別碼可從[建立套件正式發行前小眾測試版](create-a-flight.md)和[取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)要求的回應資料中取得。 針對在合作夥伴中心中建立的航班，此識別碼也可在合作夥伴中心中航班頁面的 URL 中取得。  |
| submissionId | 字串 | 必要。 要刪除之提交的識別碼。 在[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)要求的回應資料中有提供此識別碼。 針對合作夥伴中心中建立的提交，此識別碼也可在 [提交] 頁面的 URL 中找到合作夥伴中心。  |


### <a name="request-body"></a>Request body

不提供此方法的要求主體。


### <a name="request-example"></a>要求範例

下列範例示範如何刪除套件正式發行前小眾測試版提交。

```json
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

如果成功，此方法會傳回空白回應主體。

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 要求參數無效。 |
| 404  | 找不到指定的提交。 |
| 409  | 找到指定的提交，但無法刪除其目前的狀態，或是應用程式使用 [Microsoft Store 提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作夥伴中心功能。 |


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)
* [取得套件正式發行前小眾測試版提交](get-a-flight-submission.md)
* [建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)
* [認可套件正式發行前小眾測試版提交](commit-a-flight-submission.md)
* [更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)
* [取得套件正式發行前小眾測試版提交的狀態](get-status-for-a-flight-submission.md)