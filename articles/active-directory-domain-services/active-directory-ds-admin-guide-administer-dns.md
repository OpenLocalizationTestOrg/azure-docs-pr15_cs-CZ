<properties
    pageTitle="Azure Active Directory Domain Services: Správa služby DNS spravovaný domény | Microsoft Azure"
    description="Správa služby DNS spravovaný domény Azure Active Directory Domain Services"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Správa služby DNS spravovaný domény Azure AD Domain Services
Azure Active Directory Domain Services obsahuje server DNS (překlad názvů domén), který obsahuje služeb překládá názvy DNS pro doménu spravovaný. V některých případech může potřebujete konfigurace DNS spravovaný domény. Budete muset vytvoření záznamů DNS u počítačů, které nejsou spojeny na doménu, konfiguraci virtuální IP adres pro vyrovnávání zatížení nebo nastavení externího serverů DNS pro předávání. Z tohoto důvodu mít uživatelé, kteří patří do skupiny "AAD Datacentrum správci" příslušná oprávnění správy DNS spravovaný domény.


## <a name="before-you-begin"></a>Než začnete
Abyste mohli provést úkoly uvedené v tomto článku, musíte:

1. Platné **Azure předplatného**.

2. **Adresář Azure AD** - buď synchronizovat s místního adresáře nebo jen cloudu adresář.

3. **Azure AD Domain Services** musí být povolené na adresáři Azure AD. Pokud jste tak dosud neučinili, udělejte všechny úkoly uvedené v [příručce Začínáme](./active-directory-ds-getting-started.md).

4. **Doméně virtuálního počítače** , ze které spravujete Azure AD Domain Services spravovat domény. Pokud nemáte virtuálního počítače, postupujte podle všechny úkoly popsané v článku s názvem [připojení počítače se systémem Windows virtuální spravovaných doménu](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Potřebujete pověření **uživatelský účet patřící do skupiny "Administrators AAD řadiče domény"** v adresáři, jak spravovat DNS pro vaši doménu spravovaný.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Úkol 1 - poskytování virtuálního počítače doméně pro správu DNS pro doménu spravovaný
Azure AD Domain Services spravované domény můžete vzdáleně spravovat pomocí známé nástroje pro správu služby Active Directory například pro správu Centrum Active Directory (ADAC) nebo AD Powershellu. Podobně DNS pro doménu spravovaný můžete spravovat vzdáleně pomocí nástroje pro správu serveru DNS.

Správce ve vašem adresáři Azure AD nemáte oprávnění k připojení k řadiče domény na spravované doméně prostřednictvím vzdálené plochy. Členové skupiny "AAD Datacentrum správci" můžete spravovat DNS pro spravované domény vzdáleně pomocí nástrojů DNS Server z Windows Server/klientský počítač, který je připojen k spravované domény. Nástroje pro DNS Server můžete nainstalovali jako součást sady volitelná funkce Vzdálenou správu serveru nástroje () v systému Windows Server a klientské počítače připojen k spravované domény.

První úkol je zřízení virtuální počítač systému Windows Server, který je připojen k spravované domény. Pokyny naleznete v článku [připojení virtuálního počítače systému Windows Server Azure AD Domain Services spravovat domény](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Úkol 2 – nainstalujte na Server DNS nástroje v počítači virtuální
Provedení následujících kroků můžete nainstalovat nástroje pro správu DNS na počítač virtuální domény připojen. Další informace o [instalaci a používání nástrojů pro správu vzdálený Server](https://technet.microsoft.com/library/hh831501.aspx)najdete na TechNetu.

1. Přejděte na **virtuálních počítačích** uzel Azure klasické portálu. Vyberte virtuální počítač, který jste vytvořili v úkolu 1 a na panelu s příkazy v dolní části okna klikněte na **Připojit** .

    ![Připojení k systému Windows virtuálního počítače](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Klasický portál vás vyzve k otevření nebo uložení souboru s příponou "RDP", která se používá pro připojení do virtuálního počítače. Po dokončení stahování klikněte na soubor.

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

9. Na stránce **funkce** kliknutím rozbalte uzel **Vzdálené nástroje pro správu serveru** a pak klikněte na tlačítko rozbalte uzel **Nástroje pro správu rolí** . Funkce **Nástroje serveru DNS** vyberte ze seznamu nástroje pro správu rolí.

    ![Funkce stránky](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Na stránce **potvrzení** klikněte na nainstalovat na Server DNS nástroje funkce virtuální počítač **nainstalovat** . Po úspěšném dokončení instalace funkce neaktivují klikněte na **Zavřít** zavřete průvodce **přidáním rolí a funkce** .

    ![Potvrzovací stránku](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Úkol 3 – Spusťte konzolu Správa služby DNS spravovat DNS
Teď nainstalovali funkci nástroje serveru DNS domény připojen virtuálního počítače, používáme DNS nástroje pro správu DNS na spravované doméně.

> [AZURE.NOTE] Musíte být členem skupiny "AAD Datacentrum správci" ke správě DNS spravovaný domény.

1. Na úvodní obrazovce klepněte na tlačítko **Nástroje pro správu**. Měli byste vidět konzole **DNS** nainstalovaných v počítači virtuální.

    ![Nástroje pro správu – konzole DNS](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Klikněte na **DNS** spusťte konzolu Správa služby DNS.

3. V dialogovém okně **připojit k serveru DNS** klikněte na požadovanou možnost s názvem **jiný počítač**a zadejte název domény DNS spravovaný domény (třeba "contoso100.com").

    ![Konzole DNS - připojit k doméně](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. Konzole DNS se připojí k spravované domény.

    ![Konzole DNS – Správa domén](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. Nyní můžete v konzole DNS přidejte záznamy DNS pro počítače v rámci virtuální sítě, ve kterém jste povolili AAD Domain Services.

> [AZURE.WARNING] Dejte si pozor, pokud spravujete DNS pro doménu spravovaný pomocí nástroje pro správu DNS. Zajistit, že můžete **Odstranit nebo změnit předdefinované záznamy DNS, které využívají Domain Services v doméně**. Předdefinované záznamy DNS, patří záznamy DNS domény záznamy názvového serveru a další záznamy pro Datacentrum umístění. Pokud upravíte tyto záznamy, jsou virtuální síti se systémem přerušení služby domény.


Naleznete v [článku nástroje DNS na webu Technet](https://technet.microsoft.com/library/cc753579.aspx) pro další informace o správě DNS.


## <a name="related-content"></a>Související obsah

- [Služby Domain Azure AD - příručce Začínáme](./active-directory-ds-getting-started.md)

- [Spojení virtuálního počítače v systému Windows Server a spravovaných domain Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Spravovat spravovaná domain Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)

- [Nástroje pro správu DNS](https://technet.microsoft.com/library/cc753579.aspx)
