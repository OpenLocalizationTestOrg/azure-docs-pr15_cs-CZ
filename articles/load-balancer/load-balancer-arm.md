<properties
   pageTitle="Azure podpory správce prostředků pro vyrovnávání zatížení | Microsoft Azure "
   description="Pomocí prostředí powershell pro vyrovnávání zatížení pomocí Správce prostředků Azure. Použití šablony pro vyrovnávání zatížení"
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


# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Podpora Azure správce prostředků pomocí vyrovnávání zatížení Azure

Azure správce prostředků je upřednostňované management framework služby v Azure. Azure Vyrovnávání zatížení můžete spravovat pomocí nástroje a rozhraní API založené na Azure správce prostředků.

## <a name="concepts"></a>Koncepty

Pomocí Správce prostředků Vyrovnávání zatížení Azure obsahuje následující podřízené zdroje:

- Konfigurace front-end IP – Vyrovnávání zatížení můžete zahrnout jeden nebo více front-end IP adresy, jinak jmenoval virtuální adresy IP (adres VIP). Tyto IP adresy sloužit jako průniku pro přenos.

- Fond back-end adres – jedná se o IP adresy přidružené s virtuálního počítače rozhraní karty NIC (Network) do kterého zátěž rozdělit.

- Načtení vyrovnávání pravidel – pravidla vlastnost mapuje dané front-end IP a kombinaci port pro sadu back-end IP adresy a kombinace portu. Vyrovnávání zatížení jednoho může obsahovat více pravidel pro vyrovnávání zatížení. Každé pravidlo je tvořen kombinací front-end IP a port a back-end IP a port přidružené VMs.

- Sond – sond umožňují ke sledování stavu OM instance. Když se zkušební stavu nepovede, instance OM vezme mimo otočení automaticky.

- Příchozí překladu síťových adres pravidla – překladu síťových adres definování příchozích prochází přední ukončit IP a úměrně IP back-end.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Rychlý úvod šablony

Azure správce prostředků umožňuje zřízení aplikace pomocí deklarativní šablony. V šabloně jednu nástroje můžete nasazovat více služeb spolu s jejich závislosti. Používat stejnou šablonu pro opakovaně nasazení aplikace každé fázi životního cyklu aplikace.

Šablony můžete zahrnout definice virtuálních počítačích virtuálních sítí, sady dostupnost, síťových rozhraní (NIC), úložiště účty, Vyrovnávání zatížení, skupiny zabezpečení sítě a veřejnou IP adresy. Pomocí šablon můžete vytvořit všechno, co budete potřebovat pro složité aplikace. Soubor šablony můžete zkontrolovat do systému správy obsahu pro správu verzí a spolupráci.

[Další informace o šablonách](http://go.microsoft.com/fwlink/?LinkId=544798)

[Další informace o prostředcích sítě](../virtual-network/resource-groups-networking.md)

Rychlý úvod šablony pomocí vyrovnávání zatížení Azure najdete v [úložišti GitHub](https://github.com/Azure/azure-quickstart-templates) hostingu sadu šablon komunity generovaného.

Příklady šablon:

- [2 VMs ve schůzce služby Vyrovnávání zatížení a pravidla pro vyrovnávání zatížení](http://go.microsoft.com/fwlink/?LinkId=544799)
- [2 VMs v VNET pravidlům interní Vyrovnávání zatížení a vyrovnávání zatížení](http://go.microsoft.com/fwlink/?LinkId=544800)
- [2 VMs v Vyrovnávání zatížení a konfiguraci překladu síťových adres pravidla LB](http://go.microsoft.com/fwlink/?LinkId=544801)


## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Nastavení Vyrovnávání zatížení Azure pomocí prostředí PowerShell nebo rozhraní příkazového řádku

Začínáme s správce prostředků Azure rutiny nástroje příkazového řádku a rozhraní REST API

- [Azure sítě](https://msdn.microsoft.com/library/azure/mt163510.aspx) lze použít k vytvoření Vyrovnávání zatížení.
- [Jak vytvořit Vyrovnávání zatížení pomocí Správce prostředků Azure](load-balancer-get-started-ilb-arm-ps.md)
- [Azure rozhraní příkazového řádku pomocí řízení Azure zdrojů](../xplat-cli-azure-resource-manager.md)
- [Služba REST API Vyrovnávání zatížení](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## <a name="next-steps"></a>Další kroky

Můžete taky [začít vytvářet internetové Vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) a konfigurace jaký druh [režimu rozdělení](load-balancer-distribution-mode.md) pro konkrétní zatížení vyrovnávání sítě přenosy chování.

Naučte se spravovat [Nastavení časového limitu nečinnosti TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md) Co je důležité aplikace potřebujete-li zachovat spojení aktivní serverů za vyrovnávání zatížení.
