<properties
    pageTitle="Azure Active Directory Domain Services: Připojení k spravovaných doméně OM RHEL | Microsoft Azure"
    description="Připojit se ke virtuálního počítače červené klobouk Enterprise Linux Azure AD Domain Services"
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

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Připojit se ke virtuálního počítače červené čepice Enterprise Linux 7 spravované domény
V tomto článku se dozvíte, jak ke spojení červené klobouk Enterprise Linux (RHEL) 7 virtuálního počítače a spravovaných domain Azure AD Domain Services.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Zřízení červené klobouk Enterprise Linux virtuálního počítače
Proveďte následující kroky zřízení RHEL 7 virtuálního počítače pomocí portálu Azure.

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

    ![Azure portálu řídicího panelu](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. V levém podokně klikněte na **Nový** a zadejte **Červené čepice** do panelu hledání, jak je vidět na následující snímek obrazovky. Ve výsledcích hledání se zobrazí položky červená klobouk Enterprise Linux. Klikněte na **červené klobouk Enterprise Linux 7.2**.

    ![Vyberte RHEL ve výsledcích](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. Výsledky hledání v podokně **všechno, co** by měl seznam červená klobouk Enterprise Linux 7.2 obrázek. Klikněte na **červené klobouk Enterprise Linux 7.2** zobrazíte další informace o obrázek virtuálního počítače.

    ![Vyberte RHEL ve výsledcích](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. V podokně **červené klobouk Enterprise Linux 7.2** byste měli vidět další informace o obrázek virtuálního počítače. V rozevíracím seznamu **Vyberte nasazení modelu** vyberte **klasické**. Klikněte na tlačítko **vytvořit** .

    ![Zobrazení podrobností o obrázek](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. V podokně **Vytvořit OM** zadejte **Název hostitele** nového virtuálního zařízení. Taky určete místního správce uživatelské jméno do pole **uživatelské jméno** a **heslo**. Můžete taky pomocí klávesy SSH k ověření uživatele místního správce. Také vyberte **Ceny osy** virtuálního počítače.

    ![Vytvoření OM – základní informace](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Klikněte na **nepovinný konfigurace**. V podokně **volitelné konfigurace** klikněte na **síť**.

    ![Vytvoření OM – konfigurace virtuální sítě](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Tím se vyvolá podokno s názvem **síť**. V podokně **sítě** klikněte na **Virtuální sítě** vyberte virtuální sítě, ke kterému by měl být nasazené OM Linux. Otevře se podokno **Virtuální sítě** . V podokně **Virtuální sítě** vyberte možnost **použít existující virtuální sítě** . Vyberte virtuální sítě, ve kterém je k dispozici Azure AD Domain Services. V tomto příkladu jsme vyberte virtuální síť "MyPreviewVNet".

    ![Vytvoření OM – vyberte virtuální sítě](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. V podokně **volitelné konfigurace** kliknutím na tlačítko **OK** .

    ![Vytvoření OM - virtuální sítě vybrané](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. Teď jste připraveni vytvořit virtuální počítač. V podokně **Vytvořit OM** klikněte na tlačítko **vytvořit** .

    ![Vytvoření OM - připravení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Nasazení nové virtuálního počítače podle obrázku RHEL 7.2 by měly začít.

  ![Vytvoření OM – nasazení Začínáme](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Po několika minutách by měly být virtuální počítač nasazeny úspěšně a připravená k použití.

  ![Vytvoření OM – nasazení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Připojení vzdálených nově vytvořený virtuální počítač Linux
Virtuální počítač RHEL 7.2 byla zajištěna v Azure. Je další úkol vzdáleně připojovat k virtuální počítač.

**Připojte se k počítači virtuální RHEL 7.2** Postupujte podle pokynů v článku [jak se přihlásit k virtuální počítač Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

Další kroky se předpokládá, že používáte klientů SSH nátěrové se připojit k počítači virtuální RHEL. Další informace najdete v tématu [stažení nátěrové stránky](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Spusťte aplikaci nátěrové.

2. Nově vytvořený virtuální počítač RHEL zadejte **Název hostitele** . V tomto příkladu naše virtuální počítač má název hostitele contoso rhel.cloudapp .net. Pokud si nejste jisti název hostitele vaší OM, podívejte se na řídicí panel OM na portálu Azure.

    ![Nátěrové připojení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Přihlaste se do virtuálního počítače pomocí přihlašovacích údajů místního správce jste zadali, kdy byl vytvořen virtuální počítač. V tomto příkladu jsme použili místního účtu správce "mahesh".

    ![Nátěrové přihlášení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Instalace požadovaných balíčků na Linux virtuálního počítače
Po připojení k virtuální počítač, dalším úkolem je k instalaci balíčků potřebných pro připojení k doméně na virtuální počítač. Proveďte následující kroky:

1. **Instalace realmd:** Balíček realmd se používá pro připojení k doméně. V nátěrové terminálu zadejte tento příkaz:

    sudo yum nainstalovat realmd

    ![Instalace realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Po několika minutách by měla získat balíček realmd nainstalovat virtuální počítač.

    ![realmd nainstalovaný](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Instalace sssd:** Balíček realmd závisí na sssd k provádění operací spojení domény. V nátěrové terminálu zadejte tento příkaz:

    sudo yum nainstalovat sssd

    ![Instalace sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Po několika minutách by měla získat balíček sssd nainstalovat virtuální počítač.

    ![realmd nainstalovaný](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Instalace pomocí protokolu kerberos:** V nátěrové terminálu zadejte tento příkaz:

    sudo yum nainstalovat krb5 workstation krb5-knihoven

    ![Instalace pomocí protokolu kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Po několika minutách by měla získat balíček realmd nainstalovat virtuální počítač.

    ![Protokol Kerberos nainstalovaný](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Připojit se ke virtuálního počítače Linux spravované domény
Vyžadované balíčky nainstalovaných v počítači virtuální Linux, dalšího úkolu je ke spojení virtuálního počítače a spravovaných domény.

1. Seznamte se s doménu spravovaný AAD Domain Services. V nátěrové terminálu zadejte tento příkaz:

    Seznamte se s sudo sféry CONTOSO100.COM

    ![Seznamte se s sféry](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Pokud **sféry Seznamte se s** se nepodařilo najít spravované domény zajistěte, aby byl domény k nim přistupovat z virtuálního počítače (zkuste ping). Také ujistěte se, že virtuální počítač má ve skutečnosti byl používaný stejné virtuální sítě, ve kterém je k dispozici spravované domény.

2. Inicializace kerberos. Do nátěrové terminálu zadejte následující příkaz. Zajistit, aby určit uživatele, kteří patří do skupiny "AAD Datacentrum správci". Tito uživatelé může připojit počítače na spravované doménu.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Zajistit, aby zadejte název domény velkými písmeny, dalšího kinit se nezdaří.

3. Připojení počítače k doméně. Do nátěrové terminálu zadejte následující příkaz. Zadejte stejné uživatelské, které jste zadali v předchozím kroku (kinit).

    spojení sféry sudo – podrobného CONTOSO100.COM -U'bob@CONTOSO100.COM'

    ![Sféry spojení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Dostanete zprávu ("úspěšně zapsaný machine sféry) při počítač úspěšně připojen k spravované domény.


## <a name="verify-domain-join"></a>Ověření domény spojení
Můžete rychle ověřit, zda počítač úspěšně připojen k spravované domény. Připojení k nově domény připojené RHEL OM použití SSH a domény uživatelský účet a pak zaškrtněte ke zjištění, pokud je uživatelský účet vyřeší správně.

1. Do nátěrové terminálu zadejte tento příkaz připojit k nově domény připojené RHEL virtuálního počítače pomocí SSH. Použití účtu domény které patří spravované domény (třeba 'bob@CONTOSO100.COM' v tomto případě.)

    SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net

2. V nátěrové terminálu zadejte tento příkaz zobrazíte, pokud byla adresáři inicializována správně.

    PWD

3. V nátěrové terminálu zadejte tento příkaz zobrazíte-li správně vyřešit členství ve skupině.

    ID

Následující ukázkový výstup z následujících příkazů:

![Ověření domény spojení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>Poradce při potížích s připojení k doméně
Přečtěte si článek [Poradce při potížích domény spojení](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) .


## <a name="related-content"></a>Související obsah
- [Služby Domain Azure AD - příručce Začínáme](./active-directory-ds-getting-started.md)

- [Spojení virtuálního počítače v systému Windows Server a spravovaných domain Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Jak se přihlásit k virtuální počítač Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

- [Instalace pomocí protokolu Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Červené klobouk Enterprise Linux 7 – Windows integrace příručku](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
