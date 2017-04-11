<properties
    pageTitle="Azure AD verze 2.0 AngularJS Začínáme | Microsoft Azure"
    description="Vytvoření aplikace úhlu JS jednu stránku, která přihlášení uživatelé, kteří mají obou osobní účty Microsoft a pracovní nebo školní účty."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="add-sign-in-to-an-angularjs-single-page-app---nodejs"></a>Přidání přihlášení do aplikace jedné stránky AngularJS - NodeJS

V tomto článku přičteme, přihlaste se pomocí účtů Microsoft technologii aplikace AngularJS pomocí koncový bod verze 2.0 Azure Active Directory. koncový bod verze 2.0 umožňují provést jeden integrace v aplikaci a ověření uživatele s osobní a pracovní/školní účty.

V tomto příkladu je jednoduchý aplikace jedné stránky seznam úkolů, který ukládá úkoly do back-end rozhraní REST API, napsané v NodeJS a zabezpečit OAuth nosný tokenů z Azure AD pomocí.  Aplikaci AngularJS použije naše otevřít zdroj JavaScript ověřování knihovny [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) zpracovat celý procesu přihlášení a získání tokeny pro volání rozhraní REST API.  Stejný vzorek se dají použít k ověření jiných ZBÝVAJÍCÍCH API, jako je [Microsoft Graph](https://graph.microsoft.com) nebo rozhraní API Správce prostředků Azure.

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

## <a name="download"></a>Ke stažení

Nejdřív musíte stažení a instalace [node.js](https://nodejs.org).  Pak můžete kopírovat nebo [Stáhnout](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) kostry aplikace:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

Aplikaci kostry obsahuje kód často používaný jednoduché AngularJS aplikace, ale chybí všechny použitelné týkající se identity.  Pokud nechcete sledovat, můžete místo toho klonovat nebo [Stáhnout](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) dokončeným ukázkovým.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a>Registrace aplikace

Nejdřív vytvořte aplikace [Portál registrace aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nebo postupujte podle těchto [podrobných pokynů](active-directory-v2-app-registration.md).  Zkontrolujte, že:

- Přidání platformu **webové** aplikace.
- Zadejte správné **Přesměrovat URI**. Výchozí v tomto příkladu je `http://localhost:8080`.
- Nechte políčko **Povolit implicitní tok** povolené. 

Kopírovat dolů **ID aplikace** přiřazené k aplikaci, musíte ho brzy. 

## <a name="install-adaljs"></a>Instalace adal.js
Pokud chcete spustit, přejděte do projektu můžete stáhnout a nainstalovat adal.js.  Pokud máte [bower](http://bower.io/) nainstalovaný, můžete použít jenom tento příkaz.  Pro všechny problémy, verze závislosti zvolte vyšší verzi.
```
bower install adal-angular#experimental
```

Můžete taky můžete stáhnout ručně [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) a [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Přidání oba soubory a `app/lib/adal-angular-experimental/dist` adresář.

Nyní otevřete projekt v oblíbených textovém editoru a načíst adal.js na konci stránky textu:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>Nastavení rozhraní REST API

Během jsme nastavujete věci, umožní získat pracovní rozhraní REST API back-end.  Na příkazovém řádku nainstalujte všechny potřebné balíčků spuštěním (ujistit, že jsou v adresáři nejvyšší úrovně projektu):

```
npm install
```

Nyní otevřete `config.js` a nahradit `audience` hodnotu:

```js
exports.creds = {
     
     // TODO: Replace this value with the Application ID from the registration portal
     audience: '<Your-application-id>',
     
     ...
}
```

Rozhraní REST API použijete tuto hodnotu k ověření tokenů, které obdrží z aplikace úhlu na žádosti o AJAX.  Poznámka: Tento jednoduchý rozhraní REST API uchovává data v paměti - tak každý čas ukončení serveru, budou ztraceny všechny dříve vytvořené úkoly.

To je vždy, které chceme věnovat projednávat fungování rozhraní REST API.  Neváhejte Vyvrtejte v kódu, ale pokud chcete se dozvědět víc o zabezpečení webového rozhraní API s Azure AD, podívejte se na [Tento článek](active-directory-v2-devquickstarts-node-api.md). 

## <a name="sign-users-in"></a>Symbol uživatelé v
Některé kódu identity je čas.  Možná jste si už všimli, že adal.js obsahuje AngularJS zprostředkovatele, který se přehraje dobře úhlu směrování mechanismy.  Začněte tím, že přidání modulu adal do aplikace:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Teď můžete inicializace `adalProvider` s ID aplikace:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Vysoký adal.js se všechny informace, které potřebuje k zabezpečení aplikace a přihlášení uživatele.  Chcete-li vynutit přihlásit pro daný postup v aplikaci náročnosti stačí jeden řádek kódu:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Teď při kliknutí `TodoList` odkaz, adal.js bude automaticky přesměrovávat na Azure AD pro přihlášení v případě potřeby.  Můžete také explicitně odesíláte žádosti o přihlášení a odhlašovací vyvoláním adal.js v seznamu svého zařízení:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Zobrazit informace o uživateli
Teď, když je uživatel přihlášený, pravděpodobně musíte pro přístup k datům ověřování přihlášený uživatele v aplikaci.  Adal.js poskytuje tyto informace o můžete v `userInfo` objektu.  Přístup k tomuto objektu v zobrazení, napřed přidáte adal.js do kořenové oboru odpovídající řadiče domény:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Potom přímo můžete řešit jenom `userInfo` objekt v zobrazení: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Můžete taky použít `userInfo` objekt, který chcete určit, pokud je uživatel přihlášený nebo ne.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>Volání rozhraní REST API
Nakonec je čas získat některé tokeny a volání rozhraní REST API vytvářet, číst, aktualizovat a odstraňovat úkoly.  A co s tím?  Nemusíte dělat *určité věci*.  Adal.js bude automaticky starat o začíná, ukládání do mezipaměti a aktualizace tokeny.  Je taky starat o programovém připojování těchto tokenů k odchozí AJAX požadavky, které odesílají rozhraní REST API.  

Jak přesně to funguje? Je všechno vám magické [zachycovacích AngularJS dotazů](https://docs.angularjs.org/api/ng/service/$http), které umožňuje adal.js k transformaci příchozích a odchozích zpráv http.  Kromě toho adal.js se předpokládá, že všechny žádosti o posílat do stejné domény podle okna by měl použít tokeny určené pro stejný ID aplikace AngularJS aplikace.  Z tohoto důvodu použijeme stejnou ID aplikace v úhlu aplikace a rozhraní REST API NodeJS.  Samozřejmě můžete toto nastavení změnit a nástroji řekněte adal.js zobrazíte tokeny pro ostatní REST API v případě potřeby -, ale pro tento jednoduchý scénář udělá výchozí hodnoty.

Tady je fragmentů kódu, který ukazuje, jak je snadné poslat požadavků nosný tokenů z Azure AD:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Blahopřejeme!  Teď je dokončeno aplikace integrované jedné stránky Azure AD.  Pokračujte, trvat úklonou.  Ho můžete ověřování uživatelů bezpečně volání jeho back-end rozhraní REST API pomocí OpenID spojení a získat základní informace o uživateli.  Odhlášení z pole podporuje všichni uživatelé s osobním Account Microsoft nebo pracovní/školní účet z Azure AD.  Vyzkoušejte aplikaci tokem poš:

```
node server.js
```

V prohlížeči přejděte na `http://localhost:8080`.  Přihlášení pomocí osobního účtu Microsoft nebo pracovní/školní účet.  Přidání úkolů do seznamu úkolů uživatele a odhlásit se.  Zkuste se přihlásit jiný typ účtu. Pokud potřebujete tenanta Azure AD pro vytvoření pracovní/školní uživatelů [Zjistěte, jak si ho založit tady](active-directory-howto-tenant.md) (je to zadarmo).

Pokračujte získání informací o koncový bod verze 2.0 vedoucí zpět na našeho [Průvodce vývojář verze 2.0](active-directory-appmodel-v2-overview.md).  Další zdroje informací najdete v tématech:

- [Azure vzorky na GitHub >>](https://github.com/Azure-Samples)
- [Azure AD na přetečení zásobníku >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure AD si přečtěte následující dokumentaci na [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Aktualizace zabezpečení pro naše produkty

Budeme rádi, když chcete dostávat oznámení ze kdy zabezpečení incidentem probíhala po návštěvě [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru poradní výstrah zabezpečení.
