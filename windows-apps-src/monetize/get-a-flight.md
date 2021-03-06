---
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: 在 Microsoft Store 提交 API 中使用這個方法，以針對註冊至您合作夥伴中心帳戶的應用程式取得封裝航班的資料。
title: 取得套件正式發行前小眾測試版
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 正式發行前小眾測試版, 套件正式發行前小眾測試版
ms.localizationpriority: medium
ms.openlocfilehash: 3916328f2c48642596c6a0b11401f583957bc973
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172902"
---
# <a name="get-a-package-flight"></a>取得套件正式發行前小眾測試版

在 Microsoft Store 提交 API 中使用這個方法，以針對註冊至您合作夥伴中心帳戶的應用程式取得封裝航班的資料。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 說明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 包含您想要取得套件正式發行前小眾測試版之 App 的 Store 識別碼。 應用程式的 Store 識別碼可在合作夥伴中心中取得。  |
| flightId | 字串 | 必要。 要取得的套件正式發行前小眾測試版識別碼。 識別碼可從[建立套件正式發行前小眾測試版](create-a-flight.md)和[取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)要求的回應資料中取得。 針對在合作夥伴中心中建立的航班，此識別碼也可在合作夥伴中心中航班頁面的 URL 中取得。  |


### <a name="request-body"></a>Request body

不提供此方法的要求主體。

### <a name="request-example"></a>要求範例

下列範例示範如何針對 Store 識別碼值為 9WZDNCRD91MD 的 App 擷取識別碼為 43e448df-97c9-4a43-a0bc-2a445e736bcd 的套件正式發行前小眾測試版的相關資訊

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例示範成功呼叫這個方法的 JSON 回應本文。 如需回應本文中各個值的詳細資訊，請參閱下列各節。

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "lastPublishedFlightSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621086517"
  },
  "pendingFlightSubmission": {
    "id": "115292150462124364",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243647"
  },
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>回應本文

| 值      | 類型   | 說明                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | 字串  | 套件正式發行前小眾測試版的識別碼。 此值是由合作夥伴中心提供。  |
| friendlyName           | 字串  | 開發人員指定的套件正式發行前小眾測試版名稱。   |  
| lastPublishedFlightSubmission       | 物件 (object) | 此物件可提供套件正式發行前小眾測試版最後一個發佈之提交的相關資訊。 如需詳細資訊，請參閱下方的[提交物件](#submission_object)一節。  |
| pendingFlightSubmission        | 物件 (object)  |  此物件可提供套件正式發行前小眾測試版目前擱置中之提交的相關資訊。 如需詳細資訊，請參閱下方的[提交物件](#submission_object)一節。  |   
| groupIds           | array  | 此字串陣列包含與套件正式發行前小眾測試版相關聯的正式發行前小眾測試版群組的識別碼。 如需有關正式發行前小眾測試版群組的詳細資訊，請參閱[套件正式發行前小眾測試版](../publish/package-flights.md)。   |
| rankHigherThan           | 字串  | 排名位於目前套件正式發行前小眾測試版之下的套件正式發行前小眾測試版易記名稱。 如需有關正式發行前小眾測試版群組排名的詳細資訊，請參閱[套件正式發行前小眾測試版](../publish/package-flights.md)。  |


<span id="submission_object" />

### <a name="submission-object"></a>提交物件

回應內文中的 *lastPublishedFlightSubmission* 和 *pendingFlightSubmission* 值包含可提供套件正式發行前小眾測試版提交相關資源資訊的物件。 這些物件可以具有下列值：

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 字串  | 提交的識別碼。    |
| resourceLocation   | 字串  | 您可以附加到基底 `https://manage.devcenter.microsoft.com/v1.0/my/` 要求 URI 以抓取提交完整資料的相對路徑。               |


## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述     |
|--------|---------------------  |
| 400  | 要求無效。 |
| 404  | 找不到指定的套件正式發行前小眾測試版。   |   
| 409  | 應用程式會使用 [目前 Microsoft Store 提交 API 不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作夥伴中心功能。 |                                                                                                 


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [建立套件正式發行前小眾測試版](create-a-flight.md)
* [刪除套件正式發行前小眾測試版](delete-a-flight.md)