---
title: DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
assetID: ad049340-f752-e05e-8b34-62adb8e4771f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameremoveitemsdelete.html
description: " DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d26d8eaf54dcc14de3e31d7c2b54cd4454f2029f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594093"
---
# <a name="delete-usersxuidxuidlistspinslistnameremoveitems"></a>DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
依據索引，從清單移除項目。 這些 Uri 的網域是`eplists.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4ECB)
  * [查詢字串參數](#ID4ELC)
  * [要求本文](#ID4END)
  * [HTTP 狀態碼](#ID4EYD)
  * [必要的要求標頭](#ID4EOBAC)
  * [回應主體](#ID4EEDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註 
 
從清單移除項目。 要移除的項目會以其在清單中的索引和查詢字串參數 「 索引 」 中所識別。 索引的清單必須以逗號分隔，並只 100 的索引可以傳入單一呼叫。 不過，如果沒有索引傳遞 （空的查詢字串參數） 然後內容的完整清單將會刪除 （清單會保留，但空的版本號碼將會繼續遞增）。 一旦移除項目，清單會 「 壓縮 」，因此沒有安全漏洞會留在索引的排序。 
 
此呼叫都需要 「 如果-比對： versionNumber"標頭包含在要求中，其中 versionNumber 是目前的版本號碼的檔案。 如果它不包含在內，或不符合目前會傳回清單的版本號碼，則 HTTP 412-先決條件失敗的狀態碼和回應主體會包含最新的中繼資料，其中包含目前的版本號碼的清單。 這是為了防範來自欺侮彼此不同的用戶端的更新。 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 參數 
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| XUID| 字串| XUID 的使用者。| 
| listname| 字串| 要操作之清單的名稱。| 
  
<a id="ID4ELC"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數 
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| indexes| 字串| 零或正整數。 數字就不必是連續的。 例如，查詢參數索引 = 1，8 將移除 1 和 8 的索引處的項目。 <b>如果未不指定任何索引，則會刪除整個清單。</b>| 
  
<a id="ID4END"></a>

 
## <a name="request-body"></a>要求本文 
 
要求主體必須是空的這個呼叫。
  
<a id="ID4EYD"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼 
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定 | 代表要求順利完成。 回應主體應該包含要求的資源 （GET)。 POST 和 PUT 要求會收到最新清單中繼資料 （清單版本、 計數等等）。| 
| 201| 建立時間 | 已建立新的清單。 這會傳回對初始插入至清單。 回應會包含在清單上的最新中繼資料和 location 標頭包含清單的 URI。| 
| 304| 未修改| 傳回上取得。 表示用戶端具有最新版本的清單。 服務會比較中的值<b>If-match</b>清單版本標頭。 如果兩者相等，則取得傳回 304 沒有資料。 | 
| 400| 錯誤的要求 | 要求的格式不正確。 可能是無效的值或 URI 或查詢字串參數的型別。 也可以是遺漏的必要參數的結果，或資料值或要求指示 遺失或無效的 api 版本。 請參閱<b>X XBL-合約版本</b>標頭。 | 
| 401| 未經授權 | 要求需要使用者驗證。| 
| 403| 已禁止 | 要求不允許使用者或服務。| 
| 404| 找不到 | 呼叫端沒有存取資源。 這表示已建立的任何這類清單。| 
| 412| 先決條件失敗 | 表示清單的版本已變更，而且修改要求失敗。 修改要求是插入、 更新或移除。 服務會檢查<b>If-match</b>清單版本標頭。 如果它不符合目前版本的清單，作業就會失敗，這會傳回目前的清單中繼資料 （其中包含目前的版本）。 | 
| 500| 內部伺服器錯誤 | 服務會拒絕要求，因為伺服器端錯誤。| 
| 501| 未實作 | 呼叫端要求的 URI，在伺服器上尚未實作。 （目前僅使用時的要求是針對未列入允許清單的清單名稱。）| 
| 503| 服務無法使用 | 伺服器拒絕要求時，通常是因為過多的負載。 在延遲之後 (請參閱<b>retry-after</b>標頭)，可以重試要求。 | 
  
<a id="ID4EOBAC"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 包含用來驗證和授權要求的 STS 權杖。 必須是從 XSTS 服務語彙基元，並將 XUID 納入作為其中一個宣告。 | 
| X-XBL-Contract-Version| 指定的 API 版本正在要求 （正整數）。 Pin 支援第 2 版。 如果此標頭遺失，或該服務會傳回 400-不支援的值不正確的要求與 「 不受支援或遺漏合約版本標頭 」 中的狀態描述。 | 
| Content-Type| 指定是否要求/回應內文的內容會以 json 或 xml。 支援的值為"application/json"和"application/xml"| 
| If-Match| 進行修改的要求時，此標頭必須包含一份目前的版本號碼。 修改要求使用 PUT、 POST 或 DELETE 動詞命令。 如果"If-match"標頭中的版本不符合目前版本的清單，要求將會遭到拒絕，並顯示 HTTP 412-先決條件失敗傳回碼。 （選擇性）也可用於取得，如果存在且傳入的版本比對目前的清單版本則沒有清單的資料和 HTTP 304-未修改的程式碼將會傳回。 | 
  
<a id="ID4EEDAC"></a>

 
## <a name="response-body"></a>回應主體 
 
如果呼叫成功，服務就會傳回最新的中繼資料的清單。 
 
<a id="ID4EODAC"></a>

 
### <a name="sample-response"></a>範例回應 
 

```cpp
{
        "ListVersion":  1,
        "ListCount":  1,
        "MaxListSize": 200,
        "AllowDuplicates": "true",
        "AccessSetting":  "OwnerOnly"
        }

      
```

   
<a id="ID4E1DAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E3DAC"></a>

 
##### <a name="parent"></a>父系 

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)

   