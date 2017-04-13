<properties
    pageTitle="Plánovaná údržba pro Windows VMs | Microsoft Azure"
    description="Porozumět tomu, jaký Azure plánované údržby je a jaký vliv má vaše virtuálních počítačích Windows spuštěné v Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="drewm"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="drewm"/>

# <a name="planned-maintenance-for-virtual-machines-in-azure"></a>Plánovaná údržba virtuálních počítačích v Azure


Porozumět tomu, jaký Azure plánované údržby je a jak ji může ovlivnit dostupnost virtuálních počítačích Windows. Tento článek je také k dispozici pro [Linux virtuálních počítačích](virtual-machines-linux-planned-maintenance.md). 

Tento článek obsahuje pozadí tak, aby procesu Azure plánovanou údržbu. Pokud se chcete řešit, proč váš OM restartování počítače, můžete [číst tento blogu příspěvek s podrobnostmi zobrazení OM restartujte protokoly](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="why-azure-performs-planned-maintenance"></a>Proč Azure provádí plánované údržby

Microsoft Azure pravidelně provádí aktualizace po celém světě ke zlepšení spolehlivosti, výkon a zabezpečení infrastruktury Host (hostitel), který je základem virtuálních počítačích. V mnoha tyto aktualizace provádí bez žádný vliv na virtuálních počítačích nebo cloudové služby, včetně zachování paměti aktualizace.

Některé aktualizace však restartování na virtuálních počítačích instalace požadovaných aktualizací pro infrastrukturu. Virtuálních počítačích vypnout a jsme oprava infrastruktury klepněte restartování virtuálních počítačích.

Dejte pozor, aby existují dva typy údržby, které můžou ovlivnit dostupnost virtuálních počítačích: plánované a neplánované. Tato stránka popisuje, jak Microsoft Azure provádí plánovanou údržbu. Další informace o neplánovanou údržbu najdete v článku [Vysvětlení informací na plánované versus neplánovanou údržbu](virtual-machines-windows-manage-availability.md).

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]
