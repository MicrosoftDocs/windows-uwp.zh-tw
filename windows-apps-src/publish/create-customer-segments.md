---
author: shawjohn
Description: "了解如何建立客戶區隔，讓您可以一部分的客戶群為特定對象來進行促銷或互動。"
title: "建立客戶區隔"
translationtype: Human Translation
ms.sourcegitcommit: c1b97d7ca331bdea101f16d3d59879db30173a0a
ms.openlocfilehash: 6448c366784344b649084a3aa746a2fc8f21d31d

---

# 建立客戶區隔

有時候，您可能想要將一部分的客戶群做為特定對象來進行促銷和互動。 要這樣做，您可以在 Windows 開發人員中心建立一種[客戶群組](create-customer-groups.md) (稱為「區隔」)，其中包含符合您選擇的人口統計或收入條件的 Windows10 客戶。

例如，您可以建立只包含 50 或 50 歲以上客戶的區隔，或包含在 Windows 市集花費超過 $10 的客戶的區隔。 您也可以合併這些條件，建立一個區隔來包含 50 歲以上且在市集中花費超過 $10 的所有客戶。 我們提供幾個區隔範本以協助您開始進行，但是您可以定義和合併您想要的條件。

> 
            **提示**區隔在互動廣告活動中可用來[傳送特定對象的推播通知](send-push-notifications-to-your-apps-customers.md)給一組客戶。

## 建立客戶區隔

1.  在 [Windows 開發人員中心儀表板](https://developer.microsoft.com/dashboard/overview)，選取頂端功能表中的 [客戶]。
2.  在 [客戶群組] 頁面，執行下列其中一項︰
 - 在 [我的客戶群組] 區段，選取 [建立新群組] 以從頭定義區隔。 請確定已選取 [群組類型] 下拉式清單中的 [區隔]。
 - 在 [區隔範本] 區段，選取 [複製以使用預先定義的區隔]，您可以直接使用或依您的需求修改。
3.  在 [包含這個 app 中的客戶] 清單中，選取您的其中一個 app 做為目標。
4.  在 [區隔名稱] 方塊中，選擇您的區隔名稱。
5.  在 [定義包含條件] 區段中，選擇區隔的篩選條件。

    例如，如果您想建立只包含 18 到 24 歲 app 客戶的區隔，請從下拉式清單中選取 [人口統計] [年齡群組] [是] [18 到 24] 篩選條件。

    您可以使用 AND/OR 查詢建置更複雜的區隔，以根據各種屬性包含或排除客戶。 若要新增 OR 查詢，請選取 [+ OR 陳述式]。 若要新增 ADD 查詢，請選取 [新增另一個篩選]。

    因此，如果您想要將該陳述式修改為只包含指定年齡範圍內的男性客戶，您可以選取 [新增另一個篩選]，然後再選取其他篩選條件 [人口統計] [性別] [是] [男性]。 在這個範例中，[區隔定義] 會顯示 [年齡群組 = = 18 到 24 &amp;&amp; 性別 == 男性]。

    ![區隔的篩選條件範例](images/create-segment-inclusions.png)
6. 選取 [儲存]。

> 
            **重要**您將無法使用包含太少客戶的區隔。 如果區隔定義未包含足夠的客戶，您可以調整隔條件，或等到您的 app 有更多符合區隔條件的客戶時再試。

關於客戶區隔，需注意幾件事︰
- 儲存區隔之後，需要 24 小時才能用於[特定對象的推播通知](send-push-notifications-to-your-apps-customers.md)。
- 區隔結果每天會重新整理一次，因此您可能會看到區隔中的客戶總數每天隨著客戶是否符合區隔條件而變化。
- 這些屬性大部分是使用所有歷史資料計算，但仍有些例外。 例如，**App 下載日期**、**廣告活動識別碼**、**市集頁面檢視日期**和**查閱者 URI 網域**限於最近 90 天的資料。
- 區隔只會包含在 Windows10 下載您的 App 的客戶。 如果您的 App 支援舊版的作業系統版本，使用這些舊版作業系統的客戶將不會包含在您建立的任何區隔中。
- 區隔會自動排除 17 歲以下的任何客戶。


## App 統計資料

區隔上的 [App 統計資料] 區段提供有關您 App 的部分資訊，以及您剛才建立的區隔大小。

請注意，**可用的 app 客戶**不會反映實際下載您的 app 的客戶數，而是可包含在區隔內的客戶數 (也就是我們可以判斷符合年齡要求的客戶、已在 Windows10 上下載您的 app 的客戶和與有效的 Microsoft 帳戶相關聯的客戶)。

如果您檢視結果，而**此區隔中的客戶**為**少量**，表示區隔未包含足夠的客戶，區隔會標示為非使用中。 非使用中的區隔無法用於通知或其他功能。 您可以透過執行下列其中一項作業來啟用和使用區隔︰

- 在 [定義包含條件] 區段，調整篩選條件，使區隔包含更多客戶。
- 在 [客戶群組] 頁面的 [非使用中區隔] 區段，選取 [重新整理] 以查看區隔目前是否包含足夠的客戶。 如果從您首次建立區隔起有更多符合區隔條件的客戶已下載您的 app，此策略可能奏效。



<!--HONumber=Nov16_HO1-->

