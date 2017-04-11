<properties
    pageTitle="Nasazení šablony s příkazového řádku v Azure zásobníku | Microsoft Azure"
    description="Naučte se používat rozhraní různé platformy příkazového řádku (rozhraní příkazového řádku) pro nasazení šablony z uvnitř ClientVM nebo po připojení k vrstvě Azure pomocí sítě VPN."
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
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Nasazení šablon ve vrstvě Azure pomocí příkazového řádku

Nasazení šablony správce prostředků Azure Koncepce zásobníku Azure pomocí příkazového řádku. Azure šablony správce prostředků nasazení a zřízení všechny zdroje pro aplikaci v operaci jediné, koordinovaný.

## <a name="download-template"></a>Stažení šablony        
Testování nasazení s rozhraní příkazového řádku, stahování souborů azuredeploy.json a azuredeploy.parameters.json [vytvořit šablonu příklad účtu úložiště](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Nasazení šablony
Přejděte do složky, kde byly tyto soubory stáhnout a spusťte tento příkaz nasazení šablony:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Tento příkaz nasadí šablony na skupina zdroje **cliRG** Koncepce zásobníku Azure výchozího umístění.

## <a name="validate-template-deployment"></a>Ověření nasazení šablony
Pokud chcete zobrazit tento účet skupiny a úložiště zdroje, použijte následující příkazy:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Další kroky

[Správa uživatelských oprávnění](azure-stack-manage-permissions.md)
