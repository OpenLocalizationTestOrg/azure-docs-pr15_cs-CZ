<properties
    pageTitle="Azure Active Directory přihlašovací aktivity sestavy rozhraní API ukázky | Microsoft Azure"
    description="Jak začít s Azure Active Directory vykazování API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory přihlašovací aktivity sestavy rozhraní API vzorky

Toto téma je součástí sady témata nápovědy Azure Active Directory vykazování rozhraní API.  
Vytváření sestav Azure AD poskytuje rozhraní API, které umožňuje přístup k aktivity přihlašovací údaje pomocí kódu nebo související nástroje.  
Toto téma je umožňují ukázkový kód pro **přihlašovací aktivity rozhraní API**.

Najdete tady:

- Další informace [protokolů auditování](active-directory-reporting-azure-portal.md#audit-logs)
- [Začínáme s Azure Active Directory vykazování API](active-directory-reporting-api-getting-started.md) Další informace o rozhraní API pro vytváření sestav.

Otázky, problémy nebo zpětnou vazbu obraťte se prosím [AAD vykazování Nápověda](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Zjistit předpoklady pro
Než budete moct použít vzorky v tomto tématu, budete muset dokončení [požadavky pro přístup k rozhraní API sestav Azure AD](active-directory-reporting-api-prerequisites.md).  


##<a name="powershell-script"></a>Skript Powershellu

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Skriptu
Po dokončení úprav skript, ho spusťte a ověřte, že očekávaná data z auditování protokoly sestavy vrátí se hodnota.

Skript vrátí výstup ze sestavy přihlašovací ve formátu JSON. Vytváří také `SigninActivities.json` soubor se stejným výstupu. Můžete experimentovat změnou skript, který chcete vrátit data z jiných sestav a komentáře se výstupních formátů, které nepotřebujete.



## <a name="next-steps"></a>Další kroky

- Chcete přizpůsobit příklady v tomto tématu? Podívejte se na [Azure Active Directory přihlašovací aktivity rozhraní API odkaz](active-directory-reporting-api-sign-in-activity-reference.md). 

- Pokud chcete zobrazit úplný přehled použití Azure Active Directory vykazování rozhraní API, přečtěte si článek [Začínáme se službou Azure Active Directory vykazování rozhraní API](active-directory-reporting-api-getting-started.md).

- Pokud chcete zjistit víc informací o vytváření sestav služby Azure Active Directory, najdete v článku [Azure Active Directory vykazování Průvodce](active-directory-reporting-guide.md).  