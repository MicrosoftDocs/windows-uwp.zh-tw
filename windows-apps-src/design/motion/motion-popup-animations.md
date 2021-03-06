---
description: 使用快顯動畫顯示和隱藏飛出視窗的快顯 UI 或自訂快顯的 UI 元素。 快顯元素是顯示在應用程式內容之上的容器，如果使用者點選或按一下快顯元素以外的地方則會關閉。
title: 快顯 UI 動畫
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5d09363d6ecd2eb5909c98da316c7c1395ebec03
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034121"
---
# <a name="pop-up-ui-animations"></a>快顯 UI 動畫



使用快顯動畫顯示和隱藏飛出視窗的快顯 UI 或自訂快顯的 UI 元素。 快顯元素是顯示在應用程式內容之上的容器，如果使用者點選或按一下快顯元素以外的地方則會關閉。

> **重要 API** : [**PopInThemeAnimation 類別**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)、 [**PopupThemeTransition 類別**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>可行與禁止事項


-   使用快顯動畫顯示或隱藏非應用程式頁面本身一部分的自訂快顯 UI 元素。 Windows 提供的常用控制項已經內建這些動畫。
-   請勿對工具提示或對話方塊使用快顯動畫。
-   請勿使用快顯動畫顯示或隱藏應用程式主要內容中的 UI；僅使用快顯動畫顯示或隱藏會顯示在應用程式主要內容最上方的快顯容器。

## <a name="related-articles"></a>相關文章

* [動畫總覽](./xaml-animation.md)
* [讓快顯 UI 產生動畫效果](/previous-versions/windows/apps/jj649433(v=win.10))
* [快速入門：使用動畫庫讓 UI 產生動畫效果](/previous-versions/windows/apps/hh452703(v=win.10))
* [**PopInThemeAnimation 類別**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**PopOutThemeAnimation 類別**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**PopupThemeTransition 類別**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 
