## <a name="traffic-manager-profile"></a>Přenosy správce profilu

Přenosy správce a její podřízené koncový bod zdroje povolení směrování DNS pro koncové body v Azure nebo mimo ni Azure. Toto rozdělení přenosy v síti podléhá metody směrování zásad. Přenosy Správce také umožňuje koncový bod stavu mají být sledovány a provoz přesměrováni řádně podporovat podle stavu koncový bod. 

| Vlastnost | Popis |
|---|---|
|**trafficRoutingMethod**| možné hodnoty jsou *výkon*, *vážená*a *priority (priorita)* | 
| **dnsConfig** | Plně kvalifikovaný název domény profilu | 
| **Protocol (protokol)** | sledování Protocol (protokol), možná jsou hodnoty *HTTP* a *HTTPS*|
| **Port** | sledování port |  
| **Cesta** | sledování cesta |
| **Koncové body** |  kontejner endpoint zdrojů | 

### <a name="endpoint"></a>Koncový bod 

Koncový bod je zdroj podřízené správce dopravy profilu. Představuje službu nebo které uživateli provozu distribuované koncový bod web na základě nakonfigurované zásad v profilu přenosy správce prostředků. 

| Vlastnost | Popis | 
|---|---| 
| **Typ** |  Typ koncového bodu, možné hodnoty jsou *Azure koncového bodu* *Externí koncového bodu*a *Vnořené koncový bod* | 
| **targetResourceId** |  veřejnou IP adresu koncového bodu služby nebo web. Může to být Azure nebo externí koncový bod. | 
| **Weight (váha)** | koncový bod weight (váha) používá přenosy řízení. | 
| **Priority (priorita)** | priority (priorita) na koncový bod, použije převzetí akce |

Ukázka správce přenosy ve formátu Json: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>Další zdroje informací

Přečtěte si [přečtěte následující dokumentaci pro rozhraní REST API pro správce přenosy](https://msdn.microsoft.com/library/azure/mt163664.aspx) Další informace.
