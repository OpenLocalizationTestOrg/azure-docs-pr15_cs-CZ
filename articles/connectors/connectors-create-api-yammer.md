<properties
pageTitle="Přidání spojnice Yammer v aplikacích použití logických operátorů | Microsoft Azure"
description="Základní informace o konektoru Yammeru s parametry rozhraní REST API"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-yammer-connector"></a>Začínáme s konektoru Yammeru

Připojení ke službě Yammer pro access konverzace ve svojí síti organizace.

>[AZURE.NOTE] Tuto verzi článku platí pro verze schématu 2015 08 01 náhled logiky aplikace.

S Yammerem máte tyto možnosti:

- Vytvoření vaše obchodní toku podle data, která se zobrazí z Yammeru. 
- Použití spustí pro, když je novou zprávu ve skupině nebo informačního kanálu sleduji.
- Umožňuje vystavení zprávy, zobrazí všechny zprávy a další akce. Tyto akce se odpověď a zpřístupněte výstup pro jiné akce. Třeba když se zobrazí novou zprávu, můžete poslat e-mailu s použitím Office 365.

Operace v aplikacích logiku přidáte v tématu [Vytvoření logiku aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce
Yammer zahrnuje následující aktivačních událostí a akce. 

Aktivační událost | Akce
--- | ---
<ul><li>Pokud jsou ve skupině nové zprávy</li><li>Pokud jsou nové zprávy v mém následující kanálu</li></ul>| <ul><li>Zobrazí všechny zprávy</li><li>Získá zprávy ve skupině</li><li>Získá zprávy od Moje následující kanálu</li><li>Publikovat zprávy</li><li>Pokud jsou ve skupině nové zprávy</li><li>Pokud jsou nové zprávy v mém následující kanálu</li></ul>

Všechny spojnice podporují dat ve formátu JSON a XML. 

## <a name="create-a-connection-to-yammer"></a>Vytvoření připojení k Yammeru
Pomocí konektoru Yammer, můžete nejdřív vytvořit **připojení** a pak zadejte podrobnosti pro tyto vlastnosti: 

|Vlastnost| Povinné|Popis|
| ---|---|---|
|Token|Ano|Zadání přihlašovacích údajů pro Yammer|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] Toto připojení můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="yammer-rest-api-reference"></a>Odkaz rozhraní REST API pro Yammer
Tato dokumentace je pro verzi: 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Stáhnout všechny veřejné zprávy v síti Yammer přihlášený uživatel
Odpovídá "Všechny" konverzace v Yammeru webového rozhraní.  
```GET: /messages.json```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|older_then|celé číslo|Ne|dotaz|žádná|Vrátí zpráv starších než zadaný jako číselný řetězec ID zprávy. To je užitečné pro přestránkování zprávy. Například, pokud jste právě nacházíte 20 zprávy a nejstarší je zadané číslo 2912, může připojit "? older_than = 2912″ vaši žádost o doručení 20 zpráv před můžou být vidíte.|
|newer_then|celé číslo|Ne|dotaz|žádná|Vrátí zprávy novější než zadaný jako číselný řetězec ID zprávy. To by měl být vyvolají dotazování pro nové zprávy. Pokud se chcete dozvědět zpráv a nejnovější zprávy poslaný. 3516, můžete provést žádost s parametrem "? newer_than = 3516″ zajistit, že nebudete mít více kopií zpráv již na stránce.|
|limit|celé číslo|Ne|dotaz|žádná|Vrátí zadaný počet zpráv.|
|stránky|celé číslo|Ne|dotaz|žádná|Zobrazí se stránka zadaná. Pokud vrátilo dat je větší než limit, můžete toto pole pro přístup k další stránky|


### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|408|Žádost o vypršení časového limitu|
|429|Příliš mnoho požadavků|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|503|Služba není k dispozici pro Yammer|
|504|Vypršel časový limit brány|
|Výchozí|Operace se nezdařila.|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Vystavení zprávy do skupiny nebo všechny společnosti kanálu
Pokud ID skupiny zpráva vystavená do určité skupiny dalšího bude účtován ve všech kanálu společnosti.    
```POST: /messages.json``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|zadání vstupních hodnot| |Ano|textu|žádná|Příspěvek zprávu žádosti|


### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|201|Vytvoření|



## <a name="object-definitions"></a>Definice objektů

#### <a name="message-yammer-message"></a>: Yammer zpráva

| Jméno | Datový typ | Povinné |
|---|---| --- | 
|ID|celé číslo|Ne|
|content_excerpt|řetězec|Ne|
|sender_id|celé číslo|Ne|
|replied_to_id|celé číslo|Ne|
|created_at|řetězec|Ne|
|network_id|celé číslo|Ne|
|message_type|řetězec|Ne|
|sender_type|řetězec|Ne|
|Adresa URL|řetězec|Ne|
|web_url|řetězec|Ne|
|group_id|celé číslo|Ne|
|textu|Nedefinováno|Ne|
|thread_id|celé číslo|Ne|
|direct_message|Logická hodnota|Ne|
|client_type|řetězec|Ne|
|client_url|řetězec|Ne|
|jazyk|řetězec|Ne|
|notified_user_ids|pole|Ne|
|Ochrana osobních údajů|řetězec|Ne|
|liked_by|Nedefinováno|Ne|
|system_message|Logická hodnota|Ne|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Představuje příspěvek žádost o Yammeru spojnice publikovat na yammer

| Jméno | Datový typ | Povinné |
|---|---| --- | 
|textu|řetězec|Ano|
|group_id|celé číslo|Ne|
|replied_to_id|celé číslo|Ne|
|direct_to_id|celé číslo|Ne|
|vysílání|Logická hodnota|Ne|
|téma1|řetězec|Ne|
|téma2|řetězec|Ne|
|topic3|řetězec|Ne|
|topic4|řetězec|Ne|
|topic5|řetězec|Ne|
|topic6|řetězec|Ne|
|topic7|řetězec|Ne|
|topic8|řetězec|Ne|
|topic9|řetězec|Ne|
|topic10|řetězec|Ne|
|topic11|řetězec|Ne|
|topic12|řetězec|Ne|
|topic13|řetězec|Ne|
|topic14|řetězec|Ne|
|topic15|řetězec|Ne|
|topic16|řetězec|Ne|
|topic17|řetězec|Ne|
|topic18|řetězec|Ne|
|topic19|řetězec|Ne|
|topic20|řetězec|Ne|

#### <a name="messagelist-list-of-messages"></a>MessageList: Seznam zpráv

| Jméno | Datový typ | Povinné |
|---|---| --- | 
|zprávy|pole|Ne|


#### <a name="messagebody-message-body"></a>MessageBody: Text zprávy

| Jméno | Datový typ | Povinné |
|---|---| --- | 
|analyzovat|řetězec|Ne|
|prostý|řetězec|Ne|
|formátovaný|řetězec|Ne|

#### <a name="likedby-liked-by"></a>LikedBy: Podle dali Lajk

| Jméno | Datový typ | Povinné |
|---|---| --- | 
|počet|celé číslo|Ne|
|názvy|pole|Ne|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Podle dali Lajk

| Jméno | Datový typ | Povinné |
|---|---| --- | 
|Typ|řetězec|Ne|
|ID|celé číslo|Ne|
|full_name|řetězec|Ne|


## <a name="next-steps"></a>Další kroky
[Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
