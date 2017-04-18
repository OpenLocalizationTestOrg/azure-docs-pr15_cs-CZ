<properties
   pageTitle="Příklad DMZ – vytvoření DMZ k ochraně sítí bránu Firewall, UDR a NSG | Microsoft Azure"
   description="Vytvořit DMZ pomocí brány Firewall, definované uživatelem směrování (UDR) a síťových skupin zabezpečení (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Příklad 3 – vytvoření DMZ k ochraně sítí bránu Firewall, UDR a NSG

[Vraťte se na stránku osvědčené postupy zabezpečení okraj][HOME]

Tento příklad vytvoří DMZ bránu firewall, čtyři servery systému windows, uživatel definované směrování, IP přesměrování a skupiny zabezpečení sítě. Bude taky projděte si každý z příslušné příkazy k poskytování hlubší vysvětlení každého kroku. Je také oddíl přenosy scénář poskytnout hloubkové podrobné pokyny, jak pokračuje přenosy přes vrstvy obrana v DMZ. Nakonec do odkazů na oddíl je dokončeno kódem a pokyny k vytváření toto prostředí vyzkoušíte a experimentovat s různým scénáře. 

![Obousměrné DMZ s NVA, NSG a UDR][1]

## <a name="environment-setup"></a>Nastavení prostředí
V tomto příkladu je předplatné, které obsahuje následující:

- Tři cloudových služeb: "SecSvc001", "FrontEnd001" a "BackEnd001"
- Virtuální sítě "CorpNetwork", pomocí tří podsítí: "SecNet", "FrontEnd" a "Back-end"
- Virtuální spotřebiče sítě, v tomto příkladu bránu firewall připojené k podsítě SecNet
- Windows Server, který představuje serveru webových aplikací ("IIS01")
- Dvě okna servery, které představují aplikace zpět ukončit servery ("AppVM01", "AppVM02")
- Windows server, který představuje server DNS ("DNS01")

V části odkazy pod je skript Powershellu, který bude vytvořit maximálně prostředí jsme je popsali výše. Vytváření VMs a virtuální sítích, i když se dělají skriptem příklad nejsou píše v tomto dokumentu.

Vytvoření prostředí:

  1.    Uložení souboru xml konfigurace sítě zahrnuté v části odkazy (aktualizován jména, umístění a IP adresy tak, aby odpovídaly daný scénář)
  2.    Aktualizovat uživatelské proměnné skriptu prostředí, které má být skript kontrolovat (předplatná, názvy služeb atd.)
  3.    Spouštět v prostředí PowerShell

**Poznámka**: oblast označený ve skript Powershellu se musí shodovat oblasti označený ve konfiguračního souboru xml sítě.

Po spuštění skriptu úspěšně následující kroky po skriptu úvahy:

1.  Nastavení pravidel bránu firewall, to je součástí následující oddíl: popis pravidla bránu Firewall.
2.  Pokud chcete v části odkazy jsou dva skripty k nastavení webu a aplikace server s jednoduché webové aplikace umožňuje testováním pomocí této konfiguraci DMZ.

Po spuštění skriptu úspěšně bránu firewall pravidla se bude muset udělat, jsou obsaženy v oddílu s názvem: pravidla brány Firewall.

## <a name="user-defined-routing-udr"></a>Uživatelem definované směrování (UDR)
Ve výchozím nastavení se následující postupy systém definují takhle:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

VNETLocal je vždy definovaný adresu prefix(es) VNet pro konkrétní síti (ie změní se z VNet na VNet podle toho, jak je definováno každý konkrétní VNet). Zbývající trasy systém jsou statické a výchozí jako nahoře.

Jako prioritu směruje zpracovávají metodou nejdelší předpona POZVYHLEDAT (LPM), tedy nejvíce směrování v tabulce platit pro danou cílovou adresu.

Proto by být přes VNet cíle kvůli směrování 10.0.0.0/16 směrovat přenosy (třeba k serveru DNS01 10.0.2.4) určené pro místní síti (10.0.0.0/16). Jinými slovy 10.0.2.4, směrování 10.0.0.0/16 je nejvíce směrování, i když 10.0.0.0/8 a 0.0.0.0/0 také použít, ale protože jsou menší konkrétní neovlivňují tento přenos. Umožnění datových přenosů do 10.0.2.4 proto by mít přesměrování místní VNet a jednoduše směrovat cíle.

Pokud přenosy byla určena následujícím 10.1.1.1 například směrování 10.0.0.0/16 by použít, ale 10.0.0.0/8 bude přesnosti a provoz by to bylo zrušeno ("černé dírkového"), protože přesměrování je Null. 

Pokud cíle použili předpony Null nebo předpony VNETLocal a pak byste použili nejméně konkrétní směrovat, 0.0.0.0/0 a být směrován k Internetu funguje jako přesměrování a tím, okraje Azure na Internetu.

Pokud existují dva identickými předpony v tabulce směrování, je uvedeném pořadí podle atributu "zdrojový" postupy:

1.  "VirtualAppliance" = směrování definované uživatele ručně přidané do tabulky
2.  "VPNGateway" = dynamické směrování BGP (při použití hybridního sítích), přidal dynamické síťový protokol, tyto postupy v průběhu času mění podle dynamické protokol odráží automaticky změnami peered sítě
3.  "Výchozí" = směruje systém, místní VNet a statické položky podle výše uvedeného postupu tabulky.

>[AZURE.NOTE] Teď můžete uživatel definované směrování (UDR) s ExpressRoute a bran VPN vynutit příchozí a odchozí přenosy křížově místní na směrovány virtuální zařízení síti (NVA).

#### <a name="creating-the-local-routes"></a>Vytvoření místní trasy

V tomto příkladu jsou potřebné směrování tabulkami, jeden pro Frontend a back-end podsítí. Každou tabulku se načte s statické trasy vhodné pro dané podsítě. Pro účely v tomto příkladu každá tabulka obsahuje tří postupů:

1. Vysílání otevřeny další směrování definované přenosy otevřeny obejít bránu firewall
2. Virtuální v síti s další směrování rozumí bránu firewall, tato možnost přepíše výchozí pravidlo, které umožňuje místní VNet provoz směrovat přímo
3. Všechny zbývající přenosy (0/0) se další směrování rozumí bránu firewall.

Po vytvoření tabulky směrování je vázaný na jejich podsítí. Podsítě Frontend směrovací tabulky, jednou vytvořené a vázaný na podsítě by měl vypadat takto:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


V tomto příkladu jsou použít následující příkazy k vytvoření tabulce postupu, přidání trasy definované uživatelem a tabulce směrování svázat podsítě (Poznámka; nějaké položky pod začínající na s znak dolaru (například: $BESubnet) jsou definované uživatelem proměnné z skriptu v části referenční tohoto dokumentu):

1.  Nejprve je třeba vytvořit základní směrování tabulky. Tento úryvek ukazuje vytvoření tabulky podsítě back-end. V skriptu se vytvoří tabulku odpovídající taky podsítě Frontend.

        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"

2.  Po vytvoření tabulky směrování lze přidat určitým uživatelem definované trasy. V tomto vystřižených budou všechny přenosy (0.0.0.0/0) směrovaná přes virtuální zařízení (proměnnou $VMIP [0] slouží k předání IP adresy přiřazené při vytváření virtuálních zařízení ve skriptu dříve). V skriptu pravidlo odpovídající taky vytvořeno v tabulce Frontend.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

3. Položce výše uvedeného postupu přepíše výchozí "0.0.0.0/0" trasy, ale výchozí pravidlo 10.0.0.0/16 dosud která by přenosy v rámci VNet směrovat přímo do cíle a pozor, abyste virtuální zařízení sítě. Pro správné toto chování pravidla sledovat musí být přidán.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

4. V tomto okamžiku je s dotazem, jestli chcete označit jako. Pomocí výše uvedených dvou tras se všechny přenosy směrovat brány firewall pro hodnocení, i přenosy v rámci jedné podsítě. To může být potřeba, však umožňující přenosy v rámci podsítě ke směrování místně bez zásahu brány firewall třetí lze přidat specifických pravidlo. Tento postup stavy, které všechny adresy destine pro můžete právě otevřeny směrování tam přímo (NextHopType = VNETLocal).

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal

5.  Nakonec s tabulkou směrování vytvořené a vyplněné směruje definované uživatelem, tabulce musí teď vázat podsítě. V skriptu tabulce směrování front-end svázán taky podsítě Frontend. Tady je skript vazby podsítě back-end.

        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
           -SubnetName $BESubnet `
           -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>Předávání IP
Funkce Průvodce vyhledáváním pro UDR, je IP přesměrování. Toto je údaj v virtuální zařízení, která umožňuje přijímat přenosy adresovanou není výslovně zařízení a nastavíte přesměrování která vysílání cíle ultimate.

Jako příklad Pokud adres AppVM01 udělá žádost o službu DNS01 UDR by směrovat to bránu firewall. S přesměrování povolené IP přenosy pro určení DNS01 (10.0.2.4) přijal zařízení (10.0.0.4) a potom přepošlou cíle ultimate (10.0.2.4). Bez IP přesměrování zapnuta bránu Firewall budou návštěvníci by se dají přijímat tak, že zařízení i když je tabulka směrování má brána firewall jako přesměrování. 

>[AZURE.IMPORTANT] Je důležité si zapamatovat povolit přesměrování IP ve spojení s uživateli definované směrování.

Nastavení přesměrování IP je jediný příkaz a se teď dá času vytvoření OM. Fragment kódu pro tok v tomto příkladu je ke konci skript a seskupené s příkazy UDR:

1.  Volání instanci OM, která je v tomto případě zařízení virtuální bránu firewall a povolit přesměrování IP (Poznámka:; libovolnou položku červená začínající na s znak dolaru (například: $VMName[0]) je proměnná definované uživatelem z skript v části odkazy v tomto dokumentu. Nula do závorek, [0] představuje první OM v matici VMs skript příklad pracovat beze změn, první OM (OM 0) musí být bránu firewall):

        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
           Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Skupiny zabezpečení síti (NSG)
V tomto příkladu je skupinu NSG vytvořené a potom načte s jedno pravidlo. Tato skupina je potom svázáno pouze Frontend a back-end podsítí (ne SecNet). Deklarativně je vytvářeného následující pravidlo:

1.  Odepřen všechny přenosy (všechny porty) z Internetu na celý VNet (všechny podsítě)

I když jsou v tomto příkladě použita NSGs, hlavním účelem je jako sekundární vrstvy obrana před ručně stav. Chcete blokovat všechny příchozí data z Internetu buď Frontend nebo back-end podsítí, budou návštěvníci mají jenom postupují podsítě SecNet brány firewall (a pak v případě potřeby k Frontend nebo back-end podsítí). Navíc s pravidly UDR na místě, všech přenosů vytvořte z nich podsítí Frontend nebo back-end by být směrovány se brány (díky UDR). Brána firewall, uvidí to jako asymetrické tok a by přetáhnout odchozí přenosy. Proto jsou tři vrstvy zabezpečení, ochrana Frontend a back-end podsítí; 1) bez otevření koncové body FrontEnd001 a BackEnd001 cloudových služeb, (2) NSGs odepření přenosy z Internetu, 3) brány firewall odstranění asymetrické přenos.

Jeden bod zajímavé týkající se skupina zabezpečení sítě v tomto příkladu je se v ní jenom jedno pravidlo ukázáno v následujícím příkladu, která má odepřít internetových přenosů na celý virtuální sítě, která by obsahoval podsítě zabezpečení. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Však, protože NSG svázán pouze Frontend a back-end podsítí, pravidla není zpracovány zatížení příchozí podsítě zabezpečení. Výsledkem je i když pravidlo NSG s textem bez internetový provoz s adresami na VNet, protože byla NSG nikdy vázaný na podsítě zabezpečení, přenosy bude flow podsítě zabezpečení.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName
    
    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Pravidla brány firewall
V bráně firewall pravidel přesměrování muset vytvořit. Protože brány je blokování nebo přesměrování všechny příchozí, odchozí a uvnitř VNet přenosy jsou potřebné mnoho pravidla brány firewall. Všechny příchozí data také, bude narazit zabezpečení služeb veřejnou IP adresu (na různé porty), zpracování bránou firewall. Doporučený postup je diagram logickou toky před nastavením podsítí a brány firewall pravidla pro omezení přepracovat později. Na následujícím obrázku je logické zobrazení pravidla brány firewall v tomto příkladu:
 
![Zobrazení Logická pravidel brány Firewall][2]

>[AZURE.NOTE] Podle sítě virtuální zařízení používá porty pro správu se budou lišit. V tomto příkladu, které se odkazuje bránu NextGen Firewall Barracuda využívající porty 22 801 a 807. V dokumentaci zařízení dodavatele najít přesnou porty pro správu ze zařízení.

### <a name="logical-rule-description"></a>Popis logické pravidla
Ve výše uvedené Logický diagram nebude zobrazen podsítě zabezpečení vzhledem k tomu brány je pouze zdroje na tomto podsítě a tento graf zobrazuje pravidla brány firewall a jak logicky povolit nebo odepřít tok a ne skutečné směrované cesty. Také externí porty vybraném pro přenos RDP jsou vyšší pohyboval porty (8014 – 8026) a jste vybírali poněkud zarovnáte poslední dvě oktetů místní IP adresa pro snazší čitelnost (například adresa místního serveru 10.0.1.4 souvisí s externím portu 8014), ale lze použít jakýkoli vyšší než konfliktní.

V tomto příkladu potřebujeme 7 typy pravidel, těchto pravidel jsou popsány následujícím způsobem:

- Externí pravidla (u příchozích):
  1.    Pravidlo správu brány firewall: Toto pravidlo přesměrování aplikace umožňuje přenos předat porty správy zařízení virtuální sítě.
  2.    Pravidla RDP (pro každý systému windows server): tyto čtyři pravidla (jedno pro každé server) vám umožní správu jednotlivé serverů prostřednictvím RDP. Může to také seskupeny do jedno pravidlo podle toho, funkce virtuální zařízení sítě použitý.
  3.    Pravidla přenosy aplikace: Existují dva pravidla přenosy aplikace první pro přenos webových front-end a druhý pro přenos back-end (například webový server osy dat). Konfigurace tato pravidla, závisí na architektura sítě (serverech umístění) a provoz toků (směr se používají tok a které porty).
      - První pravidlo vám umožní provozu skutečné aplikací kontaktovat aplikační server. Zatímco ostatní pravidla povolit zabezpečení, správa, atd., jsou pravidel aplikací co povolit externí uživatele nebo služby pro přístup k aplikací. V tomto příkladu je jediný webového serveru na portu 80, tedy bude pravidlo aplikace jedné brány firewall přesměrování příchozích na externí IP na web servery interní IP adresu. Relace přesměrované přenosy by být překladu síťových adres dosáhl na interní server.
      - Druhé pravidlo přenosu aplikace je back-end pravidlo umožňuje webového serveru chcete mluvit AppVM01 serveru (ale ne AppVM02) prostřednictvím jakýkoli.
- Vnitřní pravidla (pro přenos uvnitř VNet)
  4.    Odchozího pravidla Internetu: Toto pravidlo vám umožní adres jakákoli síť předat vybrané sítě. Pokud chcete toto pravidlo je obvykle výchozí pravidlo už v bráně firewall, ale v zakázaném stavu. Pokud chcete toto pravidlo lze povolit v tomto příkladu.
  5.    DNS pravidlo: Toto pravidlo umožňuje pouze DNS (port 53) předání přenosu na DNS server. Toto prostředí, které jsou blokovány většina přenosy z Frontend back-end toto pravidlo konkrétně umožňuje DNS z místní podsítě.
  6.    Podsítě k podsítě pravidlo: Pokud chcete toto pravidlo je tak, aby všechny serveru podsítě back-end připojit k jakémukoli serveru na front-end podsítě (ale ne obrácené pořadí).
- Fail-Safe pravidla (pro přenos, který je nesplňuje výše uvedeného):
  7.    Odepřít všechny přenosy pravidlo: Toto by měla být vždy konečný pravidlo (z hlediska priorita) a jako takové Pokud komunikace s přestane odpovídat žádné z předchozího pravidla, která je dojde ke ztrátě toto pravidlo. Toto je výchozí pravidlo a obvykle aktivován, žádné změny jsou obvykle potřebné.

>[AZURE.TIP] Na druhé pravidlo přenosu aplikace jakýkoli je povolené pro snadné například v reálné situace nejvíce port a zmenšit napadení toto pravidlo bude použito rozsahy adres.

<br />

>[AZURE.IMPORTANT] Jakmile všechna výše pravidla vytvořené, je důležité kontroloval priority každé pravidlo zajistit přenos bude povolený nebo odepřen podle potřeby. V tomto příkladu pravidla jsou v pořadí priorit. Není těžké si uzamčení bránu firewall kvůli chybně uspořádaných pravidla. Minimálně zajistěte, aby že správa brány firewall pro sebe sama je vždy absolutní pravidlo nejvyšší prioritu.

### <a name="rule-prerequisites"></a>Požadavky na pravidla
Jeden předpokladem počítač virtuální bránu firewall jsou veřejné koncové body. Brány firewall pro přenos zpracovávají musí být vhodné veřejné koncové body otevřít. Existují tři typy přenosů v tomto příkladu; 1) správy umožnění datových přenosů do ovládacího prvku bránu firewall a pravidla brány firewall, 2) RDP umožnění datových přenosů do ovládat servery systému windows a provozu aplikací 3). Toto jsou tři sloupce typy přenosů v horním polovině logický zobrazení pravidel brány firewall nad.

>[AZURE.IMPORTANT] Klíčové takeway tady je mějte na paměti, že **všechny** přenosy chodily přes bránu firewall. Tak Vzdálená plocha na server IIS01, i když je v popředí cloudové službě konce a na podsítě front-end přístup k tomuto serveru jsme bude RDP nutné bránu firewall na port 8014 a povolit brány firewall pro směrovat požadavek RDP interně IIS01 RDP Port. Tlačítkem "Připojit" Azure portálu nebudou fungovat, protože neexistuje přímé RDP cesta k IIS01 (jde portálu vidí). To znamená, že všechna připojení z Internetu bude službu zabezpečení a portu, například secscv001.cloudapp.net:xxxx (reference diagramu pro mapování Port externích a interních IP a Port).

Koncový bod jde otevřít buď při vytváření OM nebo zveřejňují Tvůrce dotazů a je udělat i v ukázkovém skriptu vidíte na obrázku v tomto fragment kódu (Poznámka; začínající znak dolaru všechny položky (například: $VMName[$i]) je proměnná definované uživatelem z skript v části odkazy v tomto dokumentu. "$I" do závorek, [$i] představuje maticové počet konkrétní OM v matici VMs):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Sice předvádět není jasně kvůli využívání proměnné, ale koncové body uvádíme **pouze** otevřené na zabezpečení cloudové služby. Toto je zajistit, že je zpracování všech příchozích (směrování překladu síťových adres měli, nezobrazí) bránou firewall.

Správa klienta muset nainstalovat na počítač spravovat bránu firewall a vytvořit konfigurace potřeby. Zobrazit si přečtěte následující dokumentaci z brány firewall (nebo jiných NVA) dodavatelů týkající se správy zařízení. Zbytek tento oddíl a dál, vytváření pravidel bránu Firewall, bude popis konfigurace brány sebe sama přes klienta správy dodavatelé (tedy ne Azure portál nebo Powershellu).

Pokyny pro stažení klienta a připojení k Barracuda v tomto příkladě použita najdete tady: [Barracuda NG správce](https://techlib.barracuda.com/NG61/NGAdmin)

Po přihlášení k lyncu na brána firewall, ale před vytvořením pravidel brány firewall, existují dva třídy základní objektů, které můžou způsobit vytváření pravidla jednodušší; Objekty, sítě a služby.

V tomto příkladu by měl být tři objekty síťové definovaný (jednu podsítě Frontend a back-end podsítě taky síťového objektu na IP adresu serveru DNS). Vytvoření pojmenované síti. začnete na řídicím panelu Barracuda NG správce klienta, přejděte na kartu Konfigurace, v části provozní konfigurace klikněte sada pravidel pro, pak klikněte na "Sítí" v nabídce objekty bránu Firewall a pak klikněte na tlačítko Nový v nabídce Úpravy sítě. Objekt sítě teď vytvořením, tím, že přidáte název a předponu:

![Vytvoření objektu FrontEnd sítě][3]
 
Tím vytvoříte pojmenovanou síť pro podsítě FrontEnd, podobně jako objekt je nutné vytvořit do back-end podsítě. Teď podsítí můžete snadněji odkazovat podle názvu v pravidlech brány firewall.

Pro objekt Server DNS:

![Vytvoření objektu serveru DNS][4]

Jeden IP adresu odkazu bude použita v pravidlu DNS dál v dokumentu.

Druhá základní objekty jsou objekty služby. Prezentuje tyto porty připojení RDP pro každý server. Protože existující objekt služby RDP je vázaný na výchozí port RDP, 3389, nové služby můžete vytvořit za účelem povolení adres externí porty (8014 8026). Nové porty také lze přidat do existující službu RDP, ale pro snadnější ukázka, lze vytvořit jednotlivá pravidla pro každý server. Vytvoření nového pravidla RDP pro na server. začnete na řídicím panelu Barracuda NG správce klienta, přejděte na kartu Konfigurace, v části provozní konfigurace klikněte sada pravidel pro, a pak klikněte na "Služby" v nabídce objekty bránu Firewall, přejděte v seznamu dolů služeb a vyberte službu "RDP". Klikněte pravým tlačítkem myši a vyberte Kopírovat, a pak klikněte pravým tlačítkem myši a vyberte Vložit. Je teď RDP Copy1 objekt služby, kterou lze upravovat. Klikněte pravým tlačítkem myši RDP Copy1 a potom vyberte upravit, upravit objekt služby objeví okno se až znázorněná na obrázku:

![Kopie výchozí RDP pravidlo][5]

Lze upravit hodnoty představující službu RDP pro konkrétní server. Pro AppVM01 výše výchozí RDP pravidlo mění tak, aby odrážely nový služby název, popis a externí Port RDP používaný v diagramu obrázek 8 (Poznámka: porty se změnil z výchozí RDP 3389 externí port pro tento konkrétní server používány, v případě AppVM01 externím portu 8025) službu změněné se zobrazují pod :

![AppVM01 pravidla][6]
 
Tento proces opakuje vytvořit RDP služby pro zbývající servery; AppVM02 DNS01 a IIS01. Vytvoření těchto služeb pokusy vytváření pravidla jednodušší a zpřístupnit v další části.

>[AZURE.NOTE] Služby RDP brány firewall pro není potřeba ze dvou důvodů; 1) první brána firewall OM je obrázek Linux na základě SSH použila port 22 správy OM místo RDP a 2) port 22, takže v první pravidlo správy píše níže, aby umožňoval připojení správy jsou povoleny dva další porty pro správu.

### <a name="firewall-rules-creation"></a>Vytváření pravidel brány firewall
Existují tři typy v tomto příkladě použita pravidla brány firewall, mají všechna různých ikony:

Pravidlo aplikace přesměrovat: ![Ikonu přesměrování aplikace][7]

Pravidlo překladu síťových adres cíl: ![Určení překladu síťových adres ikonu][8]

Pravidlo průchodu: ![Předat ikonu][9]

Další informace o těchto pravidel najdete na webových stránkách Barracuda.

Vytvořte následující pravidla (nebo ověření existující výchozí pravidla), počínaje řídicím panelu Správce NG Barracuda klientů, přejděte na kartu Konfigurace získáte v provozní konfiguraci oddíl sada pravidel pro. Síť s názvem "Hlavní pravidla" se budou zobrazovat existující aktivní a deaktivovaný pravidla na tato brána firewall. V pravém horním rohu tuto mřížku malá, zelené "a" tlačítka, klikněte v můžete vytvořit nové pravidlo (Poznámka: Brána firewall může být "uzamčené" změny, pokud se zobrazí tlačítko označené jako "Uzamknout" a nejde vytvořit nebo upravit existující pravidla, kliknutím na toto tlačítko "odemknout" sadu pravidel a povolit úpravy). Pokud chcete upravit existující pravidlo, zaškrtněte toto pravidlo, klikněte pravým tlačítkem myši a vyberte Upravit pravidlo.

Jakmile vytvořili nebo změnili pravidel, musí být posune brány firewall a pak aktivovat, pokud není to pravidlo změny se projeví. Proces nabízených a aktivace je popsáno níže popisy pravidlo podrobnosti.

Podrobnosti každé pravidlo potřebná k dokončení v tomto příkladu jsou popsané následujícím způsobem:

- **Pravidlo brány firewall Správa**: Tato aplikace přesměrovat pravidlo umožňuje přenos předat řízení porty virtuální spotřebiče sítě, v tomto příkladu bránu NextGen Firewall Barracuda. Správa porty jsou 801 807 a volitelně 22. Porty externích a interních jsou stejné (tedy bez překlad port). Toto pravidlo, přístup k nastavením Správa je výchozí pravidlo a povolena ve výchozím nastavení (Barracuda NextGen verze 6.1).

    ![Správa pravidlo brány firewall][10]

>[AZURE.TIP] Adresní prostor zdroje v tohoto pravidla je Any, pokud je známo, rozsahy IP adres správy, snížení tento obor by také změnit napadení porty správy.

- **Pravidla RDP**: tyto překladu síťových adres cíl pravidla vám umožní správu jednotlivé serverů prostřednictvím RDP.
Existují čtyři pole Kritický potřeby toto pravidlo vytvoříte takhle:
  1.    Zdroj – umožňuje RDP odkudkoli, odkaz "Některé" slouží v poli zdroj.
  2.    Služba – použít příslušný objekt služby dříve vytvořili – v tomto případě "AppVM01 RDP", externí porty přesměrovat servery místní IP adresa a na port 3386 (výchozí port RDP). Toto pravidlo konkrétní je RDP připojovat AppVM01.
  3.    Cíl – třeba *místní port v bráně firewall*, "DHCP 1 místní IP" nebo eth0 Pokud pomocí statické IP adresy. Řadová číslovka (eth0, eth1 atd.) může být odlišné Pokud zařízení sítě obsahuje více místní rozhraní. Toto je portů brány firewall odesílá z (může být stejné jako přijímání port), je skutečná směrované cílová v cílovém seznamu polí.
  4.    Přesměrování – Tato část taky říká, virtuální zařízení kde nakonec přesměrovat tento přenos. Nejjednodušší přesměrování je přesuňte IP a Port (volitelné) do pole cílový seznam. Pokud žádný port slouží s cílovým portem příchozí žádosti bude používá (ie žádné překlad), pokud port označen port by vás překladu síťových adres by spolu s IP adres.

    ![Pravidlo RDP brány firewall][11]

    Celkem čtyři RDP pravidla bude potřeba vytvořit: 

  	|   Název pravidla    | Server  |   Služba   |  Cílový seznam  |
  	|----------------|---------|-------------|---------------|
  	| RDP IIS01   | IIS01   | IIS01 RDP   | 10.0.1.4:3389 |
  	| RDP DNS01   | DNS01   | DNS01 RDP   | 10.0.2.4:3389 |
  	| RDP AppVM01 | AppVM01 | AppVM01 RDP | 10.0.2.5:3389 |
  	| RDP AppVM02 | AppVM02 | AppVm02 RDP | 10.0.2.6:3389 |
  
>[AZURE.TIP] Zúžení dolů obor pole zdroj a přihlašovacích údajů sníží napadení. Většina omezený dosah, které vám umožní funkce bude použito.

- **Pravidel přenosu aplikací**: existují dva pravidel přenosu aplikací první pro přenos webových front-end a druhý pro přenos back-end (například webový server osy dat). Tato pravidla, závisí na architektura sítě (serverech umístění) a provoz toků (směr se používají tok a které porty).

    Nejdřív popisované je front-end pravidlo pro přenos web:

    ![Pravidla Web brány firewall][12]
 
    Pokud chcete toto pravidlo cílové překladu síťových adres umožňuje přenos skutečné aplikace kontaktovat aplikační server. Zatímco ostatní pravidla povolit zabezpečení, správa, atd., jsou pravidel aplikací co povolit externí uživatele nebo služby pro přístup k aplikací. V tomto příkladu je jediný webového serveru na portu 80, tedy bude pravidlo aplikace jedné brány firewall přesměrování příchozích na externí IP na web servery interní IP adresu.

    **Poznámka**: aby není žádný port přiřazené v cílovém seznamu polí, tedy příchozí port 80 (nebo 443 pro službu vybraná) použijí se při přesměrování na webový server. Pokud na jiný port je poslech webového serveru, například port 8080, pole cílového seznamu může aktualizuje tak, aby 10.0.1.4:8080 povolit přesměrování Port i.

    Další aplikace přenosy jedná back-end pravidlo umožňuje webového serveru chcete mluvit AppVM01 serveru (ale ne AppVM02) prostřednictvím jiné služby:

    ![Pravidlo AppVM01 brány firewall][13]

    Toto pravidlo průchodu umožňuje všechny serveru IIS na podsítě Frontend kontaktovat AppVM01 (IP adresa 10.0.2.5) na libovolném portu pomocí libovolného protokolu pro přístup k datům potřeby tak, že webové aplikace.

    V tomto snímku obrazovky "\<explicitní cíl\>" slouží k znamenat 10.0.2.5 jako cíl v cílové pole. Může to být buď explicitní viz nebo názvem síť objektu (stejně jako v požadavcích na DNS server). Toto je až správce bránu firewall, která se použije metody. Přidat 10.0.2.5 Explict Desitnation, poklepejte na první prázdný řádek pod \<explicitní cíl\> a zadejte adresu v okně, které se zobrazí.

    Toto pravidlo předat je nutné žádné překladu síťových adres protože to je interní přenos tak nastavit způsob připojení k "Bez SNAT".

    **Poznámka**: síť zdroj v tohoto pravidla je všechny zdroje na podsítě FrontEnd Pokud tam bude pouze jednu nebo známé určitý počet webových serverů, prostředek objekt sítě nelze vytvořit více specifické pro tyto přesné IP adresy namísto celého podsítě Frontend.

>[AZURE.TIP] Toto pravidlo pomocí služby "Už" usnadnit nastavení a použití ukázková aplikace, také to umožní ICMPv4 (použití příkazu ping) v jedno pravidlo. To je však není doporučený postup. Porty a protokoly ("služby") by měl být zúžit nejmenší možnou umožňující aplikace operace zmenšit napadení přes tento hranice.

<br />

>[AZURE.TIP] I když toto pravidlo zobrazuje odkaz na explicitní cíl použit, konzistentní přístup by měl použít v celém konfigurace brány firewall. Je vhodné použít pojmenované objekt sítě v celém pro snazší čitelnost a podpory. Explicitní cíl je zde používá pouze k zobrazení metodu alternativní odkaz a se nedoporučuje obecně (zejména složité konfigurace).

- **Výstupní Internet pravidlo**: Tento předat pravidla vám umožní adres jakákoli síť zdroj k předání vybrané sítí cíl. Pokud chcete toto pravidlo výchozí pravidlo obvykle už v bráně firewall Barracuda NextGen, ale v zakázaném stavu. Pravým tlačítkem myši na toto pravidlo můžete přístup k příkazu aktivujte pravidlo. Pravidlo ukazuje tato část upraven přidáte dvě místní podsítí, které byly vytvořené jako odkazy v části základní tohoto dokumentu zdrojovou vlastnost toto pravidlo.

    ![Brána firewall odchozího pravidla][14]

- **Pravidlo DNS**: Tento předat pravidlo umožňuje pouze DNS (port 53) předání přenosu na DNS server. Toto pravidlo umožňuje pro toto prostředí, které jsou blokovány většina přenosy z FrontEnd back-end konkrétně DNS.

    ![Pravidlo DNS brány firewall][15]

    **Poznámka**: V této obrazovce snímek metodu připojení je však započítávány. Vzhledem k tomu toto pravidlo pro interní IP interní přenos IP adresu, bez NATing požadované, je to způsob připojení hodnotu "Bez SNAT" pro toto pravidlo průchodu

- **Pravidlo podsítě k podsítě**: Tento předat pravidlo je výchozí pravidlo, které má aktivovat a změnit tak, aby všechny serveru podsítě back-end připojit k jakémukoli serveru na front-end podsítě. Toto pravidlo je všechny interní přenos tak způsob připojení můžete nastavit SNAT č.

    ![Pravidlo uvnitř VNet brány firewall][16]

    **Poznámka**: zaškrtávací políčko obousměrné není zaškrtnuté políčko (ani není zaškrtnuté políčko ve většině pravidla), to je důležité pro toto pravidlo, má toto pravidlo "jeden směrové", připojení se dá spouštět z podsítě back-end front-end síť, ale nikoli naopak. Pokud byl této zaškrtávací políčko zaškrtnuté, umožní toto pravidlo obousměrné přenosů z našich Logický diagram není žádoucí.

- **Zakázat všechny přenosy pravidlo**: vždycky je vhodné konečný pravidlo (z hlediska priorita) a jako takové Pokud přenosy přecházel dojde k chybě podle některou z předchozí pravidla ho dojde ke ztrátě toto pravidlo. Toto je výchozí pravidlo a obvykle aktivován, žádné změny jsou obvykle potřebné. 

    ![Brána firewall odepřít pravidla][17]

>[AZURE.IMPORTANT] Jakmile všechna výše pravidla vytvořené, je důležité kontroloval priority každé pravidlo zajistit přenos bude povolený nebo odepřen podle potřeby. V tomto příkladu pravidla jsou v pořadí, ve kterém by se měly v mřížce hlavní předávání pravidla v klientovi Barracuda správy.

## <a name="rule-activation"></a>Aktivace pravidla
S sada pravidel pro upravit tak, aby specifikace diagram logiky musí být sada pravidel pro nahrané brány firewall a pak aktivovali.

![Aktivace pravidlo brány firewall][18]
 
V pravém horním rohu klient management jsou clusteru tlačítek. Klikněte na tlačítko "Odeslat změn" změněné pravidla odešlete bránu firewall a potom klikněte na tlačítko "Aktivovat".
 
S aktivací sada pravidel brány firewall pro tento příklad prostředí sestavení dokončení.

## <a name="traffic-scenarios"></a>Přenosy scénáře
>[AZURE.IMPORTANT] Klíčové takeway je mějte na paměti, že **všechny** přenosy chodily přes bránu firewall. Tak Vzdálená plocha na server IIS01, i když je v popředí cloudové službě konce a na podsítě front-end přístup k tomuto serveru jsme bude RDP nutné bránu firewall na port 8014 a povolit brány firewall pro směrovat požadavek RDP interně IIS01 RDP Port. Tlačítkem "Připojit" Azure portálu nebudou fungovat, protože neexistuje přímé RDP cesta k IIS01 (jde portálu vidí). To znamená, že všechna připojení z Internetu bude službu zabezpečení a portu, například secscv001.cloudapp.net:xxxx.

Pro podobnému sledu by měl být následující pravidla brány firewall na místě:

1.  Správa brány firewall
2.  RDP IIS01
3.  RDP DNS01
4.  RDP AppVM01
5.  RDP AppVM02
6.  Přenosy aplikace na webu
7.  Umožnění datových přenosů aplikace do AppVM01
8.  Odchozí k Internetu
9.  Frontend DNS01
10. Přenosy uvnitř podsítě (back-end na front-end pouze)
11. Zakázat všechny

Sada pravidel skutečné brány firewall pro bude pravděpodobně mnoho dalších pravidel kromě těchto, pravidla pro dané firewall bude mít taky zajištěný čísel různých priority než těch, které jsou tady uvedené. Tento seznam a související čísla musí poskytovat relevance mezi jenom tyto 11 pravidla a relativní prioritu mezi nimi. Jinými slovy; v bráně firewall skutečné "RDP k IIS01" můžou být pravidla číslo 5, ale tak dlouho, dokud je pod "Správa brány Firewall" pravidlo a nad pravidlo "RDP k DNS01" by zarovnat za účelem tento seznam. V seznamu vám taky pomůže v pod scénáře povolení stručnost; například "Přesměrování pravidlo 9 (DNS)". Také neopakujeme, čtyři pravidla RDP bude být nazývají, "RDP pravidla" při scénář přenosy nesouvisí s RDP.

Také odvolat, že skupin zabezpečení sítě jsou místní u příchozí internetový provoz na Frontend a back-end podsítí.

#### <a name="allowed-internet-to-web-server"></a>(Povolené) Internet webový server
1.  Stránka s uživatelem žádosti o HTTP Internet z SecSvc001.CloudApp.Net (Internet protilehlé cloudové služby)
2.  Cloudové služby průchodů přenosů otevřít koncového bodu na portu 80 rozhraní brány firewall na 10.0.0.4:80
3.  Žádné NSG přiřazené k zabezpečení podsítě tak systémových NSG pravidel přenosy brány firewall
4.  Přenosy narazí interní IP adresu brány firewall (10.0.1.4)
5.  Brána firewall, nebude zahájen zpracování pravidla:
  1.    Přesměrování pravidla 1 (Správa Přesměrování) není použití, přesuňte do další pravidla
  2.    Přesměrování pravidla 2 až 5 (RDP pravidla) nechcete použít, přesuňte do další pravidla
  3.    Přesměrování pravidla 6 (aplikace: Web) použít, jsou povoleny přenosy, dále brány firewall a 10.0.1.4 (IIS01)
6.  Podsítě Frontend začíná zpracování příchozí pravidla:
  1.    Problém se netýká NSG pravidla 1 (blok Internet) (Tento přenos byl překladu síťových adres by bránou firewall, tedy adresy zdrojového je teď bránu firewall, která je na podsítě zabezpečení a prezentací, které podsítě Frontend NSG jako "místní" přenosy a tedy povolené), přesunutí k další pravidlo
  2.    Výchozí NSG pravidla přenosy podsítě k podsítě, jsou povoleny přenosy, zastavit zpracování pravidel NSG
7.  IIS01 je sleduje web přenos, obdrží tohoto požadavku a spustí zpracování požadavku
8.  IIS01 pokusy o zahájí relace FTP k AppVM01 podsítě back-end
9.  Směrování UDR Frontend podsítě přesměrování zajišťuje bránu firewall.
10. Žádná odchozího pravidla na Frontend podsítě jsou povoleny přenosy
11. Brána firewall, nebude zahájen zpracování pravidla:
  1.    Přesměrování pravidla 1 (Správa Přesměrování) není použití, přesuňte do další pravidla
  2.    Přesměrování pravidlo 2 až 5 (RDP pravidla) není použití, přesuňte do další pravidla
  3.    Přesměrování pravidla 6 (aplikace: Web) není použití, přesuňte do další pravidla
  4.    Přesměrování pravidlo 7 (aplikace: back-end) použít, jsou povoleny přenosy, brány firewall přepošle umožnění datových přenosů do 10.0.2.5 (AppVM01)
12. Podsítě back-end začíná zpracování příchozí pravidla:
  1.    NSG pravidla 1 (blok Internet) není použití, přesuňte do další pravidla
  2.    Výchozí NSG pravidla přenosy podsítě k podsítě, jsou povoleny přenosy, zastavit zpracování pravidel NSG
13.  AppVM01 obdrží žádost a zahájí relace a odpovědi
14. Směrování UDR podsítě back-end přesměrování zajišťuje bránu firewall.
15. Protože neexistují žádná odchozího pravidla NSG podsítě back-end odpověď smí
16. Protože to vrací přenosy na relaci založení předává odpověď zpátky na webový server (IIS01)
17. Frontend podsítě začíná zpracování příchozí pravidla:
  1.    NSG pravidla 1 (blok Internet) není použití, přesuňte do další pravidla
  2.    Výchozí NSG pravidla přenosy podsítě k podsítě, jsou povoleny přenosy, zastavit zpracování pravidel NSG
18. Serveru IIS obdrží odpověď, dokončí transakci AppVM01 a dokončení sestavování odpověď HTTP, je tato odpověď HTTP odeslána žadateli předán
19. Protože neexistují žádná odchozího pravidla NSG podsítě Frontend odpověď smí
20. Odpověď na HTTP narazí bránu firewall a protože jde odpověď relaci založení překladu síťových adres přijetím odvolat bránou firewall
21. Brána firewall pak přesměruje odpovědi Internet uživatele
22. Protože neexistují žádné odchozího pravidla NSG nebo UDR směrování na podsítě Frontend, které jsou povolené odpověď a Internet uživatele obdrží požadavku na webovou stránku.

#### <a name="allowed-internet-rdp-to-backend"></a>(Povolené) Internet RDP do back-end
1.  Správce serveru na internet požádá o RDP relace AppVM01 prostřednictvím SecSvc001.CloudApp.Net:8025, kde 8025 je číslo portu uživatel přiřazený pravidlo brány firewall "RDP k AppVM01"
2.  Cloudové služby předá přenosů otevřít koncový bod port 8025 rozhraní brány firewall na 10.0.0.4:8025
3.  Žádné NSG přiřazené k zabezpečení podsítě tak systémových NSG pravidel přenosy brány firewall
4.  Brána firewall, nebude zahájen zpracování pravidla:
  1.    Přesměrování pravidla 1 (Správa Přesměrování) není použití, přesuňte do další pravidla
  2.    Přesměrování pravidla 2 (RDP IIS) není použití, přesuňte do další pravidla
  3.    Přesměrování pravidla 3 (RDP DNS01) není použití, přesuňte do další pravidla
  4.    Použití pravidel Přesměrování 4 (RDP AppVM01), jsou povoleny přenosy, dále bránu firewall a 10.0.2.5:3386 (RDP portu AppVM01)
5.  Podsítě back-end začíná zpracování příchozí pravidla:
  1.    Problém se netýká NSG pravidla 1 (blok Internet) (Tento přenos byl překladu síťových adres by bránou firewall, tedy adresy zdrojového je teď bránu firewall, která je na podsítě zabezpečení a prezentací, které podsítě back-end NSG jako "místní" přenosy a tedy povolené), přesunutí k další pravidlo
  2.    Výchozí NSG pravidla přenosy podsítě k podsítě, jsou povoleny přenosy, zastavit zpracování pravidel NSG
6.  AppVM01 je sleduje RDP přenos a odpoví
7.  Žádná odchozího pravidla NSG použít výchozí pravidla a jsou povoleny zpáteční přenosy
8.  UDR přesměrovává odchozí přenosy na brána firewall jako přesměrování
9.  Protože to vrací přenosy na relaci založení předává odpovědi internet uživatele
10. Relace RDP je povolena
11. AppVM01 zobrazí dotaz na uživatelské jméno heslo

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Povolené) Vyhledání Server DNS webu na serveru DNS
1.  Webový Server, IIS01, potřeb datového kanálu na www.data.gov, ale je potřeba přeložit adresu.
2.  Konfigurace sítě pro seznamy VNet DNS01 (10.0.2.4 podsítě back-end) jako primární server DNS, IIS01 odešle žádost o DNS DNS01
3.  UDR přesměrovává odchozí přenosy na brána firewall jako přesměrování
4.  Žádná odchozího pravidla NSG je vázaný na podsítě Frontend, jsou povoleny přenosy
5.  Brána firewall, nebude zahájen zpracování pravidla:
  1.    Přesměrování pravidla 1 (Správa Přesměrování) není použití, přesuňte do další pravidla
  2.    Přesměrování pravidlo 2 až 5 (RDP pravidla) není použití, přesuňte do další pravidla
  3.    Přesměrování pravidla 6 a 7 (aplikace pravidla) není použití, přesuňte do další pravidla
  4.    Přesměrování pravidla 8 (na Internetu) není použití, přesuňte do další pravidla
  5.    Použití pravidel Přesměrování 9 (DNS), jsou povoleny přenosy, brány firewall přepošle umožnění datových přenosů do 10.0.2.4 (DNS01)
6.  Podsítě back-end začíná zpracování příchozí pravidla:
  1.    NSG pravidla 1 (blok Internet) není použití, přesuňte do další pravidla
  2.    Výchozí NSG pravidla přenosy podsítě k podsítě, jsou povoleny přenosy, zastavit zpracování pravidel NSG
7.  DNS server obdrží žádost
8.  DNS server nemá adresu mezipaměti a dotazem kořenový server DNS na Internetu
9.  UDR přesměrovává odchozí přenosy brány firewall jako přesměrování
10. Žádná odchozího pravidla NSG back-end podsítě jsou povoleny přenosy
11. Brána firewall, nebude zahájen zpracování pravidla:
  1.    Přesměrování pravidla 1 (Správa Přesměrování) není použití, přesuňte do další pravidla
  2.    Přesměrování pravidlo 2 až 5 (RDP pravidla) není použití, přesuňte do další pravidla
  3.    Přesměrování pravidla 6 a 7 (aplikace pravidla) není použití, přesuňte do další pravidla
  4.     Použití Přesměrování pravidla 8 (na Internetu), jsou povoleny přenosy, které relace SNAT mimo kořenový server DNS na Internetu
12. Internet DNS server odpovídá, protože pro tuto relaci byla spuštěná z bránu firewall, odpověď přijetím odvolat bránou firewall
13. Je to založení relace, bránu firewall přepošle odpověď zahajující serveru DNS01
14. Podsítě back-end začíná zpracování příchozí pravidla:
  1.    NSG pravidla 1 (blok Internet) není použití, přesuňte do další pravidla
  2.    Výchozí NSG pravidla přenosy podsítě k podsítě, jsou povoleny přenosy, zastavit zpracování pravidel NSG
15. DNS server obdrží uloží odpověď a potom odpoví na původní žádost zpátky na IIS01
16. Směrování UDR podsítě back-end přesměrování zajišťuje bránu firewall.
17. U podsítě back-end existují žádná odchozího pravidla NSG, jsou povoleny přenosy
18. Toto je založení relace v bráně firewall, odpověď předá bránu firewall na server služby IIS
19. Frontend podsítě začíná zpracování příchozí pravidla:
  1.    Neexistuje žádný NSG pravidlo, které se vztahuje na vstupní adres podsítí back-end Frontend podsítě, takže žádné NSG pravidla použít
  2.    Výchozí systém pravidlo povolení přenosu mezi podsítí by přenosy tento tak přenos je povolen
20. IIS01 obdrží odpovědi od DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Povolené) Back-end serveru Frontend server
1.  Správce přihlášeni do AppVM02 prostřednictvím RDP vyžaduje souboru přímo ze serveru IIS01 pomocí Průzkumníka souborů systému windows
2.  Směrování UDR back-end podsítě zajišťuje brány firewall přesměrování
3.  Protože neexistují žádná odchozího pravidla NSG podsítě back-end odpověď smí
4.  Brána firewall, nebude zahájen zpracování pravidla:
  1.    Přesměrování pravidla 1 (Správa Přesměrování) není použití, přesuňte do další pravidla
  2.    Přesměrování pravidlo 2 až 5 (RDP pravidla) není použití, přesuňte do další pravidla
  3.    Přesměrování pravidla 6 a 7 (aplikace pravidla) není použití, přesuňte do další pravidla
  4.    Přesměrování pravidla 8 (na Internetu) není použití, přesuňte do další pravidla
  5.    Přesměrování pravidlo 9 (DNS) není použití, přesuňte do další pravidla
  6.    Použití 10 pravidel Přesměrování (uvnitř adres podsítí), jsou povoleny přenosy, brány firewall předá přenosy 10.0.1.4 (IIS01)
5.  Frontend podsítě začíná zpracování příchozí pravidla:
  1.    NSG pravidla 1 (blok Internet) není použití, přesuňte do další pravidla
  2.    Výchozí NSG pravidla přenosy podsítě k podsítě, jsou povoleny přenosy, zastavit zpracování pravidel NSG
6.  Pokud začátcích slov a tak mohli ověřovat, IIS01 přijímá žádosti a odpoví
7.  Směrování UDR Frontend podsítě přesměrování zajišťuje bránu firewall.
8.  Protože neexistují žádná odchozího pravidla NSG podsítě Frontend odpověď smí
9.  Je to existující relace v bráně firewall povolené tato odpověď a bránu firewall vrátí odpověď AppVM02
10. Back-end podsítě začíná zpracování příchozí pravidla:
  1.    NSG pravidla 1 (blok Internet) není použití, přesuňte do další pravidla
  2.    Výchozí NSG pravidla přenosy podsítě k podsítě, jsou povoleny přenosy, zastavit zpracování pravidel NSG
11. AppVM02 obdrží odpověď

#### <a name="denied-internet-direct-to-web-server"></a>(Odepřen) Internet přímo na webový Server
1.  Uživatel Internet pokusí o přístup k webovému serveru, IIS01, prostřednictvím služby FrontEnd001.CloudApp.Net
2.  Od otevřený pro přenos HTTP nejsou koncové body, to by procházejí cloudové služby a nebudete kontaktovat server
3.  Kdyby koncové body otevřít z nějakého důvodu, NSG (blok Internet) na podsítě Frontend budou blokovány tento provoz
4.  Nakonec UDR směrování adres podsítí Frontend by odešlete odchozí přenosy z IIS01 bránu firewall jako přesměrování a bránu firewall by se toto jako asymetrické přenosy a přetáhnout odchozí odpověď, proto jsou nejméně tři nezávislé vrstvy obrana mezi internet a IIS01 prostřednictvím jeho cloudové služby obrana před neoprávněným nevhodnému přístup.

#### <a name="denied-internet-to-backend-server"></a>(Odepřen) Internet kvůli back-end serveru
1.  Uživatel Internet pokusí o přístup k souboru na AppVM01 prostřednictvím služby BackEnd001.CloudApp.Net
2.  Od otevřený pro sdílení souborů nejsou koncové body, to by předat cloudové služby a nebudete kontaktovat server
3.  Kdyby koncové body otevřít z nějakého důvodu, tento provoz by blokovat NSG (Internet bloku)
4.  Nakonec směrování UDR by odešlete odchozí přenosy z AppVM01 bránu firewall jako přesměrování a bránu firewall by se toto jako asymetrické přenosy a přetáhnout odchozí odpověď, proto jsou nejméně tři nezávislé vrstvy obrana mezi internet a AppVM01 prostřednictvím jeho cloudové služby obrana před neoprávněným nevhodnému přístup.

#### <a name="denied-frontend-server-to-backend-server"></a>(Odepřen) Server frontend do back-end serveru
1.  Předpokládejme IIS01 byl ohroženo a běží škodlivému kódu pokusíte skenování servery podsítě back-end.
2.  Směrování Frontend podsítě UDR by poslat odchozí přenosy z IIS01 bránu firewall jako přesměrování. Toto je něco, co můžete změnit tak, že hostují OM.
3.  Brána firewall by zpracovat přenos, pokud je požadavek AppVM01, nebo na server DNS pro vyhledávání DNS, které přenosy může být potenciálně povolí brány (kvůli 7 pravidel Přesměrování a 9). Všechny ostatní přenosy by být blokovány Přesměrování pravidla 11 (odepřít všem).
4.  Pokud Upřesnit zjišťování hrozbou, že byla povolena v bráně firewall (není součástí tohoto dokumentu najdete v dokumentaci pro zařízení určité sítě pokročilých funkcí hrozbou, že), i přenosy, který by umožňoval pravidly základní přesměrování popisované v tomto dokumentu mohou zabránit, pokud přenos obsahovalo hodnotu známé podpisy nebo fráze, které příznak pravidlo Upřesnit hrozbou, že.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Odepřen) Vyhledání Internet DNS na serveru DNS
1.  Internet při pokusu o vyhledání interní DNS záznamu na DNS01 prostřednictvím služby BackEnd001.CloudApp.Net 
2.  Od otevřený pro přenos DNS nejsou koncové body, to by procházejí cloudové služby a nebudete kontaktovat server
3.  Kdyby koncové body otevřít z nějakého důvodu, NSG pravidlo (blok Internet) na podsítě Frontend budou blokovány tento provoz
4.  Nakonec UDR směrování adres podsítí back-end by odešlete odchozí přenosy z DNS01 bránu firewall jako přesměrování a brána firewall by se toto jako asymetrické přenosy a přetáhnout odchozí odpověď, proto jsou nejméně tři nezávislé vrstvy obrana mezi internet a DNS01 prostřednictvím jeho cloudové služby obrana před neoprávněným nevhodnému přístup.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Odepřen) Internet kvůli SQL přístup přes bránu Firewall
1.  Uživatel Internet požádá SQL data z SecSvc001.CloudApp.Net (Internet protilehlé cloudové služby)
2.  Vzhledem k tomu, že je otevřeno žádné koncové body pro SQL, to by předat Cloudovou službu a by dosáhla bránu firewall.
3.  Kdyby koncové body SQL otevřít z nějakého důvodu, bránu firewall by začít zpracování pravidla:
  1.    Přesměrování pravidla 1 (Správa Přesměrování) není použití, přesuňte do další pravidla
  2.    Přesměrování pravidla 2 až 5 (RDP pravidla) nechcete použít, přesuňte do další pravidla
  3.    Přesměrování pravidla 6 a 7 (pravidel aplikací) nechcete použít, přesuňte do další pravidla
  4.    Přesměrování pravidla 8 (na Internetu) není použití, přesuňte do další pravidla
  5.    Přesměrování pravidlo 9 (DNS) není použití, přesuňte do další pravidla
  6.    10 pravidel Přesměrování (uvnitř adres podsítí) není použití, přesunutí k další pravidlo
  7.    Použití pravidel Přesměrování 11 (odepřít všem) je přenosy zpracování blokované, zastavte pravidla


## <a name="references"></a>Odkazy
### <a name="main-script-and-network-config"></a>Hlavní skript a konfigurace sítě
Uložte celou skript skript Powershellu. Konfigurace sítě uložte do souboru s názvem "NetworkConf2.xml".
Podle potřeby upravte proměnné definované uživatelem. Skript spustit a pak postupujte podle pokynů instalačního programu pravidlo brány Firewall výše.

#### <a name="full-script"></a>Úplné skriptu
Tento skript budou na základě proměnných definované uživatelem:

1.  Připojení k předplatné Azure
2.  Vytvoření nového účtu úložiště
3.  Vytvoření nového VNet a tři podsítě definované v souboru konfigurace sítě
4.  Vytvoření pět virtuálních počítačích; 1 bránu firewall a 4 windows server VMs
5.  Konfigurace, včetně UDR:
  1.    Vytvoření nového tabulkami směrování
  2.    Přidejte trasy do tabulek
  3.    Svázat odpovídající podsítí tabulek
6.  Povolit přesměrování IP na NVA
7.  Konfigurace, včetně NSG:
  1.    Vytváření NSG
  2.    Přidání pravidla
  3.    Vazba NSG na příslušný podsítí

Tento skript Powershellu by měla běžet místně na internetový připojen PC nebo serveru.

>[AZURE.IMPORTANT] Když tento skript spustit, může se upozornění a další informační zprávy, které se objeví v prostředí PowerShell. Pouze chybové zprávy v červené barvě jsou příčinu.

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA
    
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.
    
      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4
     
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"
    
    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #
    
      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }
    
    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan
    
      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"
    
      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal
    
      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName
    
     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
    
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
    

#### <a name="network-config-file"></a>Konfigurační soubor sítě
Uložte tento soubor xml aktualizované umístění a přidat odkaz na tento soubor proměnné $NetworkConfigFile ve výše uvedené skriptu.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Ukázky skriptů aplikace
Pokud chcete nainstalovat aplikace vzorku, i další příklady DMZ, jednu poskytuje následující odkaz: [Ukázka skriptu aplikace][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Obousměrné DMZ s NVA, NSG a UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Zobrazení Logická pravidel brány Firewall"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Vytvoření objektu FrontEnd sítě"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Vytvoření objektu serveru DNS"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopie výchozí RDP pravidlo"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 pravidla"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Ikona aplikace přesměrování"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Ikona překladu síťových adres cíl"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Ikona průchodu"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Správa pravidlo brány firewall"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Pravidlo RDP brány firewall"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Pravidla Web brány firewall"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Pravidlo AppVM01 brány firewall"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Brána firewall odchozího pravidla"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Pravidlo DNS brány firewall"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Pravidlo uvnitř VNet brány firewall"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Brána firewall odepřít pravidla"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Aktivace pravidlo brány firewall"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
