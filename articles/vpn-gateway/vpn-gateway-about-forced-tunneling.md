<properties 
   pageTitle="Konfigurace vynuceného tunneling pro připojení k webu pomocí klasické nasazení modelu | Microsoft Azure"
   description="Jak přesměrovat nebo "Vynutit všechny přenosy Internet vazbou zpátky do místního pracoviště."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Konfigurace vynuceného tunneling pomocí klasické nasazení modelu

> [AZURE.SELECTOR]
- [Prostředí PowerShell – klasické](vpn-gateway-about-forced-tunneling.md)
- [Prostředí PowerShell – správce](vpn-gateway-forced-tunneling-rm.md)

Vynuceného tunneling umožňuje přesměrovat nebo "platnost" všechny přenosy vazbou na Internetu zpět do místního pracoviště přes VPN k webu tunelem pro kontrolu a auditování. Toto je důležité zabezpečení požadavku na většině podnikové zásady. 

Bez vynuceného tunneling vazbou internetových adres VMs v Azure vždy prochází z Azure síťovou infrastrukturu přímo se k Internetu, bez možnost umožňuje zkontrolovat nebo auditování přenos. Neoprávněnému přístupu Internet může vést k informacím a další typy porušení zabezpečení.

Tento článek vás provede jednotlivými konfigurace vynucená tunneling virtuálních sítí vytvořené pomocí klasické nasazení modelu. 

**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Modely nasazení a nástroje pro vynuceného tunneling**

Vynuceného tunelového připojení je možné konfigurovat pro klasické nasazení modelu a nasazení modelu správce prostředků. V tématu Další informace v následující tabulce. Jakmile nové články, nové modely nasazení a další nástroje dostupné pro tuto konfiguraci aktualizujeme v této tabulce. Při článek neexistuje, jsme propojit přímo ji z tabulky.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Požadavky a co byste měli zvážit

Vynuceného tunneling v Azure nakonfigurovaný prostřednictvím virtuální sítě uživatelem definované směruje (UDR). Přesměrovat komunikaci k místnímu webu vyjádřený trasu výchozí na brána Azure VPN. V následující části jsou uvedeny aktuální omezení velikosti na směrování tabulky a trasy virtuální sítě Azure:


-  Každý virtuální podsítě přiřazenou tabulkou směrování předdefinované systému. Systém směrování tabulka obsahuje následující tři skupiny postupů:

    - **Místní VNet přesměrovává:** Přímo do cíle VMs ve stejné síti virtuální
    
    - **Na místní trasách:** K bráně Azure VPN
    
    - **Výchozí směrování:** Přímo na Internetu. Dojde ke ztrátě pakety určené soukromé IP adresy nevztahuje předchozí dvě trasy.


-  Verze směruje definované uživatelem můžete vytvořit tabulku směrování přidání výchozí trasy a přidružit tabulce směrování k VNet subnet(s) povolit vynuceného tunneling na tyto podsítě.

- Budete muset nastavit "výchozí web" mezi místních webů křížově místní připojení k virtuální sítě.

- Vynuceného tunneling musí být přidružené VNet, který má dynamické směrování VPN brány (není statické brány).
 
- ExpressRoute vynucená tunneling není nakonfigurován prostřednictvím tento mechanismus, ale místo toho je povolené reklamní výchozí směrování prostřednictvím ExpressRoute BGP peering relace. Naleznete v [Dokumentaci ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) pro další informace.



## <a name="configuration-overview"></a>Přehled konfigurace

V následujícím příkladu Frontend není vynucená podsítě vytvořena. Úloh v podsítě Frontend můžete dál přijmout a odpovědět na požadavků zákazníků z Internetu přímo. Vynucená polovině osy a back-end podsítí prostřednictvím těchto propojení. Odchozí připojení k Internetu tyto dva podsítě budou vynucené nebo přesměrovat zpátky do místního webu pomocí některého tunelů S2S VPN.

Díky zakázání a zkontrolovat přístup k Internetu z virtuálních počítačích a cloudových služeb v Azure, můžete nadále povolit architektuře vícevrstvého služby povinné. Můžete taky použít vynuceného tunneling celý virtuální sítím, pokud v síti virtuální jsou bez internetového úloh.


![Vynucená Tunneling](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Než začnete

Zkontrolujte, jestli máte následující položky před zahájením konfigurace.

- Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

- Nakonfigurované virtuální sítě. 

- Nejnovější verze rutiny prostředí PowerShell Azure. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.


## <a name="configure-forced-tunneling"></a>Konfigurace vynuceného tunneling

Následující postup vám pomůže určit vynuceného tunneling pro virtuální sítě. Postup konfigurace odpovídají konfiguračního souboru VNet sítě.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

V tomto příkladu virtuální sítě "MultiTier VNet" má tři podsítě: *Frontend*, *Midtier*a *back-end* podsítí s připojením k čtyři křížového místní: *DefaultSiteHQ*a tři *poboček*. 

Kroky *DefaultSiteHQ* nastavit jako výchozí web připojení vynucená tunneling a konfigurace Midtier a back-end podsítí používat vynucená tunneling.


1. Vytvořte tabulku směrování. Použijte následující rutinu k vytvoření nové tabulky směrování.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Přidání výchozí trasy do směrovací tabulky. 

    Následující příklad sečte výchozí trasy směrování tabulku vytvořenou v části Krok 1. Všimněte si, že podporuje pouze směrování je cílový předpona "0.0.0.0/0" k "VPNGateway" přesměrování.
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Přidružení směrovací tabulky podsítí. 

    Po vytvoření směrování tabulky a postup přidali, slouží k přidání nebo přidružení tabulce směrování adres podsítí VNet v následujícím příkladu. V příkladu přidá tabulku směrování "MyRouteTable" Midtier a back-end podsítí VNet MultiTier-VNet.

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Přiřazení výchozího webu pro vynucená tunneling. 

    V předchozím kroku ukázky skriptů rutina vytvořené tabulce směrování a související tabulce směrování dvou podsítí VNet. Zbývající krokem je jako výchozí web nebo tunelem, vyberte místní web mezi více webů připojení virtuální sítě.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>Další rutiny prostředí PowerShell

### <a name="to-delete-a-route-table"></a>Chcete-li odstranit tabulku směrování

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>Chcete-li zobrazit tabulku směrování

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>Chcete-li odstranit trasu z tabulky směrování

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>Chcete-li odebrat podsítě směrování

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Chcete-li zobrazit směrování tabulky přidružené podsítě
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Chcete-li odebrat výchozího webu z jedné brány VNet VPN

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>






