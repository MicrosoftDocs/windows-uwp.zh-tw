---
title: Windows 執行階段元件與 C++/CX
description: 本主題說明如何使用 C++/CX 來建立 Windows 執行階段元件，此元件是可使用任何 Windows 執行階段語言所建立通用 Windows 應用程式呼叫的元件。
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9cda36c6027ae74df9beb5d1de68f69f273dc5f0
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860110"
---
# <a name="windows-runtime-components-with-ccx"></a>Windows 執行階段元件與 C++/CX

> [!NOTE]
> 本主題是為協助您維護您 C++/CX 應用程式。 但我們建議您將 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 用於新的應用程式。 C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。 若要了解如何使用 C++/WinRT 建立 Windows 執行階段元件，請參閱[使用 C++/WinRT 的 Windows 執行階段元件](./create-a-windows-runtime-component-in-cppwinrt.md)。

本主題說明如何使用 c + +/CX 建立元件 Windows 執行階段元件 &mdash; ，該元件可從使用任何 Windows 執行階段語言建立的通用 Windows 應用程式， (c #、Visual Basic、c + + 或 JAVAscript) 。

在 c + + 中建立 Windows 執行階段元件有幾個原因。
- 獲得 C++ 在複雜或密集運算作業中的效能優勢。
- 重複使用已經撰寫好且經過測試的程式碼。

當您建置包含 JavaScript 或 .NET 專案以及 Windows 執行階段元件專案的方案時，JavaScript 專案檔與已編譯的 DLL 會合併成單一封裝，如此您就能在模擬器進行本機偵錯，或在行動網卡上進行遠端偵錯。 您也可以利用擴充功能 SDK 格式，僅發佈元件專案。 如需詳細資訊，請參閱[建立軟體開發套件](/visualstudio/extensibility/creating-a-software-development-kit)。

一般情況下，當您撰寫 c + +/CX 元件的程式碼時，請使用一般 c + + 程式庫和內建類型，但在抽象二進位介面 (ABI) 界限，而您會在另一個 winmd 封裝的程式碼中傳遞資料。 在該處，請使用 Windows 執行階段類型，以及 c + +/CX 針對建立和操作這些類型所支援的特殊語法。 此外，在您的 c + +/CX 程式碼中，您可以使用委派和事件之類的型別，來執行可從元件引發並以 JavaScript、Visual Basic、c + + 或 c # 處理的事件。 如需 c + +/CX 語法的詳細資訊，請參閱 [Visual C++ 語言參考 (c + +/cx) ](/cpp/cppcx/visual-c-language-reference-c-cx)。

## <a name="casing-and-naming-rules"></a>大小寫和命名規則

### <a name="javascript"></a>JavaScript
JavaScript 會區分大小寫。 因此，您必須遵循下列大小寫慣例：

-   當您參考 C++ 命名空間和類別時，請使用 C++ 中使用的相同大小寫。
-   當您呼叫方法時，即使方法名稱的第一個字母在 C++ 中為大寫，也請使用 Camel 命名法的大小寫慣例。 例如，從 JavaScript 呼叫 C++ 方法 GetDate() 時，必須改為呼叫 getDate()。
-   可啟用的類別名稱和命名空間名稱不能包含 UNICODE 字元。

### <a name="net"></a>.NET
.NET 語言都會遵循它們的一般大小寫規則。

## <a name="instantiating-the-object"></a>具現化物件
只有 Windows 執行階段類型可以跨 ABI 界限傳遞。 如果元件有 std::wstring 這類的類型做為公用方法中的傳回類型或參數，則編譯器將會發出錯誤。 Visual C++ 元件延伸模組 (C++/CX) 內建類型包括一般的純量類型 (例如 int 和 double)，以及它們的 typedef 對等項目 (int32 和 float64 等等)。 如需詳細資訊，請參閱[類型系統 (C++/CX)](/cpp/cppcx/type-system-c-cx)。

```cpp
// ref class definition in C++
public ref class SampleRefClass sealed
{
    // Class members...

    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }

};
```

```javascript
//Instantiation in JavaScript (requires "Add reference > Project reference")
var nativeObject = new CppComponent.SampleRefClass();
```

```csharp
//Call a method and display result in a XAML TextBlock
var num = nativeObject.LogCalc(21.5);
ResultText.Text = num.ToString();
```

## <a name="ccx-built-in-types-library-types-and-windows-runtime-types"></a>C + +/CX 內建類型、程式庫類型和 Windows 執行階段類型
可啟用的類別 (也稱為 ref 類別) 是可從其他語言 (例如 JavaScript、C# 或 Visual Basic) 中具現化的類別。 元件至少必須包含一個可啟用的類別，才可從其他語言取用。

Windows 執行階段元件可以包含多個公用的可啟用類別，以及其他只有在元件內部才認得的類別。 將 [WebHostHidden](/uwp/api/windows.foundation.metadata.webhosthiddenattribute) 屬性套用至不適合 JavaScript 可見的 c + +/cx 類型。

所有的公用類別都必須位於相同的根命名空間 (與元件中繼資料檔案同名) 中。 例如，名為 A.B.C.MyClass 的類別必須在名為 A.winmd、A.B.winmd 或 A.B.C.winmd 的中繼資料檔案中定義，才能執行個體化。 DLL 的名稱不需符合 .winmd 檔案名稱。

用戶端程式碼可以使用 **new** (在 Visual Basic 中為 **New**) 關鍵字，來建立元件的執行個體 (就像對於任何類別一樣)。

可啟用的類別必須宣告為 **public ref class sealed**。 **ref class** 關鍵字會指示編譯器，將類別建立為可與 Windows 執行階段相容的類型，而 sealed 關鍵字則會指定此類別無法被繼承。 Windows 執行階段目前不支援一般化繼承模型，因為受限制的繼承模型支援建立自訂的 XAML 控制項。 如需詳細資訊，請參閱 [Ref 類別與結構 (C++/CX)](/cpp/cppcx/ref-classes-and-structs-c-cx)。

若是 c + +/CX，所有的數值基本類型都會定義在預設命名空間中。 [Platform](/cpp/cppcx/platform-namespace-c-cx)命名空間包含 Windows 執行階段類型系統特有的 c + +/cx 類別。 這些包括 [Platform::String](/cpp/cppcx/platform-string-class) 類別和 [Platform::Object](/cpp/cppcx/platform-object-class) 類別。 這類具象集合類型 (例如 [Platform::Collections::Map](/cpp/cppcx/platform-collections-map-class) 類別和 [Platform::Collections::Vector](/cpp/cppcx/platform-collections-vector-class) 類別) 定義於 [Platform::Collections](/cpp/cppcx/platform-collections-namespace) 命名空間中。 這些類型實作的公用介面定義於 [Windows::Foundation::Collections 命名空間 (C++/CX)](/cpp/cppcx/windows-foundation-collections-namespace-c-cx) 中。 它是 JavaScript、C# 和 Visual Basic 所取用的這些介面類型。 如需詳細資訊，請參閱[類型系統 (C++/CX)](/cpp/cppcx/type-system-c-cx)。

## <a name="method-that-returns-a-value-of-built-in-type"></a>傳回內建類型之值的方法
```cpp
    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }
```

```javascript
//Call a method
var nativeObject = new CppComponent.SampleRefClass;
var num = nativeObject.logCalc(21.5);
document.getElementById('P2').innerHTML = num;
```

## <a name="method-that-returns-a-custom-value-struct"></a>傳回自訂值結構的方法
```cpp
namespace CppComponent
{
    // Custom struct
    public value struct PlayerData
    {
        Platform::String^ Name;
        int Number;
        double ScoringAverage;
    };

    public ref class Player sealed
    {
    private:
        PlayerData m_player;
    public:
        property PlayerData PlayerStats
        {
            PlayerData get(){ return m_player; }
            void set(PlayerData data) {m_player = data;}
        }
    };
}
```

若要在 ABI 之間傳遞使用者定義值結構，請定義一個 JavaScript 物件，該物件的成員與 c + +/CX。中定義的值結構具有相同的成員。 然後，您可以將該物件做為引數傳遞至 c + +/CX 方法，以便將物件隱含轉換成 c + +/CX 型別。

```javascript
// Get and set the value struct
function GetAndSetPlayerData() {
    // Create an object to pass to C++
    var myData =
        { name: "Bob Homer", number: 12, scoringAverage: .357 };
    var nativeObject = new CppComponent.Player();
    nativeObject.playerStats = myData;

    // Retrieve C++ value struct into new JavaScript object
    var myData2 = nativeObject.playerStats;
    document.getElementById('P3').innerHTML = myData.name + " , " + myData.number + " , " + myData.scoringAverage.toPrecision(3);
}
```

另一種方式是定義會實作 IPropertySet (未顯示) 的類別。

在 .NET 語言中，您只需建立在 c + +/CX 元件中定義之類型的變數。

```csharp
private void GetAndSetPlayerData()
{
    // Create a ref class
    var player = new CppComponent.Player();

    // Create a variable of a value struct
    // type that is defined in C++
    CppComponent.PlayerData myPlayer;
    myPlayer.Name = "Babe Ruth";
    myPlayer.Number = 12;
    myPlayer.ScoringAverage = .398;

    // Set the property
    player.PlayerStats = myPlayer;

    // Get the property and store it in a new variable
    CppComponent.PlayerData myPlayer2 = player.PlayerStats;
    ResultText.Text += myPlayer.Name + " , " + myPlayer.Number.ToString() +
        " , " + myPlayer.ScoringAverage.ToString();
}
```

## <a name="overloaded-methods"></a>多載方法
C + +/CX 公用 ref 類別可包含多載方法，但 JavaScript 的功能有限，無法區分多載的方法。 例如，它可以區別下列簽章間的差異：

```cpp
public ref class NumberClass sealed
{
public:
    int GetNumber(int i);
    int GetNumber(int i, Platform::String^ str);
    double GetNumber(int i, MyData^ d);
};
```

但它無法分辨兩者之間的差異：

```cpp
int GetNumber(int i);
double GetNumber(double d);
```

為了防止發生無法區別的情形，您可以將 [Windows::Foundation::Metadata::DefaultOverload](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) 屬性套用至標頭檔中的方法簽章，確保 JavaScript 一律會呼叫特定的多載。

下列 JavaScript 一律會呼叫屬性化多載：

```javascript
var nativeObject = new CppComponent.NumberClass();
var num = nativeObject.getNumber(9);
document.getElementById('P4').innerHTML = num;
```

## <a name="net"></a>.NET
.NET 語言會辨識 c + +/CX ref 類別中的多載，就像在任何 .NET 類別一樣。

## <a name="datetime"></a>Datetime
在 Windows 執行階段中，[Windows::Foundation::DateTime](/uwp/api/windows.foundation.datetime) 物件只是一個 64 位元帶正負號的整數，它代表 1601 年 1 月 1 日之前或之後的 100 奈秒間隔數。 Windows:Foundation::DateTime 物件上沒有任何方法。 相反地，每種語言都會以該語言的原生方式來投射日期時間： JavaScript 中的日期物件和 .NET 中的 system.string 和 DateTimeOffset 類型。

```cpp
public  ref class MyDateClass sealed
{
public:
    property Windows::Foundation::DateTime TimeStamp;
    void SetTime(Windows::Foundation::DateTime dt)
    {
        auto cal = ref new Windows::Globalization::Calendar();
        cal->SetDateTime(dt);
        TimeStamp = cal->GetDateTime(); // or TimeStamp = dt;
    }
};
```

當您將 DateTime 值從 c + +/CX 傳遞至 JavaScript 時，JavaScript 會將它接受為 Date 物件，並預設顯示為完整格式的日期字串。

```javascript
function SetAndGetDate() {
    var nativeObject = new CppComponent.MyDateClass();

    var myDate = new Date(1956, 4, 21);
    nativeObject.setTime(myDate);

    var myDate2 = nativeObject.timeStamp;

    //prints long form date string
    document.getElementById('P5').innerHTML = myDate2;

}
```

當 .NET 語言將 system.string 傳遞給 c + +/CX 元件時，方法會將它接受為 Windows：： Foundation：:D ateTime。 當元件傳遞 Windows：： Foundation：:D ateTime 至 .NET 方法時，Framework 方法會將它當作 DateTimeOffset 接受。

```csharp
private void DateTimeExample()
{
    // Pass a System.DateTime to a C++ method
    // that takes a Windows::Foundation::DateTime
    DateTime dt = DateTime.Now;
    var nativeObject = new CppComponent.MyDateClass();
    nativeObject.SetTime(dt);

    // Retrieve a Windows::Foundation::DateTime as a
    // System.DateTimeOffset
    DateTimeOffset myDate = nativeObject.TimeStamp;

    // Print the long-form date string
    ResultText.Text += myDate.ToString();
}
```

## <a name="collections-and-arrays"></a>集合和陣列
集合跨 ABI 界限傳遞時一律是採 Windows 執行階段類型之控制代碼的形式，例如 Windows::Foundation::Collections::IVector^ 與 Windows::Foundation::Collections::IMap^。 例如，如果您將控制代碼傳回 Platform::Collections::Map，它會隱含地轉換成 Windows::Foundation::Collections::IMap^。 集合介面是在命名空間中定義的，該命名空間與提供具體實作為的 c + +/CX 類別不同。 JavaScript 和 .NET 語言都會使用這些介面。 如需詳細資訊，請參閱[集合 (C++/CX)](/cpp/cppcx/collections-c-cx) 和 [Array 和 WriteOnlyArray (C++/CX)](/cpp/cppcx/array-and-writeonlyarray-c-cx)。

## <a name="passing-ivector"></a>傳遞 IVector
```cpp
// Windows::Foundation::Collections::IVector across the ABI.
//#include <algorithm>
//#include <collection.h>
Windows::Foundation::Collections::IVector<int>^ SortVector(Windows::Foundation::Collections::IVector<int>^ vec)
{
    std::sort(begin(vec), end(vec));
    return vec;
}
```

```javascript
var nativeObject = new CppComponent.CollectionExample();
// Call the method to sort an integer array
var inVector = [14, 12, 45, 89, 23];
var outVector = nativeObject.sortVector(inVector);
var result = "Sorted vector to array:";
for (var i = 0; i < outVector.length; i++)
{
    outVector[i];
    result += outVector[i].toString() + ",";
}
document.getElementById('P6').innerHTML = result;
```

.NET 語言會將 IVector&lt;T&gt; 當做 IList&lt;T&gt;。

```csharp
private void SortListItems()
{
    IList<int> myList = new List<int>();
    myList.Add(5);
    myList.Add(9);
    myList.Add(17);
    myList.Add(2);

    var nativeObject = new CppComponent.CollectionExample();
    IList<int> mySortedList = nativeObject.SortVector(myList);

    foreach (var item in mySortedList)
    {
        ResultText.Text += " " + item.ToString();
    }
}
```

## <a name="passing-imap"></a>傳遞 IMAP
```cpp
// #include <map>
//#include <collection.h>
Windows::Foundation::Collections::IMap<int, Platform::String^> ^GetMap(void)
{    
    Windows::Foundation::Collections::IMap<int, Platform::String^> ^ret =
        ref new Platform::Collections::Map<int, Platform::String^>;
    ret->Insert(1, "One ");
    ret->Insert(2, "Two ");
    ret->Insert(3, "Three ");
    ret->Insert(4, "Four ");
    ret->Insert(5, "Five ");
    return ret;
}
```

```javascript
// Call the method to get the map
var outputMap = nativeObject.getMap();
var mStr = "Map result:" + outputMap.lookup(1) + outputMap.lookup(2)
    + outputMap.lookup(3) + outputMap.lookup(4) + outputMap.lookup(5);
document.getElementById('P7').innerHTML = mStr;
```

.NET 語言會將 IMap 當做 IDictionary&lt;K, V&gt;。

```csharp
private void GetDictionary()
{
    var nativeObject = new CppComponent.CollectionExample();
    IDictionary<int, string> d = nativeObject.GetMap();
    ResultText.Text += d[2].ToString();
}
```

## <a name="properties"></a>屬性
C + +/CX 元件延伸中的公用 ref 類別會使用 property 關鍵字，將公用資料成員公開為屬性。 概念與 .NET 屬性相同。 由於 trivial 屬性的功能都是隱含的，因此與資料成員類似。 非 trivial 屬性具有明確的 get 和 set 存取子，以及一個可做為值之「備份存放區」的具名私用變數。 在此範例中，私用成員變數 \_ propertyAValue 是 PropertyA 的備份存放區。 屬性可以在其值變更時引發事件，而用戶端 app 可註冊來接收該事件。

```cpp
//Properties
public delegate void PropertyChangedHandler(Platform::Object^ sender, int arg);
public ref class PropertyExample  sealed
{
public:
    PropertyExample(){}

    // Event that is fired when PropertyA changes
    event PropertyChangedHandler^ PropertyChangedEvent;

    // Property that has custom setter/getter
    property int PropertyA
    {
        int get() { return m_propertyAValue; }
        void set(int propertyAValue)
        {
            if (propertyAValue != m_propertyAValue)
            {
                m_propertyAValue = propertyAValue;
                // Fire event. (See event example below.)
                PropertyChangedEvent(this, propertyAValue);
            }
        }
    }

    // Trivial get/set property that has a compiler-generated backing store.
    property Platform::String^ PropertyB;

private:
    // Backing store for propertyA.
    int m_propertyAValue;
};
```

```javascript
var nativeObject = new CppComponent.PropertyExample();
var propValue = nativeObject.propertyA;
document.getElementById('P8').innerHTML = propValue;

//Set the string property
nativeObject.propertyB = "What is the meaning of the universe?";
document.getElementById('P9').innerHTML += nativeObject.propertyB;
```

.NET 語言會存取原生 c + +/CX 物件上的屬性，就如同在 .NET 物件上一樣。

```csharp
private void GetAProperty()
{
    // Get the value of the integer property
    // Instantiate the C++ object
    var obj = new CppComponent.PropertyExample();

    // Get an integer property
    var propValue = obj.PropertyA;
    ResultText.Text += propValue.ToString();

    // Set a string property
    obj.PropertyB = " What is the meaning of the universe?";
    ResultText.Text += obj.PropertyB;

}
```

## <a name="delegates-and-events"></a>委派和事件
委派是一種代表函式物件的 Windows 執行階段類型。 您可以將委派連同事件、回呼和非同步方法呼叫一起使用，以指定後續要執行的動作。 如同函式物件，委派也可讓編譯器驗證函式的傳回類型和參數類型，以提供類型安全。 委派的宣告類似於函式簽章、實作類似於類別定義，而叫用則類似於函式叫用。

## <a name="adding-an-event-listener"></a>加入事件接聽程式
您可以使用 event 關鍵字，來宣告所指定委派類型的公用成員。 用戶端程式碼可使用特定語言中提供的標準機制來訂閱事件。

```cpp
public:
    event SomeHandler^ someEvent;
```

這個範例所使用的 C++ 程式碼與前一個屬性區段中的相同。

```javascript
function Button_Click() {
    var nativeObj = new CppComponent.PropertyExample();
    // Define an event handler method
    var singlecasthandler = function (ev) {
        document.getElementById('P10').innerHTML = "The button was clicked and the value is " + ev;
    };

    // Subscribe to the event
    nativeObj.onpropertychangedevent = singlecasthandler;

    // Set the value of the property and fire the event
    var propValue = 21;
    nativeObj.propertyA = 2 * propValue;

}
```

在 .NET 語言中，訂閱 c + + 元件中的事件與訂閱 .NET 類別中的事件相同：

```csharp
//Subscribe to event and call method that causes it to be fired.
private void TestMethod()
{
    var objWithEvent = new CppComponent.PropertyExample();
    objWithEvent.PropertyChangedEvent += objWithEvent_PropertyChangedEvent;

    objWithEvent.PropertyA = 42;
}

//Event handler method
private void objWithEvent_PropertyChangedEvent(object __param0, int __param1)
{
    ResultText.Text = "the event was fired and the result is " +
         __param1.ToString();
}
```

## <a name="adding-multiple-event-listeners-for-one-event"></a>為一個事件加入多個事件接聽程式
JavaScript 的 addEventListener 方法可讓多個處理常式訂閱單一事件。

```cpp
public delegate void SomeHandler(Platform::String^ str);

public ref class LangSample sealed
{
public:
    event SomeHandler^ someEvent;
    property Platform::String^ PropertyA;

    // Method that fires an event
    void FireEvent(Platform::String^ str)
    {
        someEvent(Platform::String::Concat(str, PropertyA->ToString()));
    }
    //...
};
```

```javascript
// Add two event handlers
var multicast1 = function (ev) {
    document.getElementById('P11').innerHTML = "Handler 1: " + ev.target;
};
var multicast2 = function (ev) {
    document.getElementById('P12').innerHTML = "Handler 2: " + ev.target;
};

var nativeObject = new CppComponent.LangSample();
//Subscribe to the same event
nativeObject.addEventListener("someevent", multicast1);
nativeObject.addEventListener("someevent", multicast2);

nativeObject.propertyA = "42";

// This method should fire an event
nativeObject.fireEvent("The answer is ");
```

如上述範例所示，在 C# 中，無論事件處理常式的數目為何，都可以使用 += 運算子來訂閱事件。

## <a name="enums"></a>列舉
C + +/CX 中的 Windows 執行階段列舉是使用公用類別列舉來宣告;它類似于標準 c + + 中的範圍列舉。

```cpp
public enum class Direction {North, South, East, West};

public ref class EnumExampleClass sealed
{
public:
    property Direction CurrentDirection
    {
        Direction  get(){return m_direction; }
    }

private:
    Direction m_direction;
};
```

列舉值會在 c + +/CX 和 JavaScript 之間傳遞為整數。 您可以選擇性地宣告 JavaScript 物件，其中包含與 c + +/CX 列舉相同的已命名值，並使用它，如下所示。

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

C# 和 Visual Basic 語言都支援列舉。 這些語言會看到 c + + 公用列舉類別，就像看到 .NET 列舉一樣。

## <a name="asynchronous-methods"></a>非同步方法
若要使用其他 Windows 執行階段物件所公開的非同步方法，請使用 [task 類別 (並行執行階段)](/cpp/parallel/concrt/reference/task-class)。 如需詳細資訊，請參閱[工作平行處理原則 (並行執行階段)](/cpp/parallel/concrt/task-parallelism-concurrency-runtime)。

若要在 c + +/CX 中執行非同步方法，請使用 ppltasks.h 中所定義的 [create \_ async](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017&preserve-view=true) 函數。 如需詳細資訊，請參閱 [在 c + +/cx 中為 UWP 應用程式建立異步操作](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)。 如需範例，請參閱 [建立 c + +/cx Windows 執行階段元件，以及從 JavaScript 或 c # 呼叫它的逐步](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)解說。 .NET 語言會使用 c + +/CX 非同步方法，就像在 .NET 中定義的任何非同步方法一樣。

## <a name="exceptions"></a>例外狀況
您可以擲回 Windows 執行階段所定義的任何例外狀況類型。 您無法從任何 Windows 執行階段例外狀況類型衍生自訂類型。 不過，您可以擲回 COMException，並提供自訂的 HRESULT，以供攔截到例外狀況的程式碼存取。 您無法在 COMException 中指定自訂訊息。

## <a name="debugging-tips"></a>偵錯秘訣
對含有元件 DLL 的 JavaScript 方案進行偵錯時，您可以設定偵錯工具以啟用逐步執行指令碼或逐步執行元件中的原生程式碼，但不可兩者同時啟用。 若要變更設定，請在 [方案總管] 中選取 JavaScript 專案節點，然後依序選擇 [屬性]、[偵錯]、 [偵錯工具類型]。

請務必在封裝設計工具中選取適當的功能。 例如，如果您嘗試使用 Windows 執行階段 API 來開啟使用者圖片庫中的影像檔，請務必選取資訊清單設計工具的 [功能] 窗格中的 [圖片庫] 核取方塊。

如果 JavaScript 程式碼似乎無法辨識元件中的公用屬性或方法，請確定您在 JavaScript 中使用的是 Camel 命名法的大小寫慣例。 例如，在 JavaScript 中，LogCalc c + +/CX 方法必須以 logCalc 的形式參考。

如果您從方案中移除 c + +/CX Windows 執行階段元件專案，也必須手動從 JavaScript 專案中移除專案參考。 若未執行此動作，後續的偵錯或建置作業將無法執行。 之後如有必要，您可以加入 DLL 的組件參考。

## <a name="related-topics"></a>相關主題
* [建立 C++/CX Windows 執行階段元件，並從 JavaScript 或 C# 呼叫該元件的逐步解說](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)
