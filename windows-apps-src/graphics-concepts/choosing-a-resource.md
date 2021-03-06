---
title: 選擇資源
description: 瞭解如何在您的應用程式中，找出並選取最適合與3D 圖形管線不同階段系結的資源。
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- 選擇資源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69527915988136854e201b82332c17f3e0134290
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094466"
---
# <a name="choosing-a-resource"></a>選擇資源


資源是 3D 管線使用的一組資料。 建立資源及定義其行為，是應用程式的程式設計首項步驟。 本指引涵蓋基本主題，內容為選擇應用程式所需的資源。

## <a name="span-ididentify_bindingspanspan-ididentify_bindingspanspan-ididentify_bindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>找出需要資源的管線階段


首項步驟為選擇一或多個將會使用資源的[圖形管線](graphics-pipeline.md)階段。 意即，找出會從資源讀取資料以及將資料寫入資源的階段。 了解資源使用所在的管線階段，可決定要呼叫用來將資源繫結至階段的 API。

此表列出可繫結至各管線階段的資源類型， 包括是否可將資源繫結為輸入或輸出。

| 管線階段  | 輸入/輸出 | 資源               | 資源類型                           |
|-----------------|--------|------------------------|-----------------------------------------|
| 輸入組合器 | 位於     | 頂點緩衝區          | Buffer                                  |
| 輸入組合器 | 位於     | 索引緩衝區           | Buffer                                  |
| 著色器階段   | 位於     | Shader-ResourceView    | 緩衝區, Texture1D, Texture2D, Texture3D |
| 著色器階段   | 位於     | Shader-Constant Buffer | Buffer                                  |
| 資料流輸出   | 外    | Buffer                 | Buffer                                  |
| 輸出合併   | 外    | 轉譯目標檢視     | 緩衝區, Texture1D, Texture2D, Texture3D |
| 輸出合併   | 外    | 深度/樣板檢視     | Texture1D, Texture2D                    |

 

## <a name="span-ididentify_usagespanspan-ididentify_usagespanspan-ididentify_usagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>找出每個資源的使用方式


選擇應用程式要使用的管線階段 (以及各階段需要的資源) 後，下一個步驟為決定各個資源的使用方式，意即 CPU 或 GPU 是否可存取資源。

應用程式執行所在的硬體上至少會有一個 CPU 和一個 GPU。 若要挑選使用值，請從下列選項考慮何種類型的處理器需要讀取或寫入資源。

| 資源使用狀況 | 可透過下項更新                    | 更新的頻率 |
|----------------|--------------------------------------|---------------------|
| 預設        | GPU                                  | 不常        |
| 動態        | CPU                                  | 頻繁          |
| 預備        | GPU                                  | n/a                 |
| 固定      | CPU (只能在資源建立的時間) | n/a                 |

 

預設使用方式應用於預期不常由 CPU 更新的資源 (少於每畫面一次)。 理想的狀況是，CPU 永遠不會使用預設使用方式直接寫入資源，以避免潛在的效能降低。

動態使用方式應用於相對而言較常由 CPU 更新的資源 (等於或多於每畫面一次)。 動態資源的常見案例為建立動態頂點和索引緩衝區，其會在執行階段由每個畫面使用者視角可見的幾何資料填滿。 這些緩衝區僅會用來轉譯該畫面使用者可見的幾何。

暫存使用方式應用於自其他資源複製資料，以及將資料複製到其中。 常見案例是將使用預設使用方式 (CPU 不能加以存取) 之資源的資料複製到使用暫存使用方式 (CPU 可以加以存取) 的資源。

資源中的資料永遠不會變更時，應使用固定資源。

另一種看待相同想法的方式是，試想應用程式與資源的搭配方式。

| 應用程式使用資源的方式     | 資源使用狀況       |
|---------------------------------------|----------------------|
| 載入一次且永不更新            | 固定或預設 |
| 應用程式重複填滿資源 | 動態              |
| 轉譯至紋理                     | 預設              |
| CPU 的 GPU 資料存取權                | 預備              |

 

如果您不確定要選擇哪個使用方式，請先使用預設使用方式，因其是最常見的情形。 著色器常數緩衝區這項資源類型應一律使用預設使用方式。

## <a name="span-idresource_types_and_pipeline_stagesspanspan-idresource_types_and_pipeline_stagesspanspan-idresource_types_and_pipeline_stagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>將資源系結至管線階段


資源可同時繫結至多個管線階段，只要符合資源建立時所指定的限制即可。 這些限制指定為使用方式旗標、繫結旗標或 CPU 存取旗標。 更明確地說，資源可同時繫結為輸入與輸出，只要不同時發生資源的讀取和寫入部分即可。

繫結資源時，請思考 GPU 和 CPU 存取資源的方式。 為單一目的所設計的資源 (不使用多個使用方式、繫結及 CPU 存取旗標) 效能很有可能更好。

例如，請考量轉譯目標多次作為紋理的案例。 若有兩個資源可能會更快速︰轉譯目標和紋理作為著色器資源。 每個資源都只會使用一個繫結旗標，指出「轉譯目標」或「著色器資源」。 資料會從轉譯目標紋理複製到著色器紋理。

此範例的這項技術可能會改善效能，方法是將轉譯目標寫入自著色器紋理讀取隔離。 若要確認，唯一的方式是兩種方式都實作，並測量特定應用程式的效能差異。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[資源](resources.md)

 

 




