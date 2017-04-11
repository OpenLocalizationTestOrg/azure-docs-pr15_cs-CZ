<properties
   pageTitle="Azure Active Directory zasílání zpráv o chybách: Začínáme | Microsoft Azure"
   description="Seznam různých dostupných sestav zasílání zpráv o chybách Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="getting-started-with-azure-active-directory-reporting"></a>Začínáme s Azure Active Directory vytváření sestav

## <a name="what-it-is"></a>Co je

Azure Active Directory (Azure AD) obsahuje zabezpečení, aktivity a sestavy auditování pro adresáře. Tady je seznam zahrnutý sestav:

### <a name="security-reports"></a>Zabezpečení sestavy

- Přihlášení z neznámých zdrojů
- Přihlášení za k více chybám
- Přihlášení z různých zeměpisných
- Přihlášení z IP adres pomocí podezřelé aktivity
- Aktivity nepravidelné přihlášení
- Přihlášení z možného infikovaných zařízení
- Uživatelé, kteří mají neobvyklých aktivity přihlášení

### <a name="activity-reports"></a>Sestavy aktivit

- Použití aplikace: shrnutí
- Použití aplikace: podrobné
- Řídicí panel aplikace
- Účet chyby zřizování
- Zařízení pro jednotlivého uživatele
- Aktivity pro jednotlivého uživatele
- Sestava skupin aktivit
- Heslo resetovat registrace aktivity sestavy
- Resetování hesla aktivity

### <a name="audit-reports"></a>Sestavy auditování

- Sestavy auditování adresáře

> [AZURE.TIP] Víc si přečtěte následující dokumentaci na Azure AD zasílání zpráv o chybách najdete v článku [zobrazení sestav aplikace access a použití](active-directory-view-access-usage-reports.md).



## <a name="how-it-works"></a>Jak to funguje


### <a name="reporting-pipeline"></a>Vytváření sestav kanálem k odesílání zpráv

Vytváření sestav příležitost se skládá ze tří hlavních kroků. Pokaždé, když uživatel přihlásí nebo je volání ověřování, stane se toto položky:

- Nejdřív ověření uživatele (úspěšně nebo neúspěšně) a výsledek je uložen v databázích služby Azure Active Directory.
- V pravidelných intervalech, všechny doplňky, které zpracovávají poslední znak. V tomto okamžiku naše zabezpečení a algoritmy neobvyklých aktivity vyhledávání všech poslední sign in podezřelé aktivity.
- Po zpracování zprávy jsou napsané v mezipaměti a podávané množství Azure klasické portálu.

### <a name="report-generation-times"></a>Vykazovat čas generování

Z důvodu velké množství ověřování a podepsat doplňky, které jsou zpracovány službou Azure AD platformu, poslední znak: doplňky, které zpracovávají se průměrně hodinu staré. V některých případech může trvat až 8 hodin zpracuje posledních přihlášení.

Poslední zpracovaných přihlašovací najdete porovnáním text nápovědy v horní části každé sestavy.

![Nápověda v horní části každé sestavy](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Víc si přečtěte následující dokumentaci na Azure AD zasílání zpráv o chybách najdete v článku [zobrazení sestav aplikace access a použití](active-directory-view-access-usage-reports.md).



## <a name="getting-started"></a>Začínáme


### <a name="sign-into-the-azure-classic-portal"></a>Přihlaste se k portálu Azure klasické

Nejdřív budete muset přihlásit do [Azure klasické portál](https://manage.windowsazure.com) jako globální nebo správce dodržování předpisů. Musí být také spolu správce nebo správce služby Azure předplatné nebo používat "Přístup k Azure AD" Azure předplatného.

### <a name="navigate-to-reports"></a>Přejděte na sestavy

Zobrazit sestavy, přejděte na kartu sestavy v horní části adresáře.

Pokud je to poprvé zobrazení zpráv, musíte souhlasíte s dialogovým oknem, aby bylo možné zobrazit v sestavách. Toto je a ujistěte se, že je přijatelné pro správce ve vaší organizaci zobrazení dat, které lze považovat za důvěrné informace v některých zemích.

![Dialogové okno](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Prozkoumání každou zprávu

Přejděte do každá zpráva zobrazíte informace se shromažďují a přihlášení zpracovat. [Seznam všech zpráv, tady](active-directory-reporting-guide.md)najdete.

![Všechny sestavy](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>Stažení zprávy jako souboru CSV

Každá zpráva si můžete stáhnout do souboru CSV (hodnoty oddělené čárkami). Můžete použít tyto soubory v aplikaci Excel, PowerBI nebo jiných výrobců analýzy aplikací pro další analýzu dat.

Stáhněte si jakoukoliv zprávu jako soubor CSV, přejděte k sestavě a klikněte na "Stáhnout" dole.

![Tlačítko Stáhnout](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Víc si přečtěte následující dokumentaci na Azure AD zasílání zpráv o chybách najdete v článku [zobrazení sestav aplikace access a použití](active-directory-view-access-usage-reports.md).





## <a name="next-steps"></a>Další kroky

### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Přizpůsobení upozornění pro neobvyklých znak činnost

Přejděte na kartu "Konfigurovat" adresáře.

Přejděte do oddílu "Oznámení".

Povolení nebo zakázání oddíl "E-mailové oznámení o neobvyklých přihlášení".

![V části oznámení](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Integrace s Azure AD vykazování rozhraní API

Najdete v článku [Začínáme s rozhraním API sestav](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Zapojit Vícefaktorové ověřování uživatelů

Vyberte uživatele v sestavě.

V dolní části obrazovky klepněte na tlačítko "Povolit MFA".

![Tlačítko Vícefaktorové ověřování v dolní části obrazovky](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Víc si přečtěte následující dokumentaci na Azure AD zasílání zpráv o chybách najdete v článku [zobrazení sestav aplikace access a použití](active-directory-view-access-usage-reports.md).




## <a name="learn-more"></a>Víc se uč


### <a name="audit-events"></a>Auditování události

Přečtěte si o k jakým událostem mají být auditovány v adresáři v [Azure Active Directory vykazování auditování události](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>Rozhraní API integrace

V tématu [Začínáme s rozhraním API sestav](active-directory-reporting-api-getting-started.md) a [přečtěte následující dokumentaci pro rozhraní API odkaz](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Komunikovat

E-mailu [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) pro zpětnou vazbu, Nápověda nebo jakékoli otázky a může mít.

> [AZURE.TIP] Víc si přečtěte následující dokumentaci na Azure AD zasílání zpráv o chybách najdete v článku [zobrazení sestav aplikace access a použití](active-directory-view-access-usage-reports.md).
