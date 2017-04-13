<properties 
   pageTitle="Poradce při potížích s postupy - prostředí PowerShell | Microsoft Azure"
   description="Zjistěte, jak řešit problémy s postupy v modelu nasazení Správce prostředků Azure pomocí prostředí PowerShell Azure."
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

# <a name="troubleshoot-routes-using-azure-powershell"></a>Poradce při potížích s postupy pomocí prostředí PowerShell Azure

> [AZURE.SELECTOR]
- [Azure portálu](virtual-network-routes-troubleshoot-portal.md)
- [Prostředí PowerShell](virtual-network-routes-troubleshoot-powershell.md)

Pokud máte problémy s připojením k síti do nebo ze svého počítače virtuální Azure (OM), postupů, které mohou ovlivňovat tok vaší OM. Tento článek obsahuje přehled funkcí diagnostických nástrojů pro trasy můžou pomoct odstranit dál.

Směrování tabulek jsou přidružené k podsítí a efektivní na všechna síťová rozhraní (NIC) v tomto podsítě. Následující typy postupů se dají použít pro každé síťové rozhraní:

- **Směruje systém:** Ve výchozím nastavení obsahuje každý podsítě vytvořené v síti virtuální Azure (VNet) systémové směrování tabulky, které umožňují místní VNet komunikace, přenosu místní přes VPN brány a internetový provoz. Systém také tras pro peered VNets.
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

### <a name="view-effective-routes-for-a-network-interface"></a>Zobrazit efektivní postupy pro rozhraní sítě

Zobrazí agregace směruje, která platí pro rozhraní sítě, proveďte následující kroky:

1.  Spusťte relaci Powershellu Azure a přihlaste se k Azure. Pokud znáte není Azure Powershellu, přečtěte si článek [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) .

2.  Tento příkaz vrátí všechny použité pro rozhraní sítě s názvem *VM1 NIC1* ve skupině prostředků *RG1*trasy.

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Pokud neznáte název rozhraní sítě, zadejte tento příkaz Načíst jména všechna síťová rozhraní v group.* zdroje

        Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name

    Následující výstup vypadá podobně jako výstup pro každou směrování použité u podsítě, se kterými NIC spojení:

        Name :
        State : Active
        AddressPrefix : {10.9.0.0/16}
        NextHopType : VNetLocal
        NextHopIpAddress : {}

        Name :
        State : Active
        AddressPrefix : {0.0.0.0/16}
        NextHopType : Internet
        NextHopIpAddress : {}

    Všimněte si v výstup následující kroky:
    - **Název**: název efektivní směrování může být prázdné, pokud není výslovně zadaný pro definované uživatelem trasy. 
    - **Stav**: označuje stav efektivní směrování. Možné hodnoty jsou "Aktivní" nebo "Neplatné"
    - **AddressPrefixes**: Určuje předponu adresy efektivní směrování v CIDR soustavě. 
    - **nextHopType**: označuje přesměrování pro dané trasy. Možné hodnoty jsou *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*nebo *hodnotu Null*. Hodnota *Null* pro **nextHopType** UDR mohou poukazovat neplatná. Třeba při **nextHopType** *VirtualAppliance* a virtuální síťové zařízení OM není ve stavu zřízení/spuštění. Pokud **nextHopType** je *VPNGateway* a bez brány zřízení/spuštění v dané VNet, může stanou neplatnými trasy.
    - **NextHopIpAddress**: Určuje IP adresu přesměrování efektivní směrování.
    
    Tento příkaz vrátí trasy v jednodušší zobrazení tabulky:

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table

    Následující výstup se některé z výstupu dostali scénář popsáno dříve:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {0.0.0.0/0} Internet {}
    

3. Neexistuje žádné směrování uvedené *WestUS VNet3* VNet (předpona 10.10.0.0/16)* * z *WestUS VNet1* (předpona 10.9.0.0/16) do výstupu v předchozím kroku. Jak je znázorněno na následujícím obrázku je VNet peering odkaz s VNet *WestUS VNet3* ve stavu *Odpojeno* .
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Obousměrné odkaz prozkoumávání je nefunkční, které vysvětluje, proč nejde VM1 připojit k VM3 v VNet *WestUS VNet3* . Znovu instalační program obousměrné peering odkaz VNet *WestUS VNet1* a *WestUS VNet3* VNets. Výstup vrácen po peering odkaz VNet správně takto:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
        
    Až určíte problém, můžete přidat, odebrat, nebo změnit směruje a směrování tabulek. Zadejte následující příkaz zobrazíte seznam příkazů pro postupujte takto:

        Get-Help *-AzureRmRouteConfig

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
    - Pokud prostřednictvím sítě VPN na webu nastavením trasu definované uživatelem (UDR) s nextHopType jako brána VPN
    - Pokud výchozí trasy ohlásí přes BGP
- Pro VNet prozkoumávání umožnění datových přenosů do fungovat správně musí existovat směrování systému s **nextHopType** *VNetPeering* peered VNet předponu oblast. Pokud tyto směrování neexistuje a peering odkaz VNet vypadá OK:
    - Počkejte několik sekund, než a v případě, že je nově založení peering odkaz Opakovat. Občas trvá déle, než se rozšíří trasy do rozhraní sítě v podsítě.
    - Skupina zabezpečení síti (NSG) pravidla, které mohou ovlivňovat tok. Další informace naleznete v článku [Poradce při potížích s skupiny zabezpečení sítě](virtual-network-nsg-troubleshoot-powershell.md) .
