<properties
    pageTitle="Změna velikosti a opakovaně dělá sám virtuálního počítače měřítko sady | Microsoft Azure"
    description="Zjistěte, jak používat diagnostických nástrojů a automatické měřítko zdroje automaticky zobrazit virtuálních počítačích v sadě měřítko."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Změna velikosti a opakovaně dělá sám virtuálního počítače měřítko sady

Automatické změny velikosti virtuálních počítačích v sadě měřítko je vytvoření nebo odstranění strojů v sadě v případě potřeby podle požadavků na výkon. Narůstající velikostí množství práce, aplikace může vyžadovat další zdroje informací umožňující efektivně úkoly.

Automatické změny velikosti je automatického procesu, že pomáhá usnadnění správy režijních. Zmenšením zatížení, nemusíte průběžně sledovat výkon systému nebo rozhodnout, jak přidávání a používání zdrojů. Změna velikosti je pružná obrázku. Více zdrojů můžete přidat jako zvýšením zatížení, ale jako služba je možné odebrat snižuje materiály minimalizovat náklady a Udržovat úrovně výkonu.

Nastavte automatické měřítko na měřítku nastavit pomocí šablony pro správce prostředků Azure, Azure Powershellu, rozhraní příkazového řádku Azure nebo portálu Azure.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Nastavení proměnné velikosti pomocí Správce prostředků šablon

Namísto zavádění a zvlášť, správu jednotlivé zdroje aplikace použít šablonu, která nasadí všechny zdroje v operaci jediné, koordinovaný. V šabloně aplikace zdroje jsou definované a nasazení parametrů jsou určeny pro různá prostředí. Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnoty pro nasazení. Další informace, podívejte se [šablon pro vytváření správce prostředků Azure](../resource-group-authoring-templates.md).

V šabloně zadáte element kapacita:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Kapacita označuje počet virtuálních počítačích v sadě. Kapacita ručně změníte nasazení šablony s jinou hodnotu. Pokud jsou nasazení šablony pouze Změna kapacity, můžete zahrnout pouze element SKU s aktualizovanými kapacity.

Automatická změna objemu použít měřítko můžete nastavit pomocí kombinace autoscaleSettings zdroje a rozšíření diagnostických nástrojů.

### <a name="configure-the-azure-diagnostics-extension"></a>Konfigurace koncovku diagnostiky Azure

Automatické změny velikosti provést pouze pokud metriky kolekce proběhne úspěšně do každého virtuálního počítače v sadě měřítko. Rozšíření diagnostiky Azure poskytuje možnosti sledování a diagnostických nástrojů, které vyhovuje kolekce metriky automatické měřítko zdroje. Nainstalujte rozšíření jako součást šabloně správce prostředků.

Tento příklad ukazuje proměnné, které se používají v šablonu, kterou chcete konfigurovat koncovku diagnostických nástrojů:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Při nasazení šablony jsou k dispozici parametry. V tomto příkladu název účtu úložiště, kam se ukládají data a název sady měřítko, ze kterého se údaje jsou k dispozici. Taky v tomto příkladu systému Windows Server se shromažďují pouze čítače počtu podprocesů. Všechny dostupné výkonnosti v systému Windows nebo Linux mohou sloužit k shromáždit diagnostické informace a mohou být součástí konfigurace rozšíření.

Tento příklad ukazuje definici rozšíření v šabloně:

    "extensionProfile": {
      "extensions": [
        {
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
              "storageAccount": "[variables('diagnosticsStorageAccountName')]"
            },
            "protectedSettings": {
              "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

Při spuštění koncovku diagnostických nástrojů se shromažďují data v tabulce, která se nachází v okně úložiště účet, který určíte. V tabulce WADPerformanceCounters můžete najít shromážděná data:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>Konfigurace autoScaleSettings zdroje

Zdroje autoscaleSettings použije informace z koncovku diagnostických nástrojů se rozhodnout, jestli chcete zvětšit nebo zmenšit počet virtuálních počítačích v sadě měřítko.

Tento příklad ukazuje konfiguraci zdroje v šabloně:

    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

Ve výše uvedeném příkladu dvě pravidla vytvořené pro definování automatické změny velikosti akce. První pravidlo definuje akci škálování a druhé pravidlo definuje akci měřítko se změnami. V pravidlech jsou k dispozici tyto hodnoty:

- **metricName** – tato hodnota je stejná jako čítače výkonu, který jste definovali proměnné wadperfcounter pro diagnostiku rozšíření. Ve výše uvedeném příkladu se používá podprocesu čítač.  
- **metricResourceUri** – tato hodnota je identifikátor zdroje s nastavením měřítko virtuálního počítače. Tento identifikátor obsahuje název skupiny zdrojů, název poskytovatele zdroje a název rozsahu nastavte měřítko.
- **timeGrain** – tato hodnota je metriky, které byly shromážděny. V tomto příkladu je údaje na interval jednu minutu. Tato hodnota se používá se hodnota timeWindow.
- **statistický** – tato hodnota určuje, jak kombinovat metriky tak, aby zahrnoval automatické změny velikosti akce. Možné hodnoty jsou: průměr, Min a Max.
- **hodnota timeWindow** – tato hodnota je oblast čas, ve kterém se shromažďují instance data. Musí být mezi 5 minut a 12 hodin.
- **Agregace času** – tato hodnota určuje, jak by měl kombinovat data, která se shromažďují určitou dobu. Výchozí hodnota je průměr. Možné hodnoty jsou: průměr, Minimum, Maximum, poslední, součet, počet.
- **operátor** – tato hodnota je operátor, který se používá k porovnání data metriky a prahové hodnoty. Možné hodnoty jsou: rovná se NotEquals GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
- **mezní hodnoty** – tato hodnota je hodnota, která spustí akci měřítko. Ujistěte se, že poskytují dostatek rozdíl mezi mezní škálování akce a prahové hodnoty pro akce měřítko v. Pokud jste nastavili hodnoty, které mají být stejné, připravená na systém konstanty změnit, které zabrání implementaci měřítka akce. Příklad jak 600 vláknech nastavení v předchozím příkladu nefunguje.
- **směr** – tato hodnota Určuje akci, která je provedena při dosažení mezní hodnota. Možné hodnoty jsou zvýšení nebo snížení.
- **Typ** – tato hodnota je typ akci, která by měla proběhnout a musí být nastavený na ChangeCount.
- **hodnoty** – tato hodnota je počet virtuálních počítačích, které jsou přidat nebo odebrat ze sady měřítko. Tato hodnota musí být 1 nebo větší.
- **cooldown** – tato hodnota je dobu čekat od poslední změny měřítka akce před další akce. Tato hodnota musí být mezi jednu minutu a jeden týden.

V závislosti na používaném čítače některé prvky v konfiguraci šablony fungují jinak. V předchozím příkladu čítač výkonu je počtu podprocesů, mezní hodnota je 650 akce škálování a mezní hodnota je 550 akce měřítko se změnami. Pokud používáte čítač například % času procesoru, mezní hodnota nastavenou na požadované procento využití procesoru určující měřítka akce.

Po vytvoření zatížením na virtuálních počítačích, které spustí měřítka akci zvýšen kapacita sady na základě hodnoty v šabloně. Například v měřítku sadu mají kapacita nastavenou 3 a měřítko akce hodnotu 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Načíst vytvoření, který způsobí aktivaci počet průměr podprocesů přejdete prahovou hodnotu 650:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Škálování akcí se spustí, který způsobí aktivaci schopnost množiny zvýší po úrovních:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

A virtuálního počítače se přidá do sady měřítko:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Po určité době cooldown pět minut jestli průměrný počet vláknech počítačích zůstane delší než 600, jiného počítače se má přidat do sady. Pokud počet průměr podprocesů zůstává pod 550, kapacita sadu měřítko sníží po úrovních a do počítače je odebráno ze sady.

## <a name="set-up-scaling-using-azure-powershell"></a>Nastavení, změně velikosti pomocí prostředí PowerShell Azure

Pokud chcete příklady pomocí prostředí PowerShell nastavit neobsahovaly text, podívejte se na [Azure Monitor PowerShell rychlý start vzorky](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Nastavení, změně velikosti pomocí rozhraní příkazového řádku Azure

Příklady použití rozhraní příkazového řádku Azure nastavit neobsahovaly text zobrazíte podívejte [Azure Monitor a platformy rozhraní příkazového řádku rychlý start vzorky](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Nastavte měřítko na Azure portálu

Pokud chcete vidět příklad portálu Azure nastavit neobsahovaly text, podívejte se na [vytvořit sadu měřítko virtuálního počítače na portálu Azure](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Prozkoumejte měřítka akce

- [Azure portál]() - se nyní zobrazí omezené množství informací o na portálu.
- [Azure zdroje Explorer]() - tento nástroj je nejlepší pro prohlížení aktuální stav sadě měřítko. Postupujte podle tato cesta a byste měli vidět zobrazení instance množiny měřítko, kterou jste vytvořili: předplatná > {předplatného} > resourceGroups > {skupiny zdrojů} > zprostředkovatelů > Microsoft.Compute > virtualMachineScaleSets > {sadě měřítko} > virtualMachines
- Azure PowerShell – pomocí tohoto příkazu určité informace:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput

- Připojení k počítači virtuální jumpbox úplně stejně, jako kdybyste druhý počítač a potom vzdáleně se dostanete virtuálních počítačích v měřítku nastavit sledování jednotlivé procesy.

## <a name="next-steps"></a>Další kroky

- Podívejte se na [automaticky měřítko počítačích v sadě měřítko virtuálního počítače](virtual-machine-scale-sets-windows-autoscale.md) zobrazíte příklad toho, jak vytvořit měřítko sada se automatické změny velikosti nakonfigurované.
- Vyhledání příklady Azure sledovat sledování funkcí v [Azure Monitor PowerShell rychlý start vzorky](../monitoring-and-diagnostics/insights-powershell-samples.md)
- Další informace o funkcích oznámení v [použít automatické měřítko akce, které chcete odeslat e-mailu a webhook oznámení v Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).
- Informace o [použití auditování protokoly poslat e-mailu a webhook oznámení v Azure monitoru](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Informace o [Rozšířené automatické měřítko scénáře](./virtual-machine-scale-sets-advanced-autoscale.md).
