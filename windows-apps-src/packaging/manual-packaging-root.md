---
author: laurenhughes
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: 手動封裝 App
description: 本節包含或連結至關於手動封裝通用 Windows 平台 (UWP) app 的文章。
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 封裝
ms.localizationpriority: medium
ms.openlocfilehash: 5f8a9c7f504584f64bb561a01170c965aa9d3983
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2018
ms.locfileid: "1815823"
---
# <a name="manual-app-packaging"></a>手動封裝 App

如果想要建立並簽署應用程式套件，但是沒有使用 Visual Studio 來開發 App 時，您必須使用手動 App 封裝工具。

> [!IMPORTANT] 
> 如果您使用 Visual Studio 來開發 App，建議您使用 Visual Studio 精靈建立並簽署應用程式套件。 如需詳細資訊，請參閱[使用 Visual Studio 封裝 UWP app](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)。

## <a name="purpose"></a>用途

本節包含或連結至關於手動封裝通用 Windows 平台 (UWP) app 的文章。

| 主題 | 描述 |
|-------|-------------|
| [使用 MakeAppx.exe 工具建立應用程式套件](create-app-package-with-makeappx-tool.md) | MakeAppx.exe 建立、加密、解密應用程式套件與套件組合，並從應用程式套件與套件組合中擷取檔案。 |
| [建立套件簽署的憑證](create-certificate-package-signing.md) | 使用 PowerShell 工具，建立和匯出應用程式套件簽署的憑證。 |
| [使用 SignTool 簽署應用程式套件](sign-app-package-using-signtool.md) | 使用 SignTool，手動簽署具憑證的應用程式套件。 |

### <a name="advanced-topics"></a>進階主題

這一節包含有關元件化大型和/或複雜應用程式以進行更有效率封裝和安裝的進階主題。 

> [!IMPORTANT]
> 如果您想要提交應用程式至 Microsoft Store，您需要連絡[Windows 開發人員支援](https://developer.microsoft.com/windows/support)，並取得核准使用本節中的任何進階功能。


| 主題 | 描述 |
|-------|-------------|
| [資產套件簡介](asset-packages.md) | 資產套件是一種套件，做為應用程式的常見檔案的集中位置 – 有效免除架構套件中的重複檔案。 |
| [使用資產套件與套件摺疊進行開發](package-folding.md) | 了解如何使用資產套件與套件摺疊有效地組織您的應用程式。 |
| [一般套件組合應用程式套件](flat-bundles.md) | 描述如何建立一般套件組合，使用應用程式套件的參照將應用程式的 .appx 套件檔案組合起來。 |
| [使用封裝配置的套件建立](packaging-layout.md) | 封裝配置是一份描述應用程式封裝結構的文件。 它會指定應用程式套件組合（主要和選用）、套件組合中的套件，和套件中的檔案。 |