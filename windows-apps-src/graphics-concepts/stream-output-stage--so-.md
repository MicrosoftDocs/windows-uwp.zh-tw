---
title: 資料流輸出 (SO) 階段
description: 資料流輸出 (SO) 階段會持續將頂點資料從上一個作用中階段輸出（串流）到記憶體中的一或多個緩衝區。 串流輸出到記憶體的資料可重新循環回到管線做為輸入資料，或從 CPU 讀回。
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- 資料流輸出 (SO) 階段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f56036ecc083d72f552954860d04750c1c83b8b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156202"
---
# <a name="stream-output-so-stage"></a>資料流輸出 (SO) 階段


資料流輸出 (SO) 階段會持續將頂點資料從上一個作用中階段輸出（串流）到記憶體中的一或多個緩衝區。 串流輸出到記憶體的資料可重新循環回到管線做為輸入資料，或從 CPU 讀回。

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途及用途


![在管線中資料流輸出階段的位置圖](images/d3d10-pipeline-stages-so.png)

資料流輸出階段會在基本型別資料前往轉譯器的過程中將其從管線串流到記憶體。 從上一個階段的資料可以串流輸出到記憶體，和（或）傳送到轉譯器。 串流輸出到記憶體的資料可重新循環回到管線做為輸入資料，或從 CPU 讀回。

串流輸出到記憶體的資料可在後續轉譯行程中讀回到管線，或者可以複製到預備環境資源（讓它可供 CPU 讀取）。 串流輸出的資料量有所不同。Direct3D 是設計成處理資料，而不需要查詢 (GPU) 有關撰寫的資料數量。--&gt;

有兩種方式可將資料流輸出資料饋送至管線：

-   資料流輸出資料可以饋送回到輸入組合語言 (IA) 階段。
-   使用 [Load](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load) 函式，可程式化的著色器可讀取資料流輸出資料。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>輸入


來自上一個著色器階段的頂點資料。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>輸出


資料流輸出 (SO) 階段會持續將頂點資料從上一個作用中階段 (例如幾何著色器 (GS) 階段) 輸出 (或串流) 到記憶體中的一或多個緩衝區。 如果幾何著色器 (GS) 階段處於非使用中狀態，則資料流程輸出 (因此) 階段會持續從網域著色器將頂點資料輸出到記憶體中的緩衝區 (，或如果 DS 也非使用中，則是從頂點著色器) 與 (階段 (。

當三角形或帶狀線繫結至輸入組合語言 (IA) 階段時，每一個區域在串流輸出之前會先轉換為清單。頂點永遠寫出為完整的基本類型（例如，對於三角形，一次 3 個頂點）。不完整的基本類型永遠不會串流輸出。相鄰基本類型在串流輸出資料之前會先捨棄相鄰資料。

資料流輸出階段同時支援最多 4 個緩衝區。

-   如果您要串流資料到多個緩衝區，每個緩衝區只可以擷取每個頂點資料的單一元素（最多 4 個元件），隱含的資料分散等於每個緩衝區中的元素寬度（相容於單一元素可繫結以供輸入到著色器階段的方式）。 此外，如果緩衝區有不同的大小，其中一個緩衝區已滿時寫入會立即停止。
-   如果您要串流資料到單一緩衝區，緩衝區可以擷取每個頂點資料最多 64 個純量元件 (256 位元組或更少)，或者頂點分散可能是 2048 位元組。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[圖形管線](graphics-pipeline.md)

 

 