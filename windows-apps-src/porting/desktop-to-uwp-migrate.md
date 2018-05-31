---
author: normesta
Description: Share code between a desktop app and a UWP app
Search.Product: eADQiWindows 10XVcnh
title: 在傳統型應用程式與 UWP app 之間共用程式碼
ms.author: normesta
ms.date: 10/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ec676446562d5d97ff2a7020fc494f248323450b
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
ms.locfileid: "1691937"
---
# <a name="share-code-between-a-desktop-app-and-a-uwp-app"></a>在傳統型應用程式與 UWP app 之間共用程式碼

您可以將您的程式碼移入 .NET Standard 程式庫中，然後建立通用 Windows 平台 (UWP) app，將適用範圍擴及所有 Windows 10 裝置。 沒有工具可將傳統型應用程式轉換為 UWP app 時，您可以重複使用許多現有的程式碼，而這樣還會降低建置應用程式的成本。 本指南會示範做法。

## <a name="share-code-in-a-net-standard-20-library"></a>共用 .NET Standard 2.0 程式庫中的程式碼

盡可能多地將程式碼放入 .NET Standard 2.0 類別庫中。  只要您的程式碼使用 Standard 中定義的 API，就可以在 UWP app 中重複使用該程式碼。 這比以往任何時候都還要容易共用 .NET Standard 程式庫的程式碼，因為 .NET Standard 2.0 中包含更加多的 API 了。

以下是一部告訴您更多相關資訊的不錯影片。
<br><br>
<iframe src="https://www.youtube.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

### <a name="add-net-standard-libraries"></a>新增 .NET Standard 程式庫

首先將一個或多個 .NET Standard 類別庫新增至您的方案。  

![新增 DotNet 標準專案](images/desktop-to-uwp/dot-net-standard-project-template.png)

您新增至方案的程式庫數目取決於您想要如何組合管理程式碼。

請確定每個類別庫都是以 **.NET Standard 2.0** 為目標。

![將目標設為 .NET Standard 2.0](images/desktop-to-uwp/target-standard-20.png)

您可以在類別庫專案的屬性頁中找到此設定。

在傳統型應用程式專案中，新增類別庫專案的參考。

![類別庫參考](images/desktop-to-uwp/class-library-reference.png)

接下來，使用工具來判斷您的程式碼有多少符合標準。 如此一來，將程式碼移入程式庫之前就可以決定您可重複使用哪些部分、哪些部分需要進行最基本的修改，以及哪些部分要保留給特定應用程式專用。

### <a name="check-library-and-code-compatibility"></a>檢查程式庫與程式碼的相容性

我們會從 Nuget 套件及其他從協力廠商取得的 dll 檔案開始著手。

如果您的應用程式使用這其中任何一個，請判斷是否與 .NET Standard 2.0 相容。 若要這樣做，您可以使用 Visual Studio 擴充功能或命令列公用程式。

您也可以使用這些工具來分析程式碼。 從這裡下載工具 ([dotnet-apiport](https://github.com/Microsoft/dotnet-apiport/releases))，然後觀看此影片以了解如何使用這些工具。
<br><br>
<iframe src="https://www.youtube.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

 如果您的程式碼與標準不相容，請考慮其他可以實作該程式碼的方式。 一開始先開啟 [.NET API 瀏覽器](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)。 您可以使用該瀏覽器來檢閱 .NET Standard 2.0 中提供的 API。 請務必將清單範圍限定為 .NET Standard 2.0。

![Dot Net 選項](images/desktop-to-uwp/dot-net-option.png)

您部分的程式碼專屬於特定平台，必須繼續留在傳統型應用程式專案中。

### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>範例：將資料存取程式碼移轉至 .NET Standard 2.0 程式庫

假設說，我們有非常基本的 Windows Forms 應用程式，顯示 Northwind 範例資料庫中的客戶。

![Windows Forms 應用程式](images/desktop-to-uwp/win-forms-app.png)

專案包含內有名為 **Northwind** 靜態類別的 .NET Standard 2.0 類別庫。 如果我們將此程式碼移入 **Northwind** 類別，程式碼將無法編譯，因為其中使用 ``SQLConnection``、``SqlCommand`` 和 ``SqlDataReader`` 類別，以及那些未在 .NET Standard 2.0 中提供的類別。

```csharp
public static ArrayList GetCustomerNames()
{
    ArrayList customers = new ArrayList();

    using (SqlConnection conn = new SqlConnection())
    {
        conn.ConnectionString =
            @"Data Source=" +
            @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        SqlCommand command = new SqlCommand("select ContactName from customers order by ContactName asc", conn);

        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}

```
不過，[.NET API 瀏覽器](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)告訴了我們替代方式。 我們可以使用 ``DbConnection``、``DbCommand`` 和 ``DbDataReader`` 類別，因為 .NET Standard 2.0 有提供這些類別。  

此修訂版本使用這些類別來取得客戶清單，但要建立 ``DbConnection`` 類別，就必須傳入我們在用戶端應用程式中建立的 Factory 物件。

```csharp
public static ArrayList GetCustomerNames(DbProviderFactory factory)
{
    ArrayList customers = new ArrayList();

    using (DbConnection conn = factory.CreateConnection())
    {
        conn.ConnectionString = @"Data Source=" +
                        @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        DbCommand command = factory.CreateCommand();
        command.Connection = conn;
        command.CommandText = "select ContactName from customers order by ContactName asc";

        using (DbDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}
```  

在 Windows Form 的程式碼後置頁面上，可以只是建立 Factory 執行個體後，再傳入我們的方法中。

```csharp
public partial class Customers : Form
{
    public Customers()
    {
        InitializeComponent();

        dataGridView1.Rows.Clear();

        SqlClientFactory factory = SqlClientFactory.Instance;

        foreach (string customer in Northwind.GetCustomerNames(factory))
        {
            dataGridView1.Rows.Add(customer);
        }
    }
}
```

## <a name="reach-all-windows-devices"></a>將目標範圍擴及所有 Windows 裝置

現在就可以開始將 UWP app 新增至您的方案。
<div style="float: left; padding: 10px">
    ![傳統型轉 UWP 橋接器影像](images/desktop-to-uwp/adaptive-ui.png)
</div>
您仍然需要使用 XAML 設計 UI 頁面，並撰寫任何裝置或平台特定程式碼。不過，一旦完成，您就可以將適用範圍涵蓋全系列的 Windows 10 裝置，而您的應用程式頁面也將具備可配合不同螢幕大小及解析度適當調整的現代化觀感。

應用程式除了鍵盤和滑鼠之外，還能回應其他輸入機制，而且功能和設定在所有裝置上都會變得直覺。 這表示使用者了解過一次操作方法後，不論什麼裝置，使用起來都會非常熟悉。

以上所述只是 UWP 附帶的其中幾個好處。 若要深入了解，請參閱[建置美好的 Windows 使用體驗](https://developer.microsoft.com/windows/why-build-for-uwp)。

### <a name="add-a-uwp-project"></a>新增 UWP 專案

首先，將 UWP 專案新增至您的方案。

![UWP 專案](images/desktop-to-uwp/new-uwp-app.png)

然後，從 UWP 專案新增 .NET Standard 2.0 程式庫專案的參考。

![類別庫參考](images/desktop-to-uwp/class-library-reference2.png)

### <a name="build-your-pages"></a>建置您的頁面

新增 XAML 頁面，並呼叫在 .NET Standard 2.0 程式庫中的程式碼。

![UWP app](images/desktop-to-uwp/uwp-app.png)

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel x:Name="customerStackPanel">
        <ListView x:Name="customerList"/>
    </StackPanel>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        SqlClientFactory factory = SqlClientFactory.Instance;

        customerList.ItemsSource = Northwind.GetCustomerNames(factory);
    }
}
```


若要開始使用 UWP，請參閱[什麼是 UWP app](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)。

## <a name="reach-ios-and-android-devices"></a>將目標範本擴及 iOS 和 Android 裝置

您可以透過新增 Xamarin 專案，將目標範本擴及 Android 和 iOS 裝置  
<div style="float: left; padding: 10px">
    ![Xamarin 應用程式](images/desktop-to-uwp/xamarin-apps.png)
</div>

這些專案允許您使用 C# 來建置可以完整存取平台特定及裝置特定 API 的 Android 和 iOS 應用程式。 這些應用程式充分利用平台特定的硬體加速，並且針對原生效能進行編譯。

這些應用程式可以存取基礎平台及裝置公開的各種功能 (包括 iBeacons 和 Android Fragments 等平台特定功能)，而您將會使用標準原生使用者介面控制項來建置使用者所預期之外觀與風格的 UI。

就像 UWP 一樣，新增 Android 或 iOS 應用程式的成本較低，因為您可以重複使用 .NET Standard 2.0 類別庫中的商務邏輯。 您必須使用 XAML 設計 UI 頁面，並撰寫任何裝置或平台特定的程式碼。

### <a name="add-a-xamarin-project"></a>新增 Xamarin 專案

首先，將 **\[Android\]**、**\[iOS\]** 或 **\[跨平台\]** 專案新增至您的方案。

您可以在 **\[加入新的專案\]** 對話方塊的 **\[Visual C#\]** 群組底下找到這些範本。

![Xamarin 應用程式](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>跨平台專案非常適合只有極少平台特定功能的應用程式。 您可以使用這些專案建立一個在 Android、iOS 及 Windows 上執行的原生 XAML 型 UI。 請按一下[這裡](https://www.xamarin.com/forms)進一步了解。

接著，從 Android、iOS 或跨平台專案中新增類別庫專案的參考。

![類別庫參考](images/desktop-to-uwp/class-library-reference3.png)

### <a name="build-your-pages"></a>建置您的頁面

我們的範例會在 Android 應用程式中顯示客戶清單。

![Android 應用程式](images/desktop-to-uwp/android-app.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp"
    android:id="@android:id/list">
</TextView>
```

```csharp
[Activity(Label = "MyAndroidApp", MainLauncher = true)]
public class MainActivity : ListActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SqlClientFactory factory = SqlClientFactory.Instance;

        var customers = (string[])Northwind.GetCustomerNames(factory).ToArray(typeof(string));

        ListAdapter = new ArrayAdapter<string>(this, Resource.Layout.list_item, customers);
    }
}
```

若要開始使用 Android、iOS 和跨平台專案，請參閱 [Xamarin 開發人員入口網站](https://developer.xamarin.com/)。

## <a name="next-steps"></a>後續步驟

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。