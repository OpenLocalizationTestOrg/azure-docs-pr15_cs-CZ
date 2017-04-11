<properties
    pageTitle="Začínáme s Azure Active Directory Identity Protection a Microsoft Graph | Microsoft Azure"
    description="Poskytuje úvod do dotazů aplikace Microsoft Graph seznam události rizika a související informace ze služby Azure Active Directory."
    services="active-directory"
    keywords="Ochrana identity služby Azure active directory, riziková událost, chyba, zásady zabezpečení aplikace Microsoft Graph"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Začínáme s Azure Active Directory Identity Protection a Microsoft Graph

Aplikace Microsoft Graph je společnosti Microsoft jednotné koncový bod rozhraní API a na domovské stránce [Azure Active Directory ochrana Identity prvku](active-directory-identityprotection.md) rozhraní API. Náš první rozhraní API **identityRiskEvents**umožňuje dotazu aplikace Microsoft Graph seznam [události rizika](active-directory-identityprotection-risk-events-types.md) a související informace. Tento článek vám pomůže začít dotazování toto rozhraní API. Pro důkladné úvod, si přečtěte následující dokumentaci úplné a přístup k Explorer graf najdete v článku [Microsoft Graph webu](https://graph.microsoft.io/).

Existují tři kroky pro přístup k datům ochrana Identity prostřednictvím aplikace Microsoft Graph:

1. Přidání aplikace s tajná klienta. 

2. Pomocí této tajná a několika dalších informací k ověření do aplikace Microsoft Graph, které se vám posílají token ověřování. 

3. Použijte tento token k vytvoření žádosti o koncový bod rozhraní API a vrátit ochrana Identity data.

Než začnete, musíte:

- Vytvoření aplikace v Azure AD oprávnění správce
- Název domény vašeho klienta (například contoso.onmicrosoft.com)



## <a name="add-an-application-with-a-client-secret"></a>Přidání aplikace s tajná klienta


1. [Přihlaste se](https://manage.windowsazure.com) k portálu Azure klasické jako správce. 

1. Na v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. V nabídce na horní klikněte na **aplikace**.

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. Klikněte na **Přidat** v dolní části stránky.

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. V dialogovém okně **Co chcete udělat** klikněte na **Přidat aplikaci, které vyvíjí mé organizaci**.

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. V dialogovém okně **námi o aplikaci** proveďte následující kroky:

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    na. Do textového pole **název** zadejte název aplikace (například: AADIP riziková událost rozhraní API aplikace).

    b. Jako **Typ**vyberte **webového rozhraní API aplikace a/nebo Web**.

    c. Klikněte na tlačítko **Další**.


5. V dialogovém okně **Vlastnosti aplikace** proveďte následující kroky:

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    na. Do textového pole **Přihlašovací adresa URL** zadejte `http://localhost`.

    b. Do textového pole **Identifikátor URI ID aplikace** zadejte `http://localhost`.

    c. Klikněte na **dokončení**.


Teď můžete nakonfigurovat aplikaci.

![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>Udělte oprávnění aplikace můžete rozhraní API


1. Na stránce vaše aplikace klikněte v nabídce v horní části klikněte na **Konfigurovat**. 

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. V části **oprávnění do jiných aplikací** klikněte na **Přidat aplikaci**.

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. V dialogovém okně **oprávnění do jiných aplikací** proveďte následující kroky:

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    na. Vyberte **Microsoft Graph**.

    b. Klikněte na **dokončení**.


1. Klikněte na **oprávnění aplikace: 0**a pak vyberte **přečíst informace o událostech rizika všechny identity**.

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. Klepněte na tlačítko **Uložit** v dolní části stránky.

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>Získání přístupová klávesa

1. Na stránce vaše aplikace vyberte v části **klíče** jeden rok jako doba trvání.

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. Klepněte na tlačítko **Uložit** v dolní části stránky.

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. v části klíče zkopírujte hodnotu nově vytvořený klíč a potom je vložte do bezpečí umístění.

    ![Vytvoření aplikace](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] Pokud zapomenete tento klíč, budete muset vraťte se na tento oddíl a vytvořte nový klíč. Zachovat tento klíč tajná: kdokoli, kdo má přístup ke svým datům.


1. V části **Vlastnosti** zkopírujte **Kód klienta**a potom je vložte do bezpečné místo. 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Ověření do aplikace Microsoft Graph a dotaz rozhraní API Identity rizika události

V tomto okamžiku byste si měli:

- ID klienta zkopírovaný z mapové

- Zkopírovaný z mapové klíč

- Název domény vašeho klienta


Ověření, pošlete žádost příspěvek `https://login.microsoft.com` pomocí následujících parametrů v textu:

- grant_type: "**client_credentials**"

- zdroje: "**https://graph.microsoft.com**"

- client_id:<your client ID>

- client_secret:<your key>


> [AZURE.NOTE] Budete muset zadat hodnoty pro **client_id** a parametr **client_secret** .

Pokud je úspěšná, vrátí token ověřování.  
Volání rozhraní API, vytvořte záhlaví s parametrem takto:

    `Authorization`=”<token_type> <access_token>"


Při ověřování, můžete najít typ tokenu a přístupový token v vrácené token.

Toto záhlaví na odešlete jako žádost o následující adresu URL rozhraní API:`https://graph.microsoft.com/beta/identityRiskEvents`

Odpověď, pokud je úspěšná, najdete identity rizika událostí a Spojenými daty ve formátu OData JSON, který lze analyzovat a zpracována jako najdete v článku přizpůsobení.

Tady je ukázka kód pro ověřování a volání rozhraní API pomocí Powershellu.  
Přidejte svoje ID klienta klíče a tenant domény.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Další kroky

Gratulujeme, právě udělali první hovoru do aplikace Microsoft Graph!  
Teď můžete vyhledat identity rizikové události a využívá data, ale najdete v článku přizpůsobení.

Další informace o aplikaci Microsoft Graph a k vytváření aplikací pomocí rozhraní API grafu, zkontrolujte si [přečtěte následující dokumentaci](https://graph.microsoft.io/docs) a mnohem víc na [webu Microsoft Graph](https://graph.microsoft.io/). Nezapomeňte si záložku na stránce [Azure AD identit ochrany rozhraní API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) , který obsahuje seznam všech rozhraní API ochrany identit k dispozici v grafu. Jak můžeme přidat nové způsoby práce s ochranou identit prostřednictvím rozhraní API, uvidíte je na této stránce.


## <a name="additional-resources"></a>Další zdroje informací

- [Ochrana Identity Azure Active Directory](active-directory-identityprotection.md)

- [Typy rizika událostí nerozpoznal ochranou Azure Active Directory Identity](active-directory-identityprotection-risk-events-types.md)

- [Aplikace Microsoft Graph](https://graph.microsoft.io/)

- [Základní informace o aplikaci Microsoft Graph](https://graph.microsoft.io/docs)

- [Kořen služby Azure AD Identity ochrany](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)
