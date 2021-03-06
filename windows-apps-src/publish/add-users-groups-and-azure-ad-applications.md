---
description: 您可以將使用者、群組和 Azure AD 應用程式新增至您的合作夥伴中心帳戶。
title: 新增使用者、群組和 Azure AD 應用程式至合作夥伴中心帳戶
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10、uwp、azure ad 應用程式、aad、使用者、群組、多位使用者、多使用者
ms.localizationpriority: medium
ms.openlocfilehash: 59f3a69399b33164688da3c80c781828802662f5
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032881"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>新增使用者、群組和 Azure AD 應用程式至合作夥伴中心帳戶

[ **帳戶設定** ]) 的 [合作夥伴中心](https://partner.microsoft.com/dashboard) (的 [ **使用者** ] 區段可讓您使用 Azure Active Directory 將使用者新增至您的合作夥伴中心帳戶。 為每位使用者指派角色（或一組自訂權限），定義帳戶的存取權。 您也可以新增 [使用者群組](#groups) 和 [Azure AD 應用程式](#azure-ad-applications) ，以授與他們合作夥伴中心帳戶的存取權。

新增使用者到帳戶之後，您可以[編輯帳戶詳細資料](#edit)、變更[角色與權限](set-custom-permissions-for-account-users.md)，或[移除使用者](#remove)。

> [!IMPORTANT]
> 為了將使用者新增至您的帳戶，您必須先 [將合作夥伴中心帳戶與組織的 Azure Active Directory 租使用者建立關聯](associate-azure-ad-with-partner-center.md)。 

新增使用者時，您必須將 [角色或自訂許可權集合](set-custom-permissions-for-account-users.md)指派給他們，以指定他們對您合作夥伴中心帳戶的存取權。 

請記住，所有合作夥伴中心使用者 (包括群組和 Azure AD 應用程式) 都必須在 [與您的 Azure AD 帳戶相關聯的合作夥伴中心租](associate-azure-ad-with-partner-center.md)使用者中具有有效的帳戶。 使用者管理是一次在一個租用戶上進行；您必須使用要在其中新增或編輯使用者的租用戶的管理員帳戶登入。 在合作夥伴中心中建立新的使用者也會在您已登入的 Azure AD 租使用者中建立該使用者的帳戶，而在合作夥伴中心中變更使用者名稱將會在組織的 Azure AD 租使用者中進行相同的變更。

> [!NOTE]
> 如果您的組織使用[目錄整合](/previous-versions/azure/azure-services/jj573653(v=azure.100))來同步內部部署目錄服務與您的 Azure AD，您將無法在合作夥伴中心建立新的使用者、群組或 Azure AD 應用程式。 您 (或您內部部署目錄中的其他系統管理員) 必須直接在內部部署目錄中建立這些項目，才能在合作夥伴中心看到並新增這些項目。


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>將使用者新增至您的合作夥伴中心帳戶

若要將使用者加入您的合作夥伴中心帳戶，請移至 [ **帳戶設定** ] 中的 [ **使用者** ] 頁面，然後選取 [ **新增使用者]。** 您必須使用要在其中工作的 Azure AD 租用戶的管理員帳來登入。 

### <a name="add-existing-users"></a>新增現有的使用者 

您可以選取已存在於組織租使用者中的使用者，並讓他們存取您的合作夥伴中心帳戶。 

<span id="from-directory" />

1.  選取合作夥伴中心) 右上角附近的齒輪圖示 (，然後選取 [ **開發人員設定** ]。 在 [ **設定** ] 功能表中選取 [ **使用者** ]。
2.  從 **\[使用者\]** 頁面選取 **\[新增使用者\]** 。 
3.  從顯示的清單中選取一或多個使用者。 您可以使用搜尋方塊來搜尋特定使用者。
    > [!TIP]
    > 如果您選取多個要新增至合作夥伴中心帳戶的使用者，您必須為他們指派相同的角色或一組自訂許可權。 若要新增多個具有不同角色/權限的使用者，請針對每個角色或一組自訂權限重複下列步驟。
4.  當您完成選取使用者時，請按一下 [ **新增選取** 的]。
5.  在 [ **角色** ] 區段中，為選取的使用者 (的) 指定 [角色 () 或自訂許可權](set-custom-permissions-for-account-users.md) 。
6.  按一下 [儲存]。

### <a name="additional-methods-for-adding-users"></a>新增使用者的其他方法

如果您使用管理員帳戶登入，且該帳戶也擁有您正在使用之 Azure AD 租使用者的 [全域管理員](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 許可權，您將會有其他選項可將使用者新增至您的合作夥伴中心帳戶。 您必須選取下列其中一項：

-   **新增現有的使用者** ：選擇已存在於您組織目錄中的使用者，並使用上述方法，讓他們存取您的合作夥伴中心帳戶。
-   **建立新的使用者** ：建立全新的使用者帳戶，以新增至您組織的目錄和您的合作夥伴中心帳戶
-   **邀請外部使用者** ：傳送電子郵件邀請給目前不在您的組織目錄中的使用者。 系統會邀請他們存取您的合作夥伴中心帳戶，並且會在您的 Azure AD 租使用者中為他們建立新的 [來賓使用者](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) 帳戶。

<span id="new-user" />

### <a name="create-new-users"></a>建立新的使用者

> [!IMPORTANT]
> 您必須使用您的 Azure AD 租用戶中的全域管理員帳戶登入，才能建立新的使用者。

1.  從 [ **使用者** ] 頁面的 [ **帳戶設定** ]) 下 (，選取 [ **新增使用者** ]，然後選擇 [ **建立新使用者** ]。
2.  輸入新使用者的名字、姓氏及使用者名稱。
3.  如果您想要讓新使用者在您組織的目錄中具有 [全域管理員帳戶](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) ，請核取 [ **將此使用者設為您 Azure AD 中的全域管理員] 方塊，以完整控制所有目錄資源** 。 這可讓使用者完整存取您公司 Azure AD 中的所有系統管理功能。 除非您將適當的 [角色/許可權](set-custom-permissions-for-account-users.md)) 授與帳戶，否則他們將能夠在您組織的目錄中新增和管理使用者 (但不是合作夥伴中心。 如果您核取此方塊，必須為使用者提供 **密碼復原電子郵件** 。
4.  如果您選取 **\[將這個使用者設為 Azure AD 中的全域管理員\]** 方塊，請輸入使用者需要復原其密碼時可以使用的電子郵件。
5.  在 [群組成員資格] 區段中，選取新使用者所屬的任意群組。
6.  在 [ **角色** ] 區段中，指定 [角色 () 或使用者的自訂許可權](set-custom-permissions-for-account-users.md) 。
7.  按一下 [儲存]。
8.  在確認頁面上，您將會看見新使用者的登入資訊 (包括暫時密碼)。 請確定會記下此資訊，並將它提供給新的使用者，因為您在離開此頁面之後將無法存取該暫時密碼。


<span id="email" />

### <a name="invite-outside-users"></a>邀請外部使用者

> [!IMPORTANT]
> 您必須使用您的 Azure AD 租用戶中的全域管理員帳戶登入，才能邀請外部使用者。

1.  在 [ **使用者** ] (頁面的 [ **帳戶設定** ]) 底下，選取 [ **新增使用者** ]，然後選擇 [ **以電子郵件邀請使用者** ]。
1.  輸入一或多個以逗號或分號分隔的電子郵件地址 (最多 10 個)。
2.  在 [ **角色** ] 區段中，指定 [角色 () 或使用者的自訂許可權](set-custom-permissions-for-account-users.md) 。
3.  按一下 [儲存]。

您邀請的使用者會取得電子郵件邀請來加入您的帳戶，而在您的 Azure AD 租用戶中將為其建立新的[來賓使用者](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)帳戶。 每個使用者都必須接受其邀請，才能存取您的帳戶。

如果您需要重新傳送邀請，請在 **\[使用者\]** 頁面上尋找使用者，然後選取其電子郵件地址 (或選取表示 **邀請擱置中** 的文字)。 然後，在頁面底部，按一下 [重新傳送 **邀請** ]。

> [!IMPORTANT]
> 您邀請加入合作夥伴中心帳戶的外部使用者，可以與其他使用者獲指派相同的角色和許可權。 不過，外部使用者無法在 Visual Studio 中執行某些工作，例如將關聯應用程式至 Microsoft Store，或是建立套件以上傳至 Microsoft Store。 如果使用者需要執行這些工作，請選擇 **\[建立新的使用者\]** 而不是 **\[邀請外部使用者\]** 。 (如果您不想將這些使用者新增至您現有的 Azure AD 租用戶，您可以[建立新的租用戶](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)，然後在該租用戶中為他們建立新的使用者帳戶)。 


### <a name="changing-a-users-directory-password"></a>變更使用者的目錄密碼

如果您的其中一位使用者需要變更其密碼，則如果您在建立使用者帳戶時提供 **密碼復原電子郵件** ，就可以自行進行。 您也可以依照下列步驟更新使用者的密碼 (如果您使用 Azure AD 租用戶的全域管理員帳戶登入，以變更使用者的密碼)。 請注意，這會變更您 Azure AD 租用戶中的使用者密碼，以及用來存取合作夥伴中心的密碼。 

1.  從 [使用者] 頁面 (在 [帳戶設定] 下)，選取您要編輯的使用者帳戶名稱。
2.  選取頁面底部的 [重設密碼] 按鈕。
3.  隨即會出現確認頁面，顯示該使用者的登入資訊 (包括暫時密碼)。

    > [!IMPORTANT]
    >  請務必列印或複製此資訊，並提供給使用者，因為離開此頁面之後，就無法存取臨時密碼。

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>將群組新增至您的合作夥伴中心帳戶

您可以從組織的目錄將群組新增至您的合作夥伴中心帳戶。 當您這樣做時，隸屬於該群組成員的每位使用者都能使用與群組指派角色相關聯的權限來存取它。

### <a name="add-groups-from-your-organizations-directory"></a>從組織的目錄新增群組

1.  選取合作夥伴中心) 右上角附近的齒輪圖示 (，然後選取 [ **開發人員設定** ]。 在 [ **設定** ] 功能表中選取 [ **使用者** ]。
2. 在 [ **使用者** ] 頁面上，選取 [ **新增群組** ]。
2.  從顯示的清單中選取一或多個群組。 您可以使用搜尋方塊來搜尋特定群組。
    > [!TIP]
    > 如果您要選取多個群組新增至您的合作夥伴中心帳戶，您必須為其指派相同的角色或一組相同的自訂權限。 若要新增多個具有不同角色/權限的群組，請針對每個角色或一組自訂權限重複下列步驟。

3.  當您完成選取群組時，請按一下 [ **新增選取** 的]。
4.  在 [ **角色** ] 區段中，指定) 的角色 (，或針對選取的群組 (s) 的 [自訂許可權](set-custom-permissions-for-account-users.md) 。 群組的所有成員都可以存取您的合作夥伴中心帳戶和您套用至群組的許可權，而不論其個別帳戶相關聯的角色/許可權為何。
5.  按一下 [儲存]。


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>在您組織的目錄中建立新的群組帳戶，並將它新增至您的合作夥伴中心帳戶

如果您想要將合作夥伴中心存取權授與全新的群組，您可以在 [ **使用者** ] 區段中建立新的群組。 請注意，新的群組會建立在您組織的目錄中，而不是在您的合作夥伴中心帳戶中。

1.  從 [ **使用者** ] 頁面 (的 [ **開發人員設定** ]) 底下，按一下 [ **新增群組** ]。
2.  在下一個頁面上，選取 [新增群組]。
3.  輸入新群組的顯示名稱。
4.  指定 [角色 (s) 或群組的自訂許可權](set-custom-permissions-for-account-users.md) 。 群組的所有成員都可以存取您的合作夥伴中心帳戶和您套用至群組的許可權，而不論其個別帳戶相關聯的角色/許可權為何。
5.  從出現的清單中選取要指派至新群組的使用者。 您可以使用搜尋方塊來搜尋特定使用者。
6.  完成選擇使用者之後，請按一下 [新增選取的項目] 以將其新增至新群組。
7.  按一下 [儲存]。


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>將 Azure AD 應用程式新增至您的合作夥伴中心帳戶

您可以允許組織 Azure AD 中的應用程式或服務存取您的合作夥伴中心帳戶。 這些 Azure AD 應用程式使用者帳戶可以用來呼叫 [Microsoft Store 服務](../monetize/using-windows-store-services.md)提供的 REST API。


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>從組織的目錄新增 Azure AD 應用程式

1.  1.  選取合作夥伴中心) 右上角附近的齒輪圖示 (，然後選取 [ **開發人員設定** ]。 在 [ **設定** ] 功能表中選取 [ **使用者** ]。
2. 從 **\[使用者\]** 頁面，選取 **\[新增 Azure AD 應用程式\]** 。
3.  從顯示的清單中選取一或多個 Azure AD 應用程式。 您可以使用搜尋方塊來搜尋特定 Azure AD 應用程式。
    > [!TIP]
    > 如果您要選取多個 Azure AD 應用程式新增至您的合作夥伴中心帳戶，您必須為其指派相同的角色或一組相同的自訂權限。 若要新增多個具有不同角色/權限的 Azure AD 應用程式，請針對每個角色或一組自訂權限重複下列步驟。

4.  完成選擇 Azure AD 應用程式之後，請按一下 [新增選取的項目]。
5.  在 [ **角色** ] 區段中，為選取的 Azure AD 應用程式 () 指定 [角色 () 或自訂許可權](set-custom-permissions-for-account-users.md) 。
6.  按一下 [儲存]。


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>在您組織的目錄中建立新的 Azure AD 應用程式帳戶，並將其新增至您的合作夥伴中心帳戶

如果您想要將合作夥伴中心存取權授與全新的 Azure AD 應用程式帳戶，您可以在 [ **使用者** ] 區段中建立一個。 請注意，這會在您組織的目錄中建立新的帳戶，而不只是在您的合作夥伴中心帳戶中。

> [!TIP]
> 如果您主要是使用此 Azure AD 應用程式進行合作夥伴中心驗證，而且不需要使用者直接進行存取，您可以在 [回覆 URL] 和 [應用程式識別碼 URI] 輸入任意有效的位址，前提是您目錄中的其他 Azure AD 應用程式都不會使用這些值。

1.  從 [使用者] 頁面 (在 [帳戶設定] 下)，選取 [新增 Azure AD 應用程式]。
2.  在下一個頁面上，選取 [新增 Azure AD 應用程式]。
3.  輸入新 Azure AD 應用程式的 [回覆 URL]。 這是使用者可以登入並使用您 Azure AD 應用程式的 URL (有時也稱為「應用程式 URL」或「登入 URL」)。 [回覆 URL] 的長度不能超過 256 個字元，而且在您的目錄中必須是唯一的。
4.  輸入新 Azure AD 應用程式的 [應用程式識別碼 URI]。 這是 Azure AD 應用程式的邏輯識別碼，是在傳送單一登入要求至 Azure AD 時提供。 請注意，您目錄中每個 Azure AD 應用程式的 **應用程式識別碼 URI** 必須是唯一的，且不能超過256個字元。 如需 **應用程式識別碼 URI** 的詳細資訊，請參閱將 [應用程式與 Azure Active Directory 整合](/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant)。
5.  在 [ **角色** ] 區段中，指定 [角色 () 或](set-custom-permissions-for-account-users.md) Azure AD 應用程式的自訂許可權。
6.  按一下 [儲存]。

新增或建立 Azure AD 應用程式之後，您可以返回 [使用者] 區段，然後選取應用程式名稱以檢閱應用程式的設定，包括租用戶識別碼、用戶端識別碼、回覆 URL 和應用程式識別碼 URI。

> [!NOTE]
> 如果您想要使用 [Microsoft Store 服務](../monetize/using-windows-store-services.md)所提供的 REST API，您將需要此頁面上所顯示的 [租用戶識別碼] 和 [用戶端識別碼] 的值，以取得 Azure AD 存取權杖，您可以使用此存取權杖向服務驗證呼叫。   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>管理 Azure AD 應用程式的金鑰

如果您的 Azure AD 應用程式會在 Microsoft Azure AD 中讀取和寫入資料，則需要金鑰。 您可以藉由編輯合作夥伴中心中的資訊來建立 Azure AD 應用程式的金鑰。 您也可以移除不再需要的金鑰。

1.  從 [使用者] 頁面 (在 [帳戶設定] 下)，選取 Azure AD 應用程式的名稱。
    > [!TIP]
    > 按一下 Azure AD 應用程式的名稱，您就會看到該 Azure AD 應用程式所有使用中的金鑰，包括金鑰的建立和到期日期的資料。 若要移除不再需要的金鑰，請按一下 [ **移除** ]。

2.  若要新增金鑰，請選取 [新增金鑰]。
3.  您會看到顯示 **用戶端識別碼** 和 **金鑰** 值的畫面。
    > [!IMPORTANT]
    > 請務必複製此資訊，您離開這個頁面之後就無法再存取此資訊。

4.  如果您想要建立更多金鑰，請選取 [Add another key] \(新增其他金鑰\)。

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>編輯使用者、群組或 Azure AD 應用程式

將使用者、群組和/或 Azure AD 應用程式新增至您的合作夥伴中心帳戶之後，您就可以變更其帳戶資訊。 

> [!IMPORTANT]
> 對 [角色或許可權](set-custom-permissions-for-account-users.md) 所做的變更只會影響合作夥伴中心存取。 所有其他變更 (例如變更使用者的名稱或群組成員資格，或 Azure AD 應用程式) 的 [回復 URL] 和 [應用程式識別碼 URI]，將會反映在您組織的 Azure AD 租使用者以及合作夥伴中心帳戶中。 

1.  從 [ **使用者** ] 頁面的 [ **帳戶設定** ]) 底下 (選取您要編輯的使用者、群組或 Azure AD 應用程式帳戶的名稱。
2.  變更需要的項目。 可以編輯的項目如下：
    -   對於 **\[使用者\]** ，可以編輯使用者的名字、姓氏或使用者名稱。 您也可以選取或取消選取 **\[群組成員資格\]** 區段中的群組，以更新其群組成員資格。
    -   對於 **\[群組\]** ，您可以編輯群組的名稱  (若要更新群組成員資格，請編輯要在群組中新增或移除的使用者，並在 **\[群組成員資格\]** 區段中進行變更)。
    -   對於 **\[Azure AD 應用程式\]** ，您可以為 **\[回覆 URL\]** 或 **\[應用程式識別碼 URI\]** 輸入新值。
    請記住，這些變更將會在您組織的目錄中，以及您的合作夥伴中心帳戶中進行。
3.  針對合作夥伴中心存取相關的變更，請選取或取消選取您想要套用 (s) 的角色，或選取 [ **自訂許可權** ] 並進行所需的變更。 這些變更只會影響合作夥伴中心存取權，且不會變更組織 Azure AD 租使用者中的任何許可權。
3.  按一下 [儲存]。


## <a name="view-history-for-account-users"></a>檢視帳戶使用者歷程記錄

身為帳戶擁有者，您可以檢視已加入至帳戶中的任何其他使用者的詳細瀏覽記錄。

在 [ **使用者** ] 頁面的 [ **帳戶設定** ]) 底下 (下，選取您要審核其流覽歷程記錄之使用者的 [ **上次活動** ] 下顯示的連結。 您將可以檢視使用者在最近 30 天中瀏覽之所有頁面的 URL。

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>移除使用者、群組及 Azure AD 應用程式

若要從您的合作夥伴中心帳戶中移除使用者、群組或 Azure AD 應用程式，請在 [ **使用者** ] 頁面上，選取其名稱所顯示的 [ **移除** ] 連結。 確認您要移除該使用者、群組或 Azure AD 的應用程式之後，除非您稍後再新增) ，否則該使用者、群組或應用程式將無法再存取您的合作夥伴中心帳戶 (。

> [!IMPORTANT]
> 移除使用者、群組或 Azure AD 的應用程式，表示它將無法再存取您的合作夥伴中心帳戶。 它不會從您組織的目錄 **中刪除使用者** 、群組或 Azure AD 應用程式。

 
