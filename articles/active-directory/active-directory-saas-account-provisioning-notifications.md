<properties
    pageTitle="Oznámení o zřízení účtu | Microsoft Azure"
    description="Zjistěte, jak zajistit, aby byly oznámeny problémy související s zřizování uživatelů, které vyžadují pozornost povolením zřízení oznámení účtu."
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
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="account-provisioning-notifications"></a>Účet zřizování oznámení

S zřizování uživatelů, můžete automatizovat proces Správa uživatelů v aplikacích SaaS třetích stran. <br>
Když to je automatického procesu, interakci s Tento proces požaduje od času. <br>
Toto je, například v případě, když heslo účtu, že jste nakonfigurovali vyměňovat data s třetí stranou SaaS aplikace vypršela. 

Povolením zřízení oznámení účtu můžete zajistit, aby byly oznámeny problémy související s zřizování uživatelů, které vyžadují pozornost.

Aktivace nebo deaktivace zřizování jako součást uživatele zřizování konfigurace pro třetí strany aplikace SaaS oznámení.

![Zřizování uživatelů][1] 



Abyste mohli aktivovat zřízení oznámení účtu, vyberte související zaškrtávacího políčka na stránce **potvrzení** dialogové okno a zadejte e-mailový alias příjemce.

![Účet zřizování oznámení][2]
 


Distribuční seznam můžete zadat jako příjemce; je ale důležité mít na paměti, že e-mailové oznámení obsahuje odkazy na sestavy, které jsou přístupné správci Azure AD pouze.

Pokud máte účet zřizování oznámení povoleno, dostanou e-mailů o kritické problémech, které se vztahují k zřizování uživatelů. Abyste se vyhnuli přetížení e-mailu, jenom obdržíte jeden upozornění e-mailový denně pro každou aplikaci SaaS e-mailové oznámení aktivované řešení.


##<a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Automatizace uživatele zřizování/zrušení zajišťování SaaS aplikace](active-directory-saas-app-provisioning.md)
- [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
- [Psaní výrazů pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Definování rozsahu filtry pro zřizování uživatelů](active-directory-saas-scoping-filters.md)
- [Použití SCIM povolit automatické zřizování uživatelů a skupin služby Azure Active Directory pro aplikace](active-directory-scim-provisioning.md)
- [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace](active-directory-saas-tutorial-list.md)



<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png