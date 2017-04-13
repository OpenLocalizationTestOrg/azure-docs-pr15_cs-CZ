<properties
   pageTitle="Konfigurace samostatného obrázku | Microsoft Azure"
   description="Tento článek popisuje, jak konfigurovat samostatného nebo soukromé služby struktury obrázku."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="dkshir"/>


# <a name="configuration-settings-for-standalone-windows-cluster"></a>Konfigurace nastavení pro samostatné Windows obrázku

Tento článek popisuje, jak nakonfigurovat clusteru samostatného struktury služby pomocí souboru _**ClusterConfig.JSON**_ . Tento soubor můžete zadat informace jako uzly struktury služeb a jejich IP adresy různé typy uzlů u clusteru, konfigurace zabezpečení, jakož i topologie sítě z hlediska poruch/upgrade domény pro svůj cluster samostatně.

Když si [Stáhnout balíček struktury služeb samostatně](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), pár ukázek souboru ClusterConfig.JSON stáhnou do počítače práce. Ukázky *DevCluster* s jejich jména vám pomůže vytvořit clusteru s všechny tři uzly ve stejném počítači jako logické uzlů. Mimo tyto prvky, které musí být alespoň jeden uzel označena jako primární uzel. Tento clusteru je užitečné pro vývoj nebo testovacím prostředí a nepodporuje jako cluster výroby. Ukázky *MultiMachine* s jejich jména vám pomůže vytvořit cluster kvality výrobní s jednotlivých uzlech na samostatném počítač. Počet primární uzlů pro tyto clusteru budou založeny na [úrovni spolehlivosti](#reliability).

1. *ClusterConfig.Unsecure.DevCluster.JSON* a *ClusterConfig.Unsecure.MultiMachine.JSON* ukazují, jak vytvořit nezabezpečenou test nebo výrobní cluster v tomto pořadí. 
    
2. *ClusterConfig.Windows.DevCluster.JSON* a *ClusterConfig.Windows.MultiMachine.JSON* ukazují, jak vytvořit test nebo výrobní clusteru zabezpečit pomocí [zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md).

3. *ClusterConfig.X509.DevCluster.JSON* a *ClusterConfig.X509.MultiMachine.JSON* ukazují, jak vytvořit test nebo výrobní clusteru zabezpečit pomocí [X509 založené na certifikátu zabezpečení](service-fabric-windows-cluster-x509-security.md). 


Teď můžeme prozkoumá v různých částech _**ClusterConfig.JSON**_ soubor jako pod.

## <a name="general-cluster-configurations"></a>Konfigurace obecných clusterů
Toto se vztahuje konkrétní konfigurace obecných clusteru uvedeno v fragment JSON.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

Popisný název můžete přidělit svůj cluster služby struktury přiřazením k proměnná **název** . **ClusterConfigurationVersion** je číslo verze clusteru; měli byste zvýšit ji pokaždé, když upgradujete svůj cluster struktury služby. **ApiVersion** však nechejte výchozí hodnotu.


<a id="clusternodes"></a>
## <a name="nodes-on-the-cluster"></a>Uzlů v clusteru
Uzly můžete nakonfigurovat pro svůj cluster služby struktury pomocí **uzly** oddílu, jak ukazuje následující fragment kódu.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Služby struktury clusteru musí obsahovat alespoň na úrovni 3 uzlů. Můžete přidat další uzly k tomuto podle nastavení. Následující tabulka uvádí nastavení jednotlivých uzlech.

|**Konfigurace uzel**|**Popis**|
|-----------------------|--------------------------|
|název_uzlu|Zadejte popisný název na uzel.|
|Adresa IP|Zjistit IP adresu uzel otevřením okna příkazového řádku a zadáním `ipconfig`. Poznamenejte si adresu IPV4 a přiřadit ji někomu proměnnou **adresa IP** .|
|nodeTypeRef|Je možné stanovit jednotlivých uzlech typu různých uzel. [Typy uzlů](#nodetypes) jsou definované v následující části.|
|faultDomain|Poruchy domény umožňují správcům clusteru definovat fyzické uzly, které může selhat ve stejnou dobu vzhledem k závislostem sdílené fyzické.|
|upgradeDomain|Upgrade domény popisují sady uzlů, které jsou vypnout pro upgrade služeb struktury na přibližně ve stejnou dobu. Můžete uzlů, které chcete přiřadit domény které upgradu jako nejsou omezené žádným fyzické požadavky.| 


## <a name="cluster-properties"></a>Vlastnosti obrázku

V části **Vlastnosti** v ClusterConfig.JSON slouží ke konfiguraci clusteru následujícím způsobem.

<a id="reliability"></a>
### <a name="reliability"></a>Spolehlivosti 
V části **reliabilityLevel** definuje počet kopií služeb systému, které je možné spouštět na primární uzly clusteru. Této kombinace kláves rozšíří spolehlivosti těchto služeb a tedy clusteru. Můžete nastavit tuto proměnnou *Bronzová*, *stříbrný*, *Gold* nebo *Platinum* 3, 5, 7 nebo 9 kopií z těchto služeb v tomto pořadí. Viz Příklad dole.

    "reliabilityLevel": "Bronze",
    
Všimněte si, že vzhledem primární uzel jedné kopie systémových služeb, které jsou potřeba nejméně 3 primární uzly pro *Bronzová*5 pro *stříbrný*7 pro *Gold* a 9 *Platinum* spolehlivost.


### <a name="diagnostics"></a>Diagnostika
V části **diagnosticsStore** umožňuje konfigurovat parametry povolit diagnostiky a Poradce při potížích s selhání uzel nebo obrázku, jak je uvedeno v následující ukázce. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

**Metadata** popis obrázku Diagnostika a lze nastavit podle nastavení. Tyto proměnné pomáhají shromažďovat trasování událostí pro Windows protokoly trasování, výpisy i výkonnosti. Další informace o protokolech trasování trasování událostí pro Windows číst [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) a [Trasování trasování událostí pro Windows](https://msdn.microsoft.com/library/ms751538.aspx) . Všechny protokoly včetně [selhat výpisy](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) [výkonnosti](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) můžete dostali ke složce **connectionString** ve vašem počítači. Můžete taky *AzureStorage* pro ukládání diagnostických nástrojů. Fragment výběru najdete níže.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Zabezpečení 
Do části **zabezpečení** je nutný pro clusteru struktury služby Zabezpečené samostatně. Následující úryvek ukazuje součástí v této části.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

**Metadata** je popis toho, zabezpečené obrázku a lze nastavit podle nastavení. **ClusterCredentialType** a **ServerCredentialType** zjistěte typ používaného cenného papíru, provede clusteru a uzlů. Lze nastavit buď *X509* cenného papíru certifikátů nebo *Windows* zabezpečení založené na Azure Active Directory. Zbývající části **zabezpečení** se závislosti na použitém typu zabezpečení. Informace o tom, jak vyplňte zbývající části **zabezpečení** číst [zabezpečení na základě certifikáty do samostatného obrázku](service-fabric-windows-cluster-x509-security.md) nebo [zabezpečení systému Windows do samostatného obrázku](service-fabric-windows-cluster-windows-security.md) .


<a id="nodetypes"></a>
### <a name="node-types"></a>Typy uzlů
V části **nodeTypes** popisuje typ uzly, které má svůj cluster. Typ alespoň jeden uzlu je nutné zadat clusteru, jak je znázorněno níže fragment kódu. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

**Název** je popisný název této konkrétní typ uzlu. Pokud chcete vytvořit uzel tohoto typu uzel, přiřazení jeho popisný název proměnné **nodeTypeRef** uzel, jako [výše](#clusternodes)uvedené. Pro každý typ uzlu definujte koncové body připojení, které se použijí. Libovolné číslo portu pro tyto koncové body připojení můžete zvolit, dokud nedošlo ke konfliktu s všechny koncové body v tomto obrázku. Více uzly clusteru budou jeden nebo více primární uzlů (tedy **isPrimary** nastaveno *true*), v závislosti na [**reliabilityLevel**](#reliability). Přečtěte si [služby struktury kapacita plánování důležité informace o clusteru](service-fabric-cluster-capacity.md) informace o **nodeTypes** a **reliabilityLevel** hodnoty a zjistit, co primární a typy není primární uzlů. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Koncové body používá ke konfiguraci typů uzlů

- *clientConnectionEndpointPort* je port používá klienta pro připojení k obrázku, při použití klientského rozhraní API. 
- *clusterConnectionEndpointPort* je port niž uzly vzájemně komunikovat.
- *leaseDriverEndpointPort* je port používaný ovladač zapůjčení clusteru Pokud chcete zjistit, pokud jsou uzly stále aktivní. 
- *serviceConnectionEndpointPort* je port používaný aplikací a služeb nasazený na uzel, komunikovat s klientem služby struktury na příslušném uzlu.
- *httpGatewayEndpointPort* je port používané Průzkumníkem struktury služby pro připojení k clusteru.
- *ephemeralPorts* jsou [dynamické porty používané operačního systému](https://support.microsoft.com/kb/929851). Služba struktury použije součástí jako porty aplikací a zbývajících bude k dispozici pro operační systém. Je taky namapuje tato oblast do existující oblasti účastní OS, takže ve všech oblastech, můžete použít oblastí v ukázkové JSON soubory. Potřebujete přesvědčte, že je rozdíl mezi počáteční a koncové porty aspoň 255 znaků. 
- *applicationPorts* jsou porty, které se používá v aplikacích služby struktury. Musí být podmnožinu *ephemeralPorts*, dostatečně pokrýval požadovaného koncový bod aplikací. Služba struktury bude to je vhodné použít nové porty jsou potřeba, jakož i starat o otevření brány firewall pro tyto porty. 
- *reverseProxyEndpointPort* je nepovinné reverzního proxy koncový bod. Další informace najdete v článku [Proxy Převrátit struktury služby](service-fabric-reverseproxy.md) . 


### <a name="other-settings"></a>Další nastavení
V části **fabricSettings** umožňuje nastavit kořenového adresáře služby struktury dat a protokolů. Můžete přizpůsobit tyto pouze při vytváření počáteční obrázku. Fragment výběru v této části najdete níže.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Doporučujeme používají jednotku než OS FabricDataRoot a FabricLogRoot poskytuje, že dojde k chybě Další spolehlivost proti s operačním systémem. Všimněte si, že pokud upravíte pouze kořenový data, pak kořenové protokolu budou umístěny o jednu úroveň níže kořenové data.


## <a name="next-steps"></a>Další kroky

Až budete mít celý soubor ClusterConfig.JSON nakonfigurované podle nastavení samostatného obrázku, nasazením svůj cluster podle pokynů v článku [Vytvoření struktury služby Azure clusteru místní nebo v cloudu](service-fabric-cluster-creation-for-windows-server.md) a potom přejděte k [vizualizace svůj cluster v Průzkumníkovi struktury služby](service-fabric-visualizing-your-cluster.md).


