<properties
    pageTitle="Azure Active Directory Identity Protection oznámení | Microsoft Azure"
    description="Zjistěte, jak oznámení podporují vašich aktivit najdete vyšetřování."
    services="active-directory"
    keywords="Ochrana identity služby Azure active directory, zjišťování aplikace cloudu, Správa aplikací, zabezpečení, rizika, úroveň rizika, chyba, zásady zabezpečení"
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

#<a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection oznámení 


Ochrana Azure AD Identity odešle dva typy automatické oznámení e-mailů pro správu riziko uživatele a rizik událostí:

- Uživatel ohroženo upozornění e-mailu

- Týdenní digest e-mailu

## <a name="user-compromised-alert-email"></a>Uživatel ohroženo upozornění e-mailu

Výstraha hostují e-mailu dojde ochrana Azure AD Identity identifikuje účet jako ohroženo. E-mail obsahuje odkaz uživatelům příznakem pro rizika sestavu v řídicím panelu ochrana Identity. Doporučujeme okamžitě prošetřit oznámení o ohroženo.


## <a name="weekly-digest-email"></a>Týdenní digest e-mailu

Týdenní digest e-mail obsahuje souhrn nové zvláštní události rizika.<br>
Zahrnuje:

- Uživatelé na rizika
- Podezřelých aktivit
- Zjištěné chyby
- Odkazy na související sestav v ochrana Identity


<br>
![Remediation](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br> 

Můžete přepínat odeslání týdenní e-mailu digest.
<br><br>
![Uživatel rizika](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>
 

**Otevřete dialogové okno související konfigurace**:

1. Na **ochranu Azure AD Identity** zásuvné klikněte na **Nastavení**.
<br><br>
![Zásady pro uživatele rizika](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
<br>

2. V části **Obecné** klikněte na **upozornění**.
<br><br>
![Zásady pro uživatele rizika](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
<br>




## <a name="see-also"></a>Viz taky

- [Ochrana Identity Azure Active Directory](active-directory-identityprotection.md) 

