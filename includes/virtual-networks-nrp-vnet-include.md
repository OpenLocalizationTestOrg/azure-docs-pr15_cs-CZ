## <a name="virtual-network"></a>Virtuální sítě
Virtuální sítí (VNET) a podsítí prostředky nápovědy definovat hranici zabezpečení pro pracovní vytížení v Azure VNet rozdělení pro skupinu adresního prostoru rozumí CIDR bloky. 

>[AZURE.NOTE] Správci sítě znají CIDR zápisu. Pokud znáte není CIDR, [Přečtěte si další informace o ho](http://whatismyipaddress.com/cidr).

![VNet s více podsítí](./media/resource-groups-networking/Figure4.png)

VNets obsahují následující vlastnosti.

|Vlastnost|Popis|Ukázkové hodnoty|
|---|---|---|
|**addressSpace**|Kolekce předpony adres, které tvoří VNet v CIDR soustavě|192.168.0.0/16|
|**podsítí**|Soubor podsítí tvořící VNet|v tématu [podsítí](#Subnets) dole.|
|**Adresa IP**|IP adresy přiřazené k objektu. Toto je vlastnost jen pro čtení.|104.42.233.77|

### <a name="subnets"></a>Podsítí
Podsítě je zdroj podřízené VNet a pomáhá definovat segmentů adresního prostoru v rámci bloku CIDR pomocí předpony IP adres. Nic můžete přidat do podsítí a připojený k VMs poskytování připojení pro různé úloh.

Podsítí obsahují následující vlastnosti. 

|Vlastnost|Popis|Ukázkové hodnoty|
|---|---|---|
|**addressPrefix**|Předponu jedné adresy, které tvoří podsítě v CIDR zápisu|192.168.1.0/24|
|**networkSecurityGroup**|NSG použité u podsítě|v tématu [NSGs](#Network-Security-Group)|
|**routeTable**|Směrování tabulky použité u podsítě|v tématu [UDR](#Route-table)|
|**ipConfigurations**|Kolekce objektů configruation IP používá nic připojené k podsítě|v tématu [UDR](#Route-table)|


Ukázka VNet ve formátu JSON:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Další zdroje informací

- Získáte další informace o [VNet](../articles/virtual-network/virtual-networks-overview.md).
- Přečtěte si [přečtěte následující dokumentaci pro odkaz rozhraní REST API](https://msdn.microsoft.com/library/azure/mt163650.aspx) pro VNets.
- Přečíst [si přečtěte následující dokumentaci odkaz rozhraní REST API](https://msdn.microsoft.com/library/azure/mt163618.aspx) pro podsítě.