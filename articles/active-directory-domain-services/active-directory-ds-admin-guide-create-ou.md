<properties
    pageTitle="Služby Azure Active Directory Domain Services: Příručka pro správu | Microsoft Azure"
    description="Vytvoření organizační jednotce (OU) na služby Azure AD domény spravovat domény"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Vytvoření organizační jednotce (OU) na spravované domain Azure AD Domain Services
Azure AD Domain Services spravované domény obsahovat dva předdefinované kontejnery s názvem AADDC počítačů a AADDC uživatelů. Kontejner AADDC počítače má počítač objekty, abyste všech počítačů, které jsou připojené k spravovaných doméně. Kontejneru AADDC uživatelů obsahuje uživatelů a skupin v klientovi Azure AD. V některých případech může být nutné vytvořit účty služby na spravované domény, kterou chcete nasadit úloh. K tomuto účelu můžete vytvořit vlastní organizační jednotce (OU) ve spravovaných doméně a vytvořte účty služby v rámci tohoto OU. Tento článek popisuje, jak vytvořit organizační jednotce ve vaší doméně spravovaných.


## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Instalace nástroje pro správu AD na virtuálního počítače doméně pro vzdálená správa
Azure AD Domain Services spravované domény můžete vzdáleně spravovat pomocí známé nástroje pro správu služby Active Directory například pro správu Centrum Active Directory (ADAC) nebo AD Powershellu. Správci klientů nemáte oprávnění k připojení k řadiče domény na spravované doméně prostřednictvím vzdálené plochy. Spravovat spravovaná domény, nainstalujte funkce nástroje pro správu AD virtuální počítač připojen k spravované domény. Přečtěte si článek s názvem [Spravovat domény služby Azure AD Domain Services spravováno](active-directory-ds-admin-guide-administer-domain.md) pokyny.

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Vytvořit organizační jednotku ve spravovaných doméně
Teď nástroje pro správu AD nainstalovaných v doméně připojen virtuálního počítače, používáme těchto nástrojích vytvoříte organizační jednotku na spravované domény. Proveďte následující kroky:

> [AZURE.NOTE] Požadované oprávnění k vytvoření vlastní OU mají pouze členové skupiny "AAD Datacentrum správci". Zajištění, proveďte následující kroky jako uživatel, kterému patří k této skupině.

1. Na úvodní obrazovce klepněte na tlačítko **Nástroje pro správu**. Měli byste vidět AD nástroje pro správu nainstalovaných v počítači virtuální.

    ![Nástroje pro správu na serveru nainstalovány](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Klikněte na **Centrum pro správu služby Active Directory**.

    ![Centrum pro správu služby Active Directory](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Chcete-li zobrazit doménu, klikněte na název domény v levém podokně (například "contoso100.com").

    ![ADAC – zobrazení domény](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

4. V podokně **úkoly** pravé straně klikněte na **Nový** uzlu název domény. V tomto příkladu jsme klikněte na **Nový** uzlu "contoso100(local)" v podokně **úlohy** na pravé straně.

    ![ADAC – nové OU](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

5. Měli byste vidět možnost vytvořit organizační jednotku. Klikněte na **Organizační jednotku** , kterou chcete otevřít dialogové okno **Vytvořit organizační jednotku** .

6. V dialogovém okně **Vytvořit organizační jednotku** zadejte **název** pro nová organizační jednotku. Zadejte krátký popis pro organizační jednotku. Pole **Spravovaných tak, že** mohou také nastavit pro organizační jednotku. Pokud chcete vytvořit vlastní OU, klikněte na **OK**.

    ![ADAC – OU dialogové okno vytvořit](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

7. Nově vytvořený OU by teď měla zobrazovat v AD pro správu Center (ADAC).

    ![ADAC – OU vytvořili](./media/active-directory-domain-services-admin-guide/create-ou-done.png)


## <a name="permissionssecurity-for-newly-created-ous"></a>Oprávnění a zabezpečení pro nově vytvořený organizačních jednotek
Ve výchozím nastavení je uživateli (členem skupiny "AAD Datacentrum správci") vytvořil vlastní OU udělit oprávnění správce (Úplné řízení) myši na organizační jednotku. Uživatele můžete pokračovat a udělit oprávnění pro ostatní uživatele nebo skupiny "AAD Datacentrum správci" podle potřeby. Jak je vidět na následující snímek obrazovky a uživatel 'bob@domainservicespreview.onmicrosoft.com' autora novou "MyCustomOU" organizační jednotku, která úplné řízení nad ním.

 ![ADAC - nová OU zabezpečení](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)


## <a name="notes-on-administering-custom-ous"></a>Poznámky: Správa vlastních organizačních jednotek
Teď, když jste vytvořili vlastní OU, můžete pokračovat a vytvoření uživatelé, skupiny, počítače a účty služby v Nepropojených. "AAD Datacentrum uživatelům" OU nelze přesunout uživatele nebo skupiny do vlastní organizačních jednotek.

> [AZURE.WARNING] Uživatelské účty, skupiny, účtů služeb a objektů počítačů, které vytvoříte v části vlastní organizačních jednotek nejsou k dispozici ve vašem klientovi Azure AD. Jinými slovy tyto objekty nezobrazovat pomocí rozhraní API Azure AD grafu nebo v uživatelském rozhraní Azure AD. Tyto objekty jsou dostupné pouze ve vaší doméně spravovaných Azure AD Domain Services.


## <a name="related-content"></a>Související obsah

- [Spravovat spravovaná domain Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)

- [Centrum pro správu služby Active Directory: Začínáme](https://technet.microsoft.com/library/dd560651.aspx)

- [Podrobného průvodce účty služby](https://technet.microsoft.com/library/dd548356.aspx)
