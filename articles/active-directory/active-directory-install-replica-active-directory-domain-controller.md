<properties
    pageTitle="Instalace řadiče domény Active Directory otevřené v Azure | Microsoft Azure"
    description="Kurz, která vysvětluje, jak nainstalovat řadiče domény z doménové služby Active Directory místní na Azure virtuálního počítače."
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Instalace řadiče domény Active Directory otevřené v Azure virtuální sítě

Toto téma ukazuje, jak nainstalovat další řadiče domény (označovaná taky jako otevřené řadiče domény) pro místní domény Active Directory na virtuálních počítačích Azure (VMs) v Azure virtuální sítě.

Možná by vás zajímaly tyto souvisejících témat:

-  Volitelně můžete nainstalovat novou doménové služby Active Directory Azure virtuální síti se systémem. Tyto kroky najdete v článku [Instalace nových struktura služby Active Directory Azure virtuální síti se systémem](../active-directory/active-directory-new-forest-virtual-machine.md).
-  Koncepční o instalaci v síti virtuální Azure Active Directory Domain Services (AD DS) najdete v článku [pokyny pro nasazení systému Windows služby Active Directory Server na virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).


## <a name="scenario-diagram"></a>Scénář diagramu

V tomto scénáři muset externím uživatelům přístup k aplikacím, které spouštět na serverech doméně. VMs, spusťte aplikaci servery a řadiče domény otevřené nainstalovaných v Azure virtuální sítě. Virtuální sítě jde připojit k místní síti tak, že připojení [VPN k webu](../vpn-gateway/vpn-gateway-site-to-site-create.md) , jak je vidět v následujícím diagramu nebo použijete [ExpressRoute](../expressroute/expressroute-locations-providers.md) pro rychlejší připojení.

Servery aplikace a budou řadiče domény jsou používaný v rámci samostatné cloudovým službám k distribuci výpočetním zpracování a [dostupnosti sady](../virtual-machines/virtual-machines-windows-manage-availability.md) vyšší odolnost proti chybám.
Budou řadiče domény replikovat navzájem a s místní řadiče domény pomocí replikace služby Active Directory. Je zapotřebí žádné synchronizace nástroje.

![Diagram řadiči poznámek otevřené služby Active Directory domain Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Vytvoření webu služby Active Directory pro Azure virtuální sítě

Je dobré vytvořit web ve službě Active Directory, které představuje oblast sítě odpovídající k virtuální sítě. Které optimalizovat ověřování, replikace a další operace Datacentrum umístění. Následující postup popisuje vytvoření webu a další informace najdete v článku [Přidání nového webu](https://technet.microsoft.com/library/cc781496.aspx).

1. Otevření služby Active Directory webům a službám: **Správce serveru** > **Nástroje** > **služby Active Directory webům a službám**.
2. Vytvoření webu pro oblasti, které jste vytvořili Azure virtuální sítě: klikněte na **weby** > **Akce** > **Nový web** > zadejte název nového webu, třeba západní cz Azure > Vybrat odkaz webu > **OK**.
3. Vytvoření podsítě a přidružit nového webu: poklikejte na **weby** > klikněte pravým tlačítkem myši **podsítí** > **nové podsítě** > zadejte rozsah IP adres virtuální sítě (například 10.1.0.0/16 v diagramu scénář) > vyberte nový web Azure > **OK**.

## <a name="create-an-azure-virtual-network"></a>Vytvoření Azure virtuální sítě

1. V [Azure klasický portálu](https://manage.windowsazure.com), klikněte na **Nový** > **Síťové služby** > **Virtuální sítě** > **Vytvořit vlastní** a pomocí následující hodnoty dokončete průvodce.

    Na této stránce průvodce...  | Zadejte tyto hodnoty
    ------------- | -------------
    **Virtuální nastavení sítě**  | <p>Název: Zadejte název virtuální sítě, například WestUSVNet.</p><p>Oblast: Vyberte nejbližší oblast.</p>
    **Připojení DNS a VPN**  | <p>Servery DNS: Zadejte název a IP adresu jeden nebo více serverů DNS místní.</p><p>Připojení: Vyberte **Konfigurovat VPN k webu**.</p><p>Místní sítě: Zadejte nové místní síti.</p><p>Pokud používáte ExpressRoute místo VPN, přečtěte si téma [Konfigurace připojení k ExpressRoute prostřednictvím poskytovatele Exchange](../expressroute/expressroute-locations-providers.md).</p>
    **Připojení k webu**  | <p>Název: Zadejte název místní síti.</p><p>Virtuální privátní sítě zařízení IP adresa: zadejte veřejnou IP adresu zařízení, které se připojí k virtuální sítě. Zařízení VPN nebyla nalezena za službou NAT.</p><p>Adresa: Zadejte rozsahy adres v místní síti (například 192.168.0.0/16 v diagramu scénář).</p>
    **Virtuální sítě adresu mezer**  | <p>Adresní prostor: Zadejte rozsah IP adres pro VMs, které chcete spustit v Azure virtuální sítě (například 10.1.0.0/16 v diagramu scénář). Tato oblast adresu nemůže překrývat pomocí rozsahů adres v místní síti.</p><p>Podsítí: Zadejte název a adresa podsítě serverů aplikace (například Frontend, 10.1.1.0/24) a budou řadiče domény (třeba back-end, 10.1.2.0/24).</p><p>Klikněte na **Přidat podsítě brány**.</p>

2. Pak budete nakonfigurujte bránu virtuální sítě vytvořit zabezpečené připojení VPN k webu. Pokyny najdete v článku [Konfigurace Brána virtuální sítě](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .
3. Vytvoření připojení VPN k webu mezi novou virtuální síť a zařízení s místním VPN. Pokyny najdete v článku [Konfigurace Brána virtuální sítě](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .


## <a name="create-azure-vms-for-the-dc-roles"></a>Vytvoření Azure VMs Datacentrum rolí

Opakujte následujících kroků můžete vytvořit VMs hostovat roli Datacentrum podle potřeby. Nasadíte aspoň dva virtuální řadiče domény, aby zadal odolnost proti chybám a redundance. Pokud Azure virtuální síť obsahuje aspoň dva řadiče domény, které jsou podobně nakonfigurovány (to znamená, jsou obě globálních běhu DNS server a ani obsahuje roli FSMO a tak dál) umístěte VMs spuštěné můžou být řadiče domény dostupnost pro vyšší odolnost proti chybám.
Pokud chcete vytvořit VMs pomocí prostředí Windows PowerShell místo uživatelského rozhraní, najdete v článku [použití Azure vytvoření a nastavení serveru s Windows virtuálních počítačích](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. V [Azure klasické portál](https://manage.windowsazure.com), klikněte na **Nový** > **Výpočet** > **virtuálního počítače** > **Z Galerie**. Dokončete průvodce pomocí následující hodnoty. Pokud doporučovány nebo vyžaduje jinou hodnotu, přijměte výchozí hodnotu pro nastavení.

    Na této stránce průvodce...  | Zadejte tyto hodnoty
    ------------- | -------------
    **Zvolte obrázek**  | Windows serveru 2012 R2 datacentru
    **Konfigurace virtuálního počítače**  | <p>Virtuální počítač název: Zadejte název jediný štítek (například AzureDC1).</p><p>Nové uživatelské jméno: Zadejte jméno uživatele. Tento uživatel bude členem skupiny místních správců OM. Budete potřebovat tento název se přihlásit k OM poprvé. Předdefinované účet s názvem správce nebude fungovat.</p><p>Nové heslo a potvrďte: Heslo zadejte</p>
    **Konfigurace virtuálního počítače**  | <p>Cloudové služby: Zvolte <b>vytvořit nové Cloudová služba</b> pro první OM a vyberte tento stejný název služby cloudu při vytváření další VMs poskytujících roli řadiče domény.</p><p>Název DNS cloudové služby: Globálně jedinečný název zadejte</p><p>Oblast/spřažení skupiny/virtuální sítě: Zadejte název virtuální sítě (například WestUSVNet).</p><p>Účet úložiště: Zvolte <b>použití účtu automaticky generované úložiště</b> pro první OM a pak vyberte této stejný název účtu úložiště při vytváření další VMs poskytujících roli Datacentrum.</p><p>Dostupnost nastavení: Vyberte <b>vytvořit sadu dostupnost</b>.</p><p>Dostupnost nastavte název: Zadejte název pro dostupnosti sady při vytváření prvního OM a vyberte stejný název při vytváření další VMs.</p>
    **Konfigurace virtuálního počítače**  | <p>Vyberte <b>nainstalovat agenta OM</b> a dalších potřebných rozšíření.</p>
2. Připojení k každý OM, který se spustí role serveru Datacentrum disk. Další disk je potřeba k ukládání AD databáze, protokoly a SYSVOL. Určete velikost disku (například 10 GB) a nechejte **Hostitele mezipaměti předvoleb** nastavený na **žádný**. Postup najdete v článku [jak připojit Disk dat do virtuálního počítače Windows](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Po prvním přihlášení bude v angličtině, spusťte **Správce serveru** > **úložiště služby pro soubory a** vytvářet svazku na disku pomocí NTFS.
4. Rezervujte statickou IP adresu pro VMs, které se spustí roli řadiče domény. Rezervovat statickou IP adresu, stáhněte si Microsoft Web Platform Installer a [nainstalujte prostředí PowerShell Azure](../powershell-install-configure.md) a spusťte rutinu Set-AzureStaticVNetIP. Příklad:

    "Get-AzureVM - ServiceName AzureDC1 – název AzureDC1 | Nastavení AzureStaticVNetIP - adresa IP 10.0.0.4 | Aktualizace AzureVM

Další informace o nastavení statické IP adresy najdete v článku [Konfigurace statické interní IP adresu virtuálního počítače](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Instalace služby AD DS na Azure VMs

Přihlaste se k virtuálního počítače a ověřte, že jste připojeni přes připojení VPN nebo ExpressRoute na webu k prostředkům ve vaší místní síti. Nainstalujte na Azure VMs AD DS. Můžete použít stejné proces, který používáte k instalaci další Datacentrum v místní síti (uživatelského rozhraní, prostředí Windows PowerShell nebo souboru odpovědí). Při instalaci služby AD DS zkontrolujte, že zadáte nový hlasitost umístění databáze Reklam, protokoly a SYSVOL. Pokud potřebujete si osvěžit informace o instalaci služby AD DS, přečtěte si téma [Instalace Active Directory Domain Services (úroveň 100)](https://technet.microsoft.com/library/hh472162.aspx) nebo [instalace otevřené Windows serveru 2012 řadiče domény do existující domény (úroveň 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Změna konfigurace serveru DNS pro virtuální sítě

1. [Azure klasické portál](https://manage.windowsazure.com)klikněte na název virtuální sítě a potom klikněte na kartu **Konfigurovat** [překonfigurovat adres IP serverů DNS sítě virtuální](../virtual-network/virtual-networks-manage-dns-in-vnet.md) používat statické IP adresa přiražená otevřené řadiče domény místo IP adresy serverů DNS místní.

2. Abyste měli jistotu, že všechny otevřené Datacentrum VMs virtuální síti se systémem budou nakonfigurována používání serverů DNS na virtuální sítě, klikněte na **virtuálních počítačích**, klikněte ve sloupci stav u každého OM a klikněte na **Restartovat**. Počkejte, dokud OM zobrazí **systém** stav před pokusem o Přihlaste se do něho.

## <a name="create-vms-for-application-servers"></a>Vytvoření VMs pro servery aplikace

1. Opakujte následujících kroků můžete vytvořit VMs ke spuštění aplikace servery. Pokud doporučovány nebo vyžaduje jinou hodnotu, přijměte výchozí hodnotu pro nastavení.

    Na této stránce průvodce...  | Zadejte tyto hodnoty
    ------------- | -------------
    **Zvolte obrázek**  | Windows serveru 2012 R2 datacentru
    **Konfigurace virtuálního počítače**  | <p>Virtuální počítač název: Zadejte název jediný štítek (například AppServer1).</p><p>Nové uživatelské jméno: Zadejte jméno uživatele. Tento uživatel bude členem skupiny místních správců OM. Budete potřebovat tento název se přihlásit k OM poprvé. Předdefinované účet s názvem správce nebude fungovat.</p><p>Nové heslo a potvrďte: Heslo zadejte</p>
    **Konfigurace virtuálního počítače**  | <p>Cloudové služby: Zvolte **vytvořit nové Cloudová služba** pro první OM a vyberte tento stejný název služby cloudu při vytváření další VMs hostujících aplikace.</p><p>Název DNS cloudové služby: Globálně jedinečný název zadejte</p><p>Oblast/spřažení skupiny/virtuální sítě: Zadejte název virtuální sítě (například WestUSVNet).</p><p>Úložiště účet: Zvolte **použití účtu automaticky vygenerovaný úložiště** pro první OM a pak vyberte tento stejný název účtu úložiště při vytváření další VMs hostujících aplikace.</p><p>Dostupnost nastavení: Vyberte **vytvořit sadu dostupnost**.</p><p>Dostupnost nastavte název: Zadejte název pro dostupnosti sady při vytváření prvního OM a vyberte stejný název při vytváření další VMs.</p>
    **Konfigurace virtuálního počítače**  | <p>Vyberte <b>nainstalovat agenta OM</b> a dalších potřebných rozšíření.</p>

2. Po jednotlivých OM máte k dispozici, přihlaste se a připojte k doméně. Ve **Správci serveru**, klikněte na **Místní Server** > **pracovní skupina** > **změnit...** Vyberte **doménu** a zadejte název místní domény. Zadejte přihlašovací údaje uživatele domény a restartujte OM dokončete připojení k doméně.

Pokud chcete vytvořit VMs pomocí prostředí Windows PowerShell místo uživatelského rozhraní, najdete v článku [použití Azure vytvoření a nastavení serveru s Windows virtuálních počítačích](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

Další informace o používání Windows Powershellu najdete v tématu [Začínáme s Azure rutiny](https://msdn.microsoft.com/library/azure/jj554332.aspx) a [Reference pro rutiny Azure](https://msdn.microsoft.com/library/azure/jj554330.aspx).

## <a name="additional-resources"></a>Další zdroje informací

-  [Pokyny pro nasazení systému Windows Server služby Active Directory v Azure virtuálních počítačích](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Jak nahrát existující řadiče domény místního Hyper-V Azure pomocí prostředí PowerShell Azure](http://support.microsoft.com/kb/2904015)
-  [Instalace nové doménové služby Active Directory na Azure virtuální sítě](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure IT Pro IaaS: Základy (01) virtuálního počítače](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) vytváření virtuálních sítí a křížově místní připojení](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure Powershellu](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Rutiny pro správu Azure](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
