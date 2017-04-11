<properties
    pageTitle="Přihlášení přechodu klíče Azure AD | Microsoft Azure"
    description="Tento článek popisuje podpisový přechodu klíče doporučené postupy pro službu Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>Podepsání přechodu klíče v Azure Active Directory

Toto téma popisuje, co je potřeba vědět o veřejné klávesy, pomocí kterých se používají v Azure Active Directory (Azure AD) Pokud se chcete odhlásit tokenů zabezpečení. Je důležité mít na paměti, že tyto klíče při přechodu myší v pravidelných intervalech a v případě nebezpečí může být chránění okamžitě. Všechny aplikace, které používají Azure AD by měla programově zpracovat proces přechodu klíče nebo vytvoření obrázku pravidelných ruční při přechodu myší. Pokračovat ve čtení vysvětlit, jak fungují klávesy posoudit dopad přechodu do aplikace a k aktualizaci aplikace nebo vytvořit pravidelných ruční při přechodu myší proces pro zpracování přechodu klíče v případě potřeby.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Základní informace o přihlášení Azure AD klíče

Azure AD pomocí veřejným klíčem kryptografický Integrované oborové standardy vytvořit vztah důvěryhodnosti mezi sebe sama a aplikace, které používá. V praxi to funguje následujícím způsobem: Azure AD pomocí podpisový klíč, který se skládá z veřejných a privátních klíčů pár. Pokud se uživatel přihlásí do aplikace, která používá Azure AD pro ověřování, vytvoří Azure AD tokenu zabezpečení, který obsahuje informace o uživateli. Tento token podepsáno Azure AD pomocí privátním klíčem odesílá zpátky na aplikace. Ověřte, jestli je tokenu platnou a skutečně původu z Azure AD, třeba aplikace ověřit tokenu podpis pomocí veřejný klíč vystaveným Azure AD obsažené v klienta [OpenID připojení zjišťování](http://openid.net/specs/openid-connect-discovery-1_0.html) nebo SAML/WS-Fed [Dokument metadat federace](active-directory-federation-metadata.md).

Z bezpečnostních důvodů Azure AD podepisování klíčové cívky v pravidelných intervalech a v případě nebezpečí, může být chránění okamžitě. Libovolné aplikaci, která lze integrovat s Azure AD máme ještě počítat zpracování události přechodu klíče bez ohledu na to, jak často může dojít. Pokud ne, a aplikace pokusí klávesou ukončenou platností ověřit podpis na token, žádosti o přihlášení se nezdaří.

Vždy není k dispozici v dokumentu OpenID připojení zjišťování a dokument metadat federace víc než jednoho platné klíče. Aplikace by měl být připraven vyberte některou z zkratky uvedené v dokumentu, protože vždycky jen jednu klávesu může být vrácena brzy bude k dispozici, jiné může jeho náhrada atd.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Jak posuďte, pokud bude to mít vliv aplikace a co dělat

Jak aplikace zpracovává přechodu klíče závisí na proměnné třeba typ aplikace nebo byla použita k čemu identity protokol a knihovny. Níže posuďte, zda nejběžnější typy aplikací jsou ovlivněny přechodu klíče a obsahují pokyny k aktualizaci aplikace podporují automatické při přechodu myší nebo ručně aktualizovat klávesu.

* [Získání přístupu k prostředkům nativní klientské aplikace](#nativeclient)
* [Aplikace pro web a rozhraní API přístupu k prostředkům](#webclient)
* [Aplikace pro web / rozhraní API ochrana zdroje a vytvořené pomocí aplikace služby Azure](#appservices)
* [Aplikace pro web a rozhraní API ochrana zdrojů prostřednictvím .NET OWIN OpenID připojit, WS Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware](#owin)
* [Aplikace pro web a rozhraní API ochrana zdrojů prostřednictvím .NET základní OpenID připojit nebo JwtBearerAuthentication middleware](#owincore)
* [Aplikace pro web a rozhraní API ochrana zdrojů prostřednictvím modulu passport azure ad Node.js](#passport)
* [Aplikace pro web / rozhraní API ochrana zdroje a vytvořené pomocí aplikace Visual Studio 2015](#vs2015)
* [Webové aplikace ochrana zdroje a vytvořené ve Visual Studiu 2013](#vs2013)
* [Web API ochrana zdroje a vytvořené ve Visual Studiu 2013](#vs2013_webapi)
* [Webové aplikace ochrana zdroje a vytvořené pomocí aplikace Visual Studio 2012](#vs2012)
* [Webové aplikace ochrana zdroje a vytvořené pomocí aplikace Visual Studio 2010, 2008 o použití Windows Identity Foundation](#vs2010)
* [Aplikace pro web a rozhraní API ochrana zdrojů pomocí jiných knihoven nebo ručně provádění všech podporovaných protokolů](#other)

Zásady se **není** platná pro:

* Aplikace přidá z Azure AD Galerie aplikace (včetně vlastní) mají zvláštní pokyny s ohledem na podepisování klíče. [Další informace.](active-directory-sso-certs.md)
* Místní aplikací publikovaných pomocí aplikace proxy nemusíte bát podepisování klíče.

### <a name="nativeclient"></a>Získání přístupu k prostředkům nativní klientské aplikace

Aplikace, které jsou pouze přístupu k prostředkům (tj Aplikace Microsoft Graph KeyVault, rozhraní API aplikace Outlook a jiných Microsoft APIs) obecně pouze získat token a předejte podél vlastníka zdroje. Předpokladu, že budou nejsou chránit zdroje, není prozkoumat tokenu a nepotřebují ověřit, jestli že je správně přihlášení.

Nativní klientské aplikace plochy nebo mobilní, spadají do této kategorie a proto nejsou ovlivněny při přechodu myší.

### <a name="webclient"></a>Aplikace pro web a rozhraní API přístupu k prostředkům

Aplikace, které jsou pouze přístupu k prostředkům (tj Aplikace Microsoft Graph KeyVault, rozhraní API aplikace Outlook a jiných Microsoft APIs) obecně pouze získat token a předejte podél vlastníka zdroje. Předpokladu, že budou nejsou chránit zdroje, není prozkoumat tokenu a nepotřebují ověřit, jestli že je správně přihlášení.

Webových aplikací a rozhraní API, která používají toku jen aplikace (pověření klienta / certifikát klienta), spadají a proto nejsou ovlivněny při přechodu myší.

### <a name="appservices"></a>Aplikace pro web / rozhraní API ochrana zdroje a vytvořené pomocí aplikace služby Azure

Ověření Azure aplikace služeb a funkcí (EasyAuth) se tak mohli ověřovat už je nezbytné logiky pro zpracování přechodu klíče automaticky.

### <a name="owin"></a>Aplikace pro web a rozhraní API ochrana zdrojů prostřednictvím .NET OWIN OpenID připojit, WS Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware

Používáte-li aplikace je webová část .NET OWIN OpenID připojit, WS Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware, už má potřebné logiky pro automatické zpracování přechodu klíče.

Kde můžete potvrdit, že aplikace používá některý z těchto vyhledáním některou z následujících fragmenty Startup.cs nebo Startup.Auth.cs aplikace

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Aplikace pro web a rozhraní API ochrana zdrojů prostřednictvím .NET základní OpenID připojit nebo JwtBearerAuthentication middleware

Pokud aplikace používá middleware .NET základní OWIN OpenID připojit nebo JwtBearerAuthentication, už má potřebné logiky pro automatické zpracování přechodu klíče.

Kde můžete potvrdit, že aplikace používá některý z těchto vyhledáním některou z následujících fragmenty Startup.cs nebo Startup.Auth.cs aplikace

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="passport"></a>Aplikace pro web a rozhraní API ochrana zdrojů prostřednictvím modulu passport azure ad Node.js

Pokud aplikace používá modulu passport ad Node.js, už má potřebné logiky pro automatické zpracování přechodu klíče.

Můžete potvrdit, že vaše aplikace passport ad vyhledáním následující úryvek app.js aplikace

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Aplikace pro web / rozhraní API ochrana zdroje a vytvořené pomocí aplikace Visual Studio 2015

Pokud aplikace tvořily původní pomocí šablony webové aplikace Visual Studio 2015 a z nabídky **Změnit ověřování** vybraná **Práce a školních účtů** , už má potřebné logiky pro automatické zpracování přechodu klíče. Tento použití logických operátorů, vložené do middlewarových OWIN OpenID připojení načte ukládá kláves sady zjišťování dokumentů OpenID připojení a pravidelně aktualizuje.

Pokud jste přidali ověřovací do vašeho řešení ručně, nemusí mít vaše aplikace logiku potřebné přechodu klíče. Potřebujete napsat sami nebo postupujte podle pokynů v tématu [webových aplikací / rozhraní API pomocí jiných knihoven nebo ručně provádění všech podporovaných protokolů.](#other).

### <a name="vs2013"></a>Webové aplikace ochrana zdroje a vytvořené ve Visual Studiu 2013

Pokud aplikace tvořily původní pomocí šablony webové aplikace Visual Studio 2013 a z nabídky **Změnit ověřování** vybraná **Účty organizace** , už má potřebné logiky pro automatické zpracování přechodu klíče. Tato logika ukládá jedinečný identifikátor vaší organizace a podpisový klíčové informace ve dvou tabulkách databáze přidružené k projektu. Připojovací řetězec pro databázi můžete najít v projektu nastavení(Web.config)).

Pokud jste přidali ověřovací do vašeho řešení ručně, nemusí mít vaše aplikace logiku potřebné přechodu klíče. Potřebujete napsat sami nebo postupujte podle pokynů v tématu [webových aplikací / rozhraní API pomocí jiných knihoven nebo ručně provádění všech podporovaných protokolů.](#other).

Následující postup vám pomůže ověřit, jestli logickou správně funguje v aplikaci.

1. Ve Visual Studiu 2013 otevřete řešení a potom klikněte na kartu **Průzkumník serveru** v pravém podokně okna.
2. Rozbalte **datových připojení**, **DefaultConnection**a **tabulky**. Vyhledejte tabulku **IssuingAuthorityKeys** , pravým tlačítkem myši a potom klikněte na **Zobrazit Data v tabulce**.
3. V tabulce **IssuingAuthorityKeys** budou alespoň jeden řádek, který odpovídá hodnota Miniatura klíče. Odstraňte všechny řádky v tabulce.
4. Klikněte pravým tlačítkem tabulky **Klienti** a potom klikněte na **Zobrazit Data v tabulce**.
5. V tabulce **Klienti** budou alespoň jeden řádek, který odpovídá identifikátor klienta vlastní adresář. Odstraňte všechny řádky v tabulce. Pokud nechcete odstranit řádky v tabulce **klientů** a **IssuingAuthorityKeys** tabulky, bude za běhu dojde k chybě.
6. Vytvoření a spuštění aplikace. Jakmile se přihlásili k vašemu účtu, můžete tuto aplikaci ukončit.
7. Vraťte se do **Průzkumníka serveru** a podívejte se na hodnoty v tabulce **IssuingAuthorityKeys** a **klientů** . Zjistíte, že budou mít byla automaticky naplnit příslušné informace z dokumentu federace metadata.

### <a name="vs2013"></a>Web API ochrana zdroje a vytvořené ve Visual Studiu 2013

Pokud jste vytvořili webového rozhraní API aplikace ve Visual Studiu 2013 pomocí rozhraní API webových šablony a z nabídky **Změnit ověřování** vybraná **Účty organizace** , máte nezbytná logiku v aplikaci.

Pokud ručně nakonfigurovat ověřování, postupujte podle pokynů níže se dozvíte, jak nakonfigurovat vaše rozhraní API webových k automatické aktualizaci důležité informace.

Následující fragment kódu ukazuje, jak získat nejnovější klíče z dokumentu metadat federace a potom pomocí [JWT tokenu rutiny](https://msdn.microsoft.com/library/dn205065.aspx) pro ověřování tokenu. Fragment kódu se předpokládá, že použijete vlastní mechanismus mezipaměti pro zachování klávesu k ověření budoucí tokenů z Azure AD ať to v databázi, konfiguračního souboru nebo kdekoli jinde.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Webové aplikace ochrana zdroje a vytvořené pomocí aplikace Visual Studio 2012

Pokud byl integrované aplikace Visual Studio 2012, pravděpodobně použili identit a nástroje pro přístup ke konfiguraci aplikace. Je též pravděpodobné, že používáte [Ověřování Vystavitel název registru (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). VINR je zodpovědný za údržbu klávesy používaný k ověření tokeny vydán je a informace o důvěryhodní poskytovatelé identity (Azure AD). VINR také usnadňuje automaticky aktualizovat klíčové informace uložené v souboru Web.config stažením nejnovější dokument metadat federace přidružený k adresáři vašeho kontrolu, pokud konfigurace je synchronizovaný s nejnovější dokument a aktualizace aplikace se pomocí klávesy nové podle potřeby.

Pokud jste aplikaci vytvořili pomocí ukázky nebo si přečtěte následující dokumentaci návodu poskytované společností Microsoft, logiku přechodu klíče už součástí projektu. Zjistíte, že následující kód již v projektu. Pokud aplikaci ještě nemá toto použití logických operátorů, postupujte podle pokynů přidejte a ověřte, že funguje zařízení správně.

1. V **Okně Průzkumník**přidáte odkaz na sestavení **System.IdentityModel** pro příslušný projekt.
2. Otevřete soubor **Global.asax.cs** a přidejte následující pomocí direktivy na:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. Přidejte do souboru **Global.asax.cs** následujícím způsobem:
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. Jak je znázorněno vyvolejte metodu **RefreshValidationSettings()** v metodě **Application_Start()** v **Global.asax.cs** :
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Po absolvování takto Web.config aplikace aktualizují na nejnovější informace z dokumentu metadat federace, včetně nejnovější klíčů. Tuto aktualizaci dojde pokaždé, když fondu aplikací recykluje ve službě IIS; ve výchozím nastavení služby IIS nastavenou odpadkového aplikací každých 29 hodin.

Postupujte podle pokynů a ověřte, zda je funkční logiky přechodu klíče.

1. Po ověření, že aplikace používá kódu výše otevřete **nastavení(Web.config))** a přejděte **<issuerNameRegistry>** blok konkrétně hledáte následující pár řádků:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. V **<add thumbprint=””>** nastavení, změňte hodnotu Miniatura nahrazením libovolnému znaku jiný název. Uložte soubor **Web.config** .

3. Vytvoření aplikace a pak ho spusťte. Pokud dokončení procesu přihlášení aplikace úspěšně aktualizuje klávesu stažením požadované informace z vašeho adresáře federace metadat dokumentu. Pokud máte potíže s přihlášením, zajištění změny v aplikaci správný článek [Přidání přihlašování k vaší webové aplikace pomocí Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) nebo stažením a podrobná následujícím příkladu: [Více klienta cloudu žádosti o služby Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).


### <a name="vs2010"></a>Webové aplikace ochrana zdroje a vytvořené pomocí aplikace Visual Studio 2008 nebo Outlooku 2010 a Identity Windows Foundation (WIF) 1.0 pro .NET 3.5

Pokud integrované aplikace na verze 1.0 WIF neexistuje žádný ujednaných mechanismus automaticky aktualizovat konfigurace aplikace použít nový klíč.

- *Nejjednodušším způsobem* Pomocí nástrojů FedUtil součástí WIF SDK, která můžete získat nejnovější dokument metadat a aktualizovat konfiguraci.
- Aktualizace aplikace na .NET 4.5, která zahrnuje nejnovější verzi WIF nachází v oboru systému. Provádět automatické aktualizace konfigurace aplikace můžete [Ověřování Vystavitel název registru (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) .
- Proveďte ruční přechodu podle pokynů na konci tohoto dokumentu pokyny.

Pokyny k aktualizaci konfiguraci použít FedUtil:

1. Zkontrolujte, jestli máte verze 1.0 WIF SDK nainstalované v počítači vývoj pro Visual Studio 2008 nebo Outlooku 2010. Pokud jste to ještě nenainstalovali můžete [Stáhnout tady](https://www.microsoft.com/en-us/download/details.aspx?id=4451) .
2. Ve Visual Studiu otevřete řešení, klikněte pravým tlačítkem příslušných projektu a vyberte **Aktualizovat federace metadata**. Pokud tato možnost není k dispozici, FedUtil a/nebo verze 1.0 WIF SDK nainstalována.
3. Z příkazového řádku vyberte **Aktualizovat** zahájíte aktualizace federace metadat. Pokud máte přístup k serveru prostředí, kde je hostovaný aplikací, můžete volitelně FedUtil jeho [Automatické aktualizace Plánovač metadata](https://msdn.microsoft.com/library/ee517272.aspx).
4. Klikněte na **Dokončit** dokončete proces aktualizace.

### <a name="other"></a>Aplikace pro web a rozhraní API ochrana zdrojů pomocí jiných knihoven nebo ručně provádění všech podporovaných protokolů

Pokud používáte jinou knihovnu nebo ručně implementovaná všech podporovaných protokolů, musíte zkontroluje, knihovnu nebo vaší implementací zajistit, že klávesu je načtena z zjišťování dokumentu OpenID připojit nebo dokument federace metadat. Jedním ze způsobů vyhledat to je hledáním svůj kód POSTNET nebo kód knihovny pro volání se dokument zjišťování OpenID nebo dokument federace metadat.

Případně jejich klíčové ukládána někde pevně kódovaná v aplikaci můžete ručně načítejte klíče a aktualizovat ho příslušným způsobem tak, že provedení ruční přechodu podle pokynů na konci tohoto dokumentu pokyny. **Je důrazně doporučujeme vylepšení aplikace podporuje automatické přechodu** pomocí žádného z přístupů osnovy v tomto článku, aby se zabránilo budoucí narušení a zatížení, zvyšuje Azure AD-li se při přechodu myší cadence nebo má nouzových přechodu mimo pásma.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Jak otestovat aplikace a zjistit, pokud ho bude to mít vliv na

Můžete ověřit, zda aplikace podporuje automatické přechodu klíče instituce skripty a postupujte podle pokynů v [tohoto úložiště GitHub.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Jak provést ruční při přechodu myší, pokud jste aplikaci nepodporuje automatické při přechodu myší

Pokud je vaše aplikace **není** podpora automatické při přechodu myší, musíte vytvořit proces, který pravidelně sleduje Azure AD podpisový klíče a příslušným způsobem provádí ruční při přechodu myší. [Tento GitHub úložiště](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) obsahuje skripty a pokyny, jak se to dělá.
