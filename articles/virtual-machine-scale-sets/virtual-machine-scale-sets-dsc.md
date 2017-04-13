<properties
   pageTitle="Použití žádoucí stavu konfigurace sadami měřítko virtuálního počítače | Microsoft Azure"
   description="Použití virtuálního počítače měřítko sad s Azure DSC rozšíření"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Použití virtuálního počítače měřítko sad s Azure DSC rozšíření

[Virtuální počítač měřítko sady (VMSS)](virtual-machine-scale-sets-overview.md) lze použít rutinu rozšíření [Azure žádoucí stavu konfigurace (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) . VMSS umožňuje nasazením a správou velkého množství virtuálních počítačích a můžete nebo zmenšit velikost elastically v odpovědi na načíst. DSC se používá ke konfiguraci VMs se jich přechodu do online tak, aby se běží výrobní software.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Rozdíly mezi nasazení OM a VMSS

Základní struktura šablony pro VMSS se mírně liší od jedné OM. Konkrétně jednoho OM nasadí rozšíření uzlu "virtualMachines". Existuje položka typu "rozšíření", kde DSC přidané do šablony

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

VMSS uzel má oddíl "vlastnosti" "VirtualMachineProfile", "extensionProfile" atribut. DSC přibude v části "rozšíření"

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>Chování VMSS

Chování VMSS je shodný s chování jednoho OM. Po vytvoření nového OM automaticky zřízení s příponou DSC. Pokud na novější verzi WMF se vyžaduje příponu, OM restartuje před chodit online. Když je online, stáhne ZIP konfigurace DSC a připravit na OM. Další informace najdete v [Přehledu Azure DSC rozšíření](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md).

## <a name="next-steps"></a>Další kroky ##
Prozkoumejte [Správce prostředků Azure šablony pro rozšíření DSC](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md).

Zjistěte, jak [DSC rozšíření bezpečně zpracovává přihlašovací údaje](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

Další informace o rutiny rozšíření Azure DSC najdete v článku [Úvod do rozšíření rutiny konfigurace stavu požadované Azure](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md). 

Další informace o prostředí PowerShell DSC, [Navštěvujte blog o Centru si přečtěte následující dokumentaci Powershellu](https://msdn.microsoft.com/powershell/dsc/overview). 


