---
title: 著色器資源檢視 (SRV) 和未排序存取檢視 (UAV)
description: 著色器資源檢視通常會以著色器可存取的格式包裝紋理。 未排序存取檢視提供類似的功能，但啟用任何順序的紋理（或其他資源）讀取和寫入。
ms.assetid: 4505BCD2-0EDA-40F2-887C-EC081FE32E8F
keywords:
- 著色器資源檢視 (SRV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 213ff4e2a120c91211720d887ab8f777b9265106
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "1043537"
---
# <a name="shader-resource-view-srv-and-unordered-access-view-uav"></a>著色器資源檢視 (SRV) 和未排序存取檢視 (UAV)


著色器資源檢視通常會以著色器可存取的格式包裝紋理。 未排序存取檢視提供類似的功能，但啟用任何順序的紋理（或其他資源）讀取和寫入。

包裝單一紋理可能是最簡單的著色器資源檢視格式。 更複雜的範例是子資源（個別陣列、平面或 mipmap 紋理色彩）、3D 紋理、1D 紋理色彩等等的集合。

未排序存取檢視在效能上成本略多，但允許 (例如) 同時寫入和讀取紋理。 這可讓圖形管線重複使用更新紋理，做為某些其他用途使用。 著色器資源檢視僅供唯讀（這是最常見的資源使用方式）。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[檢視](views.md)

 

 



