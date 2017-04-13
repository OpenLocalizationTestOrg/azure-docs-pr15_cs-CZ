<properties
   pageTitle="Vytvoření samostatného obrázku se systémem Windows Azure VMs | Microsoft Azure"
   description="Naučte se vytvářet a spravovat struktury služby Azure obrázku na serverem Windows Azure virtuálních počítačích."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Vytvoření struktury služby clusteru samostatného tři uzel s serverem Windows Azure virtuálních počítačích

V tomto článku se dočtete, jak vytvořit clusteru na serveru s Windows Azure virtuálních počítačích (VMs) pomocí služby struktury samostatný instalační program pro systém Windows Server. Toto je speciální případ [vytvořit a spravovat clusteru se systémem Windows Server](service-fabric-cluster-creation-for-windows-server.md) kde VMs jsou [Serverem Windows Azure VMs](../virtual-machines/virtual-machines-windows-hero-tutorial.md), ale nevytváříte [Azure cloudové služby struktury clusteru služby](service-fabric-cluster-creation-via-portal.md). Rozdíl je, že clusteru služby struktury samostatného vytvořené pomocí následujících kroků zcela spravuje můžete při Azure cloudové služby struktury clusterů spravovaných a upgradovat poskytovatelem služby struktury zdrojů.


## <a name="steps-to-create-the-standalone-cluster"></a>Postup vytvoření samostatného obrázku

1. Přihlaste se k portálu Azure a vytvořte nové Windows serveru 2012 R2 Datacentra virtuálního počítače ve skupině zdroje. Přečtěte si článek [Vytvoření OM Windows Azure portálu](../virtual-machines/virtual-machines-windows-hero-tutorial.md) další podrobnosti.
2. Přidejte pár další Windows serveru 2012 R2 Datacentra VMs do stejné skupiny prostředků. Ověřte, zda má každá VMs stejné správce uživatelské jméno a heslo při vytvoření. Jednou vytvořené byste měli vidět všechny tři VMs ve stejné síti virtuální.
3. Připojení k jednotlivým VMs a vypněte bránu Windows Firewall pomocí [Správce Server, místní Server řídicího panelu](https://technet.microsoft.com/library/jj134147.aspx). Zajistíte tím, že provozu v síti můžete komunikaci mezi strojů. Při připojení k všech počítačů, získat IP adresu otevřením příkazový řádek a zadáním `ipconfig`. Můžete taky tak, že vyberete zdroje virtuální sítě pro skupina zdroje na portálu Azure uvidíte IP adresu každého počítače.
4. Připojení k jedné z VMs a vyzkoušejte další dvě VMs můžete úspěšně ping.
5. Připojení k jedné z VMs a [Stáhnout balíček samostatné struktury služby systému Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) do nové složky v počítači a extrahujte balíček.
6. Otevřete soubor *ClusterConfig.Unsecure.MultiMachine.json* v programu Poznámkový blok a upravte jednotlivých uzlech s tři IP adresy strojů. Změna názvu obrázku nahoře a soubor uložte.  Částečné příklad manifest clusteru jsou uvedeny níže.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. Otevřete [okno ISE Powershellu](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Přejděte do složky, kde extrahovaných stažený samostatného instalační balíček a uložit soubor obrázku. Spusťte tento příkaz Powershellu.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Měli byste vidět prostředí PowerShell spusťte, připojení do všech počítačů a vytvoření clusteru. Po asi minutu, můžete zkontrolovat, zda je clusteru provozní propojením Průzkumníka struktury služby k některé z počítače IP adresu například pomocí `http://10.7.0.5:19080/Explorer/index.html`. Protože se jedná samostatného obrázku pomocí Azure VMs zabezpečit ho bude potřeba [nasadit certifikátů Azure VMs](service-fabric-windows-cluster-x509-security.md) nebo nastavit jeden strojů jako [Windows Server služby Active Directory (AD) řadiče ověřování systému Windows](service-fabric-windows-cluster-windows-security.md), úplně stejně, jako je popsaný v místní.


## <a name="next-steps"></a>Další kroky
- [Vytvoření samostatného služby struktury clusterů ve Windows Server a v Linuxu](service-fabric-deploy-anywhere.md)
- [Přidání nebo odebrání uzly clusteru struktury služeb samostatně](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Konfigurace nastavení pro samostatné Windows obrázku](service-fabric-cluster-manifest.md)
- [Zabezpečené samostatného obrázku v systému Windows pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)
- [Zabezpečené samostatného obrázku v systému Windows pomocí X509 certifikátů](service-fabric-windows-cluster-x509-security.md)
