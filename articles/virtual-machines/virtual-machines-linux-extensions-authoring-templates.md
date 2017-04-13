<properties
   pageTitle="Vytváření šablon s příponami Linux OM | Microsoft Azure"
   description="Další informace o vytváření šablon správce prostředků Azure s příponami Linux VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Vytváření šablon správce prostředků Azure s příponami Linux OM

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Z Azure rozhraní příkazového řádku spusťte následující příkaz:

      Azure VM extension list

Tento příkaz vrátí název, název rozšíření a verze jako následující:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Tyto tři vlastnosti namapovat "publisher", "typ" a "typeHandlerVersion" ve výše uvedený šabloně fragment kódu.

>[AZURE.NOTE]Vždy doporučujeme používat nejnovější verze rozšíření získat aktuálním funkce.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Identifikace schématu parametry konfigurace rozšíření

Dalším krokem s vytváření šablonu rozšíření je k identifikaci formátu umožňující parametry konfigurace. Každý rozšíření podporuje vlastní sadu parametrů.

Projít ukázkové konfigurace Linux rozšíření, klikněte na dokumentaci k najdete v článku [Příklady eExtensions Linux](virtual-machines-linux-extensions-configuration-samples.md).

Získáte následující získat plně dokončení šablonu s OM.

[Rozšíření vlastní skript na Linux OM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Po vytváření šabloně, nástroje můžete nasazovat pomocí rozhraní příkazového řádku Azure.
