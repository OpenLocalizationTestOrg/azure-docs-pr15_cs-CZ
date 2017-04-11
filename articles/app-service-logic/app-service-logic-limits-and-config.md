<properties
    pageTitle="Použití logických operátorů aplikace limity a konfiguraci | Microsoft Azure"
    description="Základní informace o limitech služby a konfigurace hodnoty dostupné pro použití logických operátorů aplikace."
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>Použití logických operátorů aplikace limity a konfiguraci

Tady jsou informace o momentální omezení a podrobnosti o konfiguraci aplikace Azure použití logických operátorů:

## <a name="limits"></a>Omezení

### <a name="http-request-limits"></a>Limity žádost HTTP

Toto jsou limity pro jeden HTTP žádost a/nebo spojnice hovoru

#### <a name="timeout"></a>Časový limit

|Jméno|Limit|Poznámky|
|----|----|----|
|Žádost o vypršení časového limitu|1 minuta|[Asynchronní vzorku](app-service-logic-create-api-app.md) nebo s [do smyčka](app-service-logic-loops-and-scopes.md) lze vyrovnávat podle potřeby|

#### <a name="message-size"></a>Velikost zprávy

|Jméno|Limit|Poznámky|
|----|----|----|
|Velikost zprávy|50 MB|Některé spojnice a rozhraní API nemusí podporovat 50 MB.  Žádost o aktivační událost podporuje až 25MB|
|Limit vyhodnocení výrazu|131 072 znaky|`@concat()`, `@base64()`, `string` nemůže být delší než|

#### <a name="retry-policy"></a>Opakovat zásad

|Jméno|Limit|Poznámky|
|----|----|----|
|Novými pokusy o aktualizaci|4|Můžete nakonfigurovat [Opakovat parametr zásad](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Maximální zpoždění při opakování|1 hodinu|Můžete nakonfigurovat [Opakovat parametr zásad](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Min zpoždění při opakování|20 minut|Můžete nakonfigurovat [Opakovat parametr zásad](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

### <a name="run-duration-and-retention"></a>Spuštění dobu trvání a uchovávání informací

Jedná se o omezení aplikace jednoho logiky spustit.

|Jméno|Limit|Poznámky|
|----|----|----|
|Spuštění doba trvání|90 dní||
|Uchovávání informací úložiště|90 dní|Toto je z spuštění počáteční čas|
|Min intervalu opakování|15 sec||
|Max intervalu opakování|500 dnů||


### <a name="looping-and-debatching-limits"></a>Opakování a debatching omezení

Jedná se o omezení aplikace jednoho logiky spustit.

|Jméno|Limit|Poznámky|
|----|----|----|
|ForEach položek|5 000|[Akce dotazu](../connectors/connectors-native-query.md) můžete filtrovat větší matice v případě potřeby|
|Až iterací|10 000||
|SplitOn položek|5 000||
|ForEach paralelismus|20|Můžete nastavit, aby postupné foreach přidáním `"operationOptions": "Sequential"` k `foreach` akce|


### <a name="throughput-limits"></a>Limity výkon

Toto jsou limity pro instanci aplikace jednoho logiky. 

|Jméno|Limit|Poznámky|
|----|----|----|
|Aktivace za sekundu|100|Distribuovat pracovních postupů ve více aplikacích v případě potřeby|

### <a name="definition-limits"></a>Definice omezení

Toto jsou limity pro definici aplikace jednoho logiku.

|Jméno|Limit|Poznámky|
|----|----|----|
|Akce v ForEach|1|Můžete přidat vnořené pracovních postupů rozšiřte to podle potřeby|
|Akce za pracovního postupu|60|Můžete přidat vnořené pracovních postupů rozšiřte to podle potřeby|
|Povolené akce vnoření hloubku|5|Můžete přidat vnořené pracovních postupů rozšiřte to podle potřeby|
|Toky jednotlivých oblastech jedno předplatné|1 000||
|Aktivace za pracovního postupu|10||
|Maximální počet znaků za výraz|čísla 8 192||
|Max `trackedProperties` velikost písmem|16,000|
|`action`/`trigger`Název omezení|80||
|`description`omezení délky|256||
|`parameters`limit|50||
|`outputs`limit|10||

## <a name="configuration"></a>Konfigurace

### <a name="ip-address"></a>IP adresa

Volání z [spojnice](../connectors/apis-list.md) budou pocházet z na IP adresu zadanou níže.

Volání z app logiky přímo (tedy prostřednictvím [protokolu HTTP](../connectors/connectors-native-http.md) nebo [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)) mohou pocházet z kteréhokoli [Rozsahy Azure Datacentra IP adres](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

|Použití logických operátorů aplikace oblast|Odchozích IP|
|-----|----|
|Austrálie východ|40.126.251.213|
|Austrálie jihovýchodní|40.127.80.34|
|Brazílie jih|191.232.38.129|
|Centrální Indie|104.211.98.164|
|Centrální USA|40.122.49.51|
|Východní Asie|23.99.116.181|
|Východní USA|191.237.41.52|
|Východní USA 2|104.208.233.100|
|Japonsko východ|40.115.186.96|
|Japonsko západní|40.74.130.77|
|Severní centrální USA|65.52.218.230|
|Severní Evropě|104.45.93.9|
|Jižní centrální USA|104.214.70.191|
|Jihovýchodní Asie|13.76.231.68|
|Jižní Indie|104.211.227.225|
|Západní Evropě|40.115.50.13|
|Západní Indie|104.211.161.203|
|Západní USA|104.40.51.248|


## <a name="next-steps"></a>Další kroky  

- Začínáme s aplikacemi jiných použití logických operátorů, postupujte podle kurz [Vytvoření logiky aplikace](app-service-logic-create-a-logic-app.md) .  
- [Zobrazení společných příklady a scénáře](app-service-logic-examples-and-scenarios.md)
- [Automatizace firemních procesů pomocí aplikace logiky](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Zjistěte, jak integrovat systémů s aplikacemi jiných logiky](http://channel9.msdn.com/Events/Build/2016/P462)