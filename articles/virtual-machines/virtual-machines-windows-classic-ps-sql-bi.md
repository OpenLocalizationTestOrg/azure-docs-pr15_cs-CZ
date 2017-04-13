<properties
    pageTitle="SQL Server Business Intelligence | Microsoft Azure"
    description="Toto téma využívá zdroje vytvořená pomocí klasické nasazení modelu a popisuje funkce Business Intelligence (BI) k dispozici pro systém SQL Server na virtuálních počítačích Azure (VMs) systém."
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

# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>SQL Server Business Intelligence v Azure virtuálních počítačích

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Galerie Microsoft Azure Virtual Machine obsahuje obrázky, které obsahují instalace SQL serveru. Edice systému SQL Server podporuje obrázky galerie jsou instalace soubory, které můžete nainstalovat do místního počítače a virtuálních počítačích. Toto téma obsahuje souhrn funkcí SQL Server Business Intelligence (BI) nainstalovaný na obrázky a konfigurace kroky po zřízení virtuálního počítače. Toto téma popisuje také nasazení podporovaných topologií funkcí BI a osvědčené postupy.

## <a name="license-considerations"></a>Důležité informace o licenci

S licencí SQL Server ve virtuálních počítačích Microsoft Azure dvěma způsoby:

1. Licenční mobilita výhod sady, které jsou součástí Software Assurance. Další informace najdete v tématu [Mobilita licence až Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. Platíte za hodinu sazba z Azure virtuálních počítačích s nainstalovanou SQL Server. V části "SQL Server" v [Ceny virtuálních počítačích](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Další informace o licencování a aktuální sazeb najdete v článku [Virtuálních počítačích licencování nejčastější dotazy týkající se](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server obrázky k dispozici v Azure virtuální Galerie počítače

Galerie Microsoft Azure Virtual Machine obsahuje několik obrázků, které obsahují Microsoft SQL Server. Softwaru nainstalovaného na obrázky virtuálního počítače se může lišit podle verzí operačního systému a verze systému SQL Server. Seznam obrázků v galerii Azure virtuálního počítače často mění.

![Obrázek SQL azure galerii OM](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)

![Prostředí PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Seznam Azure obrázky, které obsahují "SQL-Server" v ImageName vyhledaných tento skript Powershellu:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "settings" menu in Azure classic portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Další informace o edice a funkce podporované jednotlivými SQL serveru najdete v těchto článcích:

- [SQL Server edice](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)

- [Funkce podporované jednotlivými edicemi systému SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>Funkce BI nainstalovaným obrázky Galerie virtuálního počítače SQL serveru

Následující tabulka shrnuje funkcí Business Intelligence je nainstalovaná na běžné Galerie obrázků Microsoft Azure virtuálního počítače pro SQL Server"

- SQL Server 2016 RC3

- SQL Server 2014 SP1 Enterprise

- SQL Server 2014 SP1 Standard

- SQL Server 2012 SP2 Enterprise

- SQL Server 2012 SP2 Standard

|Funkce BI SQL serveru|V Galerie nainstalovaný|Poznámky|
|---|---|---|
|**Nativním režimu služby Reporting**|Ano|Nainstalovaný, ale vyžaduje konfiguraci, včetně adresy URL správce sestav. Naleznete v části [Nastavení služby Reporting Services](#configure-reporting-services).|
|**Reporting Services integrovaném režimu sharepointu**|Ne|Galerie aplikace Microsoft Azure Virtual Machine nezahrnuje Sharepointu nebo Sharepointu instalačních souborů. <sup>1</sup>|
|**Dolování Analysis Services multidimenzionální a Data (OLAP)**|Ano|Nainstalovali a nakonfigurovali jako výchozí instanci služby Analysis Services|
|**Tabulek Analysis Services**|Ne|Podporované ve verzi SQL Server 2012, 2014 a 2016 obrázky ale není nainstalovaná ve výchozím nastavení. Instalace jiné instanci služby Analysis Services. Najdete v části instalace dalších služby SQL Server a funkcích v tomto tématu.|
|**Analysis Services Power Pivot pro SharePoint**|Ne|Galerie aplikace Microsoft Azure Virtual Machine nezahrnuje Sharepointu nebo Sharepointu instalačních souborů. <sup>1</sup>|

<sup>1</sup> Další informace na serveru SharePoint a Azure virtuálních počítačích, najdete v článku [Microsoft Azure architektury pro SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) a [Nasazení Sharepointu na virtuálních počítačích Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=34598).

![Prostředí PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Spusťte tento příkaz prostředí PowerShell zobrazíte seznam nainstalované služby, které obsahují "SQL" v názvu služby.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Obecné doporučení a doporučené postupy

- Minimální velikost doporučené virtuálního počítače je **A3** při použití SQL Server Enterprise Edition. Velikost virtuálního počítače **A4** je vhodné pro nasazení SQL Server BI Analysis Services a služby Reporting Services.

    Další informace o aktuálním velikosti OM najdete v článku [Virtuálního počítače velikosti Azure](virtual-machines-linux-sizes.md).

- Doporučený postup pro správu disků je k ukládání dat, protokolu a záložních souborů na jednotkách než **C**: a **D**:. Můžete například vytvořit dat disků **E**: a **F**:.

    - Jednotka ukládání do mezipaměti zásad pro výchozí jednotky **C**: není optimální pro práci s daty.

    - **D**: jednotka je dočasný jednotku, která se používá především k souboru stránky. **D**: jednotka není zachován a v úložišti objektů blob se neuloží. Správa úkolů, jako je změna velikosti virtuální počítač obnovit **D**: jednotky. Doporučujeme **není** použijte **D**: jednotka pro soubory databáze, včetně tempdb.

    Další informace o vytváření a připojení disků najdete v článku [jak připojit Disk dat do virtuálního počítače](virtual-machines-windows-classic-attach-disk.md).

- Zastavení nebo odinstalace služby, které nejsou budete používat. Pro příklad se používá pouze virtuálního počítače pro služby Reporting Services, zastavit nebo odinstalovat služby Analysis Services a služby SQL Server Integration Services. Následující obrázek je příkladem služby, které jsou ve výchozím nastavení spuštěna.

    ![Služby SQL Server](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)

    >[AZURE.NOTE] Databázový stroj SQL Server vyžaduje Podporované scénáře BI. V jednom serveru topologie OM je nutné databázový stroj běžet na stejné OM.

    Další informace najdete v tématu následující: [Odinstalace služby Reporting Services](https://msdn.microsoft.com/library/hh479745.aspx) a [odinstalovat Instance ze služby Analysis Services](https://msdn.microsoft.com/library/ms143687.aspx).

- **Windows Update** zjišťovat nové "důležité aktualizace". Obrázky Microsoft Azure virtuálního počítače se často aktualizovat; ale důležité aktualizace může dostupné z **Webu Windows Update** po naposledy aktualizováno OM obrázek.

## <a name="example-deployment-topologies"></a>Příklad topologie nasazení

Následuje příklad nasazení, které používají virtuálních počítačích Microsoft Azure. Topologie v těchto diagramech jsou jenom některé z možných topologií použitelných s funkcemi BI SQL serveru a virtuálních počítačích Microsoft Azure.

### <a name="single-virtual-machine"></a>Jeden virtuálního počítače

Analysis Services, služby Reporting Services, SQL serveru databázový stroj a zdroje dat v jednom počítači virtuální.

![BI IAS scénář 1 virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Dva virtuálních počítačích

- Služby Analysis Services, služby Reporting Services a SQL serveru databázový stroj na jednom počítači virtuální. Toto nasazení obsahuje databáze serveru sestav.

- Zdroje dat na druhý OM. Druhá OM obsahuje jako zdroje dat SQL serveru databázový stroj.

![scénář BI iaas s 2 virtuálních počítačích](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Smíšené Azure – data v databázi Azure SQL

- Služby Analysis Services, služby Reporting Services a SQL serveru databázový stroj na jednom počítači virtuální. Toto nasazení obsahuje databáze serveru sestav.

- Zdroj dat je databáze Azure SQL.

![scénáře OM BI iaas a AzureSQL jako zdroj dat](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hybridní – data místní

- V tomto příkladu nasazení služby Analysis Services služby Reporting Services a SQL serveru databázový stroj běží v jednom počítači virtuální. Virtuální počítač hostuje databáze serveru sestav. Virtuální počítač připojen k místní domény prostřednictvím virtuální sítě Azure nebo některé další VPN tunneling řešení.

- Zdroj dat je místní.

![scénáře OM BI iaas a v místní zdroje dat](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Konfigurace nativním režimu služby Reporting

Virtuální počítač Galerie aplikace pro systém SQL Server obsahuje Reporting Services nativním režimu nainstalovali, ale není nakonfigurován serveru sestav. Postup v této části Konfigurace serveru sestav služby Reporting Services. Podrobnější informace o konfiguraci Reporting Services nativním režimu najdete v článku [Instalace Reporting Services nativním režimu sestavy serveru (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

>[AZURE.NOTE] Podobně jako obsah, který používá skripty Windows Powershellu pro nastavení serveru sestav najdete v článku [použití vytvoření Azure OM s nativním režimu sestavy serveru](virtual-machines-windows-classic-ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Připojení k virtuální počítač a spusťte Správce konfigurace služby Reporting Services

Jsou dva běžné pracovní postupy pro připojení k Azure virtuálního počítače:

- Připojení, klikněte na název virtuálního počítače a pak klikněte na **Připojit**. Připojení ke vzdálené ploše otevře a název počítače, zadá automaticky.

    ![připojení k azure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)

- Připojení k virtuální počítač s Windows připojení ke vzdálené ploše. V uživatelském rozhraní Vzdálená plocha:

    1. Zadejte **název služby cloudu** jako název počítače.

    1. Zadejte dvojtečka (:) a číslo portu veřejné nakonfigurovaný pro vzdálené plochy endpoint TCP.

        MyService.cloudapp.NET:63133

        Další informace najdete v tématu [Co je Cloudová služba?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).

**Spusťte Správce konfigurace služby Reporting.**

1. V **systému Windows Server 2012**:

1. Na obrazovce **Start** zadejte **Služby Reporting Services** zobrazíte seznam aplikací.

1. Klikněte pravým tlačítkem myši **Správce konfigurace služby Reporting Services** a klikněte na **Spustit jako správce**.

1. V **systému Windows Server 2008 R2**:

1. Klikněte na tlačítko **Start**a potom klikněte na **Všechny programy**.

1. Klikněte na **Microsoft SQL Server 2016**.

1. Klikněte na **konfigurační nástroje**.

1. Klikněte pravým tlačítkem myši **Správce konfigurace služby Reporting Services** a klikněte na **Spustit jako správce**.

Nebo

1. Klikněte na tlačítko **Start**.

1. V dialogovém okně **Prohledat programy a soubory** zadejte **služby reporting services**. Pokud OM běží Windows Server 2012, zadejte na obrazovce Start systému Windows Server 2012 **reporting services** .

1. Klikněte pravým tlačítkem myši **Správce konfigurace služby Reporting Services** a klikněte na **Spustit jako správce**.

    ![Vyhledejte Správce konfigurace ssrs](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Konfigurace služby Reporting Services

**Účet služby a adresa URL webové služby:**

1. Ověřte, zda že je v poli **Název serveru** název místní serveru a klikněte na **Připojit**.

1. Poznamenejte si prázdné **Název databáze serveru sestav**. Databáze se vytvoří po dokončení konfigurace.

1. Ověření, že **Sestavy serveru stav** je **spuštěno**. Pokud chcete ověřit službu v správce systému Windows Server, služba je služba **SQL Server Reporting Services** systému Windows.

1. Klikněte na **Účet služby** a podle potřeby změňte účet. Pokud virtuální počítač používá v prostředí spojených mimo doménu, stačí předdefinovaný **ReportServer** účet. Další informace o účtu služby najdete v článku [Účet služby](https://msdn.microsoft.com/library/ms189964.aspx).

1. V levém podokně klikněte na **Adresu URL webové služby** .

1. Klikněte na **použít** pro nastavení výchozí hodnoty.

1. Poznámka: **adresy URL webových služeb serveru sestav**. Všimněte si výchozí port TCP 80 a zadejte část adresy URL. Později vytvoření koncového bodu Microsoft Azure virtuálního počítače číslo portu.

1. V podokně **výsledků** zkontrolujte akce byla úspěšně dokončena.

**Databáze:**

1. V levém podokně klikněte na **databázi** .

1. Klikněte na **Změnit databázi**.

1. Ověřte, že je vybraná možnost **vytvořit novou databázi serveru sestav** a pak klikněte na další.

1. Zkontrolujte **Název serveru** a klikněte na **Testovat připojení**.

1. Pokud je výsledek **Testovat připojení bylo úspěšné**, klikněte na tlačítko **OK** a klikněte na tlačítko **Další**.

1. Poznámka: název databáze je **ReportServer** a **režimu serveru sestav** je **nativní** a pak klikněte na tlačítko **Další**.

1. Na stránce **přihlašovací údaje** klikněte na tlačítko **Další** .

1. Na stránce **Souhrn** , klikněte na tlačítko **Další** .

1. Na stránce **průběhu a dokončit** klikněte na **Další** .

**Webová adresa URL portálu nebo adresu URL správce sestav pro 2012 a 2014:**

1. Klikněte na **Webová adresa URL portálu**nebo **Adresu URL správce sestav** pro 2014 a 2012 v levém podokně.

1. Klikněte na tlačítko **použít**.

1. V podokně **výsledků** zkontrolujte akce byla úspěšně dokončena.

1. Klikněte na **Konec**.

Informace o oprávněních serveru sestav najdete v článku [Udělení oprávnění na nativní serveru sestav režimu](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Přejděte do místního správce sestav

Ověřte konfiguraci, přejděte na správce sestav na OM.

1. Na OM spusťte aplikaci Internet Explorer s oprávněními správce.

1. Přejděte na http://localhost/reports na OM.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Chcete připojit ke vzdálené webového portálu nebo správce sestav pro 2014 a 2012

Pokud se chcete připojit k webového portálu nebo správce sestav pro 2014 a 2012 počítače virtuální ze vzdáleného počítače, vytvořte nový virtuální počítač TCP Endpoint. Ve výchozím nastavení serveru sestav sleduje HTTP požadavky na **port 80**. Pokud jste nakonfigurovali adresy URL serveru sestav použití jiného portu, zadejte toto číslo portu v následujících pokynů.

1. Vytvoření koncového bodu virtuálního počítače TCP Port 80. Další informace naleznete v části [koncové body virtuálního počítače a portů brány Firewall](#virtual-machine-endpoints-and-firewall-ports) v tomto dokumentu.

1. Otevřete port 80 v bráně firewall virtuálního počítače.

1. Přejděte na web portálu nebo správce sestav, pomocí Azure virtuálního počítače **Název DNS** jako název serveru v adrese URL. Příklad:

    **Server sestav**: http://uebi.cloudapp.net/reportserver  **webového portálu**: http://uebi.cloudapp.net/reports

    [Konfigurace brány Firewall pro přístup k serveru sestav](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>K vytváření a publikování sestav Azure virtuálního počítače

Následující tabulka shrnuje některé z možností dostupných publikovat existující sestavy z místního počítače k serveru sestav hostovaný na Microsoft Azure virtuálního počítače:

- **Pomocí Tvůrce sestav mohou**: virtuální počítač zahrnuje kliknutím-jednou verzi aplikace Microsoft SQL Server pomocí Tvůrce sestav mohou SQL 2014 a 2012. Spuštění sestavy Tvůrce první počítače virtuální s SQL 2016:

    1. Spusťte prohlížeč s oprávněními správce.

    1. Přejděte na portál web na virtuálních počítačích a v pravém horním rohu vyberte ikonu **Stáhnout** .
    
    1. Vyberte **Tvůrce sestav**.

    Další informace najdete v tématu [Spuštění pomocí Tvůrce sestav mohou](https://msdn.microsoft.com/library/ms159221.aspx).

- **SQL Server Data Tools**: OM: SQL Server Data Tools nainstalovaná v počítači virtuální a můžete použít k vytvoření **Sestavy serveru projekty** a sestav v počítači virtuální. SQL Server Data Tools můžete publikovat sestavy na serveru sestav na virtuální počítač.

- **SQL Server Data Tools: vzdálené**: na místním počítači, vytvořte projekt služby Reporting Services v SQL Server Data Tools obsahující sestavy služby Reporting Services. Konfigurace project pro připojení k adresu URL webové služby.

    ![Vlastnosti projektu SSDT SSRS projektu](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)

- Vytvoření. Pevný disk virtuálního pevného disku, který obsahuje sestavy a potom uložíte a připojíte jednotku.

    1. Vytvoření. Virtuální pevný disk pevný disk na místním počítači, který obsahuje vaše sestavy.

    1. Vytvoření a nainstalujte certifikát správy.

    1. Nahrajte soubor virtuální pevný disk na Azure pomocí rutiny přidat AzureVHD [vytvořit a nahrát virtuálního pevného disku serveru Windows Azure k](virtual-machines-windows-classic-createupload-vhd.md).

    1. Připojte disk do virtuálního počítače.

## <a name="install-other-sql-server-services-and-features"></a>Instalace jiné služby SQL Server a funkce

Instalace další služby SQL Server, jako je služby Analysis Services v tabulkovém režimu, spusťte Průvodce instalací SQL serveru. Instalační soubory jsou na místním disku virtuálního počítače.

1. Klikněte na tlačítko **Start** a potom klikněte na **Všechny programy**.

1. Klikněte na **Microsoft SQL serveru 2016**, **Microsoft SQL Server 2014** nebo **Microsoft SQL Server 2012** a potom klikněte na **Konfigurační nástroje**.

1. Klikněte na **Centrum instalace SQL serveru**.

Nebo po spuštění C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe nebo C:\SQLServer_11.0_full\setup.exe

>[AZURE.NOTE] Při prvním spuštění instalace SQL serveru, další instalačních souborů může stáhne a vyžadovat restartování virtuálního počítače a restartujte instalační program SQL serveru.
>
>Pokud potřebujete opakovaně upravte obrázek vybraný z Microsoft Azure virtuálního počítače, zvažte vytvoření vlastního obrázku SQL serveru. Funkce Analysis Services SysPrep byla povolena s SQL Server 2012 SP1 CU2. Další informace najdete v tématu [co zvážit při instalaci SQL serveru pomocí SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) a [Sysprep podpora role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="to-install-analysis-services-tabular-mode"></a>Chcete-li nainstalovat tabulkových režimu služeb Analysis

Kroky v tomto oddílu **Souhrn** instalace tabulkových režimu služby Analysis Services. Další informace najdete v těchto článcích:

- [Instalace služby Analysis Services v tabulkovém režimu](https://msdn.microsoft.com/library/hh231722.aspx)

- [Tabulkovém modelu (kurz Adventure Works)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Nainstalovat Analysis Services tabulkových režimu:**

1. V Průvodci instalací SQL serveru, klikněte na možnost **instalace** v levém podokně a potom klikněte na **samostatné instalace nových SQL serveru nebo přidání funkcí do existující instalace**.

    - Pokud se zobrazí **Vyhledat složku**, přejděte k c:\SQLServer_13.0_full, c:\SQLServer_12.0_full nebo c:\SQLServer_11.0_full a potom klikněte na **Ok**.

1. Na stránce aktualizací produktu klikněte na tlačítko **Další** .

1. Na stránce **Instalace typ** vyberte možnost **provést novou instalaci systému SQL Server** a klikněte na tlačítko **Další**.

1. Na stránce **Nastavení Role** klikněte na **Funkce instalace SQL serveru**.

1. Na stránce **Výběr funkcí** na **Služby Analysis Services**.

1. Na stránce **Konfigurace Instance** zadejte popisný název, třeba **tabulky** do textových polí **s názvem Instance** a **Instance Id** .

1. Na stránce **Konfigurace služby Analysis Services** vyberte **Tabulkových režim**. Přidání aktuálního uživatele do seznamu oprávnění správce.

1. Dokončení a zavřete průvodce instalací SQL serveru.

## <a name="analysis-services-configuration"></a>Konfigurace služby Analysis Services

### <a name="remote-access-to-analysis-services-server"></a>Vzdálený přístup k serveru služby Analysis Services

Server služby Analysis Services podporuje pouze ověřování systému windows. Vzdálený přístup k služby Analysis Services v klientských aplikacích třeba SQL Server Management Studio nebo SQL Server Data Tools, virtuální počítač musí být členem vaší místní domény pomocí Azure virtuální sítě. Další informace najdete [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md).

**Výchozí instanci** služby Analysis Services sleduje na TCP port **2383**. Otevřete port v bráně firewall virtuálních počítačích. Skupinový pojmenovanou instanci služby Analysis Services taky na port **2383**přijímá.

Pro **s názvem instanci** služby Analysis Services je nutné služby SQL Server prohlížeče Správa port přístupu. Výchozí konfigurace Prohlížeč serveru SQL je port **. 2382**.

V bráně firewall virtuálních počítačích otevřete port **. 2382** a vytvořte statické Analysis Services s názvem instance port.

1. Pokud chcete ověřit porty, které se již používají v OM a jaké proces používá porty, spusťte tento příkaz s oprávněními správce:

        netstat /ao

1. Použití SQL Server Management Studio vytvořit statické služby Analysis Services s názvem instance port aktualizací Port"hodnotu v tabulkovém jako instance obecných vlastností. Další informace najdete v tématu "použít pevné port pro výchozí nebo pojmenované instance" v [článku Konfigurace brány Windows Firewall povolit přístup Analysis Services](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).

1. Restartujte tabulkových instanci služby Analysis Services.

Další informace naleznete v části **koncové body virtuálního počítače a portů brány Firewall** v tomto dokumentu.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Koncové body virtuálního počítače a portů brány Firewall

V této části shrnuje Microsoft Azure virtuálního počítače koncových bodů vytvořit a porty otevřete v bránách firewall virtuálního počítače. Následující tabulka shrnuje porty **TCP** vytvořit koncové body a porty otevřete v bráně firewall virtuálních počítačích.

- Pokud používáte jednu OM a následující dvě položky jsou pravdivé, nemusíte vytvářet koncové body OM a nemusíte otevřete v bránu firewall na OM porty.

    - Není vzdáleně připojíte k funkcím serveru SQL Server na OM. Vytvoření připojení ke vzdálené ploše bude v angličtině a přístupu k funkcím serveru SQL místně na OM nejsou považovány za připojení ke vzdálené funkcí SQL serveru.

    - OM není připojit k místní domény prostřednictvím virtuální sítě Azure nebo jiné VPN tunelování řešení.

- Pokud virtuální počítač není připojený k doméně, ale chcete vzdáleně připojte k serveru SQL Server funkcí u OM:

    - Otevřete v bránu firewall na OM porty.

    - Vytvořte virtuální počítač koncové body vyznačená porty (*).

- Pokud virtuální počítač připojen k domény pomocí VPN tunelem například Azure virtuální sítě, není koncové body povinný. Otevřete v bránu firewall na OM tyto porty.

  	|Port|Typ|Popis|
|---|---|---|
|**80**|TCP|Server sestav vzdáleného přístupu (*).|
|**1433**|TCP|SQL Server Management Studio (*).|
|**1434**|UDP|Prohlížeč serveru SQL. To je potřeba při OM v připojen k doméně.|
|**. 2382**|TCP|Prohlížeč serveru SQL.|
|**2383**|TCP|Výchozí instanci služby SQL Server Analysis Services a skupinový pojmenované instance.|
|**Uživatelsky definované**|TCP|Vytvoření statické Analysis Services s názvem instance port pro číslo portu, které vyberete a odblokování číslo portu v bráně firewall.|

Další informace o vytváření koncové body najdete v těchto článcích:

- Vytvoření koncové body:[jak nastavit koncové body do virtuálního počítače](virtual-machines-windows-classic-setup-endpoints.md).

- SQL Server: Najdete v části "Dokončení konfigurace kroky se připojit k počítači virtuální použití SQL Server Management Studio" [zřizování SQL Server virtuálního počítače na Azure](virtual-machines-windows-portal-sql-server-provision.md).

Následující obrázek znázorňuje porty otevřete v bráně firewall OM tak, aby vzdálený přístup k funkcím a součásti OM.

![porty otevřete bi aplikací v Azure VMs](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Zdroje informací

- Prohlédněte si zásadách podpory pro software společnosti Microsoft serveru v prostředí virtuální počítač Azure. Následující téma shrnuje podporu pro funkce, jako jsou nástroje BitLocker clusterů převzetí a vyrovnávání zatížení sítě. [Serverový software Microsoft podporu pro Azure virtuálních počítačích](http://support.microsoft.com/kb/2721672).

- [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md)

- [Virtuálních počítačích](https://azure.microsoft.com/documentation/services/virtual-machines/)

- [Zřízení SQL Server virtuálního počítače na Azure](virtual-machines-windows-portal-sql-server-provision.md)

- [Jak připojit Disk dat do virtuálního počítače](virtual-machines-windows-classic-attach-disk.md)

- [Migrace databáze na serveru SQL Server na Azure OM](virtual-machines-windows-migrate-sql.md)

- [Určení režimu serveru Analysis Services Instance](https://msdn.microsoft.com/library/gg471594.aspx)

- [Multidimenzionální modelování (kurz Adventure Works)](https://technet.microsoft.com/library/ms170208.aspx)

- [Centrum si přečtěte následující dokumentaci Azure](https://azure.microsoft.com/documentation/)

- [Použití Power BI v hybridním prostředí](https://msdn.microsoft.com/library/dn798994.aspx)

>[AZURE.NOTE] [Odeslání názoru a kontaktní informace pomocí funkce Microsoft SQL Server připojit](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Obsahu vytvořeného komunitou

- [Správa databáze Azure SQL pomocí prostředí PowerShell](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)
