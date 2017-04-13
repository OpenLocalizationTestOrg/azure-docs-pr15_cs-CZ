<properties
    pageTitle="Sítě řešení sledování výkonu ve OMS | Microsoft Azure"
    description="Sledování výkonu sítě pomáhá sledovat výkon vaší sítě se změnami v blízkosti real-úvazek k zjišťování a vyhledejte síť kritické."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="banders"/>

# <a name="network-performance-monitor-preview-solution-in-oms"></a>Sítě řešení výkon (verze Preview) ve OMS

>[AZURE.NOTE] Jedná se o [Náhled řešení](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Tento dokument popisuje, jak nastavení a použití sledování výkonu sítě řešení ve OMS, která pomáhá sledovat výkon vaší sítě se změnami v blízkosti real-úvazek-li zjistit vyhledejte síťového kritické. Řešení sledování výkonu sítě můžete sledovat ztráty a latenci mezi dvěma sítích, podsítí nebo servery. Sledování výkonu sítě jsou zjištěny problémy se sítí například přenosy blackholing směrování chyb a problémů, které metody sledování běžných sítě nemůžou zjišťování. Sledování výkonu sítě vygeneruje upozornění a jak a kdy prahovou hodnotu porušena odkazu na síť s upozorněním. Tyto prahové hodnoty můžete naučili automaticky systému nebo můžete nakonfigurovat je, aby použili vlastní výstrahy pravidla. Sledování výkonu sítě zajišťuje včasná zjišťování problémy s výkonem sítě a lokalizováno příčinu problému na určitém segmentu sítě nebo zařízení.

Můžete zjistit problémům se sítí s řídicím panelu řešení, které zobrazuje souhrnné informace o vaší síti, včetně poslední události stav sítě, chybná sítě odkazy a podsítě odkazy, které jsou protilehlé ztráty paketu vysoký a latence. Je můžete přechodu na odkaz sítě k zobrazení aktuálního stavu podsítě odkazy a také mezi uzly odkazy. Můžete taky zobrazit historické trend ztráty a latence na síti, podsítě a mezi uzly úroveň. Můžete zjistit problémům se sítí přechodná zobrazením grafy historických trendů ztráty paketu a latence a vyhledejte problémových míst na mapě topologie v síti. Graf interaktivní topologie umožňuje vizualizovat směruje směrování směrování sítě a zjistit příčinu problému. Stejně jako jakékoli jiné řešení můžete protokolu hledání pro různé požadavky analýzy k vytvoření vlastní sestavy založené na data shromážděná sledování výkonu sítě.

Řešení používá syntetické transakce jako primární mechanismus zjišťování chyb sítě. Ano můžete ho bez ohledu na dodavatele nebo modelu určitých síťových zařízení. Funguje napříč místní cloud (IaaS) a hybridní prostředí. Řešení automaticky zjistí topologie sítě a různé postupy v místní síti.

Typické síťové sledování produkty zaměřit na sledování stavu sítě zařízení (směrovači, přepínače atd.), ale nenabízejí podstatu skutečné kvalitu připojení k síti mezi dvěma body, které nemá sledování výkonu sítě.

### <a name="using-the-solution-standalone"></a>Použití samostatného řešení

Pokud chcete sledovat kvalitu síťových připojení mezi jejich kritické pracovního vytížení, sítí, datacentrech nebo weby office a pak je můžete použít řešení sledování výkonu sítě samostatně sledování stavu připojení mezi:

- víc webů datacentrech nebo office, která jsou připojená pomocí veřejné nebo soukromé sítě
- kritické úloh, ve kterých se nepoužívá obchodních aplikací
- veřejné cloudové služby, jako je Microsoft Azure nebo Amazon webové služby AWS a místní sítích, pokud máte IaaS (OM) k dispozici a máte bran Konfigurace povoluje komunikace mezi místním sítí a sítěmi cloudu
- Azure a místních sítí při použití směrování Express

### <a name="using-the-solution-with-other-networking-tools"></a>Použití řešení pomocí dalších nástrojů pro sociální sítě

Pokud chcete sledovat řádek podnikové aplikaci, můžete jako companion řešení jiných síťové nástroje řešení sledování výkonu sítě. V pomalé síti může vést k pomalé aplikací a sledování výkonu sítě můžete prozkoumat výkonu problémy s aplikací příčinou podkladového problémům se sítí. Protože řešení nevyžaduje přístup k síťových zařízení, správce aplikace nevyžaduje spolehnout se na sociální sítě týmu a poskytuje informace o jak sítě má vliv na aplikace.

Také pokud už investovat do jiné síti nástroje pro sledování potom řešení můžete doplnit nástrojů, protože většina tradiční sítě sledování řešení nenabízejí přehledy do začátku do konce sítě měřítka jako ztráty a latence.  Řešení sledování výkonu sítě pomůže této mezeru.


## <a name="installing-and-configuring-agents-for-the-solution"></a>Instalace a konfigurace agentů řešení

Použijte základní procesy nainstalovat agentů na [počítačích Windows připojit k protokolu analýzy](log-analytics-windows-agents.md) a [Připojit Operations Manager do protokolu analýzy](log-analytics-om-agents.md).

>[AZURE.NOTE]
Budete muset nainstalovat alespoň na úrovni 2 agentů Pokud chcete mít dost data zjišťovat a sledovat síťové prostředky. V opačném řešení zůstane v konfiguraci stavu, dokud instalace a konfigurace další agentů.

### <a name="where-to-install-the-agents"></a>Kam se instaluje agentů

Před instalací agentů zvažte topologie sítě a které části sítě, který chcete sledovat. Doporučujeme nainstalovat víc agent pro každý podsítě, který chcete sledovat. Jinými slovy pro každé podsítě, který chcete sledovat, vyberte dvě nebo více servery nebo VMs a nainstalujte agent na nich.

Pokud si nejste jisti topologie sítě, nainstalujte agentů na serverech s kritické úloh, ve které chcete sledovat výkon sítě. Můžete třeba Udržujte si přehled o síťového připojení mezi na webový server a serveru SQL Server. V tomto příkladu by nainstalujete agent na serverech.

Agentů sledovat připojení k síti (propojení) mezi tabulkami hosts – ne hosts sami. Abyste sledování odkaz sítě, musíte nainstalovat agentů na oba koncové body tuto vazbu.

### <a name="configure-agents"></a>Konfigurace agentů

Po instalaci agentů, musíte otevřít portů brány firewall pro tyto počítače zajistit, že agentů komunikovat. Budete muset stáhnout a pak spusťte [skript Powershellu EnableRules.ps1](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) bez parametrů v okně prostředí PowerShell s oprávněními správce

Skript vytvoří klíče registru vyžadované sledování výkonu sítě a vytvoří pravidla brány firewall systému Windows umožňující agentů vytvořit připojení TCP vzájemně. Klíče registru vytvořené skriptem také určit, zda k přihlášení protokoly ladění a cestu k souboru protokoly. Definuje také port TCP agent používá pro komunikaci. Hodnoty pro klávesy nastaveny skript, automaticky, takže byste neměli měnit ručně klávesy.

Port otevřít ve výchozím nastavení je 8084. Můžete použít vlastní port zadáním parametr `portNumber` skriptu. Však stejný port bude použito u všech počítačů, kde je spuštěn skript.

>[AZURE.NOTE] Skript EnableRules.ps1 nakonfiguruje jenom na počítači, kde je spuštěn skript pravidla brány firewall systému Windows. Pokud používáte bránu firewall sítě, nezapomeňte, že je možné přenosy určené pro port TCP použitý sledování výkonu sítě.


## <a name="configuring-the-solution"></a>Konfigurace řešení

Instalace a konfigurace řešení použijte následující informace.

1. Sledování výkonu sítě řešení získává data z počítače s Windows Server 2008 SP 1 nebo novějším nebo Windows 7 s aktualizací SP1 nebo novější, které jsou požadavky na stejné jako aplikaci Microsoft sledování Agent (MMA).
2. Přidání řešení sledování výkonu sítě do pracovního prostoru OMS pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).  
  ![Symbol sledování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. Na portálu OMS zobrazí se nová dlaždice s názvem **Sledování výkonu sítě** se zprávou *řešení vyžaduje další konfiguraci*. Musíte nakonfigurovat řešení pro přidání sítě nejzajímavější na základě subnetworks a uzly nalezené agentů. Klikněte na tlačítko **Sledování výkonu sítě** zahájíte konfigurace sítě výchozí.  
  ![řešení vyžaduje další konfiguraci](./media/log-analytics-network-performance-monitor/npm-config.png)


### <a name="configure-the-solution-with-a-default-network"></a>Konfigurace řešení s výchozí sítě

Na stránce konfigurace uvidíte jedné sítě s názvem **výchozí**. Když nezadali jste žádné sítě, všechny automaticky zjištěnou podsítě jsou umístěny ve výchozím nastavení sítě.

Pokaždé, když vytvoříte v síti, nebude k němu přidat podsítě a že podsítě se odebere z výchozí síť. Pokud byste odstranili v síti, vrátíte se automaticky jeho podsítí výchozí síť.

Jinými slovy výchozí sítě je kontejner pro všechny podsítě, které nejsou součástí jakákoli síť definované uživatelem. Nelze upravit nebo odstranit výchozí síť. Vždy zůstává v systému. Však můžete vytvořit tolik sítích podle potřeby.

Ve většině případů podsítí ve vaší organizaci bude uspořádaných do více než jedné sítě a mají vytvořit jednu nebo víc sítí pro logické seskupení vaší podsítě.

### <a name="create-new-networks"></a>Vytvoření nové sítě

Síť v nástroji Sledování výkonu sítě je kontejner pro podsítí. Můžete vytvořit v síti s názvem a přidání podsítí k síti. Například můžete vytvořit v síti s názvem *Building1* a pak přidejte podsítí, nebo můžete vytvořit v síti s názvem *DMZ* a pak přidejte všechny podsítě patřící do zóny demilitarizovaná do této sítě.

#### <a name="to-create-a-new-network"></a>Chcete-li vytvořit novou síť

1. Klikněte na **Přidat síť** a poté zadejte síťový název a popis.
2.  Vyberte jeden nebo více podsítí a potom klikněte na **Přidat**.
3. Klepněte na tlačítko **Uložit** uložte konfiguraci.  
  ![Přidání sítě](./media/log-analytics-network-performance-monitor/npm-add-network.png)



### <a name="wait-for-data-aggregation"></a>Počkejte slučování dat

Po uložení konfigurace poprvé spustí řešení shromažďování informací o ztráty a latence paketu sítě mezi uzly nainstalovanou agentů. Tento proces může nějakou dobu trvat, někdy 30 minut. Sledování výkonu sítě dlaždice na stránce Přehled v průběhu tento stav zobrazí zpráva oznamující *agregace dat v procesu*.

![agregace dat v průběhu](./media/log-analytics-network-performance-monitor/npm-aggregation.png)


Jestliže byl odeslán data, zobrazí se sledování výkonu sítě dlaždice aktualizace obsahující data.

![Dlaždice sledování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-tile.png)

Klikněte na dlaždici zobrazíte řídicím panelu sledování výkonu sítě.

![Řídicí panel sledování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Úprava nastavení sledování pro podsítě

Na kartě **Subnetworks** na stránce konfigurace jsou uvedené všechny podsítě nainstalovanou alespoň jeden agent.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Zapnutí nebo vypnutí sledování pro konkrétní subnetworks

1. Zaškrtněte nebo zrušte zaškrtnutí políčka vedle **podsítě ID** a potom zajistěte, aby byl **slouží k sledování** vybrané nebo zrušení zaškrtnutí v případě potřeby. Můžete zaškrtněte nebo zrušte více podsítí. Když zakáže, subnetworks nejsou sledovány jako agentů bude aktualizován ukončíte ping jiné.
2. Zvolte uzly, které chcete sledovat pro konkrétní podsítě výběrem podsítě ze seznamu a přesunutí požadované uzly mezi seznamy obsahující sledován a kontrolovaný uzlů.
Můžete přiřadit vlastní **Popis** podsítě, pokud chcete.
3. Klepněte na tlačítko **Uložit** uložte konfiguraci.  
  ![Úprava podsítě](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Zvolte uzly ke sledování

Všech uzlů, které mají nainstalovanou na nich agent, najdete na kartě **uzlů** .

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Zapnutí nebo vypnutí sledování uzlů

1. Zaškrtněte nebo zrušte uzly, které chcete sledovat nebo ukončení sledování.
2. Klikněte na **použít pro sledování**nebo zrušte jeho zaškrtnutí, podle potřeby.
3. Klikněte na **Uložit**.  
  ![Povolit sledování uzel](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)


### <a name="set-monitoring-rules"></a>Nastavení pravidla sledování

Sledování výkonu sítě vygeneruje události stavu o propojení mezi dvojice uzly nebo podsítě nebo k síti propojení při zobrazení porušena prahovou hodnotu. Tyto limity můžete naučili automaticky systém nebo pravidla můžete konfigurovat je vlastní upozornění.

*Výchozí pravidlo* se vytvořil systému a vytvoří stavu událost nastavit jako kdykoli ztrátě nebo latenci mezi všechny dvojice sítě nebo podsítě odkazy na porušení mezní naučili systému. Můžete zakázat výchozí pravidlo a vytvořte vlastní sledování pravidla

#### <a name="to-create-custom-monitoring-rules"></a>Vytvoření vlastního pravidla sledování

1. Klikněte na položku **Přidat pravidlo** na kartě **Monitor** a zadejte název pravidla a popis.
2. Vyberte pár sítě nebo podsítě odkazy na sledovat v seznamech.
3. Nejdřív vyberte síť, ve kterém je první podsítě/s potřebné obsažené v rozevíracím seznamu sítě a vyberte v rozevíracím seznamu odpovídající podsítě podsítě/s.
Vyberte **všechny subnetworks** , pokud chcete sledovat všechny subnetworks odkaz sítě. Podobně vyberte jiné podsítě/s zájmu. A klikněte na tlačítko **Přidat výjimku** vyloučit sledování pro konkrétní podsítě odkazy z výběru, které jste udělali.
4. Pokud nechcete vytvářet události stavu u položek, které jste vybrali, zrušte **Povolení sledování na odkazy vztahuje konsolidovaná toto pravidlo stavu**.
5. Klikněte na sledování podmínek.
Můžete nastavit vlastní mezní hodnoty pro generování události stavu zadáním prahové hodnoty. Pokaždé, když hodnota podmínka dostane nad vybranou prahové hodnoty pro vybrané sítě/podsítě dvojici, je generováno událost nastavit jako stavu.
6. Klepněte na tlačítko **Uložit** uložte konfiguraci.  
  ![Vytvoření vlastního pravidla sledování](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)


## <a name="data-collection-details"></a>Podrobnosti shromažďování dat

Sledování výkonu sítě používá TCP SYN-SYNACK-ACK handshake paketů shromažďovat ztráty a latence informace a traceroute slouží také k získat informace o topologii.

V následující tabulce jsou uvedeny metody shromažďování dat a další podrobnosti o tom, jak se údaje pro sledování výkonu sítě.

| platformy | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
| Windows |![Ano](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Ano](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Ne](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|            ![Ne](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|![Ne](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)| TCP Handshake každých 5 sekund dat odeslané každé 3 minuty |

Řešení využívá syntetické transakce k posouzení stav sítě. OMS agentů nainstalovaný v různých bodu v síti exchange TCP pakety k sobě navzájem a v procesu, zjistěte doby odezvy ztrátě času a paketu případné. Každý agent pravidelně, provede trasování cesty k jiné agentů najítnajděte všechny různých cest v síti, ve které se musí zkoušet. Pomocí těchto dat, účastníci můžou odvodit latence sítě a ztrát paketů. Testů opakují každých pět sekund a dat je agregované dobu 3 minuty zástupci před nahráním na OMS.

>[AZURE.NOTE] I když agentů vzájemně komunikovat často, není vytvářejí spoustu v síti při provádění testů. Agentů jsou založeny pouze na paketů handshake TCP SYN-SYNACK-ACK a zjistit ztráty a latence – žádná data výměny paketů. Během tohoto procesu agentů vzájemně komunikovat pouze v případě potřeby a topologie komunikace agent je optimalizováno pro zatížení sítě.

## <a name="using-the-solution"></a>Použití řešení

Tato část popisuje na řídicím panelu funkcí a jejich použití.

### <a name="solution-overview-tile"></a>Dlaždice přehled řešení

Po povolení řešení sledování výkonu sítě dlaždici řešení na stránce Přehled OMS obsahuje rychlý přehled toho, stav sítě. Zobrazí prstencový graf znázorňující počet správný a chybná podsítě odkazy. Po kliknutí na dlaždici se otevře řídicím panelu řešení.

![Dlaždice sledování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-tile.png)


### <a name="network-performance-monitor-solution-dashboard"></a>Řídicí panel řešení sledování výkonu sítě

**Síť souhrnné** zásuvné zobrazuje souhrn sítí spolu s jeho relativní velikost. Následuje dlaždice zobrazující celkový počet odkazů sítě, podsítě odkazy a cesty v systému (cesty se skládá z adresy IP dvěma tabulkami hosts s agentů a všechny směrování mezi nimi).

**Události stav sítě horní** zásuvné poskytuje seznam posledních události stavu a upozornění v systému a času, protože událost byl aktivní. Zpráva stav událost nebo upozornění kdykoli ztráty paketu nebo latence propojení sítě nebo podsítě překračuje prahovou hodnotu.

Zásuvné **Horní chybná síťových propojení** se seznamem propojení chybná sítě. Toto jsou sítě odkazy, které mají jednu nebo více událost nežádoucí stavu je v okamžiku.

**Horní podsítě odkazy se nejčastěji ztrátou** a **Podsítě odkazy s největší latence** listy zobrazit nejvyšší podsítě odkazy tak, že ztráty paketu a horním podsítě odkazy tak, že latence. Vysokou latencí nebo některé počtu ztráty paketu můžou očekávat, že na některé síťové odkazy. Tyto odkazy se zobrazí v seznamu horních deseti, ale nejsou označeny jako chybná.

**Běžné dotazy** zásuvné obsahuje sadu vyhledávací dotazy, které načíst nezpracovanými sítě monitorování dat přímo. Tyto dotazy slouží jako výchozí bod pro vytváření vlastních dotazů pro vlastní sestavy.

![Řídicí panel sledování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-dash01.png)


### <a name="drill-down-for-depth"></a>Podrobnostem pro název hloubkové

Klikněte na různé odkazy na řešení řídicím panelu k podrobnostem hlouběji do libovolné oblast zájmu. Například se zobrazí upozornění nebo chybná síťové připojení se zobrazí na řídicím panelu, můžete kliknutím ho chcete prozkoumat další. Můžete se dostali na stránku, která jsou uvedeny všechny odkazy podsítě pro konkrétní síťové připojení. Budete moct zobrazit ztráty, zpoždění a stavu stav všech odkazů podsítě a rychle zjistěte, jaké odkazy podsítě způsobují problém. Klikněte na **Zobrazit uzel odkazy** dostali k odkazům všechny uzel podsítě nefunkčního odkazu. Pak můžete zobrazit jednotlivé odkazy mezi uzly a odkazy chybná uzel.

Klikněte na **zobrazení topologie** zobrazíte směrování směrování topologie trasy mezi zdrojovým a cílovým uzlů. Nefunkční trasy nebo směrování jsou zobrazeny v červené barvě, aby mohli rychle identifikovat problém na konkrétní část sítě.

![Podrobná data](./media/log-analytics-network-performance-monitor/npm-drill.png)


#### <a name="trend-charts"></a>Trend grafy

Na každé úrovni, který můžete podrobnostem, zobrazí se trend ztráty a latence odkazu na síť. Grafy trendů lze také podsítě a uzel odkazy. Časový interval pro grafu vykreslit pomocí ovládacího prvku čas v horní části grafu můžete změnit.

Trend grafy zobrazují historických perspektivy výkonu sítě odkaz. Některé problémům se sítí jsou přechodná v přírodě a by být obtížné zachytit pouze pohledem aktuální stav sítě. Je to proto problémy můžete rychle povrchové a zmizí před každý oznámení, pouze znovu později v čase. Tyto přechodných problémů může být obtížné pro správce aplikace taky protože můžou být problémy často povrch jako nevysvětlitelné zvýšení doba odezvy aplikace, i když spuštění hladce se zobrazí všechny součásti aplikace.

Prohlížíte graf trendu umístění problém jako náhlé zásobníku v síti latence nebo ztráty paketu můžete snadno zjistit tyto typy problémy.

![Graf trendu](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Topologie směrování směrování mapy

Sledování výkonu sítě zobrazuje topologii směrování směrování tras mezi dvěma uzly na mapování interaktivní topologie. Topologie mapu můžete zobrazit výběrem uzel propojení a potom kliknutím na **zobrazení topologie**. Také můžete zobrazit mapy topologie po kliknutí na dlaždici **cesty** na řídicím panelu. Když kliknete na řídicím panelu **cesty** , budete muset vybrat zdrojové a cílové uzlů v levém panelu a potom klikněte na **zobrazované** vykreslování hodnot v průběhu směruje mezi dvěma uzly.

Mapa topologie zobrazí, kolik trasy jsou mezi dvěma uzly a co cesty datové pakety trvat. Kritické sítě jsou označené červeně na mapě topologie. Vyhledejte vadná síťové připojení nebo chyba síťové zařízení pohledem na červené barevné prvky na mapě topologie.

Po kliknutí na uzel nebo přechodovou nad ním na mapě topologie, zobrazí se vlastnosti uzel jako plně kvalifikovaný název domény a IP adres. Klikněte na směrování zobrazíte, že je IP adresa. Vymazání a výběrem pouze cesty, které chcete zvýraznit na mapě můžete zvýraznit konkrétní trasy. Pomocí kolečkem myši můžete přiblížit nebo oddálit topologie mapa.

Všimněte si, že topologie zobrazené v mapě je topologie layer 3 a neobsahuje vrstvy 2 zařízení a připojení.

![Topologie směrování směrování mapy](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Lokalizace poruch

Sledování výkonu sítě je moct najít kritické sítě body bez propojovacích síťových zařízení. Podle data, která shromažďuje ze sítě a použitím rozšířené algoritmů v grafu sítě, sledování výkonu sítě díky kterému pravděpodobnosti odhad částí sítě, které se s největší pravděpodobností příčinu problému.

Tento přístup je užitečné při přístupu k směrování není dostupný, protože ho nevyžaduje libovolných dat shromážděných z síťových zařízení, třeba směrovači nebo přepínače zjistit počet problémových míst v síti. To je také užitečné při směrování mezi dvěma uzly nejsou ve vaší správy. Směrování může být například směrovači poskytovatel internetových služeb.

### <a name="log-analytics-search"></a>Hledání analýzy protokolu

Všechna data, která je vystaveného graficky pomocí řídicího panelu sledování výkonu sítě a přechodu na podrobnosti stránky je dostupné taky nativně v protokolu analýzy vyhledávání. Můžete vyhledat dat pomocí jazyka vyhledávacího dotazu a vytvářet vlastní sestavy pomocí exportu dat do Excelu nebo PowerBI. Zásuvné **Běžné dotazy** na řídicím panelu má některá užitečná dotazy, které můžete použít jako výchozí bod pro vytváření vlastních dotazů a sestav.

![vyhledávací dotazy](./media/log-analytics-network-performance-monitor/npm-queries.png)


## <a name="investigate-the-root-cause-of-a-health-alert"></a>Prozkoumejte příčina stavu upozornění

Teď jste přečtěte si o sledování výkonu sítě, Podívejme se na jednoduché vyšetřování příčin pro událost nastavit jako stavu.

1. Na stránce Přehled dostanete snímek rychlý stav sítě tak, že pozorování **Sledování výkonu sítě** dlaždice. Všimněte si, že mimo 80 subnetworks odkazy sledován, 43 jsou chybná. To zaručuje vyšetřování. Klikněte na dlaždici zobrazíte řídicím panelu řešení.  
  ![Dlaždice sledování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-investigation01.png)

2. V následujícím příkladu obrázku zjistíte, že jsou aktuálně 4 události stavu a 4 sítě odkazy, které jsou chybná. Rozhodnete-li prozkoumat problém a klikněte na odkaz **Web služby Sharepoint** sítě zjistěte kořenovém problém.  
  ![Příklad odkazu chybná sítě](./media/log-analytics-network-performance-monitor/npm-investigation02.png)

3. Stránka podrobnostem zobrazuje všechny odkazy podsítě v **Sharepointovém webu** síťové připojení. Zjistíte, že pro obě podsítě odkazy, má latence překročených mezní díky nefunkční odkaz sítě. Najdete v článku trendů latence podsítě odkazů. Výběr času můžete použít ovládací prvek v grafu zaměření na požadované časového rozsahu. Zobrazí čas, kdy když latence dosáhne jeho ve špičce. Dál je možné hledat protokoly pro tohoto časového období a vyšetřilo tento problém. Klikněte na **Zobrazit uzel odkazy** k podrobnostem dál.  
  ![Příklad chybná podsítě odkazy](./media/log-analytics-network-performance-monitor/npm-investigation03.png)

4.  Podobně jako na předchozí stránku, na stránce podrobnostem pro konkrétní podsítě odkaz jsou uvedeny dolů základních uzel odkazy. Můžete provádět akce podobně jako v tomto poli stejně jako v předchozím kroku. Klikněte na **zobrazení topologie** zobrazíte topologii mezi uzly 2.  
  ![Příklad odkazy chybná uzel](./media/log-analytics-network-performance-monitor/npm-investigation04.png)

5. Všechny cesty mezi 2 vybrané uzly jsou zobrazeny v mapě topologie. Můžete vizualizace topologii směrování směrování tras mezi dvěma uzly na mapě topologie. Umožňuje přehledné informace o kolik trasy mezi dvěma uzly a co jsou cesty datové pakety prováděnou. Kritické sítě jsou označené v červené barvě. Vyhledejte vadná síťové připojení nebo chyba síťové zařízení pohledem na červené barevné prvky na mapě topologie.  
  ![Příklad zobrazení chybná topologie](./media/log-analytics-network-performance-monitor/npm-investigation05.png)

6. Ztráta latence a počet směrování v každé cesty můžete si prohlédnout v podokně **Podrobností cestu** . V tomto příkladu uvidíte, jak je uvedeno v podokně existují 3 chybná cesty. Použijte posuvník chcete zobrazit podrobnosti o těchto chybná cest.  Vyberte jednu z cesty tak, aby vykreslí topologie pro jedinou cestu pomocí zaškrtávacích políček. Přiblížení a oddálení topologie mapu můžete kolečkem myši.

  V pod obrázkem jasně uvidíte příčin oblasti problém na konkrétní část sítě zobrazením cest a směrování v červené barvě. Po kliknutí na uzel v mapě topologie zobrazíte vlastnosti uzel, včetně plně kvalifikovaný název domény a IP adresu. Po kliknutí na směrování zobrazuje IP adresu směrování.  
  ![nefunkční topologie – podrobnosti příklad cesty](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="next-steps"></a>Další kroky

- [Hledání protokoly](log-analytics-log-searches.md) k zobrazení Podrobný síťový výkon dat záznamů.
