<properties
   pageTitle="Virtuální pole StorSimple 1 - portálu přípravu nasazení"
   description="První kurz nasazení StorSimple virtuální pole zahrnuje Příprava na portálu"
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
   ms.date="05/24/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---prepare-the-portal"></a>Nasazení StorSimple virtuální maticový – Příprava na portálu

![](./media/storsimple-ova-deploy1-portal-prep/getstarted4.png)

## <a name="overview"></a>Základní informace

Tento článek se týká všeobecně dostupná (GA) verzi pracovního dne 2016 Microsoft Azure StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení nebo StorSimple virtuální zařízení). Toto je první článek v řadě potřebné k úplně nasazení virtuální matice jako souborový server nebo serveru iSCSI výukové programy pro nasazení. Tento článek popisuje přípravu potřebná k vytvoření a konfigurace StorSimple Správce služby před zřizování virtuální pole. Tento článek je také propojená se kontrolní seznam konfigurace nasazení a také konfigurace požadavky.

Budete potřebovat oprávnění správce dokončete proces instalace a konfigurace. Doporučujeme, abyste si prošli kontrolní seznam konfigurace nasazení než začnete. Příprava portálu bude trvat menší než 10 minut.

Informace o publikovaných v tomto článku platí pro nasazení StorSimple virtuální matice v Azure klasické portál, jakož i pro státní správu cloudu společnosti Microsoft Azure.

### <a name="get-started"></a>Začínáme

Pracovní postup nasazení se skládá z Příprava portálu, zřizování virtuální pole ve virtualizovaném prostředí a dokončení instalace. Začít s nasazením StorSimple virtuální matice jako souborový server nebo iSCSI serveru, musíte se naleznete v následující tabulce materiálech (články a videa).

#### <a name="deployment-articles"></a>Nasazení články

Naleznete v následujících článcích v předepsaném pořadí nasazení své StorSimple virtuální pole.

| **#** | **V tomto kroku**                          | **Bude udělejte toto …**                                                         | **Použijte tyto dokumenty.**|
|------|-------------------------------------------|--------------------------------------------------------------------------------|------------------------|
|1.   | **Nastavení portálu Azure klasické**       | Vytváření a konfigurace služby StorSimple správce před zřizování StorSimple virtuální zařízení.  |[Příprava na portálu](storsimple-ova-deploy1-portal-prep.md)|
|2.   | **Zřízení virtuální matice**           | Pro Hyper-V zřízení a připojte k StorSimple virtuální zařízení v systému Host (hostitel) se systémem Hyper-V systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2. <br></br> <br></br> Pro VMware zřízení a připojte k StorSimple místní virtuální zařízení v systému hostitele VMware ESXi 5.5 a vyšší.<br></br>| [Zřízení virtuální matice v Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) <br></br> <br></br> [Zřízení virtuální matice v VMware](storsimple-ova-deploy2-provision-vmware.md)|
|3.    | **Nastavení virtuální matice**              | Pro souborového serveru počáteční instalace, zaregistrovat souborového serveru StorSimple a dokončete tak nastavení na zařízení. Můžete pak vytvořit sdílené položky SMB. <br></br> <br></br> Serveru iSCSI počáteční instalace, registrovat váš server iSCSI StorSimple a dokončete tak nastavení na zařízení. Můžete pak vytvořit iSCSI svazky.| [Nastavení virtuální matice jako souborovém serveru](storsimple-ova-deploy3-fs-setup.md)<br></br> <br></br>[Nastavení virtuální matice jako iSCSI serveru](storsimple-ova-deploy3-iscsi-setup.md)|

#### <a name="deployment-videos"></a>Nasazení videa

| **Tento krok...** |  **Podívejte se na toto video.**|
|----------------|-------------|
| Podrobné pokyny začít pracovat s polem virtuální StorSimple. | [Začínáme s polem virtuální StorSimple](https://azure.microsoft.com/documentation/videos/get-started-with-the-storsimple-virtual-array/)|
| Podrobné pokyny k poskytnutí StorSimple virtuální matice v Hyper-V.|[Vytvořte pole virtuální StorSimple](https://azure.microsoft.com/documentation/videos/create-a-storsimple-virtual-array/) |
|Podrobné pokyny ke konfiguraci a zaregistrovat StorSimple virtuální matice|[Konfigurace StorSimple virtuální matice](https://azure.microsoft.com/documentation/videos/configure-a-storsimple-virtual-array/)|
|Podrobné pokyny k vytvoření sdílené složky, obecnějším údajům sdílené položky a obnovte data na StorSimple virtuální pole nakonfigurování jako souborovém serveru|[Použití maticových virtuální StorSimple](https://azure.microsoft.com/documentation/videos/use-the-storsimple-virtual-array/)|
|Podrobné pokyny k převzetí a havárie obnovení StorSimple virtuální pole|[Obnovení havárie StorSimple virtuální matice](https://azure.microsoft.com/documentation/videos/storsimple-virtual-array-disaster-recovery/)

Teď můžete začít nastavovat Azure klasické portálu.

## <a name="configuration-checklist"></a>Kontrolní seznam konfigurace

Kontrolní seznam konfigurace popisuje informace, které je potřeba shromáždit před nakonfigurovat software pro v zařízení s StorSimple. Příprava tyto informace předem vám pomůže zjednodušit proces nasazení zařízení StorSimple ve vašem prostředí. V závislosti na tom, zda má být nasazené virtuální zařízení StorSimple jako souborový server nebo iSCSI serveru, budete potřebovat jednu z následujících seznamy úkolů.

-   Stáhněte si [StorSimple kontrolní seznam konfigurace virtuální pole soubor](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).

-   Stáhněte si [virtuální pole StorSimple iSCSI kontrolní seznam konfigurace serveru](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Tady zjistíte konfigurace požadavcích na StorSimple Správce služby, virtuální zařízení StorSimple a síťové datacentra.

### <a name="for-the-storsimple-manager-service"></a>Pro službu StorSimple správce

Než začnete, ujistěte se, že:

-   Máte účet Microsoft pomocí přihlašovacích údajů aplikace access.

-   Máte účet Microsoft Azure úložiště pomocí přihlašovacích údajů aplikace access.

-   Předplatného Microsoft Azure by měl být užitečné pro StorSimple Správce služby.

### <a name="for-the-storsimple-virtual-device"></a>Virtuální zařízení StorSimple

Před nasazením virtuální zařízení, ujistěte se, že:

-   Máte přístup k Hyper-V systému Windows Server 2008 R2 nebo novější se systémem host nebo VMware (ESXi 5,5 nebo novější), kterou lze použít k ustanovení zařízení.

-   Systém hostitele je možné zajistíte v následujících zdrojích zřízení virtuální zařízení:

    -   Minimální 4 jádra.

    -   Alespoň na úrovni 8 GB paměti RAM.

    -   Rozhraní jedné sítě.

    -   Virtuální disk 500 GB pro data systému.

### <a name="for-the-datacenter-network"></a>U datacentra sítě

Než začnete, ujistěte se, že:

-   Podle požadavky na sociální sítě pro zařízení s StorSimple nakonfigurování sítě ve vaší datacentra. Další informace najdete v tématu [StorSimple virtuální pole systémové požadavky](storsimple-ova-system-requirements.md).

-   Virtuální zařízení StorSimple má vyhrazené 5 šířky pásma Internetu MB / () vždy k dispozici. Tento šířky pásma nesmí sdílet s jinými aplikacemi.

## <a name="step-by-step-preparation"></a>Podrobné Příprava

Použijte následující podrobné pokyny k přípravě portálu pro službu StorSimple správce.

## <a name="step-1-create-a-new-service"></a>Krok 1: Vytvoření nové služby

Jedna instance službu StorSimple správce můžete spravovat více StorSimple 1200 zařízení. Proveďte následující postup vytvoření nové instance službu StorSimple správce. Pokud máte existující službu StorSimple správce pro správu zařízení 1200, tento krok přeskočit a přejít na [Krok 2: získání klíč registrace služby](#step-2-get-the-service-registration-key).

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

> [AZURE.IMPORTANT]
>
> Pokud Nepovolili jste automatické vytváření účtu úložiště s vašimi službami, bude potřeba vytvořit alespoň jeden účet úložiště po úspěšném vytvoření služby.
>

> - Pokud jste úložiště účet nevytvořili automaticky, přejděte na [Konfigurovat nového účtu úložiště služby](#optional-step-configure-a-new-storage-account-for-the-service) podrobné pokyny.
>

> - Pokud jste povolili automatické vytváření účtu úložiště, přejděte na [Krok 2: získání klíč registrace služby](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Krok 2: Získání klíč registrace služby


Po službu StorSimple správce do začátků, musíte získat klíč registrace služby. Tento klíč slouží k registraci a připojte zařízení StorSimple se službou.

Proveďte následující kroky [Azure klasické portálu](https://manage.windowsazure.com/).


[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

> [AZURE.NOTE]
>
> Služba registrace používá se zaregistrovat všechna StorSimple Správce zařízení, které je potřeba se zaregistrovat s vašimi službami StorSimple správce.

## <a name="step-3-download-the-virtual-device-image"></a>Krok 3: Stažení obrázek virtuální zařízení

Po registraci klíč služby bude muset stáhnout obrázek příslušné virtuální zařízení zřízení virtuální zařízení v počítači Host (hostitel). Na některých obrázcích virtuální zařízení jsou specifické pro operační systém a si můžete stáhnout z úvodní stránky na portálu Azure klasické.

> [AZURE.IMPORTANT] Software pro pole virtuální StorSimple může použít pouze ve spojení se službou Storsimple správce.


Proveďte následující kroky [Azure klasické portálu](https://manage.windowsazure.com/).

#### <a name="to-get-the-virtual-device-image"></a>Chcete-li získat obrázek virtuální zařízení

1.  Na stránce **Správce StorSimple služby** klikněte na službu, kterou jste vytvořili. Tím přejdete na **Úvodní** stránku. (Můžete kliknout na ikonu úvodní ![](./media/storsimple-ova-deploy1-portal-prep/image8.png) pro přístup na **Úvodní** stránku kdykoli.)

1.  Klikněte na odkaz odpovídající obrázek, který chcete stáhnout z Microsoft Download Center. V souborech obrázků jsou přibližně 4,8 GB.

    -   VHDX pro Hyper-V v systému Windows Server 2012 a v novějších verzích

    -   Virtuální pevný disk pro Hyper-V systému Windows Server 2008 R2 nebo novější

    -   VMDK pro VMWare ESXi 5.5 a v novějších verzích

2.  Stáhněte si a rozbalte soubor na místní jednotku, díky poznámky ze kdy rozbaleny soubor nachází.

![Ikona video](./media/storsimple-ova-deploy1-portal-prep/video_icon.png) **Video k dispozici**

Podívejte se na video podrobné pokyny začít pracovat s polem virtuální StorSimple.

> [AZURE.VIDEO get-started-with-the-storsimple-virtual-array]



## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Volitelný krok: Konfigurace nového účtu úložiště služby

Tento krok je volitelný, kterou je potřeba provést jenom v případě, že Nepovolili jste automatické vytváření účtu úložiště s vašimi službami.

Pokud je potřeba vytvořit účet Azure úložiště v jiné oblasti, naleznete v článku [Vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account) podrobné pokyny.

Proveďte následující kroky v [Azure klasické portál](https://manage.windowsazure.com/) na stránce Správce StorSimple služby a přidejte stávající účet Microsoft Azure úložiště.

#### <a name="to-add-a-storage-account"></a>Přidání účtu úložiště

1.  Na službu StorSimple správce úvodní stránka vyberte služby a poklikejte na něj. Tím přejdete na **Úvodní** stránku. Vyberte stránku **Konfigurovat** .

2.  Klikněte na **Přidat/upravit úložiště účet**. V dialogovém okně **Přidat/upravit úložiště účet** postupujte takto:

    1.  Klikněte na tlačítko **Přidat nové**.

    1.  Zadejte název účtu úložiště.

    1.  Poskytovat primární **Přístupová klávesa** pro váš účet Microsoft Azure úložiště.

    1.  Vyberte **Povolit SSL režim** pro vytvoření zabezpečeného kanálu pro síťová komunikace mezi zařízení a cloudem. Zrušte zaškrtnutí políčka **Povolit SSL režim** jenom v případě, že pracujete v rámci soukromé cloudu.

    1.  Klikněte na ikonu kontrola ![](./media/storsimple-ova-deploy1-portal-prep/image7.png). Zobrazí se oznámení úspěšně vytvořený účet úložiště.

        ![](./media/storsimple-ova-deploy1-portal-prep/image11.png)

1.  Nově vytvořený úložiště účtu se zobrazí na stránce **Konfigurovat** ve skupinovém rámečku **úložiště účty**. Klepněte na tlačítko **Uložit** uložte úložiště nově vytvořený účet. Klikněte na **OK** po zobrazení výzvy k potvrzení.


## <a name="next-step"></a>Další krok

Dalším krokem je zřízení virtuálního počítače pro zařízení s virtuální StorSimple. V závislosti na operačním systému hostitele podrobné pokyny naleznete v:

-   [Zřízení StorSimple virtuální matice v Hyper-V](storsimple-ova-deploy2-provision-hyperv.md)

-   [Zřízení StorSimple virtuální matice v VMware](storsimple-ova-deploy2-provision-vmware.md)
