<properties
    pageTitle="Sestava přihlašovací aktivit Azure Active Directory rozhraní API odkaz | Microsoft Azure"
    description="Odkaz pro rozhraní API sestav přihlašovací aktivity Azure Active Directory"
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

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Sestava přihlašovací aktivit Azure Active Directory rozhraní API odkaz


Toto téma je součástí sady témata nápovědy Azure Active Directory vykazování rozhraní API.  
Vytváření sestav Azure AD poskytuje rozhraní API, které umožňuje přístup k přihlášení aktivity dat sestavy s použitím kódu nebo související nástroje.
Toto téma je poskytnout referenční informace o **rozhraní API sestav aktivity přihlásit**.

Najdete tady:

- [Přihlašovací aktivity](active-directory-reporting-azure-portal.md#sign-in-activities) podrobnější informace
- [Začínáme s Azure Active Directory vykazování API](active-directory-reporting-api-getting-started.md) Další informace o rozhraní API pro vytváření sestav.

Otázky, problémy nebo zpětnou vazbu obraťte se prosím [AAD vykazování Nápověda](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Kdo má přístup k rozhraní API data?

- Uživatelé v roli správce zabezpečení nebo zabezpečení Readeru

- Globální správci

- Libovolné aplikaci, která má oprávnění pro přístup k rozhraní API (povolení aplikace může být nastavení jen na základě oprávnění globální správce)



## <a name="prerequisites"></a>Zjistit předpoklady pro

Přístup k této sestavě pomocí rozhraní API sestav, musíte mít:

- S [Azure Active Directory Premium P1 nebo P2 edition](active-directory-editions.md)

- [Požadavky pro přístup k rozhraní API sestav Azure AD](active-directory-reporting-api-prerequisites.md)dokončit. 


##<a name="accessing-the-api"></a>Přístup k rozhraní API

Buď můžete využít tento rozhraní API pomocí [Průzkumníka grafu](https://graphexplorer2.cloudapp.net) nebo programově například pomocí prostředí PowerShell. Aby prostředí PowerShell správně interpretovat syntaxe filtr OData používané v AAD grafu ZBÝVAJÍCÍ hovory, je nutné použít backtick (označovaná taky jako: čárka) znaku "řídicí" $ znak. Znak backtick slouží jako [prostředí PowerShell na řídicí znak](https://technet.microsoft.com/library/hh847755.aspx), povolení prostředí PowerShell na literál výklad znaku $ a vyhněte se matoucí jako název proměnné prostředí PowerShell (ie: $filter).

V podokně grafu je fokus v tomto tématu. Jako příklad Powershellu najdete v článku tento [skript Powershellu](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).


## <a name="api-endpoint"></a>Rozhraní API koncový bod

Dostanete tento rozhraní API pomocí následující základní URI:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Z důvodu objemu dat má toto rozhraní API limit jednoho milionu vrácené záznamy. 

Volání vrátí data v listech. Každý dávku má maximálně 1 000 záznamů.  
Další dávku záznamy, použijte následující odkaz. Získáte informace [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) z první sadě vrácené záznamy. Token přeskočit na konci výsledek nastaví se.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Podporované filtrů

Můžete omezit počet záznamy, které jsou vráceny pomocí rozhraní API volání v podobě filtru.  
Rozhraní API přihlašovací souvisejících dat, jsou podporovány následující filtry:

- **$top =\<počet vrácených\> ** : Chcete-li omezit počet vrácených záznamů. Toto je drahé operaci. Pokud chcete vrátit tisíce objekty byste neměli používat tento filtr.  
- **$filter =\<filtrování údajů\> ** - určete na základě pole podporované filtru vybraného typu záznamů zajímavého



## <a name="supported-filter-fields-and-operators"></a>Pole podporované filtru a operátorů

Chcete-li určit typ záznamů, které vám záleží, je možné vytvářet výrazu filtru obsahující jednu nebo kombinací následující pole filtru:

- [signinDateTime](#signindatetime) - definuje datum nebo rozsah kalendářních dat

- [ID](#userid) – definuje konkrétní uživatele na základě ID uživatele.

- [userPrincipalName](#userprincipalname) - definuje konkrétní uživatele na základě hlavní jméno uživatele uživatele (UPN)

- [ID aplikace](#appid) - definuje konkrétní aplikace založené aplikaci ID

- [appDisplayName](#appdisplayname) - definuje konkrétní aplikace založené aplikaci zobrazované jméno

- [loginStatus](#loginStatus) - definuje stav přihlášení systému (úspěch nebo selhání)


> [AZURE.NOTE] Pomocí Průzkumníka grafu, je potřeba používat správnou velikost všech písem při pole filtru.


Chcete-li zúžit obor vrácená data, je možné vytvářet kombinace podporovaných filtrů a polí filtrů. Například následující výraz vrátí horním 10 záznamy mezi 1. července 2016 a 6 2016 dne:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Podporované operátory**: eq, stránku, le, gt, lt

**Příklad**:

Použití konkrétní den

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Použití rozsah kalendářních dat    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Poznámky**:

Parametr data a času by měl být ve formátu UTC 


----------

### <a name="userid"></a>ID uživatele

**Podporované operátory**: eq

**Příklad**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Poznámky**:

ID uživatele má hodnotu hodnotu řetězce



----------

### <a name="userprincipalname"></a>userPrincipalName

**Podporované operátory**: eq

**Příklad**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Poznámky**:

UserPrincipalName hodnotu hodnotu řetězce

----------

### <a name="appid"></a>ID aplikace

**Podporované operátory**: eq

**Příklad**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Poznámky**:

ID aplikace hodnotu hodnotu řetězce

----------


### <a name="appdisplayname"></a>appDisplayName

**Podporované operátory**: eq

**Příklad**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Poznámky**:

AppDisplayName hodnotu hodnotu řetězce

----------

### <a name="loginstatus"></a>loginStatus

**Podporované operátory**: eq

**Příklad**:

    $filter=loginStatus+eq+'1'  


**Poznámky**:

Existují dva způsoby loginStatus: 0 - Úspěch, 1 - selhání

----------



## <a name="next-steps"></a>Další kroky

- Chcete příklady za filtrované přihlásit? Podívejte se na [ukázky sestavy rozhraní API přihlašovací aktivity Azure Active Directory](active-directory-reporting-api-sign-in-activity-samples.md).

- Chcete se dozvědět víc o Azure AD vykazování rozhraní API? Najdete v článku [Začínáme s Azure Active Directory vykazování API](active-directory-reporting-api-getting-started.md).