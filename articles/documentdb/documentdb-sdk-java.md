
<properties
    pageTitle="Rozhraní API DocumentDB Java & SDK | Microsoft Azure"
    description="Seznamte se všechny rozhraní Java API a SDK včetně data vydání, odchod do důchodu dat a změny mezi jednotlivých verzích DocumentDB Java SDK."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>Rozhraní API DocumentDB a SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ZBÝVAJÍCÍ](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>Rozhraní API DocumentDB Java a SDK

<table>
<tr><td>**Stažení SDK**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**Rozhraní API si přečtěte následující dokumentaci**</td><td>[Referenční dokumentaci rozhraní API Java](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Přispívat SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Začínáme**</td><td>[Začínáme s Java SDK](documentdb-java-application.md)</td></tr>
<tr><td>**Aktuální podporovaný modul runtime**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Poznámky k verzi

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Přidaná podpora BoundedStaleness konzistence úroveň.
  - Přidaná podpora přímé připojení pro operace CRUD pro oddíly kolekce.
  - Pevná chyby v dotazu na databázi SQL.
  - Pevná chybu v mezipaměti relace, kde může být nesprávně nastaveno token relace.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Přidaná podpora křížové dotazy paralelní oddíl.
  - Přidaná podpora dotazů horní/Order Collections oddílů.
  - Přidaná podpora silných konzistence.
  - Přidaná podpora požadavky na základě názvů při použití přímé připojení.
  - Pevná aby ActivityId zachovat konzistentní napříč všechny žádosti o opakování.
  - Pevná chyb souvisejících s mezipaměti relace, při opětovném vytváření kolekce se stejným názvem.
  - Byly přidány mnohoúhelník a datové typy LineString při určování kolekce indexování zásad pro oddělení geo prostorové dotazy.
  - Pevné problémy s Java dokumentu jazyka Java 1.8.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - Pevná chybu v PartitionKeyDefinitionMap mezipaměti jeden oddíl kolekcí a není extra vzdálené použití oddílů klíčové žádosti o.
  - Pevná chybu opakované pokusy o je-li hodnoty klíče nesprávné oddíl.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - Přidá podpory pro účty databáze více oblastí.
  - Přidaná podpora automatické opakování omezené požadavky s možností přizpůsobení max pokusů a max opakovat prodleva.  Přečtěte si téma RetryOptions a ConnectionPolicy.getRetryOptions().
  - Nepoužívají se IPartitionResolver na základě vlastního kódu rozdělení. Stiskněte klávesovou zkratku rozdělený kolekcí vyšší úložiště a výkon.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Přidané opakovat podpory zásad pro omezení.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Čas s přidanou podpoře live (TTL) pro dokumenty.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Implementovaná [rozdělený kolekce](documentdb-partition-data.md) a [uživatelem definovaných výkonu úrovně](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Pevná chybu v HashPartitionResolver generovat hash hodnoty v malé endian konzistentní s jiných SDK.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Přidání Hash & oblasti oddílu překládání pomáhat s aplikacemi sharding přes více oddílů.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Implementace Upsert. Nové metody upsertXXX přidané do Upsert funkci podporují.
- Implementace ID základě směrování. Žádné veřejné změnám rozhraní API, všechny změny vnitřní.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Uvolnění přeskočilo můžete získat číslo verze zarovnání s jiných SDK

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Podporuje geografická indexu
- Ověří vlastnost id pro všechny zdroje. ID pro zdroje nesmí obsahovat ?, /, # \, znaky nebo end mezeru.
- Přidá nový záhlaví "index transformace průběh" ResourceResponse.

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- Implementuje V2 indexování zásad

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- GA SDK

## <a name="release--retirement-dates"></a>Uvolnění a vyřazování webů kalendářních dat
Microsoft poskytovala oznámení alespoň **12 měsíců** před do důchodu SDK k hladce přechod na verzi novější/podporované.

Nové funkce a funkce a optimalizace pouze přidají aktuální SDK, tak je doporučujeme vždy upgradovat na nejnovější SDK verzi nejdřívější největšímu.

Každá žádost DocumentDB pomocí vyřazené SDK odmítnuta službou.

> [AZURE.WARNING]
Všechny verze SDK DocumentDB Azure jazyka Java před verzí **1.0.0** bude vyřadit na **29 února 2016**.

<br/>

| Verze | Datum vydání | Datum vyřazování webů
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 28 říjen 2016 |---
| [1.9.0](#1.9.0) | 03 říjen 2016 |---
| [1.8.1](#1.8.1) | 30. června 2016 |---
| [1.8.0](#1.8.0) | 14 června 2016 |---
| [1.7.1](#1.7.1) | 30 duben 2016 |---
| [1.7.0](#1.7.0) | 27 duben 2016 |---
| [1.6.0](#1.6.0) | 29 březen 2016 |---
| [1.5.1](#1.5.1) | 31 prosinec 2015 |---
| [1.5.0](#1.5.0) | 04 prosinec 2015 |---
| [1.4.0](#1.4.0) | 05 říjen 2015 |---
| [1.3.0](#1.3.0) | 05 říjen 2015 |---
| [1.2.0](#1.2.0) | 05 srpen 2015 |---
| [1.1.0](#1.1.0) | 09 červenec 2015 |---
| [1.0.1](#1.0.1) | 12 květen 2015 |---
| [1.0.0](#1.0.0) | 07 duben 2015 |---
| 0.9.5-prelease | 09 března 2015 | 29 únor 2016
| 0.9.4-prelease | 17 únor 2015 | 29 únor 2016
| 0.9.3-prelease | 13 ledna 2015 | 29 únor 2016
| 0.9.2-prelease | 19 prosince 2014 | 29 únor 2016
| 0.9.1-prelease | 19 prosince 2014 | 29 únor 2016
| 0.9.0-prelease | 10 prosince 2014 | 29 únor 2016

## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Viz taky

Další informace o DocumentDB najdete v tématu stránka služby [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) .
