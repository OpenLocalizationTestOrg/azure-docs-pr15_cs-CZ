<properties
    pageTitle="Vytvoření internetové Vyrovnávání zatížení, IPv6 v Azure správce prostředků pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
    description="Naučte se vytvářet internetové Vyrovnávání zatížení, IPv6 v Azure správce prostředků pomocí rozhraní příkazového řádku Azure"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="protokol IPv6 Vyrovnávání zatížení azure, duální zásobníku, veřejnou ip, nativní protokol ipv6, mobilní telefon, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Vytvoření internetové Vyrovnávání zatížení, IPv6 v Azure správce prostředků pomocí rozhraní příkazového řádku Azure

> [AZURE.SELECTOR]
- [Prostředí PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure rozhraní příkazového řádku](./load-balancer-ipv6-internet-cli.md)
- [Šablony](./load-balancer-ipv6-internet-template.md)

Vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy-4 při instalaci (TCP, UDP). Vyrovnávání zatížení zajišťuje dostupnost distribuce příchozí komunikaci mezi správný služby instancí služby cloudu nebo virtuálních počítačích v sadě Vyrovnávání zatížení. Azure Vyrovnávání zatížení můžete zobrazit tyto služby ve více portů a více IP adres.

## <a name="example-deployment-scenario"></a>Příklad scénáře

Následující obrázek znázorňuje řešení vyrovnávání zatížení nasazení pomocí šablony příklad popisované v tomto článku.

![Scénář Vyrovnávání zatížení](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

V tomto scénáři vytvoří následující Azure zdroje:

- dva virtuálních počítačích (VMs)
- virtuální sítě rozhraní pro každou OM s IPv4 a IPv6 adresa přiražená
- Vyrovnávání zatížení internetového IPv4 a IPv6 veřejnou IP adresu
- Dostupnost hodnotu, která obsahuje dva VMs
- dvě načíst vyrovnávání pravidla přiřadit veřejné virtuální privátní koncové body

## <a name="deploying-the-solution-using-the-azure-cli"></a>Nasazení řešení pomocí rozhraní příkazového řádku Azure

Podle těchto kroků ukazují, jak vytvořit internetové Vyrovnávání zatížení správce prostředků Azure pomocí rozhraní příkazového řádku. S Azure správce prostředků, jednotlivé zdroje je vytvořili a nakonfigurovat jednotlivě a vložíte můžete vytvořit zdroj.

Abyste mohli nasadit služby Vyrovnávání zatížení, vytváření a konfigurace následující objekty:

- Konfigurace front-end IP - obsahuje veřejnou IP adres pro příchozí v síti.
- Fond back-end adres – stránka obsahuje síťová rozhraní (NIC) virtuálních počítačích přijme síťová komunikace od Vyrovnávání zatížení.
- Vyrovnávání zatížení pravidla - obsahuje pravidla mapování veřejné port na Vyrovnávání zatížení s portem ve fondu back-end adres.
- Příchozí pravidla překladu síťových adres – obsahuje pravidla mapování veřejné port na Vyrovnávání zatížení na port pro konkrétní virtuální počítač ve fondu back-end adres.
- Sond - obsahuje stavu sond použít ke kontrole dostupnosti instancí virtuálních počítačích ve fondu back-end adres.

Další informace najdete v článku [Správce prostředků Azure podpory pro vyrovnávání zatížení](load-balancer-arm.md).

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Nastavení prostředí rozhraní příkazového řádku pro použití Správce prostředků Azure

V tomto příkladu jsme do okna příkazového prostředí PowerShell běží nástroje rozhraní příkazového řádku. Jsme nepoužíváte rutiny prostředí PowerShell Azure, ale používáme funkcí skriptování na Powershellu ke zlepšení čitelnosti a opakované použití.

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../../articles/xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.

2. Spusťte příkaz **azure konfigurace režim** přepněte do režimu správce prostředků.

        azure config mode arm

    Očekávaný výstup:

        info:    New mode is arm

3. Přihlaste se k Azure a zobrazte seznam předplatných.

        azure login

    Zadejte Azure přihlašovací údaje po zobrazení výzvy.

        azure account list

    Vyberte předplatné, které chcete použít. Poznamenejte si Id odběru v dalším kroku.

4. Nastavení prostředí PowerShell proměnné pro použití s příkazy rozhraní příkazového řádku.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Vytvoření skupiny zdrojů, Vyrovnávání zatížení, virtuální sítě a podsítí

1. Vytvoření skupiny prostředků

        azure group create $rgName $location

2. Vytvoření Vyrovnávání zatížení

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Vytvořte virtuální síť (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Vytvořte dva podsítí v tomto VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Vytvoření veřejné IP adres pro fondu front-end

1. Nastavení proměnné prostředí PowerShell

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Vytvořte veřejnou IP adresu fondu front-end IP.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]Vyrovnávání zatížení používá název domény veřejnou IP jako jeho plně kvalifikovaný název domény. Tato změna z klasické nasazení, který používá cloudové služby název jako Vyrovnávání zatížení plně kvalifikovaný název domény.
    >V tomto příkladu je FQDN *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Vytvoření fondu front-end a back-end

Tento příklad vytvoří fondu front-end IP, který přijme příchozí provozu v síti na Vyrovnávání zatížení a fondu IP back-end kde fondu front-end odešle rozloženy provozu v síti.

1. Nastavení proměnné prostředí PowerShell

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. Vytvoření fondu front-end IP přidružení veřejnou IP vytvořili v předchozím kroku a vyrovnávání zatížení.

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>Vytvořte zkušební, překladu síťových adres pravidla a pravidla LB

Tento příklad vytvoří následující položky:

- zkušební pravidla ke kontrole připojení k TCP port 80
- pravidlo překladu síťových adres překladu všechny příchozí přenosy na port 3389 port 3389 RDP<sup>1</sup>
- pravidlo překladu síťových adres překladu všechny příchozí přenosy na port 3391 port 3389 RDP<sup>1</sup>
- pravidlo Vyrovnávání zatížení zůstatek všechny příchozí přenosy na portu 80 portu 80 adresy ve fondu back-end.

<sup>1</sup> jsou přidružené k instanci konkrétní virtuální počítač za vyrovnávání zatížení překladu síťových adres pravidla. Odeslané na port 3389 provozu v síti se pošle konkrétní virtuální počítač a port přidružené k pravidlu překladu síťových adres. Protocol (UDP nebo TCP) je nutné zadat u pravidla typu překladu síťových adres. Oba protokoly nelze přiřadit stejný port.

1. Nastavení proměnné prostředí PowerShell

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. Vytvoření zkušební

    Následující příklad vytvoří TCP zkušební, která kontroluje pro připojení k back-end port TCP 80 každých 15 sekund. Ji označí zdroje back-end není k dispozici po dvě po sobě jdoucí selhání.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. Vytvoření příchozí pravidla překladu síťových adres, které umožňují RDP připojení ke zdroji back-end

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Vytváření pravidel, která směrování přenosů na různé porty back-end podle toho, jaký front-end přijaté žádosti Vyrovnávání zatížení

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. Zkontrolujte nastavení

        azure network lb show --resource-group $rgName --name $lbName

    Očekávaný výstup:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>Vytvoření nic

Vytvoření nic a přiřazovat překladu síťových adres pravidla pravidla Vyrovnávání zatížení a sond.

1. Nastavení proměnné prostředí PowerShell

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. Vytvořte NIC pro každou back-end a přidejte konfigurace protokolu IPv6.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Vytvoření zdrojů OM back-end a připojit každý NIC

Pokud chcete vytvořit VMs, musí mít účet úložiště. Pro vyrovnávání zatížení VMs musejí být členy sady dostupnosti. Další informace o vytváření VMs najdete v článku [Vytvoření OM Azure pomocí prostředí PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md)

1. Nastavení proměnné prostředí PowerShell

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] Tento příklad používá uživatelské jméno a heslo pro VMs ve formátu prostého textu. Vhodné musí věnovat při použití pověření v vymazat. Bezpečnější způsob zpracování přihlašovacích údajů v prostředí PowerShell najdete v tématu rutiny [Get-pověření](https://technet.microsoft.com/library/hh849815.aspx) .

2. Vytvoření úložiště nastavení účtu a dostupnosti

    Když vytvoříte VMs, mohli byste použít existujícího účtu úložiště. Následující příkaz vytvoří nový účet úložiště.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Dále vytvořte sadu dostupnosti.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. Vytváření virtuálních počítačích s přidruženými nic

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Další kroky

[Začínáme konfigurace vyrovnávání zatížení vnitřní](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
