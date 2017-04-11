<properties
    pageTitle="Azure Active Directory Domain Services: OM serveru Windows připojení k spravovaných doméně | Microsoft Azure"
    description="Připojit se ke virtuálního počítače v systému Windows Server Azure AD Domain Services"
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

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Spojení virtuálního počítače v systému Windows Server a spravovaných domény

> [AZURE.SELECTOR]
- [Azure klasické portálu - systému Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [Prostředí PowerShell – Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

Tento článek popisuje, jak se připojit do virtuálního počítače se systémem Windows serveru 2012 R2 k spravovaných doméně služby Azure AD domény, na portálu Azure klasické.


## <a name="step-1-create-the-windows-server-virtual-machine"></a>Krok 1: Vytvoření virtuálního počítače systému Windows Server
Postupujte podle pokynů uvedených v tomto kurzu [vytvořit virtuální počítač s Windows Azure klasické portálu](../virtual-machines/virtual-machines-windows-classic-tutorial.md) . Je třeba zajistit, že to nově vytvořený virtuální počítač připojen k stejné virtuální síti, ve kterém jste povolili Azure AD Domain Services. Možnost "Vytvořit" neumožňuje připojení k síti virtuální virtuální počítač. Proto budete muset používat volbu z Galerie vytvořit virtuální počítač.

Proveďte následující kroky k vytvoření Windows virtuální počítač připojen k virtuální sítě, ve kterém jste povolili Azure AD Domain Services.

1. Azure klasické portálu na panelu s příkazy v dolní části okna klikněte na **Nový**.

2. **Výpočet**klikněte na **virtuálního počítače**, pak klikněte na **Z Galerie**.

3. První obrazovce můžete **Zvolit obrázek** v počítači virtuální ze seznamu dostupných obrázky. Vyberte příslušný obrázek.

    ![Vyberte obrázek](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. Druhá obrazovce můžete vybrat název počítače, velikost a pro správu uživatelské jméno a heslo. Použijte osy a velikost potřebné ke spuštění aplikace nebo zátěží na projektu. Uživatelské jméno, které jste vybrali tady je uživatel místního správce v počítači. Není třeba zadávat přihlašovací údaje účtu uživatele doménu v tomto poli.

    ![Konfigurace virtuálního počítače](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. Třetí obrazovce můžete nakonfigurovat materiály pro sítě, ukládání a dostupnosti. Ujistěte se, že vyberete virtuální síť, ve kterém jste povolili služby Azure AD domény z rozevíracího seznamu **Oblast/spřažení skupiny/virtuální sítě** . Zadejte **Název cloudové služby DNS** podle potřeby virtuálního počítače.

    ![Vyberte virtuální sítě virtuálního počítače](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    Ujistěte se, že se připojíte ke virtuálního počítače do stejné virtuální síti, ve kterém jste povolili Azure AD Domain Services. Virtuální počítač jako výsledek, můžete zobrazit domény a provádění úloh, jako je připojení k doméně. Pokud budete chtít vytvořit virtuálního počítače v síti různých virtuální, připojte k virtuální sítě, ve kterém jste povolili Azure AD Domain Services virtuální sítě.

6. Čtvrtý obrazovce můžete nainstalovat agenta OM a nakonfigurovat některé z dostupných rozšíření.

    ![Hotovo](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. Po vytvoření virtuální počítač portálu klasické uvádí nové virtuálního počítače uzlu **virtuálních počítačích** . Virtuální počítač a cloudové služby je spuštěn automaticky i jeho stav je uveden jako **spuštěný**.

    ![Virtuální počítač je do začátků](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Krok 2: Připojení k systému Windows Server virtuálního počítače pomocí místního účtu správce
Teď můžeme připojení k nově vytvořený virtuální počítač systému Windows Server, ke spojení a doménu. Použití pověření místního správce, které jste zadali při vytváření virtuálního počítače o připojení definován.

Proveďte následující kroky pro připojení k virtuální počítač.

1. Přejděte na **virtuálních počítačích** uzel klasické portálu. Vyberte virtuální počítač, který jste vytvořili v kroku 1 a na panelu s příkazy v dolní části okna klikněte na **Připojit** .

    ![Připojení k systému Windows virtuálního počítače](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Klasický portál vás vyzve k otevření nebo uložení souboru s příponou "RDP", která se používá pro připojení do virtuálního počítače. Klepnutím otevřete soubor po dokončení stahování.

3. Do příkazového řádku přihlášení zadejte **pověření místního správce**, který jste zadali při vytváření virtuálního počítače. Například jsme jste v tomto příkladě použita "localhost\mahesh".

V tomto okamžiku jste měli Zaprotokolují do nově vytvořený virtuálního počítače Windows pomocí místní přihlašovací údaje správce. Dalším krokem je připojení virtuálního počítače do domény.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Krok 3: Připojení virtuálního počítače systému Windows Server doménu spravovaný AAD Pošta
Proveďte následující kroky ke spojení virtuálního počítače systému Windows Server a doménu spravovanou AAD Pošta.

1. Připojení k systému Windows Server uvedeno v kroku 2. Na obrazovce Start spusťte **Správce serveru**.

2. Klikněte na **Místní Server** v levém podokně okna Správce serveru.

    ![Spuštění Správce Server na virtuálních počítačů](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. V části **Vlastnosti** klikněte na **pracovní skupina** . Na stránce vlastností **Vlastnosti systému** klikněte na **změnit** k doméně.

    ![Systém vlastností stránky](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. Zadejte název domény služby Azure AD domény spravovat domény v textovém poli **doménu** a klikněte na **OK**.

    ![Určení domény, kterou chcete být připojen](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. Zobrazí se výzva k zadání přihlašovacích údajů k doméně. Ujistěte se, můžete **zadat přihlašovací údaje pro uživatele, které patří správcům Datacentrum AAD** skupiny. Členové této skupiny oprávnění ke spojení počítačích a spravovaných domény.

    ![Zadejte přihlašovací údaje pro připojení k doméně](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. Zadejte přihlašovací údaje některým z následujících způsobů:

    - Formát UPN: Zadejte Přípony UPN pro uživatele, jak konfigurovat Azure AD. V tomto příkladu je přípony UPN pro uživatele "Jan" 'bob@domainservicespreview.onmicrosoft.com'.

    - SAMAccountName formát: můžete zadat název účtu ve formátu SAMAccountName. V tomto příkladu by uživatele "Jan" muset zadat "CONTOSO100\bob".

        > [AZURE.NOTE] **Doporučujeme používat formát UPN pro zadejte přihlašovací údaje.** SAMAccountName může být automaticky generované pokud předpona UPN uživatele je příliš dlouhá (například "joereallylongnameuser"). Pokud více uživatelů mít stejnou předponu Přípony (třeba "Jan") ve vašem klientovi Azure AD, jejich SAMAccountName formátu může být automaticky generované službou. V těchto případech formát UPN mohou sloužit problémy se spolehlivým k přihlášení do domény.

7. Po úspěšném připojení k doméně zobrazí následující zpráva, která vás přivítá domény. Restartujte počítač virtuální pro operace připojení domény k dokončení.

    ![Vítá vás domény](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Poradce při potížích s připojení k doméně
### <a name="connectivity-issues"></a>Problémy s připojením
Pokud virtuálního počítače nejde najděte doménu, v nápovědě k podle následujících kroků:

- Zkontrolujte, jestli je virtuální počítač připojený do stejné virtuální sítě, který jste povolili Domain Services v. V opačném případě virtuální počítač nemůže připojit k doméně a proto nelze k doméně.

- Pokud virtuální počítač připojen k jiné síti virtuální, ujistěte se, že tento virtuální sítě je připojen k virtuální sítě, ve kterém jste povolili Domain Services.

- Zkuste ping domény pomocí názvu domény spravované domény (třeba ping contoso100.com). Pokud se vám nepodaří k tomu nevyzve, zkuste ping IP adresy služby zobrazené na stránce místo, kam jste povolili Azure AD Domain Services (například "pomocí příkazu ping 10.0.0.4"). Pokud budete moct pomocí příkazu ping na IP adresu, ale nikoli doménu, může být nesprávně nakonfigurované DNS. Jste nemusí nakonfigurovali IP adresy domény jako servery DNS pro virtuální sítě.

- Zkuste vyprázdnění mezipaměti překládání DNS v počítači virtuální ("ipconfig/flushdns").

Pokud se zobrazí dialogové okno s dotazem, v části přihlašovací údaje k doméně, nemusíte problémy s připojením.


### <a name="credentials-related-issues"></a>Problémy s přihlašovací údaje
Pokud máte potíže s přihlašovací údaje a nemůžete soubory k doméně v nápovědě k podle těchto kroků.

- Zkuste pomocí formátu UPN zadejte přihlašovací údaje. SAMAccountName pro váš účet může být automaticky generované, pokud existuje více uživatelů s předponou UPN ve vašem klientovi nebo UPN předponu je příliš dlouho. Proto formátu SAMAccountName pro váš účet může lišit od očekávat, nebo použijte ve vaší místní doméně.

- Zkuste použít přihlašovací údaje uživatelského účtu, který patří do skupiny "AAD Datacentrum správci" ke spojení počítačích a spravovaných domény.

- Ujistěte se, že máte [povolená synchronizace hesel](active-directory-ds-getting-started-password-sync.md) podle kroků uvedených v příručce Začínáme.

- Zajištění nakonfigurovaná v Azure AD pomocí UPN uživatele (například 'bob@domainservicespreview.onmicrosoft.com') přihlásit.

- Ujistěte se, že máte počkat dostatečně dlouhou dobu synchronizace hesel dokončete uvedených v příručce Začínáme.


## <a name="related-content"></a>Související obsah

- [Služby Domain Azure AD - příručce Začínáme](./active-directory-ds-getting-started.md)

- [Spravovat spravovaná domain Azure AD Domain Services](./active-directory-ds-admin-guide-administer-domain.md)
