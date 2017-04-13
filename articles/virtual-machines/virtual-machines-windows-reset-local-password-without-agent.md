<properties
   pageTitle="Resetování hesla místní Windows Azure hosty agent není nainstalovaná | Microsoft Azure"
   description="Jak resetovat heslo místní účet systému Windows po Azure hosta agent není nainstalovaná nebo funkční na virtuálního počítače"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="iainfou"/>

# <a name="how-to-reset-local-windows-password-for-azure-vm"></a>Jak resetovat heslo místního systému Windows pro OM Azure
Místní systém Windows heslo OM v Azure pomocí na [Azure portál nebo Azure PowerShell](virtual-machines-windows-reset-rdp.md) za předpokladu, že je nainstalovaný agenta Azure hosta můžete obnovit. Tento způsob je primární způsob resetování hesla pro OM Azure. Pokud dojde k potížím s agentem Azure hosta neodpovídá nebo když nefunguje k instalaci po odeslání vlastní obrázek, můžete ručně resetování hesla pro systém Windows. Tento článek popisuje, jak resetovat heslo místního účtu připojením virtuální disk zdroje s operačním systémem jiné v angličtině. 

> [AZURE.WARNING] Tento postup můžete používejte jenom jako poslední možnost. Vždy pokusu o resetování hesla pomocí [Azure portál nebo Azure PowerShell](virtual-machines-windows-reset-rdp.md) nejdřív.


## <a name="overview-of-the-process"></a>Přehled procesu
Základní kroky k provádění místní resetování hesla pro Windows OM v Azure po přístup k agenta Azure hosty vypadá takto:

- Odstranění zdroje OM. Virtuální disků se zachovají.
- Připojení k jiné OM v rámci předplatného Azure OM zdroje s operačním systémem disku. Tento OM nazývá Poradce při potížích OM.
- Pomocí Poradce při potížích OM, vytvořte některé soubory konfigurace na disku OS OM zdroje.
- Odpojení disku OS OM z Poradce při potížích OM.
- Pomocí Správce prostředků šablony vytvoříte OM pomocí původní virtuální disk.
- Při spuštění nové OM konfigurace soubory, které vytvoříte aktualizujte heslo požadované uživatele.


## <a name="detailed-steps"></a>Podrobný postup
Vždy pokusu o resetování hesla pomocí [Azure portál nebo Azure PowerShell](virtual-machines-windows-reset-rdp.md) před pokusem podle těchto kroků. Zkontrolujte, jestli že máte záložní kopii vaší OM před spuštěním. 

1. Odstranění problémového OM Azure portálu. Odstranění OM pouze odstraní metadata, odkaz OM v Azure. Virtuální disků se zachovají, když OM odstraněna:

    - Vyberte OM na portálu Azure, klikněte na *Odstranit*:

    ![Odstranění existující OM](./media/virtual-machines-windows-reset-local-password-without-guest-agent/delete_vm.png)

2. Připojte OM zdroje s operačním systémem disk Poradce při potížích angličtině. Poradce při potížích OM musí být ve stejné oblasti jako zdroj OM OS disk (třeba `West US`):

    - Vyberte Poradce při potížích OM Azure portálu. Klikněte na *disků* | *existující připojení*:

    ![Připojit existující disk](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_attach_existing.png)

    Vyberte *Soubor virtuálního pevného disku* a vyberte účet úložiště, který obsahuje zdroj OM:

    ![Vyberte účet úložiště](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_storageaccount.PNG)

    Vyberte zdroj kontejner. Obal zdroje je obvykle *VHD*:

    ![Vyberte kontejner úložiště](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_container.png)

    Vyberte virtuální pevný disk OS připojit. Kliknutím na tlačítko Dokončit proces *Vyberte* :

    ![Vyberte zdroj virtuální disk](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_source_vhd.png)

3. Připojení k řešení potíží OM pomocí vzdálené plochy a zajistěte, aby že zobrazený OM zdroje s operačním systémem disku:

    - Vyberte Poradce při potížích OM Azure portálu a klikněte na *Připojit*.
    - Otevřete soubor RDP, který ke stažení. Zadejte uživatelské jméno a heslo Poradce při potížích OM.
    - V Průzkumníku souborů vyhledejte disku data, která jste připojili. Jestliže je zdrojem virtuální pevný disk na OM pouze data disku připojeného k řešení potíží OM, třeba F: jednotky:

    ![Prohlížení disk připojená data](./media/virtual-machines-windows-reset-local-password-without-guest-agent/troubleshooting_vm_fileexplorer.png)

4. Vytvoření `gpt.ini` v `\Windows\System32\GroupPolicy` na disku OM zdroj (pokud existuje gpt.ini přejmenujte na gpt.ini.bak):

    > [AZURE.WARNING] Ujistěte se, že nevytvoříte omylem následující soubory v C:\Windows jednotku OS pro řešení potíží OM. Vytvořte následující soubory v jednotce OS pro příslušný zdroj OM, který je připojen jako data disk.

    - Přidejte následující řádky v `gpt.ini` vytvořený soubor:

    ```
    [General]
    gPCFunctionalityVersion=2
    gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
    Version=1
    ```

    ![Vytvoření gpt.ini](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_gpt_ini.png)
 
5. Vytvoření `scripts.ini` v `\Windows\System32\GroupPolicy\Machine\Scripts`. Zkontrolujte, že se zobrazí skryté složky. V případě potřeby vytvořte `Machine` nebo `Scripts` složky.

    - Přidejte následující řádky `scripts.ini` vytvořený soubor:

    ```
    [Startup]
    0CmdLine=C:\Windows\System32\FixAzureVM.cmd
    0Parameters=
    ```

    ![Vytvoření scripts.ini](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_scripts_ini.png)
 
6. Vytvoření `FixAzureVM.cmd` v `\Windows\System32` s tímto obsahem nahrazení `<username>` a `<newpassword>` s vlastními hodnotami:

    ```
    NET USER <username> <newpassword>
    ```

    ![Vytvoření FixAzureVM.cmd](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_fixazure_cmd.png)

    Požadavky na složitost nastavené heslo pro váš OM musí splňovat při definování nové heslo.

7. Azure portálu odpojení disku z Poradce při potížích OM:

    - Vyberte Poradce při potížích OM na portálu Azure, klikněte na *disku*.
    - Vyberte data disku připojeného v kroku 2, klikněte na *Odebrat*:

    ![Odpojení disku](./media/virtual-machines-windows-reset-local-password-without-guest-agent/detach_disk.png)

8. Před vytvořením virtuálního počítače získejte URI na disk OS zdroje:

    - Vyberte účet, úložiště na portálu Azure, klikněte na *objekty BLOB*.
    - Vyberte kontejner. Obal zdroje je obvykle *VHD*:

    ![Vyberte účet úložiště objektů blob](./media/virtual-machines-windows-reset-local-password-without-guest-agent/select_storage_details.png)

    Vyberte zdroj OM OS virtuální pevný disk a klikněte na tlačítko *Kopírovat* vedle názvu *adresy URL* :

    ![Kopírování disku URI](./media/virtual-machines-windows-reset-local-password-without-guest-agent/copy_source_vhd_uri.png)

9. Vytvoření virtuálního počítače z disku OS OM zdroje:

    - Pomocí [této šablony správce prostředků Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) vytvoření virtuálního počítače z specializované virtuálního pevného disku. Klikněte `Deploy to Azure` s cílem otevřít portálu Azure se šablonami informace vyplněné za vás.
    - Pokud chcete zachovat všechny předchozí nastavení OM, vyberte *Upravit šablonu* k poskytování existující VNet, podsítě, síťový adaptér nebo veřejnou IP.
    - V `OSDISKVHDURI` parametr textové pole, vložit URI zdroj virtuální pevný disk získat v předchozím kroku:

    ![Vytvoření virtuálního počítače ze šablony](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_new_vm_from_template.png)

10. Po spuštění nové OM připojení k OM pomocí vzdálené plochy pomocí nového hesla jste zadali v `FixAzureVM.cmd` skriptu.

11. Z vaší vzdálenou relaci nové OM odeberte následující soubory vyčištění prostředí:

    - Z %windir%\System32
        - odebrání FixAzureVM.cmd
    - Z %windir%\System32\GroupPolicy\Machine\
        - odebrání scripts.ini
    - Z %windir%\System32\GroupPolicy
        - odebrání gpt.ini (je-li gpt.ini existoval a přejmenovat na gpt.ini.bak, přejmenování souboru BAK zpět gpt.ini)

## <a name="next-steps"></a>Další kroky
Pokud se stále nemůžete připojit pomocí vzdálené plochy, najdete v článku [Poradce při potížích Průvodce RDP](virtual-machines-windows-troubleshoot-rdp-connection.md). [Podrobné pokyny pro řešení potíží RDP](virtual-machines-windows-detailed-troubleshoot-rdp.md) sleduje Poradce při potížích s metody spíše než konkrétní kroky. Můžete také [Otevřete žádost o podporu Azure](https://azure.microsoft.com/support/options/) praktických požádejte o pomoc.