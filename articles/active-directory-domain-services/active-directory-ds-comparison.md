<properties
    pageTitle="Azure AD Domain Services: Porovnání Domain Azure AD služby – SVÉPOMOCNÝ domény řadiče | Microsoft Azure"
    description="Porovnejte Azure Active Directory Domain Services – SVÉPOMOCNÝ domény řadiče"
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
    ms.date="10/01/2016"
    ms.author="maheshu"/>

# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Jak se rozhodnout, pokud Azure AD Domain Services je nejlepší pro váš případ použití
Azure AD Domain Services umožňuje nasadit úloh infrastruktury služby Azure, aniž byste museli starat o ochraně infrastrukturu identity. Tato služba spravovaných se liší od typické nasazení služby Active Directory pro Windows Server, které nasazovat a spravovat sami. Služba je určený pro usnadnění nasazení automatizovaných zdraví a remediation a sledování infrastrukturu jednoduché identity pro cloudu. Jsme neustále vyvíjí služby pro podporu pro běžné scénáře nasazení přidat.

Rozhodnout, jestli se má použít Azure AD Domain Services nebo číselník nahoru a spravovat vlastní (samoobslužné) v Azure AD infrastrukturu:

- Zobrazit seznam [funkcí nabízené služby Azure AD domény](active-directory-ds-features.md).

- Zkontrolujte běžné [scénáře nasazení služby Azure AD domény](active-directory-ds-scenarios.md).

- Nakonec [porovnání Azure AD Domain Services možnost vlastními AD](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).


## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Srovnání služeb Azure AD Domain doménu – SVÉPOMOCNÝ AD v Azure
V následující tabulce vám pomůže se rozhodnout mezi Azure AD Domain Services a správě AD infrastruktury v Azure.

|**Funkce**|**Azure AD Domain Services**|**"Vlastními" AD v Azure VMs**|
|---|:---:|:---:|
|[**Služba spravovaných**](active-directory-ds-comparison.md#managed-service)|**& #x 2713;**|**& #x 2715;**|
|[**Zabezpečené nasazení**](active-directory-ds-comparison.md#secure-deployments)|**& #x 2713;**|Správce musí zajistit nasazení.|
|[**DNS server**](active-directory-ds-comparison.md#dns-server)|**& #x 2713;** (služba spravovaných)|**& #x 2713;**|
|[**Oprávnění správce domény nebo Enterprise**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|**& #x 2715;**|**& #x 2713;**|
|[**Připojení k doméně**](active-directory-ds-comparison.md#domain-join)|**& #x 2713;**|**& #x 2713;**|
|[**Ověření domény pomocí NTLM a Kerberos**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|**& #x 2713;**|**& #x 2713;**|
|[**Vlastní OU strukturu**](active-directory-ds-comparison.md#custom-ou-structure)|**& #x 2713;**|**& #x 2713;**|
|[**Rozšíření schématu**](active-directory-ds-comparison.md#schema-extensions)|**& #x 2715;**|**& #x 2713;**|
|[**Vztah důvěryhodnosti AD domény a struktury**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|**& #x 2715;**|**& #x 2713;**|
|[**Přečtěte si LDAP**](active-directory-ds-comparison.md#ldap-read)|**& #x 2713;**|**& #x 2713;**|
|[**Zabezpečený LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap)|**& #x 2713;**|**& #x 2713;**|
|[**Zápis LDAP**](active-directory-ds-comparison.md#ldap-write)|**& #x 2715;**|**& #x 2713;**|
|[**Zásady skupiny**](active-directory-ds-comparison.md#group-policy)|Jednoduché|Úplné|
|[**Distribuované GEO nasazení**](active-directory-ds-comparison.md#geo-dispersed-deployments)|**& #x 2715;**|**& #x 2713;**|


#### <a name="managed-service"></a>Služba spravovaných
Azure AD Domain Services domény spravuje Microsoft. Nemusíte bát opravy, aktualizace, sledování, zálohování a zajištění dostupnosti vaší domény. Tyto úkoly správy nabízejí jako služba Microsoft Azure spravované domény.

#### <a name="secure-deployments"></a>Zabezpečené nasazení
Spravované bezpečně uzamknutí domény dolů podle společnosti Microsoft zabezpečení doporučené postupy pro nasazení AD. Tyto doporučené postupy přerušili od produktového týmu AD desetiletí základním konstrukce a podporu AD nasazení. Samoobslužné nasazení budete muset kroků nasazení můžete uzamknout dolů/zabezpečené nasazení.

#### <a name="dns-server"></a>DNS server
Spravované domain Azure AD Domain Services obsahuje spravovaný služby DNS. Členové skupiny "AAD Datacentrum správci" můžete spravovat DNS spravovaný domény. Členové této skupiny se udělí oprávnění úplné správy DNS pro doménu spravovaný. Správa služby DNS lze provést pomocí "DNS konzole pro správu" součástí balíček Vzdálenou správu serveru nástroje ().

#### <a name="domain-or-enterprise-administrator-privileges"></a>Domény nebo podnikový správce oprávnění
Tyto zvýšenými oprávněními nejsou nabízeny na doménu spravovaný AAD Pošta. Aplikace, které vyžadují tyto zvýšenými oprávněními, abyste mohli mít nainstalovaný/spustit nejde spustit proti spravované domény. Menší podmnožiny administrativním oprávněním neexistuje členům skupiny delegovanou správu s názvem "AAD Datacentrum správci". Tato oprávnění zahrnout oprávněními, abyste mohli konfigurovat DNS, konfigurace zásad skupiny, získat oprávnění správce v doméně počítačích atd.

#### <a name="domain-join"></a>Připojení k doméně
Podobně jako jak připojit počítače k doméně služby AD spravovaného doménu se můžete připojit virtuálních počítačích.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Ověření domény pomocí NTLM a Kerberos
Azure AD Domain Services můžete pomocí přihlašovacích údajů podnikové ověření s spravované domény. Přihlašovací údaje jsou synchronizovány s Azure AD klienta. Synchronizované klienti Azure AD Connect zajištěna synchronizaci změn do udělali pověření místního Azure AD. S nastavením – SVÉPOMOCNÝ domény budete muset nastavit vztah důvěryhodnosti domény s doménové poskytujícího místní uživatelé mohli ověřovat pomocí své firemní přihlašovacích údajů. Střídavě budete muset nastavit AD replikace zajistit, aby platnost uživatelského hesla synchronizovat do vašeho Azure domény řadiče domény virtuálních počítačích.

#### <a name="custom-ou-structure"></a>Vlastní OU strukturu
Členové skupiny "AAD Datacentrum správci" můžete vytvořit vlastní organizačních jednotek v rámci spravované domény.

#### <a name="schema-extensions"></a>Rozšíření schématu
Nelze rozšířit základní schéma spravovaných domain Azure AD Domain Services. Proto aplikace, které jsou závislé na rozšíření AD schématu (například atributy v objektu uživatele) nelze zrušit a posunutou na AAD DS domény.

#### <a name="ad-domain-or-forest-trusts"></a>AD domény a vztahy důvěryhodnosti
Spravované domény se nedají konfigurovat nastavení vztahy důvěryhodnosti (příchozí nebo odchozí) s jinými doménami. Scénáře nasazení struktury zdrojů ATP případy, kdy nechcete synchronizovat hesla Azure AD nemůžete proto používat Azure AD Domain Services.

#### <a name="ldap-read"></a>Čtení LDAP
Spravované domény podporuje LDAP číst úloh. Proto nástroje můžete nasazovat aplikace, které provádí operace čtení LDAP proti spravované domény.

#### <a name="secure-ldap"></a>Zabezpečeného LDAP
Můžete nakonfigurovat služby Azure AD domény k poskytnutí zabezpečeného přístupu LDAP k spravované domény, včetně přes internet.

#### <a name="ldap-write"></a>Zápis LDAP
Spravované doména je určené jen pro čtení pro objekty uživatelů. Aplikace, které provádí operace zápisu LDAP proti atributy objektu uživatele, tedy nefungují v doméně spravované. Navíc uživatelská hesla určeny z oblasti spravovaných. Jiný příklad by změna členství ve skupině nebo skupině atributy v rámci spravovaných doménu, která není povoleno. Žádné změny atributy uživatele a hesla v Azure AD (pomocí Powershellu/Azure portál) nebo místních AD jsou synchronizovány doménu spravovaný AAD Pošta.

#### <a name="group-policy"></a>Zásady skupiny
Sofistikované skupiny zásad konstrukce nejsou podporované ve spravovaných doméně AAD Pošta. Například nelze vytvořit a nasazení samostatné objekty zásad skupiny pro každé vlastní organizační jednotce v doméně nebo použít WMI filtrování pro cílovou zásad skupiny. Existuje předdefinované objekt zásad skupiny každý kontejnerů AADDC počítače a AADDC uživatelů, které lze přizpůsobit tak, aby konfigurace zásad skupiny.

#### <a name="geo-dispersed-deployments"></a>Vzdálené GEO nasazení
Spravované domény Azure AD Domain Services jsou dostupné v jedné sítě virtuální v Azure. Scénáře, které vyžadují řadiče domény, aby byl k dispozici ve více oblastech Azure opačném konci světa nastavení řadiče domény v Azure IaaS VMs může být lepší alternativou.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>"Vlastními" (DIY) AD možnosti nasazení
Můžete mít případy použití nasazení místo, kam potřebujete některé funkce nabízené instalace systému Windows Server AD. V těchto případech zvažte jednu z následujících možností vlastními (DIY):

- **Samostatného cloudu domény:** Můžete nastavit samostatně cloudu domény pomocí Azure virtuálních počítačích, které jste nakonfigurovali jako řadiče domény. Tato infrastruktura není integrace s vaší místní AD prostředí. Tuto možnost vyžadují různé sady cloudu pověření k přihlášení a správě VMs v cloudu.

- **Zdroje doménové nasazení:** Můžete nastavit doménu v topologii struktury zdrojů pomocí Azure virtuálních počítačích nakonfigurování jako řadiče domény. Pak můžete nakonfigurovat vztah důvěryhodnosti AD s vaší místní AD prostředí. Připojení k doméně počítače (Azure VMs) můžete k této struktuře zdroje v cloudu. Ověřování uživatelů neděje přes buď připojení VPN/ExpressRoute pro místního adresáře

- **Rozšíření místních domény Azure:** Azure virtuální sítě můžete připojit k místní síti pomocí připojení VPN/ExpressRoute tak, aby Azure VMs můžete připojené k vaší místní AD. Další možností je podpora otevřené domény řadiče domény místního v Azure jako virtuálního počítače. Můžete pak nastavit ho replikace přes připojení VPN/ExpressRoute pro místního adresáře. V tomto režimu nasazení účinně sahá domény místního Azure.

> [AZURE.NOTE] Můžete určit, že – SVÉPOMOCNÝ možnost vhodnější pro vaše nasazení-případy použití. Zvažte [sdílení názoru](active-directory-ds-contact-us.md) jak pomoct pochopit, jak funkce by vám pomůžou v budoucnu rozhodnete Azure AD Domain Services. Tuto zpětnou vazbu pomáhá vyvíjet službu lépe podle potřeby nasazení a případy použití.

Zveřejněných [pokyny pro nasazení systému Windows služby Active Directory Server na virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx) usnadnit – SVÉPOMOCNÝ zařízení.


## <a name="related-content"></a>Související obsah
- [Funkce – Azure AD Domain služby](active-directory-ds-features.md)

- [Scénáře nasazení - Azure AD Domain Services](active-directory-ds-scenarios.md)

- [Pokyny pro nasazení systému Windows Server služby Active Directory v Azure virtuálních počítačích](https://msdn.microsoft.com/library/azure/jj156090.aspx)
