<properties
    pageTitle="Dotaz příklady pro běžné vzorce použití v toku analýzy | Microsoft Azure"
    description="Běžné vzorce dotazu analýzy Azure toku "
    keywords="Příklady dotazu"
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
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Příklady pro běžné vzorce toku analýzy využití dotazů

## <a name="introduction"></a>Úvod

Dotazy v Azure toku analýzy jsou vyjádřeny v jazyce SQL profesionálové dotazu, který je popsána v [Toku analýzy dotazu jazyka referenční](https://msdn.microsoft.com/library/azure/dn834998.aspx) příručka.  Tento článek shrnuje řešení několik běžných vzorků dotaz založený na reálné situace.  Je probíhající práci a mají být aktualizovány nové vzorky průběžné, zůstanou.

## <a name="query-example-data-type-conversions"></a>Příklad dotazu: datového typu Převod
**Popis**: definovat typy vlastností na vstupní proudu.
Například auta weight (váha) přichází na vstupní proudu jako řetězce a potřeb má být převeden na INT provádět SEČÍST ho.

**Zadání vstupních hodnot**:

| Zkontrolujte | Čas | Weight (váha) |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | "1000" |
| Honda | 2015-01-01T00:00:02.0000000Z | "2000" |

**Výstup**:

| Zkontrolujte | Weight (váha) |
| --- | --- |
| Honda | 3000 |

**Řešení**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Vysvětlení**: umožňuje výkaz CAST polem weight (váha) zadejte jeho typ (najdete v seznamu členů podporované datové typy [sem](https://msdn.microsoft.com/library/azure/dn835065.aspx)).


## <a name="query-example-using-likenot-like-to-do-pattern-matching"></a>Příklad dotazu: pomocí profesionálové/není třeba vzorek odpovídající
**Popis**: Zkontrolujte, jestli hodnota pole na událost odpovídá určitému vzorku například zpáteční štítků licencí, které začínají písmenem A a končí 9

**Zadání vstupních hodnot**:

| Zkontrolujte | LicensePlate | Čas |
| --- | --- | --- |
| Honda | ABC 123 | 2015-01-01T00:00:01.0000000Z |
| Toyota | AAA 999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC 369 | 2015-01-01T00:00:03.0000000Z |

**Výstup**:

| Zkontrolujte | LicensePlate | Čas |
| --- | --- | --- |
| Toyota | AAA 999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC 369 | 2015-01-01T00:00:03.0000000Z |

**Řešení**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Vysvětlení**: Zkontrolujte, jestli hodnota pole LicensePlate začíná A pak obsahuje jakýkoli řetězec žádný nebo více znaků, a končí 9 pomocí příkazu LIKE. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Příklad dotazu: Zadejte logiky pro různé případech/hodnoty (případu příkazy)
**Popis**: Zadejte jiný výpočet pro pole na základě některých kritérií.
Například zadejte popis řetězec kolik automobilů předaná stejné značky s zvláštní případy pro 1.

**Zadání vstupních hodnot**:

| Zkontrolujte | Čas |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Výstup**:

| CarsPassed | Čas |
| --- | --- | --- |
| 1 Honda | 2015-01-01T00:00:10.0000000Z |
| 2 Toyotas | 2015-01-01T00:00:10.0000000Z |

**Řešení**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Vysvětlení**: případě klauzule nám umožňují nabídnout různé výpočtu na základě některých kritérií (v našem případě počet automobilů v okně agregační).

## <a name="query-example-send-data-to-multiple-outputs"></a>Příklad dotazu: odeslání dat do několika výstupy
**Popis**: odeslání dat do několika výstupních cílů z jednoho projektu.
Například analýze dat pro upozornění na základě mezní hodnota a archivace všechny události k úložišti objektů blob

**Zadání vstupních hodnot**:

| Zkontrolujte | Čas |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output1**:

| Zkontrolujte | Čas |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output2**:

| Zkontrolujte | Čas | Počet |
| --- | --- | --- |
| Toyota | 2015-01-01T00:00:10.0000000Z | 3 |

**Řešení**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Vysvětlení**: do klauzule říká toku analýzy které výstupy k zápisu dat tohoto příkazu.
První dotaz je předávací dat, které jsme dostali do výstup, aby pojmenovali jsme ho ArchiveOutput.
Druhý dotaz má několik jednoduchých agregace filtrování a odešle výsledky podřízené výstražných systému.
*Poznámka*: můžete také znovu použít výsledky CTEs (tedy s příkazy) do několika výstupních příkazů – tato možnost má tu výhodu otevření méně čtenářům vstupní zdroj.
Například 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-counting-unique-values"></a>Příklad dotazu: počítání jedinečné hodnoty
**Popis**: určení počtu jedinečných hodnot pole, které se zobrazí v toku v rámci časového intervalu.
Například počet jedinečných uskutečnění automobilů zpráva předána stánku placená do 2 druhého okna?

**Zadání vstupních hodnot**:

| Zkontrolujte | Čas |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Výstup:**

| Počet | Čas |
| --- | --- |
| 2 | 2015-01-01T00:00:02.000Z |
| 1 | 2015-01-01T00:00:04.000Z |

**Řešení:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)


**Vysvětlení:** V takovém počáteční agregace získat jedinečný něco v ní s jejich počet přes okna.
Proveďte jsme agregaci kolik něco v ní máte – jsme dostali všechny jedinečné hodnoty v okně získat stejný časové razítko a pak druhé okno agregace musí být minimální není agregovat 2 windows z cílem prvního kroku.

## <a name="query-example-determine-if-a-value-has-changed"></a>Příklad dotazu: zjištění, pokud hodnota změnila#
**Popis**: Podívejte se na předchozí hodnotu, která určí, pokud se liší od aktuální hodnota je například předchozí auta na cestách placená stejné vytvářet aktuální Auto?

**Zadání vstupních hodnot**:

| Zkontrolujte | Čas |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Výstup**:

| Zkontrolujte | Čas |
| --- | --- |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Řešení**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Vysvětlení**: použití prodlevy k prohlížení na vstup vysílat datovými proudy jednu událost zpět a zjištění hodnoty nastavení. Potom ho porovná vytvářecí o aktuální události a vytvořit událost, pokud se liší.

## <a name="query-example-find-first-event-in-a-window"></a>Příklad dotazu: Najít první událost v okně
**Popis**: Najít první vozidla v každé 10 minut?

**Zadání vstupních hodnot**:

| LicensePlate | Zkontrolujte | Čas |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Výstup**:

| LicensePlate | Zkontrolujte | Čas |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |

**Řešení**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Teď provedeme změnit problému a najít první auto zejména v každé 10 minut.

| LicensePlate | Zkontrolujte | Čas |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Řešení**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-last-event-in-a-window"></a>Příklad dotazu: poslední akce najít v okně
**Popis**: hledání poslední auta v každé 10 minut.

**Zadání vstupních hodnot**:

| LicensePlate | Zkontrolujte | Čas |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Výstup**:

| LicensePlate | Zkontrolujte | Čas |
| --- | --- | --- |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Řešení**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Vysvětlení**: v dotazu sestává ze dvou kroků – první najde nejnovější časové razítko ve windows 10 minut. Druhým krokem spojí výsledků prvního dotazu s původním toku najít odpovídající poslední časová razítka v každém okně události. 

## <a name="query-example-detect-the-absence-of-events"></a>Příklad dotazu: zjišťování absence události
**Popis**: Ověřte, zda proudu žádná hodnota, která odpovídá určité kritérium.
Příklad 2 po sobě jdoucí automobily od stejné značky zadali řízením placená 90 vyvolané?

**Zadání vstupních hodnot**:

| Zkontrolujte | LicensePlate | Čas |
| --- | --- | --- |
| Honda | ABC 123 | 2015-01-01T00:00:01.0000000Z |
| Honda | AAA 999 | 2015-01-01T00:00:02.0000000Z |
| Toyota | DEFINICE 987 | 2015-01-01T00:00:03.0000000Z |
| Honda | GHI 345 | 2015-01-01T00:00:04.0000000Z |

**Výstup**:

| Zkontrolujte | Čas | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda | 2015-01-01T00:00:02.0000000Z | AAA 999 | ABC 123 | 2015-01-01T00:00:01.0000000Z |

**Řešení**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Vysvětlení**: použití prodlevy k prohlížení na vstup vysílat datovými proudy jednu událost zpět a zjištění hodnoty nastavení. Potom ho porovná vytvářecí o aktuální události a výstup událost, pokud jsou stejná a k získání dat o předchozí auta použít prodlevy.

## <a name="query-example-detect-duration-between-events"></a>Příklad dotazu: zjistit dobu trvání mezi události
**Popis**: hledání trvání dané události. Vzhledem k tomu webu clickstream zjistěte času stráveného funkci.

**Zadání vstupních hodnot**:  
  
| Uživatel | Funkce | Události | Čas |
| --- | --- | --- | --- |
| user@location.com | RightMenu | Zahájení | 2015-01-01T00:00:01.0000000Z |
| user@location.com | RightMenu | Ukončení | 2015-01-01T00:00:08.0000000Z |
  
**Výstup**:  
  
| Uživatel | Funkce | Doba trvání |
| --- | --- | --- |
| user@location.com | RightMenu | 7 |
  

**Řešení**

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Vysvětlení**: použití poslední funkce načíst poslední hodnoty času při typ události, "Start. Všimněte si, že poslední funkce používá oddíl uživatelem [] vyznačení, že výsledek počítá jedinečné uživatele.  Dotaz obsahuje maximální prahovou hodnotu 1 hodina pro časový rozdíl mezi "Zahájení" a "ukončení události, ale je možné nakonfigurovat jako potřebné (LIMIT DURATION(hour, 1).

## <a name="query-example-detect-duration-of-a-condition"></a>Příklad dotazu: zjistit dobu trvání podmínku
**Popis**: zjištění doby pro došlo k podmínky.
Předpokládejme například, který chybu výsledkem všechny automobily mají nesprávné hmotnost (nad 20 000 libry) – chceme vypočítat trvání chyby.

**Zadání vstupních hodnot**:

| Zkontrolujte | Čas | Weight (váha) |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | 2000 |
| Toyota | 2015-01-01T00:00:02.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:03.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:04.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:05.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:06.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:07.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:08.0000000Z | 2000 |

**Výstup**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z | 2015-01-01T00:00:07.000Z |

**Řešení**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Vysvětlení**: použití prodlevy k zobrazení vstupních proudu 24 hodin a vyhledejte případy, kdy StartFault a StopFault jsou určené weight < 20000.

## <a name="query-example-fill-missing-values"></a>Příklad dotazu: vyplnění chybějících hodnot
**Popis**: toku události, které mají chybějících hodnot plodiny toku událostí pomocí pravidelných intervalech.
Například vytvořit událost každých 5 sekund, které ohlásí naposledy příručky datový bod.

**Zadání vstupních hodnot**:

| t | Hodnota |
|--------------------------|-------|
| "2014-01-01T06:01:00" | 1 |
| "2014-01-01T06:01:05" | 2 |
| "2014-01-01T06:01:10" | 3 |
| "2014-01-01T06:01:15" | 4 |
| "2014-01-01T06:01:30" | 5 |
| "2014-01-01T06:01:35" | 6 |

**Výstup (prvních 10 řádků)**:

| windowend | lastevent.t | lastevent.Value |
|--------------------------|--------------------------|--------|
| 2014-01-01T14:01:00.000Z | 2014-01-01T14:01:00.000Z | 1 |
| 2014-01-01T14:01:05.000Z | 2014-01-01T14:01:05.000Z | 2 |
| 2014-01-01T14:01:10.000Z | 2014-01-01T14:01:10.000Z | 3 |
| 2014-01-01T14:01:15.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:20.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:25.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:30.000Z | 2014-01-01T14:01:30.000Z | 5 |
| 2014-01-01T14:01:35.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:40.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:45.000Z | 2014-01-01T14:01:35.000Z | 6 |

    
**Řešení**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Vysvětlení**: Tento dotaz bude vytvářet události každé 5 druhé a bude výstupní poslední události, která byla přijata před. [Přepínání okna] Doba trvání (https://msdn.microsoft.com/library/dn835041.aspx "Vše okno – Azure toku analýzy") Určuje, jak daleko zpět dotaz bude vypadat najít nejnovější události (300 sekund v tomto příkladu).


## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
