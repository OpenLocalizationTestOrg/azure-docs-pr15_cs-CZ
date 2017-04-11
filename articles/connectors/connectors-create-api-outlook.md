<properties
pageTitle="Outlook.com | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. Outlook.com connector umožňuje spravovat pošty, kalendáře a kontakty. Provádět různé akce například odesílat e-maily, naplánovat schůzky, můžete přidat kontakty, atd."
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

# <a name="get-started-with-the-outlookcom-connector"></a>Začínáme s Outlook.com spojnice

Outlook.com connector umožňuje spravovat pošty, kalendáře a kontakty. Provádět různé akce například odesílat e-maily, naplánovat schůzky, můžete přidat kontakty, atd.

>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2015 08 01 náhled schématu verze. 

Můžete začít s vytvářením logiky aplikaci najdete v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce

Konektor Outlook.com mohou sloužit jako akce; má které aktivační. Všechny spojnice podporují dat ve formátu JSON a XML. 

 Konektor Outlook.com obsahuje následující akce nebo které aktivační k dispozici:

### <a name="outlookcom-actions"></a>Akce Outlook.com
Můžete provést tyto akce:

|Akce|Popis|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|Načte e-mailů ve složce|
|[OdeslatE-mail](connectors-create-api-outlook.md#SendEmail)|Odešle e-mailu|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Odstranění e-mailu pomocí id|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|Označí e-mailu jako přečtené|
|[ReplyTo](connectors-create-api-outlook.md#ReplyTo)|Odpovědi na e-mailu|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Načte e-mailové přílohy pomocí id|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Odeslat e-mail s více možnostmi a počkejte, příjemce, aby odpoví zpět jednu z možností|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Odeslat e-mail schválení a počkejte na odpověď příjemce|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Načte kalendáře|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Načte položek z kalendáře|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Vytvoří novou událost|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Načte určité položky z kalendáře|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|Odstraní položky kalendáře|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Částečně aktualizuje položky kalendáře|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Načte složky kontaktů|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Načte kontakty ze složky Kontakty|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Vytvoří nový kontakt|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Načte konkrétní kontakt ze složky Kontakty|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Odstranění kontaktu|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Částečně aktualizuje kontaktu|
### <a name="outlookcom-triggers"></a>Aktivace Outlook.com
Můžete poslouchat tyto události:

|Aktivační událost | Popis|
|--- | ---|
|O události spuštění brzy bude k dispozici|Spustí tok při spuštění nadcházející události|
|Na nový e-mail.|Spustí tok příchod nové e-mailu|
|Na nové položky|Spouštěný při vytvoření nové položky kalendáře|
|Na aktualizací položek|Spouštěný při změně položky kalendáře|


## <a name="create-a-connection-to-outlookcom"></a>Vytvoření připojení k účtu Outlook.com
Vytváření aplikací pro použití logických operátorů s Outlook.com, můžete musíte nejdřív vytvořit **připojení** a pak zadejte podrobnosti pro následující vlastnosti: 

|Vlastnost| Povinné|Popis|
| ---|---|---|
|Tokenu|Ano|Zadejte přihlašovací údaje účtu Outlook.com|
Po vytvoření připojení můžete k provedení akce a poslouchat aktivace popisované v tomto článku.

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] Toto připojení můžete použít v jiných aplikacích pro použití logických operátorů.  

## <a name="reference-for-outlookcom"></a>Odkaz na Outlook.com
Platí pro verze: 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
O události spuštění brzy bude k dispozici: aktivuje tok při spuštění nadcházející události 

```GET: /Events/OnUpcomingEvents``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|dotaz|žádná|Jedinečný identifikátor kalendáře|
|lookAheadTimeInMinutes|celé číslo|Ne|dotaz|15|Čas (v minutách) můžete rovnou vyhledávat nadcházející události|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|202|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněným|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="getemails"></a>GetEmails
Získejte e-mailů: načte e-mailů ve složce 

```GET: /Mail``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|cesta_ke_složce|řetězec|Ne|dotaz|Doručená pošta|Cestu ke složce k načtení e-mailů (výchozí: "Složky Doručená pošta")|
|horní|celé číslo|Ne|dotaz|10|Číslo e-mailů k načtení (výchozí: 10)|
|fetchOnlyUnread|Logická hodnota|Ne|dotaz|PRAVDA|Načtení pouze nepřečtených e-mailech?|
|includeAttachments|Logická hodnota|Ne|dotaz|NEPRAVDA|Pokud nastavena na hodnotu true, přílohy budou také načtená společně s e-mailu|
|searchQuery|řetězec|Ne|dotaz|žádná|Hledání dotazu filtrovat e-mailů|
|Přeskočit|celé číslo|Ne|dotaz|0|Číslo e-mailů přeskočit (výchozí: 0)|
|skipToken|řetězec|Ne|dotaz|žádná|Přejít token k načtení nové stránky|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněným|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="sendemail"></a>OdeslatE-mail
Odeslání e-mailu: odešle e-mailu 

```POST: /Mail``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|e-mailová zpráva| |Ano|textu|žádná|E-mailu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněným|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="deleteemail"></a>DeleteEmail
Odstranění e-mailu: Odstraní e-mailu pomocí id 

```DELETE: /Mail/{messageId}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|messageId|řetězec|Ano|Cesta|žádná|ID e-mailu do odstranit|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněným|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="markasread"></a>MarkAsRead
Označit jako přečtené: e-mailu jako přečtené označí 

```POST: /Mail/MarkAsRead/{messageId}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|messageId|řetězec|Ano|Cesta|žádná|Přečtěte si ID e-mailu do označit jako|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněným|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="replyto"></a>ReplyTo
Odpověď na e-mailu: odpovědi na e-mailu 

```POST: /Mail/ReplyTo/{messageId}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|messageId|řetězec|Ano|Cesta|žádná|ID odpověď na e-mailu|
|Komentář|řetězec|Ano|dotaz|žádná|Komentář odpovědět|
|replyAll|Logická hodnota|Ne|dotaz|NEPRAVDA|Odpovědět všem příjemcům|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněným|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="getattachment"></a>GetAttachment
Získání přílohy: načte e-mailové přílohy pomocí id 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|messageId|řetězec|Ano|Cesta|žádná|ID e-mailu|
|attachmentId|řetězec|Ano|Cesta|žádná|ID na přílohu pro stažení|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněným|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="onnewemail"></a>OnNewEmail
Na nové e-mailech: aktivuje tok příchod nové e-mailu 

```GET: /Mail/OnNewEmail``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|cesta_ke_složce|řetězec|Ne|dotaz|Doručená pošta|E-mailové složky k načtení (výchozí: doručené pošty)|
|k|řetězec|Ne|dotaz|žádná|Příjemců e-mailové adresy|
|z|řetězec|Ne|dotaz|žádná|Z adresy|
|význam|řetězec|Ne|dotaz|Normální|Význam e-mailu (vysoké, Normální, minimum) (výchozí: normální)|
|fetchOnlyWithAttachment|Logická hodnota|Ne|dotaz|NEPRAVDA|Obnovit jenom e-mailové zprávy s přílohou|
|includeAttachments|Logická hodnota|Ne|dotaz|NEPRAVDA|Zahrnutí příloh|
|subjectFilter|řetězec|Ne|dotaz|žádná|Řetězec můžete vyhledávat v předmětu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|202|Přijaté|
|400|BadRequest|
|401|Neoprávněným|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
Odeslání e-mailu s možnostmi: e-mailovou zprávu s více možnostmi a počkejte, příjemce, aby odpoví zpět jednu z možností 

```POST: /mailwithoptions/$subscriptions``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |Ano|textu|žádná|Předplatné žádost o možnosti e-mailu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|201|Vytvoření předplatného|
|400|BadRequest|
|401|Neoprávněným|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="sendapprovalmail"></a>SendApprovalMail
Odeslání e-mailu schválení: Odeslat e-mail schválení a počkejte na odpověď příjemce 

```POST: /approvalmail/$subscriptions``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |Ano|textu|žádná|Předplatné požadavku na schvalování e-mailu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|201|Vytvoření předplatného|
|400|BadRequest|
|401|Neoprávněným|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="calendargettables"></a>CalendarGetTables
Získání kalendáře: načte kalendáře 

```GET: /datasets/calendars/tables``` 

Neexistují žádné parametry pro tento hovor
#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="calendargetitems"></a>CalendarGetItems
Získání události: načte položek z kalendáře 

```GET: /datasets/calendars/tables/{table}/items``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor kalendáře k načtení|
|$filter|řetězec|Ne|dotaz|žádná|Dotaz filtru ODATA omezení počtu položek|
|$orderby|řetězec|Ne|dotaz|žádná|Dotaz ODATA řadit podle určete pořadí položek|
|$skip|celé číslo|Ne|dotaz|žádná|Počet faktur od přeskočit (výchozí = 0)|
|$top|celé číslo|Ne|dotaz|žádná|Maximální počet položek k načtení (výchozí = 256)|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="calendarpostitem"></a>CalendarPostItem
Vytvoření události: vytvoří novou událost 

```POST: /datasets/calendars/tables/{table}/items``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor kalendáře|
|položky| |Ano|textu|žádná|Vytvoření položky kalendáře|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="calendargetitem"></a>CalendarGetItem
Získání události: načte určité položky z kalendáře 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor kalendáře|
|ID|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor načítat položky kalendáře|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
Odstranění události: Odstraní položky kalendáře 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor kalendáře|
|ID|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor položku kalendáře, kterou chcete odstranit|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="calendarpatchitem"></a>CalendarPatchItem
Aktualizace události: částečně aktualizuje položky kalendáře 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor kalendáře|
|ID|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor položku kalendáře, kterou chcete aktualizovat|
|položky| |Ano|textu|žádná|Položky kalendáře aktualizovat|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Na nové položky: spouštěný při vytvoření nové položky kalendáře 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor kalendáře|
|$filter|řetězec|Ne|dotaz|žádná|Dotaz filtru ODATA omezení počtu položek|
|$orderby|řetězec|Ne|dotaz|žádná|Dotaz ODATA řadit podle určete pořadí položek|
|$skip|celé číslo|Ne|dotaz|žádná|Počet faktur od přeskočit (výchozí = 0)|
|$top|celé číslo|Ne|dotaz|žádná|Maximální počet položek k načtení (výchozí = 256)|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
Aktualizace položky: spouštěný při změně položky kalendáře 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor kalendáře|
|$filter|řetězec|Ne|dotaz|žádná|Dotaz filtru ODATA omezení počtu položek|
|$orderby|řetězec|Ne|dotaz|žádná|Dotaz ODATA řadit podle určete pořadí položek|
|$skip|celé číslo|Ne|dotaz|žádná|Počet faktur od přeskočit (výchozí = 0)|
|$top|celé číslo|Ne|dotaz|žádná|Maximální počet položek k načtení (výchozí = 256)|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="contactgettables"></a>ContactGetTables
Získání složky kontaktů: načte složky Kontakty 

```GET: /datasets/contacts/tables``` 

Neexistují žádné parametry pro tento hovor
#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="contactgetitems"></a>ContactGetItems
Získání kontaktů: načte kontakty ze složky Kontakty 

```GET: /datasets/contacts/tables/{table}/items``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor složku Kontakty k načtení|
|$filter|řetězec|Ne|dotaz|žádná|Dotaz filtru ODATA omezení počtu položek|
|$orderby|řetězec|Ne|dotaz|žádná|Dotaz ODATA řadit podle určete pořadí položek|
|$skip|celé číslo|Ne|dotaz|žádná|Počet faktur od přeskočit (výchozí = 0)|
|$top|celé číslo|Ne|dotaz|žádná|Maximální počet položek k načtení (výchozí = 256)|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="contactpostitem"></a>ContactPostItem
Vytvoření kontaktu: vytvoří nový kontakt 

```POST: /datasets/contacts/tables/{table}/items``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor složky kontaktů|
|položky| |Ano|textu|žádná|Kontakt, který chcete vytvořit|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="contactgetitem"></a>ContactGetItem
Získání kontaktů: načte konkrétní kontakt ze složky Kontakty 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor složky kontaktů|
|ID|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor kontaktu k načtení|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="contactdeleteitem"></a>ContactDeleteItem
Odstranit kontakt: Odstraní kontaktu 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor složky kontaktů|
|ID|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor kontakt, který chcete odstranit|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="contactpatchitem"></a>ContactPatchItem
Aktualizovat kontakt: částečně aktualizuje kontaktu 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|tabulky|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor složky kontaktů|
|ID|řetězec|Ano|Cesta|žádná|Jedinečný identifikátor kontakt, který chcete aktualizovat|
|položky| |Ano|textu|žádná|Kontaktní položku, kterou chcete aktualizovat|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="object-definitions"></a>Definice objektů 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [IDictionary [řetězec, objekt]]


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="object"></a>Objekt


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|



### <a name="sendmessage"></a>SendMessage


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Přílohy|pole|Ne |
|Z|řetězec|Ne |
|Kopie|řetězec|Ne |
|Skrytá|řetězec|Ne |
|Předmět|řetězec|Ano |
|Textu|řetězec|Ano |
|Význam|řetězec|Ne |
|IsHtml|Logická hodnota|Ne |
|K|řetězec|Ano |



### <a name="sendattachment"></a>SendAttachment


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|@odata.type|řetězec|Ne |
|Jméno|řetězec|Ano |
|ContentBytes|řetězec|Ano |



### <a name="receivemessage"></a>ReceiveMessage


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|řetězec|Ne |
|IsRead|Logická hodnota|Ne |
|HasAttachment|Logická hodnota|Ne |
|DateTimeReceived|řetězec|Ne |
|Přílohy|pole|Ne |
|Z|řetězec|Ne |
|Kopie|řetězec|Ne |
|Skrytá|řetězec|Ne |
|Předmět|řetězec|Ano |
|Textu|řetězec|Ano |
|Význam|řetězec|Ne |
|IsHtml|Logická hodnota|Ne |
|K|řetězec|Ano |



### <a name="receiveattachment"></a>ReceiveAttachment


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|řetězec|Ano |
|Typ obsahu|řetězec|Ano |
|@odata.type|řetězec|Ne |
|Jméno|řetězec|Ano |
|ContentBytes|řetězec|Ano |



### <a name="digestmessage"></a>DigestMessage


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Předmět|řetězec|Ano |
|Textu|řetězec|Ne |
|Význam|řetězec|Ne |
|Digest|pole|Ano |
|Přílohy|pole|Ne |
|K|řetězec|Ano |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="datasetsmetadata"></a>DataSetsMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|tabulkové|Nedefinováno|Ne |
|objektů BLOB|Nedefinováno|Ne |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|zdroje|řetězec|Ne |
|displayName|řetězec|Ne |
|urlEncoding|řetězec|Ne |
|tableDisplayName|řetězec|Ne |
|tablePluralName|řetězec|Ne |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|zdroje|řetězec|Ne |
|displayName|řetězec|Ne |
|urlEncoding|řetězec|Ne |



### <a name="tablemetadata"></a>TableMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Jméno|řetězec|Ne |
|Název|řetězec|Ne |
|oprávnění ms x|řetězec|Ne |
|x-ms možnosti|Nedefinováno|Ne |
|schéma|Nedefinováno|Ne |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|sortRestrictions|Nedefinováno|Ne |
|filterRestrictions|Nedefinováno|Ne |
|filterFunctions|pole|Ne |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|řaditelnou|Logická hodnota|Ne |
|unsortableProperties|pole|Ne |
|ascendingOnlyProperties|pole|Ne |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|filtrovat|Logická hodnota|Ne |
|nonFilterableProperties|pole|Ne |
|Vlastnost requiredProperties|pole|Ne |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|NotificationUrl|řetězec|Ne |
|Zpráva|Nedefinováno|Ne |



### <a name="messagewithoptions"></a>MessageWithOptions


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Předmět|řetězec|Ano |
|Možnosti|řetězec|Ano |
|Textu|řetězec|Ne |
|Význam|řetězec|Ne |
|Přílohy|pole|Ne |
|K|řetězec|Ano |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|řetězec|Ne |
|zdroje|řetězec|Ne |
|NotificationTyp|řetězec|Ne |
|notificationUrl|řetězec|Ne |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|NotificationUrl|řetězec|Ne |
|Zpráva|Nedefinováno|Ne |



### <a name="approvalmessage"></a>ApprovalMessage


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Předmět|řetězec|Ano |
|Možnosti|řetězec|Ano |
|Textu|řetězec|Ne |
|Význam|řetězec|Ne |
|Přílohy|pole|Ne |
|K|řetězec|Ano |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|SelectedOption|řetězec|Ne |



### <a name="tableslist"></a>TablesList


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="table"></a>Tabulky


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Jméno|řetězec|Ne |
|DisplayName|řetězec|Ne |



### <a name="item"></a>Položky


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ItemInternalId|řetězec|Ne |



### <a name="calendaritemslist"></a>CalendarItemsList


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="calendaritem"></a>CalendarItem


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ItemInternalId|řetězec|Ne |



### <a name="contactitemslist"></a>ContactItemsList


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="contactitem"></a>ContactItem


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ItemInternalId|řetězec|Ne |



### <a name="datasetslist"></a>DataSetsList


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="dataset"></a>Datové sady


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Jméno|řetězec|Ne |
|DisplayName|řetězec|Ne |


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)