<properties
    pageTitle="Publikování aplikací HDInsight | Microsoft Azure"
    description="Naučte se vytvářet a publikovat HDInsight aplikací."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Publikování aplikací HDInsight do Azure Marketplace

HDInsight aplikace je aplikace, která můžou uživatelé nainstalovat na základě Linux HDInsight obrázku. Tyto aplikace můžete vyvinutý společností Microsoft, nezávisle prodejci nebo za vás. V tomto článku se dozvíte, jak publikovat aplikace HDInsight do Azure Marketplace.  Obecné informace o publikování na webu Azure Marketplace najdete v tématu [publikování nabídka z Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

HDInsight aplikace používají *Přenést svůj vlastní licence (BYOL)* modelu poskytovatele aplikace je zodpovědný za Správa licencí aplikací pro koncové uživatele, kde koncoví uživatelé pouze účtovány podle Azure prostředky, které vytvářejí, například clusteru HDInsight a jeho VMs/uzlů. V současné době fakturace za samotnou aplikaci neprovádí prostřednictvím Azure.

Jiné aplikace HDInsight související článku:

- [Instalace HDInsight aplikace](hdinsight-apps-install-applications.md): Přečtěte si, jak nainstalovat aplikace HDInsight clusterů.
- [Instalace aplikací vlastní HDInsight](hdinsight-apps-install-custom-applications.md): Přečtěte si, jak nainstalovat a testovat vlastních aplikací HDInsight.

 
## <a name="prerequisites"></a>Zjistit předpoklady pro

Chcete-li odeslat vlastní aplikace na web marketplace, musíte mít vytvořili a testováno vlastní aplikace. Naleznete v následujících článcích:

- [Instalace aplikací vlastní HDInsight](hdinsight-apps-install-custom-applications.md): Přečtěte si, jak nainstalovat a testovat vlastních aplikací HDInsight.

Dále je třeba mít zaregistrovat účtu vývojář. V tématu [publikování nabídka z Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) a [vytvořit účet Microsoft pro vývojáře](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Definování aplikace

Zahrnuje dva kroky pro publikování aplikací pro Azure Marketplace.  Nejdřív definujete souboru **createUiDef.json** určujících, které clusterů aplikace je kompatibilní se službou; a pak publikovat šablony z portálu Microsoft Azure. Následující obrázek je ukázkový soubor createUiDef.json.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|Pole  | Popis   | Možné hodnoty|
|-------|---------------|----------------|
|typy  | Typy obrázku, které aplikace je kompatibilní se službou.    |Hadoop, HBase, bouře, Spark (nebo jejich kombinací)|
|úrovně  | Úrovně obrázku, které aplikace je kompatibilní se službou.    |Standardní, Premium (nebo obojí)|
|verze|  Typy HDInsight clusterů, které aplikace je kompatibilní se službou.    |3.4|

## <a name="package-application"></a>Balíček aplikace

Vytvoření souboru zip, která obsahuje všechny potřebné soubory při instalaci aplikace HDInsight. Budete potřebovat zip soubor v [aplikaci publikovat](#publish-application).

- [createUiDefinition.json](#define-application).
- mainTemplate.json. Zobrazíte ukázku na [instalovat vlastní aplikace HDInsight](hdinsight-apps-install-custom-applications.md).

    >[AZURE.IMPORTANT] Název názvy skript instalovat aplikace musí být jedinečný pro konkrétní obrázku ve formátu dole. Kromě žádné instalace a odinstalace akce skriptu by měl být idempotent, což znamená, skripty můžete volat repeatly při vytváření stejný výsledek.
    
    >   název":" [propojit ("odstín nainstalovat v0","-", uniquestring('applicationName')] "
        
    >Poznámka: tvoří tři části název skriptu:
        
    >   1. Skript předponu jména, která zahrnuje název aplikace nebo název týkající se aplikace.
    >   2. A "-" pro snazší čitelnost.
    >   3. Funkce jedinečné řetězec s názvem aplikace jako parametr.

    >   Příklad je výše končí stále: odstín nainstalovat v0-4wkahss55hlas v seznamu akce trvalých skriptu. Ukázka JSON data najdete v článku [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).

- Všechny požadované skriptů.

> [AZURE.NOTE] Soubory aplikace (včetně soubory webové aplikace, pokud je any) mohou být umístěny na libovolnou veřejně přístupný koncového bodu.

## <a name="publish-application"></a>Publikování aplikace

Postupujte podle následujících kroků můžete publikovat HDInsight aplikace:

1. Přihlaste se na [portál publikování Azure](https://publish.windowsazure.com/).
2. Klikněte na **řešení šablony** z levého k vytvoření nové šablony řešení.
3. Zadejte název a potom klikněte na **vytvořit novou šablonu řešení**.
3. Klikněte na **Centrum pro vývojáře vytvořit účet a připojovat se Azure programu** zaregistrovat vaší společnosti, pokud jste tak dosud neučinili.  V tématu [Vytvoření účtu Microsoft Developer](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Klikněte na **definovat některé topologií začít pracovat**. Šablona řešení je nadřazený "objekt" u všech jeho topologie. Více topologií můžete definovat v jedné šablony nabídky a řešení. Když nabídky se posune pracovní, se posune spolu s ostatními jeho topologií. 
4. Zadejte název topologie a potom klikněte na znaménko plus.
5. Zadejte novou verzi a potom klikněte na znaménko Plus.
6. Nahrajte soubor zip připravené na [balíček aplikace](#package-application).  
7. Klikněte na **požádat o certifikační**. Certifikace týmu služeb Microsoft zkontrolujte soubory a certifikace topologie.

## <a name="next-steps"></a>Další kroky

- [Instalace HDInsight aplikace](hdinsight-apps-install-applications.md): Přečtěte si, jak nainstalovat aplikace HDInsight clusterů.
- [Instalace aplikací vlastní HDInsight](hdinsight-apps-install-custom-applications.md): Naučte se nasadit aplikace zrušením publikované HDInsight HDInsight.
- [Na základě přizpůsobení Linux HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster-linux.md): Naučte se používat akci skriptu nainstalovat další aplikace.
- [Na základě vytvořit Linux Hadoop clusterů v používání šablon správce prostředků Azure HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Naučte se volání správce prostředků šablony k vytvoření clusterů HDInsight.
- [Použití prázdné okraj uzlů v HDInsight](hdinsight-apps-use-edge-node.md): Naučte se používat prázdné okraj uzel pro přístup k HDInsight obrázku, testování HDInsight aplikací a hostování HDInsight aplikace.

