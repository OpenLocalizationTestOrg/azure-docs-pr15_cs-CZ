<properties
   pageTitle="Vykazování Azure Active Directory - náhled | Microsoft Azure"
   description="Seznam různých dostupných sestav služby Azure Active Directory (verze Preview)"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory reporting – verze preview

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-reporting-azure-portal.md)
- [Azure klasický portálu](active-directory-reporting-guide.md)

*Tento si přečtěte následující dokumentaci je součástí [Azure Active Directory vykazování Průvodce](active-directory-reporting-guide.md).*

Vytváření sestav v náhledu Azure Active Directory, můžete mít všechny informace, které potřebujete zjistit, jak vedou prostředí. [Novinky v náhledu](active-directory-preview-explainer.md)

Existují dva hlavní oblastí vytváření sestav:

- **Přihlašovací aktivity** – informace o použití aplikace a činnosti přihlášení uživatele

- **Protokoly auditování** – systém aktivity informace o uživatelích a Správa skupiny, vaše spravovaných aplikací a aktivity adresáře

V závislosti na rozsah dat, kterou hledáte můžete využít tyto sestavy buď tak, že kliknete na **Uživatelé a skupiny** nebo **podnikových aplikací** v seznamu služby [Azure portálu](https://portal.azure.com).

## <a name="sign-in-activities"></a>Přihlašovací aktivity

### <a name="user-sign-in-activities"></a>Činnosti přihlášení uživatele

Informace poskytované uživatelské přihlašovací sestavy najdete odpovědi na otázky, jako:

- Co je vzorek přihlášení uživatele?
- Počet uživatelů, kteří mají uživatelé přihlášení déle než týden?
- Co je stav tyto přihlášení?

Vstupní bod k těmto datům je uživatelské přihlašovací graf v části **Přehled** v části **Uživatelé a skupiny**.

 ![Vytváření sestav] (./media/active-directory-reporting-azure-portal/05.png "Vytváření sestav")

Uživatelské přihlašovací graf zobrazí týdenní agregace sign in pro všechny uživatele v dané časové období. Výchozí časové období je 30 dní.

![Vytváření sestav] (./media/active-directory-reporting-azure-portal/02.png "Vytváření sestav")

Po kliknutí na den v grafu přihlášení se zobrazí podrobný seznam aktivity přihlášení.

![Vytváření sestav] (./media/active-directory-reporting-azure-portal/03.png "Vytváření sestav")

Každý řádek v seznamu přihlašovací aktivity dostanete podrobné informace o vybraném Přihlaste se jako:

- Kdo má přihlášení?

- Jaký byl související UPN?

- Jaké aplikace byla cíl přihlásit?

- Co je IP adresu Odhlásit se změnami?

- Jaký byl stav Odhlásit se změnami?

### <a name="usage-of-managed-applications"></a>Použití spravované aplikace

U aplikace zaměřené na zpracování zobrazení dat přihlášení můžete odpovědět otázky jako:

- Kdo je používá aplikace?

- Co jsou nejvyšší 3 aplikace ve vaší organizaci?

- Můžu mít naposledy chránění aplikace. Jak to dělá?


Vstupní bod k těmto datům je horní 3 aplikací ve vaší organizaci v poslední sestava 30 dní v části **Přehled** v části **podnikových aplikací**.

 ![Vytváření sestav] (./media/active-directory-reporting-azure-portal/06.png "Vytváření sestav")


Aplikaci použití grafu týdenní agregací přihlaste doplňky, které jsou pro aplikace horní 3 daného období. Výchozí časové období je 30 dní.

![Vytváření sestav] (./media/active-directory-reporting-azure-portal/78.png "Vytváření sestav")

Pokud chcete, můžete nastavit fokus na dané aplikace.

![Vytváření sestav] (./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Vytváření sestav")


Po kliknutí na den v grafu použití aplikace získání podrobný seznam aktivity přihlášení.


![Vytváření sestav] (./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Vytváření sestav")



Možnost **Sign in** najdete kompletní přehled všechny události přihlášení do aplikace.

![Vytváření sestav] (./media/active-directory-reporting-azure-portal/85.png "Vytváření sestav")

Pomocí dialogového okna Výběr sloupců, můžete vybrat datová pole, která chcete zobrazit.

![Vytváření sestav] (./media/active-directory-reporting-azure-portal/column_chooser.png "Vytváření sestav")



### <a name="filtering-sign-ins"></a>Filtrování přihlášení

Sign in můžete filtrovat podle časového intervalu pro omezení počtu zobrazeného data.

![Vytváření sestav] (./media/active-directory-reporting-azure-portal/927.png "Vytváření sestav")


Jiný způsob, jak filtrovat položky aktivity přihlašovací je hledání určité položky.
Způsob hledání umožňuje rozsah vaše přihlášení kolem určité **uživatelů**, **skupin** nebo **aplikací**.


![Vytváření sestav] (./media/active-directory-reporting-azure-portal/84.png "Vytváření sestav")

## <a name="audit-logs"></a>Protokolů auditování

Protokolů auditování v Azure Active Directory obsahují záznamy o systémových aktivit za účelem zajištění shody.

Existují tři hlavní kategorie pro auditování související aktivity Azure portálu:

- Uživatelé a skupiny   

- Aplikace

- Adresář   


Úplný seznam aktivity sestavy auditování najdete v [seznamu události sestavy auditování](active-directory-reporting-audit-events.md#list-of-audit-report-events).


Vstupní bod pro všechna data auditování je **protokolů auditování** v části **aktivity** služby **Azure Active**Directory.


![Sestavy auditování] (./media/active-directory-reporting-azure-portal/61.png "Sestavy auditování")


Protokol auditování obsahuje seznam zobrazující účastníky (), která aktivit (co) a cíle.


![Sestavy auditování] (./media/active-directory-reporting-azure-portal/345.png "Sestavy auditování")


Po kliknutí na položky v zobrazení seznamu, můžete získat další podrobnosti.

![Sestavy auditování] (./media/active-directory-reporting-azure-portal/873.png "Sestavy auditování")




### <a name="users-and-groups-audit-logs"></a>Uživatelé a skupiny protokolů auditování


Uživatel a sestavy založené na skupinách auditování můžete mít odpovědi na otázky, jako:

- Jaké druhy aktualizace byly použity uživatelů?

- Kolik uživatelů byly změněny?

- Byly změněny kolik hesla?

- Co má správce udělat i v adresáři?

- Co jsou skupiny, které byly přidány?

- Nejdou nějaké skupiny se změnami členství?

- Byly změněny vlastníků skupiny?

- K čemu licence byly přiřazeny skupiny nebo uživatele?


Pokud chcete kontrolovat auditování data, která jsou v relaci uživatelům a skupinám, najdete v části **aktivity** **uživatelů**a skupin na filtrované zobrazení klikněte v části **protokolů auditování** .


![Sestavy auditování] (./media/active-directory-reporting-azure-portal/93.png "Sestavy auditování")


### <a name="application-audit-logs"></a>Aplikace protokolů auditování

S aplikací auditování sestav, můžete získat odpovědi na otázky, jako je třeba:

- Jaké aplikace, které jste přidali nebo aktualizace?

- Jaké aplikace, které mohly být odebrány?

- Změnil se pravidlo služby pro aplikaci?

- Byly změněny názvy aplikace?

- Kdo udělili souhlas aplikace?


Pokud chcete kontrolovat auditování data, která souvisí s aplikací, najdete v části **aktivity** **podnikových aplikací**filtrované zobrazení klikněte v části **protokolů auditování** .


![Sestavy auditování] (./media/active-directory-reporting-azure-portal/134.png "Sestavy auditování")


### <a name="filtering-audit-logs"></a>Filtrování protokolů auditování

Sestavy auditování můžete filtrovat podle časového intervalu pro omezení počtu zobrazeného data.

![Sestavy auditování] (./media/active-directory-reporting-azure-portal/324.png "Sestavy auditování")

Jiný způsob, jak filtrovat položky protokolu auditování, je hledání určité položky.

![Sestavy auditování] (./media/active-directory-reporting-azure-portal/237.png "Sestavy auditování")

## <a name="next-steps"></a>Další kroky

V tématu [Azure Active Directory vykazování Průvodce](active-directory-reporting-guide.md).
