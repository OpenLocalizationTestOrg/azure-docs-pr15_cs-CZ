<properties
   pageTitle="Odstranění běžných chyb Azure nasazení | Microsoft Azure"
   description="Popisuje, jak vyřešit běžné chyby při nasazení zdroje Azure pomocí Správce prostředků Azure."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   keywords="Chyba nasazení, azure nasazení nasadit na azure"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Odstranění běžných chyb Azure nasazení pomocí Správce prostředků Azure

Toto téma popisuje, jak můžete vyřešit mezi běžné chyby Azure nasazení může dojít. Pokud budete potřebovat další informace o co je špatně s nasazením, přečtěte si nejprve [Zobrazit nasazení operace](resource-manager-troubleshoot-deployments-portal.md) a pak se vrátit na tento článek pomoc s řešením chyby do schránky. Oddíly v tomto tématu seznam, který se zobrazí kód chyby.

## <a name="invalid-template"></a>Neplatné šablony

Tuto chybu může způsobit z několika různých typů chyb. 

### <a name="syntax-error"></a>Syntaktickou chybu

Pokud se zobrazí chybová zpráva, která označuje ověření šablony se nepodařilo, bude pravděpodobně syntaxi problém v šabloně. 

    Code=InvalidTemplate 
    Message=Deployment template validation failed

Tato chyba se dá snadno vytvořit, protože šablony může jít o komplikované. Například následující přiřazení název účtu úložiště obsahuje jednu sadu závorky tři funkce, tři sady závorek, jednu sadu apostrofy a jednu vlastnost:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Pokud nezadáte odpovídající syntaxe, šabloně vytvoří hodnotu, která je jiná než svůj úmysl.

Když dostanete tento typ chyby, pečlivě zkontrolujte syntaxi výrazů. Zvažte použití editoru JSON jako [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) nebo [Visual Studio kód](resource-manager-vs-code.md), který můžete vás upozorní na chyby syntaxe. 

### <a name="incorrect-segment-lengths"></a>Nesprávný segmentu délky

Jiné neplatná šablona dojde k chybě název zdroje není ve správném formátu.

    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}' 
    for type {resource-type} has incorrect segment lengths.

Kořenové úrovni zdroje musí být menší segmentů název než v seznamu Typ zdroje. Každý segmentu se liší podle lomítko. V následujícím příkladu typ obsahuje dva segmenty a název obsahuje jeden segment, takže je **platný název**.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }

Ale v následujícím příkladu **není platný název** , protože nemá stejný počet segmentů typ.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }

U podřízené zdroje typ a název mít stejný počet segmentů. Tento počet segmentů dává smysl Jelikož úplný název a typ podsložky nadřazené název a typ. Úplný název tedy stále obsahuje jeden méně segment než úplné typ. 

    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]

Získání segmenty správné může být záludné s typy správce prostředků, které jsou použity přes poskytovatelů zdroje. Například použití zámku zdroje na webový server vyžaduje typ se čtyřmi segmentů. Název tedy tři segmentů:

    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }

### <a name="copy-index-is-not-expected"></a>Kopírovat index není správně

Tento **InvalidTemplate** k chybě dojde při použitého element **Kopírovat** do části šablony, která nepodporuje: Tento element. Element Kopírovat lze použít pouze pro typ zdroje. Kopírovat nelze použít vlastnost v rámci typu zdroje. Například použijete kopírovat do virtuálního počítače, ale nemůžete ho vyrovnat s operačním systémem disků virtuálního počítače. V některých případech můžete převést podřízené zdroj zdroj nadřazené vytvořit smyčku kopírovat. Další informace o používání kopírovat najdete v článku [vytvoření několika instancích zdrojů v Azure správce](resource-group-create-multiple.md).

### <a name="parameter-is-not-valid"></a>Parametr není platný

Pokud šablona určuje povolené hodnoty parametru a zadejte hodnotu, která není jedním z těchto hodnot, zpráva podobná chybová zpráva:

    Code=InvalidTemplate; 
    Message=Deployment template validation failed: 'The provided value {parameter vaule} 
    for the template parameter {parameter name} is not valid. The parameter value is not 
    part of the allowed value(s)

Nastavení dvojitých zkontrolujte povolené hodnoty v šabloně a zadejte při nasazení.

## <a name="not-found-or-resource-not-found"></a>Nebyla nalezena (nebo zdroje nebyl nalezen)

Pokud šablona obsahuje název zdroje, který nelze převést, chybová podobně jako:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Pokud se pokoušíte nasazení chybějící prostředků v šabloně, zkontrolujte, jestli budete muset přidat závislost. Správce prostředků optimalizuje nasazení vytvořením zdroje paralelně, pokud je to možné. Pokud jeden zdroj musí být nasazené po jiného zdroje, musíte použít **dependsOn** element v šabloně vytvoříte závislost na jiných zdrojů. Třeba když nasadíte do webových aplikací, musí existovat plán služeb aplikací. Pokud nezadali jste, že v prohlížeči závisí na tom, plán služeb aplikací, správce vytvoří oba zdroje ve stejnou dobu. Se chybová zpráva, že nelze najít aplikaci služby plán zdrojů, protože není k dispozici ještě při pokusu o vlastnost na web appu. Tato chyba zabránit nastavením závislost ve web appu.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }
 
Taky uvidíte tuto chybu při zdroje existuje v jiné skupině zdrojů než nasazených na. V takovém případě pomocí [funkce resourceId](resource-group-template-functions.md#resourceid) získat plně kvalifikovaný název zdroje.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    } 

Pokud budete chtít použít funkce [odkaz](resource-group-template-functions.md#referenc) nebo [listKeys](resource-group-template-functions.md#listkeys) s zdroj, který nelze přeložit, se zobrazí následující chyba:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource 
    group {resource group name} was not found.

Vyhledejte výraz, který je taky funkce **odkaz** . Dvojité zkontrolujte správnost hodnoty parametrů.

## <a name="storage-account-already-exists-or-already-taken"></a>Již existuje účet úložiště (nebo již)

U účtů úložiště je nutné zadat název zdroje, které jsou jedinečné přes Azure. Pokud nezadáte jedinečný název, dojít k chybě jako:

    Code=StorageAccountAlreadyTaken 
    Message=The storage account named mystorage is already taken.

Vytvořit jedinečný název zřetězením zásady vytváření názvů s výsledkem funkce [uniqueString](resource-group-template-functions.md#uniquestring) .

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Pokud nasazení úložiště účtu se stejným názvem jako existující úložiště účet předplatného, ale zadejte do jiného umístění, dojít k chybě označovat tak, že již existuje úložiště účet do jiného umístění. Odstranění existující účtu úložiště, nebo zadejte stejné jako stávající účtu úložiště umístění.

## <a name="account-name-invalid"></a>Název účtu neplatné

Zobrazí chybová zpráva **AccountNameInvalid** při pokusu o název účtu úložiště, který obsahuje zakázané znaky. Názvy účtů úložiště musí být mezi 3 a 24 znaků a čísel a malými písmeny.

## <a name="no-registered-provider-found"></a>Žádné registrovaných zprostředkovatel nalezen.

Pokud nasazení zdroje, může se zobrazit následující kód chyby a zprávy:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation} 
    and API version {api-version} for type {resource-type}.

Zobrazí tato chyba z tři důvodů:

1. Umístění nejsou podporované pro typ zdroje
2. Rozhraní API verze není podporovaná pro typ zdroje
3. Zprostředkovatele prostředků nebyl registrován u předplatného

Chybová zpráva by vám měl dát návrhy podporované umístění a rozhraní API verze. Změna šablony na některou z navrhovaných hodnot. Většina poskytovatelů jsou registrované automaticky pomocí portálu Azure nebo rozhraní příkazového řádku, který používáte, ale ne vždy. Pokud nepoužijete zprostředkovatele konkrétního zdroje před, budete muset zaregistrovat tohoto poskytovatele. Můžete zjistit informace o poskytovatelích zdrojů pomocí prostředí PowerShell nebo Azure rozhraní příkazového řádku.

### <a name="powershell"></a>Prostředí PowerShell

Zobrazit stav registrace, použijte **Get-AzureRmResourceProvider**.

    Get-AzureRmResourceProvider -ListAvailable

Zaregistrujte zprostředkovatele, pomocí **Rejstříku AzureRmResourceProvider** a zadejte název poskytovatele zdroje, které se chcete zaregistrovat.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Podporované umístění pro určitý typ zdroje, použijte:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Podporované verze rozhraní API pro určitý typ zdroje, použijte:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

Zjistit, zda je registrovaná zprostředkovatele, můžete `azure provider list` příkaz.

    azure provider list

Zaregistrovat zprostředkovatele prostředků, použijte `azure provider register` příkaz a určit *obor názvů* k registraci.

    azure provider register Microsoft.Cdn

Zprostředkovatele prostředků najdete v článku podporované umístění a rozhraní API verze, můžete:

    azure provider show -n Microsoft.Compute --json > compute.json

## <a name="operation-not-allowed"></a>Operace není povolena

Máte problémy při nasazení překročí kvótu, které by mohly být za pole Skupina zdroje, předplatné, účtů a jiné obory. Například předplatného může být nakonfigurované pro omezení počtu jádra pro oblast. Pokud budete chtít nasazení virtuálního počítače s více jádra než povolený částka, se chybová zpráva, že překročení kvóty.
Dokončení kvóty informace najdete v tématu [Azure předplatné a omezení služby, kvót a omezení](azure-subscription-service-limits.md).

Podívat se vaše předplatné kvóty pro jádra, můžete použít `azure vm list-usage` příkazu v Azure rozhraní příkazového řádku. Následující příklad ukazuje, že základní kvóta pro bezplatnou zkušební účet je 4:

    azure vm list-usage

Vracející:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Pokud nasadíte šablonu, která vytvoří více než čtyři jádra v oblastí západní USA, se zobrazí chyba nasazení, který bude vypadat podobně jako:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core. 
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

Nebo v prostředí PowerShell, můžete použít rutinu **Get-AzureRmVMUsage** .

    Get-AzureRmVMUsage

Vracející:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

V těchto případech by měl přejděte na portál a zařaďte případ pro podporu na vysokoškolskou úroveň kvóty pro oblasti, do kterého chcete nasadit.

> [AZURE.NOTE] Zapamatovat, že skupin zdroje, které kvóty pro každou jednotlivé oblast, ne pro celé předplatného. Pokud potřebujete nasazení 30 jádra západní USA, budete muset po nich žádáte 30 jádra správce prostředků západní USA. Pokud potřebujete nasazení 30 jádra v některém z oblasti, do kterého budete mít přístup, je třeba požádat o 30 jádra správce prostředků ve všech oblastech.

## <a name="invalid-content-link"></a>Odkaz na neplatné obsah

Pokud se zobrazí chybová zpráva:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

Největší pravděpodobností pokusíte odkaz na vnořené šablonu, která není k dispozici. Kontrola poklepejte URI přidělený vnořené šablony. Pokud tato šablona existuje v účtu úložiště, ujistěte se, že identifikátor URI přístupných osobám s postižením. Budete muset předat token přidružení zabezpečení. Další informace najdete v tématu [použití šablony propojené s správce prostředků Azure](resource-group-linked-templates.md).

## <a name="authorization-failed"></a>Ověření se nezdařilo

Při nasazení může zobrazit chybová, protože účet nebo službu hlavní pokusem o nasazení zdrojů nemá přístup provádět tyto akce. Azure Active Directory umožňují nebo správce určit, které identit přístup k čemu zdroje s skvělé stupeň přesnosti. Například pokud váš účet je přiřazené k roli Čtenář, nejste moct vytvářet zdroje. V takovém případě se zobrazí chybová zpráva, že ověření se nezdařilo.

Další informace o řízení přístupu na základě rolí v tématu [Řízení přístupu Azure Role-Based](./active-directory/role-based-access-control-configure.md).

Kromě řízení přístupu na základě rolí může být omezený akcí nasazení zásad u předplatného. Prostřednictvím zásad správce vynutit konvence u všech zdrojů používaný v předplatného. Například správce může vyžadovat, aby hodnotu určitou značkou slouží k typu zdroje. Pokud není odpovídají požadavkům zásad, chybová během nasazení. Další informace o zásadách najdete v tématu [Použití zásady pro přidávání a používání zdrojů a řízení přístupu](resource-manager-policy.md).

## <a name="troubleshooting-virtual-machines"></a>Poradce při potížích s virtuálních počítačích

| Chyba | Články |
| -------- | ----------- |
| Vlastní skript rozšíření chyby | [Selhání rozšíření Windows OM](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />nebo<br />[Selhání rozšíření Linux OM](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) |
| Obrázek s operačním systémem chyby zřizování | [Nové Windows OM chyby](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />nebo<br />[Nové Linux OM chyby](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md ) |
| Pole přidělení chyby | [Selhání přidělení Windows OM](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />nebo<br />[Selhání přidělení Linux OM](./virtual-machines/virtual-machines-linux-allocation-failure.md) |
| Zabezpečené chyby prostředí (SSH) při pokusu o připojení | [Zabezpečené připojení prostředí Linux v angličtině](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) |
| Chyby při připojení k aplikaci spuštěna OM | [Aplikace spuštěné v systému Windows OM](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />nebo<br />[Aplikace spuštěné v Linux OM](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) |
| Připojení ke vzdálené ploše chyby | [Připojení ke vzdálené ploše Windows v angličtině](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) |
| Připojení chyby vyřešit tak, že znovu nasazení | [Přeinstalujte virtuálního počítače nový Azure uzel](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) |
| Chyby služby cloudu | [Cloudové služby nasazení problémy](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

## <a name="troubleshooting-other-services"></a>Poradce při potížích s dalšími službami

Následující tabulka není kompletní seznam problémů témat týkajících se Azure. Místo toho zaměřen na otázky týkající se nasazení a konfiguraci zdroje. Pokud potřebujete pomoc běhu potíží se zdroji, najdete v dokumentaci pro službu Azure.

| Služba | Článek |
| -------- | -------- |
| Automatizace | [Tipy pro jejich odstranění běžných chyb v Azure automatizaci](./automation/automation-troubleshooting-automation-errors.md) |
| Azure zásobníku | [Poradce při potížích s Microsoft Azure zásobníku](./azure-stack/azure-stack-troubleshooting.md) |
| Azure zásobníku | [Web Apps a Azure zásobníku](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md ) |
| Data Factory | [Poradce při potížích Data Factory](./data-factory/data-factory-troubleshoot.md) |
| Služba struktury | [Řešení běžných problémů s při nasazení služby Azure služby struktury](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Obnovení webu | [Sledování a odstraňování případných problémů ochranu virtuálních počítačích a fyzických serverů](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Úložiště | [Sledování a Diagnostika a odstraňování potíží úložišti tabulek Microsoft Azure](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [Poradce při potížích nasazení StorSimple zařízení](./storsimple/storsimple-troubleshoot-deployment.md) |
| Databáze SQL | [Řešení potíží s připojením k databázi SQL Azure](./sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| Datový sklad SQL | [Poradce při potížích datový sklad Azure SQL](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="understand-when-a-deployment-is-ready"></a>Vysvětlení, jestli už se dá nasazení

Když všech poskytovatelů z nasazení Azure správce prostředků sestavy úspěch na nasazení. Ale tato zpráva neznamená, že skupina zdroje je "aktivní a jste připraveni na vaši uživatelé." Například nasazení muset stáhnout upgradů, čekat na zdroje bez šablony nebo když si nainstalujete složité skriptů. Správce prostředků nebudou vědět, kdy tyto úkoly dokončit, protože se nejedná o činnosti, které sleduje zprostředkovatele. V těchto případech může být některé dobu, po jejímž zdroje jsou připravené k použití reálný. V důsledku toho můžete očekávat, že stav nasazení se mu určitou dobu před použitím nasazení.

Azure můžete zabránit vytváření sestav nasazení úspěch, ale tak, že vytvoříte vlastní skript pro vlastní šablonu. Skript věděli, jak sledovat celé nasazení pro přípravu systémové a vrátí úspěšně pouze v případě, že uživatelé mohou pracovat se celé nasazení. Pokud chcete zajistit, aby je linky poslední spustit, používat vlastnost **dependsOn** v šabloně.

## <a name="next-steps"></a>Další kroky

- Další informace o auditování akce najdete v tématu [operace auditování pomocí Správce prostředků](resource-group-audit.md).
- Další informace o akce, které chcete určit chyby během nasazení najdete v tématu [operace nasazení zobrazení](resource-manager-troubleshoot-deployments-portal.md).
