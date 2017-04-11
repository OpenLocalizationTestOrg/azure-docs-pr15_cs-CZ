<properties
    pageTitle="Přidání spojnice uživatele Office 365 v aplikacích pro použití logických operátorů | Microsoft Azure"
    description="Základní informace o spojnice uživatele Office 365 s parametry rozhraní REST API"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Začínáme s konektoru uživatelé Office 365.

Připojení k Office 365 uživatelům profily uživatelů vyhledávání a další. 

>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2015 08 01 náhled schématu verze.

Uživatelé Office 365 můžete:

- Vytvoření vaše obchodní toku podle data, která se zobrazí uživatelům Office 365. 
- Použití akce, které získat přímé podřízené, pokud potřebujete profilů uživatelů vedoucího a další. Tyto akce se odpověď a proveďte umožňující příkazu jiné akce výstup. Například získat osoby přímé podřízené, provést tyto informace a aktualizovat záznamy v databázi SQL Azure. 

Operace v aplikacích pro použití logických operátorů přidáte v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce

Spojnice uživatele Office 365 má k dispozici následující akce: Existuje žádné aktivačních událostí.

| Aktivace | Akce|
| --- | --- |
|Žádná | <ul><li>Získat správce</li><li>Získání Můj profil</li><li>Získání přímých podřízených</li><li>Získání profilů uživatelů</li><li>Hledat uživatele</li></ul>|

Všechny spojnice podporují dat ve formátu JSON a XML. 


## <a name="create-a-connection-to-office-365-users"></a>Vytvoření připojení k uživatelé Office 365.

Tato spojnice přidáte ke svým aplikacím použití logických operátorů, musíte přihlásit ke svému účtu uživatele Office 365 a povolení aplikace logiky pro připojení ke svému účtu.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Po vytvoření připojení zadejte vlastnostech uživatele Office 365 jako ID uživatele. Tyto vlastnosti popisuje **rozhraní REST API odkazy** v tomto tématu.

>[AZURE.TIP] Toto stejné připojení uživatele Office 365 můžete použít v jiných aplikacích pro použití logických operátorů.


## <a name="office-365-users-rest-api-reference"></a>Odkaz rozhraní REST API pro uživatele Office 365
Platí pro verze: 1.0.

### <a name="get-my-profile"></a>Získání Můj profil 
Načte profil aktuálního uživatele.  
```GET: /users/me``` 

Neexistují žádné parametry pro tento hovor.

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|202|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


### <a name="get-user-profile"></a>Získání profilů uživatelů 
Načte konkrétní uživatelský profil.  
```GET: /users/{userId}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID uživatele|řetězec|Ano|Cesta|žádná|Hlavní jméno nebo e-mailu id uživatele|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|202|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


### <a name="get-manager"></a>Získat správce 
Načte profilu uživatele pro správce má zadaný uživatel.  
```GET: /users/{userId}/manager``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID uživatele|řetězec|Ano|Cesta|žádná|Hlavní jméno nebo e-mailu id uživatele|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|202|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|



### <a name="get-direct-reports"></a>Získání přímých podřízených 
Pokud potřebujete přímé podřízené.  
```GET: /users/{userId}/directReports``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID uživatele|řetězec|Ano|Cesta|žádná|Hlavní jméno nebo e-mailu id uživatele|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|202|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|



### <a name="search-for-users"></a>Hledat uživatele 
Načte Hledat výsledky profilů uživatelů.  
```GET: /users``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|searchTerm|řetězec|Ne|dotaz|žádná|Hledaný řetězec (platí pro: Zobrazit jména, křestní jméno, příjmení, pošta, pošta přezdívek a uživatel hlavní)|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|202|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|403|Zakázáno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|



## <a name="object-definitions"></a>Definice objektů

#### <a name="user-user-model-class"></a>Uživatel: Uživatel modelu třídy

|Název vlastnosti | Datový typ |Povinné
|---|---|---|
|DisplayName|řetězec|Ne|
|GivenName (jméno)|řetězec|Ne|
|Surname (příjmení)|řetězec|Ne|
|Pošta|řetězec|Ne|
|MailNickname|řetězec|Ne|
|TelephoneNumber|řetězec|Ne|
|AccountEnabled|Logická hodnota|Ne|
|ID|řetězec|Ano
|UserPrincipalName|řetězec|Ne|
|Oddělení|řetězec|Ne|
|Funkce|řetězec|Ne|
|mobilní telefon|řetězec|Ne|


## <a name="next-steps"></a>Další kroky

[Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

Přejděte zpět do [seznamu rozhraní API](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
