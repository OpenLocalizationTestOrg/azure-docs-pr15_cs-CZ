<properties
    pageTitle="Co je R na HDInsight? Úvod k serveru R na HDInsight (verze preview) | Microsoft Azure"
    description="Co je R Server na HDInsight (verze preview) a jak používat R serveru pro vytváření aplikací pro analýzu dat velký."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>Základní informace o serveru R na HDInsight \(náhled\)

S Microsoft Azure HDInsight Premium serveru Microsoft R je k dispozici jako jednu z možností při vytváření HDInsight clusterů v Azure. Nová funkce poskytuje vědeckých dat, statistiků a R programátory okamžitý přístup k scalable, distributed metody analýzy na HDInsight.

Clusterů můžete velikostí nastavenou na projektech a úkolech po ruce a bylo odstraněno když jste už nepotřebujete. Protože jsou součástí Azure HDInsight těchto clusterů jsou součástí podpora 24/7 úrovni podniku, SLA z provozu 99,9 % a možnosti integrace s jiné součásti v Azure ekosystému.

>[AZURE.NOTE] R Server je dostupný jenom u HDInsight Premium. V současné době HDInsight Premium je dostupný jenom u Hadoop a Spark clusterů. Ano aktuálně můžete R serveru pouze pomocí Hadoop a Spark clusterů v HDInsight. Další informace najdete v tématu [Co je jiné službě úrovně a Hadoop součásti dostupné HDInsight?](hdinsight-component-versioning.md).

Server R místní HDInsight poskytuje nejnovější funkce na základě R analýzy na velkých sad dat, která je načtena k úložišti objektů Blob Azure. Protože R Server je založená na Otevřít zdroj R, můžete využít aplikace založené na R, kterou vytvoříte jednotlivých balíčků otevřít zdroj R 8000 +, jakož i rutin v ScaleR společnosti Microsoft velký analýzy dokumentace, která je součástí serveru R.

Okraje uzel Premium clusterů obsahuje praktické místo pro připojení k clusteru a spustit skripty R. S uzlu okraje máte možnost spuštění společnosti ScaleR paralelizován distribuované funkce přes jádra uzel edge serveru. Máte taky možnost spustit mezi uzly clusteru pomocí společnosti ScaleR Hadoop mapy zmenšení nebo Spark výpočet kontexty.

Modely nebo předpovědí stažené výsledky analýz mohou být pro použít místní. Budou můžete taky být operationalized jinde v Azure, jako je třeba prostřednictvím [Azure počítače výukové Studio](http://studio.azureml.net) [webové služby](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Začínáme s R na HDInsight

Zahrnout R serveru HDInsight clusteru, musíte vytvořit Hadoop nebo Spark obrázku s parametrem Premium při vytváření clusteru pomocí portálu Azure. Oba typy obrázku použít stejné konfiguraci, která obsahuje R Server na datových uzlů clusteru a postranní uzel jen cílová oblast pro R serverová analýzy. Přečtěte si článek [Začínáme se serverem R na HDInsight](hdinsight-hadoop-r-server-get-started.md) pro hloubkovou walk-through týkající se vytváření clusteru.

## <a name="learn-about-data-storage-options"></a>Další informace o možnosti ukládání dat.

Výchozí úložiště HDInsight clusterů je úložiště objektů Blob systém souborů HDFS namapované kontejneru objektů blob. Zajistíte, že jakéhokoliv data nahráli k základnímu úložišti clusteru, nebo aby došlo k zápisu úložiště clusteru v průběhu analýzy, je nastavená jako trvalý. Nástroj [AzCopy](../storage/storage-use-azcopy.md) slouží ke kopírování dat z objektů blob a.

Kromě použití úložiště objektů Blob, máte možnost [Jezera úložný prostor Azure](https://azure.microsoft.com/services/data-lake-store/) pomocí svůj cluster. Pokud používáte jezera dat, můžete pomocí úložiště objektů Blob a jezera dat pro HDFS úložiště.

Můžete také [Azure soubory](../storage/storage-how-to-use-files-linux.md) mezi možnostmi úložiště pro použití na uzel okraje. Azure soubory umožňuje připojit sdílený soubor, který byl vytvořený v úložišti Azure k systému souborů Linux. Další informace o možnosti ukládání dat pro R Server clusteru Hdinsightu najdete v článku [Možnosti ukládání R Server místní clusterů HDInsight](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>Server R přístup na clusteru

Po vytvoření clusteru serverem R můžete připojit ke konzole R na uzel okraj clusteru prostřednictvím SSH/nátěrové. Také můžete připojit pomocí prohlížeče Pokud zvolíte možnost instalace integrovaném vývojovém prostředí volitelné RStudio Server na uzel okraje. Další informace o instalaci RStudio serveru najdete v článku [Instalace RStudio serveru na clusterů HDInsight](hdinsight-hadoop-r-server-install-r-studio.md).   

## <a name="develop-and-run-r-scripts"></a>Můžete vyvíjet a spustit skripty R

R skripty vytváření a spouštění lze použít balíčků otevřít zdroj R 8000 + kromě rutiny paralelizován a distribuované v knihovně ScaleR. Obecně skript, který se spustí na serveru R na uzel okraj běží v rámci video interpreter R na uzel. Výjimky je, že tyto kroky, které volání ScaleR fungovat s místní výpočetním je to nastavit na stav Hadoop mapu zmenšení (RxHadoopMR) nebo Spark (RxSpark).

V těchto případech funkci spustí distribuované způsobem přes data (úkol) uzly clusteru, které jsou přidružené k odkazované data. Další informace o možnostech kontextu jiné výpočetním najdete v článku [Výpočet kontextu možnosti R serveru na HDInsight Premium](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Umožňují modelu

Po dokončení modelování dat můžete umožňují model a dosáhnout předpovědí pro nová data v Azure a místních i. Tento proces se nazývá bodování. Tady je několik příkladů.

### <a name="score-in-hdinsight"></a>Skóre v HDInsight

Získat v HDInsight napište R funkci, která volá svůj model a dosáhnout předpovědí pro nový datový soubor, který jste načíst ke svému účtu úložiště. Zpět k tomuto účtu úložiště uložte předpovědí. Běžné okamžitou možné spouštět na uzel okraj svůj cluster nebo pomocí plánované práce.  

### <a name="score-in-azure-machine-learning"></a>Skóre Azure počítač přečíst

Získat pomocí webové služby Azure počítače výukové použil funkci balíček Azure počítače výukové R otevřít zdroj jmenoval [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) publikovat modelu jako Azure webové služby. Pro usnadnění balíček je předinstalovaná na uzel okraje. Potom použít zařízení v počítači výukové k vytvoření uživatelského rozhraní webové služby a potom podle potřeby hodnocení volání webové služby.

Pokud vyberete tuto možnost, bude nutné převést všechny objekty modelu ScaleR objekty odpovídající otevřít zdroj modelu pomocí webové služby. Lze to provést pomocí funkce převodu ScaleR, jako například `as.randomForest()` na základě celek modelů.


### <a name="score-on-premises"></a>Skóre místní

Získat místních po vytvoření modelu serializovat modelu v R, stáhněte si ho, deserializovat ji a potom ji použijte pro bodování nová data. Nová data můžete dosáhnout pomocí přístup popsané výše v [bodování v HDInsight](#scoring-in-hdinsight) nebo pomocí [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Udržovat clusteru

### <a name="install-and-maintain-r-packages"></a>Instalace a udržovat balíčků R

Většinu balíčků R, které používáte požádáni na uzel okraj od Většina skriptů R spustí tam. Pokud chcete nainstalovat další balíčky R uzel okraje, můžete použít obvykle `install.packages()` metoda R.

Ve většině případů není potřeba nainstalovat další balíčky R uzlech dat používáte jenom rutin z knihovny ScaleR přes clusteru. Můžete však potřebovat dalších balíčků podporuje použití **rxExec** nebo spuštění **RxDataStep** uzlech data.

V těchto případech dalších balíčků je nutné zadat pomocí skriptu akce po vytvoření clusteru. Další informace najdete v tématu [Vytvoření obrázku HDInsight serverem R](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Změna nastavení Hadoop mapy zmenšení paměti

Obrázku můžete upravit tak, aby změna velikosti paměti R server běží snížit Mapa projektu. Chcete-li změnit clusteru, použijte uživatelské rozhraní Ambari Apache, která jsou dostupná přes Azure portálu zásuvné pro svůj cluster. Pokyny k tomu, jak získat přístup ke Ambari uživatelského rozhraní pro svůj cluster najdete v článku [Správa HDInsight clusterů pomocí rozhraní webových Ambari](hdinsight-hadoop-manage-ambari.md).

Také je možné změnit velikost paměti, která je dostupná R server pomocí Hadoop přepínačů ve volání **RxHadoopMR** :

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Změna velikosti svůj cluster

Existujícího clusteru je možné použít měřítko nahoru nebo dolů portálu. Roztažením, můžete získat další kapacitu, které můžete potřebovat pro větší zpracování úkolů nebo můžete změnit měřítko zpět clusteru nečinný. Další informace o tom, jak změnit velikost clusteru najdete v článku [Správa HDInsight clusterů](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Udržovat systém

Údržby proběhne v podkladovém VMs Linux HDInsight clusteru která chcete použít s operačním systémem opravy a další aktualizace. Údržby obvykle probíhá v 3:30:00 (na základě místní čas pro OM) každé pondělí a čtvrtek. Aktualizace provádí tak, že není ovlivnit víc než automatickém clusteru najednou.  

Vzhledem k tomu hlavy uzly jsou nadbytečné a jsou ovlivněny ne všechny uzly dat, může dojít ke zpomalení všechny úlohy spuštěné během tohoto období. By pořád spouštějí dokončování, ale. Vlastní software nebo místních dat, které máte se zachová přes tyto údržbu události Pokud katastrofické výpadku vyžadovaného opětovného vytvoření obrázku.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Zjistit, jaké možnosti integrovaném vývojovém prostředí pro R Server clusteru HDInsight

Uzel Linux okraj HDInsight Premium clusteru je cílová zóna založené na R analýzy. Po připojení k clusteru, můžete spustit rozhraní konzoly R server tak, že zadáte **R** příkazového řádku Linux. Použití rozhraní konzoly je rozšířeného Pokud jste dostali textovém editoru pro vývoj skriptů R v jiném okně a vyjmout a vložit oddíly skriptu do konzoly R podle potřeby.

Složitější nástroj pro vývoj skript R je na základě R integrovaném vývojovém prostředí pro použití na ploše, například společnosti Microsoft se nedávno ohlásí, [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS). Toto je skupina nástroje pro stolní počítače a servery z [RStudio](https://www.rstudio.com/products/rstudio-server/). Můžete taky použít Walware na základě zatmění [StatET](http://www.walware.de/goto/statet).

Další možností je instalace integrovaném vývojovém prostředí na samotný uzel okraj Linux.  S dotazem, jestli Oblíbené je [RStudio serveru](https://www.rstudio.com/products/rstudio-server/), který obsahuje integrovaném vývojovém prohlížečový prostředí pro použití vzdálené klienty. Instalace RStudio serveru na uzel okraj clusteru HDInsight Premium obsahuje celou prostředí integrovaném vývojovém prostředí pro vývoj a provádění skriptů R serverem R clusteru. Může být mnohem efektivnější než konzole R.  Pokud chcete použít RStudio serveru najdete v článku [Instalace serveru RStudio na clusterů HDInsight](hdinsight-hadoop-r-server-install-r-studio.md).

## <a name="learn-about-pricing"></a>Další informace o ceny

Poplatky, které jsou přidružené k obrázku HDInsight Premium serverem R jsou strukturovány podobně poplatkům za standardní clusterů HDInsight. Jsou založeny na pro změnu velikosti podkladového VMs přes jméno, dat a okraj uzly doplněný core hodinu zdvih Premium. Další informace o HDInsight Premium ceny, včetně cen během Public Preview a dostupnosti 30denní zdarma, najdete v článku [ceny HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Další kroky

Postupujte podle těchto odkazů na další informace o tom, jak pomocí serveru R clusterů HDInsight.

- [Začínáme se serverem R na HDInsight](hdinsight-hadoop-r-server-get-started.md)

- [Přidání RStudio serveru HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)

- [Výpočet kontextu možnosti R serveru na HDInsight (verze preview)](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure možnosti ukládání R serveru na HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
