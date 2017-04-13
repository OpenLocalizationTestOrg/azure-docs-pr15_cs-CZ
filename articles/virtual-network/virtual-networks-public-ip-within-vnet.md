<properties 
   pageTitle="Jak pomocí veřejné adresy IP v síti virtuální"
   description="Naučíte se konfigurovat virtuální sítě používat veřejné IP adres"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="public-ip-address-space-in-a-virtual-network-vnet"></a>Veřejné IP adresní prostor v síti virtuální (VNet)

Virtuální sítí (VNets) můžete mezerami veřejných a privátních (blok adresy RFC 1918) IP adresu. Když přidáte veřejné rozsah IP adres, bude považována jako součást soukromé VNet IP adresu pole, která je dostupná v rámci VNet propojené VNets a od místního pracoviště.

Následujícím obrázku je VNet s mezerami veřejných a privátních IP adresa.

![Koncepční veřejnou IP](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## <a name="how-do-i-add-a-public-ip-address-range"></a>Jak přidám veřejné rozsah IP adres?

Přidání veřejné rozsah IP adres stejným způsobem jako přidejte soukromé rozsah IP adres; pomocí některého z *netcfg* souboru nebo přidáním konfigurace [Azure portálu](http://portal.azure.com). Při vytváření vašeho VNet nebo můžete vrátit a přidat ho později můžete přidat veřejné rozsah IP adres. Následující příklad ukazuje veřejných a privátních IP adresu mezery konfigurovat stejné VNet.

![Veřejnou IP adresu portálu](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## <a name="are-there-any-limitations"></a>Je nějaké omezení?

Existuje několik rozsahy IP adres, které nejsou povoleny:

- 224.0.0.0/4 (Multicast)

- 255.255.255.255/32 (vysílání)

- 127.0.0.0/8 (smyčky)

- 169.254.0.0/16 (místní)

- 168.63.129.16/32 interní DNS)

## <a name="next-steps"></a>Další kroky

[Jak spravovat servery DNS používané VNet](virtual-networks-manage-dns-in-vnet.md)