<properties
    pageTitle="Archiv rozbočovače Azure události | Microsoft Azure"
    description="Základní informace o funkci Azure události rozbočovače archivu."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="azure-event-hubs-archive"></a>Archiv rozbočovače Azure události

Azure archivu rozbočovače události umožňuje automaticky předávat streamování data v události rozbočovače k účtu úložiště objektů Blob podle svého výběru s flexibilnější zadejte čas nebo velikost interval e. Nastavení archivu je rychlý, nejsou žádné pro správu náklady ho spusťte a upraví automaticky k události rozbočovače [výkon jednotky](event-hubs-overview.md#capacity-and-security). Archivace rozbočovače událostí je nejjednodušší způsob, jak načíst streamování data do Azure a umožňuje zaměřit na zpracování dat, nikoli sběr dat.

Azure archivu rozbočovače události umožňuje zpracovat v reálném čase dávku systémem a potrubí na stejném proudu. Umožňuje vytvářet řešení, které můžete roste, pomocí vašim potřebám určitou dobu. Ať vytváříte dávku systémy dnes s sledovat směrem budoucí v reálném čase zpracování nebo chcete přidat efektivně studenou cestu do existujícího v reálném čase řešení, událost rozbočovače archivu díky práci se stejnými daty jednodušší streamování.

## <a name="how-event-hubs-archive-works"></a>Jak funguje události rozbočovače archivu

Událost rozbočovače je trvalé vyrovnávací doba uchovávání informací pro telemetrie průniku, podobně jako distribuované protokol. Klíč zobrazit v události rozbočovače je [rozdělený spotř modelu](event-hubs-overview.md#partition-key). Každý oddíl je nezávislý část dat a je spotřebované množství nezávisle na sobě. Postupně tato data ages vypnout, na základě období, která dokáže nahradit uchovávání informací. V důsledku toho rozbočovači dané události nikdy plná "příliš."

Událost rozbočovače archivu umožňuje zadat vlastní úložišti objektů Blob Azure účtu a kontejner, který se použijí k ukládat archivované data. Tento účet může být ve stejné oblasti jako centrální událost nebo v jiné oblasti, přidání flexibilitu funkce události rozbočovače archivovat.

Archivované data zapisuje ve formátu [Apache Avro][] ; kompaktní, rychlé, binární formát, který poskytuje bohaté datové struktury vložené schéma. Tento formát je rozšířený v ekosystému Hadoop i tak, že toku technologie pro analýzu a Azure Data Factory. Další informace o práci s Avro neexistuje dál v tomto článku.

### <a name="archive-windowing"></a>Práce s okny archivu

Událost rozbočovače archivace umožňuje nastavení okno řídit archivace. Zde je minimální velikost a konfiguraci času s "první wins zásadu," což znamená, že první narazili aktivační operaci archivu. Pokud máte okno archiv 15 minute/100 MB a odeslat 1 MB/s velikost okna spustí před časového intervalu. Každý oddíl archivy nezávisle na sobě a data zapisuje objektů blob dokončené blok okamžiku archiv, s názvem čas, kdy došlo k interval archivu. Konvence vypadá takto:

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### <a name="scaling-to-throughput-units"></a>Změna měřítka výkon jednotky

Přenosy rozbočovače události řídí [výkon jednotky](event-hubs-overview.md#capacity-and-security). Výběru jednoho výkon umožňuje 1 MB/s nebo 1000 události sekundu průniku a dvakrát toto množství výstupní. Standardní rozbočovače události můžete nakonfigurována 1 – 20 výkon jednotky a informace se dá zakoupit pomocí tlačítka Zvětšit kvóty [žádost o podporu][]. Použití za placené výkon jednotky omezena. Událost rozbočovače archivu zkopíruje data přímo z vnitřní úložiště rozbočovače události vynechat výkon jednotku výstupní kvót a uložení vaší výstupní jiní uživatelé zpracování analýzy toku ATP Spark.

Po konfiguraci, událostí rozbočovače archivu automaticky spustí hned, jak odeslat první událost. Spuštění zůstane po celou dobu. Usnadnění pro svůj podřízené zpracování vědět, že proces pracuje, událostí rozbočovače zapisuje prázdné soubory po žádná data. Zajistíte předvídatelná cadence a značky, která můžete kanálu dávku procesorů.

## <a name="setting-up-event-hubs-archive"></a>Nastavení události rozbočovače archivu

V centrální události údaje o času vytvoření prostřednictvím portálu nebo správce prostředků Azure je možné konfigurovat archivu rozbočovače události. Kliknutím **na** tlačítko jednoduše povolit archivu. Konfigurace účtu úložiště a kontejneru po kliknutí na část **kontejneru** zásuvné. Protože události rozbočovače archivu používá ověřování služby service s úložištěm, není potřeba zadat připojovací řetězec úložiště. Výběr zdroje slouží k výběru identifikátor URI účtu úložiště automaticky. Pokud používáte Správce prostředků Azure, musíte zadat Tento identifikátor URI explicitně jako řetězec.

Výchozí časového intervalu je pět minut. Minimální hodnota je 1, maximální 15. Okno **velikost** obsahuje velké množství 10 500 MB.

![][1]

## <a name="adding-archive-to-an-existing-event-hub"></a>Přidání archivace k rozbočovači existující události

Archivy je možné konfigurovat na existující rozbočovače události, které jsou v názvů rozbočovače události. Tato funkce není k dispozici na starší zasílání zpráv nebo různé obory názvů typu. Povolit archivu na rozbočovači existující událost nebo změnit nastavení archivace, klikněte na daný obor názvů načíst zásuvné **Essentials** a potom klikněte na událost rozbočovači, pro kterou chcete povolit nebo změňte nastavení archivu. Nakonec klikněte v části **Vlastnosti** otevřít zásuvné jak je znázorněno na následujícím obrázku.

![][2]

Můžete taky nakonfigurovat archivu rozbočovače události prostřednictvím Správce prostředků Azure šablony. Další informace najdete v [tomto článku](event-hubs-resource-manager-namespace-event-hub-enable-archive.md).

## <a name="exploring-the-archive-and-working-with-avro"></a>Prozkoumání archivu a pracovat s ním Avro

Po konfiguraci, vytvoří událost rozbočovače archivu soubory v úložišti Azure účet a kontejneru umístěna na nakonfigurovaného časového intervalu. Tyto soubory můžete zobrazit všechny nástroje pro například [Průzkumníka úložišť Azure][]. Můžete stahovat soubory místně na nich mají pracovat.

Soubory vytvořené pomocí události rozbočovače archivu mají následující schéma Avro:

![][3]

Pomocí [Nástroje Avro][] sklenice z Apache je snadný způsob, jak můžete prozkoumat Avro soubory. Po stažení této sklenice, zobrazí se schématu konkrétním souborem Avro spuštěním následujícího příkazu:

```
java -jar avro-tools-1.8.1.jar getschema \<name of archive file\>
```

Tento příkaz vrátí

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Pomocí nástrojů Avro můžete také převést soubor do formátu JSON a dělat další zpracování.

Abyste mohli provést pokročilejší zpracování, stáhněte a nainstalujte Avro pro výběr platformy. Při psaní tohoto textu nejsou k dispozici pro C, C++, C implementace\#, Java NodeJS, Perl, PHP, Python a skutečné.

Apache Avro má dokončení příručky Začínáme pro [Java][] a [Python][]. Můžete také v článku [Začínáme s archivu rozbočovače události](event-hubs-archive-python.md) .

## <a name="how-event-hubs-archive-is-charged"></a>Jak bude účtováno události rozbočovače archivu

Archiv rozbočovače událostí je podle objemu dat podobně výkon jednotky, jako poplatek každou hodinu. Poplatku je přímo odpovídat počet jednotek výkon koupili pro obor. Výkon jednotky jsou zvýšit a snížit, událostí rozbočovače archivu zvyšuje a snižuje poskytnout odpovídající výkonu. Metry dojít ve spojení. Náklady pro archivaci rozbočovače událost jsou 0,10 / hod jednotkové výkon, které se slevou 50 % období náhled.

Archiv rozbočovače události Opravdu je nejjednodušší načtení dat do Azure. Pomocí jezera dat Azure, Azure Data Factory a dat Azure HDInsight, můžete provádět dávkové zpracování a další analýzy e pomocí známé nástroje a platformy ve všech velkém měřítku potřebujete.

## <a name="next-steps"></a>Další kroky

Další informace o události rozbočovače navštivte následující odkazy:

- Kompletní [Ukázková aplikace, který využívá rozbočovače události][].
- Ukázka [měřítko, událostí zpracování s rozbočovače události][] .
- [Přehled rozbočovače událostí][]

[Apache Avro]: http://avro.apache.org/
[žádost o podporu]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Průzkumník Azure úložiště]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Nástroje Avro]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Přehled rozbočovače událostí]: event-hubs-overview.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Rozšiřování události zpracování s rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3