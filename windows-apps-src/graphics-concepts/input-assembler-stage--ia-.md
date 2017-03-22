---
title: "輸入組合語言 (IA) 階段"
description: "輸入組合語言 (IA) 階段提供基本類型和管線相鄰資料，例如三角形、行及點，包括語意識別碼，藉由減少處理尚未處理的基本類型來讓著色更有效率。"
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- "輸入組合語言 (IA) 階段"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 8bdabf3a49417974acb6a134da07e9702573bf2d
ms.lasthandoff: 02/07/2017

---

# <a name="input-assembler-ia-stage"></a>輸入組合語言 (IA) 階段


輸入組合語言 (IA) 階段提供基本類型和管線相鄰資料，例如三角形、行及點，包括語意識別碼，藉由減少處理尚未處理的基本類型來讓著色更有效率。

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>用途和使用


輸入組合語言 (IA) 階段的目的是從使用者填入緩衝讀取基本類型資料 (點、行及三角形)，然後將資料分配到將由其他管線階段使用的基本類型，並且附加[系統產生的值](https://msdn.microsoft.com/library/windows/desktop/bb509647)來讓著色更有效率。 系統產生的值為也稱為語意的文字字串。 可程式化著色器模型是從使用系統產生的值 (例如基本類型識別碼、執行個體識別碼或頂點識別碼) 的一般著色器核心建構，如此，著色器階段可以減少只處理這些尚未處理的基本類型、執行個體或頂點。

IA 階段可以將頂組合到數個不同的[基本類型](primitive-topologies.md) (例如行清單、三角形連環或相鄰基本類型)。 基本類型 (例如相鄰三角形清單和相鄰行清單) 支援[幾何著色器 (GS) 階段](geometry-shader-stage--gs-.md)。

應用程式只能在在幾何著色器中看到相鄰資訊。 如果幾何著色器已叫用包括相鄰關係的三角形，輸入資料將在每個三角形包含 3 個頂點，而每個三角形有 3 個相鄰資料頂點。

當要求 IA 階段輸出相鄰資料時，輸入資料必須包含相鄰資料。 這可能需要提供假頂點 (形成變質三角形)，或可能在其中一個頂點屬性標明頂點是否存在。 這也需由幾何著色器偵測和處理，雖然在點陣化階段將會揀選變質幾何。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>輸入


IA 階段從記憶體讀取資料︰使用者填入緩衝區的基本資料 (點、行及/或三角形)。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>輸出


IA 階段將資料組合到基元，並附加系統產生的值，而如同基元的輸出將被[頂點著色器 (VS) 階段](vertex-shader-stage--vs-.md)使用，再由其他管線階段使用。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本節內容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[基本拓撲](primitive-topologies.md)</p></td>
<td align="left"><p>Direct3D 支援數個基本拓撲，其定義管線如何轉譯及呈現頂點，例如點清單、行清單和三角形連環。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用系統產生的值](using-system-generated-values.md)</p></td>
<td align="left"><p>系統產生的值由輸入組合語言 (IA) 階段產生 (根據使用者提供輸入[語意](https://msdn.microsoft.com/library/windows/desktop/bb509647)) 允許特定著色器作業效率。 藉由附加像執行個體識別碼 (顯現於[頂點著色器 (VS) 階段](vertex-shader-stage--vs-.md))、頂點識別碼 (顯現於 VS) 或基本識別碼 (顯現於[幾何著色器 (GS) 階段](geometry-shader-stage--gs-.md)/[像素著色器 (PS) 階段](pixel-shader-stage--ps-.md))，後續著色器階段可能會尋找這些系統值，以在該階段進行最佳化處理。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[圖形管線](graphics-pipeline.md)

 

 




