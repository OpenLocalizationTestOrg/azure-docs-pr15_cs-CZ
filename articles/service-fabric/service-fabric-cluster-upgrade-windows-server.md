<properties
   pageTitle="Upgrade struktury služby samostatného obrázku v systému Windows Server | Microsoft Azure"
   description="Upgrade kód služby struktury a/nebo konfigurace, které se spouští samostatného služby struktury obrázku, včetně nastavení režimu aktualizace obrázku"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Upgrade svůj cluster služby struktury samostatného v systému Windows Server

> [AZURE.SELECTOR]
- [Azure obrázku](service-fabric-cluster-upgrade.md)
- [Samostatného obrázku](service-fabric-cluster-upgrade-windows-server.md)

Navrhování pro zdokonalovány moderní systém, je klíčová k dosažení dlouhodobé úspěch produktu. Služba struktury clusteru je zdroj, kterou vlastníte. Tento článek popisuje, jak můžete zajistit, že clusteru je vždy spuštěna podporovaných verzích struktury služby kód a konfigurace.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Určení verze struktury, která poběží na svůj Cluster

Můžete nastavit svůj cluster stáhnout aktualizací struktury služeb Microsoft vydává novou verzi nebo vybrat verzi podporované struktury, která že se má svůj cluster na. 

Můžete to provést nastavením konfigurace clusteru "fabricClusterAutoupgradeEnabled" jako PRAVDA nebo NEPRAVDA.


>[AZURE.NOTE] Ujistěte se, chcete-li zachovat svůj cluster spouštět vždycky podporovanou verzi struktury služby. Jak a jsme oznámení nové verze služby struktury, v předchozí verzi je označen pro ukončení podpory nejméně 60 dní po. Nové verze oznámená je [na blogu týmu struktury služby](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Je k dispozici a potom vyberte nová verze. 


Svůj cluster můžete upgradovat na novou verzi jenom v případě, že používáte konfigurace uzel výrobní stylu, kde každý uzel služby struktury přidělit na samostatném fyzické nebo virtuálního počítače. Pokud máte vývoj clusteru, pokud existuje více než jedno služby struktury uzlů v jedné fyzické nebo virtuálního počítače, musíte byl svůj cluster a znovu vytvoří novou verzí.


Existují dva různých pracovních postupů pro upgrade svůj cluster nejnovější nebo struktury verzi podporované službě. Pro clusterů, které jste připojení k automaticky stáhnout nejnovější verzi a druhý pro clusterů, které jsou bez připojení k stáhnout nejnovější verzi struktury služby.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Upgrade clusterů s připojením ke stažení nejnovější kód a konfigurace 

Pomocí těchto kroků upgradovat svůj cluster podporovanou verzi, máte připojení k Internetu k [http://download.microsoft.com](http://download.microsoft.com) uzly clusteru 

V rámci clusterů, které jste připojení k [http://download.microsoft.com](http://download.microsoft.com)jsme průběžně kontrolovat dostupnost nové verze struktury služby.


Pokud je k dispozici nová verze struktury služby, balíček stahuje místně do clusteru a zřízení pro upgrade. Kromě sdělovat zákazníka novou verzi systému umístí upozornění stavu explicitní clusteru podobně jako tento:

"Aktuální clusteru verze [verze #] podpory končí [datum].", jakmile clusteru pracuje na nejnovější verzi, nezmizí upozornění.


#### <a name="cluster-upgrade-workflow"></a>Shluk upgradu pracovního postupu.
 
Jakmile se zobrazí upozornění stav obrázku, budete muset postupujte takto:

1. Připojte se k němu z počítače, který má přístup správce do všech počítačů, které jsou označeny jako uzlů v clusteru. Počítač, který se spustí tento skript na nemusí být součástí clusteru

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Vytvořte požadovaný seznam služby struktury, které můžete upgradovat na verze

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Výstup by měla získat nějak takto:

    ![získání struktury verze][getfabversions]

3. Zahájit clusteru upgradovat na jedna z verzí, které je k dispozici pomocí [prostředí PowerShell Start ServiceFabricClusterUpgrade cmd](https://msdn.microsoft.com/library/mt125872.aspx)

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Můžete sledovat průběh upgradu služeb struktury Průzkumníka nebo spuštěním následujícího příkazu prostředí power

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Jakmile máte pevná problémy, jejichž výsledkem vrácení zpět, budete muset zahajte upgrade znovu pomocí následujících stejným způsobem jako před.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Upgrade clusterů s <U>bez připojení</u> si chcete stáhnout nejnovější kód a konfigurace

Pokud tyto kroky použijte upgradovat svůj cluster podporovanou verzi obrázku uzly **nemají** připojení k Internetu k [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Pokud používáte obrázku, který není internetové připojení, budete muset monitorovat služby struktury týmového blogu k oznámení o nové verze. Systém **nemá** umístěte varování stavu clusteru vás upozorní na to.  

1. Úprava konfiguraci clusteru nastavit následující vlastnost na hodnotu false.

        "fabricClusterAutoupgradeEnabled": false,

a spusťte tak upgrade konfigurace. Vyhledejte [Start ServiceFabricClusterUpgrade PS cmd](https://msdn.microsoft.com/library/mt125872.aspx) podrobnostmi používání. Verze seznamu clusteru je verze, kterou máte v clusterConfig.JSON. Zkontrolujte, že ji před zahájení vypnout upgrade konfigurace.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Shluk upgradu pracovního postupu.
 


1. Stáhněte si nejnovější verzi balíček z dokumentu [vytvořit služby struktury cluster v systému windows server](service-fabric-cluster-creation-for-windows-server.md) 


1. Připojte se k němu z počítače, který má přístup správce do všech počítačů, které jsou označeny jako uzlů v clusteru. Počítač, který se spustí tento skript na nemusí být součástí clusteru 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Zkopírujte stažený balíček do úložiště obrázek obrázku.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. Registrace zkopírovaný balíčku 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Zahájit clusteru upgradovat na jedna z verzí, které je k dispozici. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Můžete sledovat průběh upgradu služeb struktury Průzkumníka nebo spuštěním následujícího příkazu prostředí power

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Jakmile máte pevná problémy, jejichž výsledkem vrácení zpět, budete muset zahajte upgrade znovu pomocí následujících stejným způsobem jako před.



## <a name="next-steps"></a>Další kroky
- Zjistěte, jak upravit některá [Nastavení struktury clusteru struktury služby](service-fabric-cluster-fabric-settings.md)
- Zjistěte, jak zobrazit [svůj cluster nebo zmenšit](service-fabric-cluster-scale-up-down.md)
- Další informace o [aplikaci upgrady](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
