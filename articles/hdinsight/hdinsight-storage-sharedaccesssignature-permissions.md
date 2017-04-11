<properties
pageTitle="Omezení HDInsight přístupu k datům používání podpisů sdílené aplikace Access"
description="Naučte se používat sdílené podpisy přístup omezení přístupu HDInsight dat uložených v Azure úložiště objektů BLOB."
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
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Použití Azure úložiště sdílené přístup podpisy omezení přístupu k datům s HDInsight

HDInsight používá objektů BLOB Azure úložiště pro ukládání dat. Během HDInsight musí mít plný přístup ke pro clusteru používat jako výchozí úložiště objektů blob, je možné omezit oprávnění dat uložených v jiných objektů BLOB používaný clusteru. Chcete například vytvořit některá data jen pro čtení. Lze provést pomocí sdílené podpisy přístup.

Podpisy sdílené aplikace Access (přidružení zabezpečení) jsou funkce osnovu Azure úložiště, která je možné omezit přístup k datům. Například poskytuje jen pro čtení přístup k datům. V tomto dokumentu budou zjistěte, jak povolit přístup seznamu určené jen pro čtení a kontejneru objektů blob z Hdinsightu pomocí přidružení zabezpečení.

##<a name="requirements"></a>Požadavky

* Předplatné Azure

* C# nebo Python. C# příklad má formu Visual Studio řešení.

    * Visual Studio musí být verze 2013 nebo 2015
    
    * Python musí být verze 2.7 nebo vyšší

* Na základě Linux HDInsight clusteru nebo [Azure PowerShell] [ powershell] – Pokud máte existující obrázku na základě Linux, můžete Ambari sdílené přístup podpis přidal do clusteru. V opačném případě Azure PowerShell můžete použít k vytvoření nového clusteru a přidat podpis sdílené aplikace Access při vytváření obrázku.

* Příklad soubory z [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Toto úložiště obsahuje následující:

    * Projekt aplikace Visual Studio, můžete vytvořit kontejner úložiště, uložené zásady a přidružení zabezpečení pro použití s HDInsight
    
    * Python skript, který můžete vytvořit kontejner úložiště, uložené zásady a přidružení zabezpečení pro použití s HDInsight
    
    * Skript Powershellu, které můžete vytvořit nový cluster HDInsight a nakonfigurovat tak, aby pomocí přidružení zabezpečení.

##<a name="shared-access-signatures"></a>Sdílený přístup podpisy

Existují dva typy sdílené podpisy Access:

* Ad hoc: čas zahájení, dobu platnosti a oprávnění pro přidružení zabezpečení jsou všechny zadané na URI přidružení zabezpečení (nebo předpokládanou v případě, kdy vynechán čas zahájení).

* Uložené zásady přístupu: uložené přístupu je definován v kontejneru zdroje - objektů blob kontejneru, tabulky, fronty nebo sdílené složky - a můžete použít ke správě omezení pro jeden nebo více podpisů sdílený přístup. Přidružení přidružení zabezpečení zásadami uložené přístupu dědí přidružení zabezpečení omezení - čas zahájení, čas vypršení a oprávnění - definované pro uložené přístupu.

Rozdíl mezi dvěma formuláři je důležité pro jeden scénář klíče: odvolání. Přidružení je adresy URL, takže kdokoli, kdo obdrží přidružení zabezpečení, mohli rychle, bez ohledu na to, kdo ho požadavku na začátku. Pokud přidružení zabezpečení je publikován veřejně, ji může používat kdekoli na světě. Přidružení distribuovaný je platný, dokud jednu ze čtyř akcí se stane:

1. Dosažení doby vypršení platnosti zadanou v přidružení zabezpečení.

2. Vypršení platnosti časovému na uložené přístupu odkazováno přidružení zabezpečení dosažení (Pokud se odkazuje uložené přístupu a určuje čas vypršení). Tato situace může buď nastat protože uplyne, nebo jste změnili uložené přístupu mít čas vypršení v minulosti, což je jedním ze způsobů přejděte do centra pro přidružení zabezpečení.

3. Zásady uložené přístupu odkazováno přidružení zabezpečení se odstraní, což je další způsob, jak odebrat přidružení zabezpečení. Všimněte si, že pokud obnovil uložené přístupu se stejným názvem všechny existující tokeny přidružení zabezpečení znovu bude platit podle oprávnění přidružit k této zásady uložené přístupu (za předpokladu, že, doby vypršení platnosti na přidružení zabezpečení neprošel). Pokud mají v úmyslu odvolat přidružení zabezpečení, je nutné použít jiný název, pokud je znovu vytvořit zásady přístupu s vypršení platnosti času v budoucnosti.

4. Klíč účtu, která byla použita k vytvoření přidružení zabezpečení se obnoví. Všimněte si, že to bude mít za následek všechny součásti aplikace pomocí tento klíč účtu selhání chcete ověřit, dokud se aktualizují použít jiné platný účet klíč nebo klíč nově regenerované účtu.

> [AZURE.IMPORTANT] Sdílený přístup podpis URI souvisí s klíč účtu použít k vytvoření podpisu a přidružené uložená zásady přístupu (pokud nějaký). Pokud není zadán žádný uložené přístupu, je jediný způsob, jak odebrat podpis sdílený přístup změnit klíč účtu. 

Doporučujeme, abyste mohli odvolat podpisy nebo prodloužit datum vypršení platnosti v případě potřeby vždy používat zásady uložené přístupu. Kroky v tomto dokumentu uložené zásady přístupu ke generování přidružení zabezpečení.

Další informace o sdílené podpisy přístupu najdete v článku [Principy modelu přidružení zabezpečení](../storage/storage-dotnet-shared-access-signature-part-1.md).

##<a name="create-a-stored-policy-and-generate-a-sas"></a>Vytvoření uložené zásad a generovat přidružení zabezpečení

Nyní vytvořte uložené zásad programově. Můžete najít C# i Python příklad vytváření uložené zásady a přidružení zabezpečení u [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).

###<a name="create-a-stored-policy-and-sas-using-c"></a>Vytvoření uložené zásady a přidružení zabezpečení pomocí C\#

1. Otevřete řešení ve Visual Studiu.

2. V Průzkumníku klikněte pravým tlačítkem myši na __SASToken__ projektu a vyberte __Vlastnosti__.

3. Klikněte na __Nastavení__ a zadat hodnoty pro následující položky:

    * StorageConnectionString: Připojovací řetězec účtu úložiště, který chcete vytvořit uložené zásady a přidružení. Formát by měl být `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` kde `myaccount` je název účtu úložiště a `mykey` je zkratka umožňující účtu úložiště.
    
    * Název_kontejneru: Kontejneru, který chcete omezit přístup k účtu úložiště.
    
    * SASPolicyName: Název pro účely uložené zásad, která se vytvoří.
    
    * FileToUpload: Cesta k souboru, který bude možné odeslat do kontejneru.
    
4. Spuštění projektu. Zobrazí se okno konzoly a podobně jako tento zobrazí se informace po vygenerování přidružení zabezpečení:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    Zásady tokenu přidružení zabezpečení uložte, protože budete potřebovat při přidružení účtu úložiště svůj cluster HDInsight. Budete taky potřebovat název účtu úložiště a jméno container.
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>Vytvoření uložené zásady a pomocí Python přidružení zabezpečení

1. Otevřete soubor SASToken.py a změňte následující hodnoty:

    * zásady\_název: název, který chcete používat pro uložené zásadu, která se vytvoří.
    
    * úložiště\_účtu\_název: název účtu úložiště.
    
    * úložiště\_účtu\_klíč: klíč účtu úložiště.
    
    * úložiště\_kontejneru\_název: kontejneru v účtu úložiště, který chcete omezit přístup k datům.
    
    * Příklad\_soubor\_cesta: cesta k souboru, který bude možné odeslat do kontejneru

2. Spusťte skript. Podobně jako tento přidružení zabezpečení token zobrazí dokončení skript:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    Zásady tokenu přidružení zabezpečení uložte, protože budete potřebovat při přidružení účtu úložiště svůj cluster HDInsight. Budete taky potřebovat název účtu úložiště a jméno container.
    
##<a name="use-the-sas-with-hdinsight"></a>Použití přidružení s HDInsight

Při vytváření HDInsight clusteru, je nutné zadat účet primární a volitelně můžete zadat další úložiště účty. Z těchto postupů obrazovky pro přidání úložiště vyžaduje úplný přístup k účtům úložiště a kontejnery, které se používají.

Pokud chcete omezit přístup k kontejneru pomocí přístup podpis sdílené, je nutné přidat vlastní položku __základní webu__ konfiguraci clusteru.

* Pro __serveru s Windows__ nebo __na základě Linux__ HDInsight clusterů musíte při vytváření clusteru pomocí Powershellu.

* __Na základě Linux__ clusterů HDInsight změníte konfigurace po vytvoření clusteru pomocí Ambari.

###<a name="create-a-new-cluster-that-uses-the-sas"></a>Vytvoření nového obrázku, který používá přidružení zabezpečení

Příklad vytvoření HDInsight obrázku, který používá přidružení zabezpečení je součástí `CreateCluster` adresáře úložiště. Můžete ho použijte podle těchto kroků:

1. Otevřít `CreateCluster\HDInsightSAS.ps1` soubor v textovém editoru a změnit tyto hodnoty na začátek dokumentu.

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    Například změnit `'mycluster'` název obrázku, který chcete vytvořit. Přidružení zabezpečení hodnoty musí shodovat s hodnotami z předchozích kroků při vytváření účtu úložiště a token přidružení zabezpečení.
    
    Po změně hodnot, uložte soubor.

1. Otevřete nový příkazovém řádku prostředí PowerShell Azure. Pokud jsou seznámeni s Azure PowerShell nebo ho nenainstalovali, přečtěte si článek [instalace a konfigurace prostředí PowerShell Azure][powershell].

2. Z příkazového řádku použijte následující ověření k předplatnému Azure:

        Login-AzureRmAccount
    
    Po zobrazení výzvy se přihlaste pomocí účtu u předplatného Azure.
    
    Pokud vaše přihlášení souvisí s víc předplatných Azure, budete muset používat `Select-AzureRmSubscription` vyberte předplatné, které chcete použít.

2. Z příkazového řádku, změna adresáře `CreateCluster` adresář, který obsahuje požadovaný soubor HDInsightSAS.ps1. Pomocí následujících spuštění skriptu
        
        .\HDInsightSAS.ps1
    
    Při spuštění skript zaznamená výstup na příkazovém řádku prostředí PowerShell při vytváření skupiny a úložiště účty zdroje. Ho bude pak vyzváni k zadání uživatelského HTTP clusteru HDInsight. Toto je uživatelský účet používá k zabezpečení protokolu HTTP/s přístup ke clusteru.
    
    Pokud vytváříte Linux clusteru, zobrazí se výzva taky pro SSH uživatelského účtu jména a hesla. Slouží k vzdáleně přihlášení k clusteru.
    
    > [AZURE.IMPORTANT] Po zobrazení výzvy k protokolu HTTP/s nebo SSH uživatelské jméno a heslo, je nutné zadat heslo, který splňuje následující kritéria:
    >
    > - Musí být délku nejméně 10 znaků
    > - Musí obsahovat alespoň jednu číslici
    > - Musí obsahovat alespoň jeden jiné než alfanumerické znaky
    > - Musí obsahovat alespoň jeden velká nebo malé písmeno


Bude trvat déle pro tento skript dokončete obvykle asi 15 minut. Po dokončení skriptu bez chyb byl vytvořen clusteru.

###<a name="update-an-existing-cluster-to-use-the-sas"></a>Aktualizace stávající clusteru používat přidružení zabezpečení

Pokud máte existující obrázku na základě Linux, můžete přiřadit přidružení zabezpečení __základní__ síť pomocí následujících kroků:

1. Otevřete web Ambari uživatelského rozhraní pro svůj cluster. Adresa pro tuto stránku je https://YOURCLUSTERNAME.azurehdinsight.net. Po zobrazení výzvy ověřit clusteru jménem správce (Správci) a heslo, které jste použili při vytváření clusteru.

2. Na levé straně webu Ambari uživatelského rozhraní vyberte __HDFS__ a pak vyberte kartu __Configs__ uprostřed stránky.

3. Vyberte kartu __Upřesnit__ a potom pomocí posuvníku najděte v části __vlastní základní webu__ .

4. Rozbalení oddílu __vlastní základní webu__ a pak přejděte na konec a vyberte odkaz __Přidat vlastnost …__ . Použijte tyto hodnoty pro pole __klíčů__ a __hodnot__ :

    * __Klíč__: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __Hodnota__: přidružení zabezpečení je vrácena C# nebo Python aplikací jste spustili dříve
    
    Nahraďte __NÁZEV_KONTEJNERU__ kontejneru název, který jste použili s C# nebo přidružení zabezpečení aplikace. Nahraďte __STORAGEACCOUNTNAME__ název účtu úložiště, kterou jste použili.

5. Klikněte na tlačítko __Přidat__ uložte tento klíč a jeho hodnota a potom na tlačítko __Uložit__ uložte změny konfigurace. Po zobrazení výzvy přidat popis změnit ("Přidání úložiště přidružení zabezpečení aplikace access" například) a potom klikněte na __Uložit__.

    Po dokončení změny, klikněte na tlačítko __OK__ .

    > [AZURE.IMPORTANT] Tím uložíte změny konfigurace, ale až po restartování několik služeb začne platit.

6. Na webu Ambari uživatelského rozhraní vyberte __HDFS__ ze seznamu na levé straně a __Akce služby__ rozevíracího seznamu na pravé straně vyberte __Restartujte všechny__ . Po zobrazení výzvy vyberte __Zapnout režim údržby__ a potom vyberte __Conform restartujte všechny".

    Tento postup opakujte pro MapReduce2 a vláken položky v seznamu na levé straně stránky.

7. Po těchto restartování, vyberte každý z nich a zakažte údržbu režimem __Akce služby__ rozevírací seznam.

##<a name="test-restricted-access"></a>Testování omezeného přístupu

Abyste ověřili, že mají omezený přístup, postupujte takto:

* __Serveru s Windows__ clusterů HDInsight pomocí vzdálené plochy se připojit k clusteru. Další informace najdete v tématu [připojit k HDInsight pomocí RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) .

    Po připojení, otevřete okno příkazového řádku pomocí ikonu __Hadoop příkazového řádku__ na stolním počítači.

* __Na základě Linux__ clusterů HDInsight umožňuje SSH připojte se k němu. Najdete v článku jednu z následujících akcí pro informace o použití SSH s na základě Linux clusterů:

    * [Použití SSH s Hadoop Linux založené na HDInsight z Linux, OS X a Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
Po připojení k clusteru, následujícím způsobem můžete ověřit můžete jen pro čtení a seznam položek na účtu úložiště přidružení zabezpečení:

1. Příkazový řádek zadejte tento příkaz. Nahraďte __SASCONTAINER__ jméno container vytvořili pro účet úložiště přidružení zabezpečení. Nahraďte __SASACCOUNTNAME__ název účtu úložiště pro přidružení zabezpečení:

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    Tím se zobrazit obsah kontejneru, která by měl obsahovat soubor, který byl odeslán vytvoření kontejneru a přidružení zabezpečení.
    
2. Použití následující k ověření, že si můžete přečíst obsah souboru. Nahrazení __SASCONTAINER__ a __SASACCOUNTNAME__ jako v předchozím kroku. Nahraďte __název souboru__ název souboru zobrazí v předchozí příkaz:

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    Tento seznam obsah souboru.
    
3. Pomocí následujících budou moct soubor stáhnout na místním pevném disku:

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    To bude stáhněte na místní soubor s názvem __testfile.txt__.

4. K nahrání souboru místní na nový soubor s názvem __testupload.txt__ úložný prostor přidružení zabezpečení použijte následující:

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    Zobrazí se zpráva podobná této:
    
        put: java.io.IOException
        
    K této chybě dochází, protože umístění úložiště je další seznam + pouze. Vložte data na výchozí úložiště pro clusteru, což je zapisovatelný pomocí následující:
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    Tentokrát je úspěšně dokončena operaci.
    
##<a name="troubleshooting"></a>Řešení potíží

###<a name="a-task-was-canceled"></a>Zrušení úkolu

__Příznaky__: při vytváření clusteru pomocí skriptů Powershellu, může se zobrazit tato chybová zpráva:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__Příčina__: k této chybě může dojít, pokud používáte heslo pro správu/HTTP uživatele pro clusteru, nebo (na základě Linux clusterů) SSH uživatele.

__Řešení__: použijte heslo, který splňuje následující kritéria:

- Musí být délku nejméně 10 znaků
- Musí obsahovat alespoň jednu číslici
- Musí obsahovat alespoň jeden jiné než alfanumerické znaky
- Musí obsahovat alespoň jeden velká nebo malé písmeno

##<a name="next-steps"></a>Další kroky

Teď, když jste se naučili přidání úložiště omezený přístup k HDInsight obrázku, přečtěte si další způsoby, jak pracovat s daty v svůj cluster:

* [Použití podregistru s HDInsight](hdinsight-use-hive.md)

* [Použití Prasátko s HDInsight](hdinsight-use-pig.md)

* [Použití MapReduce s HDInsight](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md
