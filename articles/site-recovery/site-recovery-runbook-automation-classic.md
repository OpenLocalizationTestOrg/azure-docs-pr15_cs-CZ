<properties
   pageTitle="Přidání runbooks Azure automatické obnovení plánů | Microsoft Azure"
   description="Tento článek popisuje způsob obnovení webu Azure teď můžete rozšířit plány obnovy automatické Azure dokončení úkolů, složité během obnovení Azure"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans---classic"></a>Přidání runbooks Azure automatické obnovení plánů – klasické


Tento kurz popisuje, jak lze integrovat s Azure automatizaci poskytnout poskytovaným plány obnovy obnovení webu Azure. Obnovení plány organizovat obnovení svého virtuálních počítačích chránit pomocí obnovení webu Azure replikace sekundární cloudu a replikace Azure scénáře. Pomáhají také při obnovení **přesné** **opakující**a **Automatické**. Pokud se nedaří myši virtuálních počítačích na Azure, integrace se službou Azure automatizaci rozšiřuje plány obnovy a nabídne vám možnosti provést runbooks, takže výkonné automatizaci úkolů.

Pokud jste to ještě slyšet ještě automatizace Azure, zaregistrovat se [tady](https://azure.microsoft.com/services/automation/) a stáhněte si ukázky skriptů [tady](https://azure.microsoft.com/documentation/scripts/). Přečtěte si další informace o [Obnovení webu Azure](https://azure.microsoft.com/services/site-recovery/) a jak organizovat obnovení Azure pomocí plány obnovy [tady](https://azure.microsoft.com/blog/?p=166264).

V tomto krátkém kurzu se podíváme na jak integrovat Azure automatizaci runbooks plány obnovení. Změníme automatizovat jednoduché úkoly, které dřívější vyžaduje ruční zásah a zjistěte, jak převést obnovení krok více akci jediným klepnutím obnovení. Podíváme se také na tom, jak můžete vyřešit jednoduchý skript pokud přejde nepovedlo.

## <a name="protect-the-application-to-azure"></a>Zamknout aplikaci Azure

Dejte nám začněte jednoduchou aplikaci skládající se ze dvou virtuálních počítačích. Tady máme aplikace HRweb Fabrikam. Fabrikam HRweb frontend a Fabrikam Hrweb backendovou jsou dvě virtuálních počítačích chráněny Azure pomocí obnovení webu Azure. Pokud chcete zamknout virtuálních počítačích pomocí obnovení webu Azure, postupujte následujícím způsobem.

1.  Povolte ochranu virtuálních počítačích.

2.  Zkontrolujte, že virtuálních počítačích dokončili počáteční replikace a jsou replikace.

3.  Počkejte, dokud počáteční replikace dokončení a stav replikace se objeví chráněného.

![](media/site-recovery-runbook-automation/01.png)
---------------------

V tomto kurzu budeme vytvářet obnovení plán pro aplikace společnosti Fabrikam HRweb převzetí aplikace Azure. Potom jsme bude integrace s postupu runbook vytvoření koncového bodu v selhalo přes Azure virtuálního počítače a bude předávat webových stránek přístavu 80.

Nejdřív vytvoření plánu obnovy naše aplikace.

## <a name="create-the-recovery-plan"></a>Vytvoření plánu obnovy

Když Pokud chcete obnovit aplikace Azure, je potřeba vytvořit plán obnovy.
Použití obnovení plánu, můžete určit pořadí obnovení virtuálních počítačích. Virtuální počítač umístit do skupiny 1 bude obnovit a začněte první a udělejte virtuálního počítače ve skupině 2.

Vytvoření obnovení plán, který bude vypadat podobně jako pod.

![](media/site-recovery-runbook-automation/12.png)

Chcete další informace o různých plánech obnovení, přečtěte si přečtěte následující dokumentaci pro [tady](https://msdn.microsoft.com/library/azure/dn788799.aspx "tady").

Pak v Azure automatizaci vytvoření potřebné artefakty.

## <a name="create-the-automation-account-and-its-assets"></a>Vytvoření účtu automatizace a jeho prostředky

Musíte mít účet Azure automatické vytvoření runbooks. Pokud už nemáte účet, přejděte na kartu automatické Azure symbolem ![](media/site-recovery-runbook-automation/02.png)a vytvořte nový účet.

1.  Zadejte název pro identifikaci s účtu.

2.  Zadejte zeměpisná oblast místo, kam chcete umístit účet.

Doporučujeme provést účtu ve stejné oblasti jako trezoru automatickým obnovením systému.

![](media/site-recovery-runbook-automation/03.png)

Dále vytvořte následující prostředky do účtu.

### <a name="add-a-subscription-name-as-asset"></a>Přidejte název předplatného jako materiálů

1.  Přidání nové nastavení ![](media/site-recovery-runbook-automation/04.png) v Azure automatizaci prostředky a vybrat![](media/site-recovery-runbook-automation/05.png)

2.  Vyberte proměnné typu **řetězec**

3.  Zadejte název proměnné jako **AzureSubscriptionName**

    ![](media/site-recovery-runbook-automation/06.png)

4.  Jako hodnotu proměnné zadejte skutečné název Azure předplatného.

    ![](media/site-recovery-runbook-automation/07_1.png)

Zjistíte název vašeho předplatného ze stránky nastavení vašeho účtu na portálu Azure.

### <a name="add-an-azure-login-credential-as-asset"></a>Přidání Azure přihlašovací pověření jako materiálů

Azure automatizace používá Azure Powershellu pro připojení k odběru a pracuje na artefakty tam. V takovém případě budete muset ověření pomocí účtu Microsoft nebo pracovního nebo školního účtu.
Přihlašovací údaje účtu mohou být uloženy v aktivum bezpečně používaný postupu runbook.

1.  Přidání nové nastavení ![](media/site-recovery-runbook-automation/04.png) v Azure automatizaci prostředky a vyberte položku![](media/site-recovery-runbook-automation/09.png)

2.  Vyberte typ pověření jako **Pověření systému Windows PowerShell**

3.  Zadejte název jako **AzureCredential**

    ![](media/site-recovery-runbook-automation/10.png)

4.  Zadejte uživatelské jméno a heslo pro přihlášení k aplikaci.

Nyní jsou k dispozici ve svém majetku obě tato nastavení.

![](media/site-recovery-runbook-automation/11.png)

Další informace o tom, jak připojit k vašemu předplatnému pomocí prostředí PowerShell jsou uvedeny [v tomto poli](../powershell-install-configure.md).

Pak v Azure automatizaci, které můžete přidat koncový bod pro klientské počítače virtual po převzetí vytvoříte postupu runbook.

## <a name="azure-automation-context"></a>Azure automatizaci kontextu

Funkci Automatické obnovení systému předává postupu runbook vám deterministický skripty proměnnou kontextu. Jednu může uvádějí názvy cloudové služby a virtuální počítač byla předvídatelná, že se stane, že není vždy případ kvůli některé scénáře například je místo, kam byl změněn název položky virtuálního počítače z důvodu nepodporované znaky v Azure. Proto tyto informace předána plánu obnovy automatickým obnovením systému jako součást *kontextu*.

Tady je příklad toho, jak vypadá proměnnou kontextu.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Následující tabulka obsahuje název a popis pro každou proměnnou v kontextu.

**Název proměnné** | **Popis**
---|---
RecoveryPlanName | Název plánu spuštěný. Umožňuje akce na základě názvu pomocí stejného skriptu
FailoverType | Určuje, zda je záložní otestovat, plánované nebo neplánované.
FailoverDirection | Určete, jestli je obnovení na primární a sekundární
ID skupiny | Číslo skupiny v plánu obnovy zjistit, jestli je spuštěný plánu
VmMap | Pole virtuálních počítačích ve skupině
VMMap klíč | Jedinečný klíč (GUID) pro každé OM. Pokud to jde je stejná jako ID VMM virtuálního počítače.
RoleName | Název, který obnovuje OM Azure
CloudServiceName | Azure cloudové služby název v části který je vytvořený virtuální počítač.


Jak identifikovat klávesu VmMap v kontextu lze také přejděte na stránku OM vlastnosti ve funkci Automatické obnovení systému a podívejte se na vlastnost GUID OM.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Autor postupu runbook automatizaci

Nyní vytvořte postupu runbook otevřete portu 80 front-end virtuálního počítače.

1.  Vytvoření nového postupu runbook v Azure automatizaci účet s názvem **OpenPort80**

    ![](media/site-recovery-runbook-automation/14.png)

2.  Přejděte do zobrazení Autor postupu runbook a přejít do režimu návrhu.

3.  Nejprve určete proměnnou pro použití v kontextu plán obnovení

    ```
        param (
            [Object]$RecoveryPlanContext
        )

    ```

4.  Další připojte k předplatnému nazvanou podle přihlašovací údaje a předplatného

    ```
        $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

        # Connect to Azure
        $AzureAccount = Add-AzureAccount -Credential $Cred
        $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
        Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    ```

    Všimněte si, že použijete Azure prostředky – **AzureCredential** a **AzureSubscriptionName** tady.

5.  Teď zadejte podrobnosti o koncovém bodu a GUID virtuálního počítače, pro který chcete vystavit koncový bod. V tomto případě front-end virtuálního počítače.

    ```
        # Specify the parameters to be used by the script
        $AEProtocol = "TCP"
        $AELocalPort = 80
        $AEPublicPort = 80
        $AEName = "Port 80 for HTTP"
        $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
    ```

    Určuje protokol Azure koncový bod, místní portu OM a jeho mapovaných veřejné port. Tyto proměnné jsou parametry vyžadované Azure příkazy, které přidat koncové body VMs. VMGUID obsahuje identifikátor GUID virtuální počítač, který je potřeba pracovat.

6.  Skript teď extrahovat kontextu pro zadaný identifikátor GUID OM a vytvoření koncového bodu v počítači virtuální odkazováno ho.

    ```
        #Read the VM GUID from the context
        $VM = $RecoveryPlanContext.VmMap.$VMGUID

        if ($VM -ne $null)
        {
            # Invoke pipeline commands within an InlineScript

            $EndpointStatus = InlineScript {
                # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
                # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

                $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                    Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                    Update-AzureVM
                Write-Output $Status
            }
        }
    ```

7. Po dokončení, přístupů publikovat ![](media/site-recovery-runbook-automation/20.png) umožňuje skript k dispozici pro spuštění.

Celý skript je uveden níže jako odkaz

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Přidejte skript k plánu obnovení

Jakmile je připraven skript, můžete ho přidat do obnovení plán, který jste vytvořili.

1.  V obnovení plán, na který jste vytvořili zvolte Přidání skriptu za skupinu 2. ![](media/site-recovery-runbook-automation/15.png)

2.  Zadejte název skriptu. Toto je právě popisný název tohoto skriptu pro zobrazení v plánu obnovy.

3.  V převzetí Azure skriptu – vyberte název účtu automatizaci Azure.

4.  V Azure Runbooks vyberte postupu runbook vámi vytvořený.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Primární straně skriptů

Při přepnutí do Azure jsou spuštění, můžete taky spustit skripty primární straně. Tyto skripty se spustí na serveru VMM při selhání.
Primární straně skripty jsou k dispozici pouze pro před vypnutím pouze a publikování vypnutí počítače fází: Je to proto očekáváme primární webu nedostupnost obvykle při selhání odpovídá.
Při neplánované selhání jenom v případě, že můžete určovat, jestli v pro operace primární webu, pokusí se spouštěly skripty primární straně. Pokud jsou dostupné nebo časový limit, záložní, zůstanou obnovit virtuálních počítačích.
Primární straně skripty není zrušením dostupný pro weby VMware/fyzické/Hyper-v VMM chráněny Azure - při přepnutí do Azure.
Ale pokud navrácení z Azure do místního nasazení, skripty stranu primárního (Runbooks) lze použít pro všechny cíle kromě VMware.

## <a name="test-the-recovery-plan"></a>Testování plánu obnovy

Po přidání postupu runbook plán můžete zahájit přepojení test a podívejte se, jak. Doporučujeme vždy spusťte test přepojení otestovat aplikace a obnovení budete chtít zajistit, aby byly bez chyb.

1.  Vyberte plán, obnovení a zahájí selhání testu.

2.  Během spuštění plánu můžete zjistit, zda postupu runbook provedl nebo ne prostřednictvím stavu.

    ![](media/site-recovery-runbook-automation/17.png)

3.  Můžete taky zobrazit stav spuštění podrobné postupu runbook na stránce Azure automatizace úloh postupu runbook.

    ![](media/site-recovery-runbook-automation/18.png)

4.  Po dokončení záložní kromě výsledku spuštění postupu runbook uvidíte spuštění proběhne úspěšně nebo není tak, že na stránce Azure virtuálního počítače a podíváte se koncové body.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Ukázky skriptů

Během jsme projít jednu automatizaci často používané úkolu obrazovky pro přidání koncový bod Azure virtuálního počítače v tomto kurzu, může provést několik dalších výkonné automatizace úkolů pomocí Azure automatizaci. Microsoft a komunity Azure automatizaci poskytují runbooks vzorku, které vám pomohou začít vytvářet vlastní řešení a runbooks nástroj, který slouží jako stavební bloky větší automatizaci úkolů. První kroky v galerii a vytvářet výkonné obnovení jedním kliknutím plány pro aplikace pomocí obnovení webu Azure.

## <a name="additional-resources"></a>Další zdroje informací

[Přehled Azure automatizaci] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Přehled Azure automatizaci")

[Ukázky skriptů Azure automatizaci] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Ukázky skriptů Azure automatizaci")
