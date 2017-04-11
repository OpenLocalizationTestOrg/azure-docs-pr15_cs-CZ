<properties
pageTitle="Azure Active Directory verze 2.0 .NET nativní aplikaci | Microsoft Azure"
description="Pracovní nebo školní účty a k vytváření .NET nativní aplikaci, která přihlásí uživatelů pomocí obou osobní Account Microsoft."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>Přidání přihlášení k aplikaci počítač s Windows

S koncový bod verze 2.0, můžete rychle přidat ověřování desktopových aplikací s podporou obou osobní účty Microsoft a pracovní nebo školní účty.  Umožňuje také aplikace bezpečně komunikovat s back-end webového rozhraní api, jakož i [Aplikace Microsoft Graph](https://graph.microsoft.io) a několik rozhraní [API Unified Office 365](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [AZURE.NOTE] Některé scénáře Azure Active Directory (AD) a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

Pro [.NET nativní aplikace, které lze spustit na zařízení](active-directory-v2-flows.md#mobile-and-native-apps)poskytuje Azure AD Microsoft identit Authentication Library nebo MSAL.  MSAL je jediný v životnost slouží k Usnadněte aplikace zobrazíte tokeny pro volání webové služby.  Která ukazuje, jak snadno, tady jsme budete vytvářet seznam úkolů WPF .NET aplikace, která:

- Uživatel přihlásí a získá přístup k tokenů [OAuth 2.0 ověřovací protokol](active-directory-v2-protocols.md#oauth2-authorization-code-flow).
- Bezpečné hovorů back-end webová služba seznam úkolů, které je taky zajištěná OAuth 2.0.
- Odhlásit se uživatel.

## <a name="download-sample-code"></a>Stáhnout ukázkový kód

Kód pro účely tohoto návodu spravován [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  Postup, můžete [Stáhnout v aplikaci kostry jako .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) nebo klonovat kostra:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

Dokončené aplikace je k dispozici na konci kurzu, stejně.

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nebo postupujte podle těchto [podrobných pokynů](active-directory-v2-app-registration.md).  Zkontrolujte, že:

- Kopírovat dolů **Id aplikace** přiřazené k aplikaci, musíte ho brzy bude k dispozici.
- Přidání platformu **mobilní** aplikace.

## <a name="install--configure-msal"></a>Instalace a konfigurace MSAL
Teď, když máte aplikaci registrovaná u společnosti Microsoft, můžete nainstalovat MSAL a zadejte kód týkající se identity.  Aby MSAL komunikovat koncový bod verze 2.0 budete muset poskytnout některé informace o aplikaci registrace.

-   Začněte tím, že přidáte MSAL TodoListClient projektu pomocí konzole balíčku správce.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   V projectu TodoListClient otevřete `app.config`.  Nahradit hodnoty prvků v `<appSettings>` oddíl tak, aby odrážely hodnoty vkládáte do portálu registrace aplikace.  Pokaždé, když se použije MSAL kódu odkazovat tyto hodnoty.
    -   `ida:ClientId` Je **Id aplikace** aplikaci, kterou jste zkopírovali z portálu.

- V seznamu úkolů služby projektu, otevřete `web.config` v kořenovém projektu.  
    - Nahrazení `ida:Audience` hodnota se stejným **Id aplikace** z portálu.

## <a name="use-msal-to-get-tokens"></a>Pomocí MSAL tokenů
Základní zásada za MSAL je vždy, když aplikace je přístupový token, jednoduše volat `app.AcquireToken(...)`, a MSAL ostatních.  

-   V `TodoListClient` otevřeného projekt `MainWindow.xaml.cs` a vyhledejte `OnInitialized(...)` metody.  Cílem prvního kroku je inicializace vaše aplikace `PublicClientApplication` -MSAL na primární třídy představující nativní aplikace.  Je to, kde úspěšném MSAL souřadnice nutnou komunikovat s Azure AD a určit, jak uložit do mezipaměti tokeny.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Po spuštění aplikace chceme, můžete zkontrolovat, pokud uživatel už přihlásili k aplikaci.  Však nechceme vyvolat uživatelské rozhraní přihlašovací zatím - vytočit uživatele klikněte na "Sign In" k tomu nevyzve.  Také v `OnInitialized(...)` metodu:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Pokud není přihlášení uživatele a klikněte na tlačítko "Přihlásit", chceme vyvolat přihlášení uživatelského rozhraní a máte uživatele zadejte své přihlašovací údaje.  Implementace rutině tlačítka přihlášení:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
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


}
```

- Pokud uživatel úspěšně znaménka se změnami, MSAL bude přijímat a mezipaměti token a můžete pokračovat v hovoru `GetTodoList()` metoda spolehlivé.  Vše, co zbývá získat úkoly uživatele je implementovat `GetTodoList()` metody.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Spuštění

Blahopřejeme! Nyní máte v aplikaci .NET WPF pracovní obsahující možnost ověřování uživatelů a bezpečné volat rozhraní API webových pomocí OAuth 2.0.  Spusťte oba projekty a přihlášení pomocí osobního účtu Microsoft nebo pracovního nebo školního účtu.  Přidání úkolů do seznamu úkolů tohoto uživatele.  Odhlásit a znovu přihlaste jako jiný uživatel k zobrazení seznamu úkolů.  Ukončete aplikaci a znovu ho spusťte.  Všimněte si, jak relaci, zůstane nedotčený, – to je protože aplikace ukládá tokeny v místním souboru.

MSAL usnadňuje začlenit běžné identit funkce do aplikace používat osobní a pracovní účty.  Zabezpečuje dirty práce za vás – Správa mezipaměti, podpora protokolu OAuth prezentace uživatel s přihlášením uživatelského rozhraní, aktualizace vypršela platnost tokeny a další.  Opravdu potřebujete stačí jedno volání rozhraní API `app.AcquireTokenAsync(...)`.

Pro informaci dokončeným ukázkovým (bez konfigurace hodnoty) [je k dispozici jako .zip tady](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), nebo můžete zkopírovat ho GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Další kroky

Teď můžete přesunout na další upřesňující témata.  Můžete zkusit:

- [Zabezpečení rozhraní API webových TodoListService s koncovým verze 2.0](active-directory-v2-devquickstarts-dotnet-api.md)

Další zdroje informací najdete v tématech:  

- [Průvodce pro vývojáře verze 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Značka "msal" StackOverflow >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Aktualizace zabezpečení pro naše produkty

Budeme rádi, když chcete dostávat oznámení ze kdy zabezpečení incidentem probíhala po návštěvě [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru poradní výstrah zabezpečení.
