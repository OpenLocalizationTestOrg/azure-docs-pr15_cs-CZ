<properties
    pageTitle="Stažení Azure SDK pro PHP"
    description="Zjistěte, jak si stáhněte a nainstalujte Azure SDK pro PHP."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>Stažení Azure SDK pro PHP

## <a name="overview"></a>Základní informace

Azure SDK PHP obsahuje prvky, které umožňují vývoj, nasazení a správu PHP žádosti o Azure. Konkrétně SDK Azure pro PHP patří:

* **PHP knihoven klienta pro Azure**. Tyto třídy knihovny poskytují rozhraní pro přístup k Azure funkce, jako jsou služby správy dat a cloudových služeb.  
* **Rozhraní Azure příkazového řádku pro Mac a Linux, systému Windows (Azure rozhraní příkazového řádku)**. Toto je sady příkazů pro nasazení a správu Azure služby, jako je Azure weby a Azure virtuálních počítačích. Rozhraní příkazového řádku Azure práce na všech informacemi o platformě, včetně Windows, Mac a Linux.
* **Azure Powershellu (pouze Windows)**. To je sada rutiny prostředí PowerShell pro nasazení a správu Azure služby, jako je cloudovými službami a virtuálních počítačích.
* **Azure emulátorů (pouze Windows)**. Emulátorů výpočetním a úložiště jsou místní emulátorů cloudovým službám a služby správy dat, které umožňují testování aplikace místně. Azure emulátorů operačním systému Windows pouze.

Následující oddíly popisují stažení a instalace součásti jsme je popsali výše.

Pokyny v tomto tématu se předpokládá, že máte [PHP] [ install-php] nainstalovaný.

> [AZURE.NOTE] PHP 5,5 nebo vyšší pro účely Azure knihoven klienta PHP, musíte mít.

##<a name="php-client-libraries-for-azure"></a>PHP knihoven klienta pro Azure

Knihoven PHP klienta pro Azure poskytují rozhraní pro přístup k Azure funkce, jako jsou služby správy dat a cloudových služeb z žádný operační systém. Těchto knihoven je možné nainstalovat prostřednictvím autora.

Informace o použití knihoven PHP klienta pro Azure, přečtěte si, [jak používat službu objektů Blob][blob-service], [jak používat tabulku služba] [ table-service] a [jak používat službu fronty][queue-service].

###<a name="install-via-composer"></a>Instalace prostřednictvím autora

1. [Instalace libovolná][install-git].


    > [AZURE.NOTE] Ve Windows bude taky musíte přidat spustitelný libovolná do vašeho prostředí Path.

2. Vytvoření souboru s názvem **composer.json** v kořenovém projektu a přidejte do něj následující kód:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Stáhněte si ** [composer.phar] [ composer-phar] ** v kořenu projektu.

4. Otevřete příkazový řádek a spustit v kořenu projektu

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell a Azure emulátorů

Azure Powershellu je sada rutiny prostředí PowerShell pro nasazení a správu služby Azure (například cloudovými službami a virtuálních počítačích). Azure emulátorů jsou emulátorů cloudové služby a služby správy dat, které umožňují testování aplikace místně. Jsou podporované tyto součásti jenom Windows.

Doporučené postupy k instalaci Azure PowerShell a emulátorů Azure, je použít [Webové platformy Microsoft][download-wpi]. Všimněte si, že můžete také nainstalovat jiné součásti vývoj, například PHP, SQL Server, Drivers Microsoft SQL Server pro PHP a WebMatrix.

Informace o tom, jak pomocí Powershellu Azure, přečtěte si, [jak pomocí Powershellu Azure][powershell-tools].

##<a name="azure-cli"></a>Azure rozhraní příkazového řádku

Rozhraní příkazového řádku Azure je sada příkazů pro nasazení a správu Azure služby, jako je Azure weby a Azure virtuálních počítačích. Informace o instalaci Azure rozhraní příkazového řádku naleznete v tématu [instalace Azure rozhraní příkazového řádku](xplat-cli-install.md).

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [PHP Developer Center](/develop/php/).


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
