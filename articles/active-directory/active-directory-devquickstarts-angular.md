<properties
    pageTitle="Azure AD AngularJS Začínáme | Microsoft Azure"
    description="Jak vytvořit aplikaci úhlu JS jednu stránku, která lze integrovat s Azure AD pro přihlášení a volá Azure AD zamknutí rozhraní API pomocí OAuth."
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


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>Zabezpečení AngularJS jedné stránky aplikací s Azure AD

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD umožňuje rychle a jednoduše přidat znak v odhlásit a zabezpečené volání rozhraní API OAuth ke svým aplikacím jedné stránky.  Umožňuje aplikace ověřování uživatelů s jejich účty služby Active Directory a používat všechny webového rozhraní API chráněny Azure AD, například rozhraní API Office 365 nebo rozhraní API Azure.

Azure AD pro javascript spuštěné v prohlížeči, obsahuje Active Directory Authentication Library nebo adal.js.  Adal.js je jediný v životnost slouží k Usnadněte aplikace získat přístup tokeny.  Která ukazuje, jak snadno, tady abychom budete vytvářet aplikace AngularJS se seznamem úkolů, který:

- Uživatel přihlásí k aplikaci jako zprostředkovatele identit s Azure AD.
- Zobrazuje některé informace o uživateli.
- Bezpečné zavolá v aplikaci pro proveďte seznamu API používání nosný tokenů z AAD.
- Přihlásí uživatele mimo aplikaci.

Vytvoření dokončení aplikace fungovat, musíte:

2. Zaregistrujte aplikace v Azure AD.
3. Instalace ADAL a konfigurace zabezpečené ověřování HESLA.
5. Pomocí ADAL zabezpečené stránky v zabezpečené ověřování HESLA.

Jak začít, [Stáhněte si aplikaci osnovu](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) nebo [Stáhnout dokončeným ukázkovým](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Taky musíte tenanta Azure AD, ve kterém můžete vytvářet uživatele a registrace aplikace.  Pokud ještě nemáte klienta, [Přečtěte si, jak si ho založit](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. aplikace DirectorySearcher zaregistrovat*
Povolení aplikace ověřování uživatelů a získat tokeny, Nejdřív musíte zaregistrovat ve vašem klientovi Azure AD:

-   Přihlaste se k [portálu Správa Azure](https://manage.windowsazure.com)
-   V levém navigačním podokně klikněte na **Služby Active Directory**
-   Vyberte klienta, ve kterém si zaregistrovat aplikace.
-   Klikněte na kartu **aplikace** a klikněte na **Přidat** do dolní okraj.
-   Postupujte podle pokynů a vytvořte nové **webové aplikace a/nebo WebAPI**.
    -   **Název** aplikace bude popisovat aplikace koncovým uživatelům.
    -   **Přesměrovat Uri** je umístění, do kterého AAD vrátí tokeny.  Výchozí umístění v tomto příkladu je`https://localhost:44326/`
-   Po dokončení zápisu AAD přiřadí aplikace jedinečné **ID klienta**.  Musíte mít tuto hodnotu v dalších částech tak zkopírujte ho na kartě **Konfigurovat** .
- Adal.js používá pro komunikaci s Azure AD implicitní tok OAuth.  Implicitní toku musí povolit aplikace tak, že:
    - Stáhněte si manifest aplikace kliknutím na **Spravovat Manifest**.
    - Otevřete manifest a vyhledejte `oauth2AllowImplicitFlow` vlastnost. Nastavte jeho hodnotu `true`.
    - Uložit a odeslat manifest aplikace kliknutím na **Spravovat Manifest** ještě jednou.

## <a name="2-install-adal--configure-the-spa"></a>*2. Nainstalujte ADAL a konfigurace zabezpečené ověřování HESLA*
Teď, když máte aplikace v Azure AD, můžete nainstalovat adal.js a zadejte kód týkající se identity.

-   Začněte tím, že přidáte adal.js TodoSPA projektu pomocí konzole balíčku správce:
  - Stáhněte si [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) a přidejte ji `App/Scripts/` adresáře projektu.
  - Stáhněte si [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) a přidejte ji `App/Scripts/` adresáře projektu.
  - Načtení každý skript před skončením `</body>` v `index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   Aby zabezpečené ověřování HESLA back-end k udělat seznam rozhraní API přijmout tokenů z prohlížeče musí back-end Konfigurace informací o registraci aplikace. V projectu TodoSPA otevřete `web.config`.  Nahradit hodnoty prvků v `<appSettings>` oddíl tak, aby odrážely hodnoty vkládáte do portálu Azure.  Pokaždé, když se použije ADAL kódu odkazovat tyto hodnoty.
    -   `ida:Tenant` Je domain Azure AD klienta, například contoso.onmicrosoft.com
    -   `ida:Audience` Musí být **ID klienta** aplikace, kterou jste zkopírovali z portálu.

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3. pomocí ADAL zabezpečené stránky v zabezpečené ověřování HESLA*
Adal.js byla vytvořena integrovat s AngularJS směrování a http poskytovateli služeb, která umožňuje zabezpečená jednotlivých zobrazení v vaší zabezpečené ověřování HESLA.

- V `App/Scripts/app.js`, zkrácení modulu adal.js:

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- Teď můžete inicializace `adalProvider` s hodnotami konfigurace registrace aplikace, i v `App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- Teď k zabezpečení `TodoList` zobrazit v aplikaci pouze jeden řádek kódu je nezbytné - `requireADLogin`.

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

Nyní máte zabezpečené jedné stránky aplikace umožňuje přihlašování uživatelů a nosný token chráněného požadavků vydat jeho back-end rozhraní API.  Když uživatel klikne `TodoList` odkaz, adal.js bude automaticky přesměrovávat na Azure AD pro přihlášení v případě potřeby.  Kromě toho adal.js automaticky připojí access_token na žádné ajax požadavky, které jsou odeslány back-end aplikace.  Výše uvedené je úplné minimální potřebná k vytváření zabezpečené ověřování HESLA s adal.js - ale existuje několik dalších funkcí, které jsou užitečné v SPA:

- Explicitně problém s přihlášením a odhlásit se žádostí o můžete definovat funkcí v seznamu svého zařízení, které vyvolat adal.js.  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- Můžete taky zobrazit informace o uživateli v uživatelském rozhraní aplikace na.  Službu adal již přidala `userDataCtrl` řadiče domény, aby měli přístup k `userInfo` objekt v zobrazení přidružené `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- Jsou taky mnoho scénáře, kdy budete chtít zjistit, zda je uživatel přihlášený nebo ne.  Můžete taky použít `userInfo` objekt, který chcete shromáždění tyto informace.  Například v `index.html` můžete zobrazit "Přihlášení" nebo "Odhlášení" tlačítko podle stavu ověření:

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

Blahopřejeme! Azure AD integrovanou aplikaci jedné stránky je dokončena.  Ho můžete ověřování uživatelů bezpečně volání jeho back-end pomocí OAuth 2.0 a získat základní informace o uživateli.  Pokud jste to ještě neudělali, teď je čas na naplnění vašeho klienta s někteří uživatelé.  Spusťte svůj chcete udělat seznam zabezpečené ověřování HESLA a přihlášení pomocí jedné z těchto uživatelů.  Přidání úkolů do uživatelé můžou seznam odhlásit a znova se přihlaste.

Adal.js usnadňuje všechny tyto běžné funkce identity začlenit do aplikace.  Zabezpečuje dirty práce za vás – Správa mezipaměti, podpora protokolu OAuth prezentace uživatel s přihlášením uživatelského rozhraní, aktualizace vypršela platnost tokeny a další.

Pro informaci je k dispozici dokončeným ukázkovým (bez konfigurace hodnot) [v tomto poli](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Teď přejdete k další scénáře.  Můžete zkusit:

[Zavolat CORS webového rozhraní API z zabezpečené ověřování HESLA >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
