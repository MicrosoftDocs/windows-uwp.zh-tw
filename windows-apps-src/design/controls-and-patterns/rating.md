---
description: 可讓使用者檢視並設定反映其對內容與服務滿意度的評分。
title: 評分控制項
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5835993984bd0e080a1a049d20f19c96020f80f9
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91749664"
---
# <a name="rating-control"></a>評分控制項

評分控制項可讓使用者檢視並設定反映其對內容與服務滿意度的評分。 使用者可以使用觸控、手寫筆、滑鼠、遊戲台或鍵盤，與評分控制項互動。 下列指導方針示範如何使用評分控制項的功能來提供彈性和自訂。

![評分控制項範例](images/rating_rs2_doc_ratings_intro.png)

**取得 Windows UI 程式庫**

:::row:::
   :::column:::
      ![WinUI 標誌](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **RatingControl** 控制項包含在 Windows UI 程式庫中；該程式庫是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows UI 程式庫 API：** [RatingControl 類別](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)
>
> **平台 API：** [RatingControl 類別](/uwp/api/windows.ui.xaml.controls.ratingcontrol)

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/RatingControl">開啟應用程式並查看 RatingControl 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="editable-rating-with-placeholder-value"></a>有預留位置值的可編輯評分

評分控制項最常見的使用方式或許是顯示平均評分，但仍允許使用者輸入他們自己的評分值。 在此案例中，評分控制項一開始已設定為反映對特定服務或內容類型 (例如音樂、影片、書籍等) 的平均滿意度評分。 在使用者為了對某個項目個別評分而與控制項互動之前，控制項都會保持在此狀態。 這個互動會變更評分控制項的狀態，以反映使用者的個人滿意度評分。

#### <a name="initial-average-rating-state"></a>初始平均評分狀態
![初始平均評分狀態](images/rating_rs2_doc_movie_aggregate.png)

#### <a name="representation-of-user-rating-once-set"></a>使用者評分一旦設定後的表示方式

![使用者評分一旦設定後的表示方式](images/rating_rs2_doc_movie_user.png)

```XAML
<RatingControl x:Name="MyRating" ValueChanged="RatingChanged"/>
```

```csharp
private void RatingChanged(RatingControl sender, object args)
{
    if (sender.Value == null)
    {
        MyRating.Caption = "(" + SomeWebService.HowManyPreviousRatings() + ")";
    }

    else
    {
        MyRating.Caption = "Your rating";
    }
}
```

### <a name="read-only-rating-mode"></a>唯讀評分模式

您有時需要顯示次要內容的評分，例如在推薦內容中顯示，或在顯示意見清單及其對應評分時顯示。 在此情況下，不可允許使用者編輯評分，這樣您才能讓控制項變成唯讀。
同時考量 UI 設計與效能而在非常大型虛擬化清單中使用評分控制項時，唯讀模式也是此控制項的建議使用方式。

![唯讀長清單](images/rating_rs2_doc_reviews.png)

為達此目的，您會執行下列動作：

```XAML
<RatingControl IsReadOnly="True"/>
```

## <a name="additional-functionality"></a>其他功能

評分控制項還有許多可用的其他功能。 您可以在參考文件中找到有關如何使用這些功能的詳細資訊。
以下是其他功能的清單，但並非詳盡無遺：
-   優異長清單處理效能
-   精簡調整緊湊 UI 案例
-   連續值填寫和評分
-   間距自訂
-   停用成長動畫
-   自訂星星數

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以互動式格式查看所有 XAML 控制項。
