<properties
    pageTitle="Instalace struktuře služby Active Directory Azure virtuální síti se systémem | Microsoft Azure"
    description="Kurz, která vysvětluje postup vytvoření nové doménové služby Active Directory na počítač virtuální (OM) v síti virtuální Azure."
    services="active-directory, virtual-network"
    keywords="služby Active directory virtuálního počítače doménové služby active directory instalace služby azure active directory videa "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Instalace nové doménové služby Active Directory na Azure virtuální sítě

Toto téma ukazuje, jak vytvořit nové prostředí služby Active Directory pro Windows Server Azure virtuální síti virtuální počítače (OM) v [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md). V tomto případě Azure virtuální sítě není připojený k místní síti.

Možná by vás zajímaly tyto souvisejících témat:

- Videa, které se zobrazí tyto kroky přečtěte si, [jak instalovat nové služby Active Directory doménové Azure virtuální síti se systémem](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
- Můžete volitelně [Konfigurace sítě VPN na webu](../vpn-gateway/vpn-gateway-site-to-site-create.md) a pak buď nainstalovat strukturu nový nebo rozšíření místních doménové Azure virtuální sítě. Tyto kroky najdete v článku [instalace otevřené Active Directory řadiče domény v síti virtuální Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md).
-  Koncepční o instalaci v síti virtuální Azure Active Directory Domain Services (AD DS) najdete v článku [pokyny pro nasazení systému Windows služby Active Directory Server na virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Scénář diagramu

V tomto scénáři muset externím uživatelům přístup k aplikacím, které spouštět na serverech doméně. Nainstalovaných v vlastní cloudové služby Azure virtuální sítě jsou nainstalovány VMs spuštěné aplikační servery a VMs spuštěné řadiče domény. Jsou taky součástí dostupné pro vyšší odolnost proti chybám.

![Active Directory doménové na virtuálních počítačích v Azure virtuální sítě ][1] 7
## <a name="how-does-this-differ-from-on-premises"></a>Jak to liší od místního?

Není k dispozici stojí rozdíl mezi instalaci řadiče domény na Azure a místních. V následující tabulce jsou uvedené hlavní rozdíly.

Konfigurace...  | Místní  | Azure virtuální sítě
------------- | -------------  | ------------
**IP adresa řadiče domény**  | Přiřazení statické IP adresy ve vlastnostech adaptér sítě   | Spusťte rutinu Set-AzureStaticVNetIP přiřazení statické IP adresy
**Překládání DNS klienta**  | Nastavení upřednostňované a alternativní DNS adresa serveru v síti adaptér vlastností členů domény   | Nastavit adresu serveru DNS vlastnosti virtuálního sítě
**Úložiště databáze Active Directory**  | Pokud chcete změnit umístění úložiště z C:\  | Potřebujete změnit výchozí úložiště z C:\



## <a name="create-an-azure-virtual-network"></a>Vytvoření Azure virtuální sítě

1. Přihlaste se k portálu Azure klasické.
2. Vytvořte virtuální síť. Klepněte na položku **sítě** > **vytvořit virtuální síť**. Dokončete průvodce pomocí hodnoty v následující tabulce.

    Na této stránce průvodce...  | Zadejte tyto hodnoty
    ------------- | -------------
    **Virtuální nastavení sítě**  | <p>Název: Zadejte název virtuální sítě</p><p>Oblast: Zvolte nejbližší oblast</p>
    **DNS a VPN**  | <p>Nechejte prázdné serveru DNS</p><p>Nevybírejte možnost VPN</p>
    **Virtuální sítě adresu mezer**  | <p>Podsítě název: Zadejte název vaší podsítě</p><p>Počáteční IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Vytvoření VMs spuštění řadiče domény a role serveru DNS

Opakujte následujících kroků můžete vytvořit VMs hostovat roli Datacentrum podle potřeby. Nasadíte aspoň dva virtuální řadiče domény, aby zadal odolnost proti chybám a redundance. Pokud Azure virtuální síť obsahuje aspoň dva řadiče domény, které jsou podobně nakonfigurovány (to znamená, jsou obě globálních běhu DNS server a ani obsahuje roli FSMO a tak dál) umístěte VMs spuštěné můžou být řadiče domény dostupnost pro vyšší odolnost proti chybám.

Pokud chcete vytvořit VMs pomocí prostředí Windows PowerShell místo uživatelského rozhraní, najdete v článku [použití Azure vytvoření a nastavení serveru s Windows virtuálních počítačích](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. Na portálu klasické klikněte na **Nový** > **Výpočet** > **virtuálního počítače** > **Z Galerie**. Dokončete průvodce pomocí následující hodnoty. Pokud doporučovány nebo vyžaduje jinou hodnotu, přijměte výchozí hodnotu pro nastavení.

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

## <a name="install-windows-server-active-directory"></a>Nainstalujte službu Active Directory pro Windows Server

Použití stejné rutina [instalaci služby AD DS](https://technet.microsoft.com/library/jj574166.aspx) použít místní (to znamená, můžete používat uživatelské rozhraní, soubor odpovědí nebo prostředí Windows PowerShell). Budete muset zadat přihlašovací údaje správce nainstalovat strukturu nový. Pokud chcete určit umístění databáze služby Active Directory, protokoly a SYSVOL, přejděte umístění úložiště z disku operační systém disku další data, která jste připojili bude v angličtině.

Po dokončení instalace Datacentrum znovu připojit angličtině a přihlaste se k řadiče domény. Nezapomeňte zadat pověření.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>Resetujte server DNS pro Azure virtuální sítě

1. Resetujte nastavení DNS pro předávání na nový Datacentrum/DNS server.
  1. Ve Správci serveru klikněte na **Nástroje** > **DNS**.
  2. Ve **Správci DNS**klikněte pravým tlačítkem myši na název serveru DNS a klikněte na **Vlastnosti**.
  3. Na kartě v části **předávání** klikněte na IP adresu serveru pro předávání a klikněte na **Upravit**.  Vyberte IP adresa a klikněte na **Odstranit**.
  4. Klikněte na **OK** zavřete editor a **Ok** zavřete vlastnosti serveru DNS.
2. Aktualizujte nastavení serveru DNS pro virtuální síť.
  1. Klikněte na **Virtuálních sítí** > poklikejte virtuální sítě jste vytvořili > **Konfigurovat** > **servery DNS**, zadejte název a DIP některého VMs, které běží roli Datacentrum/DNS server a klikněte na **Uložit**.
  2. Vyberte OM a klikněte na **Restartovat** aktivovat OM nakonfigurovat IP adresu serveru DNS, nové nastavení překládání DNS.


## <a name="create-vms-for-domain-members"></a>Vytvoření VMs pro členy domény

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


## <a name="see-also"></a>Viz taky

-  [Jak nainstalovat strukturu nové služby Active Directory na Azure virtuální sítě](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [Pokyny pro nasazení systému Windows Server služby Active Directory v Azure virtuálních počítačích](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [Konfigurace sítě VPN na webu](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Instalace otevřené Active Directory řadiče domény v Azure virtuální sítě](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure IT Pro IaaS: Základy (01) virtuálního počítače](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) vytváření virtuálních sítí a křížově místní připojení](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md)
-  [Instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md)
-  [Azure Powershellu](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Reference pro rutiny Azure](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Nastavení Azure OM statické IP adresy](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [Jak přiřadit statické IP OM Azure](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Instalace nové doménové Active Directory](https://technet.microsoft.com/library/jj574166.aspx)
-  [Úvod do služby Active Directory Domain Services (AD DS) virtualizace (úroveň 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
