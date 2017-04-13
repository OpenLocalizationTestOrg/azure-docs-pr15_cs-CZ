<properties
    pageTitle="Poradce při potížích s chyby přidělení OM Windows | Microsoft Azure"
    description="Poradce při potížích s přidělení chyb při vytváření, restartujte nebo změna velikosti OM Windows v Azure"
    services="virtual-machines-windows, azure-resource-manager"
    documentationCenter=""
    authors="JiangChen79"
    manager="felixwu"
    editor=""
    tags="top-support-issue,azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/02/2016"
    ms.author="cjiang"/>

# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-windows-vms-in-azure"></a>Poradce při potížích s přidělení chyb při vytváření, restartujte nebo změna velikosti VMs Windows v Azure

Při vytváření virtuálního počítače, restartujte zastaveno (zrušeny, takže) VMs nebo změna velikosti virtuálního počítače, Microsoft Azure přidělí výpočetním prostředky k předplatnému. Při provádění tyto operace – ještě před dosažením omezení Azure předplatné může občas dojít k chybám. Tento článek popisuje některé běžné chyby přidělení příčiny a navrhne možné remediation. Informace mohou být také užitečné při plán nasazení služeb. Můžete také [Poradce při potížích s přidělení chyby při vytvoření, znovu spustit nebo změna velikosti Linux VMs v Azure](virtual-machines-linux-allocation-failure.md).

[AZURE.INCLUDE [virtual-machines-common-allocation-failure](../../includes/virtual-machines-common-allocation-failure.md)]
