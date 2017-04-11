<properties
   pageTitle="Služba Vyrovnávání zatížení více virtuální Azure | Microsoft Azure"
   description="Základní informace o více virtuální na Vyrovnávání zatížení Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="chkuhtz"
   manager="narayan"
   editor=""
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="chkuhtz"
/>

# <a name="multiple-vips-for-azure-load-balancer"></a>Služba Vyrovnávání zatížení více virtuální Azure

Azure Vyrovnávání zatížení vám umožní načíst rozložení stránky služby více portů a více IP adres. Definice Vyrovnávání zatížení veřejné a interní slouží k načtení zůstatek toků přes sadu VMs.

Tento článek popisuje základy tato možnost důležitých principů a omezení. Pokud chcete vystavit služby na IP adres, můžete najít, že zjednodušené pokyny k [veřejným](load-balancer-get-started-internet-portal.md) nebo [interní](load-balancer-get-started-ilb-arm-portal.md) načítat vyrovnávání konfigurace. Přidání více virtuální je mezní jednotné VIP. Používání koncepty v tomto článku, můžete rozbalit zjednodušenou konfiguraci kdykoli.

Když definujete Vyrovnávání zatížení Azure frontend a konfigurace back-end připojeni s pravidly. Stav zkušební odkazováno pravidlo slouží k určení jak nové toků se odesílají uzel ve fondu back-end. Frontend je definován tak, že virtuální IP (VIP), což je 3 n-tici tvořen IP adresy (veřejná nebo vnitřní), transport protocol (UDP nebo TCP) a číslo portu. DIP je IP adresa v Azure virtuální NIC připojené k OM ve fondu back-end.

Následující tabulka uvádí některé příklady frontend konfigurace:

| VIRTUÁLNÍ | IP adresa | Protocol (protokol) | port |
|-----|------------|----------|------|
|1|65.52.0.1|TCP|80|
|2|65.52.0.1|TCP|_8080_|
|3|65.52.0.1|_UDP_|80|
|4|_65.52.0.2_|TCP|80|

V tabulce jsou uvedeny čtyři různé frontends. Frontends #1 #2 a #3 jsou jediný VIP s více pravidly. Stejné adresy IP používá ale port a protokol pro každou frontend liší. Příklad více virtuální, kde stejný frontend protokol a port jsou opakovaně použít ve více virtuální jsou Frontends #1 a #4.

Azure Vyrovnávání zatížení poskytuje správcům flexibilitu při definování pravidla pro vyrovnávání zatížení. Pravidlo deklaruje, jak adresu a portu frontend namapované na cílovou adresu a portu back-end. Jestli jsou porty back-end opakovaně použít ve pravidla závisí na typu pravidla. Každý typ pravidla má zvláštní požadavky, které můžou ovlivnit hostitele konfigurace a zkušební návrh. Existují dva typy pravidel:

1. Výchozí pravidlo s opakované použití žádné back-end port
2. Kde jsou porty back-end opakovaně použít pravidlo plovoucí IP

Azure Vyrovnávání zatížení umožňuje kombinovat oba typy pravidel na stejnou konfiguraci Vyrovnávání zatížení. Vyrovnávání zatížení můžete používat současně pro dané OM nebo libovolnou kombinaci, dokud dodržovat omezení pravidlo. Jaký typ pravidla zvolíte závisí na požadavky aplikace a složitosti podporuje tuto konfiguraci. By měl být typů pravidlo, které jsou pro váš scénář Nejlepší.

Jsme prozkoumat další podobnému sledu začnete s výchozí chování.

## <a name="rule-type-1-no-backend-port-reuse"></a>Pravidlo typu #1: žádné opakovaně port back-end

![MultiVIP obrázku](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

V tomto scénáři virtuální frontend nakonfigurovány následujícím způsobem:

| VIRTUÁLNÍ | IP adresa | Protocol (protokol) | port |
|-----|------------|----------|------|
|![VIRTUÁLNÍ](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIRTUÁLNÍ](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

DIP je cílový příchozí směrování. Ve fondu back-end zpřístupňuje každý OM požadované služby na jedinečné na DIP. Tato služba je přidružený frontend prostřednictvím definice pravidla.

Je definována dvě pravidla:

| Pravidlo | Namapujte frontend | Do fondu back-end |
|------|--------------|-----------------|
| 1 | ![VIRTUÁLNÍ](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 | ![VIRTUÁLNÍ](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Úplné mapování Vyrovnávání zatížení Azure je teď následujícím způsobem:

| Pravidlo | Virtuální IP adresa | Protocol (protokol) | port | Cíl | port |
|------|----------------|----------|------|-----|------|
|![pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|DIP IP adresa|80|
|![pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|DIP IP adresa|81|

Všechna pravidla musí vytvořit toku kombinací jedinečné cílová IP adresa a cílový port. Změnou s cílovým portem toku více pravidel můžete dodat toků stejné DIP na různé porty.

Stav sond přesměrováni vždy DIP virtuálního počítače. Musíte se ujistit se, že vaše zkušební odráží stavu OM.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Pravidlo typu #2: opakovanému použití port back-end pomocí plovoucí IP

Azure Vyrovnávání zatížení poskytuje možnost znovu použít frontend port napříč několika virtuální bez ohledu na typ pravidla použít. Některé scénáře aplikace kromě toho raději nebo vyžadovat stejný port pro použití v několika instancích aplikace na jeden OM ve fondu back-end. Běžné příklady port opakovaně zahrnovat clusterů vysoké dostupnosti, sítě virtuální zařízení a zpřístupnění více koncové body TLS bez znovu zašifrován.

Pokud chcete znovu použít port back-end přes více pravidel, musíte povolit plovoucí IP v definici pravidlo.

Plovoucí IP je část co se označuje jako přímý serveru vrátit DSR (). DSR tvoří dvě části: toku topologie a IP adres schéma mapování. Na úrovni platformy Vyrovnávání zatížení Azure vždy funguje v topologii toku DSR bez ohledu na to, zda je povoleno plovoucí IP nebo ne. To znamená, že části odchozí toku vždy správně přepsat přímo na plovoucí dlaždice zpátky na počátek.

Výchozí typ pravidla zpřístupňuje Azure tradiční schématu vyrovnávání zatížení IP adresu mapování pro snadnější použití. Povolení plovoucí IP změní schéma mapování IP adresu povolit za účelem vyšší flexibility, jak je vysvětleno dále.

Následující obrázek znázorňuje tento konfigurace:

![MultiVIP obrázku](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

V tomto scénáři každé OM ve fondu back-end má tři rozhraní sítě:

* DIP: virtuální NIC přidružené OM (na Azure NIC zdroje)
* VIP1: smyčky rozhraním v rámci hosta nakonfigurovaném s IP adresu VIP1 OS
* VIP2: smyčky rozhraním v rámci hosta operačního systému, které je nakonfigurována IP adresu VIP2

>[AZURE.IMPORTANT] Konfigurace logické rozhraní se provádí v hosta OS. Konfigurace není provedených nebo spravuje Azure. Bez této konfiguraci nebude fungovat pravidla. Definice zkušební stavu používat DIP OM, nikoli logické VIP. Proto služby musí poskytovat zkušební odpovědi na DIP portu, které zobrazují stav služby, které na logické VIP.

Předpokládejme stejnou konfiguraci frontend jako předchozí situaci:

| VIRTUÁLNÍ | IP adresa | Protocol (protokol) | port |
|-----|------------|----------|------|
|![VIRTUÁLNÍ](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIRTUÁLNÍ](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

Je definována dvě pravidla:

| Pravidlo | Namapujte frontend | Do fondu back-end |
|------|--------------|-----------------|
| 1 | ![pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (v VM1 a VM2) |
| 2 | ![pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (v VM1 a VM2) |

Následující tabulka zobrazuje úplné mapování v Vyrovnávání zatížení:

| Pravidlo | Virtuální IP adresa | Protocol (protokol) | port | Cíl | port |
|------|----------------|----------|------|-------------|------|
|![VIRTUÁLNÍ](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|stejný jako VIP (65.52.0.1)|stejný jako VIP (80)|
|![VIRTUÁLNÍ](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|stejný jako VIP (65.52.0.2)|stejný jako VIP (80)|

Určení toku příchozích je adresa VIP rozhraní smyčky v OM. Všechna pravidla musí vytvořit tok kombinací jedinečné cílová IP adresa a cílový port. Změnou cílová IP adresa toku je možné na stejné OM port opakovaně. Služby vystaven Vyrovnávání zatížení vazbou VIP IP adresa a port rozhraní odpovídajících smyčky.

Všimněte si, že v tomto příkladě se nezmění s cílovým portem. I když je to scénáři plovoucí IP, Vyrovnávání zatížení Azure definování pravidla přepište s cílovým portem back-end a aby se liší od s cílovým portem frontend taky podporuje.

Typ pravidla plovoucí IP se základem několik vzorků konfigurace vyrovnávání zatížení. Příklad, která je aktuálně dostupná je konfigurace [AlwaysOn SQL s více posluchače](../virtual-machines/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) . V průběhu času bude uvádíme některé podobnému sledu.

## <a name="limitations"></a>Omezení

* Více konfigurací VIP podporují pouze s IaaS VMs.
* Při použití pravidla plovoucí IP aplikace používají DIP pro odchozí toků. Pokud aplikace vytvoří vazbu s adresou VIP nakonfigurovanou rozhraní smyčky v hosta OS, pak SNAT není k dispozici přepište odchozí tok a tok se nezdaří.
* Veřejné adresy IP mít vliv na fakturace. Další informace najdete v tématu [IP adresu ceny](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Platí limity předplatného. Další informace najdete v tématu [limity služby](../azure-subscription-service-limits.md#networking-limits) podrobnosti.
