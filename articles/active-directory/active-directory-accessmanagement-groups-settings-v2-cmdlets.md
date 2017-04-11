<properties
    pageTitle="Azure Active Directory PowerShell náhled rutiny pro správu skupiny v Azure AD | Microsoft Azure"
    description="Tato stránka obsahuje příklady Powershellu ke správě skupin v Azure Active Directory"
    keywords="Azure AD, Azure Active Directory, Powershellu, Správa skupin, skupiny"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory náhled rutiny pro správu skupin

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-groups-create-azure-portal.md)
- [Azure klasické portálu](active-directory-accessmanagement-manage-groups.md)
- [Prostředí PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Následující dokument poskytuje s příklady použití Powershellu ke správě skupin služby Azure Active Directory (Azure AD).  Obsahuje taky informace o tom, jak nastavit modul Azure AD PowerShell náhled. Nejdřív musíte [stažení modulu Azure AD Powershellu](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>Instalace modulu Azure AD PowerShell

Instalace modulu AzureAD PowerShell náhled, použijte následující příkazy:

    PS C:\Windows\system32> install-module azureadpreview

Potvrďte, že byl nainstalovaný modul Náhled použijte tento příkaz:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

Teď můžete začít používat rutin v modulu. Úplný popis rutin v modulu AzureAD náhledu získáte [následující dokumentaci pro referenci](https://msdn.microsoft.com/library/azure/mt757216.aspx).

## <a name="connecting-to-the-directory"></a>Připojení k adresáři
Před spuštěním Správa skupiny pomocí rutin prostředí Azure AD náhled, musíte se připojit relaci Powershellu adresář, který chcete spravovat. K tomuto účelu použijte tento příkaz:

    PS C:\Windows\system32> Connect-AzureAD -Force

Rutiny zobrazí výzvu k zadání přihlašovacích údajů, které chcete použít pro přístup k adresáři. V tomto příkladu jsme používáte karen@drumkit.onmicrosoft.com pro přístup k adresáři ukázku. Rutiny vrátí potvrzení otevřené že relace byl úspěšně připojen k adresáři:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Teď můžete začít používat rutiny náhled AzureAD ke správě skupin v adresáři.

## <a name="retrieving-groups"></a>Načítání skupiny
Pokud chcete načíst existující skupiny z adresáře můžete použít rutinu Get-AzureADGroups. K načtení všechny skupiny v adresáři, získáte pomocí rutiny bez parametrů:

    PS C:\Windows\system32> get-azureadgroup

Rutiny vrátí všechny skupiny v adresáři připojení.

K načtení do určité skupiny, u kterého zadáte objektu skupiny můžete použít parametr – objektu:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Rutiny nyní vrátí skupiny, jejichž objektu odpovídá hodnota parametru, který jste zadali:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Můžete vyhledat určité skupiny pomocí parametr - filtr. Tento parametr používá klauzuli filtru ODATA a vrací všechny skupiny, které odpovídají filtru jako v následujícím příkladu:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Všimněte si, že OData query standard implementovat rutiny prostředí AzureAD PowerShell Další informace najdete [tady](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Vytváření skupin
Vytvoření nové skupiny v adresáři, použijte rutinu New-AzureADGroup. Tato rutina vytvoří novou skupinu zabezpečení s názvem "Marketing":

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Aktualizace skupin
Aktualizovat existující skupiny, použijte rutinu Set-AzureADGroup. V tomto příkladu jsme měníte vlastnost DisplayName skupiny "Správci Intune." Nejdřív hledáte skupiny pomocí rutiny Get-AzureADGroup a filtrovat pomocí atribut DisplayName:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Pak nám chcete změnit vlastnosti Popis na novou hodnotu "Správci zařízení Intune":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Teď když jsme Najděte skupinu vidíme vlastnosti popis aktualizuje tak, aby obsahovalo novou hodnotu:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Odstranění skupiny
Skupiny z adresáře, můžete odstranit rutinu AzureADGroup odebrat takto:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Správa členů skupin
Pokud potřebujete k přidání nových členů do skupiny, získáte pomocí rutiny AzureADGroupMember přidat. Příkaz přidá členem skupiny Intune Administrators, kterou jsme použili v předchozím příkladu:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Parametr – objektu je objektu skupiny, ke kterému chcete přidat člena a RefObjectId – je objektu uživatele, kterého chcete přidat jako člena do skupiny.

Stávající členy skupiny, použijte rutinu Get-AzureADGroupMember jako v následujícím příkladu:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Odebrání člena, kterého jsme dříve přidali do skupiny, získáte pomocí rutiny odebrat AzureADGroupMember, jak je znázorněno zde:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Pokud chcete ověřit membership(s) skupiny uživatele, získáte pomocí rutiny vyberte AzureADGroupIdsUserIsMemberOf. Tato rutina používá jako parametrů objektu uživatele, u kterého chcete zkontrolovat členství ve skupinách a seznam skupin, u kterého chcete zkontrolovat členství. Seznam skupin, musí být za předpokladu, že ve formuláři komplexního proměnné typu "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck", takže nejdřív musí vytvoříme proměnnou s tímto typem:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Další nabízíme hodnoty pro identifikátory skupiny se změnami atribut "Identifikátory skupiny" tuto proměnnou komplexního:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Teď Pokud nám chcete zkontrolovat členství ve skupinách uživatele s objektu 72cd4bbd-2594-40a2-935c-016f3cfeeeea proti skupin v $g, by měl používáme:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Vrácená hodnota je seznam skupin, které tento uživatel členem. Můžete taky použít tento způsob kontrola kontaktů, skupiny nebo objekty služby členství pro daný seznam skupin, pomocí vyberte AzureADGroupIdsContactIsMemberOf, vyberte AzureADGroupIdsGroupIsMemberOf nebo vyberte AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="managing-owners-of-groups"></a>Správa vlastníků skupiny
Pokud chcete přidat do skupiny Vlastníci, použijte rutinu AzureADGroupOwner přidat:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Parametr – objektu je objektu skupiny, ke kterému chcete přidat některé vlastníky a RefObjectId – je objektu uživatele, kterého chcete přidat jako některé vlastníky skupiny.

K načtení vlastníků skupiny, použijte Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Rutiny vrátí seznam vlastníků pro určité skupiny:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Pokud chcete některé vlastníky odebrat ze skupiny, použijte AzureADGroupOwner odebrat:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Další kroky

Následující dokumentaci pro další Azure Active Directory PowerShell můžete najít v [Azure Active Directory rutiny](http://go.microsoft.com/fwlink/p/?LinkId=808260).

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)

* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
