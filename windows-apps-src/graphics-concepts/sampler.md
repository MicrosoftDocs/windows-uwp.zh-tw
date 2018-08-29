---
title: 取樣器
description: 取樣是從紋理或其他資源讀取輸入值的程序。 「取樣器」是可讀取資源的任何物件。
ms.assetid: 7ECE13BB-9FC5-44A3-B1B2-2FE163F1D627
keywords:
- 取樣器
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: ec409f734e2f3e338f6bfe064a0bed29936980f6
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "1043517"
---
# <a name="sampler"></a>取樣器


取樣是從紋理或其他資源讀取輸入值的程序。 「取樣器」是可讀取資源的任何物件。

從紋理取樣和轉譯到螢幕區域有許多問題與成品。 例如，如果轉譯區域是 50x50 個像素，而紋理是 16x16 像素或 256x256 像素，則需要套用一些相當多的紋理延伸或壓縮。 因為這不相符的大小會導致不受歡迎的成品，所以篩選技術可用來緩和這些成品。 將小紋理用於較大轉譯區域的常見篩選方式是「雙線性」篩選。

另一個問題發生於轉譯區域的檢視角度非常傾斜（例如，因為檢視角度，256x256 紋理轉譯到 100 像素寬但只有 5 像素深的區域）。 在這種情況下，通常套用「非等向性」篩選。 非等向性篩選比雙線性篩選提供更好的影像品質，因為它會移除鋸齒效果而不過度模糊，但在運算上比較昂貴。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[檢視](views.md)

 

 



