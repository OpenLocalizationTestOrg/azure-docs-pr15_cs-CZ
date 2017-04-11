<properties
    pageTitle="Azure podmíněné přístup k aplikacím SaaS | Microsoft Azure"
    description="Podmíněné přístupu v Azure AD umožňuje konfigurace pravidla přístupu jednotlivé aplikace vícefaktorové ověřování a možnost zablokování přístupu pro uživatele není v síti důvěryhodné. "
    services="active-directory"
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
    ms.date="09/26/2016"
    ms.author="markvi"/>

# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Začínáme s Azure Active Directory podmíněné aplikace Access

Azure Active Directory podmíněné a přístup k [SaaS](https://azure.microsoft.com/overview/what-is-saas/) aplikací Azure AD připojení aplikace umožňuje konfigurace podmíněného přístupu na základě skupiny, umístění a utajení aplikace. 

Podle aplikace utajení podmíněné přístup můžete nastavit pravidla přístupu vícefaktorové ověřování (MFA) na aplikaci. MFA jedné aplikace umožňuje zablokování přístupu pro uživatele, kteří nejsou v síti důvěryhodné. Použít pravidla MFA pro všechny uživatele, které jsou přiřazené k aplikaci nebo jenom uživatelům v rámci určité skupiny zabezpečení.  Uživatelé můžou vyloučené ze požadovaného MFA, pokud přistupují aplikace z IP adresy, která je uvnitř síti organizace.

Tyto funkce bude k dispozici pro zákazníky, které jste zakoupili Azure Active Directory Premium licenci.

## <a name="scenario-prerequisites"></a>Scénář požadavky
* Licence pro Premium Azure Active Directory

* Federované nebo spravované klienta služby Azure Active Directory

* Federované klienti je nutné povolit tento vícefaktorové ověřování.

## <a name="configure-per-application-access-rules"></a>Konfigurace pravidel na aplikaci access

Tato část popisuje, jak konfigurovat pravidla jednotlivé aplikace access.

1. Přihlaste se k portálu Azure klasické pomocí účtu, který slouží jako globální správce pro Azure AD.
2. V levém podokně vyberte **Služby Active Directory**.
3. Na kartě adresáře vyberte adresář.
4. Vyberte kartu **aplikace** .
5. Vyberte aplikaci, která pravidla nastaví pro.
6. Vyberte kartu **Konfigurovat** .
7. Přejděte dolů do části pravidla přístupu. Vyberte pravidlo požadovaný přístup.
8. Určení uživatelů, které pravidla platí pro.
9. Výběrem **povoleno na**povolte.

##<a name="understanding-access-rules"></a>Principy pravidel aplikace access

Tato část nabízí podrobný popis pravidla přístupu podporované v aplikaci Access podmíněné Azure.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Určení uživatelů přístup použijí pravidla

Ve výchozím nastavení zásad platí pro všechny uživatele, kteří mají přístup k aplikaci. Zásady však můžete omezit uživatelů, kteří patří určité skupiny zabezpečení. Tlačítko **Přidat skupinu** slouží k vyberte jednu nebo více skupin požadované pravidla pro přístup k dialog pro výběr skupiny. Toto dialogové okno lze také odstranit vybrané skupiny. Pravidla vybráno vyrovnat skupiny, pravidla přístupu budou vynucené jenom pro uživatele, kteří patří do jedné ze zadaných skupin zabezpečení.

Skupiny zabezpečení mohou být také explicitně vyloučeny ze zásad vybrat možnost **kromě** a zadáním jednu nebo více skupin. I když jsou členem skupiny, který access pravidla platí pro uživatele, kteří jsou členem skupiny v seznamu **s výjimkou** nebudou vyměřené poplatky za jeho požadovaného vícefaktorové ověřování.
Pravidla pro přístup vidíte na obrázku budou všichni uživatelé ve skupině správců používat vícefaktorové ověřování při přístupu k aplikaci.

![Nastavení pravidel podmíněného přístupu s MFA](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Pravidla podmíněného přístupu se MFA
Pokud jsou nakonfigurovány uživatele pomocí funkce vícefaktorové ověřování uživatelů, bude toto nastavení na uživatele kombinovat s pravidly vícefaktorové ověřování aplikace. To znamená, že uživatel, který má nakonfigurovaný pro jednotlivé uživatele vícefaktorové ověřování budou muset provést vícefaktorové ověřování i v případě, že budou mít vyňatých z pravidel vícefaktorové ověřování aplikace. Další informace o nastavení Multi-Factor authentication i pro jednotlivé uživatele.

### <a name="access-rule-options"></a>Možnosti aplikace Access pravidla
Jsou podporovány následující možnosti:

* **Vyžadovat vícefaktorové ověřování**: uživatelů, kterým použít pravidla přístupu, budete vyzváni k dokončení vícefaktorové ověřování před přístupem aplikace, která bude použita zásada k.

* **Vyžadovat vícefaktorové ověřování při není v práci**: uživatel, který pochází z důvěryhodného IP adresy, nemusí provádět vícefaktorové ověřování. Na stránce nastavení vícefaktorové ověřování je možné konfigurovat důvěryhodných rozsahy IP adres.

* **Zablokování přístupu při není v práci**: uživatel, který není pocházejících z důvěryhodného IP adresu, budou blokovány. Na stránce nastavení vícefaktorové ověřování je možné konfigurovat důvěryhodných rozsahy IP adres.

### <a name="setting-rule-status"></a>Nastavení pravidla stavu
Stav pravidla přístupu umožňuje zapínat a vypínat pravidla. Jakmile pravidla přístupu vypnout, není nevynucují požadovaného vícefaktorové ověřování.

### <a name="access-rule-evaluation"></a>Vyhodnocování pravidel aplikace Access

Pravidla přístupu jsou vyhodnoceny, když uživatel federované aplikace, která používá OAuth 2.0, OpenID připojení, SAML nebo WS Federation. Kromě toho pravidla přístupu jsou vyhodnoceny, použijete OAuth 2.0 a připojte OpenID token aktualizace získat přístupový token. Pokud hodnocení zásad selhává, když se používá token aktualizace, bude vrácena chybová **invalid_grant** , to znamená, že uživatel musí znovu ověřit klientovi.

###<a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Konfigurace federace službám mohl vám poskytovat vícefaktorové ověřování

U federovaného klienti lze provádět MFA Azure Active Directory, nebo místní server AD FS.

Ve výchozím nastavení se projeví MFA na stránku hostitelem služby Azure Active Directory. Abyste mohli nakonfigurovat MFA místní, vlastnost **– SupportsMFA** musí nastavit **PRAVDA** v Azure Active Directory, pomocí modulu Azure AD pro Windows PowerShell.

Následující příklad ukazuje, jak povolit místní MFA pomocí [rutiny Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) na klientovi contoso.com:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Kromě nastavení tento příznak, musí být nakonfigurované instance federované klienta AD FS provádět vícefaktorové ověřování. Postupujte podle pokynů pro [nasazení Azure Vícefaktorové ověřování místní](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Související články

- [Zabezpečení přístupu k Office 365 a dalších aplikací připojení pro službu Azure Active Directory](active-directory-conditional-access.md)
- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
