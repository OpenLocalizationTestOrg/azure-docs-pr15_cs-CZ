<properties
    pageTitle="Instalace služby IIS na první OM Windows | Microsoft Azure"
    description="Experimentovat s virtuálního počítače první Windows tak, že instalace služby IIS a otevírání portu 80 pomocí portálu Azure."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cynthn"/>

# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Experimentovat s instalací roli na Windows OM
    
Až budete mít první virtuálního počítače (OM) nahoru a spuštěnou, můžete přesunout instalace softwaru a služeb. Pro účely tohoto návodu budeme používat Správce serveru v systému Windows Server OM instalace služby IIS. Potom vytvoříme sítě zabezpečení skupiny (NSG) pomocí portálu Azure otevřete port 80 IIS přenosy. 

Pokud jste ještě nevytvořili první OM, by měl přejdete zpět k [vytvoření prvního virtuální počítači se systémem Windows Azure portálu](virtual-machines-windows-hero-tutorial.md) před pokračováním v tomto kurzu.

## <a name="make-sure-the-vm-is-running"></a>Zkontrolujte, jestli že je spuštěný OM

1. Otevřete [portál Azure](https://portal.azure.com).
2. V nabídce centrální klikněte na **virtuálních počítačích**. Vyberte virtuální počítač ze seznamu.
3. Pokud je stav **Zastaveno (Deallocated)**, klikněte na tlačítko **Start** na zásuvné **Essentials** OM. Pokud je stav **spuštěná**, můžete na přesouvat k dalšímu kroku.

## <a name="connect-to-the-virtual-machine-and-sign-in"></a>Připojení k virtuálního počítače a přihlášení

1.  V nabídce centrální klikněte na **virtuálních počítačích**. Vyberte virtuální počítač ze seznamu.

3. Na zásuvné virtuálního počítače klikněte na **Připojit**. Vytvoří a soubory ke stažení souboru protokolu RDP (soubor RDP) je jako místní pro připojení k počítači. Můžete chtít soubor uložit do počítače k nim usnadnili přístup. **Otevřete** tento soubor připojení k vaší OM.

    ![Snímek obrazovky znázorňující, jak se připojit k vaší OM Azure portálu](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. Zobrazí se upozornění, která je RDP od neznámých vydavatele. Toto je normální. V okně Vzdálená plocha klikněte na **Připojit** k pokračovat.

    ![Snímek obrazovky s upozorněním týkajícím se o neznámý vydavatel](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. V okně zabezpečení systému Windows zadejte uživatelské jméno a heslo místního účtu, který jste vytvořili při vytvoření OM. Uživatelské jméno se zadává jako *vmname*& #92; *uživatelské jméno*, klikněte na tlačítko **OK**.

    ![Snímek obrazovky s zadávání OM jméno, uživatelské jméno a heslo](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  Zobrazí se upozornění, že certifikát nelze ověřit. Toto je normální. Kliknutím na tlačítko **Ano** ověřit identitu virtuálního počítače a dokončení přihlášení.

    ![Snímek obrazovky zobrazující zprávu o ověření identity OM](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Pokud budete mít s něčím problémy při pokusu o připojení, přečtěte si článek [Poradce při potížích s vzdálené plochy připojení k serveru s Windows Azure virtuálního počítače](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="install-iis-on-your-vm"></a>Instalace služby IIS na vaše OM

Teď jste přihlášeni bude v angličtině, role serveru jsme nainstaluje, takže si můžete pohrát více.

1. Pokud ještě není otevřený, spusťte **Správce serveru** . Klepněte na tlačítko **Start** a potom klikněte na **Správce serveru**.
2. Ve **Správci serveru**vyberte v levém podokně **Místního serveru** . 
3. V nabídce vyberte **Spravovat** > **Přidat rolí a funkcí**.
4. V Průvodci funkcí na stránce **Typ instalace** a přidat role zvolte **instalace na základě rolí nebo založeného na funkcích**a klikněte na tlačítko **Další**.

    ![Snímek obrazovky s kartou přidat role a Průvodce funkcí pro typ instalace](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. Vyberte OM z fondu serveru a klikněte na tlačítko **Další**.
6. Na stránce **Role serveru** vyberte **Webový Server (IIS)**.

    ![Snímek obrazovky s kartou přidat role a Průvodce funkcí pro role serveru](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. V místní nabídce o tom, že funkce potřebné pro služby IIS Ujistěte se, že je vybraná možnost **zahrnout nástroje pro správu** a pak klikněte na **Přidat funkce**. Místní nabídce zavře, klepněte na tlačítko **Další** v průvodci.

    ![Snímek obrazovky zobrazující místní k potvrzení přidání roli služby IIS](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. Na stránce funkce klikněte na **Další**.
9. Na stránce **Webový Server Role (IIS)** klikněte na **Další**. 
10. Na stránce **Služby rolí** klikněte na **Další**. 
11. Na stránce **potvrzení** klikněte na **instalovat**. 
12. Po dokončení instalace klikněte na tlačítko **Zavřít** v průvodci.



## <a name="open-port-80"></a>Otevřete port 80 

Aby OM přijmout příchozí komunikace přes port 80 budete muset přidat pravidlo pro příchozí připojení ke skupině zabezpečení sítě. 

1. Otevřete [portál Azure](https://portal.azure.com).
2. Ve **virtuálních počítačích** vyberte OM, který jste vytvořili.
3. V části Nastavení virtuálních počítačích vyberte **Síťová rozhraní** a pak vyberte existující rozhraní sítě.

    ![Snímek obrazovky zobrazující nastavení virtuálního počítače pro rozhraní sítě](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. **Základy** pro rozhraní sítě klikněte na **skupiny zabezpečení sítě**.

    ![Snímek obrazovky zobrazující části Essentials pro rozhraní sítě](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. V zásuvné **Essentials** pro NSG byste si měli jednoho existující výchozí příchozí pravidla pro **výchozí povolit rdp** , který umožňuje přihlásit bude v angličtině. Přidá další příchozí pravidla přenosy služby IIS. Klikněte na **pravidlo pro příchozí zabezpečení**.

    ![Snímek obrazovky zobrazující části Essentials pro NSG](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. V **Příchozí pravidla zabezpečení**klikněte na **Přidat**.

    ![Snímek obrazovky s tlačítkem pro přidání pravidlo zabezpečení](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. V **Příchozí pravidla zabezpečení**klikněte na **Přidat**. Zadejte **80** v rozsahu portů a ujistěte se, že je vybraná možnost **Povolit** . Až budete hotovi, klikněte na **OK**.

    ![Snímek obrazovky s tlačítkem pro přidání pravidlo zabezpečení](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
Další informace o NSGs, příchozí a odchozí pravidla, najdete v článku [Povolit externí přístup k vaší OM pomocí portálu Azure](virtual-machines-windows-nsg-quickstart-portal.md)
 
## <a name="connect-to-the-default-iis-website"></a>Připojení k webu služby IIS výchozí

1. Na portálu Azure klikněte na **virtuálních počítačích** a potom vyberte svůj OM.
2. V zásuvné **Essentials** zkopírujte **veřejnou IP adresu**.

    ![Snímek obrazovky ukazující, kde najdete veřejnou IP adresu vašeho OM](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Otevřete prohlížeč a do adresního řádku zadejte veřejnou IP adresu takto: http://<publicIPaddress> a klepněte na **Enter** přejděte na tuto adresu.
3. Prohlížeče otvírat výchozí IIS webovou stránku. Vypadá přibližně takto:

    ![Snímek obrazovky ukazující, jak výchozí stránky služby IIS vypadá v prohlížeči](./media/virtual-machines-windows-hero-tutorial/iis-default.png)

    

## <a name="next-steps"></a>Další kroky

- Můžete taky vyzkoušet [připojení disk dat](virtual-machines-windows-attach-disk-portal.md) do virtuálního počítače. Data disků poskytují další úložiště pro virtuálního počítače.
