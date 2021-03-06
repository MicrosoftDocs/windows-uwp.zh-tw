---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: 使用 Microsoft Store 促銷 API，以程式設計方式管理向您或您組織的合作夥伴中心帳戶註冊之應用程式的促銷廣告行銷活動。
title: 使用Microsoft Store 服務執行廣告行銷活動
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 促銷 API, 廣告行銷活動
ms.localizationpriority: medium
ms.openlocfilehash: 2be721137e6c09913eafd2c58bab07f1ae6f2728
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878511"
---
# <a name="run-ad-campaigns-using-store-services"></a>使用Microsoft Store 服務執行廣告行銷活動

使用 *Microsoft Store 促銷 API* ，以程式設計方式管理向您或您組織的合作夥伴中心帳戶註冊之應用程式的促銷廣告行銷活動。 這個 API 可讓您用來建立、更新及監視您的行銷活動，以及其他相關資產，例如目標和廣告素材。 此 API 特別適用于建立大量活動的開發人員，以及想要在不使用合作夥伴中心的情況下這麼做的開發人員。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您 App 或服務的呼叫。

下列步驟說明端對端的程序：

1.  請確定您已經完成所有[先決條件](#prerequisites)。
2.  在呼叫 Microsoft Store 促銷 API 中的方法之前，請先[取得 Azure AD 存取權杖](#obtain-an-azure-ad-access-token)。 取得權杖之後，在權杖到期之前，您有 60 分鐘的時間可以使用這個權杖呼叫 Microsoft Store 促銷 API。 權杖到期之後，您可以產生新的權杖。
3.  [呼叫 Microsoft Store 促銷 API](#call-the-windows-store-promotions-api)。

您也可以使用合作夥伴中心建立和管理 ad 活動，您也可以在合作夥伴中心中存取透過 Microsoft Store 促銷 API 以程式設計方式建立的任何廣告活動。 如需在合作夥伴中心中管理 ad 活動的詳細資訊，請參閱為 [您的應用程式建立 ad 行銷活動](./index.md)。

> [!NOTE]
> 具有合作夥伴中心帳戶的任何開發人員都可以使用 Microsoft Store 促銷 API 來管理其應用程式的廣告活動。 媒體代理商也可要求存取這個 API，代表廣告客戶執行廣告行銷活動。 如果您是想要深入了解或要求存取這個 API 的媒體代理商，請將您的要求傳送至 storepromotionsapi@microsoft.com。

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>步驟 1︰完成使用 Microsoft Store 促銷 API 的先決條件

開始撰寫程式碼以呼叫 Microsoft Store 促銷 API 之前，請先確定您已完成下列先決條件。

* 您必須先 [使用合作夥伴中心中的 [ **ad 活動** ] 頁面建立一個付費的 ad 活動](./index.md)，然後在此頁面上至少新增一個付款條件，您才能使用此 API 成功建立和啟動 ad 活動。 執行此動作之後，您就可以使用此 API 成功地建立廣告行銷活動的可計費廣告播送行。 您使用此 API 建立的 ad 活動傳遞行，會自動為合作夥伴中心中的 [ **ad 活動** ] 頁面上選擇的預設付款條件產生帳單。

* 您 (或您的組織) 必須具有 Azure AD 目錄，且您必須具備該目錄的[全域管理員](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) \(部分機器翻譯\) 權限。 如果您已經使用 Microsoft 365 或 Microsoft 的其他商務服務，則您已經有 Azure AD 目錄。 否則，您可以 [在合作夥伴中心中建立新的 Azure AD](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) ，而不需要額外收費。

* 您必須將 Azure AD 應用程式與您的合作夥伴中心帳戶建立關聯、取出應用程式的租使用者識別碼和用戶端識別碼，並產生金鑰。 Azure AD 應用程式代表您要呼叫 Microsoft Store 促銷 API 的 App 或服務。 您需要租用戶識別碼、用戶端識別碼及金鑰，以取得您會傳遞給 API 的 Azure AD 存取權杖。
    > [!NOTE]
    > 您只需要執行此工作一次。 在您取得租用戶識別碼、用戶端識別碼及金鑰之後，您便可以在未來需要建立新的 Azure AD 存取權杖時重複加以使用。

若要將 Azure AD 應用程式與您的合作夥伴中心帳戶建立關聯，並取出所需的值：

1.  在合作夥伴中心中，[將您組織的合作夥伴中心帳戶與您組織的 Azure AD 目錄建立關聯](../publish/associate-azure-ad-with-partner-center.md) \(部分機器翻譯\)。

2.  接下來，在合作夥伴中心的 [**帳戶設定**] 區段中的 [**使用者**] 頁面上，新增代表您將用來管理合作夥伴中心帳戶促銷活動之應用程式或服務的[Azure AD 應用程式](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account)。 確定您將此應用程式指派為 [管理員] 角色。 如果該應用程式尚未存在於您的 Azure AD 目錄，您可以[在合作夥伴中心中建立新的 Azure AD 應用程式](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account) \(部分機器翻譯\)。 

3.  返回 [使用者] 頁面，按一下您 Azure AD 應用程式的名稱以移至應用程式設定，然後複製 [租用戶識別碼] 和 [用戶端識別碼] 值。

4. 按一下 [新增金鑰]。 在後續畫面上，複製 [金鑰] 值。 在您離開此頁面之後，便無法再次存取此資訊。 如需詳細資訊，請參閱[管理 Azure AD 應用程式的金鑰](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys) \(部分機器翻譯\)。

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>步驟 2:取得 Azure AD 存取權杖

在 Microsoft Store 促銷 API 中呼叫任何方法之前，您必須先取得傳遞至 API 中每個方法的 **Authorization** 標頭的 Azure AD 存取權杖。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以重新整理權杖以便在後續呼叫 API 時繼續使用。

若要取得存取權杖，請按照[使用用戶端認證的服務對服務呼叫](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow)中的指示，將 HTTP POST 傳送至 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 端點。 以下是範例要求。

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

針對 POST URI 中的 *租使用者 \_ 識別碼* 值，以及 *客戶 \_ 端識別碼* 和用戶端 * \_ 密碼* 參數，指定您的應用程式的租使用者識別碼、用戶端識別碼和金鑰，並從上一節的合作夥伴中心中取出。 針對 *resource* 參數，您必須指定 ```https://manage.devcenter.microsoft.com```。

存取權杖到期之後，您可以按照[這裡](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens)的指示，重新整理權杖。

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>步驟 3：呼叫 Microsoft Store 促銷 API

有了 Azure AD 存取權杖之後，就可以呼叫 Microsoft Store 促銷 API。 您必須將存取權杖傳送給每個方法的 **Authorization** 標頭。

在 Microsoft Store 中促銷 API 的內容中，廣告行銷活動包含 *「行銷活動」* 物件 (其中包含行銷活動的高階資訊)，以及其他物件 (代表廣告行銷活動的 *「廣告播送行」*、*「目標設定檔」* 及 *「廣告素材」*)。 API 包括不同組的方法，依物件類型分組。 若要建立行銷活動，您通常針對每個物件呼叫不同 POST 方法。 API 也提供 GET 方法，可用來擷取任何物件，以及 PUT 方法，可以使用它來編輯行銷活動、廣告播送行及目標設定檔物件。

如需有關這些物件與其相關方法的詳細資訊，請參閱下表。


| Object       | 描述   |
|---------------|-----------------|
| 廣告活動 |  這個物件表示廣告行銷活動，它位於廣告行銷活動的物件模型階層頂端。 這個物件辨識您正在執行之行銷活動的類型 (付費、內部或社群)、行銷活動目標、行銷活動的廣告播送行，以及其他詳細資料。 每個行銷活動只能與一個 app 相關聯。<br/><br/>如需有關這個物件的相關方法的詳細資訊，請參閱[管理廣告行銷活動](manage-ad-campaigns.md)。<br/><br/>**Note** &nbsp; 注意 &nbsp;建立 ad 活動之後，您可以使用[Microsoft Store 分析 API](access-analytics-data-using-windows-store-services.md)中的 [[取得 ad 活動效能資料](get-ad-campaign-performance-data.md)] 方法，來取出活動的效能資料。  |
| 廣告播送行 | 每個行銷活動有一或多個廣告播送行用於購買庫存廣告，並播送您的廣告。 對於每個廣告播送行，您可以設為目標，設定買方出價，以及設定預算和要使用之廣告素材的連結，決定要花多少費用。<br/><br/>如需有關這個物件的相關方法的詳細資訊，請參閱[管理廣告行銷活動的廣告播送行](manage-delivery-lines-for-ad-campaigns.md)。 |
| 鎖定設定檔 | 每個廣告播送行有一個目標設定檔，指定您想要鎖定的使用者、地區和庫存廣告類型。 目標設定檔可以建立做為範本，並跨廣告播送行共用。<br/><br/>如需有關這個物件的相關方法的詳細資訊，請參閱[管理廣告行銷活動的目標設定檔](manage-targeting-profiles-for-ad-campaigns.md)。 |
| 廣告素材 | 每個廣告播送行有一或多個廣告素材，代表行銷活動中對客戶顯示的廣告。 廣告素材可能會與一個或多個廣告播送行相關聯，甚至跨廣告行銷活動，但前提是它永遠代表相同 app。<br/><br/>如需有關這個物件的相關方法的詳細資訊，請參閱[管理廣告行銷活動的廣告素材](manage-creatives-for-ad-campaigns.md)。 |


下圖顯示行銷活動、廣告播送行、目標設定檔及廣告素材之間的關係。

![廣告行銷活動階層](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>程式碼範例

下列程式碼範例示範如何取得 Azure AD 存取權杖，並從 C# 主控台應用程式呼叫 Microsoft Store 促銷 API。 若要使用此程式碼範例，請針對您的案例將 *tenantId*、*clientId*、*clientSecret* 和 *appID* 變數指定為適當的值。 此範例需要 Newtonsoft 的 [Json.NET 套件](https://www.newtonsoft.com/json)，以還原序列化 Microsoft Store 促銷 API 傳回的 JSON 資料。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Promotions/cs/Program.cs" id="PromotionsApiExample":::

## <a name="related-topics"></a>相關主題

* [管理廣告行銷活動](manage-ad-campaigns.md)
* [管理廣告行銷活動的廣告播送行](manage-delivery-lines-for-ad-campaigns.md)
* [管理廣告行銷活動的目標設定檔](manage-targeting-profiles-for-ad-campaigns.md)
* [管理廣告行銷活動的廣告素材](manage-creatives-for-ad-campaigns.md)
* [取得廣告活動績效資料](get-ad-campaign-performance-data.md)


 