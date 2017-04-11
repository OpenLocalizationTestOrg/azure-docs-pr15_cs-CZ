<properties
    pageTitle="Vlastní skupiny v Azure Active Directory | Microsoft Azure"
    description="Základní informace o způsobu vyhrazené skupiny pracovat v Azure Active Directory a jak se vytvářejí."
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
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="dedicated-groups-in-azure-active-directory"></a>Vyhrazené skupin v Azure Active Directory

V Azure Active Directory (Azure AD) funkce vyhrazené skupiny automaticky vytvoří a naplní členství ve skupinách Azure AD předdefinované. Nejde přidat nebo odebrat pomocí Azure klasické portál, rutin prostředí Windows PowerShell členů vyhrazené skupin nebo programově.

>[AZURE.NOTE] Vyhrazené skupiny vyžaduje přiřazená licence na Azure AD Premium
>- správce, kdo vám spravuje pravidlo ve skupině
>- všichni uživatelé, kteří jsou vybrány pravidlem být členem skupiny

**Chcete-li povolit vyhrazené skupiny**

1. [Azure klasické portál](https://manage.windowsazure.com)vyberte **Služby Active Directory**a pak otevřete adresář vaší organizace.

2. Vyberte kartu **skupiny** a pak otevřete skupinu, kterou chcete upravit.

3. Vyberte kartu **Konfigurovat** a pak nastavte **Povolit vlastní skupiny** na hodnotu **Ano**.

Když přepínačem Povolit vyhrazené skupiny je nastavená na hodnotu **Ano**, můžete dál povolíte adresáři automaticky vytvořit skupiny vyhrazené All Users nastavením **Povolit "Všichni" skupiny** přepnout na hodnotu **Ano**. Můžete pak taky upravovat název této vyhrazené skupiny tak, že zadáte v **zobrazované jméno "Všichni" skupiny** pole.

Skupina všichni uživatelé mohou sloužit k přiřazení stejná oprávnění pro všechny uživatele v adresáři. Můžete třeba udělte všem uživatelům v adresáři přístup k aplikaci SaaS přiřazením přístup pro všechny uživatele vyhrazené skupinu do této aplikace.

Vyhrazené skupiny všem uživatelům zahrnuje všechny uživatele v adresáři, včetně hostů a externích uživatelů. V případě potřeby skupinu, která nezahrnuje externí uživatele a pak se dají dělat s vytvořením skupiny s podle atributů dynamické pravidlo následující:

                (user.userPrincipalName -notContains "#EXT#@")

Pro skupinu nezahrnuje všechny hostů pomocí pravidla třeba takto:

                (user.userType -ne "Guest")

Další informace o tom, jak vytvořit *složitější* pravidla (pravidla, která může obsahovat více porovnání) pro dynamické členství ve skupinách najdete v tématu [použití atributů do vytvářet složitější pravidla](active-directory-accessmanagement-groups-with-advanced-rules.md).


Tyto články poskytují další informace o Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
* [Co je Azure Active Directory?](active-directory-whatis.md)
* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
