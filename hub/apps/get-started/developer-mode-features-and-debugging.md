---
title: 開發人員模式功能和調試
description: 瞭解 Windows 10 中的「開發人員模式」功能的詳細資訊，並資訊安裝錯誤。
keywords: 開始使用開發人員授權 Visual Studio, 開發人員授權啟用裝置
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 85c64cd4baed7be9edde0dfc008ab90c1d537ca4
ms.sourcegitcommit: d0eef123b167dc63f482a9f4432a237c1c6212db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077226"
---
# <a name="developer-mode-features-and-debugging"></a>開發人員模式功能和調試

如果您只想要在應用程式上安裝開發人員模式的基本概念，您可以依照 [啟用裝置以進行開發](enable-your-device-for-development.md) 時所述的指示，開始使用。 本文涵蓋開發人員模式的 advanced 功能、舊版 Windows 10 的開發人員模式，以及開發人員模式安裝中的偵測失敗。

## <a name="additional-developer-mode-features"></a>其他開發人員模式功能

每個裝置系列可能都會有額外的開發人員功能。 只有已在裝置上啟用 [開發人員模式] 時，才可使用這些功能，而且可能依 OS 版本而不同。

下圖顯示適用於 Windows 10 的開發人員功能：

![開發人員模式選項](images/devmode-mob-options.png)

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>裝置入口網站

若要深入了解裝置入口網站，請參閱 [Windows 裝置入口網站概觀](/windows/uwp/debug-test-perf/device-portal.md)。


如需裝置特定的安裝指示，請參閱︰
- [傳統型裝置的裝置入口網站](/windows/uwp/debug-test-perf/device-portal-desktop)
- [HoloLens 的裝置入口網站](/windows/mixed-reality/using-the-windows-device-portal)
- [IoT 的裝置入口網站](/windows/iot-core/manage-your-device/deviceportal)
- [行動裝置的行動裝置](/windows/uwp/debug-test-perf/device-portal-mobile)
- [Xbox 的裝置入口網站](/windows/uwp/xbox-apps/device-portal-xbox)

如果您遇到啟用開發人員模式或裝置入口網站方面的問題，請參閱[已知問題](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)論壇，以尋找這些問題的因應措施，或造訪[無法安裝開發人員模式套件或啟動裝置入口網站](#failure-to-install-developer-mode-package)，以取得其他詳細資訊及了解要允許哪些 WSUS KB 才能將開發人員模式套件解除封鎖。

### <a name="sideload-apps"></a>側載 app

> [!IMPORTANT]
> 從最新的 Windows 10 更新起，此設定將不會顯示，因為預設會啟用側載。 如果您使用舊版的 Windows 10，則預設設定只會允許您執行來自 Microsoft Store 的應用程式，而且必須啟用側載才能安裝來自非 Microsoft 來源的應用程式。

通常是需要在受管理裝置上安裝自訂應用程式而不透過 Microsoft Store 的公司或學校，或是需要執行的應用程式並非來自 Microsoft 的人員，才會使用 [側載應用程式] 設定。 在此案例中，組織強制執行會停用「UWP 應用程式」設定的原則相當常見，如先前設定頁面的圖中所示。 組織也會提供側載應用程式所需的憑證和安裝位置。 如需詳細資訊，請參閱 TechNet 文章[在 Windows 10 中側載應用程式](/windows/deploy/sideload-apps-in-windows-10)和[在 Microsoft Intune 基本概念](/mem/intune/fundamentals/)。

裝置系列特定的資訊

-   在傳統型裝置系列上：您可以執行使用套件 ("Add-AppDevPackage.ps1") 建立的 Windows PowerShell 指令碼，來安裝應用程式套件 (.appx) 以及執行該應用程式所需的任何憑證。 如需詳細資訊，請參閱[封裝 UWP app](/windows/msix/package/packaging-uwp-apps)。

-   在行動裝置系列上：如果已經安裝所需的憑證，您便可以點選檔案來安裝任何透過電子郵件傳送給您或位於 SD 記憶卡上的 .appx。


因為您無法在不具受信任憑證的裝置上安裝應用程式，所以 [側載應用程式] 會是比 [開發人員模式] 更安全的選項。

> [!NOTE]
> 如果您要側載應用程式，您應該仍然只安裝來自受信任來源的應用程式。 當您安裝未經 Microsoft Store 認證的側載應用程式時，即表示您同意您已具備側載該應用程式所需的一切權限，並且為安裝和執行該應用程式所造成的任何損害負全責。 請參閱這份[隱私權聲明](https://privacy.microsoft.com/privacystatement)的 Windows &gt; Microsoft Store 小節。


### <a name="ssh"></a>SSH

當您在裝置上啟用 [裝置探索] 時，系統會啟用 SSH 服務。  當您的裝置是 UWP 應用程式的遠端部署目標時，就會用到此服務。   服務的名稱是 'SSH Server Broker' 和 'SSH Server Proxy'。

> [!NOTE]
> 這不是 Microsoft 的 OpenSSH 實作 (可在 [GitHub](https://github.com/PowerShell/Win32-OpenSSH) 中找到)。  

為了利用 SSH 服務，您可以啟用裝置探索來允許 PIN 配對。 如果您想要執行另一個 SSH 服務，您可以在不同的連接埠上進行設定，或是關閉 [開發人員模式] SSH 服務。 若要關閉 SSH 服務，請關閉 [裝置探索]。  

SSH 登入透過 "DevToolsUser" 帳戶完成，可接受密碼用以驗證。  這個密碼就是在按下裝置探索的 [配對] 按鈕之後，在裝置上顯示的 PIN 碼，而且僅在 PIN 碼顯示期間才有效。  對於手動管理的 DevelopmentFiles 資料夾，其中從 Visual studio 安裝檔案鬆散部署，也會啟用 SFTP 子系統。

#### <a name="caveats-for-ssh-usage"></a>SSH 使用方式的注意事項
在 Windows 所用的現有 SSH 伺服器尚未與通訊協定相容，因此使用 SFTP 或 SSH 用戶端可能需要特殊的設定。  尤其是 SFTP 子系統執行第 3 版或更舊版本，因此任何連接用戶端都應設定為預期使用舊的伺服器。  舊款裝置上的 SSH 伺服器使用 `ssh-dss` 來進行公開金鑰驗證，而 OpenSSH 已棄用此項目。  若要連接至這類裝置，必須將 SSH 用戶端手動設定為接受 `ssh-dss`。  

### <a name="device-discovery"></a>裝置探索

當您啟用裝置探索時，會讓網路上的其他裝置能夠透過 mDNS 看見您的裝置。  此功能也可讓您取得在按下啟用裝置探索後顯示的 [配對] 按鈕時，用來配對此裝置的 SSH PIN。  這個 PIN 提示字元必須顯示在螢幕上，才能完成您第一個以此裝置為目標的 Visual Studio 部署。  

![PIN 配對](images/devmode-pc-pinpair.PNG)

您應該只有在您想要讓裝置成為部署目標時，才啟用裝置探索。 例如，如果您使用 Device Portal 將 App 部署到手機來進行測試，您就必須在該手機上啟用裝置探索，而不是在您的開發電腦上啟用。

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Windows Explorer、遠端桌面及 PowerShell 的最佳化 (僅限傳統型裝置)

 在傳統型裝置系列上，[開發人員專用] 設定頁面含有您可用來將電腦最佳化以進行開發工作的設定捷徑。 針對每個設定，您都可以選取核取方塊並按一下 [套用]，或按一下 [顯示設定] 連結來開啟該選項的設定頁面。


## <a name="notes"></a>附註
在舊版 Windows 10 行動裝置版中，[損毀傾印] 選項顯示在 [開發人員設定] 功能表中。  此選項已移至[裝置入口網站](/windows/uwp/debug-test-perf/device-portal.md)，因此可以從遠端使用，而不只是透過 USB。  

有數個工具可讓您用來將應用程式從 Windows 10 電腦部署到 Windows 10 裝置。 這兩個裝置都必須透過有線或無線連線連接到網路的同一個子網路，或者必須透過 USB 來連接它們。 列出的方法都只會安裝應用程式套件 (.appx/.appxbundle)；它們不會安裝憑證。

-   使用 Windows 10 應用程式部署 (WinAppDeployCmd) 工具。 深入了解 [WinAppDeployCmd 工具](/previous-versions/windows/apps/mt203806(v=vs.140))。
-   您可以使用[裝置入口網站](/windows/uwp/debug-test-perf/device-portal.md)，從您的瀏覽器部署到執行 Windows 10 版本 1511 或更新版本的行動裝置。 使用 Device Portal 中的 **[App](/windows/uwp/debug-test-perf/device-portal.md#apps-manager)** 頁面來上傳應用程式套件 (.appx)，並將它安裝在裝置上。

## <a name="failure-to-install-developer-mode-package"></a>無法安裝開發人員模式套件
有時會因網路或系統管理方面的問題，致使開發人員模式無法正確安裝。 必須有開發人員模式套件，才能 **遠端** 部署至此電腦上 (從瀏覽器使用裝置入口網站或使用 [裝置探索] 來啟用 SSH)，但本機開發則不需要。  即使您遇到這些問題，您仍可使用 Visual Studio 在本機部署您的應用程式，或從此裝置部署到另一個裝置。

若要尋找這些問題及其他問題的因應措施，請參閱[已知問題](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)。

> [!NOTE]
> 如果不正確地安裝開發人員模式下，我們鼓勵您提出的意見反應要求。 在 [意見反應中樞] 應用程式中，選取 [新增意見反應]，然後選擇 [開發人員平台] 類別和 [開發人員模式] 子類別。 提交意見反應將協助 Microsoft 解決您遇到的問題。

### <a name="failed-to-locate-the-package"></a>找不到套件

「在 Windows Update 中找不到開發人員模式套件。 錯誤程式碼 0x80004005 深入了解」   

發生此錯誤的原因，可能在於網路連線問題、企業設定或是套件已遺失。

解決此問題：

1. 確認您的電腦是否已連線至網際網路。
2. 若您位於加入網域的電腦上，請說出您的網路系統管理員。 在 WSUS 中預設會封鎖開發人員模式套件，如所有的「功能隨選安裝」。
2.1. 為了解除封鎖目前與先前版本中的開發人員模式套件，在 WSUS 中應允許下列 KB：4016509、3180030、3197985  
3. 在 [設定] > [更新與安全性] > [Windows Updates] 中，檢查 Windows 更新。
4. 確認 Windows 開發人員模式套件顯示於 [設定] > [系統] > [應用程式與功能] > [管理選用功能] > [新增功能]。 若該套件遺失，Windows 即無法找到適用於您電腦的正確套件。

執行上述步驟之後，停用和重新啟用開發人員模式確認已修正問題。


### <a name="failed-to-install-the-package"></a>無法安裝套件

「無法安裝開發人員模式套件。 錯誤程式碼 0x80004005 深入了解」

由於您的 Windows 組建與開發人員模式套件不相容，因此發生此錯誤。

解決此問題：

1. 在 [設定] > [更新與安全性] > [Windows Updates] 中，檢查 Windows 更新。
2. 重新啟動電腦，以確保套用所有的更新。


## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>使用群組原則或登錄機碼來啟用裝置

對大多數開發人員來說，您會想要使用 [設定] App 來啟用您的裝置以進行偵錯。 在某些案例 (例如自動化測試) 中，您可以使用其他方法來啟用您的 Windows 10 傳統型裝置以進行開發。  請注意，下列步驟不會啟用 SSH 伺服器或允許遠端部署及偵錯將裝置視為目標。

除非您擁有的是「Windows 10 家用版」，否則請使用 gpedit.msc 來設定群組原則以啟用您的裝置。 如果您確實擁有「Windows 10 家用版」，則需要使用 regedit 或 PowerShell 命令來直接設定登錄機碼，以啟用您的裝置。

**使用 gpedit 啟用您的裝置**

1.  執行 **Gpedit.msc**。
2.  移至 [本機電腦原則] &gt; [電腦設定] &gt; [系統管理範本] &gt; [Windows 元件] &gt; [應用程式套件部署]
3.  若要啟用側載功能，請編輯原則來啟用：

    -   **允許安裝所有受信任的應用程式**

    或者

    若要啟用開發人員模式，請編輯原則來啟用這兩者：

    -   **允許安裝所有受信任的應用程式**
    -   **允許開發 UWP 應用程式，並從整合式開發環境 (IDE) 安裝它們**

4.  將電腦重新開機。

**使用 regedit 啟用您的裝置**

1.  執行 **regedit**。
2.  若要啟用側載功能，請將此 DWORD 的值設定為 1：

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowAllTrustedApps`

    或者

    若要啟用開發人員模式，請將此 DWORD 的值設定為 1：

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowDevelopmentWithoutDevLicense`

**使用 PowerShell 啟用您的裝置**

1.  使用系統管理員權限執行 PowerShell。
2.  若要啟用側載功能，請執行下列命令：

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowAllTrustedApps" /d "1"
    ```

    或者

    若要啟用開發人員模式，請執行下列命令：

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"
    ```

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>將裝置從 Windows 8.1 升級至 Windows 10

在 Windows 8.1 裝置上建立或側載應用程式時，您必須安裝開發人員授權。 如果您將裝置從 Windows 8.1 升級為 Windows 10，則會保留此資訊。 執行下列命令，從升級後的 Windows 10 裝置移除此資訊。 如果您直接從 Windows 8.1 升級為 Windows 10 版本 1511 或更新版本，就不需要執行此步驟。

**取消註冊開發人員授權**

1.  使用系統管理員權限執行 PowerShell。
2.  執行此命令︰`unregister-windowsdeveloperlicense`。

在這之後，您需要啟用裝置來進行開發 (如本主題所述)，讓您能夠繼續在此裝置上進行開發。 如果您沒有這樣做，在偵錯您的 app 或嘗試為它建立套件時可能會收到錯誤。 以下是此錯誤的範例：

錯誤：DEP0700：DEP0700：應用程式的註冊失敗。