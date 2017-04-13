<properties 
   pageTitle="Ověřování služby Active Directory a správce prostředků | Microsoft Azure"
   description="Příručka pro vývojáře ověřování s Azure správce prostředků rozhraní API aplikace a služby Active Directory pro integraci aplikace se ostatní Azure předplatná."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="dugill;tomfitz" />

# <a name="how-to-use-azure-active-directory-and-resource-manager-to-manage-a-customers-resources"></a>Používání služby Azure Active Directory a správce prostředků pro přidávání a používání zdrojů zákazníka

## <a name="introduction"></a>Úvod

Jestliže jste vývojáři softwaru kdo potřebuje vytvořit aplikace, která spravuje Azure prostředky zákazníka, toto téma ukazuje, jak ověřit pomocí rozhraní API Správce prostředků Azure a získat přístup ke zdrojům v ostatních odběrů. 

Aplikaci můžete získat přístup k rozhraní API Správce prostředků v několika způsoby:

1. **Uživatele + aplikace access**: k aplikacím, které přístupu k prostředkům jménem přihlášený uživatel. Tento postup lze použít u aplikací, jako jsou webové aplikace a nástroje příkazového řádku, která se týkají jenom "interaktivní řízení" Azure zdrojů.
1. **Pouze aplikace access**: pro aplikace spuštěné démon služeb a plánované práce. V aplikaci identity uděleno přímý přístup k zdroji. Tento postup lze použít u aplikací, které potřebují dlouhodobé "offline přístup" k Azure.

Toto téma obsahuje podrobné pokyny k vytvoření aplikace pro využívá obou těchto postupů se tak mohli ověřovat. Ukazuje, jak provedení všech kroků rozhraní REST API nebo C#. Dokončení ASP.NET MVC aplikace je k dispozici [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

Celý kód pro toto téma je spuštěný jako do webových aplikací, který si můžete vyzkoušet na [http://vipswapper.azurewebsites.net/cloudsense](http://vipswapper.azurewebsites.net/cloudsense). 

## <a name="what-the-web-app-does"></a>Co znamená web appu

Web appu:

1. Znaky v Azure uživatele.
2. Dotaz uživatelům udělit přístup na web app správci zdrojů.
3. Získá uživatel + aplikace přístupový token pro přístup k správce prostředků.
4. Volání správce prostředků a v aplikaci služby jistinu přiřadit roli v předplatného, který dává přístup dlouhodobé aplikace k předplatnému používá token (z kroku 3).
5. Získá přístup pouze aplikace tokenu.
6. Ke správě zdrojů v předplatné prostřednictvím Správce prostředků používá token (z kroku 5).

Tady je tok začátku do konce webové aplikace.

![Tok ověřování správce prostředků](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Jako uživatel zadejte pro předplatné, které chcete použít id předplatného:

![Zadejte id předplatného](./media/resource-manager-api-authentication/sample-ux-1.png)

Vyberte účet, který chcete použít pro přihlášení.

![Vyberte účet](./media/resource-manager-api-authentication/sample-ux-2.png)

Zadejte svoje přihlašovací údaje.

![Zadejte přihlašovací údaje](./media/resource-manager-api-authentication/sample-ux-3.png)

Udělení přístupu k aplikaci pro předplatné Azure:
 
![Udělení přístupu](./media/resource-manager-api-authentication/sample-ux-4.png)
 
Správa předplatného připojení:

![Připojení předplatného](./media/resource-manager-api-authentication/sample-ux-7.png)


## <a name="register-application"></a>Registrace aplikace

Než začnete kódování zaregistrujte webovou aplikaci s Azure Active Directory (AD). Registrace aplikace vytvoří ústřední identity aplikace v Azure AD. Obsahuje základní informace o aplikaci například ID klienta OAuth odpovědět adresy URL a přihlašovací údaje, které aplikace používá ověřování a přístup k rozhraní API Azure správce prostředků. Registrace aplikace zaznamená do různých delegované oprávnění, které potřebuje vaše aplikace při přístupu k Microsoft APIs jménem uživatele. 

Protože aplikace přistupuje jiného předplatného, musí ji nakonfigurovat jako aplikace více klienta. K předání ověření, zadejte doménu spojený se službou Active Directory. Domény přidružený k Active Directory zobrazíte přihlaste [klasické portálu](https://manage.windowsazure.com). Vyberte služby Active Directory a zvolte **domény**.

Následující příklad ukazuje, jak si zaregistrovat aplikace pomocí prostředí PowerShell Azure. Musíte mít nejnovější verzi (srpen 2016) Azure PowerShell tento příkaz fungoval. 

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
    
Přihlásit se jako AD aplikace, musíte aplikace id a heslo. Zobrazit id aplikace vrácených z předchozího příkazu, můžete:

    $app.ApplicationId

Následující příklad ukazuje, jak zaregistrovat aplikace pomocí rozhraní příkazového řádku Azure. 

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

Výsledky zahrnout ID aplikace, které budete potřebovat při ověřování jako aplikace.

### <a name="optional-configuration---certificate-credential"></a>Volitelné konfigurace – certifikát přihlašovacích údajů

Azure AD taky podporuje certifikát přihlašovacích údajů pro aplikace: vytvoření certifikátu podepsaného svým držitelem, ponechat privátním klíčem a přidat veřejný klíč k registraci aplikace Azure AD. Pro ověřování aplikace odešle malé datové Azure AD přihlášení pomocí privátním klíčem a Azure AD ověří podpis pomocí veřejným klíčem, kterou jste registrovali.

Informace o vytváření AD aplikaci pomocí certifikátu najdete v článku [Použití Azure PowerShell vytvoření služby základní k přístupu k prostředkům](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) nebo [Použití Azure rozhraní příkazového řádku pro vytvoření služby základní a získat informace](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Získání klienta id od id předplatného

Požádat o token mohou sloužit k volání správce prostředků, aplikace znát ID klienta Azure AD klienta, který je hostitelem Azure předplatného. Největší pravděpodobností uživatelé vědět jejich ID předplatného, ale může se stát znají jejich klienta ID služby Active Directory. Získat klienta id uživatele, požádejte uživatele, id předplatného. Při odesílání žádost o předplatné, zadejte tento id předplatného:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Žádost selhala, protože nebyla přihlášení uživatele dosud, ale načtete id klienta z odpověď. V tato výjimka načtení id klienta ze záhlaví hodnota odpovědi **Ověření WWW**. Zobrazí se tato implementace metodu [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) .

## <a name="get-user--app-access-token"></a>Získání uživatele + aplikace přístupový token

Aplikace přesměruje uživatele Azure AD pomocí OAuth 2.0 povolte požadavek – ověření přihlašovací údaje uživatele a vraťte se tak mohli ověřovat kód. Aplikace se tak mohli ověřovat kód používá k získání přístupový token pro správce prostředků. Metoda [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) vytvoří žádost o ověření.

Toto téma popisuje požadavky rozhraní REST API pro ověření uživatele. Můžete také pomocné knihovny k provedení ověřování v kódu. Další informace o těchto knihoven najdete v tématu [Azure Active Directory Authentication knihoven](./active-directory/active-directory-authentication-libraries.md). Integrace služby Správa identit v aplikaci, najdete v článku [Příručka pro vývojáře služby Azure Active Directory](./active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Žádost o auth (OAuth 2.0)

Vydat otevřít ID připojit/OAuth2.0 povolte žádosti o povolení Azure AD koncového bodu:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Parametry řetězce dotazu, které jsou k dispozici pro tuto žádost jsou popsaná v tématu [žádost kódem se tak mohli ověřovat](./active-directory/active-directory-protocols-oauth-code.md#request-an-authorization-code) .

Následující příklad ukazuje, jak požádat o souhlas OAuth2.0:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD ověřuje uživatele a v případě potřeby výzvu k udělit oprávnění k aplikaci. Vrátí kód se tak mohli ověřovat na adresu URL odpověď aplikace. V závislosti na požadované response_mode Azure AD buď odešle zpět data v řetězci dotazu nebo jako odeslaná data.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Ověření žádosti o (otevřít připojení ID)

Pokud nejen chcete získat přístup správce prostředků Azure jménem uživatele, ale taky povolit uživatele se přihlásit k aplikaci pomocí svůj účet Azure AD, zadejte otevřít ID připojení povolte žádosti o. S otevřít připojení ID aplikace taky přijímání id_token Azure AD, aplikace můžete použít k přihlášení uživatele.

Parametry řetězce dotazu, které jsou k dispozici pro tuto žádost jsou popsaná v tématu [odeslání žádosti o přihlášení](./active-directory/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) .

Příklad otevřít připojení ID žádost o konverzaci je:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD ověřuje uživatele a v případě potřeby výzvu k udělit oprávnění k aplikaci. Vrátí kód se tak mohli ověřovat na adresu URL odpověď aplikace. V závislosti na požadované response_mode Azure AD buď odešle zpět data v řetězci dotazu nebo jako odeslaná data.

Příklad otevřít připojení ID odpověď je:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Žádost o token (OAuth2.0 kód udělit tok)

Teď, když aplikace obdržel kód se tak mohli ověřovat z Azure AD, je čas na získat přístup token pro správce prostředků Azure.  Zveřejňují příspěvky OAuth2.0 kód grant (udělit) tokenu požádat o Azure AD tokenu koncového bodu: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Parametry řetězce dotazu, které jsou k dispozici pro tuto žádost jsou popsaná v tématu [Povolení kód použít](./active-directory/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) .

Následující příklad ukazuje žádost o kód udělit token pomocí hesla:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Při práci s certifikát přihlašovací údaje, vytvořte JSON Web tokenu (JWT) a přihlaste (RSA SHA256) pomocí privátním klíčem aplikace certifikát pověření. Typy deklaraci identity pro token jsou uvedeny v [deklarace tokenů JWT](./active-directory/active-directory-protocols-oauth-code.md#jwt-token-claims). Odkaz najdete v článku [Active Directory Auth knihovny (.NET) kód](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) podepsat tokeny JWT výraz klienta.

Přečtěte si článek [připojení otevřené ID specifikace](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) podrobnosti na ověření klienta. 

Následující příklad ukazuje žádost o kód udělit token s certifikát přihlašovacích údajů:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
    
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012
    
    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Příklad odpověď token grant (udělit) kód: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Zpracování tokenu odpověď kód grant (udělit)

Úspěšné tokenu odpověď obsahuje (uživatele + aplikaci) přístupový token pro správce prostředků Azure. Vaše aplikace používá přístupový token pro přístup k správce prostředků jménem uživatele. Životnost tokeny přístupu vydán Azure AD je hodinu. Je pravděpodobné, že webové aplikace vyžaduje k obnovení (uživatele + aplikaci) přístupový token. Pokud je potřeba obnovit přístupový token, použijte aktualizace tokenu načítající aplikace v tokenu odpovědi. Zveřejňují příspěvky OAuth2.0 tokenu požádat o Azure AD tokenu koncového bodu: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Parametry pro použití s žádostí o aktualizaci jsou popsány v [aktualizace přístupový token](./active-directory/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

Následující příklad ukazuje, jak používat aktualizace tokenu:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Aktualizace tokeny lze získat nové tokeny přístupu pro správce prostředků Azure, ale nejsou vhodné pro offline přístup v aplikaci. Životnost tokeny aktualizace je omezené a tokeny aktualizace jsou vázaný na uživatele. Pokud uživatel opustí organizaci, ztratí pomocí tokenu aktualizace aplikace access. Tento přístup není vhodné pro aplikace, které využívají týmům spravovat Azure zdroje.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Zaškrtněte, pokud uživatel můžete přiřadit přístup k předplatnému

Aplikaci teď má token pro přístup k správce prostředků Azure jménem uživatele. Dalším krokem je připojení aplikace k předplatnému. Po připojení aplikace můžete spravovat tyto předplatná i v případě, že uživatel není prezentovat (dlouhodobé offline přístup). 

U každého předplatného připojení volání [oprávnění správce prostředků seznamu](https://msdn.microsoft.com/library/azure/dn906889.aspx) rozhraní API a zjistit, jestli má uživatel správy přístupových práv pro předplatné.

Metoda [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) aplikace ukázkové ASP.NET MVC provádí volání.

Následující příklad ukazuje, jak požádat o oprávnění uživatele na předplatné. 83cfe939-2402-4581-b761-4f59b0a041e4 je id předplatné.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Příklad odpověď vám udělil oprávnění uživatele předplatného je:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

Oprávnění rozhraní API vrátí více oprávnění. Každý oprávnění se skládá z povolené akce (akce) a nejsou povoleny: akce (notactions). Pokud je k dispozici v seznamu Akce povolené všechna oprávnění akce a není k dispozici v seznamu notactions tohoto oprávnění, má uživatel k provedení této akce. **Microsoft.Authorization/RoleAssignments/Write** je akci, která povolí přístup správy přístupových práv. Aplikace musí analyzovat výsledku oprávnění můžete vyhledávat regex POZVYHLEDAT na tento řetězec akce v poli Akce a notactions jednotlivých oprávnění.

## <a name="get-app-only-access-token"></a>Získat přístup jenom aplikace, tokenu

Teď budete vědět, pokud uživatel přiřadit přístup Azure předplatného. Další kroky jsou:

1. Přiřaďte odpovídající roli RBAC aplikace identity u předplatného.
2. Ověření přiřazení aplikace access pomocí dotazu pro aplikace oprávnění u předplatného nebo přístupem správce prostředků pomocí tokenu jen aplikace.
1. Zaznamenejte připojení do struktury dat aplikace "připojeného předplatná" - uchování id předplatné.

Podívejme se podrobněji na prvním krokem. Pokud chcete přiřadit roli odpovídající RBAC identitu aplikace, musíte zjistit:

- Id objektu identity aplikace ve službě uživatele Azure Active Directory
- Identifikátor role RBAC vyžadovaného aplikace u předplatného

Pokud aplikace ověřuje uživatele z Azure AD, vytvoří služby objekt aplikace v Azure AD. Azure umožňuje RBAC role pro objekty služby dáte přímý přístup k odpovídající aplikací v Azure zdroje. Tato akce je přesně přejeme dělat. Rozhraní API Azure AD grafu dotazu určit identifikátor jistiny služby aplikace v přihlášený uživatel je Azure AD.

Stačí přístupový token pro správce prostředků Azure – je nutné nové přístupový token volání rozhraní API Azure AD grafu. Každá aplikace v Azure AD má oprávnění k vytvoření dotazu vlastní služby objekt, takže stačí jen aplikace přístupový token.

<a id="app-azure-ad-graph">
### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Získat token jen aplikace access pro rozhraní API Azure AD grafu

Ověřování aplikace a získání token rozhraní API Azure AD grafu, odeslat klienta pověření udělit OAuth2.0 toku tokenu Azure AD tokenu koncový bod (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/Token**).

Metoda [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) vzorové aplikace ASP.net MVC získá jen aplikace access tokenu pro rozhraní API grafu pomocí Active Directory Authentication Library pro .NET.

Parametry řetězce dotazu, které jsou k dispozici pro tuto žádost jsou popsaná v tématu [žádost tokenu aplikace Access](./active-directory/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) .

Příklad žádost o token udělit klienta přihlašovacích údajů: 

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Příklad odpověď pro klienta credential udělit token: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Získání objektu aplikace služby jistiny v uživatele Azure AD

Teď použijte pouze aplikace přístupový token k vytvoření dotazu [objekty služby Azure AD grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) rozhraní API k určení Id objektu aplikace služby základní v adresáři.

Metoda [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) vzorové aplikace ASP.net MVC provádí volání.

Následující příklad ukazuje, jak požádat o jistinu služby aplikace. a0448380-c346-4f9f-b897-c18733de9394 je id klienta aplikace.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Následující příklad ukazuje odpověď na žádost o aplikace služby základní 

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Získání identifikátor role Azure RBAC

Pokud chcete přiřadit roli odpovídající RBAC služby základní, je třeba určit identifikátor Azure RBAC role.

Pravý RBAC roli aplikace:

- Pokud aplikace pouze sleduje předplatné, bez jakýchkoli změn, vyžaduje pouze čtečka oprávnění u předplatného. Přiřazení role **Čtenář** .
- Pokud aplikace spravuje Azure předplatného, vytváření nebo úpravy nebo odstranění entity vyžaduje oprávnění Přispěvatel.
  - Ke správě určitý typ zdroje, přiřadíte role přispěvatele specifické pro zdroje (virtuální počítač Přispěvatel, virtuální přispěvatelů sítě, přispěvatelů účtu úložiště atd.)
  - Pokud chcete spravovat libovolný typ zdroje, přiřazení role **přispěvatele** .

Přiřazování rolí aplikace je viditelná pro uživatele, takže vyberte povinné nejméně oprávnění.

Zavolejte na linku [definice role správce prostředků rozhraní API](https://msdn.microsoft.com/library/azure/dn906879.aspx) seznamu Azure RBAC rolí a hledání pak iteraci výsledek vyhledat podle jména definici požadovanou roli.

Metoda [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) aplikace ukázkové ASP.net MVC provádí volání.

Následující příklad žádost o ukazuje, jak získat identifikátor role Azure RBAC. 09cbd307-aa71-4aca-b346-5f253e6e3ebb je id předplatné.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Odpověď na probíhá v tomto formátu: 

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Volání toto rozhraní API průběžné nepotřebujete. Jakmile zjistili známý GUID definici role můžete sestavit id definice role jako:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Tady je známý GUID běžně používaných předdefinované role:

| Role | Identifikátor GUID |
| ----- | ------ |
| Čtečky | acdd72a7-3385-48EF-bd42-f606fba81ae7
| Skupiny přispěvatelů | b24988ac-6180-42A0-ab88-20f7382dd24c
| Virtuální počítač přispěvatelů | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| Virtuální sítě přispěvatelů | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Úložiště účtu přispěvatelů | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Web přispěvatelů | de139f84-1756-47ae-9be6-808fbbe84772
| Plán přispěvatelů webu | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| SQL Server přispěvatelů | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| Přispěvatel databáze SQL | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec

### <a name="assign-rbac-role-to-application"></a>Přiřazení RBAC role aplikace

Máte všechno, co potřebujete odpovídající roli RBAC služby základní pomocí přiřaďte [Vytvoření přiřazování rolí správce prostředků](https://msdn.microsoft.com/library/azure/dn906887.aspx) rozhraní API.

Metoda [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) aplikace ukázkové ASP.net MVC provádí volání.

Příklad žádosti přiřadit roli RBAC aplikace: 

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

V pozvánce na schůzku slouží následující hodnoty:

| Identifikátor GUID | Popis |
| ------ | --------- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb | id předplatného
| c3097b31-7309-4C59-b4e3-770f8406bad2 | id objektu jistiny služby aplikace
| acdd72a7-3385-48EF-bd42-f606fba81ae7 | id reader role
| 4f87261d-2816-465D-8311-70a27558df4c | identifikátor guid vytvoří nový přiřazování rolí

Odpověď na probíhá v tomto formátu: 

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Získat přístup pouze aplikace token pro správce prostředků Azure

Ověření této aplikace obsahují požadované přístup k předplatným, provádět test předplatného pomocí tokenu jen aplikace.

Získat jen aplikace access tokenu, postupujte podle pokynů z části [dostat jen aplikace tokenu pro rozhraní API Azure AD grafu](#app-azure-ad-graph), s jinou hodnotu parametru zdroje: 

    https://management.core.windows.net/

Metoda [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) vzorové aplikace ASP.NET MVC získá jen aplikace access tokenu pro správce prostředků Azure pomocí Active Directory Authentication Library pro .net.

#### <a name="get-applications-permissions-on-subscription"></a>Získejte oprávnění aplikace předplatného

Ověřte, zda aplikace požadovaný přístup na předplatném Azure, mohou také zavoláte [Oprávnění správce prostředků](https://msdn.microsoft.com/library/azure/dn906889.aspx) rozhraní API. Tento postup je podobný jak zjistili, jestli má uživatel oprávnění Správa přístupu pro předplatné. Ale tentokrát, zavolejte na linku oprávnění rozhraní API tokenu jen aplikace access, kterou jste dostali v předchozím kroku.

Metoda [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) aplikace ukázkové ASP.NET MVC provádí volání.

## <a name="manage-connected-subscriptions"></a>Správa připojení předplatná

Je-li odpovídající roli RBAC přiřazen aplikace služby základní u předplatného, aplikace můžete provádět sledování/správu pomocí tokeny jen aplikace access pro správce prostředků Azure.

Pokud vlastník předplatného odebere přiřazování rolí aplikace pomocí klasického portál nebo nástroje příkazového řádku, aplikace už není mít přístup k této předplatného. V takovém případě by měl upozornit na uživatele, který byl připojení k předplatnému rozděleny z mimo aplikaci a dát jim možnost "Opravit" připojení. "Opravit" by jednoduše opětovné vytvoření offline odstraněného přiřazování rolí.

Stejně jako povolit uživatelům připojení předplatná aplikaci musí povolit uživatelům příliš odpojit předplatná. Z aplikace access správy hlediska odpojte prostředky odebráním aplikace služby jistinu s předplatným přiřazování rolí. Volitelně můžete všechny stavu v aplikaci pro předplatné může odeberou příliš. Pouze uživatelé s oprávněním řízení přístupu u předplatného se odpojit předplatné.

[Metoda RevokeRoleFromServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) aplikace ukázkové ASP.net MVC provádí volání.

To je vše: uživatelé teď můžete snadno připojit a spravovat své Azure předplatná s aplikací.

