<properties
    pageTitle="Použití klienta Office 365 s předplatným Azure | Microsoft Azure"
    description="Naučte se přidávat adresáře služby Office 365 (klient) k předplatnému Azure aby přidružení."
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
    ms.date="09/16/2016"
    ms.author="cjiang"/>

# <a name="associate-an-office-365-tenant-with-an-azure-subscription"></a>Přidružení tenanta Office 365 s předplatným Azure
Pokud jste získali Azure a Office 365 předplatná, samostatně v minulosti a teď chcete mít přístup k tenantovi Office 365 z Azure předplatného, není těžké si to udělat. Tento článek vám ukáže, jak.

> [AZURE.NOTE] Tento článek neplatí pro zákazníky Enterprise Agreement (EA).

## <a name="quick-guidance"></a>Stručné pokyny
Do vašeho tenanta Office 365 přidružit předplatného Azure, používat účet Azure přidáte vašeho klienta Office 365 a pak předplatného Azure přidružit klientovi Office 365.

## <a name="detailed-steps"></a>Podrobný postup
V tomto scénáři Kelley zdi je uživatel, který má předplatné Azure pod účtem kelley.wall@outlook.com. Kelley také má předplatné Office 365 pod účtem kelley.wall@contoso.onmicrosoft.com. Teď Kelley chce přístup ke klientovi Office 365 s Azure předplatného.

![Snímek obrazovky s Azure Active Directory stavu](./media/billing-add-office-365-tenant-to-azure-subscription/s31_msa-aad-status.png)

![Snímek obrazovky s Office 365 admin center aktivní uživatelé](./media/billing-add-office-365-tenant-to-azure-subscription/s32_office-365-user.png)

### <a name="prerequisites"></a>Zjistit předpoklady pro
Přidružení fungoval správně jsou potřeba následující požadavky:

- Potřebujete přihlašovací údaje Správce služby Azure předplatného. Dalších správců nelze provést podmnožinu kroky.
- Potřebujete pověření globální správce tenanta Office 365.
- E-mailovou adresu má správce služby nesmí být obsaženy v klientovi Office 365.
- E-mailovou adresu má správce služby nesmí odpovídat globální správce tenanta Office 365.
- Pokud aktuálně používáte e-mailovou adresu, která je účet Microsoft a účet organizace, dočasně změňte má správce služby Azure předplatného použít jiný účet Microsoft. Na [přihlašovací stránce účtu Microsoft](https://signup.live.com/), můžete vytvořit nový účet Microsoft.


Pokud chcete změnit správce služby, postupujte takto:

1. Přihlaste se k [portálu Správa účtu](https://account.windowsazure.com/subscriptions).
2. Vyberte předplatné, které chcete změnit.
3. Vyberte **Upravit podrobnosti předplatného**.

    ![Informace o předplatném snímek Azure, s "předplatné" zvýrazněnou možností upravit podrobnosti](./media/billing-add-office-365-tenant-to-azure-subscription/s33_azure-edit-subscription-details.png)

4. V dialogovém okně **Správce služby** zadejte e-mailovou adresu nového správce služby.

    ![Snímek obrazovky s dialogovým oknem "Upravit předplatného"](./media/billing-add-office-365-tenant-to-azure-subscription/s34_change-subscription-service-admin.png)

### <a name="associate-the-office-365-tenant-with-the-azure-subscription"></a>Přidružit Azure předplatné klientovi Office 365
Pokud chcete přidružit Azure předplatné klientovi Office 365, postupujte takto:

1.  Přihlaste se k [portálu Správa účtu](https://account.windowsazure.com/subscriptions) pomocí přihlašovacích údajů správce služby.
2.  V levém podokně vyberte **Služby ACTIVE DIRECTORY**.

    ![Snímek obrazovky služby Active Directory položky](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

    > [AZURE.NOTE] Zobrazí neměli klientovi Office 365. Pokud se zobrazí, přejděte na další krok.

    ![Snímek obrazovky výchozí adresář Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s36-aad-tenant-default.png)

3. Přidání klientovi Office 365 k předplatnému Azure.

    na. Vyberte **Nový** > **adresáře** > **vlastní vytvořit**.

    ![Vytvoření vlastní snímek obrazovky s Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)

    b. Na stránce **Přidat adresář** v **adresáři**vyberte **použít existující adresář**. Vyberte **jsem připraven určený k podpisu nyní**a pak vyberte **Dokončit** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Snímek obrazovky s "Použít existující adresář"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)

    c. Po přihlášení Přihlaste se přihlašovacími globální správce tenanta Office 365.

    ![Přihlašovací obrazovka Office 365 globální správce](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)

    d. Vyberte **pokračovat**.

    ![Snímek obrazovky s ověření](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)

    e. Vyberte **Odhlásit se**.

    ![Snímek obrazovky s odhlašovací](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)

    f. Přihlaste se k [portálu Správa účtu](https://account.windowsazure.com/subscriptions) pomocí přihlašovacích údajů správce služby.

    ![Přihlašovací obrazovka Azure](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

    g. Měli byste vidět vašeho klienta Office 365 na řídicím panelu.

    ![Snímek obrazovky s řídicím panelem](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

4. Změna adresáře přidružený k předplatnému Azure.

    na. Vyberte **Nastavení**.

    ![Ikona klasické nastavení portálu snímek Azure](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)

    b. Vyberte předplatné Azure a pak vyberte **Upravit adresář**.
    ![Snímek obrazovky Azure předplatné upravit adresáře](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)

    c. Vyberte **Další** ![další ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).

    ![Snímek obrazovky s "Změnit adresáři přidružené"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)

    > [AZURE.WARNING] Zobrazí se upozornění, že budou odebrány všechny dalších správců.

    ![Azure potvrďte adresáře mapování](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)

    >[AZURE.WARNING] Kromě toho všichni uživatelé [řízení přístupu na základě rolí (RBAC)](./active-directory/role-based-access-control-configure.md) s přístupem přiřazené do existující skupiny zdrojů budou odstraněny také. Upozornění, které se zobrazí ale jenom zmínky odebrání dalších správců.

    ![přiřazené uživatelům odebrány zdroje skupiny](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)

    d. Vyberte **Hotovo** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

5. Teď můžete přiřadit účtů organizace v Office 365 jako dalších správců klienta služby Azure Active Directory.

    na. Vyberte kartu **Správci** a pak vyberte **Přidat**.

    ![Snímek obrazovky Azure klasické nastavení portálu správci kartu](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)

    b. Zadejte účet organizace pro vašeho klienta Office 365, vyberte Azure předplatné a pak vyberte **Dokončit** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Spoluvytváření správce dialogové okno Přidat snímek Azure](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)

    c. Vraťte se na kartu **Správci** . Měli byste vidět účet organizace zobrazené jako spolu správce.

    ![Snímek obrazovky s kartou správci](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)

6. Pak můžete otestovat přístup s spolu správce.

    na. Odhlaste se z portálu Správa účtů.

    b. Otevřete na [portálu Správa účtu](https://account.windowsazure.com/subscriptions) nebo [Azure portálu](https://portal.azure.com/).

    c. Pokud Azure přihlašovací stránka má odkaz **Přihlaste se pomocí svého účtu organizace**, vyberte odkaz. V opačném tento krok přeskočte.

    ![Snímek obrazovky Azure přihlašovací stránka](./media/billing-add-office-365-tenant-to-azure-subscription/3-sign-in-to-azure.png)

    d. Zadejte přihlašovací údaje spolu správce a pak vyberte **přihlásit**.

    ![Snímek obrazovky Azure přihlašovací stránka](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="next-steps"></a>Další kroky
Související scénáře, patří:

- Už máte předplatné Office 365 a je připraven pro předplatné Azure, ale chcete použít existující uživatelské účty Office 365 u předplatného Azure.
- Jsou Azure odběratele a chcete získat předplatné Office 365 pro uživatele v existující instanci služby Azure Active Directory.

Zjistěte, jak provádět tyto úkoly, najdete v článku [použití stávajícího Office 365 účet s předplatným Azure nebo naopak](billing-use-existing-office-365-account-azure-subscription.md).
