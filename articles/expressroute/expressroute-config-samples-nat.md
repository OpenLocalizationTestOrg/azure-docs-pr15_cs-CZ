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

# <a name="router-configuration-samples-to-setup-and-manage-nat"></a>Konfigurace vzorků směrovači nastavení a Správa překladu síťových adres

Tato stránka obsahuje ukázky konfigurace překladu síťových adres pro řadu směrovači Cisco ASA a tomto dokumentu se týkají Juniper. Tyto by měla být vzorky jenom pokyny a nesmí být použita jako. Můžete pracovat s dodavatele a přichází s příslušnou konfigurace sítě. 

>[AZURE.IMPORTANT] Ukázky na této stránce by měla být čistě pokyny. Musí pracovat s týmem prodejní / technické dodavatele a sociální sítě týmu přichází s příslušnou konfigurace vlastním potřebám. Společnost Microsoft nebude podporovat problémům souvisejícím s konfigurací uvedený na této stránce. Podpora problémy musíte kontaktovat dodavatele zařízení.

Následující ukázky konfigurace směrovači platí pro Azure veřejných a Microsoft peerings. Překladu síťových adres nesmí nakonfigurovat pro Azure prozkoumávání soukromé. Prostudujte si [ExpressRoute peerings](expressroute-circuit-peerings.md) a [překladu síťových adres ExpressRoute požadavky](expressroute-nat.md) další podrobnosti.

**Poznámka:** Samostatné fondy překladu síťových adres IP používají pro připojení k Internetu a ExpressRoute. Použití fondu stejné překladu síťových adres IP přes internet a ExpressRoute povede asymetrické směrování a ztrátě připojení.

## <a name="cisco-asa-firewalls"></a>Cisco ASA brány firewall

### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>Konfigurace PAT pro přenos ze sítě zákazníků společnosti Microsoft

    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>
    
    
    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>
    
    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2
    
    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>Konfigurace PAT přenosů od společnosti Microsoft k customer network

#### <a name="interfaces-and-direction"></a>Rozhraní a směru:
    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

#### <a name="configuration"></a>Konfigurace:
Fond překladu síťových adres:

    object network outbound-PAT
        host <NAT-IP>

Cílový Server:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Skupiny objektů IP adres zákazníka

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>
    
    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

Příkazy překladu síťových adres:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Juniper tomto dokumentu se týkají řady směrovači 

### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. Vytvořte nadbytečné Ethernet rozhraní pro cluster

    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. Vytvořte dvě zóny zabezpečení

 - Zóny zabezpečení pro vnitřní síti a Untrust pásma pro externí síti protilehlé směrovači okraje
 - Přiřazení příslušná rozhraní k zóně
 - Povolení služby rozhraní


    zabezpečení {zóny {zóny zabezpečení zabezpečení {hostitele – příchozí přenosy {systém služby {ping;                  } protokoly {bgp;                  rozhraní}} {reth0.100;              }} zóny zabezpečení Untrust {hostitele – příchozí přenosy {systém služby {ping;                  } protokoly {bgp;                  rozhraní}} {reth1.100;              }          }      }  }


### <a name="3-create-security-policies-between-zones"></a>3. vytvořit zásady zabezpečení mezi oblastmi
 
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4. Konfigurace zásad překladu síťových adres
 - Vytvořte dva fondy překladu síťových adres. Jednu se použijí k překladu síťových adres přenosy odchozí společnosti Microsoft a od Microsoftu zákazníkovi.
 - Vytváření pravidel pro překladu síťových adres odpovídajících přenosy

        security {
            nat {
                source {
                    pool SNAT-To-ExpressRoute {
                        routing-instance {
                            External-ExpressRoute;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    pool SNAT-From-ExpressRoute {
                        routing-instance {
                            Internal;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    rule-set Outbound_NAT {
                        from routing-instance Internal;
                        to routing-instance External-ExpressRoute;
                        rule SNAT-Out {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-To-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                    rule-set Inbound-NAT {
                        from routing-instance External-ExpressRoute;
                        to routing-instance Internal;
                        rule SNAT-In {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-From-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }


### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. konfigurace BGP inzerce selektivního předpony v každém směru

Podívejte se do vzorků ve stránce [směrování konfigurace ukázek](expressroute-config-samples-routing.md) .

### <a name="6-create-policies"></a>6. vytvořit zásady

    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Další kroky

V tématu [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md) pro další podrobnosti.
