<properties 
    pageTitle="Konfigurace jednotného přihlašování k aplikacím, které nejsou v galerii aplikace služby Azure Active Directory | Microsoft Azure" 
    description="Zjistěte, jak chcete samoobslužné připojit k aplikacím pro službu Azure Active Directory pomocí SAML a na základě heslo jednotné přihlašování" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Konfigurace jednotného přihlašování k aplikacím, které nejsou v galerii aplikace služby Azure Active Directory

Tento článek se zabývá funkci, která umožňuje správcům Konfigurace jednotného přihlašování k aplikacím není k dispozici v v Azure Active Directory aplikace Galerie *bez psaní kódu*. Tato funkce byla vydána z technical preview na 18 listopad 2015 a je součástí [Azure Active Directory Premium](active-directory-editions.md). Pokud místo toho hledáte pokyny pro vývojáře o tom, jak integrovat vlastní aplikace s Azure AD pomocí kódu, přečtěte si téma [Ověření scénáře Azure AD](active-directory-authentication-scenarios.md).

Galerie aplikace služby Azure Active Directory obsahuje seznam aplikací, které označují podporuje forma jednotné přihlašování pomocí služby Azure Active Directory, způsobem popsaným v [tomto článku](active-directory-appssoaccess-whatis.md). Když jste (jako IT specialist nebo systém integrátorem ve vaší organizaci) našli aplikace se chcete připojit, mohli rovnou začít podle pokynů podrobné prezentovány v portálu pro správu Azure povolit jednotného přihlašování.

Zákazníkům s [Azure Active Directory Premium](active-directory-editions.md) licence také zobrazit tyto další možnosti:

* Samoobslužné integrace libovolné aplikaci, která podporuje zprostředkovatele identit SAML 2.0 (SP inicializovaný nebo inicializovaný IdP)
* Samoobslužné integrace webová aplikace, která má HTML na přihlašovací stránku jednotné [přihlašování v heslo](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Samoobslužné připojení aplikace, které používají protokol SCIM pro uživatele zřizovací ([zde popsané](active-directory-scim-provisioning.md))
* Možnost přidání odkazů na všechny aplikace ve [Spouštěči aplikací Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) nebo [panel Azure AD přístup](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)

To může obsahovat pouze SaaS aplikace, které můžete použít, ale nebyly dosud byla na nacházejí do Galerie aplikací Azure AD, ale třetích stran webové aplikace, které vaše organizace má nasazených na serverech, mezi nimiž můžete řídit, v cloudu nebo místní.

Tyto možnosti, nazývaný také *integrace na šablonách*poskytnutí standardy spojovacích bodů k aplikacím, které podporují SAML, SCIM nebo ověřování pomocí formulářů, zahrnout flexibilní možností a nastavení kompatibility s obecných počet aplikací. 

##<a name="adding-an-unlisted-application"></a>Přidání aplikace nezařazené

Připojení aplikace pomocí šablony integrace s aplikací, přihlaste se k portálu Správa Azure pomocí svého účtu správce služby Azure Active Directory a přejděte **služby Active Directory > [adresář] > aplikace** vyberte **Přidat**a pak **Přidat aplikaci z Galerie**. 

![][1]

V galerii aplikace můžete přidat nezařazené aplikaci pomocí **vlastní** kategorie na levé straně nebo tak, že vyberete **Přidat nezařazené aplikaci** odkaz, který se zobrazí ve výsledcích hledání, pokud nebyl nalezen požadovanou aplikaci. Po zadání jména do aplikace, můžete nakonfigurovat možnosti přihlašování a chování. 

**Rychlý tip**: osvědčený, zkontrolujte, zda aplikace již existuje v galerii aplikace pomocí funkce Hledat. Pokud není nalezen aplikace a její popis zmínky "jednotné přihlašování" a potom aplikace je už podporováno federované jednotného přihlašování. 

![][2]

Přidání aplikace tímto způsobem obsahuje setkat i v případě velmi podobné té umožňující předem integrované aplikace. Pokud chcete začít, vyberte **Konfigurace jednotného přihlašování**. Na další obrazovce nabídne vám následující tři možnosti konfigurace jednotného přihlášení, které jsou popsané v následující části.

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure AD jednotné přihlášení

Tato možnost slouží k konfigurace ověřování na základě SAML aplikace. Tento postup vyžaduje, aby podpora aplikací SAML 2.0 a měli shromažďování informací o tom, jak používat funkce SAML aplikace, než budete pokračovat. Po výběru **Další**, zobrazí se výzva k zadání tři různé adresy URL odpovídající koncové body SAML aplikace. 

![][4]
 
Toto jsou:

* **Znak na adresu URL (SP inicializovaný pouze)** – Pokud uživatel přejde na přihlásit se k této aplikaci. Pokud aplikace nakonfigurovaný provádět služby, které iniciuje poskytovatele jednotného přihlášení, pak když uživatel přejde na tuto adresu URL, poskytovatel služeb proveďte nezbytné přesměrování Azure AD pro ověřování a přihlášení uživatele systému. Pokud toto pole vyplněné, budou Azure AD používat tuto adresu URL ke spuštění aplikace v Office 365 a Panel Azure AD přístup. Pokud toto pole je ommited a pak Azure AD místo toho provede zprostředkovatele identit-iniciovanému přihlášení při spuštění aplikace z Office 365, panelu Azure AD přístup nebo z Azure AD jeden přihlašovací adresa URL (copiable z karty řídicího panelu).

* **Adresa URL Vystavitel** - vydavatel, které URL by bylo možné jednoznačně identifikovat žádost o které jednotného přihlášení na nakonfigurovaný. Toto je hodnota, která Azure AD odešle zpět do aplikace jako parametr **cílové skupiny** SAML token a aplikace očekává jeho ověřením. Tato hodnota se bude zobrazovat i jako **Entity ID** v metadatům SAML součástí aplikace. V dokumentaci aplikace SAML podrobné informace o co je Entity ID nebo hodnotu cílová skupina. Tady je příklad jak URL cílové skupiny se objeví v token SAML do aplikace:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Adresa URL odpověď** - odpověď, která adresa URL je místo, kam aplikace očekává SAML token. To je označovány jako **Adresa URL výraz zákaznické služby ACS ()**. Dokumentaci aplikace SAML podrobnosti o novinky SAML tokenu vyjádření adresu URL nebo ACS URL.
 Po zadání tyto klikněte na tlačítko **Další** a přejděte na další obrazovce. Informace o co je třeba nakonfigurovat na straně aplikace umožňuje přijmout token SAML z Azure AD obsahuje tato obrazovka. 

![][5]

Hodnoty, které jsou potřeba lišit podle toho, aplikace, takže sledujte aplikace SAML přečtěte následující dokumentaci pro podrobnosti. **Přihlašování** a **odhlašování** služby URL obě překládal stejný koncový bod, který je koncový bod zpracování požadavků SAML instance pracovního Azure AD. Adresa URL vydavatel je hodnota, která se zobrazí jako "Vystavitel" uvnitř tokenu SAML vydat aplikace. 

Po konfiguraci aplikace klikněte na tlačítko **Další** a potom na **Dokončit** zavřete dialogové okno. 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>Přiřazení uživatelů a skupin do aplikace SAML 

Po konfiguraci aplikace používání Azure AD jako poskytovatele identit SAML základě je téměř připravená k testování. Jako ovládacího prvku zabezpečení nebude Azure AD problémů token povolením se přihlásit do aplikace, pokud je máte udělený přístup pomocí Azure AD. Uživatelům udělit přístup přímo nebo prostřednictvím skupinu, která jsou členem. 

Přiřadit uživatele nebo skupiny aplikace, klikněte na tlačítko **Přiřadit uživatele** . Vyberte uživatele nebo skupinu, kterou chcete přiřadit a potom vyberte tlačítko **přiřadit** . 

![][6]

Přiřazení uživatele vám umožní Azure AD o vystavení tokenu pro uživatele, stejně jako příčinou dlaždice pro tuto aplikaci zobrazit panel přístupu uživatele. Dlaždice aplikace se také zobrazí ve Spouštěči aplikací Office 365, pokud uživatel používá Office 365. 

Můžete nahrát logo dlaždice aplikace na kartě **Konfigurovat** pomocí tlačítka **Nahrát Logo** aplikace. 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>Přizpůsobení nároků Vystavitel tokenu SAML 

Když uživatel se ověří s aplikací, Azure AD problémů o uživateli jednoznačně identifikuje SAML token do aplikace, která obsahuje informace (nebo deklarací). Ve výchozím nastavení jedná se o uživatelské jméno, e-mailovou adresu, křestní jméno a příjmení uživatele. 

Můžete zobrazit ani upravit deklarací Odeslaná pošta v token SAML aplikaci klikněte v části **atributy** . 

![][7]

Existují dvě možné příčiny, proč může být nutné upravit deklarací Vystavitel tokenu SAML: napsal •maximální aplikace vyžadovat jinou sadu deklarace URI nebo hodnoty deklarace •vlastní aplikace byly nasazeny způsobem, který vyžaduje NameIdentifier nárok na něco než uživatelské jméno (ZTJ hlavní uživatelské jméno) uložená v Azure Active Directory. 

Informace o tom, jak přidávat a upravovat nároky na tyto scénáře podívejte se na tento [článek o přizpůsobení deklarované](active-directory-saml-claims-customization.md). 

###<a name="testing-the-saml-application"></a>Testování SAML aplikace 

Jakmile SAML adresy URL a certifikát nakonfigurovány Azure AD a v aplikaci, uživatelům nebo skupinám byly přiřazeny aplikace v Azure a deklarace byly zkontrolovány a upravovat v případě potřeby a potom uživatel bude připravená k přihlášení do aplikace. 

Chcete-li otestovat, jednoduše přihlásit se k panelu Azure AD přístup na https://myapps.microsoft.com pomocí uživatelský účet, který jste přiřadili s aplikací a potom klikněte na dlaždici aplikace a spusťte tak proces přihlášení. Případně můžete procházet přímo na adresu URL přihlašování pro aplikaci a přihlašovací odtud. 

Ladění tipy, najdete v [článku o tom, jak ladění na základě SAML jednotného přihlašování k aplikacím](active-directory-saml-debugging.md) 

##<a name="password-single-sign-on"></a>Heslo jednotné přihlašování

Tato možnost slouží ke konfiguraci [založená na heslech jednotného přihlašování](active-directory-appssoaccess-whatis.md) pro webovou aplikaci, která obsahuje přihlašovací stránku HTML. Na základě heslo SSO někdy označovány jako heslo vaulting, umožňuje řízení přístupu uživatelů a hesla pro webové aplikace, které nepodporují federaci identity. Je vhodné také scénáře, kdy je potřeba několik uživatelů sdílet jednoho účtu, například s účty aplikace sociální sítí vaší organizace. 

Po výběru **Další**, zobrazí se výzva k zadání adresa URL aplikace založené na webu přihlašovací stránky pro. Všimněte si, že to musí být stránku, která obsahuje vstupních polí uživatelské jméno a heslo. Po zadání, Azure AD spustí proces analyzovat přihlašovací stránku pro zadávání uživatelské jméno a heslo pro zadávání. Pokud tento proces není úspěšné, potom ho vás provede alternativní proces instalace (vyžaduje Internet Explorer, Chrome a Firefox) rozšíření prohlížeče, které vám umožní ručně zachytit pole.

Jakmile zachytávání přihlašovací stránku Uživatelé a skupiny musí mít přiřazené může a zásady přihlašovacích údajů se dá nastavit stejně jako běžný [heslo jednotné přihlašování aplikace](active-directory-appssoaccess-whatis.md).

Poznámka: Můžete nahrát logo dlaždice aplikace na kartě **Konfigurovat** pomocí tlačítka **Nahrát Logo** aplikace. 

##<a name="existing-single-sign-on"></a>Existující jednotné přihlašování

Tato možnost slouží k přidání odkazu na aplikaci portál Panel Azure AD přístup nebo Office 365 vaší organizace. Můžete to přidat odkazy na vlastní webová aplikace, které aktuálně použít Azure Active Directory Federation Services (nebo jiné služby federace) místo Azure AD pro ověřování. Nebo můžete přidat hloubkové odkazy na určité stránky Sharepointu nebo jiných webových stránek, které chcete zobrazit na panely přístupu uživatele. 

Po výběru **Další**, budete vyzváni k zadejte adresu URL aplikace, pokud chcete vytvořit odkaz. Po dokončení, může s aplikací, který způsobí, že aplikace se zobrazují ve [Spouštěči aplikací Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) nebo [panel přístup Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) pro tyto uživatele přiřazené uživatelům a skupinám.

Poznámka: Můžete nahrát logo dlaždice aplikace na kartě **Konfigurovat** pomocí tlačítka **Nahrát Logo** aplikace.

## <a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Jak přizpůsobit deklarací Vystavitel v tokenu SAML předem integrované aplikace](active-directory-saml-claims-customization.md)
- [Poradce při potížích na základě SAML jednotné přihlašování](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
