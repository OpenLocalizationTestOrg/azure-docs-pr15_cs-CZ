<properties
    pageTitle="Azure AD .NET Začínáme | Microsoft Azure"
    description="Jak vytvořit aplikaci .NET počítač s Windows, který lze integrovat s Azure AD pro přihlášení a volání Azure AD zamknutí rozhraní API pomocí OAuth."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Rozšiřte aplikace plochy WPF Windows Azure AD

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Pokud vytváříte desktopové aplikace, Azure AD umožňuje rychle a jednoduše k ověření uživatelů s jejich účty služby Active Directory.  Umožňuje taky aplikaci bezpečnému používání všechny webového rozhraní API chráněny Azure AD, například rozhraní API Office 365 nebo rozhraní API Azure.

Pro .NET nativní klienty, které se potřebují dostat k chráněným prostředkům Azure AD poskytuje Active Directory Authentication Library nebo ADAL.  ADAL je jediný života slouží k Usnadněte aplikace získat přístup tokeny.  Která ukazuje, jak snadno, tady jsme budete vytvořit seznam úkolů WPF .NET aplikaci, která:

-   Získá přístup k tokeny pro volání rozhraní API Azure AD grafu pomocí [OAuth 2.0 ověřovací protokol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Hledá v adresáři pro uživatele s dané alias.
-   Symboly uživatelé mimo.

Vytvoření dokončení aplikace fungovat, musíte:

2. Zaregistrujte aplikace v Azure AD.
3. Instalace a konfigurace ADAL.
5. Pomocí ADAL tokenů z Azure AD.

Jak začít, [Stáhněte si aplikaci osnovu](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) nebo [Stáhnout dokončeným ukázkovým](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Taky musíte tenanta Azure AD, ve kterém můžete vytvářet uživatele a registrace aplikace.  Pokud ještě nemáte klienta, [Přečtěte si, jak si ho založit](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. aplikace DirectorySearcher zaregistrovat*
Povolit aplikaci získat tokenů, Nejdřív musíte zaregistrovat v Azure AD klienta a udělit oprávnění pro přístup k rozhraní API Azure AD grafu:

-   Přihlaste se k portálu Správa Azure
-   V levém navigačním podokně klikněte na **Služby Active Directory**
-   Vyberte klienta, ve kterém si zaregistrovat aplikace.
-   Klikněte na kartu **aplikace** a klikněte na **Přidat** do dolní okraj.
-   Postupujte podle pokynů a vytvořte novou **Nativní klientské aplikaci**.
    -   **Název** aplikace bude popisovat aplikace pro koncové uživatele
    -   **Přesměrovat Uri** tvoří kombinace schéma a řetězec využívající Azure AD vrátíte token odpovědi.  Zadejte hodnotu v určité aplikaci, například `http://DirectorySearcher`.
-   Po dokončení zápisu AAD přiřadí aplikace klienta jedinečný identifikátor.  Musíte mít tuto hodnotu v dalších částech tak zkopírujte ho na kartě **Konfigurovat** .
- Taky na kartě **Konfigurovat** vyhledejte oddíl "Oprávnění do jiných aplikací".  Aplikace "Azure Active Directory" přidáte oprávnění k **adresáři přístup vaše organizace** ve skupinovém rámečku **Delegovat oprávnění**.  To vám umožní aplikace k vytvoření dotazu grafu rozhraní API pro uživatele.

## <a name="2-install--configure-adal"></a>*2. instalace a konfigurace ADAL*
Teď, když máte aplikace v Azure AD, můžete nainstalovat ADAL a zadejte kód týkající se identity.  Aby ADAL mohli komunikovat s Azure AD budete muset poskytnout některé informace o aplikaci registrace.
-   Začněte tím, že přidáte ADAL DirectorySearcher projektu pomocí konzole balíčku správce.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   V projectu DirectorySearcher otevřete `app.config`.  Nahradit hodnoty prvků v `<appSettings>` oddíl tak, aby odrážely hodnoty vkládáte do portálu Azure.  Pokaždé, když se použije ADAL kódu odkazovat tyto hodnoty.
    -   `ida:Tenant` Je domain Azure AD klienta, například contoso.onmicrosoft.com
    -   `ida:ClientId` Je clientId aplikace, kterou jste zkopírovali z portálu.
    -   `ida:RedirectUri` Je přesměrování adresy url registrovaná na portálu.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. pomocí ADAL tokenů z AAD*
Základní zásada za ADAL je, že vždy, když aplikace je přístupový token, jednoduše volá `authContext.AcquireTokenAsync(...)`, a ADAL ostatních.  

-   V `DirectorySearcher` otevřeného projekt `MainWindow.xaml.cs` a vyhledejte `MainWindow()` metody.  Cílem prvního kroku je inicializace vaše aplikace `AuthenticationContext` – ADAL uživatele primární předmětu.  Toto je místo, kam předáte ADAL souřadnice potřebuje komunikovat s Azure AD a určit, jak uložit do mezipaměti tokeny.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Teď vyhledejte `Search(...)` metodu, která bude vyvolat, když uživatel cliks "Hledat" tlačítek v uživatelském rozhraní aplikace na.  Tento způsob umožňuje požadavek GET rozhraní API Azure AD grafu do dotazu pro uživatele, jehož UPN začíná dané hledaný termín.  Ale k dotazu rozhraní API grafu, potřebujete zahrnout access_token v `Authorization` záhlaví žádosti – to přichází ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Pokud aplikace žádosti token tak, že zavoláte `AcquireTokenAsync(...)`, ADAL pokusí vrátit token bez výzvou k zadání přihlašovacích údajů.  Pokud ADAL zjistí, že uživatel vyžaduje se přihlásit k získání token, bude zobrazení přihlašovací dialogové okno, shromáždit přihlašovací údaje uživatele a vrátí token po úspěšném ověření.  Pokud ADAL nejde vrátit token z nějakého důvodu, vyvolá `AdalException`.
- Všimněte si, že `AuthenticationResult` objekt obsahuje `UserInfo` objekt, který můžete použít shromažďování informací aplikace může být nutné.  V DirectorySearcher `UserInfo` slouží k přizpůsobení uživatelského rozhraní aplikace na pomocí id uživatele.

- Po kliknutí na tlačítko "Odhlásit", chceme zajistit, aby dalším voláním `AcquireTokenAsync(...)` bude dotázání uživatele k přihlášení.  S knihovnou ADAL je stejně snadná jako vymazáním mezipaměti tokenů takto:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Ale pokud uživatel neklikat na tlačítko "Odhlásit", bude chcete spravovat relace uživatele pro při příštím spuštění DirectorySearcher.  Při spuštění aplikace můžete zkontrolovat společnosti ADAL tokenu mezipaměti pro existující token a aktualizovat uživatelské rozhraní příslušným způsobem.  V `CheckForCachedToken()` metody, jiné hovor `AcquireTokenAsync(...)`, předávání v současné době `PromptBehavior.Never` parametr.  `PromptBehavior.Never`ADAL se dozvíte, že uživatel neměli výzva k přihlášení a ADAL by měl místo toho výjimku nemůže vrátit token.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Blahopřejeme! Nyní máte pracovní .NET WPF aplikace, která umožňuje ověřování uživatelů, bezpečně volat rozhraní API webových pomocí OAuth 2.0 a získat základní informace o uživateli.  Pokud jste to ještě neudělali, teď je čas na naplnění vašeho klienta s někteří uživatelé.  Spusťte aplikaci DirectorySearcher a přihlášení pomocí jedné z těchto uživatelů.  Hledání podle jejich Přípony ostatním uživatelům.  Ukončete aplikaci a znovu ho spusťte.  Všimněte si, jak zůstane nedotčený relace uživatele.  Odhlásit a znovu se přihlaste jako jiným uživatelem.

ADAL usnadňuje všechny tyto běžné funkce identity začlenit do aplikace.  Zabezpečuje dirty práce za vás – Správa mezipaměti, podpora protokolu OAuth prezentace uživatel s přihlášením uživatelského rozhraní, aktualizace vypršela platnost tokeny a další.  Opravdu potřebujete stačí jedno volání rozhraní API `authContext.AcquireTokenAsync(...)`.

Pro informaci je k dispozici dokončeným ukázkovým (bez konfigurace hodnot) [v tomto poli](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Teď přejdete k další scénáře.  Můžete zkusit:

[Zabezpečení .NET webového rozhraní API s Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
