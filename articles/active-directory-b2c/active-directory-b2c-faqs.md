<properties
    pageTitle="B2C Azure Active Directory: Nejčastější dotazy týkající se | Microsoft Azure"
    description="Nejčastější dotazy k Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>Azure Active Directory B2C: Časté otázky

Na této stránce najdete odpovědi na nejčastější dotazy týkající se B2C Azure Active Directory (Azure AD). Zachování kontroly další aktualizace.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Můžete použít funkce Azure AD B2C v našem klientovi Azure AD existující, na základě zaměstnance?

Aktuálně Azure AD B2C funkce nemůže být povolené v stávajícímu klientovi Azure AD. Doporučujeme vytvoření samostatného klienta použití funkce Azure AD B2C ke správě uživatele.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Dá se pomocí Azure AD B2C poskytují sociální login (Facebook a Google +) do Office 365?

Azure AD B2C nelze použít v aplikaci Microsoft Office 365. Obecně ji nelze přihlašovacích žádné SaaS aplikace (Office 365 služby Salesforce, pracovního dne, atd.). Poskytuje správy identit a přístup jenom pro web vystavený příjemce a mobilní aplikace a není k dispozici zaměstnance nebo partnera scénáře.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Jaké jsou místní účty v Azure AD B2C? Jak se budou liší od pracovní nebo školní účty v Azure AD?

V klientovi Azure AD každý uživatel v klientovi (s výjimkou uživatelů s existujícími Microsoft účty) přihlásí pomocí e-mailovou adresu ve formuláři `<xyz>@<tenant domain>`, kde `<tenant domain>` je jedním ze ověřených domén v klientovi nebo počáteční `<...>.onmicrosoft.com` domény. Tento typ účtu je pracovního nebo školního účtu.

V klientovi Azure AD B2C většiny aplikací má uživatel Přihlaste se pomocí libovolné libovolné e-mailovou adresu (například joe@comcast.net, bob@gmail.com, sarah@contoso.com, nebo jim@live.com). Tento typ účtu je místního účtu. V současné době taky podporujeme libovolného uživatelská jména (jenom prostý řetězce) jako místní účty (například Jana, Jan, sarah nebo jim). Můžete vybrat jednu z těchto dvou typů místního účtu služby Azure AD B2C.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Které poskytovatelé sociální identity je podporují teď? Které z nich plánujete v budoucnu podporuje?

Momentálně podporujeme Facebook Google +, Linkedinu a Amazon. Přidáme podpora jinými poskytovateli Oblíbené sociální identity podle zákaznická služba.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Můžete nakonfigurovat obory můžete získat další informace o které se zobrazují koncovým z různých poskytovatelé sociální identity?

Ne, ale tato funkce je na naše Přehled. Výchozí obory pro naše podporované sadu poskytovatelé sociální identity jsou:

- Facebooku: e-mailu
- Google +: e-mailu
- Účet Microsoft: openid e-mailový profil
- Amazon: profilu
- LinkedIn: r_emailaddress r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Má se mají v Azure pro práci s Azure AD B2C aplikace?

Ne, budete moct hostovat aplikace kdekoli (v cloudu nebo místní). Nutnou k interakci s Azure AD B2C stačí možnost odesílat a přijímat HTTP žádosti o veřejně přístupný koncové body.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Mám víc klientů Azure AD B2C. Jak mohu spravovat je na portálu Azure?

Každého klienta Azure AD B2C obsahuje vlastní zásuvné funkce B2C na portálu Azure. V tématu [Azure AD B2C: registrace aplikace](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) se dozvíte, jak můžete přejít na konkrétní klienta B2C funkce zásuvné na portálu Azure. Přepínání mezi Azure AD B2C adresáře na portálu Azure nebude mít B2C funkce zásuvné otevřít ve většině prohlížečů.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Jak přizpůsobit ověření e-mailů (obsah a "od:" pole) odeslaný Azure AD B2C?

[Funkce brandingu společnosti](../active-directory/active-directory-add-company-branding.md) umožňuje přizpůsobit obsah ověření e-mailů. Konkrétně můžete přizpůsobit tyto dva prvky e-mailu:

- **Nápis Logo**: filtr v pravém dolním.
- **Barva pozadí**: filtr v horní.

    ![Snímek obrazovky přizpůsobeného ověřovací e-mail](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Podpis e-mailu obsahuje název B2C klienta, kterou jste použili při prvním vytvoření B2C klienta. Můžete změnit název pomocí následujících pokynů:

- Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com/) jako správce předplatného.
- Přejděte do vašeho tenanta B2C.
- Klikněte na kartu **Konfigurovat** .
- Změňte **název** pole v části **Vlastnosti adresáře** .
- Klepněte na tlačítko **Uložit** v dolní části stránky.

Současné době nejde žádným způsobem změnit "od:" v e-mailu. Pokud vás zajímá, v této možnosti a plně přizpůsobení textu ověřovací e-mail, Hlasujte pro funkci na [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails).

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Jak lze budu migrovat Moje existující uživatelská jména hesla a profily z databáze aplikace na Azure AD B2C?

Rozhraní API Azure AD grafu můžete použít k zápisu nástroje pro migraci. V tématu [Ukázkový graf API](active-directory-b2c-devquickstarts-graph-dotnet.md) podrobnosti. Poskytneme různé možnosti migrace a nástroje mimo předdefinovaných v budoucnu.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Jaké zásady pro heslo se používá pro místní účty v Azure AD B2C?

Zásady hesel Azure AD B2C pro místní účty je na základě zásad pro Azure AD. Azure AD B2C je registrace, registrace nebo přihlašovací heslo resetovat "" silné heslo: Zkontrolujte sílu zásady použití a nevyprší všechna hesla. Čtěte [zásady pro heslo Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) další podrobnosti.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Dá se pomocí Azure AD Connect migrace identit příjemce, které jsou uložené na svůj místní služby Active Directory do Azure AD B2C?

Ne, Azure AD Connect není navržený pro práci s Azure AD B2C. Poskytneme různé možnosti migrace a nástroje mimo předdefinovaných v budoucnu.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Funguje Azure AD B2C systémů třeba Microsoft Dynamics CRM?

Nesynchronizujete. Integrace tyto systémy zapnuté naše Přehled.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Znamená Azure AD B2C pracovat s místním Sharepointu 2016 nebo starším?

Aktuálně. Azure AD B2C nemá podpora SAML 1.1 klíčovými portály a e obchodními aplikacemi založená na SharePoint v místním potřeb. Všimněte si, že Azure AD B2C není určen k Sharepointu externí sdílení partnera scénáře; Místo toho zobrazí [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) .

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Mám používat Azure AD B2C nebo B2B ke správě externího identity?

V tomto článku o [externí identit](../active-directory/active-directory-b2b-compare-external-identities.md) zobrazíte další informace o použití funkce odpovídající externí identity scénáře.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>K čemu vytváření sestav a auditování funkce poskytuje Azure AD B2C? Jsou stejné jako v Azure AD Premium?

Ne, Azure AD B2C podporuje stejné nastavení sestavy jako Azure AD Premium. Azure AD B2C bude dodání, základní vytváření sestav a auditování rozhraní API brzy bude k dispozici.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Můžete localize uživatelském rozhraní stránek hodit Azure AD B2C? Jaké jazyky podporuje?

V současné době Azure AD B2C je optimalizována pro angličtinu pouze. Plánujeme zavedením lokalizace funkce co nejdříve.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Můžete použít vlastní adresy URL na stránkách registrace přihlašování a, které se spouští Azure AD B2C? Například můžete změnit adresu URL z login.microsoftonline.com login.contoso.com?

Nesynchronizujete. Tato funkce je na naše Přehled. Navíc nezapomeňte, že ověření vaší domény na kartě **Domains** vašeho klienta na portálu Azure klasické nebude udělejte toto.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Jak odstranit Azure AD B2C klienta?

Tímto postupem odstranit Azure AD B2C klienta:

- Postupujte podle těchto kroků [přejděte na zásuvné funkce B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) na portálu Azure.
- Přejděte na zásuvné moduly **aplikací**, **Zprostředkovatelé identit jiní** a **všechny zásady** a odstraňte všechny položky v každém z nich.
- Teď se přihlaste k [Azure klasické portál](https://manage.windowsazure.com/) jako správce předplatného. (Toto je stejný pracovní nebo školní účet nebo stejný účet Microsoft, který jste použili k registraci Azure.)
- Přejděte na služby Active Directory linku na levé straně a klikněte na B2C klienta.
- Klikněte na kartu **uživatelů** .
- Vyberte každého uživatele v zapnout (vyloučit uživatele, kterého jste právě přihlášení jako, tedy předplatné správce). Klikněte na příkaz **Odstranit** v dolní části stránky a klikněte na tlačítko **Ano** po zobrazení výzvy.
- Klikněte na kartu **aplikace** .
- Vyberte **aplikace vlastní naší společnosti** v poli **Zobrazit** rozevírací seznam a klikněte na značku zaškrtnutí.
- Zobrazí se aplikace nazvané **b2c rozšíření aplikace** uvedené níže. Klikněte na příkaz **Odstranit** v dolní části stránky a klikněte na tlačítko **Ano** po zobrazení výzvy.
- Přejděte na linku služby Active Directory znovu a vyberte B2C klienta.
- Klepněte na příkaz **Odstranit** v dolní části stránky. Postupujte podle pokynů na obrazovce dokončete proces.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Můžete získat Azure AD B2C jako součást sady mobilita Enterprise?

Ne, Azure AD B2C je přislíbený Azure služby a není součástí sady mobilita organizace.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Jak mohu ohlásit problémy s Azure AD B2C?

[Soubor podporují požadavky pro Azure Active Directory B2C](active-directory-b2c-support.md)vidět.

## <a name="more-information"></a>Další informace

Můžete taky chtít: kontrolujte potvrzení aktuální [omezení služeb, omezení a omezení](active-directory-b2c-limitations.md).
