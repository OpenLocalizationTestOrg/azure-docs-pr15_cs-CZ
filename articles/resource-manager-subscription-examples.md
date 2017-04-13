<properties
   pageTitle="Scénáře a příklady pro správu předplatného | Microsoft Azure"
   description="Obsahuje příklady, jak provádět správu Azure předplatné pro běžné scénáře."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Příklady provádění scaffold Azure enterprise

Toto téma obsahuje příklady podniku můžete jak implementovat doporučení pro [scaffold Azure organizace](resource-manager-subscription-governance.md). Fiktivní společnost s názvem Contoso používá ke znázornění doporučených postupů pro běžné scénáře. 

## <a name="background"></a>Pozadí

Contoso je Celosvětová dostupnost společnost, která poskytuje dodávek řetězec řešení pro zákazníky v všechno z modelu "Software jako služba" sbalené modelu nasazeném místní.  Vyvíjejí software po celém světě s významné vývoj centra v Indie, USA a Kanada. 

Část nezávislí výrobci softwaru společnosti je rozdělen do několika nezávisle na organizačních jednotek, které spravují produkty v významné business. Vlastní vývojáři produktových manažerů a tvůrci má každý organizační jednotka. 

Organizační jednotka Enterprise technologii služby (ETS) poskytuje centralizované funkci IT a spravuje několik datacentrech kde organizačních jednotek hostovat svých aplikací. Spolu s dat centra pro správu, organizaci ETS poskytuje a spravuje centralizované spolupráce (například e-mail a webové stránky) a telefonní sítě nebo služby. Pro menší firmy jednotky, kteří nemají provozní zaměstnanci také spravují zákazníkem úloh. 

V tomto tématu se používají následující osoby:

- Davidově je správce ETS Azure.
- Alice je společnosti Contoso ředitele vývoj v zásobovací řetězec firmy jednotky. 

Contoso je potřeba vytvořit řádek obchodní aplikace a zákazníka webových aplikací. Rozhodla ke spuštění aplikace v Azure. Davidově přečte v tématu [zásady správného řízení obvyklé předplatné](resource-manager-subscription-governance.md) a je připraven k provedení doporučení. 

## <a name="scenario-1-line-of-business-application"></a>Scénář 1: řádek obchodní aplikací

Contoso je vytvoření zdrojového správu kódu (BitBucket) pro vývojáře po celém světě.  Aplikace využívá infrastrukturu jako služba (IaaS) pro hostování a se skládá z webových serverů a databázovým serverem. Vývojáři přístup k serverům ve své vývojové prostředí, ale nepotřebují přístup k serverům v Azure. Contoso ETS chce povolit aplikaci vlastník a týmu spravovat aplikace. Aplikace neexistuje pouze při společnosti Contoso podnikové síti se systémem. Davidově musí nastavit předplatné pro tuto aplikaci. Taky další software vývojáře předplatné uspořádá v budoucnu.  

### <a name="naming-standards--resource-groups"></a>Pojmenování standardy a zdroje skupiny

Davidově vytvoří předplatné pro podporu nástrojů pro vývojáře, které jsou běžné přes organizačních jednotek. Potřebuje vytvořit smysluplné názvy skupin zdroje a předplatného (pro aplikace a sítě). Tahle vytvoří následující skupiny prostředků a předplatného:

| Položky | Jméno | Popis |
| ---- | ---- | ----------- |
| Předplatné | Výrobní DeveloperTools contoso ETS | Podporuje běžných nástrojů pro vývojáře |
| Pole Skupina zdroje | rgBitBucket | Obsahuje aplikace webový server a databázový server |
| Pole Skupina zdroje | rgCoreNetworks | Obsahuje virtuálních sítí a připojení k webu brány |


### <a name="role-based-access-control"></a>Řízení přístupu na základě rolí

Po vytvoření svého předplatného, Davidově chce zajistit, že příslušný týmy a vlastníci aplikace přístup k jejich zdroje. Davidově zjistí, že má každý tým odlišné požadavky. Tahle využívá skupiny, které byly synchronizované od společnosti Contoso místní služby Active Directory (AD) s Azure Active Directory a poskytuje správnou úroveň přístupu k týmy. 

Davidově přiřadí následující role u předplatného: 

| Role | Přiřazeno | Popis |
| ---- | ----------- | ----------- |
| [Vlastník](./active-directory/role-based-access-built-in-roles.md#owner) | Spravovat ID od společnosti Contoso AD | Toto ID ovládat pomocí pouze v aplikaci access doba běhu pomocí nástroje pro správu Identity společnosti Contoso a zajišťuje, že je plně auditovány přístup vlastník předplatného. |
| [Správce zabezpečení](./active-directory/role-based-access-built-in-roles.md#security-manager) | Oddělení správy zabezpečení a rizika | Tato role umožňuje uživatelům se podívat na Centrum zabezpečení Azure a stav zdroje. |
| [Síť přispěvatelů](./active-directory/role-based-access-built-in-roles.md##network-contributor) | Síť stará | Tato role umožňuje společnosti Contoso síť stará ke správě sítěmi VPN a virtuální sítě. |
| *Vlastní role* | Vlastník aplikace | Davidově vytvoří roli, která poskytuje možnost upravit zdroje v rámci skupiny zdrojů. Další informace najdete v tématu [Vlastní role v Azure RBAC](./active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Zásady

Davidově obsahuje následující požadavky na řízení zdrojů v předplatného:

- Protože vývojového nástroje podporují vývojáři opačném konci světa, nemá by zabránit uživatelům v vytvoření zdrojů v jakékoli oblasti. Však potřebuje vědět, kde jsou vytvářeny zdroje. 
- Se zabývají náklady. Proto chce zabránit vytváření zbytečně drahé virtuálních počítačích aplikace vlastníci.  
- Protože tuto aplikaci slouží vývojáři v mnoha organizačních jednotek, chce označení jednotlivé zdroje s organizační jednotka a aplikace vlastníkem. Pomocí těchto značky ETS účtovat odpovídající týmy.

Vytváření následující [Správce prostředků zásady](resource-manager-policy.md): 

| Pole | Efekt | Popis |
| ----- | ------ | ----------- |
| umístění | auditování | Vytvoření zdrojů v jakékoli oblasti auditování |
| Typ | Odepřít | Zakázat vytváření virtuálních počítačích G řady |
| značky | Odepřít | Vyžadovat aplikace vlastník značky |
| značky | Odepřít | Vyžadovat náklady Centrum značky |
| značky | připojení | Přidat název značky **organizační** a Příznak Hodnota **ETS** pro všechny zdroje |


### <a name="resource-tags"></a>Značky zdroje

Davidově rozumí, že osoba, musí mít konkrétních informací na faktuře k identifikaci nákladové středisko pro implementaci BitBucket. Kromě toho Davidově chce vědět všechny zdroje, které ETS vlastní. 

Následující [značky](resource-group-using-tags.md) přidá do skupiny zdrojů a zdroje. 
 
| Název značky | Příznak Hodnota |
| -------- | --------- |
| ApplicationOwner | Jméno osoby, kdo vám spravuje této aplikace. |
| CostCenter | Nákladové středisko skupině, která platí pro Azure spotřebu. |
| Organizační | **ETS** (organizační jednotky přidružená k odběru) |

### <a name="core-network"></a>Základní sítě

Contoso ETS informace o zabezpečení a rizik manažeři kontroluje společnosti navrhované plánu aplikace Azure. Potřebují zajistit, že aplikace nebude vystaven Internetu.  Davidově má i developer aplikace, které se v budoucnu přesune do Azure. Tyto aplikace vyžadují veřejná rozhraní.  Splňovat tyto požadavky, poskytuje interním a externím virtuálních sítí a skupiny zabezpečení sítě omezení přístupu.

Tahle vytvoří v následujících zdrojích:

| Pole Typ zdroje | Jméno | Popis |
| ------------- | ---- | ----------- |
| Virtuální sítě | vnInternal | Použít aplikaci BitBucket a je připojený prostřednictvím ExpressRoute k podnikové síti společnosti Contoso.  Podsítě (sbBitBucket) obsahuje aplikace mezerou konkrétní IP adres. |
| Virtuální sítě | vnExternal | K dispozici pro budoucí aplikace, které vyžadují koncové body veřejné. |
| Skupina zabezpečení sítě | nsgBitBucket | Zajišťuje, že napadení pracovního vytížení minimalizovat povolení připojení jenom na port 443 podsítě, kde je aplikace (sbBitBucket). |

### <a name="resource-locks"></a>Uzamčení prostředků 

Davidově zjistí, že připojení k podnikové síti společnosti Contoso interní virtuální síti musí být chráněny mezi libovolnými wayward skriptu nebo nechtěným odstraněním. 

Vytváření následující [zdroje zámku](resource-group-lock-resources.md):

| Typ zámku | Zdroje | Popis |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnInternal | Zabrání uživatelům odstranit virtuální sítě nebo podsítí, ale nezabrání přidávání nových podsítí. |

### <a name="azure-automation"></a>Automatizace Azure 

Davidově neobsahuje žádné k automatizaci k této aplikaci. Přestože vytvořil účet Azure automatizaci mu nebudeme potřebovat původně ho. 

### <a name="azure-security-center"></a>Centrum zabezpečení Azure 

Správa služby společnosti Contoso IT musí rychle identifikovat a obsloužení hrozeb. Chtějí porozumět tomu, jaký problémů může existovat.  

Tyto požadavky splnit, Davidově umožňuje [Azure Centrum zabezpečení](./security-center/security-center-intro.md)a poskytuje přístup k roli správce zabezpečení. 

## <a name="scenario-2-customer-facing-app"></a>Scénář 2: zákazníka webových aplikací

Vedením firmy v zásobovací řetězec firmy jednotka byl zjištěn různé možnosti víc zapojili se zákazníky společnosti Contoso kartou věrnostní. Tým Alice musíte vytvořit tuto aplikaci a rozhoduje o Azure zvyšuje jejich schopnost zahájit obchodních potřeb. Alice spolupracuje Davidově z ETS konfigurace dvou předplatná pro vývoj a provozní této aplikace.

### <a name="azure-subscriptions"></a>Azure předplatná 

Davidově přihlášení k portálu Azure a uvidí již existuje oddělení řetězec dodávky.  Však první vývojového projektu pro tým řetězec dodávek v Azure je tento projekt Davidově rozpozná potřebujete k vytvoření nového účtu pro týmu vývoje Alice.  Vytvoří "Výzkum a vývoj" účtu pro svůj tým a přiřadí Alice přístup. Alice přihlašuje prostřednictvím portálu účtu a vytvoří dva předplatná: jedna ku podržte vývoj servery a druhý na podržte provozní servery.  Anna standardům dříve vytvořené naming při vytváření následující předplatná: 

| Použití předplatného | Jméno |
| ---------------- | ---- |
| Vývojové | Vývojové LoyaltyCard SupplyChain ResearchDevelopment |
| Výrobní | SupplyChain operace LoyaltyCard výrobní |

### <a name="policies"></a>Zásady

Davidově Alice diskutovat o aplikaci a určit, že tuto aplikaci pouze slouží zákazníky v Severní Americe oblast.  Alice a svůj tým plánování používání aplikace služby prostředí a Azure SQL Azure na vytvořit aplikaci. Může být nutné k vytvoření virtuálních počítačích průběhu vývoje.  Alice chce své vývojáři zajištění prostředky, které budou muset umožňuje zkoumat a prozkoumání problémů bez zavedení v ETS. 

**Vývojové předplatné**vytvářejí následující zásady:

| Pole | Efekt | Popis |
| ----- | ------ | ----------- |
| umístění | auditování | Vytvoření zdrojů v jakékoli oblasti auditování. |

Není omezena typ sku vytvořená uživatele ve vývoji a nevyžadují značky pro všechny skupiny zdrojů nebo zdroje.

U **předplatného výrobní**vytvářejí následující zásady:

| Pole | Efekt | Popis |
| ----- | ------ | ----------- |
| umístění | Odepřít | Zakažte vytváření zdroje mimo datacentrech USA. |
| značky | Odepřít | Vyžadovat aplikace vlastník značky |
| značky | Odepřít | Je nutné oddělení značku. | 
| značky | připojení | Přidejte značky na jednotlivé skupiny zdroje, které označuje provozním prostředí. |

Není omezena typ sku vytvořená uživatele v výroby.

### <a name="resource-tags"></a>Značky zdroje 

Davidově rozumí, že osoba, musí mít konkrétních informací k identifikaci správné obchodní skupiny pro správce fakturace a vlastnictví. Tahle definuje značky zdroje pro skupiny zdrojů a zdroje. 
 
Název značky | Příznak Hodnota |
| -------- | --------- |
| ApplicationOwner | Jméno osoby, kdo vám spravuje této aplikace. |
| Oddělení | Nákladové středisko skupině, která platí pro Azure spotřebu. |
| EnvironmentType | **Výrobní** (I když předplatné obsahuje **výrobní** v názvu, včetně tuto značku umožňuje snadno identifikace při pohledu zdroje na portálu nebo na faktuře.) |

### <a name="core-networks"></a>Základní sítě

Contoso ETS informace o zabezpečení a rizik manažeři kontroluje společnosti navrhované plánu aplikace Azure. Chtějí, aby správně samostatný a chráněny v síti DMZ aplikaci věrnostní karty.  Ke splnění tohoto požadavku, Davidově a Alice vytvořit externí síť virtuální a skupiny zabezpečení sítě izolovat věrnostní karty aplikace z podnikové sítě Contoso.  

**Vývojové předplatné**vytvořit:

| Pole Typ zdroje | Jméno | Popis |
| ------------- | ---- | ----------- |
| Virtuální sítě | vnInternal | Slouží vývojové prostředí Contoso věrnostní karty a je připojený prostřednictvím ExpressRoute společnosti Contoso podnikové sítě. |

U **předplatného výrobní**vytvořit:

| Pole Typ zdroje | Jméno | Popis |
| ------------- | ---- | ----------- |
| Virtuální sítě | vnExternal | Aplikace věrnostní karty a není připojené přímo k ExpressRoute společnosti Contoso. Kód se posune prostřednictvím jejich zdrojového kódu systému přímo PaaS služby. |
| Skupina zabezpečení sítě | nsgBitBucket | Zajišťuje, že je napadení pracovního vytížení minimalizován pouze umožněním komunikace v vazbou na TCP 443.  Contoso je také zkoumá pomocí webové aplikace Firewall pro dodatečnou ochranu. |  

### <a name="resource-locks"></a>Uzamčení prostředků 

Davidově Alice udělit a budete chtít přidat uzamčení prostředků na některé klíčové zdrojů v prostředí proti nechtěným odstraněním během nabízených vyvolání chybové kód. 

Vytváření následující zámku:

| Typ zámku | Zdroje | Popis |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnExternal | Jak zabránit uživatelům v odstraňování virtuální sítě a podsítí. Uzamčení nezabrání přidávání nových podsítí. |

### <a name="azure-automation"></a>Automatizace Azure 

Alice a svého týmu vývoje mají rozsáhlé runbooks ke správě prostředí pro tuto aplikaci. Runbooks povolit pro přidání nebo odstranění uzlů pro aplikace a dalších úlohách DevOps. 

Pokud chcete použít tyto runbooks, umožňují [automatizaci](./automation/automation-intro.md).

### <a name="azure-security-center"></a>Centrum zabezpečení Azure 

Správa služby společnosti Contoso IT musí rychle identifikovat a obsloužení hrozeb. Chtějí porozumět tomu, jaký problémů může existovat.  

Tyto požadavky splnit, umožňuje Davidově Azure Centrum zabezpečení. Tahle zajišťuje, aby Centrum zabezpečení Azure sledování zdroje a poskytuje přístup k týmy DevOps a zabezpečení. 

## <a name="next-steps"></a>Další kroky

- Další informace o vytváření správce prostředků šablon najdete v tématu [Doporučené postupy pro vytváření šablon správce prostředků Azure](resource-manager-template-best-practices.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) přispěla k v tomto tématu.*
