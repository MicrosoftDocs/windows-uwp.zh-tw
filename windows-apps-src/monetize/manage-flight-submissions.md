---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: 在 Microsoft Store 提交 API 中使用這些方法來管理註冊至合作夥伴中心帳戶之應用程式的套件航班提交。
title: 管理套件正式發行前小眾測試版提交
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 正式發行前小眾測試版提交
ms.localizationpriority: medium
ms.openlocfilehash: a5c8a8c83420830c5ec20c9586c46d54a02a5238
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811268"
---
# <a name="manage-package-flight-submissions"></a>管理套件正式發行前小眾測試版提交

Microsoft Store 提交 API 提供方法讓您使用於管理應用程式的套件正式發行前小眾測試版，包括漸進式套件推出。 如需 Microsoft Store 提交 API 的簡介，包括使用此 API 的必要條件，請參閱[使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

> [!IMPORTANT]
> 如果您使用 Microsoft Store 提交 API 來建立封裝航班的提交，請務必使用 API 對提交進行進一步的變更，而不是合作夥伴中心。 如果您使用儀表板變更最初使用 API 所建立的提交，您將無法再使用 API 變更或是認可該提交。 有時候提交可能會處於錯誤狀態，而無法繼續提交過程。 若發生這種情形，您必須刪除提交並建立新的提交。

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>管理套件正式發行前小眾測試版提交的方法

使用下列方法取得、建立、更新、認可或刪除套件正式發行前小眾測試版提交。 在您可以使用這些方法之前，封裝航班必須已經存在於合作夥伴中心中。 您可以 [在合作夥伴中心](../publish/package-flights.md) 中建立封裝航班，也可以使用 [ [管理封裝航班](manage-flights.md)] 中所述的 Microsoft Store 提交 API 方法。

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">方法</th>
<th align="left">URI</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="get-a-flight-submission.md">取得現有的套件正式發行前小眾測試版提交</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">取得現有套件正式發行前小眾測試版提交的狀態</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">建立新套件正式發行前小眾測試版提交</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">更新現有的套件正式發行前小眾測試版提交</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">認可新的或已更新的套件正式發行前小眾測試版提交</a></td>
</tr>
<tr>
<td align="left">刪除</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">刪除套件正式發行前小眾測試版提交</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>建立套件正式發行前小眾測試版提交

若要建立套件正式發行前小眾測試版的提交，請遵循此程序。

1. 如果您尚未這麼做，請完成 [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)中所述的必要條件，包括將 Azure AD 應用程式與合作夥伴中心帳戶建立關聯，以及取得用戶端識別碼和金鑰。 您只需執行此動作一次；有了用戶端識別碼和金鑰之後，每當您必須建立新的 Azure AD 存取權杖時，就可以重複使用它們。  

2. [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 您必須將此存取權杖傳遞至 Microsoft Store 提交 API 中的方法。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

3. 在 Microsoft Store 提交 API 中執行下列方法來[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)。 這個方法會建立新的處理中提交，這是最後一個已發佈提交的複本。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions
    ```

    回應主體包含 [航班提交](#flight-submission-object) 資源，其中包含新提交的識別碼、共用存取簽章 (SAS) URI，用來上傳任何提交至 Azure Blob 儲存體的套件，以及新提交 (包含所有清單和定價資訊) 的資料。

    > [!NOTE]
    > SAS URI 提供 Azure 儲存體中安全資源的存取權，完全不需要帳戶金鑰。 如需有關 SAS Uri 及其搭配 Azure Blob 儲存體使用的背景資訊，請參閱 [共用存取簽章，第1部分：瞭解 sas 模型](/azure/storage/common/storage-sas-overview) 和 [共用存取簽章，第2部分：透過 Blob 儲存體建立和使用 sas](/azure/storage/common/storage-sas-overview)。

4. 如果您要為提交新增新的套件，請[準備套件](../publish/app-package-requirements.md)並將它們新增到 ZIP 封存。

5. 以新提交的任何所需變更來修訂[正式發行前小眾測試版提交](#flight-submission-object)資料，然後執行下列方法來[更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)。

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 如果您要新增提交的新套件，確定您會更新提交資料，以參考 ZIP 封存中的名稱和這些檔案的相對路徑。

4. 如果您要加入新的套件以供提交，請使用您稍早呼叫之 POST 方法的回應主體中提供的 SAS URI，將 ZIP 封存檔上傳至 [Azure Blob 儲存體](/azure/storage/storage-introduction#blob-storage) 。 您可在各種不同的平台上使用不同的 Azure Libraries 來執行，包括：

    * [適用於 .NET 的 Azure 儲存體用戶端程式庫](/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage SDK for Java](/azure/storage/storage-java-how-to-use-blob-storage)
    * [適用于 Python 的 Azure 儲存體 SDK](/azure/storage/storage-python-how-to-use-blob-storage)

    下列 c # 程式碼範例示範如何使用適用于 .NET 的 Azure 儲存體用戶端程式庫中的 [>cloudblockblob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) 類別，將 ZIP 封存上傳至 Azure Blob 儲存體。 此範例假設已將 ZIP 封存寫入串流物件。

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 執行下列方法來[認可套件正式發行前小眾測試版提交](commit-a-flight-submission.md)。 這會警示合作夥伴中心您已完成提交，而且您的更新現在應該套用至您的帳戶。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. 執行下列方法來[取得套件正式發行前小眾測試版提交的狀態](get-status-for-a-flight-submission.md)，以檢查認可狀態。

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    若要確認提交狀態，請檢閱回應主體中的 *「狀態」* 值。 這個值應該從 **CommitStarted** 變更為 **PreProcessing** (如果要求成功) 或 **CommitFailed** (如果要求中出現錯誤)。 如果出現錯誤，*statusDetails* 欄位會包含關於錯誤的進一步詳細資料。

7. 順利完成提交之後，即會將提交傳送到 Microsoft Store 以供擷取。 您可以使用先前的方法或造訪合作夥伴中心，繼續監視提交進度。

<span/>

## <a name="code-examples"></a>程式碼範例

下列文章提供詳細的程式碼範例，示範如何以數種不同的程式設計語言來建立套件正式發行前小眾測試版：

* [C# 程式碼範例](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 程式碼範例](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 程式碼範例](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 模組

另一種直接呼叫 Microsoft Store 提交 API 的方法，我們也提供了開放原始碼 PowerShell 模組，該模組在 API 上方實作命令列介面。 這個模組稱為 [StoreBroker](https://github.com/Microsoft/StoreBroker)。 您可以使用此模組從命令列來管理您的應用程式、正式發行前小眾測試版和附加元件提交，而不是直接直接呼叫 Microsoft Store 提交 API，也可以簡單瀏覽來源以檢視有關如何呼叫此 API 的更多範例。 StoreBroker 模組在 Microsoft 中積極地被用作為將眾多第一方應用程式提交至 Microsoft Store 的主要方式。

如需詳細資訊，請查看我們[在 GitHub 上的 StoreBroker 頁面](https://github.com/Microsoft/StoreBroker)。

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>管理套件正式發行前小眾測試版提交的漸進式套件推出

您可以將套件正式發行前小眾測試版提交中的已更新套件，以漸進方式推出給 Windows 10 上的一部分應用程式客戶。 這可讓您監視特定套件的意見反應和分析資料，以確保您在更廣泛地推出更新之前，就已經信心滿滿。 您可以變更所發佈之提交的推出百分比 (或停止更新)，而不需建立新的提交。 如需詳細資訊，包括如何在合作夥伴中心中啟用和管理漸進式套件推出的指示，請參閱 [這篇文章](../publish/gradual-package-rollout.md)。

若要以程式設計方式啟用和管理套件正式發行前小眾測試版提交的漸進式套件推出，請使用 Microsoft Store 提交 API 中的方法，遵照此程序執行。

  1. [建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)或[取得套件正式發行前小眾測試版提交](get-a-flight-submission.md)。
  2. 在回應資料中，找出 [packageRollout](#package-rollout-object) 資源、將 [ *isPackageRollout* ] 欄位設定為 [true]，然後將 [ *packageRolloutPercentage* ] 欄位設定為應取得更新套件的應用程式客戶百分比。
  3. 將已更新的套件正式發行前小眾測試版提交資料傳遞給[更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)方法。

在為套件正式發行前小眾測試版提交啟用漸進式套件推出之後，您可使用下列方法，以程式設計方式取得、更新、中斷或完成漸進式推出。

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">方法</th>
<th align="left">URI</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">取得套件正式發行前小眾測試版提交的漸進式推出資訊</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">更新套件正式發行前小眾測試版提交的漸進式推出百分比</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">中斷套件正式發行前小眾測試版提交的漸進式推出</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">完成套件正式發行前小眾測試版提交的漸進式推出</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>資料資源

用於管理套件正式發行前小眾測試版提交的 Microsoft Store 提交 API 方法，使用下列 JSON 資料資源。

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>正式發行前小眾測試版提交資源

此資源描述套件正式發行前小眾測試版提交。

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

此資源具有下列值。

| 值      | 類型   | 描述              |
|------------|--------|------------------------------|
| id            | 字串  | 提交的識別碼。  |
| flightId           | 字串  |  包含要與提交產生關聯之套件正式發行前小眾測試版的識別碼。  |  
| status           | 字串  | 提交的狀態。 這個值可以是下列其中一個值： <ul><li>無</li><li>已取消</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>發佈</li><li>已發行</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>認證</li><li>CertificationFailed</li><li>版本</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 物件 (object)  |  [狀態詳細資料資源](#status-details-object)包含其他有關提交狀態的詳細資料，包括任何錯誤的資訊。  |
| flightPackages           | array  | 包含可提供關於提交中每個套件之詳細資料的[正式發行前小眾測試版套件資源](#flight-package-object)。   |
| packageDeliveryOptions    | 物件 (object)  | [套件交付選項資源](#package-delivery-options-object)包含提交的漸進式套件推出和強制更新設定。   |
| fileUploadUrl           | 字串  | 共用存取簽章 (SAS) URI，可用於上傳任何適用於提交的套件。 如果您要新增提交的新套件，請將包含套件的 ZIP 封存上傳至這個 URI。 如需詳細資訊，請參閱[建立套件正式發行前小眾測試版提交](#create-a-package-flight-submission)。  |
| targetPublishMode           | 字串  | 提交的發佈模式。 這個值可以是下列其中一個值： <ul><li>立即</li><li>手動</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |
| notesForCertification           | 字串  |  為認證測試人員提供其他資訊，例如測試帳戶認證，以及存取和確認功能的步驟。 如需詳細資訊，請參閱[認證注意事項](../publish/notes-for-certification.md)。 |

<span id="status-details-object" />

### <a name="status-details-resource"></a>狀態詳細資料資源

此資源包含關於提交狀態的其他詳細資料。 此資源具有下列值。

| 值           | 類型    | 描述                   |
|-----------------|---------|------|
|  錯誤               |    物件 (object)     |   包含提交的錯誤詳細資料的[狀態詳細資料資源](#status-detail-object)陣列。   |     
|  warnings               |   物件 (object)      | 包含提交的警告詳細資料的[狀態詳細資料資源](#status-detail-object)陣列。     |
|  certificationReports               |     物件 (object)    |   提供提交認證報告資料存取的[認證報告資源](#certification-report-object)陣列。 如果認證失敗，您可以檢查這些報告來取得詳細資訊。    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>狀態詳細資料資源

此資源包含有關提交的任何相關錯誤或警告的其他資訊。 此資源具有下列值。

| 值           | 類型    | 說明       |
|-----------------|---------|------|
|  code               |    字串     |   描述錯誤或警告類型的[提交狀態碼](#submission-status-code)。 |  
|  詳細資料               |     字串    |  含有更多關於問題之詳細資料的訊息。     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>認證報告資源

此資源提供提交認證報告資料的存取。 此資源具有下列值。

| 值           | 類型    | 描述         |
|-----------------|---------|------|
|     date            |    字串     |  產生報表的日期和時間，格式為 ISO 8601。    |
|     reportUrl            |    字串     |  您可以存取報告的 URL。    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>正式發行前小眾測試版套件資源

此資源提供關於提交中套件的詳細資料。

```json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
}
```

此資源具有下列值。

> [!NOTE]
> 在呼叫 [更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)方法時，要求主體中只需要這個物件的 *fileName*、*fileStatus*、*minimumDirectXVersion* 及 *minimumSystemRam* 值。 其他值則由合作夥伴中心填入。

| 值           | 類型    | 描述              |
|-----------------|---------|------|
| fileName   |   字串      |  封裝的名稱。    |  
| fileStatus    | 字串    |  套件的狀態。 這個值可以是下列其中一個值： <ul><li>無</li><li>PendingUpload</li><li>已上傳</li><li>PendingDelete</li></ul>    |  
| id    |  字串   |  唯一識別套件的識別碼。 合作夥伴中心會使用這個值。   |     
| version    |  字串   |  應用程式套件的版本。 如需詳細資訊，請參閱[套件版本編號](../publish/package-version-numbering.md)。   |   
| 架構    |  字串   |  應用程式套件的架構 (例如，ARM)。   |     
| 語言    | array    |  應用程式所支援之語言的語言代碼陣列。 如需詳細資訊，請參閱[支援的語言](../publish/supported-languages.md)。    |     
| capabilities    |  array   |  套件所需的功能陣列。 如需功能的詳細資訊，請參閱[應用程式功能宣告](../packaging/app-capability-declarations.md)。   |     
| minimumDirectXVersion    |  字串   |  應用程式套件所支援的最低 DirectX 版本。 這只能針對目標為 Windows 8.x 的應用程式進行設定；對於目標為其他版本的應用程式則會加以忽略。 這個值可以是下列其中一個值： <ul><li>無</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字串    |  應用程式套件所需的最小 RAM。 這只能針對目標為 Windows 8.x 的應用程式進行設定；對於目標為其他版本的應用程式則會加以忽略。 這個值可以是下列其中一個值： <ul><li>無</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>套件交付選項資源

此資源包含提交的漸進式套件推出和強制更新設定。

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

此資源具有下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|------|
| packageRollout   |   物件 (object)      |   [套件推出資源](#package-rollout-object)包含用於提交的漸進式套件推出設定。    |  
| isMandatoryUpdate    | boolean    |  指出您是否要將這項提交中的套件視為自我安裝應用程式更新的強制項目。 如需有關自我安裝應用程式更新的強制套件詳細資訊，請參閱[下載與安裝應用程式的套件更新](../packaging/self-install-package-updates.md)。    |  
| mandatoryUpdateEffectiveDate    |  date   |  這項提交中的套件變成強制項目的日期和時間，採用 ISO 8601 格式和 UTC 時區。   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>套件推出資源

此資源包含提交的漸進式[套件推出設定](#manage-gradual-package-rollout)。 此資源具有下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  指出是否已為提交啟用漸進式套件推出。    |  
| packageRolloutPercentage    | FLOAT    |  將接收漸進式推出中套件的使用者百分比。    |  
| packageRolloutStatus    |  字串   |  下列其中一個字串，這些字串指出漸進式套件推出的狀態： <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  字串   |  未取得漸進式推出套件的客戶將收到的提交識別碼。   |          

> [!NOTE]
> *PackageRolloutStatus* 和 *fallbackSubmissionId* 值是由合作夥伴中心指派，且不適合由開發人員設定。 如果您將這些值包含在要求本文中，則會忽略這些值。

<span/>

## <a name="enums"></a>列舉

這些方法會使用下列列舉。

<span id="submission-status-code" />

### <a name="submission-status-code"></a>提交狀態碼

下列代碼代表提交的狀態。

| 程式碼           |  描述      |
|-----------------|---------------|
|  無            |     未指定任何代碼。         |     
|      InvalidArchive        |     包含該套件的 ZIP 封存無效，或具有無法辨識的封存格式。  |
| MissingFiles | ZIP 封存沒有您提交資料中列出的所有檔案，或者它們位於封存中的錯誤位置。 |
| PackageValidationFailed | 提交中有一或多個套件無法驗證。 |
| InvalidParameterValue | 要求主體中有一個參數不正確。 |
| InvalidOperation | 您嘗試執行的操作無效。 |
| InvalidState | 您嘗試針對套件正式發行前小眾測試版的目前狀態執行的操作不正確。 |
| ResourceNotFound | 找不到指定的套件正式發行前小眾測試版。 |
| ServiceError | 內部服務錯誤已防止要求成功。 請重試要求。 |
| ListingOptOutWarning | 開發人員已從先前提交中移除清單，或者未包含該套件支援的清單資訊。 |
| ListingOptInWarning  | 開發人員已新增清單。 |
| UpdateOnlyWarning | 開發人員正嘗試插入某些只有更新支援的項目。 |
| 其他  | 提交處於無法辨識或未分類的狀態。 |
| PackageValidationWarning | 套件驗證程序產生了警告。 |

<span/>

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Microsoft Store 提交 API 管理套件正式發行前小眾測試版](manage-flights.md)
* [取得套件正式發行前小眾測試版提交](get-a-flight-submission.md)
* [建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)
* [更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)
* [認可套件正式發行前小眾測試版提交](commit-a-flight-submission.md)
* [刪除套件正式發行前小眾測試版提交](delete-a-flight-submission.md)
* [取得套件正式發行前小眾測試版提交的狀態](get-status-for-a-flight-submission.md)
