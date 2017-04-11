<properties
   pageTitle="Vytvoření Vyrovnávání zatížení interní pomocí Powershellu v modelu klasické nasazení | Microsoft Azure"
   description="Naučte se vytvářet Vyrovnávání zatížení interní pomocí Powershellu v modelu klasické nasazení"
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
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Vytvořit interní řešení pro vyrovnávání zatížení (klasický) pomocí prostředí PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](load-balancer-get-started-ilb-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Vytvoření Vyrovnávání zatížení interní nastavení pro virtuálních počítačích

Vytvoření sady Vyrovnávání zatížení vnitřní a servery, které vám pošle provoz k němu máte postupujte takto:

1. Vytvoření instance interní Vyrovnávání zatížení, který bude obsahovat koncového bodu příchozích být rozloženy do servery sady Vyrovnávání zatížení.

1. Přidání koncové body odpovídající virtuálních počítačích, které bude přijímat příchozí přenosy.

1. Konfigurace serverům, které bude odesílání přenosy být rozloženy směrování jejich přenosů na virtuální adresy IP (VIP) instance interní Vyrovnávání zatížení.


### <a name="step-1-create-an-internal-load-balancing-instance"></a>Krok 1: Vytvoření instance interní Vyrovnávání zatížení

Pro existující cloudové služby, nebo do cloudové služby používaný v části místní virtuální sítě můžete vytvořit instance interní Vyrovnávání zatížení pomocí následujících příkazů prostředí Windows PowerShell:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


Všimněte si, že tento pomocí rutiny prostředí Windows PowerShell [Přidat AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) používá sadu DefaultProbe parametr. Další informace o sady další parametr najdete v článku [Přidání AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>Krok 2: Přidání koncové body instanci interní Vyrovnávání zatížení

Tady je příklad:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="ilbset"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>Krok 3: Konfigurace serverech směrování jejich přenosů na nový koncový bod interní Vyrovnávání zatížení

Musíte nakonfigurovat servery, jejichž přenosy bude rozloženy Pokud chcete použít nový IP adresu (VIP) instance interní Vyrovnávání zatížení. Toto je adresa, na kterém je instance interní Vyrovnávání zatížení přijímá. Ve většině případů potřebujete jenom přidat nebo upravit záznam DNS pro VIP instance interní Vyrovnávání zatížení.

Pokud jste zadali při vytváření instance interní Vyrovnávání zatížení IP adresu, máte už VIP. V opačném uvidíte VIP z následujících příkazů:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



Pokud chcete použít tyto příkazy, vyplnit hodnoty a odeberte < a >. Tady je příklad:

    $svc="mytestcloud"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


Ze zobrazení příkazu Get-AzureInternalLoadBalancer poznamenejte si IP adresu a proveďte potřebné změny servery nebo záznamů DNS zajistit, že přenosy se odešlou do VIP.

>[AZURE.NOTE]Platformu Microsoft Azure používá statické a veřejně směrovatelné adresy IPv4 pro řadu možností pro správu. IP adresu je 168.63.129.16. Tuto IP adresu by neměly být blokovány všechny bránách firewall vzhledem k tomu může dojít k neočekávané chování.
>Pokud jde o Azure interní Vyrovnávání zatížení tuto IP adresu slouží sledováním sond z vyrovnávání zatížení k určení stavu virtuálních počítačích v sadě rozloženy. Pokud skupiny zabezpečení sítě se používá k omezení přenosy Azure virtuálních počítačích v sadě interně Vyrovnávání zatížení nebo se použije pro virtuální podsítě, zajistit, aby přidaná pravidlo zabezpečení sítě přenosy z 168.63.129.16 umožňující.


## <a name="example-of-internal-load-balancing"></a>Příklad Vyrovnávání zatížení interní

Můžete přecházet mezi konce na ukončit proces vytváření sady Vyrovnávání zatížení pro dvě příklad konfigurace, naleznete v následujících částech.

### <a name="an-internet-facing-multi-tier-application"></a>Internetu, vícevrstvé aplikace

Chcete-li zadat služby Vyrovnávání zatížení databáze pro sadu internetového webových serverů. Obě množiny servery jsou umístěny v jedné Azure Cloudová služba. Webový server umožnění datových přenosů do portu TCP 1433 musí rozdělí mezi dvěma virtuálních počítačích ve vrstvě databáze. Obrázek 1 je znázorněno.

![Vnitřní Vyrovnávání zatížení nastavení osy databáze](./media/load-balancer-internal-getstarted/IC736321.png)


Nastavení se skládá z následujících akcí:

- Existující cloudové služby, který je hostitelem virtuálních počítačích jmenuje mytestcloud.

- Dva stávající databázi servery jsou s názvem ZR1 DB2.

- Webových serverů ve vrstvě web připojovat k serverům databáze ve vrstvě databáze pomocí soukromé IP adresu. Další možností je použít vlastní DNS pro virtuální sítě a zaregistrovat ručně záznamu a pro sadu Vyrovnávání zatížení vnitřní.

Následující příkazy nakonfigurovat nové instance interní Vyrovnávání zatížení s názvem **ILBset** a přidejte koncové body virtuálních počítačích odpovídající dva databázové servery:

    $svc="mytestcloud"
    $ilb="ilbset"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $vmname="DB1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="TCP-1433-1433-2"
    $vmname="DB2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


## <a name="remove-an-internal-load-balancing-configuration"></a>Odebrání konfigurace interní Vyrovnávání zatížení

Chcete-li odebrat virtuálního počítače jako koncový bod instance Vyrovnávání zatížení vnitřní, použijte následující příkazy:

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Použít tyto příkazy, zadejte hodnoty, odebrání < a >.

Tady je příklad:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Chcete-li odebrat instanci Vyrovnávání zatížení vnitřní do cloudové služby, použijte následující příkazy:

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

Pokud chcete použít tyto příkazy, vyplňte hodnotu a odeberte < a >.

Tady je příklad:

    $svc="mytestcloud"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Další informace o rutinách Vyrovnávání zatížení vnitřní


Získat další informace o rutinách interní Vyrovnávání zatížení, spusťte následující příkazy příkazového prostředí Windows PowerShell:

- Získání nápovědy nové AzureInternalLoadBalancerConfig-úplné

- Získání nápovědy Přidání AzureInternalLoadBalancer-úplné

- Získání nápovědy Get-AzureInternalLoadbalancer-úplné

- Získání nápovědy odebrat AzureInternalLoadBalancer-úplné

## <a name="next-steps"></a>Další kroky

[Konfigurace režimu rozdělení Vyrovnávání zatížení pomocí spřažení IP zdroje](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)