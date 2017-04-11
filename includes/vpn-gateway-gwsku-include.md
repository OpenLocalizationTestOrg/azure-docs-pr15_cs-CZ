Při vytváření virtuálních síťové brány se musí zadat brány SKU, který chcete použít. Po výběru vyšší brány SKU více procesorů nebo šířka pásma přiřazených k brány a v důsledku toho brány podporuje vyšší výkon sítě virtuální sítě.

Brána VPN můžete použít následující skladové jednotky:

- Základní
- Standardní
- Vysoce

Pokud vyberete jinou SKU, zvažte následující skutečnosti:

- Pokud chcete použít typu PolicyBased VPN, je nutné použít základní SKU. PolicyBased virtuální privátní sítě (dříve nazývanou statické směrování) nejsou podporovány ve všech SKU.
- BGP není podporována ve základní SKU.
- Brána ExpressRoute VPN být nainstalovány konfigurace nejsou podporovány v základní SKU.
- V vysoce skladové jednotky pouze je možné konfigurovat připojení VPN brány S2S aktivní.
