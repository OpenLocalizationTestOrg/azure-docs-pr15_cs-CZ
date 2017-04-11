<properties
   pageTitle="Vytvoření Vyrovnávání zatížení interní pomocí rozhraní příkazového řádku Azure v modelu klasické nasazení | Microsoft Azure"
   description="Naučte se vytvářet Vyrovnávání zatížení interní pomocí rozhraní příkazového řádku Azure v modelu klasické nasazení"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Vytvořit interní řešení pro vyrovnávání zatížení (klasický) pomocí rozhraní příkazového řádku Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Pokud chcete vytvořit Vyrovnávání zatížení vnitřní nastavte pro virtuálních počítačích

Vytvoření sady Vyrovnávání zatížení vnitřní a servery, které vám pošle provoz k němu, postupujte takto:

1. Vytvoření instance interní Vyrovnávání zatížení, který bude obsahovat koncového bodu příchozích být rozloženy do servery sady Vyrovnávání zatížení.

1. Přidání koncové body odpovídající virtuálních počítačích, které bude přijímat příchozí přenosy.

1. Konfigurace serverům, které bude odesílání přenosy být rozloženy směrování jejich přenosů na virtuální adresy IP (VIP) instance interní Vyrovnávání zatížení.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Krok za krokem vytvořením Vyrovnávání zatížení interní pomocí rozhraní příkazového řádku

Tato příručka ukazuje, jak vytvořit Vyrovnávání zatížení interní podle výše uvedených scénáře.

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../../articles/xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.

2. Příkaz **azure konfigurace režim** přepněte do klasického režimu, jak je ukázáno v následujícím příkladu.

        azure config mode asm

    Očekávaný výstup:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Vytvoření koncového bodu a nastavení Vyrovnávání zatížení

Scénář předpokládá virtuálních počítačích "ZR1" a "DB2" v cloudové službě s názvem "mytestcloud". Obě virtuálních počítačích používáte virtuální sítě s názvem Moje "testvnet" s podsítí "podsítě-1".

Tato příručka vytvoří sady Vyrovnávání zatížení vnitřní pomocí port 1433 jako soukromé port a 1433 jako místní port.

Toto je běžné situaci, kde máte SQL virtuálních počítačích na back-end zaručit, že databázových serverů nebude vystaven přímo pomocí veřejné adresy IP pomocí vyrovnávání zatížení vnitřní.


### <a name="step-1"></a>Krok 1

Vytvoření Vyrovnávání zatížení interní sadu pomocí `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

Parametry použít:

**-r** - název služby cloudu<BR>
**-n** - název Vyrovnávání zatížení vnitřní<BR>
**-t** - podsítě název (stejné podsítě ve virtuálních počítačích přidané do Vyrovnávání zatížení interního)<BR>
**-** - (volitelné) přidání statické soukromé IP adresa<BR>

Podívejte se na `azure service internal-load-balancer --help` Další informace.

Zkontrolujte vlastnosti Vyrovnávání zatížení vnitřní pomocí příkazu `azure service internal-load-balancer list` *název služby cloudu*.

Příklad výstupu tady takto:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Krok 2

Konfigurovat sadu Vyrovnávání zatížení vnitřní při přidání prvního koncového bodu. Přiřadíte koncový bod, virtuálního počítače a zkušební port sadu interní zatížení vyrovnávání v tomto kroku.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

Parametry použít:

**-k** – místního počítače virtuální port<BR>
**-t** - zkušební port<BR>
**-r** - zkušební Protocol (protokol)<BR>
**-e** - zkušební interval v sekundách<BR>
**-f** - časový limit v sekundách <BR>
**-i** - název Vyrovnávání zatížení vnitřní <BR>


## <a name="step-3"></a>Krok 3

Ověření pomocí konfigurace služby Vyrovnávání zatížení `azure vm show` *název virtuálního počítače*

    azure vm show DB1

Výstup bude:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Vytvoření vzdálené plochy koncový bod pro virtuálního počítače

Můžete vytvořit vzdálené plochy koncový bod předat v síti z veřejné port místní port pro konkrétní virtuální počítač pomocí `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Odebrání virtuálního počítače z vyrovnávání zatížení

Odebrání virtuálního počítače z vyrovnávání zatížení interní nastavil odstranění přidružené koncového bodu. Po odebrání koncový bod virtuální počítač nebude patří Vyrovnávání zatížení už nastavení.

 Pomocí výše uvedeném příkladu, můžete odebrat koncový bod vytvořil virtuálního počítače "ZR1" z vyrovnávání zatížení vnitřní "ilbset" pomocí příkazu `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Podívejte se na `azure vm endpoint --help` Další informace.


## <a name="next-steps"></a>Další kroky

[Konfigurace režimu rozdělení Vyrovnávání zatížení pomocí spřažení IP zdroje](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)