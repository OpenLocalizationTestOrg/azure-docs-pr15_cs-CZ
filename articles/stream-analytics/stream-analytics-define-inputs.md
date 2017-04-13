<properties
    pageTitle="Datové připojení: datových proudů vstupní hodnoty z toku události | Microsoft Azure"
    description="Informace o nastavení datového připojení k toku analýzy s názvem "vstupy". Zadávání obsahuje toku dat z události a taky odkazují na data."
    keywords="datového proudu, datové připojení, toku události"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Připojení dat: Další informace o datového proudu vstupní hodnoty z událostí pro analýzy toku

Datové připojení k toku analýzy je datového proudu událostí ze zdroje dat. Tento postup se nazývá "vstup." Technologie pro analýzu toku má prvotřídní integrace se službou Azure toku zdrojů centrální události, IoT centrální a objektů Blob ukládání dat, který může být ze stejného nebo jiného Azure předplatného jako práce analýzy.

## <a name="data-input-types-data-stream-and-reference-data"></a>Typy vstupních dat: datům datového proudu a funkce pro odkazy
Jak dat se posune ke zdroji dat, je využívané úlohy toku analýzy a zpracovávají v reálném čase. Zadávání jsou rozdělen do dvou různých typů: data vysílat vstupů a odkazují na data vstupů.

### <a name="data-stream-inputs"></a>Zadávání toku dat
Toku dat je neomezené řadu událostí chodit určitou dobu. Úlohy analýzy toku musí obsahovat alespoň jeden dat toku předávat na vstupu spotřebované množství a transformaci podle projektu. Úložiště objektů BLOB rozbočovače událostí a IoT rozbočovače jsou podporované jako vstupní zdroje dat toku. Událost rozbočovače slouží k shromáždit datových proudů událost z víc zařízení a služby, jako je aktivitách sociálních médií, informace o burzovní obchodní nebo data z senzory. IoT rozbočovače jsou optimalizováno pro shromáždit data od připojená zařízení v Internetu věcí (IoT) scénáře.  Úložiště objektů BLOB mohou sloužit jako vstupní zdroje pro ingesting hromadné dat pomocí datového proudu.  

### <a name="reference-data"></a>Přehled dat
Toku analýzy podporuje druhého typu vstupní jmenoval referenční data. Toto je pomocné data, která je statické nebo pomalu změny v čase a se obvykle používá k provádění korelace a look-ups. Úložiště objektů Blob Azure momentálně pouze podporované vstupní zdrojem dat odkaz. Odkaz objektů BLOB zdroje dat se omezuje velikosti 100MB.
Naučte se vytvářet odkaz zadávání dat, najdete v článku [Použití referenčních dat](stream-analytics-use-reference-data.md)  

## <a name="create-a-data-stream-input-with-an-event-hub"></a>Vytvoření zadávání toku dat s rozbočovači události

[Azure události rozbočovače](https://azure.microsoft.com/services/event-hubs/) jsou velmi scalable publikovat-přihlášení k odběru událostí ingestor. Miliony události sekundu, získáte tak, aby se daly procesu a analyzovat větší dat vytvořené pomocí připojení zařízení nebo aplikací. Je jednou z nejčastěji používaných vstupů pro analýzy proudu. Rozbočovače událostí a technologie pro analýzu toku společně zákazníkům komplexní řešení pro analýzy reálném čase. Událost rozbočovače umožňují zákazníkům kanálu události na Azure v reálném čase a technologie pro analýzu toku úlohy můžete zpracování v reálném čase. Zákazníci můžete třeba poslat rozbočovače události web kliknutí, senzor naměřené hodnoty, online protokolu událostí a vytvořte úlohy toku Analytics pro účely rozbočovače událost jako vstupní datových proudů filtry reálném čase, agregaci a srovnávací.

Je důležité mít na paměti, že je výchozí časové razítko událostí pocházejících z rozbočovače událostí v toku analýzy zajištění události v centrální události, která je EventEnqueuedUtcTime časové razítko. Zpracování dat pomocí datového proudu použití časového razítka v datové události, musí být použita [Časové razítko podle](https://msdn.microsoft.com/library/azure/dn834998.aspx) klíčových slov.

### <a name="consumer-groups"></a>Zákaznické skupiny

Každý vstup toku analýzy události centrální by měl být nakonfigurované pro mít vlastní skupinu příjemce. Když úlohy obsahuje vlastní spojení nebo více vstupy, mohou být čtena nějakou zpětnou čtečkou víc proudu, které ovlivňuje počet čteček ve skupině jednoho příjemce. Chcete-li předejít překročení limitu události centrální 5 čtenářů skupinu spotř na oddíl, je nejvhodnější určit spotř skupinu pro jednotlivé úlohy toku analýzy. Všimněte si, že je také maximálně 20 spotř skupiny podle centrální události. Podrobnosti najdete v tématu [Průvodce rozbočovače programování událostí](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-event-hub-as-an-input-data-stream"></a>Konfigurace události centrální jako toku zadávání dat

Následující tabulka uvádí jednotlivých vlastností v případě, že centrální vstupní pás karet se její popis:

| NÁZEV VLASTNOSTI | POPIS |
|------|------|
| Zadávání Alias | Popisný název, který se použije v dotazu úlohy neodkazuje tento vstup |
| Služba Bus Namespace | Obor názvů Bus služby je kontejner pro sadu zpráv entity. Po vytvoření nové události centrální vytvořili jste obor Bus služby. |
| Centrální události | Název rozbočovače události při zadávání. |
| Název zásady centrální události | Zásady sdílený přístup, který lze vytvořit na kartě událost centrální konfigurace. Každého zásadu sdílené přístupu budou mít název, oprávnění, která nastavíte, a přístupové klávesy. |
| Klíč zásad centrální události | Sdílené přístupové klávesy slouží k ověření přístup k oboru Bus služby. |
| Událost centrální spotř skupiny (volitelné) | Zákaznické skupině jedí data z centra události. Pokud není zadán, bude toku analýzy úlohy používat skupině výchozí spotř jedí data z centra události. Doporučujeme používat odlišné spotř skupiny pro jednotlivé úlohy toku analýzy. |
| Formát serializace události | Aby zkontrolovala, jestli že svoje dotazy práce podle vašich představ, analýzy toku je potřeba vědět, který formát serializace (JSON, CSV nebo Avro) používáte pro příchozí datových proudů. |
| Kódování | Jediný podporovaný formát kódování UTF-8 je v současné době. |

Pokud data přichází z centrální události zdroje, můžete využít k několika pole metadat v dotazu toku analýzy. Následující tabulka uvádí seznam polí a jejich popis.

| VLASTNOST | POPIS |
|------|------|
| EventProcessedUtcTime | Datum a čas zpracování události podle analýzy proudu. |
| EventEnqueuedUtcTime | Datum a čas, který událost přijala rozbočovače události. |
| ID oddílu | ID od nuly oddílu pro zadávání adaptér. |

Můžete zadat například následující dotaz:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-an-iot-hub-data-stream-input"></a>Vytvoření IoT centrální vstup toku dat

Azure centrální Iot je velmi scalable publikovat-přihlášení k odběru událostí ingestor optimalizovaná pro IoT scénáře.
Je důležité mít na paměti, že je výchozí časové razítko událostí pocházejících z IoT rozbočovače v toku analýzy zajištění události v centrální IoT, což je EventEnqueuedUtcTime časové razítko. Zpracování dat pomocí datového proudu použití časového razítka v datové události, musí být použita [Časové razítko podle](https://msdn.microsoft.com/library/azure/dn834998.aspx) klíčových slov.

> [AZURE.NOTE] Pouze zprávám odeslaným pomocí vlastnost DeviceClient lze zpracovat.

### <a name="consumer-groups"></a>Zákaznické skupiny

Každý vstup toku analýzy IoT centrální by měl být nakonfigurované pro mít vlastní skupinu příjemce. Když úlohy obsahuje vlastní spojení nebo více vstupy, mohou být čtena nějakou zpětnou čtečkou víc proudu, které ovlivňuje počet čteček ve skupině jednoho příjemce. Chcete-li předejít překročení limitu IoT centrální 5 čtenářů skupinu spotř na oddíl, je nejvhodnější určit spotř skupinu pro jednotlivé úlohy toku analýzy.

### <a name="configure-iot-hub-as-an-data-stream-input"></a>Nakonfigurujte Centrum IoT jako datového proudu při zadávání

Následující tabulka uvádí jednotlivých vlastností na kartě IoT centrální zadávání s její popis:

| NÁZEV VLASTNOSTI | POPIS |
|------|------|
| Zadávání Alias | Popisný název, který se použije v dotazu úlohy neodkazuje tyto vstupní. |
| Rozbočovač IoT | Rozbočovači IoT je kontejner pro sadu zpráv entity. |
| Koncový bod | Název koncového bodu IoT centrální. |
| Název zásady sdílený přístup | Zásady sdílený přístup udělit přístup k centru IoT. Každého zásadu sdílené přístupu budou mít název, oprávnění, která nastavíte, a přístupové klávesy. |
| Sdílené přístupová klávesa zásad | Sdílené přístupové klávesy slouží k ověření přístup k centru IoT. |
| Skupina spotř (nepovinné) | Zákaznické skupině jedí data z centra IoT. Pokud není zadán, bude toku analýzy úlohy jedí data z centra IoT používat skupině výchozí příjemce. Doporučujeme používat odlišné spotř skupiny pro jednotlivé úlohy toku analýzy. |
| Formát serializace události | Aby zkontrolovala, jestli že svoje dotazy práce podle vašich představ, analýzy toku je potřeba vědět, který formát serializace (JSON, CSV nebo Avro) používáte pro příchozí datových proudů. |
| Kódování | Jediný podporovaný formát kódování UTF-8 je v současné době. |

Pokud data přichází z centrální IoT zdroje, můžete využít k několika pole metadat v dotazu toku analýzy. Následující tabulka uvádí seznam polí a jejich popis.

| VLASTNOST | POPIS |
|------|------|
| EventProcessedUtcTime | Datum a čas, který zpracování události. |
| EventEnqueuedUtcTime | Datum a čas, který událost přijala centru IoT. |
| ID oddílu | ID od nuly oddílu pro zadávání adaptér. |
| IoTHub.MessageId | Slouží k sladit obousměrnou komunikaci v centrální IoT. |
| IoTHub.CorrelationId | Používá k odpovědi na zprávy a názoru v centrální IoT. |
| IoTHub.ConnectionDeviceId | Ověření id slouží k odeslat tuto zprávu razítko servicebound zpráv IoT centrální. |
| IoTHub.ConnectionDeviceGenerationId | GenerationId ověřené zařízení používané k odeslat tuto zprávu razítko servicebound zpráv IoT centrální. |
| IoTHub.EnqueuedTime | Čas přijetí zprávy tak, že IoT centrální. |
| IoTHub.StreamId | Vlastnost vlastní události přidané zařízení odesílatele. |

## <a name="create-a-blob-storage-data-stream-input"></a>Vytváření zadávání toku dat úložiště objektů Blob

Úložiště objektů Blob scénáře s velkým množstvím Nestrukturovaná data uložená v cloudu, nabízí efektivní a scalable řešení. Data v [úložišti objektů Blob](https://azure.microsoft.com/services/storage/blobs/) obecně považována dat "at zbývající", ale může být zpracována jako toku dat pomocí analýzy proudu. Jeden scénář běžné vstupy úložiště objektů Blob s toku analýzy je zpracování protokolu, kde telemetrie se zaznamenává ze systému a je potřeba analyzovat a zpracování extrahovat smysluplné data.

Je důležité mít na paměti, že je výchozí časové razítko událostí úložiště objektů Blob v toku analýzy časového razítka, které *isBlobLastModifiedUtcTime*naposledy změnil objektů blob. Zpracování dat pomocí datového proudu použití časového razítka v datové události, musí být použita [Časové razítko podle](https://msdn.microsoft.com/library/azure/dn834998.aspx) klíčových slov.

Navíc nezapomeňte, že CSV formátované vstupů **vyžadují** řádek záhlaví definujte pole pro uvedenou množinu dat. Další pole záhlaví řádků musí být **jedinečný**.

> [AZURE.NOTE] Technologie pro analýzu toku podporuje přidávání obsahu do existující objektů blob. Toku analýzy budou zobrazit pouze objektů blob jednou a změny provést po nebudou zpracovány tento pro čtení. Doporučený postup je jednou uložit všechna data a přidávat k úložišti objektů blob dalších událostí.

Následující tabulka uvádí všech vlastností v zadávání karta úložiště objektů Blob s její popis:

<table>
<tbody>
<tr>
<td>NÁZEV VLASTNOSTI</td>
<td>POPIS</td>
</tr>
<tr>
<td>Zadávání Alias</td>
<td>Popisný název, který se použije v dotazu úlohy neodkazuje tyto vstupní.</td>
</tr>
<tr>
<td>Úložiště účtu</td>
<td>Název účtu úložiště, kde se nacházejí objektů blob soubory.</td>
</tr>
<tr>
<td>Klíč účtu úložiště</td>
<td>Řeknu klávesu přidružené k účtu úložiště.</td>
</tr>
<tr>
<td>Kontejner úložiště
</td>
<td>Kontejnery obsahují logické seskupení pro objekty BLOB uložené ve službě objektů Blob Microsoft Azure. Když nahrajete objektů blob ke službě objektů Blob, je nutné zadat kontejner pro tento objekt.</td>
</tr>
<tr>
<td>Cesta předpona vzorek [volitelné]</td>
<td>Cesta k souboru používaných k vyhledání vaší objektů BLOB v zadaném kontejneru.
V rámci cesty můžete určit jeden nebo více instance následující 3 proměnné:<BR>{date}, {čas}<BR>{oddíl}<BR>Příklad 1: cluster1/protokoly / {dat} / {čas} / {oddílů}<BR>Příklad 2: cluster1/protokoly / {dat}<P>Všimněte si, že "*" není povolené hodnoty pro pathprefix. Lze použít pouze platné <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">znaky objektů blob Azure</a> .</td>
</tr>
<tr>
<td>Formát Datum [volitelné]</td>
<td>Pokud token datum používá v parametru path předponu, můžete vybrat formát data, ve kterém jsou uspořádány souborů. Příklad: Rrrr/MM/DD</td>
</tr>
<tr>
<td>Formát času [volitelné]</td>
<td>Pokud je token čas použitý v parametru path předponu, zadejte formát času, ve kterém jsou uspořádány souborů. Aktuálně podporované hodnota je pouze HH.</td>
</tr>
<tr>
<td>Formát serializace události</td>
<td>Aby zkontrolovala, jestli že svoje dotazy práce podle vašich představ, analýzy toku je potřeba vědět, který formát serializace (JSON, CSV nebo Avro) používáte pro příchozí datových proudů.</td>
</tr>
<tr>
<td>Kódování</td>
<td>CSV a JSON UTF-8 je jediný podporovaný formát kódování v současné době.</td>
</tr>
<tr>
<td>Oddělovače</td>
<td>Technologie pro analýzu toku podporuje několik běžných oddělovače serializaci dat ve formátu CSV. Podporované hodnoty jsou čárku, středník, místa, tab a svislá čára.</td>
</tr>
</tbody>
</table>

Když dat pocházejících z prameni úložiště objektů Blob, můžete přistupovat k několika pole metadat dotazu toku analýzy. Následující tabulka uvádí seznam polí a jejich popis.

| VLASTNOST | POPIS |
|------|------|
| BlobName | Název vstupní blob událost pochází z. |
| EventProcessedUtcTime | Datum a čas zpracování události podle analýzy proudu. |
| BlobLastModifiedUtcTime | Datum a čas poslední změny objektů blob. |
| ID oddílu | ID od nuly oddílu pro zadávání adaptér. |

Můžete zadat například následující dotaz:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````


## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
Jste se naučili informace o možnostech připojení dat v Azure pro analýzy toku úlohy. Další informace o toku analýzy, najdete tady:

- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
