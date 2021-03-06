---
title: 設定帳戶使用者的角色或自訂權限
description: 瞭解如何在將使用者新增至您的合作夥伴中心帳戶時，設定標準角色或自訂許可權。
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 使用者角色, 使用者權限, 自訂角色, 使用者存取, 自訂權限, 標準角色
ms.localizationpriority: medium
ms.openlocfilehash: bdf46b681c7ae2bce18bab464ac75984f784fc7b
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104639"
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>設定帳戶使用者的角色或自訂權限

當您 [將使用者新增到您的合作夥伴中心帳戶](add-users-groups-and-azure-ad-applications.md)時，您必須指定帳戶中的存取權。 您可以指派他們[標準角色](#roles) (套用至整個帳戶)，或是[自訂其權限](#custom)以提供適當層級的存取權。 某些自訂權限適用於整個帳戶，某些權限可限制為一或多個特定產品 (如果您想要，可針對所有產品授與)。

> [!NOTE] 
> 不論您是新增使用者、群組或 Azure AD 應用程式，都適用相同的角色和權限。

當判斷要套用哪個角色或權限，請牢記： 
-   除非您 [自訂許可權](#custom) 並指派 [產品層級許可權](#product-level-permissions) ，讓他們只能使用特定的應用程式和（或）附加元件，否則使用者 (包括群組和 Azure AD 應用程式) 將能夠以與其指派的角色 () 相關聯的許可權來存取整個合作夥伴中心帳戶。
-   您可以藉由選取多個角色，來允許使用者、群組或 Azure AD 應用程式具備一個以上之角色功能的存取權，或使用自訂權限授與您想要的存取權。
-   具有特定角色 (或一組自訂權限) 的使用者也可以是具有不同角色 (或一組權限) 之群組的一部分。 在此情況下，使用者將具備與群組和個別帳戶相關聯之所有功能的存取權。

> [!TIP]
> 本主題僅適用于 [合作夥伴中心](https://partner.microsoft.com/dashboard)中的 Windows 應用程式開發人員計畫。 如需硬體開發人員計畫中使用者角色的相關資訊，請參閱[管理使用者角色](/windows-hardware/drivers/dashboard/managing-user-roles)。 如需有關 Windows 傳統型應用程式計畫中使用者角色的詳細資訊，請參閱 [Windows 傳統型應用程式計畫](/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)。


<span id="roles" />

## <a name="assign-roles-to-account-users"></a>指派角色給帳戶使用者

依預設，當您將使用者、群組或 Azure AD 應用程式新增至合作夥伴中心帳戶時，會顯示一組標準角色供您選擇。 每個角色都具備一組特定權限，以便在帳戶內執行特定功能。 

除非選取 **\[自訂權限\]** 來選擇定義 [自訂權限](#custom)，否則您新增到帳戶的每個使用者、群組或 Azure AD 應用程式至少必須被指派下列其中一個標準角色。 

> [!NOTE]
> 帳戶的 **擁有者** 是第一位使用 Microsoft 帳戶建立它的人 (而不是任何透過 Azure AD 新增的使用者)。 這個帳戶擁有者是唯一具備帳戶完整存取權的人，包括能夠刪除 App、建立和編輯所有帳戶使用者，以及變更所有財務和帳戶設定。 


| 角色                 | 描述              |
|----------------------|--------------------------|
| Manager              | 具備帳戶的完整存取權，但變更稅務和支付設定例外。 這包括管理合作夥伴中心中的使用者，但請注意，在 Azure AD 租使用者中建立和刪除使用者的能力，取決於帳戶在 Azure AD 中的許可權。 也就是說，如果使用者被指派管理員角色，但沒有組織 Azure AD 中的全域管理員許可權，他們將無法建立新的使用者，也不能從 (目錄中刪除使用者，因為他們可以變更使用者的合作夥伴中心角色) 。 <p> 請注意，如果合作夥伴中心帳戶與多個 Azure AD 租使用者相關聯，則管理員將無法看到使用者的完整詳細資料 (包括名字、姓氏、密碼復原電子郵件，以及它們是否為 Azure AD 全域管理員) 除非使用具有該租使用者全域管理員許可權的帳戶登入相同的租使用者。 不過，他們可以新增和移除任何與合作夥伴中心帳戶相關聯的租使用者中的使用者。 |
| 開發人員            | 可以上傳套件並提交 App 和附加元件，而且可以檢視[使用方式報告](usage-report.md)來取得遙測詳細資料。 可以存取 [跨裝置體驗](https://developer.microsoft.com/windows/project-rome) 功能。 無法檢視財務資訊或帳戶設定。   |
| 商務參與者 | 可檢視[健康情況](health-report.md)與[使用量](usage-report.md)報告。 無法建立或提交產品、 變更帳戶設定或檢視財務資訊。   |
| 財務參與者  | 可檢視[支付報告](/partner-center/payout-statement)、財務資訊及下載數報告。 無法對 App、附加元件或帳戶設定進行任何變更。    |
| 行銷人員             | 可以[回應客戶評論](respond-to-customer-reviews.md)以及檢視非財務性[分析報告](analytics.md)。 無法對 App、附加元件或帳戶設定進行任何變更。      |

下表顯示每個角色 (和其帳戶擁有者) 可用的部分特定功能。

|                                                       |    帳戶擁有者                 |    Manager                       |    開發人員                     |    商務參與者    |    財務參與者    |    行銷人員                      |
|-------------------------------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    **取得報表 (包括近乎即時的資料)** |    可檢視                      |    可檢視                      |    無存取權                     |    無存取權               |    可檢視               |    無存取權                     |
|    **意見反應報告/回應**                          |    可檢視並傳送意見反應    |    可檢視並傳送意見反應    |    可檢視並傳送意見反應    |    無存取權               |    無存取權              |    可檢視並傳送意見反應    |
|    **健康情況報告 (包括近乎即時的資料)**      |    可檢視                      |    可檢視                      |    可檢視                      |    可檢視                |    無存取權              |    無存取權                     |
|    **使用方式報告**                                       |    可檢視                      |    可檢視                      |    可檢視                      |    可檢視                |    無存取權              |    無存取權                     |
|    **支付帳戶**                                     |    可更新                    |    無存取權                     |    無存取權                     |    無存取權               |    可更新             |    無存取權                     |
|    **稅金設定檔**                                        |    可更新                    |    無存取權                     |    無存取權                     |    無存取權               |    可更新             |    無存取權                     |
|    **支付摘要**                                     |    可檢視                      |    無存取權                     |    無存取權                     |    無存取權               |    可檢視               |    無存取權                     |

如果沒有適當的標準角色，或您想要限制特定 app 和/或附加元件的存取權，您可以選取 **\[自訂權限\]** 來授與使用者自訂權限，如下所述。


<span id="custom" />

## <a name="assign-custom-permissions-to-account-users"></a>指派自訂權限給帳戶使用者

新增或編輯使用者帳戶時，若要指派自訂權限而非標準角色，請按一下 **\[角色\]** 區段中的 **\[自訂權限\]**。 

若要啟用使用者的權限，請切換方塊到適當的設定。 

![存取權設定指南](images/permission_key.png)

- **無存取權**：使用者將不具有指示的權限。
- **唯讀**：使用者有權檢視指示區域相關的功能，但無法進行變更。 
- **讀/寫**：使用者有權進行區域相關的變更和檢視。
- **混合式**︰您無法直接選取這個選項，但如果您允許該權限有存取權組合，則會顯示 **混合式** 指示器。 例如，如果您針對 **所有產品** 的 **定價和可用性** 授與 **唯讀** 存取權，然後針對特定產品授與 **定價和可用性** 的 **讀/寫** 存取權，則 **所有產品** 的 **定價和可用性** 指示器會顯示為「混合式」。 如果某些產品具有 **無存取權** 的權限，但其他產品具有 **讀/寫** 和/或 **唯讀** 存取權，則情況一樣。

針對某些權限 (例如和檢視分析資料相關的權限)，您只能授與 **唯讀** 存取權。 請注意，在目前的實作中，有些權限沒有區別 **唯讀** 和 **讀/寫** 存取權。 請檢閱每個權限的詳細資料，以了解 **唯讀** 和/或 **讀/寫** 存取權賦予的特定能力。

下表說明每個權限相關的特定詳細資料。

## <a name="account-level-permissions"></a>帳戶層級權限

本節中的權限不限特定產品。 授與這其中一個權限的存取權可讓使用者在整個帳戶擁有該項權限。

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">權限名稱</th>
    <th align="left">唯讀</th>
    <th align="left">讀取/寫入</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    <b>帳戶設定</b>                    </td><td align="left">  可以在 [ <b>帳戶設定</b> ] 區段中查看所有頁面，包括 <a href="/partner-center/partner-center-account-setup">連絡人資訊</a>。       </td><td align="left">  可以在 [ <b>帳戶設定</b> ] 區段中查看所有頁面。 可變更<a href="/partner-center/partner-center-account-setup">連絡資訊</a>和其他頁面，但不能變更支付帳戶或稅金設定檔 (除非另外授與該權限)。            </td></tr>
<tr><td align="left">    <b>帳戶使用者</b>                       </td><td align="left">  可檢視已加入 <b>\[使用者\]</b> 區段中的帳戶的使用者。          </td><td align="left">  可在 <b>\[使用者\]</b> 區段中新增使用者到帳戶和變更現有使用者。             </td></tr>
<tr><td align="left">    <b>帳戶層級 ad 效能報表</b> </td><td align="left">  可檢視帳戶層級的<a href="advertising-performance-report.md">廣告效益報告</a>。      </td><td align="left">  N/A   </td></tr>
<tr><td align="left">    <b>廣告活動</b>                        </td><td align="left">  可檢視帳戶中建立的<a href="/windows/uwp/monetize/">廣告活動</a>。      </td><td align="left">  可建立、管理和檢視帳戶中建立的<a href="/windows/uwp/monetize/">廣告活動</a>。          </td></tr>
<tr><td align="left">    <b>Ad 中繼</b>                        </td><td align="left">  可檢視帳戶中所有產品的廣告流量分配設定。    </td><td align="left">  可檢視和變更帳戶中所有產品的廣告流量分配設定。        </td></tr>
<tr><td align="left">    <b>Ad 中繼報告</b>                </td><td align="left">  可檢視帳戶中所有產品的<a href="/windows/uwp/publish/advertising-performance-report">廣告流量分配報告</a>。    </td><td align="left">  N/A    </td></tr>
<tr><td align="left">    <b>Ad 效能報告</b>              </td><td align="left">  可檢視帳戶中所有產品的<a href="advertising-performance-report.md">廣告效益報告</a>。       </td><td align="left">  N/A         </td></tr>
<tr><td align="left">    <b>Ad 單位</b>                            </td><td align="left">  可檢視已為帳戶建立的<a href="in-app-ads.md">廣告單位</a>。    </td><td align="left">  可建立、管理及檢視帳戶的<a href="in-app-ads.md">廣告單位</a>。             </td></tr>
<tr><td align="left">    <b>分支機搆廣告</b>                       </td><td align="left">  可檢視帳戶中所有產品的<a href="/windows/uwp/publish/in-app-ads">聯盟廣告</a>使用量。    </td><td align="left">  可管理和檢視帳戶中所有產品的<a href="/windows/uwp/publish/in-app-ads">聯盟廣告</a>使用量。                </td></tr>
<tr><td align="left">    <b>關係企業效能報告</b>      </td><td align="left">  可檢視帳戶中所有產品的<a href="/windows/uwp/publish/advertising-performance-report">聯盟績效報告</a>。   </td><td align="left">  N/A   </td></tr>
<tr><td align="left">    <b>應用程式安裝 ads 報表</b>             </td><td align="left">  可檢視<a href="/windows/uwp/publish/ad-campaign-report">廣告活動報告</a>。           </td><td align="left">  N/A   </td></tr>
<tr><td align="left">    <b>社區廣告</b>                       </td><td align="left">  可檢視帳戶中所有產品的<a href="/windows/uwp/monetize/">社群廣告</a>使用量。          </td><td align="left">  可建立、管理和檢視帳戶中所有產品的免費<a href="/windows/uwp/monetize/">社群廣告</a>使用量。               </td></tr>
<tr><td align="left">    <b>連絡人資訊</b>                        </td><td align="left">  可檢視 \[帳戶設定\] 區段中的<a href="/partner-center/partner-center-account-setup">連絡資訊</a>。        </td><td align="left">  可編輯和檢視 \[帳戶設定\] 區段中的<a href="/partner-center/partner-center-account-setup">連絡資訊</a>。            </td></tr>
<tr><td align="left">    <b>COPPA 合規性</b>                    </td><td align="left">  可檢視帳戶中所有產品的 <a href="in-app-ads.md#coppa-compliance">COPPA 規範</a>選取項目 (指示產品的目標對象是否為 13 歲以下的兒童)。                                            </td><td align="left">  可編輯和檢視帳戶中所有產品的 <a href="in-app-ads.md#coppa-compliance">COPPA 規範</a>選取項目 (指示產品的目標對象是否為 13 歲以下的兒童)。         </td></tr>
<tr><td align="left">    <b>客戶群組</b>                     </td><td align="left">  可以)  (區段和已知的使用者群組，來查看 <a href="create-customer-groups.md">客戶群組</a> 。      </td><td align="left">  可以建立、編輯及查看 <a href="create-customer-groups.md">客戶群組</a> (區段和已知的使用者群組) 。       </td></tr>
<tr><td align="left">    <b>管理產品群組</b>&nbsp;*                            </td><td align="left">  可檢視新的產品群組建立頁面，但實際上不能在帳戶中建立新的產品群組。    </td><td align="left">  可以建立和編輯產品群組。     </td></tr>
<tr><td align="left">    <b>新應用程式</b>                            </td><td align="left">  可檢視新的 app 建立頁面，但實際上不能在帳戶中建立新的 app。    </td><td align="left">  可透過保留新的 app 名稱以在帳戶中<a href="create-your-app-by-reserving-a-name.md">建立新的 app</a>，並可建立提交內容，將 app 提交到市集。     </td></tr>
<tr><td align="left">    <b>新的配套</b>&nbsp;*                       </td><td align="left">  可檢視新的套件組合建立頁面，但實際上不能在帳戶中建立新的套件組合。     </td><td align="left">  可建立新的產品套件組合。          </td></tr>
<tr><td align="left">    <b>合作夥伴服務</b>&nbsp;*                  </td><td align="left">  可檢視安裝到服務以擷取 XTokens 的憑證。     </td><td align="left">  可管理和檢視安裝到服務以擷取 XTokens 的憑證。       </td></tr>
<tr><td align="left">    <b>支出帳戶</b>                      </td><td align="left">  可以查看<b>帳戶設定</b>中的付款<a href="/partner-center/set-up-your-payout-account#payout-account">帳戶資訊</a>。     </td><td align="left">  可以編輯及查看<b>帳戶設定</b>中的付款<a href="/partner-center/set-up-your-payout-account#payout-account">帳戶資訊</a>。       </td></tr>
<tr><td align="left">    <b>付款摘要</b>                      </td><td align="left">  可檢視<a href="/partner-center/payout-statement">支付摘要</a>以存取和下載支付報告資訊。       </td><td align="left">  可檢視<a href="/partner-center/payout-statement">支付摘要</a>以存取和下載支付報告資訊。   </td></tr>
<tr><td align="left">    <b>信賴憑證者</b>&nbsp;*                   </td><td align="left">  可檢視信賴憑證者以擷取 XTokens。    </td><td align="left">  可管理和檢視信賴憑證者以擷取 XTokens。     </td></tr>
<tr><td align="left">    <b>沙箱</b>&nbsp;*                         </td><td align="left">  可以存取 <b>沙箱</b> 頁面，並查看帳戶中的沙箱以及這些沙箱的任何適用設定。 除非被授與適當的產品層級權限，否則無法檢視每個沙箱的產品和提交內容。 </td><td align="left">  可以存取 <b>沙箱</b> 頁面，並查看和管理帳戶中的沙箱，包括建立和刪除沙箱，以及管理其設定。 除非被授與適當的產品層級權限，否則無法檢視每個沙箱的產品和提交內容。    </td></tr>
<tr><td align="left">    <b>商店銷售活動</b>&nbsp;*                            </td><td align="left">  N/A    </td><td align="left">  可以設定自動將產品包含在 Microsoft Store 銷售事件中的選項。     </td></tr>
<tr><td align="left">    <b>稅務設定檔</b>                         </td><td align="left">  可以在<b>帳戶設定</b>中查看<a href="/partner-center/set-up-your-payout-account#tax-forms">稅務設定檔資訊和表單</a>。     </td><td align="left">  可以在<b>帳戶設定</b>中填寫稅務表單，並更新<a href="/partner-center/set-up-your-payout-account#tax-forms">稅務設定檔資訊</a>。     </td></tr>
<tr><td align="left">    <b>測試帳戶</b>&nbsp;*                     </td><td align="left">  可檢視用於測試 Xbox Live 設定的帳戶。      </td><td align="left">  可建立、管理和檢視用於測試 Xbox Live 設定的帳戶。      </td></tr>
<tr><td align="left">    <b>Xbox 裝置</b>                        </td><td align="left">  可以在 [ <b>帳戶設定</b> ] 區段中，查看為帳戶啟用的 Xbox 開發主控台。       </td><td align="left">  可以在 [ <b>帳戶設定</b> ] 區段中，新增、移除和查看為帳戶啟用的 Xbox 開發主控台。     </td></tr>
    </tbody>
    </table>

\* 以星號標記的許可權 ( * ) 授與存取權給所有帳戶皆無法使用的功能。 如果您的帳戶尚未啟用這些功能，您對於這些權限所做的選擇不會有任何影響。   


## <a name="product-level-permissions"></a>產品層級的權限

本節中的權限可授與帳戶中的所有產品，或可自訂為只允許一或多個特定產品的權限。 

產品層級的權限分成四種類別︰**分析**、**創造營收**、**發佈** 和 **Xbox Live**。 您可以展開這些類別，以檢視每個類別中的個別權限。 您也可以選擇啟用一或多個特定產品的 **所有權限**。

若要為帳戶中的每個產品授與權限，請在標示 **\[所有產品\]** 的列中針對該權限做選擇 (切換方塊以指示 **唯讀** 或 **讀/寫**)。 
 
> [!TIP]
> 對 **\[所有產品\]** 做的選擇將適用於目前帳戶中的每個產品，以及未來在帳戶建立的任何產品。 若要防止權限套用到未來的產品，請個別選取所有產品，而不要選擇 **\[所有產品\]**。

在 [ **所有產品** ] 資料列下方，您會看到帳戶中的每個產品列在個別的資料列上。 若只要授與特定產品的權限，請在該產品的列中針對該權限做選擇。

每個附加元件都會列在其父產品下的個別資料列中，以及 **所有附加** 元件資料列。 針對 **所有附加** 元件所做的選擇會套用至該產品的所有目前附加元件，以及針對該產品所建立的任何未來附加元件。

請注意，附加元件無法設定一部分的權限。 這是因為它們不適用於附加元件 (例如 **客戶回函** 權限)，或因為在父產品層級授與的權限適用於該產品的所有附加元件 (例如 **促銷碼**)。 但請注意，附加元件適用的任何權限都必須個別設定；附加元件不會繼承為父產品所做的選擇。 例如，如果您想允許使用者選擇附加元件的定價和可用性，則無論是否已授與父產品的 **定價和可用性** 權限，您都必須啟用附加元件 (或 **所有附加元件**) 的 **定價和可用性** 權限。 


### <a name="analytics"></a>分析

<table>
    <thead>
    <tr class="header">
    <th align="left">權限&nbsp;名稱</th>
    <th align="left">唯讀&nbsp;</th>
    <th align="left">讀取/寫入</th>
    <th align="left">唯讀&nbsp;&nbsp; (附加元件) </th>
    <th align="left">讀寫&nbsp; (附加元件)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">     (包括近乎即時資料) 的<b>收購</b> </td><td>    可檢視產品的<a href="acquisitions-report.md">下載數</a>和<a href="add-on-acquisitions-report.md">附加元件下載數</a>報告。        </td><td>    N/A    </td><td>    N/A (父產品的設定包含附加元件的 [ **收購** ] 報告)         </td><td>    N/A                         </td></tr>
    <tr><td align="left">    <b>使用</b> </td><td>    可檢視產品的<a href="usage-report.md">使用量報告</a>。     </td><td>    N/A       </td><td>    N/A     </td><td>    N/A         </td></tr>
    <tr><td align="left">    <b>健全狀況</b> (包括近乎即時的資料)  </td><td>    可檢視產品的<a href="health-report.md">健康情況報告</a>。    </td><td>    N/A     </td><td>    N/A     </td><td>    N/A         </td></tr>
    <tr><td align="left">    <b>客戶意見反應</b>    </td><td>    可檢視產品的<a href="reviews-report.md">評論</a>及<a href="feedback-report.md">意見反應</a>報告。       </td><td>    N/A (回應意見反應或評論，必須授 <b>與 Contact 客戶</b> 的許可權)    </td><td>    N/A     </td><td>    N/A         </td></tr>
    <tr><td align="left">    <b>Xbox 分析</b> </td><td>    可以查看產品的 <a href="xbox-analytics-report.md">Xbox 分析報表</a> 。    </td><td>    N/A   </td><td>    N/A       </td><td>    N/A          </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>創造營收

<table>
    <thead>
    <tr class="header">
    <th align="left">權限&nbsp;名稱</th>
    <th align="left">唯讀&nbsp;</th>
    <th align="left">讀取/寫入</th>
    <th align="left">唯讀&nbsp;&nbsp; (附加元件) </th>
    <th align="left">讀寫&nbsp; (附加元件)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>促銷代碼</b>     </td><td>    可檢視產品和其附加元件的<a href="generate-promotional-codes.md">促銷碼</a>訂單與使用量資訊，並可檢視使用量資訊。         </td><td>    可檢視、管理及建立產品及其附加元件的<a href="generate-promotional-codes.md">促銷碼</a>訂單，並可檢視使用量資訊。          </td><td>    不適用 (父產品的設定適用於所有附加元件)     </td><td>    不適用 (父產品的設定適用於所有附加元件)     </td></tr>
    <tr><td align="left">    <b>目標優惠</b>     </td><td>    可檢視產品的<a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">針對性優惠</a>。         </td><td>    可檢視、管理及建立產品的<a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">針對性優惠</a>。          </td><td>    N/A     </td><td>    N/A      </td></tr>
    <tr><td align="left">    <b>聯絡客戶</b>  </td><td>    只要一併授與<b>客戶回函</b>權限，即可檢視<a href="respond-to-customer-feedback.md">對客戶回函的回應</a>和<a href="respond-to-customer-reviews.md">對客戶評論的回應</a>。 也可檢視已為產品建立的<a href="send-push-notifications-to-your-apps-customers.md">特定對象的通知</a>。    </td><td>    只要授與客戶<b>意見</b>反應許可權，就可以<a href="respond-to-customer-feedback.md">回應客戶的意見</a>反應並<a href="respond-to-customer-reviews.md">回應客戶評論</a>。 也可為產品<a href="send-push-notifications-to-your-apps-customers.md">建立和傳送特定對象的通知</a>。                   </td><td>    N/A         </td><td>    N/A                          </td></tr>
    <tr><td align="left">    <b>測試</b></td><td>    可檢視 <a href="../monetize/run-app-experiments-with-a-b-testing.md">實驗 (A/B 測試)</a> 及檢視產品的實驗資料。   </td><td>    可建立、管理及檢視產品的 <a href="../monetize/run-app-experiments-with-a-b-testing.md">實驗 (A/B 測試)</a> 及檢視實驗資料。     </td><td>    N/A  </td><td>    N/A                 </td></tr>
    <tr><td align="left">    <b>商店銷售活動</b>&nbsp;*</td><td>    可以檢視產品的銷售活動狀態。   </td><td>    可以新增產品銷售活動，並設定折扣。      </td><td>    可以檢視產品的銷售活動狀態。   </td><td>    可以新增產品銷售活動，並設定折扣。      </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>發佈 

<table>
    <thead>
    <tr class="header">
    <th align="left">權限&nbsp;名稱</th>
    <th align="left">唯讀&nbsp;</th>
    <th align="left">讀取/寫入</th>
    <th align="left">唯讀&nbsp;&nbsp; (附加元件) </th>
    <th align="left">讀寫&nbsp; (附加元件)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>產品設定</b>  </td><td>    可以查看產品的產品設定頁面。     </td><td>    可以查看和編輯產品的產品設定頁面。 </td><td>    可以查看附加元件的 [產品設定] 頁面。   </td><td>    可以查看和編輯產品設定頁面附加元件。          </td></tr>
    <tr><td align="left">    <b>定價與可用性</b>  </td><td>    可以查看產品的 [ <a href="set-app-pricing-and-availability.md">定價和可用性</a> ] 頁面。     </td><td>    可以查看和編輯產品的 [ <a href="set-app-pricing-and-availability.md">定價和可用性</a> ] 頁面。 </td><td>    可以查看附加元件的 [ <a href="set-add-on-pricing-and-availability.md">定價和可用性</a> ] 頁面。   </td><td>    可以查看和編輯附加元件的 [ <a href="set-add-on-pricing-and-availability.md">定價和可用性</a> ] 頁面。          </td></tr>
    <tr><td align="left">    <b>性能</b>   </td><td>    可以查看產品的 <a href="enter-app-properties.md">屬性</a> 頁面。      </td><td>    可以查看和編輯產品的 <a href="enter-app-properties.md">屬性</a> 頁面。       </td><td>    可以查看附加元件的 [ <a href="enter-add-on-properties.md">屬性</a> ] 頁面。     </td><td>    可以查看和編輯附加元件的 [ <a href="enter-add-on-properties.md">屬性</a> ] 頁面。               </td></tr>
    <tr><td align="left">    <b>年齡分級</b>    </td><td>    可以查看產品的 <a href="age-ratings.md">年齡評</a> 等頁面。       </td><td>    可以查看和編輯產品的 <a href="age-ratings.md">年齡評</a> 等頁面。    </td><td>    可以查看附加元件的 [年齡等級] 頁面。          </td><td>     可以查看和編輯附加元件的年齡分級頁面。       </td></tr>
    <tr><td align="left">    <b>包</b>        </td><td>    可以查看產品的 [ <a href="upload-app-packages.md">封裝</a> ] 頁面。  </td><td>    可以查看和編輯產品的 <a href="upload-app-packages.md">封裝</a> 頁面，包括上傳封裝。     </td><td>   如果適用) ，可以查看附加元件 (的 [ <a href="upload-app-packages.md">封裝</a> ] 頁面。   </td><td>     可以在附加元件 (中查看和編輯 <a href="upload-app-packages.md">封裝</a> 頁面（如果適用) ）。             </td></tr>
    <tr><td align="left">    <b>商店清單</b>  </td><td>    可以查看產品 <a href="create-app-store-listings.md">) 的 Store 清單頁面 (</a> 。  </td><td>    可以查看和編輯產品 <a href="create-app-store-listings.md">) 的 Store 清單 (頁面 </a> ，也可以新增不同語言的新商店清單。     </td><td>    可以查看附加元件的 <a href="create-add-on-store-listings.md">Store 清單頁面 () </a> 。            </td><td>    可以查看和編輯附加元件 <a href="create-add-on-store-listings.md">)  (的 Store 清單頁面 </a> ，也可以新增不同語言的商店清單。                 </td></tr>
    <tr><td align="left">    <b>提交商店</b>     </td><td>    若此權限設為唯讀，則不會授與任何存取權。           </td><td>    可將產品提交到市集和檢視認證報告。 包含新的與更新的提交。 </td><td>若此權限設為唯讀，則不會授與任何存取權。     </td><td>    可將附加元件提交到市集和檢視認證報告。 包含新的與更新的提交。</td></tr>
    <tr><td align="left">    <b>建立新提交</b>       </td><td>    若此權限設為唯讀，則不會授與任何存取權。        </td><td>    可為產品建立新的<a href="app-submissions.md">提交</a>。  </td><td>    若此權限設為唯讀，則不會授與任何存取權。   </td><td>    可為附加元件建立新的<a href="add-on-submissions.md">提交</a>。        </td></tr>
    <tr><td align="left">    <b>新附加元件</b>    </td><td>    若此權限設為唯讀，則不會授與任何存取權。 </td><td>    可為產品<a href="set-your-add-on-product-id.md">建立新的附加元件</a>。 </td><td>    N/A    </td><td>    N/A        </td></tr>
    <tr><td align="left">    <b>名稱保留</b>   </td><td>    可檢視產品的<a href="manage-app-names.md">管理 app 名稱</a>頁面。</td><td>    可檢視和編輯產品的<a href="manage-app-names.md">管理 app 名稱</a>頁面，包括保留其他名稱及刪除保留名稱。 </td><td>   可檢視附加元件的保留名稱。    </td><td>   可檢視和編輯附加元件的保留名稱。          </td></tr>
    <tr><td align="left">    <b>光碟要求</b>   </td><td>    可以在 [要求] 頁面上查看光碟。 </td><td>    可以建立光碟要求。 </td><td>   N/A    </td><td>   N/A          </td></tr>
    <tr><td align="left">    <b>光碟版稅 </b>   </td><td>    可以在 [版稅] 頁面中查看光碟。</td><td>    可以建立光碟版稅。 </td><td>   N/A    </td><td>   N/A          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">權限&nbsp;名稱</th>
    <th align="left">唯讀&nbsp;</th>
    <th align="left">讀取/寫入</th>
    <th align="left">唯讀&nbsp;&nbsp; (附加元件) </th>
    <th align="left">讀寫&nbsp; (附加元件)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>信賴憑證者</b>&nbsp;*</td><td>    可以查看帳戶的 [信賴憑證者] 頁面。   </td><td>    可以查看和編輯帳戶的 [信賴憑證者] 頁面。    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>合作夥伴服務</b>&nbsp;*</td><td>    可以查看帳戶的 [Web 服務] 頁面。  </td><td>    可以查看和編輯帳戶的 [Web 服務] 頁面。      </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Xbox 測試帳戶</b>&nbsp;*</td><td>    可以查看帳戶的 [Xbox Test 帳戶] 頁面。  </td><td>    可以查看和編輯帳戶的 [Xbox Test 帳戶] 頁面。    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>每個沙箱的 Xbox Test 帳戶</b>&nbsp;*</td><td>    只能針對特定的沙箱帳戶，查看 [Xbox Test 帳戶] 頁面。  </td><td>    可以查看和編輯 Xbox Test。   <tr><td align="left">    <b>帳戶的僅限指定沙箱的 [帳戶] 頁面    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Xbox 裝置</b>&nbsp;*</td><td>    可以查看帳戶的 Xbox one 開發主控台頁面。  </td><td>    可以查看和編輯帳戶的 Xbox one 開發主控台頁面。    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>每一沙箱的 Xbox 裝置</b>&nbsp;*</td><td>    只能針對特定的沙箱帳戶，查看 [Xbox one 開發主控台] 頁面。  </td><td>    只能針對特定的沙箱帳戶，查看及編輯 [Xbox one 開發主控台] 頁面。    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>應用程式通道</b>&nbsp;*</td><td>    N/A  </td><td>    可將宣傳影片頻道發佈到 Xbox 主機，以透過OneGuide 檢視。    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>服務設定</b>&nbsp;*</td><td>    可以查看產品的 Xbox Live 服務設定] 頁面。  </td><td>    可以查看和編輯產品的 Xbox Live 服務設定] 頁面。    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>工具存取</b>&nbsp;*</td><td>    可以在產品上執行 Xbox Live 工具，以僅查看資料。  </td><td>    可以在產品上執行 Xbox Live 工具來查看和編輯資料。    </td><td>    N/A    </td><td>    N/A                      </td></tr>
</tbody>
</table>

\* 以星號標記的許可權 ( * ) 授與存取權給所有帳戶皆無法使用的功能。 如果您的帳戶尚未啟用這些功能，您對於這些權限所做的選擇不會有任何影響。