---
author: stevewhims
description: 本主題會引導您完成的步驟建立簡單的自訂控制項使用 C + + /winrt。 您可以在此處資訊來建立您自己的豐富的功能和可自訂的 UI 控制項上建置。
title: XAML 自訂 （化） 的控制項使用 C + + /winrt
ms.author: stwhi
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 XAML 中，自訂，範本化，控制項
ms.localizationpriority: medium
ms.openlocfilehash: c108175c66d27b2cdbd910a0f7653ca1befb68e9
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2018
ms.locfileid: "2917307"
---
# <a name="xaml-custom-templated-controls-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>XAML （範本化） 的自訂控制項[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

其中一個最強大的功能的通用 Windows 平台 (UWP) 是在使用者介面 (UI) 堆疊提供建立根據 XAML[控制項](/uwp/api/windows.ui.xaml.controls.control)類型的自訂控制項的彈性。 XAML UI 架構提供功能，例如[自訂相依性屬性](/windows/uwp/xaml-platform/custom-dependency-properties)並附加的屬性和[控制項範本](/windows/uwp/design/controls-and-patterns/control-templates)，可讓您輕鬆地建立豐富的功能和可自訂控制項。 本主題會引導您完成的步驟建立 （範本化） 的自訂控制項使用 C + + /winrt。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>建立空白的應用程式 (BgLabelControlApp)
在 Microsoft Visual Studio 中，藉由建立新的專案來開始。 建立**Visual c + + 空白的應用程式 (C + + /winrt)** 專案，並將它命名為*BgLabelControlApp*。

我們要撰寫新的類別來代表自訂 （範本化） 的控制項。 我們在相同的編譯單位裡撰寫和使用此類別。 但我們想要能夠具現化此類別從 XAML 標記中，以及適用於這個原因，它會一個執行階段類別。 且我們會使用 C++/WinRT 撰寫和使用它。

撰寫新執行階段類別的第一個步驟是將一個新的 **Midl 檔案 (.idl)** 項目新增至專案。 將它名為 `BgLabelControl.idl`。 刪除 `BgLabelControl.idl`的預設內容，並在此執行階段類別宣告中貼上。

```idl
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Windows.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

上述清單中的顯示的模式，您可以依循宣告相依性屬性 (DP) 時。 有兩個部分每個 DP。 首先，您可以宣告[DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)類型的唯讀靜態屬性。 它有 DP 再加上*屬性*的名稱。 您將在您的實作中使用這個靜態屬性。 第二，您宣告的讀寫執行個體屬性的類型及您 DP 的名稱。

> [!NOTE]
> 如果您想 DP 的浮點數類型，請`double`(`Double` [MIDL](/uwp/midl-3/)3.0)。 宣告和實作類型的 DP `float` (`Single`在 MIDL 中)，並產生錯誤，然後在 XAML 標記中，該 DP 的設定值*無法建立 'Windows.Foundation.Single' 從文字 '<NUMBER>'*。

儲存檔案並建置專案。 在建置程序期間，執行 `midl.exe` 工具建立描述執行階段類別的 Windows 執行階段中繼資料檔案 (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)。 然後，執行 `cppwinrt.exe` 工具產生原始碼檔案在撰寫和使用執行階段類別中支援您。 這些檔案包含虛設常式，可協助您開始實作您在 IDL 中宣告的**BgLabelControl**執行階段類別。 這些虛設常式為 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` 與 `BgLabelControl.cpp`。

從 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` 將虛設常式檔案 `BgLabelControl.h` 和 `BgLabelControl.cpp` 複製到專案資料夾中，也就是 `\BgLabelControlApp\BgLabelControlApp\`。 在 **\[方案總管\]** 中，確定 **\[顯示所有檔案\]** 已切換成開啟。 按一下滑鼠右鍵您複製的虛設常式檔案，然後按一下 **\[加入至專案\]**。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>實作**BgLabelControl**自訂控制項類別
現在，我們開啟 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` 與 `BgLabelControl.cpp` 並實作我們的執行階段類別。 在`BgLabelControl.h`、 變更的建構函式，若要設定的預設樣式索引鍵，實作的**標籤**和**LabelProperty**、 新增名為**OnLabelChanged**處理的相依性屬性的值變更的靜態事件處理常式以及新增私用成員若要儲存**LabelProperty**支援欄位。

在這個逐步解說中，我們將不會使用**OnLabelChanged**。 但它有，讓您可以了解如何登錄相依性屬性的屬性變更回呼。

新增之後，您`BgLabelControl.h`如下所示。

```cppwinrt
// BgLabelControl.h
...
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
    BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

    winrt::hstring Label()
    {
        return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
    }

    void Label(winrt::hstring const& value)
    {
        SetValue(m_labelProperty, winrt::box_value(value));
    }

    static Windows::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

在`BgLabelControl.cpp`，就像這樣的靜態成員定義。

```cppwinrt
// BgLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
);

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e) {}
...
```

## <a name="design-the-default-style-for-bglabelcontrol"></a>適用於**BgLabelControl**設計預設樣式

在其建構函式， **BgLabelControl**設定預設樣式索引鍵本身。 但哪些** 預設樣式嗎？ 自訂 （範本化） 的控制項必須具有預設樣式&mdash;包含預設的控制項範本&mdash;它可以用來轉譯本身與，以便在控制項的取用者不會設定樣式和/或範本。 在本節中，我們會標記檔案加入專案包含我們的預設樣式中。

在您的專案節點下，建立新的資料夾，並將它命名為"Themes"。 在`Themes`，新增新的項目類型**Visual c + +** 的 > **XAML** > **XAML 檢視**，並將它命名為 「 Generic.xaml 」。 資料夾和檔案名稱必須讓 XAML 架構以尋找為自訂控制項的預設樣式會像這樣。 刪除的預設內容`Generic.xaml`，和在下列標記中貼上。

```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

在此情況下，預設樣式設定的唯一屬性是控制項範本。 範本包含的方形 （其背景會繫結至 XAML[控制項](/uwp/api/windows.ui.xaml.controls.control)類型的所有執行個體都**背景**屬性） 和文字元素 （其文字繫結至**BgLabelControl::Label**相依性屬性）。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>將**BgLabelControl**的執行個體新增到主要 UI 頁面

開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 後 （在**StackPanel**中) 內的**按鈕**項目，新增下列標記。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

此外，新增下列 include 指示詞到`MainPage.h`以便**MainPage**類型 （編譯的 XAML 標記和命令式程式碼的組合） 是**BgLabelControl**自訂控制項類型的注意。

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

現在建置並執行專案。 您會看到預設控制項範本繫結為背景筆刷和標籤，在標記中的**BgLabelControl**執行個體。

本逐步解說示範自訂 （範本化） 的控制項的簡單範例了在 C + + /winrt。 您可以任意豐富且完整功能，讓您的自訂控制項。 例如，自訂控制項可能需要項目一樣複雜的可編輯的資料方格、 影片播放程式或視覺化檢視的 3D 幾何的形式。

## <a name="important-apis"></a>重要 API
* [控制項](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)

## <a name="related-topics"></a>相關主題
* [控制項範本](/windows/uwp/design/controls-and-patterns/control-templates)
* [自訂相依性屬性](/windows/uwp/xaml-platform/custom-dependency-properties)