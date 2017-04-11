<properties 
    pageTitle="Jak udělit oprávnění uživatele registraci a produkčních předplatného" 
    description="Zjistěte, jak delegování uživatelů registraci a produkčních předplatné třetí straně v části Správa rozhraní API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Jak udělit oprávnění uživatele registraci a produkčních předplatného

Delegování umožňuje použití stávajícího webu pro zpracování vývojář znaménko – v/sign-množství předplatného a odpovídaly produktům namísto použití předdefinované funkce v portálu pro vývojáře. Díky váš web vlastní uživatelská data a ověření takto vlastní výzvu.

## <a name="delegate-signin-up"> </a>Delegování vývojář přihlašování a registrace

Delegování vývojář přihlašování a registrace na stávající web je potřeba vytvořit speciální delegování koncového bodu na webu, která funguje jako vstupní bod pro takové žádosti zahájená z portálu pro vývojáře rozhraní API správy.

Poslední pracovní postup bude následujícím způsobem:

1. Karta Vývojář kliknutí na přihlašovací nebo registrace odkaz v portálu pro vývojáře rozhraní API správy
2. Prohlížeč přesměrován koncový bod delegování
3. Koncový bod delegování naopak přesměruje nebo představuje uživatelské rozhraní s dotazem, přihlásit a registrace uživatelům
4. Na úspěšné bude uživatel přesměrován zpátky na stránku portálu správy rozhraní API vývojář, spuštění z


Chcete-li začít Pojďme první správy nastavuje rozhraní API pro směrování žádosti o prostřednictvím koncový bod delegování. Na portálu Správa rozhraní API aplikace publisher klikněte na **zabezpečení** a potom klikněte na kartu **delegování** . Klepnutím na zaškrtávací políčko Povolit delegáta přihlašovací & registrace.

![Delegování stránky][api-management-delegation-signin-up]

* Rozhodněte, co URL koncový bod zvláštní delegování bude a zadejte do pole **Adresa URL delegování koncového bodu** . 

* V poli **Delegování ověřování klíč** zadejte tajná, která se použije pro výpočet podpisu, které vám k ověření zajistit, že žádosti skutečně pocházejících z Azure rozhraní API správy. Kliknutím na tlačítko **Generovat** mít rozhraní API Managemnet náhodně vygenerovat klíč za vás.

Teď je potřeba vytvořit **delegování koncový bod**. Je třeba konfigurovat celou řadu akcí:

1. Přijmout žádost o následujícím způsobem:

    > *http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl= {URL zdrojovou stránku} & sůl = {řetězec} & podpis = {řetězec}*

    Parametry dotazu pro případ přihlašovací / registrace:
    - **operace**: označuje jaký druh delegování požádáte je – **přihlašovacího** může být pouze v tomto případě
    - **returnUrl**: adresu URL na stránku, kde uživatel kliknutí na odkaz přihlásit nebo registrace
    - **soli**: speciální soli řetězec používá pro výpočet hash zabezpečení
    - **podpis**: hash počítanou zabezpečení pro porovnání s vlastními Vypočítaný algoritmus hash

2. Ověřte, že žádosti pochází Azure rozhraní API Management (volitelný, ale velmi doporučené cenného papíru)

    * Výpočet algoritmus hash HMAC SHA512 řetězce na základě dotazu parametrů **returnUrl** a **soli** ([Příklad uvedeny níže]):
        > HMAC (**soli** "\n" + **returnUrl**)
         
    * Porovnejte hash nad vypočítané hodnoty parametru dotazu **podpis** . Pokud dvě hodnoty hash shodují, přejděte k dalšímu kroku, jinak odepřít žádost.

2. Ověřte, že jste obdrželi žádost o znak nebo znaménko šipka nahoru: Nastaví parametr dotazu **operace** "**přihlašovacího**".

3. Prezentovat uživatelského rozhraní pro přihlášení nebo registrace

4. Pokud uživatel je odhlášení nakreslenými budete muset vytvořit účet odpovídající jejich správy rozhraní API. [Vytvoření uživatele] pomocí rozhraní API správy REST API. Přitom, ověřte, zda je ID uživatele do stejného, který je ve vašem úložišti uživatele nebo ID, které jste měli přehled o.

5. Když uživatel úspěšně ověřené:

    * [žádost o token jednotného přihlašování (SSO)] prostřednictvím rozhraní API správy REST API

    * připojení k dotazu parametr returnUrl na adresu URL jednotné přihlašování jste přijali z volání rozhraní API nad:
        > například https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 

    * přesměrujte uživatele na výše výroby adresy URL

Kromě operaci **přihlašovacího** můžete taky provést Správa účtů podle předchozího postupu a používat jeden z následujících operací.

-   **Změna hesla byla**
-   **ChangeProfile**
-   **CloseAccount**

Musí uplynout následující parametry dotazu pro operace správy účtu.

-   **operace**: identifikuje jaký druh požadavek delegování je (Změna hesla byla ChangeProfile a CloseAccount)
-   **ID uživatele**: id uživatele účet, který chcete spravovat
-   **soli**: speciální soli řetězec používá pro výpočet hash zabezpečení
-   **podpis**: hash počítanou zabezpečení pro porovnání s vlastními Vypočítaný algoritmus hash

## <a name="delegate-product-subscription"> </a>Delegování předplatné s kódem product

Právo k delegačnímu předplatné s kódem product funguje podobně delegování uživatelů přihlašovací/zdola nahoru. Poslední pracovní postup by vypadal takto:

1. Karta Vývojář vybere produktu v portálu pro vývojáře správy rozhraní API a klepne na tlačítko Přihlásit odběr
2. Prohlížeč přesměrován koncový bod delegování
3. Koncový bod delegování provede kroky předplatné požadovaný produkt – to záleží na vás a mohou vést k přesměrování na jinou stránku požádat o další informace, otázky v dalších, nebo jednoduše uložení informací a které nevyžadují všech akcí uživatele


Chcete-li povolit funkci na stránce **delegování** klikněte na **předplatné s kódem product delegáta**.

Přesvědčte se, že koncový bod delegování provede následující akce:


1. Přijmout žádost o následujícím způsobem:

    > *http://www.yourwebsite.com/apimdelegation?operation= {operace} & Idproduktu = {produkt přihlášení k odběru} & ID = {uživatele, který tvoří žádost o} & soli = {řetězec} & podpis = {řetězec}*

    Parametry dotazu pro případ produktu předplatného:
    - **operace**: označuje, jaký druh požadavek delegování je. Pro předplatné s kódem product žádosti o platné možnosti jsou:
        - "Odběr": žádost o přihlášení k odběru uživateli daný výrobek pomocí podle ID (viz níže)
        - "Odběr": požadavek na odhlášení uživatele k produktu
        - "Obnovit": žádost potřeba obnovit předplatné (například, které mohou být vyprší platnost)
    - **Idproduktu**: ID produktu uživatel požadovány k přihlášení k odběru
    - **ID uživatele**: ID uživatele, kterému je žádost volání
    - **soli**: speciální soli řetězec používá pro výpočet hash zabezpečení
    - **podpis**: hash počítanou zabezpečení pro porovnání s vlastními Vypočítaný algoritmus hash


2. Ověřte, že žádosti pochází Azure rozhraní API Management (volitelný, ale velmi doporučené cenného papíru)

    * Výpočet SHA512 HMAC řetězce podle **Idproduktu**, **ID uživatele** a **soli** parametry dotazu:
        > HMAC (**soli** "\n" + **Idproduktu** + "\n" + **ID uživatele**)
         
    * Porovnejte hash nad vypočítané hodnoty parametru dotazu **podpis** . Pokud dvě hodnoty hash shodují, přejděte k dalšímu kroku, jinak odepřít žádost.
    
3. Proveďte jakékoli zpracování předplatné produktu v závislosti na typu operaci v **operaci** – například fakturace další otázky, atd.

4. Na úspěšně přihlášení k odběru uživatele dělený součinem jejich na vaší straně, odběr uživatele dělený součinem jejich správy rozhraní API tak, že [zavoláte rozhraní REST API pro předplatné s kódem product].

## <a name="delegate-example-code"></a> Příklad ##

Tyto příklady kódu ukazují, jak provádět *delegování ověřovací klíč*, který je nastavený na obrazovce delegování portálu publisher vytvořit HMAC, pak používaný k ověření podpisu, ověřením platnosti předané returnUrl. Téhož kódu vyhovovat Idproduktu a ID uživatele s mírně upraven.

**Kód C# ke generování hash returnUrl**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**Kód NodeJS ke generování hash returnUrl**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>Další kroky

Další informace o delegování najdete v článku následující video.

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[žádost o token jednotného přihlašování (SSO)]: http://go.microsoft.com/fwlink/?LinkId=507409
[vytvoření uživatele]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[volání rozhraní REST API pro předplatné s kódem product]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[Příklad uvedeny níže]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 