<properties
   pageTitle="Začít vytvářet internetové Vyrovnávání zatížení v klasické nasazení modelu pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
   description="Naučte se vytvářet internetové Vyrovnávání zatížení v klasické nasazení modelu pomocí rozhraní příkazového řádku Azure"
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

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Začít vytvářet internetové Vyrovnávání zatížení (klasický) v rozhraní příkazového řádku Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení. Můžete také [Naučte se vytvářet internetové Vyrovnávání zatížení pomocí Správce prostředků Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Krok za krokem vytvořením internetové Vyrovnávání zatížení pomocí rozhraní příkazového řádku

Tato příručka ukazuje, jak vytvořit Vyrovnávání zatížení Internet podle výše uvedených scénáře.

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../../articles/xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.

2. Příkaz **azure konfigurace režim** přepněte do klasického režimu, jak je ukázáno v následujícím příkladu.

        azure config mode asm

    Očekávaný výstup:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Vytvoření koncového bodu a nastavení Vyrovnávání zatížení

Scénář předpokládá virtuálních počítačích "web1" a "webu 2" vytvořená.
Tato příručka vytvoří množiny Vyrovnávání zatížení pomocí porty 80 jako veřejné port a port 80 jako místní port. Zkušební port je také nakonfigurovaný na port 80 a pojmenované sady Vyrovnávání zatížení "lbset".


### <a name="step-1"></a>Krok 1

Vytvoření prvního koncového bodu a sadu pomocí služba Vyrovnávání zatížení `azure network vm endpoint create` virtuálního počítače "web1".

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

Parametry použít:

**-k** – místního počítače virtuální port<br>
**-o** – Protocol (protokol)<BR>
**-t** - zkušební port<BR>
**-b** – název Vyrovnávání zatížení<BR>

## <a name="step-2"></a>Krok 2

Přidejte druhý počítač virtuální "webu 2" k sadě Vyrovnávání zatížení.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Krok 3

Ověření pomocí konfigurace služby Vyrovnávání zatížení `azure vm show` .

    azure vm show web1

Výstup bude:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Vytvoření vzdálené plochy koncový bod pro virtuálního počítače

Můžete vytvořit vzdálené plochy koncový bod předat v síti z veřejné port místní port pro konkrétní virtuální počítač pomocí `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Odebrání virtuálního počítače z vyrovnávání zatížení

Budete muset odstranit přidružené k vyrovnávání zatížení z virtuální počítač nastavit koncový bod. Po odebrání koncový bod virtuální počítač nepatří do Vyrovnávání zatížení už nastavení.

 Pomocí výše uvedeném příkladu, můžete odebráním koncový bod vytvoří virtuální počítač "web1" vyrovnávání zatížení "lbset" pomocí příkazu `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] Můžete prozkoumat další možnosti pro správu koncové body pomocí příkazu`azure vm endpoint --help`


## <a name="next-steps"></a>Další kroky

[Začínáme konfigurace vyrovnávání zatížení vnitřní](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)

