<properties
   pageTitle="Automatizace nasazení aplikace s příponami virtuálního počítače | Microsoft Azure"
   description="Kurz DotNet Core Azure virtuálního počítače"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Nasazení aplikace se šablonami správce prostředků Azure

Až všechny Azure infrastruktury požadavky byly identifikovat a převést na nasazení šablony, nasazení skutečné aplikace, musí být adresovaná. Nasazení aplikace v tomto poli odkazuje instalace binární soubory skutečné aplikace na Azure zdroje. Příklad hudbu úložiště, .net základní NGINX a správce muset nainstalovali a nakonfigurovali do každého virtuálního počítače. Binární hudbu úložiště je třeba nainstalovat do virtuálního počítače a databáze hudbu úložiště předem vytvořená.

Tento dokument podrobnosti, jak můžete automatizovat rozšíření virtuálního počítače nasazení aplikace a konfigurace Azure virtuálních počítačích. Závislosti a jedinečné konfigurace se zvýrazní. Která zaručuje nejlepší možnosti, předem nasazení instanci řešení Azure předplatného a práce spolu s šabloně správce prostředků Azure. Dokončení šablony najdete tady – [Nasazení úložiště hudby na systémem Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Konfigurace skriptu

Rozšíření virtuálního počítače jsou specializované programy, které spouštět oproti virtuálních počítačích stanovit konfigurační automatizaci. Rozšíření jsou k dispozici pro mnoho konkrétní účely například antivirový konfigurace protokolování a konfigurace Docker. Rozšíření vlastní skript lze spustit všechny skript vůči virtuálního počítače. S ukázkovými hudbu úložiště je koncovku vlastní skript na konfigurace virtuálních počítačích se systémem Ubuntu a instalace aplikace hudbu úložiště přihlašovacích údajů.

Před s podrobnostmi, jak deklarované rozšíření virtuálního počítače v šabloně aplikace Správce prostředků Azure, zkontrolujte skript, který se spustí. Tento skript nakonfiguruje virtuální počítač se systémem Ubuntu hostovat aplikace hudbu úložiště přihlašovacích údajů. Při spuštění, nainstaluje skript všechny potřebné software, instalace aplikace úložiště přihlašovacích údajů hudbu z ovládacího prvku zdroje a připravit databázi. 

Další informace o hostování .net základní aplikace na Linux najdete v článku [Publikovat provozním prostředí Linux](https://docs.asp.net/en/latest/publishing/linuxproduction.html). 

> V tomto příkladu je určený pro účely ukázky.

```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>Rozšíření skript OM

Rozšíření OM můžete kontrolovat virtuálního počítače v čase sestavit s využitím rozšíření prostředků v šabloně správce prostředků Azure. Rozšíření je možné přidat pomocí průvodce Visual Studio přidat zdroje nebo vložením platné JSON do šablony. Rozšíření skript zdroje vnořené do virtuálního počítače prostředku. To můžete vidět v následujícím příkladu.

Tento odkaz zobrazíte JSON vzorku v šabloně správce prostředků – [OM skript rozšíření](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359). 

Všimněte si v pod JSON skript uložený ve GitHub. Tento skript může taky uložené v úložišti objektů Blob Azure. Správce prostředků Azure šablony umožňují řetězec spuštění skript, který má vytvořen tak, aby se hodnoty parametrů šablony mohou sloužit jako parametry pro spuštění skriptu. V tomto případě je údaje při nasazování šablon a tyto hodnoty se pak dá použít při provádění skriptu.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Další informace o použití koncovku vlastní skript najdete v článku [rozšíření vlastní skript šablonami správce prostředků](./virtual-machines-linux-extensions-customscript.md).

## <a name="next-step"></a>Další krok

<hr>

[Prozkoumání více Azure správce prostředků šablony](https://github.com/Azure/azure-quickstart-templates)
