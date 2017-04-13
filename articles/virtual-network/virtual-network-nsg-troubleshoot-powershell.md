<properties 
   pageTitle="Poradce při potížích s skupiny zabezpečení sítě – prostředí PowerShell | Microsoft Azure"
   description="Zjistěte, jak řešit problémy s skupiny zabezpečení sítě v modelu nasazení Správce prostředků Azure pomocí prostředí PowerShell Azure."
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

# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Poradce při potížích s skupiny zabezpečení sítě pomocí prostředí PowerShell Azure

> [AZURE.SELECTOR]
- [Azure portálu](virtual-network-nsg-troubleshoot-portal.md)
- [Prostředí PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Pokud jste nakonfigurovali sítě skupin zabezpečení (NSGs) ve počítače virtuální (OM) a dochází k problémům s připojením k OM, tento článek obsahuje přehled funkcí diagnostických nástrojů pro NSGs můžou pomoct odstranit dál.

NSGs umožňují řídit typy přenosů s tokem a odhlášení z virtuálních počítačích (VMs). NSGs se dají použít k podsítě v síti virtuální Azure (VNet) a síťových rozhraní (NIC). Jsou efektivní pravidla použité NIC agregaci pravidla, která jsou v NSGs použité NIC a podsítě, ke kterému je připojen k. Pravidla přes tyto NSGs může někdy v konfliktu s překrývajícími a mít vliv na připojení k síti OM.  

Všechna pravidla efektivní zabezpečení uvidí z vaší NSGs používaný v vaší OM nic. Tento článek ukazuje, jak řešit problémy s připojením OM pomocí těchto pravidel v modelu nasazení Správce prostředků Azure. Pokud znáte není VNet a NSG koncepty, přečtěte si články přehled [virtuální sítě](virtual-networks-overview.md) a [skupiny zabezpečení, sítě](virtual-networks-nsg.md) .

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Pomocí pravidel pro efektivní zabezpečení řešit problémy s tok OM

Scénář, který následuje je příklad běžných problémů připojení:

OM s názvem *VM1* je součástí podsítě s názvem *Podsíť1* v rámci VNet s názvem *WestUS VNet1*. Pokus o připojení k OM pomocí RDP přes TCP port 3389 se nezdaří. NSGs jsou použity na NIC *VM1 NIC1* a podsítě *Podsíť1*. Umožnění datových přenosů do portu TCP 3389 jsou povolené v NSG přidružené rozhraní sítě *VM1 NIC1*, ale TCP ping na VM1 port 3389 dojde k chybě.

Při tomto příkladu je TCP port 3389, podle těchto kroků mohou sloužit k určení chyby příchozí a odchozí připojení přes jakýkoli.

## <a name="detailed-troubleshooting-steps"></a>Podrobné návody na řešení potíží
Proveďte následující postup řešení problémů s NSGs pro virtuálního počítače:

1. Spusťte relaci Powershellu Azure a přihlaste se k Azure. Pokud si nejste zkušenosti s pomocí Azure Powershellu, přečtěte si článek [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) .

2. Zadejte tento příkaz vrátí použita NIC s názvem *VM1 NIC1* ve skupině prostředků *RG1*všechny NSG pravidla:

        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Pokud neznáte název NIC, zadejte tento příkaz Načíst jména všech nic ve skupině zdroje: 
    
    >`Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`

    Tento text je ukázkový výstup efektivní pravidla pro *VM1 NIC1* NIC vrací:

        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
        
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]

    Poznámka: tyto informace do výstupu:

    - Jsou dva oddíly **NetworkSecurityGroup** : jeden je přidružená k podsítě (*Podsíť1*) a jednu souvisí s NIC (*VM1 NIC1*). V tomto příkladu NSG použit pro každý.
    - **Přidružení** zobrazuje že dané NSG souvisí s zdroj (podsítě nebo NIC). Pokud je NSG zdroj přesunout/přidružen těsně před spuštěním tohoto příkazu, budete muset počkat několik sekund, než aby se změna odrážet příkaz navíc. 
    - Názvy pravidel, které začínají *defaultSecurityRules*: když NSG se vytvoří, několik výchozí zabezpečení pravidla vytvořené v něm obsažené. Výchozí pravidla nejde odebrat, ale je možné přepsat se vyšší priority pravidel.
     Přečtěte si článek [Přehled NSG](virtual-networks-nsg.md#default-rules) zobrazíte další informace o NSG výchozí zabezpečení pravidla.
    - **ExpandedAddressPrefix** rozšíří předpony adres NSG výchozí značek. Značky představují předpony víc adres. Rozšíření značky může být užitečné při řešení potíží s připojením OM z konkrétní adresu předpony. S VNET prozkoumávání, například značky VIRTUAL_NETWORK rozbalí a zobrazí peered předpony VNet do předchozí výstupu.

        >[AZURE.NOTE] Příkaz pouze zobrazí efektivní pravidla Pokud NSG je přidružená k podsítě, NIC nebo obojí. Virtuálního počítače může mít více nic s jinou NSGs použitý. Při odstraňování potíží, příkaz pro každou NIC.
        
3. Usnadnit filtrování přes většího počtu NSG pravidla zadejte následující příkazy chcete odstranit další potíže: 

        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView

    Přenosů RDP (TCP port 3389), filtru k zobrazení mřížky, jak je znázorněno na následujícím obrázku:

    ![Seznam pravidel](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
    
4. Jak vidíte v zobrazení mřížky, jsou povolit i odepřít pravidla pro RDP. Výstup z kroku 2 zobrazuje pravidlo *DenyRDP* probíhá NSG použité u podsítě. Pro příchozí pravidla se zpracovávají NSGs použité u podsítě nejdřív. Pokud je nalezena shoda, není zpracovat NSG použité pro rozhraní sítě. V tomto případě pravidlo *DenyRDP* z podsítě blokuje RDP angličtině (**VM1**).

    >[AZURE.NOTE] Virtuálního počítače může mít více nic připojené k němu. Každý může být připojeni k jiné podsítě. Protože příkazy v předchozích krocích pracují proti NIC, je třeba zajistit, že jste zadali NIC, že máte Chyba při připojení k. Pokud si nejste jistí, je vždy spustit příkazy před každý NIC připojené bude v angličtině.

5. Chcete RDP do VM1 změňte na *Povolit RDP(3389)* **Podsíť1 NSG** NSG pravidlo *Odepřít RDP (3389)* . Zkontrolujte, zda TCP port 3389 otevřít otevřením připojení ke vzdálené ploše bude v angličtině nebo pomocí nástroje PsPing. Další informace o PsPing o [PsPing stáhnout stránky](https://technet.microsoft.com/sysinternals/psping.aspx)

    Můžete nebo odebrání pravidel NSG pomocí informací v výstup tento příkaz:

        Get-Help *-AzureRmNetworkSecurityRuleConfig
        

## <a name="considerations"></a>Co byste měli zvážit

Při odstraňování problémů s připojením je potřeba zvážit následující skutečnosti:

- Výchozí NSG pravidla se příchozí přístup z Internetu a pouze povolit VNet příchozí data. Pravidla má být explicitně přidán přístupu příchozí z Internetu, podle potřeby.
- Pokud žádná pravidla zabezpečení NSG příčinou připojení k síti OM selhání se problém může být termín splnění:
    - Brána firewall spuštěná v OM operační systém
    - Směruje nakonfigurovány virtuální zařízení nebo místní přenosy. Internetový provoz můžete přesměrováni do místního nasazení prostřednictvím vynucená tunneling. Připojení RDP/SSH z Internetu vaší OM nemusí fungovat s toto nastavení používají, podle toho, jak síťového hardwaru místní zpracovává tento přenos. Přečtěte si článek [Poradce při potížích s směruje](virtual-network-routes-troubleshoot-powershell.md) se dozvíte, jak diagnostikovat potíže směrování, které mohou být zpomalovat tok přenosy a odhlášení z OM. 
- Pokud máte peered VNets, ve výchozím nastavení, značku VIRTUAL_NETWORK se automaticky rozbalí a obsahují předpony pro peered VNets. Tyto předpony můžete zobrazit v seznamu **ExpandedAddressPrefix** při odstraňování všech problémů související s VNet prozkoumávání připojení. 
- Pravidla efektivní zabezpečení se zobrazí pouze pokud je NSG přidruženého OM NIC a nebo podsítě. 
- Pokud existují žádné NSGs přidružené NIC nebo podsítí a budete mít veřejnou IP adresu přiřazené k vaší OM, budou všechny porty otevřené příchozí a odchozí přístup pomocí protokolu. Pokud OM veřejnou IP adresu, použití NSGs NIC nebo podsítě důrazně doporučujeme.  
