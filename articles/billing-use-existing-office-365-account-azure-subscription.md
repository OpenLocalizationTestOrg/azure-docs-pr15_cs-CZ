<properties
    pageTitle="Sdílení jednoho klienta Azure AD u předplatných Office 365 a Azure | Microsoft Azure"
    description="Naučte se sdílet vašeho klienta Office 365 Azure AD a její uživatele k předplatnému Azure nebo naopak"
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Použití existujícího účtu Office 365 s předplatným Azure nebo naopak
Scénář: Už máte předplatné Office 365 a je připraven pro předplatné Azure, ale chcete použít existující uživatelské účty Office 365 u předplatného Azure. Můžete taky jste Azure odběratele a chcete získat předplatné Office 365 pro uživatele ve vaší stávající Azure Active Directory. V tomto článku se dozvíte, jak je snadné oběma.

> [AZURE.NOTE] Tento článek neplatí pro zákazníky Enterprise Agreement (EA). Pokud potřebujete další pomoc kdykoli v tomto článku, [Kontaktujte podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) problém vyřešit rychle.


## <a name="quick-guidance"></a>Stručné pokyny

- Pokud už máte předplatné Office 365 a chcete se přihlásit k Azure, použijte možnost **Přihlaste se pomocí účtu organizace** . Pokračujte Azure proces registrace s účtem Office 365. Zobrazit [Podrobný postup dál v tomto článku](#s1).

- Pokud už máte předplatné Azure a chcete předplatné Office 365, přihlaste se k Office 365 pomocí účtu Azure. Pokračujte kroky registrace. Po dokončení přihlašování, předplatné Office 365 se přidá na stejné instanci služby Azure Active Directory, která předplatného Azure patří. Další informace najdete v tématu oddíl [Podrobný postup dál v tomto článku](#s2).

>[AZURE.NOTE] Předplatné Office 365 získáte účet používáte pro registrace musí být členem skupiny adresáře roli globálního správce nebo správce fakturace ve vašem klientovi Azure Active Directory. [Zjistěte, jak lze zjistit roli v Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

Pokud chcete zjistit, co se stane, když přidáte účet předplatného, najdete v článku [základní informace dále v tomto článku](#background-information).

## <a name="detailed-steps"></a>Podrobný postup
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>Scénář 1: Office 365 uživatelů, kteří plánujete koupit Azure
V tomto případě jsme použijeme že Kelley zdi je uživatel, který má předplatné Office 365 a plánuje přihlášení k odběru Azure. Existují dva další aktivní uživatelé, tereza a Patricie. Účet společnosti Kelley admin@contoso.onmicrosoft.com.

![Centrum pro správu uživatelů Office 365](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Abyste si zaregistrovali pro Azure, postupujte takto:

1. Zaregistrujte Azure na [Azure.com](https://azure.microsoft.com/). Klikněte na **akci zdarma**. Na další stránce klikněte na **Začít nyní**.

    ![Zkuste Azure zdarma.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Klikněte na tlačítko **přihlásit pomocí účtu organizace**.

    ![Přihlaste se k Azure.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Přihlaste se pomocí účtu Office 365. V tomto případě je Kelley na účtu Office 365.

    ![Přihlaste se pomocí účtu Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. Vyplňte informace a dokončete proces registrace.

    ![Vyplňte informace a dokončete registrace.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![Klikněte na tlačítko Start Správa Moje služby.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Nyní máte všechno nastavené. Na portálu Azure byste měli vidět uživatelům zobrazoval. K ověření, postupujte takto:

1. Klikněte na tlačítko **Start, Správa Moje služby** v obrazovku dříve zobrazenou.
2. Klikněte na tlačítko **Procházet**a pak klikněte na **Služby Active Directory**.

    ![Klikněte na tlačítko Procházet a pak klikněte na služby Active Directory.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klikněte na **uživatele**.

    ![Na kartě Uživatelé](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Všichni uživatelé, včetně Kelley, jsou uvedené podle očekávání.

    ![Seznam uživatelů](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>Scénář 2: Azure uživatelé plánujete koupit Office 365

V tomto scénáři Kelley zdi je uživatel, který má předplatné Azure pod účtem admin@contoso.onmicrosoft.com. Kelley chce pořídí předplatné Office 365 a použít stejné adresář, který uživatel již s Azure.

>[AZURE.NOTE] Předplatné Office 365 získáte účet, který používáte pro přihlášení musí být členem adresáře roli globálního správce nebo správce fakturace ve vašem klientovi Azure Active Directory. [Zjistěte, jak najdu roli v Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

![Nastavení Azure portálu předplatného](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Azure Active Directory uživatele portálu](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Pokud chcete přihlásit do Office 365, postupujte takto:

1. Přejděte na [stránku produkt Office 365](https://products.office.com/business)a pak vyberte plán, který je pro vás vhodná.
2. Jakmile vyberete požadovaný plán, se zobrazí na následující stránce. Není vyplňte formulář. V pravém horním rohu stránky klepněte na **přihlásit** .

    ![Stránka zkušební verzi Office 365](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Přihlaste se pomocí svých přihlašovacích údajů účtu. V tomto příkladu je Kelley na účet.

    ![Přihlášení k Office 365](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Klikněte na **vyzkoušet nyní**.

    ![Potvrzení objednávky pro Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. Na stránce potvrzení objednávky klikněte na **pokračovat**.

    ![Potvrzení objednávky Office 365](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Nyní máte všechno nastavené. V Centru pro správu Office 365 byste měli vidět uživatelů v adresáři společnosti Contoso zobrazuje jako aktivní uživatelé. K ověření, postupujte takto:

1. Otevřete Centrum pro správu Office 365.
2. Rozbalte **uživatelů**a potom klikněte na **Aktivní uživatelé**.

    ![Uživatelé Centrum pro správu Office 365](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>Jak najdu vaše role v Azure Active Directory

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **Procházet**a pak klikněte na **Služby Active Directory**.

    ![Na portálu Azure Active Directory](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klikněte na **uživatele**.

    ![Uživatelům Azure portálu výchozí služby Active Directory](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Klikněte na uživatele. V tomto příkladu je uživatel Kelley zdí.

    Všimněte si pole **Organizační ROLE**.

    ![Identita uživatele Azure portálu](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Základní informace o předplatném Azure a Office 365
Office 365 a Azure pomocí služby Azure Active Directory pro správu uživatelů a předplatná. Zvažte Azure directory jako kontejner, ve kterém a můžete vytvořit skupinu uživatelů předplatná. Použít stejného uživatelského účtu u předplatného Azure a Office 365, musíte zajistit vytvoření předplatných ve stejném adresáři. Mějte na paměti následující skutečnosti:

- V adresáři, ne jiných orientovat získá vytvořen předplatné.
- Uživatelé patří do adresáře, ne opačným způsobem.
- Přihlášení k odběru výkopu v adresáři uživatel, který vytváří předplatné. Vaše předplatné Office 365 jako výsledek je svázané ke stejnému účtu jako předplatné Azure při použití tohoto účtu vytvořit předplatné Office 365.

![Základní informace](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Další informace najdete v tématu [jak Azure předplatná souvisí s Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

>[AZURE.NOTE] Jednotliví uživatelé v adresáři vlastníte Azure předplatná.

>[AZURE.NOTE] Předplatné Office 365 vlastníte adresář samotný. Pokud mají uživatelé v rámci adresáře nezbytná oprávnění, můžete pracovat v těchto předplatných.

## <a name="next-steps"></a>Další kroky
Pokud jste získali Azure a Office 365 předplatná, samostatně v minulosti a chcete mít přístup k tenantovi Office 365 z Azure předplatné, najdete v článku [přidružení klienta Office 365 pomocí Azure předplatného](billing-add-office-365-tenant-to-azure-subscription.md).

> [AZURE.NOTE] Pokud máte pořád ještě problém vyřešit rychle získat otázky, [Kontaktujte podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
