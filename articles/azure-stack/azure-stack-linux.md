<properties
    pageTitle="Hosté Linux Azure zásobníku | Microsoft Azure"
    description="Zjistěte, jak vytvořit na základě Linux virtuálních počítačích Azure zásobníku."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>Nasazení Linux virtuálních počítačích Azure zásobníku

Přidáním obrázku na základě Linux do zásobníku Azure Marketplace nástroje můžete nasazovat Linux virtuálních počítačích na Koncepce zásobníku Azure. Několik dodavatelů Linux poskytly obrázky přidané do Koncepce zásobníku Azure nebo můžete vytvořit vlastní.

## <a name="download-an-image"></a>Stáhněte si obrázek

 1. Stáhněte si a extrahovat Azure zásobníku kompatibilní s prohlížečem obrázek z následujících odkazů nebo Příprava vlastní:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [Jádro operačního systému](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Systémem Ubuntu 14.04 l](https://partner-images.canonical.com/azure/azure_stack/) / [systémem Ubuntu 16.04 l](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Extrahování obrázek virtuální pevný disk v případě potřeby a [Přidání obrázku na Tržiště](azure-stack-add-vm-image.md). Ujistěte se, že `OSType` je parametr nastaven na `Linux`.
 
 3. Po přidání obrázku na Tržiště, vytvoření položky Marketplace a nasadíte virtuálního počítače Linux.
  
## <a name="prepare-your-own-image"></a>Příprava vlastní obrázek

1. Příprava Linux obrázek jedním z následujících pokynů:
 - [Na základě centOS rozdělení](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Červené klobouk Enterprise Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Se systémem Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Stažení a instalace [Azure Linux Agent](https://github.com/Azure/WALinuxAgent/)

    Linux Agent Azure verze 2.1.3 nebo vyšší se členství požaduje pro zřízení Linux OM Azure zásobníku. Tato verze agent nebo vyšší jako balíček spoustu rozdělení výše uvedené už je součástí v jejich úložištích (obvykle s názvem `WALinuxAgent` nebo `walinuxagent`). Ale pokud verze balíček Azure agenta je menší než 2.1.3 (tedy 2.0.18 nebo nižší), a pak nainstalujte agent ručně. Nainstalovanou verzí můžete určit, pod názvem balíčku nebo spuštěním `/usr/sbin/waagent -version` na OM.

    Postupujte podle pokynů a nainstalovat ručně - agent Azure Linux

 - Nejdřív si stáhněte nejnovější agent Azure Linux z [Github](https://github.com/Azure/WALinuxAgent/releases), například:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - Rozbalení Azure agent:

            # tar -vzxf v2.2.0.tar.gz

 - Instalace python setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - Nainstalujte Azure agent:

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Systémy s Python 2.x a Python 3.x nainstalovaný vedle sebe muset spusťte tento příkaz:

        # sudo python3 setup.py install --register-service

    Další informace najdete v tématu agenta Linux Azure [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md).

3. [Přidání obrázku na Tržiště](azure-stack-add-vm-image.md). Ujistěte se, že `OSType` je parametr nastaven na `Linux`.

4. Po přidání obrázku na Tržiště, vytvoření položky Marketplace a nasadíte virtuálního počítače Linux.

## <a name="next-steps"></a>Další kroky

[Nejčastější dotazy k vrstvě Azure](azure-stack-faq.md)