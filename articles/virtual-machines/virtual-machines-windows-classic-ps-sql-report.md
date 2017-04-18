<properties 
    pageTitle="Použití Powershellu ke vytvoření virtuálního počítače pomocí serveru sestav nativním režimu | Microsoft Azure"
    description="Toto téma popisuje a provede vás nasazení a konfiguraci serveru sestav nativním režimu služby SQL Server Reporting Services do Azure virtuálního počítače. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management"/>
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>Použití Powershellu ke vytvoření Azure OM pomocí serveru sestav nativním režimu

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Toto téma popisuje a provede vás nasazení a konfiguraci serveru sestav nativním režimu služby SQL Server Reporting Services do Azure virtuálního počítače. Kroky v tomto dokumentu pomocí kombinace ruční vytvořit virtuální počítač a skriptu prostředí Windows PowerShell pro konfiguraci služby Reporting Services OM. Konfigurace skript zahrnuje otevření portů brány firewall pro HTTP nebo HTTPs.

>[AZURE.NOTE] Pokud není nutné zadávat **HTTPS** na serveru sestav, **přeskočte krok 2**.
>
>Po vytvoření OM v kroku 1, přejděte k části používat skript konfigurace serveru sestav a HTTP. Po spuštění skript je připraven k použití serveru sestav.

## <a name="prerequisites-and-assumptions"></a>Předpoklady a předpoklady

- **Azure předplatné**: ověření počet jádra k dispozici ve vašem předplatném Azure. Pokud vytvoříte doporučenou OM velikost **A3**, musíte k dispozici jádra **4** . Pokud používáte OM velikost **buňky a2**, musíte k dispozici jádra **2** .
    
    - Ověření limit core předplatného na portálu Azure klasické klikněte na nastavení v levém podokně a potom klikněte na použití v nabídce začátek.
    
    - Zvýšit kvóty základní, kontaktujte [Podporu Azure](https://azure.microsoft.com/support/options/). Informace o velikosti OM najdete v článku [Virtuálního počítače velikosti Azure](virtual-machines-linux-sizes.md).

- **Skriptů Windows Powershellu**: tématu předpokládá, že máte základní pracovní znalost prostředí Windows PowerShell. Další informace o používání Windows Powershellu najdete v těchto článcích:

    - [Spuštění prostředí Windows PowerShell v systému Windows Server](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [Začínáme používat Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Krok 1: Zřízení Azure virtuálního počítače

1. Přejděte na portál Azure klasické.

1. V levém podokně klikněte na **virtuálních počítačích** .

    ![virtuálních počítačích Microsoft azure](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)

1. Klikněte na **Nový**.

    ![tlačítko Nový](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)

1. Klikněte na **z Galerie**.

    ![nové OM z Galerie](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)

1. Klikněte na **SQL Server 2014 RTM Standard – Windows serveru 2012 R2** a potom klikněte na šipku na pokračovat.

    ![Další](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

    Pokud budete potřebovat data služby Reporting Services řízený úsilím funkce předplatná, zvolte **SQL Server 2014 RTM Enterprise – Windows serveru 2012 R2**. Další informace o podpoře funkcí a edice systému SQL Server najdete v článku [Funkce nepodporuje edicemi systému SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).

1. Na stránce **Konfigurace virtuálního počítače** upravte následující pole:
                                    
    - Pokud existuje více než jednu **verzi datum**, vyberte nejnovější verzi.
    
    - **Virtuální počítač název**: název počítače se používá taky na další stránce konfigurace jako výchozí název cloudové služby DNS. Název DNS musí být jedinečný přes službu Azure. Zvažte možnost konfigurace OM s vybraným názvem počítače, který popisuje OM k čemu slouží. Například ssrsnativecloud.
    
    - **Vrstvy**: standardní
    
    - **Velikost: A3** je doporučená velikost OM úloh systému SQL Server. Pokud virtuálního počítače se používá pouze jako serveru sestav, stačí OM velikost buňky a2 Pokud dojde k serveru sestav velké úlohu. OM ceny informace v tématu [Ceny virtuálních počítačích](https://azure.microsoft.com/pricing/details/virtual-machines/).
    
    - **Nové uživatelské jméno**: jméno poskytnete se vytvoří jako správce OM.
    
    - **Nové heslo** a **potvrďte**. Toto heslo se používá pro nový účet správce a doporučujeme že používat silné heslo.
    
    - Klikněte na tlačítko **Další**. ![Další](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Na další stránce upravte následující pole:

    - **Cloudové služby**: Vyberte možnost **vytvořit nový cloudové služby**.
    
    - **Název DNS služby cloudu**: Toto je název veřejné DNS cloudové služby, který souvisí s OM. Výchozí název je název, který jste zadali v názvu OM. Pokud na pozdější postup v tématu Vytvoření důvěryhodného certifikát SSL a pak název DNS použít hodnotu "**tak**" certifikátu.
    
    - **Oblast/spřažení skupiny/virtuální sítě**: Zvolte požadovanou oblast nejblíže k svým koncovým uživatelům.
    
    - **Úložiště účtu**: použití účtu automaticky generované úložiště.
    
    - **Nastavte dostupnost**: žádný.
    
    - **Koncové body** Zachovat koncové body **Vzdálená plocha** a **prostředí PowerShell** a zadejte HTTP nebo HTTPS koncového bodu v závislosti na vašem prostředí.

        - **Nastavit informace HTTP**: jsou výchozí veřejných a privátních porty **80**. Všimněte si, že pokud používáte soukromé port než 80, upravit **$HTTPport = 80** skriptu http.

        - **Protokol HTTPS**: výchozí veřejných a privátních porty jsou **443**. Z bezpečnostních důvodů je změnit soukromý port a konfigurace brány firewall a používat soukromé port serveru sestav. Další informace o koncové body najdete v článku [jak nastavení komunikace s virtuálního počítače](virtual-machines-windows-classic-setup-endpoints.md). Všimněte si, že používáte port než 443, změňte parametr **$HTTPsport = 443** skriptu HTTPS.
    
    - Klikněte na další. ![Další](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Na poslední stránce průvodce zachovat výchozí **Instalace agenta OM** vybrané. Kroky v tomto tématu využít agenta OM, ale pokud chcete zachovat tento OM, OM agent a rozšíření vám umožní vylepšit tahle CM.  Další informace o agenta OM najdete v článku [OM Agent a rozšíření – část 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Jednou z ad výchozí přípony nainstalovaný systém je "BGINFO" rozšíření, která se zobrazí na ploše OM, informace o systému třeba interní IP a volného místa na disku.

1. Klikněte na dokončení. ![Ok](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)

1. **Stav** OM zobrazí jako **Počáteční (Provisioning)** během procesu poskytování a zobrazí jako **spouštěnou** po OM zřizování a je připraven k použití.

## <a name="step-2-create-a-server-certificate"></a>Krok 2: Vytvoření certifikát serveru

>[AZURE.NOTE] Pokud nepotřebujete HTTPS na serveru sestav, můžete to udělat **přeskočte krok 2** a přejděte k části **používat skript konfigurace serveru sestav a HTTP**. Pomocí skriptu HTTP rychle konfigurace serveru sestav a serveru sestav bude připravená k použití.

Abyste mohli používat HTTPS v OM, musíte důvěryhodný certifikát SSL. V závislosti na nefunguje použijte jeden z následujících dvou způsobů:

- Platný certifikát SSL Vystavitel od certifikační autority (CA) a důvěryhodné společnosti Microsoft. Certifikáty kořenové CA podporují rozdělí prostřednictvím programu Microsoft Root Certificate Program. Další informace o tomto programu najdete v tématu [Windows a Windows Phone 8 SSL Root Certificate Program (CA člena)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) a [Úvod k programu Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).

- Certifikát podepsaný svým držitelem. Podepsaný certifikáty nedoporučujeme provozním prostředí.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Chcete-li použít certifikát vytvořený tak, že důvěryhodné certifikační autorita (CA)

1. **Žádost o certifikát serveru pro web od certifikační autority**. 

    Průvodce certifikátem webového serveru můžete použít ke generování žádosti o soubor certifikátu (Certreq.txt), které posíláte certifikační autority nebo k vygenerování žádosti o online certifikační autorita. Například certifikační služby společnosti Microsoft ve Windows serveru 2012. V závislosti na úrovni identifikace assurance zaměstnanecké certifikát serveru, které je několik dnů na několik měsíců certifikační autority na schválit žádost a pošlete soubor certifikátu. 

    Další informace o vyžádání certifikátů serveru najdete v těchto článcích: 
    
    - Použití [Certreq](https://technet.microsoft.com/library/cc725793.aspx) [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
    
    - Zabezpečení nástroje pro správu systému Windows Server 2012.

    [Zabezpečení nástroje pro správu systému Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE] Pole **vydat** důvěryhodných certifikát SSL by měl být stejný jako **Název DNS služby cloudu** používá pro nové OM.

1. **Nainstalujte certifikát serveru na webový server**. Vytvoření webu na pozdější kroky při konfiguraci služby Reporting Services webového serveru v tomto případě je OM, který je hostitelem serveru sestav. Další informace o instalaci certifikát serveru na webový server pomocí modulu snap-in konzoly MMC certifikát najdete v tématu [instalace certifikát serveru](https://technet.microsoft.com/library/cc740068).
    
    Pokud chcete pomocí skriptu zahrnutý v tomto tématu, konfigurace serveru sestav přínosu certifikáty **Miniatura** je potřeba parametr skriptu. Naleznete v části Další informace o tom, jak získat Miniatura certifikátu.

1. Přiřaďte certifikát serveru serveru sestav. Přiřazení dokončení v následující části při konfiguraci serveru sestav.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Chcete-li použít certifikát podepsaný svým držitelem virtuálních počítačích

Certifikát podepsaný svým držitelem byla vytvořená OM při zřizování OM. Certifikát má stejný název jako název OM DNS. A zabránit tak chybám certifikátu, je potřeba, aby certifikát je důvěryhodná na OM sebe sama a taky tak, že všichni uživatelé webu.

1. S informacemi o důvěryhodnosti kořenové CA certifikátu na místní OM, přidáte certifikát **Důvěryhodné kořenové certifikační autority**. Následuje přehled potřebných krocích. Podrobnosti o tom, jak důvěřovat certifikační Autority najdete v tématu [instalace certifikát serveru](https://technet.microsoft.com/library/cc740068).

    1. Z portálu Microsoft Azure klasické vyberte OM a klikněte na připojit. V závislosti na nastavení prohlížeče můžete být vyzváni k uložení souboru RDP připojit se bude v angličtině.
    
        ![připojení k azure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Použijte OM uživatelské jméno, uživatelské jméno a heslo, které jste nakonfigurovali, když jste vytvořili OM. 
    
        Na následujícím obrázku, například název OM je **ssrsnativecloud** a uživatelské jméno je **testuser**.
        
        ![přihlašovací jméno obsahuje OM](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
    
    1. Spusťte mmc.exe. Další informace najdete v tématu [jak: zobrazení certifikátů s modulu Snap-in konzoly MMC](https://msdn.microsoft.com/library/ms788967.aspx).
    
    1. V nabídce **soubor** konzoly aplikace přidání modulu snap-in **certifikáty** a vyberte **Účet počítače** po zobrazení výzvy klikněte na tlačítko **Další**.
    
    1. Vyberte **Místní** spravovat a potom klikněte na **Dokončit**.
    
    1. Klikněte na **Ok** a potom rozbalte položku **certifikáty - osobní** uzly a potom na položku **certifikáty**. Certifikát jmenuje za názvem DNS OM a končí **cloudapp.net**. Klikněte pravým tlačítkem myši na název certifikát a klikněte na **Kopírovat**.
    
    1. Rozbalte položku **Důvěryhodné kořenové certifikační autority** klikněte pravým tlačítkem myši **certifikáty** a potom klikněte na **Vložit**.
    
    1. Ověřte, dvojitým kliknutím na název certifikátu za **Důvěryhodné kořenové certifikační autority** a ověřte, že nejsou žádné chyby a najdete v článku certifikátu. Pokud chcete pomocí skriptu HTTPS zahrnutý v tomto tématu, konfigurace serveru sestav přínosu certifikáty **Miniatura** je potřeba parametr skriptu. **Abyste získali hodnotu Miniatura**, provedením následujícího postupu. Je také ukázkové prostředí PowerShell k načtení miniaturu v části [použití skriptu pro konfiguraci serveru sestav a HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).
        
        1. Poklikejte na název certifikátu, například ssrsnativecloud.cloudapp.net.
        
        1. Klikněte na kartu **Podrobnosti** .
        
        1. Klikněte na **miniaturu**. Hodnota miniaturou se zobrazí v poli podrobnosti, například a6 08 3c sey|f f9 0b f7 e3 7c 25 upravit a4 Upravit 7e ac 91 9c 2c 2 f fb.
        
        1. Zkopírujte Miniatura a uložte hodnotu na pozdější skript nebo upravit nyní.
        
        1. (*) Aby se vám nestalo skript odeberte mezery mezi dvojice hodnot. Příklad Miniatura poznamenat před bude teď a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
        
        1. Přiřaďte certifikát serveru serveru sestav. Přiřazení dokončení v následující části při konfiguraci serveru sestav.

Pokud používáte certifikátu podepsaného svým držitelem SSL, název na certifikát už odpovídá hostname OM. Proto DNS počítače je již registrována globálně a můžete k nim získat přístup z libovolného klienta.

## <a name="step-3-configure-the-report-server"></a>Krok 3: Konfigurace serveru sestav

V této části vás provede konfigurace OM jako serveru sestav nativním režimu služby Reporting Services. Použijte jeden z následujících metod konfigurace serveru sestav:

- Konfigurace serveru sestav pomocí skriptu

- Pomocí Správce konfigurace pro nastavení serveru sestav.

Další podrobný postup najdete v části [připojení k virtuální počítač a spusťte Správce konfigurace služby Reporting Services](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Ověřování Poznámka:** Ověřování systému Windows je doporučené ověřování a služby Reporting Services ověřování výchozí. Pouze uživatelé nakonfigurované v OM přístup k služby Reporting Services a přiřazená k rolím služby Reporting Services.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Konfigurace serveru sestav a HTTP pomocí skriptu

Konfigurace serveru sestav pomocí skriptu prostředí Windows PowerShell, proveďte následující kroky. Konfigurace zahrnuje HTTP, ne HTTPS:

1. Z portálu Microsoft Azure klasické vyberte OM a klikněte na připojit. V závislosti na nastavení prohlížeče můžete být vyzváni k uložení souboru RDP připojit se bude v angličtině.

    ![připojení k azure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Použijte OM uživatelské jméno, uživatelské jméno a heslo, které jste nakonfigurovali, když jste vytvořili OM. 

    Na následujícím obrázku, například název OM je **ssrsnativecloud** a uživatelské jméno je **testuser**.
    
    ![přihlašovací jméno obsahuje OM](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. V OM spusťte **Windows PowerShell ISE** s oprávněními správce. Prostředí PowerShell ISE nainstalovaný ve výchozím nastavení v systému Windows server 2012. Je vhodné že použít ISE místo standardní okno prostředí Windows PowerShell tak, aby se daly vložení skriptu do ISE, upravte skript a znovu spusťte skript.

1. V ISE v prostředí Windows PowerShell klikněte na **kartě zobrazení** a potom klikněte na **Zobrazit podokno skriptu**.

1. Zkopírujte tento skript a vložení skriptu do podokna Skript ISE v prostředí Windows PowerShell.

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Pokud jste vytvořili OM s portem HTTP než 80, změnit parametr $HTTPport = 80.

1. Skript je již nakonfigurovány služby Reporting Services. Pokud chcete spustit skriptu pro služby Reporting Services, upravte verze část cesty k obor "v11", na příkaz Get-WmiObject.

1. Spusťte skript.

**Ověření**: Ověřte, zda je funkční funkce základní sestavy serveru, v části [Ověřte konfiguraci](#verify-the-configuration) dále v tomto tématu.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Konfigurace serveru sestav a HTTPS pomocí skriptu

Pokud chcete používat Windows PowerShell pro nastavení serveru sestav, proveďte následující kroky. Konfigurace zahrnuje HTTPS, ne HTTP.

1. Z portálu Microsoft Azure klasické vyberte OM a klikněte na připojit. V závislosti na nastavení prohlížeče můžete být vyzváni k uložení souboru RDP připojit se bude v angličtině.

    ![připojení k azure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Použijte OM uživatelské jméno, uživatelské jméno a heslo, které jste nakonfigurovali, když jste vytvořili OM. 

    Na následujícím obrázku, například název OM je **ssrsnativecloud** a uživatelské jméno je **testuser**.

    ![přihlašovací jméno obsahuje OM](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. V OM spusťte **Windows PowerShell ISE** s oprávněními správce. Prostředí PowerShell ISE nainstalovaný ve výchozím nastavení v systému Windows server 2012. Je vhodné že použít ISE místo standardní okno prostředí Windows PowerShell tak, aby se daly vložení skriptu do ISE, upravte skript a znovu spusťte skript.

1. Povolte spouštění skriptů, spusťte tento příkaz prostředí Windows PowerShell:

        Set-ExecutionPolicy RemoteSigned

    Spusťte podle následujících pokynů ověření zásady:

        Get-ExecutionPolicy

1. V **ISE v prostředí Windows PowerShell**klikněte na **kartě zobrazení** a potom klikněte na **Zobrazit podokno skriptu**.

1. Zkopírujte tento skript a vložit ho do podokna Skript ISE v prostředí Windows PowerShell.

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Změna parametrů **$certificatehash** v skript:

    - Toto je **potřeba** parametr. Pokud jste neuložili hodnotu certifikátu z předchozích kroků, pomocí jednu z následujících metod zkopírujte hodnotu hash certifikátu z Miniatura certifikáty.:

        Na OM otevřete ISE v prostředí Windows PowerShell a spusťte tento příkaz:

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        Výstup bude vypadat podobně jako tento. Pokud skript vrátí prázdný řádek, OM nemá certifikát nakonfigurovaný třeba, naleznete v části [použít certifikát podepsaný svým držitelem virtuálních počítačích](#to-use-the-virtual-machines-self-signed-certificate).
    
    NEBO
    
    - V OM spustit mmc.exe a pak přidejte modulu snap-in **certifikáty** .
    
    - V části uzel **Důvěryhodné kořenové certifikační autority** poklikejte na název vašeho certifikátu. Pokud používáte certifikátu podepsaného svým držitelem OM, certifikát jmenuje za názvem DNS OM a končí **cloudapp.net**.
    
    - Klikněte na kartu **Podrobnosti** .
    
    - Klikněte na **miniaturu**. Hodnota miniaturou se zobrazí v poli podrobnosti, například af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48
    
    - **Aby se vám nestalo skript**, odebrat mezery mezi dvojice hodnot. Příklad af1160b64b288d890a8212ff6ba9c3664f319048

1. Změna parametrů **$httpsport** : 

    - Pokud jste použili port 443 koncového bodu HTTPS, není třeba aktualizovat tento parametr ve skriptu. V opačném případě použijte hodnotu z pole port, který jste vybrali, když jste nakonfigurovali ve OM soukromé koncový bod HTTPS.

1. Změna parametrů **$DNSName** : 

    - Skript nakonfigurovaný pro certifikát zástupné $DNSName = "a". Pokud se chcete konfigurovat pro vazbu certifikátu zástupných znaků, poznámky, $DNSName ="+"a povolení následující řádek, odkaz celou $DNSNAme, ## $DNSName="$server.cloudapp.net".

        Pokud nechcete používat název DNS virtuálního počítače pro služby Reporting Services, změňte hodnotu $DNSName. Pokud parametr certifikát musí také použít tento název a registrace názvu globálně na serveru DNS.

1. Skript je již nakonfigurovány služby Reporting Services. Pokud chcete spustit skriptu pro služby Reporting Services, upravte verze část cesty k obor "v11", na příkaz Get-WmiObject.

1. Spusťte skript.

**Ověření**: Ověřte, zda je funkční funkce základní sestavy serveru, v části [Ověřte konfiguraci](#verify-the-connection) dále v tomto tématu. Ověřit certifikát vazby otevřete okno příkazového řádku s oprávněními správce a potom spusťte tento příkaz:

    netsh http show sslcert

Výsledek bude patřit následující úkoly:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Konfigurace serveru sestav pomocí Správce konfigurace

Pokud nechcete spustit skript Powershellu pro nastavení serveru sestav, postupujte podle kroků v této části můžete pomocí Správce konfigurace nativním režimu služby Reporting Services konfigurace serveru sestav.

1. Z portálu Microsoft Azure klasické vyberte OM a klikněte na připojit. Použijte uživatelské jméno a heslo, které jste nakonfigurovali, když jste vytvořili OM.

    ![připojení k azure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)

1. Spuštění Windows update a nainstalovat aktualizace bude v angličtině. Pokud je potřeba restartovat OM, restartujte OM a znovu připojíte k OM z portálu Microsoft Azure klasické.

1. V nabídce Start OM zadejte **Služby Reporting Services** a otevřete **Správce konfigurace služby Reporting Services**.

1. Ponechejte výchozí hodnoty pro **Název** serveru a název **Instance serveru sestav**. Klikněte na **Připojit**.

1. V levém podokně klikněte na **Adresu URL webové služby**.

1. Ve výchozím nastavení RS nakonfigurovaný pro port HTTP 80 IP "Všechny přiřazené". Chcete-li přidat HTTPS:

    1. V **Certifikátu SSL**: Vyberte certifikát, který chcete použít, například [Název OM]. cloudapp.net. Pokud jsou uvedené certifikáty, naleznete v části **Krok 2: vytvoření certifikát serveru** informace o tom, jak nainstalovat a kterým důvěřujete: certifikát v OM.
    
    1. V části **SSL Port**: Zvolte 443. Pokud jste nakonfigurovali soukromé koncový bod HTTPS v OM s portem různých soukromé použijte daná hodnota.
    
    1. Klikněte na **použít** a počkejte na dokončení operace.

1. V levém podokně klikněte na **databázi**.

    1. Klikněte na **změnit Databas**e.
    
    1. Klikněte na **vytvořit novou databázi serveru sestav** a klikněte na tlačítko **Další**.
    
    1. Ponechte výchozí **Název serveru**: jako OM název a nechejte výchozí **Typ ověřování** jako **Aktuálního uživatele** – **Integrované zabezpečení**. Klikněte na tlačítko **Další**.
    
    1. Ponechte výchozí **Název databáze** jako **ReportServer** a klikněte na tlačítko **Další**.
    
    1. Ponechte výchozí **Typ ověřování** jako **Pověření služby** a klikněte na tlačítko **Další**.
    
    1. Na stránce **Souhrn** , klikněte na tlačítko **Další** .
    
    1. Po dokončení konfigurace klikněte na **Dokončit**.

1. V levém podokně klikněte na **Adresu URL správce sestav**. Ponechte výchozí **Virtuální adresář** jako **sestavy** a klikněte na **použít**.

1. Klikněte na **Konec** ukončíte Správce konfigurace služby Reporting Services.

## <a name="step-4-open-windows-firewall-port"></a>Krok 4: Otevření Windows Firewall Port

>[AZURE.NOTE] Pokud jste používali nějaká skripty konfigurace serveru sestav, můžete tuto část přeskočit. Skript zahrnuty kroku otevřete portů brány firewall. Výchozí byl porty 80 http a port 443 HTTPS.

Vzdáleně připojovat k správce sestav nebo serveru sestav v počítači virtuální, v OM není potřeba TCP koncového bodu. Je potřeba otevřít stejný port v bráně firewall OM. Koncový bod byla vytvořená při zřizování OM.

Tato část obsahuje základní informace o tom, jak otevřít portů brány firewall. Další informace najdete v tématu [Konfigurace brány Firewall pro přístup k serveru sestav](https://technet.microsoft.com/library/bb934283.aspx)

>[AZURE.NOTE] Pokud jste použili skript konfigurace serveru sestav, můžete tuto část přeskočit. Skript zahrnuty kroku otevřete portů brány firewall.

Pokud jste nakonfigurovali soukromé port pro HTTPS než 443, upravte tento skript řádně podporovat. Pokud chcete otevřít port **443** v bráně Firewall systému Windows, proveďte následující kroky:

1. Otevřete okno prostředí Windows PowerShell s oprávněními správce.

1. Pokud jste použili port než 443, když jste nakonfigurovali koncový bod HTTPS v OM, aktualizujte port v následující příkaz a potom spusťte příkaz:

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. Po dokončení příkazu, **Ok** se zobrazí v okně příkazového řádku.

Pokud chcete ověřit, zda má být otevřena číslo portu, otevřete okno prostředí Windows PowerShell a spusťte tento příkaz:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Ověřte konfiguraci

Ověřte, zda funkce základní sestavy serveru teď pracuje, otevřete prohlížeč s oprávněními správce a pak přejděte k následující sestavy serveru ad správce sestav adresy URL:

- Na OM přejděte na adresu URL serveru sestav:

        http://localhost/reportserver

- Na OM přejděte na adresu URL správce sestavy:

        http://localhost/Reports

- Z místního počítače přejděte na sestavu **vzdáleného** správce v OM. Aktualizace názvu DNS v následujícím příkladu podle potřeby. Po zobrazení výzvy k zadání hesla, použijte přihlašovací údaje správce, který jste vytvořili při zřizování OM. Uživatelské jméno je v seznamu [doména]\[uživatelské jméno] formát, kde doménu je název počítače OM, například ssrsnativecloud\testuser. Pokud nepoužíváte HTTP**S**, odeberte **s** v adrese URL. Naleznete v části Další informace o vytváření dalších uživatelů na OM.

        https://ssrsnativecloud.cloudapp.net/Reports

- Z místního počítače přejděte na adresu URL serveru vzdálené sestavy. Aktualizace názvu DNS v následujícím příkladu podle potřeby. Pokud nepoužíváte HTTPS, odeberte s v adrese URL.

        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Vytvoření uživatelů a přiřadit role

Konfigurace a ověřování serveru sestav, běžné Správce úloh po vytvoření jednoho nebo víc uživatelů a přiřaďte uživatelům k rolím služby Reporting Services. Další informace najdete v těchto článcích:

- [Vytvoření místní uživatelského účtu](https://technet.microsoft.com/library/cc770642.aspx)

- [Uživateli udělit přístup k serveru sestav (Správce sestav)](https://msdn.microsoft.com/library/ms156034.aspx))

- [Vytváření a Správa přiřazování rolí](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>K vytváření a publikování sestav Azure virtuálního počítače

Následující tabulka shrnuje některé z možností dostupných publikovat existující sestavy z místního počítače k serveru sestav hostovaný na Microsoft Azure virtuálního počítače:

- **Skript RS.exe**: použití RS.exe skript Kopírovat sestavu položek z a existujícího serveru sestav na Microsoft Azure virtuálního počítače. Další informace naleznete v části "Nativním režimu na nativní – Microsoft Azure virtuálního počítače" v [rs.exe Reporting Services ukázka skriptu pro migraci obsahu mezi serverech sestav](https://msdn.microsoft.com/library/dn531017.aspx).

- **Pomocí Tvůrce sestav mohou**: virtuální počítač zahrnuje kliknutím-jednou verzi aplikace Microsoft SQL Server pomocí Tvůrce sestav mohou. Spuštění sestavy Tvůrce první na virtuálního počítače:

    1. Spusťte prohlížeč s oprávněními správce.
    
    1. Přejděte na správce sestav v počítači virtuální a na pásu karet klikněte na tlačítko **Tvůrce sestav** .

    Další informace najdete v tématu [instalace, odinstalování a podporu pomocí Tvůrce sestav mohou](https://technet.microsoft.com/library/dd207038.aspx).

- **SQL Server Data Tools: OM**: Pokud jste vytvořili OM s SQL Server 2012, a pak SQL Server Data Tools nainstalovaná v počítači virtuální a slouží k vytvoření **Sestavy serveru projekty** a sestav na virtuální počítač. SQL Server Data Tools můžete publikovat sestavy na serveru sestav na virtuální počítač.
    
    Pokud jste vytvořili OM se serverem SQL server 2014, můžete nainstalovat SQL Server Data nástroje - BI for visual Studio. Další informace najdete v těchto článcích:

    - [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)

    - [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [SQL Server Data Tools a SQL Server Business Intelligence (SSDT BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: vzdálené**: na místním počítači, vytvořte projekt služby Reporting Services v SQL Server Data Tools obsahující sestavy služby Reporting Services. Konfigurace project pro připojení k adresu URL webové služby.

    ![Vlastnosti projektu SSDT SSRS projektu](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)

- **Používat skript**: pomocí skriptu zkopírujte obsah serveru sestav. Další informace najdete v tématu [rs.exe Reporting Services ukázka skriptu pro migraci obsahu mezi serverech sestav](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Pokud nepoužíváte OM minimalizace nákladů

>[AZURE.NOTE] Minimalizovat poplatky pro vaše virtuálních počítačích Azure není při použití, vypněte OM z portálu Microsoft Azure klasické. Pokud používáte Windows Možnosti power do virtuálního počítače vypnout OM, bude pořád Účtovaná stejnou hodnotu pro OM. Snížit náklady, budete muset vypnout OM Azure klasické portálu. Pokud již nepotřebujete OM, nezapomeňte OM se soubory přidružené VHD kvůli nižším poplatkům za úložiště. Další informace naleznete v části Nejčastější dotazy na [Virtuálních počítačích ceny podrobnosti](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>Další informace

### <a name="resources"></a>Zdroje informací

- Podobně jako obsah související se nasazení jednoho serveru SQL Server Business Intelligence a SharePoint 2013 najdete v článku [Vytvoření Azure OM s SQL Server BI a SharePoint 2013 pomocí Windows Powershellu](https://msdn.microsoft.com/library/azure/dn385843.aspx).

- Podobně jako obsah související se nasazení více serveru SQL Server Business Intelligence a SharePoint 2013 najdete v článku [Nasazení SQL Server Business Intelligence v Azure virtuálních počítačích](https://msdn.microsoft.com/library/dn321998.aspx).

- Obecné informace o nasazení systému SQL Server Business Intelligence v Azure virtuálních počítačích najdete v tématu [Funkce Business Intelligence SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-classic-ps-sql-bi.md).

- Další informace o nákladech Azure výpočet nákladů najdete v tématu Karta virtuálních počítačích [Azure ceny kalkulačky](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Obsahu vytvořeného komunitou

- Podrobné informace o tom, jak vytvořit Reporting Services nativní serveru sestav režimu bez použití skript najdete v článku [Hostingu SQL zpráv o chybách v Azure virtuálního počítače](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Odkazy na další zdroje pro systém SQL Server ve Azure VMs

[SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md)
