---
title: Xbox 裝置入口網站 REST API
description: 請參閱 Xbox 裝置入口網站 REST API 的參考主題清單，此清單可用來從遠端設定及管理您的主控台。
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: be8558daa39b126061caaa132fb134c8bdef15c1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172772"
---
# <a name="xbox-device-portal-rest-api"></a>Xbox 裝置入口網站 REST API

本節內容涵蓋 Xbox 裝置入口網站 REST API 的參考主題，用於從遠端設定及管理您的主控台。

| URI        | 說明 |
|------------|-------------|
|[/api/app/packagemanager/register](wdp-loose-folder-register-api.md)| 登錄包含在鬆散資料夾中的 App。 |
|[/api/app/packagemanager/upload](wdp-folder-upload.md)| 將整個資料夾上傳到主機。 |
|[/ext/app/sshpins](uwp-sshpins-api.md)| 從遠端清除所有信任的 SSH pin。 將需要再次針對 Visual Studio UWP 開發重新釘選配對。 |
|[/ext/app/deployinfo](uwp-deployinfo-api.md)| 對於一或多個安裝的套件要求部署資訊。 |
|[/ext/fiddler](wdp-fiddler-api.md)| 啟用和停用 Fiddler 網路追蹤。 |
|[/ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| 取得來自 Xbox 上焦點應用程式的 HTTP 流量。 |
|[/ext/networkcredential](uwp-networkcredentials-api.md)| 新增、移除或更新網路憑證。 |
|[/ext/remoteinput](uwp-remoteinput-api.md)| 從遠端傳送鍵盤和滑鼠或控制器輸入至 Xbox。 |
|[/ext/remoteinput/controllers](uwp-remoteinput-controllers-api.md)| 取得附加實體控制器數目，或關閉所有實體控制器。 |
|[/ext/screenshot](wdp-media-capture-api.md)| 擷取代表目前主機上顯示畫面的 PNG 檔案。 |
|[/ext/settings](wdp-xboxsettings-api.md)| 存取 Xbox One 開發人員設定。 |
|[/ext/smb/developerfolder](wdp-smb-api.md)| 在您的開發電腦上透過 [檔案總管] 存取您主機上的開發人員資料夾。 |
|[/ext/user](wdp-user-management.md)| 管理 Xbox One 主機上的使用者。 |
|[/ext/xbox/info](wdp-xboxinfo-api.md)| 提供有關 Xbox One 裝置的相關資訊。 |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| 管理您的 Xbox Live 沙箱。 |

## <a name="see-also"></a>另請參閱

- [Xbox One 上的 UWP](index.md)
- [Windows 裝置入口網站](../debug-test-perf/device-portal.md)