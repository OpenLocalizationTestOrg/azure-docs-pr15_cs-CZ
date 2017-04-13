<properties 
    pageTitle="Uzamčení zdrojů pomocí Správce prostředků | Microsoft Azure" 
    description="Zabránit uživatelům v aktualizace nebo odstranění některých prostředků použitím omezení pro všechny uživatele a role." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tomfitz"/>

# <a name="lock-resources-with-azure-resource-manager"></a>Uzamčení zdrojů pomocí Správce prostředků Azure

Jako správce budete muset uzamknutí předplatného, skupina zdroje nebo zdroje zabránit ostatním uživatelům ve vaší organizaci mohli omylem odstranit nebo úprava důležitých prostředků. Můžete nastavit úroveň zamknout **CanNotDelete** nebo **jen pro čtení**. 

- **CanNotDelete** znamená, že uživatelé pořád číst a měnit zdroje, ale nemůže odstranit. 
- **Jen pro čtení** znamená, že uživatelé mohou číst ze zdroje, ale nelze odstranit nebo provádět žádné akce. Oprávnění ke zdroji je omezený k roli **Čtenář** . 

Použití **jen pro čtení** může vést k neočekávané výsledky, protože některé operacích, které vypadá jako přečíst v tématu operace skutečně vyžadují další akce. Například umístění Zámek **jen pro čtení** na úložiště účet všem uživatelům zabrání zobrazení klávesy. Seznam, který zpracování klíče operace prostřednictvím žádost o příspěvek protože vrácené zkratky jsou dostupné pro operace zápisu. Jiný příklad umístění Zámek **jen pro čtení** na aplikaci služby zdroje zabrání Visual Studio Server Explorer zobrazení souborů pro tento zdroj, protože interakce vyžaduje oprávnění k zápisu.

Na rozdíl od řízení přístupu na základě rolí můžete pomocí správy zámky omezení ve všech uživatelů a rolí. Další informace o nastavení oprávnění pro uživatele a role najdete v tématu [Řízení přístupu na základě rolí Azure](./active-directory/role-based-access-control-configure.md).

Kdy použít zámek v nadřazený oboru všechny podřízené zdroje dědit stejné zámek. I prostředky, které přidáte později dědit Zámek z nadřazeného webu. Co nejvíce omezující zámek v dědičnost přednost.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Kdo může vytvářet nebo odstraňovat zámků webů ve vaší organizaci

Vytvoření nebo odstranění zámky správy, musíte mít přístup k **Microsoft.Authorization/\* ** nebo **Microsoft.Authorization/locks/\* ** akce. Předdefinované rolí jenom **vlastník** a **Správce přístup uživatelů** udělena tato akce.

## <a name="creating-a-lock-through-the-portal"></a>Vytváření zámek prostřednictvím portálu

[AZURE.INCLUDE [resource-manager-lock-resources](../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Vytváření zámku do šablony

Následující příklad ukazuje šablonu, která vytvoří zámek účtu úložiště. Účet úložiště, na které chcete použít zámek slouží jako parametr. Název zámek vznikne zřetězením název zdroje s **/Microsoft.Authorization/** a název zámek v tomto případě **myLock**.

Typ poskytnutý si specifické pro typ zdroje. Úložiště nastavíte typ na "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Vytváření zámek pomocí rozhraní REST API

Je možné uzamknout nasazeném zdroje s [Rozhraní REST API pro správu zámek](https://msdn.microsoft.com/library/azure/mt204563.aspx). Rozhraní REST API umožňuje vytvořit a odstranění zámků webů a získat informace o existující zámků webů.

Pokud chcete vytvořit zámek, spusťte:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Obor může být předplatného, skupina zdroje nebo zdroje. Zámek – název je něco jiného, co budete chtít zavolat zámek. Verze rozhraní api když použijete **2015 01 01**.

V pozvánce na schůzku patří JSON objekt, který určuje vlastností zámek.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Příklady najdete v článku [Rozhraní REST API pro správu zámek](https://msdn.microsoft.com/library/azure/mt204563.aspx).

## <a name="creating-a-lock-with-azure-powershell"></a>Vytvoření zámek pomocí prostředí PowerShell Azure

Nasazeném zdroje přes Azure PowerShell můžete uzamknout pomocí **Nový AzureRmResourceLock** jak je vidět v následujícím příkladu.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure Powershellu poskytuje další příkazy pro práci zámků webů, například **Nastavení AzureRmResourceLock** aktualizovat zámek a **Odebrat AzureRmResourceLock** odstranit zámek.

## <a name="next-steps"></a>Další kroky

- Další informace o práci s uzamčení prostředků tématech [Lock dolů Your Azure](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- Další informace o logicky uspořádání zdrojů, najdete v článku [použití značek k uspořádání prostředků.](resource-group-using-tags.md)
- Změna zdroje skupiny jako zdroje je umístěn v popsaná v tématu [přesunutí zdrojů do nové skupiny prostředků](resource-group-move-resources.md)
- Omezení a konvence můžete použít napříč předplatného s vlastní zásady. Další informace najdete v tématu [Použití zásady pro přidávání a používání zdrojů a řízení přístupu](resource-manager-policy.md).
