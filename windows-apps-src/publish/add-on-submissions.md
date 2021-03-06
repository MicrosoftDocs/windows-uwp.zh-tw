---
description: 瞭解如何提交附加元件（也稱為應用程式內產品）作為客戶可購買之應用程式的補充專案。
title: 附加元件提交
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, iap, App 內購買, 應用程式內產品, iap 提交
ms.localizationpriority: medium
ms.openlocfilehash: 25246d192f40bae096c33fae00feb1bfc2a4c530
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162143"
---
# <a name="add-on-submissions"></a>附加元件提交

附加元件 (有時也稱為應用程式內產品) 是可供客戶購買之 App 的補充項目。 附加元件可以是有趣的新功能、新的遊戲等級，或是您認為會讓使用者參與的任何其他功能。 附加元件不僅是賺錢的絕佳方式，也可以協助促進客戶的互動和投入。

附加元件會透過 [合作夥伴中心](https://partner.microsoft.com/dashboard)發佈，而且需要您擁有使用中的 [開發人員帳戶](https://developer.microsoft.com/store/register)。 您也需要在您的 App 程式碼中[啟用附加元件](../monetize/in-app-purchases-and-trials.md)。

附加元件提交程式中的第一個步驟是在合作夥伴中心中建立附加元件，方法是 [定義其產品類型和產品識別碼](set-your-add-on-product-id.md)。 之後，您將建立提交，讓您可以透過 Microsoft Store 購買附加元件。 您在[提交 App](app-submissions.md) 的同時就可以提交附加元件，也可以分別處理。 App 進到 Store 之後，您可以對附加元件[進行更新](#updating-an-add-on-after-publication)，不需要重新提交 App。

> [!NOTE]
> 本檔的這一節描述如何在合作夥伴中心中提交附加元件。 或者，您可以使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)，將附加元件提交自動化。


## <a name="checklist-for-submitting-an-add-on"></a>提交附加元件的檢查清單

以下是您在建立附加元件提交時提供之資訊的清單。 您需要提供的項目如下所示。 某些項目是選擇性的，或者已經有預設值，但您可以視需要加以變更。


### <a name="create-a-new-add-on-page"></a>建立新的附加元件頁面

| 欄位名稱                    | 備註                            |
|-------------------------------|----------------------------------|
| [**產品類型**](set-your-add-on-product-id.md#product-type)      | 必要 |  
| [**Product ID**](set-your-add-on-product-id.md#product-id)          | 必要 |        


### <a name="properties-page"></a>屬性頁面

| 欄位名稱                    | 備註                              |   
|-------------------------------|------------------------------------|
| [**產品存留期**](enter-add-on-properties.md#product-lifetime)  | 如果產品類型為 **持久性**，則為必要項。 不適用於其他產品類型。 |
| [**數量**](enter-add-on-properties.md#quantity)  | 如果產品類型是 **存放區管理**的取用，則為必要項。 不適用於其他產品類型。 |
| [**訂閱期間**](enter-add-on-properties.md#subscription-period)          | 如果產品類型是 **\[訂閱\]**，則為必要。 不適用於其他產品類型。       |  
| [**免費試用**](enter-add-on-properties.md#free-trial)          | 如果產品類型是 **\[訂閱\]**，則為必要。 不適用於其他產品類型。       |
| [**內容類型**](enter-add-on-properties.md#content-type)          | 必要    |               
| [**關鍵字**](enter-add-on-properties.md#keywords)                  | 選擇性 (最多 10 個關鍵字，每個關鍵字最多 30 個字元) |
| [**自訂開發人員資料**](enter-add-on-properties.md#custom-developer-data)   | 選擇性 (最多 3000 個字元)            |


### <a name="pricing-and-availability-page"></a>定價和可用性頁面

| 欄位名稱                    | 備註                                       |
|-------------------------------|---------------------------------------------|
| [**市場**](set-add-on-pricing-and-availability.md#markets)  | 預設：所有可能的市場 |
| [**可見度**](set-add-on-pricing-and-availability.md#visibility)   | 預設：可供購買。 可在應用程式清單中顯示。 |
| [**排程**](set-add-on-pricing-and-availability.md#schedule)    | 預設：儘速發行
| [**定價**](set-add-on-pricing-and-availability.md#pricing)                | 必要                                    |
| [**銷售定價**](put-apps-and-add-ons-on-sale.md)               | 選擇性                    |


### <a name="store-listings"></a>市集清單

需要一個 Store 清單。 建議您為您的應用程式所支援的每種 [語言](create-add-on-store-listings.md#store-listing-languages) 供應商店清單。

| 欄位名稱                    | 備註                                       |
|-------------------------------|---------------------------------------------|
| [**標題**](create-add-on-store-listings.md#title)                    | 必要 (最多 100 個字元)           |
| [**描述**](create-add-on-store-listings.md#description)       | 選擇性 (最多 200 個字元)            |
| [**圖示**](create-add-on-store-listings.md#icon)                    | 選擇性 (300x300 像素的 .png)            |


當您完成輸入此資訊之後，請按一下 **[提交至存放區**]。 在大多數情況下，認證程序大概需要一個小時。 之後，您的附加元件就會發佈到 Store，供客戶購買。

> [!NOTE]
> 您也必須在您的應用程式程式碼中實作附加元件。 如需詳細資訊，請參閱 [App 內購買和試用版](../monetize/in-app-purchases-and-trials.md)。


## <a name="updating-an-add-on-after-publication"></a>發佈之後更新附加元件

您可以隨時對已發佈的附加元件進行變更。 附加元件變更會與您的應用程式分開提交和發佈，因此您通常不需要更新整個應用程式，即可變更附加元件，例如更新其價格或描述。

若要提交更新，請移至合作夥伴中心中的附加元件頁面，然後按一下 [ **更新**]。 這將會使用您先前提交的資訊做為起點，建立附加元件的新提交。 進行您想要的變更，然後按一下 **[提交至存放區**]。

如果您想要移除先前提供的附加元件，請建立新的提交並且將 [\[配送和可見性\]](set-add-on-pricing-and-availability.md) 選項變更為 **\[在 Microsoft Store 中隱藏\]** 以及 **\[停止取得\]** 選項。 請務必視需要更新您的應用程式程式碼，以移除附加元件的參考 (特別是當先前發佈的應用程式支援之前 Windows 8.1 時：這些客戶) 不會套用此可見度設定。

> [!IMPORTANT]
> 如果您先前發佈的應用程式可供 Windows 8 的客戶使用，您將需要建立併發布新的應用程式提交，才能讓這些客戶看到附加元件更新。 同樣地，如果您在以 Windows 8.x 為目標的 App 發佈後，將新的附加元件新增到 App，您必須更新您的 App 程式碼以參考這些附加元件，然後重新提交 App。 否則，Windows 8.x 的客戶將無法看見新的附加元件。
