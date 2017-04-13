<properties
    pageTitle="Aktualizace Azure Agent Linux z GitHub | Microsoft Azure"
    description="Zjistěte, jak aktualizace Azure Linux Agent pro Linux OM v Azure verzi lateset Github"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Jak aktualizovat Linux Agent Azure na virtuálního počítače na nejnovější verzi z GitHub

Aktualizovat [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) na Linux OM v Azure, musíte už máte:

1. Průběžný Linux OM v Azure.
2. Připojení k této Linux OM pomocí SSH.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Pokud budete tento úkol z počítače s Windows, můžete nátěrové SSH do počítače Linux. Další informace najdete v tématu [jak přihlášení k virtuálního počítače systém Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Potvrzené Azure distros Linux tak jste umístili balíček Azure Linux agenta v jejich úložištích zkontrolujte a nainstalujte nejnovější verzi z této Distro úložiště nejdřív Pokud je to možné.  

Pro systémem Ubuntu stačí zadejte:

    #sudo apt-get install walinuxagent

A na CentOS, zadejte:

    #sudo yum install waagent


Oracle Linux, ujistěte se, že `Addons` úložiště je povolený. Zvolte Upravit `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) nebo `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux) a změnit plnou čáru `enabled=0` k `enabled=1` v části **[ol6_addons]** a **[ol7_addons]** v tomto souboru.

Pokud chcete nainstalovat nejnovější verzi Azure Linux Agent, zadejte:


    #sudo yum install WALinuxAgent

Pokud nenajdete úložišti doplněk můžete jednoduše přidat tyto řádky na konci souboru .repo podle verzi Oracle Linux:

Pro Oracle Linux 6 virtuálních počítačích:

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Pro Oracle Linux 7 virtuálních počítačích:

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Zadejte:

    #sudo yum update WALinuxAgent

Obvykle to je vše, budete potřebovat, ale pokud z nějakého důvodu budete muset nainstalovat z https://github.com přímo, postupujte takto.


## <a name="install-wget"></a>Instalace wget

Přihlaste se k vaší OM pomocí SSH.

Instalace wget (existují některé distros nemáte nainstalovanou ve výchozím nastavení například Redhat CentOS a Oracle Linux verze 6.4 a 6.5) tak, že zadáte `#sudo yum install wget` do příkazového řádku.


## <a name="download-the-latest-version"></a>Stáhněte si nejnovější verzi

Otevřete [verzi Azure Linux Agent v GitHub](https://github.com/Azure/WALinuxAgent/releases) na webovou stránku a o tom číslo nejnovější verzi. (Vyhledejte vaše aktuální verze zadáním `#waagent --version`.)

### <a name="for-version-20x-type"></a>U verze 2.0.x, zadejte:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   Následující řádek používá verze 2.0.14 jako příklad:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>U verze 2.1.x nebo novější, zadejte:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   Následující řádek používá verze 2.1.0 jako příklad:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>Instalace Azure Linux Agent

### <a name="for-version-20x-use"></a>U verze 2.0.x, použití:

 Vytvořit waagent spustitelný soubor:

    #chmod +x waagent

 Zkopírujte nové spustitelný soubor usr/sbin /.

  Pro většinu Linux použijte:

    #sudo cp waagent /usr/sbin

  U jádro operačního systému můžete pomocí:

    #sudo cp waagent /usr/share/oem/bin/

  Pokud jde o novou instalaci Azure Linux zástupce spustit:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>U verze 2.1.x, použití:

Budete muset nainstalovat balíček `setuptools` nejdřív – najdete v článku [tady](https://pypi.python.org/pypi/setuptools). Spusťte:

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>Restartujte službu waagent

Pro většinu linux Distros:

    #sudo service waagent restart

U systémem Ubuntu můžete pomocí:

    #sudo service walinuxagent restart

U jádro operačního systému můžete pomocí:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>Ověřte verzi Azure Linux Agent

    #waagent -version

Pro jádro operačního systému nemusí fungovat výše uvedený příkaz.

Zobrazí se, že verze služby Azure Linux Agent aktualizovaný nové verze.

Další informace týkající se Azure Linux Agent najdete v článku [Soubor Readme pro službu Azure Linux Agent](https://github.com/Azure/WALinuxAgent).
