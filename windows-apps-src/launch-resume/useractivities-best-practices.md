---
title: 使用者活動最佳做法
description: 本指南概述建立和更新使用者活動的建議作法。
keywords: user activity, user activities, timeline, cortana pick up where you left off, cortana pick up where i left off, project rome, 使用者活動, 時間軸, cortana 從先前離開的地方開始, cortana 接續未完成的部分, project rome
ms.date: 08/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f4e41ac4f340491e551fccc1c1d4700bbe8d8004
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172952"
---
# <a name="user-activities-best-practices"></a>使用者活動最佳做法

本指南概述建立和更新使用者活動的建議作法。 如需 Windows 上使用者活動功能的總覽，請參閱 [繼續進行使用者活動，即使是跨裝置也](./useractivities.md)是一樣。 或者，請參閱 Project 羅馬的「 [使用者活動」一節](/windows/project-rome/user-activities/) ，以瞭解其他開發平臺上的活動。

## <a name="when-to-create-or-update-user-activities"></a>建立或更新使用者活動的時機

由於每個應用程式都不同，因此每個開發人員都能判斷將應用程式中的動作對應至使用者活動的最佳方式。 您的使用者活動將會展示在 Cortana 和時間軸中，其著重于提升使用者的生產力和效率，方法是協助他們回頭查看過去所造訪的內容。

**一般指導方針**

* **為一組相關的使用者動作記錄單一活動。** 這與音樂播放清單或電視節目特別相關：單一活動可以定期更新，以反映使用者的進度。 在此情況下，您將會有單一使用者活動，其中包含多個記錄專案，代表在多天或數周的參與時間。 這同樣適用于以檔為基礎的活動，使用者在您的應用程式中進行逐步進度。
* **在雲端中儲存使用者資料。** 如果您想要支援跨裝置活動，您必須確定重新參與此活動所需的內容會儲存至雲端位置。 裝置特定活動會顯示在建立活動的裝置上，但可能不會出現在其他裝置上的時間軸上。
* **請勿針對使用者不需要繼續的動作建立活動。** 如果您的應用程式用來完成不會保存狀態的簡單一次性作業，您可能不需要建立使用者活動。
* **請勿為其他使用者所完成的動作建立活動。** 如果外部帳戶在您的應用程式中傳送訊息或傳送給使用者 @-mentions ，您就不應該為此建立活動。 這種類型的動作是由動作中心通知提供更佳的服務。
  * 共同作業案例是例外：如果有多個使用者同時處理相同活動 (例如 Word 檔) ，則會有其他使用者在您的使用者進行變更的情況。 在此情況下，您可能會想要更新現有的活動，以反映對檔所做的變更。 這會牽涉到更新現有的使用者活動內容資料，而不需要建立新的記錄專案。

**特定應用程式類型的指導方針**

雖然每個應用程式都不同，但大部分的應用程式都屬於下列其中一種互動模式。
* 以**檔為基礎的應用程式**-每份檔建立一個活動，其中有一或多個歷程記錄專案會反映使用期間。 當對檔進行變更時，請務必更新您的活動。
* **遊戲** —為每個遊戲的儲存或世界建立一個活動。 如果您的遊戲只支援一系列的層級，您可以在一段時間內重新發佈相同的活動，不過您可能會想要更新內容資料，以顯示最新的進度或成就。
* **公用程式應用** 程式-如果您的應用程式中沒有使用者需要離開和繼續的內容，您就不需要使用使用者活動。 簡單的應用程式（例如計算機）是很好的範例。
* **企業營運應用程式** —有許多應用程式可用於管理簡單的工作或工作流程。 針對透過您應用程式存取的每個個別工作流程建立一個活動 (例如，expense reports 會每個都是個別的活動，如此一來，使用者就可以按一下活動，查看是否已核准特定的報表) 。
* **媒體播放應用程式** ：每個邏輯群組建立一個活動，例如播放清單、程式或獨立內容) ， (。 應用程式開發人員的根本問題是 (電視節目的每個內容、歌曲) 會計為獨立內容或部分的集合。 一般來說，如果使用者選擇播放集合或順序內容，整體的集合就是活動。 如果他們選擇播放單一的內容，則該內容是活動的一部分。 請參閱以下更具體的指導方針。
  * **音樂：專輯/演出者/** 內容類型-如果使用者選取 **專輯、演出者**或內容類型與點擊數，該集合就是活動;請勿為每個歌曲撰寫個別的活動。 針對較短的集合（例如單一專輯或以隨機順序播放的集合），您可能不需要更新活動以反映使用者目前的位置。 針對長順序播放（如專輯或播放清單），錄製您在專輯內的位置可能有意義。
  * **音樂：智慧型播放清單** —以隨機順序播放音樂的應用程式應該記錄該播放清單的單一活動。 如果使用者第二次播放播放清單，您會建立相同活動的其他歷程記錄。 不需要錄製使用者在播放清單中的目前位置，因為順序是隨機的。
  * **電視系列** ：如果您的應用程式設定為在目前的節目完成後播放下一集，您應該為電視系列撰寫單一活動。 當您跨多個觀看的會話播放各種不同的節目時，您將會更新活動以反映數列中的目前位置，並建立多個記錄記錄。
  * **電影** —電影是一項內容，而且應該有自己的歷程記錄。 如果使用者停止觀看電影的部分，則最好記錄其位置。 當他們想要在未來繼續進行時，活動可能會在其離開的地方繼續播放，或甚至詢問使用者是否想要在一開始就繼續或開始。

## <a name="user-activity-design"></a>使用者活動設計

使用者活動包含三個元件：啟用 URI、視覺資料和內容中繼資料。
* 啟用 URI 是可傳遞至應用程式或體驗的 URI，以便繼續具有特定內容的應用程式。 一般而言，這些連結會採用適用于配置的通訊協定處理常式格式 (例如，"my-app：//page2？ action = edit" ) 。 由開發人員決定 URI 參數將如何由其應用程式處理。 如需詳細資訊，請參閱 [控制碼 URI 啟用](./handle-uri-activation.md) 。
* 視覺化資料，由一組必要和選擇性屬性所組成 (例如：標題、描述或調適型卡片元素) ，可讓使用者以視覺化方式識別活動。 請參閱下方有關為您的活動建立調適型卡片視覺效果的指導方針。
* 內容中繼資料是 JSON 資料，可用來在特定內容下分組和取出活動。 一般來說，這會採用資料的形式 http://schema.org 。 請參閱下文以取得填寫此資料的指導方針。

### <a name="adaptive-card-design-guidelines"></a>調適型卡片設計指導方針

當活動在時間軸中出現時，就會使用調適型 [卡片架構](/adaptive-cards/)來顯示。 如果開發人員未提供每個活動的調適型卡片，時程表會根據應用程式名稱/圖示、必要的標題欄位和選擇性的描述欄位，自動建立簡單的卡片。 

建議應用程式開發人員使用簡單調適型卡片 JSON 架構來提供自訂卡片。 如需有關如何建立調適型卡片物件的技術指示，請參閱 [調適型卡片檔](/adaptive-cards/authoring-cards/getting-started) 。 請參閱下列指導方針，以設計使用者活動中的調適型卡片。
* 使用影像
  * 如果可能的話，請針對每個活動使用唯一的映射。 您的應用程式名稱和圖示將會自動顯示在活動的卡片旁邊;其他映射可協助使用者找出他們要尋找的活動。
  * 影像不應包含使用者預期要讀取的文字。 此文字無法供具有存取範圍需求的使用者使用，且無法搜尋。
  * 如果映射不包含文字，而且可以裁剪為大約2:1 比例，您應該使用它做為背景影像。 這會導致在時間軸中有一個粗體的活動卡。 影像會稍微暗一點，以確保文字在卡片上保持可見狀態，而且建議您只在這種情況下使用活動名稱，因為較小的文字可能會變得難以閱讀。
  * 如果影像無法裁剪為2:1，您應該將它放在活動卡內。  
    * 如果外觀比例是正方形或直向，請將影像錨定在卡片右側且不含邊界。
    * 如果外觀比例為橫向，請將影像錨定到卡片的右上角。
* 每個活動都必須提供活動名稱，且應一律顯示。
  * 這個名稱應該會使用大型粗體文字選項顯示在卡片的左上角。 很重要的是，名稱很容易辨識，因為這是使用者在 Cortana 案例中顯示活動時唯一會看到的部分。 在時間軸中顯示相同的名稱，讓使用者更容易流覽大量活動。
* 對您應用程式中的所有活動使用相同的視覺化樣式，讓使用者可以輕鬆地在時間軸中找出您應用程式的活動。
  * 例如，活動全都使用相同的背景色彩。
* 請謹慎使用補充文字資訊。 
  * 避免填滿含有文字的卡片，而且只使用補充資訊，以協助使用者尋找正確的活動或反映狀態資訊 (例如特定工作) 的目前進度。

### <a name="content-metadata-guidelines"></a>內容中繼資料指導方針

使用者活動也可以包含內容中繼資料，讓 Windows 和 Cortana 用來分類活動並產生推斷。 然後，您可以在特定主題周圍分組活動，例如 (如果使用者正在研究休假) ，則物件 (如果使用者正在研究) 或動作 (的使用者是否在不同的應用程式和網站) 購物特定產品。 最好同時表示與活動相關的名詞和動詞。 

在下列範例中，content metadata JSON （遵循 [Schema.org](https://schema.org/)的標準）代表案例：「John 玩了 Steve 的鳥」。

```json
// John played angry birds with Steve.
{
  "@context": "http://schema.org",
  "@type": "PlayAction",
  "agent": {
    "@type": "Person",
    "name": "John"
  },
  "object": {
    "@type": "MobileApplication",
    "name": "Angry Birds."
  },
  "participant": {
    "@type": "Person",
    "name": "Steve"
  }
}
```

## <a name="key-apis"></a>重要 API

* [UserActivities 命名空間](/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>相關主題

* [ (Project 羅馬檔的使用者活動) ](/windows/project-rome/user-activities/)
* [調適型卡片](/adaptive-cards/)
* [調適型卡片視覺化工具範例](https://adaptivecards.io/)
* [處理 URI 啟用](./handle-uri-activation.md)
* [使用 Microsoft Graph、活動摘要及調適型卡片在任何平台上與您的客戶互動](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph)