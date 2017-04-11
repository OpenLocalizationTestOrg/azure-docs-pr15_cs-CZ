|                              | **Bod webu**                                                                            | **Na webu**                                                                                        | **ExpressRoute**                                                                                                                     |
|------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Podporované služby Azure** | Cloudovými službami a virtuálních počítačích                                                          | Cloudovými službami a virtuálních počítačích                                                                     | [Seznam služeb](../expressroute/expressroute-faqs.md#supported-services)                                                       |
| **Typické šířek pásma**       | Obvykle < 100 MB / aggregate                                                               | Obvykle < 100 MB / aggregate                                                                          | 50 MB / 100 MB /, 200 MB /, 500 MB /, 1 GB/s, 2 GB/s, 5 s technologií, 10 GB/s                                                               |
| **Protokoly podporované**      | Secure Sockets Tunneling protokol (SSTP)                                                     | IPsec                                                | Přímé připojení přes virtuální místní sítě, technologií pro server je NSP virtuální privátní sítě (MPLS, VPLS,...)                                                                                                    |
| **Směrování**                  | RouteBased (dynamické)                                                                        | Podporujeme PolicyBased (statické směrování) a RouteBased (dynamic směrování VPN)                 | BGP                                                                                                                                  |
| **Připojení odolnost proti chybám**    | aktivní pasivní                                                                               | aktivní pasivní                                                                                          | aktivní                                                                                                                        |
| **Typické použití případu**         | Vytváření prototypů, vývoj / testování / laboratorní scénáře cloudovými službami a virtuálních počítačích              | Odchylka / testování / laboratoři scénářů a malé měřítko výrobní úloh pro cloudovými službami a virtuálních počítačích | Přístup ke všem Azure služby, ověřený seznam, podnikových a velice důležité úloh, zálohování, velký Data, Azure jako web DR |
| **SLA**                      | [SLA](https://azure.microsoft.com/support/legal/sla/)                                        | [SLA](https://azure.microsoft.com/support/legal/sla/)                                                   | [SLA](https://azure.microsoft.com/support/legal/sla/)                                                                                |
| **Ceny**                  | [Ceny](https://azure.microsoft.com/pricing/details/vpn-gateway/)                           | [Ceny](https://azure.microsoft.com/pricing/details/vpn-gateway/)                                      | [Ceny](https://azure.microsoft.com/pricing/details/expressroute/)                                                                   |
| **Technická si přečtěte následující dokumentaci**  | [Si přečtěte následující dokumentaci brány VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) | [Si přečtěte následující dokumentaci brány VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/)            | [ExpressRoute si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/expressroute/)                                        |
| **NEJČASTĚJŠÍ DOTAZY**                     | [Brána VPN časté otázky](vpn-gateway-vpn-faq.md)                                                    | [Brána VPN časté otázky](vpn-gateway-vpn-faq.md)                                                               | [ExpressRoute časté otázky](../expressroute/expressroute-faqs.md)                                                                             |