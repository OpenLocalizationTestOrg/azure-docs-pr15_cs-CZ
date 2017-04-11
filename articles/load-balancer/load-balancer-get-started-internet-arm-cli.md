<properties
   pageTitle="Vytvoření internetové Vyrovnávání zatížení v správce prostředků pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
   description="Naučte se vytvářet internetové Vyrovnávání zatížení v správce prostředků pomocí rozhraní příkazového řádku Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Vytvoření Vyrovnávání zatížení interní pomocí rozhraní příkazového řádku Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje nasazení modelu správce prostředků. Můžete taky [Naučte se vytvářet internetové Vyrovnávání zatížení pomocí klasické nasazení](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Nasazení řešení pomocí rozhraní příkazového řádku Azure

Podle těchto kroků ukazují, jak vytvořit internetové Vyrovnávání zatížení správce prostředků Azure pomocí rozhraní příkazového řádku. Pomocí Správce prostředků Azure jednotlivé zdroje se vytvoří a nakonfigurovali jednotlivě, pak vložit můžete vytvořit zdroj.

Musíte vytvořit a nakonfigurovat následující objekty, které chcete nasadit Vyrovnávání zatížení:

- Konfigurace front-end IP - obsahuje veřejnou IP adres pro příchozí v síti.
- Fond back-end adres – stránka obsahuje síťová rozhraní (NIC) virtuálních počítačích přijme síťová komunikace od Vyrovnávání zatížení.
- Vyrovnávání zatížení pravidla - obsahuje pravidla mapování veřejné port na Vyrovnávání zatížení s portem ve fondu back-end adres.
- Příchozí pravidla překladu síťových adres – obsahuje pravidla mapování veřejné port na Vyrovnávání zatížení na port pro konkrétní virtuální počítač ve fondu back-end adres.
- Sond - obsahuje stavu sond použít ke kontrole dostupnosti instancí virtuálních počítačích ve fondu back-end adres.

Další informace najdete v článku [Správce prostředků Azure podpory pro vyrovnávání zatížení](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Nastavení rozhraní příkazového řádku pro použití Správce prostředků

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../../articles/xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.

2. Příkaz **azure konfigurace režim** přepněte do režimu správce prostředků, jak je ukázáno v následujícím příkladu.

        azure config mode arm

    Očekávaný výstup:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Vytváření virtuálních sítí a veřejnou IP adresu pro fondu front-end IP

1. Vytvořte virtuální síť (VNet) s názvem *NRPVnet* ve východoasijských USA umístění pomocí skupina zdroje s názvem *NRPRG*.

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Vytvoření podsítě s názvem *NRPVnetSubnet* blokem CIDR 10.0.0.0/24 v *NRPVnet*.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Vytvořte veřejnou IP adresu s názvem *NRPPublicIP* pro použití v fondu front-end IP s názvem DNS *loadbalancernrp.eastus.cloudapp.azure.com*. Následující příkaz používá typ statické rozdělení a nečinnosti 4 minut.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]Vyrovnávání zatížení použije název domény veřejnou IP jako jeho plně kvalifikovaný název domény. To do změnu oproti klasické nasazení, který využívá cloudu služby jako Vyrovnávání zatížení plně kvalifikovaný název domény (FQDN).
    >V tomto příkladu je FQDN *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Vytvoření Vyrovnávání zatížení

Následující příkaz vytvoří Vyrovnávání zatížení s názvem *NRPlb* ve skupině *NRPRG* zdroje z *Východní USA* Azure umístění.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Vytvoření IP fondu front-end a back-end fond adres

Tento příklad ukazuje, jak vytvářet fondu front-end IP, který přijme příchozí provozu v síti na Vyrovnávání zatížení a fondu IP back-end kde fondu front-end odešle provozu v síti rozloženy.

1. Vytvoření fondu front-end IP přidružení veřejnou IP vytvořili v předchozím kroku a vyrovnávání zatížení.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Nastavte si fond adresu back-end slouží k přijímání příchozích z fondu front-end IP.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>Vytvoření pravidla LB, překladu síťových adres pravidel a zkušební

Tento příklad vytvoří následující položky.

- pravidlo překladu síťových adres překladu všechny příchozí přenosy na port 21 port 22<sup>1</sup>
- pravidlo překladu síťových adres překladu všechny příchozí přenosy na portu 23 s portem 22
- pravidlo Vyrovnávání zatížení zůstatek všechny příchozí přenosy na portu 80 portu 80 adresy ve fondu back-end.
- zkušební pravidlo pro kontrolu stavu na stránku s názvem *HealthProbe.aspx*.

<sup>1</sup> jsou přidružené k instanci konkrétní virtuální počítač za vyrovnávání zatížení překladu síťových adres pravidla. Provozu v síti odeslané na port 21 jsou odeslány konkrétní virtuální počítač na port 22 přidružené toto pravidlo překladu síťových adres Protocol (UDP nebo TCP) je nutné zadat u pravidla typu překladu síťových adres. Oba protokoly nelze přiřadit stejný port.

1. Vytváření pravidel překladu síťových adres.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Vytvoření pravidla Vyrovnávání zatížení.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Vytvořte zkušební stavu.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Zkontrolujte nastavení.

        azure network lb show nrprg nrplb

    Očekávaný výstup:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Vytvoření nic

Potřebujete vytvořit nic (nebo upravit existující) a přiřazovat překladu síťových adres pravidla pravidla Vyrovnávání zatížení a sond.

1. Vytvoření NIC s názvem *lb nic1 být*a přidružit *rdp1* překladu síťových adres a pravidla fondu *NRPbackendpool* back-end adres.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

    Očekávaný výstup:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Vytvoření NIC s názvem *lb nic2 být*a přidružit *rdp2* překladu síťových adres a pravidla fondu *NRPbackendpool* back-end adres.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Vytvoření virtuálního počítače (OM) s názvem *web1*a přidružit NIC s názvem *lb nic1 být*. Úložiště účet s názvem *web1nrp* byla vytvořená před spuštěním příkazu dole.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] VMs v Vyrovnávání zatížení musí být v sadě stejné dostupnosti. Použití `azure availset create` chcete vytvořit dostupné.

    Výstup by měl být stejný následujícím způsobem:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] Informační zpráva **jedná NIC bez publicIP nakonfigurováno** očekává od NIC, vytvoří Vyrovnávání zatížení připojení k Internetu pomocí vyrovnávání zatížení veřejnou IP adresu.

    Protože *lb nic1 být* NIC je přidružená k pravidlo překladu síťových adres *rdp1* , můžete se připojit k *web1* pomocí RDP prostřednictvím portu 3441 na Vyrovnávání zatížení.

4. Vytvoření virtuálního počítače (OM) s názvem *webu 2*a přidružit NIC s názvem *lb nic2 být*. Úložiště účet s názvem *web1nrp* byla vytvořená před spuštěním příkazu dole.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Aktualizace stávající Vyrovnávání zatížení

Můžete přidat pravidla odkazování na existující Vyrovnávání zatížení. V následujícím příkladu nové pravidlo Vyrovnávání zatížení přičten k existující **NRPlb** Vyrovnávání zatížení

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Odstranění Vyrovnávání zatížení

Pomocí následujícího příkazu odebrat Vyrovnávání zatížení:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Další kroky

[Začínáme konfigurace vyrovnávání zatížení vnitřní](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
