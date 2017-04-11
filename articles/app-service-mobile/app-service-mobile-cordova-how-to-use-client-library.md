<properties
    pageTitle="Jak používat modul plug-in Cordova Apache pro Azure mobilní aplikace"
    description="Jak používat modul plug-in Cordova Apache pro Azure mobilní aplikace"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Jak používat Apache Cordova klienta knihovny pro Azure mobilní aplikace

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tato příručka učí můžete provádět běžné scénáře pomocí nejnovější [Modul plug-in Cordova Apache Azure mobilních aplikací]. Pokud začínáte na Azure mobilní aplikace první dokončení [Azure mobilní aplikace rychlý Start] vytvořit back-end, vytvořte tabulku a stáhněte předdefinovaných Apache Cordova projektu. V této příručce jsme zaměřit na modul plug-in Cordova Apache klienta.

## <a name="supported-platforms"></a>Podporované platformy

Tento SDK podporuje Apache Cordova v6.0.0 a novějším iOS, Android a Windows zařízení.  Podpora platformy vypadá takto:

* Rozhraní API Android (KitKat prostřednictvím Nougat) 19 až 24
* iOS 8.0 nebo novější verzí.
* Windows Phone 8.0
* Windows Phone 8.1
* Univerzální Windows platforma

##<a name="Setup"></a>Instalační program a požadavky

Tato příručka předpokládá, že jste vytvořili back-end s tabulkou. Tato příručka předpokládá, že v tabulce má stejné schéma jako tabulky v těchto kurzech. Tato příručka také předpokládá přidaná modul plug-in Cordova Apache kódu.  Pokud jste tak dosud neučinili, můžete přidat modul plug-in Apache Cordova projektu do příkazového řádku:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Další informace o vytváření [aplikace pro první Apache Cordova]dokumentaci k jejich.

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="auth"></a>Postup: ověřování uživatelů

Služba Azure aplikace podporuje ověřování a ověřování uživatelů aplikace pomocí různých poskytovatelů identit externí: Facebook, Google, Account Microsoft a Twitter. Nastavení oprávnění pro tabulky, které chcete omezit přístup pro konkrétní operace pouze pro ověřeného uživatele. Můžete taky identitu ověřených uživatelů implementaci ověřovacích pravidel v skripty serveru. Další informace najdete v tématu kurz [Seznámení s ověřování] .

Pokud chcete použít ověřování aplikace Apache Cordova, musí být k dispozici následující moduly plug-in Cordova:

* [cordova modul plug-in zařízení]
* [cordova modul plug-in inappbrowser]

Dva toků ověřování podporují: na serveru a klientských tok.  Tok serveru poskytuje možnosti nejjednodušší ověřování závisí na poskytovatele webového ověřování rozhraní. Tok klienta umožňuje hlubší integrace s možnostmi specifické pro zařízení jako-jednotné přihlášení jako závisí na SDK specifické pro zařízení zprostředkovatele.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Jak: Konfigurace služby mobilní aplikace pro externí přesměrování adresy URL.

Několik typů Apache Cordova aplikací pomocí funkce smyčky uživatelského rozhraní OAuth tocích zpracovat.  Toky OAuth uživatelské rozhraní na localhost způsobit problémy, protože služba ověřování pouze věděli, jak se využívají vaše služba ve výchozím nastavení.  Příklady problematický toků OAuth uživatelského rozhraní:

- Dominový emulátoru.
- Živé načíst znovu s iontové.
- Místně spuštěné mobilní back-end
- Spuštění mobilní back-end v jiné aplikaci službě Azure než jeden poskytující ověřování.

Postupujte podle těchto pokynů k přidání místní nastavení konfigurace:

1. Přihlaste se k [portálu Azure]
2. Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název aplikace Mobile.
3. Klikněte na **Nástroje**
4. Klikněte na položku **Průzkumník zdroje** v nabídce dodržovat a potom klikněte na tlačítko **Přejít**.  Otevře se nové okno nebo tab.
5. Rozbalte **Konfigurace**, **authsettings** uzly webu v levém navigačním panelu.
6. Klikněte na tlačítko **Upravit**
7. Vyhledejte element "allowedExternalRedirectUrls".  Může být nastavena na hodnotu null nebo matici hodnot.  Změňte hodnotu na následující hodnotu:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Nahraďte adresy URL adresy URL služby.  Jako příklad lze uvést "http://localhost:3000" (služby Node.js ukázkové) nebo "http://localhost:4400" (pro službu dominový).  Však tyto adresy URL jsou příklady - vaši situaci, včetně služby uvedené v příkladech se může lišit.
8. Kliknutím na tlačítko **Pro čtení i zápis** v pravém horním rohu obrazovky.
9. Zelené klikněte na tlačítko **Vložit** .

Nastavení se ukládají v tomto okamžiku.  Nezavírejte okna prohlížeče nedokončili nastavení uložení.
Také přidáte tyto adresy URL duplicitních CORS nastavení pro aplikaci služby:

1. Přihlaste se k [portálu Azure]
2. Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název aplikace Mobile.
3. Nastavení zásuvné se automaticky otevře.  Pokud ne, klikněte na **Všechna nastavení**.
4. V nabídce rozhraní API klepněte **CORS** .
5. Zadejte adresu URL, kterou chcete přidat do pole a stiskněte Enter.
6. Podle potřeby zadejte další adresy URL.
7. Klepněte na tlačítko **Uložit** uložte nastavení.

Trvá asi 10 až 15 sekund nové nastavení se projeví.

##<a name="register-for-push"></a>Postup: registrace k nabízená oznámení

Nainstalujte [phonegap modul plug-in nabízených] zpracovávání nabízená oznámení.  Tento modul plug-in lze snadno přidat pomocí `cordova plugin add` příkaz příkazového řádku nebo přes instalační program libovolná modul plug-in aplikace Visual Studio.  Následující kód v aplikaci Apache Cordova zaregistruje zařízení nabízená oznámení:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Pomocí SDK rozbočovače upozornění odesílat nabízená oznámení ze serveru.  Nikdy neodesílat nabízená oznámení přímo z klientů. Tím může použít ke spuštění útoku služby proti rozbočovače oznámení nebo PNS.  PNS může zakázat přenosy pro vaši důsledku takových útoků.

<!-- URLs. -->
[Azure portálu]: https://portal.azure.com
[Azure mobilní aplikace rychlým startem]: app-service-mobile-cordova-get-started.md
[Začínáme s ověřování]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Modul plug-in Cordova Apache pro Azure mobilní aplikace]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[první Apache Cordova aplikace]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap modul plug-in připínáčku]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova modul plug-in zařízení]: https://www.npmjs.com/package/cordova-plugin-device
[cordova modul plug-in inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
