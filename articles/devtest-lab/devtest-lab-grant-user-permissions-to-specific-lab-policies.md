<properties
    pageTitle="Udělovat uživatelům oprávnění pro konkrétní laboratorní zásady | Microsoft Azure"
    description="Zjistěte, jak udělovat uživatelům oprávnění k zásadám konkrétní laboratorní v DevTest Labs podle potřeby každého uživatele"
    services="devtest-lab,virtual-machines,visual-studio-online"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Udělovat uživatelům oprávnění pro konkrétní laboratorní zásady

## <a name="overview"></a>Základní informace

Tento článek ukazuje, jak pomocí Powershellu udělit uživatelům oprávnění k zásadu konkrétní laboratorní. Tímto způsobem oprávnění se dají použít podle potřeby každého uživatele. Můžete například udělit konkrétní možnost změnit nastavení zásad OM, ale nikoli zásady náklady.

## <a name="policies-as-resources"></a>Zásady jako zdroje

Jak je uvedeno v následujícím článku [Řízení přístupu na základě rolí Azure](../active-directory/role-based-access-control-configure.md) RBAC umožňuje správu jemně odstupňovaná přístupu prostředků pro Azure. RBAC můžete oddělit úkolů v rámci týmu DevOps a udělit jenom množství přístup uživatelům, kteří potřebují k provedení své práce.

V DevTest Labs zásadu je typ zdroje, který umožňuje akce RBAC **Microsoft.DevTestLab/labs/policySets/policies/**. Každého zásadu laboratorní je zdroj v seznamu Typ zdroje zásady a můžete přidělovat jako obor k roli RBAC.

Například k poskytnutí uživatelů pro čtení i zápis oprávnění **Povolených velikosti OM** zásad vytvoříte vlastní roli, se kterými spolupracuje **Microsoft.DevTestLab/labs/policySets/policies/** * akci a pak příslušným uživatelům přiřadit tato vlastní role v rozsahu * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Další informace o vlastních rolí v RBAC, najdete v tématu [řízení přístupu role vlastní](../active-directory/role-based-access-control-custom-roles.md).

##<a name="creating-a-lab-custom-role-using-powershell"></a>Vytvoření vlastní role laboratorní pomocí prostředí PowerShell
Abyste mohli začít pracovat, budete muset v následujícím článku se dozvíte, jak nainstalovat a nakonfigurovat rutiny prostředí PowerShell Azure: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Jakmile jste nastavili rutiny prostředí PowerShell Azure, můžete provádět následující úkoly:

- Seznam všechny operace/akce pro zprostředkovatele zdroje
- Seznam akcí v určité role:
- Vytvořit vlastní roli

Tento skript Powershellu ukazuje příklady, jak provádět následující úkoly:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Přiřazování oprávnění pro konkrétní zásady použití vlastní role uživatele
Jakmile jste definovali své vlastní role, můžete je přiřadit uživatelům. Chcete uživateli přiřadit roli vlastní, musíte nejprve získat **objektu** představující tohoto uživatele. Aby je dostala, získáte pomocí rutiny **Get-AzureRmADUser** .

V následujícím příkladu je **objektu** uživatele, *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Až budete mít **objektu** pro uživatele a název vlastní roli, můžete přiřadil určitou roli uživatele s rutinu **New-AzureRmRoleAssignment** :

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

V předchozím příkladu se používá zásad **AllowedVmSizesInLab** . Můžete použít následující zásady:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky

Jednou jste poskytuje uživatelských oprávnění pro konkrétní laboratorní zásad, tady jsou některé další kroky k zamyšlení:

- [Zabezpečený přístup k laboratoři](devtest-lab-add-devtest-user.md).

- [Nastavení zásad laboratorní](devtest-lab-set-lab-policy.md).

- [Vytvoření šablony laboratorní](devtest-lab-create-template.md).

- [Vytvořit vlastní artefakty pro vaše VMs](devtest-lab-artifact-author.md).

- [Přidat OM s artefakty laboratoři](devtest-lab-add-vm-with-artifacts.md).
