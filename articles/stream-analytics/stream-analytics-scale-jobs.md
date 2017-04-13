<properties
    pageTitle="Změnit velikost toku analýzy úlohy zvýšit výkon | Microsoft Azure"
    description="Zjistěte, jak zobrazit úlohy toku technologie pro analýzu konfigurace vstupní oddíly, optimalizace definice dotazu a nastavením úlohy streamování jednotky."
    keywords="data streaming, datových proudů zpracování dat, ladění analýzy"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Změna velikosti Azure analýzy toku úlohy zvýšit výkon toku zpracování dat.

Zjistěte, jak ladění úlohy technologie pro analýzu a výpočty *streamování jednotek* pro analýzy toku, jak zobrazit úlohy toku technologie pro analýzu konfigurace vstupní oddíly, optimalizace definice dotazu technologie pro analýzu a nastavením úlohy streamování jednotky. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Jaké jsou díly toku analýzy projektu?
Definice úlohy toku analýzy obsahuje vstupy, dotaz a výstupu. Vstupní data nejsou ze kdy úlohy přečte toku dat, dotaz se používá k transformaci vstupní toku dat a výstup je místo, kam úlohy odešle výsledky úlohy.  

Projekt vyžaduje aspoň jeden vstupní zdroj pro přenos dat. Vstupní zdroj toku dat mohou být uloženy v rozbočovači Azure služby Bus událost nebo v úložišti objektů Blob Azure. Další informace najdete v tématu [Úvod do technologie pro analýzu toku Azure](stream-analytics-introduction.md) a [začít používat Azure toku analýzy](stream-analytics-get-started.md).

## <a name="configuring-streaming-units"></a>Konfigurace streamování jednotky
Streamování jednotky (SUs) představují zdrojů a výpočetní výkon povinné spuštění úlohy Azure toku analýzy. SUs lze popsat relativní zpracování kapacity na základě prolnutí rozměru procesoru, paměti, události a čtení a zápis sazby. Jednotlivých datových proudů jednotka odpovídá zhruba 1MB/druhé výkon. 

Výběr kolik SUs jsou potřeba pro určitý projekt, závisí na oddíl konfigurace pro vstupní hodnoty a dotaz definovaný pro daný úkol. Můžete vybrat až kvóty v datových proudů jednotky práce pomocí portálu klasické Azure. Každý Azure předplatné ve výchozím nastavení obsahuje kvóta až 50 streamování jednotek pro všechny úlohy analýzy v určité oblasti. Pokud chcete zvýšit streamování jednotek pro předplatné, kontaktujte [Podporu od Microsoftu](http://support.microsoft.com).

Počet datových proudů jednotek můžete využít úlohy závisí na oddíl konfigurace pro vstupní hodnoty a dotaz definovaný pro daný úkol. Všimněte si také, že platnou hodnotu jednotek toku musí používat. Platné hodnoty začít od 1, 3, 6 a potom nahoru v částech 6, jak je ukázáno v následujícím příkladu.

![Azure toku analýzy můžete vysílat datovými proudy měřítko jednotky][img.stream.analytics.streaming.units.scale]

Tento článek vám ukáže, jak vypočítat a ladění dotazu zvýšit výkon pro analýzy úlohy.

## <a name="embarrassingly-parallel-job"></a>Embarrassingly paralelní projektu
Embarrassingly paralelní úloha byla nejčastěji scalable scénáře, které máme v Azure toku analýzy. Jeden oddíl předávat na vstupu výskyt dotaz se připojí k jeden oddíl výstupu. Dosažení tento paralelismus vyžaduje několik věcí:

1.  Pokud logiky dotazu jsou závislé na klávesu stejné zpracovávání ve stejné instanci dotazu, musíte zajistit, že události přejděte do stejného oddílu zadání. V případě události rozbočovače to znamená, že Data události, musí mít **PartitionKey** nastavení nebo můžete použít oddíly odesílatelů. U objektů Blob to znamená odeslání události do stejné složky oddíl. Pokud logiky dotazu nevyžaduje stejný klíč zpracovat stejné instanci dotazu, můžete tohoto požadavku na ignorovat. Příklad by jednoduchého dotazu vyberte / / filtr projektu.  
2.  Jakmile se data je rozloženy, jako je třeba jej na straně vstupní, potřebujeme zajistit, že je rozdělen dotazu. Je nutné nepoužívá **Oddílu** v celém postupu. Jsou povoleny několik kroků, ale všechny musí být rozdělen stejné klíčem. Dalším krokem je potřeba pamatovat je, že v současné době klávesu rozdělení je třeba nastavit na **ID oddílu** mít plně paralelní projektu.  
3.  Pouze rozbočovače událostí a objektů Blob momentálně podporují rozdělený výstupu. Pro události rozbočovače výstup budete muset konfigurace pole **PartitionKey** jako **ID oddílu**. U objektů Blob nemusíte nic dělat.  
4.  Dalším krokem je potřeba pamatovat, počet oddílů, zadávání musí být rovna počet oddílů výstupu. Výstup objektů BLOB aktuálně nepodporuje oddíly, ale totiž nevadí zdědí schématu rozdělení oddílů nadřazeného dotazu. Příklady oddíl hodnoty, které umožní plně paralelní úlohy:  
    1.  8 rozbočovače události vstupní oddíly a 8 rozbočovače události výstupní oddíly
    2.  8 rozbočovače události při zadávání oddíly a objektů Blob výstup  
    3.  8 oddíly objektů blob vstupní a výstupní objektů Blob  
    4.  8 objektů blob vstupní oddíly a 8 rozbočovače události výstup oddíly  

Tady jsou některé příklady scénářů, které jsou embarrassingly paralelní.

### <a name="simple-query"></a>Jednoduchý dotaz
Zadání vstupních hodnot – události rozbočovače 8 oddílů výstup – události centrální 8 oddílů

**Dotaz:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Tento dotaz je jednoduchý filtr a jako takové jsme nemusíte bát rozdělení vstupní posílané do události rozbočovače. Zjistíte, že dotaz má **Oddílu podle** části **ID oddílu**, proto jsme splňují požadavek #2 od výše uvedeného. Výstup potřebujeme konfigurace výstupu rozbočovače událostí v projektu na pole **PartitionKey** nastaveno na **ID oddílu**. Poslední šeků, zadávání oddíly == výstup oddíly. Tato topologie je embarrassingly paralelní.

### <a name="query-with-grouping-key"></a>Dotaz s viditelnou klávesou seskupení
Zadání vstupních hodnot – události rozbočovače 8 oddílů výstup – Blob

**Dotaz:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Tento dotaz má klíč seskupení a jako takové stejný klíč vyžaduje zpracování ve stejné instanci dotazu. To znamená, že potřebujeme odešlete naše události události rozbočovače rozdělený způsobem. Kterou klávesu věnuje? **ID oddílu** je použití logických operátorů koncepce projektu, které skutečné klíč, který se věnuje **TollBoothId**. Znamená to, že jsme by měl nastavit **PartitionKey** dat události chcete odeslat rozbočovače událost jako **TollBoothId** události. Dotaz má **Oddílu podle** části **ID oddílu**, proto jsme jsou vhodné. Pro výstup protože je objektů Blob, jsme nemusí starat o konfiguraci **PartitionKey**. Pro požadavek #4 znovu jde objektů Blob, jsme nemuseli starat o to. Tato topologie je embarrassingly paralelní.

### <a name="multi-step-query-with-grouping-key"></a>Více krok dotazu s viditelnou klávesou seskupení ###
Zadání vstupních hodnot – události centrální 8 oddílů výstup – události centrální 8 oddílů

**Dotaz:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Tento dotaz má klíč seskupení a jako takové stejný klíč vyžaduje zpracování ve stejné instanci dotazu. Můžete použít stejné strategie jako předchozí dotaz. Dotaz obsahuje několik kroků. Má každý krok **Oddílu podle** části **ID oddílu**? Ano, proto jsme dobrý. Výstup potřebujeme **PartitionKey** hodnotu **ID oddílu** je popsána nad a můžete taky vidíme, že má stejný počet oddílů jako vstup. Tato topologie je embarrassingly paralelní.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Příklady scénářů, které nejsou embarrassingly paralelní

### <a name="mismatched-partition-count"></a>Neshoda počet oddílů ###
Zadání vstupních hodnot – události rozbočovače 8 oddílů výstup – události centrální 32 oddílů

Nezávisle na tom, co dotaz totiž v tomto případě zadání oddílů počet! = počet oddílů výstupu.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Nepoužíváte rozbočovače událost nebo objekty BLOB jako výstup
Zadání vstupních hodnot – události rozbočovače 8 oddílů výstup – PowerBI

Výstup PowerBI aktuálně nepodporuje rozdělení.

### <a name="multi-step-query-with-different-partition-by-values"></a>Více krok dotazu s různými hodnotami oddílu podle
Zadání vstupních hodnot – události centrální 8 oddílů výstup – události centrální 8 oddílů

**Dotaz:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Jak vidíte, druhým krokem používá **TollBoothId** jako klávesu rozdělení. To je shodný s cílem prvního kroku a tím nám dělat náhodně. 

Toto jsou některé příklady a counterexamples toku analýzy úloh, které budou moct dosažení embarrassingly paralelní topologie a s nimi potenciální maximální měřítko. Pro projekty, které neodpovídají jednu z těchto profily budou budoucích aktualizacích o tom, jak lze uchovávat měřítko některé další kanonický scénáře toku analýzy.

Nyní vytvářecího dosáhnete obecných pokynů níže:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Vypočítá maximální počet datových proudů jednotky projektu
Celkový počet datových proudů jednotky, které může používat úlohy toku analýzy závisí na počet kroků v tématu dotaz definovaný projektu a počet oddílů u každého kroku.

### <a name="steps-in-a-query"></a>Kroky v dotazu
Dotaz může obsahovat jednu nebo více kroky. Každý krok je podřízený dotaz definovaný pomocí klíčových slov **s** . Jenom dotaz, který je mimo klíčového slova **s** se počítá také jako kroku, například příkazu **SELECT** v následující dotaz:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Předchozí query má dva kroky.

> [AZURE.NOTE] Tento ukázkový dotaz bude vysvětleno dále v tomto článku.

### <a name="partition-a-step"></a>Oddíl kroku

Rozdělení kroku vyžaduje následující podmínky:

- Je třeba rozdělit vstupní zdroj. Další informace najdete v článku [Průvodce rozbočovače programování událostí](../event-hubs/event-hubs-programming-guide.md).
- Příkaz **SELECT** dotazu musí číst z oddílů vstupní zdroj.
- Dotaz v rámci kroku, musí mít **Oddílu podle** klíčových slov

Po rozdělení dotazu vstupní události budou zpracovány a agregovaná v samostatný oddíl skupiny a výstupy události se vytvářejí pro každou skupinu. Pokud kombinované agregovaná hodnota je vhodné, musíte vytvořit druhý krok bez oddílů agregované.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Vypočítá maximální počet datových proudů jednotky práce

Všechny kroky bez oddílů společně můžete změnit velikost až 6 streamování jednotek pro analýzy toku úlohu. Chcete-li přidat další streamování jednotky, kroku je třeba rozdělit. Každý oddíl může obsahovat šest streamování jednotky.

<table border="1">
<tr><th>Dotaz úlohy</th><th>Maximální počet datových proudů jednotek pro danou úlohu</th></td>

<tr><td>
<ul>
<li>Dotaz obsahuje postupně po jednotlivých krocích.</li>
<li>V kroku neobsahuje oddíly.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Zadávání datového proudu rozdělena vynásoben 3.</li>
<li>Dotaz obsahuje postupně po jednotlivých krocích.</li>
<li>V kroku oddíly.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Dotaz obsahuje dva kroky.</li>
<li>Žádná z kroků oddíly.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>Zadávání toku dat je rozdělen vynásoben 3.</li>
<li>Dotaz obsahuje dva kroky. Oddíly v zadávání kroku a druhý krok není.</li>
<li>Příkaz SELECT načte z oddílů vstupní.</li>
</ul>
</td>
<td>24 (18 rozdělený postup) + 6 postup bez oddílů</td></tr>
</table>

### <a name="examples-of-scale"></a>Příklady měřítko
Následující dotaz vrátí počet automobilů filtrované pomocí stanice placená s tři tollbooths v okně tři minuty. Tento dotaz je možné měřítko až 6 streamování jednotek.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Použití více datových proudů jednotek pro dotaz, obě data můžete vysílat datovými proudy vstupní a musí být rozdělen dotaz. Vzhledem k tomu, že oddílu toku dat je nastavena na hodnotu 3, je možné použít následující dotaz změněné měřítko až 18 streamování jednotky:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Při rozdělení dotazu vstupní události zpracování a agregovat ve skupinách samostatný oddíl. Výstup události vygenerují pro každou skupinu. Rozdělení může způsobit některé neočekávané výsledky při pole **Seskupit podle** není klíč oddílu v zadávání dat proudu. Například pole **TollBoothId** v předchozí ukázkový dotaz není oddíl klíč Input1. Data z #1 mýtná celnice můžete šířit ve více oddílů.

Jednotlivé oddíly Input1 zpracovávat samostatně toku technologie pro analýzu a více záznamů auta – průchodu prostřednictvím počet různých hodnot stejné mýtná celnice ve stejném okně tumbling se vytvoří. Pokud klíč vstupní oddílu nelze změnit, můžete tento problém opravit přidáním další krok není oddíl, například:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Tento dotaz je možné měřítko až 24 streamování jednotky.

>[AZURE.NOTE] Pokud se připojujete dvěma datovými proudy, ujistěte se, že jsou datové proudy rozděleny klíčem oddíl sloupce, že provedete spojení a mít stejný počet oddílů v obou datových proudů.


## <a name="configure-stream-analytics-job-partition"></a>Konfigurace toku analýzy úlohy oddíl

**Úprava datových proudů jednotky práce**

1. Přihlaste se k [portálu Správa](https://manage.windowsazure.com).
2. V levém podokně klikněte na **Analýza proudu** .
3. Klikněte na toku analýzy úlohu, kterou chcete změnit velikost.
4. Klikněte na **MĚŘÍTKU** v horní části stránky.

![Azure toku analýzy můžete vysílat datovými proudy měřítko jednotky][img.stream.analytics.streaming.units.scale]

Na portálu Azure nastavení můžete k nim získat přístup ve skupinovém rámečku nastavení:

![Azure portál toku technologie pro analýzu konfigurace][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Sledování výkonu projektu

Pomocí portálu pro správu, můžete sledovat výkon projektu v události/druhé:

![Azure toku analýzy sledování úloh][img.stream.analytics.monitor.job]

Výpočet očekávané výkon pracovní zátěž v události/druhé. Pokud výkon je menší než byste čekali, ladění vstupní oddíl, ladění dotaz a přidat další streamování jednotky práce.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Výkon analýzy toku ve velkém měřítku – scénář malin pí


Pokud chcete zjistit, jak měřítko toku analýzy úlohy ve scénáři typické z hlediska zpracování výkon přes více datových proudů jednotky, tady je pokus odešle data snímače (klienti) rozbočovači události, zpracuje ji a odešle upozornění nebo statistiky jako výstup jiné centrální události.

Klient odesílá data syntetizují snímače rozbočovače události ve formátu JSON toku technologie pro analýzu a výstupu dat je také ve formátu JSON.  Tady je jak vzorová data vypadat podobně –  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Dotaz: "odesílat upozornění při vypnuto light"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Měření výkon: Výkon v tomto kontextu se o velikost zadávání dat zpracovaných toku analýzy v pevné časový úsek (10 minut). K dosažení nejlepších výkon zpracování pro zadávání dat, obě data můžete vysílat datovými proudy zadávání a musí být rozdělen dotaz. Také **COUNT()**je součástí dotazu změřit, kolik vstupní události jsou zpracovány. Zajistit, že úlohy nečeká jednoduše pro zadávání události se chystají byl každý oddíl vstupní centrální události předinstalovaných s dostatečné zadávání dat (asi 300MB).  

Tady jsou výsledků pomocí funkce zvyšuje počet jednotek, datových proudů a odpovídající oddíl počtem v rozbočovače události.  

<table border="1">
<tr><th>Zadávání oddíly</th><th>Výstup oddíly</th><th>Streamování jednotky</th><th>Trvalý výkon
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity Azure toku analýzy](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
