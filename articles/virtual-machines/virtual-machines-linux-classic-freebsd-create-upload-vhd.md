<properties
   pageTitle="Vytvořte a uložte obrázek FreeBSD OM | Microsoft Azure"
   description="Zjistěte, jak vytvořit a nahrát virtuální pevný disk (virtuální pevný disk), která obsahuje operačního systému FreeBSD vytvoření Azure virtuálního počítače"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/29/2016"
   ms.author="kyliel"/>

# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Vytvořte a uložte FreeBSD virtuální pevný disk na Azure

Tento článek popisuje, jak vytvořit a nahrát virtuální pevný disk (virtuální pevný disk), která obsahuje FreeBSD operační systém. Po odeslání, můžete ho jako vlastní obrázek vytvoření virtuálního počítače (OM) v Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Zjistit předpoklady pro
V tomto článku se předpokládá, že máte následující položky:

- **Azure předplatné**– Pokud nemáte účet, můžete vytvořit v jenom pár minut. Pokud máte předplatné MSDN, přečtěte si článek [měsíční Azure dobru Visual Studio předplatitele.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). V opačném zjistěte, jak [vytvořit účet bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).  

- **Nástroje azure PowerShell**– Azure PowerShell modul musí být nainstalovaná a nakonfigurovaný na používání předplatného Azure. Ke stažení modulu, najdete v článku [stažení Azure](https://azure.microsoft.com/downloads/). Kurz, který popisuje, jak nainstalovat a nakonfigurovat modul je k dispozici. Odeslání virtuálního pevného disku získáte pomocí rutiny [Azure stáhne](https://azure.microsoft.com/downloads/) .

- **Operační systém FreeBSD nainstalovaných v souboru VHD**– podporované FreeBSD operačním systémem musí být nainstalovaný virtuální pevný disk. Vytvářet soubory VHD existují více nástroje. Můžete například řešení virtualizace například Hyper-V vytvořit soubor VHD a nainstalovat operačního systému. Pokyny o tom, jak nainstalovat a používat Hyper-V najdete v tématu [instalace pro Hyper-V a vytvoření virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Novější formát VHDX není podporován v Azure. Disk můžete převést do formátu virtuální pevný disk pomocí Hyper-V správce nebo rutina [převést virtuální pevný disk](https://technet.microsoft.com/library/hh848454.aspx). Kromě toho je [kurz na webu MSDN o FreeBSD s Hyper-V použití](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

Tento úkol zahrnuje následující pět kroků.

## <a name="step-1-prepare-the-image-for-upload"></a>Krok 1: Příprava nahrát obrázek

Na počítači virtuální nainstalovanou FreeBSD operační systém proveďte následující postupy:

1. Povolte DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. Povolte SSH.

    SSH aktivované ve výchozím nastavení po dokončení instalace z disku. Pokud ještě není povolená z nějakého důvodu nebo přímo použít FreeBSD virtuálního pevného disku, zadejte tento příkaz:

        # echo 'sshd_enable="YES"' >> /etc/rc.conf
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
        # service sshd restart

3. Nastavte si sériové konzoly.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Nainstalujte sudo.

    Účtu root zakázaná v Azure. To znamená, že budete muset používat sudo z neoprávněnému uživateli spustit příkazy se zvýšenými oprávněními.

        # pkg install sudo
;
5. Požadavky pro Azure Agent.

        # pkg install python27  
        # pkg install Py27-setuptools27   
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git

6. Nainstalujte Azure Agent.

    Nejnovější verzi Azure Agent vždy se nachází na [github](https://github.com/Azure/WALinuxAgent/releases). Verze 2.0.10 + úředně podporuje FreeBSD 10 a 10.1 a verze 2.1.4 úředně podporuje FreeBSD 10.2 a novějších verzích.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    U 2.0 použijeme 2.0.16 jako příklad:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    Pro 2.1 použijeme 2.1.4 jako příklad:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Po instalaci Azure Agent je též vhodné ověřit, jestli je spuštěný:

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

7. Deprovision systému.

    Deprovision systému jej vyčistit a vhodné pro znovu zřizování. Následující příkaz odstraněny také poslední zřizování uživatelský účet a Spojenými daty:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Teď můžete vypnout vaše OM.

## <a name="step-2-create-a-storage-account-in-azure"></a>Krok 2: Vytvoření účtu úložiště v Azure ##

Je třeba úložiště účtu v Azure nahrát soubor VHD, aby mohou sloužit k vytvoření virtuálního počítače. Portál Azure klasické slouží k vytvoření účtu úložiště.

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).

2. Na panelu s příkazy vyberte **Nový**.

3. Vyberte **datové služby** > **úložiště** > **vytvořit**.

    ![Rychlé vytvoření účtu úložiště](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Vyplňte pole následujícím způsobem:

    - Do pole **Adresa URL** zadejte název subdoménu používat v adrese URL účtu úložiště. Položka může obsahovat ze 3 – 24 čísel a malá písmena. Tento název se změní název hostitele v rámci adresu URL, která slouží k řešení úložiště objektů Blob Azure, úložiště Azure fronty nebo tabulky Azure prostředků úložiště u předplatného.

    - V rozevírací nabídce **Umístění/spřažení skupiny** zvolte **umístění nebo skupinu spřažení** účtu úložiště. Skupina spřažení umožňuje umístit cloudovými službami a úložiště ve stejném datovém centru.

    - V poli **replikace** rozhodněte, jestli můžete **Geo nadbytečné** replikace u účtu úložiště. Replikace GEO je ve výchozím nastavení zapnutá. Tuto možnost zreplikuje dat na vedlejší umístění zdarma, tak, aby úložišti selhání na uvedeném místě pokud hlavní výpadku na hlavním pracovišti. Sekundární umístění se automaticky přiřadí a nelze změnit. Pokud potřebujete větší kontrolu nad umístění svého cloudového úložiště kvůli právní požadavky nebo organizační zásad, můžete vypnout geo replikace. Ale mějte na paměti, že pokud zapnete později geo replikace, můžete se bude účtovat poplatek přenos jednorázové dat replikovat existujících dat do vedlejší umístění. Služba úložiště bez geo opakování nabídnuta se slevou. Další informace o správě geo replikace osnovu úložiště najdete tady: [Vytvoření, spravovat, nebo odstraněním účtu úložiště](../storage-create-storage-account/#replication-options).

    ![Zadejte podrobnosti o účtu úložiště](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Vyberte možnost **vytvořit účet úložiště**. Účet se nyní zobrazí v části **úložiště**.

    ![Úspěšně vytvořený účet úložiště](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Dále vytvořte kontejner pro nahrané VHD souborů. Vyberte název účtu úložiště a pak vyberte **kontejnery**.

    ![Podrobnosti o účtu úložiště](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Vyberte možnost **vytvořit kontejner**.

    ![Podrobnosti o účtu úložiště](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. Do pole **název** zadejte název pro váš kontejner. Pak v rozevírací nabídce **přístup** vyberte, jaký typ přístupu chcete.

    ![Název kontejneru](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Ve výchozím nastavení kontejneru je soukromá a přístupný pouze vlastníkem účtu. Povolit veřejné přístup pro čtení pro objekty BLOB v kontejneru, ale ne na kontejner vlastnosti a metadata, použijte možnost **Veřejné objektů Blob** . Umožňuje důkladné veřejné přístup pro čtení pro kontejner a objekty BLOB pomocí možnosti **Veřejné kontejner** .

## <a name="step-3-prepare-the-connection-to-azure"></a>Krok 3: Příprava připojení k Azure

Před odesláním souboru VHD, musíte použít zabezpečené připojení mezi počítačem a Azure předplatné. K tomuto můžete metodu Azure Active Directory (Azure AD) nebo certifikát.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Použijte metodu Azure AD k nahrání soubor VHD

1. Spusťte konzolu Azure Powershellu.

2. Zadejte tento příkaz:  
    `Add-AzureAccount`

    Tento příkaz otevře okno přihlásit, kde se přihlásit pomocí svého pracovního nebo školního účtu.

    ![Okno prostředí PowerShell](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure ověří a ukládá přihlašovacími údaji. Potom zavře okno.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Použijte metodu certifikát k nahrání soubor VHD

1. Spusťte konzolu Azure Powershellu.

2. Typ:  `Get-AzurePublishSettingsFile`.

3. Otevře okno prohlížeče a zobrazí výzvu ke stažení souboru .publishsettings. Tento soubor obsahuje informace a certifikát Azure předplatné.

    ![Stažení stránku prohlížeče](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Uložte soubor .publishsettings.

4. Typ:  `Import-AzurePublishSettingsFile <PathToFile>`, kde `<PathToFile>` je úplnou cestu k souboru .publishsettings.

   Další informace najdete v tématu [Začínáme s Azure rutiny](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Další informace o instalaci a konfiguraci Powershellu najdete v článku [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).

## <a name="step-4-upload-the-vhd-file"></a>Krok 4: Odeslání souboru VHD

Když uložíte soubor VHD, ho můžete kdekoli v úložišti objektů Blob. Tady jsou některé termíny, které budete používat při odeslání souboru:
-  **BlobStorageURL** je adresa URL úložiště účet, který jste vytvořili v kroku 2.
-  **YourImagesFolder** je kontejner v úložišti objektů Blob místo, kam chcete ukládat obrázky.
- **VHDName** je popisek, který se zobrazí na portálu Azure klasické k identifikaci virtuální pevný disk.
- **PathToVHDFile** je úplnou cestu a název souboru VHD.


V okně Azure PowerShell, který jste použili v předchozím kroku zadejte:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Krok 5: Vytvoření virtuálního počítače se souborem nahraný VHD
Když nahrajete soubor VHD, můžete ho přidat jako obrázek do seznamu vlastní obrázky, které jsou přidružené k vašemu předplatnému a vytvoření virtuálního počítače s Tento vlastní obrázek.

1. V okně Azure PowerShell, který jste použili v předchozím kroku zadejte:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Jako typ OS použijte Linux. Aktuální verzi Azure PowerShell přijímá jenom "Linux" nebo "Windows" jako parametr.

2. Po dokončení předchozích kroků nedá se nový obrázek koncovém při zvolte kartu **obrázky** na portálu Azure klasické.  

    ![Zvolte obrázek](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Vytvoření virtuálního počítače z galerie. Tento nový obrázek je teď k dispozici v části **Moje obrázky**.
4. Vyberte nový obrázek. Pak přejděte podle pokynů k nastavení název hostitele, heslo, SSH klíče a tak dál.

    ![Vlastní obrázek](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Po dokončení zřizování, zobrazí se vaše FreeBSD OM spuštěné v Azure.

    ![Obrázek FreeBSD v azure](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)
