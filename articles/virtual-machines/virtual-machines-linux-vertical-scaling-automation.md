<properties
    pageTitle="Svisle měřítko Azure virtuálního počítače s Azure automatizaci | Microsoft Azure"
    description="Jak svisle měřítko Linux virtuálního počítače v odpovědi na sledování oznámení s automatizaci Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/29/2016"
    ms.author="singhkay"/>

# <a name="vertically-scale-azure-virtual-machine-with-azure-automation"></a>Svisle měřítko Azure virtuálního počítače s automatizaci Azure

Změna měřítka svislé dochází ke zvýšení nebo snížení zdroje počítači v odpovědi na pracovní zátěž. V Azure můžete to provést změnou velikosti virtuálního počítače. Díky v následujících situacích

- Pokud virtuální počítač není použitý často, můžete změnit velikost ho dolů zmenšené snížit náklady na měsíční
- Pokud je virtuální počítač zatížení ve špičce, můžete velikost větší velikost zvětšíte její kapacity

Osnovy pokyny k tomu je jako pod

1. Automatizace Azure nastavení pro přístup k virtuálních počítačích
2. Import runbooks Azure automatizaci svislé měřítko do vašeho předplatného
3. Přidat webhook do svého postupu runbook
4. Přidání upozornění do virtuálního počítače

> [AZURE.NOTE] Z důvodu velikost první virtuální počítač, formáty, které můžete použít měřítko, může být omezená kvůli dostupnosti ostatní formáty v clusteru aktuální virtuální počítač nasazenou v. V runbooks publikované automatizaci použitých v tomto článku starat o tomto případě jsme jenom zobrazit v pod OM velikost dvojice. To znamená, že Standard_D1v2 virtuální počítač není neočekávaně přizpůsobena Standard_G5 nebo měřítko Basic_A0.

>| Změna měřítka pár velikosti OM |   |
|---|---|
|  Basic_A0 |  Basic_A4 |
|  Standard_A0 | Standard_A4 |
|  Standard_A5 | Standard_A7  |
|  Standard_A8 | Standard_A9  |
|  Standard_A10 |  Standard_A11 |
|  Standard_D1 |  Standard_D4 |
|  Standard_D11 | Standard_D14  |
|  Standard_DS1 |  Standard_DS4 |
|  Standard_DS11 | Standard_DS14  |
|  Standard_D1v2 |  Standard_D5v2 |
|  Standard_D11v2 |  Standard_D14v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Automatizace Azure nastavení pro přístup k virtuálních počítačích

První věc, kterou musíte udělat je vytvořit účet Azure automatizaci, který bude hostitelem runbooks slouží k velikosti nastavte měřítko OM instancí. Naposledy Automation service zavádí funkci "Spustit jako účet", která umožňuje nastavení nahoru jistinu služby pro automatické spuštění runbooks jménem uživatele velmi snadno. Další informace v níže uvedených článků:

* [Ověření Runbooks s Azure spustit jako účet](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Import runbooks Azure automatizaci svislé měřítko do vašeho předplatného

Runbooks, které jsou potřebné pro svisle měřítka virtuálního počítače jsou publikované v galerii Azure automatizaci postupu Runbook. Musíte je importovat do vašeho předplatného. Můžete se naučíte, jak můžete importovat runbooks o čtení následující článek.

* [Galerie postupu Runbook a modulu pro automatizaci Azure](../automation/automation-runbook-gallery.md)

Na na následujícím obrázku jsou vidět runbooks, které potřebujete k importu

![Import runbooks](./media/virtual-machines-vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Přidat webhook do svého postupu runbook

Po importu runbooks, které budete muset přidat webhook postupu runbook tak, aby mohli spouštěný kliknutím upozornění od virtuálního počítače. Podrobnosti o vytváření webhook pro vaše postupu Runbook naleznete zde

* [Azure webhooks automatizaci](../automation/automation-webhooks.md)

Zkontrolujte, že zkopírujete webhook před zavřením okna webhook jako budete potřebovat v další části.

## <a name="add-an-alert-to-your-virtual-machine"></a>Přidání upozornění do virtuálního počítače

1. Vyberte nastavení virtuálního počítače
2. Vyberte "Upozornění pravidla"
3. Vyberte možnost "Přidat upozornění"
4. Vyberte metriky aktivováno upozornění na
5. Vyberte podmínku, která splněny bude způsobit upozornění aktivováno
6. Vyberte prahovou hodnotu pro tuto podmínku v kroku 5. musí být splněny
7. Vyberte za období, po kterou službu sledování kontroloval podmínku a prahové hodnoty v kroky 5 a 6
8. Vložte webhook, kterou jste zkopírovali předchozím oddílem.

![Přidání oznámení do virtuálního počítače 1](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-1.png)

![Přidání oznámení do virtuálního počítače 2](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-2.png)