<properties
    pageTitle="Konfigurace vždy na skupiny dostupnosti v Azure OM – klasické"
    description="Vytvořte skupinu vždy o dostupnosti s Azure virtuálních počítačích. V tomto primárně výukovém uživatelského rozhraní a nástroje místo skriptování."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm---classic"></a>Konfigurace vždy na skupiny dostupnosti v Azure OM – klasické

> [AZURE.SELECTOR]
- [Správce prostředků: šablony](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Správce prostředků: ruční](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasický: uživatelské rozhraní](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasický: prostředí PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Tento kurz začátku do konce se dozvíte, jak implementovat dostupnost skupiny pomocí SQL Server vždy o spuštění na Azure virtuálních počítačích.

Na konci kurzu SQL Server vždy v řešení v Azure bude obsahovat tyto prvky:

- Virtuální sítě obsahující více podsítí, včetně front-end a back-end podsítě

- Řadiče domény s doménou Active Directory (AD)

- Dva SQL Server VMs používaný podsítě back-end a připojení k doméně AD

- Shluk WSFC 3 uzel s modelem kvora Většina uzlů

- Skupiny dostupnosti s dvěma replikami synchronní potvrdit dostupnost databáze

Následující obrázek je grafické znázornění řešení.

![Testování laboratorní architektura pro AG v Azure](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Všimněte si, že je to možné konfigurací. Například můžete minimalizovat počet VMs pro skupinu dvě otevřené dostupnost za účelem uložení ve výpočetním hodiny v Azure pomocí řadiče domény jako kvora soubor kopií v clusteru WSFC 2 uzlů. Tento způsob snižuje počet OM pomocí jednoho z výše uvedených konfigurace.

Tento kurz předpokládá takto:

- Už máte účet Azure.

- Znáte zřízení klasické OM Server SQL z Galerie virtuálního počítače pomocí grafického uživatelského rozhraní.

- Už máte dobré porozumění skupin vždy na dostupnost. Další informace najdete v tématu [Vždy na dostupnost skupiny (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Pokud vás zajímají pomocí vždy na dostupnost se Sharepointem, podívejte se také [Konfigurace SQL Server 2012 vždy o dostupnosti skupin pro SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Vytváření virtuálních sítí a Server řadiče domény

Začnete s novým účtem zkušební verzi Azure. Po dokončení nastavení účtu by měl být na domovské obrazovce Azure klasické portálu.

1. Tlačítko **Nový** v levém dolním rohu stránky, jak je ukázáno v následujícím příkladu.

    ![Klikněte na tlačítko Nový v portálu](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)

1. Klikněte na **Síť služby**, a pak klikněte na **Virtuální sítě** a klikněte na **Vytvořit vlastní**, jak je ukázáno v následujícím příkladu.

    ![Vytvořit virtuální sítě](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)

1. V dialogovém okně **Vytvořit virtuální sítě** vytvoříte novou síť virtuální krokování stránky s následující nastavení. 

  	|Stránky|Nastavení|
|---|---|
|Virtuální nastavení sítě|**Název = ContosoNET**<br/>**OBLAST = Západ USA**|
|Servery DNS a připojením VPN|Žádná|
|Virtuální sítě adresu mezer|Nastavení se zobrazí v snímek níže: ![Vytvořit virtuální sítě](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png)|

1. Dále vytvořte OM použijete jako domény řadiče. Klikněte na **Nový** znovu, a pak **Výpočet**, a pak **virtuálního počítače**a potom **Z Galerie**, jak je ukázáno v následujícím příkladu.

    ![Vytvoření virtuálního počítače](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)

1. V dialogovém okně **vytvořit virtuální počítač A** nakonfigurujte nové OM krokování stránky s následující nastavení. 

  	|Stránky|Nastavení|
|---|---|
|Vyberte operačního systému virtuálního počítače|Windows serveru 2012 R2 datacentru|
|Konfigurace virtuálního počítače|**Datum vydání verze** = (nejnovější)<br/>**Název počítače virtuální** = ContosoDC<br/>**VRSTVY** = STANDARD<br/>**Velikost** = A2 (2 jádra)<br/>**Nové uživatelské jméno** = AzureAdmin<br/>**Nové heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000|
|Konfigurace virtuálního počítače|**CLOUDOVÁ služba** = vytvořit nové cloudové služby<br/>**Název CLOUDOVÉ služby DNS** = názvu jedinečné cloudové služby<br/>**Název DNS** = jedinečný název (ex: ContosoDC123)<br/>**Oblast/SPŘAŽENÍ skupiny/virtuální sítě** = ContosoNET<br/>**Virtuální PODSÍTÍ** = Back(10.10.2.0/24)<br/>**Úložiště účtu** = použití účtu automaticky generované úložiště<br/>**Nastavte dostupnost** = (žádné)|
|Možnosti virtuálního počítače|Použití výchozích hodnot|

Po dokončení konfigurace nové OM, počkejte OM být provsioned. Tento proces trvá delší dobu dokončení a když kliknete na kartu **virtuálního počítače** na portálu Azure klasické, uvidíte ContosoDC cyklistické států **Počáteční (Provisioning)** do **ukončení uživatelem**, **spuštění**, **spuštění (Provisioning)**a nakonec **spuštění**.

Teď je úspěšně zřízení Datacentrum serveru. Pak nakonfigurujete domény Active Directory na tomto serveru řadiče domény.

## <a name="configure-the-domain-controller"></a>Konfigurace řadiče domény

V následujících krocích konfigurace ContosoDC počítače jako řadiče domény corp.contoso.com.

1. Na portálu vyberte **ContosoDC** počítač. Na kartě **řídicího panelu** klikněte na **Připojit** k otevření souboru RDP pro přístup ke vzdálené ploše.

    ![Připojení k počítači Vritual](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)

1. Přihlaste se pomocí svého nakonfigurované účet (**\AzureAdmin**) a heslo správce (**Contoso! 000**).

1. Ve výchozím nastavení má být zobrazena na řídicím panelu **Správce serveru** .

1. Klikněte na odkaz **Přidat rolí a funkcí** na řídicím panelu.

    ![Průzkumník serveru přidat role](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)

1. Až se dostanete na části **Role serveru** vyberte **Další** .

1. Vyberte role **Active Directory Domain Services** a **Serveru DNS** . Po zobrazení výzvy, přidejte jakékoli další funkce vyžadované tyto role.

    >[AZURE.NOTE] Zobrazí se vám ověření upozornění, že je žádné statické IP adresy. Pokud testujete konfiguraci, klikněte na pokračovat. Pro výrobní scénáře [pomocí prostředí PowerShell nastavovaná statickou IP adresu počítače řadiče domény](./virtual-network/virtual-networks-reserved-private-ip.md).

    ![Dialogové okno role přidat](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)

1. Až se dostanete **potvrzení** oddíl, klikněte na tlačítko **Další** . Zaškrtněte políčko **automaticky v případě potřeby Restartujte cílový server** .

1. Klikněte na **instalovat**.

1. Po funkce dokončit instalaci, vraťte se do řídicího panelu **Správce serveru** .

1. Možnost nové **služby AD DS** v podokně na levé straně.

1. Klikněte na **Další** odkaz na žlutém panelu upozornění.

    ![AD DS při pokusu o OM serveru DNS](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)

1. Ve sloupci **Akce** obrazovky s dialogem **Všechny podrobnosti úkolu serveru** klikněte na **Přenést tomto serveru řadiče domény**.

1. V **Průvodci Active Directory Domain Services konfigurace**použijte následující hodnoty:

  	|Stránky|Nastavení|
|---|---|
|Konfigurace nasazení|**Přidat nové struktuře** = vybrané<br/>**Název kořenové domény** = corp.contoso.com|
|Možnosti řadiče domény|**Heslo** = Contoso! 000<br/>**Potvrďte heslo** = Contoso! 000|

1. Klikněte na **Další** přejdete na jiných stránkách v průvodci. Na stránce **Kontrola předpokladů** zkontrolujte, že se tato zpráva: **všechny základní kontroly předaný úspěšně**. Všimněte si, že byste měli zkontrolovat všechny související zprávy upozornění, ale je možné, pokračujte instalovat.

1. Klikněte na **instalovat**. Virtuální počítač **ContosoDC** bude automaticky restartovat.

## <a name="configure-domain-accounts"></a>Konfigurace účty domény

Další kroky konfigurace účtů Active Directory (AD) pro pozdější použití.

1. Přihlaste se zpátky k počítači **ContosoDC** .

1. Ve **Správci serveru** vyberte **Nástroje** a potom klikněte na **Centrum pro správu služby Active Directory**.

    ![Centrum pro správu služby Active Directory](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)

1. V **Centrum pro správu služby Active Directory** vyberte **corp (místní)** v levém podokně.

1. V pravém podokně **úkoly** vyberte **Nový** a potom klikněte na **uživatele**. Použijte následující nastavení:

  	|Nastavení|Hodnota|
|---|---|
|**Křestní jméno**|Instalace|
|**SamAccountName uživatele**|Instalace|
|**Heslo**|Contoso! 000|
|**Potvrďte heslo**|Contoso! 000|
|**Další možnosti hesel**|Vybraná|
|**Nikdy vypršení platnosti hesla**|Zaškrtnuté políčko|

1. Klikněte na **OK** vytvořte **instalovat** uživatel. Tento účet se používá ke konfiguraci překlopení obrázku a ve skupině dostupnost.

1. Vytvoření dvou dalších uživatelů se stejným způsobem: **CORP\SQLSvc1** a **CORP\SQLSvc2**. Tyto účty se použije pro instance serveru SQL Server. Pak budete muset dát **CORP\Install** potřebná oprávnění ke konfiguraci Windows služby převzetí clusterů (WSFC).

1. Na stránce **Centrum pro správu služby Active Directory**vyberte **corp (místní)** v levém podokně. Klikněte v pravém podokně **úkoly** klikněte na **Vlastnosti**.

    ![Vlastnosti CORP uživatele](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)

1. Vyberte **rozšíření**a potom klikněte na tlačítko **Upřesnit** na kartě **zabezpečení** .

1. V dialogovém okně **Upřesnit nastavení zabezpečení corp** . Klikněte na **Přidat**.

1. Klepněte na tlačítko **Vybrat objekt zabezpečení**. Vyhledejte **CORP\Install**. Klikněte na **OK**.

1. Vyberte oprávnění **Číst všechny vlastnosti** a **Vytvořit počítač objektů** .

    ![Corp uživatelská oprávnění](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)

1. Klikněte na tlačítko **OK**a potom klikněte na **OK** . Zavřete okno Vlastnosti corp.

Teď dokončení konfigurace služby Active Directory a objektů uživatele vytvoříte tři VMs SQL serveru a připojte k této domény.

## <a name="create-the-sql-server-vms"></a>Vytvoření VMs serveru SQL

Dále vytvořte tři VMs, včetně clusteru WSFC a dvě VMs SQL serveru. Vytvoření u každé VMs, vraťte se do portálu Azure klasické, klikněte na **Nový**, **Výpočet**, **virtuálního počítače**a pak **Z Galerie**. Potom vám pomůže s vytvářením VMs pomocí šablony v následující tabulce.

|Stránky|VM1|VM2|VM3|
|---|---|---|---|
|Vyberte operačního systému virtuálního počítače|**Windows serveru 2012 R2 datacentru**|**SQL Server 2014 RTM Enterprise**|**SQL Server 2014 RTM Enterprise**|
|Konfigurace virtuálního počítače|**Datum vydání verze** = (nejnovější)<br/>**Název počítače virtuální** = ContosoWSFCNode<br/>**VRSTVY** = STANDARD<br/>**Velikost** = A2 (2 jádra)<br/>**Nové uživatelské jméno** = AzureAdmin<br/>**Nové heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000|**Datum vydání verze** = (nejnovější)<br/>**Název počítače virtuální** = ContosoSQL1<br/>**VRSTVY** = STANDARD<br/>**Velikost** a3 (4 jádra)<br/>**Nové uživatelské jméno** = AzureAdmin<br/>**Nové heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000|**Datum vydání verze** = (nejnovější)<br/>**Název počítače virtuální** = ContosoSQL2<br/>**VRSTVY** = STANDARD<br/>**Velikost** a3 (4 jádra)<br/>**Nové uživatelské jméno** = AzureAdmin<br/>**Nové heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000|
|Konfigurace virtuálního počítače|**CLOUDOVÁ služba** = název dříve vytvořené jedinečné cloudové služby DNS (ex: ContosoDC123)<br/>**Oblast/SPŘAŽENÍ skupiny/virtuální sítě** = ContosoNET<br/>**Virtuální PODSÍTÍ** = Back(10.10.2.0/24)<br/>**Úložiště účtu** = použití účtu automaticky generované úložiště<br/>**Nastavte dostupnost** = vytvořit dostupné nastavení<br/>**Dostupnost nastavte název** = SQLHADR|**CLOUDOVÁ služba** = název dříve vytvořené jedinečné cloudové služby DNS (ex: ContosoDC123)<br/>**Oblast/SPŘAŽENÍ skupiny/virtuální sítě** = ContosoNET<br/>**Virtuální PODSÍTÍ** = Back(10.10.2.0/24)<br/>**Úložiště účtu** = použití účtu automaticky generované úložiště<br/>**Nastavte dostupnost** = SQLHADR (můžete taky nakonfigurovat dostupnost po vytvořil v počítači. Všechny tři počítače by měl být přiřazen sadu dostupnost SQLHADR.)|**CLOUDOVÁ služba** = název dříve vytvořené jedinečné cloudové služby DNS (ex: ContosoDC123)<br/>**Oblast/SPŘAŽENÍ skupiny/virtuální sítě** = ContosoNET<br/>**Virtuální PODSÍTÍ** = Back(10.10.2.0/24)<br/>**Úložiště účtu** = použití účtu automaticky generované úložiště<br/>**Nastavte dostupnost** = SQLHADR (můžete taky nakonfigurovat dostupnost po vytvořil v počítači. Všechny tři počítače by měl být přiřazen sadu dostupnost SQLHADR.)|
|Možnosti virtuálního počítače|Použití výchozích hodnot|Použití výchozích hodnot|Použití výchozích hodnot|

<br/>

>[AZURE.NOTE] Předchozí konfiguraci navrhne standardní osy virtuálních počítačích, protože základní osy počítačích nepodporují Vyrovnávání zatížení koncové body muset později vytvořit skupiny dostupnosti posluchače. Navíc velikosti počítače doporučovány tady jsou určeny pro testování dostupnost skupiny v Azure VMs. Pro dosažení nejlepších výsledků na výrobní zatížení najdete v článku doporučení pro SQL Server počítače velikosti a konfigurace ve [výkonu doporučené postupy pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md).

Až tři VMs jsou plně zřízení, budete muset připojit k doména **corp.contoso.com** a udělit oprávnění správce CORP\Install počítačích. K tomuto účelu použijte pro každou tři VMs podle těchto kroků.

1. Nejdřív změňte upřednostňované adresu serveru DNS. Začněte tím, že stahování souboru (RDP) vzdálené plochy každý OM místního adresáře výběrem OM v seznamu a kliknutím na tlačítko **Připojit** . Chcete-li vybrat virtuálního počítače, klikněte na libovolné místo ale na první buňku v řádku, jak je ukázáno v následujícím příkladu.

    ![Stáhněte si soubor RDP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)

1. Spusťte soubor RDP jste stáhli a přihlaste se k OM pomocí vaše nakonfigurované účet (**BUILTIN\AzureAdmin**) a heslo správce (**Contoso! 000**).

1. Jakmile jste se přihlásili, byste měli vidět řídicím panelu **Správce serveru** . V levém podokně klikněte na **Místní Server** .

1. Vyberte odkaz **adresy IPv4 přidělil DHCP protokol IPv6** .

1. V okně **Síťových připojení** vyberte ikonu sítě.

    ![Změna službu upřednostňované DNS OM](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)

1. Na řádku nabídek klikněte na **změnit nastavení tohoto připojení** (v závislosti na velikosti okna, bude pravděpodobně nutné klikněte na dvojitou šipku vpravo tento příkaz zobrazen).

1. Vyberte **protokolu IPv4 (TCP/IPv4)** a klikněte na vlastnosti.

1. Vyberte použít následující DNS server adresy a zadejte **10.10.2.4** **Upřednostňovaný server DNS**.

1. Adresa **10.10.2.4** je adresy přiřazené k OM v podsítě 10.10.2.0/24 v Azure virtuální sítě a že OM je **ContosoDC**. Pokud chcete ověřit **ContosoDC**na IP adresu, použijte **nslookup contosodc** na příkazovém řádku, jak je ukázáno v následujícím příkladu.

    ![Pomocí nástroje NSLOOKUP zjistěte IP adresu pro Datacentrum](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)

1. Klikněte na O**K** a potom **Zavřít** potvrďte změny. Teď se nemůžete připojit OM **corp.contoso.com**.

1. Zpátky v okně **Místního serveru** klikněte na odkaz **pracovní skupina** .

1. V části **Název počítače** klikněte na **změnit**.

1. Zaškrtněte políčko **domény** a do textového pole zadejte **corp.contoso.com** . Klikněte na **OK**.

1. V dialogovém okně místní **Zabezpečení systému Windows** , zadejte přihlašovací údaje pro výchozí účet správce domény (**CORP\AzureAdmin**) a heslo (**Contoso! 000**).

1. Pokud se zobrazí zpráva "Vítá vás doména corp.contoso.com", klikněte na **OK**.

1. Klikněte na tlačítko **Zavřít**a potom klepněte na tlačítko **Restartovat** v dialogovém okně místní.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-vm"></a>Přidání uživatele Corp\Install v jednotlivých OM jako správce:

1. Počkejte, dokud se nerestartuje OM a pak spusťte soubor RDP znovu přihlásit pomocí účtu **BUILTIN\AzureAdmin** OM.

1. Ve **Správci serveru** vyberte **Nástroje**a potom klikněte na **Správa počítače**.

    ![Správa počítače](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)

1. V okně **Správa počítače** rozšíření **místních uživatelů a skupin**a potom vyberte **skupiny**.

1. Poklikejte na skupiny **Administrators** .

1. V dialogovém okně **Vlastnosti správci** klikněte na tlačítko **Přidat** .

1. Zadejte uživatele **CORP\Install**a potom klikněte na **OK**. Po zobrazení výzvy k zadání přihlašovacích údajů, použijte účet **AzureAdmin** s **Contoso! 000** heslo.

1. Klikněte na **OK** zavřete dialogové okno **Vlastnosti Správce** .

### <a name="add-the-failover-clustering-feature-to-each-vm"></a>Přidání funkce **Převzetí clusterů** každý v angličtině.

1. Na řídicím panelu **Správce serveru** klikněte na **Přidat role a funkce**.

1. V **Přidat role a Průvodce funkcí**klikněte na **Další** až se dostanete na stránce **funkce** .

1. Vyberte **převzetí clusterů**. Po zobrazení výzvy přidáte další závislá funkce.

    ![Přidání převzetí clusterů funkce v angličtině](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)

1. Klikněte na tlačítko **Další**a potom na stránce **potvrzení** klikněte na **nainstalovat** .

1. Po dokončení instalace funkce neaktivují **Převzetí clusterů** , klikněte na tlačítko **Zavřít**.

1. Odhlaste se od OM.

1. Opakováním kroků v této části pro všechny tři servery – **ContosoWSFCNode** **ContosoSQL1**a **ContosoSQL2**.

SQL Server VMs jsou teď zřízenou a spuštění, ale instalují se serverem SQL Server s výchozími možnostmi.

## <a name="create-the-wsfc-cluster"></a>Vytvoření WSFC obrázku

V této části vytvoříte WSFC obrázku, který uspořádá dostupnost skupinu, kterou vytvoříte později. Nyní by měli provedení podle následujících pokynů všech tři VMs použijete v WSFC obrázku:

- Plně zřízení v Azure

- Spojené OM do domény

- Přidané **CORP\Install** do skupiny místních správců

- Přidat funkci převzetí clusterů

Vše Toto jsou požadavky na každý OM před připojením ke WSFC clusteru.

Nezapomeňte, že Azure virtuální sítě nechová stejným způsobem jako v místní síti. Je potřeba vytvořit cluster v následujícím pořadí:

1. Vytvoření jednoho uzel clusteru na jeden z uzlů (**ContosoSQL1**).

1. Úprava IP adresu clusteru nepoužitý IP adresu (**10.10.2.101**).

1. Uvedení názvu clusteru online.

1. Přidání dalších uzly (**ContosoSQL2** a **ContosoWSFCNode**).

Postupujte podle pokynů k provedení těchto úkolů, které plně nakonfiguruje clusteru.

1. Spusťte soubor RDP pro **ContosoSQL1** a přihlaste se pomocí účtu domény **CORP\Install**.

1. Na řídicím panelu **Správce serveru** vyberte **Nástroje**a potom klikněte na **Správce**.

1. V levém podokně klikněte pravým tlačítkem na **Správce**a potom klikněte na **vytvořit clusteru**, jak je ukázáno v následujícím příkladu.

    ![Vytvoření obrázku](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)

1. V průvodci vytvořit clusteru vytvoříte clusteru jeden uzel krokování stránky s nastavením níže:

  	|Stránky|Nastavení|
|---|---|
|Než začnete|Použití výchozích hodnot|
|Vyberte servery|Zadejte **ContosoSQL1** **Zadejte název serveru** a klikněte na **Přidat**|
|Upozornění ověření|Vyberte **číslo nevyžadují podpory společnosti Microsoft pro daný cluster a tedy nechcete spouštět testy ověření. Po klepnutí na tlačítko Další pokračovat ve vytváření clusteru**.|
|Přístupový bod pro správu clusteru|Typ **Cluster1** do pole **název obrázku**|
|Potvrzení|Pokud používáte prostor úložiště ve výchozím nastavení použít. Viz poznámka sledovat v této tabulce.|

    >[AZURE.WARNING] Pokud používáte [Úložiště mezery](https://technet.microsoft.com/library/hh831739), která seskupuje více disků do fondu úložiště, musíte zrušte zaškrtnutí políčka **Přidat všechny měl úložiště clusteru** na stránce **potvrzení** . Pokud není zrušte zaškrtnutí políčka tuto možnost, bude virtuálních disků odpojit během procesu clusteru. V důsledku toho také se nezobrazí ve Správci disku nebo Průzkumníka dokud prostor úložiště jsou odebrány z obrázku a znovu připevněn pomocí Powershellu.

1. V levém podokně rozbalte **Správce**a potom klikněte na **Cluster1.corp.contoso.com**.

1. V prostředním podokně přejděte dolů části **Obrázku základní prostředky** a rozbalte položku **Název: Clutser1** podrobnosti. Měli byste vidět **název** a **IP adresu** zdrojů ve stavu **Neúspěšné** . Prostředek IP adresy nemůžete do online režimu, protože clusteru je přiřazené stejné IP adresu jako u počítače, což je duplicitní adresu.

1. Klikněte pravým tlačítkem nezdařeném uložení zdroje **IP adresa** a potom klikněte na **Vlastnosti**.

    ![Vlastnosti obrázku](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)

1. Vyberte **Statické IP adresa** a zadejte **10.10.2.101** do textového pole Adresa. Potom klikněte na **OK**.

1. V části **Základní zdroje obrázku** , klikněte pravým tlačítkem myši **Název: Cluster1** a klikněte na **Přepnout do režimu Online**. Potom, počkejte, než jsou oba zdroje online. Název zdroje obrázku vycházejí online, aktualizuje server Datacentrum s novým účtem počítače AD. Tento účet AD se použijí k spuštění služby skupiny skupinový dostupnost později.

1. Nakonec přidáte zbývající uzly clusteru. Ve stromovém zobrazení prohlížeče **Cluster1.corp.contoso.com** pravým tlačítkem klikněte na **Přidat uzel**, jak je ukázáno v následujícím příkladu.

    ![Přidejte clusteru uzel](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)

1. V dialogovém okně **Průvodce přidáním uzel**klikněte na **Další**. Na stránce **Vyberte servery** dodejte **ContosoSQL2** a **ContosoWSFCNode** seznamu stisknutím kláves název serveru v poli **název serveru Enter** a potom kliknete na **Přidat**. Až budete hotovi, klikněte na **Další**.

1. Na stránce **Ověření upozornění** klikněte na **Ne** (ve scénáři výrobní proveďte ověření testů). Potom klikněte na **Další**.

1. Na stránce **potvrzení** klikněte na **Další** přidat uzly.

    >[AZURE.WARNING] Pokud používáte [Úložiště mezery](https://technet.microsoft.com/library/hh831739), která seskupuje více disků do fondu úložiště, musíte zrušte zaškrtnutí políčka **Přidat všechny měl úložiště clusteru** . Pokud není zrušte zaškrtnutí políčka tuto možnost, bude virtuálních disků odpojit během procesu clusteru. V důsledku toho také se nezobrazí ve Správci disku nebo Průzkumníka dokud prostor úložiště jsou odebrány z obrázku a znovu připevněn pomocí Powershellu.

1. Jakmile uzly se přidají do clusteru, klikněte na **Dokončit**. Správce teď byste měli vidět, že svůj cluster má tři uzly a seznam v kontejneru **uzlů** .

1. Odhlaste se od vzdálené plochy relace.

## <a name="prepare-the-sql-server-instances-for-availability-group"></a>Příprava skupiny dostupnosti instance serveru SQL

V této části bude provádět následující na **ContosoSQL1** i **contosoSQL2**:

- Přidání přihlašovací jméno pro **NT AUTHORITY\System** s potřebným oprávněním nastavit výchozí instanci systému SQL Server

- Přidání **CORP\Install** jako členem role pro výchozí instanci systému SQL Server

- Otevřete brány firewall pro vzdáleného přístupu SQL serveru

- Povolit funkci vždy na skupiny dostupnosti

- Změna účtu služby SQL Server **CORP\SQLSvc1** a **CORP\SQLSvc2**, respektive

V libovolném pořadí můžete provést tyto akce. Však kroků provede jednotlivými jejich pořadí. Postupujte podle pokynů pro **ContosoSQL1** a **ContosoSQL2**:

1. Pokud jste se nepřihlásili mimo vzdálené plochy relace pro OM, přihlaste se teď.

1. Spusťte soubory RDP **ContosoSQL1** a **ContosoSQL2** a přihlaste se jako **BUILTIN\AzureAdmin**.

1. Nejdřív přidejte **NT AUTHORITY\System** přihlášení SQL serveru a s potřebným oprávněním. Spusťte **SQL Server Management Studio**.

1. Klikněte na **Připojit** k připojení k výchozí instanci systému SQL Server.

1. V programu **Průzkumník objektů**rozbalte **zabezpečení**a potom rozbalte položku **přihlášení**.

1. Klikněte pravým tlačítkem myši přihlášení **NT AUTHORITY\System** a klikněte na **Vlastnosti**.

1. Na stránce **Securables** místního serveru vyberte **grant (udělit)** pro následující oprávnění a klikněte na **OK**.

    - Změnit všechny skupiny dostupnosti

    - Připojení SQL

    - Zobrazení stavu serveru

1. Dále přidejte **CORP\Install** jako **členem** role pro výchozí instanci systému SQL Server. V programu **Průzkumník objektů**klikněte pravým tlačítkem myši **přihlášení** a klikněte na **Nové přihlášení**.

1. Zadejte **CORP\Install** **přihlašovací jméno**.

1. Na stránce **Role serveru** vyberte **členem**. Potom klikněte na **OK**. Po vytvoření přihlášení uvidíte rozbalením **přihlášení** v **Prohlížeči objektů**.

1. Dále vytvořte pravidla brány firewall pro systém SQL Server. Na obrazovce **Start** spusťte **Brána Windows Firewall s pokročilým zabezpečením**.

1. V levém podokně vyberte **Příchozí pravidla**. V pravém podokně klikněte na **Nové pravidlo**.

1. Na stránce **Typ pravidla** vyberte **Program**a potom klikněte na tlačítko **Další**.

1. Na stránce **Program** vyberte **cesta k tomuto programu** a zadejte %ProgramFiles%\Microsoft **SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** v textovém poli (pokud jsou pomocí těchto pokynů, ale pomocí SQL Server 2012, SQL Server adresář, který je **MSSQL11. MSSQLSERVER**). Klepněte na tlačítko **Další**.

1. Na stránce **Akce** zachovat **Povolit připojení** vybranou a klikněte na tlačítko **Další**.

1. Na stránce **profilu** přijměte výchozí nastavení a klikněte na tlačítko **Další**.

1. Na stránce **název** zadejte název pravidla, třeba **SQL Server (pravidlo programu)** do pole **název** a pak klikněte na tlačítko **Dokončit**.

1. Potom povolit funkci **Vždy na skupiny dostupnosti** . Na obrazovce **Start** spusťte **Správce konfigurace systému SQL Server**.

1. Ve stromové struktuře prohlížeče klikněte na **Služby SQL Server**, a pak klikněte pravým tlačítkem myši služby **SQL Server (MSSQLSERVER)** a klikněte na **Vlastnosti**.

1. Klikněte na kartu **Vždy na dostupnost** a pak vyberte **Povolit vždy na dostupnost skupiny**, jak je ukázáno v následujícím příkladu a potom klikněte na **použít**. Klikněte na tlačítko **OK** v dialogovém okně místní a zatím nezavírejte okno Vlastnosti. Služba SQL Server restartuje po změně účet služby.

    ![Vždy povolit na dostupnost skupiny](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)

1. Pak můžete změnit účet služby SQL Server. Klikněte **Na** kartu přihlášení, a pak zadejte **CORP\SQLSvc1** (pro **ContosoSQL1**) nebo **CORP\SQLSvc2** (pro **ContosoSQL2**) do pole **Název účtu**a pak zadejte a potvrďte heslo a klikněte na tlačítko **OK**.

1. V okně místní klepnutím na tlačítko **Ano** znovu spustit službu SQL Server. Po restartování služby SQL Server změny provedené v okně Vlastnosti jsou platné.

1. Odhlaste se od VMs.

## <a name="create-the-availability-group"></a>Vytvoření skupiny dostupnosti

Teď jste připraveni Konfigurace dostupnosti skupiny. Tady je přehled co bude dělat:

- Vytvořit novou databázi (**MyDB1**) na **ContosoSQL1**

- Úplné zálohování a transakce protokolu zálohy databáze

- Obnovení úplné a přihlaste zálohy **ContosoSQL2** s parametrem **NORECOVERY**

- Vytvoření skupiny dostupnosti (**AG1**) s synchronní potvrdit, automatické převzetí a čitelný sekundární repliky

### <a name="create-the-mydb1-database-on-contososql1"></a>Vytvoření databáze MyDB1 na ContosoSQL1:

1. Pokud jste se ještě nepřihlásili mimo relace vzdálené plochy **ContosoSQL1** a **ContosoSQL2**, přihlaste se teď.

1. Spusťte soubor RDP pro **ContosoSQL1** a přihlaste se jako **CORP\Install**.

1. V **Průzkumníkovi souborů**klikněte v části * *C:\**, vytvořte adresář s názvem * *záložní**. Použijete tento adresář pomocí zálohování a obnovení databáze.

1. Klikněte pravým tlačítkem myši na nový adresář, najeďte myší na **sdílet s**a pak klikněte na **konkrétní lidi**, jak je ukázáno v následujícím příkladu.

    ![Vytvoření složky](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)

1. Přidání **CORP\SQLSvc1** a udělit oprávnění **Pro čtení i zápis** a pak přidat **CORP\SQLSvc2** mu oprávnění **ke čtení** , jak je ukázáno v následujícím příkladu a pak klikněte na **sdílet**. Po dokončení procesu pro sdílení souborů klikněte na tlačítko **Hotovo**.

    ![Udělení oprávnění pro složky](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)

1. Dále vytvořte databázi. V nabídce **Start** spuštění **SQL Server Management Studio**a potom klikněte na **Připojit** k připojení k výchozí instanci systému SQL Server.

1. V **Prohlížeči objektů**klikněte pravým tlačítkem myši **databáze** a klikněte na **Nové databáze**.

1. Do pole **název databáze**zadejte **MyDB1**a potom klikněte na tlačítko **OK**.

### <a name="take-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Prohlédněte plné zálohu MyDB1 a obnovit ContosoSQL2:

1. Pak můžete využít úplný záložní databáze. V **Prohlížeči objektů**rozbalte **databází**, a pak klikněte pravým tlačítkem myši **MyDB1**, a pak přejděte na **úkoly**a poté na možnost **Zálohovat**.

1. V části **zdroj** zachovat **zálohování typ** nastavený na **celé**. V části **cílový** **Odebrat** kliknutím na tlačítko výchozí cestu k souboru záložního souboru.

1. V části **cílový** klikněte na **Přidat**.

1. Do textového pole **název souboru** zadejte ** \\ContosoSQL1\backup\MyDB1.bak**. Potom klikněte na tlačítko **OK**a potom klikněte na **OK** znova zálohovat databázi. Po dokončení operace zálohování, klikněte na **OK** zavřete dialogové okno.

1. Pak můžete využít transakční protokol záložní databáze. V **Prohlížeči objektů**rozbalte **databází**, a pak klikněte pravým tlačítkem myši **MyDB1**, a pak přejděte na **úkoly**a poté na možnost **Zálohovat**.

1. **Zálohování** type vyberte **Transakční protokol**. Zachovat cesta k souboru **cíl** nastavit jeden dříve zadané a klikněte na **OK**. Po dokončení operace zálohování, znovu klikněte na **OK** .

1. Pak můžete obnovit celé a transakční protokol zálohování na **ContosoSQL2**. Spusťte soubor RDP pro **ContosoSQL2** a přihlaste se jako **CORP\Install**. Nechte vzdálené plochy relace pro **ContosoSQL1** otevřít.

1. V nabídce **Start** spuštění **SQL Server Management Studio**a potom klikněte na **Připojit** k připojení k výchozí instanci systému SQL Server.

1. V **Prohlížeči objektů**klikněte pravým tlačítkem myši **databáze** a klikněte na **Obnovit databázi**.

1. V části **zdroj** vyberte **zařízení**a kliknutím **** tlačítko.

1. V seznamu **Vyberte zálohovací zařízení**klikněte na **Přidat**.

1. Do pole umístění souboru zálohy, zadejte \\ContosoSQL1\backup, klikněte na příkaz Aktualizovat, vyberte MyDB1.bak, a pak klikněte na tlačítko OK a potom klikněte na OK. Teď byste měli vidět celou zálohování a protokolu zálohy zálohování nastaví obnovit podokno.

1. Přejděte na stránku možnosti a pak vyberte obnovit s NORECOVERY ve stavu obnovení a klikněte na OK obnovení databáze. Po dokončení obnovení kliknutím na OK.

### <a name="create-the-availability-group"></a>Dostupnost skupinu vytvoříte takto:

1. Vraťte se zpátky ke vzdálené ploše relaci **ContosoSQL1**. V **Prohlížeči objektů** v SSMS **Vždy na dostupnost** pravým tlačítkem klikněte na položku **Průvodce vytvořením skupiny dostupnost**, jak je ukázáno v následujícím příkladu.

    ![Spuštění Průvodce vytvořením dostupnost skupiny](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)

1. Na stránce **Úvod** klikněte na **Další**. Na stránce **Zadejte název skupiny dostupnost** **AG1** zadejte do pole **název skupiny dostupnost**a potom na tlačítko **Další** .

    ![Průvodce novým AG, zadejte název AG](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)

1. Na stránce **Výběr databázích** vyberte **MyDB1** a klikněte na tlačítko **Další**. Databáze splňuje požadavky pro skupinu dostupnost vzhledem k tomu, že jste provedli alespoň jeden úplné zálohování na určené primární otevřené.

    ![Průvodce novým AG vyberte databází](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)

1. Na stránce **Zadejte repliky** klikněte na **Přidat otevřené**.

    ![Průvodce novým AG určit repliky](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)

1. Dialogové okno **připojit k serveru** uvidí. Zadejte **ContosoSQL2** v poli **název serveru**a pak klikněte na **Připojit**.

    ![Průvodce novým AG připojení k serveru](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)

1. Zpět na stránce **Zadejte repliky** teď byste měli vidět **ContosoSQL2** uvedené v **Dostupné repliky**. Konfigurace replik, jak je ukázáno v následujícím příkladu. Až budete hotovi, klikněte na **Další**.

    ![Průvodce novým AG určete repliky dokončeno)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)

1. Na stránce **Vyberte počáteční synchronizace dat** vyberte **pouze připojení** a klikněte na tlačítko **Další**. Synchronizace dat už mít až pořídili celé a transakce zálohování **ContosoSQL1** a obnovit na **ContosoSQL2**provést ručně. Pozor, abyste zálohování a obnovení databáze a vyberte **úplné** chcete, aby dostupnost Průvodce novou skupinou provést synchronizaci dat můžete místo toho můžete. Však nedoporučuje obrovské databází, které se nacházejí v některých společnostech.

    ![Průvodce novým AG vyberte synchronizace počáteční dat](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)

1. Na stránce **ověření** domény v klikněte na **Další**. Tato stránka by měla vypadat podobně jako níže. Protože nenakonfigurovali posluchače dostupnost skupina je upozornění pro posluchače konfiguraci. Toto upozornění můžete ignorovat, protože tento kurz není konfigurace posluchače. Abyste mohli nakonfigurovat posluchače po dokončení tohoto kurzu, přečtěte si téma [Konfigurace posluchače ILB skupin vždy na dostupnost v Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

    ![Průvodce novým AG, ověření](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)

1. Na stránce **Souhrn** klikněte na tlačítko **Dokončit**a pak počkejte Průvodce nakonfiguruje novou skupinu dostupnosti. Na stránce **Průběh** kliknete **Podrobnosti** a zobrazit podrobný postup. Po dokončení průvodce zkontrolujte stránky **výsledků** ověřit, že se skupina dostupnost je úspěšně, jak je ukázáno v následujícím příkladu a potom klikněte na **Zavřít** zavřete průvodce.

    ![Průvodce novým AG výsledků](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)

1. V **Prohlížeči objektů**rozbalte **Vždy na dostupnost**, pak rozbalte **Skupiny dostupnosti**. Teď byste měli vidět novou skupinu dostupnost v tomto kontejneru. Klikněte pravým tlačítkem myši **AG1 (primární)** a klikněte na **Zobrazit řídicí panel**.

    ![Zobrazení řídicího panelu AG](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)

1. **Vždy na řídicí panel** by měla vypadat podobně jako takové, jaké vidíte níže. Uvidíte replik, převzetí způsob každého otevřené a stav synchronizace.

    ![Řídicí panel AG](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)

1. Vraťte se do **Správce serveru**, vyberte **Nástroje**a potom spusťte **Správce**.

1. Rozbalte **Cluster1.corp.contoso.com**a potom rozbalte položku **Services a aplikace**. Vyberte **role** a poznamenejte si vytvořené roli **AG1** dostupnost skupiny. Všimněte si, že AG1 neměl Libovolná IP adresa tak, že databázi, kterou můžete připojují klienti ke skupině dostupnost, protože nenakonfigurovali posluchače. Můžete se připojit přímo do primární uzel pro operace pro čtení i zápis a uzel vedlejší jen pro čtení dotazů.

    ![AG v správce](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

>[AZURE.WARNING] Nesnažte selhání skupiny dostupnosti z správce. Všechny operace překlápění by měl provést v rámci **Vždy na řídicí panel** v SSMS. Další informace najdete v článku [omezení na pomocí WSFC správce se skupinami dostupnosti](https://msdn.microsoft.com/library/ff929171.aspx).

## <a name="next-steps"></a>Další kroky
Jste teď úspěšně implementovali SQL Server vždy na vytvořením skupiny dostupnosti v Azure. Abyste mohli nakonfigurovat posluchače pro tuto skupinu dostupnost, přečtěte si téma [Konfigurace posluchače ILB skupin vždy na dostupnost v Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Další informace o použití serveru SQL Server Azure najdete v článku [SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).
