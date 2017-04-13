<properties 
    pageTitle="Ochrana serveru SQL Server s obnovení havárie SQL serveru a obnovení webu Azure | Microsoft Azure" 
    description="Tento článek popisuje, jak replikovat serveru SQL Server pomocí funkce havárie Azure obnovení z SQL serveru." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.workload="backup-recovery" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2016" 
    ms.author="raynew"/>


# <a name="protect-sql-server-with-sql-server-disaster-recovery-and-azure-site-recovery"></a>Ochrana serveru SQL Server s obnovení havárie SQL serveru a obnovení webu Azure 


Obnovení webu Azure služby přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Počítačích replikovat Azure nebo vedlejší místního datovém centru. Rychlý přehled číst [Co je obnovení webu Azure?](site-recovery-overview.md).

 Tento článek popisuje, jak chránit serveru SQL Server back-end aplikace pomocí kombinace technologií SQL serveru BCDR a obnovení webu Azure. Byste měli mít dobře porozumět možnosti obnovení havárie SQL Server (převzetí clusterů skupiny dostupnosti AlwaysOn databáze odrážející strukturu, protokolu dodávky) a obnovení webu Azure, před nasazením scénáře popisované v tomto článku.



## <a name="overview"></a>Základní informace

Mnoho úloh použít jako základ SQL Server. Aplikací, jako je SharePoint, Dynamics a SAP umožňuje implementace datové služby SQL Server.  Aplikace nasazení serveru SQL Server několika různými způsoby:

- **Samostatný SQL Server**: SQL Server a všechny databáze jsou hostované na jednom počítači (fyzické nebo virtuální). Když virtualizované, hostitele clusterů se používá pro místní dostupnost. Dostupnost žádné úrovni hosta implementovaná.
- **SQL Server převzetí clusterů instance (vždy na FCI)**: dva nebo více uzlů instance serveru SQL s sdílených disků nejsou nakonfigurováni v systému Windows překlopení obrázku. Pokud některé instancí clusteru je dolů, clusteru můžete převzetí SQL serveru do jiné instance. Toto nastavení se obvykle používá pro HA na primární webu. Není chránit před selhání nebo výpadku ve vrstvě sdílené úložiště. Sdílené disku lze provést pomocí ISCSI optické kanálu nebo sdílené VHDx.
- **SQL vždy na dostupnost skupin**: Toto nastavení dva uzly způsoby nastavení ve sdílené nic cluster s databáze systému SQL Server nakonfigurovaný ve skupině dostupnost synchronní a automatické převzetí.

V verze Enterprise SQL Server také poskytuje nativní havárie obnovení technologie pro obnovení databáze na vzdálený server. V tomto článku se využít jsme integrace s tyto nativní technologie obnovení havárie SQL: 

- SQL vždy na dostupnost skupiny pro obnovení pro SQL Server 2012 nebo verze Enterprise 2014.
- Odrážející strukturu databáze SQL v režimu Vysoký zabezpečení pro SQL Server Standard edition (všechny verze) nebo SQL Server 2008 R2.


Obnovení webu můžete chránit SQL Server popsaná v tabulce.

 |**Místní do místního nasazení** | **Místní na Azure** 
---|---|---
**Hyper-V** | Ano | Ano
**VMware** | Ano | Ano 
**Pole fyzicky serveru** | Ano | Ano


## <a name="support-and-integration"></a>Podpora a integrace

Tyto verze SQL serveru podporovaných scénáře v tomto článku:


- SQL Server 2014 Enterprise a Standard
- SQL Server 2012 Enterprise a Standard
- SQL Server 2008 R2 Enterprise a Standard


Obnovení webu můžete integrovat s nativní technologií SQL serveru BCDR shrnuté v následující tabulce poskytující řešení obnovení havárie.

**Funkce** |**Podrobnosti** | **Verze SQL serveru** 
---|---|---
**Skupiny AlwaysOn dostupnosti** | V překlopení obrázku, který má více uzlů spustit několika instancích samostatného systému SQL Server.<br/><br/>Databáze může seskupené do převzetí skupin, které můžete zkopírovat (zrcadlený) na instance serveru SQL Server tak, že není potřeba žádná sdílené úložiště.<br/><br/>Poskytuje havárie obnovení mezi webem primární a jeden nebo více sekundárním weby. Dva uzly lze nastavit ve sdílené nic obrázku s databáze systému SQL Server nakonfigurovaný ve skupině dostupnost synchronní a automatické převzetí. | SQL Server 2014 & 2012 Enterprise edition
**Převzetí řízení clusterů (AlwaysOn FCI)** | SQL Server využívá Windows převzetí řízení clusterů pro dostupnost pracovního vytížení místního serveru SQL Server.<br/><br/>Uzly spuštěné instance serveru SQL Server s sdílených disků jsou konfigurovat překlopení obrázku. Pokud je instance dolů clusteru selhání na jiný název.<br/><br/>Clusteru nemá chránit před selháním nebo výpadků ve sdíleném úložišti. Sdílený disk lze provést pomocí iSCSI optické kanálu nebo sdílené VHDXs. | Edice systému SQL Server Enterprise<br/><br/>SQL Server Standard edition (omezený na dvou uzlů)
**Databáze odrážející strukturu (vysokou bezpečnost režim)** | Chrání jedné databáze pro jednu vedlejší kopii. K dispozici v obou vysoké zabezpečení (synchronní) a vysoký výkon (asynchronní) replikace. Nevyžaduje překlopení obrázku. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise všech edic
**Samostatný SQL serveru** | SQL Server a databázi jsou hostované na jednom serveru (fyzické nebo virtuální). Host (hostitel) clusterů se používá pro dostupnost, pokud je server virtuální. Žádné úrovni hosta dostupnost. | Pole organizace nebo standardní vydání

## <a name="deployment-recommendations"></a>Doporučení nasazení


Tato tabulka shrnuje Naše doporučení pro integraci technologií pro server SQL Server BCDR s obnovení webu.

**Verze** |**Edition** | **Nasazení** | **Místní na místní** | **Místní na Azure** 
---|---|---|---|---
SQL Server 2012 nebo 2014 | Pole organizace | Překlopení obrázku instanci | Skupiny dostupnosti AlwaysOn | Skupiny dostupnosti AlwaysOn
 | Pole organizace | Skupiny dostupnosti AlwaysOn vysoké dostupnosti | Skupiny dostupnosti AlwaysOn | Skupiny dostupnosti AlwaysOn
 | Standardní | Překlopení obrázku instance (FCI) | Replikace obnovení webů s místní zrcadlové | Replikace obnovení webů s místní zrcadlové
 | Pole organizace nebo Standard | Samostatný | Replikace obnovení webu | Replikace obnovení webu
SQL Server 2008 R2 | Pole organizace nebo Standard | Překlopení obrázku instance (FCI) | Replikace obnovení webů s místní zrcadlové | Replikace obnovení webů s místní zrcadlové
 | Pole organizace nebo Standard | Samostatný | Replikace obnovení webu | Replikace obnovení webu
SQL Server (všechny verze) | Pole organizace nebo Standard | Překlopení obrázku instance - DTC aplikace | Replikace obnovení webu | Nepodporovaná funkce


## <a name="deployment-prerequisites"></a>Zjistit předpoklady pro nasazení

Tady je nutné před spuštění:

- Místní nasazení serveru SQL Server podporovaných verzí systému SQL Server. Obvykle taky musíte Active Directory pro systém SQL server.
- Požadavky pro scénáře, kterou chcete nasadit. Zjistit předpoklady pro najdete v článku každý nasazení. Odkazy na tyto jsou uvedeny v části [Přehled obnovení webu](site-recovery-overview.md).
- Pokud chcete nastavit obnovení v Azure, musíte spustit nástroj [Azure Virtual Machine připravenosti hodnocení](http://www.microsoft.com/download/details.aspx?id=40898) na váš SQL Server virtuálních počítačích aby zkontrolovala, jestli že jsou kompatibilní se službou Azure a obnovení webu.


## <a name="set-up-active-directory"></a>Nastavení služby Active Directory

Musíte mít na vedlejší obnovení webu pro systém SQL Server správnou služby Active Directory. Existuje několik možností:

- **Malé enterprise**– Pokud jste vytvořili malým počtem poštovních aplikací a jednoho řadiče domény u místních webů a chcete převzít celého webu, doporučujeme, abyste replikovat řadiče domény sekundárním datacentra nebo Azure pomocí repication obnovení webu.

- **Středně velké organizace**, pokud máte velký počet aplikace, používáte struktuře služby Active Directory a chcete selhání úplně od začátku aplikace nebo pracovní zátěž, doporučujeme, abyste nastavení dalšího řadiče domény na vedlejší datacentra nebo v Azure. Všimněte si, že pokud používáte skupiny dostupnosti AlwaysOn obnovit vzdálený server doporučujeme nastavit jiné dalšího řadiče domény na vedlejší webu nebo Azure, můžete u obnoveného instanci systému SQL Server.

Pokyny v tomto dokumentu předpokládá, že doménu je dostupná v sekundární umístění. [Přečtěte si další](site-recovery-active-directory.md) informace o ochraně služby Active Directory s obnovení webu.

## <a name="integrate-protection-with-sql-server-always-on-on-premises-to-azure"></a>Ochrana integrovat Always-On SQL serveru (místní na Azure)


Obnovení webu nativně podporuje SQL AlwaysOn. Pokud jste si vytvořili skupiny dostupnosti SQL s Azure virtuální počítač nastavit jako "Vedlejší" obnovení webu můžete použít ke správě převzetí skupiny dostupnosti. 

>[AZURE.NOTE] Tato funkce momentálně v náhledu a k dispozici pro Hyper-V hostitelské servery v primární datacentra je spravováno v VMM mračnech a pokud se spravuje nastavení VMware [Konfigurační Server](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Oprávnění teď tato možnost není k dispozici v novém portálu Azure.

#### <a name="prerequisites"></a>Zjistit předpoklady pro

Tady najdete, co byste měli SQL AlwaysOn integrovat obnovení webu:

- Místní SQL Server (samostatný server nebo překlopení obrázku).
- Nejméně jedna Azure virtuálních počítačích s nainstalovanou SQL Server
- Skupina dostupnost SQL nastavení mezi místního serveru SQL Server a SQL serveru v Azure
- Vzdálený PowerShell lze povolit v počítači místního serveru SQL Server. VMM server nebo konfigurace by měla volat vzdálený PowerShell na serveru SQL Server.
- Uživatelský účet má být přidán na místní SQL serveru, tyto skupiny uživatelů SQL s nejméně tato oprávnění:
    - ZMĚNIT dostupnost skupiny: oprávnění [sem](https://msdn.microsoft.com/library/hh231018.aspx)a [tady](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - Příkaz ALTER databáze – oprávnění[tady](https://msdn.microsoft.com/library/ff877956.aspx#Security)
- Účet RunAs by měl vytvořena na serveru VMM nebo účet by měl být na konfigurační Server pomocí CSPSConfigtool.exe pro uživatele uvedené v předchozím kroku 
- V SQL serverech musí běžet místní a na Azure virtuálních počítačích, měli byste mít nainstalované modulu SQL PS
- Agent OM musí být nainstalované virtuálních počítačích se systémem Azure
- NTAUTHORITY\System by měla být na serveru SQL Server na virtuálních počítačích v Azure následující oprávnění:
    - ZMĚNIT skupiny dostupnosti – oprávnění [tady](https://msdn.microsoft.com/library/hh231018.aspx)a [tady](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - Příkaz ALTER databáze – oprávnění [tady](https://msdn.microsoft.com/library/ff877956.aspx#Security)

####  <a name="step-1-add-a-sql-server"></a>Krok 1: Přidání SQL serveru


1. Klikněte na tlačítko **Přidat SQL** přidáte nové SQL serveru. 

    ![Přidání SQL](./media/site-recovery-sql/add-sql.png)

2. **Konfigurace**nastavení SQL > **název** zadejte popisný název odkázat na serveru SQL Server.
3. **V SQL Server (plně kvalifikovaný název domény)** zadejte plně kvalifikovaný název domény zdroji SQL serveru, který chcete přidat. V případě, že je nainstalovaný SQL Server na překlopení obrázku, zadejte plně kvalifikovaný název domény clusteru a ne všechny uzlů.  
4. Zvolte výchozí instanci v **Instance systému SQL Server** nebo zadejte název vlastní instance.
5. Na **Serveru správy** vyberte VMM server nebo konfigurační Server registrované trezoru obnovení webu. Obnovení webu používá tento server pro správu ke komunikaci se serverem SQL Server.
6. Do pole **Spustit jako účet** zadejte název účtu příkazu RunAs, která byla vytvořená na serveru VMM nebo účet, který byl vytvořen na serveru Configuraaaon. Tento účet se používá pro přístup k SQL serveru a měli oprávnění pro čtení a převzetí na dostupnost skupiny v počítači serveru SQL Server.

    ![Dialogové okno SQL přidat](./media/site-recovery-sql/add-sql-dialog.png)

Po přidání SQL serveru se zobrazí na kartě **Servery SQL** . 

![Seznam serveru SQL](./media/site-recovery-sql/sql-server-list.png)


#### <a name="step-2-add-a-sql-availability-group"></a>Krok 2: Přidání skupiny dostupnosti SQL

1. Po SQL serveru se přidá počítače, že je dalším krokem přidání skupin dostupnost obnovení webu. Aby je dostala, přechod k podrobnostem v SQL Server přidané v předchozím kroku a klikněte na Přidat skupinu dostupnost SQL. 

    ![Přidání SQL AG](./media/site-recovery-sql/add-sqlag.png)

2. Skupina dostupnost SQL můžete replikace na virtuálních počítačích v Azure. Po přidání skupiny dostupnosti sql, který budete vyzváni k zadání jména a předplatné Azure virtuálního počítače místo, kam chcete skupině dostupnost se nepodařilo přes tak, že obnovení webu.

    ![Dialogové okno SQL AG přidat](./media/site-recovery-sql/add-sqlag-dialog.png)

3. V tomto příkladu by se stanou dostupnost skupiny ZR1-AG na virtual počítač SQLAGVM2 uvnitř předplatné DevTesting2 na selhání primární. 

>[AZURE.NOTE] Pouze dostupnost skupiny, do kterých jsou primární na serveru SQL Server přidané v předchozím kroku jsou k dispozici, které budou přidány do obnovení webu. Pokud jste zadali primární skupiny dostupnost na serveru SQL Server nebo pokud jste přidali více skupin dostupnost na serveru SQL Server po bylo přidáno, aktualizujte ji pomocí možnosti aktualizace dostupné na serveru SQL Server.

#### <a name="step-3-create-a-recovery-plan"></a>Krok 3: Vytvoření plánu pro obnovení

Dalším krokem je vytvoření plánu obnovení pomocí virtuálních počítačích a skupiny dostupnosti. Vyberte stejné VMM nebo konfigurace serveru, který jste použili v kroku 1 jako zdroj a Microsoft Azure jako cíl.

![Vytvoření plánu pro obnovení](./media/site-recovery-sql/create-rp1.png)

![Vytvoření plánu pro obnovení](./media/site-recovery-sql/create-rp2.png)

V příkladu aplikace Sharepoint se skládá ze 3 virtuálních počítačích, které používají skupiny dostupnosti SQL jako jeho back-end. V tomto obnovení plánu, že jsme může vybrali obě dostupnost skupiny i virtuální počítač, který tvoří aplikace. 

Obnovení plán můžete upravit přesunutím virtuálních počítačích na různých převzetí skupiny a pořadové pořadí překlopení. Skupiny dostupnosti vždy selhání první jako by být použity jako backendovou libovolné aplikaci. 

![Přizpůsobení obnovení plán](./media/site-recovery-sql/customize-rp.png)

### <a name="step-4--fail-over"></a>Krok 4: Převzetí

Převzetí různé možnosti jsou k dispozici po přidání skupiny dostupnosti obnovení plánovat.

Překlopení | Podrobnosti
--- | ---
**Plánované překlopení** | Plánované převzetí znamená bez ztráty přepojení data. K dosažení danou skupinu dostupnost SQL dostupnost režimu nejdřív nastavena na synchronní a pak se selhání při spuštění aktivují Chcete-li vytvořit dostupnost primární k virtuálního počítače při přidávání skupiny dostupnosti k obnovení webu k dispozici. Po dokončení záložní dostupnost režimu nastavenou stejnou hodnotu bylo platné před plánované převzetí spuštěná.
**Neplánované překlopení** | Do ztrátou dat může způsobit neplánované převzetí. Při aktivaci neplánované převzetí dostupnost druh skupiny dostupnosti nezmění a it je nastavená jako primární k virtuálního počítače při přidávání skupiny dostupnosti k obnovení webu k dispozici. Po dokončení neplánované převzetí a znovu je k dispozici místního serveru SQL Server, Převrátit replikace musí být spouštěný ve skupině dostupnost. Všimněte si, že tato akce není k dispozici v plánu obnovy a může být na skupiny dostupnosti SQL kartě servery SQL
**Převzetí test** | Převzetí test pro skupiny SQL dostupnosti nepodporuje. Pokud aktivace Test převzetí obnovení plánování obsahující skupiny dostupnosti SQL převzetí by přeskočilo dostupnost skupiny.


Zvažte tyto možnosti překlopení.

Možnost | Podrobnosti
--- | ---
**Možnost 1** | 1. proveďte test selhání aplikace a front-end úrovní.<br/><br/>2. Aktualizujte vrstvy aplikace pro přístup k výtisku otevřené v režimu jen pro čtení a otestovat jen pro čtení aplikace.
**Možnost 2** | 1. vytvořit kopii otevřené instanci systému SQL Server virtuálního počítače (pomocí VMM klonovat pro web weby nebo zálohování Azure) a vyvoláte v síti test<br/><br/> 2. proveďte test převzetí pomocí plánu obnovy.

Krok 5: Navrácení

Pokud chcete skupiny dostupnosti zase nastavit primární na místním SQL serveru můžete tak udělejte aktivaci plánované převzetí na obnovení plánování a výběrem směru z Microsoft Azure místního VMM serveru.

>[AZURE.NOTE] Po obráceném replikace neplánované převzetí má při skupiny dostupnosti pokračovat replikace aktivován. Dokud je to replikace zbývá pozastavení.



### <a name="protect-machines-without-a-vmm-server-or-a-configuration-server"></a>Ochrana bez serveru VMM nebo konfigurace serveru

Prostředí, které nejsou spravuje VMM serveru nebo konfigurační Server Azure automatizaci Runbooks mohou sloužit k konfigurace skriptů selhání SQL dostupnost skupin. Tady najdete postup, jak konfigurovat, který:

1.  Vytvoření místní soubor skriptu překlopení skupiny dostupnosti. Tento ukázkový skript Určuje cestu ke skupině dostupnost na Azure otevřené a selhání instanci otevřené. Tento skript se spustí na serveru SQL Server otevřené virtual machine předáním je s příponou vlastní skript.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2.  Nahrajte skript objektů blob Azure úložiště účtu. Použijte v tomto příkladu:

        $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key"
        Set-AzureStorageBlobContent -Blob "AGFailover.ps1" -Container "script-container" -File "ScriptLocalFilePath" -context $context

3.  Vytvoření Azure automatizaci postupu runbook vyvolat skripty na serveru SQL Server otevřené virtuálního počítače v Azure. Můžete to udělat pomocí tohoto skriptu vzorku. [Další informace](site-recovery-runbook-automation.md) o použití runbooks automatické obnovení plány. 

        workflow SQLAvailabilityGroupFailover
        {
            param (
                [Object]$RecoveryPlanContext
            )

            $Cred = Get-AutomationPSCredential -name 'AzureCredential'
    
            #Connect to Azure
            $AzureAccount = Add-AzureAccount -Credential $Cred
            $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
            Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    
            InLineScript
            {
            #Update the script with name of your storage account, key and blob name
            $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key";
            $sasuri = New-AzureStorageBlobSASToken -Container "script-container"- Blob "AGFailover.ps1" -Permission r -FullUri -Context $context;
     
            Write-output "failovertype " + $Using:RecoveryPlanContext.FailoverType;
               
            if ($Using:RecoveryPlanContext.FailoverType -eq "Test")
                {
                #Skipping TFO in this version.
                #We will update the script in a follow-up post with TFO support
                Write-output "tfo: Skipping SQL Failover";
                }
            else
                {
                Write-output "pfo/ufo";
                #Get the SQL Azure Replica VM.
                #Update the script to use the name of your VM and Cloud Service
                $VM = Get-AzureVM -Name "SQLAzureVM" -ServiceName "SQLAzureReplica";     
       
                Write-Output "Installing custom script extension"
                #Install the Custom Script Extension on teh SQL Replica VM
                Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $VM -Publisher Microsoft.Compute -Version 1.3| Update-AzureVM; 
                    
                Write-output "Starting AG Failover";
                #Execute the SQL Failover script
                #Pass the SQL AG path as the argument.
       
                $AGArgs="-SQLAvailabilityGroupPath sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag";
       
                Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument $AGArgs | Update-AzureVM;
       
                Write-output "Completed AG Failover";

                }
        
            }
        }

4.  Při vytváření plánu pro obnovení aplikace přidáte "předem seskupení 1 spouštěcí" skriptů krok, který vyvolá postupu runbook automatizaci překlopení dostupnost skupiny.

## <a name="integrate-protection-with-sql-alwayson-on-premises-to-on-premises"></a>Ochrana integrovat AlwaysOn SQL (místní do místního)

Pokud SQL Server používá skupiny dostupnost pro dostupnost nebo instanci překlopení obrázku, doporučujeme používat skupiny dostupnosti portálu obnovení. Všimněte si, že tyto pokyny pro aplikace, které nepoužívají distributed transakce.

1. [Konfigurace databáze](https://msdn.microsoft.com/library/hh213078.aspx) do skupiny dostupnosti.
2. Vytvořte novou síť virtuální na vedlejší webu.
3. Nastavení webu na webu VPN mezi novou virtuální síť a primární webu.
4. Vytvoření virtuálního počítače na využití webu a na něj nainstalovat SQL Server.
5. Rozšiřte existující skupiny dostupnosti AlwaysOn na novém počítači virtuálního serveru SQL Server. Konfigurace této instance serveru SQL Server jako kopie asynchronní otevřené.
6. Vytvořit skupiny posluchače dostupnost nebo aktualizovat existující posluchače zahrnout asynchronní otevřené virtuálního počítače.
7. Ujistěte se, že farmy aplikace je instalace pomocí posluchače. Je-li nastavení použít název serveru databáze, aktualizujte ji používat posluchače, abyste nemuseli překonfigurovat po záložní.

Pro aplikace, které využívají distribuované transakce jsme doporučení používáte [Obnovení webu s opakováním SAN](site-recovery-vmm-san.md) nebo [replikace VMWare/fyzické serveru na webu](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Obnovení plán co byste měli zvážit

1. Přidání tento ukázkový skript ke knihovně VMM na webech primárních a sekundárních.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2. Při vytváření plánu pro obnovení aplikace přidáte "předem seskupení 1 spouštěcí" skriptů krok, který vyvolá skript překlopení dostupnost skupiny.



## <a name="protect-a-standalone-sql-server"></a>Ochrana samostatně SQL serveru

V této konfiguraci doporučujeme že použít obnovení webu replikace k ochraně počítače SQL serveru. Přesný postup závisí zda SQL Server je nastaven jako virtuálního počítače nebo pole fyzicky serveru a jestli chcete replikovat Azure nebo vedlejší místního serveru. Pokyny pro všechny scénáře nasazení vstoupit [Přehled obnovení webu](site-recovery-overview.md).


## <a name="protect-a-sql-server-cluster-standard-or-2008-r2"></a>Ochrana clusteru serveru SQL Server (standardní nebo 2008 R2)

Systém SQL Server Standard edition nebo SQL Server 2008 R2 clusteru doporučujeme že obnovení webu replikace umožňuje chránit SQL serveru.

### <a name="on-premises-to-on-premises"></a>Místní do místního nasazení

- Pokud aplikace používá distribuované transakce doporučujeme že nasazení [Obnovení webu s opakováním SAN](site-recovery-vmm-san.md) pro Hyper-V prostředí a [VMware/fyzické serveru VMware](site-recovery-vmware-to-vmware.md) VMware prostředí.

- Pro aplikace není DTC využijte výše uvedený přístup k obnovení clusteru jako samostatný server využitím místního vysokou bezpečnost DB zrcadlové.

### <a name="on-premises-to-azure"></a>Místní na Azure

Obnovení webu, které nejsou podporovány hosta clusteru podpory při replikace Azure. SQL Server také neposkytuje řešení nízké náklady havárie obnovení pro standardní vydání. Doporučujeme chránit místního serveru SQL Server obrázku do samostatného SQL serveru a obnovit v Azure.


1. Konfigurace instance serveru SQL Server Další samostatného na webu místní.
2. Konfigurace této instance sloužit jako zrcadlové pro databáze, které potřebujete ochranu. Konfigurace odrážející strukturu v režimu Vysoký zabezpečení.
3.  Konfigurace obnovení webu na webu místního prostředí ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) nebo [VMware/fyzické server](site-recovery-vmware-to-azure-classic.md).
4.  Obnovení webu replikace umožňuje replikovat nové instance serveru SQL Azure. Kopii zrcadlové vysoké zabezpečení a to bude se synchronizovat s primární obrázku, ale budete replikovat na Azure pomocí služby replikace obnovení webu.

Následující obrázek ukazuje toto nastavení.

![Standardní obrázku](./media/site-recovery-sql/BCDRStandaloneClusterLocal.png)


### <a name="failback-considerations"></a>Co byste měli zvážit navrácení

Standardní clusterů SQL navrácení po neplánované převzetí vyžadují zálohy SQL a obnovit z instance zrcadlové původní obrázku a obnovením zrcadlové.

## <a name="next-steps"></a>Další kroky
[Další informace](site-recovery-best-practices.md) o přípravu nasazení obnovení webu.










 