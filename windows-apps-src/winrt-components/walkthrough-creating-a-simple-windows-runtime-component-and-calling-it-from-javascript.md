---
title: 建立C#或 Visual Basic Windows 執行階段元件，並從 JavaScript 呼叫它的逐步解說
description: 本逐步解說會示範如何使用 .NET 搭配 Visual Basic 或C#建立自己的 Windows 執行階段類型（封裝在 Windows 執行階段元件中），以及如何從使用 JavaScript 為 Windows 建立的 UWP 應用程式呼叫元件。
ms.assetid: 1565D86C-BF89-4EF3-81FE-35367DB8D671
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b7fc8e899b402dea21a11a0c8dce09646a84dae5
ms.sourcegitcommit: cc9f5a16386be78c12821a975e43497a0693abba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72578165"
---
# <a name="walkthrough-of-creating-a-c-or-visual-basic-windows-runtime-component-and-calling-it-from-javascript"></a>建立C#或 Visual Basic Windows 執行階段元件，並從 JavaScript 呼叫它的逐步解說

本逐步解說會示範如何使用 .NET 搭配 Visual Basic， C#或建立您自己的 Windows 執行階段類型（封裝在 Windows 執行階段元件中），以及如何從 JavaScript 通用 WINDOWS 平臺（UWP）應用程式呼叫該元件。

Visual Studio 可讓您輕鬆地在以C#或 Visual Basic 撰寫的 Windows 執行階段元件（WRC）專案內撰寫和部署您自己的自訂 Windows 執行階段類型，然後從 JavaScript 應用程式專案參考該 WRC，並從該應用程式取用這些自訂類型。

就內部而言，您的 Windows 執行階段類型可以使用 UWP 應用程式中所允許的任何 .NET 功能。

> [!NOTE]
> 如需詳細資訊，請參閱[使用C#和 Visual Basic Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)和[適用于 UWP 應用程式的 .net 總覽](/dotnet/api/index?view=dotnet-uwp-10.0)。

就外部而言，您的型別成員只能公開其參數和傳回值的 Windows 執行階段類型。 當您建立方案時，Visual Studio 會建立您的 .NET WRC 專案，然後執行可建立 Windows 中繼資料（winmd）檔案的組建步驟。 這是 Visual Studio 會包含在應用程式中的 Windows 執行階段元件。

> [!NOTE]
> .NET 會自動將一些常用的 .NET 類型（例如基本資料類型和集合類型）對應到其 Windows 執行階段對等專案。 這些 .NET 型別可以在 Windows 執行階段元件的公用介面中使用，而且會對元件的使用者顯示為對應的 Windows 執行階段類型。 請參閱[使用C#和 Visual Basic Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)。

## <a name="prerequisites"></a>必要條件：

- Windows 10
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)

## <a name="creating-a-simple-windows-runtime-class"></a>建立簡單的 Windows 執行階段類別

本節會建立 JavaScript UWP 應用程式，並將 Visual Basic 或C# Windows 執行階段元件專案新增至方案。 它會顯示如何定義 Windows 執行階段型別、從 JavaScript 建立型別的實例，以及呼叫靜態和實例成員。 範例應用程式的視覺化顯示故意是低索引鍵，以便將焦點放在元件上。

1. 在 Visual Studio 中，建立新的 JavaScript 專案：在功能表列上，依序選擇 [檔案]、[新增] 及 [專案]。 在 **\[新增專案\]** 對話方塊的 **\[已安裝的範本\]** 區段中，選擇 **\[JavaScript\]** ，然後依序選擇 **\[Windows\]** 和 **\[通用\]** 。 (如果無法使用 Windows，請確定您使用的是 Windows 8 或更新版本)。選擇 [空白的應用程式] 範本，並輸入 SampleApp 做為專案名稱。
2.  建立元件專案：在 [方案總管] 中，開啟 SampleApp 方案的捷徑功能表，然後依序選擇 [新增] 和 [新增專案]，將新的 C# 或 Visual Basic 專案新增至方案。 在 [新增專案] 對話方塊的 [已安裝的範本] 區段中，選擇 [Visual Basic] 或 [Visual C#]，然後依序選擇 [Windows] 和 [通用]。 選擇 [Windows 執行階段元件] 範本，然後輸入 **SampleComponent** 做為專案名稱。
3.  將類別的名稱變更為 **Example**。 請注意，根據預設，類別會標記為 **public sealed** (在 Visual Basic 中為 **Public NotInheritable**)。 從您的元件公開的所有 Windows 執行階段類別都必須密封。
4.  將兩個簡單成員加入至類別、**static** 方法 (在 Visual Basic 中為 **Shared**) 和執行個體屬性：

    > [!div class="tabbedCodeSnippets"]
    > ```csharp
    > namespace SampleComponent
    > {
    >     public sealed class Example
    >     {
    >         public static string GetAnswer()
    >         {
    >             return "The answer is 42.";
    >         }
    >
    >         public int SampleProperty { get; set; }
    >     }
    > }
    > ```
    > ```vb
    > Public NotInheritable Class Example
    >     Public Shared Function GetAnswer() As String
    >         Return "The answer is 42."
    >     End Function
    >
    >     Public Property SampleProperty As Integer
    > End Class
    > ```

5.  選擇性：若要啟用新加入成員的 IntelliSense，請在 [方案總管] 中開啟 SampleComponent 專案的捷徑功能表，然後選擇 [建置]。
6.  在 [方案總管] 的 JavaScript 專案中，開啟 **\[參考\]** 的捷徑功能表，然後選擇 **\[加入參考\]** 以開啟 **\[參考管理員\]** 。 選擇 [專案]，然後選擇 [方案]。 選取 SampleComponent 專案的核取方塊，然後選擇 [確定] 以加入參考。

## <a name="call-the-component-from-javascript"></a>從 JavaScript 呼叫元件

若要使用來自 JavaScript 的 Windows 執行階段 類型，請在 Visual Studio 範本所提供之 default.js 檔 (在專案的 js 資料夾中) 的匿名函式中，加入下列程式碼。 它應該在應用程式.oncheckpoint 事件處理常式之後，且在呼叫應用程式.start 之前。

```javascript
var ex;

function basics1() {
   document.getElementById('output').innerHTML =
        SampleComponent.Example.getAnswer();

    ex = new SampleComponent.Example();

   document.getElementById('output').innerHTML += "<br/>" +
       ex.sampleProperty;

}

function basics2() {
    ex.sampleProperty += 1;
    document.getElementById('output').innerHTML += "<br/>" +
        ex.sampleProperty;
}
```

請注意，每個成員名稱的第一個字母會從大寫變成小寫。 這個轉換是 JavaScript 為了方便您使用 Windows 執行階段而提供的支援。 命名空間和類別名稱都會依照 Pascal 命名法的大小寫慣例。 成員名稱會依照 Camel 命名法的大小寫慣例，但全部小寫的事件名稱除外。 請參閱[在 JavaScript 中使用 Windows 執行階段](/scripting/jswinrt/using-the-windows-runtime-in-javascript)。 Camel 命名法的大小寫慣例規則可能會造成混淆。 一系列的首字大寫字母通常會顯示為小寫，但若三個大寫字母後面緊接著一個小寫字母，則只有前兩個字母會以小寫顯示：例如，名為 IDStringKind 的成員會顯示為 idStringKind。 在 Visual Studio 中，您可以建置 Windows 執行階段元件專案，然後在 JavaScript 專案中使用 IntelliSense 以查看正確的大小寫。

.NET 也以類似的方式提供支援，讓您在 managed 程式碼中自然使用 Windows 執行階段。 這會在本文的後續章節中討論，並在[使用C#和 Visual Basic 的 Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)和[適用于 UWP 應用程式的 .net 支援和 Windows 執行階段](/dotnet/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime)文章中討論。

## <a name="create-a-simple-user-interface"></a>建立簡單的使用者介面

在 JavaScript 專案中，開啟 default.html 檔案並更新內容，如下列程式碼所示。 這段程式碼包含範例應用程式的整組控制項，並指定 Click 事件的函式名稱。

> **請注意**  當您第一次執行應用程式時，只支援 [basics1]] 和 [[basics2] 按鈕。

```html
<body>
            <div id="buttons">
            <button id="button1" >Basics 1</button>
            <button id="button2" >Basics 2</button>

            <button id="runtimeButton1">Runtime 1</button>
            <button id="runtimeButton2">Runtime 2</button>

            <button id="returnsButton1">Returns 1</button>
            <button id="returnsButton2">Returns 2</button>

            <button id="events1Button">Events 1</button>

            <button id="btnAsync">Async</button>
            <button id="btnCancel" disabled="disabled">Cancel Async</button>
            <progress id="primeProg" value="25" max="100" style="color: yellow;"></progress>
        </div>
        <div id="output">
        </div>
</body>
```

在 JavaScript 專案的 css 資料夾中，開啟 default.css。 以如下所示的方式修改 body 區段，並加入樣式來控制按鈕的配置和輸出文字的位置。

```css
body
{
    -ms-grid-columns: 1fr;
    -ms-grid-rows: 1fr 14fr;
    display: -ms-grid;
}

#buttons {
    -ms-grid-rows: 1fr;
    -ms-grid-columns: auto;
    -ms-grid-row-align: start;
}
#output {
    -ms-grid-row: 2;
    -ms-grid-column: 1;
}
```

現在加入事件接聽程式註冊程式碼，方法是在 default.js 的應用程式.onactivated 中，將 then 子句加入至 processAll 呼叫。 取代目前呼叫 setPromise 的程式碼行，並將它變更為下列程式碼：

```javascript
args.setPromise(WinJS.UI.processAll().then(function () {
    var button1 = document.getElementById("button1");
    button1.addEventListener("click", basics1, false);
    var button2 = document.getElementById("button2");
    button2.addEventListener("click", basics2, false);
}));
```

若要將事件加入至 HTML 控制項，比起直接在 HTML 中加入 click 事件處理常式，這會是更好的方式。 請參閱[建立 "Hello，World" 應用程式（JS）](/windows/uwp/get-started/create-a-hello-world-app-js-uwp)。

## <a name="build-and-run-the-app"></a>建置和執行應用程式

建置之前，請先根據您的電腦而定，將適用於所有專案的目標平台變更為 ARM、x64 或 x86。

若要建置並執行方案，請選擇 F5 鍵 (如果您收到執行階段錯誤訊息，指出未定義 SampleComponent，則會遺失類別庫專案的參考)。

Visual Studio 會先編譯類別庫，然後執行可執行 [Winmdexp.exe (Windows 執行階段中繼資料匯出工具)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) 的 MSBuild 工作，來建立 Windows 執行階段元件。 該元件隨附於包含 Managed 程式碼和描述該程式碼之 Windows 中繼資料的 .winmd 檔案中。 當您撰寫的程式碼在 Windows 執行階段元件中無效時，WinMdExp.exe 會產生組建錯誤訊息，且該錯誤訊息會顯示在 Visual Studio IDE 中。 Visual Studio 會將您的元件新增至 UWP 應用程式的應用程式套件（.appx 檔案），並產生適當的資訊清單。

選擇 [Basics 1] 按鈕，將靜態 GetAnswer 方法的傳回值指派至輸出區域、建立 Example 類別的執行個體，以及將其 SampleProperty 屬性的值顯示在輸出區域中。 輸出如下所示：

``` syntax
"The answer is 42."
0
```

選擇 [Basics 2] 按鈕來累加 SampleProperty 屬性的值，並將新值顯示於輸出區域中。 基本類型 (例如字串與數字) 可做為參數類型和傳回類型，而且可在 Managed 程式碼和 JavaScript 之間傳遞。 由於 JavaScript 中的數字會利用雙精度浮點數格式來儲存，因此它們會轉換為.NET Framework 數值類型。

> **請注意**  根據預設，您只能在 JavaScript 程式碼中設定中斷點。 若要 debug Visual Basic 或C#程式碼，請參閱在和C# Visual Basic 中建立 Windows 執行階段元件。

若要停止偵錯並關閉應用程式，請從應用程式切換至 Visual Studio，然後選擇 SHIFT+F5。

## <a name="using-the-windows-runtime-from-javascript-and-managed-code"></a>從 JavaScript 和 Managed 程式碼使用 Windows 執行階段

Windows 執行階段可以從 JavaScript 或 Managed 程式碼呼叫。 Windows 執行階段物件可以在這兩者之間來回傳遞，而且可從任一端處理事件。 不過，在這兩個環境中使用 Windows 執行階段類型的方式，會因某些細節而有所不同，因為 JavaScript 和 .NET 支援的 Windows 執行階段不同。 下列範例使用 [Windows.Foundation.Collections.PropertySet](/uwp/api/windows.foundation.collections.propertyset) 類別來示範這些差異。 在這個範例中，您會在 Managed 程式碼中建立 PropertySet 集合的執行個體並註冊事件處理常式，以追蹤集合中的變更。 接著，加入可取得集合的 JavaScript 程式碼、註冊其擁有的事件處理常式，然後使用該集合。 最後，加入可從 Managed 程式碼對集合進行變更的方法，並顯示用於處理 Managed 例外狀況的 JavaScript。

> **重要**  在此範例中，事件是在 UI 執行緒上引發。 如果您從背景執行緒引發事件 (例如，在非同步呼叫中)，則必須執行一些額外工作，JavaScript 才能處理事件。 如需詳細資訊，請參閱[在 Windows 執行階段元件中引發事件](raising-events-in-windows-runtime-components.md)。

在 SampleComponent 專案中，加入名為 PropertySetStats 的新 **public sealed** 類別 (在 Visual Basic 中為 **Public NotInheritable** 類別)。 此類別會包裝 PropertySet 集合，並處理它的 MapChanged 事件。 事件處理常式會追蹤所發生的各種變更數目，而 DisplayStats 方法會產生 HTML 格式的報告。 請注意其他的 **using** 陳述式 (在 Visual Basic 中為 **Imports** 陳述式)；小心地將此陳述式加入至現有的 **using** 陳述式，而不是覆寫它們。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using Windows.Foundation.Collections;
>
> namespace SampleComponent
> {
>     public sealed class PropertySetStats
>     {
>         private PropertySet _ps;
>         public PropertySetStats()
>         {
>             _ps = new PropertySet();
>             _ps.MapChanged += this.MapChangedHandler;
>         }
>
>         public PropertySet PropertySet { get { return _ps; } }
>
>         int[] counts = { 0, 0, 0, 0 };
>         private void MapChangedHandler(IObservableMap<string, object> sender,
>             IMapChangedEventArgs<string> args)
>         {
>             counts[(int)args.CollectionChange] += 1;
>         }
>
>         public string DisplayStats()
>         {
>             StringBuilder report = new StringBuilder("<br/>Number of changes:<ul>");
>             for (int i = 0; i < counts.Length; i++)
>             {
>                 report.Append("<li>" + (CollectionChange)i + ": " + counts[i] + "</li>");
>             }
>             return report.ToString() + "</ul>";
>         }
>     }
> }
> ```
> ```vb
> Imports System.Text
>
> Public NotInheritable Class PropertySetStats
>     Private _ps As PropertySet
>     Public Sub New()
>         _ps = New PropertySet()
>         AddHandler _ps.MapChanged, AddressOf Me.MapChangedHandler
>     End Sub
>
>     Public ReadOnly Property PropertySet As PropertySet
>         Get
>             Return _ps
>         End Get
>     End Property
>
>     Dim counts() As Integer = {0, 0, 0, 0}
>     Private Sub MapChangedHandler(ByVal sender As IObservableMap(Of String, Object),
>         ByVal args As IMapChangedEventArgs(Of String))
>
>         counts(CInt(args.CollectionChange)) += 1
>     End Sub
>
>     Public Function DisplayStats() As String
>         Dim report As New StringBuilder("<br/>Number of changes:<ul>")
>         For i As Integer = 0 To counts.Length - 1
>             report.Append("<li>" & CType(i, CollectionChange).ToString() &
>                           ": " & counts(i) & "</li>")
>         Next
>         Return report.ToString() & "</ul>"
>     End Function
> End Class
> ```

事件處理常式會遵循熟悉的 .NET Framework 事件模式，但事件的傳送者（在此案例中為 PropertySet 物件）會轉換成 IObservableMap&lt;字串，物件&gt; 介面（IObservableMap Visual Basic 中的字串，物件），這是 Windows 執行階段介面[IObservableMap&lt;K，V&gt;](/uwp/api/Windows.Foundation.Collections.IObservableMap_K_V_)的具現化。 （如有必要，您可以將傳送者轉換成其類型）。此外，事件引數會呈現為介面，而不是物件。

在 default.js 檔案中，新增 Runtime1 函式，如下所示。 此程式碼會建立 PropertySetStats 物件、取得其 PropertySet 集合，並加入自有的事件處理常式 (onMapChanged 函式)，來處理 MapChanged 事件。 對集合進行變更之後，runtime1 會呼叫 DisplayStats 方法來顯示變更類型的摘要。

```javascript
var propertysetstats;

function runtime1() {
    document.getElementById('output').innerHTML = "";

    propertysetstats = new SampleComponent.PropertySetStats();
    var propertyset = propertysetstats.propertySet;

    propertyset.addEventListener("mapchanged", onMapChanged);

    propertyset.insert("FirstProperty", "First property value");
    propertyset.insert("SuperfluousProperty", "Unnecessary property value");
    propertyset.insert("AnotherProperty", "A property value");

    propertyset.insert("SuperfluousProperty", "Altered property value")
    propertyset.remove("SuperfluousProperty");

    document.getElementById('output').innerHTML +=
        propertysetstats.displayStats();
}

function onMapChanged(change) {
    var result
    switch (change.collectionChange) {
        case Windows.Foundation.Collections.CollectionChange.reset:
            result = "All properties cleared";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemInserted:
            result = "Inserted " + change.key + ": '" +
                change.target.lookup(change.key) + "'";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemRemoved:
            result = "Removed " + change.key;
            break;
        case Windows.Foundation.Collections.CollectionChange.itemChanged:
            result = "Changed " + change.key + " to '" +
                change.target.lookup(change.key) + "'";
            break;
        default:
            break;
     }

     document.getElementById('output').innerHTML +=
         "<br/>" + result;
}
```

您在 JavaScript 中處理 Windows 執行階段事件的方式，與您在 .NET Framework 程式碼中處理這類事件的方式大不相同。 JavaScript 事件處理常式只接受一個引數。 當您在 Visual Studio 偵錯工具中檢視此物件時，第一個屬性為傳送者。 事件引數介面的成員也會直接顯示在此物件上。

若要執行應用程式，請選擇 F5 鍵。 如果類別並未密封，您就會收到錯誤訊息：「目前不支援匯出未密封的類型 'SampleComponent.Example'。 請將它標記為已密封」。

選擇 [Runtime 1] 按鈕。 事件處理常式會在加入或變更元素時顯示變更，最後呼叫 DisplayStats 方法來產生計數摘要。 若要停止偵錯並關閉應用程式，請切換回 Visual Studio，然後選擇 SHIFT+F5。

若要從 Managed 程式碼加入另外兩個項目至 PropertySet 集合，請將下列程式碼加入至 PropertySetStats 類別：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public void AddMore()
> {
>     _ps.Add("NewProperty", "New property value");
>     _ps.Add("AnotherProperty", "A property value");
> }
> ```
> ```vb
> Public Sub AddMore()
>     _ps.Add("NewProperty", "New property value")
>     _ps.Add("AnotherProperty", "A property value")
> End Sub
> ```

此程式碼強調您在這兩個環境中使用 Windows 執行階段類型之方式的其他差異。 如果您自行輸入此程式碼，將會發現 IntelliSense 不會顯示您在 JavaScript 程式碼中使用的 insert 方法， 相反地，它會顯示 .NET 中的集合通常會看到的 Add 方法。 這是因為在 Windows 執行階段和 .NET 中，有些常用的集合介面具有不同的名稱，但功能類似。 當您在 Managed 程式碼中使用這些介面時，這些介面會顯示為其 .NET Framework 對等項目。 這會在[使用C#和 Visual Basic 的 Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)中討論。 當您在 JavaScript 中使用相同的介面時，來自 Windows 執行階段的唯一變更就是成員名稱開頭的大寫字母變成小寫。

最後，若要呼叫可處理例外狀況的 AddMore 方法，可將 runtime2 函式加入至 default.js。

```javascript
function runtime2() {
   try {
      propertysetstats.addMore();
    }
   catch(ex) {
       document.getElementById('output').innerHTML +=
          "<br/><b>" + ex + "<br/>";
   }

   document.getElementById('output').innerHTML +=
       propertysetstats.displayStats();
}
```

使用您先前所用的相同方法來加入事件處理常式註冊程式碼。

```javascript
var runtimeButton1 = document.getElementById("runtimeButton1");
runtimeButton1.addEventListener("click", runtime1, false);
var runtimeButton2 = document.getElementById("runtimeButton2");
runtimeButton2.addEventListener("click", runtime2, false);
```

若要執行應用程式，請選擇 F5 鍵。 依序選擇 **\[Runtime 1\]** 和 **\[Runtime 2\]** 。 JavaScript 事件處理常式會將第一個變更回報至集合。 不過，第二個變更具有重複的索引鍵。 .NET Framework 字典的使用者預期 Add 方法會擲回例外狀況，而實際情況就是如此。 JavaScript 會處理 .NET 例外狀況。

> **注意**  您無法從 JavaScript 程式碼顯示例外狀況的訊息。 因為訊息文字會由堆疊追蹤所取代。 如需詳細資訊，請參閱在中C#建立 Windows 執行階段元件和 Visual Basic 中的「擲回例外狀況」。

相反地，當 JavaScript 以重複的索引鍵呼叫 insert 方法時，項目值就會變更。 此行為差異是由於 JavaScript 和 .NET 支援 Windows 執行階段的不同方式，如[Windows 執行階段元件C#和 Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)中所述。

## <a name="returning-managed-types-from-your-component"></a>從您的元件傳回 Managed 類型

如先前所討論，您可以在 JavaScript 程式碼與 C# 或 Visual Basic 程式碼之間，自由地來回傳遞原生 Windows 執行階段類型。 類型名稱和成員名稱在這兩個環境中通常都相同 (但在 JavaScript 中以小寫字母開頭的成員名稱除外)。 不過，在上一節中，PropertySet 類別在 Managed 程式碼內看起來有不同的成員 （例如，在 JavaScript 中，您呼叫了 insert 方法，而在 .NET 程式碼中，您呼叫了 Add 方法）。本節將探討這些差異對傳遞至 JavaScript .NET Framework 類型的影響。

除了傳回您在元件中建立或從 JavaScript 傳遞至元件的 Windows 執行階段類型之外，還可以將在 Managed 程式碼中建立的 Managed 類型傳回 JavaScript，就好像是對應的 Windows 執行階段類型一樣。 即使在執行階段類別的第一個簡單範例中，成員的參數和傳回類型還是 Visual Basic 或 C# 基本類型，也就是 .NET Framework 類型。 若要針對集合進行示範，請將下列程式碼加入至 Example 類別，以建立可傳回字串泛型字典 (以整數編製索引) 的方法：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IDictionary<int, string> GetMapOfNames()
> {
>     Dictionary<int, string> retval = new Dictionary<int, string>();
>     retval.Add(1, "one");
>     retval.Add(2, "two");
>     retval.Add(3, "three");
>     retval.Add(42, "forty-two");
>     retval.Add(100, "one hundred");
>     return retval;
> }
> ```
> ```vb
> Public Shared Function GetMapOfNames() As IDictionary(Of Integer, String)
>     Dim retval As New Dictionary(Of Integer, String)
>     retval.Add(1, "one")
>     retval.Add(2, "two")
>     retval.Add(3, "three")
>     retval.Add(42, "forty-two")
>     retval.Add(100, "one hundred")
>     Return retval
> End Function
> ```

請注意，此字典必須當做由 [Dictionary&lt;TKey, TValue&gt;](/dotnet/api/system.collections.generic.dictionary-2) 實作且對應至 Windows 執行階段介面的介面傳回。 在這種情形下，此介面為 IDictionary&lt;int, string&gt; (在 Visual Basic 中為 IDictionary(Of Integer, String))。 將 Windows 執行階段類型 IMap&lt;int, string&gt; 傳遞至 Managed 程式碼時，它會顯示為 IDictionary&lt;int, string&gt;，反之，將 Managed 類型傳遞至 JavaScript 時亦然。

**重要**  當 managed 類型執行多個介面時，JavaScript 會使用清單中第一個出現的介面。 例如，若您將 Dictionary&lt;int, string&gt; 傳回 JavaScript 程式碼，則無論您將哪個介面指定為傳回類型，其皆會顯示為 IDictionary&lt;int, string&gt;。 這表示，如果第一個介面不包含出現在後續介面上的成員，該成員即不會對 JavaScript 顯示。

 

若要測試新方法並使用此字典，請將 returns1 和 returns2 函式加入至 default.js：

```javascript
var names;

function returns1() {
    names = SampleComponent.Example.getMapOfNames();
    document.getElementById('output').innerHTML = showMap(names);
}

var ct = 7;

function returns2() {
    if (!names.hasKey(17)) {
        names.insert(43, "forty-three");
        names.insert(17, "seventeen");
    }
    else {
        var err = names.insert("7", ct++);
        names.insert("forty", "forty");
    }
    document.getElementById('output').innerHTML = showMap(names);
}

function showMap(map) {
    var item = map.first();
    var retval = "<ul>";

    for (var i = 0, len = map.size; i < len; i++) {
        retval += "<li>" + item.current.key + ": " + item.current.value + "</li>";
        item.moveNext();
    }
    return retval + "</ul>";
}
```

將事件註冊程式碼加入至與另一個事件註冊程式碼相同的 then 區塊：

```javascript
var returnsButton1 = document.getElementById("returnsButton1");
returnsButton1.addEventListener("click", returns1, false);
var returnsButton2 = document.getElementById("returnsButton2");
returnsButton2.addEventListener("click", returns2, false);
```

您可以發現一些有關此 JavaScript 程式碼的有趣現象。 首先，它包含 showMap 函式，可利用 HTML 格式來顯示字典內容。 在 showMap 的程式碼中，請注意反覆運算模式。 在 .NET 中，泛型 IDictionary 介面上沒有第一個方法，而且大小是由 Count 屬性傳回，而不是 Size 方法。 在 JavaScript 中，IDictionary&lt;int, string&gt; 會顯示為 Windows 執行階段類型 IMap&lt;int, string&gt;。 (請參閱 [IMap&lt;K,V&gt;](/uwp/api/Windows.Foundation.Collections.IMap_K_V_) 介面)。

如先前範例所示，在 returns2 函式中，JavaScript 會呼叫 Insert 方法 (在 JavaScript 中為 insert)，將項目加入至字典。

若要執行應用程式，請選擇 F5 鍵。 若要建立和顯示字典的初始內容，請選擇 [Returns 1] 按鈕。 若要將其他兩個項目加入至字典，請選擇 [Returns 2] 按鈕。 請注意，項目會依插入的順序顯示，如同您對 Dictionary&lt;TKey, TValue&gt; 的預期一般。 若要將它們排序，您可以從 GetMapOfNames 傳回 SortedDictionary&lt;int, string&gt; (先前範例中使用的 PropertySet 類別與 Dictionary&lt;TKey, TValue&gt; 具有不同的內部組織)。

當然，JavaScript 不是強類型語言，所以使用強類型的泛型集合可能會導致一些意外的結果。 再次選擇 **\[Returns 2\]** 按鈕。 JavaScript 會將 "7" 強制轉型為數值 7，而將儲存在 ct 中的數值 7 強制轉型為字串。 而且，還會將字串 "forty" 強制轉型為零。 但是，這只是個開頭。 請多選擇幾次 [Returns 2] 按鈕。 在 Managed 程式碼中，即使值已轉型為正確的類型，Add 方法仍會產生重複索引鍵例外狀況。 相反地，Insert 方法會更新與現有索引鍵相關聯的值，並傳回 Boolean 值，表示是否已將新的索引鍵加入至字典。 這就是為何與索引鍵 7 相關聯的值會持續變更的緣故。

另一個非預期的行為：如果您將未指派的 JavaScript 變數當做字串引數傳遞，就會取得字串 "undefined"。 總之，當您將 .NET Framework 集合類型傳遞至 JavaScript 程式碼時，請格外小心。

> **請注意**  如果您有大量的文字要串連，您可以將程式碼移至 .NET Framework 方法並使用 StringBuilder 類別，以更有效率的方式執行，如 showMap 函數所示。

雖然您無法從 Windows 執行階段元件公開自己的泛型類型，但可使用下列程式碼，傳回 Windows 執行階段類別的 .NET Framework 泛型集合：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static object GetListOfThis(object obj)
> {
>     Type target = obj.GetType();
>     return Activator.CreateInstance(typeof(List<>).MakeGenericType(target));
> }
> ```
> ```vb
> Public Shared Function GetListOfThis(obj As Object) As Object
>     Dim target As Type = obj.GetType()
>     Return Activator.CreateInstance(GetType(List(Of )).MakeGenericType(target))
> End Function
> ```

List&lt;T&gt; 會實作 IList&lt;T&gt;，後者在 JavaScript 中會顯示為 Windows 執行階段類型 IVector&lt;T&gt;。

## <a name="declaring-events"></a>宣告事件


您可以使用標準的 .NET Framework 事件模式或 Windows 執行階段所使用的其他模式來宣告事件。 .NET Framework 支援將 System.EventHandler&lt;TEventArgs&gt; 委派和 Windows 執行階段 EventHandler&lt;T&gt; 委派視為相等，因此使用 EventHandler&lt;TEventArgs&gt; 是實作標準 .NET Framework 模式的好方法。 若要查看效果如何，可將下列類別組加入至 SampleComponent 專案：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> namespace SampleComponent
> {
>     public sealed class Eventful
>     {
>         public event EventHandler<TestEventArgs> Test;
>         public void OnTest(string msg, long number)
>         {
>             EventHandler<TestEventArgs> temp = Test;
>             if (temp != null)
>             {
>                 temp(this, new TestEventArgs()
>                 {
>                     Value1 = msg,
>                     Value2 = number
>                 });
>             }
>         }
>     }
>
>     public sealed class TestEventArgs
>     {
>         public string Value1 { get; set; }
>         public long Value2 { get; set; }
>     }
> }
> ```
> ```vb
> Public NotInheritable Class Eventful
>     Public Event Test As EventHandler(Of TestEventArgs)
>     Public Sub OnTest(ByVal msg As String, ByVal number As Long)
>         RaiseEvent Test(Me, New TestEventArgs() With {
>                             .Value1 = msg,
>                             .Value2 = number
>                             })
>     End Sub
> End Class
>
> Public NotInheritable Class TestEventArgs
>     Public Property Value1 As String
>     Public Property Value2 As Long
> End Class
> ```

當您在 Windows 執行階段中公開事件時，事件引數類別會繼承自 System.Object。 它不會繼承自 System.object，如同在 .NET 中一樣，因為 EventArgs 不是 Windows 執行階段類型。

如果您宣告事件的自訂事件存取子 (在 Visual Basic 中為 **Custom** 關鍵字)，則必須使用 Windows 執行階段事件模式。 請參閱[Windows 執行階段元件中的自訂事件和事件存取](custom-events-and-event-accessors-in-windows-runtime-components.md)子。

若要處理 Test 事件，請將 events1 函式加入至 default.js。 events1 函式會建立 Test 事件的事件處理常式函式，並立即叫用 OnTest 方法來引發此事件。 如果您在事件處理常式的內容中設定中斷點，就可以看見傳遞至單一參數的物件包含來源物件和 TestEventArgs 的兩個成員。

```javascript
var ev;

function events1() {
   ev = new SampleComponent.Eventful();
   ev.addEventListener("test", function (e) {
       document.getElementById('output').innerHTML = e.value1;
       document.getElementById('output').innerHTML += "<br/>" + e.value2;
   });
   ev.onTest("Number of feet in a mile:", 5280);
}
```

將事件註冊程式碼加入至與另一個事件註冊程式碼相同的 then 區塊：

```javascript
var events1Button = document.getElementById("events1Button");
events1Button.addEventListener("click", events1, false);
```

## <a name="exposing-asynchronous-operations"></a>公開非同步作業


根據 Task 和泛型 [Task&lt;TResult&gt;](/dotnet/api/system.threading.tasks.task-1) 類別，.NET Framework 有一組豐富的工具可用來進行非同步處理和平行處理。 若要在 Windows 執行階段元件中公開工作非同步處理，請使用 Windows 執行階段介面 [IAsyncAction](/windows/desktop/api/windows.foundation/nn-windows-foundation-iasyncaction)、[IAsyncActionWithProgress&lt;TProgress&gt;](/previous-versions/br205784(v=vs.85))、[IAsyncOperation&lt;TResult&gt;](/previous-versions/br205802(v=vs.85)) 及 [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/previous-versions/br205807(v=vs.85))。 (在 Windows 執行階段中，作業會傳回結果，但動作不會)。

本節示範可報告進度並傳回結果的可取消非同步作業。 GetPrimesInRangeAsync 方法會使用 [AsyncInfo](/dotnet/api/system.runtime.interopservices.windowsruntime) 類別來產生工作，並將它的取消與進度報告功能連接到 WinJS.Promise 物件。 現在將 GetPrimesInRangeAsync 方法加入至 Example 類別：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using System.Runtime.InteropServices.WindowsRuntime;
> using Windows.Foundation;
>
> public static IAsyncOperationWithProgress<IList<long>, double>
> GetPrimesInRangeAsync(long start, long count)
> {
>     if (start < 2 || count < 1) throw new ArgumentException();
>
>     return AsyncInfo.Run<IList<long>, double>((token, progress) =>
>
>         Task.Run<IList<long>>(() =>
>         {
>             List<long> primes = new List<long>();
>             double onePercent = count / 100;
>             long ctProgress = 0;
>             double nextProgress = onePercent;
>
>             for (long candidate = start; candidate < start + count; candidate++)
>             {
>                 ctProgress += 1;
>                 if (ctProgress >= nextProgress)
>                 {
>                     progress.Report(ctProgress / onePercent);
>                     nextProgress += onePercent;
>                 }
>                 bool isPrime = true;
>                 for (long i = 2, limit = (long)Math.Sqrt(candidate); i <= limit; i++)
>                 {
>                     if (candidate % i == 0)
>                     {
>                         isPrime = false;
>                         break;
>                     }
>                 }
>                 if (isPrime) primes.Add(candidate);
>
>                 token.ThrowIfCancellationRequested();
>             }
>             progress.Report(100.0);
>             return primes;
>         }, token)
>     );
> }
> ```
> ```vb
> Imports System.Runtime.InteropServices.WindowsRuntime
>
> Public Shared Function GetPrimesInRangeAsync(ByVal start As Long, ByVal count As Long)
> As IAsyncOperationWithProgress(Of IList(Of Long), Double)
>
>     If (start < 2 Or count < 1) Then Throw New ArgumentException()
>
>     Return AsyncInfo.Run(Of IList(Of Long), Double)( _
>         Function(token, prog)
>             Return Task.Run(Of IList(Of Long))( _
>                 Function()
>                     Dim primes As New List(Of Long)
>                     Dim onePercent As Long = count / 100
>                     Dim ctProgress As Long = 0
>                     Dim nextProgress As Long = onePercent
>
>                     For candidate As Long = start To start + count - 1
>                         ctProgress += 1
>
>                         If ctProgress >= nextProgress Then
>                             prog.Report(ctProgress / onePercent)
>                             nextProgress += onePercent
>                         End If
>
>                         Dim isPrime As Boolean = True
>                         For i As Long = 2 To CLng(Math.Sqrt(candidate))
>                             If (candidate Mod i) = 0 Then
>                                 isPrime = False
>                                 Exit For
>                             End If
>                         Next
>
>                         If isPrime Then primes.Add(candidate)
>
>                         token.ThrowIfCancellationRequested()
>                     Next
>                     prog.Report(100.0)
>                     Return primes
>                 End Function, token)
>         End Function)
> End Function
> ```

GetPrimesInRangeAsync 是一個非常簡單的質數搜尋工具，而這是設計的做法。 此處的重點在於實作非同步作業，因此簡單是很重要的，而且在示範取消時，能夠緩慢實作即為其優點。 GetPrimesInRangeAsync 會強行尋找質數：將候選項目除以小於或等於其平方根的所有整數，而不是只使用質數。 逐步執行此程式碼：

-   開始非同步作業之前，請先執行維護活動，例如，驗證參數以及對無效的輸入擲回例外狀況。
-   此實作的索引鍵是 [AsyncInfo.Run&lt;TResult, TProgress&gt;(Func&lt;CancellationToken, IProgress&lt;TProgress&gt;, Task&lt;TResult&gt;](/dotnet/api/system.runtime.interopservices.windowsruntime)&gt;) 方法，以及做為此方法唯一參數的委派。 此委派必須接受取消語彙基元和介面報告進度，而且必須傳回使用這些參數的已啟動工作。 當 JavaScript 呼叫 GetPrimesInRangeAsync 方法時，會發生下列步驟 (不一定是依照此處指定的順序)：

    -   [WinJS.Promise](/previous-versions/windows/apps/br211867(v=win.10)) 物件可提供函式來處理傳回結果、回應取消作業，以及處理進度報告。
    -   AsyncInfo.Run 方法會建立取消來源和可實作 IProgress&lt;T&gt; 介面的物件。 對此委派，它會從取消來源傳遞 [CancellationToken](/dotnet/api/system.threading.cancellationtoken) 語彙基元和 [IProgress&lt;T&gt;](/dotnet/api/system.iprogress-1) 介面。

        > **請注意**  如果承諾物件未提供函式來回應取消，請 System.runtime.interopservices.windowsruntime.asyncinfo。執行仍會傳遞可取消的標記，而且仍然可以進行取消。 如果 Promise 物件未提供函式來處理進度更新，AsyncInfo.Run 仍會提供物件來實作 IProgress&lt;T&gt;，但會忽略其報告。

    -   此委派會使用 [Task.Run&lt;TResult&gt;(Func&lt;TResult&gt;, CancellationToken](/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run__1_System_Func___0__System_Threading_CancellationToken_)) 方法來建立已啟動的工作，該工作會使用語彙基元和進度介面。 已啟動工作的委派是由可計算所需結果的 Lambda 函式所提供。 立即深入了解。
    -   AsyncInfo.Run 方法會建立物件來實作 [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) 介面、連接 Windows 執行階段取消機制與語彙基元來源，以及連接 Promise 物件的進度報告函式與 IProgress&lt;T&gt; 介面。
    -   IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 介面會傳回 JavaScript。

-   已啟動的工作所代表的 Lambda 函式不接受任何引數。 因為它是 Lambda 函式，所以可存取此語彙基元和 IProgress 介面。 每次評估候選數字時，Lambda 函式會執行下列動作：

    -   查看是否已達到進度的下一個百分點。 如果已達到，Lambda 函式就會呼叫 IProgress&lt;T&gt;.Report 方法，而此百分比會傳遞至 Promise 物件為了報告進度所指定的函式。
    -   如果作業已取消，則可使用取消語彙基元來擲回例外狀況。 如果已經呼叫 [IAsyncInfo.Cancel](/uwp/api/windows.foundation.iasyncinfo.cancel) 方法 (IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 會介面繼承此方法)，AsyncInfo.Run 方法設定的連接可確保已通知取消語彙基元。
-   當 Lambda 函式傳回質數清單時，此清單會傳遞至 WinJS.Promise 物件為了處理結果所指定的函式。

若要建立 JavaScript 承諾和設定取消機制，請將 asyncRun 和 asyncCancel 函式加入至 default.js。

```javascript
var resultAsync;
function asyncRun() {
    document.getElementById('output').innerHTML = "Retrieving prime numbers.";
    btnAsync.disabled = "disabled";
    btnCancel.disabled = "";

    resultAsync = SampleComponent.Example.getPrimesInRangeAsync(10000000000001, 2500).then(
        function (primes) {
            for (i = 0; i < primes.length; i++)
                document.getElementById('output').innerHTML += " " + primes[i];

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function () {
            document.getElementById('output').innerHTML += " -- getPrimesInRangeAsync was canceled. -- ";

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function (prog) {
            document.getElementById('primeProg').value = prog;
        }
    );
}

function asyncCancel() {    
    resultAsync.cancel();
}
```

請記住，事件註冊程式碼必須與您先前所使用的一樣。

```javascript
var btnAsync = document.getElementById("btnAsync");
btnAsync.addEventListener("click", asyncRun, false);
var btnCancel = document.getElementById("btnCancel");
btnCancel.addEventListener("click", asyncCancel, false);
```

呼叫非同步的 GetPrimesInRangeAsync 方法時，asyncRun 函式會建立 WinJS.Promise 物件。 此物件的 then 方法會採用三個函式，來處理傳回的結果、回應錯誤 (包含取消作業)，以及處理進度報告。 在這個範例中，傳回的結果會列印於輸出區域中。 取消或完成會重設可啟動和取消作業的按鈕。 進度報告會更新進度控制項。

asyncCancel 函式只會呼叫 WinJS.Promise 物件的 cancel 方法。

若要執行應用程式，請選擇 F5 鍵。 若要啟動非同步作業，請選擇 [非同步] 按鈕。 接下來發生的狀況取決於您的電腦速度有多快。 如果進度列在一瞬間就迅速完成，請以十或十的倍數增加起始數字大小，該數字會傳遞至 GetPrimesInRangeAsync。 您可以藉由增加或減少要測試的數字計數來微調作業的持續時間，但在起始數字中間加入零將造成較大的影響。 若要取消作業，請選擇 [取消非同步] 按鈕。

## <a name="related-topics"></a>相關主題

* [適用于 UWP 應用程式的 .NET 總覽](/previous-versions/windows/apps/br230302(v=vs.140))
* [適用于 UWP 應用程式的 .NET](/dotnet/api/index?view=dotnet-uwp-10.0)