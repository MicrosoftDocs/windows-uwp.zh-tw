---
title: 新增啟動顯示畫面
description: 使用 Microsoft Visual Studio 設定應用程式的啟動顯示畫面影像與背景色彩。
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
ms.date: 05/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 06f9d412976ab9ccd5af5c8108bd5632792e4112
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167912"
---
# <a name="add-a-splash-screen"></a>新增啟動顯示畫面

使用 Microsoft Visual Studio 設定應用程式的啟動顯示畫面影像與背景色彩。

## <a name="set-the-splash-screen-image-and-background-color-in-visual-studio"></a>在 Visual Studio 中設定啟動顯示畫面影像與背景色彩

當您使用 Visual Studio 範本建立應用程式時，會將預設影像新增到您的專案，並設定為啟動顯示畫面影像。 啟動顯示畫面的背景色彩預設為淺灰。 如果您想變更 app 啟動顯示畫面的預設影像或色彩，請遵循以下步驟：

1. 開啟 Visual Studio 中的現有通用 Windows 平台 (UWP) 應用程式專案。
2. 從 **\[方案總管\]** 中，開啟「Package.appxmanifest」檔案。 您也可以從功能表列中，依序選擇 **\[專案\]** &gt; **\[市集\]** &gt; **\[編輯應用程式資訊清單\]** 來開啟這個檔案。
3. 開啟 **\[視覺資產\]** 索引標籤，然後從「Package.appxmanifest」視窗左側的 **\[所有視覺資產\]** 窗格中選取 **\[啟動顯示畫面\]**。 如果您是第一次變更啟動顯示畫面，您將會 \\ 在 **啟動顯示畫面** 欄位中看到「資產SplashScreen.png」路徑。

    以下螢幕擷取畫面顯示 Visual Studio 中的「Package.appxmanifest」視窗。 根據專案類型，您會看到一組稍有不同的視覺資產。

    ![Visual Studio 2019 中 [package.appxmanifest] 視窗的螢幕擷取畫面](images/appmanifest.png)

    如果您在文字編輯器中開啟 "package.appxmanifest"， [**SplashScreen 元素**](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen) 會顯示為 [**v s 元素**](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements)的子系。 在文字編輯器中，資訊清單檔中的預設啟動顯示畫面標記看起來像這樣：

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4. 若要為 UWP app 選取新的啟動顯示畫面影像，請按顯示在 **\[縮放的資產\]** 下方 **\[1240 x 600 px\]** 標籤旁含有省略符號的按鈕。 選擇要當作啟動顯示畫面影像的 1240 x 600 像素影像 (.png、.jpg 或 .jpeg)。

    **重要**   您所選擇的啟動顯示畫面影像必須是 620 x 300 圖元（使用1x 的縮放比例）。 此外，設計您的啟動顯示畫面時，請注意它將小於螢幕，並且置中對齊。 它並不像 Windows Phone 市集應用程式的啟動顯示畫面會填滿螢幕。

5. 若要為 Windows Phone 市集應用程式選取新的啟動顯示畫面影像，請按 **\[縮放的資產\]** 下方顯示在 **\[1152 x 1920 px\]** 標籤旁具有省略符號的按鈕。 選擇要當作啟動顯示畫面影像的 1152 x 1920 像素影像 (.png、.jpg 或 .jpeg)。

    **重要**   您所選擇的啟動顯示畫面影像必須是 1152 x 1920 圖元，也就是 2.4 x 調整比例的正確大小。 如果這是您所提供的唯一資產，它將會針對 1.4x 和 1x 的縮放比例縮小。

6. 在 **\[啟動顯示畫面\]** 區段的 **\[背景色彩\]** 欄位中，設定與您的啟動顯示畫面影像一起顯示的背景色彩。 您可以輸入色彩的名稱或 ' \# '，以及色彩的十六進位值。 如需可用色彩的名稱清單，請參閱 [**SplashScreen element**](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen)。 您不一定要設定啟動顯示畫面的背景色彩。 如果您未指定 UWP 應用程式的色彩，則啟動顯示畫面的背景色彩會預設為淺灰色 (十六進位值 \# 464646) 。 這個色彩與預設的 **\[磚\]** 背景色彩相同 (請參閱 **\[視覺資產\]** 索引標籤中 **\[磚影像和標誌\]** 區段的 **\[背景色彩\]** 欄位)。 如果您沒有為 Windows Phone 指定色彩，或是將它設定成「透明」，啟動顯示畫面背景色彩就會是透明的。

## <a name="summary-and-next-steps"></a>摘要和後續步驟

如果您的 app 需要一些時間載入，可考慮加入延長式啟動顯示畫面。 如需逐步指引，請參閱[建立自訂的啟動顯示畫面](create-a-customized-splash-screen.md)。

## <a name="related-topics"></a>相關主題

* [建立自訂的啟動顯示畫面](create-a-customized-splash-screen.md)
* [封裝資訊清單結構描述參考：SplashScreen 元素](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen)
* [Windows.ApplicationModel.Activation.SplashScreen 類別](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)