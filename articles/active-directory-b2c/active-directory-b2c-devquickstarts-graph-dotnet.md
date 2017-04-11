<properties
    pageTitle="Azure Active Directory B2C: Použití grafu rozhraní API | Microsoft Azure"
    description="Jak zavolat rozhraní API grafu pro klienta B2C pomocí identitu aplikace pro automatizaci."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure AD B2C: Použití rozhraní API grafu

Azure Active Directory (Azure AD) B2C klienti jsou často obrovské. To znamená, že mnoho běžných úkolů správy klienta provést programově. Primární příkladu je Správa uživatelů. Možná budete muset migrovat stávající úložiště uživatele do B2C klienta. Můžete hostovat registrace uživatele na vlastní stránku a vytvořte uživatelské účty v Azure AD na pozadí. Tyto typy úkolů vyžadují možnost vytvořit, číst, aktualizovat a odstraňovat uživatelské účty. Tyto úkoly můžete udělat pomocí rozhraní API Azure AD grafu.

B2C student existují dva hlavní režimy komunikace s rozhraním API grafu.

- Pro interaktivní jedno spuštění úkoly má fungovat jako účet správce v klientovi B2C při provádění úloh. V tomto režimu musí správce přihlaste se přihlašovacími před, které správce může provádět všechny volání rozhraní API grafu.
- Pro automatické nepřetržitý úkoly byste měli použít určitý typ účtu služby poskytnout potřebná oprávnění k provádění úkolů správy. V Azure AD můžete to uděláte tak, že registrace aplikace a ověření Azure AD. Důvodem je pomocí **ID aplikace** , které používá [pověření klienta OAuth 2.0 udělit](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). V tomto případě aplikace funguje jako samostatně, nikoli jako uživatel, zavolejte rozhraní API grafu.

V tomto článku probereme jak provádět případu automatické použití. Prokázat, abychom budete vytvářet .NET 4.5 `B2CGraphClient` , která provádí uživatele vytvořit, číst, aktualizovat a odstraňovat operace (CRUD). Klient bude mít rozhraní systému Windows příkazového řádku (rozhraní příkazového řádku), který umožňuje vyvolat různé metody. Kód je však psát chovat bez možností interaktivity automatizované způsobem.

## <a name="get-an-azure-ad-b2c-tenant"></a>Získání Azure AD B2C klienta

Než budete moct vytvářet aplikace nebo uživatele a komunikovat s Azure AD vůbec, budete potřebovat Azure AD B2C klienta a účtu globálního správce v klientovi. Pokud nemáte klienta, kterého již, [Začínáme s Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-a-service-application-in-your-tenant"></a>Registrace aplikace služby ve vašem klientovi

Až budete mít B2C klienta, je potřeba vytvořit aplikaci služby pomocí rutin prostředí PowerShell Azure AD.
Nejdřív stáhněte a nainstalujte na [Microsoft Online Services Pomocník pro přihlášení](http://go.microsoft.com/fwlink/?LinkID=286152). Pak budou moct stáhnout a nainstalovat [64bitovou verzi modulu Azure Active Directory pro Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
Rozhraní API grafu pomocí B2C klienta, musíte se zaregistrovat vyhrazené aplikaci pomocí prostředí PowerShell. Postupujte podle pokynů v tomto článku to udělat. Nelze znovu použít již existující B2C aplikace registrovaných Azure portálu.

Po instalaci modulu PowerShell otevřete PowerShell a připojit k vašemu tenantovi B2C. Po spuštění `Get-Credential`, zobrazí se výzva k zadání uživatelského jména a hesla, zadejte uživatelské jméno a heslo účtu správce klienta B2C.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Než začnete vytvářet aplikace, budete muset generovat nové **tajná klienta**.  Aplikace použije tajná klienta ověření Azure AD a získat přístup tokeny. V prostředí PowerShell je možné vytvářet platné tajná:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

Příkaz konečný měli vytisknout nového klienta skryté. Zkopírujte někde bezpečné. Je potřeba ho později. Teď můžete vytvořit aplikaci zadáním nového klienta tajná jako pověření aplikace:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Pokud úspěšně vytvořit aplikaci by měl vytiskněte vlastnosti aplikace, jako jsou ty výše. Budete potřebovat obě `ObjectId` a `AppPrincipalId`, takže příliš zkopírujte tyto hodnoty.

Po vytvoření aplikace ve vašem klientovi B2C potřebujete přiřadit oprávnění potřebná k provedení operace CRUD uživatele. Přiřazení rolí tři aplikace: adresář čtenářům (číst uživatele), adresář autory (Pokud chcete vytvořit a aktualizovat uživatele) a o uživatelského účtu správce (odstranění uživatelů). Tyto role mají známý identifikátory, takže můžete nahradit `-RoleMemberObjectId` parametr s `ObjectId` shora a následující příkazy. Chcete-li zobrazit seznam všech rolí adresáře, zkuste spustit `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

Nyní máte aplikace, která má oprávnění k vytvořit, číst, aktualizovat a odstranit uživatele z vašeho klienta B2C.

## <a name="download-configure-and-build-the-sample-code"></a>Stažení, konfigurace a vytvoření ukázkového kódu

Nejdřív stáhnout ukázkový kód a najděte ho spuštěný. Potom jsme bude trvat bližší pohled na to.  Můžete si [Stáhnout ukázkový kód jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Můžete taky klonovat ho do adresáře podle svého výběru:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Otevřít `B2CGraphClient\B2CGraphClient.sln` Visual Studio řešení ve Visual Studiu. V `B2CGraphClient` projektu, otevřete soubor `App.config`. Nahraďte nastavení tři aplikace vlastními hodnotami:

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Pak klikněte pravým tlačítkem myši na `B2CGraphClient` řešení a opětovného vytvoření vzorku. Pokud jste úspěšné, nyní byste měli mít `B2C.exe` spustitelný soubor umístěn v `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Vytvoření operace CRUD uživatele pomocí rozhraní API grafu

Pokud chcete použít B2CGraphClient, otevřete `cmd` Windows příkaz výzvu a změňte adresář k `Debug` adresář. Spusťte `B2C Help` příkaz.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Zobrazí se stručný popis jednotlivých příkaz. Pokaždé, když vyvoláte jednu z následujících příkazů `B2CGraphClient` odešle žádost k rozhraní API Azure AD grafu.

### <a name="get-an-access-token"></a>Získání přístupový token

Jakékoliv žádosti o k rozhraní API graf vyžaduje přístupový token pro ověřování. `B2CGraphClient`používá otevřít zdroj Active Directory ověřování knihovny (ADAL) k získání přístupu tokeny. ADAL usnadňuje tokenu pořízení pomocí jednoduchého rozhraní API a starat o některé důležité podrobnosti, například ukládání do mezipaměti tokeny přístupu. Nemusíte pomocí ADAL tokenů, i když. Tokeny taky dostanete věnujte požadavků HTTP.

> [AZURE.NOTE]
    Tento kód příkladu ADAL v2 komunikovat s rozhraní API grafu.  Abyste mohli získávat tokeny přístupu, které lze použít s rozhraním API Azure AD graf je nutné použít ADAL verze 2 nebo 3.

Když `B2CGraphClient` získáte vytváří instanci `B2CGraphClient` předmětu. Konstruktoru pro tuto třídu nastaví vygenerovaných ADAL ověření:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Budeme používat `B2C Get-User` příkazu jako příklad. Když `B2C Get-User` vyvolání bez nějaké další vstupy volání rozhraní příkazového řádku `B2CGraphClient.GetAllUsers(...)` metody. Tato metoda volá `B2CGraphClient.SendGraphGetRequest(...)`, která odešle žádost HTTP GET k rozhraní API grafu. Před `B2CGraphClient.SendGraphGetRequest(...)` požádat o GET odešle, nejdřív nenarazí přístupový token pomocí ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Můžete získat přístupový token pro rozhraní API grafu tak, že zavoláte ADAL `AuthenticationContext.AcquireToken(...)` metody. Vrátí ADAL `access_token` identitu aplikace, která představuje.

### <a name="read-users"></a>Přečtěte si uživatelé

Pokud chcete získat seznam uživatelů, nebo získejte určitého uživatele ze rozhraní API grafu, můžete odeslat protokolu HTTP `GET` požádat o `/users` koncového bodu. Žádost o pro všechny uživatele v klienta vypadat takto:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Pokud chcete zobrazit tohoto požadavku, spusťte:

 ```
 > B2C Get-User
 ```

Všimněte si dvou důležitých věcí:

- Přístupový token získali prostřednictvím ADAL se přidá do `Authorization` záhlaví pomocí `Bearer` schéma.
- U tenantů B2C, je nutné použít parametr dotazu `api-version=1.6`.

Obě tyto údaje jsou zpracovány v `B2CGraphClient.SendGraphGetRequest(...)` metodu:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Vytvoření spotř uživatelských účtů

Když vytvoříte uživatelské účty ve vašem klientovi B2C, můžete poslat protokolu HTTP `POST` požádat o `/users` koncového bodu:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Většina těchto vlastností v tomto žádosti o jsou potřebná k vytvoření spotř uživatelů. Další informace, klikněte [sem](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Všimněte si, že `//` poznámky byly vytvořeny pro obrázek. Neobsahují v skutečnou žádost.

Pokud chcete zobrazit žádosti, spusťte některou z následujících příkazů:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

`Create-User` Příkaz bere .json soubor jako vstupní parametry. Tato stránka obsahuje formátu JSON objektu uživatele. Existují dva ukázkové .json soubory v ukázkovém kódu: `usertemplate-email.json` a `usertemplate-username.json`. Tyto soubory podle potřeby můžete změnit. Kromě povinná pole nahoře několik volitelná pole, které můžete použít najdete v těchto souborů. Podrobnosti o volitelná pole najdete [Přehled entit rozhraní API Azure AD grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Zobrazí se, jak je vytvořen žádost příspěvek v `B2CGraphClient.SendGraphPostRequest(...)`.

- Připojí přístupový token k `Authorization` záhlaví žádosti o.
- Nastaví `api-version=1.6`.
- V textu žádosti o obsahuje objektu uživatele JSON.

> [AZURE.NOTE]
Pokud účty, které chcete migraci z existující úložiště uživatele nižší heslo síly než [nevynucují Azure AD B2C síly silné heslo](https://msdn.microsoft.com/library/azure/jj943764.aspx), můžete zakázat pomocí silné heslo požadavek `DisableStrongPassword` hodnota argumentu `passwordPolicies` vlastnost. Například, můžete upravit požadavek uživatele vytvořit pomocí výše uvedeného takto: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Aktualizace spotř uživatelských účtů

Když aktualizujete objektů uživatele proces je podobná té, které používáte k vytvoření objektů uživatele. Tento proces pomocí protokolu HTTP, ale `PATCH` metodu:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Pokuste se aktualizovat uživatele aktualizací souborů JSON novými daty. Pak můžete použít `B2CGraphClient` spustit jednu z následujících příkazů:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Kontrola, zda `B2CGraphClient.SendGraphPatchRequest(...)` metoda podrobné informace o tom, jak poslat tohoto požadavku.

### <a name="search-users"></a>Hledání uživatelům

Uživatele můžete vyhledávat ve vašem klientovi B2C několika způsoby. Ho pomocí uživatele objektu ID nebo dva pomocí uživatele přihlašovací identifikátorem (například `signInNames` vlastnost).

Spusťte některou z následujících příkazů k vyhledání určitého uživatele:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Tady je několik příkladů:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Odstranění uživatelů

Proces odstraněním uživatele je jednoduché. Použití HTTP `DELETE` metoda a konstrukce adresu URL s správné ID objektu:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Příklad zobrazíte zadejte tento příkaz a zobrazte žádost o odstranění vytištění ke konzole:

```
> B2C Delete-User <object-id-of-user>
```

Kontrola, zda `B2CGraphClient.SendGraphDeleteRequest(...)` metoda podrobné informace o tom, jak poslat tohoto požadavku.

Můžete provést mnoho příkazu jiné akce s rozhraním API Azure AD graf kromě Správa uživatelů. [Rozhraní API Azure AD diagram reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) poskytuje podrobné informace o jednotlivých akci, spolu s požadavky na ukázky.

## <a name="use-custom-attributes"></a>Použít vlastní atributy

Většina aplikací příjemce je nutné uložit určitý typ vlastních informací o uživatelském profilu. Můžete to udělat jedním ze způsobů je definovat vlastní atribut ve vašem klientovi B2C. Můžete pak nakládáte atributu stejným způsobem jako jiné žádné jiné vlastnosti objektu uživatele. Aktualizovat atribut, odstraňte atribut, dotazu atributem, můžete odeslat atribut jako deklaraci identity v přihlašovací tokeny a další.

Definovat vlastní atribut ve vašem klientovi B2C, najdete v článku [B2C vlastní atribut odkaz](active-directory-b2c-reference-custom-attr.md).

Můžete zobrazit vlastní atributy definované ve vašem klientovi B2C pomocí `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Výstup tyto funkce zobrazíte podrobnosti každé vlastní atribut, například:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Celé jméno, můžete použít jako `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, jako vlastnost na objekty uživatelů.  Aktualizovat soubor .json nové vlastnosti a hodnotou vlastnosti a potom spusťte:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Pomocí `B2CGraphClient`, máte aplikace služby, které můžete spravovat vaši uživatelé klienta B2C programově. `B2CGraphClient`ověřuje k rozhraní API grafu Azure AD pomocí totožnost aplikace. Získá také tokeny pomocí tajná klienta. Jak tuto funkci zahrnout do aplikace, mějte na paměti několik klíčových bodů B2C aplikací:

- Potřebujete oprávnění aplikace začátcích slov v klientovi.
- Nyní budete muset pomocí ADAL v2 tokeny přístupu. (Můžete taky odeslat zprávy protokolu přímo, bez použití knihovny.)
- Při volání rozhraní API grafu použít `api-version=1.6`.
- Při vytváření a aktualizovat uživatele spotř několika vlastností podporují, jak jsme je popsali výše.

Pokud máte dotazy nebo žádosti o akce, že se mají provést pomocí rozhraní API graf na B2C klienta, napište komentář v tomto článku nebo souboru problém v úložišti GitHub kód vzorku.
