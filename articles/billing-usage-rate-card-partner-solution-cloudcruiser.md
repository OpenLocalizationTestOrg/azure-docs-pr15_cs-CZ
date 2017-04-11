<properties
   pageTitle="Cloud Cruiser a Microsoft Azure fakturace rozhraní API integrace | Microsoft Azure"
   description="Poskytuje jedinečného pohledu z Microsoft Azure fakturace partner cloudu Cruiser na jejich prostředí integrace rozhraní API fakturace Azure do svých produktů.  To je užitečné pro Azure a cloudu Cruiser zákazníky, kteří mají zájem pomocí/vyzkoušení Cruiser cloudu pro Microsoft Azure Pack."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cloud Cruiser a fakturace rozhraní API integrace Microsoft Azure

Tento článek popisuje, jak můžete použít informace shromážděné ze nové Microsoft Azure fakturace rozhraní API v cloudu Cruiser simulace náklady pracovního postupu a analýzy.

## <a name="azure-ratecard-api"></a>Rozhraní API Azure RateCard
Rozhraní API RateCard informacemi sazba z Azure. Po ověření s správné přihlašovací údaje, můžete dotaz rozhraní API přidávat metadata o službách dostupných na Azure, spolu s sazby přidružený k vaší nabízejí ID.

Následuje odpověď ukázka z rozhraní API zobrazující ceny pro A0 instance (Windows):

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Cruiser prvku rozhraní pro Azure RateCard rozhraní API v cloudu
Cloud Cruiser můžete využít informací rozhraní API RateCard různými způsoby. V tomto článku ukážeme, jak můžete použít aby IaaS pracovní zátěž nákladů simulace a analýzy.

Abychom tento případ použití Představte si úlohu několika instancí spuštěných na Microsoft Azure Pack (WAP). Cílem je simulovat stejného pracovního vytížení na Azure a odhad nákladů provedení migrace. Pokud chcete vytvořit tento simulace, existují dva hlavní úkoly, které se mají provést:

1. **Import a proces služby informace shromážděné ze rozhraní API RateCard.** Tento úkol lze provést také v sešitech, kde je extrahovat z rozhraní API RateCard transformovat a publikovaných na nový plán sazba. Tento nový plán sazba se použijí na simulace k odhadu Azure ceny.

2. **Normalizace WAP a Azure služby pro IaaS.** Ve výchozím nastavení jsou WAP služby založené na jednotlivé zdroje (procesoru velikosti paměti, velikost disku, atd.) při Azure služby založené na velikosti instance (A0 A1, A2, atd.). První úkol můžete provést modulem ETL Cruiser cloudu, s názvem sešity, kde je možné seskupit tyto materiály na instanci velikosti podobnou stránce Azure instanci služby.

### <a name="import-data-from-the-ratecard-api"></a>Import dat z rozhraní API RateCard

Sešity Cruiser cloudu poskytují automatický způsob shromáždění a zpracování informací z rozhraní API RateCard.  Sešity (extrahovat transformace – zatížením) ETL umožňují konfigurovat shromažďování, transformace a publikování dat do cloudu Cruiser databáze.

Jednotlivé sešity můžou obsahovat jednu nebo více kolekcí umožňuje sladit informace z různých zdrojů doplnit nebo rozšířit použití data. Následující dva snímky obrazovek ukazují, jak vytvořit novou *kolekci* do existující sešit a import informací do *kolekce* z rozhraní API RateCard:

![Obrázek 1 – vytvářet nové kolekce][1]

![Obrázek 2: Import dat z novou kolekci][2]

Po importu dat do sešitu, je možné vytvořit několika kroky a transformace procesy, upravovat a modelování dat. V tomto příkladu protože jsme zajímají pouze infrastruktury jako-Service (IaaS) používáme kroky transformace nepotřebných řádky odeberete nebo záznamů, týkající se ke službám kromě IaaS.

Následující obrázek ukazuje transformace postup slouží ke zpracování dat shromážděných z RateCard API:

![Obrázek 3 – postup transformace zpracuje údaje z RateCard API][3]

### <a name="defining-new-services-and-rate-plans"></a>Definování nové služby a sazby plány jednotného zasílání zpráv

Definování služeb v cloudu Cruiser různými způsoby. Jednu z možností je importovat data použití služby. Tento způsob je běžně používaných při práci s veřejné Oblaka, kde jsou služby už definovány zprostředkovatele.

Kurz plánování je sady sazeb nebo ceny, které se dají použít pro různé služby, na základě daty účinnosti nebo skupinu zákazníků i další možnosti. Plány sazba lze také v cloudu Cruiser vytvořit simulace nebo "Citlivostní" scénáře, které vysvětlení vlivu celkové náklady úlohu změny služby.

V tomto příkladu použijeme služby z rozhraní API RateCard definovat nové služby v cloudu Cruiser. Stejným způsobem můžete používáme sazby přidružené ke službám na Cruiser cloudu vytvořit nový plán sazba.

Na konci procesu transformace je možné vytvořit nový krok a publikování dat z rozhraní API RateCard jako nové služby a kurzy.

![Obrázek 4 – publikování dat z rozhraní API RateCard jako nové služby a sazby][4]

### <a name="verify-azure-services-and-rates"></a>Ověření služby Azure a sazby

Po publikování služeb a sazby, můžete ověřit seznamu importovaných služeb v cloudu Cruiser na kartě *služby* :

![Obrázek 5 – ověření nové služby][5]

Na kartě *Sazba plány jednotného zasílání zpráv* můžete zkontrolovat, že nový plán kurzu s názvem "AzureSimulation" se kurzy importovaný z rozhraní API RateCard.

![Obrázek 6 – ověření nový plán sazba a související sazeb][6]

### <a name="normalize-wap-and-azure-services"></a>Normalizace WAP a služby Azure

Ve výchozím nastavení WAP obsahuje informace o použití podle použití výpočetních, paměti a síťových prostředků. V cloudu Cruiser můžete definovat služeb na základě přímo na přidělení nebo s měřením dat používání tyto materiály. Můžete třeba nastavit základní sazbu pro každou hodinu využití procesoru nebo účtovat poplatky GB paměti přidělit instance.

V tomto příkladu abyste mohli porovnat náklady mezi WAP a Azure, potřebujeme agregovat využití prostředků v WAP do sady, které pak možné namapovat služby Azure. Tato transformace lze provést jednoduše v sešitu:

![Obrázek 7 – transformace dat WAP chcete normalizovat služby][7]

Poslední krok v sešitu je publikování dat do cloudu Cruiser databáze. Během tohoto kroku použití data jsou nyní seskupeny do služby (namapované služby Azure) a se výchozí sazby vytvořit v poplatcích stejným.

Po dokončení sešitu, můžete automatizovat zpracování dat, přidání úkolu na Plánovač a zadáním četnost a času pro sešit, který chcete spustit.

![Obrázek 8 - plánování sešitu][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Vytváření sestav pro pracovní zátěž nákladů simulace analýzy

Po použití se shromažďují a náklady jsou načíst do cloudu Cruiser databáze, jsme můžete využít modulu přehledy Cruiser cloudu vytvořit pracovní zátěž nákladů simulace, které jsme vyžadují.

Pokud chcete tento scénář je popsán, jsme vytvořili následující zprávu:

![Porovnání nákladů][9]

V horním grafu zobrazí porovnání náklady služby porovnání ceny spuštěný pracovní zátěž pro každou konkrétní službu mezi WAP (tmavě modrý) a Azure (světle modrý).

V dolní grafu zobrazí stejná data, ale nefunkční dolů tak, že oddělení. Zobrazí nákladů pro každé oddělení dělat jejich pracovního vytížení WAP a Azure, spolu s jejich rozdíl v pruhu úspor (zelené).

## <a name="azure-usage-api"></a>Rozhraní API Azure použití


### <a name="introduction"></a>Úvod

Microsoft naposledy zavádí rozhraní API pro použití Azure umožňuje účastníkům zobrazíte programově použití zásad správy informací k získání podstatu jejich spotřebu. Toto je skvělé zprávy pro zákazníky Cruiser cloudu, které můžete využít dostupné prostřednictvím toto rozhraní API bohatší datovou sadu.

Cloud Cruiser můžete využít integrace s rozhraní API použití několika způsoby. Granularity (hodinové informace o použití) a metadata informace o zdroji dostupné prostřednictvím rozhraní API poskytuje potřebné datovou sadu podporuje flexibilní Showback nebo zpětné modely. 

V tomto kurzu budeme prezentovat jeden příklad, jak vám může hodit Cruiser cloudu z informací o použití rozhraní API. Konkrétně jsme vytvoří skupina zdroje na Azure, přidružení značky pro struktuře účtu a potom popisují proces zavedení a zpracování informací značku do cloudu Cruiser.
 
Konečný cílem je moct vytvářet sestavy následující podobné a moct analyzovat náklady a spotřeby podle účtu struktury vyplněné podle značky.

![Obrázek 10 – sestava se rozdělení pomocí značek][10]

### <a name="microsoft-azure-tags"></a>Značky Microsoft Azure

Dostupné prostřednictvím rozhraní API použití Azure data obsahují pouze spotřebu informace, ale taky včetně značky přidružená metadata zdroje. Značky poskytují snadný způsob, jak uspořádat zdroje, ale aby byla efektivní, musíte zajistit, aby:

- Značky správně, použijí se k prostředkům ve dobu trvat
- Značky správně slouží procesu Showback/zpětné k nevážou použití strukturu, účet uživatele v organizaci.

Obě tyto požadavky může být obtížné, zejména v případě ručního procesu na poskytování nebo nabíjení straně. Při pomocí značek a tyto chyby může být životnost na straně nabíjení velmi obtížně srozumitelný špatně napsaných, nesprávné neobsahují žádnou hodnotu i značek jsou běžné stížnosti od zákazníků.

Nové Azure použití rozhraním API můžete cloudu Cruiser přetáhněte značek informace o zdroji a pomocí pokročilých ETL nástroji s názvem sešity, tyto běžné chyby značek opravit. Až transformaci pomocí regulární výrazy a srovnávací dat můžete cloudu Cruiser určit nesprávně označené zdroje a použít správné značky zajištění správné přidružení zdroje příjemce.

Na straně nabíjení cloudu Cruiser automaticky procesu Showback/zpětné a můžete využít značku informace, které propojují použití odpovídající příjemci (oddělení dělení, Project, atd.). Tento automatizace obsahuje velké zlepšování a můžete zajistit konzistentní a které lze auditovat nabíjení proces.
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Vytvoření skupiny zdrojů pomocí značek na Microsoft Azure
V tomto kurzu cílem prvního kroku je vytvořit skupinu zdroje na portálu Azure vytvořte nové značky k přidružení zdroje. V tomto příkladu jsme vytváření následující značky: oddělení, prostředí, vlastníka, projektu.

Následující snímek obrazovky znázorňuje výběru pole Skupina zdroje s přidruženými značky.

![Obrázek 11 – skupina zdroje s přidruženými značky prohlédnout v Azure portálu][11]

Dalším krokem je spotřebovávat informace o použití rozhraní API do cloudu Cruiser. Použití rozhraní API aktuálně poskytuje odpovědi ve formátu JSON. Tady je ukázka data načtená:


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Import dat z rozhraní API použití do cloudu Cruiser

Sešity Cruiser cloudu poskytují automatický způsob shromáždění a zpracování informací z rozhraní API použití. Sešit aplikace ETL (extrahovat transformace – zatížením) umožňuje konfigurovat kolekce, transformace a publikování dat do cloudu Cruiser databáze.

Jednotlivé sešity můžou obsahovat jednu nebo více kolekcí. Umožňuje provádět sladit informace z různých zdrojů doplnit nebo rozšířit použití data. V tomto příkladu vytvoříte nový list v sešitu Azure šablony (_UsageAPI)_ a nastavte novou _kolekci_ k importu informací z rozhraní API použití.

![Obrázek 3 – použití rozhraní API importu dat do listu UsageAPI][12]

Všimněte si, že tento sešit už ostatní listy import služby Azure (_ImportServices_) a zpracování informací spotřebu z rozhraní API fakturace (_PublishData_).

Další jsme bude pomocí rozhraní API použití k naplnění _UsageAPI_ list a sladit informace spotřebu daty z rozhraní API fakturace na listu _PublishData_ .

### <a name="processing-the-tag-information-from-the-usage-api"></a>Zpracování informací značku z rozhraní API použití

Po importu dat do sešitu, vytvoříme transformace kroky v listu _UsageAPI_ -li zpracovat informace z rozhraní API. Prvním krokem je pomocí procesor "JSON rozdělení" extrahovat značky z jednoho pole a potom vytvořit pole u každého z nich (oddělení, Project, vlastník a prostředí).

![Obrázek 4 – vytvoření nových polí informace značky][13]

Všimněte si službu "Sítí" chybí informace štítku (žlutá políčko), ale nemůžeme zkontrolujte, zda je součástí stejné skupiny prostředků pohledem pole _ResourceGroupName_ . Protože máme značky u jiných zdrojů z této skupiny zdrojů, jsme pomocí tyto informace použít chybějící značky k tomuto prostředku později v procesu.

Dalším krokem je vytvoření přidružení informace z značky _ResourceGroupName_vyhledávací tabulky. Vyhledávací tabulka se použijí na dalším krokem k obohacení spotřebu dat s informacemi o značku.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>Přidání značky informací k datům spotřeba

Teď můžeme přejít na listu _PublishData_ zpracovává informace spotřebu z rozhraní API fakturace a přidejte pole extrahovaných z značky. Tento proces se provádí prohlížíte vyhledávací tabulce vytvořili v předchozím kroku, používají _ResourceGroupName_ klávesu vyhledávání.

![Obrázek 5 – naplňovat struktuře účet s informacemi z polích pro vyhledávání][14]

Všimněte si, že byly použity polích struktury odpovídající účet služby "Sítí", řešení problému s chybějící značky. Také jsme vyplněné polích účtu struktury zdrojů než Naším cílem pole Skupina zdroje s "Jiné" k odlišení na sestavy.

Teď jenom potřebujeme Přidání kroku publikovat využití. Během tohoto kroku odpovídající sazeb pro každou službu definované na naše kurz plánování použije se informace o použití s Výsledná částka do databáze.

Nejlepší je, že máte jenom jednou absolvovat tento proces. Po dokončení sešit potřebujete jenom přidat k plánovači a bude spuštěn hodinové nebo denní naplánovaný. Je právě předmětem vytváření nových sestav nebo přizpůsobovat existující, abyste mohli analyzovat data a získat smysluplné přehledy z cloudu použití.

### <a name="next-steps"></a>Další kroky

+ Podrobné informace o vytváření cloudu Cruiser sešity a sestavy získáte cloudu Cruiser online [si přečtěte následující dokumentaci](http://docs.cloudcruiser.com/) (třeba platné přihlášení).  Další informace o Cruiser cloudu, obraťte se prosím [info@cloudcruiser.com](mailto:info@cloudcruiser.com).
+ Základní informace o používání zdrojů Azure a rozhraní API RateCard naleznete v tématu [získání podstatu využití prostředků Microsoft Azure](billing-usage-rate-card-overview.md) .
+ Podívejte se na [Azure fakturace REST API odkaz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) Další informace o obou rozhraní API, které jsou součástí sady rozhraní API správcem zdrojů Azure k dispozici.
+ Pokud chcete pusťte do ukázkový kód, podívejte se na našeho Microsoft Azure fakturace rozhraní API ukázky na [Azure ukázky](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Víc se uč
+ Naleznete v článku [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md) Další informace o správce prostředků Azure.

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Obrázek 1 – vytvářet nové kolekce"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Obrázek 2: Import dat z novou kolekci"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Obrázek 3 – postup transformace zpracuje údaje z RateCard API"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Obrázek 4 – publikování dat z rozhraní API RateCard jako nové služby a sazby"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Obrázek 5 – ověření nové služby"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Obrázek 6 – ověření nový plán sazba a související sazeb"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Obrázek 7 – transformace dat WAP chcete normalizovat služby"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Obrázek 8 - plánování sešitu"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Obrázek 9 - ukázkové sestavy pro scénář porovnání pracovní zátěž náklady"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Obrázek 10 – sestava se rozdělení pomocí značek"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Obrázek 11 – skupina zdroje s přidruženými značky prohlédnout v Azure portálu"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Obrázek 12 - použití rozhraní API importu dat do listu UsageAPI"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Obrázek 13 – vytvoření nových polí informace značky"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Obrázek 14 - naplňovat struktuře účet s informacemi z polích pro vyhledávání"
