<properties
    pageTitle="Použití prázdné okraj uzlů v HDInsight | Microsoft Azure"
    description="Jak si přidat uzel okraj ampty HDInsight clusteru, která mohou sloužit jako klienta a na test/hostitele aplikace HDInsight."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Použití prázdné okraj uzlů v HDInsight

Naučte se přidávat uzel prázdné okraj na základě Linux HDInsight obrázku. Prázdný okraj uzel je Linux virtuálního počítače s stejné klientských nástrojích nainstalovali a nakonfigurovali jako headnodes, ale žádné hadoop služby spuštěný. Uzel hrany můžete použít pro přístup k clusteru, testování klientské aplikace a který je hostitelem vaší klientské aplikace. 

Prázdný krajního uzlu můžete přidat do existující clusteru HDInsight do nového clusteru při vytváření clusteru. Přidání prázdné krajního uzlu probíhá pomocí Správce prostředků Azure šablony.  Následující příklad znázorňuje, jak se to dělá pomocí šablony:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Jak je vidět ve výběru, můžete volat volitelně [skript akce](hdinsight-hadoop-customize-cluster-linux.md) můžete provádět další konfiguraci, třeba instalace [Apache odstín](hdinsight-hadoop-hue-linux.md) v uzel okraje.

Po vytvoření uzel okraje, můžete připojení k uzel okraj pomocí SSH a spusťte klientských nástrojích přístup k obrázku Hadoop v HDInsight.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Přidejte okraj uzel do existujícího clusteru

V této části pomocí šablony správce prostředků přidejte okraj uzel do existujícího clusteru HDInsight.  Správce prostředků šablon najdete v [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). Správce prostředků šablony hovorů skript akce skript c:\ https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Skript nemá provádět žádné akce.  Toto je ukazují, volající akci skriptu ze šablony správce prostředků.

**Chcete-li přidat prázdné krajního uzlu do existujícího clusteru**

1. Pokud ještě nemáte vytvoření clusteru HDInsight.  Viz [Hadoop kurz: Začínáme s používáním na základě Linux Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klikněte na následujícím obrázku se přihlásit k Azure a otevření šablony Azure správce na portálu Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigurace následující vlastnosti:

    - NÁZEV_CLUSTERU: Zadejte název existujícího clusteru HDInsight.
    - EDGENODESIZE: Vyberte jednu z OM velikosti.
    - EDGENODEPREFIX: Výchozí hodnota je **Nová**.  Bude použita výchozí hodnota, okraje uzel je v poli Název **nové edgenode**.  Můžete přizpůsobit předponu z portálu. Můžete také upravit celé jméno ze šablony.


4. Kliknutím na **OK** uložte změny.
5. **Pole Skupina zdroje**vyberte požadovanou skupinu zdroje.
6. Klikněte na **Revize právní podmínky**a potom klikněte na **koupit**.
7. Vyberte **Připnout do řídicího panelu**a pak klikněte na **vytvořit**.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Přidání okraje uzel při vytváření clusteru

V této části pomocí šablony správce prostředků k vytvoření HDInsight clusteru s uzel okraje.  Správce prostředků šablon najdete v veřejné [Kontejner úložiště objektů Blob Azure](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json). Správce prostředků šablony hovorů skript akce skript c:\ https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Skript nemá provádět žádné akce.  Toto je ukazují, volající akci skriptu ze šablony správce prostředků.

**Chcete-li přidat prázdné krajního uzlu do existujícího clusteru**

1. Pokud ještě nemáte vytvoření clusteru HDInsight.  Viz [Hadoop kurz: Začínáme s používáním na základě Linux Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klikněte na následujícím obrázku se přihlásit k Azure a otevření šablony Azure správce na portálu Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigurace následující vlastnosti:
        
    - NÁZEV_CLUSTERU: Zadejte název nového obrázku vytvořit.
    - CLUSTERLOGINUSERNAME: Hadoop HTTP uživatelské jméno.  Výchozí název je **Správce**.
    - CLUSTERLOGINPASSWORD: Zadejte heslo uživatele Hadoop HTTP.
    - SSHUSERNAME: SSH uživatelské jméno. Výchozí název je **sshuser**.
    - SSHPASSWORD: Zadejte heslo uživatele SSH.
    - UMÍSTĚNÍ: Zadejte umístění na název pole Skupina zdroje, clusteru a výchozí účet úložiště.
    - CLUSTERTYPE: Výchozí hodnota je **hadoop**.
    - CLUSTERWORKERNODECOUNT: Výchozí hodnota je 2.
    - EDGENODESIZE: Vyberte jednu z OM velikosti.
    - EDGENODEPREFIX: Výchozí hodnota je **Nová**.  Bude použita výchozí hodnota, okraje uzel je v poli Název **nové edgenode**.  Můžete přizpůsobit předponu z portálu. Můžete také upravit celé jméno ze šablony.

4. Kliknutím na **OK** uložte změny.
5. Do **pole Skupina zdroje**zadejte název nové skupiny prostředků.
6. Klikněte na **Revize právní podmínky**a potom klikněte na **koupit**.
7. Vyberte **Připnout do řídicího panelu**a pak klikněte na **vytvořit**. 


## <a name="access-an-edge-node"></a>Access edge uzel

Uzel hrany ssh koncový bod se <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Například nové edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Uzel okraj zobrazuje se jako aplikace na portálu Azure.  Na portálu nabízí informace pro přístup pomocí SSH uzel okraje.

**Chcete-li ověřit koncový bod SSH uzel okraje**

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Otevřete HDInsight obrázku s uzel okraje.
3. Klikněte na **aplikace** z zásuvné obrázku. Zobrazí se uzel okraje.  Výchozí název je **nové edgenode**.
4. Klikněte na uzel okraje. Zobrazí se SSH koncového bodu.

**Používání podregistru na uzel okraj**

5. Připojení k uzel okraj pomocí SSH.  V tématu [použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md) nebo [použít SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Po připojení k uzlu okraj pomocí SSH, spusťte konzolu podregistru pomocí tento příkaz:

        hive
7. Spusťte tento příkaz zobrazte podregistru tabulky v obrázku:

        show tables;

## <a name="delete-an-edge-node"></a>Odstranit okraj uzel

Postranní uzel můžete odstranit z portálu Microsoft Azure.

**Přístup k krajního uzlu**

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Otevřete HDInsight obrázku s uzel okraje.
3. Klikněte na **aplikace** z zásuvné obrázku. Zobrazí se seznam okraj uzlů.  
4. Klikněte pravým tlačítkem myši okraj, který chcete odstranit a potom klikněte na **Odstranit**.
5. Klikněte na **Ano** potvrďte.

## <a name="next-steps"></a>Další kroky

V tomto článku jste se naučili jak přidat okraj uzel a jak získat přístup ke uzel okraje. Další informace naleznete v následujících článcích:

- [Instalace HDInsight aplikace](hdinsight-apps-install-applications.md): Přečtěte si, jak nainstalovat aplikace HDInsight clusterů.
- [Instalace aplikací vlastní HDInsight](hdinsight-apps-install-custom-applications.md): Naučte se nasadit aplikace nepublikované HDInsight HDInsight.
- [Publikování HDInsight aplikace](hdinsight-apps-publish-applications.md): Přečtěte si, jak publikovat vlastních aplikací HDInsight Azure Marketplace.
- [MSDN: instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Přečtěte si, jak definovat HDInsight aplikací.
- [Na základě přizpůsobení Linux HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster-linux.md): Naučte se používat akci skriptu nainstalovat další aplikace.
- [Na základě vytvořit Linux Hadoop clusterů v HDInsight pomocí Správce prostředků šablon](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Přečtěte si, jak volání správce prostředků šablony k vytvoření clusterů HDInsight.

