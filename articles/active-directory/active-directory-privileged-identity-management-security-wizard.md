<properties
   pageTitle="Průvodce zabezpečením Azure AD privilegovaných Správa identit"
   description="Při prvním použití koncovku Azure Active Directory privilegovaných Správa identit se zobrazí se Průvodce zabezpečením. Tento článek popisuje kroky pro použití průvodce."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>Průvodce zabezpečením Azure AD privilegovaných Správa identit

Pokud jste první osoba, která spuštění Azure privilegovaných Identity Správa osobních pro vaši organizaci, zobrazí se průvodce. Průvodce vám pomůže pochopit zabezpečení rizikové privilegovaných identit a používání osobních zmenšit tato rizika. Nemusíte provádět žádné změny stávajících přiřazení rolí v průvodci, pokud chcete později.

## <a name="what-to-expect"></a>Co můžete očekávat

Před spuštěním vaše organizace pomocí správce osobních informací, budou všechny přiřazování rolí trvalé: uživatelé jsou vždy v těchto rolích i v případě, že právě nepotřebují jejich oprávnění.  Cílem prvního kroku průvodce se zobrazí seznam maximum oprávněními rolí a počet uživatelů, kteří jsou v současnosti v těchto rolích. Můžete přejít na konkrétní rolí Další informace o uživatelích, pokud jeden nebo více z nich jsou cizí.

Druhý krok průvodce nabízí možnost změnit přiřazení rolí správců.  

> [AZURE.WARNING]Je důležité mít aspoň jeden globální správce a víc než jeden privilegovaných role správce s účtem organizace (ne účet Microsoft). Pokud existuje pouze jeden privilegovaných roli správce, nepůjde organizace pro správu osobních odstranili tohoto účtu.
> Navíc zachovat přiřazování rolí trvalé, pokud má uživatel účet Microsoft (účet, který používá pro přihlášení ke službám Microsoft jako Skype nebo Outlook.com). Pokud chcete vyžadovat MFA pro aktivaci pro tuto roli, tento uživatel zablokovaní.


Po provedení změn v Průvodci se už zobrazí. Při příštím vy nebo jiné privilegovaných role správce použití správce osobních informací, zobrazí se na řídicím panelu Správce osobních informací.  

- Pokud chcete přidat nebo odebrat uživatele z role nebo změna přiřazení z trvalé, aby měl, přečtěte si další na to, [jak přidat nebo odebrat role uživatele](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
- Pokud chcete uživatelům další přístup ke správě správce osobních informací, přečtěte si další informace na to, [jak umožnit přístup k zařídit správce osobních informací](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).



## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
