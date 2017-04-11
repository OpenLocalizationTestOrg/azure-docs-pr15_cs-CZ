<properties 
    pageTitle="Přizpůsobení Hadoop clusterů procesu týmu dat pro výzkum | Microsoft Azure" 
    description="K dispozici ve vlastní clusterů Azure HDInsight Hadoop Oblíbené Python moduly."
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

# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Přizpůsobení Azure HDInsight Hadoop clusterů procesu týmu dat vědy 

Tento článek popisuje jak přizpůsobit HDInsight Hadoop obrázku díky instalaci Anaconda 64-bit (Python 2.7) v jednotlivých uzlech při clusteru máte k dispozici jako služba HDInsight. Také ukazuje, jak získat přístup k headnode posílat vlastní úkoly do clusteru. Toto přizpůsobení zajišťuje mnoho oblíbených moduly Python, které jsou součástí Anaconda snadno dostupné pro použití v funkce definované uživatelem (UDF), navržené zpracuje podregistru záznamy v clusteru. Další informace o postupech použitých v tomto scénáři zjistěte, [jak můžou odeslat podregistru dotazů](machine-learning-data-science-move-hive-tables.md#submit).

Nabídka následujících odkazů na témata, která popisují, jak nastavit různých prostředích pro výzkum dat používaných [Týmu dat pro výzkum obrázku (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]


## <a name="customize"></a>Přizpůsobení clusteru Hadoop Azure HDInsight

Pokud chcete vytvořit vlastní HDInsight Hadoop obrázku, uživatelé muset přihlásit ke [**Klasické portál Azure**](https://manage.windowsazure.com/), klikněte na tlačítko **Nový** v levém dolním rohu a potom vyberte datové služby -> HDINSIGHT -> **Vytvořit vlastní** vyvoláte oknem **Podrobné informace o obrázku** . 

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Při zadávání názvu clusteru vytvořit na stránce konfigurace 1 a přijměte výchozí hodnoty pro dalších polí. Klikněte na šipku přejdete na další stránku konfigurace. 

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Na stránce konfigurace 2 pro zadávání počtu **Uzlů dat**, vyberte **Oblast/virtuální sítě**a vyberte velikosti **Vedoucí uzel** a **Uzel DATA**. Klikněte na šipku přejdete na další stránku konfigurace.

>[AZURE.NOTE] **Oblast/virtuální sítě** musí být stejné jako oblast úložiště účet, který má dělat se dá použít pro clusteru HDInsight Hadoop. V opačném ve čtvrtém stránka konfigurace úložiště účet, který chcete použít uživatelů nezobrazí na rozevírací seznam **Název účtu**.

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Na stránce konfigurace 3 zadejte uživatelské jméno a heslo clusteru HDInsight Hadoop. **Co nedělat** stiskněte _Enter Metastore podregistru/Oozie_. Klikněte na šipku přejdete na další stránku konfigurace. 

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Na stránce konfigurace 4 zadejte název účtu úložiště kontejneru výchozí clusteru HDInsight Hadoop. Pokud uživatelé vyberou _vytvořit kontejner výchozí_ v **KONTEJNERU výchozího** rozevíracím seznamu, vytvoří se kontejneru se stejným názvem jako clusteru. Klikněte na tlačítko šipka pro návrat na poslední stránku konfigurace.

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Na stránce konfigurace konečný **Skript akce** na tlačítko **Přidat akci skriptu** a zaplnit textových polí následující hodnoty.
 
* **Název** - jakýkoli řetězec jako název tohoto skriptu akce. 
* **Typ uzlu** - výběr **všech uzlů**. 
* **Identifikátor URI skript** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *publicscripts* je kontejner veřejné účtu úložiště 
    * *getgoing* sloužící ke sdílení souborů skript Powershellu k usnadnění práce uživatelů v Azure. 
* **Parametry** - (ponechat prázdné)

Nakonec klikněte na značku zaškrtnutí k zahájení vytváření vlastního obrázku HDInsight Hadoop. 

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Přístup k vedoucí uzel Hadoop obrázku

Uživatelé musí povolit vzdálený přístup k obrázku Hadoop v Azure se před používáním hlavního uzlu clusteru Hadoop prostřednictvím RDP. 

1. Přihlášení do [**Klasického portál Azure**](https://manage.windowsazure.com/)vlevo vyberte **HDInsight** , vyberte svůj cluster Hadoop ze seznamu clusterů, klikněte na kartu **Konfigurace** a potom klikněte na ikonu **Povolení VZDÁLENÉHO** v dolní části stránky.
    
    ![Vytvoření pracovního prostoru](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)

2. V okně **Konfigurovat vzdálené plochy** zadejte do polí uživatelské jméno a heslo a vyberte datum vypršení platnosti vzdáleného přístupu. Klepněte na značku zaškrtnutí povolení vzdáleného přístupu k hlavního uzlu Hadoop obrázku.

    ![Vytvoření pracovního prostoru](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)
    
>[AZURE.NOTE] Uživatelské jméno a heslo pro vzdálený přístup nejsou uživatelské jméno a heslo, které používáte při vytvoření Hadoop obrázku. Toto jsou samostatnou sadu přihlašovacích údajů. Datum vypršení platnosti vzdálený přístup taky, musí být v rámci 7 dní od aktuálního data.

Po povolení vzdáleného přístupu klikněte na **Připojit** v dolní části stránky na vzdálený do hlavního uzlu. Přihlášení k hlavního uzlu Hadoop obrázku tak, že zadáte její přihlašovací údaje uživatele vzdáleného přístupu, kterou jste zadali dříve.

![Vytvoření pracovního prostoru](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

Další kroky procesu pokročilé technologie pro analýzu jsou namapované v [Týmu dat pro výzkum obrázku (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může zahrnovat kroky, které přesunutí dat do HDInsight, zpracovat a přehrajte si ho tam při přípravě na výukové z dat s Azure počítače učení.

Zjistěte, [jak můžou odeslat podregistru dotazů](machine-learning-data-science-move-hive-tables.md#submit) pokyny pro přístup k Python moduly, které jsou součástí Anaconda z hlavního uzlu obrázku na funkce definované uživatelem (UDF), které se používají zpracuje podregistru záznamy uložené v clusteru.

 
