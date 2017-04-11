<properties
   pageTitle="Vytvoření Vyrovnávání zatížení vnitřní pomocí rozhraní příkazového řádku Azure správce prostředků | Microsoft Azure"
   description="Naučte se vytvářet Vyrovnávání zatížení vnitřní pomocí rozhraní příkazového řádku Azure ve Správci zdrojů"
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

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Vytvoření Vyrovnávání zatížení vnitřní pomocí rozhraní příkazového řádku Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klasický nasazení modelu](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Nasazení řešení pomocí rozhraní příkazového řádku Azure

Podle těchto kroků ukazují, jak vytvořit Vyrovnávání zatížení internetového pomocí Správce prostředků Azure pomocí rozhraní příkazového řádku. S Azure správce prostředků, jednotlivé zdroje je vytvořili a nakonfigurovat jednotlivě a vložíte můžete vytvořit zdroj.

Je potřeba vytvořit a nakonfigurovat následující objekty, které chcete nasadit Vyrovnávání zatížení:

- **Konfigurace předřazený IP**: obsahuje veřejnou IP adres pro příchozí v síti
- **Fond adres back-end**: obsahuje síťová rozhraní (NIC) podporující nástroj virtuálních počítačích přijme síťová komunikace od Vyrovnávání zatížení
- **Vyrovnávání zatížení pravidla**: obsahuje pravidla, která mapování veřejné portu na Vyrovnávání zatížení s portem ve fondu back-end adres
- **Překladu síťových adres příchozí pravidla**: obsahuje pravidla, která mapování veřejné port na Vyrovnávání zatížení na port pro konkrétní virtuální počítač ve fondu back-end adres
- **Sond**: obsahuje sond stavu, které se používají při kontrole dostupnosti instancí virtuálních počítačích ve fondu adres back-end

Další informace najdete v článku [Správce prostředků Azure podpory pro vyrovnávání zatížení](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Nastavení rozhraní příkazového řádku pro použití Správce prostředků

1. Pokud jste použili nikdy Azure rozhraní příkazového řádku, přečtěte si článek [instalace a konfigurace rozhraní příkazového řádku Azure](../../articles/xplat-cli-install.md). Postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.

2. Spusťte příkaz **azure konfigurace režim** přepněte do režimu správce prostředků, následujícím způsobem:

        azure config mode arm

    Očekávaný výstup:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Vytvoření Vyrovnávání zatížení vnitřní krok za krokem

1. Přihlaste se k Azure.

        azure login

    Po zobrazení výzvy zadejte Azure přihlašovací údaje.

2. Změníte nástroje příkaz Správce prostředků Azure režimu.

        azure config mode arm

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Všechny zdroje v správce prostředků Azure souvisí se skupinou zdroje. Pokud jste tak dosud neučinili ještě, vytvořte skupinu zdrojů.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Vytvoření Vyrovnávání zatížení vnitřní

1. Vytvoření Vyrovnávání zatížení vnitřní

    V následujících situacích vytvoří se skupina zdroje s názvem nrprg ve východoasijských USA oblast.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Všechny zdroje interní moduly pro vyrovnávání zatížení, například virtuálních sítí a virtuální podsítí, musí být ve stejné skupině zdroje a ve stejné oblasti.

2. Vytvoření front-end IP adresu pro vyrovnávání zatížení vnitřní.

    IP adresy, kterou používáte musí být v rozsahu podsítě virtuální sítě.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. Vytvoření fondu back-end adres.

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Po definování front-end IP adresu a adresu back-end fondu vytvoříte pravidla Vyrovnávání zatížení, příchozí pravidla překladu síťových adres a sond přizpůsobený stavu.

4. Vytvoření pravidla Vyrovnávání zatížení pro vyrovnávání zatížení vnitřní.

    Pokud jste pomocí předchozích kroků, příkaz vytvoří Vyrovnávání zatížení pravidla pro příjem port 1433 ve fondu front-end a odeslání Vyrovnávání zatížení sítě přenosy do fondu back-end adres, taky používá port 1433.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Vytvoření příchozí pravidla překladu síťových adres.

    Příchozí pravidla překladu síťových adres slouží k vytvoření koncové body v při vyrovnávání zatížení, přejděte na instanci konkrétní virtuální počítač. Postupem vytvořené dva překladu síťových adres pravidla pro vzdálené plochy.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Vytvoření sond stavu pro vyrovnávání zatížení.

    Stav zkušební kontroluje všechny instance virtuálního počítače, aby zkontrolovala, jestli že můžete odeslat v síti. Instance virtuálního počítače s selhalo zkušební kontroly se odebere z vyrovnávání zatížení, dokud přechodu do online režimu a kontrola zkušební Určuje, že je v pořádku.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]Platformu Microsoft Azure používá statické a veřejně směrovatelné adresy IPv4 pro řadu možností pro správu. IP adresu je 168.63.129.16. Tuto IP adresu by neměly být blokovány všechny brány firewall vzhledem k tomu může dojít k neočekávané chování.
    >Pokud jde o vyrovnávání Azure interní zatížení tuto IP adresu slouží sledováním sond z vyrovnávání zatížení k určení stavu virtuálních počítačích v sadě Vyrovnávání zatížení. Pokud skupiny zabezpečení sítě se používá k omezení přenosy Azure virtuálních počítačích v sadě interně Vyrovnávání zatížení nebo se použije pro virtuální podsítě, zajistit, aby přidaná pravidlo zabezpečení sítě přenosy z 168.63.129.16 umožňující.

## <a name="create-nics"></a>Vytvoření nic

Potřebujete vytvořit nic (nebo upravit existující) a přiřazovat překladu síťových adres pravidla pravidla Vyrovnávání zatížení a sond.

1. Vytvoření NIC s názvem *lb nic1 být*a potom ji přidružit pravidla překladu síťových adres *rdp1* a fondu *beilb* back-end adres.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

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

2. Vytvoření NIC s názvem *lb nic2 být*a potom ji přidružit pravidla překladu síťových adres *rdp2* a fondu *beilb* back-end adres.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. Vytvoření virtuálního počítače s názvem *ZR1*a potom ji přidružit NIC s názvem *lb nic1 být*. Vytvoření úložiště účtu s názvem *web1nrp* před spuštěním následujícího příkazu:

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] VMs v Vyrovnávání zatížení musí být v sadě stejné dostupnosti. Použití `azure availset create` chcete vytvořit dostupné.

4. Vytvoření virtuálního počítače (OM) s názvem *DB2*a potom ji přidružit NIC s názvem *lb nic2 být*. Úložiště účet s názvem *web1nrp* byla vytvořená před spuštěním následujícího příkazu.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Odstranění Vyrovnávání zatížení

Pokud chcete odebrat Vyrovnávání zatížení, použijte tento příkaz:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Další kroky

[Konfigurace režimu rozdělení Vyrovnávání zatížení pomocí spřažení IP zdroje](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
