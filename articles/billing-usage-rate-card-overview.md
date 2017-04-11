<properties
   pageTitle="Získat další informace na využití prostředků Microsoft Azure | Microsoft Azure"
   description="Poskytuje přehled použití fakturace Azure a rozhraní API RateCard, které se používají k poskytování podstatu spotřebu Azure zdrojů a trendy."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>Získat další informace na využití prostředků Microsoft Azure

Zákazníci a partneři potřebují přesně předpovídání a správu jejich Azure nákladů.  Přechází z náklady do datového modelu pro Opex, potřebují taky možnost udělejte showback porovnání analýzy zpětné, a také poskytuje režimu věrností odhad a fakturace, zejména u velkých cloudu nasazení.

Rozhraní API karty sazba a používání zdrojů Azure popisované v tuto adresu článku tyto požadavky povolením nové přehledy do spotřebu Azure zdrojů.  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Úvodní informace o používání Azure zdrojů a rozhraní API RateCard

Rozhraní API RateCard a používání zdrojů Azure jsou implementovaná jako poskytovatele služby zdrojů, jako součást rozhraní API zveřejněným správcem zdrojů Azure řady.  

### <a name="azure-resource-usage-api-preview"></a>Používání Azure zdrojů rozhraní API (verze Preview)
Zákazníků a partnerů slouží k získání dat odhadovaná Azure spotřeba rozhraní API využití prostředků Azure. Funkce patří:

- **Řízení přístupu na základě rolí azure** - zákazníků a partnerů konfigurovat jejich zásady přístupu na [Azure portál](https://portal.azure.com) nebo pomocí [rutin prostředí PowerShell Azure](powershell-install-configure.md) můžete určit, kteří uživatelé nebo aplikací k můžete získat přístup předplatné použití zásad správy informací. Volající používají standardní tokeny Azure Active Directory pro ověřování. Volající musí taky přidaní do Reader, vlastník nebo přispěvatelů role získat přístup k datům použití u určitého předplatného Azure.

- **Každou hodinu nebo denní agregace** - volající můžete určit, jestli mají data Azure použití v hodinách bloky nebo denní bloků. Výchozí hodnota je denní.

- **Instance metadata uvedená (včetně značek zdroje)** – například uri plně kvalifikovaný resource údaje o úrovni Instance (/subscriptions/ {id předplatného} /..), spolu s zdroje značky skupiny prostředků a informací se dozvíte při v odpovědi. Tím zákazníkům pomůže deterministically a programově přidělit použití podle klíčových slov, pro případy použití jako křížově nabíjení.

- **Metadata zdroje k dispozici** – údajů o zdroji třeba metr název, kategorie metr, metr podkategorie, jednotky a oblasti bude předán také v odpovědi, volající poskytnout lepší porozumění co byl spotřebované množství. Pracujeme taky zarovnání terminologie metadat zdroje na portálu Azure Azure použití CSV, EA fakturace CSV a jiné veřejné prostředí umožňují zákazníkům vazbu mezi dat v prostředí.

- **Použití pro všechny typy nabídky** – použití zásad správy informací, budou přístupné pro všechny typy včetně přislíbený MSDN, peněžní potvrzení, peněžní platební a EA prvky nabízejí.

### <a name="azure-resource-ratecard-api-preview"></a>Azure zdroje RateCard rozhraní API (verze Preview)
Zákazníci a partneři umožňuje rozhraní API RateCard zdroje Azure získat seznam dostupných Azure zdrojů, spolu s předpokládané ceny informace pro jednotlivá pole. Funkce patří:

- **Řízení přístupu na základě rolí azure** - zákazníků a partnerů konfigurovat jejich zásady přístupu na [Azure portál](https://portal.azure.com) nebo pomocí [rutin prostředí PowerShell Azure](powershell-install-configure.md) můžete určit, které uživatelé nebo aplikace, můžete získat přístup k datům RateCard. Volající používají standardní tokeny Azure Active Directory pro ověřování. Volající musí taky přidaní do Reader, vlastník nebo přispěvatelů role získat přístup k datům použití u určitého předplatného Azure.

- **Podpora přislíbený MSDN, peněžní potvrzení a peněžní platební nabízí (EA nepodporované)** – tento rozhraní API informacemi Azure úrovni nabídky sazba, porovnání úrovni předplatného.  Volající toto rozhraní API musí předat v informacích o nabídku zobrazíte údajů o zdroji a sazby.  Jako EA nabídky upravili sazby pro zápis, není možné stanovit sazby EA v současné době.

## <a name="scenarios"></a>Scénáře

Tady jsou některé scénáře, které jsou umožnily pomocí kombinace využití a rozhraní API RateCard:

- **Azure věnovat během určitého měsíce** - zákazníci mohli používat využití a RateCard rozhraní API v kombinaci získat lepší podstatu jejich cloudu věnovat během určitého měsíce analýzou hodinové a denní bloky odhady použití a náklady.

- **Nastavení upozornění** – zákazníci a partneři můžete nastavit zdroje založené nebo peněžní – upozornění na jejich cloudu spotřebu získáním odhadovaná spotřeba a odhadu nákladů pomocí využití a rozhraní API RateCard.

- **Faktury Predict** – zákazníků a partnerů dostali odhadovaná spotřeba a cloudu věnovat a použít počítač výukové algoritmů předpovídat co jejich faktury bude v konce fakturačního cyklu.

- **Náklady před spotřeby analýza** – zákazníci se dají také pomocí rozhraní API RateCard odhadnout kolik jejich faktury bude by byly přesunout jejich pracovního vytížení Azure, podle poskytující žádoucí použití čísla. Máte zákazníci existující úloh v jiných mračnech nebo soukromé Oblaka, můžete taky mapují jejich použití s Azure po kterou sazby získat lepší odhad odhadovaná Azure. Rozšířené zobrazení co lze získat prostřednictvím [Azure kalkulačky ceny](https://azure.microsoft.com/pricing/calculator/), zajistíte tak (například) partneři fakturace umožňují otočení na nabídku a porovnat/kontrast mezi jinou nabídku typy za přislíbený, včetně měnové závazky a měnové platební. Rozhraní API také umožňují nákladů změn odhad podle oblasti, povolení typ citlivostní analýzy muset rozhodovat nasazení nasazení zdrojů v různých řadiče domény ve světě můžete mít přímý vliv na celkové náklady.

- **Citlivostní analýzy** -

    - Zákazníci a partneři můžete zjistit, zda je efektivnější spustit jejich pracovního vytížení v jiné oblasti nebo na jiné konfigurace Azure zdroje. Azure zdroje, které se můžou lišit náklady založené na Azure oblast, ve kterém jsou spuštěné a díky zákazníků a partnerů získat optimalizace náklady.

    - Zákazníci a partneři můžete také určit, pokud jiný typ Azure nabídky poskytuje lepší rychlost na Azure zdroje.

## <a name="partner-solutions"></a>Partnerských řešení

[Použití Microsoft Azure a RateCard rozhraní API povolit Cloudyn k poskytnutí ITFM pro zákazníky](billing-usage-rate-card-partner-solution-cloudyn.md) popisuje možnosti integrace nabízená rozhraní API fakturace Azure partnera [Cloudyn](https://www.cloudyn.com/microsoft-azure/).  Tento článek obsahuje podrobné rozsah jejich prostředí, včetně krátké video, které ukazuje, jak zákazníkem Azure můžete použít Cloudyn a rozhraní API fakturace Azure interpretace zisků z dat Azure spotřebu.

[Cruiser cloudu a fakturace integrace rozhraní API aplikace Microsoft Azure](billing-usage-rate-card-partner-solution-cloudcruiser.md) popisuje, jak [cloudu Cruiser Express Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) funguje přímo na portálu WAP povolení zákazníkům Bezproblémová spravovat provozní a finanční aspekty svých Microsoft Azure soukromé nebo hostovanou veřejné cloudu z jednoho uživatelského rozhraní.   

## <a name="next-steps"></a>Další kroky
+ Podívejte se na [Azure fakturace REST API odkaz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) podrobné informace o obou rozhraní API, které jsou součástí sady rozhraní API správcem zdrojů Azure k dispozici.
+ Pokud chcete pusťte do ukázkový kód, podívejte se na našeho Microsoft Azure fakturace rozhraní API ukázky na [Azure ukázky](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Víc se uč
+ Naleznete v článku [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md) Další informace o správce prostředků Azure.
+ Další informace o sadu nástrojů nutné vyřešit tím, že cloud výdajů, naleznete v článku Gartner [Market Průvodce pro nástroje IT finanční Management (ITFM)](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).
