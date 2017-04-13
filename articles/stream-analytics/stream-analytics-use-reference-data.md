<properties
    pageTitle="Použití referenční dat a vyhledávací tabulky v toku analýzy | Microsoft Azure"
    description="Použití referenčních dat v dotazu toku analýzy"
    keywords="Vyhledávací tabulka v relaci referenčních dat"
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

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Použití referenční dat a vyhledávací tabulky v zadávání toku toku analýzy

Referenčních dat (označovaná taky jako vyhledávací tabulka) je omezené dat, která je statická nebo zpomaluje změny v přírodě, který připojuje vyhledávání nebo sladit se datového proudu. Používat referenčních dat v Azure toku analýzy práce se obvykle používají [Odkaz připojení dat](https://msdn.microsoft.com/library/azure/dn949258.aspx) v dotazu. Toku analýzy používá úložiště objektů Blob Azure jako vrstvy úložiště pro referenčních dat a s odkazem Azure Data Factory data transformovat nebo zkopírovali k úložišti objektů Blob Azure sloužící jako odkaz Data z [na základě a místní úložiště dat libovolný počet cloudu](../data-factory/data-factory-data-movement-activities.md). Přehled dat je modelovat jako posloupnost objektů BLOB (definované v zadávání konfiguraci) ve vzestupném pořadí podle názvu objektů blob datum a čas. Ho podporuje **pouze** přidávání na konec pořadí pomocí datum a čas **větší** než určenému poslední objektů blob v pořadí.

## <a name="configuring-reference-data"></a>Konfigurace referenčních dat

Abyste mohli nakonfigurovat referenčních dat, musíte nejdřív vytvořit vstup, která je typu **Referenčních dat**. Následující tabulka uvádí každou vlastnost, která je třeba zadat při vytváření referenčních dat při zadávání s její popis:

<table>
<tbody>
<tr>
<td>Název vlastnosti</td>
<td>Popis</td>
</tr>
<tr>
<td>Zadávání Alias</td>
<td>Popisný název, který se použije v dotazu úlohy neodkazuje tyto vstupní.</td>
</tr>
<tr>
<td>Úložiště účtu</td>
<td>Název účtu úložiště, kde se nacházejí objektů blob soubory. Pokud je ve stejném předplatném jako práce analýzy toku, můžete ho vybrat z rozevírací dolů.</td>
</tr>
<tr>
<td>Klíč účtu úložiště</td>
<td>Řeknu klávesu přidružené k účtu úložiště. Zadá dostane automaticky je-li úložiště účet v rámci stejného předplatného jako práce toku analýzy.</td>
</tr>
<tr>
<td>Kontejner úložiště</td>
<td>Kontejnery obsahují logické seskupení pro objekty BLOB uložené ve službě objektů Blob Microsoft Azure. Když nahrajete objektů blob ke službě objektů Blob, je nutné zadat kontejner pro tento objekt.</td>
</tr>
<tr>
<td>Cesta vzor</td>
<td>Cesta k souboru používaných k vyhledání vaší objektů BLOB v zadaném kontejneru. V rámci cesty můžete určit jeden nebo více instance následující 2 proměnné:<BR>{date}, {čas}<BR>Příklad 1: products/{date}/{time}/product-list.csv<BR>Příklad 2: products/{date}/product-list.csv
</tr>
<tr>
<td>Formát Datum [volitelné]</td>
<td>Pokud jste použili {date} uvnitř vzoru cestu, kterou jste zadali, můžete vybrat formát data, ve kterém jsou uspořádány souborů z rozevírací nabídky podporované formáty. Příklad: Rrrr/MM/DD</td>
</tr>
<tr>
<td>Formát času [volitelné]</td>
<td>Pokud jste použili {čas} uvnitř vzoru cestu, kterou jste zadali, můžete vybrat formát času, ve kterém jsou uspořádány souborů z rozevírací nabídky podporované formáty. Příklad: [HH</td>
</tr>
<tr>
<td>Formát serializace události</td>
<td>Aby zkontrolovala, jestli že svoje dotazy práce podle vašich představ, analýzy toku je potřeba vědět, který formát serializace používáte pro příchozí datových proudů. Přehled dat jsou podporovány následující formáty CSV a JSON.</td>
</tr>
<tr>
<td>Kódování</td>
<td>Jediný podporovaný formát kódování UTF-8 je v současné době</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Generování dat odkaz na plánu

Pokud referenčních dat je pomalu změna sady dat, potom podpory k aktualizaci odkaz, který data aktivované zadáním vzorek cesty v zadávání konfiguraci pomocí {date} a {času} nahrazení tokeny. Definice aktualizované odkaz dat podle tato cesta vzor vyzvedne toku analýzy. Například vzorec `sample/{date}/{time}/products.csv` ve formátu data **"rrrr-MM-DD"** a časem formát **"Hh"** pokyn toku analýzy na vystopovat aktualizované objektů blob `sample/2015-04-16/17:30/products.csv` na 5:30 odp na duben 2015 16 UTC časové pásmo.

> [AZURE.NOTE] Aktuálně toku analýzy úlohy vyhledejte aktualizace objektů blob pouze v případě, že čas počítače přejde čas zakódovaný v názvu objektů blob. Příklad úlohy zkontroluje `sample/2015-04-16/17:30/products.csv` hned po nejnižší, ale ne dříve než 5:30 odp na duben 2015 16 UTC Čas zóny. Bude *nikdy* vyhledejte soubor s starší než poslední ten, který je zjištěno zakódovaný čas.
> 
> Například Jakmile úlohy nalezne objektů blob `sample/2015-04-16/17:30/products.csv` bude ignorovat všechny soubory s kódováním data starší než 5:30 odp 16 duben 2015 Pokud zpožděné příchozí `sample/2015-04-16/17:25/products.csv` vytvořena objektů blob ve stejném kontejneru úlohy nebude používat.
> 
> Podobně pokud `sample/2015-04-16/17:30/products.csv` je vyrobeno pouze na 10 hh duben 2015 16 bez objektů blob s pozdějším datem je k dispozici v kontejneru, ale úlohy použije tento soubor počínaje 10 hh duben 2015 16 a použijte předchozí referenčních dat do té doby.
> 
> Výjimkou tohoto pravidla je projektu potřebujete-li znovu zpracovat data zpět v čase nebo při prvním úlohy Začínáme. Při spuštění, které úkoly hledáte posledních objektů blob vyrobeno před úlohy spuštění zadali. Důvodem je zajistěte, aby k sadám dat odkaz **Neprázdné** při spuštění úlohy. Pokud jedna nebyla nalezena, úkoly se zobrazí následující diagnostic: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) mohou sloužit k organizovat úkolu vytváření aktualizované objektů BLOB vyžadované toku analýzy aktualizovat definice dat odkaz. Data Factory je služba integrace cloudové data, která orchestrates a automaticky pohybu a transformace dat. Data Factory podporuje [připojování k velkého počtu cloudový a místní úložiště dat](../data-factory/data-factory-data-movement-activities.md) a přesunutí dat snadno v pravidelných intervalech, který určíte. Další informace a podrobné pokyny k nastavení kanálu Data Factory generování referenčních dat pro analýzy toku, který aktualizuje podle předdefinovaných plánu podívejte se na tato [Ukázka GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tipy pro aktualizaci dat odkaz ##

1. Přepsání objektů BLOB dat odkaz nezpůsobí toku analýzy načtěte znovu objektů blob a v některých případech může způsobit úloha nezdaří. Doporučené postupy změnit referenčních dat je přidání nových objektů blob pomocí stejného kontejneru a cesta vzor podle úlohu vstupní použití kalendářních dat/časů **větší** než určenému poslední objektů blob v pořadí.
2.  Přehled dat, jakým objektů BLOB nejsou seřazené podle času objektů blob "Naposledy změněno", ale jenom čas a datum zadané v objektů blob pojmenovat pomocí {date} a {času} náhrady.
3.  Na několik příležitosti, které úlohy musíte přejít zpět v čase proto objektů BLOB dat odkaz nesmí změnit nebo odstraněn.

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
Jste byla zavedená do analýzy toku, služba spravovaných datového proudu technologie pro analýzu dat z Internetu věci. Další informace o této službě najdete v tématu:

- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
