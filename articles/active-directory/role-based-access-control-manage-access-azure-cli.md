<properties
    pageTitle="Správa řízení přístupu na základě rolí (RBAC) s Azure rozhraní příkazového řádku | Microsoft Azure"
    description="Naučte se spravovat na základě rolí řízení přístupu k (RBAC) Azure rozhraní příkazového řádku obsahuje přehled rolí a rolí akce tak přiřazení role do obory předplatné a aplikace."
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

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Správa řízení přístupu na základě rolí Azure rozhraní příkazového řádku

> [AZURE.SELECTOR]
- [Prostředí PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure rozhraní příkazového řádku](role-based-access-control-manage-access-azure-cli.md)
- [ROZHRANÍ REST API](role-based-access-control-manage-access-rest.md)

Řízení přístupu na základě rolí (RBAC) na portálu Azure a rozhraní API Správce prostředků Azure umožňuje spravovat přístup k svoje předplatné a zdroje jemně odstupňovaná úrovni. Pomocí této funkce můžete udělit přístup uživatelů, skupin a objektů služby Active Directory některé role jim přiřadíte na určitý obor.

Před rozhraní Azure příkazového řádku (rozhraní příkazového řádku) slouží ke správě RBAC, musíte mít takto:

- Azure rozhraní příkazového řádku verze 0.8.8 nebo novější. Nainstalujte nejnovější verzi a přidružit předplatného Azure naleznete v tématu [instalace a konfigurace Azure rozhraní příkazového řádku](../xplat-cli-install.md).
- Azure správce prostředků v Azure rozhraní příkazového řádku. Přejděte na [pomocí rozhraní příkazového řádku Azure pomocí Správce prostředků](../xplat-cli-azure-resource-manager.md) pro další podrobnosti.

## <a name="list-roles"></a>Přehled rolí

### <a name="list-all-available-roles"></a>Seznam všech dostupných rolí
Seznam všech dostupných rolí, můžete:

        azure role list

Následující příklad zobrazuje seznam *všech dostupných rolí*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Snímek obrazovky příkazového řádku - azure role seznam – RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Akce seznamu role
Seznam akcí roli, můžete:

    azure role show "<role name>"

Následující příklad ukazuje akce role *přispěvatele* a *Přispěvatel virtuálního počítače* .

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Snímek obrazovky příkazového řádku - azure role zobrazit - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>Přístup k seznamu
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Přiřazení rolí seznamu efektivní ve skupině prostředků.
Seznam přiřazování rolí, která existují ve skupině zdroje, můžete:

    azure role assignment list --resource-group <resource group name>

Následující příklad ukazuje přiřazování rolí ve skupině *pharma prodej projecforcast* .

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure příkazového řádku - azure role přiřazení v seznamu skupiny snímek](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Přiřazení rolí seznamu pro uživatele
Seznam přiřazování rolí pro určitého uživatele a přiřazení přiřazených skupinám uživatele, můžete:

    azure role assignment list --signInName <user email>

Najdete v článku přiřazení rolí, které se dědí od skupiny změnou příkazu:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Následující příklad ukazuje udělená přiřazování *sameert@aaddemo.com* uživatele. Platí to i role, které jsou přímo se uživateli přiřadí a role, které se dědí od skupiny.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Snímek obrazovky příkazového řádku - azure role přiřazení seznam uživatelem – RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>Udělení přístupu
Teď, když jste určili, kterou chcete přiřadit roli udělit přístup, můžete:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Přiřazení role do skupiny v oboru předplatného
Přiřazení role do skupiny v oboru předplatného, můžete:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Následující příklad přiřadí roli *Čtenář* *Jana Koch týmu* , obor *předplatného* .


![RBAC Azure příkazového řádku - přiřazování rolí azure vytvoříte skupiny snímek](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Přiřazení role do aplikace v oboru předplatného
Přiřazení role do aplikace v oboru předplatného, můžete:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Následující příklad povolí role *přispěvatele* do aplikace *Azure AD* vybraného předplatného.

 ![RBAC Azure příkazového řádku - přiřazování rolí azure vytvoříte aplikace](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Přiřazení role uživatele v rozsahu skupiny zdrojů
Přiřazení role uživatele v rozsahu skupiny zdrojů, můžete:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Následující příklad povolí role *Přispěvatele virtuálního počítače* k *samert@aaddemo.com* uživatelům obor *Pharma prodej ProjectForcast* zdrojů skupina.

![RBAC Azure příkazového řádku - přiřazování rolí azure vytvoříte uživatele snímek](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Přiřazení role do skupiny v rozsahu zdrojů
Přiřazení role do skupiny v rozsahu zdrojů, můžete:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Následující příklad povolí role *Přispěvatele virtuálního počítače* do skupiny *Azure AD* v *podsítě*.

![RBAC Azure příkazového řádku - přiřazování rolí azure vytvoříte skupiny snímek](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>Odebrání aplikace access
Odebrání přiřazování rolí, můžete:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Následující příklad odebere přiřazení role *Přispěvatele virtuálního počítače* z *sammert@aaddemo.com* uživatelům ve skupině *Pharma prodej ProjectForcast* prostředků.
V příkladu odebere přiřazování rolí ze skupiny u předplatného.

![Snímek obrazovky příkazového řádku - azure role přiřazení odstranit - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Vytvořit vlastní roli
Vytvořit vlastní roli, můžete:

    azure role create --inputfile <file path>

Následující příklad vytvoří vlastní roli s názvem *Operátor virtuálního počítače*. Vlastní roli uděluje přístup pro všechny další operace *Microsoft.Compute* *Microsoft.Storage*a *Microsoft.Network* poskytovatelů zdroje a přístup k zahájení, restartujte a sledovat virtuálních počítačích. Vlastní roli lze použít v dvě předplatná. Tento příklad používá JSON soubor jako vstup.

![Snímek obrazovky JSON - definice vlastní role:](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![Vytvoření azure role RBAC Azure příkazového řádku - - snímek](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Upravit vlastní roli

Chcete-li upravit vlastní roli, nejdřív pomocí `azure role show` příkazu načtěte definice role. Druhé proveďte požadované změny souboru definice role. Nakonec pomocí `azure role set` uložte definici změněné role.

    azure role set --inputfile <file path>

Následující příklad přidává operaci *Microsoft.Insights/diagnosticSettings/* **Akce**a předplatné Azure k **AssignableScopes** vlastní roli operátor virtuálního počítače.

![JSON - Změna definice vlastní roli - snímek](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Snímek obrazovky příkazového řádku - azure role set - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Odstranění vlastní role

Pokud chcete odstranit vlastní roli, nejdřív pomocí `azure role show` příkaz k určení **ID** role. Potom použijte `azure role delete` příkaz pro odstranění roli zadáním **ID**.

Následující příklad odebere vlastní roli *Operátor virtuálního počítače* .

![Snímek obrazovky příkazového řádku - azure role odstranit - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Seznam vlastní role

Role, které jsou k dispozici pro přiřazení v obor, použijte `azure role list` příkaz.

Následující příklad uvádí všechny role, které jsou k dispozici pro přiřazení v vybraného předplatného.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Snímek obrazovky příkazového řádku - azure role seznam – RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

V následujícím příkladu vlastní roli *Operátor virtuální počítač* není k dispozici v předplatné *Production4* vzhledem k tomu, že předplatné není uvedená **AssignableScopes** role.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Snímek obrazovky příkazového řádku - seznamu azure role pro vlastní role - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>RBAC témata
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
