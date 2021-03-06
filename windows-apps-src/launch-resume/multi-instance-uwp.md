---
title: 建立多執行個體通用 Windows 應用程式
description: 本主題描述如何撰寫支援多執行個體的 UWP 應用程式。
keywords: 多重執行個體 uwp
ms.date: 09/21/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e19425222459bb3bd56a5fe406ec76c0b7fb6b9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164772"
---
# <a name="create-a-multi-instance-universal-windows-app"></a>建立多執行個體通用 Windows 應用程式

本主題描述如何建立多重執行個體通用 Universal Windows 平台 (UWP) 應用程式。

From Windows 10，version 1803 (10.0;組建 17134) 開始，您的 UWP 應用程式可以選擇支援多個實例。 如果多執行個體 UWP 應用程式的其中一個執行個體正在執行，而且後續啟用要求成功時，平台將不會啟用現有的執行個體。 反而會建立執行於不同處理序中的新執行個體。

> [!IMPORTANT]
> JavaScript 應用程式支援多重實例，但不支援多重實例重新導向。 由於 JavaScript 應用程式不支援多重實例重新導向，因此 [**AppInstance**](/uwp/api/windows.applicationmodel.appinstance) 類別對這類應用程式來說並不有用。

## <a name="opt-in-to-multi-instance-behavior"></a>加入多重實例行為

如果您正在建立新的多重執行個體應用程式，您可以安裝 [Multi-Instance App Project Templates.VSIX](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.MultiInstanceApps) (可從 [Visual Studio Marketplace](https://marketplace.visualstudio.com/) 取得)。 在安裝範本後，便可從 \[Visual C#\] > \[Windows Universal\]**** (或 \[其他語言\] > \[Visual C++\] > \[Windows Universal\]****) 下的 \[新專案\] **** 對話方塊中取得這些範本。

這會安裝兩個範本：一是**多重執行個體 UWP 應用程式**，可提供用於建立多重執行個體應用程式的範本；另一是**多重執行個體重新導向 UWP 應用程式**，可提供額外的邏輯作為建置的基礎，以啟動新的執行個體，或選擇性地啟用已啟動的執行個體。 例如，您可能只是在編輯相同的檔時，一次只需要一個實例，因此您可以將具有該檔案的實例開啟到前景，而不是啟動新的實例。

這兩個範本都會新增 `SupportsMultipleInstances` 至檔案 `package.appxmanifest` 。 請注意命名空間前置詞 `desktop4` 和 `iot2` ：僅限以桌面為目標的專案，或物聯網 (IoT) 專案的專案，支援多實例。

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2"  
  IgnorableNamespaces="uap mp desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:SupportsMultipleInstances="true"
      iot2:SupportsMultipleInstances="true">
      ...
    </Application>
  </Applications>
   ...
</Package>
```

## <a name="multi-instance-activation-redirection"></a>多重執行個體啟用重新導向

 UWP 應用程式的多重執行個體支援不僅僅是可以啟動應用程式的多重執行個體。 無論您想要選取啟動應用程式的新執行個體，或選取啟用已執行的執行個體，皆允許自訂。 例如，如果啟動應用程式以編輯一個檔案，但已有另一個執行個體正在編輯檔案，此時您想要將啟用重新導向至該執行個體，而非開啟另一個已在編輯該檔案的執行個體。

若要查看其運作方式，請觀看這段影片以瞭解如何建立多個實例的 UWP 應用程式。

> [!VIDEO https://www.youtube.com/embed/clnnf4cigd0]

**多重執行個體重新導向 UWP 應用程式**範本會將 `SupportsMultipleInstances` 新增至 package.appxmanifest 檔案，如上所示，也會將  **Program.cs** (如果您使用的是 C++ 版範本，則為 **Program.cpp**) 新增至包含 `Main()` 函數的專案。 重新導向啟用的邏輯會進入 `Main` 函數。 **Program.cs**的範本如下所示。

如果有 (或沒有一個) ，則 [**RecommendedInstance**](/uwp/api/windows.applicationmodel.appinstance.recommendedinstance) 屬性代表此啟用要求的 shell 提供的慣用實例 `null` 。 如果 shell 提供喜好設定，則您可以將啟用重新導向至該實例，或者如果您選擇，則可以忽略它。

``` csharp
public static class Program
{
    // This example code shows how you could implement the required Main method to
    // support multi-instance redirection. The minimum requirement is to call
    // Application.Start with a new App object. Beyond that, you may delete the
    // rest of the example code and replace it with your custom code if you wish.

    static void Main(string[] args)
    {
        // First, we'll get our activation event args, which are typically richer
        // than the incoming command-line args. We can use these in our app-defined
        // logic for generating the key for this instance.
        IActivatedEventArgs activatedArgs = AppInstance.GetActivatedEventArgs();

        // If the Windows shell indicates a recommended instance, then
        // the app can choose to redirect this activation to that instance instead.
        if (AppInstance.RecommendedInstance != null)
        {
            AppInstance.RecommendedInstance.RedirectActivationTo();
        }
        else
        {
            // Define a key for this instance, based on some app-specific logic.
            // If the key is always unique, then the app will never redirect.
            // If the key is always non-unique, then the app will always redirect
            // to the first instance. In practice, the app should produce a key
            // that is sometimes unique and sometimes not, depending on its own needs.
            string key = Guid.NewGuid().ToString(); // always unique.
                                                    //string key = "Some-App-Defined-Key"; // never unique.
            var instance = AppInstance.FindOrRegisterInstanceForKey(key);
            if (instance.IsCurrentInstance)
            {
                // If we successfully registered this instance, we can now just
                // go ahead and do normal XAML initialization.
                global::Windows.UI.Xaml.Application.Start((p) => new App());
            }
            else
            {
                // Some other instance has registered for this key, so we'll 
                // redirect this activation to that instance instead.
                instance.RedirectActivationTo();
            }
        }
    }
}
```

`Main()` 是第一個執行的內容。 它會在 [**>onlaunched**](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 和 [**OnActivated**](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnActivated_Windows_ApplicationModel_Activation_IActivatedEventArgs_)之前執行。 在任何其他初始化程式碼在您的應用程式執行之前，這可讓您決定要啟用此執行個體，或啟用另一個執行個體。

以上的程式碼會判斷啟用的是應用程式的現有執行個體或新執行個體。 在此會使用索引鍵來判斷是否有您想要啟用的現有執行個體。 例如，如果可以啟動您的應用程式以[處理檔案啟用](./handle-file-activation.md)，您可以使用檔案名稱作為索引鍵。 然後，您可以檢查該應用程式的執行個體是否已透過該索引鍵註冊，並啟用該執行個體，而非開啟新的執行個體。 這是程式碼背後的概念： `var instance = AppInstance.FindOrRegisterInstanceForKey(key);`

如果找到以該索引鍵註冊的執行個體，則會啟用該執行個體。 如果找不到索引鍵，則目前的執行個體 (亦即目前正在執行 `Main` 的執行個體) 會建立其應用程式物件，並開始執行。

## <a name="background-tasks-and-multi-instancing"></a>背景工作與多重執行個體

- 跨處理序背景工作支援多重執行個體。 一般而言，每個新觸發程序都會產生背景工作的新執行個體 (雖然從技術方面而言，多背景工作會在相同的主機處理序中執行)。 不過，系統會為背景工作建立不同的執行個體。
- 同處理序背景工作不支援多重執行個體。
- 背景音訊工作不支援多重執行個體。
- 當應用程式註冊背景工作時，通常會先檢查該工作是否已註冊，然後會刪除並重新註冊該工作，或是不執行任何動作以保留現有註冊。 這仍是多重執行個體應用程式的一般運作方式。 然而，多重執行個體應用程式可能會選擇根據執行個體註冊不同的背景工作名稱。 這將會造成多重註冊相同觸發程序，且當觸發程序引發時會啟用多重背景工作執行個體。
- 應用程式服務會為每個連線的應用程式服務背景工作啟動個別的執行個體。 這對於多重執行個體應用程式會保持不變，亦即多重執行個體應用程式的每個執行個體將得到自己的應用程式服務背景工作執行個體。 

## <a name="additional-considerations"></a>其他考量

- 以桌面與物聯網 (IoT) 專案為目標的 UWP 應用程式支援多重執行個體。
- 為避免產生競爭條件及發生爭用的問題，多重執行個體應用程式必須採取步驟來分割/同步化設定、應用程式本機存放區，及任何其他可在多重執行個體間共用之資源 (例如使用者檔案、資料存放區等等) 的存取權。 您可以使用標準同步處理機制，例如 mutex、信號、事件等等。
- 如果應用程式在 Package.appxmanifest 檔案中有 `SupportsMultipleInstances`，則其延伸模組不需要宣告 `SupportsMultipleInstances`。 
- 如果除了背景工作或應用程式服務外，您將 `SupportsMultipleInstances` 新增至任何其他延伸模組，且主控該延伸模組的應用程式也未在其 Package.appxmanifest 檔案中宣告 `SupportsMultipleInstances`，則會產生結構描述錯誤。
- 應用程式可以在其資訊清單中使用 [**ResourceGroup**](./declare-background-tasks-in-the-application-manifest.md) 宣告，將多個背景工作群組至相同的主控制項。 這會與多重執行個體衝突，其中的每個啟用都會分別進入不同的主機。 因此，應用程式無法在其資訊清單中宣告 `SupportsMultipleInstances` 與 `ResourceGroup`。

## <a name="sample"></a>範例

如需多重實例啟用重新導向的範例，請參閱 [多重實例範例](https://github.com/Microsoft/AppModelSamples/tree/master/Samples/BananaEdit) 。

## <a name="see-also"></a>另請參閱

[AppInstance. FindOrRegisterInstanceForKey](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_FindOrRegisterInstanceForKey_System_String_) 
[AppInstance. GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_GetActivatedEventArgs) 
[AppInstance. RedirectActivationTo](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_RedirectActivationTo) 
[處理應用程式啟用](./activate-an-app.md)