|    | **Výkon sítě VPN brány (1)** | **Max IPsec VPN brány propojení (2)** | **Výkon ExpressRoute brány** | **Brána VPN a ExpressRoute být nainstalovány**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Základní SKU (3)(5)**              |  100 MB / | 10                         |  500 MB /                           | Ne   |
| **Standardní SKU (4)(5)**           |  100 MB / | 10                         | 1000 Mb /                           | Ano  |
| **Vysoký výkon SKU (4)**   | 200 MB /  | 30                         | MB / 2000                           | Ano  |

- (1) na výkon sítě VPN je odhad podle rozměrů mezi VNets ve stejné oblasti Azure. Není zaručené výkon křížově místní připojení přes Internet. Je měření maximální možné výkon.
- (2) počet tunelů v nápovědě k RouteBased VPN. Virtuální privátní sítě PolicyBased podporuje pouze jeden tunelem VPN k webu.
- (3) základní SKU nepodporuje BGP.
- (4) virtuální privátní sítě PolicyBased nejsou podporované pro skladové jednotky. Jsou podporované pro základní skladové jednotky pouze.
- (5) připojení aktivní S2S VPN brány nejsou podporované skladové jednotky. Aktivní je podporována pouze SKU vysoce.