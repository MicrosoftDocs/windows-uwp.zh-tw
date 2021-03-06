---
ms.assetid: 0b891f63-02fa-4c30-b307-9fbcccac5caa
title: 裝置、感應器及電源
description: 為提供使用者豐富的使用經驗，您可能會發現有必要將外部裝置或感應器與您的應用程式整合。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4f2a23c1d6defb8c45c299f2abce3b05def2a26e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175452"
---
# <a name="devices-sensors-and-power"></a>裝置、感應器及電源


為提供使用者豐富的使用經驗，您可能會發現有必要將外部裝置或感應器與您的應用程式整合。 以下是一些功能範例，您可以使用本節所描述的技術將這些功能新增到您的應用程式中。

-   提供進階的列印經驗
-   將動作和方向感應器整合到您的遊戲中
-   直接或透過網路通訊協定連線到裝置

| 主題 | 描述 |
|-------|-------------|
| [啟用裝置功能](enable-device-capabilities.md) | 本教學課程描述如何在 Microsoft Visual Studio 中宣告裝置功能。 這可以讓您的應用程式使用相機、麥克風、定位感應器及其他裝置。 | 
| [以使用者模式存取 Windows IoT](enable-usermode-access.md) | 本教學課程描述如何在 Windows 10 IoT 核心版上以使用者模式存取 GPIO、I2C、SPI 及 UART。 |
| [列舉裝置](enumerate-devices.md) | 列舉命名空間可讓您尋找內部連接到系統、外部連接，或者可透過無線或網路通訊協定偵測到的裝置。 |
| [配對裝置](pair-devices.md) | 有些裝置在使用之前需要先進行配對。 [  <strong>Windows.Devices.Enumeration</strong>](/uwp/api/Windows.Devices.Enumeration) 命名空間支援三種不同方式來配對裝置。 |
| [服務點](point-of-service.md) | 本節說明如何與服務點週邊設備 (例如條碼掃描器、收據印表機、收銀機) 進行互動。 | 
| [感應器](sensors.md) | 感應器可讓 app 得知裝置與周遭實際環境之間的關係。 感應器會將裝置的方向、指向及動作告知您的 app。 |
| [Bluetooth](bluetooth.md) | 本節包含如何將藍牙整合至通用 Windows 平台 (UWP) 應用程式的文章，包括如何使用 RFCOMM、GATT 及低功耗 (LE) 廣告。 | 
| [列印與掃描](printing-and-scanning.md) | 本節描述如何從您的通用 Windows 應用程式列印與掃描。 | 
| [3D 列印](3d-printing.md) | 本節描述如何利用通用 Windows app 中的 3D 列印功能。 |
| [建立 NFC 智慧卡應用程式](host-card-emulation.md) | Windows Phone 8.1 使用以 SIM 卡為基礎的安全元素來支援 NFC 卡模擬應用程式，但該模型需要安全的付款應用程式才能與行動網路運算子 (MNO) 緊密結合。 這會限制其他未結合 MNO 的商家或開發人員所提供的各種可能付款解決方案， 在 Windows 10 行動裝置版中，我們已導入新的卡片模擬技術，稱為主機卡模擬 (HCE)。 HCE 技術可讓您的應用程式直接與 NFC 卡讀卡機進行通訊。 本主題說明主機卡模擬 (HCE) 在 Windows 10 行動裝置版裝置上的運作方式，以及如何開發 HCE 應用程式，讓您的客戶可以透過他們的手機而不是實體卡片存取您的服務，而不需要使用 MNO 共同作業。 |
| [取得電池資訊](get-battery-info.md) | 了解如何使用 [<strong>Windows.Devices.Power</strong>](/uwp/api/Windows.Devices.Power) 命名空間中的 API 取得詳細的電池資訊。 |