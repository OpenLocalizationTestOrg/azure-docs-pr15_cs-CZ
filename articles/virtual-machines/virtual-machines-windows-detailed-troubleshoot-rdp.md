<properties
    pageTitle="Podrobné vzdálené plochy Poradce při potížích s | Microsoft Azure"
    description="Kontrola podrobný postup řešení potíží pro vzdálené plochy chyby které nelze na virtuálních počítačích Windows v Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"
    keywords="Nelze se připojit k vzdálené plochy Poradce při potížích s Vzdálená plocha, nepodaří připojit ke vzdálené ploše, vzdálené plochy chyby vzdálené plochy Poradce při potížích s, vzdálené plochy problémy
"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Podrobné kroky pro řešení potíží připojení ke vzdálené ploše pro Windows VMs v Azure

Tento článek obsahuje podrobný postup řešení potíží Diagnostika a oprava chyb složité ke vzdálené ploše pro serveru s Windows Azure virtuálních počítačích.

> [AZURE.IMPORTANT] K odstranění běžných chyb Vzdálená plocha, zkontrolujte, přečtěte si [článek Základní Poradce při potížích pro vzdálené plochy](virtual-machines-windows-troubleshoot-rdp-connection.md) před pokračováním.

Může dojít ke vzdálené ploše chybovou zprávu, která není vypadat konkrétní chybové zprávy uvedených v [příručce Poradce při potížích základní Vzdálená plocha](virtual-machines-windows-troubleshoot-rdp-connection.md). Tímto postupem zjistit, proč nelze se připojit ke službě RDP OM Azure vzdálené plochy klienta.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Pokud potřebujete další pomoc kdykoli v tomto článku můžete kontaktovat Azure odborníků na [webu MSDN Azure a fórech přetečení zásobníku](https://azure.microsoft.com/support/forums/). Můžete taky můžete poslat taky případ Azure podpory. Přejděte na [Web podpory Azure](https://azure.microsoft.com/support/options/) a klikněte na **Získat podporu**. Informace o používání Azure podpory přečtěte si [Nejčastější dotazy týkající se Microsoft Azure podpory](https://azure.microsoft.com/support/faq/).


## <a name="components-of-a-remote-desktop-connection"></a>Součásti webové části připojení ke vzdálené ploše

Připojení ke vzdálené ploše jsou součástí tyto prvky:

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_0.png)

Než budete pokračovat, mohlo by pomoci zobrazíte mentálně co se změnilo od posledního úspěšné připojení ke vzdálené ploše bude v angličtině. Příklad:

- Změnil veřejnou IP adresu OM nebo cloudové služby, která obsahuje OM (nazývané také virtuální IP adresu [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)). Chyba při RDP může být způsobeno mezipaměť DNS klienta pořád má *původní IP adresu* registrovanou pro název DNS. Vyprázdněte mezipaměť DNS klienta a zkuste se znovu připojit OM. Nebo se zkuste připojit přímo pomocí nového VIP.
- Aplikace jiných výrobců používáte ke správě připojení ke vzdálené ploše místo použití připojení generovaných portálu Azure. Ověřte konfiguraci aplikace obsahuje správné port TCP přenosů Vzdálená plocha. Tento port pro klasické virtuální počítač [Azure portál](https://portal.azure.com), můžete zkontrolovat kliknutím OM Nastavení > Koncové body.


## <a name="preliminary-steps"></a>Příprava

Před pokračováním podrobné Poradce při potížích,

- Kontrola stavu virtuálního počítače v portálu Azure klasické nebo portálu Azure zjevných problémy.
- Postupujte podle [kroků rychlé Oprava běžných chyb RDP v Průvodci základní Poradce při potížích](virtual-machines-windows-troubleshoot-rdp-connection.md#quick-troubleshooting-steps).


Zkuste se znovu připojit k OM prostřednictvím vzdálené plochy po provedení těchto kroků.


## <a name="detailed-troubleshooting-steps"></a>Podrobné návody na řešení potíží

Klient vzdálené plochy nemusí mít přístup službu Vzdálená plocha na OM Azure kvůli problémů v následujících zdrojích:

- [Vzdálené plochy klientského počítače](#source-1-remote-desktop-client-computer)
- [Organizace intranet okraj zařízení](#source-2-organization-intranet-edge-device)
- [Cloud koncový bod služby a získat přístup k seznamu ovládacího prvku (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Skupiny zabezpečení sítě](#source-4-network-security-groups)
- [Serveru s Windows Azure OM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Zdroj 1: Vzdálené plochy klientského počítače

Ověřte, že váš počítač provést připojení ke vzdálené ploše pro jiné místním počítači se systémem Windows.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_1.png)

Pokud není možné, zkontrolujte následující nastavení v počítači:

- Místní nastavení brány firewall blokuje přenosy Vzdálená plocha.
- Místně nainstalovaný klientský software proxy, který způsobuje potíže s připojení ke vzdálené ploše.
- Místně nainstalovaný sledování software, který způsobuje potíže s připojení ke vzdálené ploše sítě.
- Jiné druhy softwaru zabezpečení, které monitorovat data nebo povolit nebo zakázat určité typy přenosů, který způsobuje potíže s připojení ke vzdálené ploše.

V těchto případech dočasně vypnout software a pokuste se připojit k místním počítači pomocí vzdálené plochy. Pokud můžete zjistit příčinu skutečné tímto způsobem, pracujete s správce sítě problém můžete vyřešit tak nastavení software, aby umožňoval připojení ke vzdálené ploše.

## <a name="source-2-organization-intranet-edge-device"></a>Zdroj 2: Organizace intranet okraj zařízení

Ověření, že počítač přímé připojení k Internetu provést připojení ke vzdálené ploše do počítače Azure virtuální.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_2.png)

Pokud nemáte počítač, na kterém má přímé připojení k Internetu, vytvořte a otestujte pomocí nového Azure virtuálního počítače ve službě skupiny nebo cloudového zdroje. Další informace najdete v tématu [Vytvoření virtuálního počítače s Windows v Azure](virtual-machines-windows-hero-tutorial.md). Virtuální počítač a skupina zdroje nebo cloudové služby, můžete odstranit po test.

Pokud vytvoříte připojení ke vzdálené ploše s počítačem přímo připojený k Internetu Zkontrolujte organizace intranet okraj zařízení:

- Vnitřní firewall blokuje HTTPS připojení k Internetu.
- Proxy server zabránění připojení ke vzdálené ploše.
- Průnik zjišťování nebo sledování software systém na zařízeních v síti okraj, který brání připojení ke vzdálené ploše sítě.

Práce s správce sítě problém můžete vyřešit tak nastavení zařízení organizace intranet okraj, aby na základě HTTPS vzdálené plochy připojení k Internetu.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Zdroj 3: Koncový bod služby cloudu a ACL

VMs vytvořené pomocí klasického nasazení modelu zkontrolujte, že jiný OM Azure, která je ve stejném cloudové služby, nebo virtuální sítě můžete provádět připojení ke vzdálené ploše OM Azure.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_3.png)

> [AZURE.NOTE] Pro virtuálních počítačích vytvořené ve Správci zdroje, přejděte ke [Zdroj 4: skupiny zabezpečení sítě](#source-4-network-security-groups).

Pokud nemáte jiného počítače virtuální ve stejném cloudové služby, nebo virtuální sítě, vytvořte ho. Postupujte podle pokynů v tématu [Vytvoření virtuálního počítače s Windows v Azure](virtual-machines-windows-hero-tutorial.md). Odstranění test virtuální počítač po dokončení testu.

Pokud prostřednictvím vzdálené plochy můžete připojit k počítači virtuální ve stejném cloudové služby, nebo virtuální sítě, zkontrolujte tato nastavení:

- Konfigurace koncový bod pro přenos Vzdálená plocha na cílovém OM: soukromé port TCP koncový bod se musí shodovat port TCP, na kterém je přijímá službu Vzdálená plocha OM (výchozí hodnota je 3389).
- ACL koncového bodu přenosy Vzdálená plocha na cílovém OM: ACL umožňují určit povolené nebo odepřen příchozích z Internetu založené na její zdrojová IP adresa. Jsou chybně nakonfigurované ACL můžete zabránit příchozích Vzdálená plocha na koncový bod. Kontrola ACL zajistit, aby příchozí přenosy na veřejnou IP adresy vašeho proxy serveru ani není povolené jiných edge serveru. Další informace najdete v tématu [Co je v seznamu pro řízení přístupu síti (ACL)?](../virtual-network/virtual-networks-acl.md)

Kontrola, pokud koncový bod příčinu problému, odeberte aktuální koncového bodu a vytvořte novou skupinu, výběr náhodné port v oblasti 49152 až 65535 pro číslo portu externí. Další informace najdete v tématu [jak nastavit koncové body do virtuálního počítače](virtual-machines-windows-classic-setup-endpoints.md).

## <a name="source-4-network-security-groups"></a>Zdroje 4: Skupiny zabezpečení sítě

Skupiny zabezpečení sítě povolit přesnější kontrolu nad povolené příchozí a odchozí přenosy. Můžete vytvářet pravidla, která trvá podsítí a cloudových služeb v Azure virtuální sítě. Zkontrolujte pravidla skupiny zabezpečení sítě zajistit, že jsou povoleny Vzdálená plocha přenosy z Internetu:

- Na portálu Azure vyberte svůj OM
- Klikněte na **všechna nastavení** | **Síťová rozhraní** a vyberte rozhraní sítě.
- Klikněte na **všechna nastavení** | **skupiny zabezpečení sítě** a vyberte svoji skupinu zabezpečení sítě.
- Klikněte na **všechna nastavení** | **pravidla příchozí zabezpečení** a ujistěte se, máte pravidlo umožňuje RDP na TCP port 3389.
    - Pokud nemáte pravidla, klikněte na tlačítko **Přidat** pravidlo. Zadejte **TCP** Protocol (protokol) a potom klepněte na **3389** pro oblast cílový port.
    - Ujistěte se, že akce je nastavený na **Povolit** a kliknutím na OK uložte novou příchozí pravidla.


Další informace najdete v tématu [Co je skupina zabezpečení síti (NSG)?](../virtual-network/virtual-networks-nsg.md)

## <a name="source-5-windows-based-azure-vm"></a>Zdroj 5: Serveru s Windows Azure OM

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_5.png)

Použil funkci [balíček diagnostics Azure IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) jestli selhání máte není kvůli Azure samotného virtuálního počítače. Pokud tento balíček diagnostics nedokáže **RDP připojení k Azure OM (vyžaduje restartování)** problém nevyřeší, postupujte podle pokynů v [tomto článku](virtual-machines-windows-reset-rdp.md). Tento článek Obnoví službu Vzdálená plocha na virtuálního počítače:

- Povolte výchozí pravidlo Brána Firewall systému Windows "Vzdálená plocha" (TCP port 3389).
- Povolte připojení ke vzdálené ploše nastavením hodnotu registru HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections na hodnotu 0.

Zkuste vytvořit připojení z počítače znovu. Pokud pořád nemůžete připojit pomocí vzdálené plochy, zkontrolujte následující možným potížím:

- Na cílovém OM není spuštěná služba Vzdálená plocha.
- Službu Vzdálená plocha nesleduje na TCP port 3389.
- Brána Firewall systému Windows nebo jiná místní brána firewall má odchozího pravidla, které brání přenosy Vzdálená plocha.
- Průnik zjišťování nebo monitorování spuštěná na Azure virtuálního počítače k síti způsobuje potíže s připojení ke vzdálené ploše.

Pro VMs vytvořené pomocí klasické nasazení modelu můžete použít vzdálenou relaci Powershellu Azure Azure virtuálního počítače. Nejdřív budete muset nainstalovat certifikát hostitelské služby cloudu virtuálního počítače. Přejděte na [Konfigurovat zabezpečené vzdáleného Powershellu přístup k Azure virtuálních počítačích](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) a stáhněte si soubor skriptu **InstallWinRMCertAzureVM.ps1** do místního počítače.

Pokud jste to ještě neudělali, nainstalujte prostředí PowerShell Azure. Přečtěte si, [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).

Potom otevřete okno příkazového řádku prostředí PowerShell Azure a místo aktuální složky umístění soubor skriptu **InstallWinRMCertAzureVM.ps1** . Spustit skript Powershellu Azure, musíte nastavit zásady správného spuštění. Spusťte příkaz **Get-zásady spouštění** zjistit aktuální úroveň zásad. Informace o nastavení na přijatelnou úroveň najdete v tématu [Zásady nastavení spouštění](https://technet.microsoft.com/library/hh849812.aspx).

Potom vyplňte název Azure předplatného, název služby cloudu a názvu virtuálního počítače (odebrání < a > znaky), a pak spusťte tyto příkazy.

    $subscr="<Name of your Azure subscription>"
    $serviceName="<Name of the cloud service that contains the target virtual machine>"
    $vmName="<Name of the target virtual machine>"
    .\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName

Název správné předplatného dá dostat z vlastnost _SubscriptionName_ zobrazení příkazu **Get-AzureSubscription** . Název služby cloudu virtuálního počítače můžete získat z _Název_služby_ sloupce v zobrazení na příkaz **Get-AzureVM** .

Zkontrolujte, jestli máte nový certifikát. Otevřete modulu snap-in Certifikáty pro aktuálního uživatele a podívejte se do složky **Trusted Root certifikační authorities\certificates v počítači** . Měli byste vidět certifikátu s názvem DNS cloudové služby ve sloupci vystavitel (Příklad: cloudservice4testing.cloudapp.net).

Spusťte dále vzdálenou relaci Powershellu Azure pomocí tyto příkazy.

    $uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
    $creds = Get-Credential
    Enter-PSSession -ConnectionUri $uri -Credential $creds

Po zadání přihlašovací údaje správce platné, měli byste vidět podobnou následující dotaz Azure Powershellu:

    [cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>

První část tento dotaz je název vaší cloudové služby, který obsahuje cílový OM, které by mohly být liší od "cloudservice4testing.cloudapp.net". Nyní můžete vydávat Azure PowerShell příkazy pro tento cloudové služby, pokud chcete zkontrolovat problémy uvedené a opravit konfiguraci.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>Chcete-li ručně opravit vzdálené plochy listening TCP port

Pokud není možné spustit [balíček diagnostics Azure IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) pro tento problém **RDP připojení k Azure OM (vyžaduje restartování)** do příkazového řádku vzdálený Azure PowerShell relace, spusťte tento příkaz.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Vlastnost PortNumber zobrazuje aktuální číslo portu. V případě potřeby změňte Vzdálená plocha port číslo zpět na výchozí hodnotu (3389) pomocí tohoto příkazu.

    Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389

Ověřte, že port změnila na 3389 pomocí tohoto příkazu.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Ukončete vzdálenou relaci Powershellu Azure pomocí tohoto příkazu.

    Exit-PSSession

Ověřte, že Vzdálená plocha koncový bod pro OM Azure taky používá port TCP. 3398 jako interní port. Restartujte OM Azure a znovu se připojit ke vzdálené ploše.


## <a name="additional-resources"></a>Další zdroje informací

[Azure balíček diagnostics IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[Jak obnovit heslo nebo službu ke vzdálené ploše pro virtuálních počítačích Windows](virtual-machines-windows-reset-rdp.md)

[Instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md)

[Odstraňování potíží s připojením zabezpečené prostředí (SSH) na základě Linux Azure virtuálního počítače](virtual-machines-linux-troubleshoot-ssh-connection.md)

[Odstraňování problémů s přístupem k spuštění aplikace v Azure virtuálního počítače](virtual-machines-linux-troubleshoot-app-connection.md)
