<properties 
   pageTitle="Hybridní připojení s aplikací 2 osy | Microsoft Azure"
   description="Naučte se nasadit virtuální zařízení a UDR k vytvoření vícevrstvého aplikace prostředí v Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/05/2016"
   ms.author="jdial" />

# <a name="virtual-appliance-scenario"></a>Scénář virtuální zařízení

Běžné situace mezi větší Azure zákazníka je potřeba aplikaci dvouvrstvý vystaven na Internetu zároveň přístup na zpět osy z místního datacentra. Tento dokument vás provede jednotlivými situace pomocí uživatele definované směruje (UDR), brána VPN a virtuální spotřebiče sítě nasazení dvou prostředí, který splňuje následující požadavky:

- Webová aplikace musí být přístupné z veřejné Internetu.
- Webový server hostující aplikace musíte mít přístup k back-end aplikační server.
- Všechny přenosy z Internetu na webové aplikace musí absolvovat virtuální zařízení brány firewall. Tento virtuální zařízení se použije pro internetový provoz pouze.
- Všechny přenosy aplikační server musí absolvovat virtuální zařízení brány firewall. Tento virtuální zařízení se používat pro přístup k serveru end back-end a přístup přichází místní síti prostřednictvím sítě VPN brány.
- Správci musí mít možnost spravovat virtuální zařízení brány firewall ze svého místního počítače, pomocí třetí brána firewall virtuální zařízení použít pouze pro účely správy.

Toto je standardní scénář DMZ s DMZ a chráněné sdílené síťové. Tato situace může vytvořen v Azure pomocí NSGs, virtuální zařízení brány firewall nebo kombinací obou. Následující tabulka ukazuje některé výhody a nevýhody mezi NSGs a virtuální zařízení brány firewall.

||V oblasti IT|Nevýhody|
|---|---|---|
|NSG|Žádné náklady. <br/>Integrovaný Azure RBAC. <br/>Pravidla můžete vytvořit v ARM šablony.|Složitost může být různé v větší prostředí. |
|Brána firewall|Úplnou kontrolu nad dat rovině. <br/>Centrální správa prostřednictvím konzoly bránu firewall.|Náklady na zařízení brány firewall. <br/>Není integrovaný s Azure RBAC.|

Řešení dole používá virtuální zařízení brány firewall implementovat scénáři DMZ/chráněné sítě.

## <a name="considerations"></a>Co byste měli zvážit

Můžete nasadit prostředí vysvětleny výše v Azure pomocí různých funkcí k dispozici dnes, následujícím způsobem.

- **Virtuální sítě (VNet)**. Azure VNet funguje podobným způsobem k místní síti a můžete rozdělena na jedné nebo více podsítí poskytovat přenosy izolace a oddělení pochybnosti.
- **Virtuální zařízení**. Několik partneři poskytují virtuální zařízení v Azure Marketplace, který se dá použít pro tyto tři brány firewall jsme je popsali výše. 
- **Uživatelem definované směruje (UDR)**. Směrování tabulky může obsahovat UDRs používaný Azure networking řízení toku paketů v rámci VNet. V této tabulce směrování se dají použít k podsítí. Jednou z nejnovější funkce v Azure je možnost tabulku směrování GatewaySubnet poskytuje možnost všechny přenosu přichází do Azure VNet připojení hybridní virtuální zařízení.
- **IP přesměrování**. Ve výchozím nastavení modul Azure sítě předat dál paketů virtuální sítě rozhraní kartami () jenom v případě, že paketu cílová IP adresa odpovídá NIC IP adresu. Proto pokud UDR definuje paket musí být odeslání dané virtuální spotřebiče, modul Azure sítě by přetáhnout paketu. Abyste měli jistotu, že paketu dostane OM (v tomto případě virtuální zařízení), která není skutečné cílový paketu, budete muset povolit přesměrování IP pro virtuální zařízení.
- **Skupiny zabezpečení síti (NSGs)**. V příkladu níže není můžete využít NSGs, ale můžete použít NSGs použít podsítí nebo nic v tomto řešení k dalšímu filtrování přenosy a odhlášení z těchto podsítí a nic.


![Připojení pomocí protokolu IPv6](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

V tomto příkladu je předplatné, které obsahuje následující:

- 2 skupiny zdrojů, není vidět v diagramu. 
    - **ONPREMRG**. Obsahuje všechny materiály potřebné tak, aby napodobily v místní síti.
    - **AZURERG**. Obsahuje všechny materiály potřebné pro Azure prostředí virtuální sítě. 
- VNet s názvem **onpremvnet** napodobovat místní datacentra segmentována podle těchto pokynů.
    - **onpremsn1**. Podsítě obsahující virtuální počítač (OM) se systémem Ubuntu napodobit místního serveru.
    - **onpremsn2**. Podsítě obsahující OM systém systémem Ubuntu napodobit místním počítači používá správce.
- Je jeden virtuální zařízení brány firewall s názvem **OPFW** na **onpremvnet** využít ke správě tunelem k **azurevnet**.
- VNet s názvem **azurevnet** segmentována podle těchto pokynů.
    - **azsn1**. Externí bránu firewall podsítě výhradně pro externí bránu firewall. Veškerý internetový provoz chodily prostřednictvím tohoto podsítě. Tento podsítě obsahuje pouze NIC propojen externí bránu firewall.
    - **azsn2**. Front-end podsítě hostingu OM spuštěný jako webový server, který se k nim získat přístup z Internetu.
    - **azsn3**. Back-end podsítě hostingu OM serverem back-end aplikace, která budou používána v front-end webový server.
    - **azsn4**. Správa podsítě používat výhradního přístupu ke správě pro všechna virtuální zařízení brány firewall. Tento podsítě obsahuje pouze NIC pro každého virtuální zařízení brány firewall použité v řešení.
    - **GatewaySubnet**. Připojení podsítě Azure hybridní třeba ExpressRoute a Brána VPN zadat propojení mezi Azure VNets a jiných sítích. 
- Existuje 3 virtuální zařízení brány firewall v síti **azurevnet** . 
    - **AZF1**. Externí bránu firewall vystaven na Internetu veřejné pomocí veřejných zdrojů IP adresu v Azure. Je třeba zajistit, že máte šablony z Marketplace nebo přímo z dodavatele zařízení, které ustanovení 3-NIC virtuální zařízení.
    - **AZF2**. Slouží k určení komunikaci mezi **azsn2** a **azsn3**interní bránou firewall. Toto je také 3-NIC virtuální zařízení.
    - **AZF3**. Správa firewall přístupné pro správce z místního datacentra a připojené k řízení podsítě sloužící ke správě všechna zařízení brány firewall. 2 – NIC virtuální zařízení šablony najdete ve Tržiště nebo ho přímo z vašeho zařízení dodavatele vyžádat.

## <a name="user-defined-routing-udr"></a>Uživatelem definované směrování (UDR)

Každý podsítě v Azure jde propojit s tabulkou UDR použije jak přenosy, které iniciuje v tom, že podsítě směrován. Pokud jsou definovány žádné UDRs, Azure k přenosy flow z jednoho podsítě použije výchozí trasy. Chcete-li lépe porozumět tomu UDRs, navštěvujte blog o [Co jsou definované směruje uživatele a IP přesměrování](./virtual-networks-udr-overview.md#ip-forwarding).

Zajistit komunikace probíhá prostřednictvím zařízení správné brány firewall podle výše uvedená poslední požadavek je potřeba vytvořit směrování tabulce obsahující UDRs v **azurevnet**.

### <a name="azgwudr"></a>azgwudr

V tomto scénáři pouze přenosy vyplývající z místního Azure se použijí k připojení k **AZF3**spravovat tyto brány firewall a která vysílání musíte přejít přes interní bránu firewall, **AZF2**. Jedinou směrování tedy v **GatewaySubnet** , jak je ukázáno v následujícím příkladu je nutné.

|Cíl|Přesměrování|Vysvětlení|
|---|---|---|
|10.0.4.0/24|10.0.3.11|Umožňuje místní přenos kontaktovat správy firewall **AZF3**|

### <a name="azsn2udr"></a>azsn2udr

|Cíl|Přesměrování|Vysvětlení|
|---|---|---|
|10.0.3.0/24|10.0.2.11|Umožňuje umožnění datových přenosů do back-end podsítě hostingu aplikační server prostřednictvím **AZF2**|
|0.0.0.0/0|10.0.2.10|Umožňuje všechny přenosy na směrovány **AZF1**|

### <a name="azsn3udr"></a>azsn3udr

|Cíl|Přesměrování|Vysvětlení|
|---|---|---|
|10.0.2.0/24|10.0.3.10|Umožňuje přenos **azsn2** tok z app serveru a webserver prostřednictvím **AZF2**|

Potřebujete vytvořit směrování tabulky pro podsítí v **onpremvnet** napodobit místní datacentra.

### <a name="onpremsn1udr"></a>onpremsn1udr

|Cíl|Přesměrování|Vysvětlení|
|---|---|---|
|192.168.2.0/24|192.168.1.4|Umožňuje přenos **onpremsn2** prostřednictvím **OPFW**|

### <a name="onpremsn2udr"></a>onpremsn2udr

|Cíl|Přesměrování|Vysvětlení|
|---|---|---|
|10.0.3.0/24|192.168.2.4|Umožňuje přenos zálohované podsítě v Azure pomocí **OPFW**|
|192.168.1.0/24|192.168.2.4|Umožňuje přenos **onpremsn1** prostřednictvím **OPFW**|

## <a name="ip-forwarding"></a>Předávání IP 

UDR a přesměrování IP jsou funkce, které můžete použít v kombinaci umožňuje virtuální spotřebiče, který má být slouží k určení přenos v Azure VNet.  Virtuální zařízení je nic jiného než OM, které se spustí aplikace, která slouží ke zpracování v síti nějak, například bránu firewall nebo zařízení.

Tento virtuální zařízení OM mají dostávat příchozí přenos, který není určená k sobě. Pokud chcete povolit OM pro příjem vysílání adresovanou na jiná místa, musíte povolit přesměrování IP pro OM. Toto je Azure nastavení, nikoli hostovaného operačního systému. Virtuální zařízení je ještě potřeba spustit některé aplikace ke zpracování příchozích a směrování řádně podporovat.

Další informace o přesměrování IP, navštěvujte blog o [Jaké směruje definované uživatele a IP přesměrování](./virtual-networks-udr-overview.md#ip-forwarding).

Jako příklad Představte si, že v Azure vnet máte následující nastavení:

- Podsítě **onpremsn1** obsahuje OM s názvem **onpremvm1**.
- Podsítě **onpremsn2** obsahuje OM s názvem **onpremvm2**.
- Virtuální zařízení s názvem **OPFW** je a připojovat se k **onpremsn1** **onpremsn2**.
- Uživatelem definované propojené s **onpremsn1** trasy, musí být všechny přenosy na **onpremsn2** odesílají **OPFW**.

V tomto okamžiku **onpremvm1** se snaží navázat spojení s **onpremvm2**, se použije UDR a přenosy se pošle **OPFW** jako přesměrování. Mějte na paměti, že není změněné cíle skutečné paketu, pořád s textem, že **onpremvm2** je cíle. 

Bez IP přesměrování aktivované **OPFW**Azure virtuální sítě logickou bude, a vložte pakety, protože dovoloval paketů zasílané do virtuálního počítače, pokud OM IP adresu cíl paketu.

S přesměrování IP logiky Azure virtuální sítě předává pakety OPFW, aniž byste změnili jeho původní cílovou adresu. **OPFW** musí zpracovat pakety a určit, co dělat s nimi.

Scénář výše pracovat, musíte povolit IP přesměrování na nic **OPFW**, **AZF1**, **AZF2**a **AZF3** , které slouží ke směrování (všechny nic s výjimkou těch, které jsou propojené s podsítě správy). 

## <a name="firewall-rules"></a>Pravidla brány firewall

Jak jsme je popsali výše, IP přesměrování pouze zajistí paketů na kterou se odesílají virtuální zařízení. Zařízení je ještě potřeba rozhodnout, co dělat s tyto pakety. Scénář nahoře je potřeba vytvořit následující pravidla do vašeho zařízení:

### <a name="opfw"></a>OPFW

OPFW představuje zařízení s místním obsahující následující pravidla:

- **Postup**: všechny přenosy na 10.0.0.0/16 (**azurevnet**) musí být odeslána tunelem **ONPREMAZURE**.
- **Zásady**: umožňovala všechny obousměrnou komunikaci mezi **port2** a **ONPREMAZURE**.
 
### <a name="azf1"></a>AZF1

AZF1 představuje Azure virtuální spotřebiče obsahující následující pravidla:

- **Zásady**: umožňovala všechny obousměrnou komunikaci mezi **port1** a **port2**.

### <a name="azf2"></a>AZF2

AZF2 představuje Azure virtuální spotřebiče obsahující následující pravidla:

- **Postup**: všechny přenosy na 10.0.0.0/16 (**onpremvnet**) musí odeslané na IP adresu Azure brány (tedy 10.0.0.1) až **port1**.
- **Zásady**: umožňovala všechny obousměrnou komunikaci mezi **port1** a **port2**.

## <a name="network-security-groups-nsgs"></a>Skupiny zabezpečení síti (NSGs)

V tomto scénáři NSGs nepoužívají. Však může použít NSGs do každého podsítě omezit příchozí a odchozí přenosy. Například můžete použít následující pravidla NSG externí podsítě Přesměrování.

**Příchozí pošty**

- Povolte všechny přenosy protokolu TCP z Internetu pro port 80 v libovolné OM podsítě.
- Zakázat všechny ostatní přenosů z Internetu.

**Odchozí**
- Zakázat všechny přenosy k Internetu.

## <a name="high-level-steps"></a>Vysoké úrovně kroky

Abyste mohli nasadit tento scénář, postupujte následujícím způsobem nejvyšší úrovně.

1.  Přihlášení k předplatnému Azure.
2.  Pokud chcete nasadit VNet napodobit v místní síti, zřízení prostředky, které jsou součástí **ONPREMRG**.
3.  Zřízení prostředky, které jsou součástí **AZURERG**.
4.  Poskytování tunelem z **onpremvnet** k **azurevnet**.
5.  Po všechny zdroje jsou zřízení, přihlaste se k **onpremvm2** a otestujte příkazem ping 10.0.3.101 test připojení mezi **onpremsn2** a **azsn3**.
