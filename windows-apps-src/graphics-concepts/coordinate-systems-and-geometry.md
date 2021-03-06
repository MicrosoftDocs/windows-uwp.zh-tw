---
title: 座標系統與幾何
description: 要進行 Direct3D 應用程式的程式設計，就需要在工作上熟悉 3D 幾何原則。 本節介紹建立 3D 場景所需的最重要幾何概念。
ms.assetid: E82EB0A9-0678-496B-96B3-8993BA580099
keywords:
- 座標系統與幾何
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 962002d635c3e6edbf1f9581a4cbc57fbd5b1d96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646293"
---
# <a name="coordinate-systems-and-geometry"></a>座標系統與幾何


要進行 Direct3D 應用程式的程式設計，就需要在工作上熟悉 3D 幾何原則。 本節介紹建立 3D 場景所需的最重要幾何概念。

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
<td align="left"><p><a href="coordinate-systems.md">座標系統</a></p></td>
<td align="left"><p>3D 圖形應用程式通常會使用下列兩個笛卡兒座標系統中的其中一種︰左手系或右手系。 在這兩個座標系統中，正 x 軸指向右側，而正 y 軸指向上方。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="primitives.md">基本項目</a></p></td>
<td align="left"><p>3D「原始物件」是形成單一 3D 實體的許多頂點。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="face-and-vertex-normal-vectors.md">人臉與頂點的法向量</a></p></td>
<td align="left"><p>每個網格中的平面都有一個垂直單位法向量。 向量的方向是由定義頂點的順序，和座標系統是否為右手系或左手系而決定的。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rectangles.md">矩形</a></p></td>
<td align="left"><p>在整個 Direct3D 及 Windows 程式設計中，螢幕上的物件會以週框的形式參照。 週框的側邊永遠和螢幕的側邊平行，以讓矩形可由兩個點描述，這兩點為左上角和右下角。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-interpolation.md">三角形內插補點</a></p></td>
<td align="left"><p>在轉譯期間，管線會在每個矩形上插入頂點資料。 頂點資料可以是各式各樣的資料，可能包括 (但不限於)︰擴散色彩、反射色彩，擴散 Alpha (三角形不透明度)、反射 Alpha，以及一個霧因素。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vectors--vertices--and-quaternions.md">向量、 頂點和四元數</a></p></td>
<td align="left"><p>在整個 Direct3D 中，頂點描述位置及方向。 原始物件中的每個頂點都由給予它位置、色彩、紋理座標的向量，以及給予它方向的標準向量所述。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="transforms.md">轉換</a></p></td>
<td align="left"><p>在 Direct3D 中，轉換引擎負責將幾何推入固定函式幾何管線。 它在世界中尋找模型和檢視器，將頂點投影在螢幕上顯示，並將頂點裁剪至檢視區。 轉換引擎也會執行光線計算，以判斷每個頂點上的擴散和反射元件。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="viewports-and-clipping.md">檢視區和裁剪</a></p></td>
<td align="left"><p>「檢視區」是 3D 場景的投影目標二維 (2D) 矩形。 在 Direct3D 中，矩形在 Direct3D 表面中以座標的形式存在，系統會使用矩形作為轉譯目標。 投影轉換將頂點轉換至用於檢視區的座標系統。 檢視區也用於指定轉譯目標表面 (場景轉譯目標) 上深度值的範圍 (通常是 0.0 到 1.0)。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[Direct3D 圖形學習指南](index.md)

 

 




