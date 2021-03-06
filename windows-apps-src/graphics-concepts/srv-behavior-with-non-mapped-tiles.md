---
title: 非對應磚的 SRV 行為
description: 著色器資源檢視 (SRV) 涉及非對應磚的讀取行為取決於硬體支援程度。
ms.assetid: 0E1D64BE-EB09-4F9D-9800-BD23A3B374EE
keywords:
- 非對應磚的 SRV 行為
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d62a1d3158e13187f89277a1ba009bd56fc2b39a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659423"
---
# <a name="span-iddirect3dconceptssrvbehaviorwithnon-mappedtilesspansrv-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.srv_behavior_with_non-mapped_tiles"></span>SRV 行為與非對應的圖格


著色器資源檢視 (SRV) 涉及非對應磚的讀取行為取決於硬體支援程度。 如需需求明細，請參閱[串流資源功能層](streaming-resources-features-tiers.md)的讀取行為。 本節摘要[第 2 層](tier-2.md)需要的理想行為。

請考慮可讀取 SRV 中的一組紋素的紋理篩選操作。 在格式的所有非遺失元件中，落在非對應磚的紋素貢獻 0（如果是遺失元件則為預設值）到整體篩選作業，對應紋素的貢獻在旁邊。 紋素全部都會加權和結合在一起，不受資料是來自對應或非對應磚的影響。

部分第一代[第 2 層](tier-2.md)硬體不符合此規格需求，並在任何紋素（含非零加權）落在非對應磚時傳回 0（以及預設值，如前述）做為整體篩選結果。 不允許任何其他硬體錯過將所有（非零加權）紋素包含在篩選中的需求。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[資料流資源的存取管線](pipeline-access-to-streaming-resources.md)

 

 




