<properties
    pageTitle="Nastavovat místní podmíněné připojení přes Azure Active Directory registrace zařízení | Microsoft Azure"
    description="Podrobný návod, jak povolit podmíněné přístup k místní aplikací pomocí federace služba Active Directory (AD FS) v systému Windows Server 2012 R2."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>


# <a name="setting-up-on-premises-conditional-access-using-azure-active-directory-device-registration"></a>Nastavovat místní podmíněné připojení přes Azure Active Directory zařízení registrace

Osobně vlastněná zařízení vašich uživatelů může být označen používá pro vaši organizaci uživatelích pracovišti spojení jejich zařízení pro službu Azure Active Directory registrace zařízení. Níže je podrobný návod, jak povolit podmíněné přístup k místní aplikací pomocí federace služba Active Directory (AD FS) v systému Windows Server 2012 R2.

> [AZURE.NOTE]
> Licence Office 365 nebo licenci Azure AD Premium požaduje při použití zařízení registraci v zásady podmíněné přístupu služby Azure Active Directory registrace zařízení. Jedná se o zásady nevynucují službou Active Directory Federation Services (AD FS) pro místní zdroje.

Další informace o podmíněném přístupu scénáře pro místní najdete v článku [připojení k pracovišti z libovolného zařízení pro jednotné přihlašování a bezproblémové druhý faktor ověřování přes společnosti aplikace](https://technet.microsoft.com/library/dn280945.aspx).

Tyto funkce jsou dostupné zákazníkům, kteří koupit licence pro Azure Active Directory Premium.

<a name="supported-devices"></a>Podporovaná zařízení
-------------------------------------------------------------------------
* Windows 7 domény připojen zařízení.
* Windows 8.1 osobní a domény spojeny zařízení.
* iOS 6 nebo novější pro prohlížeč Safari
* Android 4.0 nebo novější, Samsung GS3 nebo nad telefony, Poznámka Samsung 2 nebo vyšší tablety.


<a name="scenario-prerequisites"></a>Scénář požadavky
------------------------------------------------------------------------
* Předplatné Office 365 nebo Premium Azure Active Directory
* Tenanta služby Azure Active Directory
* Windows Server Active Directory (Windows Server 2008 nebo novější)
* Aktualizované schématu v systému Windows Server 2012 R2
* Licence pro Premium Azure Active Directory
* Windows Server 2012 R2 Federation Services nakonfigurován pro jednotné přihlašování Azure AD
* Windows serveru 2012 R2 webové aplikace proxy serveru Microsoft Azure Active Directory připojení (Azure AD Connect). [Stáhněte si Azure AD Connect tady](http://www.microsoft.com/en-us/download/details.aspx?id=47594).
* Ověření domény.

<a name="known-issues-in-this-release"></a>Známé problémy v této verzi
-------------------------------------------------------------------------------
* Zásady podmíněného přístupu zařízení na základě vyžadují zpětného zápisu objekt zařízení ke službě Active Directory ze služby Azure Active Directory. Může trvat až 3 hodiny u objektů zařízení zapsat zpět do služby Active Directory
* zařízení s iOS 7 se vždycky zobrazí výzvu k vyberte certifikát během ověřování certifikátu klienta.
* V některých verzích iOS8, před iOS 8.3 nefungují.

## <a name="scenario-assumptions"></a>Scénář předpoklady
Tento scénář předpokládá, že máte hybridního prostředí obsahující Azure AD klienta a na adresářová služba Active Directory. Tyto klienti by měl být připojen pomocí Azure AD Connect a ověřenou doménou a služby AD FS pro jednotné přihlašování. Následující kontrolní seznam můžete nakonfigurovat prostředí pro fázi jsme je popsali výše.

<a name="checklist-prerequisites-for-conditional-access-scenario"></a>Kontrolní seznam: Požadavcích na scénář podmíněné přístupu
--------------------------------------------------------------
Připojte vašeho klienta Azure AD pomocí na adresářová služba Active Directory.

## <a name="configure-azure-active-directory-device-registration-service"></a>Konfigurace služby Azure Active Directory zařízení registrace
Díky této příručce pro nasazení a konfiguraci Azure Active Directory zařízení registrace služby pro vaši organizaci.

Tato příručka předpokládá, že jste nakonfigurovali služby Active Directory pro Windows Server a přihlásili k odběru služby Microsoft Azure Active Directory. V tématu požadavky výše.

Nasazení Azure Active Directory zařízení registrace služby s vašeho klienta služby Azure Active Directory, udělali úkoly podle pokynů v následující kontrolního seznamu v pořadí. Po kliknutí na odkaz přejdete na toto téma, vraťte se do Tento kontrolní seznam když si prohlédnete Toto téma, takže můžete pokračovat ve zbývající úkoly v tomto kontrolním seznamu. Některé úkoly bude obsahovat kroku ověření scénáře, které se dozvíte, potvrďte, že v kroku byla úspěšně dokončena.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Část 1: Registrace zařízení Azure Active Directory povolit

Podle pokynů níže můžete povolit a nakonfigurovat službu registrace Azure Active Directory zařízení kontrolním seznamu.

| Úkol                                                                                                                                                                                                                                                                                                                                                                                             | Odkaz                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Povolte registrace zařízení ve vašem klientovi Azure Active Directory umožňuje zařízení připojit na pracovišti. Ve výchozím nastavení není povolený vícefaktorové ověřování pro službu. Vícefaktorové ověřování je vhodné při registraci zařízení. Před povolením vícefaktorové ověřování v ADRS, nakonfigurujte AD FS je vícefaktorové ověřování zprostředkovatele. | [Povolení registrace zařízení Azure Active Directory](active-directory-conditional-access-device-registration-overview.md)               |
| Zařízení zjistí služby registrace Azure Active Directory zařízení vyhledáním známý záznamy DNS. Vaše společnost DNS musíte nakonfigurovat tak, aby zařízení mohli zjišťovat služby registrace Azure Active Directory zařízení.                                                                                                                                                   | [Konfigurace zjišťování Azure Active Directory registrace zařízení.](active-directory-conditional-access-device-registration-overview.md) |

##<a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Část 2: Nasazení a konfiguraci Windows serveru 2012 R2 Active Directory Federation Services a nastavit vztah federaci s Azure AD

| Úkol                                                                                                                                                                                                                                                                                                                                                                                             | Odkaz                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Nasazení domény Active Directory Domain Services s rozšíření schématu Windows serveru 2012 R2. Některé z vaší domény řadiče upgradovat na Windows serveru 2012 R2 nepotřebujete. Upgrade schématu je pouze požadavek. | [Upgrade schéma služby Active Directory Domain Services](#upgrade-your-active-directory-domain-services-schema)               |
| Zařízení zjistí služby registrace Azure Active Directory zařízení vyhledáním známý záznamy DNS. Vaše společnost DNS musíte nakonfigurovat tak, aby zařízení mohli zjišťovat služby registrace Azure Active Directory zařízení.                                                                                                                                                   | [Příprava zařízení podpora služby Active Directory](#prepare-your-active-directory-to-support-devices) |


##<a name="part-3-enable-device-writeback-in-azure-ad"></a>Část 3: Povolit zařízení zpětným v Azure AD

| Úkol                                                                                                                                                                                                                                                                                                                                                                                             | Odkaz                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Vyplňte část 2 povolení zpětného zápisu zařízení v Azure AD Connect. Po dokončení vraťte se to této příručce. | [Povolení zpětného zápisu zařízení v Azure AD Connect](#upgrade-your-active-directory-domain-services-schema)               |


##<a name="optional-part-4-enable-multi-factor-authentication"></a>[Volitelné] Část 4: Povolit vícefaktorové ověřování

Důrazně doporučujeme nakonfigurovat jeden z několika možností pro vícefaktorové ověřování. Pokud chcete vyžadovat MFA, najdete v článku [Volba řešení Multi-Factor zabezpečení za vás](../multi-factor-authentication/multi-factor-authentication-get-started.md). Obsahuje popis jednotlivých řešení a odkazy, které umožňují konfigurovat řešení podle svého výběru.

## <a name="part-5-verification"></a>Část 5: ověření

Nasazení je nyní dokončen. Teď můžete vyzkoušet některé scénáře. Postupujte podle těchto odkazů umožňuje experimentovat se službou a seznamte se s funkcemi


| Úkol                                                                                                                                                                                                                         | Odkaz                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Spojení některých zařízení a vaším zaměstnáním pomocí Azure Active Directory registrace zařízení. Připojení ke iOS, Windows a zařízeních s Androidem                                                                                         | [Spojení zařízení a vaším zaměstnáním pomocí Azure Active Directory zařízení registrace](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Můžete zobrazit a povolit nebo zakázat registrovaných zařízení na portálu správce. V tomto úkolu zobrazí některá registrovaných zařízení na portálu správce.                                                        | [Přehled registrace zařízení Azure Active Directory](active-directory-conditional-access-device-registration-overview.md)                             |
| Ověřte, že objekty zařízení jsou zpět ze služby Azure Active Directory došlo k zápisu služby Active Directory pro Windows Server.                                                                                                                  | [Ověření jsou registrované zařízení zapsat zpět do služby Active Directory](#verify-registered-devices-are-written-back-to-active-directory)                  |
| Teď, když uživatelé můžou zaregistrovat jejich zařízení, můžete vytvořit aplikaci zásady přístupu v AD FS, pomocí kterých pouze registrovaných zařízení. V tomto úkolu, který chcete vytvořit pravidlo aplikace access a vlastní přístup k odepření zprávy | [Vytvoření zásady přístupu aplikace a vlastní zpráva o odepření přístupu](#create-an-application-access-policy-and-custom-access-denied-message)            |



## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Integrace služby Azure Active Directory s místní služby Active Directory
To vám umožní integrace vašeho klienta Azure AD s vaší místní služby active directory, pomocí Azure AD Connect. Přestože kroky k dispozici v portálu Azure klasické, poznamenejte si zvláštní pokyny uvedené v této části.

1.  Přihlaste se k portálu Azure klasické pomocí účtu, který slouží jako globální správce v Azure AD.
2.  V levém podokně vyberte **Služby Active Directory**.
3.  Na kartě **adresáře** vyberte adresář.
4.  Vyberte kartu **Integrace adresářů** .
5.  V části **nasazením a správou** postupujte podle kroků 1 až 3 integrovat do místního adresáře služby Azure Active Directory.
  1.    Přidání domény.
  2.    Instalace a spuštění Azure AD Connect: instalace Azure AD Connect postupujte podle následujících pokynů, [Vlastní instalace Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).
  3. Ověření a Správa synchronizace adresářů. Pokyny přihlašování jsou dostupné v tomto kroku.

  > [AZURE.NOTE]
  > Konfigurace federace se službou AD FS, jak je uvedeno v dokumentu spojeného výše. Konfigurace funkcí Náhled nepotřebujete.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Upgrade schématu Active Directory Domain Services

> [AZURE.NOTE]
> Upgrade schéma služby Active Directory nejde vrátit zpět. Je vhodné toto nejdřív provádět v testovacím prostředí.

1. Přihlaste se k řadiče domény pomocí účtu s oprávněními správce organizace a schématu správce.
2. Zkopírujte **[médií] \support\adprep** adresář a dílčí adresáře do jednoho z řadiče domény Active Directory.
3. [Médií]-li cestu k systému Windows Server 2012 R2 instalačního média.
4. Z příkazového řádku, přejděte v adresáři adprep a spustit: **adprep.exe/forestprep**. Postupujte podle pokynů na obrazovce a provést upgrade schématu.

## <a name="prepare-your-active-directory-to-support-devices"></a>Příprava služby Active Directory podpora zařízení

>[AZURE.NOTE] Toto je jednorázové operaci, která je třeba spustit Příprava doménové služby Active Directory podpora zařízení. Musíte být přihlášení s oprávněními správce organizace a doménové služby Active Directory musí mít schématu Windows serveru 2012 R2 k provedení tohoto postupu.


##<a name="prepare-your-active-directory-forest-to-support-devices"></a>Příprava doménové služby Active Directory podpora zařízení

> [AZURE.NOTE]
>Toto je jednorázové operaci, která je třeba spustit Příprava doménové služby Active Directory podpora zařízení. Musíte být přihlášení s oprávněními správce organizace a doménové služby Active Directory musí mít schématu Windows serveru 2012 R2 k provedení tohoto postupu.

### <a name="prepare-your-active-directory-forest"></a>Příprava doménové služby Active Directory

1.  Na federačním serveru otevřete okna příkazového prostředí Windows PowerShell a zadejte: Inicializace ADDeviceRegistration
2.  Po zobrazení výzvy k **ServiceAccountName**, zadejte název účtu služby, kterou jste vybrali jako účet služby pro službu AD FS. Pokud se jedná o gMSA účet, zadejte účet do formátu **domain\accountname Kč** . Pro účet domény použijte formát **domain\accountname**.



### <a name="enable-device-authentication-in-ad-fs"></a>Povolit ověřování zařízení ve službě AD FS

1. Na federačním serveru, spusťte konzolu Správa služby AD FS a přejděte na **AD FS** > **Zásad ověřování**.
2. Vyberte **Upravit globální primární ověřování** v podokně **akcí** .
3. Zaškrtněte **Povolit ověřování zařízení** a pak vyberte**OK**.
4. Ve výchozím nastavení služby AD FS pravidelně odeberete nepoužitý zařízení ze služby Active Directory. Tohoto úkolu musíte zakázat při použití Azure Active Directory registrace zařízení tak, aby zařízení můžete spravovat v Azure.


### <a name="disable-unused-device-cleanup"></a>Zakázání vyčištění nepoužitý zařízení
1. Na federačním serveru otevřete okna příkazového prostředí Windows PowerShell a zadejte: Set-AdfsDeviceRegistration - MaximumInactiveDays 0

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Příprava Azure AD Connect zpětného zápisu zařízení

1.  Vyplňte část 1: Příprava Azure AD připojení.


## <a name="join-devices-to-your-workplace-using-azure-active-directory-device-registration"></a>Spojení zařízení a vaším zaměstnáním pomocí Azure Active Directory zařízení registrace

### <a name="join-an-ios-device-using-azure-active-directory-device-registration"></a>Připojení pomocí Azure Active Directory zařízení registrace zařízení s iOS

Azure Active Directory registrace zařízení používá proces zápisu Over-the-Air profilu pro zařízení s iOS. Tento proces začíná připojení k zápisu adresa URL profilu ve webovém prohlížeči Safari uživatelského. Formát adresy URL vypadá takto:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Kde `yourdomainname` je název domény, který jste nastavili s Azure Active Directory. Pokud váš název domény je contoso.com, adresu URL by být například:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Komunikace tato adresa URL uživatelům mnoha různými způsoby. Jedním ze způsobů doporučené je publikovat tato adresa URL ve vlastní aplikaci přístup odepřen zprávy ve službě AD FS. Toto je uvedené v části nadcházející: [Vytvoření zásad aplikace access a vlastní zpráva o odepření přístupu](#create-an-application-access-policy-and-custom-access-denied-message).

###<a name="join-a-windows-81-device-using-azure-active-directory-device-registration"></a>Připojení pomocí Azure Active Directory zařízení registrace zařízení s Windows 8.1

1. Na zařízení s Windows 8.1, přejděte na **Nastavení počítače** > **sítě** > **pracovišti**.
2. Zadejte svoje uživatelské jméno v UPN formátu. Například dan@contoso.com..
3. Vyberte **Připojit se**.
4. Po zobrazení výzvy, přihlaste se pomocí svých přihlašovacích údajů. Zařízení se připojili.

###<a name="join-a-windows-7-device-using-azure-active-directory-device-registration"></a>Připojení zařízení s Windows 7 pomocí Azure Active Directory zařízení registrace
K registraci Windows 7 domény připojen zařízení, které potřebujete pro nasazení balíčku software registrace zařízení. Balíček softwaru se nazývá pracovišti připojit se pro systém Windows 7 a je k dispozici ke stažení na [webu Microsoft připojit](https://connect.microsoft.com/site1164). Pokyny, jak se použil funkci balíček jsou k dispozici na [Konfigurovat automatické zařízení registrace pro systém Windows 7 doménu připojen zařízení](active-directory-conditional-access-automatic-device-registration-windows7.md).

### <a name="join-an-android-device-using-azure-active-directory-device-registration"></a>Připojení zařízení s Androidem pomocí Azure Active Directory zařízení registrace

[Azure ověřovacích dat pro Android téma](active-directory-conditional-access-azure-authenticator-app.md) obsahuje pokyny o tom, jak nainstalujete Azure ověřovacích dat na zařízení s Androidem a přidejte pracovní účet. Vytvoření pracovního účtu úspěšně na zařízení s Androidem, je toto zařízení pracovišti připojen k organizace.

## <a name="verify-registered-devices-are-written-back-to-active-directory"></a>Ověření jsou registrované zařízení zapsat zpět do služby Active Directory
Můžete zobrazit a ověřte, že vaše zařízení objekty napsat zpátky ke službě Active Directory pomocí LDP.exe nebo upravit ADSI. Obě jsou dostupné v sekci nástroje Active Directory správce.

Ve výchozím nastavení se objekty zařízení, které jsou napsali zpět ze služby Azure Active Directory umístí do stejné domény jako farmě služby AD FS.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Vytvoření zásady přístupu aplikace a vlastní zpráva o odepření přístupu
Zvažte následující scénář: vytvoření aplikace Relying stran zabezpečení ve službě AD FS a konfigurace ověřovací pravidlo vydávání umožňující pouze registrovaných zařízení. Nyní jsou povoleny pouze zařízení, které jsou registrované pro přístup k aplikaci. Pro usnadnění pro uživatele k získání přístupu k aplikaci, konfigurovat vlastní přístup odepřen zprávu, která obsahuje pokyny k připojení svého zařízení. Teď vaši uživatelé mají bezproblémové způsob, jak zaregistrovat jejich zařízení při přístupu k aplikaci.

Jak implementovat tento scénář se zobrazí následující kroky.

>[AZURE.NOTE]
Tato část se předpokládá, že jste už nakonfigurovali může stran zabezpečení aplikace ve službě AD FS.

1. Otevřete nástroj AD FS MMC a přejděte na AD FS > vztahy důvěryhodnosti > může stran považuje za důvěryhodnou.
2. Vyhledejte aplikaci, pro který toto nové pravidlo aplikace access použije. Klikněte pravým tlačítkem myši aplikace a vyberte upravit pravidla deklarace...
3. Vyberte kartu **Pravidel pro vydávání oprávnění** a pak vyberte **Přidat pravidlo**
4. Z **deklarace pravidlo** šablony rozevírací seznam vyberte **Povolit nebo odepřít uživatelé založené na deklaraci příchozí**. Vyberte **Další**.
5. Do pole Název pravidla deklarace: typ pole: **Povolit přístup z registrovaných zařízení**
6. Příchozí typ deklarace: rozevírací seznam, vyberte **Je registrované uživatele**.
7. V příchozí hodnotu deklarace: zadejte: **PRAVDA**
8. Na přepínač **Povolit přístup k uživatelé, kteří mají tento příchozí deklarace** .
9. Vyberte **Dokončit** a pak vyberte **použít**.
10. Odebrání všech pravidel, která jsou opravňující více než pravidlo, které jste právě vytvořili. Například odeberte výchozí pravidlo **Povolit přístup ke všem uživatelům** .

Aplikace je teď nakonfigurované přístupu pouze v případě, že uživatel přichází ze zařízení, které jsou registrované a připojen k práci. Zásady pokročilejší přístup v tématu [Správa rizika pomocí řízení přístupu Multi-Factor](https://technet.microsoft.com/library/dn280949.aspx).

Pak nakonfigurujete vlastní chybové zprávy aplikace. Chybová zpráva umožní uživatelům vědět, že musíte připojování svého zařízení na pracovišti před mohou přístup k aplikaci. Můžete vytvořit vlastní aplikace přístup odepřen zprávy pomocí vlastní kód HTML a prostředí Windows PowerShell.

Na federačním serveru otevřete okno příkazového prostředí Windows PowerShell a zadejte tento příkaz. Nahraďte částí příkazu položky, které jsou specifické pro systému:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Pokud získat přístup k této aplikaci se musíte zaregistrovat zařízení.

**Pokud používáte zařízení s iOS vyberte tento odkaz připojit se ke zařízení**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Toto zařízení iOS spojit s vaším zaměstnáním.


**Pokud používáte zařízení s Windows 8.1**, můžete připojit zařízení tak, že přejdete na **Nastavení počítače ** >  **sítě** > **pracovišti**.


Kde "**může název zabezpečení strany**" je název aplikace může zabezpečení stran objektu ve službě AD FS.
Kde **yourdomain.com** je název domény, který jste nastavili s Azure Active Directory. Například contoso.com.
Ujistěte se, pokud chcete odebrat všechny konce řádků (pokud existuje) z obsahu html, který předat rutinu **Set-AdfsRelyingPartyWebContent** .


Teď když uživatelům přístup k aplikaci ze zařízení, které není registrovaný služby Azure Active Directory zařízení registrace, tato osoba obdrží stránky, který bude vypadat podobně jako kopii pod obrazovky.

![Screeshot chybu při uživatelé ještě registrovaný u Azure AD svého zařízení](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)

##<a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
