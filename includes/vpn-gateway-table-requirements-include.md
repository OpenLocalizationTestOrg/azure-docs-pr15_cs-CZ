Následující tabulka uvádí požadavky pro PolicyBased a RouteBased VPN brány. V této tabulce platí pro správce zdrojů a modelů klasické nasazení. Klasický modelu PolicyBased VPN bran se stejně jako statické brány a na základě směrování brány se stejně jako při dynamické brány.


|   | **Brána VPN PolicyBased Basic** | **Brána VPN RouteBased Basic** | **Standardní RouteBased VPN brány**   | **RouteBased vysokým výkonu sítě VPN brány** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Připojení k webu (S2S)**  | Konfigurace PolicyBased VPN        | Konfigurace RouteBased VPN  | Konfigurace RouteBased VPN     | Konfigurace RouteBased VPN    |
| Připojení **bodu-Server (P2S**)      | Nepodporovaná funkce   | Podporované (může existovat společně s S2S)  | Podporované (může existovat společně s S2S)  | Podporované (může existovat společně s S2S) |
| **Metody ověřování**                 |    Předem sdílený klíč  | Předem sdílený klíč pro připojení k S2S, certifikáty pro P2S připojení | Předem sdílený klíč pro připojení k S2S, certifikáty pro P2S připojení | Předem sdílený klíč pro připojení k S2S, certifikáty pro P2S připojení |
| **Maximální počet S2S připojení**       | 1                              | 10                                                                    | 10                                | 30                               |
| **Maximální počet P2S připojení**       | Nepodporovaná funkce                  | 128                                                                   | 128                               | 128                              |
|**Podpora aktivní směrování (BGP)**           | Nepodporovaná funkce                  | Nepodporovaná funkce                                                         | Podporované                     | Podporované                   |
 
