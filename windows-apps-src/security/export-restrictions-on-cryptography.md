---
title: 密碼編譯的出口限制
description: 使用這項資訊判斷您的 app 使用密碼編譯的方式，是否會使它無法被刊登於 Microsoft Store 中。
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 安全性
ms.localizationpriority: medium
ms.openlocfilehash: c647d91213ddf1fd8a3dafd80c6888a026cda576
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258941"
---
# <a name="export-restrictions-on-cryptography"></a>密碼編譯的出口限制



使用這項資訊判斷您的 app 使用密碼編譯的方式，是否會使它無法被刊登於 Microsoft Store 中。

美國商務部的工業安全局規定使用特定加密類型的技術出口事項。 Microsoft Store 列出的所有應用程式必須遵守這些法令規定，因為這些應用程式檔案可能會儲存在美國。 即使 app 開發人員從其他國家/地區上傳並在美國以外的地方發行的 app 也必須遵守這些法規。 因此，將 app 送出到 Microsoft Store 時，所有 app 開發人員必須確定他們的 app 中沒有包含這些法規管制使用的任何技術。

> **請注意**  此處提供的資訊會提供一些指引，但您會負責在 Microsoft Store 中發佈應用程式的應用程式開發人員，確保您的應用程式符合所有適用的法律和規定。

 

如需美國商務部與工業安全局的詳細資訊，請參閱[關於工業安全局](https://www.bis.doc.gov/about/index.htm)。

如需規範技術 (包含加密) 出口的出口管制條例 (EAR) 詳細資訊，請參閱 [EAR 管制使用密碼編譯的項目](https://www.bis.doc.gov/index.php/policy-guidance/encryption)。

## <a name="governed-uses"></a>受規範的使用方式

首先，請判斷您的 app 是否是使用受出口管制條例規範的密碼編譯類型。 問題包含以下清單中顯示的範例；但請記住，這份清單未詳列每種可能的密碼編譯應用方式。

> **重要**  請考慮您的應用程式所撰寫的程式碼，以及應用程式所包含或連結的所有軟體程式庫、公用程式和作業系統元件。

-   數位簽章的任何用途，例如驗證或完整性檢查
-   加密您的應用程式使用或存取的任何資料或檔案
-   金鑰管理、憑證管理或是與公開金鑰基礎結構互動的任何項目
-   使用安全通訊管道，例如 NTLM、Kerberos、安全通訊端層 (SSL) 或是傳輸層安全性 (TLS)
-   密碼或其他形式的資訊安全性加密
-   複製保護或數位版權管理 (DRM)
-   防毒保護

如需密碼編譯 app 的最新完整清單，請參閱[使用加密之項目的 EAR 控制項](https://www.bis.doc.gov/index.php/policy-guidance/encryption)。

## <a name="non-restricted-uses"></a>非限制的使用方式

請注意，某些密碼編譯的應用方式不會受到限制。 不受限制的工作如下：

-   密碼加密
-   複製保護
-   Authentication
-   數位版權管理
-   使用數位簽章

如需密碼編譯 app 的最新完整清單，請參閱[使用加密之項目的 EAR 控制項](https://www.bis.doc.gov/index.php/policy-guidance/encryption)。

如果您的 app 會呼叫、支援、包含或使用密碼編譯或加密來進行未列在此清單中的任何工作，則必須提供出口管制分類號碼 (ECCN)。

如果您沒有 ECCN，請參閱 [ECCN 問答集](https://www.bis.doc.gov/licensing/do_i_needaneccn.html)。
