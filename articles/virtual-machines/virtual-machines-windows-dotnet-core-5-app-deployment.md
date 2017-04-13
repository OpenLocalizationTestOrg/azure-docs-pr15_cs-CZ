<properties
   pageTitle="Automatizace nasazení aplikace s příponami virtuálního počítače | Microsoft Azure"
   description="Kurz DotNet Core Azure virtuálního počítače"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Nasazení aplikace se šablonami správce prostředků Azure

Až všechny Azure infrastruktury požadavky byly identifikovat a převést na nasazení šablony, nasazení skutečné aplikace, musí být adresovaná. Nasazení aplikace v tomto poli odkazuje instalace binární soubory skutečné aplikace na Azure zdroje. Příklad hudbu úložiště, .net základní funkce a služby IIS muset nainstalovali a nakonfigurovali do každého virtuálního počítače. Binární hudbu úložiště je třeba nainstalovat do virtuálního počítače a databáze hudbu úložiště předem vytvořená.

Tento dokument podrobnosti, jak můžete automatizovat rozšíření virtuálního počítače nasazení aplikace a konfigurace Azure virtuálních počítačích. Závislosti a jedinečné konfigurace se zvýrazní. Která zaručuje nejlepší možnosti, předem nasazení instanci řešení Azure předplatného a práce spolu s šabloně správce prostředků Azure. Dokončení šablony najdete tady – [Nasazení hudbu úložiště přihlašovacích údajů v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows).

## <a name="configuration-script"></a>Konfigurace skriptu

Rozšíření virtuálního počítače jsou specializované programy, které spouštět oproti virtuálních počítačích stanovit konfigurační automatizaci. Rozšíření jsou k dispozici pro mnoho konkrétní účely například antivirový konfigurace protokolování a konfigurace Docker. Rozšíření vlastní skript lze spustit všechny skript vůči virtuálního počítače. S ukázkovými hudbu úložiště je koncovku vlastní skript na konfigurace virtuálních počítačích Windows a instalace aplikace hudbu úložiště přihlašovacích údajů.

Před s podrobnostmi, jak deklarované rozšíření virtuálního počítače v šabloně aplikace Správce prostředků Azure, zkontrolujte skript, který se spustí. Tento skript nakonfiguruje virtuálního počítače Windows hostovat aplikace hudbu úložiště přihlašovacích údajů. Při spuštění, nainstaluje skript všechny potřebné software, instalace aplikace úložiště přihlašovacích údajů hudbu z ovládacího prvku zdroje a připravit databázi. 

> V tomto příkladu je určený pro účely ukázky.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>Rozšíření skript OM

Rozšíření OM můžete kontrolovat virtuálního počítače v čase sestavit s využitím rozšíření prostředků v šabloně správce prostředků Azure. Rozšíření je možné přidat pomocí průvodce Visual Studio přidat zdroje nebo vložením platné JSON do šablony. Rozšíření skript zdroje vnořené do virtuálního počítače prostředku. To můžete vidět v následujícím příkladu.

Tento odkaz zobrazíte JSON vzorku v šabloně správce prostředků – [OM skript rozšíření](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339). 

Všimněte si v pod JSON skript uložený ve GitHub. Tento skript může taky uložené v úložišti objektů Blob Azure. Správce prostředků Azure šablony umožňují řetězec spuštění skript, který má být vytvořen tak, aby se hodnoty parametrů šablony mohou sloužit jako parametry pro spuštění skriptu. V tomto případě je údaje při nasazování šablon a tyto hodnoty se pak dá použít při provádění skriptu.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Další informace o použití koncovku vlastní skript najdete v článku [rozšíření vlastní skript šablonami správce prostředků](./virtual-machines-windows-extensions-customscript.md).

## <a name="next-step"></a>Další krok

<hr>

[Prozkoumání více Azure správce prostředků šablony](https://github.com/Azure/azure-quickstart-templates)
