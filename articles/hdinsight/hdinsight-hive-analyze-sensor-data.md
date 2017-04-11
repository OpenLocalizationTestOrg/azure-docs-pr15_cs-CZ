<properties
    pageTitle="Analýza dat senzor pomocí podregistru a Hadoop | Microsoft Azure"
    description="Zjistěte, jak analyzovat data snímače pomocí dotazu konzoly podregistru HDInsight (Hadoop) a potom vizualizace dat v aplikaci Microsoft Excel s PowerView."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Analýza senzor dat pomocí dotazu konzoly podregistru na Hadoop v HDInsight

Zjistěte, jak analyzovat data snímače pomocí dotazu konzoly podregistru HDInsight (Hadoop) a pak pomocí nástroje Power View vizualizace dat v aplikaci Microsoft Excel.

> [AZURE.NOTE] Postup v tomto dokumentu pouze se systémem Windows HDInsight clusterů fungovat.

V tomto příkladu použijete podregistru k obrázku historických dat vytvořené pomocí topení, ventilace a klimatizace (TVK) systémy k identifikaci systémů, které nejsou možné problémy se spolehlivým udržovat teplotu nastavení. Se dozvíte, jak:

- Vytvořte tabulku PODREGISTRU dotaz dat uložených v soubory hodnoty s oddělovači (CSV).
- Vytvoření dotazů PODREGISTRU k analýze dat.
- V aplikaci Microsoft Excel pro připojení k HDInsight (pomocí (ODBC) open database connectivity analyzovat data načíst.
- Vizualizace dat pomocí Power View.

![Příklad některých dalších funkcí architektury řešení](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>Zjistit předpoklady pro

* Shluk HDInsight (Hadoop): informace o vytváření clusteru najdete v tématu [poskytování Hadoop clusterů HDInsight](hdinsight-provision-clusters.md) .

* Microsoft Excel 2013

    > [AZURE.NOTE] Aplikace Microsoft Excel se používá pro vizualizaci dat s [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Ovladač ODBC podregistru společnosti Microsoft](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>Chcete-li spustit výběru

1. Z webového prohlížeče přejděte k následující adresu URL. Nahrazení `<clustername>` s názvem svůj cluster HDInsight.

        https://<clustername>.azurehdinsight.net

    Po zobrazení výzvy ověřovat pomocí správce uživatelské jméno a heslo, které jste použili při zřizování tomto obrázku.

2. Z webové stránky, která se otevře klikněte na kartu **Začínáme Začínáme Galerie** a klikněte ukázku **Senzor a analýza dat** v kategorii **řešení s ukázkovými daty** .

    ![Získání Začínáme Galerie](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Postupujte podle pokynů na stránce webových dokončete vzorku.
