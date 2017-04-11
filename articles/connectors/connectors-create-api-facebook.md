<properties
    pageTitle="Doplněk aplikace logiky konektor Facebooku | Microsoft Azure"
    description="Základní informace o konektor Facebooku s parametry rozhraní REST API"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-facebook-connector"></a>Začínáme s konektor Facebooku
Připojení k Facebooku a přispívání do časové osy, stránku informačního kanálu, a dalších funkcí. 

>[AZURE.NOTE] Tuto verzi článku platí pro verze schématu 2015 08 01 náhled logiky aplikace.


Se službou Facebook máte tyto možnosti:

- Vytvoření vaše obchodní toku podle data, která se zobrazí z Facebooku. 
- Po přijetí nový příspěvek, použijte aktivační událost.
- Použití akce, které příspěvek na svou zeď, získáte na stránku informačního kanálu a další. Tyto akce se odpověď a zpřístupněte výstup pro jiné akce. Například po nový příspěvek na časovou osu můžete provést tento příspěvek a použít pro v informačním kanálu, Twitter. 



Operace v aplikacích logiku přidáte v tématu [Vytvoření logiku aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce
Konektor Facebooku zahrnuje následující aktivační události a akce. 

| Aktivace | Akce|
| --- | --- |
| <ul><li>Po nový příspěvek na svůj ose</li></ul> |<ul><li>Získání kanálu z mé časové osy</li><li>Publikovat svůj časovou osu</li><li>Po nový příspěvek na svůj ose</li><li>Stránka informačního kanálu</li><li>Získání uživatele časové osy</li><li>Publikovat stránku</li></ul>

Všechny spojnice podporují dat ve formátu JSON a XML.

## <a name="create-a-connection-to-facebook"></a>Vytvoření připojení k Facebooku
Tato spojnice přidáte ke svým aplikacím použití logických operátorů, musí povolit aplikace logiky pro připojení k vaší Facebook.

1. Přihlaste se ke svému účtu Facebooku
2. Vyberte **Ověřit**a povolte aplikací použití logických operátorů k připojení a používat vaše z Facebooku. 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] Tento stejné připojení k Facebooku můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="swagger-rest-api-reference"></a>Odkaz swagger rozhraní REST API
Platí pro verze: 1.0.

### <a name="get-feed-from-my-timeline"></a>Získání kanálu z mé časové osy
Datové kanály získá z časové osy přihlášeného uživatele.  
```GET: /me/feed```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|pole|řetězec|Ne|dotaz|žádná |Určení pole, která se má vrátit. Příklad (id, jméno a obrázek).|
|limit|celé číslo|Ne|dotaz| žádná|Maximální počet příspěvků k načtení|
|s|řetězec|Ne|dotaz| žádná|Omezte seznam příspěvků jenom ty umístění připojené.|
|Filtr|řetězec|Ne|dotaz| žádná|Načtení pouze příspěvky, které splňují určité toku filtru.|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


### <a name="post-to-my-timeline"></a>Publikovat svůj časovou osu
Vystavení zprávy stav časovou osu přihlášeného uživatele.  
```POST: /me/feed```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|příspěvek|řetězec |Ano|textu|žádná |Novou zprávu, která se publikuje|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>Po nový příspěvek na svůj ose
Spustí nový tok po nový příspěvek na ose přihlášeného uživatele.  
```GET: /trigger/me/feed```

Neexistují žádné parametry. 

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


### <a name="get-page-feed"></a>Stránka informačního kanálu
Pokud potřebujete příspěvky z kanálu určité stránky.  
```GET: /{pageId}/feed```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|pageId|řetězec|Ano|Cesta| žádná|Na stránce, ze kterého jste příspěvků k načtení ID.|
|limit|celé číslo|Ne|dotaz| žádná|Maximální počet příspěvků k načtení|
|include_hidden|Logická hodnota|Ne|dotaz|žádná |Jestli se mají zahrnout všechny příspěvky, které byly skryté stránkou|
|pole|řetězec|Ne|dotaz|žádná |Určení pole, která se má vrátit. Příklad (id, jméno a obrázek).|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


### <a name="get-user-timeline"></a>Získání uživatele časové osy
Pokud potřebujete příspěvky z časové osy uživatele.  
```GET: /{userId}/feed```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID uživatele|řetězec|Ano|Cesta|žádná |ID uživatele, jehož časovou osu muset načíst.|
|limit|celé číslo|Ne|dotaz|žádná |Maximální počet příspěvků k načtení|
|s|řetězec|Ne|dotaz|žádná |Omezte seznam příspěvků jenom ty umístění připojené.|
|Filtr|řetězec|Ne|dotaz| žádná|Načtení pouze příspěvky, které splňují určité toku filtru.|
|pole|řetězec|Ne|dotaz| žádná|Určení pole, která se má vrátit. Příklad (id, jméno a obrázek).|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


### <a name="post-to-page"></a>Publikovat stránku
Vystavení zprávy na stránku služby Facebook jako přihlášený uživatel.  
```POST: /{pageId}/feed```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|pageId|řetězec|Ano|Cesta|žádná |ID odeslání stránky.|
|příspěvek|počet |Ano|textu|žádná |Novou zprávu, která se publikuje.|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="object-definitions"></a>Definice objektů

#### <a name="getfeedresponse"></a>GetFeedResponse

|Název vlastnosti | Datový typ | Povinné|
|---|---|---|
|data|pole|Ne|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|data|pole|Ne|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: Jedna položka v profilu uživatele kanálu.
Profil může být uživatele, stránky, aplikací nebo skupiny. 

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|ID|řetězec|Ne|
|admin_creator|pole|Ne|
|popisků k obrázkům|řetězec|Ne|
|created_time|řetězec|Ne|
|Popis|řetězec|Ne|
|feed_targeting|Nedefinováno|Ne|
|z|Nedefinováno|Ne|
|Ikona|řetězec|Ne|
|is_hidden|Logická hodnota|Ne|
|is_published|Logická hodnota|Ne|
|odkaz|řetězec|Ne|
|zpráva|řetězec|Ne|
|Jméno|řetězec|Ne|
|object_id|řetězec|Ne|
|Obrázek|řetězec|Ne|
|blokování|Nedefinováno|Ne|
|Ochrana osobních údajů|Nedefinováno|Ne|
|Vlastnosti|pole|Ne|
|zdroje|řetězec|Ne|
|status_type|řetězec|Ne|
|text|řetězec|Ne|
|zaměření na|Nedefinováno|Ne|
|k|pole|Ne|
|Typ|řetězec|Ne|
|updated_time|řetězec|Ne|
|with_tags|Nedefinováno|Ne|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: Jedna položka v profilu uživatele kanálu.
Profil může být uživatele, stránky, aplikací nebo skupiny.

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|ID|řetězec|Ne|
|created_time|řetězec|Ne|
|z|Nedefinováno|Ne|
|zpráva|řetězec|Ne|
|Typ|řetězec|Ne|

#### <a name="adminitem"></a>AdminItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|ID|řetězec|Ne|
|odkaz|řetězec|Ne|

#### <a name="propertyitem"></a>PropertyItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|Jméno|řetězec|Ne|
|text|řetězec|Ne|
|Pole atribut href odkazu|řetězec|Ne|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|zpráva|řetězec|Ano|
|odkaz|řetězec|Ne|
|Obrázek|řetězec|Ne|
|Jméno|řetězec|Ne|
|popisků k obrázkům|řetězec|Ne|
|Popis|řetězec|Ne|
|blokování|řetězec|Ne|
|značky|řetězec|Ne|
|Ochrana osobních údajů|Nedefinováno|Ne|
|object_attachment|řetězec|Ne|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|zpráva|řetězec|Ano|
|odkaz|řetězec|Ne|
|Obrázek|řetězec|Ne|
|Jméno|řetězec|Ne|
|popisků k obrázkům|řetězec|Ne|
|Popis|řetězec|Ne|
|Akce|pole|Ne|
|blokování|řetězec|Ne|
|značky|řetězec|Ne|
|object_attachment|řetězec|Ne|
|zaměření na|Nedefinováno|Ne|
|feed_targeting|Nedefinováno|Ne|
|publikování|Logická hodnota|Ne|
|scheduled_publish_time|řetězec|Ne|
|backdated_time|řetězec|Ne|
|backdated_time_granularity|řetězec|Ne|
|child_attachments|pole|Ne|
|multi_share_end_card|Logická hodnota|Ne|

#### <a name="postfeedresponse"></a>PostFeedResponse

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|ID|řetězec|Ne|

#### <a name="profilecollection"></a>ProfileCollection

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|data|pole|Ne|

#### <a name="useritem"></a>UserItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|ID|řetězec|Ne|
|křestní_jméno|řetězec|Ne|
|Příjmení|řetězec|Ne|
|Jméno|řetězec|Ne|
|Zisk.|řetězec|Ne|
|informace o|řetězec|Ne|

#### <a name="actionitem"></a>ActionItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|Jméno|řetězec|Ne|
|odkaz|řetězec|Ne|

#### <a name="targetitem"></a>TargetItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|země|pole|Ne|
|národní prostředí|pole|Ne|
|oblasti|pole|Ne|
|města|pole|Ne|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Zaměření na tomto příspěvku kanálu objekt, který určuje příspěvky
Všichni účastníci tyto skupiny snáze naleznete v tomto příspěvku, ostatní pravděpodobně nižší. Platí jenom pro stránky.

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|země|pole|Ne|
|oblasti|pole|Ne|
|města|pole|Ne|
|age_min|celé číslo|Ne|
|age_max|celé číslo|Ne|
|genders|pole|Ne|
|relationship_statuses|pole|Ne|
|interested_in|pole|Ne|
|college_years|pole|Ne|
|zájmy|pole|Ne|
|relevant_until|celé číslo|Ne|
|education_statuses|pole|Ne|
|národní prostředí|pole|Ne|

#### <a name="placeitem"></a>PlaceItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|ID|řetězec|Ne|
|Jméno|řetězec|Ne|
|overall_rating|číslo|Ne|
|umístění|Nedefinováno|Ne|

#### <a name="locationitem"></a>LocationItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|Město|řetězec|Ne|
|země|řetězec|Ne|
|šířky|číslo|Ne|
|located_in|řetězec|Ne|
|délky|číslo|Ne|
|Jméno|řetězec|Ne|
|oblast|řetězec|Ne|
|Stav|řetězec|Ne|
|Ulice|řetězec|Ne|
|PSČ|řetězec|Ne|

#### <a name="privacyitem"></a>PrivacyItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|Popis|řetězec|Ne|
|Hodnota|řetězec|Ano|
|Povolit|řetězec|Ne|
|Odepřít|řetězec|Ne|
|Přátelé|řetězec|Ne|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|odkaz|řetězec|Ne|
|Obrázek|řetězec|Ne|
|image_hash|řetězec|Ne|
|Jméno|řetězec|Ne|
|Popis|řetězec|Ne|

#### <a name="postphotorequest"></a>PostPhotoRequest

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|Adresa URL|řetězec|Ano|
|popisků k obrázkům|řetězec|Ne|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|ID|řetězec|Ano|
|post_id|řetězec|Ano|

#### <a name="postvideorequest"></a>PostVideoRequest

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|videoData|řetězec|Ano|
|Popis|řetězec|Ano|
|Název|řetězec|Ano|
|uploadedVideoName|řetězec|Ne|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|data|Nedefinováno|Ano|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|Adresa URL|řetězec|Ano|
|is_silhouette|Logická hodnota|Ano|
|Výška|řetězec|Ne|
|Šířka|řetězec|Ne|

#### <a name="geteventresponse"></a>GetEventResponse

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|data|pole|Ano|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|ID|řetězec|Ano|
|Jméno|řetězec|Ano|
|start_time|řetězec|Ne|
|end_time|řetězec|Ne|
|podle časového pásma|řetězec|Ne|
|umístění|řetězec|Ne|
|Popis|řetězec|Ne|
|ticket_uri|řetězec|Ne|
|rsvp_status|řetězec|Ano|


## <a name="next-steps"></a>Další kroky

[Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).
