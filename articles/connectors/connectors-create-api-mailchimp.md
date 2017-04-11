<properties
pageTitle="MailChimp | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. MailChimp je SaaS služba, která umožňuje firmy a správa automatizovat marketingové aktivity e-mailu, včetně odesílání marketingové e-maily, automatizované zprávy a cílových kampaní."
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

# <a name="get-started-with-the-mailchimp-connector"></a>Začínáme s MailChimp spojnice

MailChimp je SaaS služba, která umožňuje firmy a správa automatizovat marketingové aktivity e-mailu, včetně odesílání marketingové e-maily, automatizované zprávy a cílových kampaní.


>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2015 08 01 náhled schématu verze.

Můžete začít s vytvářením logiky aplikaci najdete v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce

Konektor MailChimp mohou sloužit jako akce; má které aktivační. Všechny spojnice podporují dat ve formátu JSON a XML.

 Konektor MailChimp obsahuje následující akce nebo které aktivační k dispozici:

### <a name="mailchimp-actions"></a>MailChimp akce
Můžete provést tyto akce:

|Akce|Popis|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|Vytvoření nové kampaně založený na typu kampaně seznam příjemců a kampaně nastavení (předmět, název, from_name a reply_to)|
|[NewList](connectors-create-api-mailchimp.md#newlist)|Vytvoření nového seznamu ve vašem účtu MailChimp|
|[– Přidání člena](connectors-create-api-mailchimp.md#addmember)|Přidejte nebo aktualizujte seznam členů|
|[odebírat členy](connectors-create-api-mailchimp.md#removemember)|Odstranění člena ze seznamu.|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|Informace o aktualizaci pro konkrétní seznam členů|
### <a name="mailchimp-triggers"></a>Aktivace MailChimp
Můžete poslouchat tyto události:

|Aktivační událost | Popis|
|--- | ---|
|Když člena je přidán do seznamu|Spustí pracovní postup, když nového člena je přidán do seznamu|
|Vytvoření nového seznamu|Spustí pracovní postup při vytvoření nového seznamu|


## <a name="create-a-connection-to-mailchimp"></a>Vytvoření připojení k MailChimp
Vytváření aplikací pro použití logických operátorů s MailChimp, můžete musíte nejdřív vytvořit **připojení** a pak zadejte podrobnosti pro následující vlastnosti:

|Vlastnost| Povinné|Popis|
| ---|---|---|
|Tokenu|Ano|Pověření MailChimp|

>[AZURE.INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] Toto připojení můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="reference-for-mailchimp"></a>Odkaz pro MailChimp
Platí pro verze: 1.0

## <a name="newcampaign"></a>newcampaign
Nové kampaně: Vytvoření nového kampaně založený na typu kampaně seznam příjemců a kampaně nastavení (předmět, název, from_name a reply_to)

```POST: /campaigns```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|newCampaignRequest| |Ano|textu|žádná|JSON objekt, který chcete poslat v těle s novou žádost o parametry kampaně|

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


## <a name="newlist"></a>NewList
Nový seznam: Vytvoření nového seznamu ve vašem účtu MailChimp

```POST: /lists```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|newListRequest| |Ano|textu|žádná|JSON objekt, který chcete poslat v těle s novou žádost o parametry kampaně|

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


## <a name="addmember"></a>– Přidání člena
Přidání člena do seznamu: přidejte nebo aktualizujte seznam členů

```POST: /lists/{list_id}/members```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|řetězec|Ano|Cesta|žádná|Jedinečné id pro seznam|
|newMemberInList| |Ano|textu|žádná|JSON objektu, který chcete poslat v těle novými informacemi člena|

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


## <a name="removemember"></a>odebírat členy
Odebrání člena ze seznamu: Pokud chcete člena odstranit ze seznamu.

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|řetězec|Ano|Cesta|žádná|Jedinečné id pro seznam|
|member_email|řetězec|Ano|Cesta|žádná|E-mailovou adresu člena odstranit|

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


## <a name="updatemember"></a>updatemember
Aktualizovat informace o členech: aktualizujte informace o konkrétní seznam členů

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|řetězec|Ano|Cesta|žádná|Jedinečné id pro seznam|
|member_email|řetězec|Ano|Cesta|žádná|Jedinečný e-mailovou adresu člena aktualizovat|
|updateMemberInListRequest| |Ano|textu|žádná|JSON objekt, který chcete poslat v těle informacemi aktualizované člena|

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


## <a name="onmembersubscribed"></a>OnMemberSubscribed
Když člena přidala seznam: spustí pracovní postup, když nového člena přidala do seznamu

```GET: /trigger/lists/{list_id}/members```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|list_id|řetězec|Ano|Cesta|žádná|Jedinečné id pro seznam|

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


## <a name="oncreatelist"></a>OnCreateList
Vytvoření nového seznamu: spustí pracovní postup při vytvoření nového seznamu

```GET: /trigger/lists```

Neexistují žádné parametry pro tento hovor
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

### <a name="newcampaignrequest"></a>NewCampaignRequest


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Typ|řetězec|Ano |
|příjemci|Nedefinováno|Ano |
|nastavení|Nedefinováno|Ano |
|variate_settings|Nedefinováno|Ne |
|sledování|Nedefinováno|Ne |
|rss_opts|Nedefinováno|Ne |
|social_card|Nedefinováno|Ne |



### <a name="recipient"></a>Příjemce


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|list_id|řetězec|Ano |
|segment_opts|Nedefinováno|Ne |



### <a name="settings"></a>Nastavení


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|subject_line|řetězec|Ano |
|Název|řetězec|Ne |
|from_name|řetězec|Ano |
|reply_to|řetězec|Ano |
|use_conversation|Logická hodnota|Ne |
|to_name|řetězec|Ne |
|folder_id|celé číslo|Ne |
|ověření|Logická hodnota|Ne |
|auto_footer|Logická hodnota|Ne |
|inline_css|Logická hodnota|Ne |
|auto_tweet|Logická hodnota|Ne |
|auto_fb_post|pole|Ne |
|fb_comments|Logická hodnota|Ne |



### <a name="variatesettings"></a>Variate_Settings


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|winner_criteria|řetězec|Ne |
|prodleva_čekání|celé číslo|Ne |
|test_size|celé číslo|Ne |
|subject_lines|pole|Ne |
|send_times|pole|Ne |
|from_names|pole|Ne |
|reply_to_addresses|pole|Ne |



### <a name="tracking"></a>Sledování


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Otevře|Logická hodnota|Ne |
|html_clicks|Logická hodnota|Ne |
|text_clicks|Logická hodnota|Ne |
|goal_tracking|Logická hodnota|Ne |
|ecomm360|Logická hodnota|Ne |
|google_analytics|řetězec|Ne |
|clicktale|řetězec|Ne |
|služby Salesforce|Nedefinováno|Ne |
|highrise|Nedefinováno|Ne |
|obdélník s efektem|Nedefinováno|Ne |



### <a name="rssopts"></a>RSS_Opts


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|feed_url|řetězec|Ne |
|četnosti|řetězec|Ne |
|constrain_rss_img|řetězec|Ne |
|plán|Nedefinováno|Ne |



### <a name="socialcard"></a>Social_Card


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|image_url|řetězec|Ne |
|Popis|řetězec|Ne |
|Název|řetězec|Ne |



### <a name="segmentopts"></a>Segment_Opts


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|saved_segment_id|celé číslo|Ne |
|POZVYHLEDAT|řetězec|Ne |



### <a name="salesforce"></a>Služby Salesforce


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|kampaň|Logická hodnota|Ne |
|Poznámky|Logická hodnota|Ne |



### <a name="highrise"></a>Highrise


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|kampaň|Logická hodnota|Ne |
|Poznámky|Logická hodnota|Ne |



### <a name="capsule"></a>Obdélník s efektem


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Poznámky|Logická hodnota|Ne |



### <a name="schedule"></a>Plán


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodina|celé číslo|Ne |
|daily_send|Nedefinováno|Ne |
|weekly_send_day|řetězec|Ne |
|monthly_send_date|číslo|Ne |



### <a name="dailysend"></a>Daily_Send


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|neděle|Logická hodnota|Ne |
|pondělí|Logická hodnota|Ne |
|Úterý|Logická hodnota|Ne |
|středa|Logická hodnota|Ne |
|čtvrtek|Logická hodnota|Ne |
|pátek|Logická hodnota|Ne |
|sobota|Logická hodnota|Ne |



### <a name="campaignresponsemodel"></a>CampaignResponseModel


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|řetězec|Ne |
|Typ|řetězec|Ne |
|create_time|řetězec|Ne |
|archive_url|řetězec|Ne |
|Stav|řetězec|Ne |
|emails_sent|celé číslo|Ne |
|send_time|řetězec|Ne |
|hodnota CONTENT_TYPE|řetězec|Ne |
|příjemce|pole|Ne |
|nastavení|Nedefinováno|Ne |
|variate_settings|Nedefinováno|Ne |
|sledování|Nedefinováno|Ne |
|rss_opts|Nedefinováno|Ne |
|ab_split_opts|Nedefinováno|Ne |
|social_card|Nedefinováno|Ne |
|report_summary|Nedefinováno|Ne |
|delivery_status|Nedefinováno|Ne |
|_links|pole|Ne |



### <a name="absplitopts"></a>AB_Split_Opts


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|split_test|řetězec|Ne |
|pick_winner|řetězec|Ne |
|wait_units|řetězec|Ne |
|prodleva_čekání|celé číslo|Ne |
|split_size|celé číslo|Ne |
|from_name_a|řetězec|Ne |
|from_name_b|řetězec|Ne |
|reply_email_a|řetězec|Ne |
|reply_email_b|řetězec|Ne |
|subject_a|řetězec|Ne |
|subject_b|řetězec|Ne |
|send_time_a|řetězec|Ne |
|send_time_b|řetězec|Ne |
|send_time_winner|řetězec|Ne |



### <a name="reportsummary"></a>Report_Summary


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Otevře|celé číslo|Ne |
|unique_opens|celé číslo|Ne |
|open_rate|číslo|Ne |
|kliknutí|celé číslo|Ne |
|subscriber_clicks|číslo|Ne |
|click_rate|číslo|Ne |



### <a name="deliverystatus"></a>Delivery_Status


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|povoleno|Logická hodnota|Ne |
|can_cancel|Logická hodnota|Ne |
|Stav|řetězec|Ne |
|emails_sent|celé číslo|Ne |
|emails_canceled|celé číslo|Ne |



### <a name="link"></a>Odkaz


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|rel|řetězec|Ne |
|Pole atribut href odkazu|řetězec|Ne |
|Metoda|řetězec|Ne |
|targetSchema|řetězec|Ne |
|schéma|řetězec|Ne |



### <a name="newlistrequest"></a>NewListRequest


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Jméno|řetězec|Ano |
|kontakt|Nedefinováno|Ano |
|permission_reminder|řetězec|Ano |
|use_archive_bar|Logická hodnota|Ne |
|campaign_defaults|Nedefinováno|Ano |
|notify_on_subscribe|řetězec|Ne |
|notify_on_unsubscribe|řetězec|Ne |
|email_type_option|Logická hodnota|Ano |
|viditelnost|řetězec|Ne |



### <a name="contact"></a>Kontakt


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|společnosti|řetězec|Ano |
|adresa1|řetězec|Ano |
|Adresa2|řetězec|Ne |
|Město|řetězec|Ano |
|Stav|řetězec|Ano |
|PSČ|řetězec|Ano |
|země|řetězec|Ano |
|Telefon|řetězec|Ano |



### <a name="campaigndefaults"></a>Campaign_Defaults


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|from_name|řetězec|Ano |
|from_email|řetězec|Ano |
|předmět|řetězec|Ne |
|jazyk|řetězec|Ano |



### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|řetězec|Ano |
|Jméno|řetězec|Ano |
|kontakt|Nedefinováno|Ano |
|permission_reminder|řetězec|Ano |
|use_archive_bar|Logická hodnota|Ne |
|campaign_defaults|Nedefinováno|Ano |
|notify_on_subscribe|řetězec|Ne |
|notify_on_unsubscribe|řetězec|Ne |
|date_created|řetězec|Ne |
|list_rating|celé číslo|Ne |
|email_type_option|Logická hodnota|Ano |
|subscribe_url_short|řetězec|Ne |
|subscribe_url_long|řetězec|Ne |
|beamer_address|řetězec|Ne |
|viditelnost|řetězec|Ne |
|moduly|pole|Ne |
|Stat|Nedefinováno|Ne |
|_links|pole|Ne |



### <a name="stats"></a>Stat


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|member_count|celé číslo|Ne |
|unsubscribe_count|celé číslo|Ne |
|cleaned_count|celé číslo|Ne |
|member_count_since_send|celé číslo|Ne |
|unsubscribe_count_since_send|celé číslo|Ne |
|cleaned_count_since_send|celé číslo|Ne |
|campaign_count|celé číslo|Ne |
|campaign_last_sent|celé číslo|Ne |
|merge_field_count|celé číslo|Ne |
|avg_sub_rate|číslo|Ne |
|avg_unsub_rate|číslo|Ne |
|target_sub_rate|číslo|Ne |
|open_rate|číslo|Ne |
|click_rate|číslo|Ne |
|last_sub_date|řetězec|Ne |
|last_unsub_date|řetězec|Ne |



### <a name="getlistsresponsemodel"></a>GetListsResponseModel


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|seznamy|pole|Ne |
|total_items|celé číslo|Ne |



### <a name="newmemberinlistrequest"></a>NewMemberInListRequest


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|email_type|řetězec|Ne |
|Stav|řetězec|Ano |
|merge_fields|Nedefinováno|Ne |
|zájmy|řetězec|Ne |
|jazyk|řetězec|Ne |
|virtuální|Logická hodnota|Ne |
|umístění|Nedefinováno|Ne |
|email_address|řetězec|Ano |



### <a name="firstandlastname"></a>FirstAndLastName


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|FNAME|řetězec|Ne |
|LNAME|řetězec|Ne |



### <a name="location"></a>Umístění


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|šířky|číslo|Ne |
|délky|číslo|Ne |



### <a name="memberresponsemodel"></a>MemberResponseModel


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|řetězec|Ne |
|email_address|řetězec|Ne |
|unique_email_id|řetězec|Ne |
|email_type|řetězec|Ne |
|Stav|řetězec|Ne |
|merge_fields|Nedefinováno|Ne |
|zájmy|řetězec|Ne |
|Stat|Nedefinováno|Ne |
|ip_signup|řetězec|Ne |
|timestamp_signup|řetězec|Ne |
|ip_opt|řetězec|Ne |
|timestamp_opt|řetězec|Ne |
|member_rating|celé číslo|Ne |
|last_changed|řetězec|Ne |
|jazyk|řetězec|Ne |
|virtuální|Logická hodnota|Ne |
|email_client|řetězec|Ne |
|umístění|Nedefinováno|Ne |
|last_note|Nedefinováno|Ne |
|list_id|řetězec|Ne |
|_links|pole|Ne |



### <a name="lastnote"></a>Last_Note


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|note_id|celé číslo|Ne |
|created_at|řetězec|Ne |
|created_by|řetězec|Ne |
|Poznámka:|řetězec|Ne |



### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Členové|pole|Ne |
|list_id|řetězec|Ne |
|total_items|celé číslo|Ne |



### <a name="object"></a>Objekt






### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|email_address|řetězec|Ne |
|email_type|řetězec|Ne |
|Stav|řetězec|Ano |
|merge_fields|Nedefinováno|Ne |
|zájmy|řetězec|Ne |
|jazyk|řetězec|Ne |
|virtuální|Logická hodnota|Ne |
|umístění|Nedefinováno|Ne |



### <a name="getmembersresponsemodel"></a>GetMembersResponseModel


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Členové|pole|Ne |
|list_id|řetězec|Ne |
|total_items|celé číslo|Ne |



### <a name="adduserresponsemodel"></a>AddUserResponseModel


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|řetězec|Ano |
|email_address|řetězec|Ano |
|unique_email_id|řetězec|Ne |
|email_type|řetězec|Ne |
|Stav|řetězec|Ne |
|merge_fields|Nedefinováno|Ano |
|zájmy|řetězec|Ne |
|Stat|Nedefinováno|Ne |
|ip_signup|řetězec|Ne |
|timestamp_signup|řetězec|Ne |
|ip_opt|řetězec|Ne |
|timestamp_opt|řetězec|Ne |
|member_rating|celé číslo|Ne |
|last_changed|řetězec|Ne |
|jazyk|řetězec|Ne |
|virtuální|Logická hodnota|Ne |
|email_client|řetězec|Ne |
|umístění|Nedefinováno|Ne |
|last_note|Nedefinováno|Ne |
|list_id|řetězec|Ne |
|_links|pole|Ne |


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)
