<properties
    pageTitle="Ve službě cloudového připojit Linux VMs | Microsoft Azure"
    description="Připojte Linux virtuálních počítačích vytvořená pomocí klasické nasazení modelu Azure cloudové služby nebo virtuální sítě."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="connect-linux-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Připojení Linux virtuálních počítačích vytvořená pomocí klasické nasazení modelu pomocí virtuální sítě nebo cloudové služby

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Linux virtuálních počítačích vytvořená pomocí klasické nasazení modelu jsou vždy umístěny do cloudové služby. Cloudové služby se chová jako kontejner a poskytuje jedinečný název veřejné DNS, veřejnou IP adresu a sadu koncové body pro přístup k virtuálního počítače přes Internet. V síti virtuální může být cloudové služby, ale není to však povinné. Můžete také [připojení virtuálních počítačích Windows s virtuální sítě nebo cloudové služby](virtual-machines-windows-classic-connect-vms.md).

Není-li do cloudové služby v síti virtuální, nazývá do *samostatného* cloudové služby. Virtuálních počítačích v samostatné cloudové službě pouze komunikovat s jiných virtuálních počítačích pomocí veřejných názvy DNS jiných virtuálních počítačích uživatelů a přenos prochází přes Internet. Pokud do cloudové služby je virtuální sítě, virtuálních počítačích v této službě cloud komunikovat s všechny ostatní virtuálních počítačích v virtuální sítě bez odeslání všechny přenosy přes Internet.

Pokud umístíte virtuálních počítačích ve stejné samostatného cloudové služby, můžete dál používat vyrovnávání zatížení a dostupnosti sady. Podrobnosti najdete v tématu [Vyrovnávání zatížení virtuálních počítačích](virtual-machines-linux-load-balance.md) a [Spravovat dostupnost virtuálních počítačích](virtual-machines-linux-manage-availability.md). Nelze však uspořádání virtuálních počítačích podsítí nebo připojit do samostatného cloudové služby v místní síti. Tady je příklad:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Další kroky

Po vytvoření virtuálního počítače je vhodné [Přidat data disk](virtual-machines-linux-classic-attach-disk.md) tak vašich služeb a úloh měli umístění pro uložení data. 



