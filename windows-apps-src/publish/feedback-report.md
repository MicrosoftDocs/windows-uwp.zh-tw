---
description: 合作夥伴中心的意見報告可讓您查看 Windows 10 客戶透過意見反應中樞提交的問題、建議和 upvotes。
title: 意見反應報告
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2ad8cb47a2fe8df1ebbf1d1b67659a85b682d2b4
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034791"
---
# <a name="feedback-report"></a>意見反應報告

> [!WARNING]
> 2020年4月15日的意見反應報告已淘汰，在4月 15 2020 日之後，將不再支援此報表。 此報表上的資料將不會在此日期之後重新整理，且未來將會移除報表，而不會另行通知。 您可以繼續在意見反應中樞中直接查看從客戶收到的意見反應。

合作夥伴中心的 **意見報告** 可讓您查看 Windows 10 客戶透過意見反應中樞提交的問題、建議和 upvotes。 您可以在合作夥伴中心中查看這項資料，或將資料匯出以離線查看。

> [!NOTE]
> 您也可以從此報告直接[回應意見反應](respond-to-customer-feedback.md)，讓客戶知道您正在聆聽。

鼓勵客戶提供有關您 App 的意見反應，是了解對他們而言最重要的問題和功能的不錯方式。 當客戶知道他們可以直接將意見反應直接傳送給您時，較不可能在市集中留下負面評論的意見反應。

您可以使用 [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) 中的意見反應 API，讓客戶[直接從您的 App 啟動意見反應中樞](../monetize/launch-feedback-hub-from-your-app.md)。 請記住，在支援意見反應中樞的 Windows 10 裝置下載您 App 的所有客戶，都可以使用意見反應中樞 App 留下意見反應。 基於此原因，即使您未特別要求應用程式內的意見反應，您也可能會在這份報告中看到客戶的意見反應。

使用 [套件試驗](package-flights.md)時，意見反應也會很有説明，因為 **意見** 報告會顯示每個客戶在離開意見反應時已安裝在其裝置上的特定套件。

> [!TIP]
> 若要快速查看過去30天內所有應用程式的評論、評等和使用者意見反應，請展開左側導覽功能表中的 [ **參與** ]，然後選取 **評論和意見反應。** 


## <a name="apply-filters"></a>套用篩選條件

在頁面頂端附近，您可以選取您想要顯示資料的時間週期。 預設選項是 **\[存留期\]** ，但是您可以選擇顯示 30 天、3 個月、6 個月或 12 個月的資料。

您也可以展開 **\[篩選條件\]** ，依以下選項篩選此頁面上的所有資料。

- **意見反應類型** ：預設設定為 **\[全部\]** 。 您可以選取 [ **問題** ] 或 [ **建議** ] 僅顯示該類型的意見反應。
- **裝置類型** ：預設設定為 [ **所有裝置** ]。 如果您只想在此頁面上顯示使用該類型之客戶所給予的意見反應，則可以選擇特定裝置類型。
- **套件版本** ：預設設定為 **\[所有套件\]** 。 您可以選取其中一個套件，僅顯示在留下意見反應時使用該特定套件的客戶所留下的意見反應。
- **市場** ：預設設定為 **\[所有市場\]** 。 您可以選擇特定市場，僅顯示來自該市場中客戶的意見反應。
- **群組** ：預設設定為 [ **全部** ]。 您可以選擇僅檢視 [Windows 測試人員](https://insider.windows.com)所提交的意見反應。

> [!TIP]
> 如果您在頁面上看不到任何意見反應，請檢查以確定您的篩選並未排除所有意見反應。 例如，如果您依應用程式不支援的 **\[裝置類型\]** 進行篩選，則看不到任何意見反應。


## <a name="viewing-feedback-details"></a>檢視意見反應詳細資料

在這份報告，您會看到您客戶留下的個人意見反應。 在每個項目的意見反應文字的左邊，您會看到其他客戶在意見反應中樞中附議該意見反應的次數。 您可以使用三種方式來排序意見反應︰

- **附議** (預設值)：顯示其他客戶已附議的意見反應，而收到最多附議的意見反應顯示在最前面。
- **新鮮貨** ：顯示其他客戶在最後七天附議的意見反應，而取得最新活動的意見反應顯示在最前面。
- **最近** ︰顯示所有意見反應，而最近留下的意見反應顯示在最前面。

在每個意見的旁邊，您會看到留下意見反應的日期，以及意見反應的類型。 您也會看到客戶的市場，也就是在使用者離開意見反應時所使用的裝置上安裝的特定套件、該裝置的類型，以及 **Windows 測試人員** 如果客戶提交意見是 Windows 測試人員計畫的成員。

您也會在此看到[回應意見反應](respond-to-customer-feedback.md)的選項。


## <a name="translating-feedback"></a>翻譯評論

根據預設，不會為您翻譯以慣用語言撰寫的意見反應。 您也可以取消核取頁面篩選附近的 **\[翻譯意見反應\]** 核取方塊，停用意見反應翻譯的功能。

請注意：意見反應是由系統自動翻譯，結果不一定正確。 我們也提供原文，供您與翻譯比較，或透過其他方式翻譯。


## <a name="launching-feedback-hub-directly-from-your-app"></a>從您的應用程式直接啟動意見反應中樞

如前所述，建議直接在您的應用程式中納入意見反應中樞的連結，鼓勵客戶提供意見反應。 如需詳細資訊，請參閱[從您的應用程式啟動意見反應中樞](../monetize/launch-feedback-hub-from-your-app.md)。
