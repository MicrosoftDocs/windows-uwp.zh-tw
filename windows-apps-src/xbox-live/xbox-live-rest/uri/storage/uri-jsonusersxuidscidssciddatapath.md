---
title: /json/users/xuid({xuid})/scids/{scid}/data/{path}
assetID: c2745955-5e52-ea2b-7389-cb85202e01c3
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapath.html
description: " /json/users/xuid({xuid})/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 275842420b73abc6c2fd8a8fafa265777e2fa00f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637153"
---
# <a name="jsonusersxuidxuidscidssciddatapath"></a>/json/users/xuid({xuid})/scids/{scid}/data/{path}
列出在指定路徑的檔案資訊。 這些 Uri 的網域是`titlestorage.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 不帶正負號的 64 位元整數| Xbox 使用者識別碼 (XUID) 播放程式的人員提出要求。| 
| scid| guid| 要查閱的服務組態的識別碼。| 
| path| 字串| 要傳回資料項目的路徑。 傳回所有相符的目錄和子目錄。 有效字元包括大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_) 和正斜線 （/）。 可能是空的。 最大長度 256。| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-jsonusersxuidscidssciddatapath-get.md)

&nbsp;&nbsp;列出在指定路徑的檔案資訊。
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>父系 

[標題儲存體 Uri](atoc-reference-storagev2.md)

   