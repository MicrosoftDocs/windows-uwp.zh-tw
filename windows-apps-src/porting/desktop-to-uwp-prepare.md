---
author: normesta
Description: This article lists things you need to know before packaging your app with the Desktop Bridge. You may not need to do much to get your app ready for the packaging process.
Search.Product: eADQiWindows 10XVcnh
title: 準備封裝應用程式 (傳統型橋接器)
ms.author: normesta
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 1a4836992675f65773e9b5c890aca243e2a9e172
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832542"
---
# <a name="prepare-to-package-an-app-desktop-bridge"></a>準備封裝應用程式 (傳統型橋接器)

本文列出封裝傳統型應用程式之前，您需要知道的事項。 您可能不需要針對應用程式的封裝程序做太多準備，但如果以下任何項目適用於您的應用程式，您就需要先處理之後才能封裝。 請記住，Microsoft Store 會為您處理授權和自動更新，因此您可以從程式碼基底中移除與那些工作有關的任何功能。

>[!IMPORTANT]
>傳統型橋接器在 Windows 10 (版本 1607) 中引進，只適用於 Visual Studio 中目標為 Windows 10 年度更新版 (10.0；組建 14393) 或更新版本的專案。

+ __您的 App 需要使用 4.6.2 之前的 .NET 版本__。 您必須確認應用程式在 .NET 4.6.2 上執行。 您無法要求或轉散發 4.6.2 之前的版本。 這是隨附於 Windows 10 年度更新版的 .NET 版本。 確認您的應用程式在此版本上運作，可確保您的應用程式將繼續相容於 Windows 10 的未來更新。  如果您的應用程式以 .NET Framework 4.0 或更新版本為目標，它應該可在 .NET 4.6.2 上執行，但您仍然應該測試。

+ __您的應用程式一律會以提升的安全性權限執行__。 在以互動使用者身分執行時您的應用程式必須能夠運作。 從 Microsoft Store 安裝您應用程式的使用者可能不是系統管理員，因此需要以提升的權限來執行您的應用程式，表示它無法針對標準使用者正常執行。 Microsoft Store 不接受其功能的任何部分需要提高權限的應用程式。

+ __您的應用程式需要核心模式驅動程式或 Windows 服務__。 橋接器適用於應用程式，但它不支援核心模式驅動程式或需要在系統帳戶下執行的 Windows 服務。 請改用[背景工作](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)，而非 Windows 服務。

+ __您的應用程式模組會以同處理程序載入至不在您 Windows 應用程式套件中的處理程序__。 這是不允許的，這表示不支援同處理序擴充功能，例如 [Shell 擴充功能](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)。 但是，如果您在同一個套件中有兩個 app，您就可以在它們之間進行處理序間通訊。

+ __您的應用程式會使用自訂的應用程式使用者模型識別碼 (AUMID)__。 如果您的處理序會呼叫 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) 來設定自己的 AUMID，則它可能只會使用應用程式模型環境/Windows 應用程式套件為它產生的 AUMID。 您不能定義自訂的 AUMID。

+ __您的應用程式會修改 HKEY_LOCAL_MACHINE (HKLM) 登錄區__。 應用程式對於建立 HKLM 機碼或開啟某一個機碼進行修改的任何嘗試都將產生存取遭拒的失敗。 請記住，您的應用程式有自己私用的登錄虛擬化檢視，因此，不適用全部使用者和整部電腦登錄區的概念 (這是 HKLM 的概念)。 您需要尋找其他方式來封存您使用 HKLM 所做的動作，例如改為寫入 HKEY_CURRENT_USER (HKCU)。

+ __您的應用程式會使用 ddeexec 登錄子機碼做為啟動另一個應用程式的方式__。 在您的[應用程式套件資訊清單](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中，使用各種不同 Activatable* 擴充功能來設定時，請改用其中一個 DelegateExecute 動詞命令處理常式。

+ __您的應用程式會寫入 AppData 資料夾或登錄，以便與其他應用程式共用資料__。 轉換之後，系統會將 AppData 重新導向至本機 app 資料存放區，此為每一個 UWP app 的私人存放區。

  您的應用程式寫入至 HKEY_LOCAL_MACHINE 登錄區的所有項目都會重新導向至隔離的二進位檔，而您的應用程式寫入至 HKEY_CURRENT_USER 登錄區的任何項目都會放到私人的各個使用者、各個應用程式位置。 如需檔案和登錄重新導向的詳細資訊，請參閱[傳統型橋接器的幕後作業](desktop-to-uwp-behind-the-scenes.md)。  

  使用不同方式來進行處理序間的資料共用。 如需詳細資訊，請參閱[儲存及擷取設定和其他應用程式資料](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

+ __您的應用程式會寫入您應用程式的安裝目錄__。 例如，您的應用程式所寫入的記錄檔是放在與您的 exe 相同的目錄中。 不支援此情況，因此您將需要尋找其他位置，例如本機應用程式資料存放區。

+ __您的應用程式安裝需要使用者互動__。 您的應用程式安裝程式必須能夠以無訊息方式執行，而且它需要安裝所有預設不在全新作業系統映像上的必要條件。

+ __您的應用程式會使用目前的工作目錄__。 在執行階段，已封裝的傳統型應用程式將不會取得您先前在桌面 .LNK 捷徑中指定的相同工作目錄。 如果您的應用程式必須擁有正確的目錄才能正確運作，則您需要在執行階段變更 CWD。

+ __您的應用程式需要 UIAccess__。 如果您的應用程式在 UAC 資訊清單的 `requestedExecutionLevel` 元素中指定 `UIAccess=true`，則目前不支援轉換為 UWP。 如需詳細資訊，請參閱 [UI 自動化安全性概觀](https://msdn.microsoft.com/library/ms742884.aspx)。

+ __您的應用程式公開 COM 物件__。 來自套件內的處理程序和延伸模組可以正常登錄及使用 COM 物件及 OLE 伺服器，並且在處理程序內和處理程序外 (OOP) 皆可以。  Creators Updates 新增了封裝 COM 的支援，提供了現在可在套件外部看見之 OOP COM 及 OLE 伺服器的登錄能力。  請參閱[適用於傳統型橋接器的 COM 伺服器和 OLE 文件支援](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97)。

   封裝 COM 支援與現有的 COM API 相容，但無法用於依賴直接讀取登錄檔的應用程式延伸模組，因為封裝 COM 是位於一處私人位置。

+ __您的應用程式公開 GAC 組件給其他處理程序使用__。 在目前版本中，您的應用程式不能公開 GAC 組件給來自 Windows 應用程式套件外部的可執行檔處理程序使用。 來自套件內的處理程序可以正常登錄及使用 GAC 組件，但無法從外部看見它們。 這表示如果透過外部處理程序叫用 OLE 等互通性案例，將無法作用。

+ __App 以不支援的方法連結 C 執行階段程式庫 (CRT)__。 Microsoft C/C++ 執行階段程式庫提供適用於 Microsoft Windows 作業系統的程式設計常式。 這些常式將許多常見但 C 和 C++ 語言未提供的程式設計工作自動化。 如果您的應用程式利用 C/C++ 執行階段程式庫，您必須確定以支援的方式連結它。

    Visual Studio 2017 支援同時用靜態和動態連結 (讓程式碼使用一般 DLL 檔案) 或用靜態連結 (將程式庫直接連結到程式碼中)，連結到目前的 CRT 版本。 可能的話，我們建議您的應用程式搭配 VS 2017 使用動態連結。

    支援先前的多種 Visual Studio 版本。 如需詳細資訊，請參閱下表：

    <table>
    <th>Visual Studio 版本</td><th>動態連結</th><th>靜態連結</th></th>
    <tr><td>2005 (VC 8)</td><td>不支援</td><td>支援</td>
    <tr><td>2008 (VC 9)</td><td>不支援</td><td>支援</td>
    <tr><td>2010 (VC 10)</td><td>支援</td><td>支援</td>
    <tr><td>2012 (VC 11)</td><td>支援</td><td>不支援</td>
    <tr><td>2013 (VC 12)</td><td>支援</td><td>不支援</td>
    <tr><td>2015 和 2017 (VC 14)</td><td>支援</td><td>支援</td>
    </table>

    注意：在任何情況下，您都必須連結到最新可公開取得的 CRT。

+ __您的 App 會從 Windows 並列資料夾安裝與載入組件__。 例如，您的 App 使用 C 執行階段程式庫 VC8 或 VC9 且從 Windows 並列資料夾動態連結它們，表示您的程式碼是使用來自共用資料夾的通用 DLL 檔。 不支援此連結方式。 您必須以靜態方式連結它們，方法是直接將可轉散發的程式庫檔案連結到您的程式碼。

+ __您的 app 使用 System32/SysWOW64 資料夾中的相依性__。 若要讓這些 DLL 能夠運作，您必須將它們包含於 Windows 應用程式套件的虛擬檔案系統部分。 這可確保 app 行為就如同已將 DLL 安裝在 **System32**/**SysWOW64** 資料夾中。 在套件的根目錄中，建立名為 **VFS** 的資料夾。 在該資料夾內，建立 **SystemX64** 和 **SystemX86** 資料夾。 然後，將 DLL 的 32 位元版本放置於 **SystemX86** 資料夾中，並將 64 位元版本放置於 **SystemX64** 資料夾中。

+ __您的 app 使用 VCLibs 架構套件__。 如果將 VCLibs 程式庫定義為 Windows 應用程式套件中的相依性，則可直接從 Microsoft Store 安裝它們。 例如，如果您的應用程式使用 Dev11 VCLibs 套件，請針對您的應用程式套件資訊清單進行下列變更：在`<Dependencies>`節點下方，新增：  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
從 Microsoft Store 進行安裝期間，將會在安裝 app 之前，先安裝 VCLibs 架構的適當版本 (x86 或 x64)。  
如果 app 是透過側載進行安裝，將不會安裝相依性。 若要在您的電腦上手動安裝相依性，您必須下載並安裝適用於傳統型橋接器的 VCLibs 架構套件。 如需這些案例的詳細資訊，請參閱[在 Centennial 專案中使用 Visual C++ 執行階段](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)。

  **架構套件**：

  * [適用於傳統型橋接器的 VC 14.0 架構套件](https://www.microsoft.com/download/details.aspx?id=53175)
  * [適用於傳統型橋接器的 VC 12.0 架構套件](https://www.microsoft.com/download/details.aspx?id=53176)
  * [適用於傳統型橋接器的 VC 11.0 架構套件](https://www.microsoft.com/download/details.aspx?id=53340)


+ __您的應用程式包含自訂捷徑清單__。 使用捷徑清單時有幾個問題和注意事項。

    - __您應用程式的架構與作業系統不相符。__  捷徑清單目前無法正常運作，如果應用程式和作業系統架構不相符 (例如 x86 應用程式在 x64 Windows 上執行)。 目前還沒有其他因應措施，除了重新編譯應用程式以符合架構之外。

    - __您的應用程式會建立捷徑清單項目，並呼叫 [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) 或 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__。 請不要透過程式設計方式在程式碼中設定 AppID。 這樣做會造成捷徑清單項目不出現。 如果您的應用程式需要自訂識別碼，請使用資訊清單檔案加以指定。 如需指示，請參考[手動封裝應用程式 (傳統型橋接器)](desktop-to-uwp-manual-conversion.md)。 應用程式的 AppID 指定於 *YOUR_PRAID_HERE* 區段中。

    - __應用程式會新增參考套件中可執行檔的捷徑清單 Shell 連結__。 您不能從捷徑清單直接啟動套件中的可執行檔 (應用程式專屬 .exe 的絕對路徑例外)。 請改為註冊應用程式執行別名 (可讓您的已封裝傳統型應用程式透過關鍵字啟動，就形同其在 PATH 上)，並改為設定別名的連結目標路徑。 如需有關如何使用 appExecutionAlias 延伸模組的詳細資訊，請參閱[整合您的應用程式與 Windows 10 (傳統型橋接器)](desktop-to-uwp-extensions.md)。 請注意，如果您想讓捷徑清單中連結的資產符合原始 .exe，則需要使用 [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) 以及含 PKEY_Title 的顯示名稱來設定資產 (例如圖示)，就像其他自訂項目一樣。

    - __您的應用程式會新增可依絕對路徑參考應用程式套件中資產的捷徑清單項目__。 更新應用程式套件時，應用程式的安裝路徑可能會變更，也會變更資產位置 (例如圖示、文件、可執行檔等)。 如果捷徑清單項目依絕對路徑參考這類資產，則應用程式應該定期重新整理其捷徑清單 (例如應用程式啟動時)，確保路徑解析正確。 或是改成使用 UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) API，讓您可以使用套件相關 ms-resource URI 配置 (也可感知語言、DPI 和高對比) 來參考字串和影像資產。

+ __您的應用程式會執行公用程式以執行工作__。 避免啟動命令公用程式，例如 PowerShell 和 Cmd.exe。 事實上，若使用者將您的應用程式安裝至執行 Windows 10 S 的系統上，您的應用程式將會完全無法啟動他們。 這可能會使得您的應用程式無法提交至 Microsoft Store，因為提交到 Microsoft Store 的所有應用程式都必須與 Windows 10 S 相容

啟動公用程式通常可以提供一個便利的方式來取得作業系統資訊、存取登錄，或存取系統功能。 然而，您也可以改用 UWP API 來完成這類工作。 這些 API 的效能是最好的，因為他們不需要個別的可執行檔即可執行，但重要的是，他們會禁止應用程式觸達套件的外部。 應用程式的設計與隔離、信任，以及來自傳統型橋接器應用程式的安全性維持一致。您的應用程式在執行 Windows 10 S 的系統上也將會如預期般運作。

+ __您的應用程式管理增益集、外掛程式或延伸模組__。   很多時候，只要延伸模組尚未封裝，且其本身是以完全信任的模式安裝的，COM 式的延伸模組都會繼續運作。 這是因為那些安裝程式可以使用其完全信任的功能修改登錄，並將延伸模組檔案放在您主機應用程式能找到他們的任何地方。

   然而，若那些延伸模組已封裝，且以 Windows 應用程式套件的方式安裝，他們將會無法正常運作，因為每一個套件 (主機應用程式及延伸模組) 都是互相隔離的。 若要深入了解傳統型橋接器如何將應用程式隔離於系統之外的相關資訊，請參閱[傳統型橋接器的幕後作業](desktop-to-uwp-behind-the-scenes.md)。

 所以使用者安裝至執行 Windows 10 S 系統的應用程式和延伸模組，都必須是以 Windows 應用程式套件進行安裝。 因此，若您想要封裝您的延伸模組，或您想要鼓勵您的參與者們進行封裝，建議您考慮如何協助主機應用程式套件和任何延伸模組套件之間的通訊。 要達到這個目的的其中一個可能的方式，就是使用[應用程式服務](../launch-resume/app-services.md)。

+ __您的應用程式會產生程式碼__。 您的應用程式可以產生可從記憶體中取用的程式碼，但請避免將產生的程式碼寫入磁碟，因為 Windows 應用程式認證程序無法在應用程式提交之前驗證該程式碼。 同時，將程式碼寫入磁碟的應用程式將不會在執行 Windows 10 S 的系統上正常運作。這可能會使得您的應用程式無法提交至 Microsoft Store，因為提交到 Microsoft Store 的所有應用程式都必須與 Windows 10 S 相容。

>[!IMPORTANT]
> 建立 Windows 應用程式套件之後，請測試您的應用程式以確定它可在執行 Windows 10 S 的系統上正確運作。提交到 Microsoft Store 的所有應用程式都必須與 Windows 10 S 相容。Microsoft Store 不接受不相容的應用程式。 請參閱[針對 Windows 10 S 測試您的 Windows 應用程式](desktop-to-uwp-test-windows-s.md)。

## <a name="next-steps"></a>後續步驟

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**為您的傳統型應用程式建立 Windows 應用程式套件**

請參閱[建立 Windows 應用程式套件](desktop-to-uwp-root.md#convert)