---
title: 常數緩衝區檢視 (CBV)
description: 常數緩衝區包含著色器常數資料。 其價值在於資料會持續且可由任何 GPU 著色器存取，直到必須變更資料為止。
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- 常數緩衝區檢視 (CBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 50b590ab931f60b67ecd527629b681c9ffe63f71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162742"
---
# <a name="constant-buffer-view-cbv"></a>常數緩衝區檢視 (CBV)


常數緩衝區包含著色器常數資料。 其價值在於資料會持續且可由任何 GPU 著色器存取，直到必須變更資料為止。

常數緩衝區的一般資料為全球、投影及檢視矩陣，其在一個畫面的繪圖中保持不變。

常數緩衝區配置應與 HLSL 配置相符 (請參閱[常數變數的封裝規則](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-packing-rules))。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[檢視](views.md)

 

 