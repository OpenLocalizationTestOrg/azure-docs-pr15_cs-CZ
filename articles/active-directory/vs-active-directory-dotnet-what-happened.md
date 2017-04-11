<properties
    pageTitle="Co se stalo s projekt MVC (Visual Studio Azure Active Directory připojení služby) | Microsoft Azure "
    description="Popisuje, co se stane s projektu MVC při připojení k Azure AD pomocí aplikace Visual Studio připojené služby"
    services="active-directory"
    documentationCenter="na"
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

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Co se stalo s projekt MVC (Visual Studio Azure Active Directory připojení služby)?

> [AZURE.SELECTOR]
> - [Začínáme](vs-active-directory-dotnet-getting-started.md)
> - [Co se přihodilo](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Byly přidány odkazy

### <a name="nuget-package-references"></a>Odkazy na balíček NuGet

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.NET odkazy

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Byly přidány kód

### <a name="code-files-were-added-to-your-project"></a>Soubory s kódem se přidala do vašeho projektu

Spuštění třídu ověřování **App_Start/Startup.Auth.cs** byl přidán do projektu obsahující logika spuštění Azure AD ověřování. Navíc třída řadiče Controllers/AccountController.cs jsme přidali obsahující **SignIn()** a **SignOut()** metody. Nakonec částečný pohled **Views/Shared/_LoginPartial.cshtml** jsme přidali obsahující akci odkaz pro přihlášení/SignOut.

### <a name="startup-code-was-added-to-your-project"></a>Spuštění kódu byl přidán do projektu

Pokud už máte třída při spuštění v projektu, metody **Konfigurace** byl aktualizován zahrnout volání **ConfigureAuth(app)**. V opačném třída spuštění jsme přidali do projektu.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>App.config nebo web.config má nové hodnot konfigurace

Byly přidány následující položky konfigurace.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Vytvoření aplikace pro Azure Active Directory (AD)
Aplikace Azure AD byl vytvořený v adresáři, který jste vybrali v průvodci.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Pokud se zaškrtnuté, *zakázat ověřování jednotlivých uživatelských účtů*, jaké další změny provedené do projektu?
Odkazy na balíček NuGet odebraly a soubory byly odebrány a zálohovat. V závislosti na stavu projektu bude pravděpodobně nutné ručně odeberte odkazy na další nebo soubory nebo upravit kód podle potřeby.

### <a name="nuget-package-references-removed-for-those-present"></a>Odkazy na balíček NuGet odebírá (z vlastních)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Soubory s kódem zálohovat a odstraní (u těch, prezentovat)

Všechny následující soubory se zálohovala a odebrali projektu. Záložních souborů se nachází ve složce "Zálohování" v kořenovém adresáři projektu.

- **App_Start\IdentityConfig.cs**
- **Controllers\ManageController.cs**
- **Models\IdentityModels.cs**
- **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Soubory s kódem zálohovala (u těch, prezentovat)

Všechny následující soubory zálohování před je někdo jiný nahradil. Záložních souborů se nachází ve složce "Zálohování" v kořenovém adresáři projektu.

- **Startup.cs**
- **App_Start\Startup.auth.cs**
- **Controllers\AccountController.cs**
- **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Pokud mám zaškrtnuté *Číst adresáře data*, jaké další změny provedené do projektu?

Byly přidány další odkazy.

###<a name="additional-nuget-package-references"></a>Další NuGet balíčku odkazy

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>Odkazy na další .NET

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>Byly přidány další kód soubory do projektu

Dva soubory se přidala do podporují tokenu ukládání do mezipaměti: **Models\ADALTokenCache.cs** a **Models\ApplicationDbContext.cs**.  Další řadiče a zobrazení jsme přidali ke znázornění pomocí Azure grafu rozhraní API přístupu k informace profilů uživatelů.  Tyto soubory se **Controllers\UserProfileController.cs** **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>Další spouštěcí kód byl přidán do projektu

V souboru **startup.auth.cs** nový objekt **OpenIdConnectAuthenticationNotifications** jsme přidali **oznámení** členem **OpenIdConnectAuthenticationOptions**.  Toto je povolit příjem kód OAuth a exchange pro přístupový token.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Další změny provedené app.config nebo web.config

Byly přidány následující položky další konfiguraci.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Byly přidány následující části Konfigurace a připojovací řetězec.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Aktualizace aplikace Azure Active Directory
Aplikace Azure Active Directory aktualizovaném zahrnout oprávnění ke *čtení adresáře dat* a další klíč byl vytvořen, které se použije se jako *ida: ClientSecret* v **nastavení(Web.config))** .

[Další informace o Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
