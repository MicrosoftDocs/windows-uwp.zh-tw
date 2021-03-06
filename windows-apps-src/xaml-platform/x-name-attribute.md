---
description: 唯一識別用於從程式碼後置或一般程式碼中存取具現化物件的物件元素。
title: xName 屬性
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5fd9c2b482eb79fcdace2c068afa21bad673de2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161672"
---
# <a name="xname-attribute"></a>x:Name 屬性


唯一識別用於從程式碼後置或一般程式碼中存取具現化物件的物件元素。 一旦套用到支援程式撰寫模型，在建構函示傳回時，**x:Name** 可以視為等同於含有物件參考的變數。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object x:Name="XAMLNameValue".../>
```

## <a name="xaml-values"></a>XAML 值

| 詞彙 | 描述 |
|------|-------------|
| XAMLNameValue | 符合 XamlName 文法限制的字串。 |

##  <a name="xamlname-grammar"></a>XamlName 文法

下列是字串的基準文法，該字串在這個 XAML 實作中被當做是一個金鑰：

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   字元會限制為較低的 ASCII 範圍，更明確地說，是以羅馬字母大寫和小寫字母、數位和底線 (\_) 字元。
-   不支援 Unicode 字元範圍。
-   名稱開頭不可以是數字。 \_如果使用者提供一個數位做為初始字元，某些工具的執行會在 () 到字串，或者工具會根據包含數位的其他值會自動產生**x：Name**值。

## <a name="remarks"></a>備註

在處理 XAML 時，指定的 **x:Name** 會成為在基礎程式碼中建立的欄位名稱，並且該欄位含有物件參考。 建立此欄位的處理程序是由 MSBuild 目標步驟執行的，它們也負責加入 XAML 檔案的部分類別和其程式碼後置。 這個行為不一定是 XAML 語言指定的，它是 XAML 的通用 Windows 平台 (UWP) 程式撰寫為了在其程式撰寫模型或應用程式模型中使用 **x:Name** 所套用的特定實作。

每個定義的 **x:Name** 在 XAML 命名範圍內都必須是唯一的。 一般而言，XAML 命名範圍是定義在載入的頁面的根元素層級，並且包含單一 XAML 頁面該元素底下的所有元素。 其他 XAML 命名範圍是由在該頁面上定義的任何控制項範本或資料範本所定義。 在執行階段，會針對從套用的控制項範本建立的物件樹根目錄建立另一個 XAML 命名範圍，也會由透過呼叫 [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) 所建立的物件樹建立另一個 XAML 命名範圍。 如需詳細資訊，請參閱 [XAML 命名範圍](xaml-namescopes.md)。

當元素被引入到設計介面時，設計工具通常會自動為元素產生 **x:Name** 值。 自動產生配置會依據您使用的設計工具而不同，但典型的配置是產生一個以支援元素的類別名稱開頭的字串，後面則是一個遞增的整數。 例如，如果您將第一個 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 元素引入設計工具，您會發現在 XAML 中，這個元素含有 "Button1" 的 **x:Name** 屬性值。

**x:Name** 不能設定在 XAML 屬性元素語法中，或是使用 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 的程式碼中。 **x:Name** 只能使用 XAML 屬性語法設定在元素中。

**注意**   針對 c + +/CX 應用程式，系統不會針對 XAML 檔案或頁面的根項目建立**x：Name**參考的支援欄位。 如果您需要從 C++ 程式碼後置參考根物件，請使用其他 API 或樹狀目錄周遊。 例如，您可以呼叫 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 以取得已知名稱的子元素，然後再呼叫 [**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent)。

### <a name="xname-and-other-name-properties"></a>x:Name 和其他 Name 屬性

UWP XAML 中使用的一些類型也具有名為 **Name** 的屬性。 例如，[**FrameworkElement.Name**](/uwp/api/windows.ui.xaml.frameworkelement.name) 和 [**TextElement.Name**](/uwp/api/windows.ui.xaml.documents.textelement.name)。

如果 **Name** 可做為元素上的可設定屬性，則在 XAML 中就可以交替使用 **Name** 和 **x:Name**，但如果在同一個元素上同時指定這兩個屬性，就會發生錯誤。 也有情況是有 **Name** 屬性，但是為唯讀屬性 (例如 [**VisualState.Name**](/uwp/api/windows.ui.xaml.visualstate.name))。 如果是這種情況，請一律使用 **x:Name** 在 XAML 中命名該元素，而唯讀 **Name** 則是用於一些較不常見的程式碼案例。

**注意** 一般而言，不應該使用   [**FrameworkElement.Name**](/uwp/api/windows.ui.xaml.frameworkelement.name) 來變更原先由 **x:Name** 所設定的值，雖然有些案例是這項一般規則的例外。 在典型的案例中，建立與定義 XAML 命名範圍屬於 XAML 處理器作業。 在執行階段修改 **FrameworkElement.Name** 會導致 XAML 命名範圍/私用欄位命名無法對齊，這會讓您的程式碼後置難以記錄。

### <a name="xname-and-xkey"></a>x:Name 和 x:Key

**x:Name** 可以當作屬性套用到 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 內的元素，用來替代 [x:Key 屬性](x-key-attribute.md)。 (**ResourceDictionary** 中的所有元素都必須具有 x:Key 或 x:Name 屬性，是一項常規)。這通用於[腳本動畫](../design/motion/storyboarded-animations.md)。 如需詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)一節。