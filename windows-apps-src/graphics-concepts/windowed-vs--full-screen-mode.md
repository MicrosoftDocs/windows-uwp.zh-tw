---
title: "視窗與全螢幕模式"
description: "Direct3D 應用程式可以兩種模式執行：視窗或全螢幕模式。"
ms.assetid: EE8B9F87-822B-4576-A446-CA603E786862
keywords:
- "視窗與全螢幕模式"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 5c4e2cdb4fa001b66eec8556d02f307e7edcef42
ms.lasthandoff: 02/07/2017

---

# <a name="span-iddirect3dconceptswindowedvsfull-screenmodespanwindowed-vs-full-screen-mode"></a><span id="direct3dconcepts.windowed_vs__full-screen_mode"></span>視窗與全螢幕模式


Direct3D 應用程式可以兩種模式執行：視窗或全螢幕模式。 在*視窗模式*，應用程式與所有執行中的應用程式共用桌面螢幕可用空間。 在*全螢幕模式*，應用程式執行所在的視窗涵蓋整個桌面，隱藏所有執行的應用程式（包含您的開發環境）。 通常遊戲預設為全螢幕模式，隱藏執行的所有應用程式，為使用者提供身歷其境的遊戲體驗。

全螢幕模式和視窗模式之間的程式碼差異很小。

由於以全螢幕模式執行的應用程式會接管螢幕，偵錯應用程式需要其他監視器或使用遠端的偵錯工具。 視窗模式應用程式的其中一個優點是，您可以在偵錯工具中逐步執行程式碼，不需要多個監視器或遠端偵錯工具。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[裝置](devices.md)

 

 




