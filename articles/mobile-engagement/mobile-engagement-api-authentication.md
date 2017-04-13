<properties 
    pageTitle="Ověření pomocí mobilního zapojení rozhraní REST API"
    description="Popisuje, jak ověřit s Azure Mobile zapojení REST API" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Ověření pomocí mobilního zapojení rozhraní REST API

## <a name="overview"></a>Základní informace

Tento dokument popisuje, jak získat platnou Oauth AAD token ověření s REST API zapojení Mobile. 

Předpokládá se, že máte platný Azure předplatné a nevytvoříte zapojení mobilní aplikace pomocí jednoho z našich [Výukové programy pro vývojáře](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Ověřování

Microsoft Azure Active Directory na základě OAuth token slouží k ověření. 

V pořadí, ověření rozhraní API žádost musí se tak mohli ověřovat záhlaví přidat na každou žádost, což je ve formuláři následující:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Azure Active Directory tokeny končí na 1 hodinu.

Existuje několik způsobů, jak získat token. Protože rozhraní API nazývají obecně z cloudové služby, který chcete použít rozhraní API klíč. Kód Product key pro rozhraní API v Azure terminologii se nazývá služby základní heslo. Následující postup popisuje jedním ze způsobů ruční nastavení.

### <a name="one-time-setup-using-script"></a>Jednorázové instalace (s využitím skriptů)

Postupujte podle sadu níže uvedených pokynů k instalaci pomocí skript Powershellu, která trvá minimální dobu nastavením, ale používá nejčastěji dovolená výchozí hodnoty. V případě potřeby můžete taky postupujte podle pokynů uvedených v článku [Ruční nastavení](mobile-engagement-api-authentication-manual.md) tím z portálu Microsoft Azure přímo a dělat jemnější konfigurace. 

1. Získání nejnovější verzi Azure PowerShell z [tady](http://aka.ms/webpi-azps). Další informace o pokynů ke stažení zobrazí se tento [odkaz](../powershell-install-configure.md).  

2. Azure PowerShell po instalaci používat, ujistěte se, jestli máte nainstalovaný **modul Azure** pomocí následující příkazy:

    na. Ujistěte se, že je k dispozici v seznamu dostupných modulů modul Azure Powershellu. 
    
        Get-Module –ListAvailable 

    ![K dispozici Azure moduly][1]
        
    b. Pokud nemůžete najít modul Azure Powershellu ve výše uvedeném seznamu je potřeba spustit takto:
        
        Import-Module Azure 
        
3. Přihlášení k správce prostředků Azure z prostředí PowerShell spuštěním následujícího příkazu a poskytování svoje uživatelské jméno a heslo účtu Azure: 
        
        Login-AzureRmAccount

4. Pokud máte víc předplatných potom by měl spustíte takto:

    na. Zobrazte seznam Všechna předplatná a zkopírujte SubscriptionId předplatné, které chcete použít. Ujistěte se, že toto předplatné na jeden tato role má přecházíte chcete provést interakci s pomocí rozhraní API aplikace zapojení Mobile. 

        Get-AzureRmSubscription

    b. Spusťte tento příkaz poskytuje SubscriptionId konfigurace předplatného se nemusí používat.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. Zkopírujte text pro [Nový AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) skriptu do místního počítače a uložit jako rutiny prostředí PowerShell (například `APIAuth.ps1`) a spuštění `.\APIAuth.ps1`. 
    
6. Skript vás vyzve k zadání vstup pro **principalName**. Zadejte vhodný název, který chcete použít k vytvoření aplikace služby Active Directory (například APIAuth). 

7. Po dokončení skriptu, zobrazí se následující čtyři hodnoty, které budete muset ověření programově s AD tak zkontrolujte, že je zkopírujte. 
        
    **TenantId** **SubscriptionId**, **ApplicationId**a **tajná**.

    Použijete TenantId jako `{TENANT_ID}`, ApplicationId jako `{CLIENT_ID}` a tajné jako `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Výchozí zásady zabezpečení blokovat spuštění skriptů Powershellu. Pokud ano, dočasně konfigurace zásad spouštění ke spuštění skriptu pomocí následujícího příkazu:

        > Set-ExecutionPolicy RemoteSigned

8. Tady je, jak nastavit PS rutiny vypadat. 

    ![][3]

9. Vrácení se změnami portálu pro správu Azure novou aplikaci AD byl vytvořený pomocí název, který jste zadali skriptu s názvem **principalName** v části **že vlastní zobrazení aplikací naší společnosti**.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Postup získání platné tokenu

1. Volání rozhraní API pomocí následujících parametrů a ujistěte se, pokud chcete nahradit klienta\_ID, klienta\_ID a klient\_TAJNÁ:

    - **Žádost o adresy URL** jako *https://login.microsoftonline.com/ {klienta\_ID} / oauth2/token*
    - **Typ obsahu HTTP záhlaví** jako *aplikace/x--www-form-urlencoded*
    - **Žádost o textu HTTP** *udělit\_typ = klient\_přihlašovací údaje a client_id = {klienta\_ID} & client_secret = {klienta\_TAJNÁ} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Následující obrázek je žádost o konverzaci příkladu:

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Tady je příklad odpověď:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    V tomto příkladu zahrnuté kódování adres URL parametrů příspěvek `resource` hodnota je skutečně `https://management.core.windows.net/`. Nezapomeňte taky URL kódovat `{CLIENT_SECRET}` mohou obsahovat speciální znaky.

    > [AZURE.NOTE] Testování, můžete použít nástroj klientů protokolu HTTP jako [Fiddler](http://www.telerik.com/fiddler) nebo [pošťák Chrome rozšíření](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 

2. Teď na každém volání rozhraní API obsahovat hlavičky se tak mohli ověřovat:

        Authorization: Bearer {ACCESS_TOKEN}

    Pokud se zobrazí stavový kód 401 vrátil, zkontrolujte obsah odpovědí ji může obsahovat tokenu vypršela. V takovém případě získáte nový token.

##<a name="using-the-apis"></a>Použití rozhraní API

Teď, když máte platný token, budete chtít volat rozhraní API.

1. V každém rozhraní API požadavku musíte se předat platné, zbývající token, který jste získali v předchozí části.

2. Je třeba zapojit některé parametry do žádosti URI, který identifikuje aplikace. Pokud chcete žádost URI vypadá takto

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Získat parametry, klikněte na název aplikace a klepněte na řídicí panel a budete se zobrazí stránka jako následující se všemi 3 parametry.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** vaše skupina zdroje název má dělat **MobileEngagement** nevytváříte nový. 

    ![Mobilní zapojení rozhraní API URI parametry][2]

>[AZURE.NOTE] <br/>
>1. Ignorujte adresu kořenové rozhraní API, jak to bylo pro předchozí rozhraní API.<br/>
>2. Pokud jste vytvořili aplikace pomocí klasického Azure portálu budete muset používat název zdroje aplikace, která je jiná než samotný název aplikace. Pokud jste aplikaci vytvořili na portálu Azure by měl použít samotný (je odlišení název zdroje aplikace a aplikace název aplikace vytvořené v novém portálu) název aplikace.  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



