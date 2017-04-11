<properties
    pageTitle="Podmíněné přístup k aplikacím publikovaných pomocí proxy server Azure AD aplikace"
    description="Popisuje, jak nastavit podmíněné přístup k aplikacím, které publikujete ve službě ke kterému jde přistupovat vzdáleně používat proxy server Azure AD aplikace."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-conditional-access"></a>Práce s podmíněným přístupem

Můžete nakonfigurovat pravidla přístupu udělit podmíněné přístup k aplikacím publikovaných pomocí aplikace Proxy. To umožňuje:

- Vyžadovat vícefaktorové ověřování za aplikace
- Vyžadovat vícefaktorové ověřování pouze v případě, že uživatelé nejsou v praxi
- Zabránit uživatelům v přístupu k aplikace, které nejsou v práci

Tato pravidla se dají použít u všech uživatelů a skupin nebo pouze určitým uživatelům nebo skupinám. Ve výchozím nastavení se všem uživatelům, kteří mají přístup k aplikaci použít pravidlo. Pravidlo však můžete taky omezena uživatelům, kteří jsou členy určité skupiny zabezpečení.  

Pravidla přístupu jsou vyhodnoceny, když uživatel federované aplikace, která používá OAuth 2.0, OpenID připojení, SAML nebo WS Federation. Kromě toho pravidla přístupu jsou vyhodnoceny s OAuth 2.0 a připojte OpenID při aktualizaci token slouží k získání přístupový token.

## <a name="conditional-access-prerequisites"></a>Požadavky na podmíněného přístupu

- Předplatné Premium Azure Active Directory
- Federované nebo spravované klienta služby Azure Active Directory
- Federované klienti vyžadují povolit tento vícefaktorové ověřování (MFA)  
    ![Konfigurace pravidla přístupu – vyžadují vícefaktorové ověřování](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Konfigurace jednotlivé aplikace vícefaktorové ověřování
1. Přihlaste se jako správce na portálu Azure klasické.
2. Přejděte ke službě Active Directory a vyberte složku, ve kterém chcete povolit aplikaci Proxy.
3. Klikněte na **aplikace** a posuňte se dolů k oddílu **Pravidla přístupu** . Části aplikace access pravidla se zobrazí jenom pro aplikace publikována Proxy aplikace, které používají federované ověřování.
4. Povolte pravidlo tak, že vyberete **Povolit pravidla přístupu** na **zapnuto**.
5. Určení uživatelů a skupin, kterým se použijí pravidla. Chcete-li vybrat jeden nebo více skupin, ke kterým se použít pravidla pro přístup pomocí tlačítka **Přidat skupinu** . Toto dialogové okno lze také odstranit vybrané skupiny.  Pravidla vybráno vyrovnat skupiny, pravidla přístupu budou vynucené jenom pro uživatele, kteří patří do jedné ze zadaných skupin zabezpečení.  

  - Skupiny zabezpečení explicitně vyloučit z pravidla, zaškrtněte **kromě** a zadejte jednu nebo více skupin. Uživatelé, kteří patří skupiny v seznamu kromě, nemusí provádět vícefaktorové ověřování.  

  - Pokud uživatel o konfiguraci pomocí funkce vícefaktorové ověřování uživatelů, toto nastavení bude přednost pravidla vícefaktorové ověřování aplikace. To znamená, že uživatel, který má nakonfigurovaný pro jednotlivé uživatele vícefaktorové ověřování budete vyzváni k provedení vícefaktorové ověřování i v případě, že budou mít vyňatých z aplikace vícefaktorové ověřování pravidel. Další informace o [vícefaktorové ověřování a nastavení pro jednotlivé uživatele](../multi-factor-authentication/multi-factor-authentication.md).

6. Vyberte pravidlo přístupu, kterou chcete nastavit:
    - **Vyžadovat vícefaktorové ověřování**: uživatelů, kterým použít pravidla přístupu budete vyzváni k dokončení vícefaktorové ověřování před přístupem aplikace, pro které platí pravidlo.
    - **Vyžadovat vícefaktorové ověřování při není v práci**: uživatelé pokoušející se získat přístup k aplikaci z důvěryhodného IP adresy, nemusí provádět vícefaktorové ověřování. Na stránce nastavení vícefaktorové ověřování je možné konfigurovat důvěryhodných rozsahy IP adres.
    - **Zablokování přístupu při není v práci**: uživatelé pokoušející se získat přístup k aplikaci z mimo vaši podnikovou síť nebudou mít přístup k aplikaci.


## <a name="configuring-mfa-for-federation-services"></a>Konfigurace federace služby MFA
U federovaného klienti lze provádět vícefaktorové ověřování (MFA) Azure Active Directory, nebo místní server AD FS. Ve výchozím nastavení se projeví MFA na kterékoli stránce hostitelem služby Azure Active Directory. Abyste mohli konfigurovat MFA místní, spusťte Windows PowerShell a nastavení modulu Azure AD pomocí vlastnost – SupportsMFA.

Následující příklad ukazuje, jak povolit místní MFA pomocí [rutiny Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) na klientovi contoso.com:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Kromě nastavení tento příznak, musí být nakonfigurované instance federované klienta AD FS provádět vícefaktorové ověřování. Postupujte podle pokynů pro [nasazení Microsoft Azure vícefaktorové ověřování místní](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).


## <a name="see-also"></a>Viz taky

- [Práce s aplikacemi přehled pohledávek](active-directory-application-proxy-claims-aware-apps.md)
- [Publikování aplikací s Proxy aplikace](active-directory-application-proxy-publish.md)
- [Povolit jednotného přihlašování](active-directory-application-proxy-sso-using-kcd.md)
- [Publikování aplikací pomocí vlastního názvu domény](active-directory-application-proxy-custom-domains.md)

Nejnovější příspěvky a aktualizace najdete v článku [aplikace Proxy blogu](http://blogs.technet.com/b/applicationproxyblog/)
