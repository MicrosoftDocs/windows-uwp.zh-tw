---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: 這個案例研究根據 Bookstore1 中所提供的資訊來建置，是從在 SemanticZoom 控制項中顯示分組資料的通用 8.1 app 開始著手。
title: Windows Runtime 8.x 至 UWP 的案例研究：Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b2681eb1fd14ed7e86c00054c5f5c7ba3efb97c6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167612"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Windows Runtime 8.x 至 UWP 的案例研究：Bookstore2


這個案例研究—根據 [Bookstore1](w8x-to-uwp-case-study-bookstore1.md) 中所提供的資訊來建置—是從在 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控制項中顯示分組資料的通用 8.1 app 開始著手。 在檢視模型中，每個 **Author** 類別執行個體都代表該作者所著之書籍的群組，而在 **SemanticZoom** 中，我們可以檢視依作者分組的書籍清單，或是縮小來查看作者的捷徑清單。 與捲動書籍清單相比，捷徑清單可提供更快速的瀏覽。 我們會逐步說明將 app 移植到 Windows 10 通用 Windows 平台 (UWP) app 的步驟。

**注意**   \_在 Visual Studio 中開啟 Bookstore2Universal 10 時，如果您看到「需要更新 Visual Studio」訊息，請遵循[TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)中的步驟。

## <a name="downloads"></a>下載

[下載 Bookstore2 \_ 81 通用8.1 應用程式](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2_81)。

[下載 Bookstore2Universal \_ 10 Windows 10 應用程式](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10)。

## <a name="the-universal-81-app"></a>通用 8.1 應用程式

Bookstore2 \_ 81 （我們要移植的應用程式）看起來像這樣。 這會以水平捲動方式 (在 Windows Phone 上為垂直捲動) [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 來顯示依作者分組的書籍。 您可以縮小範圍至捷徑清單，然後從該處往回瀏覽至任何群組。 此應用程式有兩個主要部分：提供分組資料來源的檢視模型，以及繫結至該檢視模型的使用者介面。 誠如所見，很輕鬆地就能將這兩個部分從 WinRT 8.1 技術移植到 Windows 10。

![\-windows 上的 bookstore2 81，放大視圖](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

\_Windows 上的 Bookstore2 81，放大視圖
 

![\-windows 上的 bookstore2 81，放大視圖](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

\_Windows 上的 Bookstore2 81，放大視圖

![\-windows phone 上的 bookstore2 81，放大視圖](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

\_Windows Phone 上的 Bookstore2 81，放大視圖

![\-windows phone 上的 bookstore2 81，放大視圖](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

\_Windows Phone 上的 Bookstore2 81，放大視圖

##  <a name="porting-to-a-windows10-project"></a>移植到 Windows 10 專案

Bookstore2 \_ 81 解決方案是8.1 通用應用程式專案。 Bookstore2 \_ 81. Windows 專案會建立 Windows 8.1 的應用程式封裝，而 Bookstore2 \_ 81. WindowsPhone 專案會建立 Windows Phone 8.1 的應用程式套件。 Bookstore2 \_ 81. Shared 是包含原始程式碼、標記檔案，以及其他兩個專案所使用之其他資產和資源的專案。

就像使用先前的案例研究一樣，我們將採用的選項 ([如果您有通用 8.1 app](w8x-to-uwp-root.md) 中所述的選項) 是將共用專案的內容移植到針對通用裝置系列設計的 Windows 10。

一開始先建立新的空白應用程式 (Windows 通用) 專案。 將它命名為 Bookstore2Universal \_ 10。 這些是要從 Bookstore2 \_ 81 複製到 Bookstore2Universal 10 的檔案 \_ 。

**從共用專案**

-   複製包含書籍封面影像 PNG 檔案的資料夾 (該資料夾是 \\ \\ CoverImages) 的資產。 在複製資料夾之後，請在 [**方案總管**] 中，確定 [**顯示所有檔案**] 已切換成開啟。 以滑鼠右鍵按一下您複製的資料夾，然後按一下 [ **包含在專案中**]。 該命令就是我們所謂的在專案中「包含」檔案或資料夾。 每次複製檔案或資料夾時，每個複本都會在**方案總管**中按一下 [重新整理]，**然後在專案**中包含檔案或資料夾。 不需要對目的地中您正在取代的檔案執行此動作。
-   複製包含視圖模型來源檔案的資料夾 (資料夾 \\ ViewModel) 。
-   複製 MainPage.xaml 並取代目的地中的檔案。

**從 Windows 專案**

-   複製 BookstoreStyles.xaml。 我們將使用此項目做為一個良好的起點，因為這個檔案中的所有資源索引鍵都將在 Windows 10 應用程式中進行解析，但相等 WindowsPhone 檔案中的部分資源索引鍵則不會進行解析。
-   複製 SeZoUC.xaml 和 SeZoUC.xaml.cs。 我們將從這個檢視的 Windows 版本開始 (其適用於寬型視窗)，然後將它調整為適用於較小型視窗，接下來則是更小型裝置。

編輯您剛剛複製的原始程式碼和標記檔案，並將 Bookstore2 \_ 81 命名空間的參考變更為 Bookstore2Universal \_ 10。 快速的方法是使用 [檔案 **中取代** ] 功能。 在檢視模型中，或在任何其他非常重要的程式碼中，都不需要變更任何程式碼。 但是，為了讓您更輕鬆地查看正在執行的應用程式版本，請將 **Bookstore2Universal 的 \_ BookstoreViewModel** 所傳回的值從 "Bookstore2 \_ 81" 變更為 "Bookstore2Universal \_ 10"。

您可以立即建置並執行。 以下是新的 UWP app 在尚未執行任何工作以將其移植到 Windows 10 之後的外觀。

![放大檢視已變更初始原始程式碼且正在傳統型裝置上執行的 Windows 10 應用程式](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

在桌面裝置上執行的初始原始程式碼變更的 Windows 10 應用程式，已放大視圖

![縮小檢視已變更初始原始程式碼且正在傳統型裝置上執行的 Windows 10 應用程式](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

縮小檢視已變更初始原始程式碼且正在傳統型裝置上執行的 Windows 10 應用程式

儘管會產生難以察覺的問題，但是檢視模型與放大和縮小檢視還是可以正確地一同運作。 有一個問題是 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 不會捲動。 這是因為在 Windows 10 中，[**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 的預設樣式會導致它以垂直方式進行配置 (但 Windows 10 設計指導方針建議我們在新的和移植的應用程式中以該方式來使用它)。 但是，我們從 Bookstore2 81 專案複製而來的 [自訂專案] 面板範本中的水準滾動設定 \_ (是針對8.1 應用程式) 所設計，而這是因為我們已移植至 Windows 10 應用程式而套用的 Windows 10 預設樣式中的垂直捲動設定發生衝突。 第二點是 app 還不會調整它的使用者介面，以便在不同大小的視窗中及小型裝置上提供最佳體驗。 而第三點是還無法使用正確的樣式和筆刷，因而導致許多文字無法顯示 (包括您可以按一下來放大的群組標題)。 因此，在接下來的三個小節 ([SemanticZoom 和 GridView 設計變更](#semanticzoom-and-gridview-design-changes)、[彈性 UI](#adaptive-ui) 及[通用樣式](#universal-styling)) 中，我們將會解決這三個問題。

## <a name="semanticzoom-and-gridview-design-changes"></a>SemanticZoom 和 GridView 設計變更

Windows 10 中對於 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控制項的設計變更會在 [SemanticZoom 變更](w8x-to-uwp-porting-xaml-and-ui.md)一節中加以說明。 我們不需在本節中執行任何動作來回應這些變更。

對於 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 所做的變更，請參閱 [GridView/ListView 變更](w8x-to-uwp-porting-xaml-and-ui.md)中的說明。 我們必須進行一些非常細微的調整以適應這些變更，如下所述。

-   在 SeZoUC.xaml 的 `ZoomedInItemsPanelTemplate` 中，設定 `Orientation="Horizontal"` 和 `GroupPadding="0,0,0,20"`。
-   在 SeZoUC.xaml 中，刪除 `ZoomedOutItemsPanelTemplate`，並從縮小檢視中移除 `ItemsPanel` 屬性。

這樣就大功告成了！

## <a name="adaptive-ui"></a>彈性 UI

進行該變更之後，SeZoUC.xaml 提供的 UI 配置會優於應用程式在寬型視窗中執行的樣子 (這只適用於配有大型螢幕的裝置)。 儘管應用程式的視窗較窄 (這會發生於小型裝置上，也會發生於大型裝置上)，我們在 Windows Phone 市集應用程式中所擁有的 UI 可說是最適用的。

我們可以使用彈性的 Visual State Manager 功能來達到這個目的。 我們將設定視覺元素上的屬性，因此，預設會使用我們在 Windows Phone 市集應用程式中使用的較小型範本，以窄型狀態來配置 UI。 接著，將偵測 app 的視窗寬於或等於特定大小的時機 (以[有效像素](w8x-to-uwp-porting-xaml-and-ui.md)單位來測量)，並變更視覺元素的屬性，來取得更大且更寬的配置做為回應。 我們會將這些屬性變更放置於某個視覺狀態中，並使用彈性觸發程序持續監視，且根據視窗寬度 (以有效像素為單位) 決定是否要套用該視覺狀態。 在此案例中我們觸發了視窗寬度，但是也可能觸發視窗高度。

視窗的最小寬度為 548 epx，這是最適合這個使用案例的大小，因為這是我們想要顯示寬型配置的最小型裝置的大小。 手機通常會低於 548 epx，因此在這類小型裝置上，我們會保留預設的窄型配置。 在電腦上，視窗將使用足以觸發切換為寬型狀態的預設寬度來啟動。 您將能從該處拖曳視窗窄邊，使其足以顯示兩個包含大小為 250x250 項目的欄。 比這個還窄一點，且觸發程序將停用、將移除寬型視覺狀態，而預設的窄型配置將會生效。

因此，我們需要使用哪些屬性來設定—及變更—以實現這兩個不同的配置？ 有兩個替代方案，每個都需要不同的方法。

1.  我們會在標記中放置兩個 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控制項。 其中一個是在 Windows 執行階段8.x 應用程式中使用的標記複本 (使用它) 內的 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 控制項，預設會折迭。 另一個則是在 Windows Phone 市集應用程式中使用的標記複本 (在其內部使用 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 控制項)，且預設會顯示。 視覺狀態會切換兩個 **SemanticZoom** 控制項的可見度屬性。 這不需要花費太多心力就能達成，但一般而言，這並非高效能技術。 因此，如果您使用它，就應該分析您的應用程式，並確認它仍然符合您的效能目標。
2.  我們可以使用包含 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 控制項的單一 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)。 為了實現這兩個配置，我們會在寬型視覺狀態中，變更 **ListView** 控制項的屬性 (包括套用到它們的範本)，讓它們能夠以 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 所做的相同方式進行配置。 這可能會執行地更好，但是在 **GridView** 和 **ListView** 的各種樣式和範本間，以及在其各種項目類型 (這是更難達成的方案) 之間，有很多細微差異。 這個方案也會與目前設計預設樣式和範本的方式緊密結合，提供精細且敏感的解決方案，以便未來對預設值進行任何變更。

在這個案例研究中，我們即將使用第一個替代方案。 但是，您可以視需要嘗試第二個替代方案，並查看它的運作方式是否更適合您。 以下是要實作的第一個替代方案所採取的步驟。

-   在新專案中，於標記中的 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 上設定 `x:Name="wideSeZo"` 和 `Visibility="Collapsed"`。
-   返回 Bookstore2 \_ 81. WindowsPhone 專案中，並開啟 SeZoUC。 複製該檔案的 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 元素標記，並將它貼到新專案中緊接在 `wideSeZo` 之後。 在您剛貼上的元素上設定 `x:Name="narrowSeZo"`。
-   但是 `narrowSeZo` 需要幾個我們尚未複製的樣式。 同樣 \_ 地，在 Bookstore2 81. WindowsPhone 中，複製兩個樣式 (`AuthorGroupHeaderContainerStyle` 並 `ZoomedOutAuthorItemContainerStyle`) 出 SeZoUC，並將其貼入新專案中的 BookstoreStyles。
-   您目前在新的 SeZoUC.xaml 中有兩個 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 元素。 在 **Grid** 中將這兩個元素自動換行。
-   在新專案的 BookstoreStyles.xaml 中，將文字 `Wide` 附加到下列三個資源索引鍵 (以及它們在 SeZoUC.xaml 中的參考，但僅限 `wideSeZo` 內部的參考)： `AuthorGroupHeaderTemplate`、`ZoomedOutAuthorTemplate` 及 `BookTemplate`。
-   在 Bookstore2 \_ 81. WindowsPhone 專案中，開啟 BookstoreStyles。 從這個檔案中，複製上述 (上述的三個資源) 和兩個跳躍清單專案轉換器，以及命名空間前置詞宣告 \_ Windows \_ UI \_ Xaml \_ 來控制基本專案，並將它們全部貼入新專案中的 BookstoreStyles。
-   最後，在新專案的 SeZoUC.xaml 中，將適當的 Visual State Manager 標記新增到您在上方新增的 **Grid**。

```xml
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...

    </Grid>
```

## <a name="universal-styling"></a>通用樣式

現在，讓我們來修正某些樣式問題，包括上方所述之從舊專案複製時所發生的問題。

-   在 MainPage.xaml 中，將 `LayoutRoot` 的背景變更為 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`。
-   在 BookstoreStyles.xaml 中，將資源 `TitlePanelMargin` 的值設為 `0` (或任何您喜歡的值)。
-   在 SeZoUC.xaml 中，將 `wideSeZo` 的邊界設為 `0` (或任何您喜歡的值)。
-   在 BookstoreStyles.xaml 中，從 `AuthorGroupHeaderTemplateWide` 移除邊界屬性。
-   從 `AuthorGroupHeaderTemplate` 和 `ZoomedOutAuthorTemplate` 移除 FontFamily 屬性。
-   Bookstore2 \_ 81 使用 `BookTemplateTitleTextBlockStyle` 、 `BookTemplateAuthorTextBlockStyle` 和 `PageTitleTextBlockStyle` 資源金鑰作為間接取值，讓單一金鑰在這兩個應用程式中具有不同的實作為。 我們已經不再需要該間接取值，我們可以直接參照系統樣式。 因此，請個別使用 `TitleTextBlockStyle`、`CaptionTextBlockStyle` 及 `HeaderTextBlockStyle` 來取代那些參考。 您可以使用 Visual Studio 的 [**檔案中取代**] 功能，快速且準確地執行此動作。 然後，就可以刪除這三個未使用的資源。
-   在 `AuthorGroupHeaderTemplate` 中，使用 `SystemControlBackgroundAccentBrush` 來取代 `PhoneAccentBrush`，並在 **TextBlock** 上設定 `Foreground="White"`，讓它在行動裝置系列上執行時看起來是正確的。
-   在 `BookTemplateWide` 中，將前景屬性從第二個 **TextBlock** 複製到第一個。
-   在 `ZoomedOutAuthorTemplateWide` 中，將對 `SubheaderTextBlockStyle` (現在已經有一點太大) 的參照變更成參照 `SubtitleTextBlockStyle`。
-   新平台中的縮小檢視 (捷徑清單) 不再與放大檢視重疊，如此一來，就可以從 `narrowSeZo` 的縮小檢視移除 `Background` 屬性 。
-   因此，所有的樣式與範本都會在同一個檔案中，請將 `ZoomedInItemsPanelTemplate` 移出 SeZoUC.xaml 並移入 BookstoreStyles.xaml。

樣式作業的最後一個步驟會讓 app 看起來像這樣。

![放大檢視已移植完成且正在傳統型裝置上執行的 Windows 10 app，具備兩種視窗大小](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

在桌面裝置上執行的移植的 Windows 10 應用程式，已放大視圖，兩個大小的視窗

![縮小檢視已移植完成且正在傳統型裝置上執行的 Windows 10 app，具備兩種視窗大小](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

在桌面裝置上執行的移植的 Windows 10 應用程式、放大的視圖、兩個大小的視窗

![放大檢視已移植完成且正在行動裝置上執行的 Windows 10 app](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

放大檢視已移植完成且正在行動裝置上執行的 Windows 10 app

![縮小檢視已移植完成且正在行動裝置上執行的 Windows 10 app](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

在行動裝置上執行的移植的 Windows 10 應用程式，已放大視圖

## <a name="conclusion"></a>結論

與前一個案例研究相比，這個案例研究涉及更酷炫的使用者介面。 如同前一個案例研究，這個特定的檢視模型完全不需進行任何工作，而且我們的努力重點主要是重構使用者介面。 某些變更是將兩個專案結合成一個，同時仍支援許多不同尺寸規格所產生的必要結果 (事實上，比之前多很多)。 有一些變更是利用已對平台所做的變更來進行。

下一個案例研究是 [QuizGame](w8x-to-uwp-case-study-quizgame.md)，我們將在其中探討如何存取和顯示分組資料。