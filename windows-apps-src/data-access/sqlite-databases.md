---
title: 在 UWP 應用程式中使用 SQLite 資料庫
description: 了解如何在 UWP 應用程式中使用 SQLite 資料庫，以在使用者裝置上的輕量資料庫中儲存和擷取資料。
ms.date: 06/26/2020
ms.topic: article
keywords: Windows 10, uwp, SQLite, 資料庫
ms.localizationpriority: medium
ms.openlocfilehash: 045f327622c93d7d4c1653492e54b7939ee919a2
ms.sourcegitcommit: d0eef123b167dc63f482a9f4432a237c1c6212db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077246"
---
# <a name="use-a-sqlite-database-in-a-uwp-app"></a>在 UWP 應用程式中使用 SQLite 資料庫
您可以使用 SQLite 在使用者的裝置上儲存和擷取輕量資料庫中的資料。 本指南會示範怎麼做。

## <a name="some-benefits-of-using-sqlite-for-local-storage"></a>使用 SQLite 進行本機儲存的一些優點

:heavy_check_mark:SQLite 輕量且獨立。 它是不需要任何其他相依性的程式庫。 不需進行任何設定。

:heavy_check_mark:沒有資料庫伺服器。 用戶端和伺服器以相同程序執行。

:heavy_check_mark:SQLite 位於公用網域，您可以自由使用並隨您的應用程式散發。

:heavy_check_mark:SQLite 可跨平台和架構運作。

您也可以在[這裡](https://sqlite.org/about.html)閱讀更多有關 SQLite 的資訊。

## <a name="choose-an-abstraction-layer"></a>選擇抽象層

我們建議您使用 Entity Framework Core 或 Microsoft 所建置的開放原始碼 [SQLite 程式庫](https://github.com/aspnet/Microsoft.Data.Sqlite/)。

### <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) 為物件關聯式對應程式，可讓您使用網域特定物件處理關聯式資料。 如果您已使用此架構處理其他 .NET 應用程式中的資料，可將該程式碼移轉到 UWP 應用程式，適當變更連接字串之後就能運作。

若要試試看，請參閱 [開始使用與 EF Core](/ef/core/get-started/overview/first-app)。

### <a name="sqlite-library"></a>SQLite 程式庫

[Microsoft.Data.Sqlite](/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0&preserve-view=true) 程式庫會在 [System.Data.Common](/dotnet/api/system.data.common) 命名空間中實作介面。 Microsoft 會主動維護這些實作，並提供直覺的包裝函式來處理低階原生 SQLite API。

本指南的其餘部分可協助您使用此程式庫。

## <a name="set-up-your-solution-to-use-the-microsoftdatasqlite-library"></a>設定您的解決方案以使用 Microsoft.Data.SQlite 程式庫

我們會從基本 UWP 專案開始，新增類別庫，然後安裝適當的 Nuget 套件。

您新增至解決方案的類別程式庫類型以及安裝的特定套件，取決於您的應用程式做為目標的最低 Windows SDK 版本。 您可以在 UWP 專案的屬性頁面中找到該資訊。

![Windows SDK 的最低版本](images/min-version.png)

根據做為您 UWP 專案目標的最低 Windows SDK 版本，使用下列其中一節。

### <a name="the-minimum-version-of-your-project-does-not-target-the-fall-creators-update"></a>您專案的最低版本不是以 Fall Creators Update 為目標

如果您使用 Visual Studio 2015，請按一下 [說明]->[關於 Microsoft Visual Studio]。 然後在已安裝的程式清單中，確定您有 NuGet 套件管理員 **3.5** 版或更新版本。 如果您的版本號碼較低，請在[這裡](https://www.nuget.org/downloads)安裝較新版的 NuGet。 在該頁面上，您會看見所有 Nuget 版本列於 **Visual Studio 2015** 標題下。

接下來，新增類別庫至您的解決方案。 您不必使用類別庫包含您的資料存取碼，但我們會在範例中使用一個。 我們會將程式庫命名為 **DataAccessLibrary**，並將程式庫中的類別命名為 **DataAccess**。

![螢幕擷取畫面：顯示 [新增專案] 對話方塊，其中顯示已選取 [已安裝] > [Visual C#] > [Windows 通用]，並醒目提示 [類別庫] 選項。](images/class-library.png)

以滑鼠右鍵按一下解決方案，然後按一下 [管理解決方案的 NuGet 套件]。

![[方案總管] 面板的螢幕擷取畫面，已使用滑鼠右鍵按一下專案，且醒目提示 [管理方案的 NuGet 套件] 選項。](images/manage-nuget.png)

如果您使用 Visual Studio 2015，請選擇 [已安裝] 索引標籤，並確認 **Microsoft.NETCore.UniversalWindowsPlatform** 套件的版本號碼為 **5.2.2** 或更新版本。

![.NETCore 版本](images/package-version.png)

如果不是的話，請將套件更新到較新版本。

選擇 [瀏覽] 索引標籤，然後搜尋 **Microsoft.Data.SQLite** 套件。 安裝該套件的 **1.1.1** 版 (或更低版本)。

![[Microsoft Data SQLite] 對話方塊的螢幕擷取畫面，並指出 [版本] 文字欄位。](images/sqlite-package.png)

繼續進行本指南中的[在 SQLite 資料庫中新增和擷取資料](#add-and-retrieve-data-in-a-sqlite-database)一節。

### <a name="the-minimum-version-of-your-project-targets-the-fall-creators-update"></a>您專案的最低版本是以 Fall Creators Update 為目標

將 UWP 專案的最低版本提升為 Fall Creators Update 有幾個優點。

首先，您可以使用 .NET Standard 2.0 程式庫，而非一般類別庫。 這表示，您可以與任何其他 .NET 應用程式共用您的資料存取碼，例如 WPF、Windows Forms、Android、iOS 或 ASP.NET 應用程式。

其次，您的應用程式沒有套件 SQLite 程式庫。 您的應用程式可以改用隨 Windows 安裝的 SQLite 版本。 這可為您提供幾個方面的協助。

:heavy_check_mark:縮減應用程式的大小，因為您不需要下載 SQLite 二進位檔，然後將它封裝為應用程式的一部分。

:heavy_check_mark:您不需要推送應用程式的新版本給使用者，在 SQLite 發佈 SQLite 中錯誤和安全性弱點的重大修正時。 Windows 版 SQLite 是由 Microsoft 與 SQLite.org 共同維護。

:heavy_check_mark:應用程式載入時間可能更快，因為 SDK 版的 SQLite 很可能已載入記憶體中。

讓我們從新增 .NET Standard 2.0 類別庫至您的解決方案開始著手。 您不需使用類別庫包含您的資料存取碼，但我們會在範例中使用一個。 我們會將程式庫命名為 **DataAccessLibrary**，並將程式庫中的類別命名為 **DataAccess**。

![螢幕擷取畫面：顯示 [新增專案] 對話方塊，其中顯示已選取 [已安裝] > [Visual C#] > [.NET Standard]，並醒目提示 [類別庫] 選項。](images/dot-net-standard.png)

以滑鼠右鍵按一下解決方案，然後按一下 [管理解決方案的 NuGet 套件]。

![[方案總管] 面板的另一個螢幕擷取畫面，已使用滑鼠右鍵按一下專案，且醒目提示 [管理 NuGet 套件] 選項。](images/manage-nuget-2.png)

> [!NOTE]
> 如果您希望 .NET Standard 類別庫能夠存取 UWP 應用程式的應用程式資料夾和映射資產，則必須在其 [屬性] 中將其標示為 **EmbeddedResource** 和 **CopyAlways**。

此時，您會有一個選擇。 您可以使用隨 Windows 提供的 SQLite 版本，或如果您因故需要使用特定 SQLite 版本，可以在您的套件中包含 SQLite 程式庫。

讓我們從如何使用 Windows 隨附的 SQLite 版本開始說明。

#### <a name="to-use-the-version-of-sqlite-that-is-installed-with-windows"></a>若要使用隨 Windows 安裝的 SQLite 版本

選擇 [瀏覽] 索引標籤，然後搜尋 **Microsoft.Data.SQLite.core** 套件，再進行安裝。

![SQLite Core 套件](images/sqlite-core-package.png)

搜尋 **SQLitePCLRaw.bundle_winsqlite3** 套件，然後只將它安裝到您解決方案中 UWP 專案。

![SQLite PCL Raw 套件](images/sqlite-raw-package.png)

#### <a name="to-include-sqlite-with-your-app"></a>在您的應用程式中包含 SQLite

您不必這樣做。 但如果您因故需要在應用程式中包括特定版本的 SQLite，請選擇 [瀏覽] 索引標籤，然後搜尋 **Microsoft.Data.SQLite** 套件。 安裝該套件的 **2.0** 版 (或更低版本)。

![[Microsoft Data SQLite] 對話方塊的螢幕擷取畫面，顯示已選取 [最新穩定 2.0.0 版]。 已指出 [版本] 文字欄位。](images/sqlite-package-v2.png)

## <a name="add-and-retrieve-data-in-a-sqlite-database"></a>在 SQLite 資料庫中新增和擷取資料

我們將執行下列工作︰

:one:準備資料存取類別。

:two:初始化 SQLite 資料庫。

:three:將資料插入 SQLite 資料庫。

:four:從 SQLite 資料庫擷取資料。

:five:新增基本使用者介面。

### <a name="prepare-the-data-access-class"></a>準備資料存取類別

從 UWP 專案，在您的解決方案中新增 **DataAccessLibrary** 專案的參考。

![資料存取類別庫](images/ref-class-library.png)

將下列 ``using`` 陳述式新增到您 UWP 專案中的 **App.xaml.cs** 和 **MainPage.xaml.cs** 檔案。

```csharp
using DataAccessLibrary;
```

開啟 **DataAccessLibrary** 解決方案中的 **DataAccess** 類別，並使該類別成為靜態。

>[!NOTE]
>在範例中，我們會將資料存取碼放入靜態類別中，這只是一種設計上的選擇，完全非硬性規定。

```csharp
namespace DataAccessLibrary
{
    public static class DataAccess
    {

    }
}

```

將下列 using 陳述式新增到此檔案的頂端。

```csharp
using Microsoft.Data.Sqlite;
using System.Collections.Generic;
```


### <a name="initialize-the-sqlite-database"></a>初始化 SQLite 資料庫

新增方法至 **DataAccess** 類別，以初始化 SQLite 資料庫。

```csharp
public async static void InitializeDatabase()
{ 
     await ApplicationData.Current.LocalFolder.CreateFileAsync("sqliteSample.db", CreationCollisionOption.OpenIfExists);
     string dbpath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "sqliteSample.db");
     using (SqliteConnection db =
        new SqliteConnection($"Filename={dbpath}"))
    {
        db.Open();

        String tableCommand = "CREATE TABLE IF NOT " +
            "EXISTS MyTable (Primary_Key INTEGER PRIMARY KEY, " +
            "Text_Entry NVARCHAR(2048) NULL)";

        SqliteCommand createTable = new SqliteCommand(tableCommand, db);

        createTable.ExecuteReader();
    }
}
```

此程式碼會建立 SQLite 資料庫，並將它儲存在應用程式的本機資料存放區。

在此範例中，我們將資料庫命名為 ``sqlliteSample.db``，但您可以使用任意名稱，只要您將該名稱用於您具現化的所有 [SqliteConnection](/dotnet/api/microsoft.data.sqlite.sqliteconnection?view=msdata-sqlite-2.0.0&preserve-view=true) 物件。

在 UWP 專案的 **App.xaml.cs** 檔案建構函式中，呼叫 **DataAccess** 類別的 ``InitializeDatabase`` 方法。

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;

    DataAccess.InitializeDatabase();

}
```


### <a name="insert-data-into-the-sqlite-database"></a>將資料插入 SQLite 資料庫

新增方法至 **DataAccess** 類別，將資料插入 SQLite 資料庫。 此程式碼會在查詢中使用參數，以避免 SQL 插入攻擊。

```csharp
public static void AddData(string inputText)
{
    string dbpath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "sqliteSample.db");
    using (SqliteConnection db =
      new SqliteConnection($"Filename={dbpath}"))
    {
        db.Open();

        SqliteCommand insertCommand = new SqliteCommand();
        insertCommand.Connection = db;

        // Use parameterized query to prevent SQL injection attacks
        insertCommand.CommandText = "INSERT INTO MyTable VALUES (NULL, @Entry);";
        insertCommand.Parameters.AddWithValue("@Entry", inputText);

        insertCommand.ExecuteReader();

        db.Close();
    }

}
```


### <a name="retrieve-data-from-the-sqlite-database"></a>從 SQLite 資料庫擷取資料

新增從 SQLite 資料庫取得資料列的方法。

```csharp
public static List<String> GetData()
{
    List<String> entries = new List<string>();

   string dbpath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "sqliteSample.db");
   using (SqliteConnection db =
      new SqliteConnection($"Filename={dbpath}"))
    {
        db.Open();

        SqliteCommand selectCommand = new SqliteCommand
            ("SELECT Text_Entry from MyTable", db);

        SqliteDataReader query = selectCommand.ExecuteReader();

        while (query.Read())
        {
            entries.Add(query.GetString(0));
        }

        db.Close();
    }

    return entries;
}
```

[Read](/dotnet/api/microsoft.data.sqlite.sqlitedatareader.read?view=msdata-sqlite-2.0.0&preserve-view=true#Microsoft_Data_Sqlite_SqliteDataReader_Read) 方法會逐一處理所傳回的資料列。 如果有剩餘的列，它會傳回 **true**，否則會傳回 **false**。

[GetString](/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getstring?view=msdata-sqlite-2.0.0&preserve-view=true#Microsoft_Data_Sqlite_SqliteDataReader_GetString_System_Int32_) 方法會將所指定欄的值做為字串傳回。 它接受整數值，代表您要的資料的以零起始欄序數。 您可以使用類似的方法，例如 [GetDataTime](/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getdatetime?view=msdata-sqlite-2.0.0&preserve-view=true#Microsoft_Data_Sqlite_SqliteDataReader_GetDateTime_System_Int32_) 和 [GetBoolean](/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getboolean?view=msdata-sqlite-2.0.0&preserve-view=true#Microsoft_Data_Sqlite_SqliteDataReader_GetBoolean_System_Int32_)。 根據欄所包含的資料類型選擇方法。

在此範例中，序數參數並不那麼重要，因為我們會選取一欄中的所有項目。 不過，如果您的查詢中有多欄，請使用序數值取得要從中擷取資料的欄。

## <a name="add-a-basic-user-interface"></a>新增基本使用者介面

在 UWP 專案的 **MainPage.xaml** 檔案中，新增下列 XAML。

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Name="Input_Box"></TextBox>
        <Button Click="AddData">Add</Button>
        <ListView Name="Output">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding}"/>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackPanel>
</Grid>
```

這個基本使用者介面會提供 ``TextBox`` 給使用者，讓他們用來輸入字串，我們會將該字串新增到 SQLite 資料庫。 我們會將此 UI 中的 ``Button`` 連接至事件處理常式，它會從 SQLite 資料庫擷取資料，然後在 ``ListView`` 中顯示該資料。

在 **MainPage.xaml.cs** 檔案中，新增下列處理常式。 這是我們與 UI 中 ``Button`` 的 ``Click`` 事件產生關聯的方法。

```csharp
private void AddData(object sender, RoutedEventArgs e)
{
    DataAccess.AddData(Input_Box.Text);

    Output.ItemsSource = DataAccess.GetData();
}
```

就這麼簡單。 探索 [Microsoft.Data.Sqlite](/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0&preserve-view=true)，了解還可以利用 SQLite 資料庫做什麼。 查看下列連結，了解在您的 UWP 應用程式中使用資料的其他方式。

## <a name="next-steps"></a>接下來的步驟

**將您的應用程式直接連接至 SQL Server 資料庫**

請參閱[在 UWP app 中使用 SQL Server 資料庫](sql-server-databases.md)。

**在不同平台的不同應用程式之間共用程式碼**

請參閱[在桌面應用程式與 UWP 之間共用程式碼](../porting/desktop-to-uwp-migrate.md)。

**在 Azure SQL 後端新增主要詳細資料頁面**

請參閱[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database) (英文)。
