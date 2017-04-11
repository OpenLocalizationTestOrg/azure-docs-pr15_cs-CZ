<properties
    pageTitle="Jak používat SDK JavaScriptu pro Azure mobilní aplikace"
    description="Jak používat v Azure mobilní aplikace"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Jak používat knihovnu JavaScript klienta pro Azure mobilní aplikace

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tato příručka učí můžete provádět běžné scénáře pomocí nejnovější [JavaScript SDK Azure mobilní aplikace]. Pokud začínáte Azure mobilní aplikace, proveďte první [Azure mobilní aplikace rychlý Start] vytvořit back-end a vytvořte tabulku. V této příručce jsme zaměření se na použití mobilního back-end v HTML/JavaScript webových aplikacích.

## <a name="supported-platforms"></a>Podporované platformy

Jsme omezit podpora prohlížečů pro aktuální a poslední verzemi hlavní verze prohlížeče: Google Chrome, Microsoft Edge, Microsoft Internet Explorer a Mozilla Firefox.  Očekáváme SDK funkce s relativně moderní prohlížeče.

Balíček distribuuje jako modul univerzální JavaScript tak podporuje globals AMD, a formáty CommonJS.

##<a name="Setup"></a>Instalační program a požadavky

Tato příručka předpokládá, že jste vytvořili back-end s tabulkou. Tato příručka předpokládá, že v tabulce má stejné schéma jako tabulky v těchto kurzech.

Instalace Azure mobilní aplikace JavaScript SDK lze provést pomocí `npm` příkaz:

```
npm install azure-mobile-apps-client --save
```

Po instalaci knihovně je umístěn v `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`.  Zkopírujte tento soubor do oblasti vašeho webu.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

Knihovnu lze také jako modul ES2015 v rámci CommonJS prostředí, například Browserify a Webpack a jako AMD knihovna.  Příklad:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>Postup: ověřování uživatelů

Služba Azure aplikace podporuje ověřování a ověřování uživatelů aplikace pomocí různých poskytovatelů identit externí: Facebook, Google, Account Microsoft a Twitter. Nastavení oprávnění pro tabulky, které chcete omezit přístup pro konkrétní operace pouze pro ověřeného uživatele. Můžete taky identitu ověřených uživatelů implementaci ověřovacích pravidel v skripty serveru. Další informace najdete v tématu kurz [Seznámení s ověřování] .

Dva toků ověřování podporují: na serveru a klientských tok.  Tok serveru poskytuje možnosti nejjednodušší ověřování závisí na poskytovatele webového ověřování rozhraní. Tok klienta umožňuje hlubší integrace s možnostmi specifické pro zařízení jako-jednotné přihlášení jako závisí na SDK zprostředkovatele.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Jak: Konfigurace služby mobilní aplikace pro externí přesměrování adresy URL.

Několik typů JavaScript aplikací pomocí funkce smyčky uživatelského rozhraní OAuth tocích zpracovat.  Zahrnout tyto možnosti:

* Vaše službou místně
* Pomocí rozhraní iontové živou načíst znovu
* Přesměrování aplikace služby ověřování. 

Místně spuštěné může způsobit problémy, protože ve výchozím nastavení aplikace služby ověřování pouze nakonfigurovaný tak, aby povolit přístup z vaší back-end mobilní aplikaci. Pomocí následujících kroků můžete změnit nastavení aplikace služeb povolit ověřování při serverem místně:

1. Přihlaste se k [portálu Azure]
2. Přejděte na vaše mobilní aplikaci back-end.
3. V nabídce **Nástroje pro vývoj** vyberte **Průzkumník zdroje** .
4. Klikněte na **Přejít** na otevřete Průzkumníka zdroje pro mobilní aplikaci backendovou v novou kartu nebo okno.
5. Rozbalení **Konfigurace** > **authsettings** uzel aplikace.
6. Klikněte na tlačítko **Upravit** povolte úpravy zdroje.
7. Najděte prvek **allowedExternalRedirectUrls** , která by měla být null. Přidejte svůj adresy URL v matici:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Adresy URL do pole nahradit adresy URL služby, které v tomto příkladu je `http://localhost:3000` pro službu ukázkové Node.js. Můžete taky použít `http://localhost:4400` pro službu dominový nebo některé další adresu URL, v závislosti na konfiguraci aplikace.

8. V horní části stránky klikněte na **Pro čtení i zápis**a potom klikněte na **umístění** uložte provedené změny.

Potřebujete přidat stejné adresy URL zpětná smyčka k nastavení povolených CORS:

1. Přejděte zpátky k [Azure portálu].
2. Přejděte na vaše mobilní aplikaci back-end.
3. Klikněte na **CORS** v nabídce **rozhraní API** .
4. Všechny adresy URL zadejte do textového pole prázdné **Povolené typů** .  Vytvoří se nové textové pole.
5. Klikněte na tlačítko **Uložit**
    
Po aktualizaci back-end budete moct použít nové adresy URL smyčky v aplikaci.

<!-- URLs. -->
[Azure mobilní aplikace rychlým startem]: app-service-mobile-cordova-get-started.md
[Začínáme s ověřování]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure portálu]: https://portal.azure.com/
[JavaScript SDK Azure mobilní aplikace]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

