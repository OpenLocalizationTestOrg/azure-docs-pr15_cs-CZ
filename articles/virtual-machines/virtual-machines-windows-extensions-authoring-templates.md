<properties
   pageTitle="Vytváření stránek šablony v systému Windows OM rozšíření | Microsoft Azure"
   description="Další informace o vytváření šablon správce prostředků Azure s příponami VMs systému Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Vytváření šablon správce prostředků Azure Windows OM rozšíření

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Z Azure PowerShell spusťte následující rutinu Powershellu Azure:

      Get-AzureVMAvailableExtension


Tuto rutinu vrátí Publisheru jméno, název rozšíření a verze následujícím způsobem:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Tyto tři vlastnosti namapovat "publisher", "typ" a "typeHandlerVersion" ve výše uvedený šabloně fragment kódu.

>[AZURE.NOTE]Vždy doporučujeme používat nejnovější verze rozšíření získat aktuálním funkce.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Identifikace schématu parametry konfigurace rozšíření

Dalším krokem s vytváření šablonu rozšíření je k identifikaci formátu umožňující parametry konfigurace. Každý rozšíření podporuje vlastní sadu parametrů.

Podívejte se na ukázkové konfigurace pro doplňky systému Windows, najdete v článku [Ukázky rozšíření Windows](virtual-machines-windows-extensions-configuration-samples.md).


Získáte následující získat plně dokončení šablonu s OM rozšíření.

[Rozšíření vlastní skript na Windows OM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)


Po vytváření šabloně, nástroje můžete nasazovat pomocí prostředí PowerShell Azure.
