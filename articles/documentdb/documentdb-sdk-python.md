<properties 
    pageTitle="Rozhraní API DocumentDB Python & SDK | Microsoft Azure" 
    description="Seznamte se všechny rozhraní Python API a SDK včetně data vydání, odchod do důchodu dat a změny mezi jednotlivých verzích DocumentDB Python SDK." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>Rozhraní API DocumentDB a SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ZBÝVAJÍCÍ](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>Rozhraní API DocumentDB Python a SDK

<table>
<tr><td>**Stažení SDK**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**Rozhraní API si přečtěte následující dokumentaci**</td><td>[Rozhraní API Python dokumentaci](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**Pokyny k instalaci SDK**</td><td>[Pokyny k instalaci Python SDK](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Přispívat SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Začínáme**</td><td>[Začínáme s Python SDK](documentdb-python-application.md)</td></tr>
<tr><td>**Aktuální podporované platformy**</td><td>[Python 2.7](https://www.python.org/downloads/) a [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Poznámky k verzi

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Přidaná podpora Python 3.5.
- Přidaná podpora fond připojení pomocí modulu žádosti.
- Přidaná podpora konzistence relace.
- Přidaná podpora horní/ORDERBY dotazů pro oddíly kolekce.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Podpora zásad přidané opakovat omezené žádosti. (Omezené požadavky obdrží zadat požadavek sazba je příliš velký výjimku, kód chyby 429.) Ve výchozím nastavení DocumentDB opakuje devět časy pro každý požadavek vyskytne kód chyby 429 ctít zásady retryAfter čas v záhlaví odpověď. Časový interval pevné opakovat lze nyní nastavit jako součást vlastnost RetryOptions objektu ConnectionPolicy Pokud si chcete ignorovat retryAfter čas vrátí serverem mezi opakování. DocumentDB teď požádat o čeká než 30 sekund pro jednotlivá pole, která je právě omezena (bez ohledu na počet opakování) a vrátí odpověď s kódem chyby 429. Tentokrát lze také přepsat ve vlastnosti RetryOptions ConnectionPolicy objektu.

- DocumentDB nyní vrátí x-ms omezení--počet opakování a x-ms-throttle-retry-wait-time-ms jako záhlaví odpovědi v každé žádosti k označení omezení opakovat počet a doby cummulative žádost počkat mezi opakování.

- Odebere tříd RetryPolicy a odpovídající vlastnost (retry_policy) zveřejněným v předmětu document_client a místo toho zavádí třída RetryOptions vystavení vlastnost RetryOptions na ConnectionPolicy předmětu, které slouží k nahrazení výchozích možností opakovat.

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - Přidá podpory pro účty databáze více oblastí.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- Přidá podporu funkce čas na Live(TTL) dokumentů.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Související s rozdělení straně serveru umožňuje speciální znaky v parametru path partitionkey opravy chyb.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Implementovaná [rozdělený kolekce](documentdb-partition-data.md) a [uživatelem definovaných výkonu úrovně](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Přidání Hash & oblasti oddílu překládání pomáhat s aplikacemi sharding přes více oddílů.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Implementace Upsert. Nové metody UpsertXXX přidané do Upsert funkci podporují.
- Implementace ID základě směrování. Žádné veřejné změnám rozhraní API, všechny změny vnitřní.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Podporuje geografická index.
- Ověří vlastnost id pro všechny zdroje. ID pro zdroje nesmí obsahovat ?, /, # \, znaky nebo end mezeru.
- Přidá nový záhlaví "index transformace průběh" ResourceResponse.

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- Používá indexování zásad V2.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Podporuje připojení proxy serveru.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK.

## <a name="release--retirement-dates"></a>Datum vydání a vyřazování webů
Microsoft poskytovala oznámení alespoň **12 měsíců** před do důchodu SDK k hladce přechod na verzi novější/podporované.

Nové funkce a funkce a optimalizace pouze přidají aktuální SDK, tak je doporučujeme vždy upgradovat na nejnovější SDK verzi nejdřívější největšímu. 

Každá žádost DocumentDB pomocí vyřazené SDK odmítnuta službou.

> [AZURE.WARNING]
Všechny verze SDK DocumentDB Azure pro Python před verzí **1.0.0** bude vyřadit na **29 únor 2016**. 

<br/>

| Verze | Datum vydání | Datum vyřazování webů 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 29 září 2016 |---
| [1.9.0](#1.9.0) | 07 červenec 2016 |---
| [1.8.0](#1.8.0) | 14 června 2016 |---
| [1.7.0](#1.7.0) | 26 duben 2016 |---
| [1.6.1](#1.6.1) | 08 duben 2016 |---
| [1.6.0](#1.6.0) | 29 březen 2016 |---
| [1.5.0](#1.5.0) | 03 leden 2016 |---
| [1.4.2](#1.4.2) | 06 říjen 2015 |---
| [1.4.1](#1.4.1) | 06 říjen 2015 |---
| [1.2.0](#1.2.0) | 06 srpen 2015 |---
| [1.1.0](#1.1.0) | 09 červenec 2015 |---
| [1.0.1](#1.0.1) | 25 květen 2015 |---
| [1.0.0](#1.0.0) | 07 duben 2015 |---
| 0.9.4-prelease | 14 ledna 2015 | 29 únor 2016
| 0.9.3-prelease | 09 prosince 2014 | 29 únor 2016
| 0.9.2-prelease | 25 listopadu 2014 | 29 únor 2016
| 0.9.1-prelease | 23 září 2014 | 29 únor 2016
| 0.9.0-prelease | 21 srpen 2014 | 29 únor 2016

## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Viz taky

Další informace o DocumentDB najdete v tématu stránka služby [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . 
