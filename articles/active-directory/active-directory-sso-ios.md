<properties
    pageTitle="Jak povolit SSO křížově aplikace v iOS pomocí ADAL | Microsoft Azure"
    description="Jak používat funkce ADAL SDK povolit jednotné přihlašování v rámci aplikace. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Jak povolit SSO křížově aplikace v iOS pomocí ADAL


Poskytující jednoho přihlašování (SSO) tak, aby uživatelé stačí jednou zadejte své přihlašovací údaje a automaticky mít tyto přihlašovací údaje pracoval aplikací teď očekává zákazníci. Potíže při zadávání svoje uživatelské jméno a heslo na malé obrazovce, často časy společně s další faktor (2FA) jako telefonní hovor nebo texted kód, výsledkem je snadné nespokojenost, pokud má uživatel má můžete to udělat víc času pro produkt.

Kromě toho pokud používáte platformu identity pro využívající jiné aplikace může například Accounts Microsoft nebo pracovní účet z Office 365, zákazníci očekávají, že můžou být pověření k dispozici pro použití všech svých aplikací bez ohledu na to dodavatele.

Platformu Microsoft Identity spolu s naše SDK Identity Microsoft nemá všechny tento práce za vás a nabídne vám možnost delight zákazníkům s jednotným Přihlašováním buď v rámci vlastní sady aplikací nebo, stejně jako u naši stránku věnovanou zprostředkovatele funkce a aplikace ověřovacích dat přes celou zařízení.

V tomto návodu se dozvíte, jak nakonfigurovat naše SDK v rámci aplikace a poskytovat tuto výhodu zákazníkům.

Tento návod platí pro:

* Azure Active Directory
* B2C Azure Active Directory
* B2B Azure Active Directory
* Podmíněného přístupu Azure Active Directory


Všimněte si, že dokument dole předpokládá se mají informace o tom, jak [by aplikace na portálu starší verze služby Azure Active Directory](./develop/active-directory-how-to-integrate.md) i integrovali aplikace s [iOS identit SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Jednotné přihlašování koncepty platformu Microsoft Identity

### <a name="microsoft-identity-brokers"></a>Identit zprostředkovatelů

Microsoft poskytuje aplikace pro každý mobilní platformu povolit přemostění přihlašovací údaje ve všech aplikacích různých prodejců i umožňuje zvláštní vylepšené funkce, které vyžadují na jednom místě zabezpečené odkud ověřit pověření. Název tyto **zprostředkovatelů**. V iOS a Android tyto jsou k dispozici ke stažení aplikace, aby zákazníci instalace nezávisle na sobě, nebo můžete posunuto zařízení společností, kdo vám spravuje některé nebo všechny zařízení pro jejich zaměstnance. Tyto zprostředkovatelů podporu správy zabezpečení jenom některé aplikace nebo celé zařízení podle vyžadují správce IT. Ve Windows prvek pro tuhle funkci poskytované účtu výběr součástí operačního systému, známé doslova jako zprostředkovatel ověřování Web.

Pokud chcete zjistit, jak používáme tyto zprostředkovatelů a jak zákazníky může se zobrazit jejich ve své přihlašovací tok pro platformu Microsoft Identity, přečtěte si další informace.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Vzorce pro přihlášení na mobilních zařízeních

Přístup k přihlašovací údaje na zařízeních postupujte dva základní vzorků pro platformu Microsoft Identity:

* Zprostředkovatel nejsou podporovaných přihlášení
* Zprostředkovatel podporovaných přihlášení

#### <a name="non-broker-assisted-logins"></a>Zprostředkovatel nejsou podporovaných přihlášení

Zprostředkovatel nejsou podporovaných přihlášení jsou přihlášení prostředí, které dojít vloženým do aplikace a použít místní úložiště v zařízení pro tuto aplikaci. Toto úložiště může být sdíleny s aplikací, ale přihlašovací údaje jsou úzce vázaný na aplikaci nebo škálu aplikací pomocí tohoto přihlašovacích údajů. Toto je prostředí, které jste pravděpodobně došlo v mnoha mobilních aplikací, kde zadejte uživatelské jméno a heslo pro aplikace samotnou aplikaci.

Tyto přihlášení má následující výhody:

-  Uživatelské prostředí existuje úplně aplikace.
-  Přihlašovací údaje mohou být sdíleny aplikace, které jsou podepsáno stejný certifikát poskytuje jediné přihlašování setkat i v případě sadě aplikací.
-  Ovládací prvek kolem zkušenosti s přihlašováním slouží k aplikaci před a po přihlášení.

Tyto přihlášení k dispozici následující nevýhody:

- Nelze činnost koncového uživatele jednotného přihlašování na přes všechny aplikace, které používají Microsoft Identity jenom přes Identities Microsoft, které jsou vaše aplikace jejím vlastníkem a nakonfigurovali.
- Aplikace se nedají použít s pokročilejší obchodní funkce, jako jsou podmíněné přístup nebo použijte produktů sady InTune.
- Aplikace nepodporuje ověřování na základě certifikátů pro firemní uživatele.

Tady je znázornění fungování SDK Identity Microsoft s sdílených úložiště aplikace povolit jednotné přihlašování:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Zprostředkovatel podporovaných přihlášení

Zprostředkovatel pomoci přihlášení jsou přihlášení prostředí, které rozmezích zprostředkovatele aplikace a umožňuje sdílení přihlašovací údaje ve všech aplikacích v zařízení, které využívají platformu Microsoft Identity úložiště a zabezpečení zprostředkovatele. To znamená, že aplikace bude spolehnout zprostředkovatele pro přihlášení uživatele. V iOS a Android tyto jsou k dispozici ke stažení aplikace, aby zákazníci instalace nezávisle na sobě, nebo můžete posune zařízení společností, kdo vám spravuje zařízení pro své uživatele. Příklad tohoto typu aplikace je ověřovacích Azure dat aplikace v iOS. Ve Windows prvek pro tuhle funkci poskytované účtu výběr součástí operačního systému, známé doslova jako zprostředkovatel ověřování Web.
Možnosti se liší podle platformy a může být někdy vyloučit uživatelům v opačném případě spravovaných správně. Znáte pravděpodobně nejčastěji tento způsob Pokud máte nainstalovanou aplikaci Facebook a použití funkce Facebook přihlášení do jiné aplikace. Platformu Microsoft Identity využívá stejné vzorku.

Animace, kde se neodesílají aplikace na pozadí v průběhu ověřovacích Azure dat aplikace pro iOS, které to vede k "přechodu" Doručená popředí pro uživatele vyberte účtu, který chce se přihlašujete.  

Pro Android a Windows výběr účtu, zobrazí se na aplikaci, která je menší vyloučit uživateli.

#### <a name="how-the-broker-gets-invoked"></a>Jak se má zprostředkovatel vyvolat

Pokud je kompatibilní zprostředkovatele nainstalovaný na zařízení, jako je aplikace ověřovacích dat Azure SDK Identity Microsoft automaticky provádět práce pro vyvolání zprostředkovatele za vás, když uživatel označuje, že se chcete přihlásit pomocí libovolného účtu z platformu Microsoft Identity. Může to být osobním Account Microsoft, pracovní nebo školní účet nebo účet, který zadáte a hostitele v Azure pomocí našich B2C a B2B produktů. Pomocí velmi bezpečné algoritmy a šifrování jsme pověření se zajistila vyzývá k zadání a doručován bezpečně zpátky do okna aplikace. Přesné technické podrobnosti tyto mechanismy není publikovat ale byly vytvořili spolupráci Apple a Google.

**Vývojář má volba, pokud Microsoft Identity SDK volá má zprostředkovatel nebo používá podporovaných toku není zprostředkovatele.** Ale pokud vývojář zvolí nepoužít toku pomoci zprostředkovatele se nebudou mít dál výhod využití pověření jednotné přihlašování, že uživatel už může být přidaná na zařízení pestřejšími zabráníte jejich použití z společně s funkcí business, u kterých Microsoft poskytuje svým zákazníkům například podmíněného přístupu, možnosti správy Intune a ověřování na základě certifikát.

Tyto přihlášení má následující výhody:

-  Uživatelské prostředí jednotné přihlašování všech svých aplikací bez ohledu na to dodavatele.
-  Aplikaci můžete využít pokročilejší obchodní funkce, jako jsou podmíněné přístup nebo použít sadu InTune produktů.
-  Aplikace podporuje ověřování na základě certifikátů pro firemní uživatele.
- Ověření se mnohem víc zabezpečené přihlášení identitou aplikace a uživatel aplikací zprostředkovatele algoritmů větší zabezpečení a šifrování.

Tyto přihlášení k dispozici následující nevýhody:

- V iOS uživatel je, které přešly z prostředí aplikace během přihlašovací údaje jsou zvolené.
- Ztráta možnost spravovat přihlášení prostředí pro zákazníky v aplikaci.



Tady je znázornění fungování SDK Identity Microsoft s aplikacemi zprostředkovatele povolit jednotné přihlašování:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

Ozbrojených se tyto informace pozadí, kterou jste měli půjde vám ještě snadněji chápat a implementace jednotného přihlašování do aplikace pomocí platformu Microsoft Identity a SDK.


## <a name="enabling-cross-app-sso-using-adal"></a>Povolení více aplikací jednotné přihlašování pomocí ADAL

Tady použijeme ADAL iOS SDK na:

- Zapnutí podporovaných jednotné přihlašování – zprostředkovatel sadu aplikací
- Zapnutí podpory pomoci zprostředkovatele jednotné přihlašování

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Zapnutí jednotného přihlašování pro není zprostředkovatel pomoci jednotné přihlašování

Podporovaných jednotné přihlašování – zprostředkovatel ve všech aplikacích Microsoft Identity SDK Správa velkou část složitosti jednotné přihlašování za vás. Jedná se o hledání správná uživatelská v mezipaměti a udržování seznam přihlášeného uživatele můžete k vytvoření dotazu.

Chcete-li povolit jednotné přihlašování ve všech aplikacích vlastníte, abyste mohli postupujte takto:

1. Zajistit, aby všechny vaše uživatele aplikace se stejnými ID klienta nebo ID aplikace.
* Zajistit, aby všechny aplikace sdílet stejnou podpisový certifikát od společnosti Apple, takže můžete sdílet keychains
* Požádat o stejné nároku řetězce klíčů všech vašich aplikací.
* Pozná, že SDK Identity společnosti Microsoft o sdíleného řetězce klíčů že chcete, abychom používat.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Použití stejné ID klienta / aplikace ID pro všechny aplikace v sadě aplikací

Objednávky pro platformu Microsoft Identity vědět, že mohou být tokeny zužitkování aplikace všech vašich aplikací, musí sdílení stejného ID klienta nebo ID aplikace. Toto je jedinečný identifikátor, který jste získali při registraci aplikace první na portálu.

Budete možná vás zajímá, jak bude různých aplikací ke službě Microsoft Identity určit, zda používá stejnou aplikace ID. Odpověď se **Přesměrovat URI**. Každá aplikace může obsahovat více přesměrovat identifikátory URI registrovaná na portálu rychlého připojení. Každý ve vaší sadě bude zobrazený různých přesměrování URI. Níže je příklad toho, jak to vypadá:

Přesměrovat Apl1 URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

Přesměrovat Apl2 URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

Přesměrovat App3 URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Tyto vnořená pod stejným ID klienta / ID aplikace a chcete prohledat podle přesměrování URI vrátit se nám v konfiguraci SDK.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Všimněte si, že formát tyto přesměrovat URI jsou vysvětleny pod. Můžete použít libovolný identifikátor URI přesměrovat Pokud chcete podporu zprostředkovatel, v tomto případě musí vypadají nějak výše uvedených možností*



#### <a name="create-keychain-sharing-between-applications"></a>Vytvoření řetězce klíčů sdílení mezi aplikacemi

Povolení sdílení řetězce klíčů je nad rámec tohoto dokumentu a vztahuje konsolidovaná Apple v dokumentu [Přidání funkce](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Co je důležité se rozhodnout, co potřebujete klíčů pro volání a přidat funkci ve všech aplikacích.

Pokud máte nároky nastavení správně jste měli vidět souboru v adresáři vašeho projektu s názvem `entitlements.plist` , která obsahuje objekt, který vypadá jako následující:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Jakmile máte nárok řetězce klíčů povolené ve všech aplikacích a budete chtít použít jednotné přihlašování, informujte Microsoft Identity SDK klíčů pomocí na následující nastavení vašeho `ADAuthenticationSettings` s použité toto nastavení:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING]
Při sdílení řetězce klíčů přes aplikace libovolné aplikaci můžete odstranit uživatele nebo horší odstranit všechny tokeny přes aplikaci. Je to zejména katastrofální, pokud máte aplikace, které jsou závislé na tokeny pozadí práce. Sdílení řetězce klíčů odebrat: znamená, že musí být opatrní ve všech operace prostřednictvím SDK Identity Microsoft.

Je to! Microsoft Identity SDK nyní sdílet přihlašovací údaje ve všech aplikacích. Seznam uživatelů se sdílejí také všech instancí aplikace.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Zapnutí jednotného přihlašování pro zprostředkovatele pomoci jednotné přihlašování

Možnost pro aplikaci použít zprostředkovatele, který je nainstalovaný na zařízení je **ve výchozím nastavení vypnuté**. Abyste mohli používat aplikace s má zprostředkovatel musíte udělat některé další konfiguraci a přidání kódu pro některé aplikace.

Postupujte podle těchto kroků jsou:

1. Povolte režim zprostředkovatele v kódu aplikace volání MS SDK.
2. Vytvoření nové přesměrování URI a poskytnout, aplikace a registrace aplikace.
3. Registrace schéma adresy URL.
4. Podpora iOS9: Přidat oprávnění k souboru info.plist.


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Krok 1: Povolení režimu zprostředkovatele v aplikaci
Možnost pro aplikaci používat zprostředkovatele je zapnutá při vytváření "kontext" nebo počáteční nastavení ověřování objektu. Nastavením povahy přihlašovacích údajů v kódu udělejte toto:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
`AD_CREDENTIALS_AUTO` Nastavení vám umožní Microsoft SDK Identity zkusit provádět volání do zprostředkovatel, `AD_CREDENTIALS_EMBEDDED` Microsoft Identity SDK zabrání volání zprostředkovatele.

#### <a name="step-2-registering-a-url-scheme"></a>Krok 2: Registrace schéma adresy URL
Platformu Microsoft Identity používá adresy URL pro vyvolání má zprostředkovatel a vraťte se zpátky do okna aplikace ovládacího prvku. Dokončení této doby odezvy musíte schéma adresy URL registrované aplikace, který bude vědět platformu Microsoft Identity. Může to být současně s dalšími aplikace programy, které možná jste dříve si zaregistrovali s aplikací.

> [AZURE.WARNING]
Doporučujeme díky schéma adresy URL poměrně jedinečné minimalizovat riziko, jiné aplikace pomocí stejné schéma adresy URL. Apple není jedinečnosti režimů adresy URL, které jsou registrované v app storu.

Tady je příklad jak se objeví v konfigurace projektu. Může taky to uděláte v XCode taky:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Krok 3: Vytvoření nové přesměrování URI s schéma adresy URL

Zajistit, aby vždy vrácených tokeny přihlašovací údaje správné použití potřebujeme Ujistěte se, že název zpátky do aplikace tak, že můžete ověřit operační systém iOS. Operační systém iOS sestavy na zprostředkovatele aplikace Microsoft příruček ID aplikace volání ho. To nejde falešné aplikací podvodné. Proto doporučujeme využít to společně s URI naše zprostředkovatele aplikace zajistit, že tokeny vracejí do správné použití. Jsme vyžadují, abyste vytvořte tomuto jedinečné přesměrování URI obou v aplikaci a nastavit jako identifikátor URI přesměrovat naše portálu vývojář.

Přesměrování URI musí být ve formuláři správné:

`<app-scheme>://<your.bundle.id>`

ex: *x msauth mytestiosapp://com.myapp.mytestapp*

Tento identifikátor URI přesměrovat musí být zadán v aplikaci registrace pomocí [Azure klasické portálu](https://manage.windowsazure.com/). Další informace o registraci aplikace Azure AD najdete v článku [Integrace se službou Azure Active Directory](./develop/active-directory-how-to-integrate.md).


##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Krok 3a: Přidání přesměrování URI v portálu aplikace a vývoj podporuje ověřování na základě certifikátu

K podpoře certifikátu ověřování na základě které druhý "msauth" potřebuje k registraci v aplikaci a [Azure klasické portál](https://manage.windowsazure.com/) zpracování ověřování certifikátů, pokud chcete přidat podporující v aplikaci.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Krok 4: iOS9: Přidání parametru konfigurace aplikace

ADAL používá – canOpenURL: Chcete-li zkontrolovat, pokud má zprostředkovatel nainstalovaný na zařízení. V iOS 9 Apple uzamčení co schémata aplikaci můžete pro dotaz. Budete muset přidat "msauth" do části LSApplicationQueriesSchemes vaše `info.plist file`.

<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>Máte nastavený jednotné přihlašování!

Teď Microsoft Identity SDK bude automaticky zužitkování přihlašovacích údajů aplikace i vyvolat má zprostředkovatel, pokud je k dispozici na svém zařízení.
