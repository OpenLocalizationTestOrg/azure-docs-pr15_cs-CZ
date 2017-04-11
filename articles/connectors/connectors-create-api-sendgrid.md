<properties
pageTitle="SendGrid | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. Poskytovatel připojení SendGrid vám umožní poslat e-mailu a spravovat seznamy příjemců."
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

# <a name="get-started-with-the-sendgrid-connector"></a>Začínáme s SendGrid spojnice

Poskytovatel připojení SendGrid vám umožní poslat e-mailu a spravovat seznamy příjemců.

>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2015 08 01 náhled schématu verze. 

Můžete začít s vytvářením logiky aplikaci najdete v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce

Konektor SendGrid mohou sloužit jako akce; má které aktivační. Všechny spojnice podporují dat ve formátu JSON a XML. 

 Konektor SendGrid má k dispozici následující akce: Existuje žádné aktivačních událostí.

### <a name="sendgrid-actions"></a>SendGrid akce
Můžete provést tyto akce:

|Akce|Popis|
|--- | ---|
|[OdeslatE-mail](connectors-create-api-sendgrid.md#sendemail)|Odešle e-mailů pomocí rozhraní API SendGrid (omezené příjemcům 10 000)|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|Přidání jednotlivých příjemce do seznamu příjemců|


## <a name="create-a-connection-to-sendgrid"></a>Vytvoření připojení k SendGrid
Vytváření aplikací pro použití logických operátorů s SendGrid, můžete musíte nejdřív vytvořit **připojení** a pak zadejte podrobnosti pro následující vlastnosti: 

|Vlastnost| Povinné|Popis|
| ---|---|---|
|ApiKey|Ano|Zadejte svůj klíč rozhraní Api SendGrid|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] Toto připojení můžete použít v jiných aplikacích pro použití logických operátorů.

Po vytvoření připojení můžete k provedení akce a poslouchat aktivace popisované v tomto článku.

## <a name="reference-for-sendgrid"></a>Odkaz pro SendGrid
Platí pro verze: 1.0

## <a name="sendemail"></a>OdeslatE-mail
Odeslání e-mailu: odešle e-mailů pomocí rozhraní API SendGrid (omezené příjemcům 10 000) 

```POST: /api/mail.send.json``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|žádost o| |Ano|textu|žádná|E-mailové zprávy|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|429|Příliš mnoho požadavek|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="addrecipienttolist"></a>AddRecipientToList
Přidat příjemce do seznamu: přidání jednotlivých příjemce do seznamu příjemců 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|listId|řetězec|Ano|Cesta|žádná|Jedinečné id seznamu příjemců|
|recipientId|řetězec|Ano|Cesta|žádná|Jedinečné id příjemce|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="object-definitions"></a>Definice objektů 

### <a name="emailrequest"></a>EmailRequest


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|z|řetězec|Ano |
|že FromName|řetězec|Ne |
|k|řetězec|Ano |
|toname|řetězec|Ne |
|předmět|řetězec|Ano |
|textu|řetězec|Ano |
|ishtml|Logická hodnota|Ne |
|kopie|řetězec|Ne |
|ccname|řetězec|Ne |
|Skrytá|řetězec|Ne |
|bccname|řetězec|Ne |
|ReplyTo|řetězec|Ne |
|Datum|řetězec|Ne |
|záhlaví|řetězec|Ne |
|soubory|pole|Ne |
|názvy souborů|pole|Ne |



### <a name="emailresponse"></a>EmailResponse


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|zpráva|řetězec|Ne |



### <a name="recipientlists"></a>RecipientLists


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|seznamy|pole|Ne |



### <a name="recipientlist"></a>RecipientList


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|celé číslo|Ne |
|Jméno|řetězec|Ne |
|recipient_count|celé číslo|Ne |



### <a name="recipients"></a>Příjemci


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|příjemci|pole|Ne |



### <a name="recipient"></a>Příjemce


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|e-mailu|řetězec|Ne |
|Příjmení|řetězec|Ne |
|křestní_jméno|řetězec|Ne |
|ID|řetězec|Ne |


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiku](../app-service-logic/app-service-logic-create-a-logic-app.md)