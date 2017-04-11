<properties
    pageTitle="Přehled dat pro výzkum pomocí Spark na Azure HDInsight | Microsoft Azure"
    description="Sada nástrojů Spark MLlib přináší značné počítače výukové modelování možnosti distribuované prostředí HDInsight."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Přehled dat pro výzkum pomocí Spark na Azure HDInsight

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Tato sada témat týkajících se ukazuje, jak používat HDInsight Spark k provádění běžných úloh vědy data například požití data, funkce inženýrské, modelování a model hodnocení. Data použitá jsou výběrem 2013 NYC taxi ze služební cesty a tarif datové sady. Modely integrované zahrnutím a spoje lineární regresní, náhodné struktury a přechodu zesílen stromy. Témata také ukazují, jak ukládat tyto modely v úložišti objektů blob Azure (WASB) a jak skóre a vyhodnotíte výkon prognostické. Další rozšířené témata se týkají jak mohou být modely školení pomocí křížového ověřování a hyper parametr sweeping. Toto téma Přehled také popisuje, jak nastavit Spark clusteru, budete muset postupujte podle pokynů v tři návody k dispozici. 

[Spark](http://spark.apache.org/) je otevřít zdroj paralelní zpracování systém, který podporuje zpracování v paměti zvýšit výkon analytických aplikací velký data. Modulu zpracování Spark vytvořené pro rychlé, snadné použití a pokročilých analýz. Možnosti distribuované výpočtu v paměti společnosti Spark usnadnit Dobrá volba, pokud iterativních algoritmů ve počítače učení s pomocí grafu výpočty. [MLlib](http://spark.apache.org/mllib/) je společnosti Spark scalable počítače výukové knihovny, která přiblíží možnosti modelování k této distribuované prostředí. 

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) je Azure hostovanou nabízení Spark otevřít zdroj. Obsahuje taky podpory pro **Poznámkové bloky Jupyter PySpark** Spark clusteru, která poběží interaktivní dotazy Spark SQL transformace, filtrování a detekční data uložená v objektů BLOB Azure (WASB). PySpark je Python rozhraní API pro Spark. Fragmenty kódu, které poskytují řešení a zobrazit relevantní pozemků vizualizace dat tady spustit v poznámkových blocích Jupyter nainstalovaným clusterů Spark. Kroky modelování v těchto tématech obsahovat kód, který ukazuje, jak proškolit, jsou vyhodnoceny, uložení a používání každého typu model. 

Kroky a podle tohoto návodu kód je určený pro HDInsight 3.4 Spark 1,6. Však kód tady a poznámkovým blokům je obecný a měli spolupracovat na libovolnou Spark obrázku. Pokud nepoužíváte HDInsight Spark, může být kroky nastavení a Správa clusteru mírně liší od co je zobrazena zde.

## <a name="prerequisites"></a>Zjistit předpoklady pro

1.v, musíte mít předplatné Azure. Pokud už nemáte jeden, najdete v článku [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. musíte HDInsight 3.4 Spark 1,6 clusteru k dokončení tohoto postupu. A vytvořte si ho, postupujte podle pokynů uvedených v [Začínáme: vytvoření Apache Spark na Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Typ obrázku a verze je uvedené z nabídky **Vyberte typ obrázku** . 


![](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [AZURE.NOTE] Téma, které ukazuje, jak používat Scala spíše než Python k dokončení úkolů v procesu vědy začátku do konce dat najdete v článku [vědy dat pomocí Scala s Spark na Azure](machine-learning-data-science-process-scala-walkthrough.md).

<!-- -->

>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="the-nyc-2013-taxi-data"></a>NYC 2013 Taxi dat

Data ze služební cesty Taxi NYC o 20 GB souborů komprimovaných hodnoty oddělené čárkami (CSV) (nekomprimované ~ 48 GB), zahrnující 173 milionů jednotlivé cest a tarify zaplacený při každé ze služební cesty. Každý záznam ze služební cesty obsahuje vybrat nahoru a odkládací umístění a čas, číslo licence anonymních napadení (ovladač) a číslo Medailon (společnosti taxi jedinečné id). Data za rok 2013 zahrnuje všechny cesty a pro každý měsíc není uvedený v následující dvě datové sady:

1. Jsou soubory CSV "trip_data" obsahují informace ze služební cesty, například číslo osob, zvedněte a dropoff bodů, dojít dobu trvání a délka ze služební cesty. Tady je pár ukázkových záznamů:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Soubory CSV "trip_fare" obsahují podrobnosti o tarif zaplacený při každé ze služební cesty, jako je typ platby, tarif částka, přirážky daně, tipy a mýtné a celkovou částku zaplacenou. Tady je pár ukázkových záznamů:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5


Jsme dostali 0,1 % vzorek tyto soubory a připojen cesta\_dat a ze služební cesty\_jízdenky CVS soubory do jedné datové sady pro účely tohoto návodu jako vstupní datovou sadu. Jedinečný klíč připojení ze služební cesty\_dat a ze služební cesty\_tarif se skládá z polí: medailonu, napadení\_licence a odběru\_data a času. Každý záznam datové obsahuje následující atributy představující výletu NYC Taxi:


|Pole| Stručný popis
|------|---------------------------------
| Medailon |Anonymních taxi Medailon (jedinečné taxi id)
| hack_license |    Anonymních číslo Hackney silniční licence
| vendor_id |   Id dodavatele taxi
| rate_code | Výnosnost taxi NYC tarif
| store_and_fwd_flag | Ukládání a dopředného příznaku
| pickup_datetime | Vyberte datum a čas
| dropoff_datetime | Dropoff datum a čas
| pickup_hour | Zvedněte hodinu
| pickup_week | Zvedněte týden v roce
| Funkce Weekday | Funkce Weekday (rozsahu 1 až 7)
| passenger_count | Číslo osob v výletu taxi
| trip_time_in_secs | Odezvy v sekundách
| trip_distance | Vzdálenost ze služební cesty cesty v míle
| pickup_longitude | Zvedněte délky
| pickup_latitude | Zvedněte šířky
| dropoff_longitude | Dropoff délky
| dropoff_latitude | Dropoff šířky
| direct_distance | Přímé vzdálenost mezi vyberte nahoru a dropoff umístění
| payment_type | Typ platby (čas, platební kartou atd.)
| fare_amount | Částka tarif v
| přirážky | Přirážky
| mta_tax | Daň MTA
| tip_amount | Tip: množství
| tolls_amount | Částka mýtné
| total_amount | Celková částka
| šikmý | Šikmý (0 a 1 Ne nebo Ano)
| tip_class | Tip: třídy (0: $0, 1: $0-5, 2: $6-10 3: 11 – 20, 4: > 20 USD)


## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Spuštění kódu z poznámkového bloku Jupyter clusteru Spark 

Můžete spustit Jupyter Poznámkový blok z portálu Microsoft Azure. Najděte svůj cluster Spark na řídicím panelu a klikněte na ni přejděte stránku správy pro svůj cluster. Otevře se Poznámkový blok přidružené Spark obrázku, klikněte na **Řídicí panely clusteru** -> **Jupyter Poznámkový blok** .

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

Taky můžete přecházet na ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** pro přístup k poznámkovým blokům Jupyter. Nahrazení části NÁZEV_CLUSTERU tuto adresu URL s názvem vlastního obrázku. Potřebujete heslo k vašemu účtu správce pro přístup k poznámkovým blokům.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Vyberte PySpark zobrazíte adresář, který obsahuje několik příkladů předem sbalené poznámkových bloků, které používají rozhraní API PySpark. Poznámkové bloky, které obsahují ukázky tento sadu témat Spark jsou k dispozici na [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)


Poznámkové bloky přímo z Github server Jupyter Poznámkový blok můžete nahrávat na svůj cluster Spark. Na domovské stránce vaší Jupyter klikněte na tlačítko **Nahrát** na pravé části obrazovky. Otevře se Průzkumníka souborů. Tady můžete vložte adresu URL Github (jako nezpracovaná obsah) poznámkového bloku a klikněte na **Otevřít**. Poznámkové bloky PySpark jsou k dispozici následující adresy URL:

1.  [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb)
2.  [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-model-consumption.ipynb)
3.  [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb)

Zobrazí název souboru v seznamu Jupyter soubor s tlačítkem **Odeslat** znovu. Kliknutím na toto tlačítko **Odeslat** . Teď jste importovali Poznámkový blok. Opakujte tento postup k nahrání další poznámkové bloky z tohoto návodu.

> [AZURE.TIP] Můžete klikněte pravým tlačítkem myši na odkazy v prohlížeči a vyberte **Kopírovat odkaz** na získání adresy URL github nezpracovanými obsahu. Vložte tuto adresu URL do Jupyter nahrát soubor explorer dialogového okna.

Nyní máte tyto možnosti:

- Po kliknutí na Poznámkový blok zobrazit kód.
- Provedení každou buňku stisknutím kombinace kláves **SHIFT + ENTER**.
- Spuštění celý poznámkový blok kliknutím na **buňku** -> **Spustit**.
- Použití automatické vizualizace dotazů.

> [AZURE.TIP] Jádra PySpark automaticky vizualizuje výstup dotazy SQL (HiveQL). Budete mít možnost vybrat z několika různých typů vizualizací (tabulky, výsečový, spojnicový, oblast nebo panel) pomocí tlačítek nabídka **Typ** v poznámkovém bloku:

![Logistickou regresní křivky ROC obecný přístupu](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Co je další krok?

Teď, když se nastavují s HDInsight Spark obrázku a nahráli Jupyter poznámkové bloky, jste připraveni s přípravou témata, která odpovídají tři PySpark poznámkové bloky. Znázorňují postup zkoumání dat a jak vytvářet a používat modely. Pokročilé průzkum a modelování Poznámkový blok ukazuje, jak chcete zahrnout křížového ověřování, hyper parametr komínů a model hodnocení. 

**Průzkum a modelování pomocí Spark:** Sadě dat prozkoumání a vytvářet skóre a zjistit počítače výukové modely projdete v tématu [Vytvoření binární klasifikaci a regresní modely dat pomocí nástrojů Spark MLlib](machine-learning-data-science-spark-data-exploration-modeling.md) .

**Model spotřebu:** Informace o skóre klasifikaci a regresní modelů vytvořených v tomto tématu najdete v tématu [skóre a vyhodnotíte integrované Spark počítače výukové modely](machine-learning-data-science-spark-model-consumption.md).

**Křížového ověřování a hyperparameter komínů**: najdete v článku [Pokročilé průzkum a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak můžete být modely školení pomocí křížového ověřování a hyper parametr sweeping

