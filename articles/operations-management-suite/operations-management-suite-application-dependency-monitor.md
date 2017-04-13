<properties
   pageTitle="Aplikace závislost monitoru (ADM) sady správy operace (OMS) | Microsoft Azure"
   description="Aplikace závislost monitoru (ADM) je operace správy sady (OMS) řešení, které automaticky zjistí součástí aplikace v systémech Windows a Linux a mapy komunikace mezi službami.  Tento článek obsahuje podrobnosti o nasazení ADM ve vašem prostředí a práce v různých scénářích."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="using-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Použití aplikace závislost Monitor řešení v sadě správy operace (OMS)
![Ikona upozornění správy](media/operations-management-suite-application-dependency-monitor/icon.png) Aplikace závislost monitoru (ADM) automaticky zjistí součástí aplikace v systémech Windows a Linux a mapy komunikace mezi službami. Umožňuje zobrazit serverech tak, jak by podle vás z nich – jako propojené systémy splnit kritické služby.  Sledování závislost aplikace zobrazí připojení mezi servery, procesy, a porty přes všechny připojení TCP architektura s žádnou konfiguraci vyžadované jedině instalace agent.

Tento článek popisuje podrobnosti o sledování závislost aplikace.  Informace o konfiguraci agentů ADM a rychlého připojení najdete v tématu [Konfigurace aplikace závislost Monitor řešení v sadě správy operace (OMS)](operations-management-suite-application-dependency-monitor-configure.md)

>[AZURE.NOTE]Sledování závislost aplikace momentálně v soukromé náhledu.  Můžete požádat o přístup k soukromé náhled ADM v [https://aka.ms/getadm](https://aka.ms/getadm).
>
>Během soukromé zobrazení náhledu, máte všechny účty OMS neomezený přístup k ADM.  Uzly ADM jsou zadarmo, ale protokolu analýzy dat pro typy AdmComputer_CL a AdmProcess_CL bude podle objemu dat jako jakékoli jiné řešení.
>
>Po ADM zadá veřejné náhled, bude k dispozici pouze zákazníkům bezplatných i placených přehled & analýzy v OMS ceny plánu.  Bezplatné osy účty budou omezeny na 5 ADM uzlů.  Pokud se účastníte soukromé náhled a nejsou zaregistrované v OMS ceny plánu po ADM vložení do veřejné náhled, přestane ADM v té době. 


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Případy použití: Zkontrolujte vaše IT zpracuje závislost vědět

### <a name="discovery"></a>Zjišťování
ADM automaticky vytvoří běžné odkaz mapu závislosti mezi servery, procesy a 3 stran služby.  Zjišťuje a mapy všechny závislosti TCP identifikační překvapení připojení vzdálené 3 stran systémy závisí na a závislostí na tradiční tmavě oblasti sítě například DNS a AD.  ADM zjišťuje selhalo síťových připojení, která spravovaných systémy se pokoušíte vytvořit, pomůže vám s identifikovat potenciální serveru stav služby výpadků a problémům se sítí.

### <a name="incident-management"></a>Vyřešení incidentu správy
ADM pomáhá zabránit nepřesných problém izolace tak, že vám ukážou, jak systémy připojeni a vliv na sobě.  Kromě toho neúspěšné připojení informace o připojení klientů pomůže identifikovat jsou chybně nakonfigurované moduly pro vyrovnávání zatížení, překvapivé nebo nadbytečné zatížení kritické služby a škodlivý klientů, jako je vývojář počítačích spolu mluvit výrobní systémy.  Integrované pracovní postupy s OMS změnit sledování lze také můžete zjistit, zda událost nastavit jako změnit na počítač back-end nebo služby vysvětluje příčina události.

### <a name="migration-assurance"></a>Migrace Assurance
ADM umožňuje efektivně plánování urychlit a ověřte Azure migrací zajistit, aby nic ještě zbývá a nejsou žádné překvapení výpadků.  Všechny vzájemně závislých systémy, které je potřeba migrovat dohromady, posuďte konfigurace systému a kapacity a určete, zda pracovního systému pořád slouží uživatele nebo dojít k vyřadit z místo migrace můžete zjistit.  Po dokončení přesunutí můžete zkontrolovat, že při načtení klienta a identity pro ověření, že se připojují zkušební systémy a Zákazníci.  Máte definice plánování a brány firewall vaší podsítě problémy, selhalo připojení v mapách ADM můžete přejděte systémů, které vyžadují připojení.

### <a name="business-continuity"></a>Nepřerušený provoz
Pokud používáte obnovení webu Azure a potřebujete nápovědu definování obnovení pořadí prostředí aplikace ADM lze automaticky zjistit, jak systémy spolehnout vzájemně zajistit, aby dává oprávnění váš plán obnovení spolehlivé.  Výběr kritické serveru a zobrazením svým klientům, poznáte front-end systémů, které by měla být vrácena pouze po kritické serveru obnovená a k dispozici.  Naopak pohledem back-end závislostí důležité serveru, poznáte těchto systémů, které musí být vrácena před obnovením systému fokus.

### <a name="patch-management"></a>Správa oprav
ADM rozšiřuje možnosti použití OMS systém aktualizace hodnocení, protože vám ukážou, které ostatní týmy a servery, závisí na službě tak, aby se může v před zpřístupněním systém pro opravy předem upozornění.  Správa oprav v OMS ADM taky zvyšuje tak, že vám ukážou, zda služby jsou dostupné a správně připojené po jejich oprava a restartovat. 


## <a name="mapping-overview"></a>Přehled mapování
ADM agentů shromáždění informací o procesech všechna připojení TCP na serveru, kde máte nainstalovaný, jakož i podrobnosti o příchozí a odchozí připojení pro každý proces.  Použití seznamu počítače na levé straně ADM řešení, mohou být vybrány počítačích s ADM agentů vizualizovat jejich závislosti vybraného časového rozsahu.  Závislost typu počítače mapování fokus na konkrétní počítač a zobrazit všechny stroje, které jsou servery počítače nebo přímé klienty TCP.

![Přehled ADM](media/operations-management-suite-application-dependency-monitor/adm-overview.png)

Počítačích lze rozbalit v mapě zobrazíte pracovního procesů pomocí aktivní síťové připojení během vybraného časového rozsahu.  Rozbalený vzdálený počítač s agentem ADM zobrazit podrobnosti o procesu jsou zobrazeny pouze procesy komunikaci s počítačem fokus.  Počet agentless front-end počítačů připojení do počítače fokus je označen na levé straně procesy, které se připojit k.  Počítač fokus upravuje připojení k počítači back-end bez agent, že back-end představuje s uzel v mapě zobrazující adresy IPv4 a uzel můžete rozbalit zobrazíte jednotlivé porty a protokoly služby, které počítač fokus komunikaci s.

Ve výchozím nastavení mapy ADM zobrazovat posledních 10 minut informace o závislostech.  Pomocí ovládacích prvků čas v levém horním rohu mapy jde zpracovávat pro historickými časy oblastí, až na celém hodinovou, chcete-li zobrazit, jak chcete závislosti prohledat v minulosti, například během incident nebo před došlo ke změně.    ADM data se ukládají 30 dní v pracovních prostorech placené a dobu 7 dní v bezplatná pracovních prostorech.

![Počítače mapování vlastností vybraného počítače](media/operations-management-suite-application-dependency-monitor/machine-map.png)

## <a name="failed-connections"></a>Selhalo připojení
Selhalo připojení jsou zobrazeny v mapách ADM procesy a počítačů, s přerušované červenou čárou zobrazující Pokud systému klienta nedaří dosáhnout procesu nebo port.  Selhalo připojení vznikly systému s agentem nasazeném ADM, pokud tento režim je pokusu o nezdařeném uložení připojení.  ADM opatření to tak, že pozorování sockets TCP, kterým se nepodaří připojit.  Může to být kvůli bránu firewall, stav na klienta nebo serveru nebo služby vzdáleného nejsou k dispozici. 

![Selhalo připojení](media/operations-management-suite-application-dependency-monitor/failed-connections.png)

Principy selhalo připojení můžete pomoci při řešení ověření migrace, analýza zabezpečení a celkové architektonické principy.  Někdy se nepodařilo připojení je neškodná, ale často přejděte přímo na problém, například prostředí převzetí neočekávaně stále není dostupný, najděte nebo dvou úrovní aplikace nebude možné mluvit po migraci cloudu.  Na obrázku výše IIS a WebSphere obě běží, ale nemůžete připojit. 

## <a name="computer-and-process-properties"></a>Počítač a vlastností obrázku
Při navigaci mapování ADM, můžete vybrat počítačích a procesy získat další kontext o jejich vlastnosti.  Počítačích obsahují informace o název DNS, adresy IPv4 pro, procesoru a paměti kapacitu, OM typ, verze operačního systému, restartujte poslední čas a ID OMS a ADM agentů.

Podrobnosti o procesu jsou shromážděné z metadat operační systém o tom, jak procesy, včetně proces název, popis obrázku, uživatelské jméno a doménu (ve Windows), název společnosti, název produktu, verze produktu, pracovní adresář, přepínač příkazového řádku a čas zahájení procesu.

![Vlastnosti procesu](media/operations-management-suite-application-dependency-monitor/process-properties.png)

Panel souhrn procesu poskytující další informace o připojení k daného procesu, včetně jeho vazbu ve vázaných porty, příchozí a odchozí připojení se neúspěšně připojení. 

![Přehled procesu](media/operations-management-suite-application-dependency-monitor/process-summary.png)

## <a name="oms-change-tracking-integration"></a>Sledování integrace změn OMS
ADM jeho integrace s sledování změn při automatické povolit a konfigurovat pracovního prostoru OMS obou řešení.

Panelu počítače souhrn označuje, zda sledování změn událostem došlo vybrané počítače během vybraného časového rozsahu.

![Souhrnné panely počítače](media/operations-management-suite-application-dependency-monitor/machine-summary.png)

V počítači změnit sledování panelu zobrazí se seznam všechny změny, přičemž poslední první spolu s odkaz přejít k podrobnostem protokolu vyhledat další podrobnosti.
![Panel sledování změn počítače](media/operations-management-suite-application-dependency-monitor/machine-change-tracking.png)

Následuje po výběru **Zobrazit v protokolu analýzy**podrobnostem zobrazení změna konfigurace události.
![Konfigurace změnit událost](media/operations-management-suite-application-dependency-monitor/configuration-change-event.png)


## <a name="log-analytics-records"></a>Technologie pro analýzu záznamů protokolu
ADM na počítač a zpracování zásob dat je k dispozici pro [vyhledávání](../log-analytics/log-analytics-log-searches.md) v protokolu analýzy.  Tím se dají použít ke scénářům včetně plánování migrace analýzy kapacity, zjišťování a řešení potíží s výkonem ad hoc. 

Jeden záznam generováno / hod pro každý jedinečný počítač a proces kromě záznamy generovaného při, obrázku nebo počítač spuštění nebo na-se nacházejí na ADM.  Tyto záznamy obsahují vlastností v následujících tabulkách. 

Existují interně vygenerovaných vlastnosti, které můžete použít k určení jedinečné procesy a počítače:

- PersistentKey_s jedinečně definovanými konfigurací obrázku, například příkazový řádek a uživatelské ID.  Jsou jedinečné pro daný počítač, ale můžete opakovat mezi počítači.
- ProcessId_s a ComputerId_s jsou v modelu ADM globálně jedinečný.



### <a name="admcomputercl-records"></a>AdmComputer_CL záznamů
Záznamy s typem **AdmComputer_CL** obsahují data zásob na servery s ADM agentů.  Tyto záznamy obsahují vlastností v následující tabulce.  

| Vlastnost | Popis |
|:--|:--|
| Typ | *AdmComputer_CL* |
| SourceSystem | *OpsManager* |
| ComputerName_s | Windows nebo Linux název počítače |
| CPUSpeed_d | Rychlosti procesoru v MHz |
| DnsNames_s | Seznam jmen všech DNS pro tento počítač |
| IPv4s_s | Seznam všech adres protokolu IPv4 nepoužívá na tomto počítači |
| IPv6s_s | Seznam všechny adresy IPv6 nepoužívá na tomto počítači.  (ADM identifikuje adresy IPv6 pro ale nenajde IPv6 závislosti.) |
| Is64Bit_b | True nebo false v závislosti na typu OS |
| MachineId_s | Vnitřní GUID, jedinečný přes pracovní prostor OMS  |
| OperatingSystemFamily_s | Windows nebo Linux |
| OperatingSystemVersion_s | Dlouhé řetězce verze OS |
| TimeGenerated | Datum a čas, který byl vytvořený záznam. |
| TotalCPUs_d | Počet procesoru vzorky |
| TotalPhysicalMemory_d | Kapacita paměti v Megabajtech |
| VirtualMachine_b | True nebo false na základě toho, zda OS Host OM |
| VirtualMachineID_g | ID OM Hyper-V |
| VirtualMachineName_g | Název OM Hyper-V |
| VirtualMachineType_s | Technologií HyperV Vmware, Xen, Kvm, Ldom, Lpar, Virtualpc |


### <a name="admprocesscl-type-records"></a>Typ AdmProcess_CL záznamů 
Záznamy s typem **AdmProcess_CL** obsahují data zásob pro připojení TCP procesy na serverech s ADM agentů.  Tyto záznamy obsahují vlastností v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| Typ | *AdmProcess_CL* |
| SourceSystem | *OpsManager* |
| CommandLine_s | Úplný příkazový řádek procesu |
| CompanyName_s | Název společnosti (z Windows PE nebo Linux ot) |
| Description_s | Popis dlouhý proces (z Windows PE nebo Linux ot) |
| FileVersion_s | Spustitelný soubor verze (ze Windows PE pouze Windows) |
| FirstPid_d | ID procesu OS |
| InternalName_s | Název interního spustitelný soubor (z Windows PE pouze Windows) |
| MachineId_s | Vnitřní GUID jedinečné přes pracovní prostor OMS  |
| Name_s | Spustitelný název procesu |
| Path_s | Soubor systému cestu spustitelný soubor obrázku |
| PersistentKey_s | Vnitřní GUID jedinečný v rámci tohoto počítače |
| PoolId_d | Vnitřní ID pro agregaci procesy na základě podobné příkaz řádků. |
| ProcessId_s | Vnitřní GUID jedinečné přes pracovní prostor OMS  |
| ProductName_s | Řetězec název produktu (z Windows PE nebo Linux ot) |
| ProductVersion_s | Řetězec verze produktu (z Windows PE nebo Linux ot) |
| StartTime_t | Čas zahájení procesu na místním počítači hodin |
| TimeGenerated | Datum a čas, který byl vytvořený záznam. |
| UserDomain_s | Domény vlastník procesu (pouze Windows) |
| UserName_s | Jméno vlastníka obrázku (pouze Windows) |
| WorkingDirectory_s | Proces pracovní adresář |


## <a name="sample-log-searches"></a>Ukázka protokolu hledání

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Seznam kapacita fyzické paměti všechny spravované počítače. 
Typ = AdmComputer_CL | Vyberte TotalPhysicalMemory_d ComputerName_s | Dedup ComputerName_s

![Příklad dotazu ADM](media/operations-management-suite-application-dependency-monitor/adm-example-01.png)

### <a name="list-computer-name-dns-ip-and-os-version"></a>Seznam název počítače, DNS, IP a operační systém verze.
Typ = AdmComputer_CL | Vyberte ComputerName_s OperatingSystemVersion_s, DnsNames_s, IPv4s_s | dedup ComputerName_s

![Příklad dotazu ADM](media/operations-management-suite-application-dependency-monitor/adm-example-02.png)

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Najít všechny procesů pomocí "sql" do příkazového řádku
Typ = AdmProcess_CL CommandLine_s = \*sql\* | dedup ProcessId_s

![Příklad dotazu ADM](media/operations-management-suite-application-dependency-monitor/adm-example-03.png)

### <a name="after-viewing-event-data-for-given-process-use-its-machine-id-to-retrieve-the-computers-name"></a>Po zobrazení dat událostí pro dané procesu, použijte jeho ID počítače k načtení název počítače
Typ = AdmComputer_CL "m!m-9bb187fa-e522-5f73-66d2-211164dc4e2b" | Jednoznačné ComputerName_s

![Příklad dotazu ADM](media/operations-management-suite-application-dependency-monitor/adm-example-04.png)

### <a name="list-all-computers-running-sql"></a>Seznam všech počítačích se systémem SQL
Typ = AdmComputer_CL MachineId_s v {typ = AdmProcess_CL \*sql\* | Jednoznačné MachineId_s} | Jednoznačné ComputerName_s

![Příklad dotazu ADM](media/operations-management-suite-application-dependency-monitor/adm-example-05.png)

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>Seznam všech jedinečné produktu verzích otočení v mém datacentru
Typ = AdmProcess_CL Name_s = otočení | Jednoznačné ProductVersion_s

![Příklad dotazu ADM](media/operations-management-suite-application-dependency-monitor/adm-example-06.png)

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Umožňuje vytvořit skupinu počítače všechny počítače se systémem CentOS

![Příklad dotazu ADM](media/operations-management-suite-application-dependency-monitor/adm-example-07.png)



## <a name="diagnostic-and-usage-data"></a>Diagnostika a použití dat
Microsoft se automaticky shromažďují použití a výkonu dat prostřednictvím používání služby aplikace závislost Monitor. Společnost Microsoft použije tyto údaje poskytovat a zlepšovat kvalitu, zabezpečení a integrity služby aplikace závislost Monitor. Dat obsahuje informace o konfiguraci software jako verze operačního systému a a také kontroly IP adresu, DNS název a název Workstation za účelem zajištění přesné a efektivně Poradce při potížích možnosti. Nebudeme zjišťovat jména, adresy nebo jiné kontaktní informace.

Další informace o shromažďování dat a použití najdete v článku [Microsoft Online Services zásady ochrany osobních údajů](hhttps://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Další kroky
- Další informace o [přihlášení hledání](../log-analytics/log-analytics-log-searches.md] in Log Analytics to retrieve data collected by Application Dependency Monitor.)
