<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Jak vytvořit webovou aplikaci, která volá webového rozhraní API pomocí Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure AD B2C: Volání webového rozhraní API z web appu .NET

Pomocí služby Azure Active Directory (Azure AD) B2C můžete přiřadit výkonné identity samoobslužné funkce správy webových aplikací web apps a webové rozhraní API v krátkém chvilku. Tento článek popisuje jak vytvořit web appu .NET modelu zobrazení řadiče MVC () "seznam úkolů", která volá webového rozhraní API pomocí nosný tokenů

Tento článek se nezabývá jak implementovat přihlášení, registrace a správy profilů s Azure AD B2C. Se zaměřuje na volání webového rozhraní API po už ověření uživatele. Pokud jste to ještě neudělali, by měl začínáte s [.NET webové aplikace Začínáme kurzu](active-directory-b2c-devquickstarts-web-dotnet.md) se dozvíte o základních funkcích Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Získat Azure AD B2C adresář

Než budete moct použít Azure AD B2C, musíte vytvořit adresář nebo klient.  Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další.  Pokud nemáte už, [vytvořte adresář B2C](active-directory-b2c-get-started.md) před pokračovat v této příručce.

## <a name="create-an-application"></a>Vytvoření aplikace

Dál je potřeba vytvořit aplikaci pro v adresáři vašeho B2C. To vám Azure AD informace, které potřebuje bezpečně komunikovat s aplikací. V tomto případě web app a rozhraní API webových zastupuje jednoho **ID aplikace**, protože představují jedné logické aplikaci. Vytvoření aplikace pro, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md). Nezapomeňte:

- Zahrňte **webové aplikace/webového rozhraní API** aplikace.
- Zadejte `https://localhost:44316/` jako **Adresa URL odpověď**. Je výchozí adresu URL Tato ukázka kódu.
- Zkopírujte **ID aplikace** přiřazená aplikace. Budete taky potřebovat to později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvoření pravidla

V Azure AD B2C každé uživatelské prostředí je definován [zásad](active-directory-b2c-reference-policies.md). Tento web appu obsahuje tři identity prostředí: registrace, přihlaste se a upravit profil. Je potřeba vytvořit jednu zásadu každého typu způsobem popsaným v [zásad referenční článek](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Při vytváření tři zásady nezapomeňte:

- Zvolte **zobrazované jméno** a dalších registrace atributů registrace zásad.
- V každé zásady zvolte deklarace aplikace **zobrazované jméno** a **ID objektu** . Můžete použít i jiné deklarované.
- Zkopírujte **název** každého zásadu po jeho vytvoření. By měla být předponu `b2c_1_`. Je potřeba názvy zásad později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření tři zásad, budete chtít vytvořit aplikace.

Všimněte si, že tento článek se nezabývá použití zásad, které jste právě vytvořili. Další informace o tom, jak fungují zásady v Azure AD B2C, začněte s [.NET web app kurz Začínáme](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Stáhněte si tento kód

Kód pro tento kurz [spravován na GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet). K vytvoření ukázky průběžně, můžete si [Stáhnout kostry projekt jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). Můžete taky zkopírovat kostra:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

Dokončené aplikace je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) nebo `complete` větev stejném úložišti.

Po stažení ukázkový kód otevřete soubor SLN Visual Studio můžete začít.

## <a name="configure-the-task-web-app"></a>Konfigurace úkolu web appu

Chcete-li získat `TaskWebApp` komunikovat s Azure AD B2C, budete muset zadat několik běžných parametry. V `TaskWebApp` otevřeného projekt `web.config` soubor v kořenovém projektu a nahrazení hodnot v `<appSettings>` oddíl. Můžete ponechat `AadInstance`, `RedirectUri`, a `TaskServiceUrl` hodnoty jsou.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Získat přístup tokeny a volání úkolu rozhraní API

Tato část popisuje používání token přijaté v průběhu přihlásili s Azure AD B2C pro přístup k webu rozhraní API, které je taky zabezpečená s Azure AD B2C.

Tento článek se nezabývá podrobnosti o tom, jak zabezpečovat rozhraní API. Zjistěte, jak webového rozhraní API bezpečně ověří žádosti o pomocí Azure AD B2C, najdete v tématech [webového rozhraní API Začínáme článek](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="save-the-sign-in-token"></a>Uložení znaménko tokenu

Nejdřív ověření uživatele (pomocí jedné z vaše zásady) a přijímat token přihlášení z Azure AD B2C.  Pokud si nejste jistí, jak provádět zásad, přejděte zpátky a zkuste [.NET web app kurz Začínáme](active-directory-b2c-devquickstarts-web-dotnet.md) se dozvíte o základních funkcích Azure AD B2C.

Otevřete soubor `App_Start\Startup.Auth.cs`.  Ke změně jedné důležité, musíte `OpenIdConnectAuthenticationOptions` – je nutné nastavit `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

return new OpenIdConnectAuthenticationOptions
{
    // For each policy, give OWIN the policy-specific metadata address, and
    // set the authentication type to the id of the policy
    MetadataAddress = String.Format(aadInstance, tenant, policy),
    AuthenticationType = policy,

    // These are standard OpenID Connect parameters, with values pulled from web.config
    ClientId = clientId,
    RedirectUri = redirectUri,
    PostLogoutRedirectUri = redirectUri,
    Notifications = new OpenIdConnectAuthenticationNotifications
    {
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Získání token v seznamu zařízení

`TasksController` Je zodpovědný za komunikaci s webového rozhraní API, posílat žádosti o HTTP rozhraní API číst, vytvářet a odstraňovat úkoly.  Becuase, které rozhraní API zajištěná Azure AD B2C, musíte nejprve získat token, které jste uložili z předchozího kroku.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

`BootstrapContext` Obsahuje přihlásit token, který jste získali provedením jedné z B2C zásad.

### <a name="read-tasks-from-the-web-api"></a>Číst z webového rozhraní API úkoly

Když budete mít token, můžete ho připojte k HTTP `GET` žádosti `Authorization` záhlaví bezpečně volání `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Vytváření a odstraňování úkolů na webu rozhraní API

Postupujte podle stejném vzorce při odeslání `POST` a `DELETE` požadavky na webového rozhraní API, pomocí `BootstrapContext` k načtení přihlásit token. Jsme implementovaná další jednoznačnou akci vytvořit. Můžete zkusit dokončení v akci odstranění `TasksController.cs`.

## <a name="run-the-sample-app"></a>Spusťte aplikaci ukázka

Nakonec vytvoření a spuštění aplikace. Registrace a přihlaste se a vytváření úkolů pro přihlášený uživatel. Odhlaste se a přihlaste se jako jiného uživatele. Vytváření úkolů pro daného uživatele. Všimněte si, jak úkoly jsou uložené uživatelská na rozhraní API, protože rozhraní API extrahuje identita uživatele z tokenu obdrží.

Pro informaci dokončeným ukázkovým [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). Ho můžete zkopírovat z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
