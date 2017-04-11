<properties 
    pageTitle="Rozhraní API .NET DocumentDB & SDK | Microsoft Azure" 
    description="Seznamte se všechny rozhraní .NET API a SDK včetně data vydání, odchod do důchodu dat a změny mezi jednotlivých verzích DocumentDB .NET SDK." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>Rozhraní API DocumentDB a SDK 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ZBÝVAJÍCÍ](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>Rozhraní API .NET DocumentDB a SDK

<table>
<tr><td>**Stažení SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**Rozhraní API si přečtěte následující dokumentaci**</td><td>[Referenční dokumentaci rozhraní API .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Vzorky**</td><td>[.NET ukázky](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Začínáme**</td><td>[Začínáme s DocumentDB .NET SDK](documentdb-get-started.md)</td></tr>
<tr><td>**Kurz web app**</td><td>[Vývoj webových aplikací s DocumentDB](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Aktuální podporované framework**</td><td>[Rozhraní Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Poznámky k verzi

> [AZURE.IMPORTANT] Od verze 1.9.2, může se zobrazit System.NotSupportedException při dotazování na rozdělený kolekcí. Aby nedocházelo k této chybě, zajistěte, aby byl váš hostitel proces 64bitová verze. Spustitelný projektů stačí tak, že zrušíte zaškrtnutí možnosti "Preferovat 32bitová verze" v okně Vlastnosti projektu na kartě vytvořit.

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Podpora přidané přímé připojení pro oddíly kolekce.
  - Vyšší výkon pro úroveň konzistence ohraničených Staleness.
  - Byly přidány mnohoúhelník a datové typy LineString při určování kolekce indexování zásad pro oddělení geo prostorové dotazy.
  - Přidaná LINQ podpora StringEnumConverter, IsoDateTimeConverter a UnixDateTimeConverter při převodu predikáty.
  - Různé SDK opravy chyb.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Podařilo odstranit problém, který způsobil následující NotFoundException: čtení cvičení není k dispozici pro token vstupní relace. Tato výjimka v některých případech při dotazování na oblasti přečtené geo distributed účtu.
  - Ukazuje vlastnost ResponseStream ve třídě ResourceResponse, která umožňuje přímý přístup k základní proudu z odpověď.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[otázku 1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - K úpravě tříd ResourceResponse FeedResponse, StoredProcedureResponse a MediaResponse implementovat odpovídající veřejné rozhraní tak, aby mohli být mocked test řízený úsilím nasazení (TDD).
  - Podařilo odstranit problém, který způsobil klíčové záhlaví poškozený oddíl při použití vlastního JsonSerializerSettings objektu pro serializaci data.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Podařilo odstranit problém, který způsobil dlouhodobé dotazů selhání s chybou: povolení token není platný v současné době.
  - Podařilo odstranit problém, který odebere původní SqlParameterCollection z křížové oddíl horní/Order dotazů.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Přidaná podpora paralelní dotazy pro oddíly kolekce.
  - Přidaná podpora křížové dotazy Order a horní oddíl pro oddíly kolekce.
  - Pevná chybějící odkazy DocumentDB.Spatial.Sql.dll a Microsoft.Azure.Documents.ServiceInterop.dll, které jsou potřeba při odkazování na projektu DocumentDB s odkazem na balíček DocumentDB Nuget.
  - Pevná možnost používat parametry různých typů funkcí definovaných uživatelem v LINQ. 
  - Podařilo odstranit chybu globálně replikovanou účtů místo, kam Upsert volání jste byli přesměrováni číst umístění místo zápisu umístění.
  - Další metody rozhraní IDocumentClient, které byly chybějící: 
      - Metoda UpsertAttachmentAsync, která trvá mediaStream a možností jako parametry
      - Metoda CreateAttachmentAsync, která používá možnosti jako parametr
      - Metoda CreateOfferQuery, která trvá querySpec jako parametr.
  - Týkající se odtajněných veřejné třídy umístěné v rozhraní IDocumentClient.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - Přidal podporu pro účty databáze více oblastí.
  - Přidaná podpora opakování omezené žádosti.  Uživatel může upravit počet opakování a max prodleva nakonfigurováním vlastnost ConnectionPolicy.RetryOptions.
  - Přidání nového rozhraní IDocumentClient, který definuje podpisy DocumenClient vlastnosti a metody.  Jako součást této změně změní také rozšíření metody vytvořené IQueryable a IOrderedQueryable metod samotného třídy DocumentClient.
  - Přidá možnost konfigurace nastavení ServicePoint.ConnectionLimit pro dané koncový bod DocumentDB Uri.  Umožňuje změnit výchozí hodnotu, která je nastavena na 50 ConnectionPolicy.MaxConnectionLimit.
  - Nepoužívají se IPartitionResolver a provádění.  Podpora IPartitionResolver je nyní zastaralé. Doporučujeme použít na oddíly kolekce vyšší úložiště a výkon.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Přidá přetížení na Uri na základě ExecuteStoredProcedureAsync metodu, která trvá RequestOptions jako parametr.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Čas s přidanou podpoře live (TTL) pro dokumenty.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Stanovit chybu v Nuget balení .NET SDK balení jako součást cloudové služby Azure řešení.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Implementovaná [rozdělený kolekce](documentdb-partition-data.md) a [uživatelem definovaných výkonu úrovně](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Pevné]** Dotazování vyvolá DocumentDB koncového bodu: "System.Net.Http.HttpRequestException: Při kopírování obsahu do datového proudu.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Rozbalené LINQ podporují včetně nové operátory pro stránkování, podmíněných výrazů a oblasti porovnání.
    - Převzít operátor povolit chování vyberte horní v LINQ
    - Operátor CompareTo povolit oblast porovnávání řetězců
    - Podmíněné (?) a coalesce operátory (?)
  - **[Pevné]** ArgumentOutOfRangeException při kombinování modelu projekce s Where-In linq dotaz.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Pevné]** Nejsou-li vybrat poslední výraz poskytovatele LINQ předpokládá, že žádný projekce a vyrobeno vyberte * nesprávně.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Implementace Upsert přidali UpsertXXXAsync metody
 - Zvýšení výkonu pro všechny žádosti o
 - Podpora LINQ zprostředkovatele podmíněné, coalesce a metody CompareTo řetězců
 - **[Pevné]** Zprostředkovatel LINQ--> Implementace obsahuje metoda seznamů generovat stejné SQL jako na IEnumerable a pole
 - **[Pevná]** Stejné HttpRequestMessage BackoffRetryUtility znovu využívá místo abyste vytvářeli novou poznámku na opakování
 - **[Zastaralé]** UriFactory.CreateCollection--> nyní používejte UriFactory.CreateDocumentCollection
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Pevné]** Lokalizace při použití jiných en jazykové verze informace například nl NL atd. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - ID na základě směrování
    - Nové UriFactory Pomocníka pro pomoc s vytváření ID založeny zdroje odkazy
    - Nové přetížení na DocumentClient pořizování v URI
  - Byly přidány IsValid() a IsValidDetailed() v LINQ geografická
  - Rozšířená podpora LINQ zprostředkovatele
    - Zkraťte údaj o **matematické** - Abs ARCCOS, ARCSIN ARCTG Ceiling Cos Exp prostorového uspořádání, protokolu, Log10, Pow, ZAOKROUHLIT, odhlásit, Sin, odmocnina, Tan
    - **Řetězec** - propojit, obsahuje EndsWith IndexOf, počet, ToLower, TrimStart, nahradit, převrátit, TrimEnd StartsWith, podřetězec, ToUpper
    - **Pole** - propojit, obsahuje, počet
    - Operátor **IN**

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Přidaná podpora pro úpravu indexování zásady
    - Nová metoda ReplaceDocumentCollectionAsync v DocumentClient
    - Nová vlastnost IndexTransformationProgress v ResourceResponse<T> sledování procenta průběhu změny zásad indexu
    - DocumentCollection.IndexingPolicy je teď proměnlivých
  - Přidaná podpora prostorové indexování a dotaz
    - Nové Microsoft.Azure.Documents.Spatial obor názvů serializaci/převodu ze sériového tvaru prostorové typy jako čárky a mnohoúhelník
    - Nové třídy SpatialIndex indexování GeoJSON data uložená v DocumentDB
  - **[Pevné]** : generované linq výraz [#38](https://github.com/Azure/azure-documentdb-net/issues/38) dotazu nesprávné SQL

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Počet závislostí na Newtonsoft.Json v5.0.7 
- Změny pro podporu Order By
  - Podpora zprostředkovatele LINQ OrderBy() nebo OrderByDescending()
  - IndexingPolicy podporuje Order By 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Podpora k rozdělování dat pomocí nových HashPartitionResolver a RangePartitionResolver tříd a IPartitionResolver
- DataContract serializace
- Podpora GUID v LINQ poskytovatele
- Podpora UDF v LINQ

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK

> [AZURE.NOTE]
Došlo ke změně název balíčku NuGet mezi náhled a JM. Jsme přesouvána z **Microsoft.Azure.Documents.Client** do **Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- Náhled SDK [zastaralé]

## <a name="release--retirement-dates"></a>Uvolnění a vyřazování webů kalendářních dat
Microsoft poskytovala oznámení alespoň **12 měsíců** před do důchodu SDK k hladce přechod na verzi novější/podporované.

Nové funkce a funkce a optimalizace pouze přidají aktuální SDK, jako takové doporučujeme vždy upgradovat na nejnovější SDK verzi nejdřívější největšímu. 

Každá žádost DocumentDB pomocí vyřazené SDK odmítnuta službou.

> [AZURE.WARNING]
Všechny verze SDK DocumentDB Azure pro .NET před verzí **1.0.0** bude důchodu **29 února 2016**. 
 
<br/>
 
| Verze | Datum vydání | Datum vyřazování webů 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 27 září 2016 |---
| [1.9.5](#1.9.5) | 01 září 2016 |---
| [otázku 1.9.4](#1.9.4) | 24 srpen 2016 |---
| [1.9.3](#1.9.3) | 15 srpen 2016 |---
| [1.9.2](#1.9.2) | 23 červenec 2016 |---
| 1.9.1 | Změněny |---
| 1.9.0 | Změněny |---
| [1.8.0](#1.8.0) | 14 června 2016 |---
| [1.7.1](#1.7.1) | 06 květen 2016 |---
| [1.7.0](#1.7.0) | 26 duben 2016 |---
| [1.6.3](#1.6.3) | 08 duben 2016 |---
| [1.6.2](#1.6.2) | 29 březen 2016 |---
| [1.5.3](#1.5.3) | 19 únor 2016 |---
| [1.5.2](#1.5.2) | 14 prosinec 2015 |---
| [1.5.1](#1.5.1) | 23 listopadu 2015 |---
| [1.5.0](#1.5.0) | 05 říjen 2015 |---
| [1.4.1](#1.4.1) | 25 srpen 2015 |---
| [1.4.0](#1.4.0) | 13 srpen 2015 |---
| [1.3.0](#1.3.0) | 05 srpen 2015 |---
| [1.2.0](#1.2.0) | 06 červenec 2015 |---
| [1.1.0](#1.1.0) | 30 duben 2015 |---
| [1.0.0](#1.0.0) | 08 duben 2015 |---
| [0.9.3-prelease](#0.9.x-preview) | 12 března 2015 | 29 únor 2016
| [0.9.2-prelease](#0.9.x-preview) | Ledna 2015 | 29 únor 2016
| [.9.1-prelease](#0.9.x-preview) | 13 října 2014 | 29 únor 2016
| [0.9.0-prelease](#0.9.x-preview) | 21 srpen 2014 | 29 únor 2016

## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Viz taky

Další informace o DocumentDB najdete v tématu stránka služby [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . 
