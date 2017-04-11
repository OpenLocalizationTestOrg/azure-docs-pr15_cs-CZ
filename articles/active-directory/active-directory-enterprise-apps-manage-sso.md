<properties
    pageTitle="Jednoduchá správa přihlašování pro podnikových aplikací v náhledu Azure Active Directory | Microsoft Azure"
    description="Naučte se spravovat jednotného přihlášení na pro firemní aplikace pomocí služby Azure Active Directory"
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
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>Náhled: Správa jednotného přihlašování pro podnikových aplikací v novém portálu Azure

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-enterprise-apps-manage-sso.md)
- [Azure klasické portálu](active-directory-sso-integrate-saas-apps.md)

Tento článek popisuje, jak používat [Azure portálu](https://portal.azure.com) pro správu nastavení jednotného přihlašování pro aplikace, zejména těch, které jste přidali z [Galerie aplikace služby Azure Active Directory (Azure AD)](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Prostředí management Azure AD pro jednotné přihlašování právě veřejné náhled a tento článek popisuje s novými funkcemi a také pár omezení, které budou v místě pouze v období náhled. [Novinky v náhledu](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>Hledání aplikací v novém portálu

K září 2016 všechny aplikace, které jste nakonfigurovali pro jeden přihlašování v adresáři, správce directory pomocí [Galerie aplikace služby Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) uvnitř [Azure klasické portál](https://manage.windowsazure.com), lze nyní zobrazit a spravovaný v portálu Azure.

Tyto aplikace najdete v části **Podnikových aplikací** Azure portálu odkaz do kterého lze nalézt v seznamu **Další služby** na [portálu](https://portal.azure.com). Podnikových aplikací jsou aplikace, které již byly nasazeny a jsou používány uživatelům ve vaší organizaci.

![Zásuvné podnikových aplikací][1]

Vyberte **všechny aplikace** zobrazíte seznam všech aplikací, které jste nakonfigurovali, včetně aplikací přidané v galerii. Výběr aplikace načte zásuvné zdroje pro tuto aplikaci, kde pro tuto aplikaci je možné zobrazit sestavy a dá se ovládat řadu nastavení.

Pokud chcete spravovat nastavení jednotného přihlašování, vyberte **jednotného přihlašování**.

![Zásuvné zdrojů aplikace][2]


##<a name="single-sign-on-modes"></a>Režimy přihlašování

**Jednotné přihlašování** zásuvné začíná **režimu** nabídka, která umožňuje jednoho přihlašování režimu konfiguraci. Dostupné možnosti:

* **Na základě SAML přihlašování** – tato možnost je k dispozici, pokud aplikace podporuje úplné federované jednotné přihlašování s Azure Active Directory pomocí protokolu SAML 2.0.

* **Na základě heslo přihlašování** – tato možnost je k dispozici, pokud Azure AD podporuje vyplňování pro tuto aplikaci formulářů heslo.

* **Propojené přihlásit** - dřív označoval jako "Existující jednotného přihlašování", tato možnost umožňuje správcům umístit odkaz na této aplikace ve Spouštěči aplikací Panel Azure AD přístup nebo Office 365 své uživatelské.

Další informace o těchto režimů najdete v tématu [jak jednotné přihlášení s Azure Active Directory práce](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).


##<a name="saml-based-sign-on"></a>Na základě SAML přihlášení

Možnost **na základě SAML přihlášení** se zobrazí zásuvné, která je rozdělen do čtyři oddílů:

###<a name="domains-and-urls"></a>Adresy URL a domény
Je to, kde všechny podrobnosti o domény a adresy URL aplikace přidají do vašeho adresáře Azure AD. Všechny vstupy požadovat, aby práce přihlašování aplikace se zobrazují přímo na obrazovce, zatímco zaškrtnutím políčka **Zobrazit rozšířená nastavení adresa URL** je možné zobrazit všechny volitelné vstupy. Úplný seznam podporovaných vstupů obsahuje:

* **Přihlaste se na adresu URL** – kde přejde na přihlásit se k této aplikaci. Pokud aplikace nakonfigurovaný provádět služby, které iniciuje poskytovatele jednotného přihlášení, pak když uživatel přejde na tuto adresu URL, poskytovatel služeb se potřebné přesměrování na Azure AD ověřování a přihlaste se uživatele. Pokud toto pole vyplněné, budou Azure AD používat tuto adresu URL ke spuštění aplikace v Office 365 a Panel Azure AD přístup. Pokud toto pole vynechán, bude Azure AD místo toho provádí zprostředkovatele identit-iniciovanému přihlášení při spuštění aplikace z Office 365, panelu Azure AD přístup nebo z Azure AD jeden přihlašovací adresa URL.

* **Identifikátor URI** - tento URI by bylo možné jednoznačně identifikovat aplikace pro které jedním přihlásit se konfiguruje. Toto je hodnota, která Azure AD odešle zpět do aplikace jako parametr cílové skupiny SAML token a aplikace očekává jeho ověřením. Tato hodnota se bude zobrazovat i jako ID Entity v metadatům SAML součástí aplikace.

* **Adresa URL odpověď** - odpověď, která adresa URL je místo, kam aplikace očekává SAML token. To je označovány jako adresa URL výraz zákaznické služby ACS (). Po zadání tyto klikněte na další a přejděte na další obrazovce. Informace o co je třeba nakonfigurovat na straně aplikace umožňuje přijmout token SAML z Azure AD obsahuje tato obrazovka.

* **Předávací stavu** - stavu relay je volitelný parametr, který může pomoct zjistit aplikace, kde k přesměrování uživatele po dokončení ověřování. Hodnota je většinou platná adresa URL v aplikaci, ale některé aplikace použít toto pole jinak (viz jednotného přihlášení aplikace na si přečtěte následující dokumentaci podrobnosti). Možnost nastavit stav relay je nová funkce, které jsou jedinečné pro nový Azure portál.

###<a name="user-attributes"></a>Atributy uživatele
Toto je místo, kam správce může zobrazovat a upravovat atributy, které jsou odeslané token SAML Azure AD problémy s aplikací každý přihlášení uživatele.

Pouze upravitelné atribut podporované pro nové verze preview je atribut **Identifikátor uživatele** . Tenhle atribut hodnotu pole v Azure AD, které jednoznačně identifikuje každý uživatel v rámci aplikace. Například pokud aplikace nasazené používají "e-mailovou adresu" uživatelské jméno a jedinečný identifikátor, pak hodnotu by nastavit pole "user.mail" v Azure AD.

Úpravy další atributy se nebude podporovat na pozdější náhled.

###<a name="saml-signing-certificate"></a>Podpisový certifikát SAML
Tento oddíl vysvětluje podrobnosti o certifikátu, který Azure AD používá k podpisu tokeny SAML vydané aplikaci pokaždé, když uživatel ověří. Je možné vlastnosti aktuální certifikát, který chcete zkontrolovat, včetně datum vypršení platnosti.

Možnost při přechodu myší certifikát a správa další možnosti se nebude podporovat na pozdější předběžnou certifikát. Všimněte si, že úplné řízení certifikátů pořád provedením [Azure klasické portálu](active-directory-sso-certs.md).

###<a name="application-configuration"></a>Konfigurace aplikace
Konečný části jsou uvedeny si přečtěte následující dokumentaci a/nebo třeba konfigurovat samotnou aplikaci pro použití služby Azure Active Directory zprostředkovatelem identit ovládací prvky.

Postupně zobrazované nabídky **Konfigurace aplikace** poskytuje nové stručný a vložené pokyny ke konfiguraci aplikace. Toto je další nová funkce jedinečné pro nový Azure portál.

> [AZURE.NOTE] Dokončení příkladu si přečtěte následující dokumentaci vložený najdete v článku aplikace Salesforce.com. Si přečtěte následující dokumentaci pro další aplikace se automaticky přidá během náhledu.

![Vložený dokumenty][3]

##<a name="password-based-sign-on"></a>Na základě heslo přihlášení
Pokud podporované pro aplikaci výběrem přihlašování v režimu na základě heslo a na tlačítko **Uložit** okamžitě konfiguruje jeho základě heslo jednotné přihlašování. Další informace o nasazení na základě heslo SSO najdete v tématu [jak jednotné přihlášení s Azure Active Directory práce](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Na základě heslo přihlášení][4]


##<a name="linked-sign-on"></a>Propojené přihlášení
Pokud podporované pro aplikaci výběru propojené režimu jednotné přihlašování umožňuje zadejte adresu URL, kterou chcete Panel Azure AD přístup nebo Office 365 k přesměrování na po kliknutí na této aplikace. Podrobnosti o propojené jednotné přihlašování (dříve označované jako "existující SSO"), najdete v článku [jak jednotné přihlášení s Azure Active Directory práce](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Propojené jednotné přihlášení][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
