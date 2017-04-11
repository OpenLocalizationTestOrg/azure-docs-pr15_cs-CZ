<properties
    pageTitle="Vytvoření sestavy aplikace access změnit historie | Microsoft Azure"
    description="Generování sestavy, který obsahuje všechny změny v Accessu k předplatné Azure pomocí řízení přístupu na základě rolí za posledních 90 dnů."
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
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="create-an-access-change-history-report"></a>Vytvoření sestavy historie změn aplikace access

Kdykoli někdo uděluje nebo odebere přístup k v rámci předplatného, změny získat přihlášení Azure události. Můžete vytvořit sestavy aplikace access změnit Historie zobrazíte všechny změny během posledních 90 dnů.

## <a name="create-a-report-with-azure-powershell"></a>Vytvoření sestavy pomocí prostředí PowerShell Azure
Vytvoření sestavy aplikace access změnit historie v prostředí PowerShell, můžete `Get-AzureRMAuthorizationChangeLog` příkaz. Další podrobnosti o tuto rutinu jsou dostupné v [Galerii Powershellu](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1).

Při volání tento příkaz je možné zadat vlastností, které chcete být uvedení, včetně následujících přiřazení:

| Vlastnost | Popis |
| -------- | ----------- |
| **Akce** | Zda byl udělit ani odvolat přístup |
| **Volající** | Vlastník zodpovědný za změna přístupu |
| **Datum** | Datum a čas, naposledy změnil přístup |
| **DirectoryName** | V adresáři služby Azure Active Directory |
| **PrincipalName** | Jméno uživatele, skupinu nebo aplikace |
| **PrincipalType** | Zda bylo přiřazení pro uživatele, skupinu nebo aplikace |
| **RoleId** | GUID roli, které byl udělen nebo odvolaný |
| **RoleName** | Role, které byl udělen nebo odvolaný |
| **ScopeName** | Název předplatného, skupina zdroje nebo zdrojů |
| **ScopeType** | Zda bylo přiřazení na předplatné, skupina zdroje nebo obor zdroje |
| **SubscriptionId** | GUID Azure předplatného |
| **SubscriptionName** | Název Azure předplatného |

Tento příklad uvádí všechny změny, aplikace access v předplatného během posledních sedmi dnů:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![Powershellu Get-AzureRMAuthorizationChangeLog - snímek](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Vytvoření sestavy pomocí rozhraní příkazového řádku Azure
Vytvoření sestavy aplikace access změnit historie v Azure rozhraní příkazového řádku (rozhraní příkazového řádku), můžete `azure role assignment changelog list` příkaz.

## <a name="export-to-a-spreadsheet"></a>Exportovat do tabulky
Sestavu uložit, a pracovat s údaji, exportujte změny přístup do souboru CSV. Pak můžete zobrazit sestavu v tabulce ke kontrole.

![Changelog zobrazit jako tabulka - snímek](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>Viz taky
- Začínáme s [Azure Role-Based řízení přístupu](role-based-access-control-configure.md)
- Práce s [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md)
