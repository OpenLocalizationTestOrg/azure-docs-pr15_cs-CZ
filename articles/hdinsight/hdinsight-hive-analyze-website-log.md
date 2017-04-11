<properties 
    pageTitle="Pomocí podregistru Hadoop pro analýzu protokolu webu | Microsoft Azure" 
    description="Zjistěte, jak pomocí služby podregistru HDInsight a analyzujte data protokoly webu. Budete používat soubor protokolu předávat na vstupu do tabulky HDInsight a používejte HiveQL dotaz na data." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>Pomocí podregistru HDInsight analyzovat protokoly z webové stránky

Zjistěte, jak pomocí služby HiveQL HDInsight a analyzujte data protokoly z webu. Analýza protokolu webu mohou sloužit segmentech vaší cílovou skupinou podle podobných činností, zařadit do kategorií návštěvníci webu tak, že účastníky procesu a chcete zjistit obsah jejich zobrazení, weby, které pocházejí z a tak dál.

V tomto příkladu použijeme HDInsight obrázku a analyzujte data webu protokoly získat přehled o četnost návštěvy na webu z externích webů v den. Budete taky generovat Souhrn webu chyby, které uživatelé dojít. Se dozvíte, jak:

- Připojení k úložišti objektů Blob Azure, která obsahuje soubory protokolu webu.
- Vytvořte tabulku PODREGISTRU k vytvoření dotazu jsou protokoly.
- Vytvoření dotazů PODREGISTRU k analýze dat.
- V aplikaci Microsoft Excel pro připojení k HDInsight (pomocí (ODBC) open database connectivity analyzovat data načíst.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>Zjistit předpoklady pro

- Musí mít zřízení Hadoop clusteru v Azure HDInsight. Pokyny najdete v tématu [Poskytování HDInsight clusterů][hdinsight-provision]. 
- Pokud nemáte aplikaci Microsoft Excel 2013 nebo nainstalovaný Excel 2010.
- [Ovladač ODBC podregistru Microsoft](http://www.microsoft.com/download/details.aspx?id=40886) data z podregistru do Excelu mohli importovat, musíte mít.


##<a name="to-run-the-sample"></a>Chcete-li spustit výběru

1. Na [Portálu Azure](https://portal.azure.com/)z Startboard (Pokud připnuté tam obrázku) klikněte na dlaždici obrázku, na kterém chcete spustit vzorku.

2. Na zásuvné obrázku, klikněte v části **Odkazy**klikněte na **Řídicí panel obrázku**a na zásuvné **Clusteru řídicího panelu** klikněte na **Řídicí panel clusteru HDInsight**. Případně můžete přímo otevřít řídicího panelu pomocí následující adresu URL:

        https://<clustername>.azurehdinsight.net
    
    Po zobrazení výzvy ověřovat pomocí správce uživatelské jméno a heslo, které jste použili při zřizování clusteru.
  
2. Z webové stránky, která se otevře klikněte na kartu **Začínáme Začínáme Galerie** a pak kategorii **řešení s ukázkovými daty** , klikněte na **Web protokolu analyzovaného** vzorku.

3. Postupujte podle pokynů na stránce webových dokončete vzorku.

##<a name="next-steps"></a>Další kroky
Zkuste v následujícím příkladu: [Analýza dat senzor pomocí podregistru s HDInsight](hdinsight-hive-analyze-sensor-data.md).


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
