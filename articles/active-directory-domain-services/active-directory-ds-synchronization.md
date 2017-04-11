<properties
    pageTitle="Azure Active Directory Domain Services: Synchronizace ve spravovaných doménách | Microsoft Azure"
    description="Princip synchronizace v spravovaných domain Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Synchronizace v doméně spravovaných Azure AD Domain Services
Následující obrázek znázorňuje, jak synchronizace funguje služby Azure AD doménu spravovaný domény.

![Topologie synchronizace služby Azure AD domény](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Synchronizace z místního adresáře do vašeho tenanta Azure AD
Azure AD Connect synchronizace se používá k synchronizaci uživatelských účtů, skupina členství a vytvoří hodnotu hash pověření ke tenantovi Azure AD. Atributy uživatelské účty například s UPN a místních jsou synchronizovány ID zabezpečení (identifikátor zabezpečení). Pokud používáte Azure AD Domain Services, starší verze pověření hash potřebných pro ověřování NTLM a Kerberos synchronizovány taky ke tenantovi Azure AD.

Pokud jste nakonfigurovali zpětného zápisu, změny, které se adresáři Azure AD jsou synchronizovány zpátky na adresářová služba Active Directory. Třeba změnit heslo pomocí funkce Samoobslužné heslem změnit Azure AD-li změnit heslo se aktualizuje ve vaší místní domény AD.

> [AZURE.NOTE] Vždy použijte nejnovější verzi Azure AD Connect zajistit, že máte opravy všechny známé chyby.


## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Synchronizace z vašeho klienta Azure AD pro vaši doménu spravovaný
Uživatelské účty, členství ve skupinách a hash přihlašovacích údajů se synchronizují z vašeho klienta Azure AD pro vaši doménu spravovaný Azure AD Domain Services. Tento proces synchronizace je automaticky. Konfigurace, sledovat nebo spravovat tohoto procesu synchronizace nepotřebujete. Synchronizaci je také více-way/jednosměrný charakter. Spravované domény je převážně jen pro čtení s výjimkou všechny vlastní organizačních jednotek vytvoříte. Proto je nelze měnit uživatelích, uživatelská hesla nebo členství ve skupinách v rámci spravované domény. Výsledkem je obráceném synchronizace změn z vaší domény spravovaných zpátky ke tenantovi Azure AD.


## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Synchronizace z více doménové místního prostředí
Mnoho organizací má infrastruktura identit poměrně složité místního tvořené několika strukturami účtu. Azure AD Connect podporuje synchronizace uživatelé, skupiny a hash přihlašovacích údajů z více doménové prostředí ke tenantovi Azure AD.

Azure AD klienta je naopak mnohem jednodušší a ploché obor. Povolit uživatelům problémy se spolehlivým přístup k aplikacím zajištěná Azure AD, vyřešení konfliktů UPN mezi uživatelskými účty v různých doménových. Vaše medvědi spravované domény Azure AD Domain Services zavřete podobný vašemu klientovi Azure AD. Proto na se zobrazují ploché strukturu OU spravované domény. Všem uživatelům a skupinám jsou uložena v kontejneru AADDC uživatelé bez ohledu na místní domény nebo doménové, ze kterého jste synchronizovali v. Nakonfigurovali OU hierarchické struktury místní. Však spravované domény pořád má jednoduchou strukturu ploché OU.


## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Vyloučení – co se nesynchronizuje na vaši doménu spravovaný
Následující objekty a atributy nejsou synchronizovány ke tenantovi Azure AD nebo spravované domény:

- **Vyloučené atributy:** Můžete vyloučíte určité atributy synchronizaci ke tenantovi Azure AD z vaší domény místního pomocí Azure AD Connect. Tyto vyloučené atributy nejsou k dispozici ve vaší doméně spravovaných.

- **Seskupení zásad:** Na vaši doménu spravovaný nejsou synchronizovány nakonfigurovali ve vaší doméně místních zásad skupiny.

- **SYSVOL sdílet:** Podobně obsah sdílet SYSVOL ve vaší místní doméně nejsou synchronizovány na vaši doménu spravovaný.

- **Objekty počítače:** Objekty počítače pro počítače připojené k vaší místní domény nejsou synchronizovány spravované domény. Tyto počítače nemají vztah důvěryhodnosti s spravované domény a patří do vaší místní domény. Ve vaší doméně spravovaných vyhledání objektů počítače pouze u počítačů, které se mají explicitně doméně spravovaných doménu.

- **SidHistory atributy pro uživatele a skupiny:** Primární uživatelů a skupin primární ID zabezpečení z vaší domény místního jsou synchronizovány spravované domény. Však existující SidHistory atributy pro uživatele a skupiny nejsou synchronizují z místní domény na vaši doménu spravovaný.

- **Struktury organizace jednotky (OU):** Organizačních jednotek definované ve vaší místní doméně nejsou synchronizovány na vaši doménu spravovaný. Spravované domény existují dva předdefinované organizačních jednotek. Ve výchozím nastavení obsahuje spravovaný domény strukturu ploché OU. Můžete však [vytvářet vlastní organizační jednotce ve vaší doméně spravovaných](./active-directory-ds-admin-guide-create-ou.md).


## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Jak konkrétní atributy se synchronizují na vaši doménu spravovaný
Následující tabulka uvádí některé běžné atributy a popisuje, jak se synchronizují na vaši doménu spravovaný.

| Atribut spravované domény | Zdroje | Poznámky |
|:---|:---|:---|
|UPN|Atribut UPN uživatele ve vašem klientovi Azure AD|Atribut UPN z Azure AD klienta je synchronizovaný, stejně jako na vaši doménu spravovaný. Proto většina spolehlivé způsob, jak se přihlásit na vaši doménu spravovaný používá vaše UPN.|
|SAMAccountName|Uživatele mailNickname atribut ve vašem klientovi Azure AD nebo automaticky generované|Atribut SAMAccountName pocházející z atribut mailNickname ve vašem klientovi Azure AD. Atribut stejné mailNickname máte víc uživatelských účtů SAMAccountName je automaticky generované. Pokud mailNickname nebo UPN předponu uživatele je delší než 20 znaků, SAMAccountName je automaticky generované uspokojili limit 20 znaků SAMAccountName atributy.|
|Hesla|Heslo uživatele z Azure AD klienta|Přihlašovací údaje hash potřebných pro NTLM nebo Kerberos ověřování (nazývané také doplňkové přihlašovací údaje) se synchronizují z Azure AD klienta. Pokud klienta Azure AD je synchronizované klienta, těchto přihlašovacích údajů pocházející z místní domény.|
|Primární uživatele nebo skupinu ID zabezpečení|Automaticky generované|Primární číslo pro účty uživatele nebo skupinu je automaticky generované spravované domény. Tenhle atribut neodpovídá primární uživatele nebo skupinu ID zabezpečení objektu ve vaší místní domény AD. Tento stav totiž spravovaných doména má jiný obor názvů ID zabezpečení než místní domény.|
|Historie ID zabezpečení pro uživatele a skupiny|Místní primární uživatelů a skupin ID zabezpečení|Atribut SidHistory uživatelů a skupin ve vaší doméně spravovaných nastavenou podle odpovídající primární uživatel nebo skupina ID zabezpečení ve vaší místní doméně. Tato funkce pomáhá usnadnit výtah a směny místní aplikací na spravované doménu, protože se nemusí nové ACL zdroje.|

> [AZURE.NOTE] **Přihlaste se k spravované domény pomocí formátu UPN:** Atribut SAMAccountName může být automaticky generované pro některé uživatelské účty ve vaší doméně spravovaných. Pokud více uživatelů mají stejný atribut mailNickname nebo mají uživatelé příliš dlouhá předpony UPN, může být SAMAccountName pro tyto uživatele automaticky generované. Proto SAMAccountName formát (například "CONTOSO100\joeuser") je vždy spolehlivé způsob, jak se přihlásit k doméně. Automaticky generované SAMAccountName uživatelů se může lišit od jejich Přípony předpony. Použití formátu Přípony (třeba 'joeuser@contoso100.com') pro přihlášení k aplikaci spravované domény problémy se spolehlivým.


## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Objekty, které nejsou synchronizovány ke tenantovi Azure AD z spravované domény
Podle popisu v předchozí části tohoto článku, je synchronizace z vaší domény spravovaných zpátky ke tenantovi Azure AD. Můžete [vytvořit vlastní organizační jednotce (OU)](./active-directory-ds-admin-guide-create-ou.md) ve vaší doméně spravovaných. Kromě toho můžete vytvořit jiné organizačních jednotek, uživatelů, skupin nebo účty služby v rámci tyto vlastní organizačních jednotek. Žádná z objekty vytvořené v rámci vlastní organizačních jednotek jsou synchronizovány zpátky ke tenantovi Azure AD. Tyto objekty jsou k dispozici pouze v rámci spravované domény. Tyto objekty jsou proto není viditelná pomocí rutin prostředí PowerShell Azure AD, API Azure AD grafu nebo správou Azure AD uživatelského rozhraní.


## <a name="related-content"></a>Související obsah
- [Funkce - Azure AD Domain služby](active-directory-ds-features.md)

- [Scénáře nasazení - Azure AD Domain Services](active-directory-ds-scenarios.md)

- [Co byste měli zvážit sítě pro službu Azure AD Domain Services](active-directory-ds-networking.md)

- [Začínáme se službou Azure AD domény](active-directory-ds-getting-started.md)
