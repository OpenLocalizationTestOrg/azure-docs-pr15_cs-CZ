<properties
    pageTitle="Přihlášení technologie pro analýzu dat zabezpečení | Microsoft Azure"
    description="Zjistěte, jak technologie pro analýzu protokolu chrání osobní údaje a zabezpečuje vaše data."
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
    ms.date="09/23/2016"
    ms.author="banders"/>

# <a name="log-analytics-data-security"></a>Přihlaste se zabezpečení technologie pro analýzu dat

Microsoft velmi dbá na ochranu osobních údajů a zabezpečení dat, předvádění softwaru a služeb, které vám pomůžou se správou IT infrastrukturu vaší organizace. Jsme rozpoznat, když pověřit datům ostatním této zabezpečení vyžaduje náročné zabezpečení. Microsoft vyhovuje pro striktně zabezpečení a dodržování – z kódování provozní služby.

Zabezpečení a ochrana dat je nejvyšší prioritou společnosti Microsoft. Kontaktujte nás s dotazy, návrhy nebo potíže s některou z následujících informace, včetně naše zásady zabezpečení v [možnostech podpory Azure](http://azure.microsoft.com/support/options/).

Tento článek vysvětluje, jak Shromažďované, zpracovávané a zabezpečené technologie pro analýzu protokolu v sadě operace Management (OMS) data. Můžete agentů pro připojení k webové službě, umožňuje shromáždit funkční data System Center Operations Manager nebo načtení dat z Azure diagnostických nástrojů pro použití protokolu analýzy. Sběr dat jsou odeslány přes Internet portálu ověřování na základě certifikát SSL 3 do služby protokolu analýzy, která hostuje Microsoft Azure Před odesláním agentem komprimovaná data.

Služba protokolu Analytics má na starosti datům cloudové bezpečně pomocí následujících metod:

- oddělení dat
- uchovávání informací
- pole fyzicky zabezpečení
- vyřešení incidentu správy
- dodržování předpisů
- certifikace standardy zabezpečení


## <a name="data-segregation"></a>Oddělení dat

Data o zákaznících bude k dispozici logicky samostatné na jednotlivé součásti v rámci služby OMS. Všechna data označen za organizace. Tento označování zůstává v celém životním cyklu dat a nevynucují u každé vrstvy služby. Jednotlivé zákazníky má vyhrazené objektů blob Azure, která bude obsahovat dlouhodobé dat

## <a name="data-retention"></a>Uchovávání informací

Indexovaná protokolu hledat data se ukládají a zachovají podle plánu ceny. Další informace najdete v tématu [Ceny analýzy protokolu](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft odstraní data o zákaznících 30 dní po zavření OMS pracovního prostoru. Microsoft odstraněny také účet Azure úložiště, kde jsou uložená data. Při odebrání data o zákaznících žádné fyzické jednotky budou ztraceny.

Následující tabulka uvádí některé z dostupných řešení v OMS a příklady typů dat shromážděných.

| **Řešení** | **Datové typy** |
| --- | --- |
| Konfigurace hodnocení | Konfigurace dat, metadat a data stavu |
| Plánování kapacity | Data o výkonu a metadata |
| Antimalware | Konfigurace data a metadata |
| Systém hodnocení aktualizace | Metadata a stav dat |
| Správa protokolu | Uživatelem definované protokoly událostí, protokoly událostí systému Windows a/nebo protokoly služby IIS |
| Sledování změn | Softwarové inventáře a metadata služby systému Windows |
| Vyhodnocení služby Active Directory a SQL | WMI data, klíče registru, data o výkonu a Správa dynamických SQL Server zobrazit výsledky |

Následující tabulka zobrazuje příklady datové typy:

| **Datový typ** | **Pole** |
| --- | --- |
| Upozornit | Upozornit název, popis oznámení, BaseManagedEntityId, ID problému, IsMonitorAlert, pravidlo, ResolutionState, priority (priorita), závažnosti, kategorie, vlastníka, ResolvedBy, TimeRaised, TimeAdded, změněno, naposledy upravil, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Konfigurace | CustomerID AgentID, ID entit, ManagedTypeID, ManagedTypePropertyID, currentvalue současné, ChangeDate |
| Události | EventId EventOriginalID, BaseManagedEntityInternalId, pravidlo, PublisherId, PublisherName, FullNumber, číslo, kategorie, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Poznámka:** Když se odhlásíte události ve vlastních polích do protokolu událostí systému Windows, shromažďuje OMS je. |
| Metadata | BaseManagedEntityId, ObjectStatus, názvu, ActiveDirectoryObjectSid, PhysicalProcessors, NázevSítě, adresa IP, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP adresu, NetbiosDomainName, LogicalProcessors, Název_dns, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Výkon | Název_objektu název_čítače, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Stav | StateChangeEventId StateId, NewHealthState, OldHealthState, kontext, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, změněno, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Pole fyzicky zabezpečení

Protokol analýzy služby OMS nich denní pracovníky společnosti Microsoft a všechny aktivity přihlášeni a jde auditovat. Služba úplně spustí v Azure a splňuje Azure běžné technickým. Na stránce 18 [Přehled zabezpečení Microsoft Azure](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf)můžete zobrazit údaje o fyzické zabezpečení Azure prostředky. Pole fyzicky přístupových práv k zabezpečená oblastí, se změní do jednoho pracovního dne pro každý, kdo už má zodpovědnost OMS služby, včetně přenos a ukončení. Další informace o globálních fyzické infrastruktury používané v [Microsoft datacentrech](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Vyřešení incidentu správy

OMS má budování proces, který dodržovat všechny služby společnosti Microsoft. Shrnutí, doporučujeme:

- Použití modelu sdílenou odpovědnost kde část zabezpečení zodpovědnost patří Microsoft během část patří zákazníkovi
- Správa případů Azure zabezpečení
  - Zjištění incident na první údaj, spusťte vyšetřování
  - Posuďte, dopad a závažnosti incident tak, že člen týmu incidentu odpovědi na volání. Hodnocení podle informací, může nebo nemusí mít za následek další vedoucím týmu odpověď zabezpečení.
  - Diagnostika incident odpovědi odborníků zabezpečení provede vyšetřování technické záležitosti nebo soudní, určení strategie pro omezení, omezení rizik a řešení. Pokud týmu zabezpečení názoru, že data o zákaznících může stát se jejím vystaven neoprávněným nebo neautorizované osobě, nebude zahájen paralelně paralelního spuštění procesu zákazníka incidentu oznámení.  
  - Stabilizovat a obnovení z incidentu. Tým incidentu vytvoří plán obnovy zmírnit problém. Postup omezení krize například umístění do karantény dotčeném systémy může dojít okamžitě a současně s diagnostiky. Delší mitigations termínů může plánované započteny po termínu okamžité rizika.  
  - Zavřete incident a vedení postmortální. Tým incidentu vytvoří postmortální, které orámuje podrobností o události se v úmyslu opravit zásad, postupů a procesy nechcete, aby nový události.
- Upozornit zákazníky incidentem zabezpečení
  - Určit rozsah dotčeném zákazníky a poskytovat kdokoli, kdo je dopad na co oznámení co nejdřív
  - Vytvoření poznámky o zajistit zákazníci s podrobné dost informací, aby mohli provádět vyšetřování jejich ukončení a splňují všechny závazky, které provedli koncových uživatelů při zpoždění přílišný proces upozornění.
  - Potvrďte a deklarovat incidentu podle potřeby.
  - Upozorněte zákazníky s incidentu oznámení neprodleně množství a v souladu s právními nebo smluvní potvrzení. Do jednu nebo více správců zákazníků budete dostávat oznámení o zabezpečení incidentem jakýmkoli způsobem, který vyberete Microsoft, včetně prostřednictvím e-mailu.
- Vedení připravenosti týmu a školení
  - Pracovníci Microsoft jsou potřeba k dokončení plánování informovanosti školení, přispět tak, aby identifikovat a oznamujte problémy považovaná za zabezpečení a zabezpečení.  
  - Pracovníci pracující na službě Microsoft Azure mít povinností školení sčítání kolem jejich přístup k citlivé systémy hostingu data o zákaznících.
  - Microsoft zabezpečení odpovědi týkající se pracovníků přijímání specializované školení pro jejich role


V případě ztráty dat libovolného zákazníka informováni jednotlivé zákazníky v rámci jeden den. Však ztrátou dat zákazníků nikdy došlo k OMS. Kromě toho prohledávání kopií data, která byla vytvořená a geograficky distributed.

Další informace o jak Microsoft reagovat na události zabezpečení najdete v článku [Microsoft Azure zabezpečení odpovědi v cloudu](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Dodržování předpisů

OMS software development a služby týmu informace o zabezpečení a zásady správného řízení program podporuje dodržováním předpisů business a vyhovuje pro právních předpisů, jak je uvedeno na [Centrum zabezpečení aplikace Microsoft Azure](https://azure.microsoft.com/support/trust-center/) a [Dodržování předpisů Centrum zabezpečení aplikace Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Jak OMS naváže požadavkům na zabezpečení, identifikuje ovládací prvky zabezpečení, spravuje a sleduje rizika i popsané tam. Za rok, můžeme udělat seznámit s zásady organizace pro standardy postupy a pokyny.

Každý člen týmu vývoje OMS obdrží školení týkající se zabezpečení formální aplikace. Interně používáme systému správy verzí pro software development. Jednotlivé projekty software je chráněn systému správy verzí.

Společnost Microsoft nemá zabezpečení a dodržování předpisů týmu, který řídí karty a její všechny služby v aplikaci Microsoft. Informace o zabezpečení osoby zodpovědné za zajišťování tvoří týmu a ostatní vás přidružené k technickým oddělení, která vyvíjet OMS. Osoby zodpovědné za zajišťování zabezpečení máte svoje vlastní (managementu) a vedení nezávislé hodnocení produktů a služeb zajistit zabezpečení a dodržování předpisů.

Společnosti Microsoft správní rady proběhne a výroční zpráva o všech zabezpečení informací programy společnosti Microsoft.

Software OMS týmu vývoje a služby aktivně pracuje s týmy Legal a dodržování předpisů a jiných odvětví partnery získat řadu certifikace.

## <a name="security-standards-certifications"></a>Certifikace standardy zabezpečení

Přihlaste se analýzy OMS aktuálně splňují následující standardy zabezpečení:

- [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm) a kompatibilní se standardem [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
- Platební karty (PCI kompatibilní se standardem) dat zabezpečení standardního (PCI rozhodování) standardy Rada PCI zabezpečení.
- [Služby organizace ovládacích prvků (SOC) 1 typu 1 a 1 typ 2 SOC](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) požadavkům
- Inženýrské funkce kritéria běžné systému Windows
- Certifikace MOS (Microsoft důvěryhodný výpočetních)
- Jako služby Azure součásti, která využívají OMS dodržovat Azure předpisů. Další na [Dodržování předpisů Centrum zabezpečení aplikace Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).


## <a name="cloud-computing-security-data-flow"></a>Cloud computing zabezpečení toku dat
Na následujícím obrázku vidíte Architektura zabezpečení cloudové jako tok informací z vaší společnosti a jak je zabezpečená, jako je přesune do služby protokolu analýzy nakonec prezentací, které jste na portálu OMS. Další informace o každém kroku spočívá v diagramu.

![Obrázek OMS shromažďování dat a zabezpečení](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. registraci protokolu technologie pro analýzu a shromažďovat data

Pro vaši organizaci odeslání dat do protokolu technologie pro analýzu konfigurace Windows agenti – agentů spuštěných na Azure virtuálních počítačích nebo agentům OMS Linux. Pokud používáte Operations Manager agentů, potom použijete Průvodce konfigurací v konzole operace je nastavit. Uživatelé, (které může být můžete, jiné jednotlivé uživatele nebo skupinu lidí) vytvořit jeden nebo více účtů OMS (OMS pracovní prostory) a zaregistrovat agentů jedním z následujících účtů:


- [ID organizace](../active-directory/sign-up-organization.md)

- [Účet Microsoft - Outlooku Office Live MSN](http://www.microsoft.com/account/default.aspx)

Pracovní prostor OMS je místo, kam dat shromážděných, agregované, analyzovat a prezentovat. Pracovní prostor primárně používají jako prostředek k datům oddíl a jednotlivých pracovních prostorech je jedinečný. Můžete třeba datům výrobní správy pomocí jednoho pracovního prostoru OMS a testovací data správy pomocí jiného pracovního prostoru. Pracovní prostory taky zajistit řízení přístupu uživatelů správce data. Jednotlivých pracovních prostorech můžete mít víc uživatelských účtů přidružená a jednotlivých uživatelských účtů můžete získat přístup k více OMS pracovního prostoru. Vytváření pracovních prostorů na základě oblasti datacentra. Jednotlivých pracovních prostorech replikovat na jiné datacentrech v oblasti, především dostupnosti OMS služby.

Pro Operations Manager po dokončení Průvodce konfigurací každá skupina správy Operations Manager naváže připojení ke službě protokolu analýzy. Pomocí Průvodce přidání počítače pak zvolte kterých počítačích ve skupině Správa jsou povoleny odeslání dat do služby. Pro jiné typy agent každý připojuje bezpečně ke službě OMS.

Veškeré komunikace mezi propojených systémů a služba protokolu analýzy musí být zašifrovaný.  Protokol TLS (HTTPS) slouží k šifrování.  Microsoft SDL process následuje a zkontrolujte, že protokolu analýzy aktuální a mají nejnovější pokroky v seznamu kryptografický protokoly.

Každý typ agent shromažďuje data pro analýzu protokolu. Typ dat, která se shromažďují závisí na typech použitého. V tématu Přehled shromažďování dat u [solutions přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md). Podrobnější informace o kolekce navíc prováděna u většiny řešení. Řešení je sada předdefinovaných zobrazení, protokolu vyhledávacích dotazů, pravidla shromažďování dat a logiky pro zpracování. Pouze správci umožňuje protokolu analýzy import řešení. Po importu řešení se přesune Operations Manager serverů pro správu (Pokud je použit) a potom na agentů, které jste zvolili. Potom agentů shromáždit data.

## <a name="2-send-data-from-agents"></a>2. odeslat data z agentů

Zaregistrujte všechny typy agent klíčem zápisu a vytvoření zabezpečené připojení mezi agent a služba protokolu analýzy pomocí ověřování na základě certifikát a SSL s portem 443. OMS používá tajné úložiště generovat a udržovat klíče. Privátních klíčů jsou otočený každých 90 dnů a uložené v Azure a jsou spravovány Azure operace, kteří sledují striktně regulačních a dodržování předpisů postupy.

S Operations Manager zaregistrovat pracovního prostoru pomocí služby protokolu technologie pro analýzu a zabezpečené připojení HTTPS je mezi Operations Manager server pro správu.

U Azure virtuálních počítačích se systémem Windows agenti – klíč úložiště jen pro čtení slouží k číst diagnostických událostí v Azure tabulkách.

-Li zástupce nemůžete komunikovat ke službě z nějakého důvodu, shromážděná data uložena v dočasné mezipaměti a serveru pro správu pokusí odeslání data každé 8 minut, než 2 hodin. Data uložená v mezipaměti agenta je chráněn operační systém úložiště přihlašovacích údajů. Pokud službu se nedají zpracovat data po 2 hodin, agentů fronty data. Pokud fronty naplnění, spustí OMS přetažením datové typy, počínaje data o výkonu. Limit fronty agent je klíče registru mohli upravovat, v případě potřeby. Údaje je komprimované a odešle ke službě vynechání místní databáze, aby se nepřidá síly je. Po odeslání shromážděná data se odebere z mezipaměti.

Jak jsme je popsali výše, data ze svého agentů jsou odeslány přes připojení SSL datacentrech Microsoft Azure Pokud chcete můžete ExpressRoute ke zvýšení zabezpečení pro data. ExpressRoute je způsob, jak přímo připojit k Azure z existující sítě WAN, jako je více protokol popisek přepínání (MPLS) virtuální privátní sítě, které Dal poskytovatel služeb sítě. Další informace najdete v tématu [ExpressRoute](https://azure.microsoft.com/services/expressroute/).


## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. službu protokolu analýzy obdrží a zpracování dat.

Služba protokolu Analytics zajistí příchozí data z důvěryhodného zdroje ověřením certifikáty a integrity dat s Azure ověřování. Nezpracovaná nezpracovanými daty pak uložená jako objektů blob v [Úložišti tabulek Microsoft Azure](../storage/storage-introduction.md) a není šifrovaná. Každý Azure úložiště objektů blob má však sadu jedinečnou sadu klávesy, která je dostupná jenom pro daného uživatele. Typ dat, který je uložený závisí na typech řešení, která jste importovat a shromáždit data. Služba protokolu Analytics zpracuje původní data pro blog Azure úložiště.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. pomocí protokolu analýzy přístup k datům

Se můžete přihlásit protokolu analýzy na portálu OMS pomocí účtu organizace nebo účet Microsoft, který se dřív. Všechny přenosy mezi OMS portálem a analýzy protokolu v OMS odeslaný prostřednictvím zabezpečeného kanálu HTTPS. Pokud chcete použít portálu OMS, ID relace se v klientském počítači uživatele (webový prohlížeč) a data se ukládají v místní mezipaměti až do ukončení relace. Při ukončení, se odstraní mezipaměti. Soubory cookie klienta, které neobsahují určitelné osobní údaje, se automaticky neodebírají. Soubory cookie relací jsou označeny jako HTTPOnly a jsou správně zapojené. Po předem určených době nečinnosti bude ukončena OMS portálu relace.

Na portálu OMS, můžete exportovat data do souboru CSV a dostanete dat pomocí rozhraní API vyhledávání. Exportovat ve formátu CSV se omezí na více než 50 000 řádků na exportovat a rozhraní API dat je omezen na 5 000 řádků na vyhledávání.

## <a name="next-steps"></a>Další kroky

- [Začínáme s protokolu analýzy](log-analytics-get-started.md) se dozvíte další informace o protokolu technologie pro analýzu a a její spuštění v minutách.
