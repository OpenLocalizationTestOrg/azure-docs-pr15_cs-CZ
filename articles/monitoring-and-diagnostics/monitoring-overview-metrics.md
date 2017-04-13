<properties
    pageTitle="Základní informace o metriky v Microsoft Azure | Microsoft Azure"
    description="Základní informace o metriky a jejich použití v Microsoft Azure"
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="ashwink"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Základní informace o metriky v Microsoft Azure 

Tento článek popisuje, co metriky jsou v Microsoft Azure svoje výhody a jak začít používat je.  

## <a name="what-are-metrics"></a>Jaké jsou metriky?

Azure Monitor umožňuje využívat telemetrie abyste získali přehled o výkonu a stavu úloh na Azure. Nejdůležitější typ Azure telemetrickými daty jsou metriky (nazývané také výkonnosti), které nejčastěji Azure zdroje. Azure Monitor nabízí několik způsobů, jak konfigurovat a používat tyto metriky pro sledování a odstraňování potíží.


## <a name="what-can-you-do-with-metrics"></a>Co se dá dělat s metriky?

Metriky jsou zdrojem cenný telemetrie a umožňují postupujte takto:

- **Sledování výkonu** prostředku (například OM, web nebo aplikaci použití logických operátorů) tak, že zakreslení jeho metriky na portálu grafu a Připnutí tento graf do řídicího panelu.
- **Oznámení o nějaké záležitosti** ovlivnění výkonu prostředku, když metriky překračuje určitou prahovou hodnotu.
- **Konfigurace automatické akce**, například neobsahovaly text zdroj nebo dotazů postupu runbook když metriky překračuje určitou prahovou hodnotu.
- **Provádět pokročilé technologie pro analýzu** nebo zprávy o výkonu a využití trendů vaše zdroje.
- **Archiv** výkonu a stavu historie vašich zdroje **pro dodržování předpisů/auditování** představ.

##  <a name="metric-characteristics"></a>Metriky vlastnosti
Metriky splňovat tyto požadavky:

- Všechny metriky mít **frekvenci 1 minuty**. Dostáváte hodnotu metriky každou minutu od vašeho zdroje pojmenování můžete v reálném čase přehled o stavu a stavu příslušného zdroje.
- Metriky jsou **k dispozici o krabice aniž by musel vyjádření výslovného souhlasu** nebo nastavení další diagnostiky.
- Přístup k **30 dní historie** každý metriky. Můžete rychle vyhledat trendy nedávných a měsíční v se výkonu a stavu příslušného zdroje.

Můžeš:

- Snadno zjišťovat, přístup a **Zobrazit všechny metriky** prostřednictvím portálu Azure při vyberte zdroj a vykreslovat v grafu. 
- Konfigurace metrických **upozornění pravidla, která odešle oznámení nebo automatické akci, která bude** při metriky protíná mezní hodnoty, které jste nastavili. Automatické měřítko je speciální automatické akci, která umožňuje zobrazit zdroj zahájit příchozí žádosti nebo načíst na vašem webu nebo výpočet zdroje. Můžete nakonfigurovat pravidlo nastavení automatické měřítko zobrazit mimo/v podle metriky přes prahovou hodnotu.
- **Archiv** metriky po delší dobu nebo použít pro vytváření sestav v režimu offline. Můžete směrovat metriky do úložiště objektů blob, při konfiguraci diagnostiky nastavení pro zdroj.
- Metriky **toku** k rozbočovači události umožňuje potom směrovat je Azure toku technologie pro analýzu a vlastní aplikace pro analýzy v reálném čase. Můžete dělat pomocí diagnostických nastavení.
- **Směrování** všech metriky OMS protokolu analýzy odemknout rychlé analýzy, hledání a vlastní výstrahy metriky dat ze zdroje.
- **Spotřebovat** metriky prostřednictvím nového Azure Monitor REST API.
- Pomocí rutin prostředí PowerShell nebo rozhraní REST API platformy metriky **dotazu** .

 ![Směrování metriky v Azure monitoru](./media/monitoring-overview-metrics/MetricsOverview0.png)

## <a name="access-metrics-via-portal"></a>Metriky přístup prostřednictvím portálu
Tady je stručný návod při vytváření metrické grafu prostřednictvím Azure portálu

### <a name="view-metrics-after-creating-a-resource"></a>Zobrazení metriky po vytvoření zdroje
1. Otevřete portál Azure
2. Vytvoření nové aplikace služby – webu.
3. Po vytvoření webu přejděte na zásuvné přehled webu.
4. Nové metriky můžete zobrazit jako "Sledování" dlaždice. Můžete upravit dlaždici a vyberte další nastavení

 ![Metriky zdroje v nástroji Sledování Azure](./media/monitoring-overview-metrics/MetricsOverview1.png)    

### <a name="access-all-metrics-in-a-single-place"></a>Přístup ke všem metriky na jednom místě
1. Otevřete portál Azure 
2. Přejděte na nové kartě Monitor a vyberte možnost nastavení pod ní 
3. V rozevíracím seznamu můžete vybrat předplatného, skupina zdroje a název zdroje. 
4. Teď můžete zobrazit seznam dostupných metriky. 
5. Vyberte míru zajímají a vymyslete. 
6. Můžete připnout ho na řídicí panel kliknutím na PIN kód v pravém horním rohu.

 ![Přístup k všechny metriky na jednom místě v nástroji Sledování Azure](./media/monitoring-overview-metrics/MetricsOverview2.png) 


>[AZURE.NOTE] Získat přístup k úrovni hostitele metriky VMs (na základě Azure správce zdrojů) a OM měřítko sady bez žádné další diagnostické nastavení. Tyto nové úrovni hostitele metriky jsou k dispozici pro instance systému Windows a Linux. Tyto metriky jsou nechcete být zaměňovány s úrovně metriky hosta OS, které máte přístup k při zapnutí Azure diagnostiky na VMs nebo VMSS. Další informace o konfiguraci Azure diagnostiky najdete v tématu [Co je diagnostických nástrojů Microsoft Azure](../azure-diagnostics.md).

## <a name="access-metrics-via-rest-api"></a>Metriky přístup prostřednictvím rozhraní REST API
Azure metriky můžete k nim získat přístup prostřednictvím rozhraní API Azure Monitor. Existují dvě rozhraní API, které vám pomohou zjistit a přístup k metriky. Použití: 

- [Definice azure Monitor míru rozhraní REST API](https://msdn.microsoft.com/library/mt743621.aspx) pro přístup k seznamu metriky dostupné pro službu.
- [Azure Monitor metriky REST API](https://msdn.microsoft.com/library/mt743622.aspx) pro přístup k datům skutečné metriky

>[AZURE.NOTE] Tento článek popisuje metriky pomocí [nového rozhraní API pro metriky](https://msdn.microsoft.com/library/dn931930.aspx) Azure zdrojů. Verze rozhraní API pro nové metrických definice rozhraní API 2016 03 01 označeným je verze pro metriky rozhraní API 2016-09-01. Starší verze metrických definice a metriky můžete přistupovat pomocí verze rozhraní api 2014-04-01.

Podrobnější informace pomocí Azure Monitor REST API, najdete v článku [Azure Monitor REST API návodu](monitoring-rest-api-walkthrough.md).

## <a name="export-options-for-metrics"></a>Možnosti pro metriky exportu
Můžete přejděte na zásuvné protokolování diagnostiky na kartě Monitor a zobrazte možnosti exportu pro metriky. Můžete vybrat metriky (a protokoly pro diagnostiku) na směrovány k úložišti objektů Blob, rozbočovače událost nebo OMS protokolu Analytics pro případy použití jsme zmínili dříve v tomto článku. 

 ![Možnosti exportu pro metriky v nástroji Sledování Azure](./media/monitoring-overview-metrics/MetricsOverview3.png)   

To můžete nakonfigurovat pomocí Správce prostředků šablony, [Powershellu](insights-powershell-samples.md), [rozhraní příkazového řádku](insights-cli-samples.md) nebo [Rozhraní REST API](https://msdn.microsoft.com/library/dn931943.aspx). 

## <a name="take-action-on-metrics"></a>Provést akci metrice
Můžete zasílat oznámení ve formě nebo automatické akcí v metrických data. Můžete nakonfigurovat nastavení automatické měřítko k tomu nevyzve nebo upozornění pravidla.

### <a name="alert-rules"></a>Upozornění pravidel
Konfigurace upozornění pravidla metrice. Pokud metriky má překročených určitou prahovou hodnotu a upozorněním prostřednictvím e-mailu nebo aktivováno webhook, která lze spustit vlastní skript můžete zkontrolovat následujícími pravidly upozornění. Můžete taky webhook konfigurace 3 integrace produktu.

 ![Metriky a upozornění pravidla v Azure monitoru](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale"></a>Automatické měřítko
Některé Azure prostředky podpory víc instancí zobrazit, nebo v případě svého pracovního vytížení. Automatické měřítko platí pro aplikace služby (webové aplikace), virtuální počítač měřítko sady (VMSS) a klasické Cloudovým službám. Můžete nakonfigurovat automatické měřítko pravidla pro měřítko nebo v některých metriky ovlivňující ochranu vaše pracovní zátěž protíná mezní hodnoty, které zadáte. Další informace najdete v tématu [Přehled neobsahovaly text](monitoring-overview-autoscale.md).

 ![Metriky a automatické měřítko v nástroji Sledování Azure](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="supported-services-and-metrics"></a>Podporované služby a metriky
Azure Monitor je nová infrastruktura metriky. Poskytuje podporu pro následující Azure služby v portálu Azure a novou verzi rozhraní API Azure monitoru:

- VMs (na základě Azure správce zdrojů)
- Sady měřítko OM
- Dávkové
- Obor názvů centrální události 
- Obor názvů služby Bus (pouze premium SKU)
- SQL (verze 12)
- Fond pružná SQL
- Weby
- Web serverové farmy
- Použití logických operátorů aplikace
- IoT rozbočovače
- Redis mezipaměti
- Sítě: Bran aplikace
- Hledání

Můžete zobrazit podrobný seznam všech podporovaných služeb a jejich metriky v [Azure Monitor metriky - podporované metriky za typ zdroje](monitoring-supported-metrics.md). 


## <a name="next-steps"></a>Další kroky

Podívejte se na odkazy v tomto článku. Informace navíc:  

- informace o [běžných metriky pro neobsahovaly text](insights-autoscale-common-metrics.md)
- Jak [vytvořit upozornění pravidel](insights-alerts-portal.md)




