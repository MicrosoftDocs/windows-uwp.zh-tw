---
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: 使用本節的 C# 程式碼範例，深入了解如何使用 Microsoft Store 提交 API。
title: C# 範例 - 提交應用程式、附加元件與正式發行前小眾測試版
ms.date: 08/03/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 程式碼範例, C#
ms.localizationpriority: medium
ms.openlocfilehash: c1f5704963dd1d6d6ad786a48c63ecfcd789aff9
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811301"
---
# <a name="c-sample-submissions-for-apps-add-ons-and-flights"></a>C \# 範例：提交應用程式、附加元件和航班

本文提供 C# 程式碼範例，示範如何使用 [Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md)來執行這些任務：

* [建立應用程式提交](#create-app-submission)
* [建立附加元件提交](#create-add-on-submission)
* [更新附加元件提交](#update-add-on-submission)
* [建立套件正式發行前小眾測試版提交](#create-flight-submission)

您可以檢閱每個範例以深入了解示範的工作，或者您可以將本篇文章中的所有程式碼範例建置到主控台應用程式。 若要建置範例，請在 Visual Studio 中建立名為 **DeveloperApiCSharpSample** 的 C# 主控台應用程式，分別將每個範例複製到專案中單獨的程式碼檔案，然後建置專案。

## <a name="prerequisites"></a>先決條件

這些範例使用下列程式庫：

* Microsoft.WindowsAzure.Storage.dll。 此程式庫位於[適用於 .NET 的 Azure SDK](https://azure.microsoft.com/downloads/) 中，或者您也可以安裝 [WindowsAzure.Storage NuGet 套件](https://www.nuget.org/packages/WindowsAzure.Storage)來取得。
* 來自 Newtonsoft 的 [Newtonsoft.Json](https://www.newtonsoft.com/json) NuGet 套件。

## <a name="main-program"></a>主要程式

下列範例實作的命令列程式會呼叫本文中的其他範例方法，示範使用 Microsoft Store 提交 API 的不同方式。 若要自行調整這個程式：

* 將 ```ApplicationId```、```InAppProductId``` 和 ```FlightId``` 屬性指派給您想要管理之應用程式、附加元件和套件正式發行前小眾測試版的識別碼。
* 將 ```ClientId``` 和 ```ClientSecret``` 屬性指派給應用程式的用戶端識別碼和金鑰，並使用應用程式的租用戶識別碼來取代 ```TokenEndpoint``` URL 中的 *tenantid* 字串。 如需詳細資訊，請參閱 [如何將 Azure AD 應用程式與您的合作夥伴中心帳戶建立關聯](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/Program.cs" id="Main":::

<span id="clientconfiguration" />

## <a name="clientconfiguration-helper-class"></a>ClientConfiguration 協助程式類別

範例應用程式使用 ```ClientConfiguration``` 協助程式類別，將 Azure Active Directory 資料和應用程式資料傳遞給每個使用 Microsoft Store 提交 API 的範例方法。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/ClientConfiguration.cs" id="ClientConfiguration":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>建立應用程式提交

下列範例所實作的類別使用 Microsoft Store 提交 API 中的幾個方法來更新應用程式提交。 ```RunAppSubmissionUpdateSample```類別中的方法會建立新提交做為上次發佈提交的複製品，然後它會更新並認可複製的提交至合作夥伴中心。 具體來說，```RunAppSubmissionUpdateSample``` 方法會執行以下工作：

1. 一開始，此方法[為指定的應用程式取得資料](get-an-app.md)。
2. 接下來，它會[刪除應用程式的擱置中提交](delete-an-app-submission.md) (如果有的話)。
3. 然後它會[為此應用程式建立新的提交](create-an-app-submission.md) (新的提交是最後一個已發佈提交的複本)。
4. 它會變更新提交的一些詳細資料，並上傳新的套件以提交至 Azure Blob 儲存體。
5. [接著，](commit-an-app-submission.md)它會[更新](update-an-app-submission.md)並認可新提交合作夥伴中心。
6. 最後，它會定期[檢查新提交的狀態](get-status-for-an-app-submission.md)，直到此提交認可成功為止。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs" id="AppSubmissionUpdateSample":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>建立附加元件提交

下列範例所實作的類別使用 Microsoft Store 提交 API 中的幾個方法來建立新的附加元件提交。 類別中的 ```RunInAppProductSubmissionCreateSample``` 方法會執行以下工作：

1. 一開始，此方法會[建立新的附加元件](create-an-add-on.md)。
2. 接下來，它會[建立附加元件的新提交](create-an-add-on-submission.md)。
3. 它會上傳包含提交給 Azure Blob 儲存體之圖示的 ZIP 封存。
4. 接下來，它會 [將新提交認可至合作夥伴中心](commit-an-add-on-submission.md)。
5. 最後，它會定期[檢查新提交的狀態](get-status-for-an-add-on-submission.md)，直到此提交認可成功為止。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs" id="InAppProductSubmissionCreateSample":::

<span id="update-add-on-submission" />

## <a name="update-an-add-on-submission"></a>更新附加元件提交

下列範例所實作的類別使用 Microsoft Store 提交 API 中的幾個方法來更新現有的附加元件提交。 ```RunInAppProductSubmissionUpdateSample```類別中的方法會建立新提交做為上次發佈提交的複製品，然後它會更新並認可複製的提交至合作夥伴中心。 具體來說，```RunInAppProductSubmissionUpdateSample``` 方法會執行以下工作：

1. 一開始，此方法會[針對指定的附加元件取得資料](get-an-add-on.md)。
2. 接下來，它會[刪除附加元件的擱置中提交](delete-an-add-on-submission.md) (如果有的話)。
3. 然後它會[為此附加元件建立新的提交](create-an-add-on-submission.md) (新的提交是最後一個已發佈提交的複本)。
5. [接著，](commit-an-add-on-submission.md)它會[更新](update-an-add-on-submission.md)並認可新提交合作夥伴中心。
6. 最後，它會定期[檢查新提交的狀態](get-status-for-an-add-on-submission.md)，直到此提交認可成功為止。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs" id="InAppProductSubmissionUpdateSample":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>建立套件正式發行前小眾測試版提交

下列範例所實作的類別使用 Microsoft Store 提交 API 中的幾個方法來更新套件正式發行前小眾測試版提交。 ```RunFlightSubmissionUpdateSample```類別中的方法會建立新提交做為上次發佈提交的複製品，然後它會更新並認可複製的提交至合作夥伴中心。 具體來說，```RunFlightSubmissionUpdateSample``` 方法會執行以下工作：

1. 一開始，此方法會[為指定的套件正式發行前小眾測試版取得資料](get-a-flight.md)。
2. 接下來，它會[刪除套件正式發行前小眾測試版的擱置中提交](delete-a-flight-submission.md) (如果有的話)。
3. 然後它會[為套件正式發行前小眾測試版建立新的提交](create-a-flight-submission.md) (新的提交是最後一個已發佈提交的複本)。
4. 它會上傳新的套件以供提交 Azure Blob 儲存體。
5. [接著，](commit-a-flight-submission.md)它會[更新](update-a-flight-submission.md)並認可新提交合作夥伴中心。
6. 最後，它會定期[檢查新提交的狀態](get-status-for-a-flight-submission.md)，直到此提交認可成功為止。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs" id="FlightSubmissionUpdateSample":::

<span id="ingestionclient" />

## <a name="ingestionclient-helper-class"></a>IngestionClient 協助程式類別

```IngestionClient``` 類別提供範例應用程式中其他方法用來執行以下工作的協助程式方法：

* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以用來呼叫 Microsoft Store 提交 API 中的方法。 取得權杖之後，在權杖到期之前，您有 60 分鐘的時間可以使用這個權杖呼叫 Microsoft Store 提交 API。 權杖到期之後，您可以產生新的權杖。
* 將包含新資產的 ZIP 封存上傳至 Azure Blob 儲存體的應用程式或附加元件提交。 如需有關將 ZIP 封存上傳至應用程式和附加元件的 Azure Blob 儲存體的詳細資訊，請參閱 [建立應用程式提交](manage-app-submissions.md#create-an-app-submission) 和 [建立附加元件提交](manage-add-on-submissions.md#create-an-add-on-submission)的相關指示。
* 處理 Microsoft Store 提交 API 的 HTTP 要求。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/IngestionClient.cs" id="IngestionClient":::

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
