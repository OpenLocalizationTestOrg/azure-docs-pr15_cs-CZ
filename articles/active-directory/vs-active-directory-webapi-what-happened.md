<properties
    pageTitle="Co se stalo s WebApi projektu (Visual Studio Azure Active Directory připojení služby) | Microsoft Azure "
    description="Popisuje, co se stane s MVC projektu WebApi připojení k Azure AD pomocí aplikace Visual Studio"
  services="active-directory"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Co se stalo s WebApi projektu (Visual Studio Azure Active Directory připojení služby)

> [AZURE.SELECTOR]
> - [Začínáme](vs-active-directory-webapi-getting-started.md)
> - [Co se přihodilo](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Byly přidány odkazy

###<a name="nuget-package-references"></a>Odkazy na balíček NuGet

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>.NET odkazy

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Změny kódu

###<a name="code-files-were-added-to-your-project"></a>Soubory s kódem se přidala do vašeho projektu

Spuštění třídu ověřování **App_Start/Startup.Auth.cs** byl přidán do projektu obsahující logika spuštění Azure AD ověřování.

###<a name="startup-code-was-added-to-your-project"></a>Spuštění kódu byl přidán do projektu

Pokud už máte třída při spuštění v projektu, metody **Konfigurace** aktualizovaném zahrnout volání `ConfigureAuth(app)`. V opačném třída spuštění jsme přidali do projektu.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Soubor app.config nebo web.config má nové hodnot konfigurace.

Byly přidány následující položky konfigurace.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Vytvoření aplikace pro Azure AD

Aplikace Azure AD byl vytvořený v adresáři, který jste vybrali v průvodci.

[Další informace o Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Pokud se zaškrtnuté, *zakázat ověřování jednotlivých uživatelských účtů*, jaké další změny provedené do projektu?
Odkazy na balíček NuGet odebraly a soubory byly odebrány a zálohovat. V závislosti na stavu projektu bude pravděpodobně nutné ručně odeberte odkazy na další nebo soubory nebo upravit kód podle potřeby.

###<a name="nuget-package-references-removed-for-those-present"></a>Odkazy na balíček NuGet odebírá (z vlastních)

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>Soubory s kódem zálohovat a odstraní (u těch, prezentovat)

Všechny následující soubory se zálohovala a odebrali projektu. Záložních souborů se nachází ve složce "Zálohování" v kořenovém adresáři projektu.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Soubory s kódem zálohovala (u těch, prezentovat)

Všechny následující soubory zálohování před je někdo jiný nahradil. Záložních souborů se nachází ve složce "Zálohování" v kořenovém adresáři projektu.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Pokud mám zaškrtnuté *Číst adresáře data*, jaké další změny provedené do projektu?

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Další změny provedené app.config nebo web.config

Byly přidány následující položky další konfiguraci.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Aktualizace aplikace Azure Active Directory
Aplikace Azure Active Directory aktualizovaném zahrnout oprávnění ke *čtení adresáře dat* a další klíč byl vytvořen, které se použije se jako *Ida: heslo* v `web.config` soubor.

[Další informace o Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
