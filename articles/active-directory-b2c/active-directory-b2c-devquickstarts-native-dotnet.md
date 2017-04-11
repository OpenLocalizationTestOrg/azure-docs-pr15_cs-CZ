<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Vytvoření desktopové aplikaci systému Windows, která obsahuje přihlášení, registrace, a Správa profilů pomocí Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Vytvoření desktopové aplikace Windows

Pomocí B2C Azure Active Directory (Azure AD) můžete přidat funkce správy identit výkonné samoobslužných funkcí vaší desktopové aplikaci krátké chvilku. Tento článek vám ukáže, jak při vytváření aplikace "seznam úkolů".NET Windows Presentation Foundation (WPF), který obsahuje registrace, přihlášení, Správa uživatelů a profilu. Aplikace se zahrnují podporu registrace přihlašování a pomocí uživatelské jméno nebo e-mailu. Podpora pro registrace přihlašování a bude zahrnovat také pomocí účty sociálních třeba Facebook a Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Získat Azure AD B2C adresář

Než budete moct použít Azure AD B2C, musíte vytvořit adresář nebo klient.  Adresář je kontejner pro všechny uživatele aplikace, skupiny a další. Pokud nemáte už, [vytvořte adresář B2C](active-directory-b2c-get-started.md) před pokračovat v této příručce.

## <a name="create-an-application"></a>Vytvoření aplikace

Dál je potřeba vytvořit aplikace ve vašem adresáři B2C. To vám Azure AD informace, které potřebuje bezpečně komunikovat s aplikací. Vytvoření aplikace pro, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md).  Nezapomeňte:

- Patří **native client –** v aplikaci.
- Zkopírujte **přesměrovat URI** `urn:ietf:wg:oauth:2.0:oob`. Je výchozí adresu URL Tato ukázka kódu.
- Zkopírujte **ID aplikace** přiřazená aplikace. Budete ho později potřebovat.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvoření pravidla

V Azure AD B2C každé uživatelské prostředí je definován [zásad](active-directory-b2c-reference-policies.md). Tento příklad kód obsahuje tři identity prostředí: registrace, přihlaste se a upravit profil. Je potřeba vytvořit zásady pro každý typ způsobem popsaným v [zásad referenční článek](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Při vytváření tři zásady nezapomeňte:

- Zvolte **Uživatelské ID registrace** nebo **e-mailu registrace** zásuvné poskytovatelů identit.
- Zvolte **zobrazovaného názvu** a jiných registrace atributy registrace zásad.
- Klikněte na **zobrazované jméno** a **ID objektu** deklarované jako deklarace aplikace pro každého zásadu. Můžete použít i jiné deklarované.
- Zkopírujte **název** každého zásadu po jeho vytvoření. By měla být předponu `b2c_1_`.  Musíte mít tyto názvy zásad později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po úspěšném vytvoření tři zásad, budete chtít vytvořit aplikace.

## <a name="download-the-code"></a>Stáhněte si tento kód

Kód pro tento kurz [spravován na GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). K vytvoření ukázky průběžně, můžete si [Stáhnout kostry projekt jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Můžete taky zkopírovat kostra:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Dokončené aplikace je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) nebo `complete` pobočka stejném úložišti.

Po stažení ukázkový kód otevřete soubor SLN Visual Studio můžete začít. `TaskClient` Projektu je WPF desktopové aplikace, která interakci uživatele s. Pro účely tohoto kurzu volá back-end úloh webového rozhraní API, hostované v Azure, obsahujícího seznam úkolů každého uživatele.  Není potřeba vytvářet webového rozhraní API, jsme už je spuštěný za vás.

Zjistěte, jak webového rozhraní API bezpečně ověří žádosti o pomocí Azure AD B2C, najdete v tématech [webového rozhraní API Začínáme článek](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Spuštění zásady
Aplikace informuje uživatele o s Azure AD B2C odesláním zprávy o ověřování, které určují zásad, které má provést jako součást žádost HTTP. Pro .NET desktopových aplikací můžete pomocí funkce náhledu Microsoft Authentication Library (MSAL) posílání zpráv ověřování OAuth 2.0, spusťte zásady a získat tokeny volajících webového rozhraní API.

### <a name="install-msal"></a>Instalace MSAL
Přidání MSAL k `TaskClient` projektu pomocí konzole Visual Studio balíčku správce.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Zadejte podrobnosti o B2C
Otevřete soubor `Globals.cs` a nahradit každý z nemovitostí s hodnotou vlastním. Tento předmětu je použít v celém `TaskClient` odkaz běžně používaných hodnoty.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>Vytvoření PublicClientApplication
Primární třída MSAL je `PublicClientApplication`. Tato třída představuje aplikace v Azure AD B2C systému. Při vytváření aplikace initalizes instanci `PublicClientApplication` v `MainWindow.xaml.cs`. Lze použít v celém okně.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>Zahájení konverzace registrace toku
Když uživatel rozhodne znaménka nahoru, budete chtít zahájit registrace toku, který používá registrace zásadu, kterou jste vytvořili. Pomocí MSAL jen zavoláte `pca.AcquireTokenAsync(...)`. Parametry předáte `AcquireTokenAsync(...)` zjistit, jaký token se zobrazí, zásadách používaných v žádosti o ověřování a další.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }

            MessageBox.Show(message);
        }

        return;
    }
}
```

### <a name="initiate-a-sign-in-flow"></a>Zahájení konverzace toku přihlášení
Spusťte toku přihlašovací stejným způsobem jako zahájit registrace toku. Pokud se uživatel přihlásí, zkontrolujte stejné volání MSAL, tentokrát pomocí znaménko zásad:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Zahájení konverzace toku upravit profil
Znovu bude možné provést zásadu upravit profil stejným způsobem:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

Ve všech případech vrátí MSAL buď token v `AuthenticationResult` nebo výjimku. Pokaždé, když dostanete token z MSAL, můžete použít `AuthenticationResult.User` objekt, který chcete aktualizovat uživatelská data v seznamu aplikací, jako je uživatelské rozhraní. ADAL také ukládá token pro použití v dalších částech aplikace.


### <a name="check-for-tokens-on-app-start"></a>Vyhledat tokeny při spuštění aplikace
Můžete taky MSAL Pokud chcete mít přehled o přihlášení stavu uživatele.  V této aplikace chceme uživateli zůstat přihlášeni i po zavření aplikace a znovu otevřete.  Zpět do `OnInitialized` přepsat, použít na MSAL `AcquireTokenSilent` způsob, jak vyhledat cached tokeny:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
    return;
}
```

## <a name="call-the-task-api"></a>Volání úkolu rozhraní API
Teď použili jste MSAL k provedení zásady a získat tokeny.  Když budete chtít pomocí jednoho těchto tokenů zavolat úkolu rozhraní API, můžete znovu použít společnosti MSAL `AcquireTokenSilent` způsob, jak vyhledat cached tokeny:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }
            MessageBox.Show(message);
        }

        return;
    }
    ...
```

Při volání `AcquireTokenSilentAsync(...)` úspěšné a token nachází v mezipaměti, můžete si přidat token `Authorization` záhlaví žádost HTTP. Úkol webového rozhraní API použije toto záhlaví ověření žádost zobrazíte seznam úkolů uživatele:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Odhlášení uživatele
Nakonec můžete MSAL ukončení relace s aplikací po klepnutí na tlačítko **Odhlásit se**.  Pokud chcete použít MSAL, to provést zrušením zaškrtnutí všech tokenů z mezipaměti tokenu:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Spusťte aplikaci ukázka

Nakonec vytvoření a spuštění vzorku.  Zaregistrujte si aplikaci pomocí e-mailovou adresu nebo uživatelské jméno. Odhlaste se a přihlaste se zpět stejným uživatelským. Upravte profil uživatele. Odhlaste se a přihlaste pomocí jiného uživatele.

## <a name="add-social-idps"></a>Přidání sociální IDPs

V současné době aplikace podporuje pouze pro přihlášení uživatele a přihlašovací využívající **místní účty**. Toto jsou uložené v adresáři B2C účty, které používají uživatelské jméno a heslo. Pomocí Azure AD B2C můžete přidat podporu pro ostatní zprostředkovatelé identit jiní (IDPs) beze změny jednotlivých kódu.

Chcete aplikaci přidat sociální IDPs, začněte podrobných pokynů v těchto článcích. Pro každý IDP, který chcete podporu budete muset registraci aplikace v režimu a získat ID klienta.

- [Nastavení služby Facebook jako IDP](active-directory-b2c-setup-fb-app.md)
- [Nastavení Google jako IDP](active-directory-b2c-setup-goog-app.md)
- [Nastavení Amazon jako IDP](active-directory-b2c-setup-amzn-app.md)
- [Nastavení LinkedIn jako IDP](active-directory-b2c-setup-li-app.md)

Po přidání Zprostředkovatelé identit jiní do adresáře B2C potřebujete upravit každou tři zásad zahrnout nové IDPs způsobem popsaným v [zásad referenční článek](active-directory-b2c-reference-policies.md). Po uložení vaše zásady znova spusťte aplikaci. Měla by se zobrazit nová IDPs přidá ve formě přihlašovací a dojde k registrace možnosti v žádném z vaší identity.

Můžete experimentovat se zásadami a sledujte účincích na aplikaci vzorku. Přidání nebo odebrání IDPs, pracovat s deklarace aplikace nebo změnit atributy registrace. Vyzkoušejte až najdete v článku jak shromáždit zásad, požadavky ověřování a MSAL.

Pro informaci dokončeným ukázkovým [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Ho můžete zkopírovat z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
