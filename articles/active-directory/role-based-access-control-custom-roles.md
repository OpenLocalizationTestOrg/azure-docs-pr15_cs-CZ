<properties
    pageTitle="Vlastní role v Azure RBAC | Microsoft Azure"
    description="Zjistěte, jak definovat vlastní role pomocí řízení přístupu Azure Role-Based správy identit přesnější předplatné Azure."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="kgremban"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/25/2016"
    ms.author="kgremban"/>


# <a name="custom-roles-in-azure-rbac"></a>Vlastní role v Azure RBAC


Vytvořte vlastní roli v Azure Role-Based přístup ovládacího prvku (RBAC) Pokud žádný z předdefinovaných role vlastním potřebám zvláštní přístup. Pomocí [Prostředí PowerShell Azure](role-based-access-control-manage-access-powershell.md), [rozhraní Azure příkazového řádku](role-based-access-control-manage-access-azure-cli.md) (rozhraní příkazového řádku) a [Rozhraní REST API](role-based-access-control-manage-access-rest.md)lze vytvořit vlastní role. Stejně jako předdefinovaný role vlastní role můžete přidělovat uživatelé, skupiny a aplikací v předplatného, skupina zdroje a obory zdroje. Vlastní role jsou uloženy v klientovi Azure AD a mohou být sdíleny Všechna předplatná, které používají tomuto klientovi jako adresář Azure AD pro subsciption.

Následující obrázek je příkladem vlastní role pro monitorování a restartování virtuálních počítačích:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Akce
Vlastnost **Akce** vlastní role určuje Azure operace, ke kterým uděluje roli přístup. Je sbírka operace řetězce, které identifikují zabezpečitelné operace poskytovatelů Azure zdroje. Operace řetězce, které obsahují zástupné znaky (\*) udělit přístup k všechny operace, které odpovídají řetězec operace. Například:

-   `*/read`poskytuje přístup k další operace pro všechny typy zdrojů všech poskytovatelů Azure zdroje.
-   `Microsoft.Network/*/read`poskytuje přístup k další operace pro všechny typy zdrojů v zprostředkovatele prostředků Microsoft.Network Azure.
-   `Microsoft.Compute/virtualMachines/*`poskytuje přístup k všechny operace virtuálních počítačích a podřízených typů zdrojů.
-   `Microsoft.Web/sites/restart/Action`poskytuje přístup k restartujte weby.

Použití `Get-AzureRmProviderOperation` (v prostředí PowerShell) nebo `azure provider operations show` (v Azure rozhraní příkazového řádku) seznam způsobů poskytovatelů Azure zdroje. Taky můžete tyto příkazy ověřte platnost operace řetězce a rozbalte zástupných operace řetězce.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Kopie obrazovky Powershellu: Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | Operace FT, název operace](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure rozhraní příkazového řádku snímek - azure poskytovatele operace zobrazit "Microsoft.Compute/virtualMachines/\*/action" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Použijte vlastnost **NotActions** , je-li nastavit operacích, které chcete povolit snadněji definována vyloučením s omezeným přístupem operace. Udělení přístupu podle vlastní role vyhodnocované tak, že odečteš operace **NotActions** operacích **Akce** .

> [AZURE.NOTE] Pokud uživateli přiřadí roli, která nezahrnuje operaci **NotActions**a přiřazenou roli druhé, která zajišťuje přístup k stejnou operaci, uživatel oprávnění k provedení operace. **NotActions** není odepřít pravidlo – je jednoduše pohodlný způsob, jak vytvořit sadu povolené operace při určitých operace není potřeba vyloučit.

## <a name="assignablescopes"></a>AssignableScopes
Vlastnost **AssignableScopes** vlastní role určuje oborů (předplatná skupiny zdrojů a zdrojů), ve kterých je dostupný pro přiřazení vlastní roli. Vlastní roli zpřístupnit pro přiřazení v předplatných nebo skupiny zdrojů, které vyžadují ho a ne složky nepotřebné uživatelského prostředí pro zbytek předplatných nebo skupiny zdrojů.

Příklady platné přiřadit obory:

-   "/ předplatná/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ předplatná/e91d47c4-76f3-4271-a796-21b4ecfe3624" - zpřístupní role pro přiřazení ve dvou předplatná.
-   "/ předplatná/c276fc76-9cd4-44c9-99a7-4fd71546436e" - zpřístupní role pro přiřazení v jedné předplatné.
-  "/ předplatná/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/sítě" - zpřístupní role pro přiřazení pouze ve skupině prostředků sítě.

> [AZURE.NOTE] Je nutné použít alespoň jedno předplatné, skupina zdroje nebo číslo ID zdroje.

## <a name="custom-roles-access-control"></a>Řízení přístupu vlastní role
Vlastnost **AssignableScopes** vlastní role také určuje, kdo může zobrazit, upravit a odstranit roli.

- Kdo může vytvářet vlastní roli?
    Vlastníci (a správci přístup uživatelů) předplatných, skupiny zdrojů a zdroje můžete vytvořit vlastní role pro použití v těchto obory.
    Vytváření roli uživatele je potřeba provést `Microsoft.Authorization/roleDefinition/write` operace na všechny **AssignableScopes** role.

- Kdo může upravit vlastní roli?
    Vlastníci (a správci přístup uživatelů) předplatných, skupiny zdrojů a zdroje můžete upravit vlastní role v těchto obory. Uživatelé potřebují k provedení `Microsoft.Authorization/roleDefinition/write` operace na všechny **AssignableScopes** vlastní roli.

- Kdo může zobrazit vlastní role?
    Všechny předdefinované role v Azure RBAC povolit zobrazení role, které jsou k dispozici pro přiřazení. Uživatelé, kteří mohou provádět `Microsoft.Authorization/roleDefinition/read` operace v oboru uvidí RBAC role, které jsou k dispozici pro přiřazení v oboru.

## <a name="see-also"></a>Viz taky
- [Řízení přístupu na základě rolí](role-based-access-control-configure.md): Začínáme s RBAC Azure portálu.
- Naučte se spravovat přístupu pomocí:
    - [Prostředí PowerShell](role-based-access-control-manage-access-powershell.md)
    - [Azure rozhraní příkazového řádku](role-based-access-control-manage-access-azure-cli.md)
    - [ROZHRANÍ REST API](role-based-access-control-manage-access-rest.md)
- [Předdefinované role](role-based-access-built-in-roles.md): zobrazení podrobností o rolích dodaných standardní v RBAC.
