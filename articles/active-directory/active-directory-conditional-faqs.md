<properties
    pageTitle="Azure Active Directory podmíněného přístupu nejčastější dotazy týkající se | Microsoft Azure"
    description="Nejčastější dotazy týkající se podmíněného přístupu "
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-faq"></a>Azure Active Directory podmíněného přístupu časté otázky

## <a name="which-applications-work-with-conditional-access-policies"></a>Aplikace, které fungují zásady přístupu podmíněné?

**A:** Najdete v tématu [podmíněné aplikace access co jsou podporovány](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Zásady přístupu podmíněné zdědí B2B spolupráce a uživatele typu Host?

**A:** Platí zásady pro uživatele B2B spolupráce. V některých případech nemusí být moct splňují požadavek zásad Pokud například organizace podporuje vícefaktorové ověřování uživatele. 

Zásady není aktuálně používá pro uživatele typu Host SharePoint. Vztah hosta je zachován v rámci služby SharePoint. Účty nejsou vyměřené poplatky za jeho přístupu uživatele typu Host zásady ověřování serveru. Dá se ovládat přístup Guest na SharePoint.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>SharePoint Online zásady také platí pro OneDrive pro firmy?

**A:** Ano.
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Proč nemůžu nastavit zásady na klientské aplikace, jako je Word nebo Outlook?

**A:** Podmíněné přístupu Nastaví požadavky pro přístup ke službě a v situacích, kdy ověřování probíhá této službě. Není nastavená zásada přímo na klientské aplikace; Místo toho použije se při volání do služby. Například zásadu nastavené pro službu SharePoint platí pro klienty volající SharePoint a zásadám nastaveným na Exchange platí pro aplikace Outlook.


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Použití podmíněného přístupu účtů služeb?

**A:** Zásady podmíněného přístupu platí pro všechny účty. Jedná se o uživatelských účtů používat jako účty služeb. V mnoha případech, které se spouští bezobslužného účtu služby nedokáže splňovat zásadu. Toto je, například v případě, když požaduje MFA. V těchto případech můžete účty služby vyloučené ze zásad použití nastavení zásad správy podmíněného přístupu. Další informace o použití zásad pro uživatele tady.
