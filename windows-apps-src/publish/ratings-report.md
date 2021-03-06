---
description: 合作夥伴中心中的評等報告可讓您瞭解客戶如何在商店中為您的應用程式評分。
title: 評分報告
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10、uwp、評等、費率、星星、星星、星級
ms.localizationpriority: medium
ms.openlocfilehash: 4d1d0f9ceaa660245c537c4caf1f1770f1c5be6d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030341"
---
# <a name="ratings-report"></a>評分報告


[合作夥伴中心](https://partner.microsoft.com/dashboard)中的 **評** 等報告可讓您瞭解客戶如何在商店中為您的應用程式評分。 

您可以在合作夥伴中心中查看這項資料，或 [下載報表](download-analytic-reports.md) 以離線查看。 或者，您可以使用[Microsoft Store 分析 REST API](../monetize/access-analytics-data-using-windows-store-services.md)中的 [[取得應用程式評論](../monetize/get-app-reviews.md)] 方法，以程式設計方式取出此資料。

> [!TIP]
> 如需快速查看過去 30 天為您的所有應用程式提供的評論、評分和使用者意見反應，請展開左側瀏覽功能表的 **\[互動\]** ，然後選取 **\[評論和意見反應\]** 。 

## <a name="apply-filters"></a>套用篩選條件

在頁面頂端附近，您可以選取您想要顯示評論的時間週期。 預設選項是 **\[存留期\]** ，但您可以選擇顯示 30 天、3 個月、6 個月或 12 個月期間或指定之自訂日期範圍的評論。 請注意， **評分明細** 和 **一段時間的平均評分** 圖表一律會顯示過去 12 個月的資料。這些期間選項將不會影響那些圖表。

您可以展開 **\[篩選條件\]** ，依以下選項篩選此頁面上顯示的評論。 這些篩選條件不會套用到 **評分明細** 和 **一段時間的平均評分** 圖表。

-   **市場** ：預設設定為 **\[所有市場\]** 。 如果您只想在此頁面上顯示某特定市場中客戶所給予的評等，則可選擇該市場。
-   **裝置類型** ：預設篩選為 **\[所有裝置\]** 。 如果您只想在此頁面上顯示使用該類型之使用者所給予的評等，則可以選擇特定裝置類型。
-   **作業系統版本** ：預設設定為 **\[全部\]** 。 如果您想要讓此頁面只顯示客戶在該作業系統版本上留下的評等，您可以選擇特定的作業系統版本。
-   **類別名稱** ：預設篩選為 **\[全部\]** 。 您可以選擇只顯示與已識別為屬於特定 [審核見解類別](reviews-report.md#insight-categories) 的評論相關聯的評等，只顯示與該類別相關的評論。 

> [!TIP]
> 如果您沒有在頁面上看到任何評等，請檢查以確定您的篩選器未排除所有評等。 例如，如果您依應用程式不支援的目標 OS 進行篩選，將不會看到任何評等。


## <a name="rating-breakdown"></a>評等明細

**評等細目** 圖顯示： 
- app 的平均評分/星級評等。
- 在過去 12 個月 app 的評分總數。
- 每個星級評等的評分總數。
- 在過去 12 個月每種評分類型 (全新或已修改) 的評分數目 (依星級評等)。
 - **\[全新評分\]** 是客戶已經提交但尚未變更的評分。
 - **\[已修改的評分\]** 是客戶已透過任何方式變更的評分，甚至只變更評論的文字。

> [!TIP]
> 客戶在市集中看見的平均評分會將客戶的市場和裝置類型納入考量，因此，可能會與您在此報告中所看見的內容不同。


## <a name="average-rating"></a>平均評等

**平均評** 等圖表會顯示應用程式的平均評等在過去12個月內的平均評等有何變化。

**平均評****等圖表會** 顯示在指定的一周內客戶如何分級應用程式，而不是計算過去12個月期間剩餘的平均評) 等 (。 這可協助您辨識出趨勢或判斷評分是否受更新或其他因素影響。

## <a name="rating-by-market"></a>依市場評分

**依市場評** 等會顯示所選時段內每個市場的平均評等明細。 根據預設，我們會在視覺 **地圖** 表單中顯示這項資料，但您也可以在右上角切換控制項，以將它視為 **資料表** 來查看。

**資料表** 視圖會一次顯示五個市場，並依字母順序或最高/最低的平均評等進行排序。 您也可以下載此圖表的資料，以同時查看所有市場的資訊。

您也可以依 **評** 等來篩選此圖表。 系統會檢查所有星級評等的預設評論，但您可以從1到5顆星 (檢查和取消核取特定評等) 如果您只想要查看與特定星級分級相關的評論。
