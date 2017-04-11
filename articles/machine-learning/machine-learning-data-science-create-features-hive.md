<properties
    pageTitle="Vytvoření funkcí pro data v Hadoop clusteru pomocí dotazů podregistru | Microsoft Azure"
    description="Příklady podregistru dotazy, které mohou generovat funkcí v data uložená v Azure HDInsight Hadoop obrázku."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Vytvoření funkcí pro data v Hadoop clusteru pomocí podregistru dotazů

Tento dokument ukazuje, jak vytvořit funkcí pro data uložená v Azure HDInsight Hadoop clusteru pomocí podregistru dotazů. Tyto dotazy podregistru pomocí vložený podregistru uživatelsky definované funkce (UDF), skripty pro které jsou k dispozici.

Operace potřebné k vytvoření funkce může být velké množství paměti. Výkon podregistru dotazů stane kritickým více v těchto případech a vylepšit optimalizace určité parametry. Optimalizace těchto parametrů jsou popsány v části poslední.

Dotazy, které jsou uvedeny příklady specifické pro [Data ze služební cesty Taxi NYC](http://chriswhong.com/open-data/foil_nyc_taxi/) scénáře jsou také podle [Github úložiště](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Tyto dotazy už máte schéma dat zadali a je připraven k spuštění. V části poslední jsou rovněž popisované parametrů, které uživatelé vyladit tak, že můžete zlepšit výkonu podregistru dotazů.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Této **nabídce** odkazů na témata, která popisují, jak vytvořit funkcí pro data v různých prostředích. Tento úkol je kroku v [Týmu dat pro výzkum obrázku (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Zjistit předpoklady pro
V tomto článku se předpokládá, že máte:

* Vytvoření účtu Azure úložiště. Pokud potřebujete pokyny najdete v článku [Vytvoření účet Azure úložiště](../storage/storage-create-storage-account.md#create-a-storage-account)
* Zřízení přizpůsobený clusteru Hadoop ke službě HDInsight.  Pokud potřebujete pokyny, přečtěte si článek [Přizpůsobení Azure HDInsight Hadoop clusterů pokročilé technologie pro analýzu](machine-learning-data-science-customize-hadoop-cluster.md).
* Data je uložená na podregistru tabulky v Azure HDInsight Hadoop clusterů. Pokud ne, postupujte podle [Create a načítání dat do tabulek podregistru](machine-learning-data-science-move-hive-tables.md) odeslání dat do tabulky podregistru nejdřív.
* Povolit vzdálený přístup k clusteru. Pokud potřebujete pokyny najdete v článku [přístup vedoucí uzel Hadoop obrázku](machine-learning-data-science-customize-hadoop-cluster.md#headnode).


##<a name="hive-featureengineering"></a>Funkce generování

V této části jsou popsány několika příklady způsobů, ve kterém můžete být generování funkcí, pomocí podregistru dotazů. Po vytvoření dalších funkcí můžete přidat ve sloupcovém grafu do existující tabulky nebo vytvořit novou tabulku s charakteristikami a primární klíč, který lze připojit se původní tabulky. Zde jsou uvedeny příklady:

1. [Četnost na základě funkce generování](#hive-frequencyfeature)
2. [Rizika kategorií proměnných v binární klasifikace](#hive-riskfeature)
3. [Extrahování funkce z pole data a času](#hive-datefeatures)
4. [Extrahování funkcí v textovém poli](#hive-textfeatures)
5. [Výpočet vzdálenost mezi GPS souřadnic](#hive-gpsdistance)

###<a name="hive-frequencyfeature"></a>Četnost na základě funkce generování

Je často užitečné pro výpočet frekvencí úrovně proměnnou kategorií nebo frekvencí určitých kombinací úrovně z více kategorií proměnných. Uživatelé můžou použijte tento skript k výpočtu těchto frekvencí:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="hive-riskfeature"></a>Rizika kategorií proměnných v binární klasifikace

Binární klasifikace potřebujeme kategorií proměnné nečíselná převést číselné funkce když modelů, který se používá pouze číselné funkce. Důvodem je nahrazením jednotlivých úrovní nečíselná číselné rizika. V této části ukážeme některé obecné dotazy podregistru, která provádí výpočet hodnot rizika (pravděpodobnost protokolu) kategorií proměnné.


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

V tomto příkladu proměnné `smooth_param1` a `smooth_param2` jsou nastavené hladce rizika hodnoty vypočítá z data. Rizika získat oblast mezi – informace a informace. Rizika > 0 označuje, že je větší než 0,5 pravděpodobnost, že cíl roven 1.

Po riziko tabulky počítán, uživatelé můžete přiřadit rizika hodnoty tabulky spojením s tabulkou rizika. Spojování dotazu podregistru získali v předchozí části.

###<a name="hive-datefeatures"></a>Extrahování funkce data a času pomocí polí

Podregistru je součástí sady UDF pro zpracování pole data a času. V podregistru, je výchozí formát data a času "rrrr MM-dd 00:00:00" ("1970 01 01 12:21:32" například). V této části ukážeme příklady, kde extrahovat den měsíc, měsíce z pole data a času a další příklady, které převádět řetězec datum a čas ve formátu jiné než výchozí formát data a času řetězec ve výchozí formátování.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Tento dotaz podregistru předpokládá, že *& #60; pole data a času >* je v poli Výchozí formát data a času.

Pokud pole data a času nejsou výchozí formát, musíte nejdřív převést pole data a času na časové razítko Unix a následný převod Unix časové razítko na řetězec data a času, který je v poli Výchozí formát. Je-li data a času ve výchozí formát, uživatelé mohou použít vložená data a času UDF extrahovat funkce.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

V tomto dotazu Pokud *& #60; pole data a času >* má vzorek jako *03/26/2015 12:04:39*, *"& #60; vzorek pole data a času >"* by měl být `'MM/dd/yyyy HH:mm:ss'`. Chcete-li otestovat, uživatelé spustit

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

*Hivesampletable* v tomto dotazu předinstalovaného na všech clusterů Azure HDInsight Hadoop ve výchozím nastavení při zřizování clusterů.


###<a name="hive-textfeatures"></a>Extrahování funkce z textových polí

Textové pole, která obsahuje řetězec slova, která jsou oddělený mezerami po tabulku podregistru následující dotaz extrahuje délku řetězce a počet slov v řetězci.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="hive-gpsdistance"></a>Výpočet vzdálenosti mezi sady souřadnic GPS

Dotaz uvedených v této části můžete přímo použije Data NYC Taxi ze služební cesty. Má tento dotaz účel pro předvedení použití vložený matematických funkcí v podregistru generovat funkce.

Pole, která se používají v tomto dotazu je souřadnice GPS odběru a dropoff umístění s názvem *odběru\_délky*, *odběru\_šířky*, *dropoff\_délky*, a *dropoff\_šířky*. Dotazy, která provádí výpočet přímé vzdálenost mezi souřadnice odběru a dropoff jsou:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

Matematické rovnice, která provádí výpočet vzdálenost mezi dvěma souřadnice GPS najdete na webu <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Pohyblivé typ skripty</a> autorem SV Lapisu. V jeho Javascript, funkce `toRad()` je právě *lat_or_lon*pí/180*, které převede stupně na radiány. Tady *lat_or_lon * je šířky a délky. Od podregistru neposkytuje funkci `atan2`, ale poskytuje funkce `atan`, `atan2` funkce prováděn `atan` funkce ve výše uvedený dotaz podregistru pomocí definici podle <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">wikipedii</a>.

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Úplný seznam podregistru vložený funkce definované uživatelem najdete v části **Předdefinované funkce** na <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache podregistru wikiwebu</a>).  

## <a name="tuning"></a>Rozšířené témata: optimalizace podregistru parametry zlepšit rychlost dotazu

Výchozí nastavení parametrů podregistru clusteru nemusí být vhodné pro podregistru dotazů a data, která jsou zpracování dotazů. V této části probereme některé parametry, které uživatelé můžete optimalizovat výkon podregistru dotazů. Uživatelé se muset přidat optimalizací dotazy před dotazů zpracování dat parametrů.

1. **Místo haldy Java**: pro dotazy týkající se připojení velkých sad dat nebo zpracování dlouhých záznamech, **dost místa na disku haldy** je jedním ze běžné chyby. To může být správně zapnuli nastavením parametrů *mapreduce.map.java.opts* a *mapreduce.task.io.sort.mb* na požadované hodnoty. Tady je příklad:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    Tento parametr přidělí 4GB paměti Java haldy místa a taky díky řazení zlepšují účinnost přidělování paměti další jej. Je dobré přehrát s těmito rozdělením Pokud jsou všechny úlohy selhání chyb souvisejících s haldy místa.

2. **Velikost bloku distribuovaného systému souborů** : Tento parametr určuje nejmenší jednotky data, která jsou uložená v systému souborů. Jako příklad je-li distribuovaného systému souborů blok velikost 128MB, pak žádná data velikost menší než a až 128MB uložený ve jeden blok při data, která je větší než 128MB přidělený navíc bloky. Výběr velikosti příliš malý bloku způsobí, že velké režijních nákladů Hadoop od uzel název je zpracuje mnoho další žádosti o hledání relevantní blok vztahující se k souboru. Doporučené nastavení, když zabývají gigabajtů (nebo větší) jsou data:

        set dfs.block.size=128m;

3. **Optimalizace připojení operace v podregistru** : během operace spojení v rámci mapy a zmenšení obvykle proběhnout ve zmenšení fázi, někdy obrovské zisky můžete dosáhnout plánování spojení ve fázi mapy (neboli "mapjoins"). Pokud chcete směrovat podregistru můžete to udělat, pokud je to možné, můžete nastavení:

        set hive.auto.convert.join=true;

4. **Určení počtu mappers do podregistru** : během Hadoop umožňuje uživateli nastavte počet přechodky je počet mappers obvykle nastaven uživatelem. Vtip, který zajišťuje, že určitý stupeň ovládací prvek na toto číslo je zvolit proměnné Hadoop, *mapred.min.split.size* a *mapred.max.split.size* jako velikost každé mapování úkolů, je určený podle:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    Obvykle výchozí *mapred.min.split.size* hodnotu 0, který *mapred.max.split.size* **Long.MAX** a který *dfs.block.size* je 64 MB. Jako se zobrazují, zadaných dat velikost optimalizace tyto parametry "nastavením" je nám umožňují ladění počet mappers použít.

5. Několik dalších Další **Rozšířené možnosti** pro optimalizaci výkonu podregistru jsou uvedeny níže. Tyto umožňují nastavit paměť přiřazená mapování a zmenšení úkoly a může být užitečné v doladění výkonu. Upozorňujeme, že *mapreduce.reduce.memory.mb* nesmí být větší než velikost fyzické paměti jednotlivých pracovních uzlů v Hadoop obrázku.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
