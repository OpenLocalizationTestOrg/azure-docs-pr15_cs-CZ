<properties
   pageTitle="Začít vytvářet internetové Vyrovnávání zatížení v klasického režimu pomocí prostředí PowerShell | Microsoft Azure"
   description="Naučte se vytvářet internetové Vyrovnávání zatížení v klasického režimu pomocí prostředí PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Začít vytvářet internetové Vyrovnávání zatížení (klasický) v prostředí PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení. Můžete také [Naučte se vytvářet internetové Vyrovnávání zatížení pomocí Správce prostředků Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Nastavení Vyrovnávání zatížení pomocí prostředí PowerShell

Pokud chcete nastavit vyrovnávání zatížení pomocí powershellu, postupujte následujícím způsobem:

1. Pokud máte Azure Powershellu, přečtěte si, [Jak nainstalovat a konfigurace prostředí PowerShell Azure](../../articles/powershell-install-configure.md) a postupujte podle pokynů úplně za účelem Přihlaste se k Azure a potom vyberte předplatné.


2. Po vytvoření virtuálního počítače, můžete pomocí rutin prostředí PowerShell přidáte virtuálního počítače v rámci stejné cloudové služby Vyrovnávání zatížení.

V následujícím příkladu přidáte sady Vyrovnávání zatížení označované jako "webfarm" do cloudu služba "mytestcloud" (nebo myctestcloud.cloudapp.net), přidání koncové body pro vyrovnávání zatížení virtuálních počítačích s názvem "web1" a "webu 2". Vyrovnávání zatížení přijímá přenosy na portu 80 a načtení zůstatků mezi virtuálních počítačích definované místní koncový bod (v tomto případu port 80) pomocí protokolu TCP.


### <a name="step-1"></a>Krok 1
Vytvoření koncového bodu rozloženy pro první OM "web1"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Krok 2

Vytvořte jiný koncový bod pro druhý OM "webu 2" se stejným názvem sadu Vyrovnávání zatížení

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Odebrání virtuálního počítače z vyrovnávání zatížení

Můžete odebrat AzureEndpoint koncový bod virtuálního počítače odeberete Vyrovnávání zatížení

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Další kroky

Můžete taky [začít vytvářet Vyrovnávání zatížení vnitřní](load-balancer-get-started-ilb-classic-ps.md) a konfigurace jaký druh [režimu rozdělení](load-balancer-distribution-mode.md) pro especific zatížení vyrovnávání sítě přenosy chování.

Pokud potřebujete-li zachovat spojení aktivní serverů za vyrovnávání zatížení aplikace, můžete chápete víc o [Nastavení časového limitu nečinnosti TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md). Další informace o chování nečinná připojení, když používáte Vyrovnávání zatížení Azure vám pomůže.

