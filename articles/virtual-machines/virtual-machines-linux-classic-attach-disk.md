<properties
    pageTitle="Přiložení disk Linux OM | Microsoft Azure"
    description="Zjistěte, jak připojit disk dat Azure virtuální počítači spuštěná Linux a inicializace, takže je připraven k použití."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Jak připojit Disk dat do virtuálního počítače Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]V tématu Jak [Připojit disk dat pomocí nasazení modelu správce prostředků](virtual-machines-linux-add-disk.md).

Můžete připojit prázdné disků a disků, které obsahují data, která chcete Azure VMs. Jsou oba typy disků VHD soubory, které se nacházejí v účet Azure úložiště. Jako s přidáváním jakýkoli disk do počítače Linux po připojte disk je třeba inicializace a naformátovat ji tak, aby je připraven k použití. Tento článek podrobnosti připojení prázdné disků a disků už obsahující data, která chcete svůj VMs, a taky jak potom inicializace a naformátujte nový disk.

> [AZURE.NOTE]Je nejvhodnější použít jeden nebo víc samostatných discích k ukládání dat virtuálního počítače. Při vytváření Azure virtuální počítač má disk operačního systému a dočasné disku. **Nepoužívejte dočasné disku uložit trvalý data.** Jak naznačuje název obsahuje dočasné úložiště. Vzhledem k tomu, že není uložena v Azure úložiště nabízí žádné redundance nebo zálohování.
> Dočasné disku je obvykle spravuje Azure Linux Agent a automaticky připojit **/mnt/resource** (nebo **/mnt** na systémem Ubuntu obrázky). Na druhé straně disk dat může být názvem jádra Linux nějak `/dev/sdc`, a budete muset oddíl, formátování a připojit tohoto zdroje. V [Azure Linux Agent User Guide] [ Agent] podrobnosti.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Inicializace nového disku dat v Linux

1. SSH vaší v angličtině. Další informace najdete v článku [jak se přihlásit k virtuální počítač Linux][Logon].

2. Dál potřebujete najít identifikátor zařízení datový disk inicializace. Existují dva způsoby, jak to udělat:

    a) Grep pro zařízení SCSI v protokolech, například tento příkaz:

            $sudo grep SCSI /var/log/messages

    Pro poslední systémem Ubuntu rozdělení, budete muset používat `sudo grep SCSI /var/log/syslog` protože protokolování `/var/log/messages` mohou být zakázány ve výchozím nastavení.

    Můžete najít identifikátor posledního data disku, který byl přidán do zprávy, které se zobrazí.

    ![Získání zprávy disku](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)

    NEBO

    b) použití `lsscsi` příkaz zjistěte id zařízení. `lsscsi`je možné nainstalovat některým `yum install lsscsi` (na červené klobouk základě distribuce) nebo `apt-get install lsscsi` (na Debian základě distribuce). Můžete najít na disk, který hledáte _logické jednotky_ nebo **číslo logické jednotky**. Třeba _logické jednotky_ disků připojené můžete snadno vidět z `azure vm disk list <virtual-machine>` jako:

            ~$ azure vm disk list TestVM
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        TestVM-2645b8030676c8f8.vhd  Linux
            data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    Porovnání tato data pomocí výstup `lsscsi` pro stejný přehrajte virtuálního počítače:

            ops@TestVM:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc

    Poslední číslo v n-tici v jednotlivých řádcích je _logické jednotky_. Viz `man lsscsi` Další informace.

3. Do příkazového řádku zadejte následující příkaz a vytvořte zařízení:

        $sudo fdisk /dev/sdc


4. Po zobrazení výzvy zadejte **n** vytvořte nový oddíl.


    ![Vytvoření zařízení](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Po zobrazení výzvy zadejte **p** změnit oddíl primární oddíl. Zadejte hodnotu **1** usnadnit první oddíl a zadejte typ přijměte výchozí hodnoty pro lahve. V některých sítě ji můžete zobrazit výchozí hodnoty první a poslední odvětví místo lahve. Můžete přijmout tyto výchozí hodnoty.


    ![Vytvoření oddílu](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartdetails.png)



6. Zadejte **p** znemožnit zobrazení podrobností o disku, který je právě oddíly.


    ![Informace o seznamu disku](./media/virtual-machines-linux-classic-attach-disk/fdiskpartitiondetails.png)



7. Zadejte **w** psát nastavení disku.


    ![Psaní změny disku](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. Teď můžete vytvořit systém souborů na nový oddíl. Přidat číslo oddílu do ID zařízení (v následujícím příkladu `/dev/sdc1`). Následující příklad vytvoří oddíl ext4 na /dev/sdc1:

        # sudo mkfs -t ext4 /dev/sdc1

    ![Vytvoření systému souborů](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)

    >[AZURE.NOTE] Systémy SuSE Linux Enterprise 11 podporují pouze jen pro čtení pro systémy ext4 souborů. Tyto systémy v počítačích doporučujeme formátu ext3 spíše než ext4 nového systému souborů.


9. Zkontrolujte adresář, který chcete připojit nová systému souborů takto:

        # sudo mkdir /datadrive


10. Nakonec můžete připojit jednotku, následujícím způsobem:

        # sudo mount /dev/sdc1 /datadrive

    Disk dat je nyní připravena k použití jako **/datadrive**.
    
    ![Vytvořit adresář a připojit disk](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)


11. Přidání nové jednotky /etc/fstab:

    Zajistit že jednotku se znovu připojit automaticky po restartování musí být přidán do souboru fstab/atd /. Kromě toho důrazně doporučujeme, aby UUID (všeobecně jedinečný identifikátor) se používá v /etc/fstab odkázat na jednotku, nikoli pouze název zařízení (tedy /dev/sdc1). Použití UUID zabráněno nesprávné disku je připojen do zadaného umístění, pokud operační systém rozpozná chybu disku při spuštění a všech zbývajících dat disků potom přiřazený ID těchto zařízení. Najít UUID nové jednotky, můžete použít nástroj **blkid** :

        # sudo -i blkid

    Výstup vypadá nějak takto:

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] Nesprávně úpravy souboru **/etc/fstab** může vést k systému nelze spustit. Pokud si jisti, informace v nápovědě k funkci rozdělení informace o tom, jak správně upravit tento soubor. Doporučujeme také vytvořené záložní kopii souboru /etc/fstab před úpravou.

    Potom otevřete soubor **/etc/fstab** v textovém editoru:

        # sudo vi /etc/fstab

    V tomto příkladu použijeme hodnotu UUID **/dev/sdc1** zařízení, která byla vytvořená v předchozí kroky a BodPřipojení **/datadrive**. Přidejte následující řádek na konec **/etc/fstab** souboru:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2

    Nebo v systémech podle SuSE Linux budete muset mírně odlišnou formát:

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    
    >[AZURE.NOTE] `nofail` Možnost zaručuje, že OM spustí i v případě souborů je poškozený nebo disk neexistuje při spuštění. Bez tuto možnost můžete narazit chování podle [Nelze SSH Linux angličtině důsledku FSTAB chyb](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Nyní můžete otestovat, že připojení k systému souborů správně odpojení a potom tedy remounting systému souborů v příkladu připojit čárky `/datadrive` vytvořených v předchozích krocích:

        # sudo umount /datadrive
        # sudo mount /datadrive

    Pokud `mount` příkaz vytvoří chybu, zkontrolujte/atd/fstab souboru pro správné syntaxe. Pokud se vytvářejí další data jednotek nebo oddílů, zadejte je do pole/atd/fstab samostatně taky.

    Zkontrolujte jednotku zapisovatelný pomocí tohoto příkazu:

        # sudo chmod go+w /datadrive

>[AZURE.NOTE] Následně odebrání data disk bez úprav fstab může způsobit OM nepodařilo spustit. Pokud je to běžné výskyt, většina distribuce poskytují `nofail` a/nebo `nobootwait` fstab možnosti, které umožňují systému spuštění i v případě, že disk nepodaří připojit spuštění. Další informace o těchto parametrech naleznete v dokumentaci vaší rozdělení.

### <a name="trimunmap-support-for-linux-in-azure"></a>Střih/UNMAP podpora Linux v Azure
Některé Linux jádra podporují střih/UNMAP operace kliknutím na Zrušit nepoužívané bloky na disku. Tyto operace je vhodné zejména v standardní úložiště informovat Azure, která odstraněné stránky už nejsou platné a můžete zrušit. Zrušení stránky můžete ukládat náklady, pokud vytvořte velkých souborů a potom odstraňte je.

Existují dva způsoby, jak povolit střih podpory OM Linux. Jako obvykle naleznete distribučního doporučený postup:

- Použití `discard` připojit možnost v `/etc/fstab`, například:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Případně můžete spustit `fstrim` příkaz ručně z příkazového řádku nebo přidat do crontab pravidelně spuštění:

    **Se systémem Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL/CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Řešení potíží
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]


## <a name="next-steps"></a>Další kroky
Další informace o používání Linux OM v těchto článcích:

- [Jak se přihlásit k virtuální počítač Linux][Logon]

- [Jak se odpojit disk od Linux virtuálního počítače](virtual-machines-linux-classic-detach-disk.md)

- [Rozhraní příkazového řádku Azure pomocí klasického nasazení modelu](../virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-mac-create-ssh-keys.md
