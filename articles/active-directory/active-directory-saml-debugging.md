<properties 
    pageTitle="Ladění na základě SAML jednotného přihlašování k aplikacím služby Azure Active Directory | Microsoft Azure" 
    description="Naučte se ladění na základě SAML jednotného přihlašování k aplikacím služby Azure Active Directory " 
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

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Ladění na základě SAML jednotného přihlašování k aplikacím služby Azure Active Directory

Při integraci aplikace založené na SAML ladění, je často vhodné používat nástroje, jako je [Fiddler](http://www.telerik.com/fiddler) zobrazíte SAML žádost, SAML odpověď a skutečné token SAML spuštěný s aplikací. Prozkoumáním SAML token zajistíte, že všechny požadované atributy, uživatelské jméno v předmětu SAML a Vystavitel URI pochází podle očekávání.

![][1]

Odpověď z Azure AD, který obsahuje SAML token se obvykle myslí, který bude proveden poté, co HTTP 302 přesměrovat z https://login.windows.net a odeslaný do konfigurované **Odpovědět URL** aplikace. 
 
SAML token můžete zobrazit tak, že vyberete tento řádek vyberete **kontroly > webových formulářích** kartu na pravém panelu. Z tohoto umístění klikněte pravým tlačítkem na hodnotu **SAMLResponse** a vyberte **Poslat TextWizard**. Vyberte z nabídky **transformace** dekódovat tokenu a zobrazit její obsah **Z ve formátu Base 64** .
 
**Poznámka**: zobrazíte obsah tohoto požadavku HTTP Fiddler můžete být vyzváni ke konfiguraci dešifrování HTTPS přenosy v síti, která bude potřeba udělat.

## <a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Konfigurace jednotného přihlašování k aplikacím, které nejsou v galerii aplikace služby Azure Active Directory](active-directory-saas-custom-apps.md)
- [Jak přizpůsobit deklarací Vystavitel v tokenu SAML předem integrované aplikace](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
