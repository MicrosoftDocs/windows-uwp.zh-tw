---
description: 提供可供使用的應用程式內產品&\# 郵件; 可透過商店 commerce 平臺再次購買、使用及購買的專案&\# 郵件; 透過商店 commerce 平臺，為您的客戶提供既穩固又可靠的購買體驗。
title: 啟用消費性應用程式內產品購買
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: UWP, 消費性, 附加元件, app 內購買, IAP, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fdd09fdb0d80f7811014dc910772175e152505d0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033540"
---
# <a name="enable-consumable-in-app-product-purchases"></a>啟用消費性應用程式內產品購買

您可以透過 Microsoft Store 商業平台提供消費性的應用程式內產品 (亦即可購買、使用，然後再次購買的項目)，為客戶提供既健全又可靠的購買體驗。 這對於像遊戲內貨幣 (金幣、錢幣等) 這種可在買來後用來購買特定火力升級配備的東西，特別有用。

> [!IMPORTANT]
> 這篇文章示範如何使用 [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 命名空間的成員來啟用消費性 App 內產品購買。 此命名空間不再提供新功能更新，建議您改為使用 [Windows.Services.Store](/uwp/api/windows.services.store) 命名空間。 **Windows. Store** 命名空間支援最新的附加元件類型（例如儲存管理的可使用附加元件和訂閱），其設計目的是要與合作夥伴中心和存放區所支援的未來產品和功能類型相容。 **Windows.Services.Store** 命名空間在 Windows 10 (版本 1607) 中引進，只適用於目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的專案。 如需有關使用 **Windows.Services.Store** 命名空間來啟用消費性 App 內產品購買的詳細資訊，請參閱 [本文](enable-consumable-add-on-purchases.md)。

## <a name="prerequisites"></a>Prerequisites

-   本主題涵蓋消費性應用程式內產品的購買和履行狀況報告。 如果您不熟悉應用程式內產品，請檢閱[啟用應用程式內產品購買](enable-in-app-product-purchases.md)，以了解授權資訊及如何在 Microsoft Store 中正確列出應用程式內產品。
-   初次撰寫並測試新應用程式內產品的程式碼時，您必須使用 [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 物件，而不是 [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 物件。 如此一來，您就可以利用對授權伺服器進行模擬呼叫來驗證授權邏輯，而不是呼叫使用中的伺服器。 若要這樣做，您必須自訂% userprofile% \\ AppData \\ 本機 \\ 套件 \\ &lt; 套件名稱 &gt; \\ LocalState \\ Microsoft \\ Windows Store \\ ApiData 中名為 WindowsStoreProxy.xml 的檔案。 Microsoft Visual Studio 模擬器會在您第一次執行您的 App 時建立這個檔案，或者您也可以在執行階段載入自訂的檔案。 如需詳細資訊，請參閱[使用 WindowsStoreProxy.xml 檔案搭配 CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)。
-   本主題也會參照[ Microsoft Store 範例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)中提供的程式碼範例。 這個範例非常適合用來體驗實機操作針對通用 Windows 平台 (UWP) app 提供的不同貨幣選項。

## <a name="step-1-making-the-purchase-request"></a>步驟 1：提出購買要求

就像透過 Microsoft Store 進行的任何其他購買一樣，初始購買要求是以 [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 提出。 消費性應用程式內產品的不同之處在於，客戶在順利購買這類產品之後，除非應用程式已通知 Microsoft Store 先前的購買已順利履行，否則客戶無法再次購買相同的產品。 您的應用程式必須負責履行已購買的消費性產品，並在履行後通知 Microsoft Store。

下列範例示範消費性應用程式內產品購買要求。 您會注意到程式碼註解指出在下列兩種不同的情況下，App 應該於何時在本機履行消費性應用程式內產品，一是在要求成功的情況，二是在因為購買尚未履行的相同產品而導致要求失敗的情況。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="MakePurchaseRequest":::

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>步驟 2：追蹤消費性產品的本機履行

將消費性應用程式內產品的存取權授與客戶時，請務必追蹤哪些產品已履行 ( *productId* )，以及該履行動作與哪個交易關聯 ( *transactionId* )。

> [!IMPORTANT]
> 您的 App 必須將履行動作準確回報給 Microsoft Store。 若要為客戶維護公平可靠的購買體驗，這個步驟是不可或缺的。

下列範例示範如何使用上一個步驟之 [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 呼叫中的 [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults) 屬性，來識別購買的產品是否已經履行。 此範例使用集合將產品資訊儲存在可供參照的位置，以便稍後確認是否已在本機順利履行。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="GrantFeatureLocally":::

下個範例說明將履行回報給 Microsoft Store 後，如何使用上個範例的陣列來存取之後要使用的產品識別碼/交易識別碼。

> [!IMPORTANT]
> 無論您的 App 使用哪種方法來追蹤和確認履行，都必須提供審查評鑑，以確保不會針對客戶尚未收到的項目向客戶收費。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="IsLocallyFulfilled":::

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>步驟 3：將產品履行回報給 Microsoft Store 

完成本機履行之後，您的 App 必須進行 [ReportConsumableFulfillmentAsync](/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) 呼叫，此呼叫包含 *productId* 及含括該項產品購買的交易。

> [!IMPORTANT]
> 若未將已履行的消費性應用程式內產品報告給 Microsoft Store，將導致使用者無法再次購買該產品，必須等到回報已履行上次的購買後才能再購買。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="ReportFulfillment":::

## <a name="step-4-identifying-unfulfilled-purchases"></a>步驟 4：識別未履行的購買

您的 App 可以隨時使用 [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) 方法，來檢查是否有任何未履行的消費性應用程式內產品。 您應該定期呼叫這個方法，以檢查是否有任何因為未預期的應用程式事件 (例如網路連線中斷或應用程式終止) 而未履行的消費性產品存在。

下列範例示範如何使用 [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) 列舉未履行的消費性產品，以及您的 App 如何重複此清單來完成本機履行。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="GetUnfulfilledConsumables":::

## <a name="related-topics"></a>相關主題

* [啟用應用程式內產品購買](enable-in-app-product-purchases.md)
* [Microsoft Store 範例 (示範試用版和 app 內購買)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](/uwp/api/Windows.ApplicationModel.Store)
 

 
