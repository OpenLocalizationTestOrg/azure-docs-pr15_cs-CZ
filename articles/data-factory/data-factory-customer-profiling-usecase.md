<properties 
    pageTitle="Použít písmena – vytvoření profilu zákazníka" 
    description="Zjistěte, jak Azure Data Factory slouží k vytvoření založených na datech pracovního postupu (kanálem k odesílání zpráv) do profilu herní zákazníci." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="use-case---customer-profiling"></a>Použít písmena – vytvoření profilu zákazníka

Azure Factory dat je jedním z mnoha služeb provádět sadu Intelligence Cortana akcelerátory řešení.  Další informace o Cortana měřítka Navštěvujte blog o [Cortana Intelligence sadu](http://www.microsoft.com/cortanaanalytics). V tomto dokumentu popisu případu použití jednoduchého vám pomůžou začít s vysvětlení, jak můžete Azure Data Factory řešení běžných potíží analýzy.

Potřebujete přístup a vyzkoušet si tento případ použití jednoduchého stačí [Azure předplatného](https://azure.microsoft.com/pricing/free-trial/).  Ukázka implementující tento případ použití podle kroků popsaných v článku [Příklady](data-factory-samples.md) nástroje můžete nasazovat.

## <a name="scenario"></a>Scénář

Contoso je herní společnost, která se vytvoří her pro více platformy: her konzoly, ruční zařízení a počítače (PC). Jak přehrávače tyto hry, pochází velké množství dat protokolu sledující vzorce použití, herní stylu a předvolby uživatele.  Když v kombinaci s demografické, místní a údajů o produktech, Contoso můžete provádění analýzy průvodce o tom, jak zážitek hráčů cílové nim upgradech a v hra nákup. 

Cíl společnosti Contoso je určit příležitosti nahoru – zákazník/křížově – prodej podle historii herní jeho účastníků a přidání atraktivní funkcí k růstu podniku a poskytnout lepší zákazníkům. Pro tento případ použití používáme herní společnosti jako příklad podniku. Chce optimalizovat jeho her na základě hráčů chování. Tyto zásady platí pro všechny business, který chce zapojit svým zákazníkům kolem jeho zboží a služeb a zážitek svým zákazníkům.

## <a name="challenges"></a>Úkoly


## <a name="solution-overview"></a>Přehled řešení

Tento případ použití jednoduchého mohou sloužit jako příklad použití Azure Data Factory jedí Příprava, transformace, analyzovat a publikovat data.

![Pracovní postup začátku do konce](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Tento obrázek znázorňuje, jak potrubí dat na portálu Azure po zobrazovat již byly nasazeny.

1.  **PartitionGameLogsPipeline** načítá nezpracovanými herní události z úložiště objektů blob a vytvoří oddílů podle rok, měsíc a den.
2.  **EnrichGameLogsPipeline** spojí rozdělený herní událostí pomocí geo kód referenčních dat a obohacuje data pomocí mapování IP adres do odpovídajícího umístění geo.
3.  Profilace **AnalyzeMarketingCampaignPipeline** používá Obohacená data a procesy s reklamě dat, aby vznikly konečný výstup, který obsahuje marketingové kampaně účinnosti.

V tomto příkladu Data Factory slouží k organizovat činnosti, které kopírovat zadávání dat, transformace a proces data a konečné exportovat data k databázi SQL Azure.  Můžete taky vizualizace dat potrubní síť, je Správa a sledovat svůj stav v uživatelském rozhraní.

## <a name="benefits"></a>Výhody

Optimalizace jejich analýzy profilu uživatele a zarovnání s obchodními cíli, herní společnosti je možné rychle shromáždit využití vyhledávání a analyzovat účinnosti marketingových kampaní.




