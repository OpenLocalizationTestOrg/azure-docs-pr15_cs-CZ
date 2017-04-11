<properties
    pageTitle="Jak se chránit back-end rozhraní API webových s Azure Active Directory a rozhraní API správy"
    description="Zjistěte, jak chránit back-end rozhraní API webových s Azure Active Directory a rozhraní API správy." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Jak se chránit back-end rozhraní API webových s Azure Active Directory a rozhraní API správy

Následující video ukazuje, jak vytvořit back-end rozhraní API webových a chránit pomocí protokolu OAuth 2.0 s Azure Active Directory a rozhraní API správy.  Tento článek obsahuje přehled a dalších informací o kroky v tomto videu. Toto video 24 minutu vám ukáže, jak chcete:

-   Vytvoření back-end rozhraní API webových a zabezpečit s AAD – od 1:30.
-   Rozhraní API naimportujte rozhraní API Správa - počínaje 7:10
-   Konfigurace portálu pro vývojáře pro volání rozhraní API - počínaje 9:09
-   Konfigurace desktopové aplikace pro volání rozhraní API - počínaje 18:08
-   Konfigurace zásad ověření JWT předem povolit požadavky – od 20:47

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Vytvoření Azure AD adresáře

Zajistit rozhraní API webových záložní služby Azure Active Directory Nejdřív musíte mít AAD klienta. V tomto videu se používá klienta, kterého s názvem **APIMDemo** . Pokud chcete vytvořit AAD klienta, přihlaste se k [Portálu klasické Azure](https://manage.windowsazure.com) a klikněte na **Nový**->**Aplikace služby**->**Služby Active Directory**->**Directory**->**Vytvořit vlastní**. 

![Azure Active Directory][api-management-create-aad-menu]

V tomto příkladu je vytvořen adresář s názvem **APIMDemo** s výchozí doménou s názvem **DemoAPIM.onmicrosoft.com**. Tento adresář je použít v celém video.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Vytvoření rozhraní API webových služeb zajištěná Azure Active Directory

V tomto kroku back-end rozhraní API webových vytvoří se na základě Visual Studio 2013. Tento krok videa začíná hodnotou 1:30. Vytvoření rozhraní API webových back-end projektu ve Visual Studiu, klikněte na **soubor**->**Nový**->**projektu**a zvolte **ASP.NET webovou aplikaci** ze seznamu **webové** šablony. V tomto videu se projekt pojmenovanou **APIMAADDemo**. Klikněte na **OK** vytvořte projekt. 

![Visual Studio][api-management-new-web-app]

Klikněte na **Rozhraní API webových** **Vyberte seznam šablony** vytvořit projekt rozhraní API webových. Konfigurace ověřování služby Directory Azure klikněte na **Změnit ověření**.

![Nový projekt][api-management-new-project]

Klikněte na **Účty organizace**a zadejte **doménu** AAD klienta. V tomto příkladu je doména **DemoAPIM.onmicrosoft.com**. Domény adresáře lze získat z karty **domény** z adresáře.

![Domény][api-management-aad-domains]

Konfigurace požadovaná nastavení v dialogovém okně **Změnit ověření** a klikněte na **OK**.

![Změna ověřování][api-management-change-authentication]

Po klepnutí na tlačítko **OK** Visual Studio pokusí registrovat aplikaci adresáři Azure AD a můžete být vyzváni se přihlásit pomocí aplikace Visual Studio. Přihlaste se pomocí účtu správce pro adresáře.

![Přihlaste se k aplikaci Visual Studio][api-management-sign-in-vidual-studio]

Konfigurovat tohoto projektu jako rozhraní API webových Azure zaškrtněte políčko hostitele **v cloudu** a potom klikněte na **OK**.

![Nový projekt][api-management-new-project-cloud]

Můžete být vyzváni k přihlášení k Azure a nakonfigurujte Web Appu.

![Konfigurace][api-management-configure-web-app]

V tomto příkladu je určen nový **plán služeb aplikací** s názvem **APIMAADDemo** .

Klikněte na tlačítko **OK** a nakonfigurujte je Web Appu vytvořit projekt.

## <a name="add-the-code-to-the-web-api-project"></a>Přidání kódu pro rozhraní API webových projektu

Dalším krokem v tomto videu přidá kód Web API projektu. Tento krok začíná hodnotou 4:35.

Rozhraní API webových v tomto příkladu používá služby základní Kalkulačka pomocí modelu a řadiči. Přidat modelu služby, klikněte pravým tlačítkem **modely** v **Okně Průzkumník** a zvolte **Přidat** **předmětu**. Pojmenujte třídu `CalcInput` a klikněte na **Přidat**.

Přidejte následující `using` údajů do horní části `CalcInput.cs` soubor.

    using Newtonsoft.Json;

 Nahraďte generované třídy následující kód.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

Klikněte pravým tlačítkem myši **řadiče** v **Okně Průzkumník řešení** a zvolte **Přidat**->**řadiče domény**. Zvolte **Webového rozhraní API 2 řadiče - prázdné** a klikněte na **Přidat**. Zadejte **CalcController** jeho název a klikněte na **Přidat**.

![Přidání řadiče domény][api-management-add-controller]

Přidejte následující `using` údajů do horní části `CalcController.cs` soubor.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

Nahraďte třídy generovaný řadiče následující kód. Tento kód implementuje `Add`, `Subtract`, `Multiply`, a `Divide` operace základní API kalkulačky.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Stisknutím klávesy **F6** k vytvoření a ověřte řešení.

## <a name="publish-the-project-to-azure"></a>Publikování projektu do Azure

V tomto kroku Visual Studio projekt publikovat na Azure. Tento krok videa začíná 5:45.

Publikování projektu do Azure, klikněte pravým tlačítkem na **APIMAADDemo** projekt ve Visual Studiu a vyberte **Publikovat**. Zachovejte výchozí nastavení v dialogovém okně **Publikovat Web** a klikněte na **Publikovat**.

![Web publikování][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Udělení oprávnění pro aplikaci služby Azure AD back-end

Nová žádost o službu back-end se vytvoří v adresáři Azure AD jako součást procesu konfigurace a publikování projektu rozhraní API webových. V tomto kroku video od 6:13 mít příslušná oprávnění k rozhraní API webových back-end.

![Aplikace][api-management-aad-backend-app]

Klikněte na název aplikace a konfigurace potřebná oprávnění. Přejděte na kartu **Konfigurovat** a posuňte se dolů do části **oprávnění na jiné aplikace** . Klikněte na **Oprávnění aplikace** rozevíracího seznamu vedle **Windows** **Azure Active Directory**, zaškrtněte políčko u **dat adresáře pro čtení**a klikněte na tlačítko **Uložit**.

![Přidat oprávnění][api-management-aad-add-permissions]

>[AZURE.NOTE] Pokud **Windows** **Azure Active Directory** není uvedený v části oprávnění pro ostatní aplikace, klikněte na **Přidat aplikaci** a přidejte ji ze seznamu.

Poznamenejte **Identifikátor URI aplikaci** pro použití v následujícím kroku při aplikace Azure AD nakonfigurovaný pro portálu pro vývojáře rozhraní API správy.

![Id aplikace URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Import webového rozhraní API na stránce pro správu rozhraní API

Rozhraní API se konfigurovat z portálu Publisheru rozhraní API, který je prostřednictvím portálu klasické Azure. Přístup na portál Publisheru, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě. Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [Spravovat první rozhraní API][] .

![Portál aplikace Publisher][api-management-management-console]

Operace může být [přidán do API ručně](api-management-howto-add-operations.md)nebo lze importovat. V tomto videu se operace importují ve formátu Swagger od 6:40.

Vytvoření souboru s názvem `calcapi.json` s tímto obsahem a uložte ho do počítače. Zkontrolujte, že je `host` atributu ukazatel rozhraní API webových backendovou. V tomto příkladu `"host": "apimaaddemo.azurewebsites.net"` se používá.

{"swagger": "2.0", "informace": {"název": "Kalkulačky", "Popis": "Aritmetický přes HTTP!", "verze": "1.0"}, "host": "apimaaddemo.azurewebsites.net", "basePath": "/ api", "schémata": ["http"], "cesty": {"/ Přidat? = {a} a b = {b}": {"získat": {"Popis": "Odpoví součet dvou čísel.", "operationId": "Přidat dvěma celá čísla", "parametrů": [{"název": "d", "v": "Odeslat dotaz na", "Popis": "První operand. Výchozí hodnota je <code>51</code>. ","požadované": PRAVDA,"Výchozí":"51","výčet": ["51"]}, {"název":"b","v":"Odeslat dotaz na","Popis":"Druhý operand. Výchozí hodnota je <code>49</code>. "" požadovaný ": PRAVDA,"Výchozí":"49","výčet": ["49"]}],"odpovědi": {}}}," / sub?a = {a} & b = {b} ": {"získat": {"Popis":"Odpoví rozdíl mezi dvěma čísly.","operationId":"Odečíst dvě celá čísla","parametrů": [{"název":"d","v":"Odeslat dotaz na","Popis":"První operand. Výchozí hodnota je <code>100</code>. ","požadované": PRAVDA,"Výchozí":"100","výčet": ["100"]}, {"název":"b","v":"Odeslat dotaz na","Popis":"Druhý operand. Výchozí hodnota je <code>50</code>. "" požadovaný ": PRAVDA,"Výchozí":"50","výčet": ["50"]}],"odpovědi": {}}}," / div?a = {a} & b = {b} ": {"získání": {"Popis":"Odpoví podíl dvou čísel.","operationId":"Děleno dvou celých čísel v nástroji","parametrů": [{"název":"d","v":"Odeslat dotaz na","Popis":"První operand. Výchozí hodnota je <code>100</code>. ","požadované": PRAVDA,"Výchozí":"100","výčet": ["100"]}, {"název":"b","v":"Odeslat dotaz na","Popis":"Druhý operand. Výchozí hodnota je <code>20</code>. "" požadovaný ": PRAVDA,"Výchozí":"20","výčet": ["20"]}],"odpovědi": {}}}," / mul?a = {a} & b = {b} ": {"získat": {"Popis":"Odpoví součin dvou čísel.","operationId":"Násobit dvě celá čísla","parametrů": [{"název":"d","v":"Odeslat dotaz na","Popis":"První operand. Výchozí hodnota je <code>20</code>. ","požadované": PRAVDA,"Výchozí":"20","výčet": ["20"]}, {"název":"b","v":"Odeslat dotaz na","Popis":"Druhý operand. Výchozí hodnota je <code>5</code>. ","požadované": PRAVDA,"Výchozí":"5","výčet": ["5"]}],"odpovědi": {}}}}}

K importu kalkulačky rozhraní API, v nabídce **Rozhraní API správy** na levé straně klikněte na **rozhraní API** a klikněte na **Import rozhraní API**.

![Tlačítko pro import rozhraní API][api-management-import-api]

Proveďte následující kroky pro nastavení kalkulačky rozhraní API.

1. Klikněte na **ze souboru**, přejděte `calculator.json` soubor uložený a klikněte na přepínač **Swagger** .
2. Do textového pole **přípona API webových adres URL** zadejte **Přepočítat** .
3. Klikněte do pole **produkty (nepovinné)** a zvolte **Starter**.
4. Klepnutím na tlačítko **Uložit** import rozhraní API.

![Přidání nového rozhraní API][api-management-import-new-api]

Po importu rozhraní API souhrnnou stránku pro rozhraní API se zobrazí na portálu aplikace publisher.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Volání rozhraní API neúspěšně z portálu pro vývojáře

V tomto okamžiku rozhraní API importoval na stránce pro správu rozhraní API, ale nelze ještě volat úspěšně z portálu pro vývojáře protože službu back-end se po zamknutí s Azure AD ověřování. Tento postup je znázorněn v tomto videu počínaje 7:40 pomocí následujících kroků.

Klikněte na **portál pro vývojáře** z pravé strany portálu aplikace publisher.

![Karta Vývojář v portálu][api-management-developer-portal-menu]

Klikněte na **rozhraní API** a klikněte na **kalkulačky** rozhraní API.

![Karta Vývojář v portálu][api-management-dev-portal-apis]

Klikněte na **vyzkoušet**.

![Vyzkoušejte si to][api-management-dev-portal-try-it]

Klepněte na tlačítko **Odeslat** a poznamenejte si stav odpovědi **401 Neautorizováno**.

![Odeslání][api-management-dev-portal-send-401]

Žádost nemá autorizaci, protože back-end rozhraní API aplikace se po zamknutí službou Azure Active Directory. Před úspěšně voláním rozhraní API vývojář portál musí být nakonfigurované pro povolte vývojáři používající OAuth 2.0. Tento postup je popsán v následujících částech.

## <a name="register-the-developer-portal-as-an-aad-application"></a>Registrace portálu pro vývojáře jako AAD aplikace

Cílem prvního kroku v konfiguraci portálu pro vývojáře povolit vývojáři používající OAuth 2.0 je zaregistrovat portálu pro vývojáře jako AAD aplikace. Tento postup je znázorněn počínaje 8:27 ve videu.

Přejděte na klienta Azure AD z první krok toto video, v tomto příkladu **APIMDemo** a přejděte na kartu **aplikace** .

![Nové aplikace][api-management-aad-new-application-devportal]

Klikněte na tlačítko **Přidat** k vytvoření nové aplikace služby Azure Active Directory a zvolte **Přidat aplikaci, které vyvíjí mé organizaci**.

![Nové aplikace][api-management-new-aad-application-menu]

Vyberte **Webová aplikace a/nebo rozhraní API webových**, zadejte název a klikněte na Další šipky. V tomto příkladě se používá **APIMDeveloperPortal** .

![Nové aplikace][api-management-aad-new-application-devportal-1]

Pro **přihlášení adresa URL** zadejte adresu URL služby správy API a připojte `/signin`. V tomto příkladě se používá **https://contoso5.portal.azure-api.net/signin **.

**Adresa URL Id aplikace** zadejte adresu URL služby správy API a připojit některé jedinečné znaky. V tomto příkladě se používá **https://contoso5.portal.azure-api.net/dp** tyto být všechny požadované znaky. Pokud jsou nakonfigurovány požadované **Vlastnosti aplikace** , zaškrtněte políčko Vytvořit aplikaci.

![Nové aplikace][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Konfigurace serveru se tak mohli ověřovat rozhraní API Management OAuth 2.0

Dalším krokem je konfigurace serveru se tak mohli ověřovat OAuth 2.0 správy rozhraní API. Tento krok je znázorněn v tomto videu počínaje 9:43.

V nabídce rozhraní API správy na levé straně klikněte na **zabezpečení** , klikněte **OAuth 2.0**a potom klikněte na **Přidat povolení** serveru.

![Přidání se tak mohli ověřovat serveru][api-management-add-authorization-server]

V polích **název** a **Popis** zadejte název a popis. Tato pole slouží k identifikaci serveru se tak mohli ověřovat OAuth 2.0 v instanci služby správy rozhraní API. V tomto příkladě se používá **se tak mohli ověřovat serveru ukázku** . Později při zadávání server OAuth 2.0 pro ověřování pro rozhraní API, vyberete tento název.

Pro **klienta registrace adresa URL** zadejte hodnotu zástupného symbolu `http://localhost`.  **Adresa URL stránky registrace klienta** bodů na stránku, která uživatelům slouží k vytváření a konfigurace svých účtů pro OAuth 2.0 poskytovatelům, kteří podporují Správa uživatelských účtů. V tomto příkladu uživatelů není vytvořit a nakonfigurovat svých účtů, aby je použit zástupný symbol.

![Přidání se tak mohli ověřovat serveru][api-management-add-authorization-server-1]

Pak zadejte **adresu URL koncový bod se tak mohli ověřovat** a **Token adresa URL koncového bodu**.

![Povolení serveru][api-management-add-authorization-server-1a]

Na stránce **Koncové body aplikaci** , kterou jste vytvořili pro portálu pro vývojáře aplikace AAD můžete načíst tyto hodnoty. Pro přístup k koncové body přejděte na kartu **Konfigurovat** aplikace AAD a klikněte na **zobrazení koncové body**.

![Aplikace][api-management-aad-devportal-application]

![Zobrazení koncové body][api-management-aad-view-endpoints]

**Koncový bod se tak mohli ověřovat OAuth 2.0** zkopírovat a vložit ho do textového pole **Adresa URL se tak mohli ověřovat koncového bodu** .

![Přidání se tak mohli ověřovat serveru][api-management-add-authorization-server-2]

**Koncový bod token OAuth 2.0** zkopírovat a vložit ho do textového pole **Adresa URL Token koncového bodu** .

![Přidání se tak mohli ověřovat serveru][api-management-add-authorization-server-2a]

Kromě vkládání v tokenu koncový bod, přidejte parametr další textu s názvem **zdroje** a pro hodnoty použití **Aplikace identifikátor URI** z aplikace AAD pro službu back-end, která byla vytvořená, pokud byl publikován projekt aplikace Visual Studio.

![Id aplikace URI][api-management-aad-sso-uri]

Potom zadejte přihlašovací údaje klienta. Jedná se o pověření pro zdroj, který chcete přístup, v tomto případě službu back-end.

![Pověření klienta][api-management-client-credentials]

Chcete-li získat **Kód klienta**, přejděte na kartu **Konfigurovat** AAD žádosti o službu back-end a zkopírujte **Kód klienta**.

Získání **Klienta tajná** kliknutím **vyberte dobu trvání** rozevírací seznam v části **klíče** a určit interval. V tomto příkladu je použit jeden rok.

![ID klienta][api-management-aad-client-id]

Klepněte na tlačítko **Uložit** uložte konfiguraci a zobrazení klíče. 

>[AZURE.IMPORTANT] Poznamenejte si tento klíč. Po zavření okna konfigurace Azure Active Directory nelze se znovu klávesu.

Zkopírujte klávesu do schránky, přepněte se zpátky do portálu Publisheru, klávesu vložte textové pole **Tajná klienta** a klikněte na tlačítko **Uložit**.

![Přidání se tak mohli ověřovat serveru][api-management-add-authorization-server-3]

Bezprostředně následující pověření klienta je grant kód ověření. Zkopírujte tento kód povolení a konfigurace přepnout zpátky do okna aplikace portál vývojář Azure AD stránky a vložit povolení udělit do pole **Adresa URL odpovědět** a znova klikněte na **Uložit** .

![Adresa URL odpověď][api-management-aad-reply-url]

Dalším krokem je pro nastavení oprávnění pro portálu pro vývojáře aplikace AAD. Klikněte na **Oprávnění aplikace** a zaškrtněte políčko u **dat adresáře pro čtení**. Klikněte na tlačítko **Uložit** uložte této změny a klikněte na **Přidat aplikaci**.

![Přidat oprávnění][api-management-add-devportal-permissions]

Klikněte na ikonu hledání zadejte **APIM** do začátku pole vyberte **APIMAADDemo**a zaškrtněte políčko Uložit.

![Přidat oprávnění][api-management-aad-add-app-permissions]

Klikněte na **Delegovat oprávnění** pro **APIMAADDemo** a zaškrtněte políčko u **APIMAADDemo přístup**a klikněte na tlačítko **Uložit**. Díky tomu vývojář aplikace portálu pro přístup ke službě back-end.

![Přidat oprávnění][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Povolení OAuth 2.0 ověření uživatele pro rozhraní API Kalkulačka

Teď, když server OAuth 2.0 nakonfigurován, zadáte v dialogovém okně Nastavení zabezpečení vaší rozhraní API. Tento krok je znázorněn v tomto videu počínaje 14:30.

V levé nabídce na příkaz **rozhraní API** a klikněte na **kalkulačky** k zobrazení a jeho nastavení.

![Kalkulačka rozhraní API][api-management-calc-api]

Přejděte na kartu **zabezpečení** , zaškrtněte políčko **OAuth 2.0** , vyberte požadovanou se tak mohli ověřovat server z rozevíracího seznamu **se tak mohli ověřovat serveru** a klikněte na tlačítko **Uložit**.

![Kalkulačka rozhraní API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Úspěšně zavolejte API kalkulačky z portálu pro vývojáře

Teď povolení OAuth 2.0 je nakonfigurovaný na rozhraní API, můžete jeho operace úspěšně jen v Centru pro vývojáře. Tento krok je znázorněn v tomto videu počínaje 15:00.

Přejděte zpět do operaci **přidat dvěma celých čísel v nástroji** kalkulačky služby v portálu pro vývojáře a klikněte na **vyzkoušet**. Poznámka: nové položky v části **se tak mohli ověřovat** odpovídající se tak mohli ověřovat serveru, který jste právě přidali.

![Kalkulačka rozhraní API][api-management-calc-authorization-server]

Vyberte z rozevíracího seznamu se tak mohli ověřovat **se tak mohli ověřovat kód** a zadejte přihlašovací údaje účet, který chcete použít. Pokud jste již přihlášeni pomocí účtu, který se nemusí zobrazit výzva.

![Kalkulačka rozhraní API][api-management-devportal-authorization-code]

Klepněte na tlačítko **Odeslat** a poznamenejte si **Stav odpovědi** **200 OK** a výsledky operace v obsahu odpověď.

![Kalkulačka rozhraní API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Konfigurace desktopové aplikace pro volání rozhraní API

Následující postup v tomto videu začíná 16:30 a konfiguruje jednoduché desktopovou aplikaci pro volání rozhraní API. Cílem prvního kroku je zaregistrovat desktopovou aplikaci v Azure AD a dodá přístup do složky a ve službě back-end. Na 18:25 je ukázka desktopové aplikaci volání operaci kalkulačky rozhraní API.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Konfigurace zásad ověření JWT předem povolit požadavky

Konečný postupu v tomto videu začíná 20:48 a se dozvíte, jak používat zásad [Ověřit JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) předem povolit žádostí ověřením tokeny přístupu každé příchozí žádosti. Pokud není ověření žádost zásadami ověřit JWT žádosti je blokován funkcí Správa rozhraní API a není předávají funkcím podél back-end.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

Jiné ukázku konfigurace a používání této zásady, najdete v části [177 dílu průvodní cloudu: Další funkce správy API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed až 13:50. Rychlý přechod 15:00 zobrazíte zásady konfigurované v editoru zásad a klikněte na 18:50 ukázku volání operaci z portálu pro vývojáře s i bez token vyžaduje ověření.

## <a name="next-steps"></a>Další kroky
-   Podívejte se na další [videa](https://azure.microsoft.com/documentation/videos/index/?services=api-management) k rozhraní API správě.
-   Informace o jiných způsobech zabezpečení služby back-end najdete v tématu [ověření vzájemné certifikátu](api-management-howto-mutual-certificates.md) a [připojení prostřednictvím virtuální privátní sítě nebo ExpressRoute](api-management-howto-setup-vpn.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance
[Správa první rozhraní API]: api-management-get-started.md
