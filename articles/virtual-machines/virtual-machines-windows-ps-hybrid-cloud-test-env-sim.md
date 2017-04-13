<properties 
    pageTitle="Simulovaný hybridní cloudu testovacím prostředí | Microsoft Azure" 
    description="Vytvoření simulovaný hybridního prostředí cloudu pro IT pro nebo vývoj testování třetí strana, pomocí dvou Azure virtuálních sítí a VNet VNet připojení." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Nastavení simulovaný hybridního prostředí cloudu pro účely testování

Tento článek vás provede jednotlivými kroky vytvoření simulovaný hybridního prostředí cloudu pomocí Microsoft Azure pomocí dvou Azure virtuální sítě. Tady je Výsledná konfigurace.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Napodobuje hybridního prostředí výrobní cloudu a obsahuje:

- Simulovaný a zjednodušené místní síti hostované v síti Azure virtuální (virtuální síť testovací podsíť).
- Simulovaný křížově místní virtuální síti hostované v Azure (TestVNET).
- VNet VNet spojení mezi dvěma virtuální sítě.
- Sekundární domény řadiče v síti virtuální TestVNET.

Zajistíte, že základna a běžných spuštění odkazovat ze kterého můžete provést tyto akce:

- Můžete vyvíjet a otestujte aplikace v cloudu simulovaný hybridního prostředí.
- Vytvoření test konfigurace počítačů, některé v rámci virtuální sítě testovací podsíť a některé v síti virtuální TestVNET tak, aby napodobily hybridní cloudové pracovních vytížení.

Existují čtyři hlavní fáze pro nastavení toto hybridní cloudu testovacím prostředí:

1.  Konfigurace testovací podsíť virtuální sítě.
2.  Vytvořte virtuální sítě mezi místním nasazení.
3.  Vytvoření připojení k síti VPN VNet VNet.
4.  Konfigurace DC2. 

Tato konfigurace vyžaduje předplatné Azure. Pokud máte předplatné MSDN nebo Visual Studio, najdete v článku [měsíční Azure dobru Visual Studio účastníky](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

>[AZURE.NOTE] Virtuálních počítačích a bran virtuální sítě v Azure vzniknou probíhající peněžní náklady, když jsou spuštěné. Tyto náklady fakturované proti váš web MSDN nebo zaplaceného předplatného. Azure VPN brány je implementovaná jako sady dvou Azure virtuálních počítačích. Minimalizovat náklady, vytvořte testovacím prostředí a provést vaše potřeby testování a ukázkové co nejdříve.

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Fáze 1: Konfigurace testovací podsíť virtuální sítě

Postupujte podle pokynů v tématu [Konfigurace Base testovacím prostředí](https://technet.microsoft.com/library/mt771177.aspx) pro nastavení počítače DC1 Apl1 a KLIENT1 v Azure virtuální sítě s názvem testovací podsíť. 

Pak začněte příkazovém řádku prostředí PowerShell Azure.

> [AZURE.NOTE] Následující příkaz nastaví použití Azure PowerShell 1.0 a novější.

Přihlaste se ke svému účtu.

    Login-AzureRMAccount

Zobrazí název vašeho předplatného pomocí následujícího příkazu.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Nastavte Azure předplatné. Použijte stejný předplatné, které jste použili k vytvoření základní konfigurace ve fázi 1. Nahrazení všechno, co do uvozovek, včetně < a > znaky se správným názvem.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Dále přidejte brány podsítě virtuální sítě testovací podsíť základní konfigurace, které se použijí k hostování Azure brány.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Pak požádat o veřejnou IP adresu přiřadit k bráně pro testovací podsíť virtuální sítě.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Dále vytvořte bránu.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Mějte na paměti, která nové brány může trvat a 20 minut k vytvoření.

Z portálu Azure na místním počítači připojte pomocí přihlašovacích údajů CORP\User1 k DC1. Pro nastavení domény CORP tak, aby počítačů a uživatelů účely jejich místním doménovém ověřování, spusťte tyto příkazy z příkazového řádku prostředí Windows PowerShell na úrovni správce v počítači DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Toto je aktuální konfigurace.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)
 
## <a name="phase-2-create-the-testvnet-virtual-network"></a>Fáze 2: Vytvoření TestVNET virtuální sítě

Nejdřív vytvořit virtuální sítě TestVNET a chránit pomocí síťové skupiny zabezpečení.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Pak požádat o veřejnou IP adresu mají přidělit brány virtuální sítě TestVNET a vytvářet brány.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Toto je aktuální konfigurace.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)
 
##<a name="phase-3-create-the-vnet-to-vnet-connection"></a>Fáze 3: Vytvoření připojení VNet VNet

Nejprve získejte předem sdílené klíč náhodné neposkytuje silných, 32 znaků od správce sítě nebo zabezpečení. Můžete také pomocí informací v [vytvořit náhodné řetězec IPsec klíče](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) získat předem sdíleného klíče.

Pak použijte tyto příkazy k vytvoření připojení VPN VNet VNet, které může chvíli trvat dokončete.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Po několika minutách stanovit připojení.

Toto je aktuální konfigurace.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)
 
## <a name="phase-4-configure-dc2"></a>Fáze 4: Konfigurace DC2

Nejprve vytvořte virtuálního počítače pro DC2. Tyto příkazy můžete spusťte na příkazovém řádku prostředí PowerShell Azure ve vašem počítači.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Pak připojte k nový virtuální počítač DC2 z portálu Microsoft Azure.

Potom nakonfigurujte pravidlo Brána Firewall systému Windows umožňující přenosy pro účely testování základní připojení. Na úrovni správce prostředí Windows PowerShell příkazovém řádku DC2 spusťte tyto příkazy.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

Příkaz ping by měl mít za následek čtyři úspěšné odpovědi z IP adresy 10.0.0.4. Toto je testovací přenosy přes připojení VNet VNet.

Dále přidejte disku navíc dat na DC2 jako nový hlasitost s písmenem F:.

1.  V levém podokně Správce serveru klikněte na **soubor a úložiště služby**a potom klikněte na **disku**.
2.  V podokně obsah ve skupině **disků** klikněte na **disku 2** (s **oddílu** nastavení na **Neznámý**).
3.  Klikněte na **úkoly**a potom klikněte na **Nový hlasitost**.
4.  V polích před zahájení stránka průvodce nový objem, klikněte na tlačítko **Další**.
5.  Na stránce vyberte server a disku klikněte na **disku 2**a klikněte na tlačítko **Další**. Po zobrazení výzvy klikněte na **OK**.
6.  Na stránce zadání velikosti stránky hlasitost klikněte na **Další**.
7.  Na přiřazení na stránku písmeno nebo složku jednotka klikněte na **Další**.
8.  Na stránce nastavení systému vyberte soubor klikněte na **Další**.
9.  Na stránce Potvrďte výběr klikněte na **vytvořit**.
10. Až budete hotovi, klepněte na tlačítko **Zavřít**.

Potom nakonfigurujte DC2 jako otevřené domény řadiče domény corp.contoso.com. Tyto příkazy spusťte z příkazového řádku prostředí Windows PowerShell na DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Všimněte si, že se zobrazí výzva k zadání hesla CORP\User1 a hesla Directory Services obnovit režim (DSRM) a restartujte DC2.

Nyní má virtuální sítě TestVNET vlastní DNS server (DC2), musíte nakonfigurovat virtuální sítě TestVNET použít tento server DNS.

1.  V levém podokně portálu Azure klikněte na ikonu virtuální sítě a potom klikněte na **TestVNET**.
2.  Na kartě **Nastavení** klikněte na **servery DNS**.
3.  V dialogovém okně **primární DNS server**zadejte **192.168.0.4** nahrazení 10.0.0.4.
4.  Klikněte na **Uložit**.

Toto je aktuální konfigurace. 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)
 
Simulovaný hybridní prostředí cloudu je nyní připravena k testování.

## <a name="next-step"></a>Další krok

- Nastavte si [založené na webu řada obchodních aplikací](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) v prostředí.
