<properties
pageTitle=" Pomocí konektoru časové rezervy v aplikacích logiky | Microsoft Azure"
description="Začínáme s používáním konektoru časová rezerva v aplikacích Microsoft Azure aplikace služby logiky"
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

# <a name="get-started-with-the-slack-connector"></a>Začínáme s časovou rezervu spojnice

Časová rezerva je nástroj komunikace týmu, který shromáždí všechny komunikace týmu v jednom umístit, okamžité vyhledávání a k dispozici kamkoli.

>[AZURE.NOTE] Tuto verzi článku platí pro verze schématu 2015 08 01 náhled logiky aplikace.

Časová rezerva spojnici můžete:

* Použít k vytvoření logiku aplikace

Operace v aplikacích logiku přidáte v tématu [Vytvoření logiku aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Promluvme si o aktivačními událostmi a akce

Časová rezerva spojnice mohou sloužit jako akce; existuje žádné aktivačních událostí. Všechny spojnice podporují dat ve formátu JSON a XML. 

 Konektor časová rezerva obsahuje následující akce nebo které aktivační k dispozici:

### <a name="slack-actions"></a>Časová rezerva akce
Můžete provést tyto akce:

|Akce|Popis|
|--- | ---|
|Zpravy|Vystavení zprávy do určité kanálu.|
## <a name="create-a-connection-to-slack"></a>Vytvoření připojení k časová rezerva
Pomocí konektoru časovou rezervu, je nejdřív vytvořit **připojení** a pak zadejte podrobnosti pro tyto vlastnosti: 

|Vlastnost| Povinné|Popis|
| ---|---|---|
|Token|Ano|Časová rezerva pověření|

Postupujte podle těchto kroků a přihlaste se k časová rezerva dokončení konfigurace časové rezervy **připojení** v aplikaci použití logických operátorů:

1. Vyberte **opakování**
2. Vyberte **počet_plateb** a zadejte **Interval**
3. Klikněte na **Přidat akci**  
![Konfigurace časová rezerva][1]  
4. Do pole Hledat zadejte časová rezerva a počkejte hledání se všechny položky s časová rezerva v názvu
5. Vyberte **časovou rezervu - publikovat zprávy**
6. Vyberte, **Přihlaste se k časová rezerva**:  
![Konfigurace časová rezerva][2]
7. Zadejte časová rezerva pověření k přihlášení k povolit aplikaci    
![Konfigurace časová rezerva][3]  
8. Budete přesměrovaní na protokolu vaší organizace na stránce. **Povolte** Časová rezerva chcete provést interakci s aplikací použití logických operátorů:      
![Konfigurace časová rezerva][5] 
9. Po dokončení povolení budete přesměrovaní na aplikaci logiky dokončete nakonfigurováním části **časová rezerva - se všechny zprávy** . Přidejte další aktivačními událostmi a akce, které potřebujete.  
![Konfigurace časová rezerva][6]
10. Uložte kliknutím na tlačítko **Uložit** na řádku nabídek.


>[AZURE.TIP] Toto připojení můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="slack-rest-api-reference"></a>Časová rezerva odkaz rozhraní REST API
#### <a name="this-documentation-is-for-version-10"></a>Tato dokumentace je pro verzi: 1.0


### <a name="post-a-message-to-a-specified-channel"></a>Vystavení zprávy do určité kanálu.
**```POST: /chat.postMessage```** 



| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|kanálu|řetězec|Ano|dotaz|žádná|Kanálu, soukromá skupina nebo kanálu rychlé zprávy odešlete zprávu. Může být název (ex: #general) nebo zakódovaný ID.|
|text|řetězec|Ano|dotaz|žádná|Text zprávy odešlete. Možnosti formátování, najdete v článku https://api.slack.com/docs/formatting.|
|uživatelské jméno|řetězec|Ne|dotaz|žádná|Název bot|
|as_user|Logická hodnota|Ne|dotaz|žádná|Předání PRAVDA, pokud chcete vystavit zprávu jako ověřený uživatel, ne jako moduly bot|
|Analýza|řetězec|Ne|dotaz|žádná|Změňte, jak jsou zpracovány zprávy. Podrobnosti najdete v tématu https://api.slack.com/docs/formatting.|
|link_names|celé číslo|Ne|dotaz|žádná|Vyhledání a propojit jména kanálů a uživatelská jména.|
|unfurl_links|Logická hodnota|Ne|dotaz|žádná|Předat chcete povolit unfurling převážně textové obsahu.|
|unfurl_media|Logická hodnota|Ne|dotaz|žádná|Předáte nepravda zakázat unfurling médií obsahu.|
|icon_url|řetězec|Ne|dotaz|žádná|Adresa URL obrázek použít jako ikonu pro tuto zprávu|
|icon_emoji|řetězec|Ne|dotaz|žádná|Emoji pro účely tuto zprávu jako ikona|


### <a name="here-are-the-possible-responses"></a>Tady je možné odpovědi:

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|408|Žádost o vypršení časového limitu|
|429|Příliš mnoho požadavků|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|503|Časová rezerva služba není k dispozici|
|504|Vypršel časový limit brány|
|Výchozí|Operace se nezdařila.|
------



## <a name="object-definitions"></a>Definice objektu: 

 **Zpráva**: Yammer zprávy

Požadované vlastnosti zprávy:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|ID|celé číslo|
|content_excerpt|řetězec|
|sender_id|celé číslo|
|replied_to_id|celé číslo|
|created_at|řetězec|
|network_id|celé číslo|
|message_type|řetězec|
|sender_type|řetězec|
|Adresa URL|řetězec|
|web_url|řetězec|
|group_id|celé číslo|
|textu|Nedefinováno|
|thread_id|celé číslo|
|direct_message|Logická hodnota|
|client_type|řetězec|
|client_url|řetězec|
|jazyk|řetězec|
|notified_user_ids|pole|
|Ochrana osobních údajů|řetězec|
|liked_by|Nedefinováno|
|system_message|Logická hodnota|



 **PostOperationRequest**: představuje příspěvek žádost o Yammeru spojnice publikovat na yammer

Požadované vlastnosti PostOperationRequest:

textu

**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|textu|řetězec|
|group_id|celé číslo|
|replied_to_id|celé číslo|
|direct_to_id|celé číslo|
|vysílání|Logická hodnota|
|téma1|řetězec|
|téma2|řetězec|
|topic3|řetězec|
|topic4|řetězec|
|topic5|řetězec|
|topic6|řetězec|
|topic7|řetězec|
|topic8|řetězec|
|topic9|řetězec|
|topic10|řetězec|
|topic11|řetězec|
|topic12|řetězec|
|topic13|řetězec|
|topic14|řetězec|
|topic15|řetězec|
|topic16|řetězec|
|topic17|řetězec|
|topic18|řetězec|
|topic19|řetězec|
|topic20|řetězec|



 **MessageList**: seznam zpráv

Požadované vlastnosti MessageList:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|zprávy|pole|



 **MessageBody**: text zprávy

Požadované vlastnosti MessageBody:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|analyzovat|řetězec|
|prostý|řetězec|
|formátovaný|řetězec|



 **LikedBy**: dali Lajk tak, že

Požadované vlastnosti LikedBy:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|počet|celé číslo|
|názvy|pole|



 **YammmerEntity**: dali Lajk tak, že

Požadované vlastnosti YammmerEntity:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|Typ|řetězec|
|ID|celé číslo|
|full_name|řetězec|


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>Definice objektu: 

 **WebResultModel**: výsledky hledání webové služby Bing

Požadované vlastnosti WebResultModel:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|Název|řetězec|
|Popis|řetězec|
|DisplayUrl|řetězec|
|ID|řetězec|
|FullUrl|řetězec|



 **VideoResultModel**: výsledky hledání videa Bing

Požadované vlastnosti VideoResultModel:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|Název|řetězec|
|DisplayUrl|řetězec|
|ID|řetězec|
|MediaUrl|řetězec|
|Za běhu|celé číslo|
|Miniatura|Nedefinováno|



 **ThumbnailModel**: miniaturu vlastnosti multimediální prvku

Požadované vlastnosti ThumbnailModel:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|MediaUrl|řetězec|
|Typ obsahu|řetězec|
|Šířka|celé číslo|
|Výška|celé číslo|
|FileSize|celé číslo|



 **ImageResultModel**: výsledky hledání obrázků služby Bing

Požadované vlastnosti ImageResultModel:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|Název|řetězec|
|DisplayUrl|řetězec|
|ID|řetězec|
|MediaUrl|řetězec|
|SourceUrl|řetězec|
|Miniatura|Nedefinováno|



 **NewsResultModel**: výsledky hledání příspěvky Bing

Požadované vlastnosti NewsResultModel:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|Název|řetězec|
|Popis|řetězec|
|DisplayUrl|řetězec|
|ID|řetězec|
|Zdroje|řetězec|
|Datum|řetězec|



 **SpellResultModel**: Bing pravopis návrhy výsledků

Požadované vlastnosti SpellResultModel:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|ID|řetězec|
|Hodnota|řetězec|



 **RelatedSearchResultModel**: Bing souvisejících výsledků hledání

Požadované vlastnosti RelatedSearchResultModel:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|Název|řetězec|
|ID|řetězec|
|BingUrl|řetězec|



 **CompositeSearchResultModel**: výsledky hledání složené Bing

Požadované vlastnosti CompositeSearchResultModel:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|WebResultsTotal|celé číslo|
|ImageResultsTotal|celé číslo|
|VideoResultsTotal|celé číslo|
|NewsResultsTotal|celé číslo|
|SpellSuggestionsTotal|celé číslo|
|WebResults|pole|
|ImageResults|pole|
|VideoResults|pole|
|NewsResults|pole|
|SpellSuggestionResults|pole|
|RelatedSearchResults|pole|


## <a name="object-definitions"></a>Definice objektu: 

 **PostOperationResponse**: představuje odpověď operace post časová rezerva konektoru odesíláním časová rezerva

Požadované vlastnosti PostOperationResponse:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|Ok|Logická hodnota|
|kanálu|řetězec|
|TS|řetězec|
|zpráva|Nedefinováno|
|Chyba|řetězec|



 **Položku MessageItem**: zpráva kanálu.

Požadované vlastnosti pro položku MessageItem:


Žádná z vlastnosti jsou potřeba. 


**Všechny vlastnosti**: 


| Jméno | Datový typ |
|---|---|
|text|řetězec|
|ID|řetězec|
|uživatel|řetězec|
|Vytvoření|celé číslo|
|Odstranit is_user|Logická hodnota|


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
