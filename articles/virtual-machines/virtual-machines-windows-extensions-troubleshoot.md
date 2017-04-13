<properties
   pageTitle="Poradce při potížích s Windows OM rozšíření selhání | Microsoft Azure"
   description="Další informace o řešení potíží s selhání rozšíření OM Windows Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Poradce při potížích s selhání rozšíření OM Windows Azure

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Zobrazení stavu rozšíření
Azure šablony správce zdrojů lze spustit z Azure Powershellu. Po spuštění šabloně uvidí stav rozšíření z Průzkumníka zdroje Azure nebo nástroje příkazového řádku.

Tady je příklad:

Azure Powershellu:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Tady je ukázka výstupu:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Poradce při potížích s selhání rozšíření

### <a name="re-running-the-extension-on-the-vm"></a>Opětovným spuštěním rozšíření v OM

Používáte skripty na OM pomocí vlastní skript linky, které můžou nastat někdy chybu kdy OM byl úspěšně vytvořen, ale skript se nezdařila. Doporučené postupy k obnovení ze tato chyba za těchto podmínek je odebrat rozšíření a znovu spusťte šabloně.
Poznámka: V budoucnu prvek pro tuhle funkci by je možné vylepšovat odebrat nutnosti odinstalace rozšíření.


#### <a name="remove-the-extension-from-azure-powershell"></a>Odebrání rozšíření z Azure PowerShell

    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Po odebrání rozšíření může být znovu provést spustit skripty v OM šabloně.
