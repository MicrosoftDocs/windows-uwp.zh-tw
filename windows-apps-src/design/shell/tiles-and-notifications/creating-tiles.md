---
author: mijacobs
Description: A tile is an app's representation on the Start menu. Every app has a tile. When you create a new Universal Windows Platform (UWP) app project in Microsoft Visual Studio, it includes a default tile that displays your app's name and logo.
title: 磚
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6bf07375b3ff27f87fb3f19db3f1f33c26c194e6
ms.sourcegitcommit: eead3c00b27d9f66f79ec08c81a97e91dc1fdb3c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2018
ms.locfileid: "1523137"
---
# <a name="tiles-for-uwp-apps"></a>適用於 UWP app 的磚

 

App 在 [開始] 功能表上以*磚*的形式顯示。 每個 app 都會有一個磚。 當您在 Microsoft Visual Studio 中建立新的通用 Windows 平台 (UWP) app 專案時，它會包含顯示 app 名稱和標誌的預設磚。 Windows 會在第一次安裝 app 時顯示這個磚。 安裝 app 之後，您可以透過通知變更磚的內容。例如，您可以變更磚以傳遞新的資訊 (例如新聞頭條或最新未讀郵件的主旨) 給使用者。

## <a name="configure-the-default-tile"></a>設定預設磚


在 Visual Studio 中建立新的專案時，它會建立一個顯示 app 名稱和標誌的簡單預設磚。

若要編輯磚，請按兩下主要 UWP 專案中的 **Package.appxmanifest** 檔案，以開啟設計工具 (或在檔案上按一下滑鼠右鍵，並選取 \[檢視程式碼\])。

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Square150x150Logo.png"
        Square44x44Logo="Assets\Square44x44Logo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

以下是您應該更新的幾個項目。

-   DisplayName：使用您想要在磚上顯示的名稱來取代此值。
-   ShortName：因為磚上可容納顯示名稱的空間有限，建議您另外指定 ShortName，以確保您的 app 名稱不會被截斷。
-   標誌影像：

    您應該以自己的影像取代這些影像。 您可以選擇為不同的視覺比例提供影像，但不需全部提供。 若要確保您的 app 在各種裝置上有很好的顯示效果，我們建議您提供每個影像的 100%、200% 及 400% 比例版本。 請參閱[磚和圖示資產](app-assets.md)，以深入了解如何產生這些資產。

    縮放影像按照以下命名慣例：
    
    *&lt;影像名稱&gt;*.scale-*&lt;縮放比&gt;*.*&lt;影像檔案副檔名&gt;* 

    例如：SplashScreen.scale-100.png

    參考影像時，您將以 *&lt;影像名稱&gt;*.*&lt;影像檔案副檔名&gt;* 的格式來參考它 (在此範例中為 "SplashScreen.png")。 系統會從您提供的影像中，為裝置自動選取適當的縮放影像。

-   您不需要 (但強烈建議您) 提供適用於寬形磚和大型磚大小的標誌，方便使用者可以將 App 的磚調整成那些尺寸。 若要提供這些額外的影像，您可以建立 **DefaultTile** 元素，並使用 **Wide310x150Logo** 和 **Square310x310Logo** 屬性來指定其他影像：
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Square150x150Logo.png"
            Square44x44Logo="Assets\Square44x44Logo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\Wide310x150Logo.png"
              Square310x310Logo="Assets\Square310x310Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <a name="use-notifications-to-customize-your-tile"></a>使用通知來自訂磚


安裝 App 後，您可以使用通知來自訂磚。 您可以在第一次啟動應用程式或在回應事件 (例如推播通知) 時進行這個動作。

若要了解如何傳送磚通知，請參閱[傳送本機磚通知](sending-a-local-tile-notification.md)。