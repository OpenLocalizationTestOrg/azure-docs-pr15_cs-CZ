<properties
    pageTitle="Použití koncovku CustomScript na Linux OM | Microsoft Azure"
    description="Naučte se používat rozšíření CustomScript nasazení aplikace na Linux virtuálních počítačích v Azure vytvořené pomocí klasické nasazení modelu."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Nasazení aplikace svítilna ve tvaru přípony Azure CustomScript Linux#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Azure CustomScript v článku rozšíření Microsoftu pro Linux umožňuje přizpůsobit virtuálních počítačích (VMs) spuštěním libovolného kódu v jakémkoli skriptování jazyku nepodporuje OM (například Python a flám). To umožňuje flexibilní k automatizaci nasazení aplikace do více počítačů.

Můžete nasadit koncovku CustomScript pomocí portálu Azure klasické, prostředí Windows PowerShell nebo Azure příkazového řádku prostředí (Azure rozhraní příkazového řádku).

V tomto článku, které budeme používat Azure rozhraní příkazového řádku pro nasazení aplikace jednoduché svítilna angličtině se systémem Ubuntu vytvořené pomocí klasické nasazení modelu.

## <a name="prerequisites"></a>Zjistit předpoklady pro

V tomto příkladu nejprve vytvořte dvěma Azure VMs s systémem Ubuntu 14.04 nebo novějším. VMs nazývají *skript OM* a *svítilna OM*. Používejte jedinečné názvy vytvoříte VMs. Jednu slouží ke spuštění příkazu rozhraní příkazového řádku a jednu slouží k nasazení aplikace svítilna.

Potřebujete ještě účet Azure úložiště a klíč pro přístup k (dostanete se to z portálu Microsoft Azure klasické).

Pokud potřebujete pomoc s vytvořením Linux VMs na Azure v nápovědě k [Vytvoření virtuálního počítače systém Linux](virtual-machines-linux-classic-createportal.md).

Příkazy nainstalovat předpokládají systémem Ubuntu, ale můžete příklad upravit instalace pro všechny podporované distro Linux.

Skript OM OM musí mít nainstalovaný pracovní připojení k Azure rozhraní příkazového řádku Azure. S tím vám pomůže příručce [Nainstalujte a nakonfigurujte rozhraní Azure příkazového řádku](../xplat-cli-install.md).

## <a name="upload-a-script"></a>Nahrání skriptu

Použijeme koncovku CustomScript spuštění skriptu na remote OM nainstalovat zásobníku svítilna a slouží k vytvoření PHP stránky. Při přístupu k skript odkudkoli jsme budete ho uložte jako objektů blob Azure.

### <a name="script-overview"></a>Přehled skriptu

Příklad skript nainstaluje svítilna zásobníku do (včetně nastavení pasivní nainstalovat MySQL) se systémem Ubuntu, zapíše jednoduchý soubor PHP a spustí Apache.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Nahrání skriptu

Skript uložit jako textový soubor, například *install_lamp.sh*a pak ho nahrajte do úložiště Azure. Můžete to udělat snadno s Azure rozhraní příkazového řádku. Následující příklad odešlou soubor kontejneru úložiště s názvem "skripty". Pokud kontejneru neexistuje bude nutné nejprve vytvořit.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Také vytvořte JSON soubor, který popisuje, jak stáhnout skript z Azure úložiště. Uložte jako *public_config.json* (nahrazení "mystorage" název účtu úložiště):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Nasazení rozšíření

Teď můžete další příkaz pro nasazení rozšíření CustomScript Linux vzdálené angličtině pomocí rozhraní příkazového řádku Azure.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Příkaz předchozí a soubory ke stažení spustí skript *install_lamp.sh* na OM s názvem *svítilna OM*.

Protože aplikace obsahuje na webový server, nezapomeňte otevřete listening port HTTP na remote OM pomocí příkazu Další.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Sledování a odstraňování potíží

Můžete zkontrolovat, že na chod vlastní skript spustí prohlížíte soubor protokolu na remote OM. SSH *svítilna OM* a zadní soubor protokolu s příkazem Další.

    cd /var/log/azure/customscript
    tail -f handler.log

Po spuštění koncovku CustomScript můžete přecházet na stránce PHP, který jste vytvořili informace. Na stránce PHP, aby příklad v tomto článku je *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Další zdroje informací

Můžete stejnou základní kroky pro nasazení složitější aplikace. V tomto příkladu je jako veřejné objektů blob v úložišti Azure uložený skript nainstalovat. Bezpečnější možnosti bude obsahují instalační skript jako bezpečné objektů blob [Zabezpečený přístup k podpisu](https://msdn.microsoft.com/library/azure/ee395415.aspx) (přidružení zabezpečení).

Další zdroje pro rozhraní příkazového řádku Azure, Linux a rozšíření CustomScript, najdete další.

[Automatizace úkolů přizpůsobení Linux OM ve tvaru přípony CustomScript](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Rozšíření Azure Linux (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux a otevřít zdroje na Azure](virtual-machines-linux-opensource-links.md)
