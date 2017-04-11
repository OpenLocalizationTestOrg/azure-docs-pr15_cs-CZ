<properties 
    pageTitle="Chyba při zjišťování ověřování" 
    description="Průvodce připojení služby active directory zjistil typu kompatibilní ověřování" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>Chyba při zjišťování ověřování

Při zjišťování předchozí ověřovací kód, Průvodce zjistil typu kompatibilní ověřování.   

##<a name="what-is-being-checked"></a>Co je kontroly?

**Poznámka:** Zjišťovat správně předchozí ověřovací kód v projektu, musí být vytvořené projektu.  Pokud došlo k této chybě a nemáte předchozí ověřovací kód v projektu, znovu vytvořit a zkuste to znovu.

###<a name="project-types"></a>Vytvoření vlastního typu projektu

Průvodce zkontroluje, jaký typ projektu vyvíjíte tak, aby ho vloží správné ověřování logiky do projektu.  Pokud je zařízení, která je odvozena z `ApiController` v projektu projektu se považovat za WebAPI projektu.  Pokud jsou pouze řadiče, které jsou odvozeny od `MVC.Controller` v projektu projektu se považovat za MVC projektu.  Cokoli jiného Průvodce nepodporuje.  Projekty webových formulářích aktuálně nepodporuje.

###<a name="compatible-authentication-code"></a>Kompatibilní ověřovací kód

Průvodce taky hledá nastavení ověřování, která byla dříve konfigurované pomocí průvodce nebo jsou kompatibilní s průvodcem.  Pokud existují všechna nastavení, nebude považována za reentrantního případu a průvodce otevřít a zobrazit nastavení.  Pokud pouze existují některá nastavení, nebude považována za případu chyby.

V projektu MVC Průvodce kontroluje některou z následujících nastavení, které vyplynou ze předchozí použití průvodce:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Kromě toho Průvodce kontroluje následující nastavení v rozhraní API webových projektu, které vyplynou ze předchozí použití průvodce:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Nekompatibilní ověřovací kód

Nakonec pokusí se průvodce zjistit, které jste nakonfigurovali s předchozími verzemi aplikace Visual Studio verze ověřovací kód. Pokud jste obdrželi tuto chybu, znamená to, že váš projekt obsahuje typ kompatibilní ověřování. Průvodce rozpozná následující typy ověřování z předchozích verzí aplikace Visual Studio:

* Ověřování systému Windows 
* Jednotlivých uživatelských účtů 
* Účty organizace 
 

Zjistit ověřování systému Windows v projektu MVC průvodce vyhledá `authentication` prvek ze souboru **web.config** .

<pre>
    &lt;konfigurace&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;režim ověřování = "Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/Configuration&gt;
</pre>

Zjistit ověřování systému Windows v projektu rozhraní API webových průvodce vyhledá `IISExpressWindowsAuthentication` element ze svého projektu **.csproj** souboru:

<pre>
    &lt;Projekt&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;povolené&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/projektu        &gt;
</pre>

Zjišťování ověřování jednotlivých uživatelských účtů, průvodce vyhledá element balíčku ze souboru **Packages.config** .

<pre>
    &lt;balíčky&gt;
        <span style="background-color: yellow">&lt;balíček id="Microsoft.AspNet.Identity.EntityFramework" verze = "2.1.0" targetFramework = "net45" /&gt;</span>
    &lt;/balíčky&gt;
</pre>

Zjišťování formuláře starý účet organizace ověřování, průvodce vyhledá následující element z **web.config**:

<pre>
    &lt;konfigurace&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;přidání klíče = "ida: sféry" hodnota = "***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/Configuration&gt;
</pre>

Pokud chcete změnit typ ověřování, odeberte typ kompatibilní ověřování a znovu spusťte průvodce.

Další informace najdete v tématu [Ověření scénáře Azure AD](active-directory-authentication-scenarios.md).