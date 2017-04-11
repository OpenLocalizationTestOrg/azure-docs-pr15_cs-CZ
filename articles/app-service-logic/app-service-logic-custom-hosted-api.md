<properties
    pageTitle="Volání vlastní rozhraní API v aplikacích pro použití logických operátorů"
    description="Používání vaší vlastní rozhraní API hostitelem služby aplikace s aplikacemi jiných logiky"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>Používání vaší vlastní rozhraní API hostitelem služby aplikace s aplikacemi jiných logiky

Ačkoli aplikace logiku obsahuje celá řada spojnic 40 + různých služeb, které chcete volat do vlastního vlastní rozhraní API mohlo by umožnit spuštění vlastního kódu. Jedním ze způsobů nejjednodušší a nejčastěji scalable hostovat vlastní vlastního webového rozhraní API je používat aplikaci služby. Tento článek popisuje, jak k volání do libovolné webového rozhraní API použitý ve aplikace rozhraní API aplikace služby, v prohlížeči nebo mobilní aplikaci.

Informace o vytváření rozhraní API jako aktivační událost nebo akce aplikace použití logických operátorů podívejte se na [Tento článek](app-service-logic-create-api-app.md).

## <a name="deploy-your-web-app"></a>Nasazení aplikace pro Web

Nejdřív musíte nasadit vaše rozhraní API jako Web App v aplikaci služby. Základní informace o nasazení najdete pokyny tady: [Vytvoření aplikace pro web ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).  I když můžete volat na jakékoliv rozhraní API pomocí funkčně logiku aplikace, která zaručuje nejlepší možnosti doporučujeme můžete sčítat Swagger metadat snadno integrovat s aplikací akce logiku.  Podrobné informace najdete na [Přidání swagger](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui).

### <a name="api-settings"></a>Nastavení rozhraní API

Popořádku návrháře aplikace použití logických operátorů k analýze vaší Swagger je důležité povolit CORS a nastavit vlastnosti APIDefinition svojí webové aplikace.  Toto je velmi snadné nastavená portálu Azure.  Jednoduše otevřít zásuvné nastavení svojí webové aplikace a v části rozhraní API "Rozhraní API definici" na adresu URL swagger.json souboru (je to obvykle https://{name}.azurewebsites.net/swagger/docs/v1) a přidání CORS zásady pro "*" umožňující žádosti z logických aplikací Návrhář.

## <a name="calling-into-the-api"></a>Volání do rozhraní API

V portálu aplikace použití logických operátorů, pokud jste nastavili CORS a rozhraní API definice vlastnosti by měly je možné snadno přidáte rozhraní API vlastní akce v rámci vašeho toku.  V Návrháři vyberete Procházet vaše předplatné weby zobrazíte seznam webů s adresou URL swagger definované.  Můžete taky použít HTTP + Swagger Akce přejděte swagger a seznam dostupných akcí a zadávání.  Nakonec můžete kdykoli vytvořit žádost o vytočení jakékoliv rozhraní API, i ty, které mají nebo vystavit swagger dokumentu pomocí HTTP akce.

Pokud chcete zajistit vaše rozhraní API, existuje několik způsobů, jak to udělat:

1. Kód stejně jako povinné - Azure Active Directory mohou sloužit k ochraně svého rozhraní API bez nutnosti kód změny nebo nové nasazení.
1. Vynucení základní Auth, AAD Auth nebo Auth certifikátu do kódu vašeho rozhraní API.

## <a name="securing-calls-to-your-api-without-a-code-change"></a>Zabezpečení volání na váš rozhraní API bez změny kódu

V této části vytvoříte dva Azure Active Directory aplikace – jeden pro použití logických operátorů aplikace a druhý pro webovou aplikaci.  Budete ověřování volání do webové aplikace pomocí služby hlavního uživatele (id klienta a tajná) přidružené k aplikaci AAD logiku aplikace. Nakonec se zahrnout aplikaci ID v definici aplikace vaší logiky.

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>Část 1: Nastavení identitu aplikace aplikace logiky

Toto je aplikaci logiky používá k ověřování služby active directory. Je jenom *potřebujete* udělat jednou u adresáře. Například můžete použít stejné identity pro všechny vaše aplikace logiky sice můžete taky vytvořit jedinečný identit za použití logických operátorů aplikace chcete-li. Můžete to udělat v uživatelském rozhraní nebo pomocí Powershellu.

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>Vytvoření aplikace identity na portálu Azure klasické

1. Přejděte do [služby Active directory v portálu Azure klasické](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) a vyberte adresář, který používáte pro webovou aplikaci
2. Klikněte na kartu **aplikace**
3. V panelu s příkazy v dolní části stránky klikněte na tlačítko **Přidat**
4. Vaši identitu pojmenovat, klikněte na Další šipky
5. Vložte do jedinečný řetězec formátu doménu ve dvou textových polí a klikněte na zaškrtnutí
6. Klikněte na kartu **Konfigurovat** pro tuto aplikaci
7. Zkopírujte **Kód klienta**, použije se v aplikaci použití logických operátorů
8. V části **klávesy** klikněte na **Výběr dobu trvání** a vyberte buď jeden rok, nebo dva roky
9. Klikněte na tlačítko **Uložit** v dolní části obrazovky (možná bude potřeba počkat několik sekund, než)
10. Teď je potřeba zkopírovat klíč v rozevíracím seznamu. To bude použito v aplikaci použití logických operátorů

#### <a name="create-the-application-identity-using-powershell"></a>Vytvoření aplikace identitu pomocí prostředí PowerShell
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Zkopírujte **ID klienta**, **ID aplikace** a heslo, které jste použili

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>Část 2: Ochrana webovou aplikaci k identitě AAD aplikace

Pokud už nasazený webovou aplikaci můžete ji povolit jenom na portálu. V ostatních případech může být povolení se tak mohli ověřovat v části Správce nasazení služby Azure zdroje.

#### <a name="enable-authorization-in-the-azure-portal"></a>Povolení se tak mohli ověřovat na portálu Azure

1. Přejděte na Web appu a klikněte na možnost **Nastavení** na panelu příkazů.
2. Klikněte na **Povolení/ověření**.
3. **Zapněte.**

V tomto okamžiku aplikace se automaticky vytvoří. Potřebujete tuto aplikaci klienta ID pro část 3, takže budete muset:

1. Přejděte do [služby Active directory v portálu Azure klasické](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) a vyberte adresáře.
2. Hledání v aplikaci Vyhledávací pole
3. Klikněte na ni v seznamu
4. Klikněte na kartu **Konfigurovat**
5. Měli byste vidět **ID klienta**

#### <a name="deploying-your-web-app-using-an-arm-template"></a>Nasazení webovou aplikaci pomocí šablony ARM

Nejprve potřebujete k vytvoření aplikace pro Web app. To musejí být různé z aplikace, která slouží k použití logických operátorů aplikace. Začněte tím, že po výše uvedené kroky v části 1, ale teď pro **domovskou stránku** a **IdentifierUris** použití skutečné https://**Adresa URL** webové aplikace.

>[AZURE.NOTE]Při vytváření aplikace pro Web app, je nutné použít [Azure klasické portálu přístup](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory), jak prostředí PowerShell není nastaven požadovaná oprávnění pro přihlášení uživatele k webu.

Jakmile máte klienta ID a ID klienta, patří jako zdroj sub ve Web appu v šabloně nasazení:

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Spusťte na nasazení automaticky, která nasadí prázdný Web app a app logiky dohromady, využívající AAD, klikněte na toto tlačítko:

[![Nasazení Azure](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Dokončení šablony naleznete v tématu [logiku aplikace do vlastní rozhraní API hostitelem aplikaci služby a chráněny AAD](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json).


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>Část 3: Naplnění části se tak mohli ověřovat v aplikaci použití logických operátorů

V části **se tak mohli ověřovat** **HTTP** akce:`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Element | Popis |
|---------|-------------|
| Typ | Typ ověřování. Ověřování ActiveDirectoryOAuth hodnotu ActiveDirectoryOAuth. |
| klienta | Slouží k identifikaci klienta AD identifikátor klienta. |
| cílové skupiny | Povinné. Zdroje, ke kterému se připojujete. |
| clientID | Identifikátor klientské aplikaci Azure AD. |
| tajná | Povinné. Tajná klienta, který požaduje tokenu. |

Výše uvedené šablony už má toto nastavení, ale pokud vytváříte aplikaci logiky přímo, musíte se zahrnout celou se tak mohli ověřovat oddíl.

## <a name="securing-your-api-in-code"></a>Zabezpečení API v kódu

### <a name="certificate-auth"></a>Certifikát auth

Můžete certifikátů k ověření příchozí žádosti do webové aplikace. V tématu [Jak chcete konfigurovat TLS vzájemné ověřování pro Web App](../app-service-web/app-service-web-configure-tls-mutual-auth.md) pro nastavíte kódu.

V části *se tak mohli ověřovat* by měl obsahovat: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Element | Popis |
|---------|-------------|
| Typ | Povinné. Typ ověřování. Certifikáty SSL klienta musí být hodnota ClientCertificate. |
| PFX | Povinné. Kódováním Base 64 obsah souboru PFX. |
| heslo | Povinné. Heslo pro přístup k souboru PFX. |

### <a name="basic-auth"></a>Základní ověřování

Základní ověřování (například uživatelské jméno a heslo) můžete použít k ověření příchozí žádosti. Základní ověřování se řídí určitým vzorem běžné a můžete to udělat v jiném jazyce, který vytvoříte v aplikaci.

V části *se tak mohli ověřovat* by měl obsahovat: `{"type": "basic","username": "test","password": "test"}`.

| Element | Popis |
|---------|-------------|
| Typ | Povinné. Typ ověřování. Základní ověřování musí být hodnota Basic. |
| uživatelské jméno | Povinné. Uživatelské jméno k ověření. |
| heslo | Povinné. Heslo k ověření. |

### <a name="handle-aad-auth-in-code"></a>Úchyt AAD auth v kódu

Ve výchozím nastavení ověřování Azure Active Directory, umožňující na portálu nedělá jemně odstupňovaná oprávnění. Například uzamknout není rozhraní API pro určitého uživatele nebo aplikace, ale jenom na určité klienta.

Pokud chcete omezit rozhraní API jenom logiky aplikaci, třeba v kódu, se dají extrahovat záhlaví, která obsahuje JWT a zjištění, kdo je, volající odmítnutí požadavky, které se neshodují.

Budete pokračovat, pokud chcete implementovat úplně ve vlastním kódu a ne využít funkci portál, můžete v tomto článku: [Použití služby Active Directory pro ověřování aplikace služby Azure](../app-service-web/web-sites-authentication-authorization.md).

Potřebujete postupujte podle výše uvedených kroků identitu aplikace aplikace logiky Kam zmizely, který volání rozhraní API.
