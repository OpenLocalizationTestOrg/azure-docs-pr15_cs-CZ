<properties
   pageTitle="Nasazení StorSimple virtuální maticový – ustanovení Hyper-V"
   description="Tento druhý kurz virtuální pole StorSimple nasazení zahrnuje zřizování virtuální zařízení v Hyper-V."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/11/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-hyper-v"></a>Nasazení StorSimple virtuální maticový – zřízení virtuální matice v Hyper-V

![](./media/storsimple-ova-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Základní informace

Tento kurz zřizovací platí všeobecně dostupná (GA) verzi pracovního dne 2016 Microsoft Azure StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení nebo StorSimple virtuální zařízení). Tento kurz popisuje zřízení StorSimple virtuální matici na hostitele systému Hyper-V systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2. Tento článek se týká nasazení StorSimple virtuální matice v Azure klasické portál, jakož i cloudu společnosti Microsoft Azure pro státní správu.

Budete potřebovat oprávnění správce by bylo možné vytvořit a konfigurace virtuální zařízení. Nastavení zřizování a počáteční může trvat zhruba 10 minut dokončete.


## <a name="provisioning-prerequisites"></a>Zřízení požadavky

Tady zjistíte požadavky zřízení virtuální zařízení v systému Host (hostitel) se systémem Hyper-V systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2.

### <a name="for-the-storsimple-manager-service"></a>Pro službu StorSimple správce

Než začnete, ujistěte se, že:

-   Dokončení všech kroků v tématu [Příprava na portálu pro StorSimple virtuální pole](storsimple-ova-deploy1-portal-prep.md).

-   Obrázek virtuální zařízení pro Hyper-V jste stáhli z portálu Microsoft Azure. Další informace najdete v tématu [Krok 3: Stáhněte si obrázek virtuální zařízení](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

    > [AZURE.IMPORTANT] Software pro pole virtuální StorSimple může použít pouze ve spojení se službou Storsimple správce.

### <a name="for-the-storsimple-virtual-device"></a>Virtuální zařízení StorSimple

Před nasazením virtuální zařízení, ujistěte se, že:

-   Máte přístup k systému hostitele Hyper-V systému Windows Server 2008 R2 nebo novější, který může být použit na základě zařízení.

-   Systém hostitele je možné zajistíte v následujících zdrojích zřízení virtuální zařízení:

    -   Minimální 4 jádra.

    -   Alespoň na úrovni 8 GB paměti RAM.

    -   Rozhraní jedné sítě.

    -   Virtuální disk 500 GB pro data systému.

### <a name="for-the-network-in-the-datacenter"></a>V síti datacentra

Než začnete, projděte si sítě požadavků nasadit virtuální zařízení StorSimple a konfigurace sítě datacentra řádně podporovat. Další informace najdete v tématu [požadavky sítě StorSimple virtuální pole](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Podrobné zřízení

Zřízení a připojte k virtuální zařízení, musíte provést následující postup:

1.  Zajistěte, aby měl systému hostitele dostatečné zdroje požadavkům minimální virtuální zařízení.

2.  Zřízení virtuální zařízení ve vaší hypervisoru.

3.  Zahájení virtuální zařízení a najděte IP adresu.

Každý z těchto kroků jsou vysvětleny v následujících částech.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements"></a>Krok 1: Ověřte, zda systému hostitele splňuje požadavky na minimální virtuální zařízení

Pokud chcete vytvořit virtuální zařízení, budete potřebovat:

-   Roli Hyper-v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2 SP1 nainstalovaný.

-   Správce pro Hyper-V Microsoft v klientském počítači Microsoft Windows připojení k hostiteli.

Ujistěte se, že je možné vyhrazeného v následujících zdrojích pro zařízení s virtuální základní hardware (systém hostitele) na které vytváříte virtuální zařízení:

- Minimální 4 jádra.
- Alespoň na úrovni 8 GB paměti RAM.
- Rozhraní jedné sítě.
- Virtuální disk 500 GB pro data systému.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Krok 2: Zřízení virtuální zařízení v hypervisoru

Proveďte následující kroky zřízení zařízení ve vaší hypervisoru.

#### <a name="to-provision-a-virtual-device"></a>Zřízení virtuální zařízení

1.  Na hostitele systému Windows Server Kopírovat obrázek virtuální zařízení na místní jednotku. Toto je obrázek (virtuálního pevného disku nebo VHDX), který jste stáhli přes Azure portál. Poznamenejte si na místo, kam jste zkopírovali obrázek jako budete používat to později v procesu.

2.  Spusťte **Správce serveru**. V pravém horním rohu klikněte na **Nástroje** a vyberte **Hyper-V správce**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image1.png)

    Pokud používáte Windows Server 2008 R2, spusťte správce pro Hyper-V. Klikněte na správce serveru **rolí > Hyper-V > Správce pro Hyper-V**.

1.  V **Hyper-V správce**v podokně obor klikněte pravým tlačítkem na uzel systém otevřete místní nabídku a pak klikněte na **Nový** > **virtuálního počítače**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image2.png)

1.  Na stránce **než začnete** Průvodce vytvořením virtuálního počítače klikněte na **Další**.

1.  Na stránce **Zadejte název a umístění** zadejte **název** pro virtuální zařízení. Klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image4.png)

1.  Na stránce **generování zadejte** typ obrázku zařízení a potom na tlačítko **Další**. Pokud používáte Windows Server 2008 R2, se nezobrazuje na této stránce.

    * **Analytický nástroj Generátor 2** vyberte, pokud jste si stáhli obrázku .vhdx pro Windows Server 2012 nebo novější.
    * **Analytický nástroj Generátor 1** vyberte, pokud jste si stáhli obrázku VHD pro Windows Server 2008 R2 nebo novější.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image5.png)

1.  Na stránce **přiřadit paměti** zadat **paměti při spuštění** aspoň **8192 MB**, není povolit dynamické paměti a klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image6.png)

1.  Na stránce **Konfigurace sítě** virtuální přepínač, který je připojený k Internetu a klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image7.png)

1.  Na stránce **připojit virtuální pevný disk** vyberte **použít existující virtuální pevný disk**, zadejte umístění obrázku virtuální zařízení (.vhdx nebo VHD) a klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image8m.png)

1.  Zkontrolujte **souhrnné informace** a klepněte na tlačítko **Dokončit** vytvořit virtuální počítač. Ale nechcete přeskočit ještě - potřebujete přidat některé jádra procesoru a druhý jednotky. 

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image9.png)

1.  Minimální požadavky, budete potřebovat 4 jádra. Chcete-li přidat virtuální procesorů systému hostitele vybrané v okně **Správce pro Hyper-V** pravém podokně klikněte v seznamu **virtuálních počítačích**, vyhledejte virtuální počítač, který jste právě vytvořili. Vyberte a klikněte pravým tlačítkem na název počítače a vyberte **Nastavení**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image10.png)

1.  Na stránce **Nastavení** v levém podokně klikněte na **procesor**. V pravém podokně nastavte **počet procesorů virtuální** a 4 (Další). Klikněte na tlačítko **použít**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image11.png)

1.  Minimální požadavky, musíte taky přidat disk virtuální dat 500 GB. Na stránce **Nastavení** :

    1.  V levém podokně vyberte **Řadiče SCSI**.
    2.  V pravém podokně vyberte **Jednotku pevného** a klikněte na **Přidat**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image12.png)

1.  Na stránce **pevném disku** vyberte možnost **virtuální pevný disk** a klikněte na **Nový**. Spustí **Průvodce vytvořením virtuální pevném disku**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image13.png)

1.  Na stránce **než začnete** Průvodce vytvořením virtuální pevného disku klikněte na **Další**.

1.  Na **stránce zvolte formát disku**přijměte výchozí možnost **VHDX** formát. Klikněte na tlačítko **Další**. Pokud používáte Windows Server 2012 R2 nebo Windows Server 2008 R2 neuvidíte tuto obrazovku.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image15.png)

1.  Na **stránce vyberte typ disku**nastavení typu virtuální pevný disk jako **dynamicky rozbalování** (doporučeno). Pokud se rozhodnete **pevnou velikost** disku, budou fungovat i ale budete muset počkat moc dlouho. Doporučujeme, ale nepoužívejte možnost **Differencing** . Klikněte na tlačítko **Další**. Všimněte si, že **dynamicky rozbalování** výchozí nastavení systému Windows Server 2012 R2 a Windows Server 2012. V systému Windows Server 2008 R2 nezdaří výchozí hodnota je **pevnou velikost**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image16.png)

1.  Na stránce **Zadejte název a umístění** poskytují disku dat **názvem** stejně jako **umístění** (můžete přecházet na jednu). Klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image17.png)

1.  Na stránce **Konfigurace Disk** vyberte možnost **vytvořit nový prázdný virtuální pevný disk** a specifikovaly velikost jako 500 (minimálně **GB** ). Minimální požadavky při 500 GB je vždy zřídit větší disku. Všimněte si, že nelze zvětšení nebo zmenšení disku jednou zřízení. Další informace o velikosti disku trvat přečtěte si část pro změnu velikosti v dokumentu [Doporučené postupy](storsimple-ova-best-practices.md#configuration-best-practices) . Klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image18.png)

1.  Na stránce **Souhrn** zkontrolujte podrobnosti disku virtuální dat a splněny, klikněte na tlačítko **Dokončit** vytváření disku. Průvodce se zavře a virtuální pevný disk se přidají do svého počítače.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image19.png)

2.  Se vrátíte na stránku **Nastavení** . Kliknutím na **OK** zavřete stránku **Nastavení** a vraťte se do okna Hyper-V správce.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Krok 3: Zahájení virtuální zařízení a získání adresy IP

Proveďte následující kroky a spusťte virtuální zařízení a připojte k němu.

#### <a name="to-start-the-virtual-device"></a>Spusťte virtuální zařízení

1.  Spusťte virtuální zařízení.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image21.png)

1.  Po spuštění zařízení vyberte zařízení, klikněte pravým tlačítkem myši a vyberte **Připojit**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image22.png)

1.  Se může projevit až 5 až 10 minut zařízení provozu. Stavová zpráva se zobrazí v konzole k určení průběhu. Po zařízení je připravená, přejděte na **Akce**. Stiskněte `Ctrl + Alt + Delete` k přihlášení do virtuálního zařízení. Uživatel výchozí *StorSimpleAdmin* ani výchozí heslo *hesla1*.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image23.png)

1.  Z bezpečnostních důvodů heslo správce zařízení vyprší při prvním přihlášení. Zobrazí se výzva ke změně hesla.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image24.png)

    Zadejte heslo, které obsahuje alespoň na úrovni 8 znaky. Heslo musí splnit nejméně 3 mimo tyto požadavky 4: velká, malá písmena, číselné a speciální znaky. Zadejte znovu heslo pro potvrzení. Zobrazí se oznámení, že se změnilo heslo.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image25.png)

1.  Po úspěšném se změní heslo, může restartovat virtuální zařízení. Počkejte zařízení spusťte.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image26.png)

    Zobrazí se konzoly Windows PowerShell zařízení spolu s indikátor.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image27.png)

1.  Kroky 6 až 8 platí jenom při spouštění v prostředí není DHCP. Pokud jste v prostředí DHCP, přeskočte tento postup a přejděte ke kroku 9. Pokud spuštěn vaše zařízení není DHCP prostředí, zobrazí se na následující obrazovce.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image28m.png)

    Teď je třeba nakonfigurovat síť.

1.  Použití `Get-HcsIpAddress` seznam síťových rozhraní povolena na zařízení s virtuální příkazu. Pokud má vaše zařízení jedné sítě rozhraní povolena, je výchozí název přiřazené k tomuto rozhraní `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image29m.png)

1.  Použití `Set-HcsIpAddress` rutina ke konfiguraci sítě. Příklad je uveden níže:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image30.png)

1.  Po dokončení počáteční instalace a zařízení spustí nahoru, zobrazí se text nápisu zařízení. Poznamenejte si IP adresu a adresu URL zobrazen nápis text ke správě zařízení. Tuto IP adresu bude používat pro připojení k webu uživatelského rozhraní virtuální zařízení a dokončete místního nastavení a registrace.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image31m.png)



1. (Volitelné) Tento krok proveďte jenom v případě, že nasazujete zařízení v cloudu pro státní správu. Teď budou povolit režim Spojené státy informace standardní FIPS (Federal Processing) na vašem zařízení. Standardní FIPS 140 definuje cryptographic algoritmů pro použití schváleno nám Federal government počítačové systémy pro ochranu citlivá data.
    1. Povolit režim FIPS, spusťte následující rutinu:

        `Enter-HcsFIPSMode`

    2. Po povolení režimu FIPS tak, aby cryptographic ověření se projeví, restartujte zařízení.

        > [AZURE.NOTE] Můžete povolit nebo zakázat režim FIPS na vašem zařízení. Alternující zařízení mezi režimem FIPS a jiných FIPS není podporovaná.

Pokud vaše zařízení nesplňuje požadavky na minimální konfigurace, zobrazí se chyba v nápis (viz níže). Bude muset změnit konfigurace zařízení tak, aby měl odpovídající zdroje minimální požadavky. Můžete pak restartujte a připojte zařízení. V nápovědě k požadavky minimální konfigurace [Krok 1: Ověřte, zda systému hostitele splňuje požadavky na minimální virtuální zařízení](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-hyperv/image32.png)

Pokud obličej jiné chyby při počáteční konfiguraci pomocí místní web uživatelského rozhraní v nápovědě k následující pracovní postupy při [používání spravovat své virtuální pole StorSimple místní web uživatelského rozhraní](storsimple-ova-web-ui-admin.md).

-   Spuštění diagnostických testů řešit problémy s [nastavením uživatelské rozhraní webu](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Generovat protokolu zabalit a zobrazení souborů protokolu](storsimple-ova-web-ui-admin.md#generate-a-log-package).

![Ikona video](./media/storsimple-ova-deploy2-provision-hyperv/video_icon.png)  **Video k dispozici**

Podívejte se na video a zjistěte, jak vytvořit virtuální pole StorSimple v Hyper-V.

> [AZURE.VIDEO create-a-storsimple-virtual-array]

## <a name="next-steps"></a>Další kroky

-   [Nastavení vašeho StorSimple virtuální matice jako souborovém serveru](storsimple-ova-deploy3-fs-setup.md)

-   [Nastavení vašeho StorSimple virtuální matice jako iSCSI server](storsimple-ova-deploy3-iscsi-setup.md)
