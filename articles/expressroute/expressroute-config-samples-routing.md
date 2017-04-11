<properties
   pageTitle="ExpressRoute zákazníka směrovači konfigurace ukázky | Microsoft Azure"
   description="Tato stránka obsahuje ukázky konfigurace směrovači pro Cisco a Juniper směrovači."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Ukázky směrovači konfigurace nastavení a správě směrování

Tato stránka obsahuje rozhraní a směrování ukázky konfigurace pro Cisco IOS XE a Juniper MX směrovači řady. Tyto by měla být ukázky pouze pokyny a nesmí být použita jako. Můžete pracovat s dodavatele a přichází s příslušnou konfigurace sítě. 

>[AZURE.IMPORTANT] Ukázky na této stránce by měla být čistě pokyny. Musí pracovat s týmem prodejní / technické dodavatele a sociální sítě týmu přichází s příslušnou konfigurace vlastním potřebám. Společnost Microsoft nebude podporovat problémům souvisejícím s konfigurací uvedený na této stránce. Podpora problémy musíte kontaktovat dodavatele zařízení.

Následující ukázky konfigurace směrovači platí pro všechny peerings. Podrobné informace o směrování kontrolujte [ExpressRoute peerings](expressroute-circuit-peerings.md) a [směrování požadavky ExpressRoute](expressroute-routing.md) .

## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS XE na základě směrovači

Příklady v této části platí pro všechny směrovači řady s operačním systémem IOS XE.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. konfigurace rozhraní a dílčí

Budete potřebovat sub rozhraní na prozkoumávání v každé směrovači přihlášení do Microsoftu. Rozhraní sub můžete označena VLAN ID nebo skládaný dvojice VLAN ID a IP adresy.

#### <a name="dot1q-interface-definition"></a>Definice rozhraní Dot1Q

Tato ukázka poskytuje definice dílčí rozhraní pro dílčí rozhraní pomocí jednoho ID VLAN. VLAN ID je jedinečný na prozkoumávání. Poslední osmičkové adresu IPv4 vždycky liché číslo.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>Definice rozhraní QinQ

Tato ukázka poskytuje definice dílčí rozhraní pro dílčí rozhraní pomocí dvou ID VLAN. Vnější VLAN ID (s štítků), pokud použili zůstane stejná napříč všechny peerings. Vnitřní VLAN ID (c značka) je jedinečný na prozkoumávání. Poslední osmičkové adresu IPv4 vždycky liché číslo.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. nastavení eBGP relací

Musíte nastavit BGP relace s Microsoftem pro každý prozkoumávání. Následující ukázce umožňuje nastavit relaci BGP u Microsoftu. Pokud IPv4 adresu, kterou jste použili pro rozhraní sub a.b.c.d, bude IP adresa sousedního BGP (Microsoft) a.b.c.d+1. Poslední osmičkové adresy IPv4 sousední BGP vždycky sudé číslo.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. nastavení předpony inzerovanému BGP relace

Můžete nakonfigurovat směrovači k oznámení, vyberte předpony společnosti Microsoft. Můžete to udělat pomocí následující ukázce.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. mapy směrování

Použití směrování mapy a předponu seznamu filtr předpony rozšíří do sítě. Následující ukázce slouží k provedení úkolu. Zajištění instalace seznamy příslušný prefix.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Směrovači řady Juniper MX 

Příklady v této části požádat o libovolnými směrovači řady Juniper MX.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. konfigurace rozhraní a dílčí

#### <a name="dot1q-interface-definition"></a>Definice rozhraní Dot1Q

Tato ukázka poskytuje definice dílčí rozhraní pro dílčí rozhraní pomocí jednoho ID VLAN. VLAN ID je jedinečný na prozkoumávání. Poslední osmičkové adresu IPv4 vždycky liché číslo.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>Definice rozhraní QinQ

Tato ukázka poskytuje definice dílčí rozhraní pro dílčí rozhraní pomocí dvou ID VLAN. Vnější VLAN ID (s štítků), pokud použili zůstane stejná napříč všechny peerings. Vnitřní VLAN ID (c značka) je jedinečný na prozkoumávání. Poslední osmičkové adresu IPv4 vždycky liché číslo.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. nastavení eBGP relací

Musíte nastavit BGP relace s Microsoftem pro každý prozkoumávání. Následující ukázce umožňuje nastavit relaci BGP u Microsoftu. Pokud IPv4 adresu, kterou jste použili pro rozhraní sub a.b.c.d, bude IP adresa sousedního BGP (Microsoft) a.b.c.d+1. Poslední osmičkové adresy IPv4 sousední BGP vždycky sudé číslo.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. nastavení předpony inzerovanému BGP relace

Můžete nakonfigurovat směrovači k oznámení, vyberte předpony společnosti Microsoft. Můžete to udělat pomocí následující ukázce.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. mapy směrování

Použití směrování mapy a předponu seznamu filtr předpony rozšíří do sítě. Následující ukázce slouží k provedení úkolu. Zajištění instalace seznamy příslušný prefix.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Další kroky

V tématu [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md) pro další podrobnosti.
