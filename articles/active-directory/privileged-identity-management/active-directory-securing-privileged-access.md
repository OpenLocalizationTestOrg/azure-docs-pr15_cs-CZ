<properties
    pageTitle="Zabezpečení privilegovaných přístupu v Azure AD | Microsoft Azure"
    description="Téma, které vysvětluje postupy pro zabezpečení privilegovaných přístupu přes Azure, Azure Active Directory a službám Microsoft Online Services."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>Zabezpečení privilegovaných přístupu v Azure AD

Zabezpečení privilegovaných přístup je důležitým prvním krokem se můžete pomoci chrá majetku firmy v moderní organizace. Privilegovaných účty používané spravovat a Správa IT systémy. Internetovým časově útočníků zacílit tyto účty k získání přístupu k datům a systémy organizace. Bezpečná přístup privilegovaných měli izolovat účty a systémy z rizik napadení škodlivým uživateli.

Více uživatelů začínají přístup privilegovaných přes cloud services. To může obsahovat globální správci Office 365, správce Azure předplatné a uživatelé, kteří mají přístup pro správu v VMs nebo na SaaS aplikace.

Microsoft doporučuje sledovat tento návod [zabezpečení privilegovaných](https://technet.microsoft.com/library/mt631194.aspx)přístup.

Zákazníkům, kteří používají Azure Active Directory spravovat přístup k Azure, Office 365 nebo jiné služby společnosti Microsoft a aplikací tyto zásady platí pro uživatelské účty spravovaných a ověření službou Active Directory nebo v Azure Active Directory. Další informace o funkcích Azure AD pro podporu zabezpečení privilegovaných přístupu v následujících oddílech.

## <a name="multi-factor-authentication"></a>Vícefaktorové ověřování

Pokud chcete zvýšit zabezpečení Správce ověřování, je potřeba povolit vícefaktorové ověřování (MFA) před udělením oprávnění. MFA je způsob, jak ověřit, kdo si, že je potřeba použít větší kontrolu nad uživatelské jméno a heslo. Poskytuje druhou vrstvu zabezpečení a uživatel přihlášení, začátky transakce.

Azure Vícefaktorové ověřování pomáhá chránit přístup k datům a aplikace během schůzky požádání uživatele pro jednoduchý proces přihlášení. Poskytuje silných ověřovat pomocí celou řadu možností snadno ověření včetně telefonní hovory, textové zprávy, oznamování v mobilní aplikaci, nebo ověřovací kód a 3 tokeny MÍSTOPŘÍSEŽNÉM stran.

Základní informace o fungování Azure Vícefaktorové ověřování naleznete v následujícím videu.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

Další informace najdete v tématu [MFA pro Office 365 a MFA Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Čas vazbou oprávnění

V některých organizacích zjistit, že máte příliš mnoho uživatelů v vysoce privilegovaných role. Uživatel může byly přidány roli pro konkrétní činnosti, jako je zaregistrovat ke službě, ale často potom nepoužili tato oprávnění.

Pokud chcete snížit čas oprávnění a zlepšení viditelnosti do jejich použití, omezit uživatelům pouze provedením u jejich oprávnění ještě v času (za běhu), po kterou potřebují k provedení úkolu. Azure Active Directory a službám Microsoft Online Services můžete použít [Azure AD privilegovaných Správa identit (osobních)](http://aka.ms/AzurePIM).


![Řídicí panel Správce osobních informací][2]


## <a name="attack-detection"></a>Útok zjišťování

[Ochrana Azure Active Directory Identity](../active-directory-identityprotection.md) nabízí konsolidované zobrazení do rizika událostí a potenciální chyby došlo k ovlivnění identit vaší organizace. Ochrana Identity na základě rizika událostí, vypočítá úroveň rizika uživatelů pro každého uživatele, umožňují konfigurovat zásady založené na rizika automaticky chránit identit vaší organizace. Tyto zásady spolu s ovládacích prvků podmíněného přístupu poskytované Azure Active Directory a EMS, můžete automaticky blokovat uživatele nebo nabízejí návrhů, které obsahují resetování hesla a prosazování vícefaktorové ověřování.

![Ochrana Azure AD Identity][3]

## <a name="conditional-access"></a>Podmíněný přístup

Řízení přístupu podmíněné zkontroluje Azure Active Directory určité podmínky, jaká jste při ověřování uživatele, před který umožňuje přístup k aplikaci. Když jsou splněné tyto podmínky, uživatel ověření a povolený přístup k aplikaci.


![Nastavení pravidel podmíněného přístupu s MFA][4]


## <a name="related-articles"></a>Související články

- Povolení [Azure Vícefaktorové ověřování](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
- Povolení [správy identit Azure AD oprávněními](../active-directory-privileged-identity-management-configure.md)
- Povolte [ochranu Azure AD Identity](../active-directory-identityprotection.md)
- Povolení [ovládacích prvků podmíněného přístupu](../active-directory-conditional-access.md)


Další informace o vytváření Přehled zabezpečení naleznete v části "povinnosti zákazníka a materiálů" [Zabezpečení cloudu společnosti Microsoft pro organizace tvůrci](http://aka.ms/securecustomer) dokumentu. Další informace o atraktivních službu pro pomoc s některou z těchto témat kontaktujte zástupce Microsoftu nebo navštivte naše [počítačové bezpečnosti řešení stránky](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
