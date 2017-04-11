<properties
    pageTitle="Doplněk Office 365 Outlook connector aplikací logiky | Microsoft Azure"
    description="Vytváření aplikací pro použití logických operátorů s Office 365 spojnice k povolení interakce s Office 365. Příklad: vytváření, úpravy a aktualizace kontaktů a položek kalendáře."
    services=""    
    documentationCenter=""     
    authors="MandiOhlinger"    
    manager="anneta"    
    editor="" 
    tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="10/18/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-office-365-outlook-connector"></a>Začínáme s Office 365 Outlook connector 

Office 365 Outlook connector umožňuje interakce s Outlookem v Office 365. Tato spojnice umožňuje vytvořit, upravit a aktualizovat kontaktů a položek kalendáře a taky dostat, Odeslat a odpovídání na e-maily.

S Office 365 Outlook můžete:

- Vytvoření pracovního postupu pomocí funkce e-mailu a kalendáře v Office 365. 
- Pomocí aktivačních událostí lze spustit pracovní postup při nový e-mail, když se aktualizuje položky kalendáře a další.
- Použití akce Odeslat e-mail, vytvořte novou událost kalendáře a další. Například po nový objekt v služby Salesforce (aktivační) poslat e-mailu Office 365 Outlooku (akce). 

V tomto tématu se dozvíte, jak používat Office 365 Outlook connector v aplikaci použití logických operátorů a také aktivačními událostmi a akce.

>[AZURE.NOTE] Tuto verzi článku platí pro aplikace logiky všeobecně dostupná (GA).

Další informace o použití logických operátorů aplikace, najdete v článku [Co jsou logiky aplikace](../app-service-logic/app-service-logic-what-are-logic-apps.md) a [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-office-365"></a>Připojení ke službě Office 365

Pokud aplikace logiku získat přístup k jiné služby, nejprve vytvoříte *připojení* ke službě. Připojení umožňuje připojení mezi logiku aplikace a další služby. Například pro připojení k Office 365 Outlooku, musíte nejdřív *připojení*k Office 365. Pokud chcete vytvořit připojení, zadejte přihlašovací údaje, které obvykle používáte pro přístup ke službě, kterou chcete připojit k. Takže s Office 365 Outlook, zadejte její přihlašovací údaje ke svému účtu Office 365 k vytvoření připojení.


## <a name="create-the-connection"></a>Vytvoření připojení

>[AZURE.INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]

## <a name="use-a-trigger"></a>Použijte aktivační událost

Aktivační událost je u události, mohou sloužit k spuštění pracovního postupu definované v aplikaci použití logických operátorů. Aktivace "hlasování" službu na interval, počet_plateb, který chcete. [Další informace o aktivačních událostí](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Zadejte "office 365" k získání seznamu aktivační události v aplikaci použití logických operátorů:  

    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)

2. Vyberte **Office 365 Outlook – při spuštění brzy nadcházející události**. Pokud připojení už existuje, vyberte v rozevíracím seznamu kalendáře.

    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)

    Pokud se zobrazí výzva k přihlášení, pak zadejte znaménko podrobnosti o vytvoření připojení. [Vytvoření připojení](connectors-create-api-office365-outlook.md#create-the-connection) v tomto tématu jsou uvedené kroky. 

    > [AZURE.NOTE] V tomto příkladu se spustí aplikaci logiky při aktualizaci události kalendáře. Pokud chcete zobrazit výsledky tohoto aktivační událost, přidáte akci, která se odešle textovou zprávu. Například přidat akci Twilio *odeslání zprávy* , které znění při spuštění události kalendáře ve 15 minut. 

3. Klikněte na tlačítko **Upravit** a nastavte hodnoty **počet_plateb** a **Interval** . Například pokud chcete aktivační událost dotazování každých 15 minut, pak **frekvence** nastavte na **minuty**a nastavit **Interval** **15**. 

    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)

4. **Uložte** provedené změny (levého horního rohu panelu). Vaše aplikace logiku se uloží a může být automaticky zapnutá.


## <a name="use-an-action"></a>Použití akce

Akce je operace prováděné definované v aplikaci logiky pracovního postupu. [Další informace o akce](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Vyberte znaménko plus. Uvidíte několik možností: **Přidat akci**, **Přidat podmínku**nebo nějakého s **dalšími** možnostmi.

    ![](./media/connectors-create-api-office365-outlook/add-action.png)

2. Zvolte **Přidat akci**.

3. Do textového pole zadejte "office 365" zobrazte seznam všech dostupných akcí.

    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 

4. V našem příkladu vyberte **Office 365 Outlook - vytvořit kontakt**. Pokud připojení už existuje, klikněte na **Složku ID**, **Křestní jméno**a další vlastnosti:  

    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)

    Pokud se zobrazí výzva pro informace o připojení, zadejte podrobnosti o vytvoření připojení. [Vytvoření připojení](connectors-create-api-office365-outlook.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti. 

    > [AZURE.NOTE] V tomto příkladu jsme vytvoření nového kontaktu v Office 365 Outlook. Výstup jinou aktivační událost můžete vytvořit kontakt. Například přidáte aktivační událost služby SalesForce *Při vytvoření objektu* . Potom přidejte Office 365 Outlook, *obraťte se na vytvoření* akci, která používá služby SalesForce polí k vytvoření nové nový kontakt v Office 365. 

5. **Uložte** provedené změny (levého horního rohu panelu). Vaše aplikace logiku se uloží a může být automaticky zapnutá.


## <a name="technical-details"></a>Podrobné technické informace

Tady jsou podrobnosti o aktivačních událostí, akce a odpovědi, které podporuje připojení:

## <a name="office-365-triggers"></a>Aktivace Office 365

|Aktivační událost | Popis|
|--- | ---|
|[Při brzy spouštění nadcházející události](connectors-create-api-office365-outlook.md#when-an-upcoming-event-is-starting-soon)|Tuto operaci spustí tok při spuštění nadcházející události.|
|[Při příchodu nové e-mailu](connectors-create-api-office365-outlook.md#when-a-new-email-arrives)|Tuto operaci spustí tok příchod nové e-mailu|
|[Vytvoření nové události](connectors-create-api-office365-outlook.md#when-a-new-event-is-created)|Tuto operaci spustí tok, když se vytvoří nová událost v kalendáři.|
|[Změny zvláštní události](connectors-create-api-office365-outlook.md#when-an-event-is-modified)|Tuto operaci spustí tok při změně události v kalendáři.|


## <a name="office-365-actions"></a>Office 365 akce

|Akce|Popis|
|--- | ---|
|[Získejte e-mailů](connectors-create-api-office365-outlook.md#get-emails)|Tuto operaci získá e-mailů ve složce.|
|[Odeslání e-mailu](connectors-create-api-office365-outlook.md#send-an-email)|Tuto operaci odešle e-mailové zprávy.|
|[Odstranění e-mailu](connectors-create-api-office365-outlook.md#delete-email)|Tento odstranění e-mailu pomocí id.|
|[Označit jako přečtené](connectors-create-api-office365-outlook.md#mark-as-read)|Tuto operaci označí e-mailu jako přečtené.|
|[Odpověď na e-mailu](connectors-create-api-office365-outlook.md#reply-to-email)|Tuto operaci odpovědi na e-mailu.|
|[Získání přílohy](connectors-create-api-office365-outlook.md#get-attachment)|Tuto operaci získá přílohy e-mailu pomocí id.|
|[Odeslání e-mailu s možnostmi](connectors-create-api-office365-outlook.md#send-email-with-options)|Tuto operaci pošle e-mail s více možnostmi a čeká příjemce, aby odpoví zpět jednu z možností.|
|[Odeslání e-mailu schválení](connectors-create-api-office365-outlook.md#send-approval-email)|Tato operace poslala e-mail schválení a čekání na odpověď příjemce.|
|[Získání kalendáře](connectors-create-api-office365-outlook.md#get-calendars)|Tuto operaci jsou uvedeny dostupné kalendáře.|
|[Získání události](connectors-create-api-office365-outlook.md#get-events)|Tuto operaci získá události z kalendáře.|
|[Vytvoření události](connectors-create-api-office365-outlook.md#create-event)|Tuto operaci vytvoří novou událost v kalendáři.|
|[Získání události](connectors-create-api-office365-outlook.md#get-event)|Tuto operaci získá zvláštní událost z kalendáře.|
|[Odstranění události](connectors-create-api-office365-outlook.md#delete-event)|Tento odstranění události v kalendáři.|
|[Aktualizace události](connectors-create-api-office365-outlook.md#update-event)|Operace aktualizace událost v kalendáři.|
|[Získání složky kontaktů](connectors-create-api-office365-outlook.md#get-contact-folders)|Tuto operaci seznam složek dostupných kontaktů.|
|[Získání kontaktů](connectors-create-api-office365-outlook.md#get-contacts)|Tuto operaci získá kontakty ze složky Kontakty.|
|[Vytvoření kontaktu](connectors-create-api-office365-outlook.md#create-contact)|Tuto operaci vytvoří nový kontakt ve složce Kontakty.|
|[Získání kontaktů](connectors-create-api-office365-outlook.md#get-contact)|Tuto operaci získá konkrétní kontakt ze složky Kontakty.|
|[Odstranění kontaktu](connectors-create-api-office365-outlook.md#delete-contact)|Tuto operaci odstraní kontakt ze složky Kontakty.|
|[Aktualizovat kontakt](connectors-create-api-office365-outlook.md#update-contact)|Operace aktualizace kontakt ve složce Kontakty.|

### <a name="trigger-and-action-details"></a>Podrobnosti o aktivační události a akce

V této části najdete v článku konkrétní informace o jednotlivých aktivační události a akce, včetně všechny požadované nebo volitelné vstupní vlastnosti a odpovídající výstup přidružené spojnice.

#### <a name="when-an-upcoming-event-is-starting-soon"></a>Při brzy spouštění nadcházející události
Tuto operaci spustí tok při spuštění nadcházející události. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id kalendáře|Jedinečný identifikátor kalendáře|
|lookAheadTimeInMinutes|Vzhled napřed času|Čas (v minutách) můžete rovnou vyhledávat nadcházející události|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
CalendarItemsList: Seznam položek kalendáře

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|Hodnota|pole|Seznam položek kalendáře|


#### <a name="get-emails"></a>Získejte e-mailů
Tuto operaci získá e-mailů ve složce. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|cesta_ke_složce|Cestu ke složce|Cestu ke složce k načtení e-mailů (výchozí: "Složky Doručená pošta")|
|horní|Horní|Číslo e-mailů k načtení (výchozí: 10)|
|fetchOnlyUnread|Vzdálené použití jenom nepřečtených zpráv|Načtení pouze nepřečtených e-mailech?|
|includeAttachments|Zahrnutí příloh|Pokud nastavena na hodnotu true, přílohy budou také načtená společně s e-mailu|
|searchQuery|Vyhledávání|Hledání dotazu filtrovat e-mailů|
|Přeskočit|Přeskočit|Číslo e-mailů přeskočit (výchozí: 0)|
|skipToken|Přeskočit tokenu|Přejít token k načtení nové stránky|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
ReceiveMessage: Příjem e-mailové zprávy

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|Z|řetězec|Z|
|K|řetězec|K|
|Předmět|řetězec|Předmět|
|Textu|řetězec|Textu|
|Význam|řetězec|Význam|
|HasAttachment|Logická hodnota|Obsahuje přílohy|
|ID|řetězec|Id zprávy|
|IsRead|Logická hodnota|Je pro čtení|
|DateTimeReceived|řetězec|Datum a čas přijetí|
|Přílohy|pole|Přílohy|
|Kopie|řetězec|Zadejte e-mailové adresy oddělené středníkem jakosomeone@contoso.com|
|Skrytá|řetězec|Zadejte e-mailové adresy oddělené středníkem jakosomeone@contoso.com|
|IsHtml|Logická hodnota|Je ve formátu Html|


#### <a name="send-an-email"></a>Odeslání e-mailu
Tuto operaci odešle e-mailové zprávy. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|e-mailová zpráva *|E-mailu|E-mailu|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.

#### <a name="delete-email"></a>Odstranění e-mailu
Tento odstranění e-mailu pomocí id. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|messageId *|Id zprávy|ID e-mailu do odstranit|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.

#### <a name="mark-as-read"></a>Označit jako přečtené
Tuto operaci označí e-mailu jako přečtené. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|messageId *|Id zprávy|Přečtěte si ID e-mailu do označit jako|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.


#### <a name="reply-to-email"></a>Odpověď na e-mailu
Tuto operaci odpovědi na e-mailu. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|messageId *|Id zprávy|ID odpověď na e-mailu|
|Komentář *|Komentář|Odpovědět komentář|
|replyAll|Odpovědět všem|Odpovědět všem příjemcům|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.


#### <a name="get-attachment"></a>Získání přílohy
Tuto operaci získá přílohy e-mailu pomocí id. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|messageId *|Id zprávy|ID e-mailu|
|attachmentId *|Id přílohy|ID na přílohu pro stažení|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.


#### <a name="when-a-new-email-arrives"></a>Při příchodu nové e-mailu
Tuto operaci spustí tok příchod nové e-mailu.

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|cesta_ke_složce|Cestu ke složce|E-mailové složky k načtení (výchozí: doručené pošty)|
|k|K|Příjemců e-mailové adresy|
|z|Z|Z adresy|
|význam|Význam|Význam e-mailu (vysoké, Normální, minimum) (výchozí: normální)|
|fetchOnlyWithAttachment|Obsahuje přílohy|Obnovit jenom e-mailové zprávy s přílohou|
|includeAttachments|Zahrnutí příloh|Zahrnutí příloh|
|subjectFilter|Předmět filtru|Řetězec můžete vyhledávat v předmětu|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
TriggerBatchResponse [ReceiveMessage]

| Název vlastnosti | Datový typ |
|---|---|
|Hodnota|pole|


#### <a name="send-email-with-options"></a>Odeslání e-mailu s možnostmi
Tuto operaci pošle e-mail s více možnostmi a čeká příjemce, aby odpoví zpět jednu z možností. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|optionsEmailSubscription *|Předplatné žádost o možnosti e-mailu|Předplatné žádost o možnosti e-mailu|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
SubscriptionResponse: Model k odběru e-mailů schválení

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|ID|řetězec|ID předplatného|
|zdroje|řetězec|Zdroje požadavek na odběr|
|NotificationTyp|řetězec|Typ oznámení|
|notificationUrl|řetězec|Adresa Url upozornění|


#### <a name="send-approval-email"></a>Odeslání e-mailu schválení
Tato operace poslala e-mail schválení a čekání na odpověď příjemce. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|approvalEmailSubscription *|Předplatné požadavku na schvalování e-mailu|Předplatné požadavku na schvalování e-mailu|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
SubscriptionResponse: Model k odběru e-mailů schválení

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|ID|řetězec|ID předplatného|
|zdroje|řetězec|Zdroje požadavek na odběr|
|NotificationTyp|řetězec|Typ oznámení|
|notificationUrl|řetězec|Adresa Url upozornění|


#### <a name="get-calendars"></a>Získání kalendáře
Tuto operaci jsou uvedeny dostupné kalendáře. 

Neexistují žádné parametry pro tento hovor.

##### <a name="output-details"></a>Podrobnosti výstupu
TablesList

| Název vlastnosti | Datový typ |
|---|---|
|Hodnota|pole|


#### <a name="get-events"></a>Získání události
Tuto operaci získá události z kalendáře. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id kalendáře|Vyberte kalendář|
|$filter|Filtrování dotazu|Dotaz filtru ODATA omezení položky vrácené|
|$orderby|Řadit podle|Dotaz ODATA řadit podle určete pořadí položek|
|$skip|Přeskočit počet|Počet faktur od přeskočit (výchozí = 0)|
|$top|Získání maximální počet|Maximální počet položek k načtení (výchozí = 256)|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
CalendarEventList: Seznam položek kalendáře

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|Hodnota|pole|Seznam položek kalendáře|


#### <a name="create-event"></a>Vytvoření události
Tuto operaci vytvoří novou událost v kalendáři. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id kalendáře|Vyberte kalendář|
|položky *|Položky|Vytvoření události|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
CalendarEvent: Spojnice kalendář událostí modelu předmětu.

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|ID|řetězec|Jedinečný identifikátor požadovanou událost.|
|Účastníky|pole|Seznam účastníků události.|
|Textu|Nedefinováno|Text zprávy přidružené k události.|
|BodyPreview|řetězec|Náhled zprávy přidružené k události.|
|Kategorie|pole|Kategorie přidružené k události.|
|ChangeKey|řetězec|Určuje verzi objekt události. Pokaždé, když dojde ke změně události, změní se také ChangeKey.|
|DateTimeCreated|řetězec|Funkce data a času vytvoření události.|
|DateTimeLastModified|řetězec|Datum a čas poslední změny události.|
|Ukončení|řetězec|Čas ukončení události.|
|EndTimeZone|řetězec|Určuje časové pásmo schůzky a čas ukončení. Tato hodnota musí být definované v systému Windows (Příklad: "Tichomoří standardní").|
|HasAttachments|Logická hodnota|Nastavte na hodnotu true, pokud má událost přílohy.|
|Význam|řetězec|Význam události: minimum, normální nebo od nejvyšší.|
|IsAllDay|Logická hodnota|Nastavte na hodnotu true, pokud bude zvláštní událost trvat celý den.|
|IsCancelled|Logická hodnota|Nastavte na hodnotu true, pokud bylo zrušeno události.|
|IsOrganizer|Logická hodnota|Nastavte na hodnotu true, pokud je odesílatel taky Organizátor.|
|Umístění|Nedefinováno|Umístění události.|
|Organizátor|Nedefinováno|Organizátor události.|
|Opakování|Nedefinováno|Opakování události.|
|Připomenutí|celé číslo|Čas v minut před začátkem události, který vám později připomene.|
|ResponseRequested|Logická hodnota|Nastavte na hodnotu true, pokud odesílateli má odpověď při přijali nebo odmítli události.|
|ResponseStatus|Nedefinováno|Označuje typ odpovědi odeslané jako odpověď na zprávu o události.|
|SeriesMasterId|řetězec|Jedinečný identifikátor pro typ řady předlohy události.|
|ShowAs|řetězec|Zobrazuje jako volném.|
|Zahájení|řetězec|Počáteční čas dané události.|
|StartTimeZone|řetězec|Určuje časové pásmo schůzky spuštění. Tato hodnota musí být definované v systému Windows (Příklad: "Tichomoří standardní").|
|Předmět|řetězec|Předmět události.|
|Typ|řetězec|Typ události: jedna Instance, výskyt, výjimek ani na předlohu řady.|
|Webový odkaz|řetězec|Náhled zprávy přidružené k události.|


#### <a name="get-event"></a>Získání události
Tuto operaci získá zvláštní událost z kalendáře. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id kalendáře|Vyberte kalendář|
|ID *|Id položky|Vyberte událost|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
CalendarEvent: Spojnice kalendář událostí modelu předmětu.

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|ID|řetězec|Jedinečný identifikátor požadovanou událost.|
|Účastníky|pole|Seznam účastníků události.|
|Textu|Nedefinováno|Text zprávy přidružené k události.|
|BodyPreview|řetězec|Náhled zprávy přidružené k události.|
|Kategorie|pole|Kategorie přidružené k události.|
|ChangeKey|řetězec|Určuje verzi objekt události. Pokaždé, když dojde ke změně události, změní se také ChangeKey.|
|DateTimeCreated|řetězec|Funkce data a času vytvoření události.|
|DateTimeLastModified|řetězec|Datum a čas poslední změny události.|
|Ukončení|řetězec|Čas ukončení události.|
|EndTimeZone|řetězec|Určuje časové pásmo schůzky a čas ukončení. Tato hodnota musí být definované v systému Windows (Příklad: "Tichomoří standardní").|
|HasAttachments|Logická hodnota|Nastavte na hodnotu true, pokud má událost přílohy.|
|Význam|řetězec|Význam události: minimum, normální nebo od nejvyšší.|
|IsAllDay|Logická hodnota|Nastavte na hodnotu true, pokud bude zvláštní událost trvat celý den.|
|IsCancelled|Logická hodnota|Nastavte na hodnotu true, pokud bylo zrušeno události.|
|IsOrganizer|Logická hodnota|Nastavte na hodnotu true, pokud je odesílatel taky Organizátor.|
|Umístění|Nedefinováno|Umístění události.|
|Organizátor|Nedefinováno|Organizátor události.|
|Opakování|Nedefinováno|Opakování události.|
|Připomenutí|celé číslo|Čas v minut před začátkem události, který vám později připomene.|
|ResponseRequested|Logická hodnota|Nastavte na hodnotu true, pokud odesílateli má odpověď při přijali nebo odmítli události.|
|ResponseStatus|Nedefinováno|Označuje typ odpovědi odeslané jako odpověď na zprávu o události.|
|SeriesMasterId|řetězec|Jedinečný identifikátor pro typ řady předlohy události.|
|ShowAs|řetězec|Zobrazuje jako volném.|
|Zahájení|řetězec|Počáteční čas dané události.|
|StartTimeZone|řetězec|Určuje časové pásmo schůzky spuštění. Tato hodnota musí být definované v systému Windows (Příklad: "Tichomoří standardní").|
|Předmět|řetězec|Předmět události.|
|Typ|řetězec|Typ události: jedna Instance, výskyt, výjimek ani na předlohu řady.|
|Webový odkaz|řetězec|Náhled zprávy přidružené k události.|


#### <a name="delete-event"></a>Odstranění události
Tento odstranění události v kalendáři. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id kalendáře|Vyberte kalendář|
|ID *|ID|Vyberte událost|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.


#### <a name="update-event"></a>Aktualizace události
Operace aktualizace událost v kalendáři. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id kalendáře|Vyberte kalendář|
|ID *|ID|Vyberte událost|
|položky *|Položky|Událost aktualizovat|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
CalendarEvent: Spojnice kalendář událostí modelu předmětu.

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|ID|řetězec|Jedinečný identifikátor požadovanou událost.|
|Účastníky|pole|Seznam účastníků události.|
|Textu|Nedefinováno|Text zprávy přidružené k události.|
|BodyPreview|řetězec|Náhled zprávy přidružené k události.|
|Kategorie|pole|Kategorie přidružené k události.|
|ChangeKey|řetězec|Určuje verzi objekt události. Pokaždé, když dojde ke změně události, změní se také ChangeKey.|
|DateTimeCreated|řetězec|Funkce data a času vytvoření události.|
|DateTimeLastModified|řetězec|Datum a čas poslední změny události.|
|Ukončení|řetězec|Čas ukončení události.|
|EndTimeZone|řetězec|Určuje časové pásmo schůzky a čas ukončení. Tato hodnota musí být definované v systému Windows (Příklad: "Tichomoří standardní").|
|HasAttachments|Logická hodnota|Nastavte na hodnotu true, pokud má událost přílohy.|
|Význam|řetězec|Význam události: minimum, normální nebo od nejvyšší.|
|IsAllDay|Logická hodnota|Nastavte na hodnotu true, pokud bude zvláštní událost trvat celý den.|
|IsCancelled|Logická hodnota|Nastavte na hodnotu true, pokud bylo zrušeno události.|
|IsOrganizer|Logická hodnota|Nastavte na hodnotu true, pokud je odesílatel taky Organizátor.|
|Umístění|Nedefinováno|Umístění události.|
|Organizátor|Nedefinováno|Organizátor události.|
|Opakování|Nedefinováno|Opakování události.|
|Připomenutí|celé číslo|Čas v minut před začátkem události, který vám později připomene.|
|ResponseRequested|Logická hodnota|Nastavte na hodnotu true, pokud odesílateli má odpověď při přijali nebo odmítli události.|
|ResponseStatus|Nedefinováno|Označuje typ odpovědi odeslané jako odpověď na zprávu o události.|
|SeriesMasterId|řetězec|Jedinečný identifikátor pro typ řady předlohy události.|
|ShowAs|řetězec|Zobrazuje jako volném.|
|Zahájení|řetězec|Počáteční čas dané události.|
|StartTimeZone|řetězec|Určuje časové pásmo schůzky spuštění. Tato hodnota musí být definované v systému Windows (Příklad: "Tichomoří standardní").|
|Předmět|řetězec|Předmět události.|
|Typ|řetězec|Typ události: jedna Instance, výskyt, výjimek ani na předlohu řady.|
|Webový odkaz|řetězec|Náhled zprávy přidružené k události.|


#### <a name="when-a-new-event-is-created"></a>Vytvoření nové události
Tuto operaci spustí tok, když se vytvoří nová událost v kalendáři. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id kalendáře|Vyberte kalendář|
|$filter|Filtrování dotazu|Dotaz filtru ODATA omezení položky vrácené|
|$orderby|Řadit podle|Dotaz ODATA řadit podle určete pořadí položek|
|$skip|Přeskočit počet|Počet faktur od přeskočit (výchozí = 0)|
|$top|Získání maximální počet|Maximální počet položek k načtení (výchozí = 256)|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
CalendarItemsList: Seznam položek kalendáře

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|Hodnota|pole|Seznam položek kalendáře|


#### <a name="when-an-event-is-modified"></a>Změny zvláštní události
Tuto operaci spustí tok při změně události v kalendáři. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id kalendáře|Vyberte kalendář|
|$filter|Filtrování dotazu|Dotaz filtru ODATA omezení položky vrácené|
|$orderby|Řadit podle|Dotaz ODATA řadit podle určete pořadí položek|
|$skip|Přeskočit počet|Počet faktur od přeskočit (výchozí = 0)|
|$top|Získání maximální počet|Maximální počet položek k načtení (výchozí = 256)|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
CalendarItemsList: Seznam položek kalendáře


| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|Hodnota|pole|Seznam položek kalendáře|


#### <a name="get-contact-folders"></a>Získání složky kontaktů
Tuto operaci seznam složek dostupných kontaktů. 

Neexistují žádné parametry pro tento hovor.

##### <a name="output-details"></a>Podrobnosti výstupu
TablesList

| Název vlastnosti | Datový typ |
|---|---|
|Hodnota|pole|


#### <a name="get-contacts"></a>Získání kontaktů
Tuto operaci získá kontakty ze složky Kontakty. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id složky|Jedinečný identifikátor složku Kontakty k načtení|
|$filter|Filtrování dotazu|Dotaz filtru ODATA omezení položky vrácené|
|$orderby|Řadit podle|Dotaz ODATA řadit podle určete pořadí položek|
|$skip|Přeskočit počet|Počet faktur od přeskočit (výchozí = 0)|
|$top|Získání maximální počet|Maximální počet položek k načtení (výchozí = 256)|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
ContactList: Seznam kontaktů

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|Hodnota|pole|Seznam kontaktů|


#### <a name="create-contact"></a>Vytvoření kontaktu
Tuto operaci vytvoří nový kontakt ve složce Kontakty. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id složky|Vyberte složku kontaktů|
|položky *|Položky|Kontakt, který chcete vytvořit|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Kontakt: kontaktu

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|ID|řetězec|Jedinečný identifikátor kontaktu.|
|Atribut ParentFolderId|řetězec|ID nadřazené složky kontaktu|
|Narozenin|řetězec|Narozeniny kontaktu.|
|FileAs|řetězec|V části je zařazeno jméno kontaktu.|
|DisplayName|řetězec|Zobrazované jméno kontaktu.|
|GivenName (jméno)|řetězec|Na křestní jméno kontaktu.|
|Pole Iniciály|řetězec|Pole Iniciály kontaktu.|
|MiddleName|řetězec|Na druhé jméno kontaktu.|
|Popisný název|řetězec|Popisný název daného kontaktu.|
|Surname (příjmení)|řetězec|Příjmení kontaktu.|
|Název|řetězec|Název kontaktu.|
|Analytický nástroj Generátor|řetězec|Analytický nástroj Generátor kontaktu.|
|EmailAddresses|pole|Kontaktu e-mailové adresy.|
|ImAddresses|pole|Kontaktu rychlých zpráv (IM) adresy.|
|Funkce|řetězec|Kontaktu.|
|Firma|řetězec|Název firmy kontaktu.|
|Oddělení|řetězec|Oddělení kontaktu.|
|OfficeLocation|řetězec|Umístění office kontaktu.|
|Povolání|řetězec|Povolání kontaktu.|
|BusinessHomePage|řetězec|Domovská stránka obchodní kontakt.|
|AssistantName|řetězec|Jméno kontaktu asistenta.|
|Správce|řetězec|Jméno kontaktu správce.|
|HomePhones|pole|Telefon domů čísla kontaktu.|
|BusinessPhones|pole|Kontaktu telefonních čísel|
|MobilePhone1|řetězec|Číslo mobilního telefonu kontaktu.|
|AdresaDomů|Nedefinováno|Na výchozí adresu kontaktu.|
|BusinessAddress|Nedefinováno|Adresy obchodního kontaktu.|
|OtherAddress|Nedefinováno|Další adresy kontaktu.|
|YomiCompanyName|řetězec|Zvukové japonský společnosti jméno kontaktu.|
|YomiGivenName|řetězec|Zvukové japonské zadaný název (křestní jméno) kontaktu.|
|YomiSurname|řetězec|Příjmení japonština phonetic (příjmení) kontaktu|
|Kategorie|pole|Kategorie přidružené ke kontaktu.|
|ChangeKey|řetězec|Určuje verzi objekt události|
|DateTimeCreated|řetězec|Čas vytvoření kontakt.|
|DateTimeLastModified|řetězec|Čas, kdy byl upraven kontaktu.|


#### <a name="get-contact"></a>Získání kontaktů
Tuto operaci získá konkrétní kontakt ze složky Kontakty. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id složky|Vyberte složku kontaktů|
|ID *|Id položky|Jedinečný identifikátor kontaktu k načtení|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Kontakt: kontaktu

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|ID|řetězec|Jedinečný identifikátor kontaktu.|
|Atribut ParentFolderId|řetězec|ID nadřazené složky kontaktu|
|Narozenin|řetězec|Narozeniny kontaktu.|
|FileAs|řetězec|V části je zařazeno jméno kontaktu.|
|DisplayName|řetězec|Zobrazované jméno kontaktu.|
|GivenName (jméno)|řetězec|Na křestní jméno kontaktu.|
|Pole Iniciály|řetězec|Pole Iniciály kontaktu.|
|MiddleName|řetězec|Na druhé jméno kontaktu.|
|Popisný název|řetězec|Popisný název daného kontaktu.|
|Surname (příjmení)|řetězec|Příjmení kontaktu.|
|Název|řetězec|Název kontaktu.|
|Analytický nástroj Generátor|řetězec|Analytický nástroj Generátor kontaktu.|
|EmailAddresses|pole|Kontaktu e-mailové adresy.|
|ImAddresses|pole|Kontaktu rychlých zpráv (IM) adresy.|
|Funkce|řetězec|Kontaktu.|
|Firma|řetězec|Název firmy kontaktu.|
|Oddělení|řetězec|Oddělení kontaktu.|
|OfficeLocation|řetězec|Umístění office kontaktu.|
|Povolání|řetězec|Povolání kontaktu.|
|BusinessHomePage|řetězec|Domovská stránka obchodní kontakt.|
|AssistantName|řetězec|Jméno kontaktu asistenta.|
|Správce|řetězec|Jméno kontaktu správce.|
|HomePhones|pole|Telefon domů čísla kontaktu.|
|BusinessPhones|pole|Kontaktu telefonních čísel|
|MobilePhone1|řetězec|Číslo mobilního telefonu kontaktu.|
|AdresaDomů|Nedefinováno|Na výchozí adresu kontaktu.|
|BusinessAddress|Nedefinováno|Adresy obchodního kontaktu.|
|OtherAddress|Nedefinováno|Další adresy kontaktu.|
|YomiCompanyName|řetězec|Zvukové japonský společnosti jméno kontaktu.|
|YomiGivenName|řetězec|Zvukové japonské zadaný název (křestní jméno) kontaktu.|
|YomiSurname|řetězec|Příjmení japonština phonetic (příjmení) kontaktu|
|Kategorie|pole|Kategorie přidružené ke kontaktu.|
|ChangeKey|řetězec|Určuje verzi objekt události|
|DateTimeCreated|řetězec|Čas vytvoření kontakt.|
|DateTimeLastModified|řetězec|Čas, kdy byl upraven kontaktu.|


#### <a name="delete-contact"></a>Odstranění kontaktu
Tuto operaci odstraní kontakt ze složky Kontakty. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id složky|Vyberte složku kontaktů|
|ID *|ID|Jedinečný identifikátor kontakt, který chcete odstranit|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.


#### <a name="update-contact"></a>Aktualizovat kontakt
Operace aktualizace kontakt ve složce Kontakty. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Id složky|Vyberte složku kontaktů|
|ID *|ID|Jedinečný identifikátor kontakt, který chcete aktualizovat|
|položky *|Položky|Kontaktní položku, kterou chcete aktualizovat|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Kontakt: kontaktu

| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|ID|řetězec|Jedinečný identifikátor kontaktu.|
|Atribut ParentFolderId|řetězec|ID nadřazené složky kontaktu|
|Narozenin|řetězec|Narozeniny kontaktu.|
|FileAs|řetězec|V části je zařazeno jméno kontaktu.|
|DisplayName|řetězec|Zobrazované jméno kontaktu.|
|GivenName (jméno)|řetězec|Na křestní jméno kontaktu.|
|Pole Iniciály|řetězec|Pole Iniciály kontaktu.|
|MiddleName|řetězec|Na druhé jméno kontaktu.|
|Popisný název|řetězec|Popisný název daného kontaktu.|
|Surname (příjmení)|řetězec|Příjmení kontaktu.|
|Název|řetězec|Název kontaktu.|
|Analytický nástroj Generátor|řetězec|Analytický nástroj Generátor kontaktu.|
|EmailAddresses|pole|Kontaktu e-mailové adresy.|
|ImAddresses|pole|Kontaktu rychlých zpráv (IM) adresy.|
|Funkce|řetězec|Kontaktu.|
|Firma|řetězec|Název firmy kontaktu.|
|Oddělení|řetězec|Oddělení kontaktu.|
|OfficeLocation|řetězec|Umístění office kontaktu.|
|Povolání|řetězec|Povolání kontaktu.|
|BusinessHomePage|řetězec|Domovská stránka obchodní kontakt.|
|AssistantName|řetězec|Jméno kontaktu asistenta.|
|Správce|řetězec|Jméno kontaktu správce.|
|HomePhones|pole|Telefon domů čísla kontaktu.|
|BusinessPhones|pole|Kontaktu telefonních čísel|
|MobilePhone1|řetězec|Číslo mobilního telefonu kontaktu.|
|AdresaDomů|Nedefinováno|Na výchozí adresu kontaktu.|
|BusinessAddress|Nedefinováno|Adresy obchodního kontaktu.|
|OtherAddress|Nedefinováno|Další adresy kontaktu.|
|YomiCompanyName|řetězec|Zvukové japonský společnosti jméno kontaktu.|
|YomiGivenName|řetězec|Zvukové japonské zadaný název (křestní jméno) kontaktu.|
|YomiSurname|řetězec|Příjmení japonština phonetic (příjmení) kontaktu|
|Kategorie|pole|Kategorie přidružené ke kontaktu.|
|ChangeKey|řetězec|Určuje verzi objekt události|
|DateTimeCreated|řetězec|Čas vytvoření kontakt.|
|DateTimeLastModified|řetězec|Čas, kdy byl upraven kontaktu.|



## <a name="http-responses"></a>Odpovědi na HTTP

Jeden nebo více z následujících kódů stav HTTP se můžete vrátit akcí a aktivační události nad: 

|Jméno|Popis|
|---|---|
|200|Ok|
|202|Přijaté|
|400|Chybný požadavek|
|401|Neoprávněnému|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="next-steps"></a>Další kroky

[Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md). Prozkoumejte jiných spojnice k dispozici v aplikacích pro použití logických operátorů v našem [seznamu rozhraní API](apis-list.md).