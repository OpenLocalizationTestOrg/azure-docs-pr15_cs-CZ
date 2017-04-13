<properties 
   pageTitle="Nasazení StorSimple zařízení (Update 1) | Microsoft Azure"
   description="Popisuje kroky a osvědčené postupy pro nasazení StorSimple aktualizace 1 zařízení a služeb."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="deploy-your-on-premises-storsimple-device-update-1"></a>Nasazení zařízení StorSimple místní (Update 1)

> [AZURE.SELECTOR]
- [Aktualizace 2](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Aktualizace 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [GA vydání](../articles/storsimple/storsimple-deployment-walkthrough.md)

## <a name="overview"></a>Základní informace

Vítá vás Microsoft Azure StorSimple zařízení nasazení. Tyto výukové programy pro nasazení platí pro StorSimple 8000 řady aktualizace 1.0. Této řadě kurzů popisuje, jak konfigurovat StorSimple zařízení a obsahuje kontrolního seznamu konfigurace, konfiguraci požadavky a podrobné konfigurace kroky.

Informace v těchto kurzech předpokládá, že máte zkontrolována bezpečnostní a rozbalili, racked a zapojené StorSimple zařízení. Pokud potřebujete k provedení těchto úkolů, začněte s revizí [bezpečnostní](storsimple-safety.md). V závislosti na modelu zařízení, které můžete pak rozbalit, připevnění regálu a kabel podle pokynů v části:

- [Rozbalení regálových připojení a kabel vaší 8100](storsimple-8100-hardware-installation.md)
- [Rozbalení regálových připojení a kabel vaší 8600](storsimple-8600-hardware-installation.md)

Budete potřebovat oprávnění správce dokončete proces instalace a konfigurace. Doporučujeme, abyste si prošli kontrolní seznam konfigurace než začnete. Dokončení procesu nasazení a konfiguraci může chvíli trvat.

> [AZURE.NOTE] Informace o nasazení StorSimple publikované na webu Microsoft Azure platí pro StorSimple 8000 řady pouze zařízení. Podrobné informace o 5000 a 7000 zařízení řady, přejděte na: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 5000 a 7000 řady informace o nasazení najdete v článku [StorSimple systém – úvodní příručka](http://onlinehelp.storsimple.com/111_Appliance/).

## <a name="deployment-steps"></a>Kroků nasazení

Pomocí těchto kroků požadované ke konfiguraci StorSimple zařízení a jeho připojení ke službě StorSimple správce. Kromě požadované kroky jsou volitelné kroky a postupy, které budete potřebovat při nasazení. Podrobné pokyny označuje, když je třeba provést jednotlivých kroků volitelné.


| Krok                                                                                   | Popis                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ZJISTIT PŘEDPOKLADY PRO**                                                                      | Tyto bude muset udělat při přípravě na nadcházející nasazení.                                                                                        |
| Kontrolní seznam konfigurace nasazení.                                                     | Shromáždění a záznam informace před a během nasazení využít tento kontrolní seznam.                                                                       |
| Požadavky na nasazení.                                                               | Tyto ověřit prostředí je připravený na nasazení.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **PODROBNÉ NASAZENÍ**                                                                   | Tyto kroky nutné nasazení StorSimple zařízení v výroby.                                                                                      |
| Krok 1: Vytvoření nové služby.                                                         | Nastavení správy cloudu a prostor úložiště pro vaše zařízení StorSimple. Když máte existující službu pro jiná zařízení StorSimple tento krok přeskočte.                |
| Krok 2: Získání klíč registrace služby.                                               | Pomocí tohoto klíče zaregistrovat a připojte zařízení StorSimple pomocí služby správy.                                                                         |
| Krok 3: Nakonfigurovat a zaregistrovat zařízení přes Windows PowerShell pro StorSimple.    | Připojení k síti zařízení a zaregistrujte s Azure dokončit nastavení pomocí služby pro správu.                                            |
| Krok 4: Dokončení instalace minimální zařízení</br>Volitelné: Aktualizujte StorSimple zařízení.      | Pomocí služby správy dokončit nastavení zařízení a povolte ho k poskytování úložiště.                                                                      |
| Krok 5: Vytvoření kontejneru hlasitost.                                                      | Vytvoření kontejneru k poskytování svazky. Hlasitost kontejneru má účtu úložiště, šířku pásma a nastavení šifrování u všech objemů obsažené v něm.    |
| Krok 6: Vytvoření svazku.                                                                | Zřízení svazky úložiště v zařízení StorSimple serverů.                                                                                        |
| Krok 7: Připojit, inicializace a formátování svazku.</br>Volitelné: Nakonfigurujte MPIO.            | Připojte serverech k základnímu úložišti iSCSI poskytovanou zařízení. Volitelně nakonfigurujte MPIO zajistit, že serverech nevadí vám odkaz, sítě a selhání rozhraní.                                                                                                                                                              |
| Krok 8: Prohlédněte zálohy.                                                                  | Nastavit zásady zálohování ochrana dat                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **DALŠÍ POSTUPY**                                                                   | Můžete odkázat na tyto postupy při nasazení řešení.                                                                                        |
| Nastavení nového účtu úložiště služby.                                      |                                                                                                                                                               |
| Použití nátěrové připojení konzoly sériové zařízení.                                    |                                                                                                                                                               |
| Pokud potřebujete IQN hostitel systému Windows Server.                                                   |                                                                                                                                                               |
| Vytvoření ruční zálohování.                                                                 | 


## <a name="deployment-configuration-checklist"></a>Kontrolní seznam konfigurace nasazení

Následující seznam konfigurace nasazení popisuje informace, které je potřeba shromáždit před a jak nakonfigurovat software pro v zařízení s StorSimple. Příprava schůzky v některé z těchto informací předem vám pomůže zjednodušit proces nasazení zařízení StorSimple ve vašem prostředí. Využít tento kontrolní seznam a Všimněte si také dolů podrobnosti o konfiguraci při nasazení zařízení.

| Fáze                                  | Parametr                                         | Podrobnosti                                                                                                                                                                | Hodnoty |
|----------------------------------------|---------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|
| **Kabel zařízení**                      | Sériový přístup                                     | Konfigurace počáteční zařízení                                                                  | Ano/Ne |
|   |   |  |  |
| **Nakonfigurovat a zaregistrovat zařízení**          | Nastavení sítě data 0                           | Data 0 IP adresy:</br>Maska podsítě:</br>Brána:</br>Primární server DNS:</br>Primární NTP serveru:</br>Proxy serveru IP nebo plně kvalifikovaný název domény (nepovinné):</br>Port serveru proxy web:|        |
|                                        | Heslo správce zařízení                     | Heslo musí mít mezi 8 a 15 znaky obsahující malá, velká písmena, číselné a speciální znaky. |        |
|                                        | StorSimple snímek správce hesel              | Heslo musí být 14 15 znaků obsahující malá, velká písmena, číselné a speciální znaky.|        |
|                                        | Klíč registrace služby                          | Tento klíč generováno z portálu Microsoft Azure klasické.    |        |
|                                        | Služba dat šifrovacího klíče                       | Tento klíč se vytvoří při zařízení máte zaregistrované v rámci službu pro správu přes Windows PowerShell pro StorSimple. Zkopírujte tento klíč a uložte jej bezpečné místo.|  |
|   |   |  |  |
| **Nastavení dokončení minimální zařízení**          | Popisný název pro zařízení                     | Toto je popisný název zařízení. |        |
|                                        | Podle časového pásma                                          | Zařízení s použije časového pásma pro všechny plánované operace.  |        |
|                                        | Sekundární DNS server                              | Toto je požadovaná konfigurace.                                  |        |
|                                        | Rozhraní sítě: správce údajů 0 pevnou IP adresy                                     | Tyto IP by měl být směrovatelné k Internetu.</br>Řadiče 0 pevnou IP adresy:</br>Řadiči 1 pevnou IP adresy:|
|   |   |  |  |
| **Další rozhraní nastavení sítě**  | Rozhraní sítě: dat 1</br>Pokud je povoleno iSCSI konfiguraci brány.      | Účel: Použít cloudu/iSCSI/není</br>IP adresy:</br>Maska podsítě:</br>Brána:|
|                                        | Rozhraní sítě: dat 2</br>Pokud je povoleno iSCSI konfiguraci brány.      | Účel: Použít cloudu/iSCSI/není</br>IP adresy:</br>Maska podsítě:</br>Brána:|
|                                        | Rozhraní sítě: dat 3</br>Pokud je povoleno iSCSI konfiguraci brány.      | Účel: Použít cloudu/iSCSI/není</br>IP adresy:</br>Maska podsítě:</br>Brána:|
|                                        | Rozhraní sítě: dat 4</br>Pokud je povoleno iSCSI konfiguraci brány.      | Účel: Použít cloudu/iSCSI/není</br>IP adresy:</br>Maska podsítě:</br>Brána:|
|                                        | Rozhraní sítě: dat 5</br>Pokud je povoleno iSCSI konfiguraci brány.      | Účel: Použít cloudu/iSCSI/není</br>IP adresy:</br>Maska podsítě:</br>Brána:|
|   |   |  |  |
| **Vytvoření kontejneru hlasitosti**                      | Název svazku kontejner:                            | Název kontejneru                                                                                                                                                 |        |
|                                        | Účet Azure úložiště:                            | Klíč název a přístup k účtu úložiště chcete přidružit k této hlasitost kontejneru                                                                                              |        |
|                                        | Cloudové úložiště šifrovací klíč:                     | Šifrovací klíč pro ukládání v jednotlivých kontejneru                                                                                                                           |        |
|   |   |  |  |
| **Vytvoření svazku**                        | Podrobnosti pro každou hlasitosti                           | Hlasitost název:                                                                                                                                                           |        |
|                                        |                                                   | Velikost:                                                                                                                                                                  |        |
|                                        |                                                   | Typ použití:                                                                                                                                                            |        |
|                                        |                                                   | Název ACR:                                                                                                                                                              |        |
|                                        |                                                   | Výchozí zásady zálohování:                                                                                                                                                 |        |
|   |   |  |  |
| **Připojit, inicializace a formátování svazku** | Podrobnosti pro každou serveru připojení k základnímu úložišti | Název serveru Windows:                                                                                                                                                   |        |
|                                        |                                                   | Windows Server IQN:                                                                                                                                                    |        |
|                                        |                                                   | Název serveru Windows hlasitost:                                                                                                                                                   |        |
|                                        |                                                   | NTFS připojení čárky/jednotky:                                                                                                                                      |        |

## <a name="deployment-prerequisites"></a>Zjistit předpoklady pro nasazení

Následující části popisují konfigurace požadavcích na StorSimple Správce služby a StorSimple zařízení.

### <a name="for-the-storsimple-manager-service"></a>Pro službu StorSimple správce

Než začnete, ujistěte se, že:

- Máte účet Microsoft pomocí přihlašovacích údajů aplikace access.

- Máte účet Microsoft Azure úložiště pomocí přihlašovacích údajů aplikace access.

- Vaše předplatné Microsoft Azure aktivované řešení službu StorSimple správce. Vaše předplatné má zakoupené prostřednictvím [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/).

- Máte přístup k softwaru emulace terminálu například nátěrové.

### <a name="for-the-device-in-the-datacenter"></a>Zařízení v datacentra

Než začnete konfigurovat zařízení, ujistěte se, že:

- Zařízení je plně rozbalili připojeny regálu a plně zapojené power, sítě a sériový přístup podle popisu v:

    -  [Rozbalení regálových připojení a kabel 8100 zařízení](storsimple-8100-hardware-installation.md)
    -  [Rozbalení regálových připojení a kabel 8600 zařízení](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>V síti datacentra

Než začnete, ujistěte se, že:

- Porty v bráně firewall datacentra jsou otevřené a povolte iSCSI přenosy podle popisu v [síti požadavky pro zařízení s StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)v cloudu.


## <a name="step-by-step-deployment"></a>Podrobné nasazení

Použijte následující podrobné pokyny pro nasazení StorSimple zařízení v datacentra.

## <a name="step-1-create-a-new-service"></a>Krok 1: Vytvoření nové služby

Služba StorSimple správce můžete spravovat více StorSimple zařízení. Proveďte následující postup vytvoření nové instance službu StorSimple správce.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Pokud Nepovolili jste automatické vytváření účtu úložiště s vašimi službami, bude potřeba vytvořit alespoň jeden účet úložiště po úspěšném vytvoření služby. Tento účet úložiště použijí při vytváření kontejneru hlasitost. 
>
> * Pokud jste úložiště účet nevytvořili automaticky, přejděte na [Konfigurovat nového účtu úložiště služby](#configure-a-new-storage-account-for-the-service) podrobné pokyny. 
> * Pokud jste povolili automatické vytváření účtu úložiště, přejděte na [Krok 2: získání klíč registrace služby](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Krok 2: Získání klíč registrace služby

Po službu StorSimple správce do začátků, musíte získat klíč registrace služby. Tento klíč slouží k registraci a připojte zařízení StorSimple se službou.

Na portálu Azure klasické proveďte následující kroky.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Krok 3: Nakonfigurovat a zaregistrovat zařízení přes Windows PowerShell pro StorSimple

Použijte Windows PowerShell pro StorSimple dokončit počáteční nastavení zařízení StorSimple způsobem popsaným v následujícím způsobem. Musíte používat software pro emulaci terminálu dokončete tento krok. Další informace najdete v tématu [Použití nátěrové připojení konzoly sériové zařízení](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Krok 4: Dokončení instalace minimální zařízení

Konfigurace minimální zařízení StorSimple zařízení je nutné: 

- Nastavte sekundární server DNS.
- Povolte iSCSI rozhraní alespoň jeden sítě.
- Přiřadíte obou řadiče pevné IP adres.

Proveďte následující kroky v portálu Azure klasické dokončit nastavení minimální zařízení.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Krok 5: Vytvoření kontejneru hlasitosti

Hlasitost kontejneru má účtu úložiště, šířku pásma a nastavení šifrování u všech objemů obsažené v něm. Bude je potřeba vytvořit kontejner hlasitost před spuštěním zřizování svazky na zařízení s StorSimple. 

Proveďte následující kroky v Azure klasické portálu pro vytvoření kontejneru hlasitost.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Krok 6: Vytvoření svazku

Po vytvoření kontejneru hlasitost můžete zřizujete svazku úložiště v zařízení StorSimple serverech. Na portálu Azure klasické k vytvoření svazku proveďte následující kroky.

> [AZURE.IMPORTANT] Pouze tence zřizování svazky lze vytvářet StorSimple správce. Nemůžete vytvořit zřizování plně nebo částečně zřizování svazky. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Krok 7: Připojit, inicializace a formátování svazku

Podle těchto kroků provádí na hostitele systému Windows Server. 


> [AZURE.IMPORTANT]

> - Dostupnost StorSimple řešení doporučujeme konfigurace MPIO na serverech hostitele (volitelné) před konfigurace iSCSI. Konfiguraci MPIO na serverech hostitele zajistí, že servery nevadí vám odkazu, síť nebo selhání rozhraní.

> - Pokyny MPIO a iSCSI instalace a konfigurace hostitele systému Windows Server přejděte na [Konfigurovat MPIO StorSimple zařízení](storsimple-configure-mpio-windows-server.md). Patří bude taky jednotlivých kroků pro připojení, inicializace a formátovat StorSimple.

> - Pokyny MPIO a iSCSI instalace a konfigurace hostitele Linux přejděte na [Konfigurovat MPIO vašeho hostitele StorSimple Linux](storsimple-configure-mpio-on-linux.md)

Pokud se rozhodnete konfigurace MPIO, proveďte následující kroky připojit, inicializace a formátovat svazky StorSimple na hostiteli systému Windows Server.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Krok 8: Prohlédněte zálohy

Zálohování poskytují ochrana svazky a zlepšit zotavení: při minimalizace obnovení časy. Dva typy zálohování lze provádět se zařízení StorSimple: místní snímky a snímky cloudu. Jednotlivé typy záložní může být **naplánováno** nebo **ručně**. 

Proveďte následující kroky v Azure klasické portálu pro vytvoření naplánované zálohy.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Zálohování ručním může trvat kdykoli. Postupy přejděte na téma [vytváření ruční zálohování](#create-a-manual-backup). 

## <a name="configure-a-new-storage-account-for-the-service"></a>Nastavení nového účtu úložiště služby

Toto je nepovinný krok, který je potřeba provést jenom v případě, že Nepovolili jste automatické vytváření účtu úložiště s vašimi službami. Účet Microsoft Azure úložiště je potřeba k vytvoření kontejneru StorSimple hlasitost.

Pokud je potřeba vytvořit účet Azure úložiště v jiné oblasti, přečtěte si článek [O úložiště účty adresářové služby Azure](../storage/storage-create-storage-account.md) podrobné pokyny.

Na portálu na stránce **Správce StorSimple služby** Azure klasické proveďte následující kroky.

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Použití nátěrové připojení konzoly sériové zařízení

Připojit se k prostředí Windows PowerShell pro StorSimple, budete muset používat software emulace terminálu například nátěrové. Nátěrové se dají používat při přístupu zařízení přímo v konzole sériové nebo otevřením rámci těchto relací ze vzdáleného počítače.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]


## <a name="scan-for-and-apply-updates"></a>Vyhledání a použití aktualizací

Aktualizace zařízení může trvat několik hodin. Provedení následujících kroků můžete vyhledat a nainstalovat aktualizace na vašem zařízení.
<!--can take 1-4 hours--> 

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Chcete-li aktualizovat zařízení

1.  Na **Úvodní** stránce klikněte na **zařízení**. Vyberte fyzické zařízení, klikněte na **údržbu** a potom klikněte na **Zkontrolovat aktualizace**.  

2.  Je vytvořen projektu k vyhledání dostupných aktualizací. Pokud jsou k dispozici aktualizace, **Zkontrolovat aktualizace** se změní na **Instalovat aktualizace**. Klikněte na **instalovat aktualizace**. 

3.  Aktualizace úlohy se vytvoří. Sledování stavu aktualizace tak, že přejdete na **projekty**.

    > [AZURE.NOTE] Po spuštění úlohy aktualizace okamžitě se zobrazí stav jako 50 procent. Změny stavu na 100 % rovná až po dokončení aktualizace projektu. Neexistuje žádná data v reálném stav procesu aktualizace.

4.  Po úspěšném aktualizaci zařízení povolte Pokud nebyly zakázané tyto Data 2 a 3 dat síťová rozhraní.

<!-- In step 2, you may be requested to disable Data 2 and Data 3 prior to installing the updates. You must disable these network interfaces or the updates may fail.-->

## <a name="get-the-iqn-of-a-windows-server-host"></a>Získání IQN hostitel systému Windows Server

Takto zobrazíte iSCSI kvalifikovaný název IQN () hostitele Windows, na kterém běží Windows Server® 2012.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Vytvoření ruční zálohování

Na portálu Azure klasické vytvořit zálohu ruční na vyžádání pro jeden hlasitost na zařízení s StorSimple proveďte následující kroky.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="configure-mpio"></a>Konfigurace MPIO

Funkce v/v (MPIO) je volitelná funkce a není nainstalovaný v systému Windows Server ve výchozím nastavení. Měli byste mít nainstalované jako funkce pomocí Správce serveru. MPIO pokyny k instalaci přejděte na [Konfigurovat MPIO StorSimple zařízení](storsimple-configure-mpio-windows-server.md).

U MPIO pokyny k instalaci StorSimple zařízení připojené k hostitel Linux přejděte na [Konfigurovat MPIO vašeho hostitele Linux](storsimple-configure-mpio-on-linux.md).


> [AZURE.NOTE] MPIO není podporována ve StorSimple virtuální zařízení. 



## <a name="next-steps"></a>Další kroky

- Konfigurace [virtuální zařízení](storsimple-virtual-device-u2.md).

- Umožňuje spravovat vaše zařízení StorSimple [StorSimple Správce služby](storsimple-manager-service-administration.md) .
 
