<properties
    pageTitle="Azure Active Directory auditování rozhraní API odkaz | Microsoft Azure"
    description="Jak začít s rozhraní API auditování Azure Active Directory"
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
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory auditování rozhraní API odkaz

Toto téma je součástí sady témata nápovědy Azure Active Directory vykazování rozhraní API.  
Vytváření sestav Azure AD poskytuje rozhraní API, které umožňuje přístup k datům auditování použijte kód nebo související nástroje.
Toto téma je poskytnout referenční informace o **auditování rozhraní API**.

Najdete tady:

- Další informace [protokolů auditování](active-directory-reporting-azure-portal.md#audit-logs)
- [Začínáme s Azure Active Directory vykazování API](active-directory-reporting-api-getting-started.md) Další informace o rozhraní API pro vytváření sestav.

Otázky, problémy nebo zpětnou vazbu obraťte se prosím [AAD vykazování Nápověda](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Kdo má přístup k data?

- Uživatelé v roli správce zabezpečení nebo zabezpečení Readeru

- Globální správci

- Libovolné aplikaci, která má oprávnění pro přístup k rozhraní API (povolení aplikace může být nastavení jen na základě oprávnění globálního správce)



## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste získali přístup v této sestavě pomocí rozhraní API sestav, musíte mít:

- S [Azure Active Directory bezplatné nebo lepší edition](active-directory-editions.md)

- [Požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites.md)dokončit. 
 

##<a name="accessing-the-api"></a>Přístup k rozhraní API

Buď můžete využít tento rozhraní API pomocí [Průzkumníka grafu](https://graphexplorer2.cloudapp.net) nebo programově například pomocí prostředí PowerShell. Aby prostředí PowerShell správně interpretovat syntaxe filtr OData používané v AAD grafu ZBÝVAJÍCÍ hovory, je nutné použít backtick (označovaná taky jako: čárka) znaku "řídicí" $ znak. Znak backtick slouží jako [prostředí PowerShell na řídicí znak](https://technet.microsoft.com/library/hh847755.aspx), povolení prostředí PowerShell na literál výklad znaku $ a vyhněte se matoucí jako název proměnné prostředí PowerShell (ie: $filter).

V podokně grafu je fokus v tomto tématu. Jako příklad Powershellu najdete v článku tento [skript Powershellu](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## <a name="api-endpoint"></a>Rozhraní API koncový bod


Dostanete tento rozhraní API pomocí následující URI:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Existuje bez omezení počtu zobrazených záznamů vrácenou rozhraní API auditování Azure AD (pomocí OData stránkování).
Uchovávání informací kvůli omezením dat sestav najdete v článku [Vytváření sestav zásady uchovávání informací](active-directory-reporting-retention.md).

Volání vrátí data v listech. Každý dávku má maximálně 1 000 záznamů.  
Další dávku záznamy, použijte následující odkaz. Získáte informace skiptoken z první sadě vrácené záznamy. Token přeskočit na konci výsledek nastaví se.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Podporované filtrů

Můžete omezit počet záznamů, které se vracejí rozhraní API volání v podobě filtru.  
Rozhraní API přihlašovací související data, jsou podporovány následující filtry:

- **$top =\<počet vrácených\> ** : Chcete-li omezit počet vrácených záznamů. Toto je drahé operaci. Pokud chcete vrátit tisíce objekty byste neměli používat tento filtr.     
- **$filter =\<filtrování údajů\> ** - určete na základě pole podporované filtru vybraného typu záznamů zajímavého



## <a name="supported-filter-fields-and-operators"></a>Pole podporované filtru a operátorů

Chcete-li určit typ záznamů, které vám záleží, je možné vytvářet výrazu filtru obsahující jednu nebo kombinací následující pole filtru:

- [Datum](#activitydate) – definuje datum nebo rozsah kalendářních dat
- [activityType](#activitytype) - definuje typ aktivita
- [aktivity](#activity) – definuje aktivity jako řetězec  
- [objekt actor/název](#actorname) - definuje objekt actor ve formě název objektu actor
- [objekt actor/objektu](#actorobjectid) - definuje objekt actor ve formě ID objektu actor   
- [actor/upn](#actorupn) - definuje objekt actor formou zásady název objektu actor uživatele (UPN) 
- [název cílové /](#targetname) - definuje cíl v podobě název objektu actor
- [cílové/objektu](#targetobjectid) - definuje cíl v podobě cíle ID  
- [cíl/upn](#targetupn) – definuje objekt actor ve formě zásady název objektu actor uživatele (UPN)   




----------

### <a name="activitydate"></a>Datum

**Podporované operátory**: eq, stránku, le, gt, lt

**Příklad**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Poznámky**:

data a času by měl být ve formátu UTC

----------

### <a name="activitytype"></a>activityType

**Podporované operátory**: eq

**Příklad**:

    $filter=activityType eq 'User'  

**Poznámky**:

malá a velká písmena

----------

### <a name="activity"></a>Aktivita

**Podporované operátory**: eq, obsahuje, startsWith

**Příklad**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Poznámky**:

malá a velká písmena

----------

### <a name="actorname"></a>objekt actor/název

**Podporované operátory**: eq, obsahuje, startsWith

**Příklad**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Poznámky**:

velká a malá písmena

    

----------
### <a name="actorobjectid"></a>objekt actor/objektu

**Podporované operátory**: eq

**Příklad**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>Název cílové /

**Podporované operátory**: eq, obsahuje, startsWith

**Příklad**:

    $filter=targets/any(t: t/name eq 'some name')   

**Poznámky**:

Velká a malá písmena

----------

### <a name="targetupn"></a>cíl/upn

**Podporované operátory**: eq, startsWith

**Příklad**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Poznámky**:

- Velká a malá písmena
- Budete muset přidat úplný obor názvů při dotazování Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

----------

### <a name="targetobjectid"></a>ID cílové nebo objektu

**Podporované operátory**: eq

**Příklad**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>objekt actor/upn

**Podporované operátory**: eq, startsWith

**Příklad**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Poznámky**:

- Velká a malá písmena 
- Budete muset přidat úplný obor názvů při dotazování Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

----------




## <a name="next-steps"></a>Další kroky

- Chcete Příklady aktivit filtrované systém? Podívejte se na [Azure Active Directory auditování rozhraní API vzorky](active-directory-reporting-api-audit-samples.md).

- Chcete se dozvědět víc o Azure AD vykazování rozhraní API? Najdete v článku [Začínáme s Azure Active Directory vykazování API](active-directory-reporting-api-getting-started.md).