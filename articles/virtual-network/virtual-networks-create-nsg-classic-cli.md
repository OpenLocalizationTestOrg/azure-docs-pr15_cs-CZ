<properties
   pageTitle="Jak vytvořit NSGs v klasického režimu pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
   description="Naučte se vytvářet a publikovat NSGs v klasického režimu pomocí rozhraní příkazového řádku Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Jak vytvořit NSGs (klasický) v rozhraní příkazového řádku Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení. Můžete taky [vytvořit NSGs v modelu nasazení Správce prostředků](virtual-networks-create-nsg-arm-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Ukázkové příkazy Azure rozhraní příkazového řádku pod očekávat jednoduché prostředí vytvořen podle výše uvedených scénáře. Pokud chcete spustit příkazy tak, jak jsou zobrazeny v tomto dokumentu, nejprve sestavte testovacím prostředí [vytváření VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Jak vytvořit NSG podsítě front-end
Pokud chcete vytvořit NSG s názvem pojmenované **NSG FrontEnd** podle výše uvedených scénáře, postupujte následujícím způsobem.

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.

2. Spustit **`azure config mode`** příkaz Přepnout do klasického režimu, jak je ukázáno v následujícím příkladu.

        azure config mode asm

    Očekávaný výstup:

        info:    New mode is asm

3. Spustit **`azure network nsg create`** příkaz Vytvořit NSG.

        azure network nsg create -l uswest -n NSG-FrontEnd

    Očekávaný výstup:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parametry:

    - **-l (nebo – umístění)**. Kde se bude vytvořena nová NSG Azure oblast. Pro naše scénář *westus*.
    - **n (nebo – název)**. Název pro nový NSG. Pro naše scénář *NSG FrontEnd*.

4. Spustit **`azure network nsg rule create`** příkazu Vytvořit pravidlo, které poskytuje přístup k port 3389 (RDP) z Internetu.

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Očekávaný výstup:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Parametry:

    - **-(nebo – nsg název)**. Název NSG, ve kterém se vytvoří pravidlo. Pro naše scénář *NSG FrontEnd*.
    - **n (nebo – název)**. Název nové pravidlo. Pro naše scénář *rdp pravidlo*.
    - **-c (nebo – akce)**. Úroveň přístupu pravidla (povolit nebo odepřít).
    - **-p (nebo – protocol)**. Protocol (Tcp, Udp, nebo *) pro dané pravidlo.
    - **-r (nebo – typ)**. Směr připojení (příchozí nebo odchozí).
    - **-y (nebo – priorita)**. Priorita pravidla.
    - **-f (nebo – předponu zdrojové adresy)**. Předpona adresy zdrojového CIDR nebo s použitím výchozího značky.
    - **-o (nebo – rozsah zdrojových portů)**. Zdrojový port nebo rozsah portů.
    - **-e (nebo – předponou cílové adresy)**. Určení předpona adresy CIDR nebo s použitím výchozího značky.
    - **-u (nebo – rozsah cílových portů)**. Cílový port nebo rozsah portů.

5. Spustit **`azure network nsg rule create`** příkazu Vytvořit pravidlo, které poskytuje přístup k port 80 (HTTP) z Internetu.

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Očekávaná putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Spustit **`azure network nsg subnet add`** příkaz chcete vytvořit odkaz NSG podsítě front-end.

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    Očekávaný výstup:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Jak vytvořit NSG podsítě back-end
Pokud chcete vytvořit NSG s názvem pojmenovaný *NSG back-end* podle výše uvedených scénáře, postupujte následujícím způsobem.

3. Spustit **`azure network nsg create`** příkaz Vytvořit NSG.

        azure network nsg create -l uswest -n NSG-BackEnd

    Očekávaný výstup:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parametry:

    - **-l (nebo – umístění)**. Kde se bude vytvořena nová NSG Azure oblast. Pro naše scénář *westus*.
    - **n (nebo – název)**. Název pro nový NSG. Pro naše scénář *NSG FrontEnd*.

4. Spustit **`azure network nsg rule create`** příkazu Vytvořit pravidlo, které umožňuje získat přístup k port 1433 (SQL) podsítě front-end.

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Očekávaný výstup:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. Spustit **`azure network nsg rule create`** příkazu Vytvořit pravidlo, které odepřít přístup k Internetu.

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    Očekávaná putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Spustit **`azure network nsg subnet add`** příkaz chcete vytvořit odkaz NSG podsítě back-end.

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    Očekávaný výstup:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
