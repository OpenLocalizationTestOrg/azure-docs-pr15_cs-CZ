<properties
    pageTitle="Oznámení hlášení Azure Active Directory"
    description="Použití Azure Active Directory vykazování oznámení pro podezřelé sign in."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Oznámení hlášení Azure Active Directory

## <a name="what-reports-generate-email-notifications"></a>K čemu sestavy generovat e-mailová oznámení

V současné době pouze Nesouměrné přihlásit aktivity sestavy aktivace e-mailové oznámení.

## <a name="what-is-an-irregular-sign-in"></a>Co je "Nesouměrné přihlásit"?

Nepravidelný přihlášení používané identifikovali podle našeho počítače výukové algoritmů na základě podmínku "nelze cestovní" společně s neobvyklých přihlašovací místem konání a zařízení. Může to znamenat, že má byla hacker potíže s přihlášením pomocí tohoto účtu.

## <a name="who-receives-the-email-notifications"></a>Kdo dostane e-mailová oznámení?

E-mail odeslaný do všech globálního správce, kteří přiřadili licenci Active Directory Premium. Abyste měli jistotu, že je Doručená, jsme odesláním správcům alternativní e-mailovou adresu stejně. Správce by měl obsahovat aad-alerts-noreply@mail.windowsazure.com v jejich seznamu bezpečných odesílatelů, budou nezmeškali e-mailu.

## <a name="how-often-are-these-emails-sent"></a>Jak často se tyto e-mailů odesílaných?

E-mailu Pokud 10 nové nepravidelné přihlašovací aktivity dojít v posledních 30 dní a od posledního e-mailu byla odeslána, podle toho, co je menší.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Jak se dostanu ke zprávě uvedené v e-mailu?

Když kliknete na odkaz, přesměrovaný na stránce sestavy v Azure klasické portálu. Pokud chcete získat přístup k sestavě, je třeba obě:

- Správce nebo správce co předplatného Azure

- Globální správce v adresáři a přiřadit licenci Active Directory Premium. Další informace najdete v tématu [edicí Azure Active Directory](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Můžu vypnout tyto e-mailech?

Ano, vypnout upozornění související s neobvyklých přihlášení do portálu Azure klasické, klikněte na **Konfigurovat**a v části **oznámení** klikněte na příkaz **Zakázané** .

## <a name="whats-next"></a>Další krok
- Zajímá vás, jaký zabezpečení, auditování a aktivity sestavy jsou dostupné? Podívejte se na [Azure AD zabezpečení, auditování a sestavy aktivit](active-directory-view-access-usage-reports.md)
- [Začínáme s Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Přidání společnosti branding přihlásit a panely aplikace Access stránky](active-directory-add-company-branding.md)
