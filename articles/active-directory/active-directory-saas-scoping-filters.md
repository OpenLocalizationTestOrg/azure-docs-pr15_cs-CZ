<properties
    pageTitle="Na základě atribut aplikace zřizování s definování rozsahu filtry | Microsoft Azure"
    description="Zjistěte, jak používat oboru filtry abychom zabránili posílání objektů v aplikací, které podporují zřizování automatické uživatele z skutečně zřízení Pokud objektu nevyhovují vašim požadavkům pro firmy."
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


# <a name="attribute-based-app-provisioning-with-scoping-filters"></a>Na základě atribut aplikace zřizování s definování rozsahu filtry

Cílem v této části je vysvětleno, jak pomocí oboru filtrů k definování pravidla atributů, které bude určovat, kteří uživatelé budou zřízení do aplikace.





## <a name="clauses-and-scope-groups"></a>Klauzule a obor skupiny


![Definování rozsahu filtru][1] 




Oboru filtry jsou definovány jednu nebo více **skupin obor**, přičemž každý z nich stiskněte a podržte jednu či více **klauzulí**. Klauzule pro určitý obor skupiny zobrazíte rozbalte kliknutím na šipku vlevo od názvu skupiny.

**Klauzule** Určuje, které uživatelé mohou projít oboru filtr určování atributy každého uživatele. Máte jednu klauzule, která vyžaduje, aby uživatele "státu" atribut rovno New York, což znamená, že pouze uživatelé New York bude zřízení do aplikace.

![Definování rozsahu název skupiny][2] 



Každá **Skupina rozsah** začíná jednu povinné **klauzule**, viz snímek výše. Tato klauzule jednoduše uvádí, že uživatel musí nejdřív mít přiřazené k aplikaci předtím, než je vyhodnocen tak, že oboru filtry. Tato klauzule nelze odstranit nebo změnit.

Stisknutím kombinace kláves na příslušné tlačítko můžete přidat nové věty nebo nových skupin obor. Dodáte každá skupina obor názvu úpravou jeho vlastnost **Obor názvu skupiny** .





## <a name="how-scoping-filters-are-evaluated"></a>Jak jsou vyhodnoceny definování rozsahu filtry

Při vytváření, testování každé přiřazené uživateli oboru filtry určit, pokud tento uživatel si zaslouží přístup k aplikaci. Si můžete představit každé klauzule jako test, které musí být v pořadí u uživatele, chcete-li předejít začíná odfiltrované. 

Pokud máte více skupin obor definované projít aspoň jedna z nich jednotlivých uživatelů pro přístup k aplikaci. V rámci jednotlivých skupin obor však uživatel musí uplynout každého jednoho klauzule k předání danou skupinu určitý obor. 

Jinými slovy, můžete si funkci obor skupiny jako by dohromady, nebo si můžete představit klauzule obsahují jako a by společně. Zvažte například oboru filtr níže:


![Definování rozsahu název skupiny][2]  


Podle oboru filtr uživatelé musí splňovat následující kritéria, abyste mohli být zřízení:

1. Musí mít přiřazené k aplikaci.

2. Musíte pracovat v oblasti inženýrské funkce

3. Musí být práce v San Francisco nebo Kanada.


##<a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Automatizace uživatele zajišťování a zrušení zajišťování SaaS aplikací](active-directory-saas-app-provisioning.md)
- [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
- [Psaní výrazů pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
- [Použití SCIM povolit automatické zřizování uživatelů a skupin služby Azure Active Directory pro aplikace](active-directory-scim-provisioning.md)
- [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png
