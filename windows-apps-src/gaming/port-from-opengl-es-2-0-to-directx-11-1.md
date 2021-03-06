---
title: 從 OpenGL ES 2.0 移植到 Direct3D 11
description: 包含適用於將 OpenGL ES 2.0 圖形管線移植到 Direct3D 11 與 Windows 執行階段的文章、概觀及逐步解說。
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, games, opengl, direct3d 11, port, graphics, 遊戲, 連接埠, 圖形
ms.localizationpriority: medium
ms.openlocfilehash: 9215c06c97fb8e45a445ccd3691f484a6b2ad513
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258458"
---
# <a name="port-from-opengl-es-20-to-direct3d-11"></a>從 OpenGL ES 2.0 移植到 Direct3D 11



包含適用於將 OpenGL ES 2.0 圖形管線移植到 Direct3D 11 與 Windows 執行階段的文章、概觀及逐步解說。

> **請注意**   用來移植 OpenGL ES 2.0 專案的中繼步驟是使用 MICROSOFT STORE 的角度。 ANGLE 可讓您透過將 OpenGL ES API 呼叫轉譯為 DirectX 11 API 呼叫，在 Windows 上執行 OpenGL ES 內容。 如需關於 ANGLE 的詳細資訊，請移至[適用於 Microsoft Store 的 ANGLE Wiki](https://github.com/microsoft/angle/wiki)。

 

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
<td align="left"><p><a href="map-concepts-and-infrastructure.md">將 OpenGL ES 2.0 對應至 Direct3D 11。1</a></p></td>
<td align="left"><p>當您第一次開始進行將圖形架構從 OpenGL ES 2.0 移植到 Direct3D 的程序時，請務必熟悉這些 API 間的重要差異。 本節中的主題可以協助您計劃移植策略，以及當您將圖形處理移至 Direct3D 時必須進行的 API 變更。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md">如何：將簡單的 OpenGL ES 2.0 轉譯器移植到 Direct3D 11。1</a></p></td>
<td align="left"><p>針對此移植練習，我們將從頭開始：將適用於頂點已著色旋轉立方體的簡單轉譯器從 OpenGL ES 2.0 帶入 Direct3D，如此讓它符合 Visual Studio 2015 的 DirectX 11 App (通用 Windows) 範本。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="opengl-es-2-0-to-directx-11-1-reference.md">OpenGL ES 2.0 至 Direct3D 11.1 參考</a></p></td>
<td align="left"><p>從 OpenGL ES 2.0 移植到 Direct3D 11 時，可以使用這些參考主題來查詢 API 對應和簡短的程式碼範例。</p></td>
</tr>
</tbody>
</table>

 

 

 




