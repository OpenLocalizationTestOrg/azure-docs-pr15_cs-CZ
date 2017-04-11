<properties
    pageTitle="Použití šablon správce prostředků Azure ve vrstvě Azure (klienta vývojáři) | Microsoft Azure"
    description="Informace o použití šablon správce prostředků Azure ve vrstvě Azure nasadit a zřízení všechny zdroje pro aplikaci v operaci jediné, koordinovaný."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Použití šablon správce prostředků Azure ve vrstvě Azure

Azure šablony správce prostředků nasazení a zřízení všechny zdroje pro aplikaci v operaci jediné, koordinovaný. Zdroje pro aplikaci a jak ho má být nasazené definovat.  Můžete taky přeinstalujte šablony měnit k prostředkům ve skupině prostředků.

Tyto šablony můžete nasadit s portálu Microsoft Azure zásobníku, Powershellu, přepínač příkazového řádku a Visual Studia.

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

Následující šablony jsou k dispozici na [GitHub](http://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-non-high-availability"></a>Nasazení Sharepointu (bez vysoké dostupnosti)

Pomocí prostředí PowerShell DSC rozšíření vytvořit farma SharePoint 2013, který obsahuje následující:

-   Virtuální sítě

-   Tři účty úložiště

-   Dva Vyrovnávání zatížení externí

-   Jeden OM nakonfigurování jako nový struktuře s jedné domény řadiče domény

-   Jeden OM nakonfigurování jako samostatný server SQL Server 2014

-   Jeden OM nakonfigurování jako farma SharePoint 2013 jednom počítači

## <a name="deploy-ad-non-high-availability"></a>Nasazení AD (bez vysoké dostupnosti)

Pomocí prostředí PowerShell DSC rozšíření vytvořit AD domény řadiče domény serveru, který obsahuje následující:

-   Virtuální sítě

-   Úložiště účtů

-   Jeden Vyrovnávání zatížení externí

-   Jeden OM nakonfigurování jako nový struktuře s jedné domény řadiče domény

## <a name="deploy-adsql-non-high-availability"></a>Nasazení AD/SQL (bez vysoké dostupnosti)

Pomocí prostředí PowerShell DSC rozšíření vytvořit SQL Server 2014 samostatný server, který obsahuje následující:

-   Virtuální sítě

-   Dva účty úložiště

-   Jeden Vyrovnávání zatížení externí

-   Jeden OM nakonfigurování jako nový struktuře s jedné domény řadiče domény

-   Jeden OM nakonfigurování jako samostatný server SQL Server 2014

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Pomocí prostředí PowerShell DSC rozšíření ke konfiguraci existující virtuální počítač místní konfigurace správce (LCM) a zaregistrovat k Azure automatizaci účtu DSC vyžádat serveru.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Vytvoření virtuálního počítače z obrázku uživatele

Vytvoření virtuálního počítače z vlastních uživatelských obrázku. Tato šablona taky nasadí virtuální síti (s DNS), veřejnou IP adresu a rozhraní sítě.

## <a name="simple-vm"></a>Jednoduchý OM

Nasazení jednoduché OM Windows, která obsahuje virtuální síti (s DNS), veřejnou IP adresu a rozhraní sítě.

## <a name="cancel-a-running-template-deployment"></a>Zrušení pracovního nasazení šablony

Zrušení pracovního nasazení šablony, můžete `Stop-AzureRmResourceGroupDeployment` rutiny prostředí PowerShell.


## <a name="next-steps"></a>Další kroky

[Nasazení šablony pomocí portálu](azure-stack-deploy-template-portal.md)

[Azure Přehled Správce zdrojů](../azure-resource-manager/resource-group-overview.md)

