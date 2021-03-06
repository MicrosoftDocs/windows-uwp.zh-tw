---
description: 您可以為您在 Microsoft Store 中發佈的 App 或附加元件產生促銷代碼。
title: 產生促銷碼
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 促銷代碼, 促銷碼, 預付碼, 預付代碼
ms.localizationpriority: medium
ms.openlocfilehash: b9640ad9ddc00741677b8a1341c12d0489564368
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029641"
---
# <a name="generate-promotional-codes"></a>產生促銷碼


[合作夥伴中心](https://partner.microsoft.com/dashboard) 可讓您產生已在 Microsoft Store 中發佈之應用程式或附加元件的促銷代碼。 促銷代碼是讓有影響力使用者可以免費存取您的 App 或附加元件的簡單方式。 您也可以使用促銷碼來處理客戶服務案例，方法是提供使用者免費存取您的 App 或附加元件，搭配 Windows 10 提供[搶鮮版 (Beta) 測試](beta-testing-and-targeted-distribution.md)。 

每個促銷代碼都有對應的唯一兌換 URL，可供客戶按一下以兌換程式碼，並從 Microsoft Store 安裝您的應用程式或附加元件。  請注意，您的應用程式必須通過 [應用程式認證程序](the-app-certification-process.md)的最終發佈階段，客戶才能兌換促銷碼以安裝它。

您可以 (產生單一使用的程式碼，並將其散發給每個客戶) ，也可以選擇產生可由指定數目的客戶多次使用的程式碼。

> [!TIP]
> 您可以使用[目標式推播通知](send-push-notifications-to-your-apps-customers.md)散發促銷代碼給特定客戶區隔。 當您這樣做，請務必使用允許多位客戶使用相同代碼的促銷碼。


## <a name="promotional-code-policies"></a>促銷碼原則

請注意下列促銷代碼原則：

-   您可以為您發佈到 Microsoft Store 的任何 App 或附加元件 (但訂閱附加元件除外) 產生促銷代碼。 客戶可以在您的應用程式或附加元件所支援的任何版本 Windows 上兑換代碼。
-   針對遊戲：
    - 您可以為每個遊戲產生最多5000的促銷代碼。
    - 為遊戲產生的促銷代碼永遠不會過期。
- 針對所有其他類型的應用程式或附加元件：
    - 在任何六個月的期間內，您最多可以產生1600個單一使用的促銷代碼，或任意數量的多重使用代碼，讓兌換次數總數不超過1600。
    - 6個月的期間開始時，您會建立第一個促銷代碼，並持續6個月，不論您是否在代碼上設定較早的到期日。
    - 在現有的六個月期間內建立的任何程式碼，將會算在該期間內產生的程式碼數目，即使在該期間結束後會過期 (比方說，如果您在六個月的時間範圍內的最後一天產生程式碼，則在建立後的6個月內仍會有效。 ) 
-   您必須遵循 [應用程式開發人員合約](/legal/windows/agreements/app-developer-agreement)中定義的需求，包括小節 **3k。促銷碼** 。

> [!NOTE]
> 即使客戶無法使用您的應用程式，也可以使用促銷代碼 (也就是如果您已選取 [ **讓此產品可供使用但無法在商店中** 探索]，則具有 **直接連結的任何客戶都可以看到產品的 Store 清單，但只有在您之前擁有產品，或有促銷代碼，並** 在您提交的 [可搜尋性] [區段中使用](choose-visibility-options.md#discoverability) [Windows 10 裝置] 選項時，才可以下載此產品。)  使用此選項時，客戶必須在 Windows 10 (包括 Xbox) ，才能使用促銷代碼取得您的產品。


## <a name="order-promotional-codes"></a>訂購促銷碼

訂購應用程式或附加元件的促銷代碼：

1.  在 [合作夥伴中心](https://partner.microsoft.com/dashboard)的左側導覽功能表中，展開 [ **吸引** ]，然後選取 [ **促銷代碼** ]。

2.   在 **\[促銷碼\]** 頁面上，按一下 **\[訂購代碼\]** 。

3.  在 **\[新促銷碼訂購\]** 頁面上，輸入下列項目：
    -   選取您要產生代碼的 App 或附加元件 (請注意，您無法產生訂閱附加元件的促銷代碼)。
    -   指定訂單的名稱。 您可以使用這個名稱，在檢閱您的促銷碼使用資料時區分不同代碼的訂購。
    -   選取訂單類型。 您可以選擇產生一組每個可以使用一次的促銷碼，或者您可以選擇產生一個可以多次使用的促銷碼。
    -   指定要訂購的代碼數目 (如果產生一組代碼)，或代碼可兌換的次數 (如果產生一組代碼供多次使用)。
    -   指定促銷碼應生效的時間。 若要選擇特定的開始日期和時間，請清除 [ **代碼會立即生效** ] 核取方塊。 否則，雖然您的產品必須已完成發佈程式，才能讓客戶使用程式碼) ，但是程式碼會立即變成作用中 (。
    -   指定促銷碼過期的時間。 若要選擇早於 6 個月的特定到期日期和時間，請清除 [ **代碼會在 6 個月之後到期** ] 核取方塊。

4.  按一下 [ **訂購代碼** ]。 您會再回到 [ **促銷碼** ] 頁面，您就可以在該應用程式的促銷碼摘要表中看到新訂單。


## <a name="download-and-distribute-promotional-codes"></a>下載及發佈促銷碼

若要下載完成的促銷碼訂單並發佈代碼給客戶：

1.  在 [合作夥伴中心](https://partner.microsoft.com/dashboard)的左側導覽功能表中，展開 [ **吸引** ]，然後選取 [ **促銷代碼]。**
2.  按一下您的促銷碼訂單的 **\[下載\]** 連結，然後將所產生的檔案儲存到您的電腦。 這個檔案包含您促銷碼訂單的相關資訊，採定位字元分隔值 (.tsv) 格式。
3.  在您選擇的編輯器中開啟 .tsv 檔案。 為獲得最佳體驗，請在可以使用表格結構顯示資料的應用程式中開啟 .tsv 檔案，如 Microsoft Excel。 不過，您可以在任何文字編輯器中開啟檔案。

    檔案包含每個代碼的下列資料欄：

    -   **產品名稱** ：與代碼相關聯的應用程式或附加元件的名稱。
    -   **訂單名稱** ：產生此代碼的訂單名稱。
    -   **促銷碼** ：代碼本身。 這是以連字號分隔的英數字元 5x5 字串。 例如︰DM3GY-M2GYM-6YMW6-4QHHT-23W2Z
    -   **可兌換的 URL** ：客戶可以用來兌換代碼並安裝您的應用程式或附加元件的 URL。 URL 的格式如下： https://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt ;p romotional_code>
    -   **開始日期** ：此代碼生效的日期。
    -   **到期日期** ：此代碼的到期日期。
    -   **代碼識別碼** ：此代碼的唯一識別碼。
    -   **訂購識別碼** ：產生此代碼的訂單的唯一識別碼。
    -   **提供給** ：空白欄位，您可以填入能識別提供代碼的目標使用者的值。
    -   **可用** ︰代碼仍可兌換的次數 (檔案產生時)。
    -   **已兌換** ：已兌換的代碼數目 (檔案產生時)。

4.  透過您偏好的任何通訊格式 (例如目標式通知、電子郵件、SMS 訊息或列印卡片)，將可兌換 URL 發佈給您的客戶。 我們建議您的通訊內容包含下列：
    -   促銷碼適用的應用程式或附加元件的說明，以及選擇性地說明客戶為什麼會收到代碼。
    -   可兌換的代碼 URL。
    -   引導客戶瀏覽可兌換的 URL，使用他們的 Microsoft 帳戶登入並依照指示來下載並安裝您的應用程式的指示。


## <a name="code-redemption-user-experience"></a>代碼兌換使用者經驗

將促銷代碼 (或其兌換 URL) 散發給客戶之後，他們就可以按一下 URL 以免費取得產品。 按一下可兌換 URL 將會啟動經驗證的 **\[兌換您的代碼\]** 頁面，網址：<https://account.microsoft.com/billing/redeem>。 這個頁面包含使用者即將兌換應用程式的描述。 如果客戶未使用其 Microsoft 帳戶登入，可能會提示他們登入。 您的客戶也可以造訪 <https://account.microsoft.com/billing/redeem> 並直接輸入代碼。

> [!IMPORTANT]
> 我們建議您在產品完成發行程序之前，不要將促銷碼散發給您的客戶 (即使您已選取 [ **在市集推出此產品，但不供搜尋** ])。 如果客戶嘗試使用尚未發行的產品的促銷碼，將會看到錯誤。

客戶按一下 **\[兌換\]** 後， Microsoft Store 將會開啟應用程式的概觀頁面 (如果他們使用 Windows 10 或 Windows 8.1 裝置)，並且可以按一下 **\[安裝\]** 免費下載並安裝應用程式。 如果客戶是在未安裝 Microsoft Store 的電腦或裝置上，連結將會啟動應用程式的 Microsoft Store 頁面。 代碼將會套用到客戶的 Microsoft 帳戶，方便他們之後在 Windows 裝置 (與同一個 Microsoft 帳戶相關聯) 上免費下載應用程式。

> [!NOTE]
> 在某些情況下，客戶可能會看到 [ **購買** ] 按鈕，而不是 [ **安裝** ]，即使已透過促銷代碼成功兌換應用程式也一樣。 客戶可以按一下 [ **購買** ] 免費安裝應用程式。


## <a name="review-your-promotional-codes"></a>檢閱您的促銷碼

若要查看您的應用程式和附加元件的促銷程式碼訂單詳細摘要，請流覽至合作夥伴中心左側導覽功能表中 (的 [ **促銷代碼** ] 頁面，展開 [ **吸引** ]，然後選取 [ **促銷代碼** ]) 。 您可以檢閱下列有關您所有目前和非使用中促銷碼的詳細資料：
-   訂單名稱
-   應用程式或附加元件
-   開始日期
-   到期日期
-   可用
-   已兌換

您也可以從此表格 [下載](#download-and-distribute-promotional-codes) 訂單。

 
