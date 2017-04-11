<properties
    pageTitle="Nastavení služby Azure Active Directory pro správu přístupu aplikace Samoobslužné | Microsoft Azure"
    description="Správa skupiny samoobslužné umožňuje uživatelům k vytváření a Správa skupin zabezpečení a skupin Office 365 na Azure Active Directory a nabízí možnost skupiny zabezpečení žádost nebo členství ve skupině služeb Office 365 uživatelům"
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
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Nastavení služby Azure Active Directory pro správu samoobslužné skupiny

Správa skupiny samoobslužných funkcí umožňuje uživatelům k vytváření a Správa skupin zabezpečení a skupin Office 365 na Azure Active Directory (Azure AD). Můžete taky vyžádání skupina zabezpečení nebo členství ve skupině služeb Office 365 a pak můžete vlastník skupiny schválit nebo zamítnout členství. Tímto způsobem můžete delegovat každodenní řízení členství ve skupinách uživatelům, kteří Principy kontextu business pro členství. Funkce správy samoobslužné skupiny jsou k dispozici pouze pro skupiny zabezpečení a skupiny služeb Office 365, ale ne pro skupiny zabezpečení s podporou poš nebo distribuční seznamy.

Správa skupiny samoobslužných funkcí v současné době zahrnuje dva základní scénáře: delegované správy skupiny a správa samoobslužné skupiny.

- **Delegované správy skupiny** 
   příkladu je správce, který je Správa přístup k aplikaci SaaS používající technologii společnosti. Správa těchto přístupových práv se náročný, abyste tento správce dotazem vlastníka firmy k vytvoření nové skupiny. Správce přiřadí přístupu pro aplikace do nové skupiny a přidá do skupiny všem uživatelům už přístup k aplikaci. Obchodní vlastník pak můžete přidat více uživatelů a tito uživatelé jsou automaticky zřízení aplikaci. Vlastník firmy nevyžaduje potřeba počkat, správce pro správu přístupu pro uživatele. Pokud správce udělí stejná oprávnění k odpovědi ve skupině podniková, pak tuto osobu můžete spravovat přístup pro svoje uživatele. Vlastník firmy ani vedoucí můžete zobrazit nebo Správa ostatních uživatelů. Správce vám dál zobrazovat všichni uživatelé, kteří mají přístup k aplikaci a blok přístupových práv v případě potřeby.

- **Správa skupiny samoobslužné** 
   příkladem tento scénář je dva uživatelé, kteří obou webech Sharepointu Online, které je nastavit nezávisle na sobě. Budou chcete dát přístup týmy dalších uživatelů k jejich webům. K tomu můžete vytvořit jednu skupinu v Azure AD a ve službě SharePoint Online každý z nich slouží k výběru danou skupinu zajistit přístup k jejich webům. Když vám někdo chce přístup, požádají z panelu přístup a po schválení budou automaticky získat přístup k obou webech Sharepointu Online. Později jedna z nich rozhoduje o, že všichni uživatelé přístup k webu taky získat přístup pro konkrétní aplikaci SaaS. Správce aplikace SaaS můžete přidat přístupových práv pro aplikaci na web služby SharePoint Online. Od požadavky, které zobrazí schválené vám přístup na dvě weby Sharepointu Online a taky k této aplikaci SaaS.

## <a name="making-a-group-available-for-end-user-self-service"></a>Zpřístupnění skupiny pro koncového uživatele samoobslužných funkcí

1. [Azure klasické portál](https://manage.windowsazure.com)otevřete Azure AD adresář.

2. Na kartě **Konfigurovat** nastavení **delegovaná Správa skupiny** povoleno.

3. Nastavení **mohou uživatelé vytvářet skupiny zabezpečení** nebo **mohou uživatelé vytvářet skupin Office** povoleno.

Při zapnuté funkci **že mohou uživatelé vytvářet skupiny zabezpečení** , všechny uživatele v adresáři jsou povoleny k vytvoření nových skupin zabezpečení a o přidávání členů do těchto skupin. Tyto nových skupin by také objeví v panelu aplikace Access pro všechny ostatní uživatele. Pokud nastavení zásad skupiny umožňuje, můžete vytvořit jiní uživatelé žádosti o připojení se tyto skupiny. Pokud **Uživatelé můžete vytvářet skupiny zabezpečení** zakázané, uživatelé nelze vytvářet skupiny a nelze změnit existující skupiny, ke kterým jsou některé vlastníky. Můžete však stále Správa členství skupin a schválení žádosti o od jiných uživatelů ke jejich skupin.

Taky můžete **uživatelům, kteří používají samoobslužné skupin zabezpečení** dosáhnout více jemně odstupňovaná přístup ovládat Správa samoobslužné skupiny pro uživatele. Při zapnuté funkci **že mohou uživatelé vytvářet skupiny** , jsou povoleny všechny uživatele v adresáři k vytvoření nových skupin a o přidávání členů do těchto skupin. Nastavením také **Uživatelé, kteří používají samoobslužné skupin zabezpečení** do některé, jste omezení skupina Správa na pouze omezenou skupinu uživatelů. Pokud tento přepínač nastavena na některé, třeba přidání uživatelů do skupiny SSGMSecurityGroupsUsers před můžete vytvořit novou skupinu a k nim přidat členy. Nastavením **uživatelů, kteří používají samoobslužné skupin zabezpečení** u všech povolit všem uživatelům v adresáři vašeho k vytvoření nových skupin.

Pole **skupiny, které můžete použít samoobslužné skupin zabezpečení** můžete taky zadat vlastní název skupiny, jejichž členy můžete použít samoobslužné.

## <a name="additional-information"></a>Další informace

Tyto články poskytují další informace o Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)

* [Azure Active Directory rutiny pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)

* [Co je Azure Active Directory?](active-directory-whatis.md)

* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
