<properties
    pageTitle="Aplikace Androidu verze 2.0 Azure Active Directory | Microsoft Azure"
    description="Postup vytvoření aplikace pro Android, který se přihlašuje uživatelé, kteří mají osobní účet Microsoft a pracovní nebo školní účty i volání rozhraní API grafu pomocí knihoven třetích stran."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Přidání přihlášení do aplikace Android knihovně třetích stran pomocí rozhraní API grafu pomocí koncový bod verze 2.0

Platformu Microsoft identity používá otevřít standardy například OAuth2 a OpenID připojení. Vývojáři lze pomocí libovolné knihovny požadovaným integrovat s služeb společnosti Microsoft. Aby vývojáři naší platform pomocí jiných knihoven, jsme jste napsali několik návody zpráva a ukazuje, jak nakonfigurovat knihoven jiných výrobců pro připojení k platformu Microsoft identity. Většina knihoven, které implementace [specifikace RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) můžete připojit k platformu Microsoft identity.

S aplikací, která vytvoří tento návod uživatelé Přihlaste se k jejich organizace a vyhledejte sami ve své organizaci pomocí rozhraní API grafu.

Pokud jste ještě moc nedělali a OAuth2 OpenID připojení hodně této ukázkové konfigurace nemusí můžete uspořádat. Doporučujeme číst [2.0 protokoly - OAuth 2.0 se tak mohli ověřovat kód obrázku](active-directory-v2-protocols-oauth-code.md) pozadí.

> [AZURE.NOTE] Některé funkce naše platformy, které mají výraz standardy OAuth2 nebo OpenID připojit třeba podmíněného přístupu a Správa zásad Intune vyžadují, abyste použít naše otevřít zdroj Microsoft Azure Identity knihoven.

Koncový bod verze 2.0 nepodporuje všechny scénáře Azure Active Directory a funkce.

> [AZURE.NOTE] Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).


## <a name="download-the-code-from-github"></a>Stáhněte si tento kód z GitHub
Kód pro účely tohoto návodu spravován [na GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  Postup, můžete [Stáhnout osnovu v aplikaci jako ZIP](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) nebo klonovat kostra:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Můžete taky jen stáhnout vzorku a rovnou začít:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace na [portál registrace aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nebo postupujte podle podrobných pokynů v tématu [jak si zaregistrovat aplikace s koncovým verze 2.0](active-directory-v2-app-registration.md).  Zkontrolujte, že:

- Zkopírujte **Id aplikace** přiřazená aplikace vzhledem k tomu, že ho musíte brzy bude k dispozici.
- Přidání platformu **mobilní** aplikace.

> Poznámka: Na portálu registrace aplikace obsahuje hodnotu **Přesměrovat URI** . V tomto příkladu musí však použít výchozí hodnoty `https://login.microsoftonline.com/common/oauth2/nativeclient`.


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>Stáhnout knihovnu NXOAuth2 třetích stran a vytvoření pracovního prostoru aplikace Groove

V tomto návodu použijete OIDCAndroidLib z GitHub, což je OAuth2 knihovně založené na připojení OpenID kód Google. Implementuje nativní aplikaci profilu a podporuje koncový bod se tak mohli ověřovat uživatele. Toto jsou všechny položky, které budete muset integrace s platformu Microsoft identity.

Klonovat repo OIDCAndroidLib s vaším počítačem.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Nastavení prostředí Android Studio

1. Vytvoření nového projektu se systémem Android Studio a přijměte výchozí hodnoty v průvodci.

    ![Vytvoření nového projektu v Android Studio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Cílové zařízení s Androidem](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![Přidat aktivitu na mobilní telefon](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. Pokud chcete nastavit modulech projektu, přesunutí klonovaný repo umístění projektu. Můžete taky vytvořit projekt a potom klonovat přímo do umístění projektu.

    ![Moduly projektu](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Otevřete nastavení moduly projektu pomocí místní nabídky nebo klávesové zkratky Ctrl + Alt + avní + S.

    ![Nastavení moduly projektu](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. Výchozí aplikace modul odeberte, protože chcete nastavení kontejneru projektu.

    ![Výchozí aplikace modul](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. Import moduly z klonovaný repo projektu stávajícím.

    ![Import projektu gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![vytvořit novou stránku modul](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Opakujte tento postup pro `oidlib-sample` module.

7. Kontrola oidclib závislostí `oidlib-sample` modul.

    ![oidclib závislostí na modulu oidlib ukázka](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Klikněte na tlačítko **OK** a počkejte gradle synchronizace.

    Vaše settings.gradle by měl vypadat:

    ![Snímek obrazovky s settings.gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Vytvořit aplikaci vzorku a ujistěte se, že ukázku správné.

    Nebudete moct používat to Azure Active Directory ještě. Budeme muset nejdřív nakonfigurovat některé koncové body. Toto je zajistit že nemáte problémy s Androidem Studio před začneme přizpůsobení aplikaci vzorku.

10. Vytvoření a spuštění `oidlib-sample` jako cíl Android Studio.

    ![Průběh oidlib vzorku sestavení](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Odstranění `app ` adresář, který nechaný při odebrání modulu z projektu, protože Android Studio nebudou odstraněna bezpečnost.

    ![Struktura souboru, který obsahuje adresář aplikací](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Otevření nabídky **Úpravy konfigurace** odebrat běhu konfiguraci nechaný i po odebrání modulu z projektu

    ![Konfigurace nabídka Upravit](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![spuštění nástroje Konfigurace aplikace](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>Konfigurace koncové body vzorku

Teď, když máte `oidlib-sample` úspěšným, úprava některé koncové body získat tento práci s Azure Active Directory.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Nakonfigurujte klienta úpravou oidc_clientconf.xml souboru

1. Vzhledem k tomu, že používáte OAuth2 jsou přenášena pouze pro získání token a volání rozhraní API grafu, nastavte klienta dělat OAuth2 pouze. OIDC chodily na pozdější příklad.

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. Konfigurace svoje ID klienta, který jste dostali od portálu registrace.

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. Nakonfigurujte přesměrování URI té dole.

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. Konfigurace obory, které potřebujete k přístup k rozhraní API grafu.

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

`User.Read` Hodnota argumentu `oidc_scopes` umožňuje čte základní profilu přihlášený uživatel.
Další informace o všechny obory k dispozici v [aplikaci Microsoft Graph oprávnění obory](https://graph.microsoft.io/docs/authorization/permission_scopes).

Pokud budete chtít vysvětlení o `openid` nebo `offline_access` jako obory v OpenID připojení, najdete v článku [2.0 protokoly - OAuth 2.0 se tak mohli ověřovat kód tok](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>Konfigurace koncové body klienta úpravou oidc_endpoints.xml souboru

- Otevřít `oidc_endpoints.xml` soubor a proveďte následující změny:

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Tyto koncové body měli nikdy změnit, pokud používáte OAuth2 jako protokol.

> [AZURE.NOTE]
Koncové body `userInfoEndpoint` a `revocationEndpoint` aktuálně nepodporuje Azure Active Directory. Pokud necháte tyto s výchozí hodnotou example.com, budete upozorněni, že nejsou k dispozici ve výběru :-)


## <a name="configure-a-graph-api-call"></a>Konfigurace hovoru rozhraní API grafu

- Otevřít `HomeActivity.java` soubor a proveďte následující změny:

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Tady jednoduché grafu volání rozhraní API vrací naše informace.

Jsou všechny změny, které musíte udělat. Spustit `oidlib-sample` aplikace a klikněte na **přihlásit**.

Poté, co jste byly úspěšně ověřené vyberte tlačítko **Požádat o chráněné zdroje** na zkušební hovor k rozhraní API graf.

## <a name="get-security-updates-for-our-product"></a>Aktualizace zabezpečení pro naše produkt

Budeme rádi, když návštěva [Web TechCenter o zabezpečení](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru poradní výstrah zabezpečení dostávat oznámení o zabezpečení incidentem.
