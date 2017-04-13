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


# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Přidání runbooks Azure automatické obnovení plánů


Tento kurz popisuje, jak lze integrovat s Azure automatizaci poskytnout poskytovaným plány obnovy obnovení webu Azure. Obnovení plány organizovat obnovení svého virtuálních počítačích chránit pomocí obnovení webu Azure replikace sekundární cloudu a replikace Azure scénáře. Pomáhají také při obnovení **přesné** **opakující**a **Automatické**. Pokud se nedaří myši virtuálních počítačích na Azure, integrace se službou Azure automatizaci rozšiřuje plány obnovy a nabídne vám možnosti provést runbooks, takže výkonné automatizaci úkolů.

Pokud jste to ještě slyšet ještě automatizace Azure, zaregistrovat se [tady](https://azure.microsoft.com/services/automation/) a stáhněte si ukázky skriptů [tady](https://azure.microsoft.com/documentation/scripts/). Přečtěte si další informace o [Obnovení webu Azure](https://azure.microsoft.com/services/site-recovery/) a jak organizovat obnovení Azure pomocí plány obnovy [tady](https://azure.microsoft.com/blog/?p=166264).

V tomto kurzu se podíváme na jak integrovat Azure automatizaci runbooks plány obnovení. Změníme automatizovat jednoduché úkoly, které dřívější vyžaduje ruční zásah a zjistěte, jak převést obnovení vícekrokové akci jediným klepnutím obnovení. Podíváme se také na tom, jak můžete vyřešit jednoduchý skript pokud přejde nepovedlo.

## <a name="customize-the-recovery-plan"></a>Přizpůsobení plánu obnovy

1. Dejte nám začněte operning zásuvné zdroje obnovení plánu. Vidíte, že plánu obnovy má dvě virtuálních počítačích do něj přidali k obnovení. 

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
---------------------

2. Klikněte na tlačítko Vlastní můžete začít přidávat postupu runbook. Otevře se plánu obnovy přizpůsobení zásuvné.


    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


3. Klikněte pravým tlačítkem na skupinu zahájení 1 a vyberte Přidat "přidat akci post".

4. Výběr textu v nové zásuvné zvolte skriptu.

5. Pojmenujte skript "Ahoj světe".

6. Vyberte název účtu automatizaci. Jedná se o účet Azure automatizaci. Nezapomeňte, že tento účet v Azure geography může být ale musí být v rámci stejného předplatného jako trezoru obnovení webu.

7. Vyberte postupu runbook z účtu automatizaci. Toto je skript, který se spustí při provádění plánu obnovy po obnovení první skupiny.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)


8. Klikněte na OK uložte skript. Přidá skript ke skupině příspěvek akce skupiny 1: zahájení.


    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>Důležité body obrazovky pro přidání skriptu

1. Můžete klikněte pravým tlačítkem na skript a zvolte "odstranit krok" nebo "aktualizovat skript".

2. Skript je možné spouštět na Azure při převzetí z místního na Azure a je možné spouštět na Azure jako primární straně skript před vypnutí během navrácení z Azure do místního nasazení.

3. Při spuštění skriptu se vloží kontextu plán obnovení.

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
CloudServiceName (v modelu nasazení Správce prostředků) | Pole Skupina zdroje Azure název pod kterým se vytvoří virtuální počítač.


## <a name="using-complex-variables-per-recovery-plan"></a>Pomocí proměnných komplexního za obnovení plán

V některých případech postupu runbook vyžaduje další informace o než jenom RecoveryPlanContext. Neexistuje žádný prvotřídní mechanismus parametru předat postupu runbook. Ale pokud chcete použít stejné skript prostřednictvím více plány obnovy pomocí proměnnou obnovení plánování kontextu "RecoveryPlanName" a použít pod pokusné postup použít proměnnou komplexního Azure automatizaci v postupu runbook. Jak můžete vytvořit tři různé proměnné komplexního prostředky a jejich použití v postupu runbook podle názvu plánu obnovy ukazuje tento příklad.

Zvažte, který chcete použít 3 Další parametry v postupu runbook. Dejte nám kódovat do formuláře JSON {"Var1": "testautomation", "Var2": "Neplánované", "Var3": "PrimaryToSecondary"}

Vytvoření nového majetku automatizaci pomocí [AA komplexního proměnné](../automation/automation-variables.md#variable-types Complex variable) .
Název proměnné jako <RecoveryPlanName>- parametry.
Odkaz na zde umožňuje vytvořit [komplexní proměnné](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396).

Plány různých obnovy název proměnné jako

1. recoveryPlanName1 >-Parametry
2. recoveryPlanName2 >-Parametry
3. recoveryPlanName3 >-Parametry

Teď ve skriptu odkazují parametry jako

1. Zjištění názvu RP z $rpname = $Recoveryplancontext proměnná
2. Získání materiálů $paramValue = "$($rpname) parametry"
3. Používejte tyto informace jako proměnnou komplexního pro plán obnovení tak, že zavoláte Get-AzureAutomationVariable [-AutomationAccountName] <String> – název $paramValue.

Jako příklad zobrazíte komplexního proměnné/parametr pro plán obnovení SharepointApp Vytvořte proměnnou komplexního Azure automatizaci s názvem "SharepointApp parametry".

Použití v plánu obnovy tak, že extrahování proměnné z materiálů pomocí příkazu Get-AzureAutomationVariable [-AutomationAccountName] <String> [– název] $paramValue. [Tento odkaz Další informace](https://msdn.microsoft.com/library/dn913772.aspx)

Tímto způsobem stejné skript lze použít pro různé obnovení plán uložením konkrétní proměnnou komplexního plán aktiv.

## <a name="sample-scripts"></a>Ukázky skriptů

Úložiště skripty, které můžete importovat přímo do svého účtu automatizaci najdete v článku [Kristian čínštiny OMS úložiště skriptů](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/Solutions/asrautomation)

Skript tady je šablona správce prostředků Azure, budou všechny nasazení pod skriptů

* NSG

Postupu runbook NSG přiřadí veřejnou IP adres pro každý OM v rámci plánování obnovení a jejich virtuální síťové adaptéry připojení k síti skupiny zabezpečení, které vám umožní výchozí komunikace

* PublicIP

Postupu runbook veřejnou IP přiřadí veřejnou IP adresy každé OM v rámci plánování obnovení. Přístup k počítači a aplikace závisí na nastavení brány firewall v rámci každé hosta


* CustomScript

Postupu runbook CustomScript bude přiřaďte každé OM v rámci plánování obnovení veřejnou IP adresy a nainstalujte příponu vlastní skript, který získávat skript, který se hodit při nasazení šablony

* NSGwithCustomScript

NSGwithCustomScript, které postupu runbook přiřadíte veřejnou IP adresy každé OM v rámci plánování obnovení, nainstalovat vlastní skript ve tvaru přípony a připojte virtuální síťové adaptéry NSG povoleny výchozí příchozí a odchozí komunikaci vzdálený přístup pomocí protokolu

## <a name="additional-resources"></a>Další zdroje informací

[Služba Azure automatizace spustili účtu](../automation/automation-sec-configure-azure-runas-account.md)

[Přehled Azure automatizaci] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Přehled Azure automatizaci")

[Ukázky skriptů Azure automatizaci] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Ukázky skriptů Azure automatizaci")
