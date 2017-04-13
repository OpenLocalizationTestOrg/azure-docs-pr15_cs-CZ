<properties
   pageTitle="Nasazení StorSimple virtuální maticový – ustanovení VMware"
   description="Tento druhý kurz v řadě nasazení virtuální pole StorSimple zahrnuje zřizování virtuální zařízení v VMware."
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
   ms.date="04/12/2016"
   ms.author="alkohli"/>


# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-vmware"></a>Nasazení StorSimple virtuální maticový – zřízení virtuální matice v VMware

![](./media/storsimple-ova-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Základní informace 
Tento kurz zřizovací platí všeobecně dostupná (GA) verzi pracovního dne 2016 StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení nebo StorSimple virtuální zařízení). Tento kurz popisuje zřízení a připojte k StorSimple virtuální polem na hostitele systému VMware ESXi 5.5 a nad. Tento článek se týká nasazení StorSimple virtuální matice v Azure klasické portál, jakož i cloudu společnosti Microsoft Azure pro státní správu.

Bude mít oprávnění správce trvat a připojte k virtuální zařízení. Nastavení zřizování a počáteční může trvat zhruba 10 minut dokončete.


## <a name="provisioning-prerequisites"></a>Zřízení požadavky

Tady zjistíte požadavky zřízení virtuální zařízení v systému hostitele VMware ESXi 5.5 a vyšší.

### <a name="for-the-storsimple-manager-service"></a>Pro službu StorSimple správce

Než začnete, ujistěte se, že:

-   Dokončení všech kroků v tématu [Příprava na portálu pro StorSimple virtuální pole](storsimple-ova-deploy1-portal-prep.md).

-   Obrázek virtuální zařízení pro VMware jste stáhli z portálu Microsoft Azure. Další informace najdete v tématu [Krok 3: Stáhněte si obrázek virtuální zařízení](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

### <a name="for-the-storsimple-virtual-device"></a>Virtuální zařízení StorSimple 

Před nasazením virtuální zařízení, ujistěte se, že:

-   Máte přístup k systému hostitele Hyper-V (2008 R2 nebo novější), které lze použít na základě zařízení.

-   Systém hostitele je možné zajistíte v následujících zdrojích zřízení virtuální zařízení:

    -   Minimální 4 jádra.

    -   Alespoň na úrovni 8 GB paměti RAM.

    -   Rozhraní jedné sítě.

    -   Virtuální disk 500 GB pro data systému.

### <a name="for-the-network-in-datacenter"></a>Pro síť ve datacentru 

Než začnete, ujistěte se, že:

-   Kontrole síťové požadavky pro nasazení virtuální zařízení StorSimple a datacentra sítě podle požadavky. Další informace najdete v tématu [Systémové požadavky StorSimple virtuální pole](storsimple-ova-system-requirements.md).

## <a name="step-by-step-provisioning"></a>Podrobné zřízení 

Zřízení a připojte k virtuální zařízení, musíte provést následující postup:

1.  Zajistěte, aby měl systému hostitele dostatečné zdroje požadavkům minimální virtuální zařízení.

2.  Zřízení virtuální zařízení ve vaší hypervisoru.

3.  Zahájení virtuální zařízení a najděte IP adresu.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Krok 1: Zajistěte, aby hostitelského systému splňuje požadavky na minimální virtuální zařízení

Pokud chcete vytvořit virtuální zařízení, budete potřebovat:

-   Přístup k systému host spuštěný VMware ESXi Server 5.5 a nad.

-   VMware vSphere klient v počítači ke správě hostiteli ESXi.

    -   Minimální 4 jádra.

    -   Alespoň na úrovni 8 GB paměti RAM.

    -   Jedné sítě rozhraní připojení k síti může směrování přenosu k Internetu. Minimální šířka internetového pásma by měl být MB 5 / umožňující optimální pracovní zařízení.

    -   Virtuální disk 500 GB pro data.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Krok 2: Zřízení virtuální zařízení v hypervisoru

Proveďte následující kroky zřízení virtuální zařízení ve vaší hypervisoru.

1.  Zkopírujte obrázek virtuální zařízení v počítači. Toto je obrázek, který jste stáhli přes Azure klasické portál. 
    1.  Ujistěte se, že se jedná o nejnovější soubor obrázku, který jste stáhli. Pokud jste dříve stáhli obrázku, stáhněte si ho znovu a zkontrolujte, že máte nejnovější obrázek. Nejnovější obrázek obsahuje dva soubory (místo jedné).
    2.  Poznamenejte si na místo, kam jste zkopírovali obrázek jako budete používat to později v procesu.

2.  Přihlaste se k serveru ESXi pomocí klienta vSphere. Musíte mít oprávnění správce k vytvoření virtuálního počítače.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image1.png)

1.  V klientovi vSphere v části zásob v levém podokně vyberte ESXi Server.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image2.png)

1.  Bude nejdřív nahrajete VMDK ESXi server. Přejděte na kartu **Konfigurace** v pravém podokně. V části **Hardware**vyberte **úložiště**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image3.png)

1.  V pravém podokně v části **Datastores**vyberte místo, kam chcete nahrát VMDK úložiště. Úložišti dat může mít dostatek místa pro disků OS a data.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image4.png)

1.  Klikněte pravým tlačítkem myši a vyberte **Procházet úložiště**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image5.png)

1.  Zobrazí se okno **Prohlížeče úložiště** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image6.png)

1.  Na panelu nástrojů klikněte na ![](./media/storsimple-ova-deploy2-provision-vmware/image7.png) na ikonu Vytvořit novou složku. Zadejte název složky a poznamenejte si ji. Použijete tento název složky později při vytváření virtuálních počítačů (doporučeno doporučený postup). Klikněte na **OK**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image8.png)

1.  Nová složka se zobrazí v levém podokně **Úložiště prohlížeče**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image9.png)

1.  Klikněte na ikonu Upload ![](./media/storsimple-ova-deploy2-provision-vmware/image10.png) a vyberte **Nahrát soubor**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image11.png)

1.  Teď by měl procházet a přejděte na VMDK soubory, které jste si stáhli. Nastane dvou souborů. Vyberte soubor, který chcete nahrát.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image12m.png)

1.  Klikněte na **Otevřít**. Teď bude vytvořena nahrávání souboru VMDK zadaný úložiště. Může trvat několik minut, než soubor nahrát.


1.  Po dokončení nahrávání se zobrazí souboru v úložišti dat ve složce, kterou jste vytvořili. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image14.png)

    Teď je třeba odeslání druhý VMDK souboru do stejné úložiště.

1.  Vraťte se do okna vSphere klienta. Serverem ESXi vybrané klikněte pravým tlačítkem myši a vyberte **Nový virtuální počítač**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image15.png)

1.  Zobrazí se okno **vytvořit nový virtuální počítač** . Na stránce **Konfigurace** vyberte možnost **vlastní** . Klikněte na tlačítko **Další**.
    ![](./media/storsimple-ova-deploy2-provision-vmware/image16.png)

2.  Na stránce **název a umístění** zadejte název virtuálního počítače. Tento název by měly odpovídat název složky (doporučené doporučený postup), které jste zadali v kroku 8.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image17.png)

1.  Na stránce **úložiště** vyberte úložiště, který chcete použít zřízení vaší OM.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image18.png)

1.  Na stránce **Virtuálního počítače verze** vyberte **virtuálního počítače verze: 8**. Všimněte si, že verze 8 až 11 podporují.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image19.png)

1.  Na stránce **hostovaný operační systém** vyberte **hostovaný operační systém** jako **Windows**. **Verze**v rozevíracím seznamu vyberte **Microsoft Windows Server 2012 (64bitová verze)**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image20.png)

1.  Na stránce **procesorů** upravte **počet virtuální sockets** a **počet jádra za virtuální socket** tak, aby **Celkový počet jádra** 4 (nebo další). Klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image21.png)

1.  Na stránce **paměti** zadejte 8 GB (a víc) RAM. Klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image22.png)

1.  Na stránce **sítě** zadejte počet rozhraní sítě. Minimální požadavky je jeden rozhraní sítě.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image23.png)

1.  Na stránce **Řadiče SCSI** přijměte výchozí **přidružení LSI logiky zabezpečení zařízení**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image24.png)

1.  Na stránce **Vyberte Disk,** vyberte **použít existující virtuální disk**. Klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image25.png)

1.  Na stránce **Vybrat existující Disk** **Cesta k souboru disku**, klikněte na **Procházet**. Otevře se dialogové okno **Procházet Datastores** . Přejděte do umístění, které jste nahráli VMDK. Zobrazí pouze jeden soubor v úložišti dat jako dva soubory, které jste původně nahráli sloučená. Vyberte soubor a klikněte na **OK**. Klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image26.png)

1.  Na stránce **Upřesnit možnosti** přijmout výchozí a klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image27.png)

1.  Na stránce **chtít dokončeno** zkontrolujte všechna nastavení přidružené nového virtuálního zařízení. Zkontrolujte **nastavení virtuální počítač před dokončením**. Klikněte na **pokračovat**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image28.png)

1.  Na stránce **Virtuálních počítačích vlastnosti** na kartě **Hardware** vyhledejte hardware zařízení. Vyberte **nový pevný Disk**. Klikněte na **Přidat**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image29.png)

1.  Tím se vyvolá okně **Přidat Hardware** . Na stránce **Typ zařízení** ve skupinovém rámečku **Vybrat typ zařízení, které chcete přidat**vyberte **jednotku pevného disku** a klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image30.png)

1.  Na stránce **Vyberte Disk** vyberte **vytvořit nový virtuální disk**. Klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image31.png)

1.  Na stránce **vytvořit Disk** změňte **Velikost disku** a 500 GB (Další). V části **Vytváření disku**vyberte **Tenké poskytování**. Klikněte na tlačítko **Další**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image32.png)

1.  Na stránce **Upřesnit možnosti** přijmete výchozí.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image33.png)

1.  Na stránce **chtít dokončeno** zkontrolujte možnosti disku. Klikněte na **Dokončit**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image34.png)

1.  Teď se vrátíte na stránku vlastnosti virtuálního počítače. Nový pevný disk je přidán do virtuálního počítače. Klikněte na **Dokončit**.
  
    ![](./media/storsimple-ova-deploy2-provision-vmware/image35.png)

2.  S virtuálního počítače vybrané v pravém podokně přejděte na kartu **Souhrn** . Zkontrolujte nastavení virtuálního počítače.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image36.png)

Nyní máte k dispozici virtuálního počítače. Dalším krokem je power v tomto počítači a získat IP adresu.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Krok 3: Zahájení virtuální zařízení a získání adresy IP

Proveďte následující kroky a spusťte virtuální zařízení a připojte k němu.

#### <a name="to-start-the-virtual-device"></a>Spusťte virtuální zařízení

1.  Spusťte virtuální zařízení. V vSphere Správce konfigurace v levém podokně vyberte své zařízení a klikněte pravým tlačítkem myši vyvoláte místní nabídky. Vyberte **Power** a pak vyberte **zapněte**. Power by ve vašem počítači virtuální. Zobrazení stavu v dolním podokně **Poslední úkoly** vSphere klienta.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image37.png)

1.  Úlohy nastavení bude trvat několik minut. Jakmile zařízení pracuje, přejděte na kartu **konzoly** . Odeslání kombinaci kláves Ctrl + Alt + Delete přihlásit do zařízení. Můžete taky můžete přejděte kurzorem v okně konzoly a stiskněte kombinaci kláves Ctrl + Alt + Insert. Uživatel výchozí *StorSimpleAdmin* ani výchozí heslo *hesla1*.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image38.png)

1.  Z bezpečnostních důvodů heslo správce zařízení vyprší při prvním přihlášení. Zobrazí se výzva ke změně hesla.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image39.png)

1.  Zadejte heslo, které obsahuje alespoň na úrovni 8 znaky. Heslo musí obsahovat 3 ze 4 tyto požadavky: velká, malá písmena, číselné a speciální znaky. Zadejte znovu heslo pro potvrzení. Zobrazí se oznámení, že se změnilo heslo.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image40.png)

1.  Po úspěšném se změní heslo, může restartujte virtuální zařízení. Počkejte, restartujte počítač dokončete. Konzoly Windows PowerShell zařízení se může zobrazovat spolu s indikátor.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image41.png)

1.  Kroky 6 až 8 platí jenom při spouštění v prostředí není DHCP. Pokud jste v prostředí DHCP, přeskočte tento postup a přejděte ke kroku 9. Pokud spuštěn vaše zařízení není DHCP prostředí, zobrazí se na následující obrazovce. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image42m.png)

    Teď je třeba nakonfigurovat síť.

1.  Použití `Get-HcsIpAddress` seznam síťových rozhraní povolena na zařízení s virtuální příkazu. Pokud má vaše zařízení jedné sítě rozhraní povolena, je výchozí název přiřazené k tomuto rozhraní `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image43m.png)

1.  Použití `Set-HcsIpAddress` rutina ke konfiguraci sítě. Příklad je uveden níže:


    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-vmware/image44.png)

1.  Po dokončení počáteční instalace a zařízení spustí nahoru, zobrazí se text nápisu zařízení. Poznamenejte si IP adresu a adresu URL zobrazen nápis text ke správě zařízení. Tuto IP adresu bude používat pro připojení k webu uživatelského rozhraní virtuální zařízení a dokončete místního nastavení a registrace.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image45.png)


1. (Volitelné) Tento krok proveďte jenom v případě, že nasazujete zařízení v cloudu pro státní správu. Teď budou povolit režim Spojené státy informace standardní FIPS (Federal Processing) na vašem zařízení. Standardní FIPS 140 definuje cryptographic algoritmů pro použití schváleno nám Federal government počítačové systémy pro ochranu citlivá data.
    1. Povolit režim FIPS, spusťte následující rutinu:
        
        `Enter-HcsFIPSMode`

    2. Po povolení režimu FIPS tak, aby cryptographic ověření se projeví, restartujte zařízení.

        > [AZURE.NOTE] Můžete povolit nebo zakázat režim FIPS na vašem zařízení. Alternující zařízení mezi režimem FIPS a jiných FIPS není podporovaná.


Pokud vaše zařízení nesplňuje požadavky na minimální konfigurace, zobrazí se chyba v nápis (viz níže). Bude muset změnit konfigurace zařízení tak, aby měl odpovídající zdroje minimální požadavky. Můžete pak restartujte a připojte zařízení. V nápovědě k požadavky minimální konfigurace [Krok 1: Ověřte, zda systému hostitele splňuje požadavky na minimální virtuální zařízení](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-vmware/image46.png)

Pokud obličej jiné chyby při počáteční konfiguraci pomocí místní web uživatelského rozhraní v nápovědě k následující pracovní postupy při [používání spravovat své virtuální pole StorSimple místní web uživatelského rozhraní](storsimple-ova-web-ui-admin.md).

-   Spuštění diagnostických testů řešit problémy s [nastavením uživatelské rozhraní webu](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Generovat protokolu zabalit a zobrazení souborů protokolu](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Další kroky

-   [Nastavení vašeho StorSimple virtuální matice jako souborovém serveru](storsimple-ova-deploy3-fs-setup.md)

-   [Nastavení vašeho StorSimple virtuální matice jako iSCSI server](storsimple-ova-deploy3-iscsi-setup.md)

