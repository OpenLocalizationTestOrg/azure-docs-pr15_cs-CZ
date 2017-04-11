<properties
   pageTitle="Provádění hybridní architektura sítě s Azure ExpressRoute | Vytvořte odkaz architektura | Microsoft Azure"
   description="Jak implementovat Architektura zabezpečení webu webu sítě, který přesahuje Azure virtuální sítě a v místní síti připojení pomocí Azure ExpressRoute."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>Provádění hybridní architektura sítě s Azure ExpressRoute

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tento článek popisuje doporučené postupy pro připojení k místní síti k virtuálních sítí na Azure pomocí ExpressRoute. ExpressRoute propojení se vytvářejí pomocí soukromé vyhrazené připojení přes připojení jiného poskytovatele. Připojení k soukromé rozšíří do Azure poskytuje přístup ke IaaS infrastruktury v Azure, veřejné koncové body používán PaaS služby a služby Office 365 SaaS v místní síti. V tomto dokumentu se zaměřuje na používání ExpressRoute pro připojení k jedné Azure virtuální sítě (VNet) pomocí takzvanou soukromé prozkoumávání.

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento plán používá správce zdroje, který Microsoft doporučuje nové nasazení.

Typické použití případů pro tuto architekturu patří:

- Hybridní aplikace, kde rozdělení pracovního vytížení mezi místní síti a Azure.

- Aplikace spuštěné ve velkém měřítku a kritické pracovního vytížení vyžadujících vysoké škálovatelnost.

- Ve velkých zálohování a obnovení zařízení pro data, která musí být uloženy mimo.

- Zpracování velké dat úloh.

- Použití Azure jako havárie obnovení webu.

> [AZURE.NOTE] [Technický přehled ExpressRoute] [ expressroute-technical-overview] obsahuje úvod k ExpressRoute.

## <a name="architecture-diagram"></a>Diagram architektury

V následujícím diagramu jsou uvedeny důležitou součástí tato architektura:

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento diagram je na stránce "Hybridní sítě – ER".

![[0]][0]

- **Azure virtuálních sítí (VNets).** Každý VNet umístěná v jedné oblasti Azure a můžete hostovat více úrovní aplikace. Aplikace úrovní můžete segmentována pomocí podsítí v jednotlivých VNet a/nebo sítě skupin zabezpečení (NSGs). 

- **Azure veřejných služeb.** Jedná se o Azure služby, které můžete využít aplikace hybridní. Tyto služby jsou dostupné taky veřejné Internetu, ale přístupu k nim prostřednictvím ExpressRoute okruh poskytuje zhoršeným latence a předvídatelně výkonu, protože přenosy nepřekročí přes Internet. Připojení provádí pomocí **veřejných prozkoumávání**adresy, které jsou vlastněná vaší organizace nebo nastavuje váš poskytovatel připojení. 

- **Služby Office 365.** Toto jsou veřejně dostupný Office 365 aplikací a služeb společnosti Microsoft. Připojení provádí pomocí **Microsoft prozkoumávání**adresy, které jsou vlastněná vaší organizace nebo nastavuje váš poskytovatel připojení.

>[AZURE.NOTE] Taky můžete připojit přímo na webu Microsoft CRM Online pomocí Microsoft prozkoumávání.

- **Místní podnikové sítě.** Toto je síť počítačích a zařízeních připojení přes privátní sítě místní systém v rámci organizace.

- **Místní okraj směrovači.** Toto jsou směrovači, která se připojují místní síti obvodu spravuje poskytovatel. V závislosti na tom, jak máte k dispozici připojení je třeba zadat na veřejné adresy IP směrovači. 

- **Microsoft edge směrovači.** Toto jsou dvěma směrovači v konfiguraci vysoce dostupné aktivní. Tyto směrovači povolit připojení poskytovatele jejich obvody přímé připojení k jejich datacentra. V závislosti na tom, jak máte k dispozici připojení je třeba zadat na veřejné adresy IP směrovači.

- **ExpressRoute obvod.** Toto je vrstvy 2 nebo layer 3 okruh poskytnutých poskytovatel připojení, které spojení místní síti s Azure pomocí směrovači okraje. Obvod využívá infrastrukturu hardwaru spravuje poskytovatel připojení.

- **Připojení poskytovatelů.** Toto jsou společností, které poskytují připojení pomocí vrstvy 2 nebo layer 3 propojení mezi vaší datacentra a Azure datacentra.

## <a name="recommendations"></a>Doporučení

Azure nabízí mnoho různých zdrojů a typů zdrojů, takže může být tento odkaz architektura zřízení mnoha různými způsoby. Správce prostředků Azure šablonu nainstalovat architektura odkaz, který následuje těmito doporučeními uvádíme. Pokud budete chtít vytvořit vlastní odkaz architektura Pokud požadujete konkrétní příliš velký doporučení by měl těmito doporučeními.

### <a name="connectivity-providers"></a>Poskytovatelé připojení

Vyberte vhodné poskytovatel připojení ExpressRoute ve své lokalitě. Seznam poskytovatelů připojení k dispozici na svém pracovišti zobrazíte pomocí následujícího příkazu Powershellu Azure:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

ExpressRoute připojení poskytovatelů připojení vaše datové centrum Microsoftu následujícími způsoby:

- **Spoluvytváření umístěný v cloudu exchange.** Pokud se nacházíte spoluvytváření v zařízení se serverem exchange cloudu, můžete objednejte virtuální křížově připojení ke cloudu společnosti Microsoft prostřednictvím poskytovatele společné umístění Ethernet exchange. Společné umístění poskytovatelů můžou nabízet vrstvy 2 křížově připojení nebo spravovaných Layer 3 křížově připojení mezi infrastrukturu ve funkci společné umístění a v cloudu společnosti Microsoft.

- **Připojení pomocí Ethernet.** Místní datacentrech/poboček můžete připojit k v cloudu společnosti Microsoft prostřednictvím dvoustranných Ethernet propojení. Bod k Ethernet poskytovatelů můžou nabízet vrstvy 2 připojení nebo spravuje Layer 3 připojení mezi webem a v cloudu společnosti Microsoft.

- **Libovolný – libovolného sítí (IPVPN).** Vaší sítě WAN můžete integrovat s v cloudu společnosti Microsoft. Poskytovatelé IPVPN (obvykle MPLS VPN) nabízejí libovolného libovolného propojení mezi pobočky a datacentrech. V cloudu společnosti Microsoft můžete být propojeny WAN aby vypadal stejně jako jakékoli jiné pobočce. Sítě WAN poskytovatelů obvykle nabízejí spravovaných připojení Layer 3.

Další informace o připojení poskytovatelů, najdete v článku [ExpressRoute obvody a směrování domény][connectivity-providers].

### <a name="expressroute-circuit"></a>ExpressRoute okruh

Bylo zajištěno, že vaše organizace má [základní požadavky ExpressRoute] [ expressroute-prereqs] připojit se k Azure.

Pokud jste to ještě neudělali, přidejte podsítě s názvem `GatewaySubnet` na Azure VNet a vytvořte bránu ExpressRoute virtuální sítě pomocí služby Azure VPN brány. Podrobnosti o tomto procesu, najdete v článku [pracovní postupy ExpressRoute pro zřizování obvodu a států okruh][ExpressRoute-provisioning].

Vytvoření ExpressRoute okruh následujícím způsobem:

1. Spusťte tento příkaz Powershellu:

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. Odeslání `ServiceKey` nové okruhem poskytovateli služeb.

3. Čekat na poskytovatele zřízení obvod. Ověření stavu zřizovací obvodu pomocí následujícího příkazu Powershellu:

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

`Provisioning state` Pole v `Service Provider` část výstup se změní z `NotProvisioned` k `Provisioned` jestli už se dá obvod.

>[AZURE.NOTE]Pokud používáte připojení layer 3, by měl poskytovatele konfigurovat a spravovat směrování. Zadejte třeba povolit poskytovatele implementovat trasy potřebné informace.

Pokud používáte připojení vrstvy 2, postupujte následujícím způsobem:

1. Rezervovat dvě/30 podsítí skládající se ze platné adresy IP u jednotlivých typů prozkoumávání chcete implementace. Tyto/30 podsítí se použijí k zadání IP adres pro směrovači pro obvod. Pokud soukromé veřejné a Microsoft prozkoumávání, musíte mít 6 /30 podsítí platné veřejnou IP adresy.     

2. Konfigurace směrování okruhem ExpressRoute. Potřebujete k spuštění příkazu pod u jednotlivých typů prozkoumávání chcete konfigurovat (soukromé veřejné a Microsoft).

    >[AZURE.NOTE]V tématu [vytvořit a upravit směrování okruhem ExpressRoute] [ configure-expressroute-routing] podrobnosti. Umožňuje přidat síť prozkoumávání směrování přenosů následující příkazy Powershellu:

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. Rezervujte další fond platné veřejných adres IP a slouží k překladu síťových adres pro veřejnost prozkoumávání Microsoft. Je vhodné mít různých fondu pro každou prozkoumávání. Fond připojení poskytovatele zadejte, takže můžete nakonfigurovat BGP inzerce u těchto oblastí.

[Propojení soukromé VNet(s) v cloudu okruh ExpressRoute][link-vnet-to-expressroute]. Použijte následující příkazy Powershellu:

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

Mějte na paměti následující skutečnosti:

- ExpressRoute pro výměnu informací o směrování mezi síť a Azure používá ohraničení brány (BGP Protocol).

- Více VNets nachází v různých oblastech stejné obvodu ExpressRoute, pokud jsou všechny VNets a okruh ExpressRoute umístěny ve stejné oblasti geopolitické se můžete připojit.

## <a name="scalability-considerations"></a>Co byste měli zvážit škálovatelnost:

Obvody ExpressRoute zadejte cestu velkou šířkou pásma mezi sítě. Obecně vzato vyšší šířku pásma větší náklady. Přestože někteří poskytovatelé umožňují změnit šířky pásma, ujistěte se, že si vybrat počáteční šířky pásma, který překročí vašim potřebám a poskytuje prostor pro růst. V případě potřeby zvětšení šířky pásma v budoucnu jsou doleva dvou možností.

Zvětšení šířky pásma. Myslete na to, že ne všechny poskytovatelé umožňují dělat, dynamicky. A neměli byste, nemusíte udělat toto co nejvíc. Ale pokud potřeby po kontrola u poskytovatele služeb, spusťte následující příkazy.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Můžete zvětšit šířky pásma bez ztráty připojení. Snížení šířky pásma, jakou povede přerušení připojení. Budete muset odstranit obvodu a znovu se nová konfigurace.

Pokud používáte standardní SKU pro ExpressRoute a potřebujete upgradovat Premium nebo změnit jste svůj ceny plánování (s měřením dat nebo neomezený), spusťte následující příkazy. `Sku.Tier` Může být vlastnost `Standard` nebo `Premium`; `Sku.Name` může být vlastnost `MeteredData` nebo `UnlimitedData`.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] Přesvědčte se, zda `Sku.Name` vlastnost shody `Sku.Tier` a `Sku.Family`. Pokud změníte rodinu a osy, ale ne název, připojíte se budou zakázané.

Upgrade SKU bez přerušení, ale nemůžete přepnout z neomezený ceny plánu podle objemu dat. Při přechodu SKU, musí zůstat využití šířky pásma v rámci výchozí limit standardní SKU.

> [AZURE.NOTE] ExpressRoute nabízí dvě ceny schémata zákazníkům založených na datech měření nebo neomezeno. V tématu [ceny ExpressRoute] [ expressroute-pricing] podrobnosti. Náklady lišit podle toho, okruh pásma. Dostupné šířky pásma pravděpodobně lišit od poskytovatele poskytovatele. Použití `Get-AzureRmExpressRouteServiceProvider` rutinu najdete v článku poskytovatelé k dispozici ve vaší oblasti a šířek pásma, které nabízejí.

Jeden okruh ExpressRoute podporují celou řadu peerings a VNet odkazy. V tématu [limity ExpressRoute] [ expressroute-limits] Další informace.

U dalších poplatků ExpressRoute Premium doplněk nabízí:

- Až 10 000 směruje za okruh. Bez ExpressRoute Premium doplňku je limit 4 000 směruje za okruh.

- Globální připojení k povolení ExpressRoute okruh umístěné kdekoli na světě při přístupu k prostředkům ve všechny oblasti nikoli pouze oblastí ve stejném kontinent.

- Zvýšení počtu VNet odkazy na okruh z 10 větší omezení, v závislosti na šířku pásma obvodu.

Obvody ExpressRoute umožňují dočasná síť roztržení až dvakrát omezení šířky pásma, získané žádné další náklady. To dosáhnete pomocí nadbytečné odkazy. Ale ne všech připojení podporují tuto funkci; Ověřte, že váš poskytovatel připojení umožňuje tuto funkci před v závislosti na to.

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Dostupnost Azure připojení můžete nakonfigurovat různými způsoby podle toho, typ zprostředkovatele, který používáte a počet ExpressRoute obvody a virtuální síťových připojení brány, který jste ochotni konfigurace. Následující body shrnutí možností dostupnost:

- ExpressRoute nepodporuje směrovači redundance protokoly HSRP VRRP pro implementaci a dostupnost. Místo toho používá nadbytečné dvojice BGP relací na prozkoumávání. Usnadnit vysoce dostupná připojení k síti ustanovení Microsoft Azure se dvěma nadbytečné porty na dvěma směrovači (součást Microsoft edge) v konfiguraci aktivní.

- Pokud používáte připojení vrstvy 2, nasazení nadbytečné směrovači ve svojí místní síti v konfiguraci aktivní. Připojte primární okruh jedním směrovačem a vedlejší okruh k druhému. To vám umožní vysoce dostupné připojení na obou koncích připojení. Toto je nutné, pokud nastavíte, že ExpressRoute SLA. V tématu [SLA Azure ExpressRoute] [ sla-for-expressroute] podrobnosti.

Následující diagram ukazuje a konfigurace se nadbytečné místního směrovači připojení k obvody primárních a sekundárních. Každý okruh zpracovává přenosy pro prozkoumávání veřejné a privátní prozkoumávání (každý prozkoumávání označen dvojice /30 adresy mezery, podle popisu v předchozí části).

![[1]][1]

Pokud používáte připojení layer 3, ověřte, že poskytuje nadbytečné BGP relací, které zpracovávají dostupnost za vás.

Virtuální sítě můžete být připojeni k více ExpressRoute obvody a každý obvody lze zadat různé poskytovatele služeb. Tato strategie poskytuje další vysoké dostupnosti a havárie možnosti obnovení.

Nakonfigurujte VPN k webu jako cestu převzetí ExpressRoute. To platí pouze pro soukromé prozkoumávání. Služby Azure a Office 365 Internet je pouze převzetí cestu.

Ve výchozím nastavení BGP relace pomocí nečinnosti přínosu 60 sekund. Jakmile relaci vypršení časového limitu 3krát, postupu označen jako není k dispozici a všechny přenosy se přesměruje do zbývající směrovači. Tento časový limit 180 druhé může být příliš dlouhá důležitých aplikací. V takovém případě můžete změnit nastavení časového limitu BGP na směrovači místní menší hodnotu.

## <a name="manageability-considerations"></a>Důležité informace o možnostech správy

Můžete použít [Azure připojení nástrojů (AzureCT)] [ azurect] sledování propojení mezi místním datacentra a Azure.

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Možnosti zabezpečení pro Azure připojení můžete nakonfigurovat různými způsoby podle pochybnosti zabezpečení a dodržování předpisů potřeb. Následující body souhrn nastavení zabezpečení.

ExpressRoute pracuje v layer 3. Hrozeb ve vrstvě aplikace můžete zabránit pomocí sítě spotřebiče zabezpečení, které omezí přenosy legitimní zdroje. Navíc můžete ExpressRoute připojení pomocí veřejných prozkoumávání spuštěná pouze z místního. Tím škodlivý služba z přístupu a nebezpečné místních dat z veřejného Internetu.

Maximalizovat zabezpečení, přidejte síť zabezpečení zařízení mezi místní síti a směrovači okraj poskytovatele. To umožní zabránit přírůstek neoprávněným přenosy VNet:

![[2]][2]

Sestavy auditování nebo dodržování předpisů důvodů, může být nutné přímý přístup zakázanou součásti spuštěné v VNet k Internetu a implementace [Vynucená tunneling][forced-tuneling]. V takovém případě by měl být internetový provoz přesměrován dozadu na proxy server, místní systém, kde je jde auditovat. Blokovat neoprávněným přenosy toku a filtrovat potenciálně nebezpečný příchozích je možné konfigurovat proxy server.

![[3]][3]

Ve výchozím nastavení vystavit Azure VMs koncové body použít k poskytnutí přístup pro přihlášení pro účely správy - RDP a vzdálený Powershell pro Windows VMs a SSH pro VMs Linux při nasazení prostřednictvím portálu Azure. Maximalizace zabezpečení, není povolit veřejnou IP adresu pro vaše VMs a ujistěte se, jestli nejsou tyto VMs veřejně přístupný pomocí NSGs. VMs by měl být k dispozici pouze pomocí interní IP adresu. Tyto adresy lze získat přístup prostřednictvím sítě ExpressRoute povolení místní DevOps zaměstnanců provádět všechny potřebné konfiguraci nebo údržbu.

Pokud musí zpřístupnit koncové body správy pro VMs externí síti, použijte NSGs a přistupovat k seznamy řízení omezte viditelnost tyto porty povolených IP adres nebo sítě.

## <a name="troubleshooting-considerations"></a>Poradce při potížích s co byste měli zvážit

Selhání dříve funkční okruh ExpressRoute teď se chcete připojit, bez jakékoli konfigurace změny v místním nasazení nebo v rámci soukromé VNet, budete muset kontaktujte poskytovatele připojení a pracovat s nimi tento problém vyřešit. Taky můžete provádět následující příkazy Powershellu Azure k provádění některých omezené kontroly a pomoct zjistit, kde může leží problémů:

- Ověřte, že byla zajištěna obvod:

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Výstup tento příkaz zobrazí několik vlastnosti okruhem, včetně `ProvisioningState`, `CircuitProvisioningState`, a `ServiceProviderProvisioningState` jak je ukázáno v následujícím příkladu.

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

Pokud `ProvisioningState` není nastavena na `Succeeded` poté, co jste se pokusili vytvořit nový okruh, odeberte obvod pomocí příkazu níže a zkuste znovu vytvořit.

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Pokud váš poskytovatel vám to již zřízení obvodu a `ProvisioningState` je nastavený na `Failed`, nebo `CircuitProvisioningState` není `Enabled`, požádejte o další pomoc poskytovatele.

## <a name="solution-deployment"></a>Nasazení řešení

Pokud máte existující místní infrastruktury nakonfigurováno pomocí zařízení vhodné sítě, můžete nasadit architektura odkaz pomocí následujících kroků:

Pokud máte existující místní infrastruktury už nakonfigurována VPN zařízení, můžete nasadit architektura odkaz pomocí následujících kroků:

1. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně":  
[![Nasazení Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. Čekat na odkaz v portálu Azure otevřít a pak postupujte podle těchto kroků: 
  - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte **Vytvořit nový** a zadejte `ra-hybrid-er-rg` do textového pole.
  - Vyberte oblast z **umístění** rozevíracím seznamu.
  - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
  - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
  - Klikněte na tlačítko **koupit** .

3. Počkejte nasazení dokončete.

4. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně":  
[![Nasazení Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. Čekat na odkaz v portálu Azure otevřít a pak postupujte podle těchto kroků: 
  - Vyberte možnost **použít existující** v části **Skupina zdroje** a zadejte `ra-hybrid-er-rg` do textového pole.
  - Vyberte oblast z **umístění** rozevíracím seznamu.
  - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
  - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
  - Klikněte na tlačítko **koupit** .

6. Počkejte nasazení dokončete.

## <a name="next-steps"></a>Další kroky

- V tématu [implementace architektura sítě vysoce dostupné hybridní] [ highly-available-network-architecture] informace o prodloužení dostupnost síti hybridní na základě ExpressRoute tak, že při přechodu na připojení VPN.

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "Hybridní síťové architektury pomocí Azure ExpressRoute"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "Pomocí nadbytečné směrovači ExpressRoute primárních a sekundárních obvody"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "Přidání zabezpečení zařízení v místní síti"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "Použití vynucená tunneling mají být auditovány přenosy vazbou na Internetu"
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "Vyhledání klíč ServiceKey obvodu ExpressRoute"
