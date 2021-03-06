---
description: 在 Microsoft Store 分析 API 中使用此方法，以下載適傳統型應用程式中錯誤的 CAB 檔案。
title: 下載傳統型應用程式中錯誤的 CAB 檔案
ms.date: 03/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 分析 API, 下載 CAB, 傳統型應用程式
ms.localizationpriority: medium
ms.openlocfilehash: 98a0d6931a59e037bb15679a20fae11eb82dae32
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162492"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>下載傳統型應用程式中錯誤的 CAB 檔案

在 Microsoft Store 分析 API 使用此方法，下載與傳統型應用程式之特殊錯誤相關聯的 CAB 檔案，而您已將其加入到 [Windows 傳統型應用程式](/windows/desktop/appxpkg/windows-desktop-application-program)。 這個方法只可以下載最近 30 天發生之應用程式錯誤的 CAB 檔案。 合作夥伴中心的桌面應用程式 [健康情況報告](/windows/desktop/appxpkg/windows-desktop-application-program) 也提供 CAB 檔案下載。

使用此方法之前，您必須先使用[取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)方法，擷取要下載之 CAB 檔案的識別碼雜湊。

## <a name="prerequisites"></a>先決條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。
* 取得要下載之 CAB 檔案的識別碼雜湊。 若要取得此值，請使用[取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)方法，以擷取您的 App 中特定錯誤的詳細資料，並在該方法的回應主體中使用 **cabIdHash** 值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  說明      |  必要  |
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要下載 CAB 檔案之傳統型應用程式的產品識別碼。 若要取得傳統型應用程式的產品識別碼，請 [為您的桌面應用 (程式開啟任何合作夥伴中心分析報表](/windows/desktop/appxpkg/windows-desktop-application-program) ，例如 **健康情況報告**) 然後從 URL 取出產品識別碼。 |  是  |
| cabIdHash | 字串 | 要下載之 CAB 檔案的唯一識別碼雜湊。 若要取得此值，請使用[取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)方法，以擷取您的應用程式中特定錯誤的詳細資料，並在該方法的回應主體中使用 **cabIdHash** 值。 |  是  |


### <a name="request-example"></a>要求範例

下列範例示範如何使用此方法下載 CAB 檔案。 將 *applicationId* 和 *cabIdHash* 參數以傳統型應用程式的適當值取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

這個方法傳回 302（重新導向）回應碼，而且回應中的 **Location** 標頭會指派給 CAB 檔案的共用存取簽章 (SAS) URI。 呼叫端會重新導向至此 URI，以自動下載 CAB 檔案。

## <a name="related-topics"></a>相關主題

* [健康情況報告](../publish/health-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得傳統型應用程式的錯誤報告資料](get-desktop-application-error-reporting-data.md)
* [取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)
* [取得傳統型應用程式中錯誤的堆疊追蹤](get-the-stack-trace-for-an-error-in-your-desktop-application.md)