<properties
    pageTitle="Instalace MongoDB na Windows OM | Microsoft Azure"
    description="Zjistěte, jak nainstalovat MongoDB Azure OM vytvořená pomocí klasické nasazení modelu spuštěný Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>Instalace MongoDB na Windows OM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Instalace a konfigurace MongoDB pomocí modelu správce prostředků nasazení, najdete v [tomto článku](virtual-machines-windows-classic-install-mongodb.md).

[MongoDB] [ MongoDB] Oblíbené zdroje otevřít, výkonné NoSQL databázi. Tento článek vás provede vytváření virtuálních počítačů systému Windows Server (OM) pomocí [Azure klasické portál][AzurePortal]. Vytvořte a připojte disk dat k OM před instalace a konfigurace MongoDB. Pokud máte existující OM v Azure, který chcete použít, můžete přejít přímo k [instalace a konfigurace MongoDB](#install-and-run-mongodb-on-the-virtual-machine).


## <a name="create-a-virtual-machine-running-windows-server"></a>Vytvoření virtuálního počítače spuštěný Windows Server

Postupujte podle těchto pokynů k vytvoření virtuálního počítače.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] Můžete přidat koncový bod pro MongoDB při vytváření virtuálního počítače a nakonfigurujte ji následujícím způsobem: nazvěte ji jako **Mongo**používají protokolu **TCP** a nastavte veřejných a privátních porty na **27017**.

## <a name="attach-a-data-disk"></a>Připojení dat disku
Stanovit úložiště virtuální počítač, připojte disk dat a potom inicializace tak, aby ji mohli použít Windows. Pokud už máte disk dat můžete připojit existující disku nebo připojíte prázdný disk.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Pokyny na inicializace disku najdete v tématu "jak: Inicializace nového disku dat v systému Windows Server" v tématu [jak připojit disk dat do virtuálního počítače Windows](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Instalace a spuštění MongoDB na virtuálního počítače

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Souhrn
V tomto kurzu se naučíte vytvořit virtuální počítač serverem Windows vzdáleně se k ní připojit a připojte data disk.  Naučili jste instalace a konfigurace MongoDB počítače virtuálního serveru s Windows. Nyní máte přístup MongoDB v počítači virtuálního serveru s Windows pomocí následujících Pokročilá témata v [si přečtěte následující dokumentaci MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
