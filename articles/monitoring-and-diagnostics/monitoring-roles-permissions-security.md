<properties
    pageTitle="Začínáme s rolí, oprávnění a cenného papíru s Azure Monitor | Microsoft Azure"
    description="Naučte se používat předdefinované rolích a oprávněních Azure Monitor na omezení přístupu k sledování zdroje."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Začínáme s rolí, oprávnění a cenného papíru s Azure monitoru

Počet týmů muset sporná, i když řízení přístupu k monitorování dat a nastavení. Pokud máte členy týmu, kteří pracují výhradně na sledování (pracovníci technické podpory, devops inženýrů) nebo pokud používáte zprostředkovatele služba spravovaných, je vhodné udělit jim přístup jenom monitorování dat při omezení schopnost vytvoření, úprava nebo odstranění zdroje. Tento článek ukazuje, jak rychle použít předdefinované sledování RBAC role uživatele v Azure nebo vytvoření vlastní vlastní role pro uživatele, který potřebuje omezené sledování oprávnění. Potom popisuje otázky bezpečnosti pro zdroje týkající se sledování Azure a jak můžete omezit přístup k datům, které obsahují.

## <a name="built-in-monitoring-roles"></a>Integrované sledování role

Předdefinované role Azure Monitor jsou navržené tak, aby omezení přístupu k prostředkům ve předplatné ale stále kontrolní infrastruktury získat a konfigurovat data potřebují. Azure Monitor nabízí dvě mimo pole role: A sledování čtečka a Přispěvatel sledování.

### <a name="monitoring-reader"></a>Sledování Readeru

Lidé přiřazenou roli sledování Čtenář můžete zobrazit všechna data sledování v předplatné ale nemůžete změnit všechny zdroje nebo upravit nastavení týkající se sledování zdroje. Tato role je vhodné pro uživatele v organizaci, například podporu nebo operace inženýrů, kteří potřebují mít možnost:

- Zobrazení sledování řídicí panely na portálu a vytvořte svoje vlastní soukromé sledování řídicí panely.
- Dotaz pro metriky pomocí [Rozhraní REST API Azure monitoru](https://msdn.microsoft.com/library/azure/dn931930.aspx), [rutiny prostředí PowerShell](insights-powershell-samples.md)nebo [různé platformy rozhraní příkazového řádku](insights-cli-samples.md).
- Dotaz protokolu činnosti pomocí portálu, Azure Monitor REST API, rutiny prostředí PowerShell nebo různé platformy rozhraní příkazového řádku.
- Zobrazte [diagnostické nastavení](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) zdroje.
- Zobrazení [protokolu profilu](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) pro předplatné.
- Nastavení automatické měřítko zobrazení.
- Zobrazit upozornění aktivitu a nastavení.
- Přístup k datům aplikace přehledy a zobrazení dat v AI analýzy.
- Hledat data pracovního prostoru protokolu analýzy (OMS), včetně údaje o využití pracovního prostoru.
- Zobrazení protokolu analýzy (OMS) Správa skupin.
- Načtěte analýz protokolu (OMS) schéma vyhledávání.
- Seznam technologie pro analýzu protokolu (OMS) intelligence balíky XML
- Načítat a spouštět analýzu protokolu (OMS) uložená hledání.
- Načtení konfigurace úložiště protokolu analýzy (OMS).

> [AZURE.NOTE] Tato role neposkytuje protokolu data, která byla streamují k rozbočovači událost nebo uložené na účtu úložiště přístup pro čtení. Informace o konfiguraci přístup k těmto zdrojům [najdete níže](#security-considerations-for-monitoring-data) .

### <a name="monitoring-contributor"></a>Sledování přispěvatelů

Přiřazené role přispěvatele sledování uživatelé můžou prohlížet všechna sledování data v předplatné vytvořit nebo upravit nastavení sledování, ale nemůžete změnit další zdroje informací. Tato role je podmnožina roli sledování Čtenář a je vhodné pro členy sledování týmu nebo služba spravovaných poskytovatelů, kteří kromě oprávnění výše uvedená taky muset mít možnost organizace:

- Sledování řídicí panely publikujte jako sdílenou řídicího panelu.
- Nastavení [diagnostiky nastavení](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) resource.*
- Nastavení [protokolu profilu](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) subscription.*
- Nastavit upozornění aktivity a nastavení.
- Vytvoření testů web aplikaci přehledy a složek.
- Pracovní prostor protokolu analýzy (OMS) seznam sdílených klíče.
- Povolení nebo zakázání sady intelligence protokolu analýzy (OMS).
- Vytváření a odstraňování a spuštění analýzy protokolu (OMS) uložená hledání.
- Vytváření a odstraňování konfigurace úložiště protokolu analýzy (OMS).

* uživatele taky samostatně oprávnění ListKeys na cílové zdrojů (úložiště klienta nebo události centrální názvů) a nastavte protokol profilu nebo diagnostiky nastavení.

> [AZURE.NOTE] Tato role neposkytuje protokolu data, která byla streamují k rozbočovači událost nebo uložené na účtu úložiště přístup pro čtení. Informace o konfiguraci přístup k těmto zdrojům [najdete níže](#security-considerations-for-monitoring-data) .

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Sledování vlastní RBAC role a oprávnění
Pokud výše předdefinované role není potřebám přesné vašeho týmu, můžete [vytvořit vlastní roli RBAC](../active-directory/role-based-access-control-custom-roles.md) odstupňovaný oprávnění. Tady jsou běžné Azure Monitor RBAC operace s jejich popisy.

| Operace                                                   | Popis                                                                                                                                               |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Odstranit Microsoft.Insights/AlertRules/[Read zapisovat]         | Čtení a zápis nebo odstraňování oznámení pravidla.                                                                                                                            |
| Microsoft.Insights/AlertRules/Incidents/Read                | Seznam incidentem (historie pravidla výstrahy aktivují) pro upozornění pravidla. To platí jenom k portálu.                                              |
| Odstranit Microsoft.Insights/AutoscaleSettings/[Read zapisovat]  | Nastavení automatické měřítko pro čtení a zápis nebo odstranit.                                                                                                                     |
| Odstranit Microsoft.Insights/DiagnosticSettings/[Read zapisovat] | Čtení a zápis nebo odstraňování diagnostiky nastavení.                                                                                                                    |
| Microsoft.Insights/eventtypes/digestevents/Read             | Toto oprávnění stačí pro uživatele, kteří potřebují mít přístup k protokolům aktivity prostřednictvím portálu.                                                                   |
| Microsoft.Insights/eventtypes/values/Read                   | Seznam protokolu aktivity události (události správy) v předplatné. Toto oprávnění je k dispozici programové a portálu přístup k protokolu činnosti. |
| Microsoft.Insights/LogDefinitions/Read                      | Toto oprávnění stačí pro uživatele, kteří potřebují mít přístup k protokolům aktivity prostřednictvím portálu.                                                                   |
| Microsoft.Insights/MetricDefinitions/Read                   | Přečtěte si metrických definice (seznam dostupných typů metrických zdroje).                                                                                  |
| Microsoft.Insights/Metrics/Read                             | Přečtěte si metriky zdroje.                                                                                                                              |

> [AZURE.NOTE] Přístup k upozornění, diagnostické nastavení a metriky pro zdroj vyžaduje, jestli má uživatel přístup pro čtení pole Typ zdroje a oboru daný zdroj. Vytvoření ("psát") diagnostiky nastavení nebo protokolu profil, který archivy k účtu úložiště nebo datové proudy do události rozbočovače vyžaduje uživatelům oprávnění ListKeys na cílový prostředek.

Příklad použití výše uvedenou tabulku můžete vytvořit vlastní roli RBAC pro "Aktivity protokolu čtečku" takto:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Otázky bezpečnosti pro monitorování dat
Sledování dat – zejména souborů protokolu – může obsahovat citlivých informací, například IP adresy nebo jména uživatelů. Sledování dat z Azure existují ve tři základní formy:
1. Protokol aktivity popisuje všechny akce řídicí předplatného Azure.
2. Diagnostických protokolů, které jsou protokolů, které zdroje.
3. Metriky jsou emitovány podle zdrojů.

Všechny tři typy dat můžete uložené na účtu úložiště nebo streamují k rozbočovači události, které je obecný Azure zdrojů. Protože jsou univerzální zdrojů, vytvoření, odstranění a přístupu k nim je privilegovaných operace obvykle vyhrazená pro správce. Měli byste použít následující postupy týkající se sledování zdrojů předejít zneužití:

- Použití účtu jediné, vyhrazené úložiště pro sledování data. Pokud potřebujete k oddělení sledování data do několika účtů úložiště, nikdy sdílet použití účtu úložiště mezi sledování a -monitorování dat, jako to může omylem dát uživatelům, kteří pouze potřebují mít přístup k monitorování dat (např. třetích stran SIEM) přístup k sledování jiných data.
- Použijte jediné, vyhrazené služby Bus nebo centrální události obor názvů přes všechna nastavení diagnostické ze stejného důvodu jako nahoře.
- Omezit přístup k týkající se sledování úložiště účty nebo události rozbočovače tím, že jim do samostatných zdroje skupiny a [použít obor](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) na sledování role omezit přístup k pouze tato skupina zdroje.
- Nikdy oprávnění ListKeys pro účty úložiště nebo rozbočovače událostí v oboru předplatného uživatele jenom potřebujete přístup k monitorování dat. Místo toho přidělit tato oprávnění uživatele na zdroj nebo skupina zdroje (Pokud máte vyhrazená sledování skupina zdroje) obor.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Omezení přístupu k účtům týkající se sledování úložiště
Pokud uživatel nebo aplikace potřebuje přístup k monitorování dat v účtu úložiště, byste měli úložiště účet, který obsahuje monitorování dat pomocí služby určené jen pro čtení přístupu k úložišti objektů blob [Generovat přidružení zabezpečení účtu](https://msdn.microsoft.com/library/azure/mt584140.aspx) . V prostředí PowerShell mohla vypadat takto:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Potom dodáte tokenu osobě, musí číst z úložiště účet a můžete seznam a číst ze všech objektů BLOB v tomto účtu úložiště.

Můžete taky Pokud potřebujete řídit tato oprávnění s RBAC, můžete udělit entity Microsoft.Storage/storageAccounts/listkeys/action oprávnění k tomuto účtu určité úložiště. Toto je nutné, aby uživatelé, kteří mají mít možnost nastavit diagnostiky nebo přihlášení profilu archivace k účtu úložiště. Můžete například vytvořit následující vlastní RBAC roli uživatele nebo aplikace, která potřebuje pouze pro čtení z jednoho účtu úložiště:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] Oprávnění ListKeys umožňuje uživateli zobrazit klíče účtu primárních a sekundárních úložiště. Klávesy uživateli udělit všechny podepsané oprávnění (číst, zapisovat, vytvořit objekty BLOB, odstranění objektů BLOB, atd.) ve všech podepsané služby (objektů blob fronty, tabulky, soubor) v tomto účtu úložiště. Doporučujeme používat k účtu přidružení zabezpečení jsme je popsali výše Pokud je to možné.

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Omezení přístupu k rozbočovače sledování událost související se zabezpečením
Podobné vzorek může být zahájen s rozbočovače událost, ale je potřeba nejdřív vytvořit vyhrazené poslech ověřovací pravidlo. Pokud chcete udělit přístup k aplikaci, která potřebuje pouze poslouchat rozbočovače sledování událost související se zabezpečením, postupujte takto:

1. Vytvoření zásad sdílený přístup na rozbočovače události, které byly vytvořené pro přenos monitorování dat pomocí pouze poslech deklarované. Lze to na portálu. Například můžete ho může volat "monitoringReadOnly." Pokud je to možné budete chtít dejte tomuto klíči přímo do příjemce a další krok vynechat.
2. Pokud příjemce musí být dostali klíčové ad-hoc, dejte uživateli ListKeys akce pro daný rozbočovač události. To je nutné pro uživatele, kteří mají mít možnost nastavit diagnostiky nebo zaznamenat profilu do datového proudu rozbočovače události. Můžete například vytvořit pravidlo RBAC:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## <a name="next-steps"></a>Další kroky
- [Přečtěte si o RBAC a oprávnění správce prostředků](../active-directory/role-based-access-control-what-is.md)
- [Přečtěte si základní informace o sledování v Azure](monitoring-overview.md)
