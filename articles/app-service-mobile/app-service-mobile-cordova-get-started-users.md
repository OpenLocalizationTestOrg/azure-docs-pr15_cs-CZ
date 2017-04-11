<properties
    pageTitle="Přidání ověřování na Apache Cordova s aplikací Mobile | Azure aplikace služby"
    description="Informace o používání mobilní aplikace v aplikaci služby Azure k ověření uživatele aplikace Apache Cordova celou řadu Zprostředkovatelé identit jiní, včetně Google, Facebooku, Twitteru a Microsoft."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Přidání ověřování aplikace Apache Cordova

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Souhrn

V tomto kurzu přidáte do projektu rychlý úvod seznam úkolů na Apache Cordova pomocí zprostředkovatele identit podporovaného ověřování. Tento kurz se podle kurz [Seznámení s aplikací Mobile] , které musíte nejdřív udělat.

##<a name="register"></a>Registrace aplikace pro ověřování a konfigurace aplikace služby

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Podívejte se na video zobrazující podobné kroky](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>Omezení oprávnění pro ověřeného uživatele

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Teď můžete ověřit, že byl zakázán anonymní přístup k vaší back-end. Ve Visual Studiu otevřete projekt, který jste vytvořili po dokončení kurz [Seznámení s aplikací Mobile], a pak spustíte aplikaci ve **Android emulátoru Google** a ověřte, že k neočekávané chybě připojení zobrazený po spuštění aplikace.

Dále se aktualizovat aplikaci k ověření uživatele před žádosti o zdrojů z back-end mobilní aplikaci.

##<a name="add-authentication"></a>Přidání ověřování do aplikace

1. Otevřete projekt ve **Visual Studiu**a potom ho otevřete `www/index.html` souboru pro úpravy.

2. Vyhledejte `Content-Security-Policy` metaznačky v části hlavní.  Bude muset přidat hostitele OAuth do seznamu povolených zdrojů.

  	| Zprostředkovatel               | Název poskytovatele SDK | Host (hostitel) OAuth                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure Active Directory | aad               | https://Login.Windows.NET   |
  	| Facebooku               | facebooku          | https://www.Facebook.com    |
  	| Google                 | Google            | https://accounts.Google.com |
  	| Microsoft              | MicrosoftAccount  | https://Login.live.com      |
  	| Twitter                | twitteru           | https://API.Twitter.com     |

    Příklad obsahu--zásady zabezpečení (implementovaná pro službu Azure Active Directory) vypadá takto:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    Měli byste nahradit `https://login.windows.net` od hostitele osobních OAuth z předchozí tabulky.  Další informace o tomto metaznačky naleznete v [si přečtěte následující dokumentaci zásady zabezpečení obsah] .

    Všimněte si, že někteří poskytovatelé ověřování není nutné zadávat, že zásady zabezpečení obsah se změní při použití na příslušný mobilních zařízeních.  Například žádné změny zásady zabezpečení obsah podporují při použití Google ověřování na zařízení s Androidem.

3. Otevřít `www/js/index.js` souboru pro úpravy, vyhledejte `onDeviceReady()` metody, a v části Vytvoření klienta kód přidejte následující:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Všimněte si, že tento kód nahradí existující kód, který vytvoří odkaz tabulky a aktualizuje uživatelského rozhraní.

    Metoda login() začíná na poskytovatele ověřování. Metoda login() je asynchronní funkci, která vrací JavaScript Promise.  Zbytek inicializace umístěn uvnitř odpověď promise tak, že nebude provedena až do dokončení metodu login().

4. V kódu, který jste právě přidali, nahraďte `SDK_Provider_Name` s názvem poskytovatele přihlášení. Například pro službu Azure Active Directory, použijte `client.login('aad')`.

4. Spuštění vašeho projektu.  Po dokončení projektu inicializace aplikace se zobrazí na přihlašovací stránku OAuth zprostředkovatel vybraný ověření.

##<a name="next-steps"></a>Další kroky

* Přečtěte si další [Informace o ověřování] aplikace službou Azure.
* Pokračujte kurzu přidáním [Nabízená oznámení] pro aplikace Apache Cordova.

Naučte se používat SDK.

* [Apache Cordova SDK]
* [Serverového SDK]
* [Node.js Server SDK]

<!-- URLs. -->
[Začínáme s aplikací Mobile]: app-service-mobile-cordova-get-started.md
[Si přečtěte následující dokumentaci zásady zabezpečení obsahu]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Nabízená oznámení]: app-service-mobile-cordova-get-started-push.md
[Informace o ověřování]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[Serverového SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
