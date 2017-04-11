<properties
    pageTitle="Rozhraní API DocumentDB Node.js & SDK | Microsoft Azure"
    description="Seznamte se všechny rozhraní Node.js API a SDK včetně data vydání, odchod do důchodu dat a změny mezi jednotlivých verzích DocumentDB Node.js SDK."
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>Rozhraní API DocumentDB a SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ZBÝVAJÍCÍ](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>Rozhraní API DocumentDB Node.js a SDK

<table>
<tr><td>**Stažení SDK**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**Rozhraní API si přečtěte následující dokumentaci**</td><td>[Si přečtěte následující dokumentaci odkaz Node.js rozhraní API](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**Pokyny k instalaci SDK**</td><td>[Pokyny k instalaci](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Přispívat SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**Vzorky**</td><td>[Ukázky Node.js](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Získání Začínáme kurz**</td><td>[Začínáme s Node.js SDK](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Kurz web app**</td><td>[Vytvoření webové aplikace Node.js pomocí DocumentDB](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Aktuální podporované platformy**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>Poznámky k verzi

###<a name="1.10.0"/>1.10.0</a>

- Přidaná podpora křížové dotazy paralelní oddíl.
- Přidaná podpora dotazů horní/Order Collections oddílů.

###<a name="1.9.0"/>1.9.0</a>

- Podpora zásad přidané opakovat omezené žádosti. (Omezené požadavky obdrží zadat požadavek sazba je příliš velký výjimku, kód chyby 429.) Ve výchozím nastavení DocumentDB opakuje devět časy pro každý požadavek vyskytne kód chyby 429 ctít zásady retryAfter čas v záhlaví odpověď. Časový interval pevné opakovat lze nyní nastavit jako součást vlastnost RetryOptions objektu ConnectionPolicy Pokud si chcete ignorovat retryAfter čas vrátí serverem mezi opakování. DocumentDB teď požádat o čeká než 30 sekund pro jednotlivá pole, která je právě omezena (bez ohledu na počet opakování) a vrátí odpověď s kódem chyby 429. Tentokrát lze také přepsat ve vlastnosti RetryOptions ConnectionPolicy objektu.

- DocumentDB nyní vrátí x-ms omezení--počet opakování a x-ms-throttle-retry-wait-time-ms jako záhlaví odpovědi v každé žádosti k označení omezení opakovat počet a doby cummulative žádost počkat mezi opakování.

- Třídy RetryOptions jsme přidali, vystavení vlastnost RetryOptions na ConnectionPolicy třídy, která slouží k nahrazení výchozích možností opakovat.

###<a name="1.8.0"/>1.8.0</a>

 - Přidá podpory pro účty databáze více oblastí.

###<a name="1.7.0"/>1.7.0</a>

- Přidá podporu funkce čas na Live(TTL) dokumentů.

###<a name="1.6.0"/>1.6.0</a>

- Implementovaná [rozdělený kolekce](documentdb-partition-data.md) a [uživatelem definovaných výkonu úrovně](documentdb-performance-levels.md).

###<a name="1.5.6"/>1.5.6</a>

- Pokud nebyla vrácení odkazy kvůli chybná propojit výsledky pevné RangePartitionResolver.resolveForRead chyb.

###<a name="1.5.5"/>1.5.5</a>

- Pevná hashParitionResolver resolveForRead(): při zadaný žádný oddíl klíč byl vyvolání výjimky místo vrací seznam všech registrovaných odkazů.

###<a name="1.5.4"/>1.5.4</a>

- Opravy problémů [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - snaží Agent HTTPS: Blokovat úpravy globální agent pro účely DocumentDB. Vyhrazené agent použijte u všech požadavků knihovny.

###<a name="1.5.3"/>1.5.3</a>

- Opravy problémů [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - správně úchyt pomlčky ID média.

###<a name="1.5.2"/>1.5.2</a>

- Opravy problémů [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter posluchače proniknout upozornění.

###<a name="1.5.1"/>1.5.1</a>

- Opravy vydat [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - přejmenujte složku Hash hash systému malá a velká písmena.

### <a name="1.5.0"/>1.5.0</a>

- Podpora sharding implementace přidáním hash & rozsah překládání oddíl.

### <a name="1.4.0"/>1.4.0</a>

- Implementace Upsert. Nové metody upsertXXX na documentClient.

### <a name="1.3.0"/>1.3.0</a>

- Vynechán, můžete získat čísla verze zarovnání s jiných SDK.

### <a name="1.2.2"/>1.2.2</a>

- Rozdělení Q slibuje obálky do nového úložiště.
- Aktualizovat soubor balíčku pro npm registru.

### <a name="1.2.1"/>1.2.1</a>

- Implementuje ID základě směrování.
- Se dá vyřešit problém [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - aktuální vlastnost konflikty pomocí metody current().

### <a name="1.2.0"/>1.2.0</a>

- Přidaná podpora geografická index.
- Ověří vlastnost id pro všechny zdroje. Nesmí obsahovat ID pro zdroje?, /, #, #47; & #47; znaky nebo end mezeru.
- Přidá nový záhlaví "index transformace průběh" ResourceResponse.

### <a name="1.1.0"/>1.1.0</a>

- Používá indexování zásad V2.

### <a name="1.0.3"/>1.0.3</a>

- Problém – #40 (https://github.com/Azure/azure-documentdb-node/issues/40) - implementovaná eslint grunt konfigurace a v základní a promise SDK.

### <a name="1.0.2"/>1.0.2</a>

- Problém [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - sliby obálky nezahrnuje záhlaví s chybou.

### <a name="1.0.1"/>1.0.1</a>

- Implementace možnost dotazu konflikty přidáním readConflicts, readConflictAsync a queryConflicts.
- Aktualizované rozhraní API si přečtěte následující dokumentaci.
- Zadejte [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync chyby.

### <a name="1.0.0"/>1.0.0</a>

- GA SDK.

## <a name="release--retirement-dates"></a>Uvolnění a vyřazování webů kalendářních dat
Microsoft poskytovala oznámení alespoň **12 měsíců** před do důchodu SDK k hladce přechod na verzi novější/podporované.

Nové funkce a funkce a optimalizace pouze přidají aktuální SDK, tak je doporučujeme vždy upgradovat na nejnovější SDK verzi nejdřívější největšímu.

Každá žádost DocumentDB pomocí vyřazené SDK odmítnuta službou.

> [AZURE.WARNING]
Všechny verze SDK DocumentDB Azure pro Node.js před verzí **1.0.0** bude vyřadit na **29 únor 2016**.

<br/>

| Verze | Datum vydání | Datum vyřazování webů
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 03 říjen 2016 |---
| [1.9.0](#1.9.0) | 07 červenec 2016 |---
| [1.8.0](#1.8.0) | 14 června 2016 |---
| [1.7.0](#1.7.0) | 26 duben 2016 |---
| [1.6.0](#1.6.0) | 29 březen 2016 |---
| [1.5.6](#1.5.6) | 08 březen 2016 |---
| [1.5.5](#1.5.5) | 02 únor 2016 |---
| [1.5.4](#1.5.4) | 01 únor 2016 |---
| [1.5.2](#1.5.2) | 26 leden 2016 |---
| [1.5.2](#1.5.2) | 22 leden 2016 |---
| [1.5.1](#1.5.1) | 4 leden 2016 |---
| [1.5.0](#1.5.0) | 31 prosinec 2015 |---
| [1.4.0](#1.4.0) | 06 říjen 2015 |---
| [1.3.0](#1.3.0) | 06 říjen 2015 |---
| [1.2.2](#1.2.2) | 10 září 2015 |---
| [1.2.1](#1.2.1) | 15 srpen 2015 |---
| [1.2.0](#1.2.0) | 05 srpen 2015 |---
| [1.1.0](#1.1.0) | 09 červenec 2015 |---
| [1.0.3](#1.0.3) | 04 června 2015 |---
| [1.0.2](#1.0.2) | 23 květen 2015 |---
| [1.0.1](#1.0.1) | 15 květen 2015 |---
| [1.0.0](#1.0.0) | 08 duben 2015 |---
| 0.9.4-prerelease | 06 duben 2015 | 29 únor 2016
| 0.9.3-prerelease | 14 ledna 2015 | 29 únor 2016
| 0.9.2-prerelease | 18 prosince 2014 | 29 únor 2016
| 0.9.1-prerelease | 22 srpen 2014 | 29 únor 2016
| 0.9.0-prerelease | 21 srpen 2014 | 29 únor 2016


## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Viz taky

Další informace o DocumentDB najdete v tématu stránka služby [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) .
