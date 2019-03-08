---
title: /serviceconfigs/{scid}/sessions
assetID: d1932817-dcce-5a5c-d7e4-9fd97e1d287c
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessions.html
description: " /serviceconfigs/{scid}/sessions"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 700575ee9e9afa7bc7a1d34388dc071873d3b9ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612573"
---
# <a name="serviceconfigsscidsessions"></a>/serviceconfigs/{scid}/sessions
支援取得作業以擷取一組工作階段的文件。 
<a id="ID4EO"></a>

 
## <a name="domain"></a>網域
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| scid| GUID| 服務設定識別項 (SCID)。 第 1 部分的工作階段識別碼。| 
| 關鍵字| 字串| 關鍵字，可用來篩選到只以該字串識別的工作階段的結果。| 
| xuid| GUID| Xbox 使用者要擷取工作階段的使用者識別碼。 使用者必須是作用中的工作階段中。| 
| 保留項目| 字串| 值，指出如果工作階段的清單中包含的使用者則未接受。 這個參數只能設定為 true。 此設定需要呼叫者必須具備伺服器層級存取工作階段中，或呼叫端的 XUID 宣告以符合 Xbox 使用者識別碼篩選器。 | 
| 非使用中| 字串| 值，指出工作階段的清單包含這些使用者已接受，但不主動播放。 這個參數只能設定為 true。| 
| 私用| 字串| 值，指出是否工作階段的清單包含私用的工作階段。 這個參數只能設定為 true。 只有當查詢自己的工作階段，或是查詢伺服器對伺服器時，會是有效的。 將此參數設定為 true 呼叫者必須具備伺服器層級存取工作階段中，或呼叫端的 XUID 宣告以符合 Xbox 使用者識別碼篩選器。 | 
| 可見度| 字串| 列舉值，指出用於篩選結果的可見性狀態。 目前這個參數可以只會設定為開啟包含開啟的工作階段。 請參閱<b>MultiplayerSessionVisibility</b>。| 
| version| 字串| 正整數，表示主要的工作階段的版本或較低的工作階段加入。 值必須是小於或等於要求的合約版本 100 模數。| 
| Take| 字串| 正整數，指出工作階段數目上限來擷取。| 
  
<a id="ID4E1D"></a>

 
## <a name="valid-methods"></a>有效的方法

[取得 (/serviceconfigs/ {scid} / 工作階段)](uri-serviceconfigsscidsessionsget.md)

&nbsp;&nbsp;擷取指定的工作階段資訊。
 
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>父系 

[工作階段目錄的 Uri](atoc-reference-sessiondirectory.md)

   