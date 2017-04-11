<properties
   pageTitle="Vytvoření clusterů HDInsight pomocí úložiště jezera dat Azure pomocí prostředí PowerShell | Azure"
   description="Použití Powershellu Azure umožňuje vytvářet a používat s jezera dat Azure HDInsight clusterů"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Vytvoření clusteru HDInsight s úložiště jezera dat pomocí prostředí PowerShell Azure

> [AZURE.SELECTOR]
- [Pomocí portálu](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Pomocí prostředí PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Použití Správce prostředků](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Zjistěte, jak pomocí Powershellu Azure konfigurace s přístupem k úložiště jezera dat Azure HDInsight obrázku (Hadoop HBase a bouře). Některé důležité důležité informace o této verzi:

* **Pro Spark clusterů (Linux) a Hadoop/bouře clusterů (Windows a Linux)**, úložiště jezera dat lze použít pouze jako účet další úložiště. Výchozí účet úložiště u těchto clusterů budou nadále úložiště objektů BLOB Azure (WASB).

* **Pro HBase clusterů (Windows a Linux)**, úložiště jezera dat lze použít jako výchozí úložiště nebo další úložiště.

> [AZURE.NOTE] Několik důležitých aspektů, je potřeba pamatovat. 
> 
> * Možnost vytvoření HDInsight clusterů s přístupem k úložišti jezera je dostupný jenom u verze HDInsight 3,2 a 3.4 (pro Hadoop HBase a bouře clusterů v systému Windows, jakož i Linux). Spark clusterů na Linux tato možnost je k dispozici pouze na clusterů HDInsight 3.4.
>
> * Výše uvedené, úložiště jezera dat neexistuje jako výchozí úložiště u některých typů obrázku (HBase) a další úložiště pro jiné typy obrázku (Hadoop, Spark, bouře). Použití úložiště jezera dat jako účet další úložiště nemají vliv na výkon nebo možnost pro čtení i zápis k základnímu úložišti z clusteru. V případě použití úložiště jezera dat jako dalšího úložiště soubory související s obrázku (například protokoly atd.) jsou došlo k zápisu výchozí úložiště (objektů BLOB Azure), zatímco data, která chcete zpracovat mohou být uloženy v úložišti jezera účtu.


V tomto článku jsme zřízení Hadoop obrázku s úložiště jezera dat jako dalšího úložiště.

Konfigurace HDInsight pro práci s úložiště jezera dat pomocí prostředí PowerShell zahrnuje tyto kroky:

* Vytvoření úložiště jezera dat Azure
* Nastavení ověřování na základě rolí přístupu pro jezera úložiště dat
* Vytvoření HDInsight obrázku s ověřením dat jezera Store
* Spusťte testovací úlohy na clusteru

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- **Azure PowerShell 1.0 nebo vyšší**. Přečtěte si, [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).

- **Windows SDK**. Můžete ji nainstalovat z [tady](https://dev.windows.com/en-us/downloads). Které slouží k vytvoření certifikátu zabezpečení.

- **Azure jistinu služby Active Directory**. Kroky v tomto kurzu obsahují pokyny k vytváření hlavní název služby Azure AD. Ale musíte být správcem Azure AD mohli vytvořit hlavní název služby. Pokud jste správcem Azure AD, můžete přeskočit tento předpoklad a pokračovat v kurzu.
    
    **Pokud jste správcem není Azure AD**, nebude moct proveďte kroky potřebné k vytvoření hlavní název služby. V takovém případě správce Azure AD nejdřív vytvořte hlavní název služby před vytvořením HDInsight obrázku s úložištěm jezera Data. Navíc jistinu služby musí být vytvořen pomocí certifikátu, jak je uvedeno na stránce [vytvořit služby základní pomocí certifikátu](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate). 


## <a name="create-an-azure-data-lake-store"></a>Vytvoření úložiště jezera dat Azure

Tímto postupem vytvoření úložiště jezera Data.

1. Z počítače otevřete nové okno Azure PowerShell a zadejte následující fragment kódu. Po zobrazení výzvy k přihlášení, zkontrolujte, jestli že se přihlaste se jako jednu z admininistrators/vlastník předplatného:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Pokud se zobrazí zpráva podobná `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` při registraci poskytovatele zdroje dat jezera úložiště, je možné, že vaše subsrcription není povolené pro úložiště jezera dat Azure. Zkontrolujte, jestli že povolíte předplatného Azure jezera úložiště veřejného (verze Preview) podle těchto [pokynů](data-lake-store-get-started-portal.md#signup).

3. Účet Azure dat jezera úložiště je přidružené ke skupině Azure zdroje. Začněte tak, že vytvoříte skupina zdroje Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Vytvoření skupiny zdroje Azure] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Vytvoření skupiny zdroje Azure")

2. Vytvořte účet Azure dat jezera úložiště přihlašovacích údajů. Název účtu, který zadáte musí obsahovat pouze malá písmena a číslice.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Vytvořit účet jezera dat Azure] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Vytvořit účet jezera dat Azure")

3. Ověřte, že účet je úspěšně.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Výstup pro tuto by měl být **pravdivé**.

4. Nahrajte ukázkových dat na jezera dat Azure. Použijeme tento dál v tomto článku můžete ověřit, že data jsou přístupné z Hdinsightu obrázku. Pokud hledáte ukázkových dat chcete odeslat, můžete získat složce **Ambulance dat** z [Azure dat jezera libovolná úložiště](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Nastavení ověřování na základě rolí přístupu pro jezera úložiště dat

Každý Azure předplatné souvisí s Azure Active Directory. Uživatelé a služby, které přístupu k prostředkům předplatného pomocí portálu klasické Azure nebo Azure správce prostředků API nejdřív ověřuje se službou Azure Active Directory. Access uděleno Azure předplatná a služby přiřazením odpovídající roli na Azure zdroje.  Hlavní název služby státní, identifikuje službu v Azure Active Directory (AAD). Tato část ukazuje, jak poskytnout aplikaci služby, jako je HDInsight, přístup k prostředku Azure (účet Azure úložišti jezera jste dříve vytvořili) vytvořením hlavní název služby pro aplikaci a přiřazení rolí, který pomocí prostředí PowerShell Azure.

Nastavení ověřování služby Active Directory pro jezera dat Azure, je nutné provést následující úkoly.

* Vytvoření certifikátu podepsaného svým držitelem
* Vytvoření aplikace v Azure Active Directory a hlavní název služby

### <a name="create-a-self-signed-certificate"></a>Vytvoření certifikátu podepsaného svým držitelem

Zkontrolujte, že máte [Windows SDK](https://dev.windows.com/en-us/downloads) nainstalovaný před provedením kroků v této části. Musí taky nevytvoříte adresáře, například **C:\mycertdir**, kde se bude vytvořena certifikát.

1. V okně prostředí PowerShell přejděte do umístění, kam jste nainstalovali Windows SDK (obvykle `C:\Program Files (x86)\Windows Kits\10\bin\x86` a používat [MakeCert] [ makecert] k vytvoření certifikátu podepsaného svým držitelem a privátním klíčem. Použijte následující příkazy.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    Zobrazí se výzva k zadání hesla privátní klíče. Po příkazu úspěšně, byste měli vidět **CertFile.cer** a **mykey.pvk** v adresáři certifikát, který jste zadali.

4. Použití [Pvk2Pfx] [ pvk2pfx] nástroje pvk a .cer soubory vytvořené MakeCert převést soubor .pfx. Spusťte tento příkaz.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Po zobrazení výzvy zadejte dříve zadané heslo soukromého klíče. Zadané pro parametr **nákupních objednávek** hodnotu heslo, které je přidružený soubor .pfx. Po úspěšném dokončení příkazu, byste měli vidět CertFile.pfx v adresáři certifikát, který jste zadali.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Vytvoření Azure Active Directory a hlavní název služby

V této části proveďte kroky k vytváření služby základní pro aplikaci služby Azure Active Directory, přiřadit roli služby základní a ověření jako hlavní služby zadáním certifikát. Spusťte následující příkazy k vytvoření aplikace služby Azure Active Directory.

1. Vložte tyto rutiny prostředí PowerShell konzoly okna. Zkontrolujte, že hodnota, kterou zadáte vlastnost **- DisplayName** je jedinečný. Hodnoty **– Domovská stránka** a **- IdentiferUris** také jsou hodnoty zástupný symbol a nejsou ověření.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Vytvořte hlavní název služby pomocí ID aplikace.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. Udělte služby základní přístup k úložišti jezera soubor nebo složka, bude přistupovat z obrázku HDInsight. Fragment dole poskytuje přístup k kořenové úložiště jezera dat účtu.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    Do příkazového řádku zadejte **Y** .

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Vytvoření clusteru HDInsight s ověřením dat jezera Store

V této části vytvoříme HDInsight Hadoop obrázku. V této verzi obrázku HDInsight a jezera úložišti musí být ve stejném umístění (východoasijských USA 2).

1. Začít s načítání ID předplatného klienta. Je třeba, později.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. V této verzi produktu pro Hadoop cluster úložiště jezera dat pouze lze jako další úložiště clusteru. Výchozí úložiště objektů BLOB Azure úložiště (WASB) nebude. Ano nejdřív vytvoříme úložiště klienta a kontejnery úložiště potřebných pro clusteru.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. Vytvoření obrázku HDInsight. Pomocí těchto rutin.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Po úspěšném dokončení rutinu byste měli vidět výstup takto:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Spusťte testovací úlohy clusteru HDInsight používat jezera úložiště dat

Po konfiguraci clusteru HDInsight spuštěním testovací úlohy clusteru otestovat clusteru HDInsight přístup k úložišti jezera. Postup jsme se spustí úlohy podregistru vzorku, která vytvoří tabulku pomocí ukázkových dat, která jste nahráli dříve vašeho úložiště jezera.

### <a name="for-a-linux-cluster"></a>Pro Linux obrázku

V této části se dozvíte, jak SSH do obrázku a spustit ukázkový podregistru dotaz. Windows neposkytuje předdefinované SSH klienta. Doporučujeme používat **nátěrové**, které si můžete stáhnout z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Další informace o použití nátěrové najdete v tématu [Použití SSH s bázi Linux Hadoop na HDInsight z Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Po připojení, začněte tím, že pomocí následujícího příkazu podregistru rozhraní příkazového řádku:

        hive

2. Pomocí rozhraní příkazového řádku zadejte následující příkazy k vytvoření nové tabulky s názvem **vozidla** pomocí ukázková data v úložišti jezera dat:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Měli byste vidět výstup podobná této:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>Pro clusteru systému Windows

Pomocí těchto rutin spusťte dotaz podregistru. V tomto dotazu můžeme vytvořit tabulku z dat v úložišti jezera dat a potom spustit výběrový dotaz na vytvořenou tabulku.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Bude to mít následující výstup. **ExitValue** 0 do výstupu o tom, že úloha byla úspěšně dokončena.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

Načíst výstup z úlohy pomocí následující rutinu:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

Výstup projektu vypadá takto:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Pomocí příkazů HDFS úložiště jezera dat aplikace Access

Po konfiguraci clusteru HDInsight používat úložiště jezera dat, můžete příkazy HDFS prostředí pro přístup k úložišti.

### <a name="for-a-linux-cluster"></a>Pro Linux obrázku

V tomto oddílu, který bude SSH do clusteru a spusťte HDFS příkazy. Windows neposkytuje předdefinované SSH klienta. Doporučujeme používat **nátěrové**, které si můžete stáhnout z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Další informace o použití nátěrové najdete v tématu [Použití SSH s Hadoop Linux založené na HDInsight z Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Po připojení seznamu souborů v úložišti jezera dat pomocí následujícího příkazu systému souborů HDFS.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

To uvádět soubor, který jste nahráli dříve jezera úložišti.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Můžete taky použít `hdfs dfs -put` příkaz nahrát některé soubory k úložišti jezera dat a potom pomocí `hdfs dfs -ls` k ověření, zda soubory, které byly úspěšně nahrál(a) na server.


### <a name="for-a-windows-cluster"></a>Pro clusteru systému Windows

1. Přihlaste se na novém [Portálu Azure](https://portal.azure.com).

2. Klikněte na tlačítko **Procházet**, klikněte na **HDInsight clusterů**a klikněte HDInsight obrázku, který jste vytvořili.

3. V zásuvné clusteru klikněte **Vzdálená plocha**a pak v zásuvné **Vzdálené plochy** , klikněte na **Připojit**.

    ![Vzdálené do HDI obrázku] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Vytvoření skupiny zdroje Azure")

    Po zobrazení výzvy zadejte přihlašovací údaje, které jste uvedli pro vzdálené plochy uživatele.

4. V vzdálenou relaci spusťte Windows PowerShell a taky pomocí příkazů systému souborů HDFS seznam souborů v úložišti jezera dat Azure.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    To měli do seznamu, které jste nahráli dříve jezera úložišti.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    Můžete taky použít `hdfs dfs -put` příkaz nahrát některé soubory k úložišti jezera dat a potom pomocí `hdfs dfs -ls` k ověření, zda soubory, které byly úspěšně nahrál(a) na server.

## <a name="see-also"></a>Viz taky

* [Portál: Vytvoření clusteru HDInsight používat jezera úložiště dat](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
