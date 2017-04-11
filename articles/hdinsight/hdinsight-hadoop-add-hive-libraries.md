<properties
pageTitle="Přidání knihoven podregistru během vytváření clusteru HDInsight | Azure"
description="Naučte se přidávat podregistru knihoven (soubory sklenice) HDInsight clusteru během vytváření clusteru."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>Přidání knihoven podregistru během vytváření clusteru HDInsight

Pokud máte knihoven, které často používáte s podregistru na HDInsight, tento dokument obsahuje informace o použití akce skriptu pro předem načíst knihovny během vytváření clusteru. Díky knihoven globálně dostupné v podregistru (není potřeba umožňuje [Přidat JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) načíst je.)

##<a name="how-it-works"></a>Jak to funguje

Při vytváření clusteru, můžete volitelně určit skript akci, která běží skript uzlech clusteru, zatímco jsou vytvářeny. Skript v tomto dokumentu přijímá jeden parametr, což je WASB umístění, která obsahuje knihovnu (uložené jako soubory sklenice), které bude předem načtena.

Při vytváření clusteru skript zjistí počet souborů, zkopíruje, aby `/usr/lib/customhivelibs/` adresáře v záhlaví a pracovníka uzlech, potom je přidá do `hive.aux.jars.path` vlastnosti v `core-site.xml` soubor. Na základě Linux clusterů také aktualizuje `hive-env.sh` soubor s umístění souborů.

> [AZURE.NOTE] Pomocí skriptu akce v tomto článku zpřístupní knihovny v následujících situacích:
>
> * __Na základě Linux HDInsight__ - při použití __podregistru příkazového řádku__, __WebHCat__ (Templeton) a __HiveServer2__.
> * __HDInsight serveru s Windows__ – při použití __podregistru příkazového řádku__ a __WebHCat__ (Templeton).

##<a name="the-script"></a>Skript

__Umístění skriptu__

__Na základě Linux__clusterů: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

U __clusterů serveru s Windows__: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__Požadavky__

* Tyto skripty musí být použity pro __vedoucí uzly__ a __pracovní uzlů__.

* Sklenic po g, kterou chcete nainstalovat musí být uloženy v úložišti objektů Blob Azure v __jedné kontejner__. 

* Účet úložiště obsahující knihovna sklenice soubory __musí__ být propojené s clusteru HDInsight při vytváření. Můžete to provést jedním ze dvou způsobů:

    * Tím, že je v kontejneru na výchozí účet úložiště clusteru.
    
    * Tím, že je v kontejneru na kontejner propojené úložiště. Například v portálu můžete __Volitelná konfigurace__ __účtů úložiště propojené__ přidat další úložiště.

* Cesta WASB do kontejneru musí být zadán jako parametr akci skriptu. Například, pokud sklenic po g jsou uloženy v kontejneru s názvem __knihoven__ na úložiště účet s názvem __mystorage__, parametr by __wasbs://libs@mystorage.blob.core.windows.net/__.

    > [AZURE.NOTE] Tento dokument předpokládá, že jste již vytvořit účet úložiště, objektů blob kontejner a nahrát soubory. 
    >
    > Pokud jste nevytvořili účet úložiště, můžete to uděláte přes [Azure portálu](https://portal.azure.com). Nástroj například [Průzkumníka úložišť Azure](http://storageexplorer.com/) potom slouží k vytvoření nového kontejneru na účet a nahrajte soubory do ní.

##<a name="create-a-cluster-using-the-script"></a>Vytvoření clusteru pomocí skriptu

> [AZURE.NOTE] Následující postup vytvoření nového na základě Linux HDInsight clusteru. Vytvoření nového clusteru serveru s Windows, vyberte __okna__ jako obrázku s operačním systémem při vytváření clusteru a místo skript flám použijte skript systému Windows (Powershellu).
> 
> K vytvoření clusteru pomocí tohoto skriptu můžete taky použít Azure PowerShell a HDInsight .NET SDK. Další informace o použití těchto postupů najdete v článku [přizpůsobení HDInsight clusterů akcím skriptu](hdinsight-hadoop-customize-cluster-linux.md).

1. Spuštění zřizování clusteru pomocí kroků v [poskytování HDInsight clusterů na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), ale není dokončena zřizování.

2. Na zásuvné **Volitelná konfigurace** vyberte **Akce skriptu**a zadejte informace, jak je ukázáno v následujícím příkladu:

    * __Název__: Zadejte popisný název akci skriptu.
    * __Identifikátor URI skript__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __Hlavy__: Toto políčko zaškrtněte,
    * __Pracovní__: Toto políčko zaškrtněte.
    * __ZOOKEEPER__: nechte toto pole prázdné.
    * __Parametry__: Zadejte adresu WASB kontejner a úložiště účtu, který obsahuje sklenic po g. Například __wasbs://libs@mystorage.blob.core.windows.net/__.

3. V dolní části **Akce skriptu**pomocí tlačítka **Vybrat** konfiguraci uložte.

4. Na zásuvné **Volitelná konfigurace** vyberte __Propojené účty úložiště__ a vyberte odkaz __Přidat klíči úložiště__ . Vyberte účet úložiště, který obsahuje sklenic po g a potom pomocí tlačítek __Vyberte__ uložte nastavení a vraťte se zásuvné __Volitelná konfigurace__ .

5. Pomocí tlačítka **Vybrat** v dolní části zásuvné **Volitelná konfigurace** pro uložení informací o volitelná konfigurace.

6. Pokračujte v zřizování clusteru podle [ustanovení HDInsight clusterů na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

Po vytvoření clusteru hotové, bude moct používat sklenic po g přidat s použitím tohoto skriptu z podregistru bez nutnosti použití `ADD JAR` údajů.

##<a name="next-steps"></a>Další kroky

V tomto dokumentu se naučili přidat podregistru knihovny obsažené v souborech sklenice clusteru HDInsight během vytváření clusteru. Další informace o práci s podregistru najdete v článku [Použití podregistru s HDInsight](hdinsight-use-hive.md)
