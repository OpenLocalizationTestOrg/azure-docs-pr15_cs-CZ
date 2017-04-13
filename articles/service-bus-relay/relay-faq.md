<properties 
    pageTitle="Předávání zpráv nejčastější dotazy týkající se | Microsoft Azure"
    description="Odpovídá na některé časté otázky týkající se předávací Azure."
    services="service-bus"
    documentationCenter="na"
    authors="jtaubensee"
    manager=""
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub" />

# <a name="relay-faq"></a>Předávací časté otázky

Tento článek odpovídá na některé časté otázky k Microsoft Azure Relay. Můžete také navštívit [Azure podporují nejčastější dotazy týkající se](http://go.microsoft.com/fwlink/?LinkID=185083) pro obecné Azure ceny a informace o podpoře. V následujících tématech jsou však započítávány:

- [Obecné dotazy týkající se Azure Relay](#general-questions)
- [Ceny](#pricing)
- [Kvóty](#quotas)
- [Správa předplatného a obor názvů](#subscription-and-namespace-management)
- [Řešení potíží](#troubleshooting)

## <a name="general-questions"></a>Obecné otázky

### <a name="what-is-azure-relay"></a>Co je Azure Relay?

[Služby pro přenos dat](relay-what-is-it.md) poskytuje možnost transparentně hostovat a přístup k služby WCF odkudkoli. Jinými slovy díky hybridní aplikace spuštěné v Azure datacentra a místní podnikovém prostředí.

### <a name="what-is-a-relay-namespace"></a>Co je obor Relay?

[Obor názvů](Relay-create-namespace-portal.md) poskytuje oboru kontejner adresování Relay zdrojů v aplikaci. Vytvoření některého je nutné použít předávání a bude se jednat o první kroky v příručce Začínáme.

## <a name="pricing"></a>Ceny

V této části najdete odpovědi na některé časté otázky o přenosu ceny strukturu. Můžete taky navštivte [Nejčastější dotazy týkající se Azure podpory](http://go.microsoft.com/fwlink/?LinkID=185083) obecné informace ceny Microsoft Azure. Podrobné informace o přenosu ceny najdete v článku [služby Bus ceny podrobnosti](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="how-do-you-charge-for-relay"></a>Jak můžete účtovat poplatky za Relay?

Podrobné informace o předávací ceny, najdete v tématu [služby Bus ceny podrobnosti][Pricing overview]. Kromě ceny poznamenat vám bude účtovaná za převodů spojenými daty pro výstupní nejsou v datovém centru, ve kterém máte k dispozici aplikace.

### <a name="what-usage-of-relay-is-subject-to-data-transfer"></a>K čemu použití předávací podléhá přenos dat?

Předávací obsahuje 5 GB dat průniku měsíčně na jedno předplatné. Je bezplatná další Azure průniku/výstupní data použitá Relay.

Data částka za předávací pro průniku od odesílatelů pouze je jako předávací posluchače ušetří poplatků data. Například pokud odešlete 1 GB je bude pouze vám nebudou účtovat poplatky pro 1 GB Přestože posluchače taky dostali 1 GB a může být mimo datacentrech Azure společnosti.

### <a name="how-are-relay-hours-calculated"></a>Jak se počítají Relay hodin?

Předávací hodin je faktura pro kumulativního dobu, po kterou každý Relay otevřeném "" během určitého fakturační období. Aby sloužil implicitně vytvořena a otevřeli v dané názvů při předávání posluchače prvním připojení ke, které budou řešit. Přenos je zavřený pouze v případě, že poslední posluchače odpojí od jeho adresu. Proto z důvodu fakturaci, který předávací je považován za "Otevřít" od doby první posluchače Relay spojí, do doby, kdy poslední posluchače Relay odpojí od názvů. Jinými slovy přenosu se považuje za otevřít kdykoli jeden nebo více posluchače Relay připojeni k jeho adresu Bus služby.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-given-relay"></a>Co když používám víc posluchače připojené k dané Relay?

V některých případech může být jeden Relay více propojených posluchače. Aby sloužil je považován za "Otevřít"-li alespoň jeden posluchače Relay připojen k němu. Přidáním dalších posluchače otevřít předat bude mít za následek další relay hodin. Počet předávací odesílatelů (klientech vyvolat nebo odeslání zprávy na relé) připojených k předávací také nemá žádný vliv na výpočet předávací hodin.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>Výpočet měřiči zpráv pro WCF relé

**To platí pouze pro relé WCF a není náklady hybridní připojení**

Obecně se počítají fakturaci zprávy pro relé stejným způsobem jako jsme je popsali výše brokered entit (fronty, témata a předplatná). Můžou ale nastat několik důležitých rozdílů:

Odesílání že zprávy Service Bus Relay zpracován jako "celou prostřednictvím" Odeslat Relay posluchače, který přijme zprávu, nikoli odeslat do Service Bus Relay následovat oznámení o doručení pro posluchače Relay. Proto požadavek odpověď styl služby vyvolání (až 64 KB) proti posluchače Relay bude mít za následek dvě fakturaci zprávy: jeden fakturaci zprávu žádosti a jedné fakturaci zprávy na odpověď (za předpokladu, že odpověď je také \<= 64 KB). Tím se liší od pomocí fronty roli prostředníka mezi klientem a služby. V takovém případě vyžadují stejné vzorek odpověď na žádost o odeslání žádosti o do fronty, následovaný dequeue/oznámení o doručení ve frontě ke službě a za ním uveďte odeslat odpověď do jiné fronty a dequeue/oznámení o doručení z této fronty klientovi. Použití stejné (\<= 64 KB) velikost předpoklady v celém vzorek zprostředkovaného fronty by tedy za následek čtyři fakturaci zprávy dvakrát číslo fakturované implementovat stejné vzorek pomocí přenosových. Samozřejmě jsou výhody používání fronty dosáhnout tohoto vzorku, například životnosti a načíst vyrovnání. Tyto výhody může do bloku další výdajů.

Relé, které jsou otevřené pomocí vazby WCF netTCPRelay zacházet zprávy není jako jednotlivé zprávy, ale jako toku dat předávaných celém systému. Jinými slovy jen odesílatele a posluchače mít přehled o orámování jednotlivé zprávy: odeslané/doručeného pomocí tuto vazbu. Pro relé pomocí vazby netTCPRelay, tedy všechna data zpracován jako toku při výpočtu fakturaci zprávy. V tomto případě služby Bus vypočítá celkovou kapacitu dat poslali nebo přijmete prostřednictvím každý jednotlivé Relay na základě 5 minut a dělit celková 64 KB s cílem určit počet fakturaci zpráv pro přenos během tohoto časového období.

## <a name="quotas"></a>Kvóty

|Název kvóty|Rozsah|Typ|Chování při překročení|Hodnota|
|---|---|---|---|---|
|Souběžné posluchačů na předávací|Entita|Statická|Následující požadavky na další připojení budou odmítnuty a výjimku obdrží volající kód.|25|
|Souběžné relay posluchače|Systémové|Statická|Následující požadavky na další připojení budou odmítnuty a výjimku obdrží volající kód.|2 000|
|Souběžné relay připojení za všechny koncové body relay v služby názvů|Systémové|Statická|-|5 000|
|Koncové body Relay service obor|Systémové|Statická|-|10 000|
|Velikost zprávy [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) a [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx) přenášet|Systémové|Statická|Odmítnuty příchozích zpráv, které překročení kvóty a výjimku obdrží volající kód.|64KB
|Velikost zprávy [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) a [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) přenášet|Systémové|Statická|-|Neomezený|

### <a name="does-relay-have-any-usage-quotas"></a>Má Relay všechny kvóty využití?

Ve výchozím nastavení pro všechny cloudové služby Microsoft nastaví agregační měsíční kvóta využití prostředků, které se nevypočítávají ve všech předplatných zákazníka. Protože nám jasné, že budete potřebovat víc než tato omezení, obraťte se prosím informace o službě zákazníkům kdykoli tak, aby můžeme pochopit vašim potřebám a upravte tyto limity řádně podporovat. Pro službu Bus agregační využití kvóty patří následující:

- 5 miliard zprávy
- 2 miliony Relay hodin

Během společnost Microsoft si vyhrazuje právo zakázat účet zákazníka, který má překročilo jeho používání v daném měsíci, jsme bude poskytovat e-mailové oznámení a několika pokusech o kontakt zákazníků před provedením všech akcí. Zákazníci překročení kvóty pořád budete zodpovědní za náklady, které překročení kvóty.

#### <a name="naming-restrictions"></a>Pojmenování omezení

Obor názvů název přenosu lze pouze mezi 6-50 znaků.

## <a name="subscription-and-namespace-management"></a>Správa předplatného a obor názvů

### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Jak můžu migrovat obor názvů na jiné předplatné Azure?

Můžete použít příkazy Powershellu (naleznete v článku [tady](../service-bus-messaging/service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription)) přesunout obor z Azure předplatných. Abyste mohli provést operaci, musí být oboru už aktivní. Také uživatele provádění příkazů, musíte být správcem ve zdrojové a cílové předplatná.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-their-suggested-actions"></a>Co je několik výjimek generovaných Azure Relay API a jejich navržené akce?

[Předávací výjimky] [ Relay exceptions] článek popisuje některé výjimky s navržené akce.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Co je aplikace Access podpis sdílené a jazyky, které podporují generování podpis?

Sdílené podpisy přístupu jsou mechanismus ověřování podle SHA – 256 zabezpečené hash nebo identifikátory URI. Informace o tom, jak vytvářet vlastní podpisů v uzel PHP, Java a C\#, najdete v článku [Sdílené podpisy přístup][] .

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Sdílený přístup podpisy]: service-bus-sas-overview.md

## <a name="next-steps"></a>Další kroky:

- [Vytvořit obor názvů](relay-create-namespace-portal.md)
- [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Začínáme s uzel](relay-hybrid-connections-node-get-started.md)