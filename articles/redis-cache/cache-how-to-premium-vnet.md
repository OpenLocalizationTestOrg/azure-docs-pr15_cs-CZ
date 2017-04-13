<properties 
    pageTitle="Postup při konfiguraci virtuální sítě podpory pro Premium Azure Redis mezipaměť | Microsoft Azure" 
    description="Naučte se vytvářet a spravovat virtuální sítě podpora instance Azure Redis mezipaměti vašeho Premium osy" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Postup při konfiguraci virtuální podpora sítě pro mezipaměť Redis Premium Azure
Azure Redis mezipaměti má nabídky různých mezipaměti, které poskytují flexibilitu při volbě mezipaměti, velikosti a funkcí, včetně nové osy Premium.

Azure Redis mezipaměti prémiových funkcí osy zahrnout clusterů trvalé a podporu virtuální sítě (VNet). VNet je privátní síť v cloudu. Když instance Azure Redis mezipaměť nastavena VNet, není veřejně s možností zadání a přístupný pouze z virtuálních počítačích a aplikace v rámci VNet. Tento článek popisuje, jak nakonfigurovat virtuální sítě podporu instanci Azure Redis mezipaměť premium.

>[AZURE.NOTE] Azure Redis mezipaměť podporuje obou klasické a ARM VNets.

Informace o dalších prémiových funkcí mezipaměti najdete v článku [Úvod Azure Redis mezipaměti Premium osy](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Proč VNet?
Nasazení [Azure virtuální sítě (VNet)](https://azure.microsoft.com/services/virtual-network/) poskytuje lepší zabezpečení a izolace Azure Redis mezipaměť, jakož i podsítí, zásady řízení přístupu a dalších funkcí omezit přístup k Azure Redis mezipaměti.

## <a name="virtual-network-support"></a>Podpora virtuální sítě
Virtuální podporu sítí (VNet) je nakonfigurovaný na zásuvné **Novou mezipaměť Redis** při vytváření mezipaměti. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Po výběru premium ceny osy můžete nakonfigurovat integraci Azure Redis mezipaměti VNet tak, že vyberete VNet, který je ve stejném umístění jako mezipaměť a předplatného. Použít nový VNet, vytvořte nejdřív podle postupu v části [vytvořit virtuální sítě pomocí portálu Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) nebo [síti virtuální (klasický) pomocí portálu Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md) a vraťte se do zásuvné **Novou mezipaměť Redis** můžete vytvořit a nakonfigurovat mezipaměť premium.

Abyste mohli nakonfigurovat VNet pro novou mezipaměť, klikněte na **Virtuální sítě** na zásuvné **Novou mezipaměť Redis** a v rozevíracím seznamu vyberte požadované VNet.

![Virtuální sítě][redis-cache-vnet]

Vyberte požadovanou podsítě z rozevíracího seznamu **adres podsítí** a zadejte požadované **statické IP adresu**. Pokud používáte klasické VNet **statické IP adresu** pole není povinné, a pokud není zadán žádný, jednu vybrané z vybrané podsítě.

>[AZURE.IMPORTANT] Při nasazení mezipaměti Redis Azure ARM VNet, mezipaměti musí být ve vyhrazené podsítě obsahující žádné jiné zdroje s výjimkou instance Azure Redis mezipaměti. Pokud je pokus o nasazení mezipaměti Redis Azure ARM VNet do podsítě, která obsahuje další zdroje, selhání nasazení.

![Virtuální sítě][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] První čtyři adresy podsítě jsou vyhrazená a nelze použít. Další informace najdete v tématu [existují omezení k použití IP adres v těchto podsítí?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Po vytvoření mezipaměti uvidí konfiguraci VNet kliknutím **Virtuální sítě** z zásuvné **Nastavení** .

![Virtuální sítě][redis-cache-vnet-info]


Připojení k instanci aplikace Azure Redis mezipaměti při použití VNet, zadejte název hostitele mezipaměť v připojovacím řetězci, jak je vidět v následujícím příkladu.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis mezipaměti VNet časté otázky

V tomto seznamu najdete odpovědi na nejčastější dotazy týkající se Azure Redis mezipaměti měřítko.

-   [Jaké jsou některé běžné problémy s Azure Redis mezipaměti a VNets stav?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-   [Můžete použít VNets s mezipamětí standardní nebo základní?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-   [Proč vytváření Redis mezipaměti nezdaří v některých podsítí a jiné ne?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-   [Fungují všechny funkce mezipaměti hostování mezipaměti v VNET?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Jaké jsou některé běžné problémy s Azure Redis mezipaměti a VNets stav?

Když Azure Redis mezipaměti je hostovaný ve VNet, používá porty v následující tabulce. Pokud tyto porty blokovány, mezipaměti nemusí fungovat správně. Máte jednu nebo více tyto porty blokované problém nejběžnější stav při použití Azure mezipaměti VNet Redis.

| Porty     | Směr        | Transport Protocol (protokol) | Účel                                                                           | Vzdálená IP                           |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| 80, 443     | Odchozí         | TCP                | Redis závislostí na Azure úložiště/KLÍČŮ (Internet)                                | *                                   |
| 53          | Odchozí         | TCP/UDP            | Redis závislostí na DNS (Internet/VNet)                                         | *                                   |
| 6379, 6380  | Příchozí          | TCP                | Komunikace klienta do Redis Vyrovnávání zatížení Azure                               | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 8443        | Příchozí nebo odchozí | TCP                | Implementace podrobností Redis                                                   | VIRTUAL_NETWORK                     |
| 8500        | Příchozí          | TCP/UDP            | Azure Vyrovnávání zatížení                                                              | AZURE_LOADBALANCER                  |
| 10221 10231 | Příchozí nebo odchozí | TCP                | Implementace podrobností Redis (můžou omezit koncovém VIRTUAL_NETWORK) | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 13000 13999 | Příchozí          | TCP                | Komunikace klienta Redis clusterů, Vyrovnávání zatížení Azure                      | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 15000 15999 | Příchozí          | TCP                | Komunikace klienta Redis clusterů, Vyrovnávání zatížení Azure                      | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 16001       | Příchozí          | TCP/UDP            | Azure Vyrovnávání zatížení                                                              | AZURE_LOADBALANCER                  |
| 20226       | Příchozí + odchozí | TCP                | Provádění podrobností Redis clusterů                                          | VIRTUAL_NETWORK                     |


Je potřeba síťové připojení věcí Redis mezipaměti Azure, která nemusí být splněny původně virtuální sítě. Azure Redis mezipaměti vyžaduje všechny následující položky ke správnému při použití v rámci virtuální sítě.

-  Odchozí síťové připojení k úložišti Azure Celosvětová dostupnost koncové body. Platí to i pro koncové body umístěny ve stejné oblasti jako instanci Azure Redis mezipaměti stejně jako koncové body úložiště součástí **jiných** Azure oblastí. Azure koncové body úložiště vyřešit podle těchto DNS domény: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*a *file.core.windows.net*. 
-  Připojení k síti odchozí *ocsp.msocsp.com*, *mscrl.microsoft.com* a *crl.microsoft.com*. Toto připojení je potřeba k podporují funkce SSL.
-  Konfigurace DNS pro virtuální sítě musí být může řešení všechny koncové body a domény uvedené v dřívějších bodů. Zajištění nakonfigurované a zachovány virtuální sítích platné infrastruktury služby DNS můžete splňovat tyto požadavky DNS.



### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Můžete použít VNets s mezipamětí standardní nebo základní?

VNets lze použít pouze s mezipamětí premium.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Proč vytváření Redis mezipaměti nezdaří v některých podsítí a jiné ne?

Pokud nasazujete mezipaměti Redis Azure ARM VNet, mezipaměti musí být ve vyhrazené podsítě obsahující žádný jiný typ zdroje. Pokud je pokus o nasazení mezipaměti Redis Azure ARM VNet podsítě, která obsahuje další zdroje, selhání nasazení. Než budete moct vytvářet novou mezipaměť Redis je potřeba odstranit existující zdroje uvnitř podsítě.

Můžete nasadit více typů zdrojů do klasického VNet dlouhou, jak máte dost IP adresy dostupné.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Fungují všechny funkce mezipaměti hostování mezipaměti v VNET?

Část VNET po mezipaměť pouze klienty v VNET zpřístupníte mezipaměti. Následující funkce správy mezipaměti jako výsledek nefungují v současné době.

-   Redis konzoly: vzhledem k tomu Redis konzoly používá klienta redis cli.exe hostitelem VMs, které nejsou součástí vašeho VNET, nemůže připojit k mezipaměť.


## <a name="use-expressroute-with-azure-redis-cache"></a>Použití ExpressRoute s Azure Redis mezipaměti

Zákazníci se můžete připojit okruh [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) pro jejich virtuální síťové infrastruktury, tak rozšířit jejich místní síti Azure. 

Ve výchozím nastavení oznamuje nově vytvořený okruh ExpressRoute výchozí trasu, která povolí odchozí připojení k Internetu. V této konfiguraci klientských aplikacích se připojit k jiné Azure koncové body včetně Azure Redis mezipaměti.

Běžné konfigurace zákazníka je ale definovat vlastní výchozí trasy (0.0.0.0/0) vynutí, odchozí internetový provoz místo toho na plovoucí dlaždice místní. Tento tok vždy konce připojení protokolem Azure Redis mezipaměti, protože odchozí přenosy buď blokovaných místně, nebo překladu síťových adres chcete do exportu sady adres, které už práce s různými Azure koncové body.

Řešení je definovat jeden nebo více zástupců uživatelem definovaných směruje (UDRs) na podsítě obsahující mezipaměť Redis Azure. UDR definuje podsítě specifické postupy, které bude použito místo výchozího postupu.

Pokud je to možné doporučujeme použít následující konfigurace:

- Konfigurace ExpressRoute oznamuje 0.0.0.0/0 a ve výchozím nastavení vynutit propojení všechny odchozí přenosy místní.
- UDR použité u podsítě obsahující mezipaměť Redis Azure definuje 0.0.0.0/0 s typem další směrování internetových (například je dolů dále v tomto článku).

Kombinované efektem z těchto kroků se, že úroveň podsítě UDR přednost před ExpressRoute vynucená tunneling, tedy zajištění odchozí přístup k Internetu z mezipaměti Redis Azure.

I když připojení k instanci Azure Redis mezipaměti z místního aplikace pomocí ExpressRoute není scénáři obvyklém kvůli výkonu důvody (dosažení nejlepších výsledků Azure Redis mezipaměti klientů by měl být ve stejné oblasti jako mezipaměť Redis Azure) v tomto případě se odchozí síťové cesty nejde přecházet mezi interní firemní proxy servery, ani může být vyšší vytvořena do místního nasazení. Z mezipaměti Redis Azure změní se tím adresu efektivní překladu síťových adres odchozí provozu v síti. Změna adresy překladu síťových adres mezipaměti Redis Azure instance odchozí v síti způsobí, že problémů s připojením k mnoha koncové body výše uvedené. Výsledkem neúspěšné pokusy vytváření Azure Redis mezipaměti.

**Důležité:**  Směruje podle UDR, **musí** být dostatečně specifické pro přednost před všechny tras oznamovaných konfigurací ExpressRoute. Následující příklad používá rozsah adres obecných 0.0.0.0/0 a jako takové můžete potenciálně omylem přepsat trasách pomocí konkrétnější rozsahy adres.

**Velmi důležité:**  Azure Redis mezipaměť není podporované s konfigurací ExpressRoute této **nesprávně inzerce směruje z veřejné peering cestu k soukromé peering cestu více**. ExpressRoute konfigurace veřejné prozkoumávání nakonfigurován, dostanou trasách od společnosti Microsoft pro velké množství rozsahy Microsoft Azure IP adres. Pokud tyto rozsahy adres nesprávně křížově webového soukromé peering cestu, konečným výsledkem je, že všechny odchozí síťové pakety z mezipaměti Redis Azure instance podsítě nesprávně platnost vytvořena pro zákazníka místní síťovou infrastrukturu. Tento vývojový sítě konce Azure Redis mezipaměti. Řešení tohoto problému je zastavit křížově reklamě směruje z veřejné peering cesty soukromé peering cestu.

Základní informace o uživatelem definovaných směruje je k dispozici v tomto [Přehled](../virtual-network/virtual-networks-udr-overview.md). 

Další informace o ExpressRoute najdete v tématu [ExpressRoute technický přehled](../expressroute/expressroute-introduction.md)

## <a name="next-steps"></a>Další kroky
Naučte se práce s dalšími funkcemi mezipaměť premium.

-   [Úvod do osy Azure Redis mezipaměť Premium](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

