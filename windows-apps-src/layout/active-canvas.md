---
author: Jwmsft
Description: "一種包含內容區域和命令區域的模式，適用於單一檢視應用程式或模態體驗，例如相片檢視器/編輯器、文件檢視器、地圖、繪畫或讓其他利用自由捲動檢視的應用程式。"
title: "主動式畫布配置模式"
ms.assetid: 4D768472-64D6-406C-9E87-F750F6B981A0
label: TBD
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 1782540069254f30b241022b687f493110d3b280
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="active-canvas-layout-pattern"></a>主動式畫布配置模式

主動式畫布是一種包含內容區域和命令區域的模式。 它適用於單一檢視應用程式或模態體驗，例如相片檢視器/編輯器、文件檢視器、地圖、繪畫或讓其他利用自由捲動檢視的應用程式。 對於要採取的動作，根據您需要的動作之數量和類型，主動式畫布可搭配使用命令列或只有按鈕。

## <a name="examples"></a>範例

這個相片編輯應用程式的設計強調主動式畫布模式，左側是行動裝置範例，右側則是桌面範例。 影像編輯表層是一個畫布，底部的命令列則包含應用程式的所有內容相關動作。

![使用主動式畫布模式的相片編輯器範例](images/uap-photo-pc-phone-700.png)

這個捷運地圖應用程式採用的主動式畫布在上方有一個簡單的帶狀 UI，其中只有兩個動作和一個搜尋方塊。 如右側影像所示，內容相關動作會顯示在內容功能表。

![採用主動式畫布模式的地圖應用程式範例](images/uap-subway-pc-phone-700.png)


## <a name="implementing-this-pattern"></a>實作此模式

主動式畫布由內容區域和命令區域組成。

**內容區域。**  內容區域通常是可以自由捲動的畫布。 一個應用程式內可以有多個內容區域。

**命令區域。**  如果您要放置很多命令，使用會根據畫面大小而回應的命令列是不錯的選擇。 如果您沒有要放置很多命令，而且也不考慮回應式 UI，則使用能節省空間的按鈕即可。



## <a name="related-articles"></a>相關文章

-   [**應用程式列與命令列**](../controls-and-patterns/app-bars.md)