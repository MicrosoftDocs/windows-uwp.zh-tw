---
description: 瞭解如何使用回應式應用程式內說明作為預設方法，以顯示使用者的說明，以及應用程式內說明的類型。
title: 設計應用程式內說明的指導方針。
label: In-app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6208b71b-37a7-40f5-91b0-19b665e7458a
ms.localizationpriority: medium
ms.openlocfilehash: d7144a72dd3af1c9c902e0dfd401799f50ea55b0
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054168"
---
# <a name="in-app-help-pages"></a>應用程式內說明頁面

在大部分的情況下，說明最好是在應用程式內，於使用者選擇檢視它時顯示。

## <a name="when-to-use-in-app-help-pages"></a>使用應用程式內說明頁面的時機

應用程式內說明應該是顯示使用者說明的預設方法。 它應該用於簡單、易懂且不會對使用者產生新內容的任何說明。 指示、建議以及提示與訣竅都適用於應用程式內說明。

複雜的指示或教學課程不容易快速參考，並且會佔用大量空間。 因此，它們應該裝載在外部，不要納入應用程式本身。

對於基本指示或探索新功能，使用者應不需要尋找其說明。 如果您需要有教育使用者的說明，請使用指示性 UI。

## <a name="types-of-in-app-help"></a>應用程式內說明的類型

應用程式內說明可以有數種形式，但都遵循相同的設計和可用性一般原則。

#### <a name="help-pages"></a>說明頁面

在應用程式內包含個別的說明頁面，是一種能快速且簡單地顯示實用指示的方式。

-   **簡潔︰** 大型說明主題庫十分龐大，不適合應用程式內說明。
-   **一致：** 確定使用者可以使用相同的方式，從您應用程式的任何部分到達您的說明頁面。 他們應該永遠不需要進行搜尋。
-   **使用者會掃視，而非閱讀：** 因為使用者所尋找的說明可能與其他說明主題位在相同的頁面上，請確定他們可以輕易分辨出需要聚焦的部分。


#### <a name="popups"></a>快顯

快顯允許高度內容相關的說明，能顯示與使用者正在嘗試執行之特定工作相關的指示和建議。

-   **聚焦一個問題︰** 快顯中的空間比說明頁面更為受限。 說明快顯需要特別參考單一工作才會生效。
-   **可見度十分重要︰** 因為說明快顯只能從一個位置進行檢視，所以請確定使用者可以不受防礙清楚地看到它們。 如果使用者錯過它，則可能離移開快顯，來搜尋說明頁面。
-   **不要使用太多的資源︰** 說明不應該延遲或讓載入變慢。 在快顯中使用視訊或音訊檔案，或是高解析度的影像，反而更可能為使用者造成不便。

#### <a name="descriptions"></a>說明

在某些情況下，描述很適合用來在使用者檢查某項功能時，為該功能提供更詳細的資訊。 描述與指示性 UI 類似，但主要差異在於指示性 UI 嘗試教導和訓練使用者有關他們不知道的功能，而詳細的描述可增強使用者對已感興趣的應用程式功能的了解。

-   **不教導基本知識︰** 假設使用者已經知道如何使用所描述項目的基本概念。 釐清或提供更進一步的資訊十分有用。 但不告訴他們已經知道的資訊。
-   **描述有趣的互動：** 描述的一種最佳使用方式是教育使用者有關他們已知道的功能如何進行互動。 這可協助使用者深入了解他們已經想要使用的項目。
-   **不造成干擾：** 描述與指示性 UI 一樣，需要避免干擾使用者使用 App 的體驗。

## <a name="related-articles"></a>相關文章

* [應用程式說明的指導方針](guidelines-for-app-help.md)
