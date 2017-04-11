<properties
   pageTitle="Výpočet kontextu možnosti R serveru na HDInsight (verze preview) | Microsoft Azure"
   description="Další informace o různých výpočetním kontextu možnosti dostupné pro uživatele se serverem R v HDInsight (verze preview)"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>Výpočet kontextu možnosti R serveru na HDInsight (verze preview)

Microsoft R Server na Azure HDInsight (verze preview) poskytuje nejnovější funkce na základě R analýzy. Použije data, která je uložena v HDFS v kontejneru v účtu úložiště [Objektů Blob Azure](../storage/storage-introduction.md "úložiště objektů Blob Azure") nebo místní systém souborů Linux. Protože R Server je založená na Otevřít zdroj R, můžete využít aplikace založené na R, kterou vytvoříte jakékoli balíčky otevřít zdroj R 8000 +. Můžete taky využít rutin v [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "ScaleR analýzy otočení"), společnosti Microsoft velký analýzy dokumentace, která je součástí serveru R.  

Postranní uzel clusteru Premium poskytuje praktické místo pro připojení k clusteru a spustit skripty R. S uzlu okraje máte možnost spuštěných na ScaleR paralelizován distribuované funkce přes jádra uzel edge serveru. Máte taky možnost spustit mezi uzly clusteru pomocí společnosti ScaleR Hadoop mapy zmenšení nebo Spark výpočet kontexty.

## <a name="compute-contexts-for-an-edge-node"></a>Výpočet kontexty pro edge uzel

Obecně R skript, který se spustí na serveru R na uzel okraj běží v rámci video interpreter R na uzel. Výjimkou jsou tyto kroky, které volají funkce ScaleR. Volání ScaleR spustit ve výpočetním prostředí, které je určený podle nastavení kontextu výpočetním ScaleR.  Při spuštění skript R z krajního uzlu možnými hodnotami výrazu kontextu výpočetním jsou místní sekvenčních ("místní"), místní paralelní (localpar) zmenšit mapy a Spark.

Možnosti místní"a"localpar"liší pouze jak spouštět rxExec volání Obě provést další funkce příjmu volání paralelní způsobem přes všechny dostupné jádra Pokud není zadáno jinak pomocí možnosti numCoresToUse ScaleR, například rxOptions(numCoresToUse=6). Následující shrnuje různé možnosti výpočetním kontextu

| Výpočet kontextu  | Jak nastavit                      | Kontext spuštění                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Místní sekvenční | rxSetComputeContext('local')    | Paralelizován provádění různých jádra uzel edge serveru s výjimkou rxExec hovory, které zpracují sériově |
| Místní současně   | rxSetComputeContext('localpar') | Paralelizován provádění různých jádra edge serveru uzel                                 |
| Spark            | RxSpark()                       | Paralelizován distribuované provádění prostřednictvím Spark mezi uzly HDI obrázku      |
| Zmenšení mapy       | RxHadoopMR()                    | Paralelizován distribuované provádění prostřednictvím snížit mapování mezi uzly HDI obrázku |


Za předpokladu, že byste chtěli paralelizován spuštění pro účely výkonu, jsou tři možnosti. Zvolená možnost závisí na druhu pracovních technologie pro analýzu a velikost a umístění dat v.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Pokyny pro rozhodování o výpočetním kontextu

V současné době není žádný vzorec, která sděluje, které výpočet kontextu používat. Existuje, ale některé zásady, které pomáhají ten správný nebo alespoň pomoci zúžit své volby před spuštěním průběhu testu. Zásady patří:

1.  Místní systém souborů Linux je rychlejší než HDFS.
2.  Pokud jsou data místní a pokud je v XDF se rychlejší opakované analýzy.
3.  Je vhodné stáhnete malých krocích dat ze zdroje dat textu; Pokud je větší množství dat, převeďte ho XDF před analýzy.
4.  Režijních kopírování nebo datových proudů data, která chcete okraj uzel pro analýzu stane nezvládnutelným pro velmi velké objemy dat.
5.  Spark je rychlejší než mapy snížit analýz v Hadoop.

Možnost tyto zásady, některé obecné zásady pro výběr kontextu výpočetním jsou:

### <a name="local"></a>Místní

- Pokud množství dat a analyzujte data je malý a nevyžaduje opakované analýzy, vysílat přímo do rutina analýzy a použít místní"nebo"localpar".
- Pokud množství dat a analyzujte data je malé a středně velké a vyžaduje opakované analýzy, potom ho zkopírujte do místního systému souborů, importujte ho XDF a analyzovat prostřednictvím místní"nebo"localpar".

### <a name="hadoop-spark"></a>Hadoop Spark

- Pokud výši data, která chcete analyzovat velký, pak je naimportujte do XDF v HDFS (Pokud úložiště není problém) a analyzovat prostřednictvím "Spark".

### <a name="hadoop-map-reduce"></a>Zmenšení Hadoop mapy

- Používejte pouze v případě, že setkáte nepřekonatelnou problém s volbou kontextu výpočetním Spark od obecně budou pomaleji.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Vložené informace o rxSetComputeContext

Další informace a příklady ScaleR výpočetním kontexty zobrazit textu nápovědy v R metodu rxSetComputeContext, například:

    > ?rxSetComputeContext

Můžete také odkazovat na "ScaleR Distributed Computing Průvodce", která je dostupná v knihovně [R serveru MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R serveru na webu MSDN") .


## <a name="next-steps"></a>Další kroky

V tomto článku se naučíte vytvoření nového clusteru Hdinsightu, který obsahuje R serveru. Základy používání konzole R z relaci SSH naučili. Teď si můžete přečíst v těchto článcích zjistit další způsoby, jak pracovat se serverem R na HDInsight:

- [Základní informace o serveru R pro Hadoop](hdinsight-hadoop-r-server-overview.md)
- [Začínáme s R serveru pro Hadoop](hdinsight-hadoop-r-server-get-started.md)
- [Přidání RStudio serveru HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)
- [Azure možnosti ukládání R serveru na HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
