<properties
    pageTitle="Skript vývoj akce s HDInsight | Microsoft Azure"
    description="Zjistěte, jak přizpůsobit Hadoop clusterů pomocí skriptu akce. Akci skriptu mohou sloužit k instalaci další software výpočetnímu clusteru Hadoop nebo změnit konfiguraci aplikace nainstalované clusteru."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Vytvořit skript akce skripty pro serveru s HDInsight Windows clusterů

Naučte se skripty akci skriptu pro HDInsight. Informace o použití akci skriptu skripty najdete v tématu [clusterů přizpůsobení HDInsight pomocí skriptu akce](hdinsight-hadoop-customize-cluster.md). Stejné článek napsané pro clusterů na základě Linux Hdinsightu najdete v článku [skriptů vyvíjet akci skriptu pro HDInsight](hdinsight-hadoop-script-actions-linux.md).

> [AZURE.NOTE] Informace v tomto dokumentu jsou specifické pro clusterů serveru s Windows HDInsight. Informace o použití akce skriptu se systémem Windows clusterů najdete v tématu [vývoj akce skriptů s HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).


Akci skriptu mohou sloužit k instalaci další software výpočetnímu clusteru Hadoop nebo změnit konfiguraci aplikace nainstalované clusteru. Skript akce jsou spuštěné v operačním systému uzlů při zavedení HDInsight clusterů skripty a jsou prováděna po uzlů v clusteru dokončete konfiguraci HDInsight. Skript akce se spustí v části oprávnění účtu správce systému a nabízí plný přístup práva uzlů. Každý cluster lze zadat se seznamem skript akcí má provádět v pořadí, ve které byla vyplněná.

> [AZURE.NOTE] Pokud se objevit tato chybová zpráva:
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> Je to proto zahrnete neměli pomocné metody.  V části [metody Pomocníka pro vlastních skriptů](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).

## <a name="sample-scripts"></a>Ukázky skriptů

Vytvoření HDInsight clusterů v operačním systému Windows, je akci skript skript Powershellu Azure. Následuje ukázka skriptu pro konfigurace soubor konfigurace webu:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Skript má čtyři parametry, název souboru konfigurace, vlastnost, kterou chcete upravit, hodnotu, kterou chcete nastavit a popis. Příklad:

    hive-site.xml hive.metastore.client.socket.timeout 90

Tyto parametry nastaví hodnotu hive.metastore.client.socket.timeout 90 v souboru podregistru site.xml.  Výchozí hodnota je 60 sekund.

Tento ukázkový skript najdete taky v [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

HDInsight obsahuje několik skriptů nainstalovat dodatečný HDInsight clusterů:

Jméno | Skriptem
----- | -----
**Instalace Spark** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. V tématu [instalace a použití podnítit na HDInsight clusterů][hdinsight-install-spark].
**Instalace R** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Přečtěte si téma [instalace a používání R na HDInsight clusterů][hdinsight-r-scripts].
**Instalace Solr** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. V tématu [instalace a použití clusterů Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- **Instalace Giraph** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. V tématu [instalace a použití clusterů Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).

Skript akce můžete nasadit z portálu Microsoft Azure, Azure PowerShell nebo pomocí HDInsight .NET SDK.  Další informace najdete v tématu [přizpůsobení HDInsight clusterů pomocí skriptu akce][hdinsight-cluster-customize].

> [AZURE.NOTE] Ukázky skriptů pracovat jenom s HDInsight clusteru podverzí 3.1 nebo vyšší. Další informace o verzích clusteru Hdinsightu najdete v článku [verze obrázku HDInsight](hdinsight-component-versioning.md).





## <a name="helper-methods-for-custom-scripts"></a>Pomocné metody pro vlastních skriptů

Skript akce pomocné metody jsou nástroje, které můžete použít při vytváření vlastních skriptů. Tyto se definují [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)a mohou být součástí skriptů pomocí následujících akcí:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Tady jsou pomocné metody, které jsou k dispozici tak, že tento skript:

Metoda Pomocník | Popis
-------------- | -----------
**Uložit HDIFile** | Stáhněte si soubor z zadaný identifikátor URI (Uniform Resource) do umístění na místním disku, který souvisí s uzel Azure OM přiřazená clusteru.
**Rozbalení HDIZippedFile** | Rozbalení zip souboru.
**Vyvolání HDICmdScript** | Spusťte skript z cmd.exe.
**Zápis HDILog** | Napište výstup z vlastní skript pro akci skriptu.
**Get služby** | Dostaňte seznam služeb spuštěných na počítači, kde skript spustí.
**Get-Service** | S názvem konkrétní službu předávat na vstupu najdete podrobné informace pro konkrétní službu (název služby, zpracuje ID, stav, atd.) v počítači, kde se skript spustí.
**Get-HDIServices** | Dostaňte seznam služeb HDInsight spuštěné v počítači, kde se skript spustí.
**Get-HDIService** | S konkrétní HDInsight název služby předávat na vstupu, najdete podrobné informace pro konkrétní službu (název služby, zpracuje ID, stav, atd.) v počítači, kde se skript spustí.
**Get-ServicesRunning** | Zobrazte seznam služeb spuštěných na počítači místo, kam se skript spustí.
**Get-ServiceRunning** | Zkontrolujte, jestli konkrétní službu (podle názvu) je spuštěný v počítači, kde se skript spustí.
**Get-HDIServicesRunning** | Dostaňte seznam služeb HDInsight spuštěné v počítači, kde se skript spustí.
**Get-HDIServiceRunning** | Zkontrolujte, jestli konkrétní službu HDInsight (podle názvu) je spuštěný v počítači, kde se skript spustí.
**Get-HDIHadoopVersion** | Pokud potřebujete verzi Hadoop nainstalované v počítači, kde se skript spustí.
**Test-IsHDIHeadNode** | Ověřte, zda počítač, kde se skript spustí hlavního uzlu.
**Test-IsActiveHDIHeadNode** | Ověřte, zda počítač, kde se skript spustí aktivní hlavy uzel.
**Test-IsHDIDataNode** | Ověřte, zda počítač, kde se skript spustí uzel data.
**Upravit HDIConfigFile** | Konfigurace soubory podregistru site.xml, základní site.xml, hdfs site.xml, mapred site.xml nebo vláken site.xml upravte.


## <a name="best-practices-for-script-development"></a>Doporučené postupy pro vývoj skriptů

Při vytváření vlastní skript pro HDInsight clusteru jsou mít na paměti několik doporučené postupy:

- Vyhledat Hadoop verze

    Pouze HDInsight podverzí 3.1 (Hadoop 2.4) a nad podpora pomocí skriptu akce nainstalovat vlastní součásti clusteru. V části vlastní skript musíte použít metodu pomocníka **Get-HDIHadoopVersion** Kontrola verze Hadoop před pokračováním dalších úkolů ve skriptu.


- Stabilní odkazy na prostředky skriptu

    Uživatelé měli všechny tyto skripty a jiných artefakty při přizpůsobení clusteru zůstávají k dispozici po celou dobu clusteru a zkontrolujte, že verze tyto soubory se nemění po celou dobu. Pokud znovu pro zpracování obrázků uzlů v clusteru je vyžadováno jsou potřeba tyto materiály. Doporučený postup je stáhnout a archivovány všechny položky v úložiště účet, který určuje uživatele. To je výchozí úložiště účet nebo na jakékoli další úložiště účty zadané v době nasazení pro vlastní obrázku.
    V poli Spark a R přizpůsobený clusteru vzorky za předpokladu, že v dokumentaci, například jsme provedli místní kopii zdroje v tomto účtu úložiště: https://hdiconfigactions.blob.core.windows.net/.


- Zajistěte, aby byl clusteru přizpůsobení skriptu idempotent

    Musíte očekávat, že uzly clusteru HDInsight budou znovu s nainstalovanou bitovou kopií během životnost obrázku. Přizpůsobení skript clusteru je spuštění pokaždé, když clusteru je znovu s nainstalovanou bitovou kopií. Tento skript musí sloužit jako idempotent znamená, že po znovu pro zpracování obrázků, skript zajistila vrácení clusteru do stejné přizpůsobený stav, který byl ihned po skript spuštěna poprvé původně vytvoření clusteru. Například nainstalovaného vlastní skript aplikace v D:\AppLocation na jeho první spuštění a pak na každý další spustit, při opětovném pro zpracování obrázků, skript zaškrtněte zda aplikace v D:\AppLocation umístění existuje před pokračováním další kroky v skript.


- Nainstalovat vlastní součásti optimální umístění

    Po opětovném s nainstalovanou bitovou kopií uzlech C:\ zdroje jednotky a D:\ systémovou jednotku lze znovu formátovaný, výsledkem ztráty dat a aplikací nainstalovaných na tyto jednotky. Tato situace může nastat rovněž Jestliže uzel služby Azure virtuálního počítače (OM), která je součástí clusteru havaruje a nahrazuje nový uzel. Součásti můžete nainstalovat na disku D:\ nebo ve C:\apps umístění na clusteru. Všechny místa na disku C:\ rezervovány. Zadejte umístění, kde aplikace nebo knihoven mají mít nainstalovanou na skript úprav obrázku.


- Zajištění dostupnost architektury obrázku

    HDInsight má aktivní pasivní Architektura vysoké dostupnosti, ve kterém, který používá hlavní uzel aktivní (pokud jsou spuštěné služby HDInsight) a jiných hlavy uzel reprodukujte v úsporném režimu (které HDInsight nejsou spuštěny služby). Uzly přepnout aktivní a pasivní režimy-li k přerušení služby HDInsight. Pokud akci skriptu se používá k instalaci služby v obou hlavy uzlech vysoké dostupnosti, mějte na paměti, že mechanismus přepnutí HDInsight nebude moct automaticky převzít tyto uživatele instalované služby. Tak uživatele instalované služby uzlech hlavy HDInsight, které očekává vysoce možné musíte mít vlastní mechanismus přepnutí-li v režimu aktivní pasivní nebo v režimu aktivní.

    Při roli Vedoucí uzel zadaný jako hodnota v parametru *ClusterRoleCollection* v obou hlavy uzlech spustí příkaz akci skriptu HDInsight. Při návrhu vlastní skript proto přesvědčte, že skript známa toto nastavení. Neměli dostanete do potíží stejné služby jsou nainstalovány a pracovat na obou hlavy uzlů kde jsou skončily soutěží navzájem. Také mějte na paměti, že data budou ztracena během znovu pro zpracování obrázků, tak softwaru nainstalovaného prostřednictvím skriptu akce musí být pružné pro tyto události. Aplikace by měl navržený pro práci s vysokou dostupnost data, která je distribuovaná pomocí mnoho uzlů. Všimněte si, že až 1/5 uzlů v clusteru může být znovu s obrázky ve stejnou dobu.


- Konfigurace vlastních součástí používat úložiště objektů Blob Azure

    Vlastní součásti instalované na uzlů pravděpodobně výchozí konfigurace používat úložiště Hadoop Distributed soubor systému (HDFS). Změníte nastavením můžete úložiště objektů Blob Azure. Na obrázku znovu obrázek dostane formátované systému souborů HDFS a ztratí všechna data, která se uloží tam. Použití úložiště objektů Blob Azure místo toho zajistí, že vaše data se zachovají.

## <a name="common-usage-patterns"></a>Společné použití vzorky

Tato část obsahuje pokyny na některé běžné vzorce použití, které můžou nastat při psaní vlastní skript implementace.

### <a name="configure-environment-variables"></a>Konfigurace proměnné

Často ve vývoji akce skript, bude domníváte potřebné k nastavení proměnné. Například s největší pravděpodobností situace při stažení binární z externího webu, nainstalujte si ho na clusteru a přidat umístění místo, kam se instaluje do vašeho prostředí "path". Následující ukázce se dozvíte, jak nastavit proměnné v části vlastní skript.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Tento příkaz nastaví proměnnou prostředí **MDS_RUNNER_CUSTOM_CLUSTER** hodnotu "true" a také nastaví obor tuto proměnnou být celého systému. V některých případech je důležité, že proměnné se nastavují v příslušné oboru – počítače nebo uživatele. Podívejte se [sem] [ 1] Další informace o nastavení proměnné.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Přístup k místa, kde jsou uloženy vlastních skriptů

Skripty slouží k přizpůsobení clusteru musí buď lze výchozí účet úložiště pro clusteru nebo veřejných jen pro čtení kontejneru na jiný účet úložiště. Pokud skript odkazuje na prostředky umístěné jinde tyto musí být v veřejně přístupný (minimálně veřejné jen pro čtení). Můžete například získat přístup k souboru a uložte jej pomocí příkazu SaveFile HDI.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

V tomto příkladu musíte se ujistit, že veřejně přístupný kontejneru "somecontainer" úložiště účtu "somestorageaccount". Skript jinak, budou výjimku "Nebyl nalezen" a nezdaří.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Předat rutinu přidat AzureRmHDInsightScriptAction parametry

K předání rutinu AzureRmHDInsightScriptAction přidat více parametrů, budete muset formátování hodnotu řetězce má být všechny parametry skriptu. Příklad:

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

nebo

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Výjimku pro nasazení nezdařeném uložení obrázku

Pokud chcete přesně oznámení o to, že clusteru přizpůsobení se nezdařila podle očekávání, je důležité výjimku a nepovede vytváření clusteru. Můžete například zpracování souboru, pokud existuje a obsloužení chyb případ, kde soubor neexistuje. By se zajistilo, že skript řádně zavře a znovu správně známé stav clusteru. Následující úryvek uveden příklad, jak dosáhnout:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

V této fragment kódu Pokud soubor neexistuje, ho povede k stavu skript skutečně řádně ukončí po tisku se chybová zpráva, kde clusteru dosáhne spuštěna za předpokladu, že ho "" úspěšně proces úprav obrázku. Pokud chcete přesně oznámení o to, že v podstatě clusteru přizpůsobení se nezdařila podle očekávání, protože chybí soubor, vhodnější výjimku a nepovede krok úprav obrázku. Docílit musíte použít následující ukázkové fragment kódu.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Kontrolní seznam pro nasazení akci skriptu
Tady je postup, trvání jsme přípravu nasazení tyto skripty:

1. Umístění souborů, které obsahují vlastních skriptů na místě, přístupný pomocí uzlů během nasazení. Může to být v kterémkoliv výchozích nebo další úložiště účty zadané v době nasazení obrázku nebo jiné kontejner veřejně přístupný úložiště.
2. Přidání kontroly do skripty a ujistěte se, že provádějí idempotently, tak, aby skript jde spouštět tisknutím ve stejném uzlu.
3. Tisk STDOUT, jakož i STDERR získáte pomocí rutiny prostředí PowerShell Azure **Zápisu výstupu** . Nepoužívejte **Zápisu hostitele**.
4. Použití složky dočasný soubor, například $env: TEMP, když chcete zachovat stažený soubor používá skripty a potom vyčistit je po provedení mít skriptů.
5. Vlastní instalace pouze na D:\ nebo C:\apps. Jinam na jednotku C: neměla používat, jako jsou rezervovány. Všimněte si, že instalace souborů na jednotku C: mimo složku C:\apps může způsobit chyby instalačního programu během znovu obrázky uzlu.
6. V případě, že byly změněny nastavení na úrovni OS nebo souborů Hadoop služby konfigurace, je vhodné restartování HDInsight služby, které můžete vybrat všechny úrovně s operačním systémem nastavení, například proměnné nastavení v skripty.

## <a name="debug-custom-scripts"></a>Ladění vlastních skriptů

Protokoly chyb skriptem jsou uloženy spolu s výstup ve výchozí účet úložiště, který jste určili jako obrázku v jeho vytvoření. Protokoly jsou uložené v tabulce s názvem *u < \cluster-name-fragment >< \time-stamp > setuplog*. Toto jsou souhrnné protokolů, které mají obsahující záznamy ze všech uzlů (hlavního uzlu a pracovní uzly) na kterých běží skript clusteru.
Snadný způsob, jak v protokolech je používat nástroje HDInsight for Visual Studio. Instalace nástrojů, najdete v článku [Začínáme s používáním aplikace Visual Studio Hadoop nástroje pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio)

**Chcete-li zkontrolovat protokol pomocí aplikace Visual Studio**

1. Otevřete aplikaci Visual Studio.
2. Klikněte na kartu **zobrazení**a potom klikněte na položku **Průzkumník serveru**.
3. Klikněte pravým tlačítkem "Azure", klikněte na připojit k **Microsoft Azure předplatného**a potom zadejte svoje přihlašovací údaje.
4. Rozšíření **úložiště**rozbalte účet Azure úložiště použít jako výchozí systém souborů, rozbalení **tabulky**a potom poklikejte na název tabulky.


Můžete taky vzdálené do uzlech zobrazíte obou STDOUT a STDERR vlastních skriptů. Protokoly v jednotlivých uzlech specifické pouze pro tento uzel a jsou přihlášeni **C:\HDInsightLogs\DeploymentAgent.log**. Tyto soubory protokolu zaznamenat všechny výstupy z vlastní skript. Fragment protokolu příklad pro akci skriptu Spark vypadat takto:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


V tomto protokolu je jasné akci skriptu Spark proběhlo na OM s názvem HEADNODE0 a, aby byly během provádění vyvolání žádné výjimky.

V případě, že dojde k chybě spuštění, výstupu popisující ho taky obsažené v tento soubor protokolu. Informace uvedené v těchto protokoly mohou být užitečné ladění skriptu problémy, které můžou nastat.


## <a name="see-also"></a>Viz taky

- [Přizpůsobení clusterů HDInsight pomocí skriptu akce][hdinsight-cluster-customize]
- [Instalace a používání Spark v HDInsight clusterů][hdinsight-install-spark]
- [Instalace a používání R v HDInsight clusterů][hdinsight-r-scripts]
- [Instalace a použití clusterů Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- [Instalace a použití clusterů Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
