<properties
   pageTitle="Provádění hybridní architektura sítě s Azure a místních VPN | Microsoft Azure"
   description="Jak implementovat Architektura zabezpečení webu webu sítě, který přesahuje Azure virtuální sítě a v místní síti připojení prostřednictvím sítě VPN."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
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
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>Provádění hybridní architektura sítě s Azure a místních VPN

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tento článek shrnuje sadu postupy pro rozšíření v místní síti na Azure pomocí na webu webu virtuální privátní sítě (VPN). Přenos přetékat mezi místní síti a k Azure virtuální síti (VNet) tunelem IPSec VPN. Tato architektura je vhodná pro hybridní aplikace s následujícími vlastnostmi:

- Části aplikace spustit místní, zatímco ostatní se spouštějí v Azure.

- Přenos mezi místním hardware a cloudu je pravděpodobně light nebo je výhodné obchodu mírně rozšířené latence Flexibilita a zpracování power v cloudu.

- Rozšířené sítě představuje uzavřený systém. Neexistuje žádná přímé cesta z Internetu na Azure VNet.

- Uživatelé připojit k místní síti k používání služeb hostovaných v Azure. Most mezi místní síti a služeb spuštěných ve Azure je průhledné uživatelům.

Příklady situací, které odpovídají profil patří:

- Řádek z podnikových aplikací použít v organizaci, kde má součást funkce migraci do cloudu.

- Vývojové laboratorní úloh.

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků Azure] [ resource-manager-overview] a klasické. Tento plán používá správce zdroje, který Microsoft doporučuje nové nasazení.

## <a name="architecture-diagram"></a>Diagram architektury

V následujícím diagramu jsou uvedeny součástí tohoto architektura:

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento graf je ve "hybridní síti - VPN" stránky.

![[0]][0]

- **V místní síti.** Toto je síť počítačích a zařízeních připojení přes privátní sítě místní systém v rámci organizace.

- **Virtuální privátní sítě zařízení.** Toto je zařízení nebo služba, která poskytuje externí připojení k místní síti. Zařízení VPN mohou být zařízení nebo může být softwarové řešení například směrování a vzdáleného přístupu služby () v systému Windows Server 2012.

> [AZURE.NOTE] Seznam podporovaných spotřebiče VPN a informace o konfiguraci vybrané VPN zařízení pro připojení k bráně VPN Azure, naleznete v pokynech k příslušné zařízení v [seznamu zařízení VPN podporovaných Azure][vpn-appliance].

- **N-osy cloudu aplikace.** Toto je aplikace umístěná v Azure. Může obsahovat více úrovní s více podsítí připojení přes Vyrovnávání zatížení Azure. Přenosy v každé podsítě vyměřit poplatky pravidla definované pomocí [Azure sítě skupin zabezpečení (NSGs)][azure-network-security-group]. Další informace najdete v tématu [Začínáme s Microsoft Azure zabezpečením][getting-started-with-azure-security].

> [AZURE.NOTE] Tento článek popisuje aplikaci cloudu jako jedna entita. V tématu [spuštění N-vrstvy architektury na Azure] [ implementing-a-multi-tier-architecture-on-Azure] podrobné informace.

- **[Virtuální sítě (VNet)][azure-virtual-network].** Aplikace cloudu a součásti brány VPN Azure umístěny ve stejné VNet.

- **[Brána Azure VPN][azure-vpn-gateway].** Služba brány VPN umožňuje připojit VNet v místní síti prostřednictvím VPN zařízení. Další informace najdete v článku [připojení v místní síti k síti virtuální Microsoft Azure][connect-to-an-Azure-vnet]. Brána VPN zahrnuje následující prvky:

  - **Virtuální sítě brány.** Toto je zdroj, který obsahuje virtuální zařízení VPN VNet. Je zodpovědní za směrování přenosu v místní síti VNet.

  - **Brána pro místní síti.** Toto je odběru spotřebiče VPN místní. V síti z aplikace cloudu pro místní síti směrovaná přes tento brány.

  - **Připojení.** Připojení má vlastnosti, které určují připojení typ (IPSec) a klávesu nasdílel zařízení VPN místní šifrovat komunikaci.

  - **Podsítě brány.** Virtuální sítě brány směřuje v samostatném podsítě, což je vyměřené poplatky za jeho různé požadavky, jak je popsáno v části doporučení.

- **Vyrovnávání zatížení vnitřní.** V síti z brány VPN směrován ke cloudové aplikaci prostřednictvím Vyrovnávání zatížení vnitřní. Vyrovnávání zatížení se nachází v front-end podsítě aplikace.

## <a name="recommendations"></a>Doporučení

Azure nabízí mnoho různých zdrojů a typů zdrojů, takže může být tento odkaz architektura zřízení mnoha různými způsoby. Správce prostředků Azure šablonu nainstalovat architektura odkaz, který následuje těmito doporučeními uvádíme. Pokud budete chtít vytvořit vlastní odkaz architektura Pokud požadujete konkrétní příliš velký doporučení by měl těmito doporučeními.

### <a name="vnet-and-gateway-subnet"></a>Podsítě VNet a brány

Vytvoření Azure VNet při blokování součásti v cloudu. Adresní prostor Azure VNet musí být dostatečně velký k tomu tak, aby zahrnoval adresy používané VMs a podsítí v VNet. Zkontrolujte, že VNet adresní prostor dostatek místa pro růst Pokud další VMs mohou být potřeba v budoucnu. Adresní prostor VNet nesmí překrývat s místní síti. Například diagramu nahoře používá 10.20.0.0/16 místo adresu pro VNet.

Vytvoření podsítě s názvem _GatewaySubnet_rozsah adres /27. Tento podsítě je potřeba brány virtuální sítě a přidělení 32 adresy tak, aby tento podsítě vám pomůže tak, aby spuštěný s desktopovým omezení velikosti možné brány v budoucnu. Vyhněte se zaškrtnete toto podsítě uprostřed adresní prostor. Vhodné je nastavení adresní prostor pro podsítě brány na konci horní VNet adresní prostor. V diagramu příkladu 10.20.255.224/27.  Rychlý postup pro výpočet CIDR vypadá takto:

1. Nastavte proměnnou bitů adresní prostor VNet 1 až bitů se používá podsítě brány a potom zbývajících bitů na hodnotu 0.

2. Převod výsledné bitů na desítkové a express jako adresní prostor se sadou délka předponu velikosti podsítě brány.

Například pro VNet s rozsah IP adres 10.20.0.0/16 použití kroku #1 bude 10.20.0b11111111.0b11100000.  Převod, desetinná a vyjadřující, jak adresní prostor dává 10.20.255.224/27

> [AZURE.WARNING] Nasazení jiných virtuálních počítačích nebo role instance podsítě brány. Navíc není přiřadit NSG tento podsítě jako způsobí brány přestanou fungovat.

### <a name="virtual-network-gateway"></a>Virtuální sítě brány

Přiřaďte veřejnou IP adresu brány virtuální sítě.

Vytvoření brány virtuální sítě v podsítě brány a přiřadit ji nově přidělené veřejnou IP adresu. Použití typu brány, která nejvíc odpovídá vašim požadavkům a který je povolené VPN zařízení:

Vytvoření [brány zásadami] [ policy-based-routing] v případě potřeby přesně určit, jak jsou požadavky na směrovány. na základě zásad kritérií například předpony adres. Zásadami brány pomocí statické směrování a fungují jen pro připojení k webu.

Vytvoření [brány pro směrování] [ route-based-routing] připojit k místní síti pomocí RRAS podporu více webu nebo více oblastí připojení, zda implementace VNet VNet připojení (včetně postupy, které procházet více VNets). Na základě směrování brány musí používat dynamické směrování přímé přenosu mezi sítě. Vzhledem k tomu můžete zkusit alternativní cesty budou nevadí vám k chybám v lepší než statické trasy, síťové cesty. Na základě směrování bran můžete taky snížení režijních správy, protože směruje nebudete potřebovat ručně aktualizovat, pokud dojde ke změně adresy sítě.

Seznam podporovaných VPN spotřebiče, najdete v článku [o VPN zařízení pro připojení VPN brány na webu][vpn-appliances].

> [AZURE.NOTE] Po vytvoření brány nemůžete změnit mezi brány typy bez odstraníte a znovu vytvořit brány.

Vyberte Azure VPN brány skladové jednotky, která nejvíc odpovídá vašim požadavkům na výkon. Azure VPN brána je k dispozici ve třech SKU uvedené v následující tabulce. Každý SKU obsahuje různé výkon [ukládají poplatky] [ azure-gateway-charges] podle množství času, který máte k dispozici brány a k dispozici.

| SKU | Výkon sítě VPN | Propojení Max IPSec|
|-----|----------------|------------------|
| Základní | 100 MB / | 10 |
| Standardní | 100 MB / | 10 |
| Vysoký výkon | 200 MB / | 30 |

> [AZURE.NOTE] Základní SKU není kompatibilní se službou Azure ExpressRoute. Můžete [změnit Skladovou] [ changing-SKUs] po vytvoření brány.

Vytvoření pravidla směrování podsítě brány přímé příchozí provozu aplikací z bránu Vyrovnávání zatížení interní namísto povolení žádosti o předat VMs implementující aplikace.

### <a name="on-premises-network-connection"></a>Místní připojení k síti

Vytvoření brány pro místní síti. Zadejte veřejnou IP adresu zařízení VPN místní a adresní prostor v místní síti. Všimněte si, že zařízení VPN místní musí mít veřejnou IP adresy, kterou lze přistupovat pomocí místní síti brány v Azure VPN brány. Zařízení VPN nemohou být umístěny za zařízení.

Vytvoření připojení k webu pro brány virtuální sítě a branou místní síti. Vyberte typ připojení webu (IPSec) a zadejte sdíleného klíče. Šifrování webu webu Azure VPN brána je založeno na protokol IPSec ověřování pomocí předem sdílené klíče. Klíč zadáváte při vytváření Brána VPN Azure. Musíte nakonfigurovat VPN zařízení s místním se stejným klíčem. Další metody ověřování aktuálně nepodporuje.

Zkontrolujte, zda místní, které infrastruktury směrování nakonfigurovaný tak, aby přeposílání žádostí o určené pro adresy v Azure VNet VPN zařízení.

Otevřete libovolnou porty vyžaduje aplikace cloudu v místní síti.

Test připojení ověřte, že:

- Místní VPN zařízení správně přesměrovává přenosy ke cloudové aplikaci prostřednictvím sítě VPN brány Azure.

- VNet správně přesměrovává přenosy zpět na místní síti.

- Zakázané přenosy v obou směrech blokovány správně.

## <a name="scalability-considerations"></a>Škálovatelnost: co byste měli zvážit

Omezené svislé škálovatelnost můžete dosáhnout přesouvající se z základní nebo standardní SKU brány VPN k vysoký výkon VPN SKU.

VNets, očekáváte velké množství přenosů VPN zvažte možnost distribuce různých pracovního vytížení do samostatných menší VNets a konfiguraci brány VPN pro každý z nich.

VNet můžete rozdělit vodorovně nebo svisle. Oddíl vodorovně, přesuňte některých případech OM jednotlivé vrstvy do podsítí nové VNet. Výsledek je, že má každý VNet stejnou strukturu a funkce. Rozdělit svisle, změnit návrh jednotlivé vrstvy rozdělení funkci do jiných oblastí logické (například zpracování objednávky, fakturace, Správa účtu zákazníka a tak dál). Každý funkčních oblastí mohou být umístěny klikněte v samostatném VNet.

Replikace řadiče domény místní služby Active Directory v VNet a provádění DNS v VNet, pomůže snížit některé související se zabezpečením a správy provozu vyplývající z místního do cloudu.

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Pokud potřebujete zajistit, aby zůstal v místní síti VPN brány Azure k dispozici, implementujte překlopení obrázku pro místní brána VPN.

Pokud má vaše organizace víc místních webů, vytvoření [připojení k více serveru] [ vpn-gateway-multi-site] na jeden nebo více VNets Azure. Tento postup vyžaduje dynamické (směrování na základě) směrování, takže ověřte, že brána VPN místní podporuje tuto funkci.

V tématu [SLA VPN brány] [ sla-for-vpn-gateway] podrobnosti o SLA brány VPN.

## <a name="manageability-considerations"></a>Důležité informace o možnostech správy

Sledovat diagnostické informace z místního VPN zařízení. Tento postup závisí na funkce poskytované VPN zařízení. Například, pokud používáte směrování a služby vzdáleného přístupu na Windows Server 2012, byste měli povolit [protokolování RRAS][rras-logging].

Použít [Brána VPN Azure diagnostiky] [ gateway-diagnostic-logs] zachycení informací o problémy s připojením. Tyto protokoly slouží ke sledování informací například zdrojové a cílové připojení žádosti o protokol, který jste použili a jak bylo připojení (a proč se nezdařilo).

Sledujte provozní protokoly Brána VPN Azure pomocí k dispozici v Azure portálu protokolů auditování. Samostatné protokoly jsou k dispozici pro místní síti brány, brána Azure sítě a připojení. Tyto informace slouží ke sledování změn brány a může být užitečné, když dříve funkční brány z nějakého důvodu přestane fungovat.

![[2]][2]

Sledování připojení a sledovat události chyba při připojení. Můžete použít sledování balíček, například [Nagios] [ nagios] zachycení a vykazování tyto informace.

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Vygenerujte jiné sdílené klíč pro každou VPN bránu. Pomocí klíči silných sdílené odolat hrubou útokům.

> [AZURE.NOTE] Nemůže v současné době umožňuje předem sdílené klíče brány Azure VPN Azure klíč trezoru.

Zajistit, aby používala zařízení VPN místní metodu šifrování, která je [kompatibilní se službou Azure Brána VPN][vpn-appliance-ipsec]. Zásadami směrování, brána VPN Azure podporuje šifrování algoritmů AES256, AES128 a 3DES. Na základě směrování bran podporují AES256 a 3DES.

Pokud zařízení VPN místní v síti obvod, která má brána firewall mezi obvod síť a Internet, bude pravděpodobně nutné její konfiguraci a [Další] [ additional-firewall-rules] povolit připojení VPN k webu.

Pokud aplikace v VNet odešle data k Internetu, implementace [vynuceného tunneling] [ forced-tunneling] ke směrování všech vazbou internetových přenosů na místní síti. Tento přístup umožňuje auditování odchozí žádosti aplikací z místního infrastruktury.

> [AZURE.NOTE] Vynuceného tunneling může být ovlivněné připojení ke službám Azure (službu úložiště, například) a Windows správce licencí.

## <a name="troubleshooting-considerations"></a>Poradce při potížích s co byste měli zvážit

> [AZURE.NOTE] Obecné informace o [odstraňování běžných chyb souvisejících s VPN]Navštěvujte blog směrování a vzdálený přístup k blogu[troubleshooting-vpn-errors].

Je-li přenosy nejde přecházet připojení VPN, zkontrolujte následující věci:

### <a name="on-premises-vpn-appliance"></a>Místní VPN zařízení:

**Místní VPN zařízení funguje správně?**

Kontrola žádné soubory generovaných zařízení VPN chyby a selhání protokolu. Umístění tyto informace se liší podle zařízení. Například pokud používáte RRAS v systému Windows Server 2012, můžete následujícího příkazu Powershellu zobrazíte informace o událostech Chyba služby RRAS:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

Vlastnost _zpráva_ pro každou položku obsahuje popis chyby do schránky. Je několik běžných příkladů:

- Nelze se chcete připojit, případně kvůli nesprávné IP adresu zadanou brány VPN Azure v konfiguraci rozhraní sítě RRAS VPN:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- Nepovedlo sdílené klávesu zadané v konfiguraci rozhraní sítě RRAS VPN:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

Můžete taky získat informace o pokusech o připojení prostřednictvím služby RRAS pomocí následujícího příkazu Powershellu v protokolu událostí:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

V případě připojení bude obsahovat tento protokol chyb, které vypadají podobně jako tento:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**Je správně směrování přenosy v síti VPN zařízení přes VPN brány Azure?**

Použijte nástroj například [PsPing] [ psping] k ověření připojení a směrování přes VPN brány. Například na Testovat připojení z místního počítače na webový server umístěné v VNet, spusťte tento příkaz (nahradit `<<web-server-address>>` adresu webového serveru):

```
PsPing -t <<web-server-address>>:80
```

Pokud do místního počítače můžete směrovat přenosy v síti na webový server, měli byste zkontrolovat výstup podobně jako tento:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Pokud s místo určení nemůžete komunikovat místního počítače, zobrazí se zprávy takto:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**Povoluje blokuje místní brána přenosy přes virtuální privátní sítě? Byly otevřeny správné porty?**

Ověřte, že zařízení VPN místní používá metodu šifrování, která je [kompatibilní se službou Azure VPN brány][vpn-appliance].

Zásadami směrování, brána VPN Azure podporuje šifrování algoritmů AES256, AES128 a 3DES. Na základě směrování bran podporují AES256 a 3DES.

### <a name="azure-vnet-gateway"></a>Azure VNet brána:

Zkontrolujte [protokoly pro diagnostiku Brána VPN Azure] [ gateway-diagnostic-logs] potenciální problémy.

**Jsou brána VPN Azure a místních VPN zařízení nakonfigurována klávesu stejné sdílené ověřování?**

Můžete zobrazit sdílený klíč uloženy bránou VPN Azure pomocí následujícího příkazu Azure rozhraní příkazového řádku:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

Příkazem vhodné pro zařízení VPN místní zobrazíte sdílené klíč nakonfigurovaný pro tuto zařízení.

Ověřte, že podržíte Azure VPN brány není přidružený NSG podsítě _GatewaySubnet_ .

Podrobnosti podsítě pomocí následujícího příkazu Azure rozhraní příkazového řádku:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Zajistit, aby tam je datové pole s názvem _id skupiny zabezpečení sítě_. Následující příklad zobrazuje výsledky například _GatewaySubnet_ obsahující přiřazené NSG (_VPN brány skupiny_). Může dojít kvůli brány správně fungovat, pokud jsou všechna pravidla definované pro tento NSG:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**Není nakonfigurován povolení komunikace pocházející mimo VNet virtuálních počítačích v Azure VNet? Zkontrolujte všechna NSG pravidla přidružené podsítí obsahující tyto virtuálních počítačích.**

Všechna pravidla NSG můžete zobrazit pomocí následujícího příkazu Azure rozhraní příkazového řádku:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**Byla odpojena Brána VPN Azure?**

Pomocí následujícího příkazu Powershellu Azure zkontrolovat aktuální stav připojení Azure VPN. `<<connection-name>>` Parametru je název, který propojuje brány virtuální sítě a místní brána připojení Azure VPN.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

Následující zlomky zvýraznění výstup generovaný brány připojili (první příklad) a od ní odpojilo (druhý příklad):

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>Konfigurace hostitele OM využití šířky pásma sítě a výkonu aplikace:

Ověřte správně nakonfigurované bránu firewall v operační systém Host spuštěný v Azure VMs v podsítě umožňuje povolené adres rozsahy IP adres místní.

**Neexistuje objem provozu zavřít omezení šířky pásma k bráně VPN Azure?**

Nástrojů závisí na k dispozici pro zařízení VPN s místním zařízením. Například pokud používáte RRAS v systému Windows Server 2012, můžete použít výkon sledování množství dat dostali a přenášena připojení VPN; pomocí _RAS celkové_ objektu, vyberte čítače _Přijaté bajty/s_ a _Odeslané bajty/s_ :

![[3]][3]

By měl porovnají výsledky s šířkou pásma sítě VPN brány (100 Basic a standardní skladové jednotky až 200 MB/s pro vysoký výkon SKU) k dispozici:

![[4]][4]

**Některé z virtuálních počítačích v Azure VNet běží pomalu? Jsou, které budou přetížený, jsou dostatečně z nich zpracovávání načíst jsou všechny Vyrovnávání zatížení, správně nakonfigurované?**

Při této akci musí [zachycení a analýza diagnostické informace][azure-vm-diagnostics]. Můžete zkoumat výsledky pomocí portálu Azure, ale celou řadu nástrojů jiných výrobců jsou rovněž k dispozici, která umožňuje podrobné podstatu data o výkonu.

**Je aplikace, což efektivní využití prostředků cloudu?**

Kód aplikace nástroje spuštěných pro každou OM a zjistit, zda aplikace využívají nejlepší využití prostředků. Pomocí nástrojů, jako jsou [Aplikace přehledy] [ application-insights] můžete provést tyto úlohy.

## <a name="solution-deployment"></a>Nasazení řešení

Pokud máte existující místní infrastruktury už nakonfigurována VPN zařízení, můžete nasadit architektura odkaz pomocí následujících kroků:

1. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně":  
[![Nasazení Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Čekat na odkaz v portálu Azure otevřít a pak postupujte podle těchto kroků: 
    - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte **Vytvořit nový** a zadejte `ra-hybrid-vpn-rg` do textového pole.
    - Vyberte oblast z **umístění** rozevíracím seznamu.
    - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
    - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
    - Klikněte na tlačítko **koupit** .

3. Počkejte nasazení dokončete.

## <a name="next-steps"></a>Další kroky

- [Instalace další řadiče domény pro místní domény Active Directory pomocí VMs v Azure VNet][installing-ad].

- [Pokyny pro nasazení systému Windows Server služby Active Directory v Azure VMs][deploying-ad].

- [Vytvoření DNS server VNet][creating-dns].

- [Konfigurace a správy DNS VNet][configuring-dns].

- [Používání místního Stormshield sítě zabezpečení zařízení přes VPN][stormshield].

- [Provádění hybridní architektura sítě s Azure ExpressRoute][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Struktura hybridním sítě, která trvá místní a cloudové infrastruktury"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "Rozdělení VNet zlepšit škálovatelnost:"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Protokoly auditování do portálu Azure"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Výkonnosti pro sledování zatížení sítě VPN"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Příklad graf výkonu sítě VPN"