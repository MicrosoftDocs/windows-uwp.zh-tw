---
ms.assetid: DC235C16-8DAF-4078-9365-6612A10F3EC3
title: 使用 C++/CX 建立 Hello World 應用程式 (Windows 10)
description: 透過 Microsoft Visual Studio 2019，您可以使用 C++/CX 來開發在 Windows 10 上執行的應用程式，包括在執行 Windows 10 的手機上執行。 這些應用程式具有使用 Extensible Application Markup Language (XAML) 所定義的 UI。
ms.date: 06/11/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b1420fc5b5e74cba46b486b57be1098ac17ad1f5
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339826"
---
# <a name="create-a-hello-world-app-in-ccx"></a>使用 C++/CX 建立 "Hello, World!" 應用程式

> [!IMPORTANT]
> 本教學課程使用 C++/CX。 Microsoft 已發生 C++/WinRT：Windows 執行階段 (WinRT) API 的完全標準現代 C++17 語言投影。 如需有關此語言的詳細資訊，請參閱 [ C++/WinRT](../cpp-and-winrt-apis/index.md)。

在 Microsoft Visual Studio 2017，您可以使用 C++/CX 開發在 Windows 10 上執行且具有使用 Extensible Application Markup Language (XAML) 定義之 UI 的應用程式。

> [!NOTE]
> 本教學課程使用 Visual Studio Community 2019。 如果您使用不同版本的 Visual Studio，它的外觀可能會略有不同。

## <a name="before-you-start"></a>開始之前

-   若要完成這個教學課程，您必須在執行 Windows 10 的電腦上，使用 Visual Studio Community，或是某一個非 Community 版本的 Visual Studio。 若要下載，請參閱[取得工具](https://visualstudio.microsoft.com/downloads/)。
-   我們假設您對 [XAML 概觀](../xaml-platform/xaml-overview.md)中的 C++/CX、XAML 及概念有基本的了解。
-   我們假設您在 Visual Studio 中使用預設的視窗配置。 若要重設為預設配置，在功能表列上選擇 [視窗] >  [重設視窗配置]。

## <a name="comparing-c-desktop-apps-to-windows-apps"></a>比較 C++ 傳統型應用程式和 Windows 應用程式

如果您的基礎知識是使用 C++ 來設計 Windows 傳統型應用程式，或許會發現撰寫 UWP 應用程式有某些類似的地方，但其他方面則有待學習。

### <a name="whats-the-same"></a>有何相同之處？

-   您可以使用 STL、CRT (但有些例外) 及任何其他 C++ 程式庫，只要程式碼僅呼叫可從 Windows 執行階段環境存取的 Windows 函式即可。

-   如果您習慣使用視覺化設計工具，仍然可以使用 Microsoft Visual Studio 內建的設計工具，或使用功能更完整的 Blend for Visual Studio。 如果您習慣自行撰寫 UI 程式碼，可以手動撰寫 XAML 的程式碼。

-   您仍然可以建立一些使用 Windows 作業系統類型和自訂類型的應用程式。

-   您仍然可以使用 Visual Studio 偵錯工具、分析工具以及其他開發工具。

-   您仍然可以建立使用 Visual C++ 編譯器編譯原生機器程式碼的應用程式。 使用 C++/CX 編譯的 UWP 應用程式不在受管理的執行階段環境中執行。

### <a name="whats-new"></a>新功能

-   UWP 應用程式和通用 Windows 應用程式的設計原則與傳統型應用程式的設計原則有很大的差別。 設計的重點不再強調視窗的邊框、標籤、對話方塊等等。 內容才是最重要的。 完美的通用 Windows app 會在規劃階段就遵循這些原則。

-   您是使用 XAML 來定義整個 UI。 在 Windows 通用 app 中，UI 和核心程式邏輯之間的界線遠比 MFC 或 Win32 應用程式清楚得多。 當您處理程式碼檔案的行為時，其他人可以在 XAML 檔案中設計 UI 的外觀。

-   雖然您仍然可以在 Windows 裝置上使用 Win32 的部分功能，不過主要是使用全新、易於瀏覽、物件導向的 API (Windows 執行階段) 來設計程式。

-   您使用 C++/CX 來取用和建立 Windows 執行階段物件。 C++/CX 可啟用動態建立物件的 C++ 例外狀況處理、委派、事件及自動參考計數。 當您使用 C++/CX 時，應用程式程式碼會隱藏基礎 COM 和 Windows 結構的詳細資訊。 如需相關資訊，請參閱 [C++/CX 語言參考](/cpp/cppcx/visual-c-language-reference-c-cx)。

-   您的應用程式會編譯成一個套件，其中包括應用程式包含之類型的中繼資料、應用程式使用的資源以及需要的功能 (檔案存取、網際網路存取、相機存取等等)。

-   在 Microsoft Store 和 Windows Phone Store 中，您的應用程式會經過認證程序檢查其安全性，然後再公開給數百萬的潛在客戶。

## <a name="hello-world-store-app-in-ccx"></a>使用 C++/CX 的 Hello World Store 應用程式

我們的第一個應用程式是 "Hello World"，將示範互動功能、配置及樣式的某些基本功能。 我們將從 Windows 通用 app 專案範本建立應用程式。 如果您先前已開發過適用於 Windows 8.1 和 Windows Phone 8.1 的 app，就可能記得必須在 Visual Studio 中擁有三個專案，一個用於 Windows app、一個用於手機 app，另一個則包含共用程式碼。 Windows 10 通用 Windows 平台 (UWP) 讓您只需一個專案，就能在所有裝置 (包括執行 Windows 10 的桌上型電腦和膝上型電腦、平板電腦、行動電話、VR 裝置等裝置) 上執行。

我們將從基礎開始：

-   如何在 Visual Studio 中建立通用 Windows 專案。

-   如何了解建立的專案和檔案。

-   如何了解 Visual C++ 元件延伸 (C++/CX) 中的擴充功能，以及使用它們的時機。

**首先，在 Visual Studio 中建立方案**

1.  在 Visual Studio 的功能表列上，選擇 [檔案] > [新增] > [專案...]。

2.  在 [建立新專案] 對話方塊中，選取 [空白應用程式 (通用 Windows- C++/CX)]。  如果您沒有看到這些選項，請確定您已經安裝「通用 Windows 應用程式開發工具」。 如需詳細資訊，請參閱[開始設定](/windows/apps/get-started/get-set-up)。

![[建立新專案] 對話方塊中的 C++/CX 專案範本 ](images/vs2019-uwp-01.png)

3.  選擇 [下一步]，然後輸入專案的名稱。 我們將它命名為 HelloWorld。

4.  選擇 [建立] 按鈕。

> [!NOTE]
> 如果這是您第一次使用 Visual Studio，您可能會看到 [設定] 對話方塊要求您啟用 [開發人員模式]。 開發人員模式是啟用某些功能的特殊設定，例如，直接執行應用程式的權限，而非只執行來自 Microsoft Store 的。 如需詳細資訊，請閱讀[啟用您的裝置以進行開發](/windows/apps/get-started/enable-your-device-for-development)。 若要繼續使用此指南，請選取 [開發人員模式]，按一下 [是]，並關閉對話方塊。

   您的專案檔案已成功建立。

繼續之前，先看看方案中有些什麼功能。

![節點摺疊的通用應用程式方案](images/vs2017-uwp-02.png)

### <a name="about-the-project-files"></a>關於專案檔案

專案資料夾中的每個 .xaml 檔案在相同資料夾中都有對應的 .xaml.h 檔案和 .xaml.cpp 檔案，在 [產生的檔案] 資料夾中則有 .g 檔案和 .g.hpp 檔案 (它在磁碟上但不屬於專案)。 修改 XAML 檔案可建立 UI 元素，並將它們連接到資料來源 (DataBinding)。 修改 .h 和 .cpp 檔案可新增事件處理常式的自訂邏輯。 自動產生的檔案代表已從 XAML 標記轉換成 C++/CX。 不要修改這些檔案，但您可以研究它們以深入了解程式碼後置的運作方法。 基本上，產生的檔案包含 XAML 根元素的部分類別定義；這個類別與您在 \*.xaml.h 和 .cpp 檔案修改的類別相同。 產生的檔案將 XAML UI 子元素宣告為類別成員，讓您能夠在自己撰寫的程式碼中參考它們。 建置期間，產生的程式碼會與您的程式碼合併成一個完整的類別定義，然後進行編譯。

我們先來看看專案檔案。

-   **App.xaml、App.xaml.h、App.xaml.cpp：** 代表應用程式物件，該物件是應用程式的進入點。 App.xaml 不包含頁面特定 UI 標記，但您可以新增要從任何頁面存取的 UI 樣式和其他元素。 程式碼後置檔案包含 **OnLaunched** 和 **OnSuspending** 事件的處理常式。 通常，您會在這裡新增自訂程式碼，在應用程式啟動時起始應用程式，並在應用程式暫停或終止時執行清理。
-   **MainPage.xaml、MainPage.xaml.h、MainPage.xaml.cpp：** 包含應用程式預設「起始」頁的 XAML 標記和程式碼後置。 它沒有瀏覽支援或內建控制項。
-   **pch.h、pch.cpp：** 預先編譯的標頭檔，及將它內含在您專案中的檔案。 在 pch.h，您可以包含任何不常變更的標頭，以及包含在方案其他檔案的標頭。
-   **package.appxmanifest：** 描述應用程式所需的裝置功能，以及應用程式版本資訊和其他中繼資料的 XML 檔案。 若要在 [資訊清單設計工具] 開啟此檔案，只要按兩下即可。
-   **HelloWorld\_TemporaryKey.pfx：** 從 Visual Studio 將應用程式部署到此電腦所需的金鑰。

## <a name="a-first-look-at-the-code"></a>初窺程式碼

如果您仔細觀察 App.xaml.h 的程式碼、共用專案中 App.xaml.cpp 的程式碼，就會發現大部分的 C++ 程式碼看起來都很熟悉。 不過，如果您是 Windows 執行階段 app 的新手，或曾用過 C++/CLI，可能不熟悉某些語法元素。 下列是 C++/CX 中最常見的非標準語法元素：

**參考類別**

幾乎所有 Windows 執行階段類別，包含 Windows API 中的所有類型--XAML 控制項、應用程式的頁面、App 類別本身、所有裝置和網路物件，所有容器類型--都會宣告為 **ref class** (有些 Windows 類型是 **value class** 或 **value struct** )。 ref 類別可從任何語言取用。 在 C++/CX 中，這些類型的存留期由自動參考計數 (而非記憶體回收) 管理，因此絕不能明確刪除這些物件。 您也可以建立自己的 ref 類別。

```cpp
namespace HelloWorld
{
   /// <summary>
   /// An empty page that can be used on its own or navigated to within a Frame.
   /// </summary>
   public ref class MainPage sealed
   {
      public:
      MainPage();
   };
}
```    

所有 Windows 執行階段類型都必須在命名空間內宣告，與 ISO C++ 不同的是，這些類型本身有存取範圍修飾詞。 **public** 修飾詞可讓命名空間外的 Windows 執行階段元件看見類別。 **sealed** 關鍵字代表類別不能做為基底類別使用。 幾乎所有 ref 類別都是密封的；因為 Javascript 無法理解類別繼承，所以並未受到廣泛的使用。

**ref new** 和 **^ (hats)**

您可以使用 ^ (hat) 運算子宣告 ref 類別的變數，ref new 關鍵字則可用來具現化物件。 之後，您就能以與 C++ 指標一樣的方式，使用 -&gt; 運算子存取物件的執行個體方法。 和 ISO C++ 一樣，要使用 :: 運算子存取靜態方法。

在下列程式碼中，我們使用完整名稱來具現化物件，並使用 -&gt; 運算子呼叫執行個體方法。

```cpp
Windows::UI::Xaml::Media::Imaging::BitmapImage^ bitmapImage =
     ref new Windows::UI::Xaml::Media::Imaging::BitmapImage();

bitmapImage->SetSource(fileStream);
```

我們通常會在 .cpp 檔案新增 `using namespace  Windows::UI::Xaml::Media::Imaging` 指示詞和 auto 關鍵字，相同的程式碼看起來像這樣：

```cpp
auto bitmapImage = ref new BitmapImage();
bitmapImage->SetSource(fileStream);
```

**屬性**

ref 類別可以有屬性，與受管理的語言一樣，這些屬性是特殊的成員函式，以欄位的形式顯示以取用程式碼。

```cpp
public ref class SaveStateEventArgs sealed
{
   public:
   // Declare the property
   property Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ PageState
   {
      Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ get();
   }
   ...
};

   ...
   // consume the property like a public field
   void PhotoPage::SaveState(Object^ sender, Common::SaveStateEventArgs^ e)
   {    
      if (mruToken != nullptr && !mruToken->IsEmpty())
   {
      e->PageState->Insert("mruToken", mruToken);
   }
}
```

**委派**

如同受管理的語言，委派是一種參考類型，可使用特定簽章封裝函式。 它們最常用於事件及事件處理常式

```cpp
// Delegate declaration (within namespace scope)
public delegate void LoadStateEventHandler(Platform::Object^ sender, LoadStateEventArgs^ e);

// Event declaration (class scope)
public ref class NavigationHelper sealed
{
   public:
   event LoadStateEventHandler^ LoadState;
};

// Create the event handler in consuming class
MainPage::MainPage()
{
   auto navigationHelper = ref new Common::NavigationHelper(this);
   navigationHelper->LoadState += ref new Common::LoadStateEventHandler(this, &MainPage::LoadState);
}
```

## <a name="adding-content-to-the-app"></a>將內容新增到應用程式

讓我們在應用程式中新增一些內容。

**步驟 1：修改起始頁**

1.  在 [方案總管] 中，開啟 MainPage.xaml。
2.  將下列 XAML 新增到根 [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) (在其結束標記的正前方)，以建立 UI 的控制項。 它包含一個 [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel)，其中有會詢問使用者名稱的 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)、會接受使用者名稱的 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 元素、一個 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)，以及另一個 **TextBlock** 元素。

    ```xaml
    <StackPanel x:Name="contentPanel" Margin="120,30,0,0">
        <TextBlock HorizontalAlignment="Left" Text="Hello World" FontSize="36"/>
        <TextBlock Text="What's your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
        </StackPanel>
        <TextBlock x:Name="greetingOutput"/>
    </StackPanel>
    ```

3.  到目前為止，您已建立一個非常基本的通用 Windows app。 若要查看該 UWP app 的外觀，可按 F5 在偵錯模式中建置、部署及執行該 app。

預設啟動顯示畫面最先顯示。 它有應用程式資訊清單檔案中指定的影像—Assets\\SplashScreen.scale-100.png—及背景色彩。 若要了解如何自訂啟動顯示畫面，請參閱[新增啟動顯示畫面](/previous-versions/windows/apps/hh465332(v=win.10))。

當啟動顯示畫面消失後，您的 app 便會出現。 它會顯示 app 的主頁面。

![包含控制項的 UWP 應用程式畫面](images/xaml-hw-app2.png)

它還沒有太多功能，但是，恭喜您，您已經建立第一個通用 Windows 平台 app 了！

若要停止偵錯並關閉 app，請返回 Visual Studio 並按 Shift+F5。

如需詳細資訊，請參閱[從 Visual Studio 執行市集 app](/visualstudio/debugger/run-store-apps-from-visual-studio)。

您可以在應用程式的 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 輸入文字，但按一下 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 不會有任何反應。 在之後的步驟中，您要為按鈕的 [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件建立事件處理常式，以顯示個人化的問候語。

## <a name="step-2-create-an-event-handler"></a>步驟 2：建立事件處理常式

1.  在 MainPage.xaml 的 XAML 或設計檢視中，於 [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) 選取您之前新增的 "Say Hello" [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)。
2.  按 F4 開啟 [屬性視窗]，然後選擇 [事件] 按鈕 (![事件按鈕](images/eventsbutton.png))。
3.  找尋 [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件。 在文字方塊中，輸入處理 **Click** 事件的函式名稱。 在這個範例中，輸入 "Button\_Click"。

    ![屬性視窗、事件檢視](images/xaml-hw-event.png)

4.  按 Enter 鍵。 這時候會在 MainPage.xaml.cpp 中建立事件處理常式方法並開啟它，這樣您就可以新增要在事件發生時執行的程式碼。

   同時，在 MainPage.xaml 中， [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 的 XAML 已更新，以宣告 [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件處理常式，就像這樣：

```xaml
<Button Content="Say &quot;Hello&quot;" Click="Button_Click"/>
```

   您也可能已直接手動將它新增至 XAML 程式碼，則這會有所幫助 (如果設計工具不會載入)。 如果您手動輸入這個資訊，請輸入 "Click"，然後讓 IntelliSense 呈現出選擇加入新事件處理常式的選項。 如此一來，Visual Studio 會建立必要的方法宣告和虛設常式。

   如果在轉譯期間發生無法處理的例外狀況，設計工具會無法載入。 在設計工具中的轉譯涉及了執行頁面的設計階段版本。 停用執行中的使用者程式碼會很有幫助。 您可以藉由在 [工具]、[選項] 對話方塊變更設定來執行此動作。 在 [XAML 設計工具] 下，取消核取 [在 XAML 設計工具中執行專案程式碼 (如果支援)]。

5.  在 MainPage.xaml.cpp 中，將下列程式碼新增到您剛才建立的 **Button\_Click** 事件處理常式。 此程式碼會從 `nameInput` [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 控制項擷取使用者的名稱，並用其來建立問候語。 `greetingOutput` [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 會顯示結果。

```cpp
void HelloWorld::MainPage::Button_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    greetingOutput->Text = "Hello, " + nameInput->Text + "!";
}
```

6.  將專案設定為起始專案，然後按 F5 建置並執行應用程式。 在文字方塊中輸入名稱並按一下按鈕時，應用程式會顯示個人化問候語。

![顯示訊息的應用程式畫面](images/xaml-hw-app4.png)

## <a name="step-3-style-the-start-page"></a>步驟 3：設定起始頁的樣式

### <a name="choosing-a-theme"></a>選擇佈景主題

自訂您 app 的外觀和操作方式非常容易。 根據預設，您的 app 會使用淺色樣式的資源。 系統資源也包含淺色佈景主題。 讓我們來嘗試一下，再看看它的外觀如何。

**切換至深色佈景主題**

1.  開啟 App.xaml。
2.  在開頭的 [**Application**](/uwp/api/Windows.UI.Xaml.Application) 標記中，編輯 [**RequestedTheme**](/uwp/api/windows.ui.xaml.application.requestedtheme) 屬性並將其值設定為 **Dark** ：

    ```xaml
    RequestedTheme="Dark"
    ```

    下列為完整的 [**Application**](/uwp/api/Windows.UI.Xaml.Application) 標記，其中包含深色佈景主題：

    ```xaml
    <Application
    x:Class="HelloWorld.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloWorld"
    RequestedTheme="Dark">
    ```

3.  按 F5 來建置並執行它。 請注意，它使用深色佈景主題。

    ![深色佈景主題的 App 畫面](images/xaml-hw-app3.png)

您應該使用哪個佈景主題？ 隨您的喜愛選擇。 以下是我們的想法：對於主要顯示影像或視訊的應用程式，建議您使用深色佈景主題；對於包含大量文字的應用程式，則建議使用淺色佈景主題。 如果您要使用自訂色彩配置，請使用與您 app 外觀及操作方式最搭配的佈景主題。 在本教學課程的其餘部分，我們會在螢幕擷取畫面使用淺色佈景主題。

**注意** ：佈景主題會在應用程式啟動時套用，且無法在應用程式執行時進行變更。

### <a name="using-system-styles"></a>使用系統樣式

現在，Windows 應用程式的文字非常小，而且不易閱讀。 我們透過套用系統樣式來修正這個問題。

**變更元素的樣式**

1.  在 Windows 專案中開啟 MainPage.xaml。
2.  在 XAML 或設計檢視中，選取之前新增的 [What's your name?] [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)。
3.  在 [屬性]  視窗 ( **F4** )，選擇右上角的 [屬性] 按鈕 (![屬性按鈕](images/propertiesbutton.png))。
4.  展開 [文字]  群組並將字型大小設定為 18 像素。
5.  展開 [其他]  群組，找到 [樣式]  屬性。
6.  按一下屬性標記 ([樣式]  屬性右側的綠色方塊)，然後在功能表上，選擇 [系統資源]   > [BaseTextBlockStyle]  。

     **BaseTextBlockStyle** 是在 <root>\\Program Files\\Windows Kits\\10\\Include\\winrt\\xaml\\design\\generic.xaml 的 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 中定義的資源。

    ![屬性視窗、屬性檢視](images/xaml-hw-style-cpp.png)

     在 XAML 設計表面中，文字的外觀改變了。 在 XAML 編輯器中， [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 的 XAML 已更新：

```xaml
<TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
```

7.  重複上述程序來設定字型大小，並將 **BaseTextBlockStyle** 指派給 `greetingOutput`[**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 元素。

    **提示** ：雖然這個 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 沒有任何文字，但是當您將指標移至 XAML 設計表面上時，會以藍色外框顯示其位置，讓您可以選取它。  

    您的 XAML 現在看起來應該會像這樣：

```xaml
<StackPanel x:Name="contentPanel" Margin="120,30,0,0">
    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="18" Text="What's your name?"/>
    <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
        <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
        <Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="Button_Click"/>
    </StackPanel>
    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="18" x:Name="greetingOutput"/>
</StackPanel>
```

8.  按 F5 來建置並執行應用程式。 它現在看起來像這樣：

![文字較大的應用程式畫面](images/xaml-hw-app5.png)

### <a name="step-4-adapt-the-ui-to-different-window-sizes"></a>步驟 4：隨著不同的視窗大小調整 UI

現在要讓 UI 可隨著不同的螢幕大小進行調整，使其在行動裝置上看起來很美觀。 若要這樣做，您要新增 [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager)，並設定不同視覺狀態套用的屬性。

**調整 UI 配置**

1.  在 XAML 編輯器中，將 XAML 的這個區塊新增到根 [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) 元素的開頭標記之後。

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="wideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="641" />
            </VisualState.StateTriggers>
        </VisualState>
        <VisualState x:Name="narrowState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="0" />
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

2.  在本機電腦上偵錯應用程式。 請注意，除非視窗的寬度小於 641 裝置獨立畫素 (DIP)，否則 UI 看起來會和之前的一樣。
3.  在行動裝置模擬器上偵錯應用程式。 請注意，UI 會使用您在 `narrowState` 中定義的屬性，並正確地顯示在小型螢幕上。

![已設定文字樣式的行動應用程式畫面](images/hw10-screen2-mob.png)

如果您已在舊版的 XAML 中使用 [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager)，您可能會注意到這裡的 XAML 使用簡化的語法。

名為 `wideState` 的 [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) 有一個 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 的 [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 屬性設為 641。 這表示只有在視窗寬度不小於最小值 641 DIP 時才會套用狀態。 您不需為此狀態定義任何 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 物件，因此它會使用您在頁面內容的 XAML 中定義的配置屬性。

第二個 [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) (`narrowState`) 有一個 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 的 [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 屬性設為 0。 當視窗寬度大於 0 但小於 641 DIP 時，會套用這個狀態。 (在 641 DIP 時，會套用 `wideState`)。在這個狀態中，您要定義一些 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 物件，以變更 UI 中的控制項配置屬性：

-   將 `contentPanel` 元素的左邊界從 120 減少至 20。
-   將 `inputPanel` 元素的 [**Orientation**](/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) 從 **Horizontal** 變更為 **Vertical** 。
-   新增 4 個 DIP 的上邊界至 `inputButton` 元素。

### <a name="summary"></a>摘要

恭喜，您已經完成第一個教學課程了！ 本教學課程教導如何新增內容至 Windows 通用 app、如何新增互動功能，以及如何變更其外觀。

## <a name="next-steps"></a>接下來的步驟

如果您擁有目標為 Windows 8.1 和 (或) Windows Phone 8.1 的 Windows 通用 app 專案時，可將它移植到 Windows 10。 沒有任何自動處理程序可用來進行此動作，但您可以手動完成此動作。 開始使用新的 Windows 通用專案，以取得最新的專案系統結構與資訊清單檔案、將程式碼檔案複製到專案的目錄結構、將項目新增到專案，然後根據本主題中的指導方針，使用 [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) 重新撰寫您的 XAML。 如需詳細資訊，請參閱[將 Windows Runtime 8 專案移植到通用 Windows 平台 (UWP) 專案](../porting/w8x-to-uwp-porting-to-a-uwp-project.md)和[移植到通用 Windows 平台 (C++)](/cpp/porting/porting-to-the-universal-windows-platform-cpp)。

如果您有想要與 UWP 應用程式整合的現有 C++ 程式碼，例如為現有應用程式建立新的 UWP UI，請參閱[如何：在通用 Windows 專案中使用現有的 C++ 程式碼](/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app)。