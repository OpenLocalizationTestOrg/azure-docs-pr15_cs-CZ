<properties 
    pageTitle="Integrace aplikace s Azure virtuální sítě" 
    description="Se dozvíte, jak se připojit k novým nebo stávajícím Azure virtuální sítě aplikace v aplikaci služby Azure" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integrace aplikace s Azure virtuální sítě #

Tento dokument popisuje funkce integrace aplikaci služby Azure virtuální sítě a ukazuje, jak nastavit s aplikacemi jiných [aplikací](http://go.microsoft.com/fwlink/?LinkId=529714)služby Azure.  Pokud neznáte Azure virtuálních sítí (VNETs), je to funkci, která umožňuje spoustu Azure zdroje umístění v Internetu jiných sítích routeable řízení přístupu k.  Tyto sítě můžete pak připojení k síti na místní pomocí různých technologie VPN.  Jestliže chce zjistit další informace o virtuálních sítí Azure začít s informacemi o tady: [Virtuální přehled sítě Azure][VNETOverview].  

Aplikace služby Azure má dva typy.  

1. Více klienta systémů podporujících široká řada ceny plány
1. Funkce aplikace služby prostředí řízení premium, která instaluje do vašeho VNET.  

Tento dokument prochází VNET integrace a aplikace prostředí služby.  Pokud chcete další informace o funkci pomocného mechanismu řízení pak začněte s informacemi o zde: [Úvod prostředí aplikace služeb][ASEintro].

Integrace VNET zobrazí váš web app přístup ke zdrojům v síti virtuální ale neposkytuje soukromé přístup do webové aplikace z virtuální sítě.  Soukromé webů přístup je dostupná jenom s pomocného mechanismu řízení nakonfigurované s interní zatížení vyrovnávání (ILB).  Podrobné informace o použití pomocného mechanismu řízení ILB začínat následujícím článku: [vytváření a používání pomocného mechanismu řízení ILB][ILBASE]. 

Běžné situace místo, kam chcete použijete VNET je povolení mít přístup k databázi nebo webových služeb spuštěných virtuálního počítače v síti Azure virtuální z web appu.  Integrace VNET nemusíte zpřístupnit veřejný koncový bod pro aplikace ve OM, ale mohou použijte soukromé Internetu jiných směrovatelné adresy.  

Funkce integrace VNET:

- vyžaduje standardní nebo Premium ceny plán 
- budou fungovat s Classic(V1) nebo VNET Manager(V2) zdroje 
- podporuje protokoly TCP a UDP
- Práce s aplikacemi jiných webový server, mobilní telefon a rozhraní API
- umožňuje aplikace pro připojení k jenom 1 VNET najednou
- umožňuje až na 5 VNETs integraci s služby aplikace plánu 
- Umožňuje stejné VNET pro použití v více aplikací služby aplikace plánu
- podporuje SLA 99,9 % z důvodu závislost na VNET brány

Existují některé věci, které VNET integrace nepodporuje včetně:

- připojení jednotky
- Integrace AD 
- NetBios
- soukromé Web access

### <a name="getting-started"></a>Začínáme ###
Tady jsou některé věci byste měli mít na paměti před připojením k síti virtuální webovou aplikaci:

- Integrace VNET funguje jenom s aplikací na **Standardní** nebo **Premium** ceny plán.  Pokud povolit funkci a potom měřítko plánu aplikace služby nepodporované ceny plánu aplikace ztratí jejich připojení k VNETs používají.  
- Pokud virtuální sítě cílové už existuje, musí mít VPN čárky webu povolené dynamické směrování brána před můžete být připojeni k aplikaci.  Pokud brána nakonfigurovaný s statické směrování není možné povolit čárky webu virtuální privátní sítě (VPN).
- VNET musí být v rámci stejného předplatného jako vaše aplikace služby Plan(ASP).  
- Aplikace, které integrovat VNET použije DNS určené pro tento VNET.
- Ve výchozím nastavení se integrační aplikace pouze směrovat přenosy v síti do svého VNET založené na cesty, které jsou definovány ve vaší VNET.  


## <a name="enabling-vnet-integration"></a>Povolení integrace VNET ##

Tento dokument se zaměřuje na portálu Azure pro integraci VNET.  Postup povolení integrace VNET s aplikací pomocí Powershellu, pokyny tady: [připojení k síti virtuální pomocí prostředí PowerShell aplikace][IntPowershell].

Máte možnost připojení aplikace do nového nebo existujícího virtuální sítě.  Pokud vytváříte novou síť jako součást Integrace potom jenom kromě vytváření i VNET dynamické směrování brány bude předkonfigurovaná za vás a přejděte na web VPN bude povoleno.  

>[AZURE.NOTE] Konfigurace nová integrace virtuální sítě může trvat několik minut.  

Povolení integrace VNET otevřít aplikaci nastavení a potom vyberte sítě.  Uživatelské rozhraní, které se objeví nabízí tři možnosti sítě.  Tato příručka je pouze přejdete do integrace VNET přes připojení hybridní a aplikace služeb prostředí popsané dál v tomto dokumentu.  

Pokud vaše aplikace není ve správné ceny plánu uživatelské rozhraní helpfully umožní zobrazit plánu vyšší cenových plán podle svého výběru.


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Povolení VNET integrace se službou stávajících VNET###
Uživatelské rozhraní integrace VNET umožňuje vybrat ze seznamu vaší VNETs.  Klasický VNETs výskyt znamená, že takové pocházejí se slovem "Klasické" v závorkách vedle názvu VNET.  V seznamu se řadí tak, aby se nejdřív jsou zobrazené VNETs správce prostředků.  Na obrázku dole uvidíte vybraný jenom jeden VNET.  Existuje několik důvodů, že VNET bude zašedlé včetně:

- VNET je v jiné předplatné, které mají přístup k účtu
- VNET nemá čárky web povolená
- VNET nemá dynamické směrování brány


![][2]

Povolení integrace jednoduše kliknout na VNET chcete integrovat.  Po výběru VNET aplikace automaticky restartovat změny se projeví.  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Povolení, přejděte na web ve klasické VNET #####
Pokud vaše VNET nemá brány ani má bod k webu budete muset tomuto nastavení první.  Postup pro klasické VNET přejděte na [Portál Azure] [ AzurePortal] a zobrazte seznam virtuální Networks(classic).  Zde klepněte na tlačítko v síti chcete integrovat a klikněte na pole velký v části Základy s názvem sítě VPN.  Tady můžete vytvořit svůj, přejděte na web VPN a ještě jste je vytvoření brány pro.  Po přejdete kliknutím na místo na web se prostředí vytváření brány bude asi 30 minut, než je připraven.  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Povolení, přejděte na web ve VNET správce prostředků #####

Správce prostředků VNET nakonfigurovat brány a najeďte myší na webu, budete muset pomocí prostředí PowerShell jak je uvedeno, [Konfigurace čárky webu připojení k síti virtuální pomocí prostředí PowerShell][V2VNETP2S].  Uživatelské rozhraní provádět tato možnost není k dispozici. 

### <a name="creating-a-pre-configured-vnet"></a>Vytváření předkonfigurovaná VNET ###
Pokud chcete vytvořit nové VNET nakonfigurovaný bránu a bod k webu, aplikací služby sítě uživatelského rozhraní má možnost dělat, ale pouze pro správce prostředků VNET.  Pokud chcete vytvořit klasické VNET brány a čárky webu budete muset provést ručně prostřednictvím uživatelského rozhraní sítě. 

Vytvoření správce prostředků VNET prostřednictvím uživatelského rozhraní integrace VNET, jednoduše vyberte **Vytvořit nový virtuální sítě** a poskytují:

- Virtuální síťový název
- Blok adresy virtuální sítě
- Podsítě název
- Blok adresy podsítě
- Blok adresy brány
- Blok adresy bodu webu

Pokud chcete tento VNET se připojit k některé z vaší síti byste neměli, vyberete IP adresní prostor, který překrývá tyto sítích.  

>[AZURE.NOTE] Vytvoření správce prostředků VNET pomocí brány trvá asi 30 minut a aktuálně nebude integrovat VNET aplikace.  Po vytvoření vaší VNET s brány potřebujete vrátit do aplikace VNET integrace uživatelského rozhraní a vyberte nové VNET.

![][3]

Azure VNETs obvykle vytvořené v rámci adresy privátní sítě.  Ve výchozím nastavení VNET integrace funkce bude směrovat všechny přenosy určené pro tyto rozsahy IP adres do svého VNET.  Soukromé rozsahy IP adres jsou:

- 10.0.0.0/8 – to je stejný jako 10.0.0.0 - 10.255.255.255
- 172.16.0.0/12 – to je stejný jako 172.16.0.0 - 172.31.255.255 
- 192.168.0.0/16 – to je stejný jako 192.168.0.0 - 192.168.255.255
 
Adresní prostor VNET musí být zadán v CIDR zápisu.  Pokud neznáte CIDR zápisu, je metodu zadávání pomocí IP adresy a celé číslo, které představuje masky sítě blok adresy. Jako stručný přehled zvažte 10.1.0.0/24 bude 256 adres a 10.1.0.0/25 bude 128 adresy.  Adresy IPv4 pomocí /32 bude právě 1 adresu.  

Pokud sem zadejte informace o serveru DNS pak, nastaví se pro váš VNET.  Po vytvoření VNET můžete upravit tyto informace z VNET uživatelského prostředí.

Při vytváření klasické VNET pomocí rozhraní VNET integrace se vytvoří VNET ve stejné skupině zdroje jako aplikace. 

## <a name="how-the-system-works"></a>Jak funguje systému ##
Na pozadí je tato funkce založena přes VPN čárky webu technologie pro připojení aplikace k vaší VNET.   Aplikace v aplikaci služby Azure mít architektura více klienta systému, který vylučuje zřizujete aplikaci přímo v VNET jako hotový s virtuálních počítačích.  Vytvořením na technologii čárky webu jsme omezit přístup k síti jenom virtuálního počítače hostitelské aplikace.  Přístup k síti další omezení u těchto hostitelů aplikace, aby vaše aplikace jenom přístup, které jim přístup ke konfiguraci sítě.  

![][4]
 
Pokud jste to ještě nakonfigurovali serveru DNS k síti virtuální musíte se IP adres.  Při používání IP adresy, mějte na paměti, že je hlavních výhod tato funkce umožňuje použití soukromých adres ve své soukromé síti.  Pokud nastavíte aplikace používal veřejnou IP adresy pro jeden z vaší VMs a nepoužíváte funkci VNET integrace a komunikace přes internet.


##<a name="managing-the-vnet-integrations"></a>Správa integrace VNET##

Možnost připojení a odpojení VNET je na úrovni aplikace.  Operace, které můžou mít vliv integrace VNET napříč několika aplikace jsou úrovni ASP.  Na uživatelské rozhraní, které se zobrazují na úrovni aplikace můžete získat informace na vaší VNET.  Většina stejné informace se taky zobrazí na úrovni ASP.  

![][5]

Na stránce Stav sítě funkce se zobrazí, pokud vaše app stejně připojuje k vaší VNET.  Pokud brána VNET určen něco jiného, co důvod pak, to se zobrazí jako není připojen.  

Informace, které teď máte k dispozici v aplikaci, kterou úrovně uživatelského rozhraní VNET integrace je stejná jako podrobné informace získáte od ASP.  Tady jsou tyto položky:

- Otevře VNET název - tento odkaz síť uživatelského rozhraní
- Umístění - odráží umístění svého VNET.  Je možné integrovat s VNET do jiného umístění.
- Stav certifikátu - jsou certifikáty používá k zabezpečení připojení VPN mezi aplikace a VNET.  To odráží test má jistotu, že jsou synchronizovány.
- Stav brány - vaše bran třeba dolů ať už z jakéhokoliv důvodu pak aplikace nemají přístup k prostředkům v VNET.  
- VNET adresní prostor – to je IP adres pro vaše VNET.  
- Přejděte na web adresní prostor – to je přejděte na web IP adresu prostoru pro vaše VNET.  Aplikace se zobrazí komunikace jako pocházející z jednoho z IP adresy do tohoto pole adresy.  
- Webu na webu adresní prostor - použijete sítěmi VPN k připojení vaší VNET na místní zdroje nebo jiných VNETs.  Pokud máte, které nakonfigurovali a rozsahy IP adres tato pole definovány připojení VPN zobrazí tady.
- Servery DNS – Pokud máte servery DNS s vaší VNET nakonfigurovanou a jsou zde uvedeny.
- IP adresy přesměrované do VNET - tam je seznam IP adres, že má vaše VNET směrování definované pro.  Tyto adresy se zobrazí v tomto poli.  

Pouze operaci, kterou budete moct využívat v zobrazení aplikace VNET integrace je odpojit VNET je právě připojený k aplikaci.  To jednoduše klikněte na možnost Odpojit nahoře.  Tato akce nezmění vaší VNET.  VNET a jeho konfigurace včetně brány se nezmění.  Pokud pak chcete-li odstranit vaší VNET musíte nejprve odstranit zdroje v něm včetně brány.  

Zobrazení časového plánu aplikace služby obsahují několik dalších operací.  Je taky přistupuje jinak než v aplikaci.  Kontaktovat rozhraní sítě ASP jednoduše otevřete ASP uživatelského rozhraní a posuňte se dolů.  Existuje prvek uživatelského rozhraní s názvem stav sítě funkce.  Poskytne některé menší podrobnosti kolem VNET integrace.  Po kliknutí na tento uživatelského rozhraní otevře uživatelského rozhraní funkce stav sítě.  Když pak kliknete na "Kliknutím sem můžete spravovat" otevřou uživatelského rozhraní, který obsahuje integrace VNET v tomto ASP.

![][6]

Umístění ASP je dobré pamatovat při pohledu umístění VNETs integrace s.  Po do jiného umístění VNET jste mnohem více pravděpodobně zobrazí latenci problémy.  

VNETs integrovaný s je připomínku na kolik VNETs, jestli jsou vaše aplikace integrována v tomto ASP a kolik můžete mít.  

Zobrazíte další informace na každý VNET stačí klikněte na VNET vás zajímají.  Kromě podrobnosti, které byly bylo uvedeno dříve, že taky uvidíte seznam aplikací v tomto ASP, kteří používají tento VNET.  

Pokud jde o akce jsou dvě primární akce.  První je možnost Přidat postupy, které jednotka přenosů z aplikace do svého VNET.  Druhá akce je možnost Synchronizovat certifikáty a informace o síti.

![][7]

**Směrování** Výše uvedených cesty, které jsou definovány ve vaší VNET jsou, jaký se používá k přesměrování přenosy do VNET z aplikace.  Když místo, kam Zákazníci chcete odeslat další odchozí přenosy z aplikace do VNET a je tato možnost je k dispozici je několik používá.  Co se stane s přenos, po až jak zákazníka nakonfiguruje jejich VNET.  

**Certifikáty** Stav certifikátu odráží prováděné službou aplikace k ověření, že je stále dobré certifikáty, které jsme používáte pro připojení k síti VPN kontroly.  Při integraci VNET povoleno, pokud se jedná první integrace, které VNET žádných aplikací v tomto ASP nejsou požadované exchange certifikátů k zabezpečení připojení.  Spolu s certifikáty jsme potřebujete konfigurace DNS, cesty a další podobné věci, které popisují sítě.
Pokud dojde ke změně tyto certifikáty nebo síti informace budete muset klikněte na "Synchronizovat síť".  **Poznámka**: po klepnutí na tlačítko "Synchronizovat síť", pak můžete způsobí stručný výpadku připojení mezi aplikace a vaše VNET.  Zatímco aplikace nebude nerestartuje ztrátě připojení může způsobit svůj web a ne (funkce) správně.  

##<a name="accessing-on-premise-resources"></a>Přístup k místní zdroje##

Jednou z výhod funkce integrace VNET je, že pokud váš VNET je připojení k síti na místní prostřednictvím sítě VPN na webu aplikace mohou mít taky přístup k na místní zdroje z aplikace a pak.  Tento postup vyžaduje když budete muset aktualizovat bránu VPN na místní trasy pro vaše, přejděte na web IP oblasti.  Když VPN sítěmi nejdřív nastavit skripty použité ji nakonfigurovat by měl nastavit cesty včetně přejděte na web VPN.  Pokud přidáte místo na webu VPN po vaší vytvořit sítěmi VPN, je třeba ručně aktualizovat trasy.  Podrobnosti o tom, jak to udělat závisí na brány a nejsou zde popsané.  

>[AZURE.NOTE] Zatímco funkce integrace VNET fungovat s VPN sítěmi pro přístup k pro místní zdroje aktuálně fungovat s ExpressRoute VPN udělat stejný.  To platí při integraci s Classic nebo správce prostředků VNET.  Pokud potřebujete s přístupem k prostředkům přes ExpressRoute VPN můžete pomocného mechanismu řízení, který mohlo by umožnit spuštění ve vaší VNET. 

##<a name="pricing-details"></a>Podrobnosti o cenách##
Existuje několik ceny detailů, které je třeba si uvědomit při použití funkce integrace VNET.  Existují 3 souvisejících náklady na použití této funkce:

- ASP ceny požadavky osy
- Náklady na převod dat
- Brána VPN náklady.

Vaše aplikace nebudou moct používat tuto funkci budou muset být na standardní nebo Premium aplikace služby plánování.  Zobrazí se další informace na tyto náklady: [Aplikace služby ceny][ASPricing]. 

Z důvodu je možné čárky VPN webu jsou zpracovány, máte vždycky částka za odchozí dat prostřednictvím integrace VNET připojení i v případě, že VNET je ve stejném datovém centru.  Pokud chcete zjistit, jaká tyto náklady se podívejte sem: [Podrobnosti přepojit ceny dat][DataPricing].  

Poslední položky je cena VNET brány.  Pokud nepotřebujete brány co chcete najít například sítěmi VPN pak můžete platíte bran podporuje funkci VNET integrace.  Na náklady, tady jsou podrobnosti: [Ocenění brány VPN][VNETPricing].  

##<a name="troubleshooting"></a>Řešení potíží##

Při nastavení můžete snadno funkci, která neznamená, po které bude prostředí problému bezplatná.  By měl narazíte na problémy s přístupem k požadované koncový bod tam jsou některé nástroje, které můžete použít k testování připojení z konzoly aplikace.  Existují dva konzoly prostředí, které můžete použít.  Jedna je v konzole Kudu a druhý konzole se dostanete na portálu Azure.  Pro přístup ke konzole Kudu z aplikace-přejít na Nástroje > Kudu.  To je stejný jako přejdete na [název webu]. scm.azurewebsites.net.  Jakmile, která otevře jednoduše přejděte na kartu konzoly ladění.  Získat ke konzole Azure portálu hostovanou a pak z aplikace-přejít na Nástroje > konzoly.  


####<a name="tools"></a>Nástroje####

Ping nástroje nslookup a tracert nebude fungovat prostřednictvím konzoly z důvodu omezení zabezpečení.  Vyplnění byly void tam dvou samostatných nástroje přidali.  K otestování funkcí DNS jsme přidali nástroje s názvem nameresolver.exe.  Syntaxe je:

    nameresolver.exe hostname [optional: DNS Server]

Nameresolver umožňuje zkontrolovat názvy hostitelů, které závisí na tom aplikace.  Tímto způsobem můžete otestovat, pokud máte všechno, co chybně nakonfigurované s DNS nebo možná nebudete mít přístup k serveru DNS.

Další nástroj umožňuje otestujte TCP připojení ke kombinaci Host (hostitel) a portu.  Tento nástroj se nazývá tcpping.exe a je syntaxe:

    tcpping.exe hostname [optional: port]

Tento nástroj se pozná, že pokud zobrazíte konkrétní hostitele a portu, ale nebude zvolený s ICMP získáte základě Nástroj ping.  Nástroj ping ICMP bude zjistit, zda je váš hostitel nahoru.  S tcpping zjištění, pokud máte přístup k určitému portu na hostiteli.  


####<a name="debugging-access-to-vnet-hosted-resources"></a>Ladění přístup k VNET hostované zdroje####

Existuje několik věcí, které můžete zabránit doručení konkrétní hostitel a port aplikace.  Ve většině případů je nějaká tři položky:

- **Je brána firewall způsobem**  Pokud používáte bránu firewall způsobem bude časový limit TCP přístupů.  To je v tomto případě 21 sekund.  Použijte nástroj tcpping testovat připojení.  TCP časové limity dají kvůli hodně věcí za bránách firewall, ale začít zde.  
- **Je to DNS není k dispozici**  Časový limit DNS je 3 sekundy na DNS server.  Pokud máte 2 servery DNS, který je 6 sekund.  Pomocí nameresolver jestli DNS funguje.  Uvědomte si, že nslookup nelze použít, která nepoužívá DNS vaší VNET nakonfigurována.
- **Rozsah neplatný P2S IP** Přejděte na web rozsah IP musí být v RFC 1918 soukromé rozsahy IP adres (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) Pokud oblast používá IP adresy mimo, pak co nebude fungovat.  

Pokud tyto položky si odpovědi na váš problém, Hledat první pro jednoduché například:  

- Ukazuje brány tak, aby v portálu?
- Certifikáty zobrazit jako synchronní?
- Aniž byste museli dělat "Synchronizace síti" v problémového ASP kdokoli došlo změně konfigurace sítě? 

Pokud brána je dolů přepněte ho zálohovat.  Pokud vaše certifikáty jsou nejsou synchronizovány návrat do zobrazení ASP VNET integrace a stiskněte klávesu "Synchronizovat síť".  Pokud máte podezření, že byl změny provedené v konfiguraci VNET a nebyl synchronizace by se vaše ASP potom přejděte do zobrazení ASP VNET integrace a stiskněte klávesu "Synchronizovat síť" pouze jako připomenutí, to bude mít za následek výpadku stručný s VNET připojení a vaše aplikace.  

Pokud všechny, který je v pořádku, budete muset prozkoumat trochu důkladněji:

- Nejdou nějaké jiné aplikace pomocí integrace VNET kontaktovat zdrojů ve stejném VNET? 
- Můžete přejít ke konzole aplikace a používat tcpping kontaktovat ostatní prostředky ve vaší VNET?  

Pokud platí některá z výše uvedených pak je v pořádku VNET integrace a je to jinam, postupujte takto.  Je to, kde se být více výzvu, protože nejde žádným způsobem jednoduché zobrazíte, proč se nemůžu dostanete Host (hostitel): portu.  Příčin patří:

- používáte bránu firewall na hostitele brání přístup k port aplikace ukazatel rozsah IP webu.  Často přes podsítí vyžadují připojení k veřejné.
- Cílový hostitel je vypnutý
- aplikace je dolů
- jste měli nepovedlo IP nebo název hostitele
- aplikace je přijímá na jiný port než se očekává.  Zjistíte to tak, že přejdete na tomto hostiteli a práce s nimi "netstat - aon" z příkazového řádku.  Tím zobrazíte jaký proces ID listening na co port.  
- skupiny zabezpečení sítí nakonfigurovaná způsobem, budou zabránit přístupu k hostitele aplikace a port z vaší, přejděte na web rozsah IP

Myslete na to, že nevíte, jaký IP v ukazatel rozsah IP webu, který bude aplikace používat, budete muset povolit přístup z celé oblasti.  

Obsahuje další ladění kroky:

- Přihlaste se k jiné OM ve vaší VNET a pokusí kontaktovat Host (hostitel): port zdroje odtud.  Existují některé TCP ping nástroje můžete použít k tomuto účelu nebo dokonce pokud můžete použít telnet třeba.  K čemu slouží je prostě zjistit, zda připojení tam z této OM. 
- přenesení instalaci aplikace na jiné OM a otestujte přístup k této Host (hostitel) a port z konzoly z aplikace  
####<a name="on-premise-resources"></a>Pro místní zdroje####
Pokud vaše mít přístup k prostředkům místního nasazení je první věc, měli byste zkontrolovat, pokud se dostanete zdroj ve vaší VNET.  Pracuje, která je poměrně snadná jsou dalším krokům.  Z OM ve vaší VNET budete muset pokuste se dosáhnout na místní aplikace.  Můžete použít telnet nebo nástroj ping TCP.  Vaše OM nelze kontaktovat na místní zdroje nakonec nejdřív zkontrolujte připojení k síti VPN sítěmi pracuje.  Pokud je funkční zkontrolujte totéž poznamenat dříve i na místní brána konfiguraci a stavu.  

Teď Pokud hostovány vaše VNET OM zobrazíte na místní systém ale aplikace nelze důvod, proč je pravděpodobně jednu z těchto věcí:
- svoje směry nejsou nakonfigurovány s vaší, přejděte na web rozsahy IP adres do pole na místní brána
- skupiny zabezpečení sítě blokují přístupu pro vaše, přejděte na web IP oblast
- v bránách firewall místní blokují vysílání mezi bodem rozsah IP webu
- budete mít uživatel definované Route(UDR) na vaše VNET zabraňující ukazatel na webu na základě přenosy sahající v místní síti

## <a name="hybrid-connections-and-app-service-environments"></a>Hybridní připojení a prostředí aplikace služby##
Existuje 3 funkce, které umožňují přístup ke zdrojům VNET hostované.  Jsou:

- Integrace VNET
- Hybridní připojení
- Aplikace služby prostředí

Hybridní připojení vyžaduje instalaci agenta relay s názvem Manager(HCM) připojení hybridní ve vaší síti.  HCM je potřeba se připojit k Azure a aplikaci.  Toto řešení je zejména skvělé z vzdáleně například v místní síti nebo i jiné cloudové hostované sítě, protože nevyžaduje internet přístupných osobám s postižením koncový bod.  HCM lze spustit pouze v systému Windows a může mít až na 5 instance spuštěna poskytnout dostupnost.  Hybridní připojení podporuje pouze TCP přes a každý HC koncový bod se musí shodovat kombinaci konkrétní Host (hostitel): port.  

Funkce aplikace služby prostředí umožňuje spustit instanci aplikace služby Azure ve vaší VNET.  Díky zdrojů aplikace access v VNET bez dalších kroků.  Některé další výhody prostředí aplikace služby je, že můžete použít 8 core snaží zaměstnanců s 14 GB paměti RAM.  Další výhodou je, že je možné přizpůsobit systému podle svých potřeb.  Na rozdíl od více klienta prostředí, kde je vaše ASP omezenou velikost v pomocného mechanismu řízení můžete určit, kolik zdrojů, kterým chcete dát k systému.  Týkající fokus sítě tohoto dokumentu však jedním z kroků, které získáte s pomocného mechanismu řízení, který není integrací VNET je, můžete pracovat s ExpressRoute VPN.  

Když se, že některé používají případu překrytí žádný z těchto funkcí nahraďte všechny ostatní.  Vědět, jaké funkce používat je svázané se vašim potřebám a jak budete chtít použít.  Příklad:

- Pokud jste vývojáři a jenom potřebujete ke spuštění webem v Azure a mít přístup k databázi na workstation podle svého pracovního stolu je nejjednodušší cesta používat hybridní připojení.  
- Pokud jste velké organizace, která chcete umístit velkého počtu webové vlastnosti veřejnosti cloudu a spravovat vlastní sítě a chcete se vrátit s prostředím služby aplikace.  
- Pokud máte řadu aplikaci služby hostovaný aplikací a jednoduše má přístup k prostředkům ve vaší VNET VNET integrace je způsob, jak přejít.  

Za případy použití existují některé zjednodušení související aspekty.  Pokud vaše VNET už připojení k síti na místní pomocí integrace VNET nebo prostředí aplikace služby je snadný způsob, jak používat pro místní zdroje.  Na druhé straně Pokud váš VNET není připojený k síti na místní je jich režijních nastavit připojení k webu vašeho VNET ve srovnání s instalací HCM.  

Kromě rozdíly ve funkcích vzít v úvahu i ceny rozdíly.  Funkce aplikace služby prostředí je Premium služby nabízející ale nabízí největší sítě možností konfigurace kromě Další skvělé funkce.  Integrace VNET se dá používat se standardní nebo ASP Premium a je ideální pro bezpečné jinými zdrojů v VNET z více klienta aplikaci služby.  Hybridní připojení aktuálně závisí na tom BizTalk účtu, který má ceny úrovně, které spusťte zdarma a pak se postupně více drahé na základě velikosti, které potřebujete.  Když přijde na práci v mnoha sítích přes, je žádná funkce jako hybridní připojení, které vám umožní přístup k prostředkům ve i víc než 100 jednotlivé sítě.    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
