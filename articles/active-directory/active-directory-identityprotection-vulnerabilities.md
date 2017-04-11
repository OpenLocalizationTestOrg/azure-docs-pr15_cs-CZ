<properties
    pageTitle="Chyby detekovaný ochranou Azure Active Directory Identity | Microsoft Azure"
    description="Základní informace o chyby detekovaný ochranou Azure Active Directory Identity."
    services="active-directory"
    keywords="Ochrana identity služby Azure active directory, zjišťování aplikace cloudu, Správa aplikací, zabezpečení, rizika, úroveň rizika, chyba, zásady zabezpečení"
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
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Chyby detekovaný ochranou Azure Active Directory Identity 

Chyby jsou slabých v prostředí, můžete využívat zlými úmysly. Doporučujeme, abyste řešení těchto chyb zlepšit postoje zabezpečení vaší organizace a zabránit útočníků využívání je.
<br><br>
![chyby](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")
<br>

Následující části obsahují přehled chyby vykázaného ochranou Identity.

## <a name="multi-factor-authentication-registration-not-configured"></a>Registrace vícefaktorové ověřování není nakonfigurováno 

Tuto chybu umožňuje určit nasazení Azure Vícefaktorové ověřování ve vaší organizaci. 

Azure vícefaktorové ověřování obsahuje druhý úroveň zabezpečení pro ověřování uživatelů. Ho pomáhá chránit přístup k datům a aplikace během schůzky požádání uživatele pro jednoduchý proces přihlášení. Poskytuje silných ověřovat pomocí celou řadu možností snadno ověření – telefonní hovor, textová zpráva nebo mobilní aplikaci oznámení nebo ověření kód a 3rd stran MÍSTOPŘÍSEŽNÉM tokeny.

Doporučujeme vyžadovat Azure Vícefaktorové ověřování pro přihlášení uživatele. Vícefaktorové ověřování přehraje klíčovou roli v podmíněné přístupu na základě rizika zásady dostupné prostřednictvím ochrana Identity.

Další informace najdete v tématu [Co je Azure Vícefaktorové ověřování?](../multi-factor-authentication/multi-factor-authentication.md)


## <a name="unmanaged-cloud-apps"></a>Aplikace nespravované cloudu

Tuto chybu budete moci určit nespravované cloudu aplikace ve vaší organizaci.
 
V moderní podniky IT oddělení, jsou často všech aplikací cloudu používajících své práci uživateli z jejich organizace neví. Není těžké si najdete v článku proč Správci by vás neoprávněnému přístupu podnikových dat, možná data únik a dalšími riziky zabezpečení. 

Doporučujeme, aby vaše organizace nasazení cloudu aplikace zjišťování Seznamte se s aplikací nespravované cloudu a spravovat tyto aplikace pomocí služby Azure Active Directory.

Další informace najdete v článku [hledání nespravované cloudové aplikace s zjišťování aplikace cloudu](active-directory-cloudappdiscovery-whatis.md).



##<a name="security-alerts-from-privileged-identity-management"></a>Správa privilegovaných identit výstrahy zabezpečení

Tuto chybu vám pomůže objevit a vyřešit upozornění o privilegovaných identit ve vaší organizaci.  

Povolit uživatelům provádění privilegovaných operací, třeba udělit uživatelům dočasné nebo trvalé přístup privilegovaných v Azure AD organizace Azure nebo Office 365 zdrojů nebo jiných aplikací SaaS. Každá z těchto privilegovaných uživatelů zvyšuje napadení vaší organizace. Tuto chybu vám pomůže identifikovat uživatele s nepotřebných přístup privilegovaných a proveďte odpovídající akce ke snížení nebo odstranění rizika, které představují. 

Doporučujeme, abyste vaše organizace používá Azure AD privilegovaných Správa identit ke správě ovládací prvek, a sledování privilegované identit a jejich přístup ke zdrojům v Azure AD, jakož i ostatní online služby jako je Office 365 nebo Microsoft Intune.

Další podrobnosti najdete v tématu [Správa Azure AD privilegovaných identit](active-directory-privileged-identity-management-configure.md). 



## <a name="see-also"></a>Viz taky

 - [Ochrana Identity Azure Active Directory](active-directory-identityprotection.md)
