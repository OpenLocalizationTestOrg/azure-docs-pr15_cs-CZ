<properties 
    pageTitle="Úvod do technologie pro analýzu toku | Microsoft Azure" 
    description="Informace o toku analýzy, služba spravovaných, který umožňuje analyzovat streamování data z Internetu věcí (IoT) v reálném čase." 
    keywords="technologie pro analýzu jako služba, spravovat služeb, zpracování toku, datových proudů analýzy, co je technologie pro analýzu toku"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="what-is-stream-analytics"></a>Co je technologie pro analýzu toku?

Azure analýzy toku je modul zpracování plně spravovaných a nákladů efektivní událostí v reálném čase, který umožňuje odemknout hloubkové přehledy z dat. Technologie pro analýzu toku snadno nastavit v reálném čase analytický výpočty dat streamování ze zařízení, senzory, weby, sociálních médií, aplikací, infrastruktura systémy a další.

Několika kliknutími v portálu Azure vytváříte toku analýzy úlohy zadání vstupních zdroje streamování dat jímce výstup výsledků práce, a transformace dat vyjádřený v jazyce SQL profesionálové. Můžete sledovat a úprava měřítko a rychlosti práce na portálu Azure zobrazit z několika kB gigabajtů nebo více zpracovaných sekundu událostí.

Toku analýzy využití roky Microsoft Research práce ve vývoji vysoce správně zapnuli streamování moduly na řeč pro zpracování citlivé na čas i jazyk integrací aplikace intuitivní specifikace těchto.

## <a name="what-can-i-use-stream-analytics-for"></a>Co můžu používat toku analýzy?
Velké objemy dat jsou toku na vysokorychlostní než dnes. Organizacím, které můžete zpracování a pracovat na tomto streamování data v reálném čase může výrazně zvýšit efektivitu a odlišení na trhu. Scénáře v reálném čase streamování analýzy najdete přes všechny průmysl: přizpůsobených, v reálném čase obchodování burzovního analýzy a upozornění na poskytované společností finančních služeb; data v reálném podvod zjišťování; služby ochrany dat a identity; spolehlivé požití a analýza dat generovaných čidly a válcích vložené do pole fyzicky objektů (Internet z, ale i IoT); Služba Web analytics clickstream; a aplikace pro zákazníka relací správu (CRM) vydávání upozornění, pokud prostředí zákazníka v určeném časovém rozmezí má sníženou kvalitu. Firmy hledáte flexibilní, spolehlivý a efektivní způsob, jak se tyto analýza dat v reálném čase události toku úspěšné vysoce konkurenční firmy moderní světě.

## <a name="key-capabilities-and-benefits"></a>Klíčové funkce a výhody
-   **Snadnější použití:** Technologie pro analýzu toku podporuje model jednoduché, deklarativní dotazu pro popis transformace. Pokud chcete optimalizovat pro snadnější použití, analýzy toku používá varianty T-SQL a odstraní potřebné pro zákazníky řešení technické složitostí toku zpracování systémy. Použití [analýzy toku dotazu jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx) v editoru dotazů v prohlížeči, můžete získat intelli smysl automatického dokončování vám pomůže můžete rychle a snadno implementovat časové řady dotazech, včetně časový založené na spojení, zobrazením agregace, časový filtrů a dalších společná operace například spojení, agregace, průzkumy a filtry. V prohlížeči dotazu otestování Ukázka datového souboru navíc umožňuje snadné iterativních vývojářské.  

-   **Škálovatelnost:** Technologie pro analýzu toku je schopen zpracování vysoké události výkon až 1GB/druhé. Integrace se službou [Azure události rozbočovače](https://azure.microsoft.com/services/event-hubs/) a [Azure IoT rozbočovače](https://azure.microsoft.com/services/iot-hub/) povolit řešení jedí miliony událostí každého druhého pochází z připojeného zařízení, clickstreams a soubory pojmenování několik protokolu. K dosažení tohoto toku analýzy využití funkci rozdělení rozbočovače události, které může mít 1 MB/s na oddíl. Uživatelé můžou oddílů výpočet do několika logických kroků v rámci definice dotazu, oba objekty mají možnost dále oddíly zvětšíte škálovatelnost.  

-   **Spolehlivost, opakovatelnosti a snadné obnovení:** Služba spravovaných v cloudu, analýzy toku pomáhá předešli ztrátě dat a poskytuje nepřerušený v případě chyby prostřednictvím předdefinované obnovení. Možnost interně udržovat státu tato služba poskytuje opakující výsledky zajistit, že je možné archivovat události a opakované použití zpracování v budoucnu, vždy začíná stejných výsledků. Díky zákazníci přejděte zpátky v čase a prošetřit výpočty při provádění analýzu hlavních příčin citlivostní analýzy, atd.  

-   **Nízké náklady:** Jako do cloudové služby analýzy toku optimalizován umožníte uživatelům velmi nízký nákladů Začínáme a udržovat v reálném čase analýzy řešení. Služba je vytvořen za vás zaplatit průběžně založené na použití datových proudů jednotky a velikost dat zpracovaných systému. Použití pochází podle objemu zpracovaných událostí a množství výpočetního výkonu zřízení v rámci clusteru zpracovávat odpovídajících úlohy toku analýzy.  

-   **Odkazují na data:** Technologie pro analýzu toku umožňuje uživatelům můžete zadat a použít odkaz data. Může to být historických data nebo jednoduše není streaming nezměnilo méně často určitou dobu. Systém zjednodušuje používání referenčních dat zpracovávaly jako jakékoli jiné příchozí události datového proudu připojení přes datové proudy události požití v reálném čase provést transformací.  

-   **Funkce definované uživatelem:** Technologie pro analýzu toku má integrace se službou Azure počítače výukové definovat volání funkce služby strojového výukové jako součást dotazu toku analýzy. Rozbalení možnosti analýzy toku můžete využít existujících řešení výukové počítače Azure. Další informace o tom zkontrolujte [počítač výukové integrace kurz](stream-analytics-machine-learning-integration-tutorial.md).

-   **Připojení:** Toku analýzy ke kterému je připojen přímo Azure události rozbočovače a Azure IoT rozbočovače toku požití a služba objektů Blob Azure jedí historických data. Výsledky můžete být z toku analýzy došlo k zápisu Azure úložiště objektů BLOB nebo tabulky databáze SQL Azure, úložiště jezera dat Azure, DocumentDB, rozbočovače události, témata Bus služby Azure nebo fronty a Power BI, tam, kde můžete pak být vizualizovat, dál zpracovány službou pracovní postupy, použité v dávku analýzy přes [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) nebo znovu zpracována jako řadu událostí. Při použití rozbočovače událostí je možné vytvořit více analýzy toku společně s dalšími zdroje dat a zpracování moduly pro řeč bez ztráty streamování přírodní výpočty.  

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
Jste byla zavedená do analýzy toku, služba spravovaných datového proudu technologie pro analýzu dat z Internetu věci. Další informace o této službě najdete v tématu:

- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

