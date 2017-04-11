<properties
   pageTitle="Konfigurace služby Vyrovnávání zatížení pro SQL vždy na | Microsoft Azure"
   description="Konfigurace služby Vyrovnávání zatížení pro práci s SQL vždy na a jak pomocí powershellu vytvořit Vyrovnávání zatížení pro implementaci SQL"
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

# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigurace služby Vyrovnávání zatížení pro SQL vždy na

SQL Server AlwaysOn dostupnost skupiny můžete spustit teď ILB. Dostupnost skupina je serveru SQL Server nejdůležitějšího řešení pro vysokou dostupnost a havárie obnovení. Skupina posluchače dostupnost umožňuje klienta aplikacím bezproblémové připojení k primárního otevřené, bez ohledu na počet replikám v konfiguraci.

Název posluchače (DNS) namapovala k IP adrese Vyrovnávání zatížení a vyrovnávání zatížení na Azure směrovat tak, aby příchozích jenom primární server v sadě otevřené.

Použít ILB podporu pro koncové body AlwaysOn SQL Server (posluchače). Nyní ovládat přístupnosti posluchače a můžete vybírat IP adresu Vyrovnávání zatížení konkrétní podsítě v síti virtuální (VNet).

Pomocí ILB na posluchači koncový bod SQL server (například serveru = tcp:ListenerName, 1433; databáze = název databáze) je dostupné jenom tak, že:

- Služby a VMs ve stejné síti virtuální
- Služby a VMs z připojeného místní síti
- Služby a VMs z propojené VNets

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Obrázek 1 – SQL AlwaysOn nakonfigurována internetového Vyrovnávání zatížení

## <a name="add-internal-load-balancer-to-the-service"></a>Přidání interního Vyrovnávání zatížení služby

1. V následujícím příkladu jsme nastaví její konfiguraci virtuální síti, která obsahuje podsítě s názvem "Podsítě-1":

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Přidání koncové body rozloženy pro ILB na každý OM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Ve výše uvedeném příkladu máte označované jako "sqlsvc1" i "sqlsvc2" 2 OM v cloudu služby "Sqlsvc". Po vytvoření ILB přepínačem "DirectServerReturn", přidáte koncové body rozloženy ILB umožňuje SQL konfigurace posluchače pro skupiny dostupnosti.

Další informace o SQL AlwaysOn najdete v článku [Konfigurace Vyrovnávání zatížení interní dostupnost skupiny AlwaysOn v Azure](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Viz taky

[Začínáme konfigurace Internetové Vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md)

[Začínáme konfigurace vyrovnávání zatížení vnitřní](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
