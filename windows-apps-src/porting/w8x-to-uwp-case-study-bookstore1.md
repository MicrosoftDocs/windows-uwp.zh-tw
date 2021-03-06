---
title: Windows 執行階段 8.x 至 UWP 的案例研究，Bookstore1
ms.assetid: e4582717-afb5-4cde-86bb-31fb1c5fc8f3
description: 本主題提供將一個非常簡單的通用 8.1 應用程式移植到 Windows 10 通用 Windows 平台 (UWP) app 的案例研究。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ff8e095fc46d2c08771f3e2e002fe2d8b602ab71
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933059"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore1"></a>Windows 執行階段 8.x 至 UWP 的案例研究：Bookstore1


本主題提供將一個非常簡單的通用 8.1 應用程式移植到 Windows 10 通用 Windows 平台 (UWP) app 的案例研究。 通用 8.1 app 會針對 Windows 8.1 建置一個應用程式套件，並針對 Windows Phone 8.1 建置另一個應用程式套件。 您可以使用 Windows 10，來建立可供客戶安裝至各種類型裝置的單一 app 套件，而那就是我們將在這個案例研究中執行的工作。 請參閱 [UWP app 指南](../get-started/universal-application-platform-guide.md)。

我們將移植的 app 包含繫結到檢視模型的 **ListBox**。 此檢視模型有一個顯示書名、作者及封面的書籍清單。 書籍封面影像的 **組建動作** 設定為 [ **內容** ]，並將 [ **複製到輸出目錄** ] 設為 [ **不要複製**]。

本節之前的主題說明平台之間的差異，並針對 app 各個方面 (從 XAML 標記、經過繫結到檢視模型，再到存取資料) 的移植程序，提供深入的詳細資料和指導方針。 案例研究旨在藉由真實範例中的運作示範，來為該指導方針提供補充。 這些案例研究是假設您已看過指導方針，因此不會重複其內容。

**注意**   \_在 Visual Studio 中開啟 Bookstore1Universal 10 時，如果您看到「需要更新 Visual Studio」訊息，請遵循[TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)中的步驟。

## <a name="downloads"></a>下載

[下載 Bookstore1 \_ 81 通用8.1 應用程式](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1_81)。

[下載 Bookstore1Universal \_ 10 Windows 10 應用程式](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10)。

## <a name="the-universal-81-app"></a>通用 8.1 應用程式

Bookstore1 \_ 81 （我們要移植的應用程式）看起來像這樣。 它是一個垂直捲動的書籍清單方塊，位於 app 名稱的標題和網頁標題下方：

![bookstore1 \- 81 在 windows 上的外觀](images/w8x-to-uwp-case-studies/c01-01-win81-how-the-app-looks.png)

\_Windows 上的 Bookstore1 81

![bookstore1 \- 81 在 windows phone 上的外觀](images/w8x-to-uwp-case-studies/c01-02-wp81-how-the-app-looks.png)

\_Windows Phone 上的 Bookstore1 81

##  <a name="porting-to-a-windows10-project"></a>移植到 Windows 10 專案

Bookstore1 \_ 81 解決方案是8.1 通用應用程式專案，其中包含這些專案。

-   Bookstore1 \_ 81. Windows。 這是建置適用於 Windows 8.1 之應用程式套件的專案。
-   Bookstore1 \_ 81. WindowsPhone。 這是建立適用於 Windows Phone 8.1 之應用程式套件的專案。
-   Bookstore1 \_ 81. Shared。 這是包含其他兩個專案也會用到之原始程式碼、標記檔案及其他資產與資源的專案。

在這個案例研究中，我們擁有[如果您有通用 8.1 App](w8x-to-uwp-root.md)中所述之與支援哪些裝置有關的常見選項。 此處的決定很簡單：這個應用程式在其 Windows 8.1 與 Windows Phone 8.1 的形式中具有相同的功能，因此大部分會使用相同程式碼。 因此，我們會將共用專案的內容 (和我們所需且來自其他專案的任何內容) 移植到目標為通用裝置系列的 Windows 10 (您可以將它安裝到種類最廣泛的裝置上)。

在 Visual Studio 中建立新的專案、從 Bookstore1 81 將檔案複製到其中， \_ 然後將複製的檔案包含在新的專案中，是非常快速的工作。 一開始先建立新的空白應用程式 (Windows 通用) 專案。 將它命名為 Bookstore1Universal \_ 10。 這些是要從 Bookstore1 \_ 81 複製到 Bookstore1Universal 10 的檔案 \_ 。

**從共用專案**

-   複製包含書籍封面影像 PNG 檔案的資料夾 (該資料夾是 \\ \\ CoverImages) 的資產。 在複製資料夾之後，請在 [**方案總管**] 中，確定 [**顯示所有檔案**] 已切換成開啟。 以滑鼠右鍵按一下您複製的資料夾，然後按一下 [ **包含在專案中**]。 該命令就是我們所謂的在專案中「包含」檔案或資料夾。 每次複製檔案或資料夾時，每個複本都會在**方案總管**中按一下 [重新整理]，**然後在專案**中包含檔案或資料夾。 不需要對目的地中您正在取代的檔案執行此動作。
-   複製包含視圖模型來源檔案的資料夾 (資料夾 \\ ViewModel) 。
-   複製 MainPage.xaml 並取代目的地中的檔案。

**從 Windows 專案**

-   複製 BookstoreStyles.xaml。 我們將使用此項目做為一個良好的起點，因為這個檔案中的所有資源索引鍵都將在 Windows 10 應用程式中進行解析，但相等 WindowsPhone 檔案中的部分資源索引鍵則不會進行解析。

編輯您剛剛複製的原始程式碼和標記檔案，並將 Bookstore1 \_ 81 命名空間的參考變更為 Bookstore1Universal \_ 10。 快速的方法是使用 [檔案 **中取代** ] 功能。 在檢視模型中，或在任何其他非常重要的程式碼中，都不需要變更任何程式碼。 但是，為了讓您更輕鬆地查看正在執行的應用程式版本，請將 **Bookstore1Universal 的 \_ BookstoreViewModel** 所傳回的值從 "BOOKSTORE1 \_ 81" 變更為 "Bookstore1Universal \_ 10"。

您可以立即建置並執行。 以下是新的 UWP app 在尚未明確執行任何工作以將其移植到 Windows 10 之後的外觀。

![沒有明確變更的 Windows 10 應用程式螢幕擷取畫面。](images/w8x-to-uwp-case-studies/c01-03-desk10-initial-source-code-changes.png)

已變更初始原始程式碼且正在傳統型裝置上執行的 Windows 10 應用程式

![具有初始原始程式碼變更的 Windows 10 應用程式螢幕擷取畫面。](images/w8x-to-uwp-case-studies/c01-04-mob10-initial-source-code-changes.png)

已變更初始原始程式碼且正在行動裝置上執行的 Windows 10 應用程式

檢視與檢視模型一起正常運作，而 **ListBox** 也功能正常。 我們只需要修正樣式。 在淺色佈景主題中的行動裝置上，我們可以看到清單方塊的框線，但是很容易就能隱藏。 而且，印刷樣式太大，所以我們將改變正在使用的樣式。 此外，如果我們想要讓應用程式看起來像預設的應用程式，應用程式在桌上型裝置上執行時，其顏色應該是淺色的。 因此，我們將會變更顏色。

## <a name="universal-styling"></a>通用樣式

Bookstore1 \_ 81 應用程式使用兩個不同的資源字典 (BookstoreStyles) ，以 Windows 8.1 和 Windows Phone 8.1 作業系統量身打造其樣式。 這兩個 BookstoreStyles.xaml 檔案都未完整包含我們的 Windows 10 應用程式所需的所有樣式。 但是，好消息是我們需要的實際上比它們任何一個都更簡單。 所以接下來的步驟大部分將會涉及移除及簡化我們的專案檔案和標記。 步驟如下。 而且您可以使用此主題頂端的連結來下載專案，並查看這裡和案例研究結束時所有變更的結果。

-   若要盡可能縮短項目之間的間距，請找出 MainPage.xaml 中的 `BookTemplate` 資料範本，並將 `Margin="0,0,0,8"` 從根目錄 **Grid** 中刪除。
-   此外，在 `BookTemplate` 中，有 `BookTemplateTitleTextBlockStyle` 和 `BookTemplateAuthorTextBlockStyle` 的參考。 Bookstore1 \_ 81 使用這些索引鍵作為間接取值，讓單一金鑰在兩個應用程式中有不同的執行方式。 我們已經不再需要該間接取值，我們可以直接參照系統樣式。 因此請個別使用 `TitleTextBlockStyle` 和 `SubtitleTextBlockStyle` 取代那些參照。
-   現在我們需要將 `LayoutRoot` 的背景設為正確的預設值，如此一來，無論使用哪一種佈景主題，app 在所有裝置上執行時都會具有適當的外觀。 請將它從 `"Transparent"` 變更為 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`。
-   在 `TitlePanel` 中，將對 `TitleTextBlockStyle` (現在已經有一點太大) 的參照變更成參照 `CaptionTextBlockStyle`。 `PageTitleTextBlockStyle` 是另一種 Bookstore1 \_ 81 間接取值，不再需要。 請將它變更成參照 `HeaderTextBlockStyle`。
-   我們不再需要在 **ListBox** 上設定任何特殊的 Background、Style，以及 ItemContainerStyle，因此請將該三種屬性與其值從標記中刪除。 但是我們確實想要隱藏 **ListBox** 的框線，因此請對它加入 `BorderBrush="{x:Null}"` 。
-   我們已經不再參照 BookstoreStyles.xaml **ResourceDictionary** 檔案中的任何資源。 您可以將這些資源全數刪除。 但請勿刪除 BookstoreStyles.xaml 檔案本身，因為它對我們還有最後一項用處，您將會在下一節中看到。

樣式作業的最後一個步驟會讓 app 看起來像這樣。

![具有最後一系列樣式作業的應用程式螢幕擷取畫面。](images/w8x-to-uwp-case-studies/c01-05-desk10-almost-ported.png)

幾乎移植完成且正在傳統型裝置上執行的 Windows 10 應用程式

![幾乎移植之 windows 10 應用程式的螢幕擷取畫面。](images/w8x-to-uwp-case-studies/c01-06-mob10-almost-ported.png)

幾乎移植完成且正在行動裝置上執行的 Windows 10 app

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>選擇性調整行動裝置的清單方塊

當應用程式在行動裝置上執行時，兩個佈景主題中清單方塊的背景顏色都是預設為淺色。 這可能就是您喜好的樣式，若是如此，那麼除了整理之外，您已經不需要再執行任何動作：請將 BookstoreStyles.xaml 資源字典檔案從您的專案中刪除，然後把將它合併到 MainPage.xaml 的標記移除。

但是控制項的設計可讓您自訂它們的外觀，卻不會影響它們的行為。 因此，如果您想要在深色系佈景主題中讓清單方塊的顏色也是深色—像原始應用程式的顯示外觀一樣—本節即說明設定的方式。

我們所做的變更只需要在 app 於行動裝置上執行時影響 app。 所以我們在行動裝置系列上執行時，將使用僅稍微自訂過的清單方塊樣式，而且我們將會在其他裝置上執行時，繼續使用預設樣式。 為了這樣做，我們將製作 BookstoreStyles.xaml 的複本，並且為它指定一個特殊的 MRT 限定名稱，該名稱將可讓它只會在行動裝置上被導入。

新增一個新的 **ResourceDictionary** 專案項目並將它命名為 BookstoreStyles.DeviceFamily-Mobile.xaml。 您現在已經有兩個檔案，它們的邏輯名稱都是 BookstoreStyles.xaml (而且這就是您在標記和程式碼中使用的名稱)。 但是，檔案的實體名稱不同，所以它們可以包含不同的標記。 您可以搭配任何 xaml 檔案使用此 MRT 限定的命名配置，但請注意，邏輯名稱相同的所有 xaml 檔案都會共用單一 xaml.cs 程式碼後置檔案 (其中一個適用)。

編輯清單方塊之控制項範本的複本，然後使用 `BookstoreListBoxStyle` 的索引鍵將它儲存在新的資源字典 BookstoreStyles.DeviceFamily-Mobile.xaml 中。 現在我們將對三個 setter 做一些簡單的變更。

-   在 Foreground setter 中，將值變更為 `"{x:Null}"`。 請注意，直接在元素上將屬性設定為 `"{x:Null}"` 及使用程式碼將它設定為 `null` 的結果是一樣的。 但是在 setter 中使用 `"{x:Null}"` 的值都會有一項獨特效果：那就是它會覆寫預設樣式 (針對相同的屬性) 中的 setter，並還原目標元件上屬性的預設值。
-   在 Background setter 中，將值變更為 `"Transparent"` 會移除該淺色背景。
-   請在範本 setter 中，找出名為 `Focused` 的視覺狀態，然後刪除它的腳本，讓它變成空的標籤。
-   刪除標記中所有其他的 setter。

最後，將 `BookstoreListBoxStyle` 複製到 BookstoreStyles.xaml 中，然後刪除它的三個 setter，讓它變成空的標籤。 這樣做之後，就可以在行動裝置以外的其他裝置上，解析我們對 BookstoreStyles.xaml 與對 `BookstoreListBoxStyle` 的參照，但是不會有任何影響。

![移植完成的 Windows 10 app](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

移植完成且正在行動裝置上執行的 Windows 10 app

## <a name="conclusion"></a>結論

這個案例研究示範移植一個非常簡單之 app 的程序—可以說是相當不實際的簡單 app。 例如，清單方塊可用來進行選取或用來建立要瀏覽的內容；應用程式會瀏覽到含有所點選之項目的更多相關詳細資料的頁面。 這個特定的應用程式既不處理使用者的選項，也沒有任何瀏覽。 即使如此，提供此案例研究是為了介紹移植程序，並示範您能用於實際 UWP app 的重要程式碼共用技術。

我們也有證據顯示移植檢視模型通常是可順暢進行的程序。 使用者介面和尺寸規格支援，很可能是我們在移植時需要注意的部分。

下一個案例研究是 [Bookstore2](w8x-to-uwp-case-study-bookstore2.md)，我們將在其中探討如何存取和顯示分組資料。