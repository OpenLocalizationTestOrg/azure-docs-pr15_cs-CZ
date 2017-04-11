<properties
    pageTitle="Automatické uživatele aplikace SaaS zřizování v Azure AD | Microsoft Azure"
    description="Úvod k používání Azure AD automaticky zřízení zrušte zřízení a průběžně aktualizují uživatelské účty ve více aplikacích SaaS třetích stran."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatizace uživatele zajišťování a zrušení zajišťování aplikacím SaaS službou Azure Active Directory

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>Co je aplikace SaaS automatické zřizování uživatele?

Azure Active Directory (Azure AD) umožňuje automatizovat vytváření, údržbu a odebrání identit uživatelů v cloudu ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) aplikací, jako je Dropboxu, Salesforce, ServiceNow a další.

**Níže najdete pár příkladů co tato funkce umožňuje provádět:**

- Automatické vytváření nových účtů správné SaaS aplikací pro nové uživatele při přihlášení ke svým týmem.
- Když lidé nevyhnutelně ponechat týmu automaticky deaktivujte účty z SaaS aplikace.
- Ujistěte se, že identit v aplikacích SaaS jsou k dispozici aktuální na základě změn v adresáři.
- Zřízení neuživatelských poštovních objekty, například skupinám tak, aby SaaS aplikace, které je podporují.

**Zřizování automatické uživatele obsahuje následující funkce:**

- Možnost podle existující identit mezi Azure AD a SaaS aplikace.
- Možnosti přizpůsobení k nápovědě Azure AD podle aktuální konfigurace SaaS aplikace, které aktuálně vaše organizace používá.
- Volitelné e-mailová upozornění pro chyby zřizování.
- Vytváření sestav a aktivity protokoly pomoct s monitorování a řešení potíží.

##<a name="why-use-automated-provisioning"></a>Proč používat automatické vytváření?

Některé běžné podnětů pro použití této funkce patří:

- Chcete-li předejít náklady, nedostatky a lidské chybu přidruženou k příručku pro zřízení procesů.
- Odebráním okamžitě identit uživatelů klíčové aplikace SaaS při opustí organizaci zabezpečení vaší organizace.
- Snadno importovat hromadné počet uživatelů do určité SaaS aplikace.
- Pokud si výhodou s zřizovací řešení spustit mimo stejné zásady přístupu aplikace, které jste definovali pro Azure AD jednotného přihlašování.

##<a name="frequently-asked-questions"></a>Nejčastější dotazy

**Jak často Azure AD zapisovat adresáře změny aplikaci SaaS?**

Azure AD hledá změny každé 5 až 10 minut. Vrací-li aplikaci SaaS několika chyb (například stejně jako v případě přihlašovacích údajů správce neplatné), pak v Azure AD postupně zpomalit jeho frekvence až jednou za den až pevnou chyby.

**Jak dlouho trvá zřízení svoje uživatele?**

Přírůstková změn dojít téměř okamžitě, ale pokud se pokoušíte zřízení většina adresáře, potom záleží na počtu uživatelů a skupin, které máte. Malé adresářů pouze chvíli trvat, střední adresářů může trvat několik minut a obrovské adresářů může trvat několik hodin.

**Jak lze sledovat průběh aktuální úloha zřizování?**

Můžete zobrazit zprávu zřízení účtu adresáře v části sestavy. Další možností je navštívit karta řídicí panel pro SaaS aplikaci, která jsou vytváření a vyhledejte v části "Stav integrace" v dolní části stránky.

**Jak poznám, že pokud uživatelé moct získat zřízení správně?**

Na konci zřizovací konfigurace Průvodce tam je možnost přihlášení k odběru e-mailová oznámení pro zřizování k chybám. Můžete taky zaškrtnout sestava chyby zřizování najdete v článku určující, kteří uživatelé se nepodařilo zřídit a proč.

**Můžete Azure AD odepsat změny v aplikaci SaaS do adresáře?**

Pro většinu SaaS aplikace zřizování je odchozího, což znamená, že uživatelé jsou z adresáře došlo k zápisu aplikace a změny z aplikace nemůže být zpět do adresáře. [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx)však zřizování je pouze příchozí, což znamená, že uživatelé naimportují do adresáře z Workday a podobně změny v adresáři není získat zpět do pracovního dne.

**Jak můžete odeslat názor technickým týmu?**

Kontaktujte nás prostřednictvím [fórum pro názory Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="how-does-automated-provisioning-work"></a>Jak funguje Automatická zřizovací práce?

Azure AD ustanovení uživatelům aplikace SaaS připojením k vytváření koncové body poskytnutých jednotlivých dodavatele aplikace. Tyto koncové body povolit Azure AD programově vytvářet, aktualizovat a odstranit uživatele. Tady je stručný přehled různé kroky, které přebírají Azure AD pro automatizaci zřizování.

1. Pokud povolíte zřizování aplikace poprvé, jsou provést následující akce:
 - Azure AD pokusí se shodují všechny stávajícím uživatelům v aplikaci SaaS s jejich odpovídající identit v adresáři. Když se uživatel spárována, *Ne* pro jednotné přihlašování automaticky zapnutá. Aby uživatel měl přístup k aplikacím budou musí mít explicitně přiřazené k aplikaci v Azure AD přímo nebo prostřednictvím členství ve skupině.
 - Pokud jste již zadali určující, kteří uživatelé by měl být přiřazen aplikace a Azure AD se nepodařilo najít stávajících účtů pro uživatele, zřízení Azure AD nových účtů je v aplikaci.
2. Po počáteční synchronizace jak jsme je popsali výše, Azure AD kontroloval každých 10 minut pro následující změny:
 - Pokud byly přiřazeny noví uživatelé aplikace (přímo nebo prostřednictvím členství ve skupinách), potom budou zřízení nového účtu v aplikaci SaaS.
 - Pokud byla odebrána přístup uživatele a pak svůj účet v aplikaci SaaS bude označeno zakázané (uživatelé budou odstraněny nikdy úplně, které chrání před ztrátou dat v případě konfigurace stavové).
 - Pokud uživatel naposledy přidělené aplikace a jejich už máte účet v aplikaci SaaS, tento účet bude označeno povolené a pokud zastaralé ve srovnání s adresářem může dojít k aktualizaci vlastností určité uživatele.
 - Došlo ke změně informací o uživateli (například telefonního čísla, umístění pracoviště atd.) v adresáři, potom tyto informace se taky aktualizují v aplikaci SaaS.

Další informace o tom, jak jsou namapované atributy mezi Azure AD a SaaS aplikace, najdete v článku na [Přizpůsobení mapování atributů](active-directory-saas-customizing-attribute-mappings.md).

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>Seznam aplikací, které podporují automatické zřizování uživatelů

Klepněte na aplikace zobrazíte kurz o tom, jak konfigurovat automatické vytváření ho:

- [Pole](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [Vyústit](http://go.microsoft.com/fwlink/?LinkId=309575)
- [Docusign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [Dropboxu pro firmy](http://go.microsoft.com/fwlink/?LinkId=309581)
- [Google Apps](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [Služby Salesforce](http://go.microsoft.com/fwlink/?LinkId=286017)
- [Izolovaného prostoru služby Salesforce](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [WORKDAY](http://go.microsoft.com/fwlink/?LinkId=690250) (příchozí zřizování)

Aby aplikace pro podporu zřizování automatické uživatele musíte nejdřív dát potřebné koncové body, které umožňují externí programy automatizovat vytváření, údržbu a odebrání uživatelů. Ne všechny aplikace SaaS jsou proto kompatibilní s touto funkcí. K aplikacím, které podporují to tým Azure AD pak budou moct vytvářet zřizovací spojnice tak, aby těmito aplikacemi a tento úkol je přednostně podle potřeb aktuální a potenciální zákazníci.

Kontaktovat tým Azure AD požádat o zřizovací podpora pro další aplikace, odešlete zprávu prostřednictvím [fórum pro názory Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
- [Psaní výrazů pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Definování rozsahu filtry pro zřizování uživatelů](active-directory-saas-scoping-filters.md)
- [Použití SCIM povolit automatické zřizování uživatelů a skupin služby Azure Active Directory pro aplikace](active-directory-scim-provisioning.md)
- [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
- [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace](active-directory-saas-tutorial-list.md)
