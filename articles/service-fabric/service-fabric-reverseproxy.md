<properties
   pageTitle="Služby reverzního Proxy struktury | Microsoft Azure"
   description="Použití služby struktury reverzního proxy pro sdělení microservices z uvnitř a mimo clusteru"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Služba struktury reverzního Proxy

Služba struktury reverzního proxy je integrována do služby sítě reverzního proxy umožňující adresování microservices clusteru struktury služby, které budou obsahovat HTTP koncové body.

## <a name="microservices-communication-model"></a>Microservices komunikace modelu

Microservices v služby struktury obvykle spustit podmnožinu společnosti OM clusteru a můžete přesouvat z jednoho OM do jiného různých důvodů. Abyste mohli koncové body microservices dynamicky převést. Typické vzorek sdělovat microservice je níže, vyřešit smyčka

1. Vyřešte umístění služby počáteční prostřednictvím služba WINS.
2. Připojení ke službě.
3. Zjistit příčinu chyby připojení a znovu přeložit umístění služby v případě potřeby.

Tenhle proces zahrnuje obecně obtékání komunikace klienta knihoven ve smyčce opakovat implementující zásady služby rozlišení a opakování.
Další informace o tomto tématu najdete v tématu [komunikaci se službami](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-via-sf-reverse-proxy"></a>Komunikace prostřednictvím SF reverzního proxy
Služba struktury reverzního proxy běží ve všech uzlech clusteru. Provede rozlišení obrázku celé služby jménem klienta a potom předá požadavek klienta. Tak klienti spuštěné v clusteru můžou jenom pomocí všech klientských HTTP komunikace knihoven Pokud chcete mluvit cílové služby prostřednictvím reverzního proxy SF místně spuštěné ve stejném uzlu.

![Interní komunikaci][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Dosažení Microservices z mimo clusteru
Výchozí model externí komunikace pro microservices je **určovat, jestli v** kde každé služby ve výchozím nastavení není přístupný přímo z externí klienti. [Vyrovnávání zatížení Azure](../load-balancer/load-balancer-overview.md) je sítě hranici mezi microservices a externí klienti, která provádí překládání adres a předá externí požadavky na vnitřní **IP: port** koncové body. Před uskutečněním koncového bodu microservice přímo přístupných osobám s postižením externí klientům, musí být vyrovnávání zatížení Azure nakonfigurované nejdřív pro předávání přenosu pro každý port používaný službou clusteru. Kromě toho většina microservices (esp. stavové microservices) není live ve všech uzlech clusteru a můžete přesouvat mezi uzly na převzetí, tak v takovém případě vyrovnávání zatížení Azure nemůže efektivně určit cílový uzel replik umístěných k předání dat na.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>Dosažení Microservices prostřednictvím SF reverzního proxy z mimo clusteru

Místo konfiguraci jednotlivých služeb porty v Vyrovnávání zatížení azure, je možné konfigurovat jenom port serveru proxy obrácené pořadí SF v služby Vyrovnávání zatížení Azure. Díky klientům mimo clusteru dosáhla služby uvnitř clusteru prostřednictvím reverzního proxy bez další konfiguraci.

![Externí komunikace][0]

>[AZURE.WARNING] Konfigurace reverzního proxy port na Vyrovnávání zatížení, díky kterému malých služby v obrázku, které budou obsahovat http koncový bod, aby addressible z mimo clusteru.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>Identifikátor URI formát pro adresování služby přes reverzního proxy

Reverzního proxy určitého formátu URI používá k identifikaci kterém oddílu service bude předávat příchozí žádosti:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **http (s):** Reverzního proxy můžete nakonfigurován pro přenosy protokolu HTTP nebo HTTPS. V případě HTTPS přenosy SSL ukončení probíhá reverzního proxy. Požadavky, které jsou předávat dál reverzního proxy ke službám clusteru jsou přes http.
 - **Shluk plně kvalifikovaný název domény | interní IP:** Externí klientům reverzního proxy možné konfigurovat tak, aby se k nim přistupovat prostřednictvím clusteru domény (například mycluster.eastus.cloudapp.azure.com). Ve výchozím nastavení spuštění reverzního proxy v každém uzlu tak pro interní přenos ho zastižení na localhost nebo všechny adresy IP interní uzel (například 10.0.0.1).
 - **Portu:** Číslo portu, která byla specifikována pro reverzního proxy. Např: 19008.
 - **ServiceInstanceName:** To je název instance plně kvalifikovaný nasazeném služby služby snažíte se dostanete "struktury: /" schéma. Například dosáhla služby *struktury: / aplikace/Moje_služba/*, použijete *aplikace/Moje_služba*.
 - **Přípona cestu:** Toto je skutečnou cestu URL pro službu, kterou chcete připojit. Například *myapi/hodnoty/přidat/3*
 - **PartitionKey:** Rozdělený služby jedná se o počítanou oddíl klíč oddílu, který chcete zastihnout. Všimněte si, že se jedná *není* oddíl Identifikátor GUID. Tento parametr není požadovaný pro služby, které využívají schéma oddílu jednoznačné.
 - **PartitionKind:** Schéma oddílu služby. Může to být "Int64Range" nebo "S názvem". Tento parametr není požadovaný pro služby, které využívají schéma oddílu jednoznačné.
 - **Vypršení časového limitu:**  Určuje časový limit pro požadavek http vytvořil reverzního proxy ke službě jménem požadavek klienta. Jako výchozí hodnota je 60 sekund. Toto je volitelný parametr.

### <a name="example-usage"></a>Příklad použití

Jako příklad Podívejme služby **struktury: / aplikace/Moje_služba** , která otevře posluchače HTTP na následující adresu URL:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

S v následujících zdrojích:

 - `/index.html`
 - `/api/users/<userId>`

Pokud služby používá schématu rozdělení oddílů jednoznačné, *PartitionKey* a *PartitionKind* řetězec parametry dotazu není povinný a službu zastižení přes bránu jako:

 - Externě:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Interně:`http://localhost:19008/MyApp/MyService`

Pokud služby používá schématu rozdělení oddílů jednotné Int64, musíte *PartitionKey* a *PartitionKind* řetězec parametry dotazu použít k dosažení oddíl služby:

 - Externě:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Interně:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Pokud chcete mít přístup k prostředkům zveřejněné pro službu, jednoduše za název služby v adrese URL dali cestu zdroje:

 - Externě:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Interně:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Brány nastavíte přesměrování tyto požadavky na adresu URL služby:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Zvláštní zpracování pro sdílení port služby

Brána aplikace pokusy o znovu přeložit adresy služby a opakovat žádost, když službu nemůže získat přístup na stránku. Toto je jednou z hlavních výhod brány jako kód klienta nemusí implementovat vlastní rozlišení služeb a řešení opakovat.

Obecně při nelze přejít do služby znamená to, že instance služby nebo otevřené přesunul různých uzel v rámci svého životního cyklu normální. V takovém případě brány může chybová síťové připojení označovat tak, že už není otevřené na adrese původně přeložena koncový bod.

Však repliky instancí služby můžete sdílet proces host nebo můžou taky sdílet port při hostovaných na základě HTTP webového serveru, včetně:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [WebListener základní technologie ASP.NET](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

V takovém případě je pravděpodobné, že webový server je k dispozici v procesu Host (hostitel) a reagování na žádosti o ale instance přeložena služby nebo otevřené už není dostupná v hostiteli. V tomto případě brány, dostanou odpověď HTTP 404 z webového serveru. V důsledku toho HTTP 404 obsahuje dva odlišné význam:

 1. Adresy služby je správně, ale zdroj uživatel požádá neexistuje.
 2. Adresy služby je nesprávné a požadovaný zdroj uživatel může skutečně v různých uzlu.

V prvním případě jde normální 404 HTTP se považuje za chybu uživatele. Ve druhém případě uživatel vyžaduje zdroj, který neexistuje však brány se nepodařilo najít, protože služba sama přesunul, přičemž brány musí znovu přeložit adresu a zkuste to znovu.

Brány tak musí být k rozlišení mezi těmito dvěma případy. Před uskutečněním této rozlišení, požaduje tip ze serveru.

 - Ve výchozím nastavení brány aplikace předpokládá případ #2 a pokusí se znovu přeložit a nového vydání žádost.
 - Vyznačení případ #1 k bráně aplikace služby, měly vrátit následující hlavičku HTTP odpovědi:

`X-ServiceFabric : ResourceNotFound`

Toto záhlaví HTTP odpověď označuje normální HTTP 404 situaci, ve kterém požadovaný zdroj neexistuje a brány se pokusí se znovu přeložit adresy služby.

## <a name="setup-and-configuration"></a>Instalace a konfigurace
Reverzního proxy struktury služby může být užitečné pro clusteru prostřednictvím [Správce prostředků Azure šablony](./service-fabric-cluster-creation-via-arm.md).

Až budete mít šablonu, kterou chcete nasadit obrázku (z ukázkové šablony nebo vytvořením šablony Správce vlastních prostředků) reverzního proxy lze povolit v šabloně podle následujících kroků.

1. Definování port pro reverzního proxy v [oddílu parametry](../resource-group-authoring-templates.md) šablony.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Zadejte port pro jednotlivá pole nodetype objektů v **Cluster** [oddíl typ zdroje](../resource-group-authoring-templates.md)

    Pro apiVersion společnosti před "2016-09-01" port je určen parametr název ***httpApplicationGatewayEndpointPort***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Pro apiVersion společnosti na nebo po "2016-09-01" port je určen parametr název ***reverseProxyEndpointPort***

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. Při řešení reverzního proxy z mimo azure clusteru, nastavte **pravidla Vyrovnávání zatížení azure** číslo portu zadali v kroku 1.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Abyste mohli nakonfigurovat certifikáty SSL na port pro reverzního proxy, na tlačítko vlastnost httpApplicationGatewayCertificate v **Cluster** [oddíl typ zdroje](../resource-group-authoring-templates.md)

    Pro apiVersion společnosti před "2016-09-01" certifikát je určen parametr název ***httpApplicationGatewayCertificate***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Pro apiVersion společnosti na nebo po "2016-09-01" certifikát je určen parametr název ***reverseProxyCertificate***
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Další kroky
 - Viz příklad HTTP komunikace mezi službami v [ukázkové projektu na GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Vzdálené volání procedur s Vzdálená spolehlivé služby](service-fabric-reliable-services-communication-remoting.md)

 - [Webového rozhraní API, který používá OWIN spolehlivé služby](service-fabric-reliable-services-communication-webapi.md)

 - [Komunikace pomocí spolehlivé služby WCF](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
