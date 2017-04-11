<properties
    pageTitle="Azure Active Directory B2C: Volání webového rozhraní API z aplikace Android | Microsoft Azure"
    description="Tento článek vám ukáže, jak vytvořit Androidu úkolů seznamu aplikace, která volá Node.js webového rozhraní API pomocí tokeny nosný OAuth 2.0. Aplikace Androidu a webového rozhraní API umožňuje Azure Active Directory B2C správy identit uživatelů a ověřování uživatelů."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD B2C: Volání webového rozhraní API z aplikace Android

> [AZURE.WARNING] Tento kurz vyžaduje některé důležité aktualizace, konkrétně odebrat použití ADAL Android B2C.  Budeme publikovat svěže pokyny k používání Azure AD B2C v aplikacích pro Android v příští týden a doporučujeme držící do té doby.  Ale pokud chcete co potřebujete, zkuste, neváhejte budeme pokračovat v níže uvedených článků.



Pomocí služby Azure Active Directory (Azure AD) B2C můžete přiřadit výkonné identit samoobslužné funkce správy Android aplikace a webového rozhraní API v krátkém chvilku. Tento článek vám ukáže, jak vytvořit Androidu "seznam úkolů" aplikace, která volá Node.js webového rozhraní API pomocí tokeny nosný OAuth 2.0. Aplikace Androidu a webového rozhraní API umožňuje Azure AD B2C správy identit uživatelů a ověřování uživatelů.

Tento rychlý úvod vyžaduje, abyste měli webového rozhraní API, který je chráněn Azure AD pomocí B2C plně pracovat. Jsme jste vytvořili jeden pro .NET a Node.js k použití. Tento walk-through předpokládá, že ukázku webového rozhraní API Node.js nakonfigurován. Další informace najdete v tématu [Azure AD B2C webového rozhraní API pro kurz Node.js](active-directory-b2c-devquickstarts-api-node.md).

Azure AD pro Android klienti, které se potřebují dostat k chráněné prostředky, obsahuje Active Directory ověřování knihovna (ADAL). ADAL účel jediným tak, aby snadno získat přístup tokeny aplikace. Chcete-li ukazují, jak je snadné, v této příručce abychom budete vytvářet aplikace Android úkolů seznam, který:

- Získá přístup tokeny volajících seznam úkolů rozhraní API aplikace pomocí [ověřování protokolem OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
- Získá seznamů úkolů.
- Znaky mimo uživatelů.

> [AZURE.NOTE] Tento článek nepokrývá jak implementovat přihlášení, registrace a správy profilů pomocí Azure AD B2C. Se zaměřuje na způsob kontaktování webového rozhraní API po ověření uživatele. Pokud jste to ještě neudělali, by měly začít s [.NET web app kurz Začínáme](active-directory-b2c-devquickstarts-web-dotnet.md) se dozvíte o základních funkcích Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Získat Azure AD B2C adresář

Než budete moct použít Azure AD B2C, musíte vytvořit adresář nebo klient. Adresář je kontejner pro všechny uživatele aplikace, skupiny a další. Pokud nemáte už, [vytvořte adresář B2C](active-directory-b2c-get-started.md) před pokračovat v této příručce.

## <a name="create-an-application"></a>Vytvoření aplikace

Dál je potřeba vytvořit aplikace ve vašem adresáři B2C. To vám Azure AD informace, které potřebuje bezpečně komunikovat s aplikací. Aplikace a webového rozhraní API představují jednoho **ID aplikace** v tomto případě protože představují jedné logické aplikaci. Vytvoření aplikace pro, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md). Nezapomeňte:

- Zahrnout z **web appu**/**webového rozhraní API** aplikace.
- Zadejte `urn:ietf:wg:oauth:2.0:oob` jako **Adresa URL odpověď**. Je výchozí adresu URL Tato ukázka kódu.
- Vytvoření **aplikace tajná** aplikace a zkopírujte je. Budete ho později potřebovat. Všimněte si, že tato hodnota musí být před použitím [uvést XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) .
- Zkopírujte **ID aplikace** přiřazená aplikace. Budete taky potřebovat to později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvoření pravidla

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

V Azure AD B2C každé uživatelské prostředí je definován [zásad](active-directory-b2c-reference-policies.md). Tato aplikace obsahuje tři identity prostředí: registrace, přihlaste se a přihlášení pomocí služby Facebook.  Je potřeba vytvořit jednu zásadu každého typu způsobem popsaným v [zásad referenční článek](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Při vytváření tři zásady nezapomeňte:

- Zvolte **zobrazovaného názvu** a jiných registrace atributy registrace zásad.
- V každé zásady zvolte deklarace aplikace **zobrazované jméno** a **ID objektu** . Můžete použít i jiné deklarované.
- Zkopírujte **název** každého zásadu po jeho vytvoření. By měla být předponu `b2c_1_`.  Musíte mít tyto zásady názvy později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření tři zásady budete připraveni k vytvoření aplikace.

Všimněte si, že tento článek nepokrývá použití zásad, které jste právě vytvořili. Další informace o tom, jak fungují zásady v Azure AD B2C, začněte s [.NET web app kurz Začínáme](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Stáhněte si tento kód

Kód pro tento kurz [spravován na GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android). K vytvoření ukázky průběžně, můžete si [Stáhnout kostry projekt jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). Můžete taky zkopírovat kostra:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **Budete vyzváni ke stažení osnovu pro dokončení tohoto kurzu.** Z důvodu složitost provádění aplikace plně funkční Android kostra obsahuje kód činnosti koncového uživatele, který se spustí po dokončení tohoto kurzu. Toto je míra ušetřit čas pro vývojáře. Kód činnosti koncového uživatele není germane na téma Přidání B2C aplikace Android.

Dokončené aplikace je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) nebo `complete` větev stejném úložišti.

Pokud chcete vytvořit s Maven, můžete použít `pom.xml` na nejvyšší úrovni.

  1. Postupujte podle kroků uvedených v [části požadavky nastavit Maven pro Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
  2. Nastavte si emulátoru s SDK 21.
  3. Přejděte do kořenové složky, kde klonovat repo.
  4. Spusťte příkaz `mvn clean install`.
  5. Přejděte v adresáři ukázku rychlý úvod `cd samples\hello`.
  6. Spusťte příkaz `mvn android:deploy android:run`.

Měli byste vidět spuštění aplikace. Zadejte přihlašovací údaje uživatele test to vyzkoušet.

Balíčky Java archivu (SKLENICE) se také odeslaný vedle balíček Android archivu o Příchodu.

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>Stáhněte si Android ADAL a přiřadit ji někomu Android Studio pracovního prostoru

Máte možnosti informace o použití této knihovny Android projektu:

* Zdrojový kód můžete použít naimportujte knihovnu zatmění a odkaz na aplikace.
* Pokud používáte Android Studio, můžete použít formát AAR balíčku a odkazovat binární.

### <a name="option-1-binaries-via-gradle-recommended"></a>Možnost 1: Binární prostřednictvím Gradle (doporučeno)

Binární soubory můžete získat z centrálního repo Maven. Balíček AAR mohou být součástí projektu v Android Studio (například v `build.gradle`) takto:

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
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>Možnost 2: AAR prostřednictvím Maven

Pokud používáte `m2e` modul plug-in v zatmění, můžete zadat závislosti na vaší `pom.xml` souboru:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>Možnost 3: Zdroj prostřednictvím libovolná (poslední středisko)

Zdrojový kód SDK prostřednictvím libovolná získáte zadejte:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Použití pobočky **konvergence.**

## <a name="set-up-your-configuration-file"></a>Nastavení konfigurační soubor

Použití konfigurace, který jste si nastavili dřív v portálu B2C konfigurace Android projektu.

Otevřít `helpes/Constants.java` a vyplnit hodnoty pro následující:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: Obory, které předáváte serveru, na kterém chcete požádat o zpět ze serveru, pokud uživatel přihlásí. Pro náhled B2C předáte `client_id`. Však očekává se, že můžete změnit na `read scopes` v budoucnu. Tento dokument aktualizují, dojde.
- `ADDITIONAL_SCOPES`: Další obory, které chcete použít pro aplikaci. Očekává se použije v budoucnu.
- `CLIENT_ID`: ID aplikace, kterou jste získali od portálu.
- `REDIRECT_URL`: Přesměrování kde očekáváte token účtován zpět.
- `EXTRA_QP`: Žádné další chcete předat serveru ve formátu kódování URL.
- `FB_POLICY`: Zásada, které nechcete. Sem zadejte tu část nejdůležitější pro tento walk-through.
- `EMAIL_SIGNIN_POLICY`: Zásada, které nechcete. Sem zadejte tu část nejdůležitější pro tento walk-through.
- `EMAIL_SIGNUP_POLICY`: Zásada, které nechcete. Sem zadejte tu část nejdůležitější pro tento walk-through.

## <a name="add-references-to-android-adal-to-your-project"></a>Přidání odkazů na Android ADAL do projektu

> [AZURE.NOTE]  ADAL pro Android používá model na základě záměr vyvolat ověřování. Způsoby "rozložení" aplikaci pro práci. V tomto příkladu celý a všechny ADAL pro Android centra o tom, jak spravovat způsoby a předejte informace mezi nimi.

Android nejdřív sdělte rozložení aplikace, včetně způsoby, které chcete použít. Tyto způsoby bude vysvětlení podrobně dál v tomto kurzu.

Aktualizovat svůj projekt `AndroidManifest.xml` soubor, který chcete zahrnout všechny vaše způsoby:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Jak vidíte, definujete pět aktivity. Všechny tyto použijete.

- `AuthenticationActivity`: Spojit ADAL a poskytuje zobrazení webového přihlášení.
- `LoginActivity`: Zobrazí se vaše přihlašovací zásady a tlačítky pro každého zásadu.
- `SettingsActivity`: Tento změníte pomocí nastavení aplikace za běhu.
- `AddTaskActivity`: Můžete slouží k přidání úkolů do svého rozhraní REST API, které jsou chráněny Azure AD.
- `ToDoActivity`: Toto je hlavní činnost, která se zobrazí úkoly.

## <a name="create-the-sign-in-activity"></a>Vytvoření aktivity přihlášení

Vytvořte hlavní aktivitu a zavolejte ho `LoginActivity`.

Vytvoření souboru s názvem `LoginActivity.java`.

Potřebujete inicializace aktivity a přidat několik tlačítek, která bude řídit uživatelského rozhraní. To je známý Android kód jste nenapsali.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
Teď jste vytvořili tlačítka, která hovorů vaše `ToDoActivity` záměr (který volá ADAL, když potřebujete token). Je to pomocí aktivitách jako odkaz a další parametr. Tento parametr navíc předaných `intent.putExtra()` metody. Definování `"thePolicy"` pomocí, co jste zadali v `Constants.java`. To, bude záměr zásad vyvolat při ověřování.

## <a name="create-the-settings-activity"></a>Vytvoření aktivity nastavení

Toto je aktivity, který naplní nastavení uživatelského rozhraní.

Vytvoření souboru s názvem `SettingsActivity.java` pro jednoduché vytvořit, číst, aktualizovat a odstraňovat operace (CRUD).

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>Vytvoření aktivity přidat úkol

Tato akce slouží k přidání úkolu do koncového bodu rozhraní REST API.

Vytvoření souboru s názvem `AddTaskActivity.java` a napište následující.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>Vytvoření aktivity seznam úkolů

Toto je nejdůležitější aktivity. Můžete využít, aby se zobrazilo token z Azure AD pro zásady a pomocí token zavolat serveru rozhraní REST API úkolu.

Vytvoření souboru s názvem `ToDoActivity.java` a napište následující. (Volání bude vysvětleno později.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 Možná jste si všimli, že to závisí na metody, které nebyly dosud vloženy. Obsahují `updateLoggedInUser()`, `clearSessionCookie()`, a `getTasks()`. Budete psát můžou být v této příručce. Chyby ve Android Studio můžete ignorovat nyní.

Vysvětlení parametrů:

  - `SCOPES`: Potřeby obory zkoušíte požádat o přístup. Náhled B2C to je stejný jako `client_id`, ale očekává se, v budoucnosti změnit.
  - `POLICY`: Zásada kdy se má ověření uživatele.
  - `CLIENT_ID`: Povinné, pochází z portálu Microsoft Azure AD.
  - `redirectUri`: Můžete nastavit jako název balíčku. Nemusejí být k dispozici pro `acquireToken` volání.
  - `getUserInfo()`: Umožňuje vyhledat o tom, jestli je uživatel již v mezipaměti. Parametr také popisuje výzvu Pokud tomuto uživateli nebyl nalezen nebo uživatele přístupový token je neplatný. Tento způsob budou napsané v této příručce.
  - `PromptBehavior.always`: Umožňuje po nich žádáte přihlašovací údaje pro přeskočit mezipaměti a souborů cookie.
  - `Callback`: Volat autorizační kód vyměňovat pro token. Bude mít objektu `AuthenticationResult`, který obsahuje přístupový token, datum vypršení platnosti a ID tokenu informace.

> [AZURE.NOTE]  Aplikaci portálu společnosti Microsoft Intune poskytuje komponentu zprostředkovatel a může být nainstalovaný na zařízení uživatele. Aplikace poskytuje jediné přihlašování (SSO) aplikace access ve všech aplikacích v zařízení. Vývojáři by měl být připraven umožňující Intune. Pokud není jeden uživatelský účet vytvořený v ověřovacích dat ADAL pro Android používat účet zprostředkovatele. Abyste mohli použít zprostředkovatele, musí vývojář zaregistrovat speciální `redirectUri` pro zprostředkovatele používat. `redirectUri`je ve formátu msauth://packagename/Base64UrlencodedSignature. Můžete získat `redirectUri` aplikace pomocí skriptu `brokerRedirectPrint.ps1` nebo pomocí volání rozhraní API `mContext.getBrokerRedirectUri()`. Podpis souvisí s podpisového certifikátu z Google Play storu.

 Zprostředkovatel uživatele můžete přejít pomocí:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] Pokud chcete zjednodušit tento rychlý úvod B2C jsme se přihlásily přeskočit zprostředkovatele ve výběru.

Dále vytvořte pomocné metody k získání tokenu pouze během volání ověřování úkolu rozhraní API.

Ve stejném `ToDoActivity.java` soubor, napsat následující.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Také přidat metody, které bude "nastavit" a "získat" `AuthenticationResult` (se tokenu) do globální šablony `Constants`. I když `ToDoActivity.java` používá `sResult` v jeho toků, budete muset přidat těchto postupů. Pokud nechcete, vašich aktivit nebudou mít přístup k token fungují (třeba přidat úkol na `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>Vytvoření metodu pro návrat identifikátor uživatele

ADAL pro Android představuje uživatele ve formě `UserIdentifier` objektu. Slouží ke správě uživatele. Objekt slouží k určení, zda používá jednoho uživatele ve volání. Pomocí těchto informací můžete přes v mezipaměti, místo volání na server. Pro zjednodušení, jsme vytvořili `getUserInfo()` metodu, která vrací `UserIdentifier`. Můžete se `acquireToken()`. Jsme také vytvořili `getUniqueId()` metodu, která vrátí ID `UserIdentifier` v mezipaměti.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Psaní pomocné metody

Dále napište některé helper způsobů vymažte soubory cookie a poskytovat jim `AuthenticationCallback`. Těchto postupů slouží výhradně k výběru a ujistěte se, že se účastníte čistého stavu při volání na váš `ToDo` aktivity.

Ve stejném souboru s názvem `ToDoActivity.java`, napište následující.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>Volání úkolu rozhraní API

Po aktivitě chtít uchopte tokeny můžete napsat API pro přístup k serveru úkolu.

`getTasks`obsahuje pole, které představuje úkoly na serveru.

Začít s `getTasks`.

Ve stejném souboru s názvem `ToDoActivity.java`, napište následující.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

Také napište metodu, která spustí tabulkách při prvním spuštění.

Ve stejném souboru s názvem `ToDoActivity.java`, napište následující.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

Uvidíte, že tento kód vyžaduje některé další metody dělat svou práci. Napište tyto další.

### <a name="create-an-endpoint-url-generator"></a>Vytvoření generátor koncový bod adresy URL

Potřebujete generovat adresa URL koncového bodu, ke kterému se připojujete k. To udělejte ve stejném souboru třídy.

**Ve stejném souboru** s názvem `ToDoActivity.java`, napište následující.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Poznámka: Přidání přístupový token požadavek na kód popisované v další části.

## <a name="write-some-ux-methods"></a>Psaní některé metody činnosti koncového uživatele

Android vyžaduje zpracovat některé zpětná k ovládání aplikace. Toto jsou `createAndShowDialog` a `onResume()`. To je známý Android kód jste nenapsali.

Ve stejném souboru s názvem `ToDoActivity.java`, napište následující.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Pak můžete spravujte zpětná dialogového okna.

Ve stejném souboru s názvem `ToDoActivity.java`, napište následující.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

Nyní byste měli mít `ToDoActivity.java` soubor, který kompilaci. V tomto okamžiku by také sestavit celý projekt.

## <a name="run-the-sample-app"></a>Spusťte aplikaci ukázka

Nakonec vytvářet a Androidem Studio nebo zatmění spouštění aplikace. Registraci nebo přihlášení aplikace. Vytváření úkolů pro přihlášený uživatel. Odhlásit a znovu se přihlaste jako jiný uživatel a vytvořte úkoly pro daného uživatele.

Všimněte si, že úkoly jsou uložené uživatelská na rozhraní API, protože rozhraní API extrahuje identita uživatele z přístupový token, které obdrží.

Pro informaci dokončeným ukázkovým [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip). Ho můžete zkopírovat z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Důležité informace


### <a name="encryption"></a>Šifrování

ADAL šifruje tokeny a úložiště přihlašovacích údajů v `SharedPreferences` ve výchozím nastavení. Můžete si může samostatně prohlížet `StorageHelper` třídy zobrazovat si podrobnosti. Android zavádí **AndroidKeyStore 4.3(API18)** zabezpečeného úložiště privátních klíčů. ADAL použije pro API18 a vyšší. Pokud chcete použít ADAL pro nižší SDK verze, je třeba zadat tajné klíč na `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Soubory cookie relací v zobrazení webové stránky

Zobrazení Android webového vymažte soubory cookie relací po zavření aplikace. Je to zpracovat následujícím kódem vzorku.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[Další informace o soubory cookie](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).
