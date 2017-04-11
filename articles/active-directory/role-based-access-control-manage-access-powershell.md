<properties
    pageTitle="Správa řízení přístupu na základě rolí (RBAC) s Azure Powershellu | Microsoft Azure"
    description="Jak spravovat RBAC pomocí prostředí PowerShell Azure, včetně seznamu role, přiřazení rolí a odstraňování přiřazování rolí."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-azure-powershell"></a>Správa řízení přístupu na základě rolí pomocí prostředí PowerShell Azure

> [AZURE.SELECTOR]
- [Prostředí PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure rozhraní příkazového řádku](role-based-access-control-manage-access-azure-cli.md)
- [ROZHRANÍ REST API](role-based-access-control-manage-access-rest.md)


Řízení přístupu na základě rolí (RBAC) na portálu Azure a rozhraní API správy zdrojů Azure umožňuje spravovat přístup k svému předplatnému jemně odstupňovaná úrovni. Pomocí této funkce můžete udělit přístup uživatelů, skupin a objektů služby Active Directory některé role jim přiřadíte na určitý obor.

Před použití Powershellu ke správě RBAC, musíte mít takto:

- Azure Powershellu verze 0.8.8 nebo novější. Nainstalujte nejnovější verzi a přidružit Azure předplatné, najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

- Azure rutiny pro správce prostředků. Nainstalujte [Správce prostředků Azure rutin](https://msdn.microsoft.com/library/mt125356.aspx) v prostředí PowerShell.

## <a name="list-roles"></a>Přehled rolí

### <a name="list-all-available-roles"></a>Seznam všech dostupných rolí
K rolím RBAC seznamu, které jsou dostupné pro přiřazení a zkontrolovat operace, ke kterým se udělit přístup, můžete `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![Snímek obrazovky Powershellu: Get-AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Akce seznamu role
Seznam akcí pro konkrétní rolí, použijte `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![Snímek obrazovky Powershellu: Get-AzureRmRoleDefinition pro konkrétní rolí - RBAC](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Zjistit, kdo má přístup
Seznam přiřazení RBAC přístup, použijte `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Přiřazení rolí seznam na určitý obor
Můžete zobrazit všechny přiřazení přístup pro zadané předplatného, skupina zdroje nebo zdroje. Například zobrazit všechny aktivní přiřazení Skupina zdroje, použít `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![Snímek obrazovky prostředí PowerShell-Get AzureRmRoleAssignment pro skupinu prostředků - RBAC](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Seznam role přiřazené uživateli
Seznam všech role, které jsou přiřazené k zadaný uživatel a role, které jsou přiřazené k skupiny, do které patří uživatele, můžete `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![Snímek obrazovky Powershellu: Get-AzureRmRoleAssignment pro uživatele – RBAC](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Seznam přiřazení rolí správce a coadmin klasické služby
Seznam přiřazení přístup pro klasické předplatné Administrators a coadministrators, použití:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Udělení přístupu
### <a name="search-for-object-ids"></a>Vyhledání ID objektů
Přiřazení role, budete muset popsána objektu (uživatele, skupinu nebo aplikace) a obor.

Pokud si nejste jisti ID předplatného, můžete najít v zásuvné **předplatného** na portálu Azure. Zjistěte, jak k vytvoření dotazu pro ID předplatného, najdete v článku [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) na webu MSDN.

Identifikátor objektu skupiny Azure AD, použijte:

    Get-AzureRmADGroup -SearchString <group name in quotes>

ID objektu jistinu služby Azure AD nebo aplikace, použijte:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Přiřazení role do aplikace v oboru předplatného
Udělit přístup k aplikaci v oboru předplatného, můžete:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![Snímek obrazovky prostředí PowerShell – nové AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Přiřazení role uživatele v rozsahu skupiny zdrojů
Udělení přístupu pro uživatele v rozsahu skupiny zdrojů, můžete:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![Snímek obrazovky prostředí PowerShell – nové AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Přiřazení role do skupiny v rozsahu zdrojů
Udělení přístupu ke skupině v rozsahu zdrojů, můžete:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![Snímek obrazovky prostředí PowerShell – nové AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Odebrání aplikace access
Odebrání přístupu pro uživatele, skupiny a aplikací, můžete:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![Snímek obrazovky prostředí PowerShell odebrat AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Vytvořit vlastní roli
Chcete-li vytvořit vlastní roli, použijte `New-AzureRmRoleDefinition` příkaz.

Když vytvoříte vlastní roli pomocí prostředí PowerShell, budete muset začněte s některou z [předdefinovaných role](role-based-access-built-in-roles.md). Upravit vlastnosti přidejte *Akce*, *notActions*nebo *obory* požadovaný a změny uložit jako novou roli.

Následující příklad začíná role *Přispěvatele virtuálního počítače* a, která se používají k vytvoření vlastní roli s názvem *Operátor virtuálního počítače*. Novou roli uděluje přístup pro všechny další operace *Microsoft.Compute* *Microsoft.Storage*a *Microsoft.Network* poskytovatelů zdroje a přístup k zahájení, restartujte a sledovat virtuálních počítačích. Vlastní roli lze použít v dvě předplatná.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![Snímek obrazovky Powershellu: Get-AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>Upravit vlastní roli
Upravit vlastní roli, nejdřív, můžete `Get-AzureRmRoleDefinition` příkazu načtěte definici role. Za druhé proveďte požadované změny definici role. Nakonec, použít `Set-AzureRmRoleDefinition` příkaz Uložit definici změněné role.

Následující příklad přidává `Microsoft.Insights/diagnosticSettings/*` operace vlastní roli *Operátor virtuálního počítače* .

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![Snímek obrazovky prostředí PowerShell-Set AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Následující příklad přidává předplatné Azure přiřadit obory vlastní roli *Operátor virtuálního počítače* .

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![Snímek obrazovky prostředí PowerShell-Set AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>Odstranění vlastní role

Chcete-li odstranit vlastní roli, použijte `Remove-AzureRmRoleDefinition` příkaz.

Následující příklad odebere vlastní roli *Operátor virtuálního počítače* .

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![Snímek obrazovky prostředí PowerShell odebrat AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Seznam vlastní role
Role, které jsou k dispozici pro přiřazení v obor, použijte `Get-AzureRmRoleDefinition` příkaz.

Následující příklad uvádí všechny role, které jsou k dispozici pro přiřazení v vybraného předplatného.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![Snímek obrazovky Powershellu: Get-AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

V následujícím příkladu vlastní roli *Operátor virtuální počítač* není k dispozici v předplatné *Production4* vzhledem k tomu, že předplatné není uvedená **AssignableScopes** role.

![Snímek obrazovky Powershellu: Get-AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Viz taky
- [Správce Azure Powershellu s Azure zdroje](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
