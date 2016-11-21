---
author: awkoren
Description: "此文章列出使用傳統型轉 UWP 橋接器轉換 App 之前，您需要知道的事項。 您可能不需要針對 App 的轉換程序做太多準備。"
Search.Product: eADQiWindows 10XVcnh
title: "針對傳統型轉 UWP 橋接器準備 App"
translationtype: Human Translation
ms.sourcegitcommit: 8429e6e21319a03fc2a0260c68223437b9aed02e
ms.openlocfilehash: 4cf9c509be52a8b2c03cdaa9ac68b98ba49b7094

---

# 準備 App 以使用傳統型橋接器來轉換

此文章列出使用傳統型轉 UWP 橋接器轉換 App 之前，您需要知道的事項。 您可能不需要針對 App 的轉換程序做太多準備，但如果以下任何項目適用於您的應用程式，您就需要先處理之後才能轉換。 請記住，Windows 市集會為您處理授權和自動更新，因此您可以從程式碼基底中移除那些功能。

+ __您的 App 是使用 4.6.1 之前的 .NET 版本__。 僅支援 .NET 4.6.1。 您必須先將應用程式重新定位至 .NET 4.6.1 才能轉換。 

+ __您的應用程式一律會以提升的安全性權限執行__。 在以互動使用者身分執行時您的應用程式必須能夠運作。 從 Windows 市集安裝您應用程式的使用者可能不是系統管理員，因此需要以提升的權限來執行您的應用程式，表示它無法針對標準使用者正常執行。

+ __您的應用程式需要核心模式驅動程式或 Windows 服務__。 橋接器適用於應用程式，但它不支援核心模式驅動程式或需要在系統帳戶下執行的 Windows 服務。 請改用[背景工作](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)，而非 Windows 服務。

+ __您的應用程式模組會以同處理序載入至不在您 AppX 套件中的處理序__。 這是不允許的，這表示不支援同處理序擴充功能，例如 [Shell 擴充功能](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)。 但是，如果您在同一個套件中有兩個 app，您就可以在它們之間進行處理序間通訊。

+ __您的 app 呼叫 [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) 或 [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)__。 已轉換的應用程式目前不支援這些功能。 我們正致力於在未來版本中新增支援。 因應措施是，您可以將使用這些功能找到的所有 .dll 複製到套件根目錄。 

+ __您的 app 會使用自訂的應用程式使用者模型識別碼 (AUMID)__。 如果您的處理序會呼叫 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) 來設定自己的 AUMID，則它可能只會使用應用程式模型環境/AppX 套件為它產生的 AUMID。 您不能定義自訂的 AUMID。

+ __您的應用程式會修改 HKEY_LOCAL_MACHINE (HKLM) 登錄區__。 應用程式對於建立 HKLM 機碼或開啟某一個機碼進行修改的任何嘗試都將產生存取遭拒的失敗。 請記住，您的應用程式有自己私用的登錄虛擬化檢視，因此，不適用全部使用者和整部電腦登錄區的概念 (這是 HKLM 的概念)。 您需要尋找其他方式來封存您使用 HKLM 所做的動作，例如改為寫入 HKEY_CURRENT_USER (HKCU)。

+ __您的應用程式會使用 ddeexec 登錄子機碼做為啟動另一個應用程式的方式__。 在您的[應用程式套件資訊清單](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中，使用各種不同 Activatable* 擴充功能來設定時，請改用其中一個 DelegateExecute 動詞命令處理常式。

+ __您的應用程式會寫入 AppData 資料夾，以便與其他應用程式共用資料__。 轉換之後，系統會將 AppData 重新導向至本機 app 資料存放區，此為每一個 UWP app 的私人存放區。 使用不同方式來進行處理序間的資料共用。 如需詳細資訊，請參閱[儲存及擷取設定和其他應用程式資料](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

+ __您的應用程式會寫入您應用程式的安裝目錄__。 例如，您的應用程式所寫入的記錄檔是放在與您的 exe 相同的目錄中。 不支援此情況，因此您將需要尋找其他位置，例如本機應用程式資料存放區。

+ __您的應用程式安裝需要使用者互動__。 您的應用程式安裝程式必須能夠以無訊息方式執行，而且它需要安裝所有預設不在全新作業系統映像上的必要條件。

+ __您的應用程式會使用目前的工作目錄__。 在執行階段，已轉換的應用程式將不會取得您先前在桌面 .LNK 捷徑中指定的相同工作目錄。 如果您的應用程式必須擁有正確的目錄才能正確運作，則您需要在執行階段變更 CWD。

+ __您的應用程式需要 UIAccess__。 如果您的應用程式在 UAC 資訊清單的 `requestedExecutionLevel` 元素中指定 `UIAccess=true`，則目前不支援轉換為 UWP。 如需詳細資訊，請參閱 [UI 自動化安全性概觀](https://msdn.microsoft.com/library/ms742884.aspx)。

+ __您的應用程式公開 COM 物件或 GAC 組件給其他處理程序使用__。 在目前版本中，您的應用程式不能公開 COM 物件或 GAC 組件給來自 AppX 套件外部的可執行檔處理程序使用。 來自套件內的處理程序可以正常登錄及使用 COM 物件和 GAC 組件，但無法從外部看見它們。 這表示如果透過外部處理程序叫用 OLE 等互通性案例，將無法作用。 

+ __App 以不支援的方法連結 C 執行階段程式庫 (CRT)__。 Microsoft C/C++ 執行階段程式庫提供適用於 Microsoft Windows 作業系統的程式設計常式。 這些常式將許多常見但 C 和 C++ 語言未提供的程式設計工作自動化。 如果您的應用程式利用 C/C++ 執行階段程式庫，您必須確定以支援的方式連結它。 
    
    Visual Studio 2015 支援用動態連結 (讓程式碼使用一般 DLL 檔案) 或用靜態連結 (將程式庫直接連結到程式碼中)，連結到目前的 CRT 版本。 可能的話，我們建議您的應用程式搭配 VS 2015 使用動態連結。 

    支援先前的多種 Visual Studio 版本。 如需詳細資訊，請參閱下表： 

    <table>
    <th>Visual Studio 版本</td><th>動態連結</th><th>靜態連結</th></th>
    <tr><td>2005 (VC 8)</td><td>不支援</td><td>支援</td>
    <tr><td>2008 (VC 9)</td><td>不支援</td><td>支援</td>
    <tr><td>2010 (VC 10)</td><td>支援</td><td>支援</td>
    <tr><td>2012 (VC 11)</td><td>支援</td><td>不支援</td>
    <tr><td>2013 (VC 12)</td><td>支援</td><td>不支援</td>
    <tr><td>2015 (VC 14)</td><td>支援</td><td>支援</td>
    </table>
    
    注意：在任何情況下，您都必須連結到最新可公開取得的 CRT。

+ __您的 App 會從 Windows 並列資料夾安裝與載入組件__。 例如，您的 App 使用 C 執行階段程式庫 VC8 或 VC9 且從 Windows 並列資料夾動態連結它們，表示您的程式碼是使用來自共用資料夾的通用 DLL 檔。 不支援此連結方式。 您必須以靜態方式連結它們，方法是直接將可轉散發的程式庫檔案連結到您的程式碼。


<!--HONumber=Nov16_HO1-->

