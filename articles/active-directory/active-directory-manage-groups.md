<properties
    pageTitle="Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory | Microsoft Azure"
    description="Jak používat skupiny v Azure Active Directory spravovat přístup uživatelů k místním prostředím a cloudové aplikace a prostředky."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="managing-access-to-resources-with-azure-active-directory-groups"></a>Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory

Azure Active Directory (Azure AD) je komplexní identit a přístup k řešení pro správu, která poskytuje robustní sadu funkcí pro správu přístupu k místním prostředím a cloudu aplikací a materiálů, včetně službám Microsoft online services jako je Office 365 a prezentace aplikace Microsoft SaaS. Tento článek obsahuje přehled, ale pokud chcete začít používat skupiny Azure AD teď, postupujte podle pokynů uvedených v článku [Správa skupin zabezpečení v Azure AD](active-directory-accessmanagement-manage-groups.md). Pokud chcete zjistěte, jak pomocí Powershellu ke správě skupin služby Azure Active directory si můžete přečíst víc v [Azure Active Directory náhled rutiny pro správu skupin](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).


> [AZURE.NOTE] Abyste mohli používat Azure Active Directory, potřebujete účet Azure. Pokud nemáte účet, můžete [zaregistrovat si bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/).


V rámci Azure AD jednou z hlavních funkcí je možnost spravovat přístup ke zdrojům. Tyto materiály může být součástí v adresáři, stejně jako v případě oprávnění ke správě objekty prostřednictvím rolí v adresáři nebo zdroje, které jsou mimo v adresáři, například SaaS aplikací, Azure služeb a Sharepointových webech nebo na místní zdroje. Uživatele můžete přidělovat přístupová práva zdroji čtyři způsoby:


1. Přímé přiřazení

    Uživatele můžete přidělovat přímo zdroj vlastníkem daný zdroj.

2. Členství ve skupinách

    Skupiny můžete musí mít přiřazené zdroji vlastníkem zdroje a tím, poskytnutí členové této skupiny přístup k tomuto prostředku. Členství ve skupině můžete pak spravován vlastník skupiny. Efektivní vlastníka zdroje deleguje oprávnění pro přiřazení uživatelů k jejich zdroj vlastník skupiny.

3. Na základě pravidel

    Vlastníka zdroje můžete použít pravidla pro vyjádření určující, kteří uživatelé mají přiřazeny přístup ke zdroji. Výsledek pravidla závisí na atributy použité toto pravidlo a jejich hodnoty pro konkrétní uživatele a tím vlastníka zdroje efektivně delegátů právo spravovat přístup k jejich zdroje ke zdroji autoritativní atributy, které se používají v rámci pravidla. Vlastníka zdroje pořád spravuje pravidlo sebe sama a určí, které atributy a hodnoty poskytnout přístup k jejich zdroje.

4. Externí autorita

    Přístup k zdroje je odvozeno z externího zdroje; například na skupinu, která se synchronizuje ze autoritativní zdroj například místního adresáře nebo z aplikace SaaS například pracovního dne. Vlastníka zdroje přiřadí skupině k poskytnutí přístupu ke zdroji a externím zdroji spravuje členy této skupiny.

  ![Základní informace o diagramu řízení přístupu](./media/active-directory-access-management-groups/access-management-overview.png)


## <a name="watch-a-video-that-explains-access-management"></a>Podívejte se na video, kde správu přístupu

Podívejte se na krátké video, kde více o tomto:

**Azure AD: Úvod k dynamické členství ve skupinách**

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Jak získat přístup k správy v Azure Active Directory práce?
V centru Azure AD je řešení pro správu přístupu skupiny zabezpečení. Správa přístupu k prostředkům pomocí skupiny zabezpečení je známý paradigma, které umožňuje flexibilní a snadno srozumitelné způsob, jak zajistit přístup k prostředku zamýšleného skupinu uživatelů. Vlastníka zdroje (nebo správce v adresáři) můžete přiřadit skupinu, do určité přístup zprava prostředky, které vlastní. Členové skupiny se dozvíte přístup a vlastníka zdroje můžete udělit oprávnění ke správě seznamu členy skupiny jinému uživateli, například oddělení správce nebo správce technickou podporu.

![Diagram řízení přístupu k Azure Active Directory](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

Vlastník skupiny můžete zpřístupnit danou skupinu pro samoobslužné požadavky. Přitom koncového uživatele můžete vyhledávat Najděte skupinu a požádat o připojení, efektivní hledání oprávnění pro přístup k prostředky, které je spravováno prostřednictvím skupiny. Vlastník skupiny můžete nastavit skupině tak, aby požadavků spojení schvalují automaticky nebo vyžadovat schválení vlastník skupiny. Když uživatel odešle žádost o připojení ke skupině, požadavek na připojit se ke předán vlastníky skupiny. Pokud schválí jednu vlastníků žádost, požadování uživatel oznámení a uživatel připojen ke skupině. Pokud jeden z vlastníci zakazuje žádost, uživatel oznámení ale není připojený ke skupině.


## <a name="getting-started-with-access-management"></a>Začínáme s správu přístupu
Jste připravení začít? Měli vyzkoušet některé základní úkoly, které můžete dělat s Azure AD skupiny. Použijte tyto možnosti k poskytování specializované přístupu k různým skupinám lidí pro různé zdroje ve vaší organizaci. Seznam základní první kroky, které jsou vypsané dole.

* [Vytvoření jednoduchého pravidla pro dynamické členství ve skupině nastavení](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)

* [Používání skupiny pro správu přístup k aplikacím SaaS](active-directory-accessmanagement-group-saasapps.md)

* [Zpřístupnění skupiny pro koncového uživatele samoobslužných funkcí](active-directory-accessmanagement-self-service-group-management.md)

* [Synchronizaci místních skupiny na Azure pomocí Azure AD Connect](active-directory-aadconnect.md)

* [Správa vlastníků skupiny](active-directory-accessmanagement-managing-group-owners.md)


## <a name="next-steps-for-access-management"></a>Další kroky pro správu přístupu
Teď máte rozumí základní informace o řízení přístupu, tady jsou některé další rozšířené možnosti k dispozici v Azure Active Directory pro správu přístup k aplikacím a prostředky.

* [Chcete-li vytvářet složitější pravidla pomocí atributů](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [Správa skupin zabezpečení v Azure AD](active-directory-accessmanagement-manage-groups.md)

* [Nastavení vyhrazené skupin v Azure AD](active-directory-accessmanagement-dedicated-groups.md)

* [Přehled grafu rozhraní API pro skupiny](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

* [Azure Active Directory rutiny pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
