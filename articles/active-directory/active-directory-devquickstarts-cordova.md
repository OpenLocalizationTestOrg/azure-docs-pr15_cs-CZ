<properties
    pageTitle="Azure AD Cordova Začínáme | Microsoft Azure"
    description="Jak vytvořit Cordova aplikaci, která lze integrovat s Azure AD pro přihlášení a volá Azure AD zamknutí rozhraní API pomocí OAuth."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Integrace Azure AD pomocí Apache Cordova aplikace

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova umožňuje zabývají vývojem aplikací HTML5/JavaScript, které je možné spouštět na mobilních zařízeních jako plnohodnotný nativní aplikace.
S Azure AD můžete přidat ověřování testu s podnikovými aplikacím Cordova. Díky modul plug-in Cordova obtékání nativní SDK Azure AD IOS, Android a Windows Store a Windows Phone, můžete být díky dokonalejšímu aplikaci podpora znaménko na s AD účtů vašich uživatelů, získat přístup k Office 365 a rozhraní API Azure a dokonce i zamknout volání na své vlastní vlastní rozhraní API webových.

V tomto kurzu používáme Apache Cordova modul plug-in pro Active Directory Authentication knihovny (ADAL) kvůli zlepšení jednoduché aplikace pomocí následujících funkcí:

-   S několika řádky kódu ověření uživatele služby AD a získat token pro volání rozhraní API Azure AD grafu.
-   Pomocí této token vyvolat rozhraní API grafu dotazu tento adresář a zobrazte výsledky  
-   Využijte ADAL tokenu mezipaměti pro minimalizaci pokynů ověřování pro uživatele.

K tomuto účelu budete muset:

2. Registrace aplikace s Azure AD
2. Přidání kódu pro aplikace tokeny požadavek
3. Přidání kódu pro účely tokenu dotazování rozhraní API grafu a zobrazte výsledky.
4. Vytvoření projektu nasazení Cordova s všechny platformy, které chcete jej směrovat a Cordova ADAL modul plug-in a otestujte řešení v emulátorů.

## <a name="0--prerequisites"></a>*0. požadavky na*

Tento kurz, které budete potřebovat:

- Tenanta Azure AD, kde máte nastavený účet s oprávněními vývoj aplikací
- Vývojové prostředí nakonfigurovaný na používání Apache Cordova  

Pokud už obě nastavení, přejděte přímo krok 1.

Pokud nemáte klienta Azure AD, můžete najít [pokyny, jak si ho založit tady](active-directory-howto-tenant.md).

Pokud nemáte Apache Cordova nastavení na vašem počítači, nainstalujte takto:

- [Libovolná](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova rozhraní příkazového řádku](https://cordova.apache.org/) (je možné snadno nainstalovat prostřednictvím Správce balíčků NPM: `npm install -g cordova`)

Všimněte si, že můžou být měli spolupracovat na počítači i na Mac.

Každý platformu obsahuje různé požadavky.

- K vytvoření a spuštění Windows Tabletu nebo telefonu verzi aplikace
    - [Visual Studio 2013 pro Windows s aktualizace 2 nebo novější](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express nebo jinou verzi).
- K vytvoření a spuštění pro iOS
    -   Xcode 5.x nebo vyšší. Stáhněte si ho na http://developer.apple.com/downloads nebo obchodu [App Store pro Mac.](http://itunes.apple.com/us/app/xcode/id497799835?mt=12)
    -   [ios sim](https://www.npmjs.org/package/ios-sim) – umožňuje snadné spuštění iOS aplikace v iOS Simulator z příkazového řádku (je možné snadno nainstalovat prostřednictvím terminál: `npm install -g ios-sim`)

- K vytvoření a spuštění aplikace pro Android
    - Instalace [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo novější. Ujistěte se, `JAVA_HOME` (proměnné prostředí) je správně nastavená podle JDK Instalační cesta (například C:\Program Files\Java\jdk1.7.0_75).
    - Instalace [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) a přidejte `<android-sdk-location>\tools` umístění (například C:\tools\Android\android-sdk\tools) pro váš `PATH` proměnné prostředí.
    - Spusťte správce SDK Android (například prostřednictvím terminálu: `android`) a instalace
    - *Android 5.0.1 (21 rozhraní API)* platform SDK
    - *Android SDK – nástroje pro vytváření* verze 19.1.0 nebo vyšší
    - *Android podpory úložiště* (Extra)

  Android sdk nenabízí možnost všechny instance emulátoru výchozí. Vytvořte nový účet spuštěním `android avd` z terminál a následném vybrání *vytvořit...* , pokud chcete spustit aplikace Androidu emulátoru. Doporučené *Rozhraní Api úroveň* je 19 nebo vyšší, přečtěte si téma – AVD správce (http://developer.android.com/tools/help/avd-manager.html) Další informace o Android emulátoru a vytváření možnosti.


## <a name="1--register-an-application-with-azure-ad"></a>*1. registrace aplikace s Azure AD*

Poznámka: Tento __krok je nepovinný krok__. Kurz k dispozici předem zřizování hodnoty, které vám umožní zobrazit ukázku v akci aniž byste museli dělat jakékoli zřizování vlastní klientovi. Doporučujeme ale, že provádění tohoto kroku a seznamte se s proces, jak to bude potřeba když vytvoříte vlastní aplikace.

Azure AD pouze vydávat tokeny známých aplikací. Než budete moct použít Azure AD z aplikace, musíte pro něj vytvořte položky ve vašem klientovi.  Zaregistrujte nové aplikace ve vašem klientovi

-   Přihlaste se k [portálu Správa Azure](https://manage.windowsazure.com)
-   V levém navigačním podokně klikněte na **Služby Active Directory**
-   Vyberte klienta, kde se chcete zaregistrovat aplikace.
-   Klikněte na kartu **aplikace** a klikněte na **Přidat** do dolní okraj.
-   Postupujte podle pokynů a vytvořte novou **Nativní klientské aplikaci** (bez ohledu kromě toho, že aplikace Cordova jsou ve formátu HTML na základě vytváříme tady aplikace native client – takže `Native Client Application` musí být vybraná možnost; v opačném nebude fungovat).
    -   **Název** aplikace bude popisovat aplikace pro koncové uživatele
    -   **Přesměrovat URI** je URI používá k vrácení tokeny do aplikace. Zadejte `http://MyDirectorySearcherApp`.

Po dokončení zápisu AAD přiřadí aplikace klienta jedinečný identifikátor.  Musíte mít tuto hodnotu v dalších částech: najdete na kartě **Konfigurovat** nově vytvořený aplikace.

Chcete-li spustit `DirSearchClient Sample`, udělit oprávnění nově vytvořený aplikace k vytvoření dotazu rozhraní _API Azure AD grafu_:
-   Na kartě **Konfigurovat** vyhledejte oddíl "Oprávnění do jiných aplikací".  Aplikace "Azure Active Directory" přidáte oprávnění k **přístupu k adresáři jako uživatel přihlášený** ve skupinovém rámečku **Delegovat oprávnění**.  To vám umožní aplikace k vytvoření dotazu grafu rozhraní API pro uživatele.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. klonovat úložišti aplikace ukázkové potřebných pro kurz*

Z vašeho prostředí nebo příkazového řádku zadejte tento příkaz:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. vytvoření Cordova aplikace*

Vytváření aplikací Cordova několika způsoby. V tomto kurzu použijeme rozhraní Cordova příkazového řádku (rozhraní příkazového řádku).
Z vašeho prostředí nebo příkazového řádku zadejte tento příkaz:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

Která vytvoří struktura složek a vygenerovaných Cordova projektu, kopírování obsahu starter projektu v podsložce www.
Přesunutí do nové složky DirSearchClient.

    cd .\DirSearchClient

Přidání modulu plug-in povolených, potřebné pro vyvolání rozhraní API grafu.

     cordova plugin add cordova-plugin-whitelist

Dále přidejte všechny, které chcete podporují platformách. Aby mohl pracovní vzorek, je nutné provést alespoň jedno z následujících příkazů. Všimněte si, že nebudete moct emulovat iOS na Windows nebo Windows/Windows Phone na počítači Mac.

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Nakonec můžete přidat ADAL pro modul plug-in Cordova do projektu.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. přidáte kód pro ověřování uživatelů a od AAD získat tokenů*

Aplikaci, kterou vyvíjíte v tomto kurzu poskytne bez nutnosti dodatečného softwaru kostní adresáře funkce vyhledávání, kde můžete koncového uživatele zadejte alias všichni uživatelé v adresáři a vizualizovat některé základní atributy.  Projekt starter definici základní uživatelské rozhraní v aplikaci (www/index.html) a vygenerovaných, který vedení vodiče nahoru základní aplikace událost cyklů uživatelské rozhraní vazby a výsledky logiku zobrazení (v www/js/index.js). Jediné zanechal je přidání logiky pro provádění úkolů identity.

Velmi první věc, kterou musíte udělat je zavést v kódu protokol hodnoty, které využívají AAD k identifikaci aplikace a prostředky, které jste obrázku. Jsou tyto hodnoty se použijí k konstrukce tokenu žádosti o později. Vložte fragment pod k hornímu index.js souboru.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

`redirectUri` a `clientId` hodnoty měly odpovídat hodnotám popisující vaši aplikaci distribuovali AAD. Najdete ty z karty konfigurovat v portálu Azure podle pokynů v kroku 1 dříve v tomto kurzu.
Poznámka: Pokud jste pro není registraci nové aplikace v vlastního klienta, můžete jednoduše vložit uvedených předkonfigurovaná hodnot je –, která vám umožní v tématu spuštění vzorku by měli byste vždy vytvořit vlastní položku pro aplikace určeny výroby.

Pak potřebujeme přidání kódu pro skutečnou tokenu žádost. Vložení následující úryvek mezi `search `a `renderdata `definice.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Podívejme se podrobněji tuto funkci rozdělením ho na dvě hlavní části.
V tomto příkladu je navržený pro práci s všechny klienta, nikoli na některá. "/ Běžné" koncový bod, který umožňuje uživateli zadejte jakémukoli účtu během ověřování a žádost směrovat tak, aby používala ke klientovi patří.
První část metodu vyhodnotí ADAL mezipaměti vidět – Pokud je už uložené token – a pokud existuje, použije student, ze které pochází znovu inicializace ADAL. Toto je nutné navíc výzev při používání aplikace "/ běžné" vždy má za následek výzvou k zadání nového účtu.
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Druhá část metodu provádí velkými tokewn žádost.
`acquireTokenSilentAsync` Metoda požádá o ADAL vrátíte token pro daný zdroj bez zobrazení všechny UX Může dojít, pokud mezipaměti už je vhodné přístupový token uložené, nebo pokud je aktualizace token, který lze použít k získání nové přístupový token bez shwoing jakékoli dotaz.
Pokud tento pokus nezdaří, jsme přejdou `acquireTokenAsync` -které bude viditelně zobrazí výzvu k ověření.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Teď, když máme tokenu, můžeme nakonec vyvolat rozhraní API grafu a provádění vyhledávacího dotazu, které chceme. Vložení následující úryvek přímo pod to `authenticate` definice.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Počáteční bod soubory předávaných barebone činnosti koncového uživatele k zadání uživatelského alias v textovém poli. Tento způsob používá tuto hodnotu k vytvoření dotazu, kombinovat s přístupový token, odeslat do grafu a analýza výsledků. Metoda renderData již v souboru počátečního bodu stará vizualizovat výsledky.

## <a name="4-run"></a>*4. spustit*
Aplikace je nakonec chtít spustit. To je velmi jednoduché: Po spuštění aplikace zadejte do textového pole alias uživatele, který chcete vyhledat – a pak klikněte na tlačítko. Zobrazí se výzva k ověření. Po úspěšném ověření a úspěšné hledání se zobrazí atributy prohledávaných uživatele. Při dalším spuštění vyhledá bez zobrazení všechny řádku, díky informace o stavu v mezipaměti tokenu dříve získali.
Konkrétní kroky pro spuštění aplikace se liší podle platformy.

####<a name="windows-10"></a>Windows 10:

   Tabletu:`cordova run windows --archs=x64 -- --appx=uap`

   Mobil (vyžaduje Windows10 mobilní zařízení připojené k počítači):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Poznámka__: při prvním spuštění se může výzva k přihlášení pro vývojáře licenci. Další informace najdete v článku [Vývojář licenci](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-81-tabletpc"></a>Windows 8.1 nebo Tabletu:

   `cordova run windows`

   __Poznámka__: při prvním spuštění se může výzva k přihlášení pro vývojáře licenci. Další informace najdete v článku [Vývojář licenci](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   Spuštění na připojené zařízení:`cordova run windows --device -- --phone`

   Spuštění na výchozí emulátoru:`cordova emulate windows -- --phone`

   Použití `cordova run windows --list -- --phone` zobrazíte všechny dostupné cílů a `cordova run windows --target=<target_name> -- --phone` ke spuštění aplikace na určité zařízení nebo emulátoru (například `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   Spuštění na připojené zařízení:`cordova run android --device`

   Spuštění na výchozí emulátoru:`cordova emulate android`

   __Poznámka__: Ujistěte se, že jste vytvořili emulátoru instance správce *AVD* jako není zobrazeno v oddílu *požadavky* .

   Použití `cordova run android --list` zobrazíte všechny dostupné cílů a `cordova run android --target=<target_name>` ke spuštění aplikace na určité zařízení nebo emulátoru (například `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   Spuštění na připojené zařízení:`cordova run ios --device`

   Spuštění na výchozí emulátoru:`cordova emulate ios`

   __Poznámka__: Zkontrolujte, jestli `ios-sim` balíčku spouštět v PDC. *Předpoklady* v části Další informace.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Použití `cordova run --help` v tématu další vytvoření a spuštění možnosti.

Pro informaci je k dispozici dokončeným ukázkovým (bez konfigurace hodnot) [v tomto poli](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  Teď můžete přesunout na pokročilejší scénářů (ok a zajímavější).  Můžete zkusit:

[Zabezpečené Node.js webového rozhraní API s Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
