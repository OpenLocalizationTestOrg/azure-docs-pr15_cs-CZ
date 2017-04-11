<properties
    pageTitle="Azure Active Directory Domain Services: Správa spravovaných doménu | Microsoft Azure"
    description="Správa domén spravovaných Azure Active Directory Domain Services"
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
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Spravovat spravovaná domain Azure Active Directory Domain Services
Tento článek ukazuje, jak spravovat spravovaných domain Azure Active Directory (AD) Domain Services.


## <a name="before-you-begin"></a>Než začnete
Abyste mohli provést úkoly uvedené v tomto článku, musíte:

1. Platné **Azure předplatného**.

2. **Adresář Azure AD** - buď synchronizovat s místního adresáře nebo jen cloudu adresář.

3. **Azure AD Domain Services** musí být povolené na adresáři Azure AD. Pokud jste tak dosud neučinili, udělejte všechny úkoly uvedené v [příručce Začínáme](./active-directory-ds-getting-started.md).

4. **Doméně virtuálního počítače** , ze které spravujete Azure AD Domain Services spravovat domény. Pokud nemáte virtuálního počítače, postupujte podle všechny úkoly popsané v článku s názvem [připojení počítače se systémem Windows virtuální spravovaných doménu](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Potřebujete pověření **uživatelský účet patřící do skupiny "Administrators AAD řadiče domény"** v adresáři, jak spravovat vaši doménu spravovaný.

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Úkoly správy, které lze provádět na spravované domény
Členové skupiny "AAD Datacentrum správci" mít příslušná oprávnění spravované domény které jim umožňuje dělat třeba tyto úkoly:

- Připojte zařízení do spravované domény.

- Konfigurace předdefinovaných zásad skupiny pro kontejnery AADDC počítačů a AADDC uživatele v spravované domény.

- Správa služby DNS spravovaný domény.

- Vytvářet a spravovat vlastní organizační jednotkám ve spravovaných doméně.

- Zisk přístup pro správu k počítači připojené k spravovaných doméně.


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Oprávnění správce nemáte na spravované doméně
Doména se spravuje Microsoft, včetně činností například opravy, sledování a zálohování. Proto Uzamknout doménu a nemáte oprávnění k provedení některých úkolů správy pro tuto doménu. Některé úkoly, které nelze provést příklady dole.

- Budete nejsou příslušná oprávnění správce domény nebo podnikový správce pro spravovaných doménu.

- Nelze rozšířit schématu spravované domény.

- Nelze se připojit ke řadiče domény pro doménu spravovaný pomocí vzdálené plochy.

- Spravované domény nemůžete přidat řadiče domény.


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Úkol 1 - poskytování virtuálního počítače doméně systému Windows Server pro správu spravované domény
Azure AD Domain Services spravované domény můžete spravovat pomocí známé nástroje pro správu služby Active Directory například pro správu Centrum Active Directory (ADAC) nebo AD Powershellu. Správci klientů nemáte oprávnění k připojení k řadiče domény na spravované doméně prostřednictvím vzdálené plochy. Členové skupiny "AAD Datacentrum správci" proto můžete spravovat spravovaných domén vzdáleně pomocí nástroje pro správu AD z Windows Server/klientský počítač, který je připojen k spravované domény. Nástroje pro správu AD můžete nainstalovaný jako součást volitelná funkce Vzdálenou správu serveru nástroje () v systému Windows Server a klientské počítače připojen k spravované domény.

Cílem prvního kroku je nastavení systému Windows Server virtuálního počítače, který je připojen k spravované domény. Pokyny naleznete v článku [připojení virtuálního počítače systému Windows Server Azure AD Domain Services spravovat domény](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Správu spravované domény z klientského počítače (například Windows 10)
Pokyny v tomto článku virtuálního počítače v systému Windows Server pro správu AAD Pošta spravovat domény. Ale můžete to udělat pomocí virtuálního počítače Windows klienta (například Windows 10).

Můžete [nainstalovat Vzdálenou správu serveru nástroje ()](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) v počítači virtuální klienta Windows postupujte podle pokynů na TechNetu.


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Úkol 2 – nástroje pro správu služby Active Directory nainstalovat na počítač virtuální
Proveďte následující postup instalace nástroje pro správu služby Active Directory v počítači virtuální domény připojen. Další [informace o instalaci a pomocí nástroje pro správu vzdálený Server](https://technet.microsoft.com/library/hh831501.aspx)najdete na TechNetu.

1. Přejděte na **virtuálních počítačích** uzel Azure klasické portálu. Vyberte virtuální počítač, který jste vytvořili v úkolu 1 a na panelu s příkazy v dolní části okna klikněte na **Připojit** .

    ![Připojení k systému Windows virtuálního počítače](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Klasický portál vás vyzve k otevření nebo uložení souboru s příponou "RDP", která se používá pro připojení do virtuálního počítače. Klepnutím otevřete soubor po dokončení stahování.

3. Do příkazového řádku přihlášení použijte přihlašovací údaje uživatele, které patří do skupiny "AAD Datacentrum správci". Například používáme 'bob@domainservicespreview.onmicrosoft.com' v našem případě.

4. Na obrazovce Start spusťte **Správce serveru**. Klikněte na **Přidat role a funkce** ve středovém podokně okna Správce serveru.

    ![Spuštění Správce Server na virtuálních počítačů](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Na stránce **Než začnete** **přidávat rolí a Průvodce funkcí**na tlačítko **Další**.

    ![Před zahájením stránky](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Na stránce **Typ instalace** ponechte možnost **instalace na základě rolí nebo založeného na funkcích** zaškrtnuté a klikněte na tlačítko **Další**.

    ![Stránka Typ instalace](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Na stránce **Výběr serveru** vyberte aktuální virtuální počítač z fondu serveru a klikněte na tlačítko **Další**.

    ![Stránka pro výběr serveru](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Na stránce **Role serveru** klikněte na **Další**. Tuto stránku jsme přeskočit, protože jsme nejsou žádné role instalaci na serveru.

9. Na stránce **funkce** kliknutím rozbalte uzel **Vzdálené nástroje pro správu serveru** a pak klikněte na tlačítko rozbalte uzel **Nástroje pro správu rolí** . Funkce **služby AD DS a nástroje AD LDS** vyberte ze seznamu nástroje pro správu rolí.

    ![Funkce stránky](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. Na stránce **potvrzení** klikněte na **instalovat** nainstalovat AD a funkce nástroje AD LDS na virtuální počítač. Po úspěšném dokončení instalace funkce neaktivují klikněte na **Zavřít** zavřete průvodce **přidáním rolí a funkce** .

    ![Potvrzovací stránku](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Úkol 3: připojení k a prozkoumání spravované domény
Teď nástroje pro správu AD nainstalovaných v doméně připojen virtuálního počítače, používáme těchto nástrojích zkoumání a spravovat spravovaná doménu.

> [AZURE.NOTE] Musíte být členem skupiny "AAD Datacentrum správci" spravovat spravovaná domény.

1. Na úvodní obrazovce klepněte na tlačítko **Nástroje pro správu**. Měli byste vidět AD nástroje pro správu nainstalovaných v počítači virtuální.

    ![Nástroje pro správu na serveru nainstalovány](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Klikněte na **Centrum pro správu služby Active Directory**.

    ![Centrum pro správu služby Active Directory](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Můžete prozkoumat doménu, klikněte na název domény v levém podokně (například "contoso100.com"). Všimněte si dvou kontejnery s názvem AADDC počítačů a AADDC uživatelů.

    ![ADAC – zobrazení domény](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. Klikněte na kontejner nazvanou **Uživatelé AADDC** zobrazíte všem uživatelům a skupinám patřící do spravované domény. Měli byste vidět uživatelské účty a skupiny z Azure AD klient zobrazit v tomto kontejneru. Oznámení v tomto příkladu, účet uživatele pro uživatele s názvem "Adam" a skupiny nazvané "AAD Datacentrum správci" jsou dostupné v tomto kontejneru.

    ![ADAC - uživatelé domény](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. Klikněte na kontejner s názvem **Počítače AADDC** zobrazíte počítače připojené k této spravované domény. Měli byste vidět položky pro aktuální virtuální počítač, který je připojen k doméně. Účet počítače pro všech počítačů, které jsou připojené k doméně spravovaných Azure AD Domain Services jsou uložené v tomto kontejneru AADDC počítačů.

    ![ADAC - domény připojen počítačů](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Související obsah

- [Služby Domain Azure AD - příručce Začínáme](./active-directory-ds-getting-started.md)

- [Spojení virtuálního počítače v systému Windows Server a spravovaných domain Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Nasazení nástroje pro správu vzdálený Server](https://technet.microsoft.com/library/hh831501.aspx)
