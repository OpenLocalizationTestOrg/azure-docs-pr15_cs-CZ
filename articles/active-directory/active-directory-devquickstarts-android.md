<properties
    pageTitle="Začínáme Azure AD Android | Microsoft Azure"
    description="Vytvoření Android aplikace, která lze integrovat s Azure AD pro přihlášení a volá Azure AD zamknutí rozhraní API pomocí OAuth."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Integrace Azure AD do aplikace pro Android

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Pokud desktopovou aplikaci vyvíjíte, Azure AD umožňuje rychle a jednoduše k ověření uživatele pomocí svých účtů služby Active Directory.  Umožňuje taky aplikaci bezpečnému používání všechny webového rozhraní API chráněn Azure AD, například rozhraní API Office 365 nebo rozhraní API Azure.

Pro Android klienty, které se potřebují dostat k chráněným prostředkům Azure AD poskytuje Active Directory Authentication Library nebo ADAL.  ADAL je jediný v životnost slouží k Usnadněte aplikace získat přístup tokeny.  Která ukazuje, jak snadno, tady abychom budete vytvářet aplikace Android seznam úkolů, který:

-   Získá přístup k tokeny pro volání rozhraní API seznamu úkolů pomocí [OAuth 2.0 ověřovací protokol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Získá uživatel v seznamu úkolů
-   Symboly uživatelé mimo.

Nejdřív musíte mít tenanta Azure AD, ve kterém můžete vytvářet uživatele a registrace aplikace.  Pokud ještě nemáte klienta, [Přečtěte si, jak ho získat](active-directory-howto-tenant.md).

> [AZURE.TIP] Vyzkoušejte předběžnou verzi našeho nového [portál pro vývojáře](https://identity.microsoft.com/Docs/Android) , které vám pomůžou do začátků s Azure Active Directory za několik okamžiků!  Portálu pro vývojáře vás provede jednotlivými procesu registrace aplikace a integrace Azure AD do kódu.  Když budete mít hotové, bude mít jednoduchou aplikaci, která může ověřovat uživatele do vašeho tenanta a back-end, které můžete přijmout tokeny a ověření. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Krok 1: Stáhněte si a běží Server ukázkové Node.js REST API úkol

V tomto příkladu je zapsán zvlášť, aby spolupracoval s našimi ukázkovými existující pro vytváření jednoho tenanta, kterého úkol REST API pro Microsoft Azure Active Directory. Toto je předpoklad rychlý Start.

Informace o tom, jak nastavit, navštivte naše existující vzorky tady:

* [Microsoft Azure Active Directory ukázkové REST API služby pro Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Krok 2: Zaregistrujte webového rozhraní API u vašeho klienta AD Microsoft Azure

**Co mám udělat?**

*Microsoft Active Directory podporuje přidávání dva typy aplikací. Webového rozhraní API, které nabízejí služby pro uživatele a aplikace (buď na webu nebo z aplikace spuštěna na zařízení) přístup k rozhraní API tyto Web. V tomto kroku registrujete rozhraní API webových spustíte místně testování v tomto příkladu. Tento rozhraní API webových by zpravidla služba REST, která nabízí funkce má aplikace pro přístup k. Microsoft Azure Active Directory můžete chránit všechny koncový bod!*

*Tady jsme jsou za předpokladu, že registrujete rozhraní REST API úkol výše uvedené, ale to funguje pro všechny rozhraní API webových je vhodné Azure Active Directory chránit.*

Kroky pro registraci rozhraní API webových s Microsoft Azure AD

1. Přihlaste se k [portálu Správa Azure](https://manage.windowsazure.com).
2. Klikněte na služby Active Directory v levém horním nav.
3. Klikněte na adresář klienta, kde se chcete zaregistrovat ukázková aplikace.
4. Klikněte na kartu aplikace.
5. V okraj klikněte na Přidat.
6. Klikněte na "Přidat aplikaci, kterou organizaci vyvíjí".
7. Zadejte popisný název vyberte aplikaci, třeba "TodoListService", "Webové aplikace a či nebo rozhraní API webových" a klikněte na další.
8. Přihlašování adresy URL, zadejte základní adresu URL pro vzorku, který je ve výchozím nastavení `https://localhost:8080`.
9. Identifikátor URI ID aplikace zadejte `https://<your_tenant_name>/TodoListService`, nahrazení `<your_tenant_name>` s názvem Azure AD klienta.  Klikněte na OK dokončete registraci.
10. Zůstaňte ještě v portálu Azure klikněte na kartu Konfigurovat aplikace.
11. **Vyhledání hodnotu ID klienta a zkopírujte ji zrušit**, budete potřebovat, později při konfiguraci aplikace.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Krok 3: Registrace aplikace Android Native Client – ukázka

Cílem prvního kroku je registrace webové aplikace. Pak budete potřebovat sdělte aplikace a služby Azure Active Directory. Díky aplikaci komunikovat jenom registrovaných rozhraní API webových

**Co mám udělat?**  

*Výše uvedenou, Microsoft Azure Active Directory podporuje přidávání dva typy aplikací. Webového rozhraní API, které nabízejí služby pro uživatele a aplikace (buď na webu nebo z aplikace spuštěna na zařízení) přístup k rozhraní API tyto Web. V tomto kroku registrujete aplikace v tomto příkladu. Musíte to udělat v pořadí pro tuto aplikaci mají být k žádosti o přístup k rozhraní API webových jenom registrován. Azure Active Directory odmítne i povolit aplikaci požádání uživatele o přihlášení, pokud je registrovaná! Která je součástí zabezpečení modelu.*

*Tady jsme jsou za předpokladu, že registrujete Tato ukázková aplikace výše uvedené, ale to funguje pro všechny aplikace, které vyvíjíte.*

**Proč mám vložení aplikace a rozhraní API webových do jednoho klienta?**

*Jak se může mít uhodnout, vytvoříte aplikace, která má přístup k externím rozhraní API, které máte zaregistrované v Azure Active Directory z jiného klienta. Pokud to uděláte, se vaši zákazníci výzva k souhlasu s používáním rozhraní API aplikace. Části hodní se má na starosti Active Directory Authentication Library pro iOS svůj souhlas za vás. Jak můžeme vstoupit do pokročilejších funkcí, uvidíte, že toto je důležitou součástí práce potřebné pro přístup k sadu Microsoft APIs z Azure a Office, jakož i jiné poskytovatele služeb. Prozatím protože registrované rozhraní API webových a aplikace v části stejném klientovi neuvidíte výzev o souhlas. Je to obvykle případ Pokud vyvíjíte aplikace určené pro svou vlastní firmu používat.*

1. Přihlaste se k [portálu Správa Azure](https://manage.windowsazure.com).
2. Klikněte na služby Active Directory v levém horním nav.
3. Klikněte na adresář klienta, kde se chcete zaregistrovat ukázková aplikace.
4. Klikněte na kartu aplikace.
5. V okraj klikněte na Přidat.
6. Klikněte na "Přidat aplikaci, kterou organizaci vyvíjí".
7. Zadejte popisný název aplikace, například "TodoListClient Android", zaškrtněte políčko "nativní klientské aplikace" a klikněte na další.
8. Identifikátor URI přesměrovat zadejte `http://TodoListClient`.  Klikněte na Dokončit.
9. Klikněte na kartu Konfigurovat aplikace.
10. Vyhledání hodnoty ID klienta a zkopírujte ho zrušit, budete potřebovat později při konfiguraci aplikace.
11. "Oprávnění do jiných aplikací" klikněte na "Přidat aplikaci."  "Jiné" vyberte v rozevíracím seznamu "Zobrazit" a klikněte na horní značka zaškrtnutí.  Vyhledejte a klikněte na TodoListService a klepněte na značku zaškrtnutí dole na Přidat aplikaci.  Vyberte z rozevíracího seznamu "Delegovat oprávnění" "Access TodoListService" a uložte konfigurace.



Pokud chcete vytvořit s Maven, můžete pom.xml na nejvyšší úrovni

  * Klonovat tento repo v adresáři podle svého výběru:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Postupujte podle pokynů v [části Prerequests nastavit maven pro android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * Nastavení emulátoru s SDK 19
  * Přejděte do kořenové složky, kde klonovat repo
  * Spuštění příkazu: instalace mvn vyčistit
  * Přejděte v adresáři ukázku rychlý Start: samples\hello disk cd
  * Spuštění příkazu: mvn android: nasazení android: spustit
  * Měli byste vidět spuštění aplikace
  * Zadejte přihlašovací údaje uživatele test zkusit!

Sklenice balíčků taky odeslán vedle balíček aar.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Krok 4: Stáhněte si Android ADAL a přiřadit ji někomu zatmění pracovního prostoru

Jsme jste provedli usnadňuje můžete použít tuto knihovnu Android projektu několika způsoby:

* Zdrojový kód můžete použít naimportujte tuto knihovnu zatmění a odkaz na aplikace.
* Pokud používáte Android Studio, můžete použít formát *aar* balíčku a odkazovat binární.

####<a name="option-1-source-zip"></a>Možnost 1: Zip zdroj

Ke stažení kopie zdrojového kódu, "Stáhnout ZIP" v pravé části stránky klepněte na tlačítko klikněte [sem](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>Možnost 2: Zdroj prostřednictvím libovolná

Chcete-li získat zdrojového kódu SDK prostřednictvím libovolná napsat:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Možnost 3: Binární prostřednictvím Gradle

Binární soubory můžete získat z centrálního repo Maven. AAR balíčku můžete takto součástí projektu v AndroidStudio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>Možnost 4: aar prostřednictvím Maven

Pokud používáte modul plug-in m2e v zatmění, je možné zadat závislost v souboru pom.xml:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Možnost 5: sklenice balíčku do složky knihoven
Můžete získat sklenice soubor z maven repo a přetáhněte do složky *knihoven* v projektu. Musíte zkopírovat požadovaných zdrojů do projektu a od balíčků sklenice Nezahrnovat.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Krok 5: Přidání odkazů na Android ADAL do projektu


2. Přidání odkazu do projektu a zadejte jako Android knihovna. Pokud si nejste jisti, jak to udělat, [kliknutím sem zobrazíte další informace] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. Přidání závislosti projektu pro ladění v nastavení projektu

4. Aktualizace projektu AndroidManifest.xml v souboru:

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Vytvoření instance AuthenticationContext na hlavní činnosti. Podrobnosti o volání jsou nad rámec tohoto souboru README, ale dostanete dobré start pohledem na [Android nativního vzorku klienta](https://github.com/AzureADSamples/NativeClient-Android). Tady je příklad:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * Poznámka: mContext je pole v o aktivitách

8. Zkopírujte tento kód blok zpracovávání konec AuthenticationActivity poté, co uživatel zadá pověření volání a přijímání se tak mohli ověřovat kód:

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. Kdybyste žádali o token, definujete zpětné

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Nakonec požádejte o token pomocí této zpětné:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

Vysvětlení parametrů:

  * Zdroje je vyžadován a je zdroj, který se pokoušíte otevřít.
  * ClientID požaduje a pochází z portálu AzureAD.
  * RedirectUri můžete nastavit jako svůj název balíčku. Není nutné stanovit acquireToken volání.
  * PromptBehavior pomáhá kdybyste žádali o přihlašovací údaje pro přeskočit mezipaměti a souborů cookie.
  * Po povolení kód vyměňovat pro token bude zpětné s názvem.

  Zpětné bude mít objektu AuthenticationResult, který má accesstoken, datum, kterým vypršela platnost a idtoken informace.

Volitelné: **acquireTokenSilent**

Můžete volat **acquireTokenSilent** zpracovávání ukládání do mezipaměti a tokenu aktualizace. Poskytuje taky verze synchronizace. Přijme uživatelské ID jako parametr.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Zprostředkovatel**: aplikaci portálu společnosti Microsoft Intune poskytne komponentu zprostředkovatele. ADAL bude pomocí zprostředkovatele účet, pokud existuje jednoho uživatelského účtu se vytvoří na tento nástroj pro ověření, vývojář rozhodnete ho přeskočit. Karta Vývojář můžete přejít zprostředkovatele uživateli:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 Je třeba zaregistrovat zvláštní redirectUri nápovědu zprostředkovatel. RedirectUri je ve formátu msauth://packagename/Base64UrlencodedSignature. Získáte vaší redirecturi aplikace pomocí skriptu "brokerRedirectPrint.ps1" nebo pomocí rozhraní API volání mContext.getBrokerRedirectUri. Podpis souvisí s podpisového certifikátu.

 Zprostředkovatel modelu je pro jednoho uživatele. AuthenticationContext poskytuje rozhraní API způsob, jak získat uživatele zprostředkovatele.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Zprostředkovatel uživatele budou vráceny, pokud je platný účet.

 Manifestu aplikace musí mít oprávnění k používání AccountManager účtů: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


Pomocí tohoto návodu, měli byste mít co byste měli úspěšně integrace se službou Azure Active Directory. Další příklady tento práce, získáte v blogu AzureADSamples / úložiště na GitHub.

## <a name="important-information"></a>Důležité informace

### <a name="customization"></a>Vlastní nastavení

Knihovna zdrojů projektu může přepsat prostředky aplikace. K tomu dojde při vytváření aplikace. Z tohoto důvodu můžete upravit rozložení ověřování aktivity požadovaným způsobem. Musíte zajistit, aby id ovládací prvky, které ADAL uses(Webview).

### <a name="broker"></a>Zprostředkovatel

Zprostředkovatel component budete dostávat s aplikaci portálu společnosti Microsoft Intune. Účet se vytvoří ve Správci účtů. Typ účtu je "com.microsoft.workaccount". Umožňuje pouze jeden jednotného přihlašování k účtu. Po dokončení zařízení ověřovací kód programu pro některé z aplikací vytvoří soubor cookie jednotného přihlašování pro tohoto uživatele.

### <a name="authority-url-and-adfs"></a>Adresa Url autorita a služby AD FS

Služby AD FS nebyl rozpoznán jako výrobní STS, takže budete muset tuto instanci zjišťování a uplynout false AuthenticationContext konstruktor.

Adresa url autorita potřebuje STS instanci a název klienta: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Dotazování položky v mezipaměti

ADAL obsahuje výchozí mezipaměti SharedPreferences s některé jednoduché mezipaměti funkce dotazu. Aktuální mezipaměti se dá dostat z AuthenticationContext se:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Můžete také zadat implementace mezipaměti, pokud chcete upravit.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL poskytuje možnost chování výzvy zadejte. Pokud aktualizace token je neplatný a jsou vyžadovány přihlašovací údaje uživatele, objeví se PromptBehavior.Auto nahoru uživatelského rozhraní. PromptBehavior.Always přeskočit použití mezipaměti a pořád zobrazoval uživatelského rozhraní.

### <a name="silent-token-request-from-cache-and-refresh"></a>Žádost o pasivní token z mezipaměti a aktualizace

Tento způsob nepoužívá uživatelského rozhraní pop nahoru a není nutné zadávat činnosti. Vrátí tokenů z mezipaměti, pokud je k dispozici. Pokud vypršela platnost tokenu pokusí o její obnovení. Pokud aktualizace token vyprší nebo se nezdařilo, vrátí AuthenticationException.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

Lze také nastavit synchronizaci volání tímto způsobem. Můžete nastavit hodnotu null zpětné nebo použít acquireTokenSilentSync.

### <a name="diagnostics"></a>Diagnostika

Následují primární zdroje informací o Diagnostika problémů:

+ Výjimky
+ Protokoly
+ Trasování sítě

Taky Všimněte si, že ID korelace centrální Diagnostika v knihovně. Můžete nastavit svůj korelace ID na základě na žádost o, pokud chcete ke koordinaci ADAL požádat o další operace v kódu. Pokud bude ADAL generovat náhodných jednu and všechny nenastavíte id korelace bude s id korelace razítkem protokolu zpráv a hovorů síť. Vlastní vygeneruje id změny na každou žádost.

#### <a name="exceptions"></a>Výjimky

Toto je očividně první diagnostic. Pokusíme poskytnout užitečné chybové zprávy. Pokud zjistíte, který není užitečné oznamte problému a dejte nám vědět. Zadejte informace o zařízení například modelu a SDK # taky.

#### <a name="logs"></a>Protokoly

Můžete nakonfigurovat knihovnu generování protokolu zpráv, které můžete použít k lépe diagnostikovat potíže. Konfigurace protokolování provedením následujících volání konfigurace zpětné ADAL použije předat vypnout každá zpráva protokolu je vygenerovaný.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Zprávy můžou být došlo k zápisu vlastního souboru protokolu jak je vidět níže. Bohužel nejde žádným způsobem standardní získání protokoly ze zařízení. Existují některé služby, které vám pomohou s tímto. Se můžete taky vytvořte vlastní, například odeslání souboru na server.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Úroveň protokolování

+ Error(Exceptions)
+ Warn(warning)
+ Info (informace jako referenci)
+ Podrobné (podrobnosti)

Je třeba nastavit úroveň protokol takto:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Všechny zprávy protokolu jsou odeslány logcat kromě zpětná všechny vlastní protokolu.
Získat protokol do formuláře logcat soubor jako zachycujících belog:

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Další příklady o příkazů adb: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Trasování sítě

Používání různých nástrojů k zaznamenání přenosy protokolu HTTP generovaný ADAL.  Toto je nejužitečnější, pokud máte zkušenosti s protokolem OAuth nebo pokud je třeba zadat diagnostické informace společnosti Microsoft nebo jiných kanálů podpory.

Fiddler je nástroj pro trasování nejjednodušší HTTP.  Pomocí následujících odkazů nastavit až správně záznamu ADAL v síti.  Abyste mohli být užitečné, je nutné konfigurace fiddler nebo jiný nástroj například Charlese pořizovat záznam nešifrovaném přenosy SSL.  Poznámka: Trasování generované tímto způsobem může obsahovat vysoce privilegovaných informace například tokeny přístupu, uživatelská jména a hesla.  Pokud používáte výrobní účty, Nesdílet tyto stopy s 3 stran.  Pokud budete muset zadat trasování někomu, abyste mohli získávat podporu, reprodukování problému s účtem dočasné s uživatelská jména a hesla, která vám nevadí sdílení.

+ [Nastavení Fiddler pro Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [Konfigurace Fiddler pravidla pro ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Dialogové okno režimu
Metoda acquireToken bez aktivity podporuje řádku dialogové okno.

### <a name="encryption"></a>Šifrování

ADAL šifruje tokeny a úložiště přihlašovacích údajů v SharedPreferences ve výchozím nastavení. Můžete si může samostatně prohlížet třídě StorageHelper zobrazovat si podrobnosti. Android zavádí AndroidKeyStore pro 4.3(API18) zabezpečeného úložiště privátních klíčů. ADAL použije pro API18 a vyšší. Pokud chcete použít ADAL pro nižší SDK verze, je třeba zadat tajné klíč na AuthenticationSettings.INSTANCE.setSecretKey

### <a name="oauth2-bearer-challenge"></a>Ověřovací kód programu Oauth2 nosný

Třídy AuthenticationParameters poskytuje funkce Přejít authorization_uri z ověřovací kód programu Oauth2 nosný.

### <a name="session-cookies-in-webview"></a>Soubory cookie relací ve webovém zobrazení

Android webové zobrazení vymažte soubory cookie relací po zavření aplikace. Můžete to zpracovávat kódem ukázkové níže:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Další informace o soubory cookie: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Přepsání zdroje

ADAL knihovna obsahuje anglické řetězce pro následující dvě ProgressDialog zprávy.

Aplikace je měli přepsat, pokud budete chtít lokalizované řetězce.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>Dialogové okno NTLM
Adal 1.1.0 podporuje NTLM dialogové okno, které zpracovávají prostřednictvím onReceivedHttpAuthRequest událost z WebViewClient. Dialogové okno rozložení a řetězce můžete přizpůsobit.

### <a name="cross-app-sso"></a>Jednotné přihlašování více aplikací
Zjistěte, [jak povolit více aplikací SSO na Androidu pomocí ADAL](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
