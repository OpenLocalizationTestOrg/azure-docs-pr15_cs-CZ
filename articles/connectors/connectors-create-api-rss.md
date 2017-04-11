<properties
pageTitle="RSS | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. Spojnice RSS umožňuje uživatelům publikovat a informačních kanálů položky obnovit. Také umožňuje uživatelům aktivovat operace při vytvoření nové položky publikování k tomuto kanálu."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-rss-connector"></a>Začínáme s konektoru RSS
RSS je formát syndikace Oblíbené web používá pro publikování obrázků často aktualizovaný obsah – jako položky blogu a hlavní zprávy.  Mnoho vydavatele obsahu poskytují informačního kanálu RSS umožníte uživatelům přihlásit se.  Pomocí konektoru RSS k načtení informačních kanálů informace a aktivační událost jsou přenášena po publikování nové položky do informačního kanálu RSS.

>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2015 08 01 náhled schématu verze. 

Můžete začít s vytvářením logiky aplikaci najdete v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce

Konektor RSS mohou sloužit jako akce; má které aktivační. Všechny spojnice podporují dat ve formátu JSON a XML. 

 Konektor RSS obsahuje následující akce nebo které aktivační k dispozici:

### <a name="rss-actions"></a>Akce RSS
Můžete provést tyto akce:

|Akce|Popis|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|Pokud potřebujete všechny informační kanál RSS položky.|
### <a name="rss-triggers"></a>Aktivace RSS
Můžete poslouchat tyto události:

|Aktivační událost | Popis|
|--- | ---|
|Při publikování nová položka informačního kanálu|Spustí pracovní postup, když je publikován nový informační kanál|


## <a name="create-a-connection-to-rss"></a>Vytvoření připojení k technologii RSS

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] Toto připojení můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="reference-for-rss"></a>Přehled technologie RSS
Platí pro verze: 1.0

## <a name="onnewfeed"></a>OnNewFeed
Pokud nová položka informačního kanálu publikovat: spustí pracovní postup, když je publikován nový informační kanál 

```GET: /OnNewFeed``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|Adresa URL kanálu|řetězec|Ano|dotaz|žádná|Adresa url informačního kanálu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|202|Přijaté|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="listfeeditems"></a>ListFeedItems
Seznam všech informačního kanálu sociální sítě položek.: Zobrazí všechny informační kanál RSS položky. 

```GET: /ListFeedItems``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|Adresa URL kanálu|řetězec|Ano|dotaz|žádná|Adresa url informačního kanálu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|202|Přijaté|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="object-definitions"></a>Definice objektů 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [položka kanálu]


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="feeditem"></a>Položka kanálu


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|řetězec|Ano |
|Název|řetězec|Ano |
|obsah|řetězec|Ano |
|odkazy|pole|Ne |
|updatedOn|řetězec|Ne |


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)