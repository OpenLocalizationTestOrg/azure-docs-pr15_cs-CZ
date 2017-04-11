<properties
    pageTitle="Zřízení řízení podnikových aplikací v náhledu Azure Active Directory uživatele | Microsoft Azure"
    description="Naučte se spravovat uživatelského účtu zřizování pro podnikových aplikací pomocí náhledu Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Náhled: Správa uživatelský účet zřizování pro podnikových aplikací v novém portálu Azure

Tento článek popisuje, jak používat [Azure portálu](https://portal.azure.com) pro správu automatické uživatelský účet zřizování a zrušte zřizování pro aplikace, které podporují, zejména těmi, které přidané v kategorii "doporučené" [Galerie aplikace služby Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Tento možnostem správy v novém portálu Azure právě veřejné náhled a tento článek popisuje s novými funkcemi a také pár omezení, které jsou na místě náhledu období. [Novinky v náhledu](active-directory-preview-explainer.md)

Další informace o zřízení účtu automatické uživatele a jak to funguje, najdete v článku [automatizovat zřizování uživatelů a Deprovisioning aplikacím SaaS s Azure Active Directory](active-directory-saas-app-provisioning.md).

##<a name="finding-your-apps-in-the-new-portal"></a>Hledání aplikací v novém portálu

K září 2016 všechny aplikace, které jste nakonfigurovali pro jeden přihlašování v adresáři, správce directory pomocí [Galerie aplikace služby Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) uvnitř [Azure klasické portál](https://manage.windowsazure.com), lze nyní zobrazit a spravovat v novém portálu Azure.

Tyto aplikace najdete v části **Podnikových aplikací** nové Azure portálu, který je přístupný z nabídky **Další služby** v levé navigační oblasti. Podnikových aplikací jsou aplikace, které již byly nasazeny a jsou používány uživatelům ve vaší organizaci.

![Zásuvné podnikových aplikací][0]

Výběr **všech aplikací** odkaz na levé straně, zobrazí se seznam všech aplikací, které jste nakonfigurovali, včetně aplikací přidané v galerii. Výběr aplikace načte zásuvné zdroje pro tuto aplikaci, kde pro tuto aplikaci je možné zobrazit sestavy a dá se ovládat řadu nastavení.

Uživatelský účet nastavení pro vytváření můžete spravovat výběrem **Provisioning** na levé straně.

![Zásuvné zdrojů aplikace][1]


##<a name="provisioning-modes"></a>Zřízení režimy

Zásuvné **Provisioning** začíná nabídky **režim** , který ukazuje, jaké zřizování režimy jsou podporovány pro podnikovou aplikací a umožňuje, aby se konfigurovat. Dostupné možnosti:

* **Automatické** – tato možnost se zobrazí, pokud Azure AD podporuje automatické vytváření založené na rozhraní API a/nebo zrušit zřizování uživatelských účtů do této aplikace. Výběr tohoto režimu zobrazí rozhraní, které vede správce konfigurace Azure AD pro připojení k rozhraní API aplikace uživatele správy, vytvoření mapování účtů a pracovní postupy, které určují způsob, jakým by měl data uživatelských účtů probíhá mezi Azure AD a aplikace a řízení schůzky Azure AD zřízení služby.

* **Ruční** – tato možnost je zobrazen, pokud Azure AD nepodporuje automatické vytváření uživatelských účtů do této aplikace. Tato možnost znamená, že musíte spravovat záznamy klientů uživatele uložená v aplikaci pomocí externího procesu, podle uživatele řízení a zřízení možnosti poskytované aplikace (který může obsahovat SAML JIT zřizování).


##<a name="configuring-automatic-user-account-provisioning"></a>Konfigurace zřizování automatické uživatelského účtu

Výběr možnosti **Automatické** zobrazí obrazovka, která je rozdělen do čtyři oddílů:

###<a name="admin-credentials"></a>Přihlašovací údaje správce
Je to, kde potřebných pro Azure AD pro připojení k rozhraní API jsou zadány Správa uživatelů aplikaci její přihlašovací údaje. Vstupní povinné liší se podle aplikace. Informace o požadavcích pro konkrétní aplikace a přihlašovacích údajů typů najdete v tématu [kurz konfigurace pro určitou aplikaci](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Výběr tlačítko **Testovat připojení** umožňuje testovat pověření tím, že Azure AD pokus o připojení k aplikaci je zřizujete aplikaci zadaná pověření.

###<a name="mappings"></a>Mapování
Toto je místo, kam správce může zobrazovat a upravovat jaké atributy toku uživatelů mezi Azure AD a cílové aplikace, když zřízení nebo aktualizovat uživatelské účty.

Existuje předkonfigurované sada mapování mezi objekty uživatelů Azure AD a objektů uživatele každý SaaS aplikace. Některé aplikace spravovat jiné druhy objekty, například skupiny nebo kontakty. Výběrem jedné z těchto mapování v v tabulce jsou uvedeny editoru mapování doprava, kde mohou být zobrazit a upravit.

![Zásuvné zdrojů aplikace][2]

Podporované vlastní nastavení během prvního náhled patří:

* Povolení nebo zakázání mapování pro určité objekty, například objekt Azure AD uživatele k aplikaci SaaS objektu uživatele.

* Úpravy, které atributy flow z objektu uživatele Azure AD objektu uživatele v aplikaci. Další informace o mapování atribut najdete v článku [Principy atribut mapování typů](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).

* Filtrování jaké zřizovací akce Azure AD by měl provést cílovou aplikaci, která je nová funkce v portálu Azure. Takže není nutné Azure AD plně synchronizace objektů, můžete nastavit omezení akce provést. Například pouze výběrem **aktualizací**jenom aktualizace Azure AD existující uživatelské účty v aplikaci a nevytvoří nové. Výběrem pouze **vytvořit**, Azure pouze vytvoří novým uživatelským účtům ale neaktualizuje existující. Tato funkce umožňuje správcům k vytvoření různých mapování pro vytvoření účtu a aktualizaci pracovní postupy. Úplné možnost vytvořit více mapování za aplikace je plánované pro později v období placení náhled.

###<a name="settings"></a>Nastavení
Tato část umožňuje správcům spustit a zastavit Azure AD zřízení služby pro vybranou aplikaci i můžete také vymazat mezipaměť zřizování a restartovat službu.

Pokud zřizování je povoleno poprvé aplikace, zapněte změnou **Zřizování stav** **na**služby. To způsobí, že Azure AD zřízení služby provádět počáteční synchronizace, kde bude číst uživatelé přiřazení v části **Uživatelé a skupiny** dotazů cílovou aplikaci pro ně a potom provádí zřizovací akce definované v části Azure AD **mapování** . Během tohoto procesu zřizovací služby jsou uložená data uložená v mezipaměti o jaké uživatelských účtů je správa, tak nespravované účtů do cílové aplikace, které byly nikdy v rozsahu přiřazení neovlivní zrušte zřízení operace. Po počáteční synchronizační službu zřizovací automaticky synchronizuje uživatele a skupiny na deset minut.

Změní **Zřizování stavu** na **vypnout** jednoduše pozastaví poskytování služby. V tomto stavu Azure není vytvořit, aktualizovat nebo odebrat uživatele nebo skupiny objektů v aplikaci. Změna stavu zpět na zapnuto způsobí, že služba vystopovat tam, kde ho přestali.

Výběr zaškrtávacího políčka **zrušte aktuální stav a znovu spustit synchronizaci** a uložení zastaví službu zřizovací výpisy data uložená v mezipaměti o účty Azure AD spravuje, restartuje služeb a provede počáteční synchronizaci znovu. Tato možnost umožňuje správcům ke spuštění zřizování procesu nasazení znovu.

###<a name="synchronization-details"></a>Podrobnosti o synchronizaci
Tato část obsahuje Sčítání podrobností o operaci zřizovací služby, včetně první a poslední kolikrát službu zřizovací spustili proti aplikace a kolik uživatele a skupiny jsou spravovány.

Jsou odkazy za předpokladu, že **Provisioning aktivity sestavy**, která poskytuje protokol všichni uživatelé a skupiny vytvořeného aktualizované a odebírat mezi Azure AD a cílové aplikace a **zprávu o chybách Provisioning** , který poskytuje další podrobné chybové zprávy pro uživatele a seskupit objekty, které se nepodařilo číst, vytvořili, aktualizovat nebo odebrat. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
