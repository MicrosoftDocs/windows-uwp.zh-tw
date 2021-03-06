---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: 傳統型裝置的 Windows 裝置入口網站
description: 了解 Windows 裝置入口網站如何在您的桌上型電腦上提供設定、診斷和自動化功能。
ms.date: 01/08/2021
ms.topic: article
ms.custom: contperf-fy21q3
keywords: windows 10, uwp, 裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 11bfc81f045887dfdbba9d380eedd6b9ff40709a
ms.sourcegitcommit: 02d220ef0ec0ecd7ed733086ba164ee9653d9602
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2021
ms.locfileid: "98055981"
---
# <a name="windows-device-portal-for-desktop"></a>傳統型裝置的 Windows 裝置入口網站

Windows 裝置入口網站 (WDP) 是裝置管理和偵錯工具，可讓您設定和管理裝置設定，並透過 HTTP 從網頁瀏覽器查看診斷資訊。 針對其他裝置上的 WDP 詳細資料，請參閱 [Windows 裝置入口網站概觀](device-portal.md)。

您可以針對下列各項使用 WDP：

- 管理裝置設定 (類似於 **Windows 設定** 應用程式)
- 請參閱和操作執行中處理程序清單
- 安裝、刪除、啟動和終止 app
- 變更 Wi-Fi 設定檔、檢視訊號強度和查看 ipconfig 詳細資料
- 檢視 CPU、記憶體、I/O、網路及 GPU 使用量的動態圖形
- 收集處理程序傾印
- 收集 ETW 追蹤
- 操作側載應用程式隔離的儲存裝置

## <a name="set-up-windows-device-portal-on-a-desktop-device"></a>在桌上型裝置上設定 Windows 裝置入口網站

### <a name="turn-on-developer-mode"></a>開啟開發人員模式

從 Windows 10 版本 1607 開始，某些適用於桌上型電腦的新功能只有在啟用開發人員模式時才提供。 如需有關如何啟用開發人員模式的資訊，請參閱[啟用您的裝置以用於開發](/windows/apps/get-started/enable-your-device-for-development)。

> [!IMPORTANT]
> 有時會因網路或相容性問題，致使開發人員模式無法正確安裝在您的裝置上。 如需協助進行這些問題的疑難排解，請參閱[相關的＜啟用您的裝置以用於開發＞一節](/windows/apps/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package)。

### <a name="turn-on-windows-device-portal"></a>開啟 Windows 裝置入口網站

您可以在 [設定] 的 [適用於開發人員] 區段中啟用 WDP。 啟用時，您也必須建立對應的使用者名稱與密碼。 請勿使用您的 Microsoft 帳戶或其他 Windows 認證。

![設定應用程式的 Windows 裝置入口網站區段](images/device-portal/device-portal-desk-settings.png)

一旦啟用 WDP 後，您會在區段的底端看見 Web 連結。 記下附加至所列 URL 結尾的連接埠號碼：啟用 WDP 時會隨機產生此號碼，但桌面重新開機時此號碼應保持一致。

這些連結提供兩種連線至 WDP 的方式：透過區域網路 (包括 VPN) 或透過本機主機。 連線之後，其外觀顯示如下：

![Windows 裝置入口網站](images/device-portal/device-portal-example.png)

### <a name="turn-off-windows-device-portal"></a>關閉 Windows 裝置入口網站

您可以在 [Windows 設定] 的 [適用於開發人員] 區段中停用 WDP。

### <a name="connect-to-windows-device-portal"></a>連線至 Windows 裝置入口網站

若要透過本機主機連線，請開啟瀏覽器視窗並輸入此處所顯示的其中一個 URI (根據您要使用的連線類型)。

- 本機主機：`http://127.0.0.1:<PORT>` 或 `http://localhost:<PORT>`
- 區域網路：`https://<IP address of the desktop>:<PORT>`

HTTPS 需要進行驗證和安全通訊。

如果您是在受保護的環境中使用 WDP (例如，在測試實驗室中)，而在此環境中您信任區域網路上的每個人、裝置上沒有個人資訊、且有獨特的需求，則您可以停用「驗證」。 這會啟用未加密的通訊，並允許具有您電腦 IP 位址的任何人進行連線並對其進行控制。

## <a name="windows-device-portal-content"></a>Windows 裝置入口網站內容

WDP 提供下列一組頁面。

- 應用程式管理員
- Xbox Live
- 檔案總管
- 執行中的處理序
- 效能
- 偵錯
- ETW (Windows 事件追蹤) 記錄
- 效能追蹤
- 裝置管理員
- Bluetooth
- 網路功能
- 當機資料
- 特性
- 混合實境
- 串流安裝偵錯工具
- Location
- 臨時

## <a name="using-windows-device-portal-to-test-and-debug-msix-apps"></a>使用 Windows 裝置入口網站來測試和偵錯 MSIX 應用程式


> [!VIDEO https://www.youtube.com/embed/PdgXeOMt4hk]


## <a name="more-windows-device-portal-options"></a>其他 Windows 裝置入口網站選項

### <a name="registry-based-configuration"></a>以登錄為基礎的設定

若您想要選取 WDP 的連接埠號碼 (例如 80 和 443)，則可設定下列登錄機碼︰

- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service` 底下
    - `UseDynamicPorts`：所需的 DWORD。 將此設為 0，以保留選擇的連接埠號碼。
    - `HttpPort`：所需的 DWORD。 包含 WDP 用來接聽 HTTP 連線的連接埠號碼。
    - `HttpsPort`：所需的 DWORD。 包含 WDP 用來接聽 HTTPS 連線的連接埠號碼。
    
在相同的 regkey 路徑底下，您也可以關閉驗證需求：
- `UseDefaultAuthorizer` - `0` 代表停用；`1` 代表啟用。  
    - 這可控制每個連線以及從 HTTP 重新導向至 HTTPS 的基本驗證要求。  
    
### <a name="command-line-options-for-windows-device-portal"></a>Windows 裝置入口網站的命令列選項

從系統管理命令提示字元中，您可以啟用及設定 WDP 的幾個部分。 若要查看您的組建所支援的最新命令集，您可以執行 `webmanagement /?`

- `sc start webmanagement` 或 `sc stop webmanagement`
    - 開啟或關閉服務。 這仍需要啟用開發人員模式。 
- `-Credentials <username> <password>` 
    - 設定 WDP 的使用者名稱和密碼。 使用者名稱必須符合基本驗證標準，因此不能包含冒號 (:) 且應使用標準 ASCII 字元建置，例如 [a-zA-Z0-9]，因為瀏覽器無法以標準方式剖析完整字元集。  
- `-DeleteSSL` 
    - 這樣會重設用於 HTTPS 連線的 SSL 憑證快取。 如果您遇到 TLS 連線錯誤，而無法略過 (與預期的憑證警告不同)，此選項可修正問題。 
- `-SetCert <pfxPath> <pfxPassword>`
    - 如需詳細資料，請參閱[使用自訂 SSL 憑證佈建 Windows 裝置入口網站](./device-portal-ssl.md)。  
    - 這可讓您安裝自己的 SSL 憑證，以修正通常在 WDP 中看見的 SSL 警告頁面。 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - 執行具有特定組態並且會顯示偵錯訊息的單機版 WDP。 這對於建置[封裝外掛程式](./device-portal-plugin.md)來說最實用。 
    - 如需如何以 System 身分執行此操作以完整測試封裝外掛程式的詳細資料，請參閱 [MSDN Magazine 文章](/archive/msdn-magazine/2017/october/windows-device-portal-write-a-windows-device-portal-packaged-plug-in)。

## <a name="troubleshooting"></a>疑難排解

以下是您在設定 Windows 裝置入口網站時可能會遇到的一些常見錯誤。

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch 傳回不正確更新數目 (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

當您嘗試在 Windows 10 的發行前版本上安裝開發人員套件時，可能會收到這個錯誤。 這些隨選功能 (FoD) 套件會裝載在 Windows Update 上，且若要在發行前版本的組建上下載它們，必須選擇在正式發行前進行小眾測試。 如果您的安裝未針對正確的組建和通道組合進行正式發行前的小眾測試，將無法下載承載。 請重複檢查下列各項：

1. 瀏覽至 [設定] > [更新與安全性] > [Windows 測試人員計畫]，確認 [Windows 測試人員帳戶] 區段有您的正確帳戶資訊。 如果您沒有看到該區段，請選取 [連結 Windows 測試人員帳戶]，新增您的電子郵件帳戶，並確認它顯示在 [Windows 測試人員帳戶] 標題下 (您可能需要選取 [連結 Windows 測試人員帳戶] 兩次才能實際連結新加入的帳戶)。
 
2. 在 [您要接收何種內容?] 底下，請確定已選取 [Windows 的實際開發]。
 
3. 在 [您要取得新組建的步調為何?]，請確定已選取 [Windows 測試人員快速]。
 
4. 您現在應該可以安裝 FoD 了。 如果您確認已選取 Windows 測試人員快速，但仍無法安裝 Fod，請提供意見反應，並附加 **C:\Windows\Logs\CBS** 下的記錄檔。

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC] StartService:OpenService FAILED 1060:指定的服務不是以已安裝的服務形式存在

如果沒有安裝開發人員套件，您可能會收到這個錯誤。 少了開發人員套件，就沒有 Web 管理服務。 請嘗試再次安裝開發人員套件。

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>CBS 無法開始下載，因為系統位於計量付費網路 (CBS_E_METERED_NETWORK)

如果您是使用計量付費網際網路連線，可能會出現此錯誤。 您無法在計量付費網路連線上下載開發人員套件。

## <a name="see-also"></a>另請參閱

* [Windows 裝置入口網站概觀](device-portal.md)
* [Windows 裝置入口網站核心 API 參考資料](./device-portal-api-core.md)