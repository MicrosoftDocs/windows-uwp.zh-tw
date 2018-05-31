---
author: Karl-Bridge-Microsoft
Description: User interactions in the Universal Windows Platform (UWP) are a combination of input and output sources (such as mouse, keyboard, pen, touch, touchpad, speech, Cortana, controller, gesture, gaze, and so on), along with various modes or modifiers that enable extended experiences (including mouse wheel and buttons, pen eraser and barrel buttons, touch keyboard, and background app services).
title: 互動基本資訊
ms.assetid: 73008F80-FE62-457D-BAEC-412ED6BAB0C8
label: Interaction primer
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 910920c1f5eb5bdc3e55b51d7886be1632559c14
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
ms.locfileid: "1396607"
---
# <a name="interaction-primer"></a>互動基本資訊


![Windows 輸入類型](images/input-interactions/icons-inputdevices03.png)

通用 Windows 平台 (UWP) 中的使用者介面是輸入與輸出來源的組合 (例如，滑鼠、鍵盤、手寫筆、觸控、觸控板、語音、**Cortana**、控制器、手勢、注視等)，以及啟用延伸體驗的各種不同模式或輔助按鍵 (包括滑鼠滾輪和按鈕、手寫筆橡皮擦和筆身按鈕、觸控式鍵盤及背景應用程式服務)。

UWP 會使用「智慧型」且與內容相關的互動系統，在大部分情況下，就不需要個別處理您應用程式所接收的獨特輸入類型。 這包含了做為一般指標類型的處理觸控、觸控板、滑鼠和手寫筆輸入，以支援靜態手勢 (例如點選或長按)、操作手勢 (例如滑動進行移動瀏覽，或呈現數位筆跡)。

與特定尺寸規格配對使用時，請熟悉各個輸入裝置類型及其行為、功能與限制。 這可以協助您判斷您的應用程式是否有足夠的平台控制項與能供性，或者需要您提供自訂的互動體驗。

## <a name="surface-dial"></a>Surface Dial

在 Windows10 年度更新版中，我們引進了新的輸入裝置類別，稱為 Windows Wheel。 Surface Dial 是第一個這類裝置。 

### <a name="device-support"></a>裝置支援

-   平板電腦
-   電腦和膝上型電腦

### <a name="typical-usage"></a>一般使用方式

Surface Dial 使用根據旋轉動作 (或手勢) 的形狀規格，做為次要、多重強制回應的輸入裝置，補充或修改主要裝置的輸入。 在大部分情況下，使用者是以慣用手執行工作 (例如以手寫筆寫字)，同時以非慣用手操作這類裝置。

### <a name="more-info"></a>其他資訊

[Surface Dial 設計指導方針](windows-wheel-interactions.md)


## <a name="cortana"></a>Cortana

在 Windows10 中，**Cortana** 擴充性可讓您處理使用者的語音命令，並啟動您的應用程式來執行單一動作。

### <a name="device-support"></a>裝置支援

-   手機和平板手機
-   平板電腦
-   電腦和膝上型電腦
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![cortana](images/input-interactions/icons-cortana01.png)

### <a name="typical-usage"></a>一般使用方式

語音命令會定義於語音命令定義 (VCD) 檔中，它是一種單次語言表達，會透過 **Cortana** 導向已安裝的應用程式。 根據互動的層次和複雜性，您可以在前景或背景啟動這個應用程式。 例如，某些語音命需要參考前後文或者需要使用者輸入資料，那麼在前景進行處理最合適；而基本的命令，則可以在背景進行處理。

整合應用程式的基本功能，並提供一個中心進入點，讓使用者不需要開啟應用程式就能完成大部分的工作，這讓 **Cortana** 成為您的應用程式與使用者之間的橋樑。 在多數情況下，這可以為使用者節省很多時間和精力。 如需詳細資訊，請參閱 [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)。

### <a name="more-info"></a>其他資訊

[Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)
 

## <a name="speech"></a>語音

語音是使用者可用來與應用程式互動的有效且自然的方式。 這是與應用程式通訊的簡單且精確的方式，並讓使用者能夠提高生產力，且在各種不同的情況下收到通知。

根據使用者的裝置，語音可以做為補充項，或者，在許多情況下，可以是主要的輸入類型。 例如，像是 HoloLens 和 XBox 的裝置不支援傳統的輸入類型 (但特定案例中的軟體鍵盤除外)。 相反地，它們依賴語音輸入和輸出 (通常會與其他非傳統的輸入類型相結合，例如注視和手勢) 來進行大部分的使用者互動。

文字轉換語音 (也稱為 TTS 或語音合成) 可用來通知或引導使用者。

### <a name="device-support"></a>裝置支援

-   手機和平板手機
-   平板電腦
-   電腦和膝上型電腦
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![語音](images/input-interactions/icons-speech01.png)

### <a name="typical-usage"></a>一般使用方式

有三種語音互動的模式：

**自然語言**

自然語言是我們平常口頭上與人們互動的方式。 我們的語音會根據對象及狀況而不同，且通常能夠理解。 如果不是，我們通常會使用不同的文字和文字順序，來取得其中的相同概念。

與應用程式的自然語言互動類似：我們透過裝置對應用程式說話，就像裝置是人一樣，並預期它會了解並據以回應。

自然語言是最先進的語音互動模式，可透過 **Cortana** 來實作和公開。

**命令和控制項**

命令和控制項就是使用口頭命令來啟動控制項和功能，例如，按一下按鈕或選取功能表項目。

由於命令和控制項是成功使用者經驗的關鍵，所以通常不建議使用單一輸入類型。 根據使用者的喜好設定或硬體功能，語音對使用者而言通常是數種輸入選項的其中一個。

**聽寫**

最基本的語音輸入方法。 每次說話都會轉換成文字。

當應用程式不需要了解意義或意圖時，通常會使用聽寫。

### <a name="more-info"></a>其他資訊

[語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)
 

## <a name="pen"></a>手寫筆

手寫筆可以當做像素精確指標裝置 (例如滑鼠)，而且是適用於數位筆跡輸入的最佳裝置。

**注意**  有兩種類型的手寫筆裝置：主動式及被動式。
  -   被動式手寫筆不包含電子產品，可有效地模擬來自手指的觸控輸入。 它們需要基本的裝置顯示器，根據接觸點的壓力來辨識輸入。 由於使用者在輸入介面上書寫時通常會將手擱在上面，因此，輸入資料會因為未順利防手掌誤觸而受到干擾。
  -   主動式手寫筆包含電子產品，而且可與複雜的裝置顯示器一起使用，為系統和應用程式提供更廣泛的輸入資料 (包括暫留或鄰近性資料)。 防手掌誤觸的功能更加強固。

當我們在此處提到手寫筆裝置時，指的是可提供豐富的輸入資料且主要用於精確筆跡和指標互動的主動式手寫筆。

### <a name="device-support"></a>裝置支援

-   手機和平板手機
-   平板電腦
-   電腦和膝上型電腦
-   Surface Hub
-   IoT

![手寫筆](images/input-interactions/icons-pen01.png)

### <a name="typical-usage"></a>一般使用方式

Windows 筆跡平台搭配手寫筆之後，使用者就可以自然的方式手寫筆記、繪圖以及註解。 這個平台支援擷取數位板輸入的筆跡資料、產生筆跡資料、將資料轉譯成輸出裝置上的筆跡筆觸、管理筆跡資料，以及執行手寫辨識。 當使用者書寫或畫圖時，除了感應手寫筆的空間移動外，應用程式還可以收集各種資訊，例如壓力、形狀、顏色以及不透明度，讓使用者感受如同用鋼筆、鉛筆或筆刷在紙上繪圖一樣。

手寫筆和觸控輸入的區別在於觸控能夠透過在這些物件上實際運用手勢 (如撥動、滑動、拖曳、旋轉等等)，在畫面上模擬對 UI 元素的直接操作。

您應該提供手寫筆特定的 UI 命令或能供性來支援這些互動。 例如，使用上一個和下一個 (或是 + 與 -) 按鈕讓使用者翻頁內容，或旋轉、調整物件大小，以及縮放物件。

### <a name="more-info"></a>其他資訊

[手寫筆設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn456352)
 

## <a name="touch"></a>觸控

利用觸控功能，一或多隻手指的實際手勢就能用來模擬直接操作 UI 元素 (例如移動瀏覽、旋轉、調整大小或移動)、做為替代輸入法 (類似滑鼠或手寫筆)，或做為互補輸入法 (修改其他輸入法的外觀比例，例如手寫筆繪製之筆跡筆觸的汙點部分)。 類似這樣的觸覺經驗可以在使用者與螢幕上的元素互動時，提供更為自然、貼近真實世界的感覺。

### <a name="device-support"></a>裝置支援

-   手機和平板手機
-   平板電腦
-   電腦和膝上型電腦
-   Surface Hub
-   IoT

![觸控](images/input-interactions/icons-touch01.png)

### <a name="typical-usage"></a>一般使用方式

針對觸控輸入的支援可能會根據裝置的不同而有顯著差異。

有些裝置並未完全支援觸控、有些裝置支援單點觸控點，其他的裝置則支援多點觸控 (兩個或多個接觸點)。

大多數支援多點觸控輸入的裝置通常可辨識十個獨特的並行處理接觸點。

Surface Hub 裝置可辨識 100 個獨特的並行處理觸控點。

一般而言，觸控是：

-   單一使用者，除非是和像是 Surface Hub 的 Microsoft 小組裝置搭配使用，否則就會強調共同作業。
-   不受限於裝置方向。
-   用於所有互動，包括文字輸入 (觸控式鍵盤) 和筆跡 (應用程式設定)。

### <a name="more-info"></a>其他資訊

[觸控設計指導方針](https://msdn.microsoft.com/library/windows/apps/hh465370)
 

## <a name="touchpad"></a>觸控板

觸控板結合了間接多點觸控輸入與指標裝置 (如滑鼠) 精確輸入。 這項結合讓觸控板既適用於觸控最佳化 UI，也適用於較小的生產力應用程式目標。

### <a name="device-support"></a>裝置支援

-   電腦和膝上型電腦
-   IoT

![觸控板](images/input-interactions/icons-touchpad01.png)

### <a name="typical-usage"></a>一般使用方式

觸控板通常支援一組觸控手勢，可提供類似於直接操作物件與 UI 的觸控支援。

因為這個觸控板支援的整合互動體驗，建議您也提供滑鼠樣式 UI 命令或能供性，而不僅是仰賴觸控輸入支援。 提供觸控板特定的 UI 命令或能供性來支援這些互動。

您應該提供滑鼠特定的 UI 命令或能供性來支援這些互動。 例如，使用上一個和下一個 (或是 + 與 -) 按鈕讓使用者翻頁內容，或旋轉、調整物件大小，以及縮放物件。

### <a name="more-info"></a>其他資訊

[觸控板設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn456353)
 

## <a name="keyboard"></a>鍵盤

鍵盤是文字的主要輸入裝置，對於某些行動不便的使用者，或是認為鍵盤是與應用程式互動更快速且更有效率之方式的使用者來說，通常是不可或缺。

透過 [Continuum 手機版](http://go.microsoft.com/fwlink/p/?LinkID=699431) (此為適用於可相容之 Windows10 行動裝置的新體驗)，使用者可將其手機連接到滑鼠和鍵盤，讓手機可以像膝上型電腦一樣運作。

### <a name="device-support"></a>裝置支援

-   手機和平板手機
-   平板電腦
-   電腦和膝上型電腦
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![鍵盤](images/input-interactions/icons-keyboard01.png)

### <a name="typical-usage"></a>一般使用方式

使用者與通用 Windows 應用程式互動的方式可透過硬體鍵盤和兩種軟體鍵盤來進行：螢幕小鍵盤 (OSK) 和觸控式鍵盤。

OSK 是視覺化的軟體鍵盤，可用來取代實體鍵盤，透過觸控、滑鼠、畫筆/手寫筆或其他指標裝置 (不需要觸控式螢幕) 來輸入資料。 提供 OSK 的用意是針對沒有實體鍵盤的系統，或者使用者因行動不便而無法使用傳統實體輸入裝置的情況。 OSK 模擬了大部分的 (如果沒有全部) 硬體鍵盤功能。

觸控式鍵盤是視覺化的軟體鍵盤，使用觸控輸入來輸入文字。 觸控式鍵盤不能取代 OSK，因為它只能輸入文字 (它不會模擬硬體鍵盤)，而且只有文字欄位或其他可編輯的文字控制項得到焦點之後，才會顯示。 觸控式鍵盤不支援應用程式或系統命令。

**注意**  OSK 的優先順序高於觸控式鍵盤，當顯示 OSK 時就不會顯示觸控式鍵盤。

一般而言，鍵盤是：

-   單一使用者。
-   不受限於裝置方向。
-   用於文字輸入、瀏覽、遊戲，以及協助工具。
-   一律提供 (不論是主動或被動)。

### <a name="more-info"></a>其他資訊

[鍵盤設計指導方針](https://msdn.microsoft.com/library/windows/apps/hh972345)
 

## <a name="mouse"></a>滑鼠

滑鼠最適合用於生產力應用程式和高密度 UI，其中的使用者互動要求適用於目標和命令的像素層級精確度。

### <a name="device-support"></a>裝置支援

-   手機和平板手機
-   平板電腦
-   電腦和膝上型電腦
-   Surface Hub
-   IoT

![滑鼠](images/input-interactions/icons-mouse01.png)

### <a name="typical-usage"></a>一般使用方式

滑鼠輸入可以利用加入各種鍵盤按鍵 (Ctrl、Shift、Alt 等等) 進行修改。 這些按鍵可以和滑鼠左鍵、滑鼠右鍵、滾輪按鈕，以及擴充的滑鼠最佳化命令集的 [X] 按鈕結合使用。 (部分 Microsoft 滑鼠裝置具有兩個稱為 X 按鈕的額外按鍵，通常會用來在 Web 瀏覽器中來回瀏覽)。

類似於手寫筆，滑鼠和觸控輸入的區別在於觸控能夠透過在這些物件上實際運用手勢 (如撥動、滑動、拖曳、旋轉等等)，在畫面上模擬對 UI 元素的直接操作。

您應該提供滑鼠特定的 UI 命令或能供性來支援這些互動。 例如，使用上一個和下一個 (或是 + 與 -) 按鈕讓使用者翻頁內容，或旋轉、調整物件大小，以及縮放物件。

### <a name="more-info"></a>其他資訊

[滑鼠設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn456351)
 

## <a name="gesture"></a>手勢

手勢是辨識為輸入之任何形式的使用者動作，用來控制或與應用程式互動。 手勢有許多種形式，從簡單的用手在螢幕上指定目標，到學習特定的動作模式、使用全身的一連串連續動作。 設計自訂手勢時請注意，因為手勢的意義可能會因地區和文化特性而有所不同。

### <a name="device-support"></a>裝置支援

-   電腦和膝上型電腦
-   IoT
-   Xbox
-   HoloLens

![手勢](images/input-interactions/icons-gesture01.png)

### <a name="typical-usage"></a>一般使用方式

靜態手勢事件會在互動完成之後引發。

- 靜態手勢事件包含 Tapped、DoubleTapped、RightTapped 及 Holding。

操作手勢事件指出進行中的互動。 當使用者觸控元素並繼續進行，直到該使用者舉起手指或操作取消為止，就會開始引發它們。

- 操作事件包括多點觸控互動 (例如縮放、移動瀏覽或旋轉)，以及使用慣性和速度資料的互動 (例如拖曳)。 (操作事件所提供的資訊不會識別互動，而是會提供像是位置、平移量及速度等資料。)

- 指標事件 (例如 PointerPressed 和 PointerMoved) 會針對每個觸控點提供低階詳細資料，包括指標移動以及區分按下和放開事件的能力。

因為 Windows 所支援的整合互動體驗，建議您也提供滑鼠樣式 UI 命令或能供性，而不僅是仰賴觸控輸入支援。 例如，使用上一個和下一個 (或是 + 與 -) 按鈕讓使用者翻頁內容，或旋轉、調整物件大小，以及縮放物件。


## <a name="gamepadcontroller"></a>遊戲台/控制器

遊戲台/控制器是高度特殊化裝置，通常專用於玩遊戲。 不過，它也能用來模擬基本鍵盤輸入，並提供與鍵盤非常類似的 UI 瀏覽體驗。

### <a name="device-support"></a>裝置支援

-   電腦和膝上型電腦
-   IoT
-   Xbox

![控制器](images/input-interactions/icons-controller01.png)

### <a name="typical-usage"></a>一般使用方式

玩遊戲和與特殊的主機互動。


## <a name="multiple-inputs"></a>多重輸入

儘可能容納多個使用者和裝置、將應用程式設計為可以使用各種輸入類型 (手勢、語音、觸控、觸控板、滑鼠與鍵盤)，以將彈性、可用性及可存取性最大化。

### <a name="device-support"></a>裝置支援

-   手機和平板手機
-   平板電腦
-   電腦和膝上型電腦
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![多重輸入](images/input-interactions/icons-inputdevices03-vertical.png)

### <a name="typical-usage"></a>一般使用方式

就如同人們彼此聯繫時會使用語音和手勢的組合一樣，多種類型與模式的輸入在與應用程式互動時相當有用。 不過，這些組合的互動必須盡可能直覺且自然，因為它們也可能造成非常令人混淆的體驗。





 

 