<properties
pageTitle="Wunderlist | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. Wunderlist poskytují úkol seznamu a úkol vedoucímu pomoct ostatním lidem jejich všechno zvládnout.  Jestli sdílíte seznam potravin nejbližších mapou, práci na projektu nebo plánování dovolené, Wunderlist usnadňuje zachytit, sdílení a dokončete svůj to¬dos. Wunderlist okamžitě synchronizuje mezi telefonu, tabletu a počítač, a proto k všemi vašimi úkoly mohli dostat odkudkoli."
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

# <a name="get-started-with-the-wunderlist-connector"></a>Začínáme s Wunderlist spojnice

Wunderlist poskytují úkol seznamu a úkol vedoucímu pomoct ostatním lidem jejich všechno zvládnout.  Jestli sdílíte seznam potravin nejbližších mapou, práci na projektu nebo plánování dovolené, Wunderlist usnadňuje zachytit, sdílení a dokončete svůj to¬dos. Wunderlist okamžitě synchronizuje mezi telefonu, tabletu a počítač, a proto k všemi vašimi úkoly mohli dostat odkudkoli.

>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2015 08 01 náhled schématu verze. 

Můžete začít s vytvářením logiky aplikaci najdete v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce

Konektor Wunderlist mohou sloužit jako akce; má které aktivační. Všechny spojnice podporují dat ve formátu JSON a XML. 

 Konektor Wunderlist obsahuje následující akce nebo které aktivační k dispozici:

### <a name="wunderlist-actions"></a>Wunderlist akce
Můžete provést tyto akce:

|Akce|Popis|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|Načtení seznamů přidruženého k vašemu účtu.|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|Vytvoření seznamu.|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|Načtení úkoly z konkrétního seznamu.|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|Vytvoření úkolu|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|Načtení dílčích úkolů z konkrétního seznamu nebo z určitého úkolu.|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|Vytvoření dílčích úkolů v rámci konkrétního úkolu|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|Vyhledávání poznámek pro určitého seznamu nebo s konkrétním úkolem.|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|Přidání poznámky k konkrétního úkolu|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|Načtení úloh komentáře pro konkrétní seznam nebo s konkrétním úkolem.|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|Přidat komentář s konkrétním úkolem|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|Načtení připomenutí pro konkrétní seznam nebo s konkrétním úkolem.|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|Nastavení připomenutí.|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|Získání souborů pro konkrétní seznam nebo s konkrétním úkolem.|
|[GetList](connectors-create-api-wunderlist.md#getlist)|Načte z určitého seznamu|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|Odstranění seznamu|
|[UpdateList](connectors-create-api-wunderlist.md#updatelist)|Aktualizace z určitého seznamu|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|Načte konkrétního úkolu|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|Aktualizuje konkrétního úkolu|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|Odstraní konkrétního úkolu|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|Načte konkrétní dílčích úkolů|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|Aktualizuje konkrétní dílčích úkolů|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|Odstraní konkrétní dílčích úkolů|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|Načtení určité poznámky|
|[SprávcePoznámka](connectors-create-api-wunderlist.md#updatenote)|Aktualizace určité poznámky|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|Odstranění určité poznámky|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|Načtení komentář konkrétního úkolu|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|Aktualizace konkrétní připomenutí|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|Odstranění určité připomenutí|
### <a name="wunderlist-triggers"></a>Aktivace Wunderlist
Můžete poslouchat tyto události:

|Aktivační událost | Popis|
|--- | ---|
|Pokud je úkol termín|Spustí nový tok, když je termín splnění úkolu v seznamu|
|Vytvoření nového úkolu|Spustí nový tok při vytvoření nového úkolu v seznamu|
|Pokud dojde k připomenutí|Spustí nový tok, když nastane připomenutí|


## <a name="create-a-connection-to-wunderlist"></a>Vytvoření připojení k Wunderlist
Vytváření aplikací pro použití logických operátorů s Wunderlist, můžete musíte nejdřív vytvořit **připojení** a pak zadejte podrobnosti pro následující vlastnosti: 

|Vlastnost| Povinné|Popis|
| ---|---|---|
|Tokenu|Ano|Pověření Wunderlist|
Po vytvoření připojení můžete k provedení akce a poslouchat aktivace popisované v tomto článku. 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] Toto připojení můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="reference-for-wunderlist"></a>Odkaz pro Wunderlist
Platí pro verze: 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
Pokud je úkol termín splnění: aktivuje nový tok po splatnosti úkolu v seznamu 

```GET: /trigger/tasksdue``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|


## <a name="triggertasknew"></a>TriggerTaskNew
Když se vytvoří nový úkol: aktivuje nový tok při vytvoření nového úkolu v seznamu 

```GET: /trigger/tasksnew``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|


## <a name="triggerreminder"></a>TriggerReminder
Pokud dojde k připomínání: spustí nový tok, když nastane připomenutí 

```GET: /trigger/reminders``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|
|TASK_ID|celé číslo|Ne|dotaz|žádná|Pole číslo ID úkolu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|


## <a name="retrievelists"></a>RetrieveLists
Získání seznamy: načtení seznamů přidruženého k vašemu účtu. 

```GET: /lists``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="createlist"></a>CreateList
Vytvoření seznamu: vytvoření seznamu. 

```POST: /lists``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|příspěvek| |Ano|textu|žádná|Vytvoření nového seznamu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|
|Výchozí|Operace se nezdařila.|


## <a name="listtasks"></a>ListTasks
Získání úkoly: načíst z konkrétního seznamu úkolů. 

```GET: /tasks``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|
|dokončení|Logická hodnota|Ne|dotaz|žádná|Dokončení|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="createtask"></a>CreateTask
Vytvoření úkolu: vytvoření úkolu 

```POST: /tasks``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|příspěvek| |Ano|textu|žádná|Vytvořit nový úkol|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|201|Vytvoření|


## <a name="listsubtasks"></a>ListSubTasks
Získání dílčí úkoly: načíst dílčích úkolů z konkrétního seznamu nebo z určitého úkolu. 

```GET: /subtasks``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|
|TASK_ID|celé číslo|Ne|dotaz|žádná|Pole číslo ID úkolu|
|dokončení|Logická hodnota|Ne|dotaz|žádná|Dokončení|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="createsubtask"></a>CreateSubTask
Vytvoření dílčího úkolu: Vytvoření dílčího úkolu v rámci konkrétního úkolu 

```POST: /subtasks``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|příspěvek| |Ano|textu|žádná|Vytvořit nové dílčích úkolů|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|201|Vytvoření|


## <a name="listnotes"></a>ListNotes
Získání poznámky: vyhledávání poznámek pro určitého seznamu nebo s konkrétním úkolem. 

```GET: /notes``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|
|TASK_ID|celé číslo|Ne|dotaz|žádná|Pole číslo ID úkolu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="createnote"></a>CreateNote
Vytvoření poznámky: Přidání poznámky k konkrétního úkolu 

```POST: /notes``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|příspěvek| |Ano|textu|žádná|Vytvořit novou poznámku|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|201|Vytvoření|


## <a name="listcomments"></a>ListComments
Získání úloh komentáře: načtení úloh komentáře pro konkrétní seznam nebo s konkrétním úkolem. 

```GET: /task_comments``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|
|TASK_ID|celé číslo|Ne|dotaz|žádná|Pole číslo ID úkolu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="createcomment"></a>CreateComment
Přidat poznámku k úkolu: Přidat komentář s konkrétním úkolem 

```POST: /task_comments``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|příspěvek| |Ano|textu|žádná|Vytvořit novou poznámku úkolu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|201|Vytvoření|


## <a name="retrievereminders"></a>RetrieveReminders
Nastavení připomenutí: načíst připomenutí pro konkrétní seznam nebo s konkrétním úkolem. 

```GET: /reminders``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|
|TASK_ID|celé číslo|Ne|dotaz|žádná|Pole číslo ID úkolu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="createreminder"></a>CreateReminder
Nastavení připomenutí: nastavení připomenutí. 

```POST: /reminders``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|příspěvek| |Ano|textu|žádná|Vytvořit nové připomenutí|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|
|Výchozí|Operace se nezdařila.|


## <a name="retrievefiles"></a>RetrieveFiles
Získání souborů: načíst soubory pro konkrétní seznam nebo s konkrétním úkolem. 

```GET: /files``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|
|TASK_ID|celé číslo|Ne|dotaz|žádná|Pole číslo ID úkolu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace povedla|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="getlist"></a>GetList
Získání seznamu: načte z určitého seznamu 

```GET: /lists/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|žádná|ID seznamu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|


## <a name="deletelist"></a>DeleteList
Odstranění seznamu: Odstraní seznam 

```DELETE: /lists/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|celé číslo|Ano|Cesta|žádná|ID seznamu|
|Revize|celé číslo|Ano|dotaz|žádná|Revize|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|204|Žádný obsah|


## <a name="updatelist"></a>UpdateList
Aktualizace seznamu: aktualizace z určitého seznamu 

```PATCH: /lists/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|celé číslo|Ano|Cesta|žádná|ID seznamu|
|příspěvek| |Ano|textu|žádná|Podrobnosti seznamu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|


## <a name="gettask"></a>GetTask
Získání úkolu: načte konkrétního úkolu 

```GET: /tasks/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|
|ID|celé číslo|Ano|Cesta|žádná|Pole číslo ID úkolu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|


## <a name="updatetask"></a>UpdateTask
Aktualizace úkolu: aktualizuje konkrétního úkolu 

```PATCH: /tasks/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|
|ID|celé číslo|Ano|Cesta|žádná|Pole číslo ID úkolu|
|příspěvek| |Ano|textu|žádná|Podrobnosti úkolu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|


## <a name="deletetask"></a>DeleteTask
Odstranit úkol: Odstraní konkrétního úkolu 

```DELETE: /tasks/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|celé číslo|Ano|dotaz|žádná|ID seznamu|
|ID|celé číslo|Ano|Cesta|žádná|Pole číslo ID úkolu|
|Revize|celé číslo|Ano|dotaz|žádná|Revize|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|204|Žádný obsah|


## <a name="getsubtask"></a>GetSubTask
Získání dílčích úkolů: načte konkrétní dílčích úkolů 

```GET: /subtasks/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|žádná|ID dílčích úkolů|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|


## <a name="updatesubtask"></a>UpdateSubTask
Aktualizace dílčího úkolu: aktualizuje konkrétní dílčích úkolů 

```PATCH: /subtasks/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|celé číslo|Ano|Cesta|žádná|ID dílčích úkolů|
|příspěvek| |Ano|textu|žádná|Podrobnosti o dílčích úkolů|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|


## <a name="deletesubtask"></a>DeleteSubTask
Odstranění dílčího úkolu: Odstraní konkrétní dílčích úkolů 

```DELETE: /subtasks/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|celé číslo|Ano|Cesta|žádná|ID dílčích úkolů|
|Revize|celé číslo|Ano|dotaz|žádná|Revize|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|204|Žádný obsah|


## <a name="getnote"></a>GetNote
Získání poznámky: načíst určité poznámky 

```GET: /notes/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|žádná|Poznámka: ID|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|


## <a name="updatenote"></a>SprávcePoznámka
Aktualizovat poznámky: aktualizujte určité poznámky 

```PATCH: /notes/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|celé číslo|Ano|Cesta|žádná|Poznámka: ID|
|příspěvek| |Ano|textu|žádná|Poznámka: Podrobnosti|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|


## <a name="deletenote"></a>DeleteNote
Odstranění poznámky: odstranění určité poznámky 

```DELETE: /notes/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|celé číslo|Ano|Cesta|žádná|Poznámka: ID|
|Revize|celé číslo|Ano|dotaz|žádná|Revize|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|204|Žádný obsah|


## <a name="getcomment"></a>GetComment
Získání komentář úkolu: načíst komentář konkrétního úkolu 

```GET: /task_comments/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|žádná|ID komentáře|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|


## <a name="updatereminder"></a>UpdateReminder
Aktualizace připomenutí: aktualizujte konkrétní připomenutí 

```PATCH: /reminders/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|celé číslo|Ano|Cesta|žádná|ID připomenutí|
|příspěvek| |Ano|textu|žádná|Podrobnosti o připomenutí|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|


## <a name="deletereminder"></a>DeleteReminder
Odstranění připomenutí: odstranění konkrétní připomenutí 

```DELETE: /reminders/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|celé číslo|Ano|Cesta|žádná|ID připomenutí.|
|Revize|celé číslo|Ano|dotaz|žádná|Revize|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|204|Žádný obsah|


## <a name="object-definitions"></a>Definice objektů 

### <a name="list"></a>Seznam


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|celé číslo|Ne |
|created_at|řetězec|Ne |
|Název|řetězec|Ne |
|list_type|řetězec|Ne |
|Typ|řetězec|Ne |
|Revize|celé číslo|Ne |



### <a name="createdlist"></a>CreatedList


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|celé číslo|Ne |
|created_at|řetězec|Ne |
|Název|řetězec|Ne |
|Revize|celé číslo|Ne |
|Typ|řetězec|Ne |



### <a name="task"></a>Úkol


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|celé číslo|Ne |
|assignee_id|celé číslo|Ne |
|assigner_id|celé číslo|Ne |
|created_at|řetězec|Ne |
|created_by_id|celé číslo|Ne |
|due_date|řetězec|Ne |
|list_id|celé číslo|Ne |
|Revize|celé číslo|Ne |
|starred|Logická hodnota|Ne |
|Název|řetězec|Ne |



### <a name="subtask"></a>Dílčí úkol


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|celé číslo|Ne |
|TASK_ID|celé číslo|Ne |
|created_at|řetězec|Ne |
|created_by_id|celé číslo|Ne |
|Revize|řetězec|Ne |
|Název|řetězec|Ne |



### <a name="note"></a>Poznámka:


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|celé číslo|Ne |
|TASK_ID|celé číslo|Ne |
|obsah|řetězec|Ne |
|created_at|řetězec|Ne |
|updated_at|řetězec|Ne |
|Revize|celé číslo|Ne |



### <a name="comment"></a>Komentář


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|celé číslo|Ne |
|TASK_ID|celé číslo|Ne |
|Revize|celé číslo|Ne |
|text|řetězec|Ne |
|Typ|řetězec|Ne |
|created_at|řetězec|Ne |



### <a name="reminder"></a>Připomenutí


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|celé číslo|Ne |
|Datum|řetězec|Ne |
|TASK_ID|celé číslo|Ne |
|Revize|celé číslo|Ne |
|Typ|řetězec|Ne |
|created_at|řetězec|Ne |
|updated_at|řetězec|Ne |



### <a name="createdreminder"></a>CreatedReminder


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|celé číslo|Ne |
|Datum|řetězec|Ne |
|TASK_ID|celé číslo|Ne |
|Revize|celé číslo|Ne |
|created_at|řetězec|Ne |
|updated_at|řetězec|Ne |



### <a name="file"></a>Soubor


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|celé číslo|Ne |
|Adresa URL|řetězec|Ne |
|TASK_ID|celé číslo|Ne |
|list_id|celé číslo|Ne |
|USER_ID|celé číslo|Ne |
|název_souboru|řetězec|Ne |
|hodnota CONTENT_TYPE|řetězec|Ne |
|file_size|celé číslo|Ne |
|local_created_at|řetězec|Ne |
|created_at|řetězec|Ne |
|updated_at|řetězec|Ne |
|Typ|řetězec|Ne |
|Revize|celé číslo|Ne |



### <a name="newtask"></a>Nový úkol


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|list_id|celé číslo|Ano |
|Název|řetězec|Ano |
|assignee_id|celé číslo|Ne |
|dokončení|Logická hodnota|Ne |
|recurrence_type|řetězec|Ne |
|recurrence_count|celé číslo|Ne |
|due_date|řetězec|Ne |
|starred|Logická hodnota|Ne |



### <a name="newlist"></a>NewList


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Název|řetězec|Ano |



### <a name="newsubtask"></a>NewSubtask


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|list_id|celé číslo|Ano |
|TASK_ID|celé číslo|Ano |
|Název|řetězec|Ano |
|dokončení|Logická hodnota|Ne |



### <a name="newnote"></a>NewNote


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|list_id|celé číslo|Ano |
|TASK_ID|celé číslo|Ano |
|obsah|řetězec|Ano |



### <a name="newcomment"></a>Bez mezer


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|list_id|celé číslo|Ano |
|TASK_ID|celé číslo|Ano |
|text|řetězec|Ano |



### <a name="newreminder"></a>NewReminder


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|list_id|celé číslo|Ano |
|TASK_ID|celé číslo|Ano |
|Datum|řetězec|Ano |



### <a name="updatetask"></a>UpdateTask


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Revize|celé číslo|Ne |
|Název|řetězec|Ne |
|assignee_id|celé číslo|Ne |
|dokončení|Logická hodnota|Ne |
|recurrence_type|řetězec|Ne |
|recurrence_count|celé číslo|Ne |
|due_date|řetězec|Ne |
|starred|Logická hodnota|Ne |



### <a name="updatelist"></a>UpdateList


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Revize|celé číslo|Ne |
|Název|řetězec|Ne |



### <a name="updatesubtask"></a>UpdateSubtask


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Revize|celé číslo|Ne |
|Název|řetězec|Ne |
|dokončení|Logická hodnota|Ne |



### <a name="updatenote"></a>SprávcePoznámka


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Revize|celé číslo|Ne |
|obsah|řetězec|Ne |



### <a name="updatereminder"></a>UpdateReminder


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Datum|řetězec|Ne |
|Revize|celé číslo|Ne |


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)