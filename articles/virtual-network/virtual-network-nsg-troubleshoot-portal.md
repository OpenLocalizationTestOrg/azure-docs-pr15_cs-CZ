<properties 
   pageTitle="Poradce při potížích s skupiny zabezpečení sítě – portál | Microsoft Azure"
   description="Zjistěte, jak řešit problémy s skupiny zabezpečení sítě v modelu nasazení Azure správce na portálu Azure."
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

# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Poradce při potížích s skupiny zabezpečení sítě pomocí portálu Azure

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

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Zobrazit efektivní zabezpečení pravidla pro virtuálního počítače

Proveďte následující postup řešení problémů s NSGs pro virtuálního počítače:

Zobrazit úplný seznam pravidel efektivní zabezpečení na NIC z OM samotné. Můžete taky přidat, upravit a NIC a podsítě NSG pravidla zabránění zásuvné efektivní pravidla Pokud máte oprávnění k provedení těchto operací.

1. Přihlaste se k portálu Microsoft Azure na https://portal.azure.com.
2. Klikněte na **Další služby**a potom v zobrazeném seznamu klikněte na **virtuálních počítačích** .
3. Vyberte OM řešit problémy s ze seznamu, který se zobrazí a zobrazí se OM zásuvné s možnostmi.
4. Klikněte na **diagnostikování & řešení problémů** a pak vyberte běžných problémů. V tomto příkladu **nelze se připojit ke Moje Windows OM** zaškrtnuté. 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)

5. Kroky se zobrazí pod problém, jak je znázorněno na následujícím obrázku: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)

    Klikněte na *pravidla skupiny efektivní zabezpečení* v seznamu doporučené kroky.

6. Zobrazí se zásuvné **získat pravidla efektivní zabezpečení** , jak je znázorněno na následujícím obrázku:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)

    Všimněte si v následujících částech obrázku:

    - **Obor:** Nastavit jiné *VM1*, OM vybrali v kroku 3.
    - **Rozhraní sítě:** *VM1 NIC1* zaškrtnuté. Virtuálního počítače může obsahovat více síťových rozhraní (NIC). Každý NIC může obsahovat jedinečné efektivní zabezpečení pravidla. Při odstraňování potíží, budete potřebovat zobrazíte pravidla efektivní zabezpečení pro každou NIC.
    - **Přidružené NSGs:** NSGs se dají použít NIC a podsítě, se kterými NIC spojení. Na obrázku které byly použity pro NIC a podsítě, ke kterému je připojen k NSG. Kliknete na názvy NSG přímo upravit v NSGs.
    - **Kartu VM1 nsg:** Seznam pravidla zobrazená v obrázku je určený pro NSG použité u síťovou Několik výchozí pravidla jsou vytvářeny Azure pokaždé, když se vytvoří NSG. Nemůžete odebrat výchozí pravidla, ale je možné přepsat s pravidly vyšší prioritu. Další informace o pravidlech výchozí najdete v článku [Přehled NSG](virtual-networks-nsg.md#default-rules) .
    - **CÍLOVÉM sloupci:** Některá pravidla mít text ve sloupci, zatímco ostatní mají předpony adres. Text je název značky výchozí použit na pravidlo zabezpečení, když byl vytvořen. Značky jsou poskytnuté systému, které představují více předpony. Vyberte pravidlo se značkou, například *AllowInternetOutBound*seznamy předpony v zásuvné **předpony adres** .
    - **Stáhnout:** Seznam pravidel mohou být dlouhé. Stažení souboru .csv pravidel pro offline analýzu kliknutím na **Stáhnout** a uložení souboru.
    - **AllowRDP** Příchozí pravidla: Pokud chcete toto pravidlo umožňuje připojení RDP bude v angličtině.
7. Klikněte na kartu **Podsíť1 NSG** zobrazíte efektivní pravidla z NSG použité u podsítě, jak je znázorněno na následujícím obrázku: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)

    Všimněte si *denyRDP* **příchozí** pravidla. Příchozí pravidla používaná při podsítě jsou vyhodnoceny před pravidla používaná při rozhraní sítě. Vzhledem k tomu, použije se pravidlo odepřít v podsítě, žádost o připojení k TCP 3389 se nezdaří, protože povolit pravidla v NIC nikdy Vyhodnocená každá její položka. 

    Pravidlo *denyRDP* je důvod, proč se nedaří RDP připojení. Odebrání by měl problém vyřešit.

    >[AZURE.NOTE]Pokud OM přidružené NIC není spuštěna nebo nenainstalovali NSGs NIC nebo podsítě, zobrazí se žádné pravidlo.

8. Chcete-li upravit NSG pravidla, klikněte na *Podsíť1 NSG* v části **Související NSGs** .
   Otevře se zásuvné **Podsíť1 NSG** . Pravidla můžete upravovat přímo kliknutím na **zabezpečení příchozí pravidla**.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)

9. Po odebrání příchozí pravidla *denyRDP* **Podsíť1 NSG** a přidání pravidlo *allowRDP* , seznamu efektivní pravidel vypadá jako na následujícím obrázku:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)

    Zkontrolujte, zda TCP port 3389 otevřít otevřením připojení ke vzdálené ploše bude v angličtině nebo pomocí nástroje PsPing. Další informace o PsPing o [PsPing stáhnout stránky](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="view-effective-security-rules-for-a-network-interface"></a>Zobrazit efektivní zabezpečení pravidla pro rozhraní sítě

Pokud vaše OM tok je dopad pro konkrétní NIC, můžete zobrazit úplný seznam efektivní pravidla pro NIC z místní síti rozhraní podle těchto kroků:

1. Přihlaste se k portálu Microsoft Azure na https://portal.azure.com.
2. Klikněte na **Další služby**a potom na **Síťová rozhraní** v zobrazeném seznamu.
3. Vyberte rozhraní sítě. Na následujícím obrázku je vybrána NIC s názvem *VM1 NIC1* .

    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)

    Všimněte si, že je nastavený **obor** rozhraní sítě vybrané. Další informace o další informace najdete v tématu kroku 6 v části **Poradce při potížích s NSGs pro virtuálního počítače** tohoto článku.

    >[AZURE.NOTE] Pokud NSG se odebere z rozhraní sítě, podsítě NSG je pořád efektivní v dané NIC. Výstup v tomto případě bude ukazovat jenom pravidla z podsítě NSG. Pravidla se zobrazí, pouze pokud NIC je připojen k virtuálního počítače.

4. NSGs související s NIC a podsítě můžete upravovat přímo pravidla. Se dozvíte, jak, přečtěte si krok 8 **Zobrazit efektivní zabezpečení pravidla virtuálního počítače v** části tohoto článku.

## <a name="view-effective-security-rules-for-a-network-security-group-nsg"></a>Zobrazit efektivní zabezpečení pravidla pro skupinu zabezpečení sítí (NSG)

Při úpravách NSG pravidla, budete chtít zkontrolovat dopad pravidla vás někdo přidá na konkrétní OM. Můžete zobrazit úplný seznam pravidel efektivní zabezpečení pro všechny nic použité dané NSG, aniž byste museli přepnout kontext z dané zásuvné NSG. Řešení problémů s efektivní pravidla v rámci NSG, proveďte následující kroky:

1. Přihlaste se k portálu Microsoft Azure na https://portal.azure.com.
2. Klikněte na **Další služby**a potom v zobrazeném seznamu klikněte na **skupiny zabezpečení sítě** .
3. Vyberte NSG. Na následujícím obrázku je vybraný NSG s názvem VM1 nsg.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)

    Všimněte si v následujících částech na předchozím obrázku:

    - **Obor:** Nastavte, aby NSG vybrané.
    - **Virtuálního počítače:** Když použijete NSG je podsítě, bude použit pro všechna síťová rozhraní připojené k všechny VMs připojené k podsítě. Tento seznam uvádí všechny VMs tento NSG se použije pro. Vyberte libovolnou OM ze seznamu.

    >[AZURE.NOTE] Pokud NSG se použije jenom prázdné podsítě, VMs se neobjeví. Pokud NSG se použije na NIC, který není přidružený k virtuálního počítače, nebude uvedené taky tyto nic. 
    - **Sítě rozhraní:** Virtuálního počítače může obsahovat více síťových rozhraní. Můžete vybrat síťové rozhraní připojené k vybrané OM.
    - **AssociatedNSGs:** Kdykoli NIC může mít až dva efektivní NSGs, použité NIC a druhý do podsítě. Přestože obor je zúžený VM1 nsg, pokud NIC má efektivní podsítě NSG, zobrazí výstup obou NSGs.
4. NSGs NIC nebo podsítě můžete upravovat přímo pravidla. Se dozvíte, jak, přečtěte si krok 8 **Zobrazit efektivní zabezpečení pravidla virtuálního počítače v** části tohoto článku.

Další informace o další informace najdete v tématu kroku 6 v části **Zobrazit efektivní zabezpečení pravidla pro virtuální počítač** tohoto článku.

>[AZURE.NOTE] Když podsítí a NIC každý může mít jenom jeden je použita NSG, může být přidružené k více nic a více podsítí NSG.

## <a name="considerations"></a>Co byste měli zvážit

Při odstraňování problémů s připojením je potřeba zvážit následující skutečnosti:

- Výchozí NSG pravidla se příchozí přístup z Internetu a pouze povolit VNet příchozí data. Pravidla má být explicitně přidán přístupu příchozí z Internetu, podle potřeby.
- Pokud žádná pravidla zabezpečení NSG příčinou připojení k síti OM selhání se problém může být termín splnění:
    - Brána firewall spuštěná v OM operační systém
    - Směruje nakonfigurovány virtuální zařízení nebo místní přenosy. Internetový provoz můžete přesměrováni do místního nasazení prostřednictvím vynucená tunneling. Připojení RDP/SSH z Internetu vaší OM nemusí fungovat s toto nastavení používají, podle toho, jak síťového hardwaru místní zpracovává tento přenos. Přečtěte si článek [Poradce při potížích s směruje](virtual-network-routes-troubleshoot-powershell.md) se dozvíte, jak diagnostikovat potíže směrování, které mohou být zpomalovat tok přenosy a odhlášení z OM. 
- Pokud máte peered VNets, ve výchozím nastavení, značku VIRTUAL_NETWORK rozbalíte automaticky mají být předpony peered VNets. Tyto předpony můžete zobrazit v seznamu **ExpandedAddressPrefix** při odstraňování všech problémů související s VNet prozkoumávání připojení. 
- Pravidla efektivní zabezpečení se zobrazí pouze pokud je NSG přidruženého OM NIC a nebo podsítě. 
- Pokud existují žádné NSGs přidružené NIC nebo podsítí a budete mít veřejnou IP adresu přiřazené k vaší OM, budou všechny porty otevřené příchozí a odchozí přístup pomocí protokolu. Pokud OM veřejnou IP adresu, použití NSGs NIC nebo podsítě důrazně doporučujeme.