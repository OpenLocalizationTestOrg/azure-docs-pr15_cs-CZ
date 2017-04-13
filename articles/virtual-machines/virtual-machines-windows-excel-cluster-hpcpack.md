<properties
 pageTitle="Shluk HPC Pack pro Excel a SOA | Microsoft Azure"
 description="Začínáme ve velkém měřítku Excelu a SOA úloh spuštěna HPC Pack obrázku v Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Začínáme spuštěna aplikace Excel a SOA pracovního vytížení HPC Pack obrázku v Azure

V tomto článku se dozvíte, jak nasazení Microsoft HPC Pack clusteru v Azure virtuálních počítačích pomocí šablony Azure rychlý úvod a volitelně skriptu prostředí PowerShell Azure nasazení. Clusteru používá obrázky Azure Marketplace OM navržený pro práci s HPC Pack Microsoft Excelu nebo pracovního vytížení orientovaných na služby architektura (SOA). Clusteru slouží ke spuštění jednoduché HPC Excelu a službách SOA z místního klientského počítače. Služby Excel HPC zahrnují odstranění sešitu aplikace Excel a uživatelem definovaných funkcí aplikace Excel nebo funkce definované uživatelem.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Na následujícím obrázku vidíte na vysoké úrovni, HPC Pack obrázku, že které vytvoříte.

![HPC obrázku s uzly spuštění pracovního vytížení aplikace Excel][scenario]

## <a name="prerequisites"></a>Zjistit předpoklady pro

*   **Klientský počítač** - potřebujete serveru s Windows klientský počítač ukázkové Excelu a SOA úlohy do clusteru. Budete potřebovat počítače s Windows spustit skript Powershellu Azure clusteru nasazení (Pokud se rozhodnete příslušný způsob nasazení) a

*   **Azure předplatné** – Pokud nemáte předplatné Azure, můžete vytvořit [účet zase uvolnit](https://azure.microsoft.com/pricing/free-trial/) v jenom pár minut.

*   **Kvóta jádra** - může být nutné zvětšit kvóty jádra, zejména pokud nasadíte několik uzlech s vícejádrových OM velikosti. Pokud používáte šablonu Azure rychlý úvod, kvóta jádra ve Správci zdrojů je za Azure oblast. V takovém případě budete muset zvýšit kvóty v určité oblasti. V tématu [limity Azure předplatného, kvót a omezení](../azure-subscription-service-limits.md). Pokud chcete zvýšit kvóty, [Otevřete žádost o konverzaci online Zákaznická podpora](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zdarma.

*   **Licenci Microsoft Office** – Pokud nasadíte výpočetním uzlů pomocí Marketplace HPC Pack OM obrázek v aplikaci Microsoft Excel 30denní zkušební verze aplikace Microsoft Excel Professional Plus 2013 nainstalovaný. Po zkušební období je třeba zadat platnou licenci Microsoft Office k aktivaci aplikace Excel nadále spustit úloh. V tématu [Aktivace Excelu](#excel-activation) dál v tomto článku. 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Krok 1. Nastavení obrázku HPC Pack v Azure

Ukážeme dvě možnosti pro nastavení clusteru: první použití šablony pro Azure rychlý úvod a Azure portálu. a druhou, pomocí skriptu prostředí PowerShell Azure nasazení.


### <a name="option-1-use-a-quickstart-template"></a>Možnost 1. Použít šablonu rychlý úvod
Použití šablony pro Azure rychlý úvod k rychle a snadno nasazení HPC Pack obrázku na portálu Azure. Při otevření šablony na portálu uslyšíte jednoduché uživatelské rozhraní místo, kam zadáte nastavení pro svůj cluster. Tady je postup. 

>[AZURE.TIP]Když budete chtít použijte pro [šablony z Azure Marketplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) , která vytvoří podobné clusteru speciálně pro Excel úloh. Postup trochu lišit následující.

1.  Navštivte [stránku šablony vytvořit clusteru HPC na GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Pokud budete potřebovat, prohlédněte si informace o šabloně a zdrojového kódu.

2.  Klikněte na **Deploy Azure** spusťte na nasazení pomocí šablony na portálu Azure.

    ![Nasazení šablony Azure][github]

3.  Na portálu takto zadat parametry šablony HPC obrázku.

    na. Na stránce **Parametry** zadejte nebo změňte hodnoty parametrů šablony. (Klikněte na ikonu vedle jednotlivých nastaveních pro nápovědu.) Ukázkové hodnoty se zobrazí na následující obrazovce. Tento příklad vytvoří obrázku s názvem *hpc01* v doméně *hpc.local* skládající se z hlavního uzlu a 2 výpočet uzlů. Uzly výpočetním vytvořené z HPC Pack OM obrázku, který obsahuje aplikaci Microsoft Excel.

    ![Zadejte parametry][parameters]

    >[AZURE.NOTE]Hlavy uzel OM automaticky se vytvoří z [nejnovější obrázek Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC Pack 2012 R2 v systému Windows Server 2012 R2. Obrázek se aktuálně podle HPC Pack 2012 R2 aktualizace 3.
    >
    >Výpočetním uzel VMs vytvořené z nejnovější obrázku vybrané výpočetním uzel řady. Vyberte možnost **ComputeNodeWithExcel** pro obrázek výpočetním uzel nejnovější HPC Pack obsahuje zkušební verze aplikace Microsoft Excel Professional Plus 2013. Abyste mohli nasadit clusteru obecné SOA relace nebo odstranění UDF Excelu, vyberte možnost **ComputeNode** (bez nainstalované aplikace Excel).

    b. Zvolte předplatné.

    c. Vytvoření skupiny zdroje pro obrázku, například *hpc01RG*.

    d. Vyberte umístění pro skupinu zdrojů, například centrální USA.

    e. Na stránce **právní podmínky** přečtěte si podmínky. Pokud souhlasíte, klikněte na **koupit**. Potom po dokončení nastavení hodnot šablony klikněte na **vytvořit**.

4.  Při nasazení dokončení (obvykle to trvá asi 30 minut), export souboru certifikát obrázku z hlavního uzlu obrázku. V pozdější kroku importujete certifikát veřejné v klientském počítači k zajištění straně serveru ověřování zabezpečené HTTP vazby.

    na. Připojení k hlavního uzlu tak, že Vzdálená plocha z portálu Microsoft Azure.

     ![Připojení k hlavy uzel][connect]

    b. Pomocí standardní postupy ve Správci certifikátů exportujte certifikát hlavou uzel (umístěné pod certifikátu: \LocalMachine\My) bez privátním klíčem. V tomto příkladu exportovat *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Exportujte certifikát][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Možnost 2. Pomocí skriptu HPC Pack IaaS nasazení

Skript pro nasazení HPC Pack IaaS umožňuje jiné univerzální nasazení HPC Pack obrázku. Vytvoří cluster v modelu klasické nasazení, že šablona používá správce prostředků Azure nasazení modelu. Skript je také kompatibilní s předplatným služby Azure globální nebo Azure Čína.

**Další požadavky**

* **Azure Powershellu** - [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) (verze 0.8.10 nebo novější) ve vašem klientském počítači.

* **Skript pro nasazení HPC Pack IaaS** – stáhněte si a rozbalte nejnovější verzi skript z [Webu služby Stažení softwaru](https://www.microsoft.com/download/details.aspx?id=44949). Kontrola verze skript spuštěním `New-HPCIaaSCluster.ps1 –Version`. Tento článek je založená na verzi 4.5.0 nebo vyšší skript.

**Vytvoření konfiguračního souboru**

 Skript pro nasazení HPC Pack IaaS používá konfiguračního souboru XML předávat na vstupu popisující infrastruktury HPC obrázku. Abyste mohli nasadit cluster obsahuje hlavy uzel a 18 výpočetním uzly vytvořené z výpočetním uzel obrázek, který obsahuje aplikaci Microsoft Excel, nahraďte hodnoty ve vašem prostředí do následující ukázkový soubor konfigurace. Další informace o konfiguračního souboru najdete v článku Manual.rtf soubor ve složce skriptu a [vytvořit HPC obrázku se skriptem HPC Pack IaaS nasazení](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Poznámky k konfiguračního souboru**

* **VMName** hlavou uzlu **musí** být stejné jako **Název_služby**nebo SOA úlohy se nepodařilo spustit.

* Zkontrolujte, že zadáte **EnableWebPortal** tak, aby certifikát hlavního uzlu vytvářejí a exportovat.

* Soubor Určuje skript Powershellu konfigurace PostConfig.ps1, která poběží na uzel hlavy. Následující ukázkový skript nakonfiguruje připojovací řetězec Azure úložiště, odebere roli uzel výpočetním z hlavního uzlu a spojuje online všechny uzly při jejich nasazení. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Spuštění skriptu**

1.  Spusťte konzolu Powershellu v klientském počítači jako správce.

2.  Změna adresáře ke složce skript (E:\IaaSClusterScript v tomto příkladu).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Abyste mohli nasadit clusteru HPC Pack, spusťte tento příkaz. V tomto příkladu předpokládá, že konfigurační soubor uložený v E:\HPCDemoConfig.xml.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

Skript pro nasazení HPC Pack spustí pro určitou dobu. Jedna věc, kterou skriptu je exportovat a stáhněte si certifikát obrázku a uložte ho ve aktuální složka Dokumenty v klientském počítači. Skript vygeneruje zprávu podobnou následující. V následujícím kroku importujete certifikát v úložišti příslušný certifikát.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Krok 2. Převzít Excelových sešitů a spuštění funkce definované uživatelem z místního klienta

### <a name="excel-activation"></a>Aktivace aplikace Excel

Při použití obrázek ComputeNodeWithExcel OM pro výrobní zatížení, je třeba zadat platný klíč licenci Microsoft Office k aktivaci aplikace Excel v jednotlivých uzlech výpočetním. V opačném zkušební verzi Excelu vyprší po 30 dnech a systémem Excelové sešity se nezdaří s COMException (0x800AC472). 

Obnovení aplikace Excel pro jiné 30denní zkušební dobu: Přihlaste se k hlavou uzel a clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` na všechny aplikace Excel výpočet uzly prostřednictvím Správce clusteru HPC. Obnovení maximálně dvakrát. Až to je nutné zadat platnou licenci klíč Office.

Office Professional Plus 2013 nainstalovaný na ikonu OM je multilicenční edice s obecný hlasitost licenci klíč (GVLK). Ho můžete aktivovat pomocí správy služby KLÍČŮ / Active Directory-Based aktivace (AD-BA) nebo více aktivaci kódu MAK. 

    * Pomocí služby správy KLÍČŮ/AD-BA, použijte existující server služby správy KLÍČŮ nebo vytvořit novou pomocí Microsoft Office 2013 hlasitost licenci Pack. (Pokud budete chtít, nastavení serveru na uzel hlavou.) Potom aktivujte klíč hostitele služby správy KLÍČŮ prostřednictvím Internetu nebo telefonu. Potom clusrun `ospp.vbs` k nastavení služby správy KLÍČŮ serveru a portu a aktivace Office na všechny aplikace Excel výpočet uzlů. 
    
    * Použití MAK, první clusrun `ospp.vbs` klávesu při zadávání a aktivovat všechny aplikace Excel výpočet uzly prostřednictvím Internetu nebo telefonu. 

>[AZURE.NOTE]Prodejní product key pro Office Professional Plus 2013 není možné použít u tohoto OM obrázku. Pokud máte platný klíče a instalační médium pro edice Office nebo Excel kromě tohoto multilicenční edice Office Professional Plus 2013, můžete je místo toho. Nejdřív odinstalujte multilicenční edice a nainstalovat edici, které máte. Znovu nainstalován výpočetní uzly Excel lze zachytit jako vlastní obrázek OM pro použití v nasazení ve velkém měřítku.

### <a name="offload-excel-workbooks"></a>Převzít sešitů aplikace Excel

Tímto postupem převzít Excelového sešitu tak, aby ho clusteru HPC Pack v Azure. K tomuto účelu musí mít Excelu 2010 nebo 2013 už nainstalovaný v klientském počítači.

1. Použijte jednu z možností v části Krok 1 pro nasazení HPC Pack obrázku s v aplikaci Excel výpočet uzel obrázek. Získejte certifikát soubor obrázku (.cer) a clusteru uživatelské jméno a heslo.

2. V klientském počítači importujte certifikátu obrázku v části certifikátu: \CurrentUser\Root.

3. Zkontrolujte, že je nainstalovaný Excel. Vytvoření souboru Excel.exe.config pomocí následujícího obsahu do stejné složky jako Excel.exe na klientském počítači. Tento krok zajistí, že doplněk HPC Pack 2012 R2 Excel COM načte úspěšně.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  Nastavení klienta odesílat úlohy clusteru HPC Pack. Jednou z možností je úplná [instalace HPC Pack 2012 R2 aktualizace 3](http://www.microsoft.com/download/details.aspx?id=49922) stáhněte a nainstalujte klienta HPC Pack. Můžete taky stáhněte a nainstalujte [HPC Pack 2012 R2 aktualizace 3 klientské nástroje](https://www.microsoft.com/download/details.aspx?id=49923) a odpovídající Visual C++ 2010 redistributable pro počítače ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  V tomto příkladu použijeme Ukázkový sešit aplikace Excel s názvem ConvertiblePricing_Complete.xlsb. Můžete si ji [tady](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  Kopírování Excelového sešitu do pracovní složku například D:\Excel\Run.

7.  Otevřete sešit aplikace Excel. Na pásu karet **vývoje** klikněte na **Doplňky modelu COM** a potvrďte, že doplněk HPC Pack Excel COM načtení úspěšně.

    ![Doplněk pro HPC Pack aplikace Excel][addin]

8.  Úprava makra jazyka VBA HPCControlMacros v aplikaci Excel změnou z řádků, jak je vidět v následujícím skriptu. Nahradit hodnoty odpovídající prostředí.

    ![Akce makra aplikace Excel pro HPC Pack][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  Zkopírujte Excelový sešit do adresáře nahrát například D:\Excel\Upload. Tento adresář není zadán konstanty HPC_DependsFiles makra jazyka VBA.

10. Sešitu clusteru v Azure spustíte kliknutím na tlačítko **obrázku** na listu.

### <a name="run-excel-udfs"></a>Spuštění funkce definované uživatelem aplikace Excel

Spusťte Excel funkce definované uživatelem, postupujte podle předchozích kroků 1 – 3 můžete nastavit klientský počítač. Pro Excel funkce definované uživatelem nemusíte mít nainstalovanou uzlech výpočetním aplikaci Excel. Ano při vytváření svůj cluster výpočet uzly, mohli byste normální výpočet obrázek uzlu místo obrázek výpočetním uzel s Excelem.

>[AZURE.NOTE] Existuje limit 34 znak v aplikaci Excel 2010 a 2013 clusteru spojnice dialogovým oknem. Pomocí tohoto dialogového můžete určit, která poběží funkce definované uživatelem obrázku. Pokud název úplné clusteru je delší (například hpcexcelhn01.southeastasia.cloudapp.azure.com), je příliš velký v dialogovém okně. Alternativní řešení je nastavit proměnnou celého například *CCP_IAASHN* hodnoty název dlouhé clusteru. Zadejte *CCP_IAASHN %* v dialogovém okně hlavou uzel názvem obrázku. 

Po úspěšném nasazení clusteru, pokračujte následujícími kroky spuštění předdefinované výběru UDF aplikace Excel. Vlastní Excelové funkce definované uživatelem najdete v článku tyto [materiály](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) XLL sestavovat a nasazovat je IaaS clusteru.

1.  Otevřete nový Excelový sešit. Na pásu karet **vývoje** klikněte na **Položku doplňky**. V dialogovém okně klikněte na **Procházet**, přejděte do složky %CCP_HOME%Bin\XLL32 a vyberte ukázkový ClusterUDF32.xll. Pokud ClusterUDF32 neexistuje v klientském počítači, zkopírujte ho ze složky %CCP_HOME%Bin\XLL32 na uzel hlavy.

    ![Výběr souboru UDF][udf]

2.  Klikněte na **soubor** > **Možnosti** > **Upřesnit**. V části **vzorce**zaškrtněte **Povolit funkcí XLL definovaných uživatelem ve výpočetním clusteru spustit**. Potom klikněte na tlačítko **Možnosti** a zadejte název úplné obrázku do pole **název hlavního uzlu obrázku**. (Uvedeno dříve vstupním poli se omezí na 34 znaků, tak, aby název dlouhé clusteru nemusí vešly. Mohli byste použít proměnnou celého názvu dlouhé clusteru.)

    ![Konfigurace souboru UDF][options]

3.  Dělat výpočty UDF clusteru, klikněte na buňku s =XllGetComputerNameC() hodnotu a stiskněte klávesu Enter. Funkce jednoduše načte název výpočetní uzly spuštěn souboru UDF. Pro první spuštění dialogové okno přihlašovací údaje zobrazí dotaz na uživatelské jméno a heslo pro připojení k obrázku IaaS.

    ![Spuštění UDF][run]

    Pokud existuje více buněk Chcete-li vypočítat, stiskněte klávesu Alt Shift Ctrl + F9 spustíte výpočet na všechny buňky.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Krok 3. Spuštění pracovního vytížení SOA z místního klienta

Spustit obecné aplikace SOA HPC Pack IaaS clusteru, nejdřív pomocí jedné z metod v kroku 1 nasazení clusteru. Zadejte obecnou výpočet uzel obrázek v tomto případě, protože se nemusí Excel uzlech výpočetním. Pak postupujte podle těchto kroků.

1. Po načtení certifikát clusteru, importujte ho na klientském počítači v rámci certifikátu: \CurrentUser\Root.

2. Nainstalujte [HPC Pack 2012 R2 aktualizace 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) a [klientské nástroje HPC Pack 2012 R2 aktualizace 3](https://www.microsoft.com/download/details.aspx?id=49923). Tyto nástroje umožňují k vytvoření a spuštění SOA klientské aplikace.

3. Stáhněte si HelloWorldR2 [ukázkový kód](https://www.microsoft.com/download/details.aspx?id=41633). Otevřete HelloWorldR2.sln ve Visual Studio 2010 nebo 2012.

4. Nejdřív vytvořte EchoService projektu. Potom nasaďte služby clusteru IaaS stejným způsobem, který nasadíte místní clusteru. Podrobný postup najdete v tématech Readme.doc v HelloWordR2. Úprava a vytvořte HellWorldR2 a jiné projekty popsané v následující části generovat SOA klientské aplikace spuštěné v operačním systému Azure IaaS obrázku.

### <a name="use-http-binding-with-azure-storage-queue"></a>Použít Http vazbu s Azure úložiště fronty

Vazba Http pomocí služby Azure úložiště fronty, měnit několik ukázkový kód.

* Aktualizujte název obrázku.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Volitelně můžete použít výchozí TransportScheme v SessionStartInfo nebo výslovně nastavené na Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Použití výchozí vazby pro BrokerClient.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Nebo explicitně pomocí basicHttpBinding.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Volitelně můžete nastavte příznak UseAzureQueue SessionStartInfo true (pravda). Pokud není nastavena, bude nastavena true ve výchozím nastavení při název clusteru Azure domain přípony a TransportScheme Http.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>Použít Http vazbu bez fronty Azure úložiště

Použití protokolu Http vazby bez fronty Azure úložiště, explicitně nastavte příznak UseAzureQueue false v SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Použít NetTcp vazbu

Používat NetTcp vazbu, konfiguraci je podobný připojení k místní obrázku. Musíte otevřít několik koncové body na uzel hlavou OM. Pokud jste použili k vytvoření clusteru skript pro nasazení HPC Pack IaaS, například vytvořit koncové body v portálu Azure klasické následujícím způsobem.


1. Vypnout OM.

2. Přidejte porty TCP 9090, 9087, 9094 relace, zprostředkovatele, zprostředkovatel pracovníka a datové služby, respektive 9091,

    ![Konfigurace koncové body][endpoint]

3. Začněte OM.

Klientské aplikaci SOA vyžaduje žádné změny kromě změny názvu hlavou IaaS úplný název obrázku.

## <a name="next-steps"></a>Další kroky

* V tématu Další informace o tom, jak Excel úloh s aktualizací Service Pack HPC [tyto materiály](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) .

* Další informace o nasazení a správu služeb SOA s aktualizací Service Pack HPC najdete v článku [Správa služby SOA v Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) .

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png
