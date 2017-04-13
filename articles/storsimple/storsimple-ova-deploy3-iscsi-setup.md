<properties 
   pageTitle="Instalační program serveru iSCSI virtuální pole StorSimple | Microsoft Azure"
   description="Popisuje, jak provádět počátečním nastavení, registrovat váš server iSCSI StorSimple a dokončete nastavení zařízení."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/18/2016"
   ms.author="alkohli" />


# <a name="deploy-storsimple-virtual-array--set-up-your-virtual-device-as-an-iscsi-server"></a>Nasazení StorSimple virtuální maticový – nastavení virtuální zařízení jako iSCSI server

![tok procesu instalace iSCSI](./media/storsimple-ova-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Základní informace

Tento kurz nasazení platí všeobecně dostupná (GA) verzi pracovního dne 2016 Microsoft Azure StorSimple virtuální pole (označovaná taky jako virtuální zařízení místní StorSimple nebo StorSimple virtuální zařízení). Tento kurz popisuje, jak počáteční instalace registrovat váš server iSCSI StorSimple, dokončit nastavení zařízení a pak vytvořit, připojit, inicializace a formátovat svazky na serveru iSCSI StorSimple virtuální zařízení. Informace o nastavení StorSimple v tomto článku platí pouze pro StorSimple virtuální matice. 

Postupy popsané tady trvat zhruba 30 minut na 1 hodinu dokončete. Informace o publikovaných v tomto článku platí pouze pro StorSimple virtuální matice.

## <a name="setup-prerequisites"></a>Zjistit předpoklady pro instalaci

Konfigurace a nastavení zařízení virtuální StorSimple, ujistěte se, že:

- Máte zřízení virtuální zařízení a připojení k němu podle popisu v [Nasazení StorSimple virtuální matice - poskytování virtuální matice v Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) nebo [Nasadit StorSimple virtuální maticový – poskytování virtuální matice v VMware](storsimple-ova-deploy2-provision-vmware.md).

- Máte klíč registrace služby z službu StorSimple správce, který jste vytvořili StorSimple virtuální zařízení můžete spravovat. Další informace najdete v tématu **Krok 2: získání klíč registrace služby** [nasazení StorSimple virtuální](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key)matice - připravit na portálu.

- Pokud toto je druhý nebo další virtuální zařízení, které jsou zaregistrovali stávající služba StorSimple správce, měli byste mít šifrovací klíč dat služby. Tento klíč vygenerované při prvním zařízení byla úspěšně registrovaný u této služby. Pokud jste ztratili tento klíč, najdete v článku [použití uživatelské rozhraní webu spravovat své StorSimple virtuální pole](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) **získat šifrovací klíč dat služby** .

## <a name="step-by-step-setup"></a>Podrobné nastavení 

Použijte následující podrobné pokyny k nastavení a konfiguraci StorSimple virtuální zařízení:

-  [Krok 1: Dokončit nastavení uživatelského rozhraní místní web a zaregistrovat zařízení](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
-  [Krok 2: Dokončit nastavení požadovaných zařízení](#step-2-complete-the-required-device-setup)
-  [Krok 3: Přidání svazku](#step-3-add-a-volume)
-  [Krok 4: Připojit, inicializace a formátování svazku](#step-4-mount-initialize-and-format-a-volume)  

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Krok 1: Dokončit nastavení uživatelského rozhraní místní web a zaregistrovat zařízení 

#### <a name="to-complete-the-setup-and-register-the-device"></a>Dokončete nastavení a zaregistrovat zařízení

1. Otevřete okno prohlížeče a připojení k webu rozhraní zadáním:

    `https://<ip-address of network interface>`

    Použití adresy URL připojení uvedeno v předchozím kroku. Zobrazí se chyba oznamující, že došlo k potížím s certifikátem zabezpečení na webu. Kliknutím na **pokračovat tuto webovou stránku**.

    ![Chyba certifikátu zabezpečení](./media/storsimple-ova-deploy3-iscsi-setup/image3.png)

2. Přihlaste se k webu rozhraní virtuální zařízení jako **StorSimpleAdmin**. Zadejte heslo správce zařízení, které jste změnili v kroku 3: zahájení virtuální zařízení v [Nasazení StorSimple virtuální matice - poskytování virtuální zařízení v Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) nebo [Nasadit StorSimple virtuální maticový – poskytování virtuální zařízení v VMware](storsimple-ova-deploy2-provision-vmware.md).

    ![Přihlašovací stránka](./media/storsimple-ova-deploy3-iscsi-setup/image4.png)

3. Přejdete na **domovské** stránce. Tato stránka popisuje různé nastavení pro nakonfigurovat a zaregistrovat virtuální zařízení se službou StorSimple správce. Všimněte si, že jsou volitelné **sítě, nastavení**, **Nastavení webového proxy serveru**a **Nastavení času** . Pouze požadovaná nastavení jsou **Nastavení zařízení** a **Nastavení cloudu**.

    ![Domovská stránka](./media/storsimple-ova-deploy3-iscsi-setup/image5.png)

4. Na stránce **Nastavení sítě** klikněte v části **síť rozhraní**DATA 0 automaticky nakonfigurujete za vás. Každý rozhraní sítě je ve výchozím nastavení automaticky získat IP adresu (DHCP). Proto IP adresu podsítě a brány bude automaticky přiřazeno (pro IPv4 a IPv6).

    Jak plánujete nasazení zařízení jako iSCSI serveru (zřízení úložiště bloků), doporučujeme vypnout možnost **získat IP adresu automaticky** a konfigurace statické IP adres.

    ![Stránka nastavení sítě](./media/storsimple-ova-deploy3-iscsi-setup/image6.png)

    Pokud jste přidali více rozhraní sítě během vytváření zařízení, můžete je zde nakonfigurovat. Poznámka: síťové rozhraní můžete nakonfigurovat jako IPv4 nebo jen IPv4 a IPv6. Konfigurace pouze protokolu IPv6 nepodporuje.

5. Servery DNS jsou potřeba, protože se používají při zařízení pokusí komunikovat s zprostředkovatelů vaše cloudové úložiště nebo zařízení vyřešit název případě nakonfigurování jako souborovém serveru. Na stránce **Nastavení sítě** ve skupinovém rámečku **servery DNS**:

    1. Primární a sekundární server DNS bude automaticky nakonfigurované. Pokud se rozhodnete sdělit konfigurace statické IP adresy, můžete určit serverů DNS. Dostupnost doporučujeme konfigurace primární a sekundární server DNS.

    2. Klikněte na tlačítko **použít**. Použije a ověřte nastavení sítě.

6. Na stránce **Nastavení zařízení** :

    1. Přiřadíte jedinečný **název** do zařízení. Tento název může být znaky 1-15 a může obsahovat písmeno, čísel a spojovníky.

    2. Klikněte na ikonu **serveru iSCSI** ![ikona serveru iSCSI](./media/storsimple-ova-deploy3-iscsi-setup/image7.png) **Typ** zařízení, které vytvoříte. ISCSI server vám umožní poskytování blok úložiště.

    3. Určete, jestli se má toto zařízení doméně. Pokud vaše zařízení není iSCSI server, připojení k doméně je nepovinný krok. Pokud se rozhodnete připojit iSCSI serveru pro doménu, klikněte na **použít**, počkejte nastavení se použije a potom přejděte k dalšímu kroku.

        Pokud se chcete připojit zařízení na doménu. Zadejte **název domény**a potom klikněte na **použít**.

        > [AZURE.NOTE] Pokud se připojujete iSCSI serveru pro doménu, zajistěte virtuální pole je v své vlastní organizační jednotce (OU) pro Microsoft Azure Active Directory a bez objekty zásad skupiny (objekt zásad skupiny), použijí se k němu.

    5. Zobrazí se dialogové okno. Zadejte svoje přihlašovací údaje domény zadaného formátu. Klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Bude ověření domény pověření. Zobrazí se chybová zpráva a pokud nejsou správné přihlašovací údaje.

        ![přihlašovací údaje](./media/storsimple-ova-deploy3-iscsi-setup/image8.png)

    6. Klikněte na tlačítko **použít**. Použije a ověřte nastavení zařízení.
 
7. (Volitelně) konfigurace proxy serveru webové. Konfigurace proxy serveru webové je sice volitelný, mějte na paměti, že používáte proxy serveru webové můžete jenom nakonfigurovat ho tady.

    ![konfigurace proxy serveru webové](./media/storsimple-ova-deploy3-iscsi-setup/image9.png)

    Na **proxy serveru webové** stránky:

    1. Zadejte **adresu URL webové proxy serveru** v tomto formátu: *http://host-IP adresu* nebo *číslo FDQN:Port*. Všimněte si, že nejsou podporované adresy URL HTTPS.

    2. Zadejte **ověřovací** jako **základní** nebo **žádný**.

    3. Pokud používáte ověřování, bude taky muset zadal **uživatelské jméno** a **heslo**.

    4. Klikněte na tlačítko **použít**. To bude ověřit a použít nastavení nakonfigurované webového proxy serveru.
 
8. Zařízení, třeba časové pásmo a servery NTP primárních a sekundárních (volitelně) v nastavení času. Protože zařízení, musíte k synchronizaci čas tak, aby může ověřovat pomocí poskytovatele služby cloudu jsou vyžadovány NTP servery.

    ![Nastavení času](./media/storsimple-ova-deploy3-iscsi-setup/image10.png)

    Na stránce **Nastavení času** :

    1. V rozevíracím seznamu vyberte **časové pásmo** podle zeměpisné polohy, ve kterém je nasazený zařízení. Výchozí časové pásmo pro zařízení je PST. Zařízení s použije časového pásma pro všechny plánované operace.

    2. Určení **primární NTP serveru** pro zařízení nebo ponechte výchozí hodnotu time.windows.com. Ujistěte se, že síti umožňuje NTP předání přenosu z vaší datacentra na Internetu.

    3. Volitelně zadejte **sekundární NTP serveru** pro svoje zařízení.

    4. Klikněte na tlačítko **použít**. Bude ověření a nastavení nakonfigurované čas.

9. Konfigurace nastavení cloudu pro svoje zařízení. V tomto kroku dokončete konfiguraci místního zařízení a pak zaregistrovat zařízení s vašimi službami StorSimple správce.

    1. Zadání **klíč registrace služby** , kterou jste získali v **Krok 2: získání klíč registrace služby** [nasazení StorSimple virtuální](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key)matice - připravit na portálu.

    2. Pokud to není prvním zařízení, které jste se zaregistrovali tuto službu, musíte se k poskytování **služby dat šifrovacího klíče**. Tento klíč se musí používat klávesu služby registrace k registraci další zařízení se službou StorSimple správce. Další informace najdete v příručce [získat šifrovací klíč dat služby](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) na svůj místní web uživatelského rozhraní.

    3. Klikněte na **zaregistrovat**. To bude Restartujte zařízení. Budete muset počkat 2 – 3 minuty předtím, než úspěšně registrována zařízení. Po restartování zařízení přejdete na přihlašovací stránku.

       ![Registrace zařízení](./media/storsimple-ova-deploy3-iscsi-setup/image11.png)

10. Vraťte se do portálu Azure klasické. Na stránce **zařízení** ověřte, že zařízení úspěšně se připojil ke službě vyhledáním stav. Stav zařízení by měl být **aktivní**.

    ![Stránka zařízení](./media/storsimple-ova-deploy3-iscsi-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Krok 2: Dokončit nastavení požadovaných zařízení

Dokončete konfiguraci zařízení StorSimple zařízení, musíte:

- Vyberte účet úložiště chcete přidružit k zařízení.

- Zvolte nastavení šifrování u data, která se neodesílají do cloudu.

Proveďte následující kroky v portálu Azure klasické dokončit nastavení požadované zařízení.

#### <a name="to-complete-the-minimum-device-setup"></a>Dokončete nastavení minimální zařízení

1. Na stránce **zařízení** vyberte zařízení, které jste právě vytvořili. Toto zařízení chcete zobrazit jako **aktivní**. Klikněte na šipku vedle názvu zařízení a pak klikněte na **Rychlý Start**.

    ![Stránka zařízení](./media/storsimple-ova-deploy3-iscsi-setup/image13.png)

2. Klikněte na příkaz **Nastavení dokončení zařízení** spusťte Průvodce konfigurací zařízení.

    ![Konfigurace Průvodce zařízení](./media/storsimple-ova-deploy3-iscsi-setup/image14.png)

3. V Průvodci konfigurací zařízení na stránce **Základní nastavení** postupujte takto:

   1. Zadejte účet úložiště se nemusí používat se svým zařízením. V toto předplatné existujícího účtu úložiště můžete vybrat z rozevíracího seznamu nebo můžete **Přidat další** účet vyberte z jiné předplatné.

   2. Definování nastavení šifrování pro všechna data na ostatních odeslání do cloudu. (StorSimple používá šifrování AES 256.) Šifrování dat, zaškrtněte políčko **Povolit cloudové úložiště šifrování** . Zadejte šifrování cloudové úložiště, která obsahuje 32 znaků. Klíč pro potvrzení znovu.

   3. Klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-ova-deploy3-iscsi-setup/image15.png).

    ![Základní nastavení](./media/storsimple-ova-deploy3-iscsi-setup/image16.png)

    Nastavení bude teď aktualizují. Po úspěšném aktualizaci nastavení na tlačítko Nastavit dokončení zařízení přestane být dostupná. Se vrátíte na stránku **Rychlý Start** zařízení.                                                        

>[AZURE.NOTE]Všechny ostatní nastavení zařízení kdykoli můžete upravit tak, že přístup ke stránce **Konfigurovat** .

## <a name="step-3-add-a-volume"></a>Krok 3: Přidání svazku

Na portálu Azure klasické k vytvoření svazku proveďte následující kroky.

#### <a name="to-create-a-volume"></a>Vytvoření svazku

1. Na **Úvodní** stránce klikněte na **Přidat hlasitost**. Spustí se Průvodce hlasitost přidat.

2. V dialogu přidat průvodce svazku, klikněte v části **Základní nastavení**postupujte takto:

    1. Zadejte jedinečný název pro hlasitost. Název musí být řetězec obsahující znaky 3 až 127.

    2. Zadejte popis hlasitost. Popis vám pomůže identifikovat vlastníci hlasitost.

    3. Vyberte typ použití pro hlasitost. Typ použití může být **Tiered** nebo **místně připnuté hlasitost.** (**Tiered hlasitost** je výchozí hodnota.) Úloh, které vyžadují místní záruky, zhoršeným čekacích dob a vyšší výkon vyberte **místně připnuté** **Hlasitost**. Všechna ostatní data vyberte **Tiered** **Hlasitost**.

        Místně připnuté hlasitost thickly zřízenou a zajišťuje, že primární data objemu zůstane v zařízení a ne přepadového do cloudu. Pokud vytváříte místně připnuté Hlasitost zařízení kontroloval volného místa na místní úrovně zřízení objemu na požadovanou velikost. Vytvoření místně připnuté svazku může vyžadovat přesahu existující data ze zařízení do cloudu a čas potřebný k vytvoření hlasitost může být dlouhé. Celkový čas závisí na velikosti zřizování hlasitost, k dispozici šířka pásma nebo dat na vašem zařízení.

        Vrstvené hlasitosti na druhou stranu tence zřízenou a lze vytvořit velmi rychle. Při vytváření vrstvené hlasitost přibližně vypočítá 10 % prostor máte k dispozici u místních osy a máte k dispozici 90 % mezery v cloudu. Například pokud zřízení 1 TB hlasitost 100 GB by uložena v místním prostoru a 900 GB by být použity v cloudu při úrovní dat. To znamená zase je, že pokud docházet místním prostoru zařízení, nemůžete zřizujete vrstvené sdílet (protože 10 % nebude k dispozici).

    4. Určení zřizování kapacitu hlasitost. Všimněte si, že určité kapacity musí být menší než dostupné kapacity. Pokud vytváříte vrstvené objem, velikost by měl být mezi 500 GB a 5 TB. Místně připnuté objem určete velikost hlasitost mezi 50 GB a 500. Použijte dostupné kapacity jako příručku pro zřízení svazku. Pokud je dostupné místní kapacity 0 GB, pak nebudete moci zřízení místně připnuté nebo vrstvené hlasitost.

        ![Základní nastavení](./media/storsimple-ova-deploy3-iscsi-setup/image17.png)

    5. Klikněte na ikonu se šipkou ![ikonu se šipkou](./media/storsimple-ova-deploy3-iscsi-setup/image18.png) Chcete-li přejít na další stránku.

3. Na stránce **Další nastavení** přidáte nový záznam řízení přístupu (ACR):

    1. Zadejte **název** pro váš ACR.

    2. V části **iSCSI vyzývající název**zadejte iSCSI kvalifikovaný název IQN () Windows Host. Pokud nemáte IQN, přejděte na [dodatku odpověď: Get IQN hostitel systému Windows Server](#appendix-a-get-the-iqn-of-a-windows-server-host).

    3. Doporučujeme povolit zálohy výchozí tak, že zaškrtnete políčko **Povolit zálohy výchozí pro tento hlasitost** . Zálohování výchozí vytvoří zásadu, která provede při 22:30 každý den (zařízení čas) a vytvoří cloudu snímek tento hlasitost.

        ![Další nastavení](./media/storsimple-ova-deploy3-iscsi-setup/image19.png)

    4. Klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Spustí se úlohy vytváření hlasitost. Zobrazí se zpráva postupu podobně jako tento.

        ![zpráva o postupu](./media/storsimple-ova-deploy3-iscsi-setup/image20.png)

        Objem vytvoří se zadaným nastavením. Ve výchozím nastavení monitorování a zálohování bude používat pro hlasitost.

    5. Abyste si ověřili, jestli byla úspěšně vytvořené, přejděte na stránku **svazky** . Měli byste vidět hlasitost uvedené.

        ![](./media/storsimple-ova-deploy3-iscsi-setup/image21.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Krok 4: Připojit, inicializace a formátování svazku

Provedení následujících kroků můžete připojit, inicializace a formátovat svazky StorSimple na hostiteli systému Windows Server.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Pokud chcete připojit, inicializace a formátování svazku

1. Spusťte Microsoft iSCSI vyzývající.

2. V okně **iSCSI vyzývající vlastnosti** na kartě **zjišťování** klikněte na **Zjistit portálu**.

    ![Seznamte se s portálu](./media/storsimple-ova-deploy3-iscsi-setup/image22.png)

3. V dialogovém okně **Seznamte se s cílový portál** zadejte IP adresu rozhraní povolena iSCSI sítě a klikněte na **OK**.

    ![IP adresa](./media/storsimple-ova-deploy3-iscsi-setup/image23.png)

4. V okně **iSCSI vyzývající vlastnosti** na kartě **cíle** vyhledejte **Discovered cílů**. (Každý hlasitost bude zjištěnou cílové.) Stav zařízení by se měly jako **neaktivní**.

    ![zjištěnou cíle](./media/storsimple-ova-deploy3-iscsi-setup/image24.png)

5. Vyberte cílový zařízení a pak klikněte na **Připojit**. Po připojení zařízení by měl stav změní na **Připojit**. (Další informace o používání vyzývající iSCSI Microsoftu, najdete v tématu [instalace a konfigurace aplikace Microsoft iSCSI vyzývající] [1].

    ![Vyberte cílový zařízení](./media/storsimple-ova-deploy3-iscsi-setup/image25.png)

6. Na hostitele Windows stisknutím klávesy s logem Windows + X a potom klikněte na **Spustit**.

7. V dialogovém okně **Spustit** zadejte **Diskmgmt.msc**. Klikněte na tlačítko **OK**a zobrazí se dialogové okno **Správu disku** . V pravém podokně zobrazí svazky na hostitele.

8. V okně **Správa disků** připojené svazky zobrazí, jak je znázorněno na následujícím obrázku. Klikněte pravým tlačítkem myši zjištěnou hlasitost (klikněte na název disku) a potom klikněte na **Online**.

    ![Správa disků](./media/storsimple-ova-deploy3-iscsi-setup/image26.png)

9. Klikněte pravým tlačítkem myši a vyberte **Inicializace disku**.

    ![Inicializace disku 1](./media/storsimple-ova-deploy3-iscsi-setup/image27.png)

10. V dialogovém okně vyberte discích inicializace a potom klikněte na **OK**.

    ![Inicializace disku 2](./media/storsimple-ova-deploy3-iscsi-setup/image28.png)

11. Spustí se Průvodce nové jednoduché hlasitost. Vyberte velikost disku a klikněte na tlačítko **Další**.

    ![Průvodce novým hlasitost 1](./media/storsimple-ova-deploy3-iscsi-setup/image29.png)

12. Písmeno přiřadit hlasitost a klikněte na tlačítko **Další**.

    ![Průvodce novým hlasitost 2](./media/storsimple-ova-deploy3-iscsi-setup/image30.png)

13. Zadejte parametry formátovat hlasitost. **V systému Windows Server je podporována pouze NTFS.** Nastavte Austrálie 64 kB. Zadání popisku pro hlasitost. Je doporučený osvědčený postup pro tento název shodovat hlasitost název, který jste zadali v zařízení s virtuální StorSimple. Klikněte na tlačítko **Další**.

    ![Průvodce novým hlasitost 3](./media/storsimple-ova-deploy3-iscsi-setup/image31.png)

14. Zkontrolujte hodnoty hlasitost a potom klikněte na **Dokončit**.

    ![Průvodce novým hlasitost 4](./media/storsimple-ova-deploy3-iscsi-setup/image32.png)

    Svazky se zobrazí jako **Online** na stránce **Správa disku** .

    ![svazky online](./media/storsimple-ova-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Další kroky

Naučte se používat místní webu uživatelského rozhraní [Spravovat své StorSimple virtuální pole](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Dodatek odpověď: získání IQN hostitel systému Windows Server

Takto zobrazíte iSCSI kvalifikovaný název IQN () hostitele Windows, na kterém běží Windows Server 2012.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Chcete-li získat IQN hostitelem systému Windows

1. Spusťte vyzývající iSCSI Microsoft Windows hostiteli.

2. V okně **iSCSI vyzývající vlastnosti** na kartě **Konfigurace** vyberte a zkopírujte řetězec z pole **Název vyzývající** .

    ![Vlastnosti vyzývající iSCSI](./media/storsimple-ova-deploy3-iscsi-setup/image34.png)

2. Uložte tento řetězec.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



