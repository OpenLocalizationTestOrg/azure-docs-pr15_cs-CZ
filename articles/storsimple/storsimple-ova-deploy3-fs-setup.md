<properties
   pageTitle="Nasazení StorSimple virtuální matice 3: nastavení virtuální zařízení jako souborový server"
   description="Třetí kurzu v virtuální pole StorSimple nasazení pokyn k nastavení virtuální zařízení jako souborový server."
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
   ms.date="05/26/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Nasazení StorSimple virtuální maticový – nastavení nahoru jako souborový server

![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Úvod 

Tento článek se týká všeobecně dostupná (GA) verzi pracovního dne 2016 Microsoft Azure StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení nebo StorSimple virtuální zařízení). Tento článek popisuje, jak počáteční instalace zaregistrovat souborového serveru StorSimple, dokončete nastavení zařízení a vytvořit a připojit ke sdíleným složkám SMB. Toto je poslední článek v řadě potřebné k úplně nasazení virtuální matice jako souborový server nebo serveru iSCSI výukové programy pro nasazení.

Nastavení a konfigurace může trvat zhruba 10 minut dokončete.


## <a name="setup-prerequisites"></a>Zjistit předpoklady pro instalaci

Konfigurace a nastavení zařízení virtuální StorSimple, ujistěte se, že:

-   Máte zřízení virtuální zařízení a připojení k němu podle [ustanovení StorSimple virtuální matice v Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) nebo [poskytování StorSimple virtuální matice v VMware](storsimple-ova-deploy2-provision-vmware.md).

-   Máte klíč registrace služby z službu StorSimple správce, který jste vytvořili StorSimple virtuální zařízení můžete spravovat. Další informace najdete v tématu [Krok 2: získání klíč registrace služby](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) pro StorSimple virtuální pole.

-   Pokud toto je druhý nebo další virtuální zařízení, které jsou zaregistrovali stávající služba StorSimple správce, měli byste mít šifrovací klíč dat služby. Tento klíč vygenerované při prvním zařízení byla úspěšně registrovaný u této služby. Pokud jste ztratili tento klíč, naleznete v tématu [získání šifrovací klíč data service](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) své StorSimple virtuální pole.

## <a name="step-by-step-setup"></a>Podrobné nastavení

Použijte následující podrobné pokyny k nastavení a konfiguraci StorSimple virtuální zařízení.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Krok 1: Dokončit nastavení uživatelského rozhraní místní web a zaregistrovat zařízení 


#### <a name="to-complete-the-setup-and-register-the-device"></a>Dokončete nastavení a zaregistrovat zařízení

1.  Otevřete okno prohlížeče a připojte k místní web uživatelského rozhraní. Typ: 

    `https://<ip-address of network interface>`

    Použití adresy URL připojení uvedeno v předchozím kroku. Zobrazí se chyba označující, že došlo k potížím s certifikátem zabezpečení na webu. Kliknutím na **pokračovat tuto webovou stránku**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)

1.  Přihlaste se k webu rozhraní virtuální zařízení jako **StorSimpleAdmin**. Zadejte heslo správce zařízení, které jste změnili v kroku 3: zahájení virtuální zařízení poskytování [StorSimple virtuální matice v Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) nebo poskytování [StorSimple virtuální matice v VMware](storsimple-ova-deploy2-provision-vmware.md).

    ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)

1.  Přejdete na **domovské** stránce. Tato stránka popisuje různé nastavení pro nakonfigurovat a zaregistrovat virtuální zařízení se službou StorSimple správce. Všimněte si, že jsou volitelné **sítě, nastavení**, **Nastavení webového proxy serveru**a **Nastavení času** . Pouze požadovaná nastavení jsou **Nastavení zařízení** a **Nastavení cloudu**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)

1.  Na stránce **Nastavení sítě** klikněte v části **síť rozhraní**DATA 0 automaticky nakonfigurujete za vás. Každý rozhraní sítě je ve výchozím nastavení automaticky získat IP adresu (DHCP). Proto IP adresu, podsítě a brána automaticky přiřadíte (pro IPv4 a IPv6).

    ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)

    Pokud jste přidali více rozhraní sítě během vytváření zařízení, můžete je zde nakonfigurovat. Poznámka: síťové rozhraní můžete nakonfigurovat jako IPv4 nebo jen IPv4 a IPv6. Konfigurace pouze protokolu IPv6 nepodporuje.

1.  Servery DNS jsou potřeba, protože jsou používány Pokud vaše zařízení snaží komunikovat s zprostředkovatelů vaše cloudové úložiště nebo vyřešit zařízení název po nakonfigurování jako souborovém serveru. Ve skupinovém rámečku **serverů DNS**na stránce **Nastavení sítě** :

    1.  Primární a sekundární server DNS bude automaticky nakonfigurované. Pokud se rozhodnete sdělit konfigurace statické IP adresy, můžete určit serverů DNS. Dostupnost doporučujeme konfigurace primární a sekundární server DNS.

    2.  Klikněte na tlačítko **použít**. Použije a ověřte nastavení sítě.

2.  Na stránce **Nastavení zařízení** :

    1.  Přiřadíte jedinečný **název** do zařízení. Tento název může být znaky 1-15 a může obsahovat písmeno, čísel a spojovníky.

    2.  Klikněte na ikonu **souborovém serveru** ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) **Typ** zařízení, které vytvoříte. Souborový server vám umožní vytvoření sdílené složky.

    3.  Je zařízení souborovém serveru, musíte se ke spojení zařízení a doménu. Zadejte **název domény**.

    1.  Klikněte na tlačítko **použít**.

2.  Zobrazí se dialogové okno. Zadejte svoje přihlašovací údaje domény zadaného formátu. Klikněte na ikonu zaškrtnutí. Bude ověření domény pověření. Zobrazí se chybová zpráva a pokud nejsou správné přihlašovací údaje.

    ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)

1.  Klikněte na tlačítko **použít**. Použije a ověřte nastavení zařízení.

    ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)

    > [AZURE.NOTE]
    > 
    > Zkontrolujte, zda je virtuální argument matice v své vlastní organizační jednotce (OU) služby Active Directory a použitá nebo dědí žádné objekty zásad skupiny (objekt zásad skupiny). Zásady skupiny nainstalovat virtuální pole StorSimple aplikací, jako je antivirový software. Instalace dalšího softwaru nepodporuje a může vést k poškození dat. 

1.  (Volitelně) konfigurace proxy serveru webové. Konfigurace proxy serveru webové je sice volitelný, mějte na paměti, že používáte proxy serveru webové můžete jenom nakonfigurovat ho tady.

    ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)

    **Proxy serveru webové** stránky:

    1.  Zadejte **adresu URL webové proxy serveru** v tomto formátu: *http://&lt;hostitele IP adresa nebo FDQN&gt;: číslo portu*. Všimněte si, že nejsou podporované adresy URL HTTPS.

    2.  Zadejte **ověřovací** jako **základní** nebo **žádný**.

    3.  Pokud pomocí ověřování, bude taky muset zadal **uživatelské jméno** a **heslo**.

    4.  Klikněte na tlačítko **použít**. To bude ověřit a použít nastavení nakonfigurované webového proxy serveru.

1.  Zařízení, třeba časové pásmo a servery NTP primárních a sekundárních (volitelně) v nastavení času. Protože zařízení, musíte k synchronizaci čas tak, aby může ověřovat pomocí poskytovatele služby cloudu jsou vyžadovány NTP servery.

    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)

    Na stránce **Nastavení času** :

    1.  V rozevíracím seznamu vyberte **časové pásmo** podle zeměpisné polohy, ve kterém je nasazený zařízení. Výchozí časové pásmo pro zařízení je PST. Zařízení s použije časového pásma pro všechny plánované operace.

    2.  Určení **primární NTP serveru** pro zařízení nebo ponechte výchozí hodnotu time.windows.com. Ujistěte se, že síti umožňuje NTP předání přenosu z vaší datacentra na Internetu.

    3.  Volitelně zadejte **sekundární NTP serveru** pro svoje zařízení.

    4.  Klikněte na tlačítko **použít**. Bude ověření a nastavení nakonfigurované čas.

1.  Konfigurace nastavení cloudu pro svoje zařízení. V tomto kroku dokončete konfiguraci místního zařízení a pak zaregistrovat zařízení s vašimi službami StorSimple správce.

    1.  Zadání **klíč registrace služby** , kterou jste získali v [Krok 2: získání klíč registrace služby](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) pro StorSimple virtuální pole.

    2.  Když to je první zařízení zaregistrovali tuto službu tento krok přeskočit a přejít k dalšímu kroku. Pokud to není prvním zařízení, které jste se zaregistrovali tuto službu, musíte se k poskytování **služby dat šifrovacího klíče**. Tento klíč se musí používat klávesu služby registrace k registraci další zařízení se službou StorSimple správce. Další informace najdete v příručce získat [služby dat šifrovací klíč](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) na svůj místní web uživatelského rozhraní.

    3.  Klikněte na **zaregistrovat**. To bude Restartujte zařízení. Budete muset počkat 2 – 3 minuty předtím, než úspěšně registrována zařízení. Po restartování zařízení přejdete na přihlašovací stránku.

        ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
    

1.  Vraťte se do portálu Azure klasické. Na stránce **zařízení** ověřte, že zařízení úspěšně se připojil ke službě vyhledáním stav. Stav zařízení by měl být **aktivní**.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Krok 2: Dokončit nastavení požadovaných zařízení

Dokončete konfiguraci zařízení StorSimple zařízení, musíte:

-   Vyberte účet úložiště chcete přidružit k zařízení.

-   Zvolte nastavení šifrování u data, která se neodesílají do cloudu.

Proveďte následující kroky v [Azure klasické portál](https://manage.windowsazure.com/) a dokončete tak nastavení na požadované zařízení.

#### <a name="to-complete-the-minimum-device-setup"></a>Dokončete nastavení minimální zařízení

1.  Na stránce **zařízení** vyberte zařízení, které jste právě vytvořili. Toto zařízení chcete zobrazit jako **aktivní**. Klikněte na šipku vedle jména zařízení a pak klikněte na **Rychlý Start**.

2.  Klikněte na příkaz **Nastavení dokončení zařízení** spusťte Průvodce konfigurací zařízení.

3.  V Průvodci konfigurací zařízení na stránce **Základní nastavení** postupujte takto:

    1.  Zadejte účet úložiště se nemusí používat se svým zařízením. Můžete vybrat existující účet úložiště toto předplatné v rozevíracím seznamu nebo zadejte **Přidat další** účet vyberte z jiné předplatné.

    2.  Určete nastavení šifrování dat – v ostatní (šifrování AES) odeslané do cloudu. Šifrování dat, zaškrtněte políčko pole se seznamem umožňující **cloudové úložiště šifrovacího klíče**. Zadejte šifrování cloudové úložiště, která obsahuje 32 znaků. Klíč pro potvrzení znovu. Klíč AES 256-bit bude použit s uživatelem definovaných klíč pro šifrování.

    3.  Klikněte na ikonu kontrola ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).

        ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

Nastavení bude teď aktualizují. Po úspěšném aktualizaci nastavení na tlačítko Nastavit Dokončeno zařízení zobrazit zašedle. Se vrátíte na stránku **Rychlý Start** zařízení.

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)


> [AZURE.NOTE]                                                              
>
> Všechny ostatní nastavení zařízení kdykoli můžete upravit tak, že přístup ke stránce **Konfigurovat** .

## <a name="step-3-add-a-share"></a>Krok 3: Přidání sdílené složky

Proveďte následující kroky v [Azure klasické portálu](https://manage.windowsazure.com/) pro vytvoření sdílené složky.

#### <a name="to-create-a-share"></a>Vytvoření sdílené složky

1.  Na **Úvodní** stránce klikněte na **Přidat do sdílené složky**. Spustí se Průvodce sdílet přidat.

    ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

1.  Na stránce **Základní nastavení** postupujte takto:

    1.  Zadejte jedinečný název do sdílené složky. Název musí být řetězec obsahující znaky 3 až 127.

    2.  (Volitelné) Zadejte popis pro sdílenou složku. Popis vám pomůže identifikovat vlastníci sdílet.

    3.  Vyberte typ použití pro sdílené položky. Použití lze zadat **Tiered** nebo **místně připnuté**s vrstvené je výchozí hodnota. Pro úloh, které vyžadují místní záruky, zhoršeným čekacích dob a vyšší výkon vyberte **místně připnuté** sdílet. Pro všechna ostatní data vyberte **Tiered** sdílet.

    Místně připnuté sdílet thickly zřízenou a zajišťuje, že primární dat ve sdílené složce zůstane místní zařízení a ne přepadového do cloudu. Vrstvené sdílet na druhou stranu máte tence k dispozici. Při vytváření vrstvené sdílet 10 % mezery máte k dispozici u místních osy a máte k dispozici 90 % mezery v cloudu. Například pokud zřízení 1 TB hlasitost 100 GB by uložena v místním prostoru a 900 GB by být použity v cloudu při úrovní dat. To zase znamená, že pokud docházet místní místa na zařízení nelze vytvořit vrstvené sdílet.

1.  Určení zřizování kapacity do sdílené složky. Všimněte si, že určité kapacity musí být menší než dostupné kapacity. Pokud používáte vrstvené sdílet, sdílet velikost by měl být mezi 500 GB a 20 TB. Sdílené místně připnuté určete velikost sdílet 50 GB až 2 TB. Umožňuje dostupné kapacity jako vodítko zřízení sdílené. Pokud je k dispozici místní kapacitou 0 GB, pak se nebude moct pořizovat zřízení místní nebo vrstvené jejich počet.

    ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)

1.  Klikněte na ikonu se šipkou ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) přejdete na další stránku.

1.  Na stránce **Další nastavení** přiřadíte oprávnění uživatele nebo skupinu, která bude přístup k této sdílené položky. Zadejte název uživatele nebo skupiny uživatelů v *<john@contoso.com>* formát. Doporučujeme použít skupinu uživatelů (ne jednoho uživatele) Pokud chcete povolit správu oprávnění pro přístup k těmto sdíleným složkám. Po přiřazení oprávnění Tady můžete pomocí Průzkumníka Windows těchto oprávnění k úpravám.

    ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)

1.  Klikněte na ikonu kontrola ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). Sdílené se vytvoří se zadaným nastavením. Ve výchozím nastavení monitorování a zálohování bude používat pro sdílení.

## <a name="step-4-connect-to-the-share"></a>Krok 4: Připojení ke sdílené složce

Teď budou muset připojit ke sdílené položky, kterou jste vytvořili v předchozím kroku. Pomocí těchto kroků na hostitele systému Windows Server.

#### <a name="to-connect-to-the-share"></a>Připojení ke sdílené složce

1.  Stiskněte ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. V okně Spustit zadejte * \\ * jako cesty, nahraďte *název serveru soubor* zařízení název, který jste přiřadili souborového serveru. Klikněte na **OK**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)

2.  Otevře se nahoru Průzkumníka. Teď byste měli být zobrazit jako složky sdílené složky, které jste vytvořili. Vyberte a poklikejte na položku sdílet (složku) k prohlížení obsahu.

    ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)

3.  Teď můžete přidat soubory do těchto sdílené položky a prohlédněte zálohy.

![Ikona video](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **Video k dispozici**

Podívejte se na video a zjistěte, jak můžete nakonfigurovat a zaregistrovat StorSimple virtuální matice jako souborový server.

> [AZURE.VIDEO configure-a-storsimple-virtual-array]

## <a name="next-steps"></a>Další kroky

Naučte se používat místní webu uživatelského rozhraní [Spravovat své StorSimple virtuální pole](storsimple-ova-web-ui-admin.md).
