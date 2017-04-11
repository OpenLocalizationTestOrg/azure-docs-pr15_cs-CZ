<properties
   pageTitle="Zobrazení aplikace v galerii aplikace služby Azure Active Directory"
   description="Jak získat seznam aplikace, která podporuje jednotné přihlašování v galerii Azure Active Directory | Microsoft Azure"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Zobrazení aplikace v galerii aplikace služby Azure Active Directory

Seznam aplikace, která podporuje jednotné přihlašování s Azure Active Directory v [Galerii Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/), nejdřív potřebuje implementovat jednu z těchto režimů integrace:

* **Připojení OpenID** - přímé integrace se službou Azure AD pomocí OpenID připojení pro ověřování a rozhraní API souhlas Azure AD pro konfiguraci. Pokud spouštíte jenom integrační a aplikace nepodporuje SAML, jedná se o doporučená režim.

* **SAML** – aplikace už má možnost konfigurovat pomocí protokolu SAML poskytovatelů identit třetích stran.

Požadavky na výpis u jednotlivých režimech jsou níže.

##<a name="openid-connect-integration"></a>OpenID připojení integrace

Integrace aplikace služby Azure AD, postupujte podle [pokynů vývojář](active-directory-authentication-scenarios.md). Potom vyplňte následující otázky a odešlete waadpartners@microsoft.com.

* Zadejte přihlašovací údaje pro klienta test nebo účet s aplikací, které mohou sloužit k testování integrace týmem Azure AD.  

* Obsahují pokyny na tom, jak můžete týmu Azure AD přihlášení a připojení instance Azure AD pomocí [Azure AD souhlas framework](active-directory-integrating-applications.md#overview-of-the-consent-framework)aplikaci. 

* Pokyny libovolné další požadované pro tým Azure AD otestovat, jednotné přihlašování s aplikací. 

* Zadejte následující informace:

> Název společnosti:
> 
> Web společnosti:
> 
> Název aplikace:
> 
> Popis aplikace (omezení 256 znaků):
> 
> Web aplikace (informační):
> 
> Web aplikace technické podpory nebo informace o kontaktu:
> 
> ID klienta aplikace, jak je vidět v podrobnostech aplikace na https://manage.windowsazure.com:
> 
> Adresa URL aplikace poskytovateli kde moct přihlásit k zákazníci a/nebo koupit aplikace:
> 
> Zvolte až tři kategorie pro aplikace uvedené v části (pro dostupných kategorií najdete v článku Azure Active Directory Marketplace):
> 
> Malá ikona aplikace (soubor PNG, 45px tak, že 45px plnou barvou pozadí) připojení:
> 
> Připojení velké ikony aplikace (soubor PNG, 215px tak, že 215px plnou barvou pozadí):
> 
> Připojení aplikace Logo (soubor PNG, 150px tak, že 122px, barvy pozadí):

##<a name="saml-integration"></a>Integrace SAML

Libovolné aplikaci, která podporuje SAML 2.0 lze integrovat přímo s klientem Azure AD pomocí [tyto pokyny pro přidání vlastních aplikací](active-directory-saas-custom-apps.md). Po testování integraci aplikace spolupracuje Azure AD odeslat tyto informace do <waadpartners@microsoft.com>.

* Zadejte přihlašovací údaje pro klienta test nebo účet s aplikací, které mohou sloužit k testování integrace týmem Azure AD.  

* Obsahují hodnoty (výraz zákaznické služby) SAML přihlašování adresy URL, Vystavitel URL (entity ID) a adresu URL odpověď aplikace, jako popsaných [v tomto poli](active-directory-saas-custom-apps.md). Zadáte obvykle tyto hodnoty jako součást soubor metadat SAML, pak pošlete nám prosím, stejně.

* Uveďte krátký popis toho, jak nakonfigurovat Azure AD jako zprostředkovatelem identit v aplikaci pomocí SAML 2.0. Pokud vaše aplikace podporuje konfigurace Azure AD jako zprostředkovatelem identit prostřednictvím portálu pro správu samoobslužných funkcí, pak zkontrolujte, zda přihlašovací údaje pomocí výše uvedeného patří možnost nastavit.

* Zadejte následující informace:

> Název společnosti:
> 
> Web společnosti:
> 
> Název aplikace:
> 
> Popis aplikace (omezení 256 znaků):
> 
> Web aplikace (informační):
> 
> Web aplikace technické podpory nebo informace o kontaktu:
> 
> Adresa URL aplikace poskytovateli kde moct přihlásit k zákazníci a/nebo koupit aplikace:
> 
> Zvolte až tři kategorie pro aplikace uvedené v části (pro dostupných kategorií najdete v článku [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Malá ikona aplikace (soubor PNG, 45px tak, že 45px plnou barvou pozadí) připojení:
> 
> Připojení velké ikony aplikace (soubor PNG, 215px tak, že 215px plnou barvou pozadí):
> 
> Připojení aplikace Logo (soubor PNG, 150px tak, že 122px, barvy pozadí):
