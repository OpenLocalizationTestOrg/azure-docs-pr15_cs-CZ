<properties
    pageTitle="Azure AD verze 2.0 .NET Web Appu | Microsoft Azure"
    description="K vytváření do .NET MVC webových aplikací, které volání webové služby pomocí osobní účty Microsoft a pracovní nebo školní účty pro přihlásit."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Volání webového rozhraní API z web appu .NET

S koncovým verze 2.0 můžete rychle přidat ověřování k web apps a webového rozhraní API se podpora pro obě osobní účty Microsoft a pracovní nebo školní účty.  Tady abychom budete vytvářet webové aplikace MVC přihlásí uživatelů v pomocí připojení OpenID trochu pomoct z middleware OWIN společnosti Microsoft.  Web appu získat tokeny OAuth 2.0 přístupu pro webového rozhraní api zajištěná OAuth 2.0, která umožňuje vytvořit, číst a odstraňovat na daný uživatel "seznam úkolů".

Tento kurz se zaměřuje na používání MSAL a získat přístup tokeny ve web appu popsaná v celé [zde](active-directory-v2-flows.md#web-apps).  Jako požadavky, je vhodné nejdřív zjistěte, jak [Přidat základní Přihlaste se k do webových aplikací](active-directory-v2-devquickstarts-dotnet-web.md) nebo o způsobech [správně zabezpečeného webového rozhraní API](active-directory-v2-devquickstarts-dotnet-api.md).

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

## <a name="download-sample-code"></a>Stáhnout ukázkový kód

Kód pro účely tohoto návodu spravován [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  Postup, můžete [Stáhnout osnovu v aplikaci jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) nebo klonovat kostra:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Můžete taky můžete [Stáhnout aplikaci dokončené jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) nebo klonovat dokončené aplikace:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nebo postupujte podle těchto [podrobných pokynů](active-directory-v2-app-registration.md).  Zkontrolujte, že:

- Kopírovat dolů **Id aplikace** přiřazené k aplikaci, musíte ho brzy bude k dispozici.
- Vytvoření **Aplikace tajná** typu **heslo** a zkopírujte dolů jeho hodnotu na pozdější
- Přidání platformu **webové** aplikace.
- Zadejte správné **Přesměrovat URI**. Identifikátor uri přesměrování označuje Azure AD místo, kam mají být směrovány ověřování odpovědi – je jako výchozí pro tento kurz `https://localhost:44326/`.


## <a name="install-owin"></a>Instalace OWIN
OWIN middleware NuGet balíčky přidány `TodoList-WebApp` projektu pomocí konzole balíčku správce.  OWIN middleware se použijí k problému žádosti o přihlášení a odhlašovací, spravovat relace uživatele a získat informace o uživateli, mimo jiné.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Přihlaste se uživatele
Nakonfigurujte middleware OWIN pomocí [ověřování protokolem OpenID připojení](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  

-   Otevřít `web.config` soubor v kořenovém adresáři `TodoList-WebApp` projektu a zadejte hodnoty konfigurace vaše aplikace v `<appSettings>` oddíl.
    -   `ida:ClientId` Je **Id aplikace** přiřazená vaši aplikaci distribuovali portálu registrace.
    - `ida:ClientSecret` Je **Aplikace tajná** jste vytvořili v portálu registrace.
    -   `ida:RedirectUri` Je **Přesměrovat Uri** jste zadali v portálu.
- Otevřít `web.config` soubor v kořenovém adresáři `TodoList-Service` project a nahradit `ida:Audience` se stejným **Aplikace Id** jako nahoře.


- Otevřete soubor `App_Start\Startup.Auth.cs` a přidejte `using` příkazy pro knihovny z výše.
- Ve stejném souboru implementace `ConfigureAuth(...)` metody.  Parametry zadáte v `OpenIDConnectAuthenticationOptions` bude sloužit jako souřadnice aplikaci komunikovat s Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Získáte přístup tokeny pomocí MSAL
V `AuthorizationCodeReceived` oznámení, chceme použít [OAuth 2.0 ve spojení s OpenID připojení](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) pro uplatnění authorization_code přístupový token ke službě seznam úkolů.  MSAL může být tento proces snadno za vás:

- Nejdřív nainstalujte verzi preview MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- A přidat další `using` příkaz `App_Start\Startup.Auth.cs` soubor MSAL.
- Teď přidejte nová metoda `OnAuthorizationCodeReceived` obslužná rutina události.  Tento proces zpracování použije MSAL získat přístupový token k rozhraní API seznam úkolů a uloží tokenu do mezipaměti tokenů společnosti MSAL na pozdější:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- Ve webových aplikacích má MSAL extensible token mezipaměti, které mohou sloužit k ukládat tokeny.  Tento příklad implementuje `NaiveSessionCache` využívající http relace ukládání do mezipaměti tokeny.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Volání webového rozhraní API
Teď je čas na skutečně používat access_token, kterou jste získali v kroku 3.  Otevřete web appu `Controllers\TodoListController.cs` soubor, který je všechny žádosti o CRUD pro rozhraní API seznamu úkolů.

- Můžete MSAL znovu tady vzdáleně access_tokens z mezipaměti MSAL.  Nejdřív přidejte `using` údajů pro MSAL na tento soubor.

    `using Microsoft.Identity.Client;`

- V `Index` akce, použití MSAL `AcquireTokenSilentAsync` způsob, jak získat access_token, použitý k načtení dat z službu seznam úkolů:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- Vzorku přidá výsledný token žádost HTTP GET jako `Authorization` hlavičky, která seznam úkolů služba používá pro požadavek ověřit.
- Pokud vrátí službu seznam úkolů `401 Unauthorized` odpověď access_tokens v MSAL mít stanou neplatnými z nějakého důvodu.  V tomto případě by měl uvolněte všechny access_tokens z mezipaměti MSAL a se uživateli zobrazí zpráva, že možná bude potřeba se přihlásit znovu, které restartuje tok tokenu WIA.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Podobně MSAL nejde vrátit access_token z nějakého důvodu, by měl sdělte uživatelům se znovu přihlaste.  Toto je jednoduchá – stačí zachycení některou `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- Stejné přesná `AcquireTokenSilentAsync` hovor dosah – stačí implementd v `Create` a `Delete` akce.  Ve webových aplikacích můžete tento způsob MSAL získat access_tokens pokaždé, když potřebujete v aplikaci.  MSAL zpracuje získáním, ukládání do mezipaměti a aktualizace tokeny za vás.

Nakonec vytvoření a spuštění aplikace.  Přihlaste se pomocí Account Microsoft nebo účet Azure AD a Všimněte si, jak se projeví identita uživatele v horním navigačním panelu.  Přidávání a odstraňování některých položek ze seznamu úkolů uživatele zobrazíte OAuth 2.0 zabezpečené volání rozhraní API v akci.  Nyní máte v prohlížeči a rozhraní API webových obě zabezpečená pomocí standardních protokolů, které můžete ověřování uživatelů s jejich osobní a pracovní/školní účty.

Pro informaci dokončeným ukázkovým (bez konfigurace hodnoty) [není uvedený tady](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Další kroky

Další zdroje informací najdete v tématech:
- [Průvodce pro vývojáře verze 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Značka StackOverflow "azure active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Aktualizace zabezpečení pro naše produkty

Budeme rádi, když chcete dostávat oznámení ze kdy zabezpečení incidentem probíhala po návštěvě [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru poradní výstrah zabezpečení.
