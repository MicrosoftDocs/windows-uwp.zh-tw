---
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: 了解如何使用 Windows.Services.Store 命名空間來購買 App 或其附加元件。
title: 啟用應用程式和附加元件的應用程式內購買
keywords: Windows 10, UWP, 附加元件, App 內購買, IAP, Windows.Services.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3064a651715329a3602c0c3539394d2ce72f90a7
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362811"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>啟用應用程式和附加元件的應用程式內購買

本文示範如何使用 [Windows.Services.Store](/uwp/api/windows.services.store) 命名空間中的成員，讓使用者要求購買目前的應用程式或其附加元件之一。 例如，若使用者目前擁有的是試用版 App，您可以使用這個程序來讓該使用者購買完整版授權。 或者，您可以使用這個程序來購買附加元件，像是提供使用者新的遊戲關卡。

[Windows.Services.Store](/uwp/api/windows.services.store) 命名空間提供了幾個方法可要求購買 App 或附加元件：
* 如果您知道應用程式或附加元件的[ Store 識別碼](in-app-purchases-and-trials.md#store_ids)，則您可以使用 [StoreContext](/uwp/api/windows.services.store.storecontext) 類別的 [RequestPurchaseAsync](/uwp/api/windows.services.store.storecontext.requestpurchaseasync) 方法。
* 如果您已經有代表應用程式或附加元件的[ **StoreProduct**、 **StoreSku**或**StoreAvailability**物件](in-app-purchases-and-trials.md#products-skus)，您可以使用這些物件的**RequestPurchaseAsync**方法。 如需在程式碼中擷取 **StoreProduct** 的不同方式範例，請參閱[取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)。

每個方法會對使用者呈現一個標準購買 UI，並且會在交易完成之後以非同步的方式完成。 該方法會傳回指示交易是否成功的物件。

> [!NOTE]
> **Windows.Services.Store** 命名空間在 Windows 10 (版本 1607) 中引進，只適用於目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的專案。 如果您的 app 目標為較早版本的 Windows 10，您必須使用 [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 命名空間，而不是 **Windows.Services.Store** 命名空間。 如需詳細資訊，請參閱[這篇文章](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

## <a name="prerequisites"></a>先決條件

這個範例包含下列先決條件：
* 適用於目標為 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的通用 Windows 平台 (UWP) App 的 Visual Studio 專案。
* 您已在合作夥伴中心中 [建立提交應用](../publish/app-submissions.md) 程式，並在存放區中發佈此應用程式。 測試時您也可以選擇將應用程式設定為不可在市集中搜尋。 如需詳細資訊，請參閱我們的[測試指南](in-app-purchases-and-trials.md#testing)。
* 如果您想要為應用程式啟用應用程式附加元件的應用程式內購買，您也必須 [在合作夥伴中心中建立附加](../publish/add-on-submissions.md)元件。

這個範例中的程式碼假設：
* 程式碼會在 [Page](/uwp/api/windows.ui.xaml.controls.page) 的內容中執行，其中包含名為 ```workingProgressRing``` 的 [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) 和名為 ```textBlock``` 的 [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock)。 這些物件可個別用來表示發生非同步作業，以及顯示輸出訊息。
* 程式碼檔案含有適用於 **Windows.Services.Store** 命名空間的 **using** 陳述式。
* App 是單一使用者 app，僅會在啟動 app 的使用者內容中執行。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md#api_intro)。

> [!NOTE]
> 如果您的傳統型應用程式使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)，您可能需要新增此範例中未顯示的額外程式碼來設定 [StoreContext](/uwp/api/windows.services.store.storecontext) 物件。 如需詳細資訊，請參閱[在使用傳統型橋接器的傳統型應用程式中使用 StoreContext 類別](in-app-purchases-and-trials.md#desktop)。

## <a name="code-example"></a>程式碼範例

此範例示範如何使用 [StoreContext](/uwp/api/windows.services.store.storecontext) 類別的 [RequestPurchaseAsync](/uwp/api/windows.services.store.storecontext.requestpurchaseasync) 方法來購買已知其[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)的 App 或附加元件。 如需完整的範例應用程式，請參閱[市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs" id="PurchaseAddOn":::

## <a name="video"></a>影片

觀看下列影片，了解如何在應用程式中實作 App 內購買項目。
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

## <a name="related-topics"></a>相關主題

* [應用程式內購買和試用版](in-app-purchases-and-trials.md)
* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得應用程式和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [實作應用程式的試用版](implement-a-trial-version-of-your-app.md)
* [市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
