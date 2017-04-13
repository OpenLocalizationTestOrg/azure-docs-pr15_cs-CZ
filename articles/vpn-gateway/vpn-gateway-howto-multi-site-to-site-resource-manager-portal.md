<properties
   pageTitle="Přidání více brány na webu připojení VPN k virtuální sítě pro nasazení modelu správce prostředků pomocí portálu Azure | Microsoft Azure"
   description="Přidání spojení na více webů S2S VPN brány, který má existující připojení"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>



# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Přidání připojení k webu k VNet pomocí existujícího připojení VPN brány

> [AZURE.SELECTOR]
- [Správce prostředků - portálu](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klasický - prostředí PowerShell](vpn-gateway-multi-site.md)

Tento článek vás provede pomocí portálu Azure přidat webu (S2S) připojení VPN brány, který má existující připojení. Tohoto typu připojení je někdy označovány jako "více webu" konfigurace. 

Tento článek slouží k přidání S2S připojení k VNet, který již obsahuje S2S připojení, čárky webu připojení nebo připojení VNet VNet. Při přidávání připojení platí určitá omezení. Zkontrolujte oddíl [než začnete](#before) v tomto článku můžete ověřit před zahájením konfiguraci. 

Tento článek se týká VNets vytvořené pomocí Správce prostředků nasazení modelu obsahujících bránu RouteBased VPN. Tyto kroky, nebudou mít vliv na existujících konfigurace připojení ExpressRoute/server-server. Informace o existujících připojení najdete v tématu [ExpressRoute/S2S existujících spojení](../expressroute/expressroute-howto-coexist-resource-manager.md) .

### <a name="deployment-models-and-methods"></a>Modely nasazení a metody

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

V této tabulce aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci. Při článek neexistuje, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 


## <a name="before"></a>Než začnete

Zkontrolujte následující položky:

- Nevytváříte existujících připojení ExpressRoute/S2S.
- Máte virtuální sítě, který byl vytvořený pomocí existujícího připojení nasazení modelu správce prostředků.
- Brána virtuálních sítí pro vaše VNet je RouteBased. Pokud máte brána PolicyBased VPN, musíte odstranit Brána virtuální sítě a vytvořit novou bránu VPN jako RoutBased.
- Žádná z rozsahů adres překrývat některého z VNets, která tento VNet připojení k.
- Máte kompatibilní VPN zařízení a někoho, kdo je možné ji nakonfigurovat. Přečtěte si [téma VPN zařízení](vpn-gateway-about-vpn-devices.md). Pokud nejste obeznámení s konfigurací zařízení virtuální privátní sítě nebo neznají IP adresu oblastech umístěny v konfiguraci sítě vaší místní, budete muset koordinaci s někým, kdo může poskytnout tyto údaje za vás.
- Máte externě vystaveného veřejnou IP adresu pro vaše zařízení virtuální privátní sítě. Tato IP adresa nebyla nalezena za službou NAT.


## <a name="part1"></a>Část 1: Konfigurace připojení

1. V prohlížeči přejděte na [portál Azure](http://portal.azure.com) a v případě potřeby Přihlaste se pomocí účtu Azure.
2. Klikněte na **všechny zdroje** a vyhledejte **virtuální sítě bránu** v seznamu zdrojů a klikněte na něj.
3. Na zásuvné **virtuální sítě brány** na **připojení**.

    ![Připojení zásuvné](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>

4. Na zásuvné **připojení** klikněte na tlačítko **+ Přidat**.

    ![Přidání tlačítka připojení](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>

5. Na **Přidat připojení** zásuvné vyplňte následující pole:
    - **Název:** Název, který chcete dát na web, který vytváříte spojení.
    - **Typ připojení:** Vyberte **webu (IPsec)**.

    ![Přidání připojení zásuvné](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>Část 2 – Přidání místní síti brány

1. Klikněte na **místní síti brány** ***Zvolit brány místní síti***. Otevře se zásuvné **zvolit místní síti brány** .

    ![Vyberte místní síti brány](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Klikněte na **vytvořit nový** otevřete zásuvné **vytvořit místní síti brány** .

    ![Vytvoření brány zásuvné místní síti](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>

3. Na zásuvné **vytvořit místní síti brány** vyplňte následující pole:
    - **Název:** Název, který chcete poskytnout ke zdroji brány místní síti.
    - **IP adresa:** Veřejnou IP adresu zařízení VPN na web, který chcete připojit.
    - **Adresa místa:** Adresní prostor, který chcete být směrovány na nový web místní síti.
4. Klikněte na **OK** v zásuvné **vytvořit místní síti brány** k uložení změn.

## <a name="part3"></a>Část 3 – Přidání sdílené klíče a vytvořit připojení

1. Na zásuvné **Přidat připojení** přidáte sdílený klíč, který chcete použít k vytvoření připojení. Můžete získat sdílený klíč z vašeho zařízení virtuální privátní sítě nebo vytvořit si tady a potom i konfiguraci zařízení VPN používat stejný sdílený klíč. Důležité je, že klávesy se přesně shodují.

    ![Sdílený klíč](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. V dolní části zásuvné klikněte na **OK** vytvořte připojení.

## <a name="part4"></a>Část 4: ověření připojení VPN

Připojení k síti VPN na portálu nebo pomocí prostředí PowerShell můžete ověřit.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## <a name="next-steps"></a>Další kroky

- Po dokončení připojení lze virtuálních počítačích vašich virtuální sítí. Najdete v článku virtuálních počítačích [cesta výuky](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) pro další informace.
