<properties 
    pageTitle="Integrace místních identit s Azure Active Directory."
    description="Toto je Azure AD Connect, který popisuje, co je a proč byste použili."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Vytváření Vícefaktorové ověřování do vlastních aplikací (SDK)

> [AZURE.IMPORTANT]  Pokud chcete, můžete si stáhnout SDK, bude potřeba vytvořit poskytovatele Azure Multi-Factor Auth i když máte Azure MFA, AAD Premium nebo EMS licence.  Když vytvoříte poskytovatele Azure Multi-Factor Auth k tomuto účelu a už máte licencí, je nutné vytvořil poskytovatel s modelem **Za uživatele** a odkaz na poskytovatele na adresář, který obsahuje Azure MFA, Azure AD Premium nebo EMS licencí.  Zajistíte, že nejsou fakturované Pokud máte víc jedinečných uživatelů pomocí SDK než počet licencí, které vlastníte.

Azure Multi-Factor Authentication Software Development Kit (SDK) umožňuje vytvářet telefonní hovor a text zprávy ověření přímo do procesu přihlášení nebo transakce aplikací ve vašem klientovi Azure AD.

SDK Multi-Factor Authentication je dostupný u C# Visual Basic (.NET), Java, Perl, PHP a skutečné. V SDK poskytuje tenkou obálkou okolo vícefaktorové ověřování. Všechno, co potřebujete napsat kódu, včetně souborů skrytými v komentářích zdrojového kódu, příklad souborů a podrobné souboru ReadMe zahrnuje. Každý SDK obsahuje taky certifikát a soukromý klíč pro šifrování transakce, které jsou jedinečné pro poskytovatele Multi-Factor Authentication. Když máte zprostředkovatele, si můžete stáhnout SDK v tolik formáty a jazyky podle potřeby.

Struktura rozhraní API v SDK Multi-Factor Authentication je velmi jednoduché. Uděláte jednu funkci volat rozhraní API s parametry Multi-Factor možnosti, třeba režimu ověřování a uživatelská data, třeba telefonní číslo nebo číslo PIN kódu ověřuje. Rozhraní API přeložit volání funkce do webové služby žádosti ke cloudové Azure Multi-Factor Authentication službě. Všechna volání musí obsahovat odkaz na soukromé certifikát, který je součástí každého SDK.

Protože rozhraní API nemají přístup uživatelům registraci v Azure Active Directory, je nutné zadat informace o uživatelích, jako jsou telefonní čísla a PIN kódy v souboru nebo v databázi. Rozhraní API navíc nenabízejí funkce správy pro zápis nebo uživateli, proto je potřeba k vytvoření těchto procesů do aplikace.






## <a name="download-the-azure-multi-factor-authentication-sdk"></a>Stáhněte si Azure Vícefaktorové ověřování SDK

Stažení SDK Multi-Factor Azure vyžaduje [Azure Multi-Factor Auth poskytovatele](multi-factor-authentication-get-started-auth-provider.md).  Při této akci musí úplné Azure předplatné, i když vlastní Azure MFA, Azure AD Premium nebo Enterprise mobilita sadu licencí.  Pro stažení SDK, přejděte na portál správy Multi-Factor pomocí správy poskytovatele Auth Multi-Factor přímo nebo po kliknutí na odkaz **"Přejděte na portál"** na stránce nastavení služby MFA.


### <a name="to-download-the-azure-multi-factor-authentication-sdk-from-the-azure-portal"></a>Pokud si chcete stáhnout SDK Azure Multi-Factor Authentication z portálu Microsoft Azure


1. Přihlaste se k portálu Azure jako správce.
2. V levé části vyberte služby Active Directory.
3. Na stránce služby Active Directory nahoře klikněte na **Multi-Factor Auth poskytovatelů**
4. V dolní části klikněte na **Spravovat**
5. Tím se otevře novou stránku.  V levé dolní klikněte na SDK.
<center>![Ke stažení](./media/multi-factor-authentication-sdk/download.png)</center>
6. Vyberte požadovaný jazyk a klikněte na jednu odkazů přidružené ke stažení.
7. Uložte soubor.



### <a name="to-download-the-azure-multi-factor-authentication-sdk-via-the-service-settings"></a>Pokud si chcete stáhnout SDK Azure Multi-Factor Authentication prostřednictvím nastavení služeb


1. Přihlaste se k portálu Azure jako správce.
2. V levé části vyberte služby Active Directory.
3. Dvojitým kliknutím na instanci aplikace Azure AD.
4. Nahoře klikněte na **Konfigurovat**
5. V části vícefaktorové ověřování vyberte **Nastavení služby Správa**
![stáhnout](./media/multi-factor-authentication-sdk/download2.png)
6. Na stránce nastavení služby v dolní části obrazovky klikněte na **Přejít na portál**.
![Ke stažení](./media/multi-factor-authentication-sdk/download3a.png)
7. Tím se otevře novou stránku.  V levé dolní klikněte na SDK.
8. Vyberte požadovaný jazyk a klikněte na jednu odkazů přidružené ke stažení.
9. Uložte soubor.

## <a name="contents-of-the-azure-multi-factor-authentication-sdk"></a>Obsah Azure Vícefaktorové ověřování SDK
V SDK najdete následující položky:

- **Soubor Readme pro**. Vysvětluje, jak používat rozhraní API Multi-Factor Authentication do nového nebo existujícího aplikací.
- **Zdrojový soubor** pro Vícefaktorové ověřování
- **Klientský certifikát** , který slouží ke komunikaci se službou Vícefaktorové ověřování
- **Soukromý klíč** pro certifikát
- **Zavolejte na výsledky.** Seznam kódů výsledek volání. Tento soubor otevřít pomocí aplikace formátování textu, např. Pomocí kódů výsledek volání otestujte a řešení potíží s provádění Vícefaktorové ověřování aplikace. Nejsou ověření stavů.
- **Příklady.** Ukázkový kód pro základní pracovní provádění Vícefaktorové ověřování.


>[AZURE.WARNING]Certifikát klienta je jedinečný soukromé certifikát, který byl vytvořen zejména za vás. Sdílení nebo ztratíte tento soubor. Je váš klíč zajistit bezpečnost vaší komunikace se službou Vícefaktorové ověřování.

## <a name="code-sample-standard-mode-phone-verification"></a>Ukázka kódu: Ověření telefonní standardním režimu

Tato ukázka kódu ukazuje, jak můžete pomocí rozhraní API v SDK Azure Multi-Factor Authentication standardním režimu hlasový hovor ověření přidat aplikaci. Standardním režimu je telefonní hovor, který uživatel odpoví stisknout klávesu #.

V tomto příkladu je C# .NET 2.0 Multi-Factor Authentication SDK v základní ASP.NET aplikaci C# serverovou použití logických operátorů, ale proces je velmi podobné pro jednoduché implementace v jiných jazycích. Jelikož SDK zdrojové soubory, ne spustitelný, můžete vytvářet soubory a odkazovat na ně nebo je zahrnout přímo v aplikaci.

>[AZURE.NOTE]Při provádění Vícefaktorové ověřování, použijte další faktory jako sekundární nebo třetího ověření pro doplnění primární ověřování. Má být použit jako primární metod nejsou určeny těchto postupů.

### <a name="code-sample-overview"></a>Přehled ukázka kódu
Ukázkový kód pro velmi jednoduché webovou aplikaci ukázku používá telefonní hovor s odpovědí klíčové # dokončete ověřování uživatele. Tento telefonní hovor faktor je v Vícefaktorové ověřování jmenoval standardním režimu.

Kód klienta neobsahuje žádné Multi-Factor Authentication konkrétní prvky. Vzhledem k tomu další ověření faktory nezávisle na primární ověřování, můžete je přidat beze změny existující rozhraní přihlašování. Rozhraní API v SDK Multi-Factor umožňují přizpůsobení uživatelského prostředí, ale není třeba provádět žádné změny vůbec.

Kód serverovou přidá standardní režim ověřování v kroku 2. Vytvoří objekt PfAuthParams s parametry, které jsou potřeba k ověření standardní režim: uživatelské jméno, telefonní číslo a režimu a cestu k certifikát klienta (CertFilePath), který je potřeba v každé volání. Ukázka všechny parametry v PfAuthParams najdete v článku ukázkový soubor v SDK.

Potom kód předává objekt PfAuthParams funkci pf_authenticate(). Vrácená hodnota označuje úspěšně nebo neúspěšně ověření. Výstupní parametry callStatus a errorID, obsahují informace výsledek další volání. Kódy výsledek volání jsou popsány v souboru výsledků volání v SDK.

Minimální implementaci můžete napsaný v několika řádků. V kódu výrobní by však obsahoval složitější zpracování chyb, kódu Další databáze a Rozšířené uživatelské prostředí.

### <a name="web-client-code"></a>Kód klienta Web

Následující obrázek je kód klienta web na stránce ukázka.


    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Kódem na straně serveru

Následující kód serverovou Vícefaktorové ověřování a je spustit v kroku 2. Standardní režim (MODE_STANDARD) je telefonní hovor, ke kterému uživatel odpovídá stisknutím klávesy #.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "9134884271";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = " Multi-Factor Authentication failed.";
                }
            }

        }
    }
