<properties
   pageTitle="Nastavení služby struktury clusteru pomocí aplikace Visual Studio | Microsoft Azure"
   description="Popisuje, jak nastavit clusteru služby struktury pomocí Správce prostředků Azure vytvořený projekt Azure pole Skupina zdroje aplikace Visual Studio"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Nastavení služby struktury clusteru pomocí aplikace Visual Studio
Tento článek popisuje, jak nastavit clusteru struktury služby Azure pomocí Visual Studia a šablony pro správce prostředků Azure. Použijeme Visual Studio Azure zdroje skupinovém projektu Pokud chcete vytvořit šablonu. Po vytvoření šablony ji může být nasazené přímo na Azure z aplikace Visual Studio. Jej lze také pomocí skriptu nebo jako součást zařízení nepřetržitý integrace (odvětví).

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Vytvoření šablony clusteru služby struktury pomocí skupinovém projektu Azure zdroje
Nejdřív otevřete aplikaci Visual Studio a vytvořte projekt skupiny Azure zdroje (je k dispozici ve složce **cloudu** ):

![Dialogové okno Nový projekt s projektem Azure pole Skupina zdroje vybraným][1]

Můžete vytvořit nové aplikace Visual Studio řešení pro tento projekt nebo můžete přidat do existujícího řešení.

>[AZURE.NOTE] Pokud není projekt skupiny Azure zdrojů uzlu cloudu, nemusíte SDK Azure nainstalovaný. Spuštění webové platformy ([nainstalovat teď](http://www.microsoft.com/web/downloads/platform.aspx) máte už není-li) a potom vyhledejte "Azure SDK pro .NET" a nainstalovat verzi, která je kompatibilní s vaší verzí aplikace Visual Studio.

Když kliknete na tlačítko OK, Visual Studio vás vyzve k vyberte správce prostředků šablonu, kterou chcete vytvořit:

![Vyberte šablonu Azure dialog s vybraná šablona služby struktury obrázku][2]

Vyberte šablonu služby struktury obrázku a znovu klikněte na tlačítko OK. Teď byly vytvořeny projektu a šablon správce prostředků.

## <a name="prepare-the-template-for-deployment"></a>Příprava šablony pro nasazení
Před nasazení šablony k vytvoření clusteru je nutné zadat hodnoty parametrů požadované šablony. Tyto hodnoty parametrů se budou číst z `ServiceFabricCluster.parameters.json` souboru, který je v `Templates` složky projektu skupina zdroje. Otevřete soubor a zadejte tyto hodnoty:

|Název parametru           |Popis|
|-----------------------  |--------------------------|
|adminUserName            |Název účtu správce služby struktury stroje (uzly).|
|Miniatura certifikátu    |Miniatura certifikát, který zabezpečuje clusteru.|
|sourceVaultResourceId    |*Pole číslo ID zdroje* klíčové trezoru, kde je uložena certifikát, který zabezpečuje clusteru.|
|certificateUrlValue      |Adresa URL obrázku certifikátu zabezpečení.|

Šablona Visual Studio služby struktury zdrojů správce vytvoří zabezpečené obrázku, který je chráněn certifikát. Certifikát je určen parametry posledních třech šablony (`certificateThumbprint`, `sourceVaultValue`, a `certificateUrlValue`), a musí existovat v **Azure klíč trezoru**. Další informace o tom, jak vytvořit certifikát zabezpečení obrázku najdete v článku [služby struktury clusteru scénáře zabezpečení](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) článek.

## <a name="optional-change-the-cluster-name"></a>Volitelné: přejmenování obrázku
Každé služby struktury clusteru má název. Vytvoření struktury obrázku v Azure clusteru Určuje název (spolu s Azure oblast) systému DNS (Domain Name) název clusteru. Například, pokud zadáte název svůj cluster `myBigCluster`a (Azure oblast) skupina zdroje, který uspořádá nového obrázku umístěný východoasijských USA, bude název DNS clusteru `myBigCluster.eastus.cloudapp.azure.com`.

Ve výchozím nastavení název clusteru je automaticky a provedena jedinečné připojením příponu náhodné předponu "clusteru". Díky tomu velmi snadné použití šablony jako součást systému **Nepřetržitý integrace** (odvětví). Pokud chcete použít určitý název svého obrázku, který bude dávat smysl, nastavte hodnotu `clusterName` proměnných v souboru šablony správce prostředků (`ServiceFabricCluster.json`) na zvolený název. Je první proměnná definované v tomto souboru.

## <a name="optional-add-public-application-ports"></a>Volitelné: Přidání porty veřejné aplikací
Můžete také změnit porty veřejné aplikací pro clusteru před jeho zavedením. Ve výchozím nastavení šablony se objeví jenom dvěma veřejné porty TCP (80 a 8081). Pokud potřebujete další aplikace, upravte Vyrovnávání zatížení Azure v šabloně. Definice uložené v souboru hlavní šablony (`ServiceFabricCluster.json`). Otevřete tento soubor a vyhledejte `loadBalancedAppPort`. Každý port je přidružená k tři artefakty:

1. Proměnné šablony, který definuje hodnoty TCP port číslo portu:

    ```json
    "loadBalancedAppPort1": "80"
    ```

2. *Zkušební* , který definuje, jak často a za jak dlouho Azure náklad vyrovnávání pokusí o přístup konkrétní uzel služby struktury před při přechodu na jiný. Sond jsou součástí služby Vyrovnávání zatížení zdroje. Tady je definici zkušební pro první výchozí port aplikace:

    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```

3. *Vyrovnávání zatížení pravidla* , která přiřazuje společně s portem a zkušební, což umožňuje přes sadu uzlech struktury služby Vyrovnávání zatížení:

    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
V případě aplikace, které plánujete nasazení clusteru další porty, můžete je přidat tak, že vytváření další zkušební a definic pravidel pro vyrovnávání zatížení. Další informace o tom, jak pracovat s služby Vyrovnávání zatížení Azure pomocí Správce prostředků šablon najdete v článku [začít vytvářet Vyrovnávání zatížení interní pomocí šablony](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>Nasazení šablony pomocí aplikace Visual Studio
Po uložení všechny potřeba parametr hodnoty`ServiceFabricCluster.param.dev.json` soubor, jste připraveni nasazení šablony a vytvořte svůj cluster struktury služby. Klikněte pravým tlačítkem myši projekt skupiny zdrojů v okně Průzkumník řešení Visual Studia a zvolte **nasazení | Nové nasazení...**. V případě potřeby Visual Studio se zobrazí dialogové okno **nasadit do pole Skupina zdroje** s žádostí o ověření Azure:

![Nasazení do dialogového okna pole Skupina zdroje][3]

Dialogové okno můžete vybrat existující skupiny zdrojů správce prostředků pro cluster a nabídne vám možnost vytvořit novou. Za normálních okolností smysl pro účely clusteru služby struktury skupina samostatné zdroje.

Po klikněte na tlačítko Instalovat aplikace Visual Studio vás vyzve k potvrzení hodnoty parametrů šablony. Klikněte na tlačítko **Uložit** . Jeden parametr nemá trvalých hodnotu: heslo účtu správce obrázku. Budete muset zadat heslo hodnotu při vyzve aplikace Visual Studio za jiný.

>[AZURE.NOTE] Začnete s Azure SDK 2.9, Visual Studio podporuje čtení hesel z **Trezoru klíč Azure** během nasazení. V dialogovém okně Šablona parametry Všimněte si, že `adminPassword` parametr textové pole obsahuje ikonku "klávesy" na pravé straně. Tato ikona umožňuje vybrat existující tajná klíčové trezoru jako heslo správce pro clusteru. Ujistěte se první povolit správce prostředků Azure access pro nasazení šablony v zásady přístupu Upřesnit trezoru klíče. 

Můžete sledovat průběh procesu nasazení v okně výstupu Visual Studio. Po dokončení nasazení šablony se chystá použít novou clusteru!

>[AZURE.NOTE] Pokud prostředí PowerShell byl ke správě Azure z počítače, který používáte nyní, musíte udělat nevelká domácnosti.
>1. Povolení skriptů Powershellu spuštěním [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) příkaz. Vývojové počítačích je obvykle přijatelné "neomezený" zásad.
>2. Rozhodnutí o povolení shromažďování diagnostiky dat z Azure PowerShell příkazy a spusťte [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) nebo [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) podle potřeby. To zabrání nepotřebných pokynů během nasazení šablony.

Pokud jsou tu uvedené všechny chyby, přejděte na [portál Azure](https://portal.azure.com/) a otevřete skupina zdroje, které nasazené na. Klikněte na **všechna nastavení**a potom na zásuvné nastavení klikněte na **nasazení** . Selhání nasazení skupina zdroje ponechá podrobné diagnostické informace tam.

>[AZURE.NOTE] Služba struktury clusterů vyžadují počtu uzlů být nahoru, abyste zachovali dostupnost a zachovat stav - označovány jako "Správa kvora." Proto není bezpečný vypnout všechny strojů clusteru Pokud jste provedli [úplné zálohování váš stav](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Další kroky
- [Další informace o nastavení služby struktury clusteru pomocí portálu Azure](service-fabric-cluster-creation-via-portal.md)
- [Naučte se spravovat a nasazení služeb struktury aplikace Visual Studio](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
