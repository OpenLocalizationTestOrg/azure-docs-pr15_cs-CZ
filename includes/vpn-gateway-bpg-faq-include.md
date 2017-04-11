### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>Je BGP podporována u všech SKU brány VPN Azure?

Ne, je podporována BGP Azure **standardních** a **vysoce** VPN brány. **Základní** SKU není podporována.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Mám používat BGP se Azure Policy-Based VPN bran?

Ne, BGP je podporována pouze na základě směrování VPN bran.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Můžete použít soukromé avíza expedice zboží (samostatné systém čísla)?

Ano, můžete použít vlastní avíza expedice zboží veřejné nebo soukromé avíza expedice zboží pro místní sítě a Azure virtuální sítě.

#### <a name="are-there-asns-reserved-by-azure"></a>Nejdou nějaké avíza expedice zboží vyhrazeno Azure?

Ano, následující avíza expedice zboží jsou vyhrazeno Azure pro interní i externí peerings:

- Veřejné avíza expedice zboží: 8075 8076, 12076
- Avíza expedice zboží soukromé: 65515, 65517, 65518, 65519 65520

Tyto avíza expedice zboží nelze zadat pro vaše na místní počítače v síti VPN při připojování k Azure VPN brány.

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Můžete použít stejné ASN pro místní sítě VPN a Azure VNets?

Ne, musíte přiřadit různé avíza expedice zboží mezi místní sítě a Azure VNets Pokud se připojujete je společně s BGP. Azure bran VPN mít výchozí ASN 65515 přiřazen, ať už BGP aktivované řešení není pro více místní připojení. Přepsat tuto výchozí přiřazením různých ASN při vytváření Brána VPN nebo změnit ASN po vytvoření brány. Je třeba váš avíza expedice zboží místní přiřadit odpovídající Azure místní síti bran.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Pro mě jaké předpony adres vydává brány Azure VPN?

Azure brány VPN vydává následující postupy k místním zařízením BGP:

- Předpony adres VNet
- Předpony adres pro každou místní síti brány připojené k brány sítě VPN Azure
- Trasy získané z jiných BGP peering relací připojení k bráně Azure VPN **kromě výchozího postupu nebo směruje překryté předponou všechny VNet**.

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Můžete inzerce výchozí směrování (0.0.0.0/0) do brány Azure VPN?

V této chvíli ne.

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Můžete inzerce přesné předpony jako Moje předpony virtuální sítě?

Ne, reklamní stejné předpony jako takové virtuální sítě předpony adres budou blokované nebo filtrované podle platformu Azure. Můžete však přístupné předpony, která je nadřazený máte uvnitř virtuální sítě. Například virtuální sítě můžou použít místo 10.10.0.0/16 adresa a může inzerce 10.0.0.0/8.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Můžete použít BGP s připojením k VNet VNet?

Ano, můžete provádět BGP křížově místní připojení a VNet VNet připojení.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Můžu kombinovat BGP s připojením k jiné BGP Moje bran Azure VPN?

Ano, můžete kombinovat BGP i bez BGP připojení pro stejné bráně Azure VPN.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Znamená směrování Azure VPN brány podpory BGP při přenosu šifrovaná?

Ano, směrování přenosu BGP je podporováno, s tím rozdílem, že bude Azure VPN brány **není** uvedena výchozí trasy do jiných BGP kolegové. Povolení směrování přenosu napříč několika bran Azure VPN, musíte povolit BGP u všech přechodné připojení VNet VNet.

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>Můžete mít víc tunelů mezi brána Azure VPN a v místní síti?

Ano, můžete vytvořit více tunelů S2S VPN mezi brány Azure VPN a v místní síti. Všimněte si, že tyto tunelů se započítávají celkový počet tunelů Azure VPN bran. Například pokud máte dvě nadbytečné tunelů mezi bránu Azure VPN a jeden v místní síti, budou spotřebovávat 2 tunelů z celkové kvóty pro bránu Azure VPN (10 standardu) a čísla 30 vysoce.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Můžete mít víc tunelů mezi dvěma VNets Azure s BGP?

Ne, nejsou podporované nadbytečné tunelů mezi dvěma virtuální sítě.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Můžu používat BGP S2S privátní v konfiguraci vzájemnou spolupráci ExpressRoute/S2S VPN?

V této chvíli ne.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Pro IP mezi dvěma účastníky BGP jakou adresu používá brána Azure VPN?

Brána Azure VPN přidělí jednu IP adresu z oblasti GatewaySubnet definované pro virtuální sítě. Ve výchozím nastavení je druhý poslední adresu dané oblasti. Například pokud váš GatewaySubnet 10.12.255.0/27, od 10.12.255.0 10.12.255.31, BGP mezi dvěma účastníky IP adresu na Azure VPN brány bude 10.12.255.30. Tyto informace najdete při vypsat informace brány Azure VPN.

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Jaké jsou požadavky pro BGP mezi dvěma účastníky IP adresách, které na svém zařízení virtuální privátní sítě?

Místní BGP adresa druhé strany **Nesmí** být stejné jako veřejnou IP adresu VPN zařízení. Použití různých IP adresu na zařízení virtuální privátní sítě pro svoji IP BGP mezi dvěma účastníky. Může být adresa přiřazená rozhraní zpětná smyčka na zařízení. Zadejte tuto adresu do pole odpovídající místní brána sítě představující umístění.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Co můžu zadají jako Moje předpony adres pro místní brána sítě při použití BGP?

Azure místní brána sítě určuje počáteční adresy předpony pro místní síti. S BGP, musí přidělit předponou hostitele (/ 32 předpona) BGP mezi dvěma účastníky IP adresy jako adresní prostor pro místní síti. Pokud svoji IP mezi dvěma účastníky BGP 10.52.255.254, je nutné zadat "10.52.255.254/32" jako localNetworkAddressSpace místní brána sítě představující v místní síti. Toto je zajistit, že brána Azure VPN vytvoří relaci BGP tunelem S2S VPN.

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Co by měl můžu přidat na zařízení VPN místní peering relaci BGP?

Na svém zařízení VPN přejdete tunelem IPsec S2S VPN bychom přidat hostitelské směrování Azure BGP mezi dvěma účastníky IP adresy. Například "10.12.255.30" při IP Azure VPN mezi dvěma účastníky bychom přidat hostitele trasa "10.12.255.30" s rozhraním přesměrování odpovídající rozhraní tunelem IPsec na zařízení s VPN.
