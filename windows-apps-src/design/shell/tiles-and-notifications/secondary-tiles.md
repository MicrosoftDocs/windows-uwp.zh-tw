---
description: 次要磚可讓使用者將您應用程式中的特定內容與深度連結釘選到其 \[開始\] 功能表，讓使用者未來能夠輕鬆地存取您應用程式中的內容。
title: 次要磚
label: Secondary tiles
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, uwp, secondary tiles, 次要磚
ms.localizationpriority: medium
ms.openlocfilehash: 066a6dcb3683e2e55f7452b1f09bb834157aee62
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034501"
---
# <a name="secondary-tiles"></a>次要磚


次要磚可讓使用者將您應用程式中的特定內容與深度連結釘選到其 \[開始\] 功能表，讓使用者未來能夠輕鬆地存取您應用程式中的內容。

![次要磚螢幕擷取畫面](images/secondarytiles.png)

例如，使用者可將許多特定位置的天氣釘選在其 \[開始\] 功能表，如此可 (1) 輕鬆一覽動態磚所提供目前天氣的即時資訊，(2) 快速進入其所關注特定城市的天氣。 使用者也可釘選特定的股票、新聞文章，及更多對使用者而言重要的項目。

透過將次要磚新增到您的應用程式，您可以快速且有效率地協助使用者一再使用您的應用程式，並因次要磚所帶來的便利性而增加使用者的回頭率。

**只有使用者才能釘選次要磚；未經使用者的核准，應用程式無法以程式設計的方式釘選次要磚** 。 使用者必須明確地按一下您應用程式內的 \[釘選\] 按鈕 (這也是您之後使用 API 要求建立次要磚的位置)，接著系統會顯示對話方塊要求使用者確認是否想要將次要磚釘選。

## <a name="quick-links"></a>快速連結

| 發行項 | 描述 |
| --- | --- |
| [次要磚指導方針](secondary-tiles-guidance.md) | 了解您使用次要磚的時機和位置。 |
| [釘選次要磚](secondary-tiles-pinning.md) | 了解如何釘選次要磚。 |
| [從桌面應用程式釘選](secondary-tiles-desktop-pinning.md) | 傳統型應用程式可以釘選次要磚，因為這是傳統型橋接器！ |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>次要磚與主要磚比較

次要磚與單一父項應用程式相關聯， 並釘選到 \[開始\] 功能表，讓使用者有一致且有效的方法直接啟動至父項應用程式的常用區域， 這可以是父系應用程式中包含經常更新內容的一般子區段，也可以是應用程式中特定區域的深度連結。

次要磚案例的範例有：

* 天氣應用程式中特定城市的氣象更新
* 行事曆應用程式中即將進行活動的摘要
* 社交應用程式中重要連絡人的狀態與更新
* RSS 讀取器中的特定摘要
* 音樂播放清單
* 部落格

使用者想要監視的任何經常變更內容，也是很好的次要磚候選項目。 在釘選次要磚後，使用者便可以透過次要磚收到快速更新，並用於直接啟動父系應用程式。

次要磚與主要磚有許多相似處：

* 兩者使用磚通知來顯示豐富的內容。
* 兩者必須為預設的磚內容包含 150 x 150 像素標誌。
* 兩者可以選擇性地包含其他的標誌大小，以允許有更大的磚大小。
* 兩者可以顯示通知與徽章。
* 兩者可以在 \[開始\] 功能表上重新排列。
* 當應用程式解除安裝時會自動刪除兩者。
* 在鎖定時會顯示其徽章與鎖定的詳細狀態文字。

不過，次要磚與主要磚間也有一些顯著的相異處：

* 使用者可以隨時刪除其次要磚，無須刪除父項應用程式。
* 次要磚可以在執行階段建立。 應用程式磚只能在安裝期間建立。
* 飛出視窗會提示使用者在新增次要磚前先確認。
* 無法以程式設計的方式透過向使用者要求來為鎖定畫面選取次要磚。 使用者必須透過 [電腦設定] 中的 [個人化] 頁面，手動新增次要磚。

對於傳送通知，會為搭配次要磚使用的磚與徽章更新程式及推播通知通道提供特定的方法。 這些方法與搭配主要磚使用的版本一致。 例如，CreateBadgeUpdaterForApplication 與 CreateBadgeUpdaterForSecondaryTile 比較。


## <a name="guidance-on-secondary-tiles"></a>次要磚指導方針
若要了解您使用次要磚的時機與位置，及其他的使用指導方針，請參閱[次要磚指導方針](secondary-tiles-guidance.md)


## <a name="pinning-secondary-tiles"></a>釘選次要磚
若要了解如何釘選次要磚，請參閱[釘選次要磚](secondary-tiles-pinning.md)。


## <a name="desktop-applications-and-secondary-tiles"></a>傳統型應用程式和次要磚
若要瞭解如何透過傳統型橋接器從您的桌面應用程式使用次要磚，請參閱 [從桌面應用程式釘選次要磚](secondary-tiles-desktop-pinning.md)。
