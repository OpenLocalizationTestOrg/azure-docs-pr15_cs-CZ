<properties 
   pageTitle="Správa NSGs pomocí prostředí PowerShell správce prostředků | Microsoft Azure"
   description="Naučte se spravovat stávajícího NSGs pomocí prostředí PowerShell ve Správci zdrojů"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-powershell"></a>Správa NSGs pomocí prostředí PowerShell

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Načtení informací

Můžete zobrazit existující NSGs, načtení pravidel pro existující NSG a zjistěte, jaké prostředky NSG souvisí s.

### <a name="view-existing-nsgs"></a>Zobrazení existujícího NSGs
Pokud chcete zobrazit všechny existující NSGs předplatné, spusťte `Get-AzureRmNetworkSecurityGroup` rutina jak je ukázáno v následujícím příkladu.

    Get-AzureRmNetworkSecurityGroup

Očekávaný výsledek:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Chcete-li zobrazit seznam NSGs v určité skupiny zdrojů, spusťte `Get-AzureRmNetworkSecurityGroup` rutina jak je ukázáno v následujícím příkladu. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Očekávaný výstup:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>Seznam všech pravidel pro NSG

Pravidla NSG s názvem **NSG FrontEnd**zobrazíte spustit `Get-AzureRmNetworkSecurityGroup` rutina jak je ukázáno v následujícím příkladu. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Očekávaný výstup:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] Můžete taky použít `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` seznam pravidel výchozí z **NSG FrontEnd** NSG.

### <a name="view-nsgs-associations"></a>Zobrazit NSGs přidružení

Co zdroje je NSG **NSG FrontEnd** přidružit zobrazíte spustit `Get-AzureRmNetworkSecurityGroup` rutina jak je ukázáno v následujícím příkladu.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Vyhledejte **NetworkInterfaces** a **podsítí** vlastnosti, jak je ukázáno v následujícím příkladu:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

Ve výše uvedeném příkladu NSG není přidružený k žádné síťová rozhraní (NIC) a je přidružený podsítě s názvem **FrontEnd**.

## <a name="manage-rules"></a>Správa pravidel

Můžete přidat pravidla do existující NSG, upravit existující pravidla a odebrat pravidla.

### <a name="add-a-rule"></a>Přidání pravidla

Přidání pravidla umožňují **příchozích** port **443** z jakéhokoli stroje **NSG FrontEnd** NSG, postupujte následujícím způsobem.

1. Spustit `Get-AzureRmNetworkSecurityGroup` rutina načíst existující NSG a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Spustit `Add-AzureRmNetworkSecurityRuleConfig` rutina, jak je ukázáno v následujícím příkladu.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Pokud chcete uložit změny provedené NSG, spusťte `Set-AzureRmNetworkSecurityGroup` rutina jak je ukázáno v následujícím příkladu.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Očekávaný výstup zobrazující pouze pravidla zabezpečení:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Změnit pravidlo

Chcete-li změnit pravidlo vytvořené nad povolíte příchozí data z **Internetu** , postupujte následujícím způsobem.

1. Spustit `Get-AzureRmNetworkSecurityGroup` rutina načíst existující NSG a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Spustit `Set-AzureRmNetworkSecurityRuleConfig` rutina, jak je ukázáno v následujícím příkladu.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Pokud chcete uložit změny provedené NSG, spusťte `Set-AzureRmNetworkSecurityGroup` rutina jak je ukázáno v následujícím příkladu.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Očekávaný výstup zobrazující pouze pravidla zabezpečení:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Odstranění pravidla

1. Spustit `Get-AzureRmNetworkSecurityGroup` rutina načíst existující NSG a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Spustit `Remove-AzureRmNetworkSecurityRuleConfig` rutina, jak je ukázáno v následujícím příkladu.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. Pokud chcete uložit změny provedené NSG, spusťte `Set-AzureRmNetworkSecurityGroup` rutina jak je ukázáno v následujícím příkladu.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Očekávaný výstup zobrazující pouze pravidla zabezpečení, oznámení, které už není uvedená **https pravidla** :

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Správa přidružení

Můžete přiřadit NSG do podsítí a nic. Můžete taky oddělit NSG z libovolného zdroje, které je přidružené k.

### <a name="associate-an-nsg-to-a-nic"></a>Přidružení NSG na NIC

Chcete-li přidružit **NSG FrontEnd** NSG **TestNICWeb1** NIC, postupujte následujícím způsobem.

1. Spustit `Get-AzureRmNetworkSecurityGroup` rutina načíst existující NSG a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Spustit `Get-AzureRmNetworkInterface` rutina načíst existující NIC a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Vlastnost **NetworkSecurityGroup** proměnné **NIC** hodnotu do proměnné **NSG** , jak je ukázáno v následujícím příkladu.

        $nic.NetworkSecurityGroup = $nsg

4. Pokud chcete uložit změny provedené NIC, spusťte `Set-AzureRmNetworkInterface` rutina jak je ukázáno v následujícím příkladu.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Očekávaný výstup zobrazující pouze vlastnost **NetworkSecurityGroup** :

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Oddělit NSG z NIC

Chcete-li oddělit **NSG FrontEnd** NSG z **TestNICWeb1** NIC, postupujte následujícím způsobem.

1. Spustit `Get-AzureRmNetworkSecurityGroup` rutina načíst existující NSG a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Spustit `Get-AzureRmNetworkInterface` rutina načíst existující NIC a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Nastavte vlastnost **NetworkSecurityGroup** proměnné **NIC** na **$null**, jak je ukázáno v následujícím příkladu.

        $nic.NetworkSecurityGroup = $null

4. Pokud chcete uložit změny provedené NIC, spusťte `Set-AzureRmNetworkInterface` rutina jak je ukázáno v následujícím příkladu.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Očekávaný výstup zobrazující pouze vlastnost **NetworkSecurityGroup** :

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Oddělit NSG z podsítě

Chcete-li oddělit **NSG FrontEnd** NSG z podsítě **FrontEnd** , postupujte následujícím způsobem.

1. Spustit `Get-AzureRmVirtualNetwork` rutina načíst existující VNet a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Spustit `Get-AzureRmVirtualNetworkSubnetConfig` rutina načíst podsítě **FrontEnd** a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. Nastavte vlastnost **NetworkSecurityGroup** proměnné **podsítě** na **$null**, jak je ukázáno v následujícím příkladu.

        $subnet.NetworkSecurityGroup = $null

4. Pokud chcete uložit změny provedené podsítě, spusťte `Set-AzureRmVirtualNetwork` rutina jak je ukázáno v následujícím příkladu.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Zobrazení pouze vlastností podsítě **FrontEnd** očekávaného výstupu. Všimněte si, že není vlastnost **NetworkSecurityGroup**:

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Přidružení NSG do podsítě

Pokud chcete znovu přiřadit **NSG FrontEnd** NSG podsítě **FronEnd** , postupujte následujícím způsobem.

1. Spustit `Get-AzureRmVirtualNetwork` rutina načíst existující VNet a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Spustit `Get-AzureRmVirtualNetworkSubnetConfig` rutina načíst podsítě **FrontEnd** a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Spustit `Get-AzureRmNetworkSecurityGroup` rutina načíst existující NSG a uložte ho do proměnné, jak je ukázáno v následujícím příkladu.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. Nastavte vlastnost **NetworkSecurityGroup** proměnné **podsítě** na **$null**, jak je ukázáno v následujícím příkladu.

        $subnet.NetworkSecurityGroup = $nsg

4. Pokud chcete uložit změny provedené podsítě, spusťte `Set-AzureRmVirtualNetwork` rutina jak je ukázáno v následujícím příkladu.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Očekávaný výstup zobrazující pouze vlastnost **NetworkSecurityGroup** podsítě **FrontEnd** :

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Odstranění NSG

NSG můžete odstranit pouze pokud máte není přidružený k jakémukoli prostředku. Pokud chcete odstranit NSG, postupujte následujícím způsobem.

1. Kontrola prostředků připojených NSG, spustit `azure network nsg show` uvedeno v [zobrazení NSGs přidružení](#View-NSGs-associations).
2. Pokud NSG souvisí s všechny nic, spusťte `azure network nic set` uvedeno v [Dissociate NSG z NIC](#Dissociate-an-NSG-from-a-NIC) pro každou NIC. 
3. Pokud NSG souvisí s všechny podsítě, spusťte `azure network vnet subnet set` uvedeno v [Dissociate NSG z podsítě](#Dissociate-an-NSG-from-a-subnet) pro každou podsítě.
4. Chcete-li odstranit NSG, spusťte `Remove-AzureRmNetworkSecurityGroup` rutina jak je ukázáno v následujícím příkladu.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] **– Vyšší** parametr zaručuje, nemusíte odstranění potvrďte.

## <a name="next-steps"></a>Další kroky

- [Povolit protokolování](virtual-network-nsg-manage-log.md) pro NSGs.