<properties
    pageTitle="Jak nakonfigurovat aplikaci služby prostředí | Microsoft Azure"
    description="Konfigurace, Správa a sledování aplikací služby prostředí"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>


# <a name="configuring-an-app-service-environment"></a>Konfigurace prostředí aplikace služby #

## <a name="overview"></a>Základní informace ##

Na vysoké úrovni prostředí služby Azure aplikace se skládá z několika hlavní komponenty:

- Výpočet prostředky, které používáte v prostředí aplikace služeb hostovaných služby
- Úložiště
- Databáze
- Classic(V1) nebo zdroje Manager(V2) Azure virtuální síti (VNet) 
- Podsítě prostředí aplikace služeb hostovaných služba spuštěná v něm

### <a name="compute-resources"></a>Výpočet zdrojů

Používáte pro využití prostředků pro fondy čtyři zdrojů.  Jednotlivé aplikace služby prostředí řízení obsahuje sadu front-end a tři možné pracovní skupiny. Nemusíte používat všechny tři pracovní skupiny – Pokud budete potřebovat, můžete jenom použít jednu nebo dvě.

Hosts v fondů zdrojů (front-end a zaměstnanců) nejsou přímo přístupných osobám s postižením k tenantům. Nelze použít Vzdálená plocha (RDP Protocol) připojovat se k nim, měnit jejich zřizování nebo fungují jako správce k nim.

Můžete nastavit množství fondu zdrojů a velikost. V pomocný máte čtyři velikost možnosti, které jsou označeny P1 prostřednictvím P4. Podrobnosti o tyto formáty a jejich ceny najdete v tématu [ceny aplikaci služby](../app-service/app-service-value-prop-what-is.md).
Změnit počet nebo velikost nazývá operace měřítko.  Jedinou měřítko operace může být v průběhu najednou.

**Přední konce**: přední konce se koncové body protokolu HTTP/HTTPS pro vaše aplikace, které jsou ve vaší pomocného mechanismu řízení. V front-end nespouštět úloh.

- Pomocného mechanismu řízení začíná dvou P2s, tedy dostatečný pro vývojáře nebo zkoušení pracovního vytížení a úloh nízkoúrovňový výroby. Důrazně doporučujeme P3s pro střední k hodně výrobní úloh.
- Průměrné až hodně výrobní zatížení doporučujeme mít nejméně čtyři P3s zajistit existuje dostatek front-end spuštění, když nastane plánovanou údržbu. Aktivity plánovaná údržba zobrazíte dolů jeden front-end najednou. Sníží celkové dostupné front-end kapacity během činnosti spojené s údržbou.
- Nové instance front-end nemůžete přidat okamžitě. Můžou používat mezi 2 až 3 hodiny trvat.
- Další doladění měřítko, můžete sledovat procento využití procesoru a paměti procento metrik aktivních požadavků pro fondu front-end. Pokud procenta procesoru a paměti nad 70 procent při spuštění P3s, přidejte další front-end. Pokud hodnota aktivních požadavků průměrů na 15 000 20 000 žádosti na jeden front-end, měli byste taky přidat další front-end. Celková cílem je zachovat procesoru a paměti procent dole 70 % a aktivních požadavků Výpočet průměru k pod 15 000 žádosti na jeden přední konce při provozu P3s.  

**Zaměstnanci**: zaměstnanci jsou, které skutečně spuštění aplikace. Škálování vaše plány aplikaci služby používající nahoru pracující s informacemi z fondu přidružené pracovníka.

- Pracovníci nemůžete přidat okamžitě. Může trvat 2 až 3 hodiny by bylo možné, bez ohledu na to, kolik při přidávání.
- Měřítko velikost výpočetním zdroje pro všechny fondu bude trvat 2 až 3 hodiny za aktualizace domény. Je v pomocného mechanismu řízení 20 aktualizace domén. Pokud diagramů s měřítky výpočetním velikost fond pracovníka s 10 instancemi může trvat mezi hodin 20 až 30.
- Změna velikosti výpočetním prostředky, které se používají v fond pracovní způsobí studenou spustí spuštěné v tomto pracovníka fondu aplikací.

Nejrychlejší způsob, jak změnit velikost zdroje výpočetním skupinu pracovníka, která není spuštěný žádných aplikací, je:

- Neomezovat počet instancí na hodnotu 0. Zrušit vaší instance bude trvat asi 30 minut.
- Vyberte novou výpočetním velikost a počet instancí. Tady bude trvat mezi hodin 2 až 3.

Pokud vaše aplikace vyžadují větší zdroje výpočetním, nemůžete využít výhod předchozí pokyny. Nemusíte měnit velikost fondu kolegy, který je hostitelem těmito aplikacemi, můžete naplnit další fond pracovníka s dalšími lidmi požadované velikosti a ukazatel aplikace do tohoto fondu.

- Vytvořte další instance tohoto velikost potřebné výpočetním do jiného pracovního fondu. To bude trvat 2 až 3 hodiny dokončete.
- Změna přiřazení vaše plány aplikaci služby, které hostitelem aplikace, které potřebujete větší velikost do fondu nově konfigurované pracovní. Toto je rychlé operace, který by měl mít méně než jednu minutu dokončete.  
- Pokud nepotřebujete tyto nepoužívané instance Neomezovat prvního pracovního fondu. Tuto operaci trvá asi 30 minut.

**Neobsahovaly text**: nástrojů, které pomáhají spravovat využití prostředků výpočetním reprodukujte neobsahovaly text. Můžete použít neobsahovaly text pro front-end nebo pracovní skupiny. Můžete dělat akcí například tlačítka Zvětšit instance jakéhokoli typu fondu během dopoledních hodin a snížit večer. Nebo případně můžete přidáte instance počet pracující s informacemi, které jsou dostupné v fond pracovníka vynechává pod určitou prahovou hodnotu.

Pokud chcete nastavit automatické měřítko pravidla týkající se výpočetním metriky fondu zdrojů, pak mějte na paměti údaj o čase, zřizování vyžaduje. Podrobné informace o neobsahovaly text aplikace služby prostředí, najdete v článku [jak nakonfigurovat automatické měřítko v prostředí aplikace služby][ASEAutoscale].

### <a name="storage"></a>Úložiště

Každý pomocného mechanismu řízení nastaven 500 GB úložiště. Zde se používá ve všech aplikacích pomocného mechanismu řízení. Toto úložiště je součástí pomocného mechanismu řízení a aktuálně se nedají přepínat používat úložného prostoru. Pokud do směrování virtuální sítě nebo zabezpečení při provádění úprav, budete muset přesto povolit přístup k základnímu úložišti Azure – nebo pomocného mechanismu řízení nefunguje.

### <a name="database"></a>Databáze

Databáze obsahuje informace, které definují prostředí, jakož i informace o aplikacích, ve kterých se nepoužívá v něm obsažené. Toto je příliš součástí předplatného uskutečňuje Azure. Je něco, co máte přímé možnost pracovat. Pokud do směrování virtuální sítě nebo zabezpečení při provádění úprav, budete muset přesto povolit přístup k SQL Azure – nebo pomocného mechanismu řízení nefunguje.

### <a name="network"></a>Sítě

VNet, který se používá se vaše pomocného mechanismu řízení může být jednu, které jste udělali při vytvoření pomocného mechanismu řízení nebo které jste udělali předem. Když vytvoříte podsítě při vytváření pomocného mechanismu řízení, vynutí pomocného mechanismu řízení byli ve stejném skupina zdroje jako virtuální sítě. Pokud potřebujete skupina zdroje používané vaší pomocným jiný než svůj VNet, je potřeba vytvořit svůj pomocného mechanismu řízení pomocí šablony správce zdrojů.

Existují některá omezení virtuální sítě, který se používá pro pomocného mechanismu řízení:
 
- Virtuální sítě musí být místní virtuální sítě.
- Je třeba podsítě 8 nebo víc adres kde nasazení pomocného mechanismu řízení.
- Poté, co podsítě hostovat pomocného mechanismu řízení, nemůžete už změnit rozsah adres podsítě. Proto doporučujeme podsítě obsahuje aspoň 64 adresy tak, aby pokryly všechny budoucí růst pomocného mechanismu řízení.
- Je možné nic jiného podsítě ale pomocného mechanismu řízení.

Na rozdíl od hostovanou službu, která obsahuje pomocný [virtuální sítě] [ virtualnetwork] a podsítě pod kontrolou uživatele.  Můžete spravovat svou síť virtuální prostřednictvím virtuální uživatelského rozhraní sítě nebo Powershellu.  Pomocného mechanismu řízení můžete nasazenou v Classic nebo VNet správce zdrojů.  Portál a rozhraní API prostředí mírně odlišnou mezi klasické a správce prostředků VNets ale prostředí pomocného mechanismu řízení je stejný.

VNet, který slouží jako hostitel pomocného mechanismu řízení můžete použít buď soukromé RFC1918 IP adresy nebo ho můžete použít veřejnou IP adres.  Pokud chcete použít rozsah IP, které nejsou zahrnuty RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) a pak je potřeba vytvořit VNet a podsítě použije vaše pomocným před vytvořením pomocného mechanismu řízení.

Protože tuto funkci umístí aplikace služby Azure do virtuálního sítě, znamená to, že vaše aplikace, které jsou hostovány vaše pomocného mechanismu řízení Teď máte přístup ke prostředky, které jsou k dispozici prostřednictvím ExpressRoute nebo na webu virtuální privátní sítě (VPN) přímo. Aplikace, které jsou v prostředí aplikace služby nevyžadují další funkce sítě a získat informace k dispozici pro virtuální sítě, který je hostitelem prostředí aplikace služby. To znamená, že nemusíte pomocí integrace VNET nebo hybridní připojení k prostředkům ve nebo připojení k síti virtuální. Pořád můžete obě tyto funkce když při přístupu k prostředkům ve sítí, ke kterým nejste připojení k síti virtuální.

Můžete třeba VNET integrace integrovat s virtuální sítě, který je ve vašem předplatném, ale není připojený k virtuální síť, která je vaše pomocného mechanismu řízení. Pořád můžete hybridní připojení k přístupu k prostředkům, které jsou v jiných sítích, podobně jako normálně.  

Pokud máte virtuální sítě ExpressRoute VPN nakonfigurována, byste měli znát některých směrování požadavky, které obsahuje pomocného mechanismu řízení. Existují některé konfigurace uživatelem definovaných směrování (UDR), které jsou kompatibilní s pomocného mechanismu řízení. Další informace o spuštění pomocného mechanismu řízení v síti virtuální s ExpressRoute, najdete v článku [spuštěna aplikace služby prostředí virtuální síť s ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Zabezpečení příchozích

Určit příchozí přenosy na váš pomocného mechanismu řízení dva hlavní způsoby.  Síť zabezpečení Groups(NSGs) umožňuje určit, jaké IP adresy přístup k vaší pomocného mechanismu řízení podle popisu v tomto [jak určit příchozích v prostředí aplikace služby](app-service-app-service-environment-control-inbound-traffic.md) a vaše pomocného mechanismu řízení můžete konfigurovat taky s interní Balancer(ILB) načíst.  Tyto funkce lze také použít společně Pokud chcete omezit přístup pomocí NSGs pomocného mechanismu řízení vaší ILB.

Při vytváření pomocného mechanismu řízení vytvoří VIP ve vaší VNet.  Existují dva typy VIP externích a interních.  Při vytváření pomocného mechanismu řízení s externím VIP budou mít přístup přes internet směrovatelné IP adresu aplikace ve vaší pomocného mechanismu řízení. Když vyberete interní vaší pomocného mechanismu řízení budou nakonfigurována ILB a nebudou přímo přístup z Internetu.  Pomocného mechanismu řízení ILB pořád vyžaduje externí VIP ale se používá pouze pro Azure přístup pomocí protokolu správy nebo údržbu.  

Při vytváření pomocného mechanismu řízení ILB poskytnout subdoména používá pomocným ILB a bude mít spravovat vlastní DNS pro subdoména, kterou zadáte.  Protože nastavíte název subdoménu budete potřebovat pro správu certifikát používaný HTTPS přístup pomocí protokolu.  Po vytvoření pomocného mechanismu řízení zobrazí výzva k zadání certifikátu.  Další informace o vytváření a používání pomocného mechanismu řízení ILB přečteno [Interní Vyrovnávání zatížení s prostředím služby aplikace][ILBASE]. 


## <a name="portal"></a>Portál

Můžete spravovat a sledovat prostředí aplikace služby pomocí uživatelské rozhraní na portálu Azure. Pokud máte pomocného mechanismu řízení, pak budete pravděpodobně zobrazíte symbol aplikaci služby na svůj postranní panel. Tento symbol se používá pro znázornění prostředí aplikace služby Azure portálu:

![Symbol služby prostředí aplikace][1]

Otevřete uživatelského rozhraní, který obsahuje seznam všech prostředí aplikace služby, můžete použít na ikonu nebo kliknutím na dvojitou šipku (">" symbol) v dolní části na bočním panelu vyberte aplikaci služby prostředí. Po výběru jedné z ASEs uvedené, otevřete uživatelského rozhraní, který slouží ke sledování a spravovat.

![Uživatelské rozhraní pro monitorování a správu prostředí aplikace služby][2]

Tento první zásuvné jsou uvedeny některé vlastnosti vaší pomocný spolu s metrických graf za fondu zdrojů. Některé vlastnosti, které jsou zobrazeny v bloku **Essentials** jsou také hypertextové odkazy, které otevřou zásuvné, který je spojený s ním. Například můžete vybrat **Virtuální síťový** název otevřete uživatelské rozhraní spojené s virtuální síť, která váš pomocného mechanismu řízení běží. **Aplikace služby plány** a **aplikace** otevřít listy, které tyto položky, které jsou ve vaší pomocného mechanismu řízení.  

### <a name="monitoring"></a>Sledování

Grafy umožňuje zobrazit řadu metrik výkonu v jednotlivých fond zdrojů. Pro fondu front-end můžete sledovat průměr vytížení procesoru a paměti. Pro pracovní skupiny můžete sledovat množství, které se používá a množství, které je k dispozici.

Více aplikaci služby, kterou můžete udělat plány pomocí pracovníků v fond pracovník. Pracovní zátěž není distribuuje do provincie v stejným způsobem jako s front-end serverech tak využití procesoru a paměti neposkytnete hodně cestě užitečné informace. Je další důležité ke sledování kolik pracující s informacemi, které jste použili jsou k dispozici – zejména pokud spravujete systém ostatním uživatelům.  

Můžete také všechny metrik, která můžete sledovat v grafech nastavit upozornění. Nastavení upozornění tady funguje stejným způsobem jako jinde v aplikaci služby. Upozornění můžete nastavit z některého z části **upozornění** uživatelské rozhraní nebo procházení všech metriky uživatelského rozhraní a vyberete **Přidat upozornění**.

![Metriky uživatelského rozhraní][3]

Metriky jenom popsanými jsou metriky prostředí aplikace služeb. Existují také metriky, které jsou dostupné na úrovni plán služeb aplikací. Je to, kde sledování procesoru a paměti zajišťuje spoustu smysl.

Pomocný všech plánů aplikaci služby způsoby vyhrazené aplikaci služby plány. To znamená, že pouze aplikace spuštěné v hosts přidělit plán služeb aplikací se aplikace v této aplikaci služby plán. Zobrazit podrobnosti na plán služeb aplikací, vyvoláte plán služeb aplikací ze všech seznamů v uživatelském rozhraní pomocného mechanismu řízení nebo z **plánů vyhledejte aplikaci služby** (které je uvedené všem poznámkám).   

### <a name="settings"></a>Nastavení

V rámci zásuvné pomocného mechanismu řízení je **Nastavení** oddíl, který obsahuje několik důležitých funkcí:

**Nastavení** > **Vlastnosti**: zásuvné **Nastavení** automaticky otevírat vyvoláte zásuvné pomocného mechanismu řízení. Nahoře je **Vlastnosti**. Existuje celá řada položky zde, které jsou nadbytečné zobrazíte v **Essentials**, ale co je velmi užitečné je **Virtuální IP adresu**, jakož i **Odchozích IP adres**.

![Nastavení zásuvné a vlastností][4]

**Nastavení** > **IP adresy**: při vytváření aplikace pro IP Sockets Layer Secure (SSL) ve vaší pomocného mechanismu řízení, musíte IP SSL adresu. K získání jednu potřebuje vaše pomocného mechanismu řízení IP SSL adresy, které vlastní, které lze rozdělit. Po vytvoření pomocného mechanismu řízení nemá k tomuto účelu IP SSL adres, ale můžete přidat další. Existuje náklady pro další SSL IP adresy, jak je vidět v [Aplikaci služby ceny] [ AppServicePricing] (v části na připojení SSL). Další cena je cena IP SSL.

**Nastavení** > **Přední konce fondu** / **Pracovní skupiny**: všech tyto listy fondu zdrojů umožňuje zobrazit informace jenom na tento fondu zdrojů, kromě poskytnutí ovládací prvky plně zobrazit tento fond zdrojů.  

Základní zásuvné pro každou fondu zdrojů obsahuje graf s metriky pro tuto fondu zdrojů. Stejně jako s grafy z zásuvné pomocného mechanismu řízení můžete přejít do grafu a nastavte upozornění podle potřeby. Nastavení upozornění na zásuvné pomocného mechanismu řízení pro fond konkrétních zdrojů dělá totéž jako ale když ho uděláte z fondu zdrojů. V **Nastavení** zásuvné pracovníka fondu máte přístup k části všechny aplikace nebo aplikaci služby plány jednotného zasílání zpráv spuštěné v této oblasti pracovního.

![Nastavení fondu pracovníka uživatelského rozhraní][5]

### <a name="portal-scale-capabilities"></a>Funkce portálu měřítko  

Existují tři měřítko operace:

- Změna počtu IP adresy pomocného mechanismu řízení, které jsou k dispozici pro použití IP SSL.
- Změna velikosti výpočetním prostředek, který se používá v fond zdrojů.
- Změna počtu výpočetním prostředky, které se používají v fond zdrojů ručně nebo pomocí neobsahovaly text.

Na portálu jsou tři způsoby, jak určit, kolik servery, které máte v fondů zdrojů:

- Operace měřítko z hlavního zásuvné pomocného mechanismu řízení nahoře. Můžete provádět více měřítko změny konfigurace front-end a pracovní skupiny. Všechny jsou použity jako jediná operace.
- Ruční měřítko operace z **Měřítko** zásuvné fondu konkrétního zdroje, který je v části **Nastavení**.
- Neobsahovaly text, který jste určili z zásuvné **Měřítko** fondu konkrétního zdroje.

Pokud chcete použít operaci měřítko na zásuvné pomocného mechanismu řízení, přetáhněte posuvník na množství a uložte. Toto uživatelské rozhraní taky podporuje, změně velikosti.  

![Měřítko uživatelského rozhraní][6]

Použití funkcí ručního nebo Automatické měřítko v fond konkrétních zdroje, přejděte na **Nastavení** > **Přední konce fondu** / **Pracovní skupiny** podle potřeby. Otevřete fondu, který chcete změnit. Přejděte na **Nastavení** > **měřítko,** nebo **Nastavení** > **škálování**. Zásuvné **Měřítko,** umožňuje určit množství instance. **Měřítko nahoru** vám umožní určit velikost zdroje.  

![Nastavení uživatelského rozhraní][7]

## <a name="fault-tolerance-considerations"></a>Co byste měli zvážit odolnost proti chybám

Můžete nakonfigurovat prostředí aplikace služby používat až 55 celkové výpočetním zdroje. Tyto 55 pro využití prostředků pouze 50 mohou sloužit k hostitele úloh. Důvodem je dvojí. Je minimálně 2 front-end pro využití prostředků.  Které ponechá až 53 podporuje přiřazení pracovního fondu. Abyste mohli poskytnout odolnost proti chybám, musíte mít další pro využití prostředků, přidělený podle následujících pravidel:

- Každý pracovní fond musí minimálně 1 Další výpočetním prostředek, který není k dispozici možné úlohu.
- Když počtu výpočetním zdrojů do fondu pracovníka dostane nad určitou hodnotou, jinému zdroji výpočetním je vyžadováno odolnost proti chybám. To neplatí pro do fondu front-end.

V každém fondu jednoho pracovníka chybám požadavky se zobrazí, který pro zadanou hodnotu X zdroje přiřazené k fond pracovní:

- Pokud X je 2 až 20, je množství použitelné výpočetním prostředky, které můžete použít pro pracovního vytížení X-1.
- Pokud argument X není 21 až 40, je množství použitelné výpočetním prostředky, které můžete použít pro pracovního vytížení X-2.
- Pokud argument X není 41 až 53, je množství použitelné výpočetním prostředky, které můžete použít pro pracovního vytížení X-3.

Minimální stopu má 2 front-end serverech a 2 zaměstnanců.  Pomocí výše uvedených výpisů pak příkladů několik Upřesnit:  

- Máte 30 zaměstnanců jednu fondu potom 28 je z ní mohou sloužit k hostitele úloh.
- Pokud máte 2 zaměstnanci v jedné fondu potom 1 mohou sloužit k pracovního vytížení Host (hostitel).
- Pokud máte 20 zaměstnanci v jedné fondu potom 19 mohou sloužit k pracovního vytížení Host (hostitel).  
- Pokud máte 21 zaměstnanci v jedné fondu pak pořád pouze 19 mohou sloužit k pracovního vytížení Host (hostitel).  

Poměr odolnost proti chybám je důležité, ale musíte mít na paměti při změně velikosti nad určité mezní hodnoty. Pokud chcete přidat další kapacitu přecházet od 20 instance, potom přejděte k části 22 nebo vyšší protože 21 nelze přidat jakékoli další funkci. Platí totéž i přejdete nad 40, kde je číselnou přidá kapacita 42.  

## <a name="deleting-an-app-service-environment"></a>Odstranění prostředí aplikace služby ##

Pokud chcete odstranit prostředí aplikace služby, klikněte jednoduše použijte akci **Odstranění** v horní části zásuvné prostředí aplikace služeb. Až to uděláte, budete vyzváni k zadání názvu prostředí aplikace služby potvrďte, že Opravdu chcete provést. Všimněte si, že když odstraníte prostředí aplikace služby, odstraníte veškerý obsah v ní taky.  

![Odstranění prostředí aplikace služby UI][9]  

## <a name="getting-started"></a>Začínáme

Začínáme s prostředím služby aplikace najdete v článku [jak vytvářet prostředí aplikace služby](app-service-web-how-to-create-an-app-service-environment.md).

Další informace o aplikaci služby Azure platformy najdete v článku [Aplikace služby Azure](../app-service/app-service-value-prop-what-is.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
