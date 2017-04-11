<properties
   pageTitle="Virtuální více ke cloudové službě"
   description="Základní informace o tom, jak nastavit více virtuální na Cloudová služba a multiVIP"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Konfigurace více virtuální ke cloudové službě

Azure cloudovým službám získat přístup prostřednictvím veřejné Internet pomocí IP adresu podle Azure. Tento veřejnou IP adresu se nazývá VIP (virtuální IP) vzhledem k tomu je propojená s Vyrovnávání zatížení Azure a ne virtuálního počítače (OM) instance do cloudové služby. Všechny instance OM v rámci do cloudové služby se dostanete pomocí jednoho VIP.

Můžou ale nastat situace, kdy budete potřebovat víc než jednu VIP jako položky přejděte na stejné cloudové služby. Cloudové služby, například hostitelském více webů, které vyžadují připojení SSL pomocí výchozí port 443, jako každý web je hostovaný jinou zákazníka nebo klient. V tomto scénáři je třeba a jiné veřejné vystaveného IP adresa pro každý web. Následující diagram znázorňuje hostování s potřeba více certifikáty SSL na stejném veřejné portu typické více klienta webů.

![Scénáře s víc VIP SSL](./media/load-balancer-multivip/Figure1.png)

Ve výše uvedeném příkladu všech adres VIP používat stejný veřejné port (443) a přenosy se přesměruje do jedné nebo více zatížení rovnováha VMs jedinečné soukromé port pro interní IP adresu cloudové službě hostingu všechny weby.

>[AZURE.NOTE] Jiné situace vyžadující použití více virtuální hostuje více SQL AlwaysOn dostupnost skupiny posluchačů na stejnou sadu virtuálních počítačích.

Virtuální jsou dynamické ve výchozím nastavení, což znamená, že v průběhu času mění skutečné IP adresa přiřazená cloudovou službu. Chcete-li zabránit, můžete rezervovat VIP službě. Další informace o rezervovaná virtuální, najdete v článku [Veřejné User](../virtual-network/virtual-networks-reserved-public-ip.md).

>[AZURE.NOTE] Najdete v tématu [IP adresu ceny](https://azure.microsoft.com/pricing/details/ip-addresses/) pro informace o cenách na virtuální a rezervovaná IP adresy.

Můžete pomocí prostředí PowerShell můžete ověřit virtuální používá cloudové služby, jakož i přidat a odebrat virtuální, přidružit VIP na koncový bod a konfigurace služby Vyrovnávání na konkrétní VIP zatížení.

## <a name="limitations"></a>Omezení

Funkce více VIP je v současné době omezena v následujících případech:

- **Pouze IaaS**. Více VIP můžete povolit jenom pro cloudové služby, které obsahují VMs. Více VIP nelze použít v PaaS scénáře s rolí instance.
- **Pouze Powershellu**. Více VIP můžete spravovat pouze pomocí prostředí PowerShell.

Tato omezení jsou dočasné a může kdykoli změnit. Zkontrolujte, že návštěvě této stránky a ověřte budoucí změny.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Jak přidat VIP do cloudové služby

Přidání VIP ke službě, spusťte tento příkaz Powershellu:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

Tento příkaz zobrazí výsledek podobně jako v následujícím příkladu:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Jak odebrat VIP z cloudové služby

Pokud chcete odebrat VIP přidali do služby v předchozím příkladu, spusťte tento příkaz Powershellu:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] VIP můžete odebírat jenom v případě, že má žádné koncové body přidružené k němu.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Jak získat VIP informace z cloudové služby

Pokud chcete načíst virtuální přidružené do cloudové služby, spusťte tento skript Powershellu:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Skript zobrazí výsledek podobně jako v následujícím příkladu:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

V tomto příkladu cloudovou službu obsahuje 3 virtuální:

- **Vip1** je výchozí VIP, víte, že protože hodnota pro IsDnsProgrammedName je nastavena na hodnotu true.
- **Vip2** a **Vip3** nejsou použity jako nemají žádné IP adres. Pouze používají Pokud přiřadíte koncový bod k VIP.

>[AZURE.NOTE] Vaše předplatné pouze strhne příslušná navíc virtuální po jsou přidružené k koncový bod. Další informace o cenách najdete v článku [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Jak přidružit VIP na koncový bod

Pokud chcete spojit VIP na do cloudové služby na koncový bod, spusťte tento příkaz Powershellu:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

Příkaz vytvoří koncový bod propojen VIP s názvem *Vip2* na port *80*a odkazy se bude v angličtině s názvem *myVM1* v cloudové službě s názvem *Moje_služba* pomocí *protokolu TCP* na port *8080*.

Ověřte konfiguraci, spusťte tento příkaz Powershellu:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Výstup vypadá podobně jako v následujícím příkladu:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Jak povolit na konkrétní VIP Vyrovnávání zatížení

Jeden VIP lze přidružit více virtuálních počítačích pro účely Vyrovnávání zatížení. Například máte do cloudové služby s názvem *Moje_služba*a dvě virtuálních počítačích s názvem *myVM1* a *myVM2*. A cloudové služby obsahuje více virtuální, jedna z nich s názvem *Vip2*. Pokud chcete zajistit, aby se všechny přenosy na port *81* na *Vip2* vyrovnávání mezi *myVM1* a *myVM2* na port *8181*, spusťte tento skript Powershellu:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

Můžete taky aktualizovat Vyrovnávání zatížení používat různé VIP. Třeba při spuštění příkazu Powershellu, se změní nastavit na stav použít VIP s názvem Vip1 Vyrovnávání zatížení:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Další kroky

[Protokol analýz pro vyrovnávání zatížení Azure](load-balancer-monitor-log.md)

[Přehled Vyrovnávání zatížení vystaveného internetového](load-balancer-internet-overview.md)

[Začínáme na Internetu Vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md)

[Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md)

[User REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx)