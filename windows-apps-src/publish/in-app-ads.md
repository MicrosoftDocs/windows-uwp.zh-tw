---
description: 如果您的應用程式使用 Microsoft Advertising SDK 來顯示廣告，請使用合作夥伴中心的應用程式內 ads 頁面來管理廣告的使用方式。
title: 應用程式內廣告
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c0951a26f0475d9565e783720263a6c6be9f2f08
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033931"
---
# <a name="in-app-ads"></a>應用程式內廣告

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

使用合作夥伴中心中的 **銷售** &gt; **應用程式內 ads** 頁面來建立和管理 ad 單位： [Partner Center](https://partner.microsoft.com/dashboard)

* 使用 [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 的通用 Windows 平台 (UWP) 應用程式。
* 先前發行 Windows 8. x 和 Windows Phone 8.x 應用程式，這些應用程式使用 [適用于 Windows 和 Windows Phone](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x)8.x 的 Microsoft Advertising SDK。

> [!IMPORTANT]
> 您無法再上傳使用 Windows Phone 8.x SDK (s) 建立的新 XAP 套件。 已存在於 XAP 套件中的應用程式將會繼續在 Windows 10 行動裝置版裝置上運作。 如需詳細資訊，請參閱這 [篇 blog 文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。

如需有關如何將這些 SDK 與應用程式整合來顯示廣告的詳細資訊，請參閱[使用 Microsoft Advertising SDK 在您的應用程式中顯示廣告](../monetize/display-ads-in-your-app.md)。

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>建立廣告單元

若要在您的應用程式中建立[橫幅廣告](../monetize/banner-ads.md)、[插播式廣告](../monetize/interstitial-ads.md)或[原生廣告](../monetize/native-ads.md)的廣告單元：

1.  移至合作夥伴中心中的 **銷售** &gt; **應用程式內 ads** 頁面，然後按一下 [ **建立 ad 單位** ]。
2.  在 **\[應用程式名稱\]** 下拉式清單中，選取使用廣告單元所在的應用程式。
3.  在 **\[廣告單元名稱\]** 欄位中，輸入廣告單元的名稱。 這可以是任何要用來識別報告用途廣告單元的描述性字串。
4.  在 **\[廣告單元類型\]** 下拉式清單中選取廣告類型。

    * 如果您在應用程式中顯示橫幅廣告，請選取 [ **橫幅** ]。
    * 如果您在應用程式中顯示插入式 video ad 或插入式橫幅 ad，請選取 [ **影片插入式** ] 或 [ **橫幅插入式** ]， (務必針對您想要顯示) 的插入式 ad 類型，選取適當的選項。
    * 如果您在應用程式中顯示原生 ad，請選取 [ **原生** ]。

5. 在 **\[裝置系列\]** 下拉式清單中，選取使用廣告單元所在應用程式的目標裝置系列。 可用的選項包括： **\[UWP (Windows 10)\]** 、 **\[電腦/平板電腦 (Windows 8.1)\]** 或 **\[行動裝置 (Windows Phone 8.x)\]** 。

6. 視需要設定下列其他設定：

    * 如果您為廣告單元選取 **\[UWP (Windows 10)\]** 裝置系列，則可以選擇性設定廣告單元的 [流量分配設定](#mediation)。
    * 如果為橫幅廣告單元選取 **\[電腦/平板電腦 (Windows 8.1)\]** 或 **\[行動裝置 (Windows Phone 8.x)\]** 裝置系列，您可以選擇性選取 **\[在應用程式中顯示社群廣告\]** 以選擇加入 [社群廣告](../monetize/index.md)。

7.  如果您尚未設定所選應用程式的 COPPA 合規性，請選擇 [COPPA 合規性](#coppa)區段中的選項。
8.  按一下 **\[建立廣告單元\]** 。

建立新的廣告單元後，廣告單元會顯示在 **\[創造營收\]** &gt; **\[應用程式內廣告\]** 頁面的可用廣告單元表格中。

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>檢閱和編輯廣告單元

建立一個或多個應用程式的廣告單元後，這些廣告單元會顯示在 **\[創造營收\]** &gt; **\[應用程式內廣告\]** 頁面底部的表格中。 此表格會顯示每個廣告單元的 **\[應用程式識別碼\]** 和 **\[廣告單元識別碼\]** ，以及其他資訊。 若要在應用程式中顯示廣告，您必須在程式碼中使用下列值。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](../monetize/set-up-ad-units-in-your-app.md)。

* 如果您的 app 顯示[橫幅廣告](../monetize/banner-ads.md)，請將這些值指派給 [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 物件的 [ApplicationId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 和 [AdUnitId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) 屬性。
* 如果您的應用程式顯示[插入式 ads](../monetize/interstitial-ads.md)，請將這些值傳遞至[InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)物件的[RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)方法。
* 如果您的應用程式顯示 [原生廣告](../monetize/native-ads.md)，請將這些值傳遞至 **NativeAdsManagerV2** 的函式。
  > [!IMPORTANT]
  > 每個廣告單元只能在一個應用程式中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

  > [!NOTE]
  > 您可以在單一 App 中使用多個橫幅、插播式廣告及原生廣告控制項。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/in-app-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供給應用程式的廣告。

若要編輯 UWP 廣告單元的[流量分配設定](#mediation)或使用廣告單元所在應用程式的 [COPPA 合規性](#coppa)，請按一下廣告單元名稱。

> [!NOTE]
> 如果 ad 單位沒有過去六個月的活動，我們會將它標示為 **非** 使用中，最後將它從合作夥伴中心中移除。 您可以使用篩選，只顯示 **使用中** 或 **非使用中** 廣告單元。 如果您看到任何您認為不正確標示為 **非使用中** 的廣告單元時，請 [連絡客戶支援](https://developer.microsoft.com/windows/support)。

<span id="mediation" />

## <a name="mediation-settings"></a>流量分配設定

當您 [建立新的 uwp ad 單位](#create-ad-unit) 或 [編輯現有的 uwp ad 單位](#available-ad-units)時，請使用本節中的選項來設定 ad 單位的 [ad](../monetize/ad-mediation-service.md) 中繼。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路的廣告，以及 Microsoft 應用程式促銷活動的非營收產生廣告， 我們負責處理來自您選擇的廣告網路的橫幅廣告要求的流量分配。 如果您的 App 中的橫幅廣告、插播式廣告或原生廣告已經與 UWP 廣告單元相關聯，則不需要改變 App 的程式碼就能啟用廣告流量分配。

> [!NOTE]
> 當您啟用 UWP 廣告單元的廣告流量分配時，不需要從協力廠商廣告網路取得廣告單元。 我們的廣告流量分配服務會自動建立任何必要的協力廠商廣告單元。

若要針對應用程式中的 UWP 廣告單元設定廣告流量分配設定：

1. [建立廣告單元](#create-ad-unit)或[選取現有的廣告單元](#available-ad-units)。
2. 在 **應用程式內 ads** 頁面上，移至 [中繼 **設定** ] 區段，並設定您的設定。

    * 預設會選取 [ **讓 Microsoft 優化我的設定** ] 核取方塊。 建議您使用此選項。 這個選項會使用機器學習演算法為您的應用程式自動選擇廣告流量分配設定，幫助您在應用程式支援的所有市場獲得最大的廣告收益。 當您使用此選項時，您也可以選擇要在設定中使用的 ad 網路。 取消核取您不想要成為設定一部分的 ad 網路，我們的演算法會確保您的應用程式只會從選取的 ad 網路接收廣告。
    * 如果您想要選擇您自己的 ad 中繼設定，請選擇 [ **修改預設設定** ]。

    > [!NOTE]
    > 本節中的其餘步驟僅適用于您選擇 [ **修改預設設定** ]。

3. 在 **\[目標\]** 下拉式清單中選擇 **\[基準\]** ，為您的廣告流量分配設定預設設定。 這個預設設定將適用於所有市場，除了您定義市場特定設定的市場之外。
4. 接著指定要在控制項中顯示付費網路 (依據廣告效果支付收益給您) 和其他廣告網路 (不依據廣告效果支付收益給您) 的廣告比率。 若要這樣做，請在 **\[付費廣告網路\]** 和 **\[其他廣告網路\]** 的 **\[權重\]** 欄位中輸入介於 0 到 100 之間的值。  
5. 在 **\[付費廣告網路\]** 區段中，選取 **\[作用中\]** 欄中每個您要使用的 [付費網路](#paid-networks)的核取方塊，然後使用 **\[排名\]** 欄中的箭號依排名順序排列網路 (這會指定控制項使用每個網路的頻率)。
6. 如果您選取 **\[橫幅\]** 或 **\[插播式橫幅\]** 廣告單元，您會看到名為 **\[其他廣告網路\]** 的區段。 此區段中的網路不會依廣告效果讓您賺取收益。 這些網路會從來源顯示廣告，像是應用程式促銷活動。

    在 [ **其他 ad 網路** ] 區段中，針對您要使用的每個 [其他網路](#other-networks)選取 [作用中] **資料行中** 的核取方塊，然後使用 [ **排名** ] 資料行中的箭號依序 (這會指定您的控制項) 應該使用每個網路的頻率。 以下是目前支援的其他網路：

7. 對於您要覆寫預設流量分配設定的每個市場，在 **\[目標\]** 下拉式清單中選取市場，然後更新廣告網路選取項目和排名。
8. 按一下 **\[建立廣告單元\]** (如果要建立新的廣告單元) 或 **\[儲存\]** (如果要編輯現有的廣告單元)。

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>支援的付費廣告網路

下表列出每個廣告類型目前支援的付費網路。 請注意，其中一些網路[並非所有市場都有提供](#network-markets)。

|  廣告網路  |  描述  |  支援的廣告類型  |
|--------------|---------------|---------------------|
| Oath 和 AppNexus |  這是由 Microsoft 管理的 ad 網路，可透過我們的合作夥伴網路（Oath 和 AppNexus）提供廣告。<p/>**注意** ： Oath 和 AppNexus 一律排名在橫幅 ad 單位的 **付費 ad 網路** 清單中，而且無法變更為這些類型廣告的較低等級。 | 橫幅、插播式影片 |
| AppNexus (直接) | 選取此選項以提供來自 [AppNexus](https://www.appnexus.com)的廣告。 | 插播式影片、原生  |
| Microsoft 應用程式安裝廣告 | 選取此選項可提供 Windows 生態系統中其他[為自己的 App 建立促銷廣告活動](../monetize/index.md)的開發人員建立的應用程式安裝廣告或應用程式重新佔用廣告。  |  橫幅、插播式橫幅、原生  |
| MSN 內容建議 |  選取此選項以提供來自 MSN 內容建議的廣告。 |  橫幅、插播式橫幅  |
| Outbrain |  選取此選項可從 [Outbrain](https://www.outbrain.com/) 提供廣告。 |  橫幅、插播式橫幅  |
| Revcontent |  選取此選項可從 [Revcontent](https://www.revcontent.com/) 提供廣告。 |  橫幅、原生  |
| Smaato |  選取此選項可從 [Smaato](https://www.smaato.com/) 提供廣告。 |  橫幅  |
| smartclip |  選取此選項可從 [smartclip](http://www.smartclip.com/) 提供廣告。 |  插播式影片  |
| SpotX |  選取此選項可從 [SpotX](https://www.spotx.tv/) 提供廣告。 |  插播式影片  |
| Taboola |  選取此選項可從 [Taboola](https://www.taboola.com/) 提供廣告。 |  橫幅  |
| Vungle | 選取此選項以從[Vungle](https://vungle.com/)提供廣告 | 插播式影片 |
| 底色 | 選取此選項以提供來自 [Undertone](https://www.undertone.com/)的廣告。 | 橫幅插入式 |


<span id="other-networks" />

### <a name="other-ad-networks"></a>其他廣告網路

下表列出每個廣告類型目前支援的其他網路。

|  廣告網路  |  描述  |  支援的廣告類型  |
|--------------|---------------|---------------------|
| Microsoft 社群廣告 |  如果您[建立其中一個應用程式的促銷廣告活動](../monetize/index.md)，並將此行銷活動設定為[社群廣告活動](../monetize/index.md)，則選取此選項可從這個行銷活動顯示廣告。 | 橫幅、插播式橫幅 |
| Microsoft 自家廣告 | 如果您[建立其中一個應用程式的促銷廣告活動](../monetize/index.md)，並將此行銷活動設定為[自家廣告活動](../monetize/index.md)，則選取此選項可從這個行銷活動顯示廣告。 | 橫幅、插播式橫幅  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>廣告網路支援的市場

可用的廣告網路會在所有[支援的市場](define-market-selection.md#microsoft-store-consumer-markets)中提供廣告，但有下列例外。

|  廣告網路  |  支援的市場  |
|--------------|---------------------|
| Revcontent | 巴西、加拿大、法國、德國、義大利、日本、西班牙、英國、美國  |
| Smaato | 巴西、加拿大、法國、德國、義大利、日本、西班牙、英國、美國 |
| smartclip | 奧地利、比利時、丹麥、芬蘭、德國、義大利、荷蘭、挪威、瑞典、瑞士  |
| 底色 | 美國 |

<span id="coppa" />

## <a name="coppa-compliance"></a>COPPA 合規性

當您 [建立 ad 單位](#create-ad-unit)或 [選取現有的 ad 單位](#available-ad-units)時，如果針對 ad 單位選取的應用程式至少有一個提交已在應用程式憑證程式的 [商店](../publish/the-app-certification-process.md#in-the-store)步驟中到達，則 [ **COPPA 合規性** ] 區段會出現在頁面底部。

基於兒童線上隱私權保護法 (「COPPA」) 的立法宗旨，如果您的應用程式是針對 13 歲以下的兒童，您必須選取本區段中的 **\[這個應用程式是針對 13 歲以下的孩童\]** 。 如果您選取此選項，當傳送廣告到您的應用程式時，Microsoft 會採取步驟來停用其行為廣告服務。

您選擇的 **\[COPPA 合規性\]** 設定會自動套用到所選應用程式的所有廣告單元。

> [!IMPORTANT]
> 如果您的 app 是針對 13 歲以下的兒童，基於 COPPA 的規範，您必須承擔某些義務。 如需有關您的義務的詳細資訊，請參閱[此頁面](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule)。
