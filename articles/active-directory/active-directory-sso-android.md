<properties
    pageTitle="Jak povolit více aplikací SSO na Androidu pomocí ADAL | Microsoft Azure"
    description="Jak používat funkce ADAL SDK povolit jednotné přihlašování v rámci aplikace. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Jak povolit více aplikací SSO na Androidu pomocí ADAL


Poskytující jednoho přihlašování (SSO) tak, aby uživatelé stačí jednou zadejte své přihlašovací údaje a automaticky mít tyto přihlašovací údaje pracoval aplikací teď očekává zákazníci. Potíže při zadávání svoje uživatelské jméno a heslo na malé obrazovce, často časy společně s další faktor (2FA) jako telefonní hovor nebo texted kód, výsledkem je snadné nespokojenost, pokud má uživatel má můžete to udělat víc času pro produkt.

Kromě toho pokud používáte platformu identity pro využívající jiné aplikace může například Accounts Microsoft nebo pracovní účet z Office 365, zákazníci očekávají, že můžou být pověření k dispozici pro použití všech svých aplikací bez ohledu na to dodavatele.

Platformu Microsoft Identity spolu s naše SDK Identity Microsoft nemá všechny tento práce za vás a nabídne vám možnost delight zákazníkům s jednotným Přihlašováním buď v rámci vlastní sady aplikací nebo, stejně jako u naši stránku věnovanou zprostředkovatele funkce a aplikace ověřovacích dat přes celou zařízení.

V tomto návodu se dozvíte, jak nakonfigurovat naše SDK v rámci aplikace a poskytovat tuto výhodu zákazníkům.

Tento návod platí pro:

* Azure Active Directory
* B2C Azure Active Directory
* B2B Azure Active Directory
* Podmíněného přístupu Azure Active Directory

Všimněte si, že dokument dole předpokládá se mají informace o tom, jak [by aplikace na portálu starší verze služby Azure Active Directory](./develop/active-directory-how-to-integrate.md) i integrovali aplikace s [Androidem SDK Microsoft Identity](https://github.com/AzureAD/azure-activedirectory-library-for-android).

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

Pokud je kompatibilní zprostředkovatele nainstalovaný na zařízení, jako je aplikace ověřovacích dat Azure SDK Identity Microsoft automaticky provádět práce pro vyvolání zprostředkovatele za vás, když uživatel označuje, že se chcete přihlásit pomocí libovolného účtu z platformu Microsoft Identity. Může to být osobním Account Microsoft, pracovní nebo školní účet nebo účet poskytnout a hostovat v Azure pomocí našich B2C a B2B produktů. Pomocí velmi bezpečné algoritmy a šifrování jsme pověření se zajistila vyzývá k zadání a doručován bezpečně zpátky do okna aplikace. Přesné technické podrobnosti tyto mechanismy není publikovat ale byly vytvořili spolupráci Apple a Google.

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

Tady použijeme ADAL Android SDK:

- Zapnutí podporovaných jednotné přihlašování – zprostředkovatel sadu aplikací
- Zapnutí podpory pomoci zprostředkovatele jednotné přihlašování


### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Zapnutí jednotného přihlašování pro není zprostředkovatel pomoci jednotné přihlašování

Podporovaných jednotné přihlašování – zprostředkovatel ve všech aplikacích Microsoft Identity SDK Správa velkou část složitosti jednotné přihlašování za vás. Jedná se o hledání správná uživatelská v mezipaměti a udržování seznam přihlášeného uživatele můžete k vytvoření dotazu.

Chcete-li povolit jednotné přihlašování ve všech aplikacích vlastníte, abyste mohli postupujte takto:

1. Zajistit, aby všechny vaše uživatele aplikace se stejnými ID klienta nebo ID aplikace.
* Zajistěte, aby že všechny vaše aplikace mají stejnou sadu SharedUserID.
* Zajistit, aby všechny aplikace sdílet stejnou podpisového certifikátu z obchodu Google Play, takže můžete sdílet úložiště.

#### <a name="step-1-using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Krok 1: Použití stejné ID klienta / aplikace ID pro všechny aplikace v sadě aplikací

Objednávky pro platformu Microsoft Identity vědět, že mohou být tokeny zužitkování aplikace všech vašich aplikací, musí sdílení stejného ID klienta nebo ID aplikace. Toto je jedinečný identifikátor, který jste získali při registraci aplikace první na portálu.

Budete možná vás zajímá, jak bude různých aplikací ke službě Microsoft Identity určit, zda používá stejnou aplikace ID. Odpověď se **Přesměrovat URI**. Každá aplikace může obsahovat více přesměrovat identifikátory URI registrovaná na portálu rychlého připojení. Každý ve vaší sadě bude zobrazený různých přesměrování URI. Níže je příklad toho, jak to vypadá:

Přesměrovat Apl1 URI:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

Přesměrovat Apl2 URI:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

Přesměrovat App3 URI:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

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


#### <a name="step-2-configuring-shared-storage-in-android"></a>Krok 2: Konfigurace sdíleného úložiště v Androidu

Nastavení `SharedUserID` je nad rámec tento dokument, ale můžete dozvěděli o dokumentace Google Android na [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html). Důležité je rozhodnout, co se má váš sharedUserID se nazývá a použít ve všech aplikacích.

Až budete mít `SharedUserID` ve všech aplikacích budete chtít použít jednotné přihlašování.

> [AZURE.WARNING]
Při sdílení úložiště v aplikacích libovolné aplikaci můžete odstranit uživatele nebo horší odstranit všechny tokeny přes aplikaci. Je to zejména katastrofální, pokud máte aplikace, které jsou závislé na tokeny pozadí práce. Sdílení úložiště znamená, že musí být ve všech odebrat operace prostřednictvím SDK Identity Microsoft opatrní.

Je to! Microsoft Identity SDK nyní sdílet přihlašovací údaje ve všech aplikacích. Seznam uživatelů se sdílejí také všech instancí aplikace.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Zapnutí jednotného přihlašování pro zprostředkovatele pomoci jednotné přihlašování

Možnost pro aplikaci použít zprostředkovatele, který je nainstalovaný na zařízení je **ve výchozím nastavení vypnuté**. Abyste mohli používat aplikace s má zprostředkovatel musíte udělat některé další konfiguraci a přidání kódu pro některé aplikace.

Postupujte podle těchto kroků jsou:

1. Povolit režim zprostředkovatele v kódu aplikace volání MS SDK
2. Vytvoření nové přesměrování URI a poskytnout, aplikace a registrace aplikace
3. Nastavení oprávnění na správné Android manifest


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Krok 1: Povolení režimu zprostředkovatele v aplikaci
Možnost pro aplikaci používat zprostředkovatele je zapnutá při vytváření "nastavení" nebo počáteční instalace instance ověřování. Nastavením povahy ApplicationSettings v kódu udělejte toto:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Krok 2: Vytvoření nové přesměrování URI s schéma adresy URL

Zajistit, aby vždy vrácených tokeny přihlašovací údaje správné použití potřebujeme Ujistěte se, že jsme volání zpět do aplikace tak, že můžete ověřit operační systém Android. Operační systém Android používá algoritmus hash certifikátu v obchodu Google Play store. To nejde falešné aplikací podvodné. Proto doporučujeme využít to společně s URI naše zprostředkovatele aplikace zajistit, že tokeny vracejí do správné použití. Jsme vyžadují, abyste vytvořte tomuto jedinečné přesměrování URI obou v aplikaci a nastavit jako identifikátor URI přesměrovat naše portálu vývojář.

Přesměrování URI musí být ve formuláři správné:

`msauth://packagename/Base64UrlencodedSignature`

ex: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Tento identifikátor URI přesměrovat musí být zadán v aplikaci registrace pomocí [Azure klasické portálu](https://manage.windowsazure.com/). Další informace o registraci aplikace Azure AD najdete v článku [Integrace se službou Azure Active Directory](./develop/active-directory-how-to-integrate.md).


#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Krok 3: Nastavení správná oprávnění v aplikaci

Náš zprostředkovatele aplikaci Android používá funkci správci účtů operační systém Android Správa přihlašovacích údajů v aplikacích. Mohli použít zprostředkovatele v Androidu manifestu aplikace musíte mít oprávnění k používání AccountManager účtů. To je popsána podrobně [Google si přečtěte následující dokumentaci pro Account Manager tady] (http://developer.android.com/reference/android/accounts/AccountManager.html)

Zejména je tato oprávnění:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Máte nastavený jednotné přihlašování!

Teď Microsoft Identity SDK bude automaticky zužitkování přihlašovacích údajů aplikace i vyvolat má zprostředkovatel, pokud je k dispozici na svém zařízení.
