<properties 
   pageTitle="Připojení k víc webů pomocí Brána VPN a prostředí PowerShell virtuální sítě | Microsoft Azure"
   description="Tento článek vás provede jednotlivými připojení víc webů místní místního k síti virtuální pomocí VPN bránu pro klasické nasazení modelu."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Přidání připojení k webu k VNet pomocí existujícího připojení VPN brány

> [AZURE.SELECTOR]
- [Správce prostředků - portálu](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klasický - prostředí PowerShell](vpn-gateway-multi-site.md)

Tento článek vás provede online přes přidání spojení na webu (S2S) VPN brány, který má existující připojení. Tohoto typu připojení je někdy označovány jako "více webu" konfigurace. 

Tento článek se týká virtuální sítě vytvořené pomocí klasické nasazení modelu (označovaná taky jako správce služby správy). Tyto kroky, nebudou mít vliv na existujících konfigurace připojení ExpressRoute/server-server. Informace o existujících připojení najdete v tématu [ExpressRoute/S2S existujících spojení](../expressroute/expressroute-howto-coexist-classic.md) .

### <a name="deployment-models-and-methods"></a>Modely nasazení a metody

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

V této tabulce aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci. Při článek neexistuje, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>O připojení

Víc místních webů můžete připojit k jedné virtuální sítě. To je zvlášť pro vytváření hybridních cloudové řešení. Vytvoření více webů připojení k bráně Azure virtuální sítě je velmi podobné vytváření jiných připojení k webu. Ve skutečnosti můžete použít existující brána Azure VPN, dokud brána je dynamické (směrování na základě).

Pokud už máte statické brány připojení k síti virtuální, můžete změnit typ brány na dynamické aniž by musel znovu vytvořit virtuální sítě za účelem pokryly více webu. Před změnou směrování typu, zkontrolujte, že bránu VPN místní podporuje na základě směrování konfigurace VPN. 

![diagram více webů] (./media/vpn-gateway-multi-site/multisite.png "více webů")

## <a name="points-to-consider"></a>Ukazatel na zvážit

**Nebude moct používat portál klasické Azure ke změnám tento virtuální sítě.** Pro tuto verzi musíte provést změny konfiguračního souboru síti místo na portálu Azure klasické. Pokud uděláte změny v portálu klasické Azure, budete přepsání nastavení odkaz na více web pro tento virtuální sítě. 

Si měli projděte poměrně pomocí konfiguračního souboru sítě postupně po dokončení procesu více webu. Ale pokud máte víc lidem práce ke konfiguraci sítě, musíte zajistit, aby všichni věděli, vidí informace o toto omezení. To neznamená, že nelze používat portál klasické Azure vůbec. Můžete ho použít pro všechny ostatní kromě změn konfigurace této konkrétní virtuální sítě.

## <a name="before-you-begin"></a>Než začnete

Než začnete konfigurace, zkontrolujte, jestli máte takto:

- Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

- Kompatibilní VPN hardware pro každou místní umístění. Zkontrolujte [O zařízení VPN virtuální připojení k síti](vpn-gateway-about-vpn-devices.md) ověření, zda zařízení, které chcete použít něco níž je známo, aby byl kompatibilní.

- Externě vystaveného veřejné IPv4 IP adresa pro každé VPN zařízení. IP adresa nebyla nalezena za službou NAT. Toto je požadavek.

- Musíte nainstalovat nejnovější verzi rutiny prostředí PowerShell Azure. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.

- Někoho, kdo je znalosti v konfigurace hardwaru, virtuální privátní sítě. Nebudete moct používat VPN skripty automaticky generované z portálu Microsoft Azure klasické ke konfiguraci sítě VPN zařízení. To znamená, že budete muset mít silného znalost jak konfigurovat zařízení virtuální privátní sítě nebo práce s někým, kdo má.

- Rozsahy IP adres, které chcete použít pro síť virtuální (pokud už jste ještě nevytvořili jednu). 

- Rozsahy IP adres pro každou místní síti webů, které jste budete připojení k. Musíte, abyste měli jistotu, že na IP adresu oblasti pro každou místní síti webů, které se chcete připojit k nepřekrývaly. V opačném portálu klasické Azure nebo rozhraní REST API odmítne konfigurace odesílaných. 

    Například pokud máte dvě místní síti weby obsahují 10.2.3.0/24 rozsah IP adresa a máte balík s cílovou adresu 10.2.3.3 Azure nebudou vědět, webů, jejichž chcete odeslat balíček, protože se stejnými rozsahy adres. Chcete-li předejít problémům s směrování, Azure neumožňuje nahrát konfigurační soubor, který má překrývajícími se oblastmi.



## <a name="1-create-a-site-to-site-vpn"></a>1. Vytvořte VPN na webu

Pokud už máte VPN na webu pomocí dynamického směrování brány skvěle! Můžete přejít k [Export nastavení konfigurace virtuální sítě](#export). Pokud ne, postupujte takto:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Pokud už máte web webu virtuální sítě, ale má statické směrování brány (na základě zásad):

1. Změna typu brány na dynamické směrování. Více webů VPN vyžaduje dynamické směrování brány (označovaná taky jako směrování založené). Pokud chcete změnit typ brány, musíte nejdřív existující bránu odstranit, a pak vytvořte nový účet. Pokyny najdete v tématu [jak změnit typ směrování VPN brány](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  

2. Konfigurace Nová brána a vytvořte tunelem VPN. Pokyny najdete v tématu [Konfigurace brány VPN na portálu klasické Azure](vpn-gateway-configure-vpn-gateway-mp.md). Nejdřív přejděte povahy brány dynamické směrování. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Pokud nemáte na webu virtuální sítě:

1. Vytvoření webu na webu virtuální sítí tyto pokyny: [vytvořit virtuální sítě s připojení VPN k webu na portálu klasické Azure](vpn-gateway-site-to-site-create.md).  

2. Konfigurace dynamické směrování brány pomocí těchto pokynů: [Konfigurace brány VPN](vpn-gateway-configure-vpn-gateway-mp.md). Je potřeba vybrat **dynamické směrování** pro příslušný typ brány.

## <a name="export"></a>2. exportovat do souboru konfigurace sítě 

Exportujte souboru konfigurace sítě. Soubor, který exportujete se použijí k nakonfiguroval nastavení nové více webu. Pokud potřebujete pokyny, jak se dají exportovat do souboru, naleznete v části v následujícím článku: [Vytvoření VNet pomocí konfiguračního souboru sítě portálu Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. otevřít soubor konfigurace sítě

Otevřete konfiguračního souboru sítě, který jste stáhli v posledním kroku. Použití editoru xml, který se vám líbí. Soubor by měl vypadat takto:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. přidat odkazy na více webů

Při přidání nebo odebrání webu referenční informace budete provedené změny konfigurace ConnectionsToLocalNetwork/LocalNetworkSiteRef. Přidání nového aktivace odkaz místní webu Azure k vytvoření nové tunelem. V následujícím příkladu je konfigurace sítě pro připojení k jedné webu. Po dokončení změn, uložte soubor.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. importovat soubor konfigurace sítě

Importujte souboru konfigurace sítě. Při importu tohoto souboru se změnami se přidá nová tunelů. Tunelů budou používat dynamické brány, který jste vytvořili. Pokud potřebujete návod k importu souboru, naleznete v části v následujícím článku: [Vytvoření VNet pomocí konfiguračního souboru sítě portálu Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. stáhnout klíče

Po přidání nového tunelů získáte pomocí rutiny prostředí PowerShell `Get-AzureVNetGatewayKey` získat předem sdílené klíče IPsec/IKE pro každou tunelem.

Příklad:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Pokud chcete, můžete také rozhraní REST API *Získat virtuální sítě sdílené klíče brány* získat předem sdílené klíče.

## <a name="7-verify-your-connections"></a>7. připojení k ověření

Kontrola stavu tunelem více webu. Po stažení klávesy pro každou tunelem, je vhodné ověřit připojení. Použití `Get-AzureVnetConnection` zobrazíte seznam tunelů virtuální sítě, jak je vidět v následujícím příkladu. VNet1 stejný název VNet.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Další kroky

Další informace o bran VPN, najdete v článku [O bran VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).

