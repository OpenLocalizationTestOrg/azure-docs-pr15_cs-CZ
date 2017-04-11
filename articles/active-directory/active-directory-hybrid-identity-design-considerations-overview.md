<properties
    pageTitle="Azure Active Directory hybridní identity navrhování – přehled | Microsoft Azure"
    description="Přehled a mapa obsahu příručky pro návrh aspektech hybridní Identity"
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Důležité informace o Azure Active Directory hybridní Identity návrhu

Na základě spotřebitelských zařízení jsou proliferating podnikové světa a cloudové aplikace software jako služeb (SaaS) snadno přijmout. Výsledkem je náročné zachování řízení přístupu k aplikaci uživatelů přes interní platformách datacentrech a cloudu.  Řešení společnosti Microsoft identity zahrnovat místním prostředím a cloudové funkce vytváření jednoho uživatele identity pro a tak mohli ověřovat pro všechny zdroje, bez ohledu na umístění. Název této hybridní Identity. Existují různé návrh a konfigurace možností pro hybridní identitu pomocí řešení společnosti Microsoft a v některých případech, který může být obtížné určit, které kombinaci bude vhodné podle potřeb vaší organizace. Toto hybridní Identity návrh aspektech Průvodce vám pomůže porozumět tomu, jak navrhnout hybridní identity řešení, které nejlépe odpovídá firmy a technologie potřebuje pro vaši organizaci.  Tato příručka, bude podrobností řadu kroků a úkoly, které sledujete vám navržení řešení totožnost hybridní, který splňuje specifickým požadavkům vaší organizace. V celém kroky a úkoly Průvodce zobrazeného odpovídající technologie a funkce Možnosti k dispozici organizacím schůzky funkční a kvality služeb (například dostupnost škálovatelnost, výkonu, správu a zabezpečení) úrovní požadavků. Konkrétně hybridní identity návrh aspektech Průvodce je odpovězte na následující otázky: 

- K čemu otázky potřebujete klást otázky a odpovědi řídit hybridní identity specifické návrhu pro doménu technologie nebo problém, nejlepší splňuje požadavky na můj?
- Jaké pořadí aktivity dokončení navrhnout vyřešíte identity hybridní domény technologie nebo problém? 
- Jaké hybridní identity konfigurace a technologické možnosti jsou k dispozici pomoc Moje předpisů? Jaké jsou střídání mezi tyto možnosti tak, aby se vybrat je nejlepší možností pro svou firmu?


## <a name="who-is-this-guide-intended-for"></a>Kdo je tato příručka určena?
 CIO, CITO, hlavní Identity tvůrci, tvůrci organizace a IT tvůrci zodpovědný za navrhování hybridní identity řešení pro organizace, střední nebo velký.

## <a name="how-can-this-guide-help-you"></a>Jak tento průvodce vám může pomoct? 
Tato příručka, můžete použít k pochopení navrhování řešení totožnost hybridní integrovat systému správy identit cloudový s aktuálním řešení totožnost místní. Následující obrázek ukazuje příklad hybridní identity řešení, které umožňuje správcům IT ke správě integrovat jejich aktuálním řešení služby Active Directory pro Windows Server nachází místních s Microsoft Azure Active Directory umožníte uživatelům pomocí jednoho přihlašování (SSO) ve všech aplikacích umístěné v cloudu a místní.

![](./media/hybrid-id-design-considerations/hybridID-example.png)


Na obrázku nahoře je příklad hybridní identity řešení, které je využívání cloudovým službám integrovat s místním funkcí k jednoho prostředí do procesu ověření koncového uživatele a usnadnit IT řízení těchto zdrojů. I když může to být velmi běžné situace organizace všech hybridní identity je pravděpodobně se liší od příklad znázorněné na obrázku 1 z důvodu odlišné požadavky. Tato příručka obsahuje řadu kroků a úkoly, které se dalo přejít k návrhu řešení totožnost hybridní, který splňuje specifickým požadavkům vaší organizace. V celém následující kroky a úkoly Průvodce nabídne vám možnosti odpovídající technologie a funkce dostupné pro vás podle funkčních a kvalitu požadavků na úrovně služeb pro vaši organizaci.

**Předpoklady**: máte zkušenosti s Windows Server, Active Directory Domain Services a služby Azure Active Directory. V tomto dokumentu jsme předpokládá, že jste hledali jak můžete tato řešení podle potřeb podniku vlastní nebo v integrovaném řešení.

## <a name="design-considerations-overview"></a>Přehled aspektech návrhu
Tento dokument obsahuje sadu kroky a úkoly, které se dalo přejít k návrhu hybridní identity řešení, která odpovídá vašim požadavkům. Kroky jsou uvedeny v sekvenci uspořádaných. Důležité informace o návrhu, který se naučíte na pozdější kroků může vyžadovat změnu rozhodnutí, které jste udělali v předchozích krocích, ale kvůli konfliktní návrh volby. Každý pokusu o vás upozorní na potenciální konflikty návrh v celém dokumentu. 

Budou přicházet v návrhu, že nejlépe odpovídá vašim požadavkům až po iterace kroky tolikrát, jako třeba používat všechny funkce důležité informace o dokumentu. 

| Hybridní Identity fáze                                             | Seznam témat                                                                                                                                                                                       |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Určit požadavky identity                                   | [Určení potřeb podniku](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Určení požadavky synchronizace adresářů](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Určit požadavky vícefaktorové ověřování](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Definování strategii zavádění hybridní identity](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)                       |
| Plán pro lepší zabezpečení dat pomocí řešení silných identity | [Zjistit, požadavky ochrany dat](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Určit požadavky na správu obsahu](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Určit požadavky na řízení přístupu](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Určit incidentu požadavky](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Definování strategie ochrana dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)  |
| Plánování hybridního identity cyklus                                | [Určení úlohy správy identit hybridní](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Správa synchronizace](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Určení strategie řízení hybridní identity](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##<a name="download-this-guide"></a>Stáhnout příručku
Verze pdf Průvodce hybridní Identity navrhování můžete stáhnout z [Galerie Technet](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

                                                             
