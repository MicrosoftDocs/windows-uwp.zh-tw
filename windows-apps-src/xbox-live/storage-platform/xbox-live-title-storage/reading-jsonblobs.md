---
title: 讀取 JSON blob
description: 了解如何讀取 JSON blob 的 Xbox Live 的標題儲存體中。
ms.assetid: 3697af16-d054-4835-af7f-7fee8c628345
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 30d2b9f6539e73df1c5ea5ae18b3d1a6ca061d60
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613483"
---
# <a name="reading-a-json-blob-in-xbox-live-title-storage"></a>讀取在 Xbox Live 的標題儲存體中的 JSON blob

1.  傳送要求，使用*取得*從標題的儲存體讀取資料的方法。 此範例會使用全域的標題儲存體。

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/surprise.json,json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive

-   使用者必須更新該工作階段中。

-   STSTokenString 是為求簡單明瞭的預留位置，而且應該以驗證要求所傳回的語彙基元取代。

#### <a name="reference"></a>參考

**/global/scids/{scid}/data/{pathAndFileName},{type}**