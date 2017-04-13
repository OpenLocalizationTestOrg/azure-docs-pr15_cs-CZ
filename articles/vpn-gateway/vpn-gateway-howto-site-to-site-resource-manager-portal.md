<properties
   pageTitle="Vytvořit virtuální sítě s připojení VPN k webu pomocí Správce prostředků Azure a portálu Azure | Microsoft Azure"
   description="Jak vytvořit VNet pomocí Správce prostředků nasazení modelu a připojte ji k vaší místní místní síti pomocí připojení S2S VPN brány."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Vytvoření VNet s připojení k webu pomocí portálu Azure

> [AZURE.SELECTOR]
- [Správce prostředků - Azure portálu](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Správce prostředků - prostředí PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasický – klasické portálu](vpn-gateway-site-to-site-create.md)


Tento článek vás provede vytváření virtuálních sítí a připojení VPN k webu brány k sítí místního nasazení modelu správce prostředků Azure a portálu Azure. Připojení k webu se dá použít pro více místní a hybridní konfigurace.

![Diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Modely nasazení a metody pro připojení k webu

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Následující tabulka zobrazuje modelů momentálně neexistuje nasazení a metody konfigurace webů serveru. Při článek s kroky konfigurace je k dispozici, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Další konfigurace 

Pokud se chcete připojit VNets dohromady, ale nejsou vytvoření připojení k místního pracoviště, přečtěte si téma [Konfigurace VNet VNet připojení](vpn-gateway-vnet-vnet-rm-ps.md). Pokud chcete přidat připojení k webu VNet kterém už je připojení, najdete v článku [Přidání S2S připojení k VNet pomocí existujícího připojení VPN brány](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Než začnete

Zkontrolujte, jestli máte následující položky před zahájením konfigurace:

- Kompatibilní zařízení VPN a někoho, kdo je možné ji nakonfigurovat. Přečtěte si [téma VPN zařízení](vpn-gateway-about-vpn-devices.md). Pokud nejste obeznámení s konfigurací zařízení virtuální privátní sítě nebo neznají IP adresu oblastech umístěny v konfiguraci sítě vaší místní, budete muset koordinaci s někým, kdo může poskytnout tyto údaje za vás.

- Externě vystaveného veřejnou IP adresa pro zařízení s VPN. Tato IP adresa nebyla nalezena za službou NAT.
    
- Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>Konfigurace ukázkové hodnoty pro tento cvičení


Pokud chcete použít tento postup jako cvičení, můžete použít ukázkové konfigurace hodnoty:

- **VNet název:** TestVNet1
- **Adresa místa:** 10.11.0.0/16 a 10.12.0.0/16
- **Podsítí:**
    - FrontEnd: 10.11.0.0/24
    - Back-end: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- **Pole Skupina zdroje:** TestRG1
- **Umístění:** Východní USA
- **DNS Server:** 8.8.8.8
- **Název brány:** VNet1GW
- **Veřejnou IP:** VNet1GWIP
- **Typ VPN:** Na základě směrování
- **Typ připojení:** Webu (IPsec)
- **Typu brána:** VIRTUÁLNÍ PRIVÁTNÍ SÍTĚ
- **Název místní síti brány:** Web2
- **Název připojení:** VNet1toSite2


## <a name="CreatVNet"></a>1. vytvořit virtuální sítě 

Pokud už máte VNet, ověřte, jestli jsou kompatibilní s návrhu brány VPN nastavení. Věnujte zvláštní pozornost podsítí, které může překrývat v jiných sítích. Pokud máte překrývající se podsítí, připojíte se nebude fungovat správně. Pokud vaše VNet nakonfigurovaný na správné nastavení, můžete začít kroků uvedených v části [Specify serveru DNS](#dns) .

### <a name="to-create-a-virtual-network"></a>Chcete-li vytvořit virtuální sítě

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. přidat další adresní prostor a podsítí

Můžete přiřadit další adresní prostor a podsítí vaší VNet po jeho vytvoření.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. Zadejte DNS server

### <a name="to-specify-a-dns-server"></a>Chcete-li zadat serveru DNS

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. Vytvoření podsítě brány

Před připojením virtuální sítě brány, musíte nejdřív vytvořit podsítě brány virtuální sítě, ke kterému chcete připojit. Pokud je to možné je vhodné vytvořit podsítě brány pomocí CIDR blokem /28 nebo /27 k poskytují dostatek IP adresy tak, aby zahrnoval další konfiguraci budoucí požadavky.

Pokud vytváříte tuto konfiguraci jako cvičení, podívejte se do tyto [hodnoty](#values) při vytváření vaší podsítě brány.

### <a name="to-create-a-gateway-subnet"></a>Vytvoření podsítě brány


[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Vytvoření brány pro virtuální sítě

Pokud vytváříte tuto konfiguraci jako cvičení, můžete odkázat na [ukázkové konfigurace hodnoty](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Vytvoření brány virtuální sítě

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. Vytvoření brány pro místní síti

Místní síti brány odkazuje místního pracoviště. Název místní síti brány prodloužením doby Azure může označovat ho. 

Pokud vytváříte tuto konfiguraci jako cvičení, můžete odkázat na [ukázkové konfigurace hodnoty](#values).

### <a name="to-create-a-local-network-gateway"></a>Vytvoření brány místní síti

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. zařízení VPN konfigurace

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Vytvořte připojení VPN k webu

Vytvoření připojení VPN k webu mezi brány virtuální sítě a zařízení VPN. Ujistěte se, že nahradit hodnoty vlastní. Sdílený klíč se musí shodovat hodnotu, kterou jste použili pro konfiguraci VPN zařízení. 

Před zahájením v této části, ověřte, že virtuální sítě brány a místní síti bran máte hotový. Pokud vytváříte tuto konfiguraci jako cvičení, podívejte se do tyto [hodnoty](#values) při vytváření připojení.

### <a name="to-create-the-vpn-connection"></a>Vytvoření připojení k síti VPN


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. připojení k síti VPN ověření

Připojení k síti VPN na portálu nebo pomocí prostředí PowerShell můžete ověřit.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Další kroky

- Po dokončení připojení lze virtuálních počítačích vašich virtuální sítí. Najdete v článku virtuálních počítačích [cesta výuky](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) pro další informace.

- Informace o BGP najdete v tématu [Přehled BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci BGP](vpn-gateway-bgp-resource-manager-ps.md).
