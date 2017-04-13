
<properties
    pageTitle="Přidávání a používání zdrojů pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
    description="Umožňuje spravovat Azure zdrojů a skupin Azure rozhraní příkazového řádku (rozhraní příkazového řádku)"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="danlep"/>

# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Správa Azure materiály a zdroje skupiny pomocí rozhraní příkazového řádku Azure


> [AZURE.SELECTOR]
- [Portál](azure-portal/resource-group-portal.md) 
- [Azure rozhraní příkazového řádku](xplat-cli-azure-resource-manager.md)
- [Azure Powershellu](powershell-azure-resource-manager.md)
- [ROZHRANÍ REST API](resource-manager-rest-api.md)


Rozhraní příkazového řádku Azure (Azure rozhraní příkazového řádku) je jeden z několika nástroje můžete nasazovat a spravovat zdroje s správce prostředků. Tento článek uvádí obvyklé způsoby správy Azure materiály a zdroje skupiny pomocí rozhraní příkazového řádku Azure v režimu správce prostředků. Informace o používání rozhraní příkazového řádku pro nasazení zdroje tématech [nasazení se šablonami správce prostředků a Azure rozhraní příkazového řádku](resource-group-template-deploy-cli.md). Představu o tom Azure zdrojů a správce prostředků Navštěvujte blog o [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md).

>[AZURE.NOTE] Ke správě Azure zdroje s Azure rozhraní příkazového řádku, budete muset [nainstalovat Azure rozhraní příkazového řádku](xplat-cli-install.md)a [Přihlaste se k Azure](xplat-cli-connect.md) pomocí `azure login` příkaz. Musí být rozhraní příkazového řádku v režimu správce prostředků (Spustit `azure config mode arm`). Pokud už máte hotové tyto věci, jste připravená k odeslání.



## <a name="get-resource-groups-and-resources"></a>Získání skupiny zdrojů a zdrojů

### <a name="resource-groups"></a>Skupiny zdrojů

Zobrazte seznam všech skupin zdrojů v předplatného a jejich umístění, spusťte tento příkaz.

    azure group list
    

### <a name="resources"></a>Zdroje informací
 Seznam všech zdrojů v ke skupině, třeba s názvem *testRG*, použijte následující příkaz.

    azure resource list testRG

Zobrazíte konkrétního zdroje v rámci skupiny, například virtuálního počítače s názvem *MyUbuntuVM*, použijte příkaz například následující.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
Všimněte si parametr **Microsoft.Compute/virtualMachines** . Tento parametr určuje typ zdroje, které požadujete informace na.
    
>[AZURE.NOTE]Pokud chcete použít příkazy **azure zdroje** než příkaz **list** , je třeba určit rozhraní API verzi zdroje s parametrem **– o** . Pokud si nejste jistí o verzi rozhraní API, použijte soubor šablony a vyhledejte pole apiVersion pro daný zdroj. Další informace o rozhraní API verze správce prostředků najdete v článku [Správce prostředků poskytovatelů, oblastí, verze rozhraní API a schémat](resource-manager-supported-services.md).

Při zobrazení podrobností o zdroji, je často užitečný pro práci `--json` parametr. Tento parametr dělá výstupní čitelnější, protože některé hodnoty jsou vnořené struktury nebo kolekcí. Následující příklad ukazuje nevrací žádné výsledky příkazu **Zobrazit** jako dokument JSON.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Pokud budete potřebovat, vám ušetří JSON dat do souboru &gt; znaku nasměrovat výstup do souboru. Příklad:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### <a name="tags"></a>Značky

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Přidávání a používání zdrojů


Přidání zdroje třeba úložiště účet do skupiny zdrojů, spusťte příkaz podobně jako:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"
    
Kromě určení verze rozhraní API zdroje s parametrem **– o** , použijte parametr **-p** předat řetězec formátu JSON s vyžadované nebo další vlastnosti.
    
    
Pokud chcete odstranit existující zdroj například prostředek virtuální počítač, použijte příkaz například následující.

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Existujících zdrojů přesunout na jiné pole Skupina zdroje nebo předplatného, použijte příkaz **Přesunout azure zdroje** . Následující příklad ukazuje, jak přesunout mezipaměti Redis do nové skupiny prostředků. V parametru **-i** poskytují že hodnoty oddělené seznam číslo id zdroje je přesunout.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Řízení přístupu k prostředkům

Rozhraní příkazového řádku Azure slouží k vytváření a Správa zásad pro řízení přístupu k Azure zdroje. Představu o tom, definice zásad a přiřazení zásady ke zdrojům najdete v článku [použití zásad pro přidávání a používání zdrojů a řízení přístupu](resource-manager-policy.md).

Například definovat následující zásady zamítnout všechny žádosti o kde umístění není západní USA nebo severní centrální US a si ji uložit do policy.json soubor definice zásad:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Spusťte příkaz **create definice zásad** :

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
Tento příkaz zobrazí výstup podobně jako tento.

    + Vytváření zásad definice MyPolicy dat: název_zásad: MyPolicy dat: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    data: Třída PolicyType: vlastní dat: DisplayName: Nedefinováno dat: Popis: Nedefinováno dat: PolicyRule: pole = umístění, ve [westus, northcentralus], efekt = Odepřít

 Pokud chcete přiřadit zásady v oboru budete potřebovat, použijte **PolicyDefinitionId** vrátil příkaz předchozí. V následujícím příkladu je tento obor předplatné, ale můžete omezit zdroje skupinám nebo jednotlivým zdrojům:

    přiřazení zásad Azure vytvořit MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy – ne /subscriptions/###-###-###-###-### /

Můžete získat, změna nebo odebrání zásad definice pomocí příkazů **Zobrazit definici zásad**, **nastavení zásad definice**a **zásady definice odstranit** .

Podobně můžete získat, změna nebo odebrání přiřazení zásad pomocí příkazů **Zobrazit přiřazení zásad**, **přiřazení zásady nastaveno**a **přiřazení zásad odstranit** .


## <a name="export-a-resource-group-as-a-template"></a>Export skupina zdroje jako šablony

Pro existující skupiny zdrojů můžete zobrazit šabloně správce prostředků pro skupiny zdrojů. Export šablony nabízí dva výhody:

1. Můžete snadno automatizovat budoucí nasazení řešení vzhledem k tomu, že všechny infrastruktury je definována v šabloně.

2. Můžete seznámit se s syntaxe šablony pohledem JSON, který představuje řešení.

Pomocí rozhraní příkazového řádku Azure, můžete buď exportovat šablonu, která představuje aktuální stav skupiny prostředků nebo stáhnout šablonu, která byla použita pro konkrétní nasazení.

* **Export šablonu vhodnou pro skupinu zdrojů** – to je užitečné, když změny provedené v skupina zdroje a načtení JSON znázornění jeho aktuálním stavu. Však vygenerovaných šablona obsahuje pouze minimální počet parametrů a žádné proměnné. Většina hodnot v šabloně je pevně. Než nasadíte vygenerovaných šablony, budete pravděpodobně chtít převést více hodnot parametry tak, aby si můžete přizpůsobit nasazení pro různá prostředí.

    Export šablony pro skupinu prostředků do místního adresáře, spustit `azure group export` příkaz jak je vidět v následujícím příkladu. (Nahraďte místního adresáře vhodná pro váš operační systém prostředí.)

        azure group export testRG ~/azure/templates/

* **Stažení šablony pro konkrétní nasazení** – to je užitečné, když potřebujete zobrazit skutečné šablonu, která byla použita pro nasazení zdroje. Šablona obsahuje všechny parametry a proměnné definované původní nasazení. Ale pokud někdo ve vaší organizaci změny provedené v skupina zdroje mimo definici v šabloně, tato šablona není představovat aktuální stav skupina zdroje.

    Pokud chcete stáhnout šablonu použít pro konkrétní nasazení místního adresáře, spusťte `azure group deployment template download` příkaz. Příklad:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Export šablony se v náhledu a ne všechny typy zdrojů momentálně podporují export šablony. Při pokusu o exportu šablon, je chyba se může zobrazit, která tvrdí, že nebyly exportovány některé zdroje informací. V případě potřeby ručně definujte tyto materiály v šabloně po si ji stáhnete.



## <a name="next-steps"></a>Další kroky

* Zobrazení podrobností operací nasazení a Poradce při potížích s rozhraní příkazového řádku Azure nasazení, najdete v části [Zobrazit nasazení operace s Azure rozhraní příkazového řádku](resource-manager-troubleshoot-deployments-cli.md).
* Pokud chcete použít rozhraní příkazového řádku pro nastavení aplikace nebo skript, a získat informace, najdete v článku [Použití Azure rozhraní příkazového řádku pro vytvoření služby základní a získat informace](resource-group-authenticate-service-principal-cli.md).


