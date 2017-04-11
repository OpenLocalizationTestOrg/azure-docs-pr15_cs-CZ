<properties 
    pageTitle="Jaké aplikace logiky?" 
    description="Další informace o aplikaci služby logiky aplikace" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="what-are-logic-apps"></a>Jaké aplikace logiky?

Použití logických operátorů aplikací lze ke zjednodušení a implementaci scalable integrace a pracovní postupy v cloudu. Poskytuje vizuálního návrháře modelů a automatizaci procesů jako řadu kroků jmenoval pracovního postupu.  Existuje [Řada spojnic](../connectors/apis-list.md) různých cloudu a místní rychle integrovat přes služby a protokoly.  Použití logických operátorů aplikace začíná aktivační (like "při přidání účtu k Dynamics CRM") a po pálení mohli začít mnoho kombinace akce převodu a podmínky použití logických operátorů.

Výhody používání aplikací logiky patřit následující úkoly:  

- Úspora času navržením složité procesy pomocí snadno pochopit nástroje pro navrhování
- Bezproblémová implementace vzorků a pracovní postupy, které by jinak být obtížné provádět v kódu
- Začínáme se rychle ze šablony
- Přizpůsobení aplikace logika s vlastními vlastní API, kód a akce
- Připojení a synchronizace synchronisace různými systémy mezi místním prostředím a cloudu
- Vytvoření mimo BizTalk serveru správy rozhraní API, funkce Azure a Bus služby Azure s podporou prvotřídní integrace

Logika aplikace je plně spravovanou iPaaS (Integrace platformy jako služba) vývojářům není starat o vytváření hostingu, škálovatelnost, dostupnost a správy.  Použití logických operátorů aplikace se automaticky škálování plnit poptávka.

![Návrhář aplikace toku](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Jak je uvedeno, s aplikacemi jiných použití logických operátorů, můžete automatizace firemních procesů. Tady je pár příkladů:  
 
* Přesunutí souborů uloženy na serveru FTP v úložišti Azure
* Proces a směrování objednávek různých místních i cloudových systémy
* Sledovat všechny tweety zabývající se některé, analyzovat myšlenkou a vytvořte si upozornění a úkolů pro položky, které vyžadují zpracování.

Scénáře, jako jsou například je možné konfigurovat z vizuálního návrháře a bez psaní jedním řádkem kódu. Získání začnete [vytvářet aplikace použití logických operátorů][create].  Po - lze v aplikaci použití logických operátorů [rychle nasazeném a překonfigurovat](app-service-logic-create-deploy-template.md) napříč několika prostředí a oblastí.

## <a name="why-logic-apps"></a>Proč aplikace logiky?

Logika aplikace spojuje rychlost a škálovatelnost do místa podnikové integrace.  Snadnější použití návrháře různých dostupné aktivačních událostí a akce a výkonné nástroje pro správu zkontrolujte centralizace vaše rozhraní API jednodušší než kdykoli dřív.  Jako firmy přesunout směrem digitalization, použití logických operátorů aplikace umožňuje spojit starších a špičkové systémy.

Navíc s náš [Účet integrace Enterprise] [ biztalk] je možné přizpůsobit zrát scénáře integrace s výkonem [zpráv XML][xml], [obchodování partnera správy][tpm]apod.

- **Nástroje pro navrhování snadno se použije** - použití logických operátorů aplikací může být navržené začátku do konce v prohlížeči nebo pomocí nástrojů Visual Studia. Začněte s aktivační – od jednoduchého rozpis při vytvoření GitHub problém. Potom organizovat libovolný počet akcí pomocí Galerie bohaté spojnic.

- **Připojení rozhraní API snadno** -i složení úkoly, které se snadno popisují se dá obtížně provádět v kódu. Logika aplikace snadno připojit různými systémy. Chcete připojit vaše cloudové marketingové řešení vaší místní fakturace systém? Chcete soustředit zpráv různých rozhraní API a systémy s Bus služeb Enterprise? Použití logických operátorů aplikace jsou nejrychlejší a nejčastěji spolehlivý způsob při řešení těchto problémů.

- **Začít rychle ze šablon, které** -vám pomůžou začít jsme jste uvedli v [galerii šablon] [ templates] , které umožňují rychle vytvářet některé běžné řešení. Z rozšířené B2B řešení jednoduché SaaS připojení a i několik, které jsou určené "pro zábavy" - Galerie je nejrychlejší způsob, jak začít pracovat s power logiky aplikace.

- **Rozšiřitelnost peče se změnami** – nezobrazuje konektoru potřebujete? Použití logických operátorů aplikace je navržený pro práci s vlastní rozhraní API a kód; rozhraní API aplikace používat jako vlastní spojnice nebo volání [Funkce Azure](https://functions.azure.com) provést fragmenty kódu okamžitou mohli snadno vytvářet. 

- **Koňská síla skutečné integrace** - zahájení snadno a postupně se zvětšují podle potřeby. Použití logických operátorů aplikace můžete snadno díky BizTalk společnosti Microsoft odvětví počáteční integrace řešení povolení integrace profesionály vytvářet řešení, které potřebují. Další informace o [Integration Pack organizace](./app-service-logic-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Použití logických operátorů aplikace koncepty

Tady jsou uvedené některé klíčové částí, které zahrnují možnosti použití logických operátorů aplikace. 

- **Pracovní postup** – použití logických operátorů aplikace umožňuje grafické k modelování obchodních procesů jako řadu kroků nebo pracovního postupu.
- **Spravovat spojnic** - aplikací použití logických operátorů potřebují mít přístup k datům a služeb. Spravované spojnic vzniká konkrétně na podporu můžete po připojení k a práci s daty. Prohlédněte si seznam spojnic součástí teď [spravovaných spojnic][managedapis].
- **Aktivace** - některé spojnice spravovaných můžete taky představovat aktivační událost. Aktivační události spustí novou instanci pracovního postupu založeného na zvláštní události jako přijetí e-mailovou zprávu nebo změnu ve vašem úložišti Azure účtu.
-  **Akce** - každý krok po aktivační událost v pracovním postupu se nazývá akce. Každou akci obvykle mapuje operaci na spravované spojnice nebo vlastní rozhraní API aplikace.
- **Podnikové integrace Pack** – pokročilejší scénáře integrace aplikace logiky přináší funkce BizTalk. BizTalk je společnosti Microsoft odvětví počáteční integrace platformu. Spojnice Enterprise integrace Pack umožňují snadno vložit ověření, transformace a dalších možností v do pracovních postupů logiky aplikace.

## <a name="getting-started"></a>Začínáme  

- Začínáme s aplikacemi jiných použití logických operátorů, postupujte podle [Vytvoření aplikace logiky] [ create] kurz.  
- [Zobrazení společných příklady a scénáře](app-service-logic-examples-and-scenarios.md)
- [Automatizace firemních procesů pomocí aplikace logiky](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Zjistěte, jak integrovat systémů s aplikacemi jiných logiky](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md
