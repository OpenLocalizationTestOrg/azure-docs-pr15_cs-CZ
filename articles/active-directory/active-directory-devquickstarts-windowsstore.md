<properties
    pageTitle="Azure AD pro Windows Store Začínáme | Microsoft Azure"
    description="Vytvoření aplikace pro Windows Store, která lze integrovat s Azure AD pro přihlášení a volá Azure AD zamknutí rozhraní API pomocí OAuth."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Integrace Azure AD pomocí aplikace pro Windows Store

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Pokud vytváříte aplikace pro Windows Store, Azure AD umožňuje rychle a jednoduše k ověření uživatelů s jejich účty služby Active Directory.  Umožňuje taky aplikaci bezpečnému používání všechny webového rozhraní API chráněny Azure AD, například rozhraní API Office 365 nebo rozhraní API Azure.

Azure AD pro Windows Store aplikace klasické pracovní plochy, které se potřebují dostat k chráněné prostředky, obsahuje Active Directory Authentication Library nebo ADAL.  ADAL na výhradní v životnost slouží k Usnadněte aplikace získat tokeny přístupu.  Která ukazuje, jak snadno, tady jsme budete vytvářet "Adresáře osobou hledající" Windows Store aplikace, která:

-   Získá přístup k tokeny pro volání rozhraní API grafu Azure AD pomocí [ověřování protokolem OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Hledá v adresáři pro uživatele s dané UPN.
-   Symboly uživatelé mimo.

Vytvoření dokončení aplikace fungovat, musíte:

2. Zaregistrujte aplikace v Azure AD.
3. Instalace a konfigurace ADAL.
5. Pomocí ADAL tokenů z Azure AD.

Jak začít, [můžete si stáhnout kostry projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) nebo [Stáhnout dokončeným ukázkovým](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Každý je řešení Visual Studio 2015.  Taky musíte tenanta Azure AD, ve kterém můžete vytvářet uživatele a registrace aplikace.  Pokud ještě nemáte klienta, [Přečtěte si, jak si ho založit](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>*1. zaregistrovat aplikaci osobou hledající adresáře*
Povolit aplikaci získat tokenů, Nejdřív musíte zaregistrovat v Azure AD klienta a udělit oprávnění pro přístup k rozhraní API Azure AD grafu:

-   Přihlaste se k [portálu Správa Azure](https://manage.windowsazure.com)
-   V levém navigačním podokně klikněte na **Služby Active Directory**
-   Vyberte klienta, ve kterém si zaregistrovat aplikace.
-   Klikněte na kartu **aplikace** a klikněte na **Přidat** do dolní okraj.
-   Postupujte podle pokynů a vytvořte novou **Nativní klientské aplikaci**.
    -   **Název** aplikace bude popisovat aplikace pro koncové uživatele
    -   **Přesměrovat Uri** tvoří kombinace schéma a řetězec využívající Azure AD vrátíte tokenu odpovědi.  Zadejte hodnotu zástupný symbol pro tuto, například `http://DirectorySearcher`.  Tato hodnota jsme budete nahradit později.
-   Po dokončení zápisu AAD přiřadí aplikace klienta jedinečný identifikátor.  Musíte mít tuto hodnotu v dalších částech tak zkopírujte ho na kartě **Konfigurovat** .
- Taky na kartě **Konfigurovat** vyhledejte oddíl "Oprávnění do jiných aplikací".  Aplikace "Azure Active Directory" přidáte oprávnění k **přístupu k adresáři jako uživatel přihlášený** ve skupinovém rámečku **Delegovat oprávnění**.  To vám umožní aplikace k vytvoření dotazu grafu rozhraní API pro uživatele.

## <a name="2-install--configure-adal"></a>*2. instalace a konfigurace ADAL*
Teď, když máte aplikace v Azure AD, můžete nainstalovat ADAL a zadejte kód týkající se identity.  Aby ADAL mohli komunikovat s Azure AD budete muset poskytnout některé informace o aplikaci registrace.
-   Začněte tím, že přidáte ADAL DirectorySearcher projektu pomocí konzole balíčku správce.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   V projectu DirectorySearcher otevřete `MainPage.xaml.cs`.  Nahrazení hodnot v `Config Values` oblast, aby odrážely hodnoty vkládáte do portálu Azure.  Pokaždé, když se použije ADAL kódu odkazovat tyto hodnoty.
    -   `tenant` Je domain Azure AD klienta, například contoso.onmicrosoft.com
    -   `clientId` Je clientId aplikace, kterou jste zkopírovali z portálu.
-   Potřebujete zjistit uri zpětné aplikace pro Windows Store.  Nastavení zarážky na tento řádek v `MainPage` metodu:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Vytvoření řešení, jak zajistit všechny odkazy balíčku obnoveny.  Chybí balíčků, otevřete si balíčku správce Nuget a obnovení balíčků.
- Spusťte aplikaci a zkopírujte vyhraďte hodnotu `redirectUri` Při zarážce přístupů.  By měl vypadat nějak

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Zpět na kartě **Konfigurace** aplikace v portálu pro správu Azure nahraďte hodnotu **RedirectUri** s touto hodnotou.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. pomocí ADAL tokenů z AAD*
Základní zásada za ADAL je, že vždy, když aplikace je přístupový token, jednoduše volá `authContext.AcquireToken(…)`, a ADAL ostatních.  

-   Cílem prvního kroku je inicializace vaše aplikace `AuthenticationContext` -ADAL uživatele primární předmětu.  Toto je místo, kam předáte ADAL souřadnice potřebuje komunikovat s Azure AD a určit, jak uložit do mezipaměti tokeny.

```C#
public MainPage()
{
    ...

    authContext = new AuthenticationContext(authority);
}
```

- Teď vyhledejte `Search(...)` metodu, která bude uplatnit po kliknutí na tlačítko "Hledat" v uživatelském rozhraní aplikace.  Tento způsob umožňuje požadavek GET rozhraní API Azure AD grafu do dotazu pro uživatele, jehož UPN začíná dané hledaný termín.  Ale k dotazu rozhraní API grafu, potřebujete zahrnout access_token v `Authorization` záhlaví žádosti – to přichází ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Pokud aplikace žádosti token tak, že zavoláte `AcquireTokenAsync(...)`, ADAL pokusí vrátit token bez výzvou k zadání přihlašovacích údajů.  Pokud ADAL zjistí, že uživatel vyžaduje se přihlásit k získání token, bude zobrazení přihlašovací dialogové okno, shromáždit přihlašovací údaje uživatele a vrátí token po úspěšném ověření.  Pokud ADAL nejde vrátit token z nějakého důvodu `AuthenticationResult` stav bude chybu.

- Teď je čas na použít access_token jste získali.  Také v `Search(...)` metody, připojení token k získání rozhraní API graf na požadavek na záhlaví se tak mohli ověřovat:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- Můžete taky použít `AuthenticationResult` objekt, který chcete zobrazit informace o uživateli v aplikaci, jako jsou id uživatele:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Nakonec můžete ADAL podepsat uživatele mimo aplikaci stejně.  Po kliknutí na tlačítko "Odhlásit", chceme zajistit, aby dalším voláním `AcquireTokenAsync(...)` přihlášení se zobrazí v zobrazení.  S knihovnou ADAL je stejně snadná jako vymazáním mezipaměti tokenů takto:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Blahopřejeme! Nyní máte pracovní pro Windows Store, která umožňuje ověřování uživatelů, bezpečně volat rozhraní API webových pomocí OAuth 2.0 a získat základní informace o uživateli.  Pokud jste to ještě neudělali, teď je čas na naplnění vašeho klienta s někteří uživatelé.  Spusťte aplikaci DirectorySearcher a přihlášení pomocí jedné z těchto uživatelů.  Hledání podle jejich Přípony ostatním uživatelům.  Ukončete aplikaci a znovu ho spusťte.  Všimněte si, jak relaci, zůstane nedotčený.  Odhlásit (tak, že kliknete pravým tlačítkem myši zobrazíte dolním panelu) a znovu se přihlaste jako jiným uživatelem.

ADAL usnadňuje všechny tyto běžné funkce identity začlenit do aplikace.  Zabezpečuje dirty práce za vás – Správa mezipaměti, podpora protokolu OAuth prezentace uživatel s přihlášením uživatelského rozhraní, aktualizace vypršela platnost tokeny a další.  Opravdu potřebujete stačí jedno volání rozhraní API `authContext.AcquireToken*(…)`.

Pro informaci je k dispozici dokončeným ukázkovým (bez konfigurace hodnot) [v tomto poli](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Teď můžete přesunout na další identity scénářích.  Můžete zkusit:

[Zabezpečení .NET webového rozhraní API s Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
