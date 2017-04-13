<properties 
   pageTitle="Poradce při potížích s postupy - portál | Microsoft Azure"
   description="Zjistěte, jak řešit problémy s postupy v modelu nasazení Azure správce na portálu Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-routes-using-the-azure-portal"></a>Poradce při potížích s postupy pomocí portálu Azure

> [AZURE.SELECTOR]
- [Azure portálu](virtual-network-routes-troubleshoot-portal.md)
- [Prostředí PowerShell](virtual-network-routes-troubleshoot-powershell.md)

Pokud máte problémy s připojením k síti do nebo ze svého počítače virtuální Azure (OM), postupů, které mohou ovlivňovat tok vaší OM. Tento článek obsahuje přehled funkcí diagnostických nástrojů pro trasy můžou pomoct odstranit dál.

Směrování tabulek jsou přidružené k podsítí a efektivní na všechna síťová rozhraní (NIC) v tomto podsítě. Následující typy postupů se dají použít pro každé síťové rozhraní:

- **Směruje systém:** Ve výchozím nastavení obsahuje každý podsítě vytvořené v síti virtuální Azure (VNet) systémové směrování tabulky, které umožňují místní VNet komunikace, místní přenosy přes VPN brány a internetový provoz. Systém také tras pro peered VNets.
- **BGP směruje:** Rozšíří do sítě rozhraní prostřednictvím ExpressRoute nebo připojení VPN k webu. Další informace o BGP směrování článků [BGP s VPN brány](../vpn-gateway/vpn-gateway-bgp-overview.md) a [Přehled ExpressRoute](../expressroute/expressroute-introduction.md) pro čtení.
- **Uživatelem definovaných směruje (UDR):** Pokud používáte zařízení virtuální sítě nebo pokud je vynucená tunneling přenosů na místní síti prostřednictvím sítě VPN na webu, bude pravděpodobně uživatelem definovaných postupy (UDRs) spojené s vaší podsítě směrování tabulky. Pokud znáte není UDRs, přečtěte si článek [uživatelem definovaných trasy](virtual-networks-udr-overview.md#user-defined-routes) .

S různé postupy, které se dají použít pro síť rozhraní může být obtížné určit, které agregační trasy platí. Můžou pomoct odstranit připojení k síti OM, můžete zobrazit všechny efektivní cesty rozhraní sítě v modelu nasazení Správce prostředků Azure.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Použití efektivní tras řešit problémy s tok OM

Tento článek používá následující situaci jako příklad pro ukazují, jak řešit problémy s efektivní směruje rozhraní sítě:

OM (*VM1*) připojené k VNet (*VNet1*, předponu: 10.9.0.0/16) nemůže připojit k VM(VM3) v nově peered VNet (*VNet3*, 10.10.0.0/16 předpona). Nejsou žádné UDRs nebo BGP přesměrovává použité u VM1 NIC1 síťové připojení bude v angličtině, platí pouze systém trasy.

Tento článek vysvětluje, jak zjistit příčinu chyba při připojení pomocí funkce efektivní směruje v modelu nasazení řízení zdrojů Azure.
Během příkladu pouze systém trasy stejným způsobem mohou sloužit k určení chyby příchozí a odchozí připojení na jakýkoli typ směrování.

>[AZURE.NOTE] Pokud vaše OM víc NIC připojené, zaškrtněte efektivní směruje u každé nic diagnostikovat problémy s připojením k síti na a od virtuálního počítače.

### <a name="view-effective-routes-for-a-virtual-machine"></a>Zobrazit efektivní postupy pro virtuálního počítače

Zobrazí agregace směruje, které jsou použity virtuálního počítače, proveďte následující kroky:

1. Přihlaste se k portálu Microsoft Azure na https://portal.azure.com.
2. Klikněte na **Další služby**a potom v zobrazeném seznamu klikněte na **virtuálních počítačích** .
3. Vyberte OM řešit problémy s ze seznamu, který se zobrazí a zobrazí se OM zásuvné s možnostmi.
4. Klikněte na **diagnostikování & řešení problémů** a pak vyberte běžných problémů. V tomto příkladu **nelze se připojit ke Moje Windows OM** zaškrtnuté. 

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)

5. Kroky se zobrazí pod problém, jak je znázorněno na následujícím obrázku: 

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Klikněte na *efektivní směruje* v seznamu doporučené kroky.

6. **Efektivní směruje** zásuvné se zobrazí, jak je znázorněno na následujícím obrázku:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Pokud je vaše OM jedinou NIC, je vybrané ve výchozím nastavení. Pokud máte víc než jeden NIC, vyberte NIC, u kterého chcete zobrazit efektivní postupy.

    >[AZURE.NOTE] Nejsou-li OM přidružené NIC spuštěna, nebude zobrazen efektivní trasy. Na portálu se zobrazí pouze prvních 200 efektivní trasy. Úplný seznam klikněte na **Stáhnout**. Dál je můžete vyfiltrovat výsledky ze souboru .csv stažený.

    Všimněte si v výstup následující kroky:
    - **Zdroje**: označuje typ trasy. Směruje systém jsou zobrazeny jako *výchozí*, UDRs se zobrazí jako *uživatel* a branou směruje (statické nebo BGP) se zobrazí jako *VPNGateway*.
    - **Stav**: označuje stav efektivní směrování. Možné hodnoty jsou *aktivní* nebo je *neplatný*.
    - **AddressPrefixes**: Určuje předponu adresy efektivní směrování v CIDR zápisu. 
    - **nextHopType**: označuje přesměrování pro dané trasy. Možné hodnoty jsou *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*nebo *hodnotu Null*. Hodnota *Null* pro **nextHopType** UDR mohou poukazovat neplatná. Třeba při **nextHopType** *VirtualAppliance* a virtuální síťové zařízení OM není ve stavu zřízení/spuštění. Pokud není žádná brána zřízení/spuštění v dané VNet **nextHopType** je *VPNGateway* , postupu může stanou neplatnými.
    
7. Neexistuje žádné směrování uvedenou na VNet *WestUS VNET3* (předpona 10.10.0.0/16) z *WestUS VNet1* (předpona 10.9.0.0/16) na obrázku v předchozím kroku. Na následujícím obrázku je peering odkaz ve stavu *Odpojeno* :
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Obousměrné odkaz prozkoumávání je nefunkční, které vysvětluje, proč nejde VM1 připojit k VM3 v VNet *WestUS VNet3* .

8. Následující obrázek znázorňuje trasy po vytvoření propojení peering obousměrné:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Poradce při potížích scénáře pro vynucená tunneling a směrování hodnocení najdete [Důležité informace o](virtual-network-routes-troubleshoot-portal.md#Considerations) části tohoto článku.

### <a name="view-effective-routes-for-a-network-interface"></a>Zobrazit efektivní postupy pro rozhraní sítě

Pokud je vliv na tok pro konkrétní síťové rozhraní (NIC), můžete zobrazit úplný seznam efektivní tras na NIC přímo. Agregační postupy, které jsou použity NIC zobrazíte proveďte následující kroky:

1. Přihlaste se k portálu Microsoft Azure na https://portal.azure.com.
2. Klikněte na **Další služby**a potom klikněte na **síť rozhraní**
3. Hledat v seznamu název NIC nebo vyberte ze seznamu, který se zobrazí. V tomto příkladu **VM1 NIC1** zaškrtnuté.
4. Vyberte **efektivní směruje** zásuvné **rozhraní sítě** , jak je znázorněno na následujícím obrázku:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    Rozhraní sítě vybrané ve výchozím nastavení **oboru** .

    ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)


### <a name="view-effective-routes-for-a-route-table"></a>Zobrazit efektivní postupy pro tabulku postupu

Při úpravách uživatelem definovaných směruje (UDRs) v tabulce směrování, budete chtít zkontrolovat dopad směruje vás někdo přidá na konkrétní OM. Tabulka směrování může být přidružené k jiné číslo podsítí. Teď můžete zobrazit všechny efektivní postupy pro všechny nic použité dané směrování tabulky, aniž byste museli přepnout kontext z tabulky zásuvné dané trasy.

V tomto příkladu je UDR (*UDRoute*) podle postupu tabulku (*UDRouteTable*). Tento postup odešle veškerý internetový provoz ze *Podsíť1* VNet *WestUS VNet1* až sítě virtuální zařízení (NVA) v *Podsíť2* stejné VNet. Postup je vidět na následujícím obrázku:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Zobrazit agregační směruje pro tabulku postupu, proveďte následující kroky:

1. Přihlaste se k portálu Microsoft Azure na https://portal.azure.com.
2. Klikněte na **Další služby**a potom klikněte na **tabulky směrování**
3. Hledat v seznamu pro tabulku postupu, který chcete zobrazit agregační trasy a vyberte ho. V tomto příkladu je vybrána **UDRouteTable** . Zobrazí se zásuvné k tabulce vybrané postup, jak je znázorněno na následujícím obrázku:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)

4. Vyberte **Efektivní směruje** zásuvné **směrování tabulky** . Směrování tabulku, kterou jste vybrali je nastavený **obor** .
5. Směrování tabulky se dají použít k více podsítí. Vyberte **podsítě** chcete provést revizi ze seznamu. V tomto příkladu je vybrána **Podsíť1** .
6. Vyberte **rozhraní sítě**. Jsou uvedené všechny nic připojené k vybrané podsítě. V tomto příkladu **VM1 NIC1** zaškrtnuté.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

    >[AZURE.NOTE] Pokud NIC není přidruženo pracovního OM, se zobrazí efektivní trasy.

## <a name="considerations"></a>Co byste měli zvážit

Co je potřeba mít na paměti při prohlížení seznamu tras vrátit:

- Směrování je založená na nejdelší předpona POZVYHLEDAT (LPM) mezi UDRs, BGP a systémem trasy. Pokud existuje více než jeden trasa s stejné LPM shoda, pak trasu je vybrán podle původnímu v následujícím pořadí:
    - Uživatelem definované směrování
    - BGP směrování
    - Směrování systému (výchozí)

    Efektivní trasy uvidíte jenom efektivní postupy, které se shodují LPM založené na všechny dostupné trasy. Zobrazením jak trasy skutečně vyhodnoceny pro dané NIC usnadníte mnohem řešení problémů s konkrétní postupy, které mohou být ovlivňující ochranu připojení ze svého OM.

- Pokud máte UDRs a odesíláte přenosy síti virtuální zařízení (NVA) s *VirtualAppliance* jako **nextHopType**, zajistit, aby IP přesměrování je zapnuta NVA příjem přenos nebo paketů se nezobrazí. 
- Pokud vynucený tunneling aktivní, budou všechny odchozí přenosy Internet směrovány do místního nasazení. RDP/SSH z Internetu na vaše OM nemusí fungovat s toto nastavení používají, v závislosti na zpracování tyto přenosy místní. 
  Vynucená tunneling lze povolit:
    - Pokud prostřednictvím sítě VPN na webu nastavením trasu definované uživatelem (UDR) s nextHopType jako VPN brány
    - Pokud výchozí trasy ohlásí přes BGP
- Pro VNet prozkoumávání umožnění datových přenosů do fungovat správně musí existovat směrování systému s **nextHopType** *VNetPeering* peered VNet předponu oblast. Pokud tyto směrování neexistuje a peering odkaz VNet vypadá OK:
    - Počkejte několik sekund, než a v případě, že je nově založení peering odkaz Opakovat. Občas trvá déle, než se rozšíří trasy do rozhraní sítě v podsítě.
    - Skupina zabezpečení síti (NSG) pravidla, které mohou ovlivňovat tok. Další informace naleznete v článku [Poradce při potížích s skupiny zabezpečení sítě](virtual-network-nsg-troubleshoot-portal.md) .
