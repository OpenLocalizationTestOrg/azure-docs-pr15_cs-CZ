<properties
   pageTitle="Začínáme s nasazení a upgrade aplikace na svůj místní cluster | Microsoft Azure"
   description="Nastavení místní clusteru služby struktury a nasazení aplikace existující k němu a upgrade aplikace."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="ryanwi;mikhegn"/>

# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Začínáme s nasazení a upgrade aplikace ve vaší místní obrázku
SDK struktury služby Azure zahrnuje úplnou místní vývojové prostředí, které můžete použít k rychlému nasazení a Správa aplikací místní clusteru. V tomto článku Vytvoření místní clusteru, nasazení existující aplikaci k němu a upgrade aplikace novou verzi, všechny z prostředí Windows PowerShell.

> [AZURE.NOTE] V tomto článku se předpokládá, který jste už [nastavení vývojové prostředí](service-fabric-get-started.md).

## <a name="create-a-local-cluster"></a>Vytvoření místní obrázku
Služba struktury obrázku představuje sady hardwaru zdrojů, které nasadíte aplikací. Obvykle clusteru je tvořen kdekoli z pěti pro mnoho tisíce počítačích. Však SDK struktury služby obsahuje konfigurace clusteru, mohlo by umožnit spuštění na jednom počítači.

Je důležité pochopit místní cluster struktury služba není emulátoru nebo simulator. Spustí se téhož kódu platformy, které se nacházejí ve více počítačů clusterů. Pouze rozdíl je, že běží platformy procesů, které jsou obvykle rozšířit mezi pět počítačů na jednom počítači.

V SDK obsahuje dva způsoby, jak nastavit místní clusteru: skriptu prostředí Windows PowerShell a Správce clusteru místní systém panelu aplikace. V tomto kurzu budeme pomocí skriptů Powershellu.

> [AZURE.NOTE] Pokud jste už vytvořili místní obrázku tak, že nasazení aplikace Visual Studio, můžete tuto část přeskočit.


1. Nové okno prostředí PowerShell spusťte jako správce.

2. Spusťte instalační skript obrázku ve složce SDK:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Instalace trvá jenom chvíli. Po dokončení instalace byste měli vidět výstup:

    ![Výstup vzhled obrázku][cluster-setup-success]

    Teď jste připraveni zkusit nasazením aplikace na svůj cluster.

## <a name="deploy-an-application"></a>Nasazení aplikace
SDK struktury služby obsahuje celá řada rámce a vývojář nástroje pro vytváření aplikací. Pokud vás zajímají naučit, jak vytvářet aplikace ve Visual Studiu, najdete v článku [Vytvoření aplikace služby struktury první ve Visual Studiu](service-fabric-create-your-first-application-in-visual-studio.md).

V tomto kurzu použijeme existující ukázkové aplikaci (nazývané WordCount) tak, aby můžeme se zaměřit na Správa aspekty platformu – včetně nasazení, sledování a upgrade.


1. Nové okno prostředí PowerShell spusťte jako správce.

2. Importujte modul služby struktury SDK Powershellu.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Vytvořte adresář, který chcete uložit aplikace, která můžete stáhnout a nasadit, například C:\ServiceFabric.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. [Stáhněte si aplikaci WordCount](http://aka.ms/servicefabric-wordcountapp) do umístění jste vytvořili.  Poznámka: prohlížeče Microsoft Edge uloží soubor s příponou *ZIP* .  Změňte příponu souboru *.sfpkg*.

5. Připojení k místní obrázku:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Vytvořte novou aplikaci pomocí příkazu nasazení v SDK s názvem a cestu k balíček aplikace.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    V případě všechny dobře, měli byste vidět výstup takto:

    ![Nasazení aplikace do místní obrázku][deploy-app-to-local-cluster]

7. Provedení akce zobrazíte spuštění prohlížeč a přejděte na [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). Měli byste vidět:

    ![Nasazení aplikace uživatelského rozhraní][deployed-app-ui]

    Aplikace WordCount je velmi jednoduché. Obsahuje kód v JavaScriptu klienta pro generování náhodného pět znaků "slova", což jsou potom přenos aplikaci prostřednictvím rozhraní API webových ASP.NET. Stavová služba sleduje počet slov počítá. Jsou odděleny podle prvního znaku slova. O aplikaci WordCount v [Začínáme ukázky](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/)najdete zdrojového kódu.

    Aplikace, které jsme nasazené obsahuje čtyři oddíly. Aby slova začíná písmenem A až G máte uložené v první oddíl, slova začínající H až N jsou uloženy v druhý oddíl atd.

## <a name="view-application-details-and-status"></a>Zobrazení podrobností o aplikaci a stavu
Teď můžeme nasazení aplikace Podívejme se na část Podrobnosti aplikace v prostředí PowerShell.

1. Dotazu všechny nasazeném aplikací na clusteru:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Za předpokladu, že jenom nasadili aplikaci WordCount, uvidíte podobnou:

    ![Dotazu všechny nasazeném aplikací v prostředí PowerShell][ps-getsfapp]

2. Přejděte na vyšší úrovni pomocí dotazu nastavení služby, které jsou součástí aplikace WordCount.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Seznam služeb aplikací v prostředí PowerShell][ps-getsfsvc]

    Aplikace se skládá ze dvou služeb – webových front-end a stavová služba, která spravuje slova.

3. Nakonec podívejte se na seznam oddílů pro WordCountService:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![Zobrazení oddílů služby v prostředí PowerShell][ps-getsfpartitions]

    Příkazy, které jste použili, jako jsou všechny příkazy Powershellu struktury služby, jsou k dispozici pro obrázku, který může připojit k místní nebo vzdálené.

    Vizuální způsob, jak chcete provést interakci s clusteru můžete použít nástroj webové služby struktury Explorer tak, že přejdete do [http://localhost:19080/Explorer](http://localhost:19080/Explorer) v prohlížeči.

    ![Zobrazení podrobností o aplikaci v Průzkumníkovi struktury služby][sfx-service-overview]

    > [AZURE.NOTE] Další informace o Explorer struktury služby, najdete v článku [vizualizace svůj cluster v Průzkumníkovi struktury služby](service-fabric-visualizing-your-cluster.md).

## <a name="upgrade-an-application"></a>Upgrade aplikace
Služba struktury zajišťuje bez prostoje upgrady sledování stavu aplikace zahrne přes clusteru. Pojďme provádět jednoduché upgrade aplikace WordCount.

Novou verzi aplikace nyní počítat pouze slova, které začínají samohláska. Jak upgrade vrátí, vidíme dvě změny v chování aplikace. Nejdřív by měl zpomalit sazba niž počet roste, protože se počítají méně slov. Za druhé vzhledem k tomu první oddíl obsahuje dva samohláskami (A a E) a všechny ostatní oddíly obsahovat pouze jedné, jeho počet by měl postupně začít outpace ostatní.

1. [Stáhnout balíček v2 WordCount](http://aka.ms/servicefabric-wordcountappv2) do stejného umístění, kam jste stáhli balíček v1.

2. Vraťte se do okna prostředí PowerShell a příkazem v SDK upgradu zaregistrovat novou verzi clusteru. Začněte upgrade struktury: / WordCount aplikace.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Měli byste vidět výstup v prostředí PowerShell, který vypadá podobně jako tento jako upgradu, nebude zahájen.

    ![Aktualizace průběhu v prostředí PowerShell][ps-appupgradeprogress]

3. Při upgradu řízení, může najít snadněji sledovat její stav z Průzkumníka struktury služby. Spuštění okně prohlížeče a přejděte na [Http://localhost:19080/Průzkumníka](http://localhost:19080/Explorer). Rozbalení **aplikací** ve stromové struktuře na levé straně a pak zvolte **WordCount**a nakonec **struktury: / WordCount**. Na kartě základy zobrazí se stavu upgradu jako vytvářejí prostřednictvím upgradu domény obrázku.

    ![Aktualizace průběhu v Průzkumníku struktury služby][sfx-upgradeprogress]

    Jak upgrade probíhá přes každou doménu, kontroly stavu provádí zajistit, že aplikace nechová správně.

4. Pokud je znovu spustit starší dotazu pro nastavení služeb v struktury: / WordCount aplikace, Všimněte si, že verze WordCountService změnit, ale verze WordCountWebService not:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Dotaz aplikačních služeb po upgradu][ps-getsfsvc-postupgrade]

    Zvýrazní se způsob, jakým služba struktury spravuje upgrady aplikace. Dotkl pouze požadovanou sadu služby (nebo kód/konfigurace balíčků v těchto služeb) změnily, takže procesu upgradu rychlejší a spolehlivější.

5. Nakonec vraťte se do prohlížeče sledujte chování novou verzi aplikace. Podle očekávání, počet přejde pomaleji a první oddíl má na konci něco víc hlasitost.

    ![Zobrazení nové verzi aplikace v prohlížeči][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Vyčištění

Před shrnutí, je důležité si pamatovat, že je místní cluster typu real. Aplikace se nadále běží na pozadí, dokud je odebrat.  Podle povahy aplikace spuštěné aplikace může trvat až významné prostředkům ve vašem počítači. Máte několik možností spravovat aplikace a obrázku:

1. Pokud chcete odebrat jednotlivé aplikace a její data, spusťte následující:

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Nebo odstranění aplikace z nabídky **Akce** služby struktury Průzkumníka nebo místní nabídky v seznamu aplikací v levém podokně.

    ![Odstranění aplikace je Explorer struktury služby][sfe-delete-application]

2. Po odstranění aplikace z clusteru, můžete unregister verze 1.0.0 a 2.0.0 typu WordCount aplikace. Odstranění odebere balíčků aplikací, včetně kódu a konfigurace z úložiště obrázek obrázku.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Nebo v Průzkumníku služby struktury zvolte **Typ rušení vytváření** aplikace.

3. Vypnutí clusteru, ale zachovat data aplikací a trasování, klikněte na **Zastavit místní obrázku** v aplikaci panelu systému.

4. Úplně odstranit clusteru, klikněte na **Odebrat místní obrázku** v aplikaci panelu systému. Tuto možnost, bude výsledkem jiného pomalé nasazení při příštím Stisknutím F5 ve Visual Studiu. Odebrání místní cluster pouze v případě, že nechcete použít pro určitou dobu nebo v případě potřeby uvolnit prostředky.

## <a name="1-node-and-5-node-cluster-mode"></a>1 uzel a 5 režim clusteru uzel

Když pracujete s místní clusteru k vývoji aplikací, často nenajdete dělat snadné iterací psaní kódu, ladění, změna kódu, ladění atd. Pro optimalizaci tento proces, místní obrázku, můžete v spustit dvěma způsoby: 1 uzel nebo 5 uzel. Obě režimy clusteru mají svoje výhody.
5 režim clusteru uzel umožňuje pracovat s reálným obrázku. Můžete otestovat scénáře převzetí a pracovat s víc instancí a kopie svých služeb.
1 režim uzel clusteru je optimalizována rychlého nasazení a registrace služeb, můžete rychle ověřit kód pomocí modul runtime služby struktury.

1 uzel clusteru režimu i 5 režim clusteru uzel není emulátoru nebo simulator. Spustí se téhož kódu platformy, které se nacházejí ve více počítačů clusterů.

> [AZURE.NOTE] Tato funkce je k dispozici v SDK verze 5,2 nad.

Změna způsobu obrázku na 1 uzel obrázku, obrázku správce služby struktury místní nebo pomocí prostředí PowerShell následujícím způsobem:

1. Nové okno prostředí PowerShell spusťte jako správce.

2. Spusťte instalační skript obrázku ve složce SDK:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```

    Instalace trvá jenom chvíli. Po dokončení instalace byste měli vidět výstup:
    
    ![Výstup vzhled obrázku][cluster-setup-success-1-node]

Pokud používáte Správce služeb struktury místní obrázku:

![Přepnout režim obrázku][switch-cluster-mode]

> [AZURE.WARNING] Při změně režim clusteru, aktuální clusteru je odebraná ze systému a vytvoření nového obrázku. Data, která musí uložených v clusteru, budou odstraněny při změně režim clusteru.

## <a name="next-steps"></a>Další kroky
- Teď máte nasazeném a upgradovat některé předdefinovaných aplikace, můžete [zkusit vytváření vlastní ve Visual Studiu](service-fabric-create-your-first-application-in-visual-studio.md).
- Na služby [Azure obrázku](service-fabric-cluster-creation-via-portal.md) i lze provést všechny akce provádějí místní cluster v tomto článku.
- Upgrade, kterou jsme provést v tomto článku je základní. Najdete v článku [upgrade si přečtěte následující dokumentaci](service-fabric-application-upgrade.md) Další informace o výkon a flexibilitu inovací struktury služby.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
