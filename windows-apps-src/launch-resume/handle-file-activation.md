---
title: 處理檔案啟用
description: 應用程式可以登錄為特定檔案類型的預設處理常式。
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 0c334caa159b423771bfe64cf7c1a769d5f84c99
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164782"
---
# <a name="handle-file-activation"></a>處理檔案啟用

**重要 API**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](/uwp/api/windows.ui.xaml.application.onfileactivated)

您的應用程式可以註冊成為特定檔案類型的預設處理常式。 Windows 傳統型應用程式和「通用 Windows 平台」(UWP) app 皆可登錄為預設檔案處理常式。 如果使用者選擇您的 App 作為特定檔案類型的預設處理常式，則在啟動該類型的檔案時就會啟用您的 App。

建議只有當您希望處理某個檔案類型的所有檔案啟動時，才登錄該檔案類型。 如果您的應用程式只需在內部使用檔案類型，就不需要登錄為預設處理常式。 如果您選擇登錄某個檔案類型，則在針對該檔案類型啟用您的應用程式時，您必須為使用者提供預期的功能。 例如，圖片檢視器應用程式需要登錄以顯示 .jpg 檔案。 如需檔案關聯的詳細資訊，請參閱[檔案類型與 URI 的指導方針](../files/index.md)。

這些步驟示範如何登錄自訂檔案類型 .alsdk，以及如何在使用者啟動 .alsdk 檔案時啟用您的 app。

> **注意**   在 UWP 應用程式中，某些 Uri 和副檔名是保留供內建應用程式和作業系統使用。 如果嘗試以保留的 URI 或副檔名登錄 app，該嘗試將會被忽略。 如需詳細資訊，請參閱[保留的檔案和 URI 配置名稱](reserved-uri-scheme-names.md)。

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>步驟 1：在封裝資訊清單中指定擴充點

App 僅會接受封裝資訊清單中列示之副檔名的啟用事件。 以下是如何指示應 app 處理具有 `.alsdk` 副檔名的檔案。

1.  在 **\[方案總管\]** 中按兩下 package.appxmanifest，以開啟資訊清單設計工具。 選取 [ **宣告] 索引** 標籤，然後在 [ **可用** 的宣告] 下拉式清單中，選取 [ **檔案類型關聯** ]，然後按一下 [ **新增**]。 請參閱[以程式設計方式撰寫識別碼](/windows/desktop/shell/fa-progids)，了解檔案關聯使用的識別碼詳細資料。

    以下簡短說明可在資訊清單設計工具中填寫的每個欄位：

| 欄位 | 說明 |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **顯示名稱** | 指定一組檔案類型的顯示名稱。 顯示名稱是用來識別**主控台**上的 [[設定預設程式](/windows/desktop/shell/default-programs)] 中的檔案類型。 |
| **標誌** | 指定用來識別桌上型電腦上之檔案類型的標誌，以及**主控台**上的 [[設定預設程式](/windows/desktop/shell/default-programs)]。 如果沒有指定標誌，則會使用應用程式的小標誌。 |
| **資訊提示** | 指定一組檔案類型的[資訊提示](/windows/desktop/shell/fa-progids)。 當使用者的滑鼠游標暫留在這類檔案的圖示上時，就會顯示這個工具提示文字。 |
| **名稱** | 為共用相同顯示名稱、標誌、資訊提示以及編輯旗標的一組檔案類型選擇一個名稱。 選擇可以在所有 app 更新都維持一致的群組名稱。 **注意**  [名稱] 必須全都是小寫字母。 |
| **內容類型** | 為特定檔案類型指定 MIME 內容類型，例如 **image/jpeg**。 **允許之內容類型的重要事項：** 以下是您無法在套件資訊清單中輸入的 MIME 內容類型清單 (依字母順序排序)，因為它們已經被保留或禁止使用：**application/force-download**、**application/octet-stream**、**application/unknown**、**application/x-msdownload**。 |
| **檔案類型** | 指定要登錄的檔案類型，前面加上句號，例如 “.jpeg”。 **保留和禁止的檔案類型：** 請參閱[保留 URI 配置名稱和檔案類型](reserved-uri-scheme-names.md)，以取得因為已被保留或禁止使用，而無法為 UWP app 登錄之內建 app 的檔案類型清單 (依字母順序排序)。 |

2.  輸入 `alsdk` 做為 **名稱**。
3.  輸入 `.alsdk` 做為 **檔案類型**。
4.  輸入「影像 \\Icon.png」作為標誌。
5.  按下 Ctrl+S 以將變更儲存至 package.appxmanifest。

上述步驟會將和這個一樣的 [**Extension**](/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) 元素新增至封裝資訊清單。 **windows.fileTypeAssociation** 類別指示 app 處理具有 `.alsdk` 副檔名的檔案。

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## <a name="step-2-add-the-proper-icons"></a>步驟 2：新增適當圖示

成為檔案類型預設程式的 app，會在系統的各個地方顯示它們的圖示。 例如，這些圖示會顯示在：

-   Windows 檔案總管清單檢視、操作功能表及功能區
-   預設程式控制台
-   檔案選擇器
-   [開始] 畫面上的搜尋結果

連同您的專案包含 44x44 圖示，讓您的標誌可以出現在這些位置中。 請調整為相符的應用程式磚標誌外觀，並使用應用程式的背景色彩，而不要讓圖示變成透明。 請將標誌延伸至邊緣，且沒有邊框間距。 在白色背景上測試您的圖示。 請參閱[磚和圖示資產的指導方針](../design/style/app-icons-and-logos.md)，以取得圖示的詳細資訊。

## <a name="step-3-handle-the-activated-event"></a>步驟 3：處理啟用的事件

[**OnFileActivated**](/uwp/api/windows.ui.xaml.application.onfileactivated) 事件處理常式會接收所有的檔案啟用事件。

```csharp
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```

```cppwinrt
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs const& args)
{
    // TODO: Handle file activation.
    auto numberOfFilesReceived{ args.Files().Size() };
    auto nameOfTheFirstFile{ args.Files().GetAt(0).Name() };
}
```

```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
    // TODO: Handle file activation
    // The number of files received is args->Files->Size
    // The name of the first file is args->Files->GetAt(0)->Name
}
```

> [!NOTE]
> 透過檔案合約啟動時，請確定 [上一頁] 按鈕將使用者帶回啟動應用程式的畫面，而不是應用程式先前的內容。

建議您為每個啟用事件建立新的 XAML **框架** ，以開啟新的頁面。 如此一來，新 XAML 框架的導覽 >frame.backstack 就不會包含應用程式在暫止時可能在目前視窗上擁有的任何先前內容。 如果您決定要使用單一 XAML **框架** 來啟動和檔案合約，則在流覽至新頁面之前，您應該先清除 **框架**導覽日誌中的頁面。

當您的應用程式透過檔案啟用啟動時，您應該考慮加入可讓使用者回到應用程式頂端頁面的 UI。

## <a name="remarks"></a>備註

您接收的檔案可能來自不受信任的來源。 建議您先驗證檔案內容，然後才在上面執行動作。

## <a name="related-topics"></a>相關主題

### <a name="complete-example"></a>完整範例

* [關聯啟動範例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

### <a name="concepts"></a>概念

* [預設程式](/windows/desktop/shell/default-programs)
* [檔案類型與通訊協定關聯模型](/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>工作

* [啟動檔案的預設應用程式](launch-the-default-app-for-a-file.md)
* [處理 URI 啟用](handle-uri-activation.md)

### <a name="guidelines"></a>指導方針

* [檔案類型與 URI 的指導方針](../files/index.md)

### <a name="reference"></a>參考
* [Windows.ApplicationModel.Activation.FileActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
* [Windows.UI.Xaml.Application.OnFileActivated](/uwp/api/windows.ui.xaml.application.onfileactivated)