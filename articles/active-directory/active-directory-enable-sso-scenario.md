<properties
    pageTitle="Správa aplikací službou Azure Active Directory | Microsoft Azure"
    description="Tento článek výhody integrace služby Azure Active Directory s místním, cloudu a SaaS aplikací."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>Správa aplikací službou Azure Active Directory

Za vlastní pracovní postup nebo obsah podniky mají dva základní požadavky pro všechny aplikace:

1. Zvýšení produktivity aplikace by měly být snadno zjišťovat a přístup

2. Umožňuje zabezpečení a zásady správného řízení organizace potřebuje ovládací prvek a dohled na kdo může a skutečně přistupuje jednotlivých aplikací

Ve světě cloudové aplikace to nejlepší dosáhnout pomocí identity na ovládací prvek ",*KDO může co udělat*".

Ve počítače terminologie:

- *Kdo* je jmenoval *identit* - Správa uživatelů a skupin

- *K čemu* se označuje jako *Správa přístupu* – řízení přístupu k chráněným prostředkům

Obě součásti společně se označují jako *identit a řízení přístupu (IAM)*, který je definován skupinou [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) jako "*discipline zabezpečení, který umožňuje doprava osob a získat správné informace na pravé časy správné důvodů*".

Chápu, tak v čem je problém? Je-li IAM *není spravované* na jednom místě pomocí integrovaných řešení:

- Identita Správci mají jednotlivě vytvářet a aktualizovat uživatelské účty ve všech aplikacích v samostatně, nadbytečné a časově náročný aktivity.

- Uživatelé mají nazpaměť více přihlašovací údaje pro přístup k aplikacím, které potřebují pro práci s. Uživatelé obvykle jako výsledek, zapište si svoje hesla nebo použít jiné řešení pro správu heslo, které představuje zabezpečení rizikové další data.

- Nadbytečné, časově náročný aktivity Snižte množství času uživatelů a správci pracujeme na činnosti, které zvyšují vaší firmě dolní řádek.

Ano co obecně zabrání organizace přijímat integrovaných řešení IAM?

- Většina technických řešení založené na platformách software, které je potřeba nasazeném a přizpůsobit tak, že každá organizace pro svoje vlastní aplikace.

- Cloudové aplikace přijímají často sazbou vyšší než IT organizace můžete integrovat s existující IAM řešení.

- Zabezpečení a sledování nástrojů vyžadují dalších úprav a integrace dosáhnout úplný E2E scénáře.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory integrovat s aplikacemi

Azure Active Directory je společnosti Microsoft komplexní identitu jako řešení služby (IDaaS), který:

- Umožňuje IAM jako do cloudové služby 

- Poskytuje přístup k centrální správy, jednotného přihlašování (SSO) na a vytváření sestav 

- Správa integrované aplikace access podporuje [tisíce aplikací](https://azure.microsoft.com/marketplace/active-directory/) v galerii aplikace, včetně služby Salesforce, Google Apps, pole, Concur apod. 


Se službou Azure Active Directory všechny aplikace publikování pro partnery a zákazníky (podnik nebo spotř) mají stejnou identitu a přístup k možnostem správy.<br> Umožňuje značně snížit provozní náklady.

Co dělat, když potřebujete implementovat aplikaci, která zatím není uvedený v galerii aplikace? Když to je trochu náročnější než Konfigurace jednotného přihlašování pro aplikace z Galerie aplikace, Azure AD poskytuje průvodce, který vám pomůže s konfigurací.

Hodnota Azure AD přesahuje "jenom" cloudové aplikace. Taky můžete ho s místní aplikací zadáním zabezpečené vzdáleného přístupu. Zabezpečený vzdálený přístup, můžete odstranit potřebu virtuální privátní sítě nebo jiné implementace správy tradiční vzdáleného přístupu.

Zadáním Správa centrální přístupu a jednotné přihlašování (SSO) pro všechny aplikace obsahuje Azure AD řešení problémů zabezpečení a produktivity hlavní data.

- Uživatelé přístup k více aplikací jeden poštu na základě víc času pro příjem generování nebo pracovní aktivity operace Hotovo.

- Správci identity můžete spravovat přístup k aplikacím na jednom místě.

Výhoda pro uživatele a pro vaši firmu je zjevných. Podívejme se bližší pohled na výhody pro správce identit a organizaci.

## <a name="integrated-application-benefits"></a>Výhody integrované aplikace

Procesu jednotného přihlašování obsahuje dva kroky:

- Ověřování, procesu ověření identity uživatele.

- Povolení, rozhodnutí o blokovat nebo povolit přístup k prostředku zásady přístupu.

Při použití Azure AD Správa aplikací a povolení jednotné přihlašování:

- Ověřování probíhá na místním (například AD) nebo Azure AD účtu uživatele.

- Povolení prováděn Azure AD ochrana a přiřazení zásady zajistit konzistentní koncového uživatele a umožňuje přidat přiřazení, umístění a podmínky MFA v libovolné aplikaci, bez ohledu na jeho vnitřní možnosti.

Je důležité pochopit, že je způsob, jakým tak mohli ověřovat použity na cílovou aplikaci se liší podle toho, jak byla aplikace integrovaný s Azure AD.

- **Aplikace předem integrovanou poskytovatel služeb** Jako je Office 365 a Azure jedná se o aplikace integrované přímo na Azure AD a může v něm pro jejich komplexní řízení funkce identit a přístup. Pomocí informací v adresáři a vydání tokenu je povolen přístup pro tyto aplikace.

- **Aplikace integrovaná předem společností Microsoft a vlastní aplikace** Toto jsou nezávislé cloudové aplikace, které jsou závislé na adresář interní aplikace a můžete pracovat bez ohledu na Azure AD. Zadáním aplikace konkrétní přihlašovacích údajů namapované účet aplikace je povolen přístup pro tyto aplikace. V závislosti na možnosti aplikace může být pověření federace token nebo uživatelské jméno a heslo účtu, která byla dříve vytvořena v aplikaci.

- **Místní aplikací** Aplikace publikovat proxy server Azure AD aplikace primárně povolení přístupu k místní aplikací. Tyto aplikace spolehnout webu enomcentral na místní adresář jako Windows Server Active Directory. Předvádění aplikace obsah koncový uživatel respektováním přihlašování požadovaného místního je povolen přístup pro tyto aplikace tak, že aktivaci proxy server.

Například pokud uživatel připojí ke vaší organizace, potřebujete k vytvoření účtů pro uživatele v Azure AD pro primární operace přihlašování. Vyžaduje-li tento uživatel přístup k spravované aplikace například Salesforce, budete potřebovat k vytvoření účtu pro tohoto uživatele služby Salesforce a odkaz na Azure účet a udělat SSO práce. Pokud uživatel opustí vaši organizaci, je vhodné odstranit účet Azure AD a ukládá všechny účty protějšek v IAM aplikací, které má uživatel přístup k.

## <a name="access-detection"></a>Zjišťování přístupu

V moderní podniky IT oddělení nejsou často přehled o všech aplikací cloudu, které jsou použity. Ve spojení s cloudu aplikace zjišťování Azure AD umožňuje řešení zjišťování tyto aplikace.

## <a name="account-management"></a>Správa účtů

Správa účtů v různých aplikacích obvykle je ručního procesu prováděné IT nebo podporu osobní v organizaci. Azure AD plně automatické Správa účtu přes všechny aplikace integrovaná poskytovatele služeb a tyto aplikace předem integrované společností Microsoft podpůrné zřizování automatické uživatele nebo za běhu SAML.

## <a name="automated-user-provisioning"></a>Zřizování automatizovaných uživatele

Některé aplikace zadejte automatizaci rozhraní pro vytváření a odstraňování (nebo deaktivaci) účtů. Pokud poskytovatele nabízí nějakého rozhraní, je využít tak, že Azure AD. Snižuje provozní náklady, protože úkoly správy dojít automaticky a zvyšuje zabezpečení prostředí, protože snižuje možnost neoprávněnému přístupu.

## <a name="access-management"></a>Správa přístupu

Pomocí Azure AD můžete spravovat přístup k aplikací pomocí jednotlivé nebo pravidla řízený úsilím přiřazení. Můžete také delegovat přístup k řízení správné lidem v organizaci zajištění nejlepší dohled a snižuje počet zatížení technickou podporu.

## <a name="on-premises-applications"></a>Místní aplikací

Vytvořená v aplikaci proxy vám umožní publikovat aplikace místním uživatelům výsledkem jak konzistentní přístup zkušenosti s aplikací moderní cloudu a výhodách z Azure AD monitorování, podávání zpráv a zabezpečení možnosti.

## <a name="reporting-and-monitoring"></a>Vytváření sestav a sledování

Azure AD umožňuje předem integrované podávání zpráv a sledování možnosti, které umožňují vědět, kdo bude mít přístup k aplikacím a kdy je skutečně používají.

## <a name="related-capabilities"></a>Související funkce

S Azure AD můžete zabezpečení aplikací s zásady podrobného přístupu a předem integrovaném MFA. Další informace o Azure MFA najdete v článku [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Začínáme

Začít integrace aplikace s Azure AD, přečtěte si téma [Příručka Začínáme s integrace Azure Active Directory s aplikacemi začíná](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Viz taky

[Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
