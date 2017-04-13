<properties 
   pageTitle="Virtuální zařízení StorSimple aktualizace 2 | Microsoft Azure"
   description="Naučte se vytvářet, nasazení a spravovat StorSimple virtuální zařízení v síti Microsoft Azure virtuální. (Platí pro StorSimple aktualizace 2)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/23/2016"
   ms.author="alkohli" />

# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Nasazením a správou StorSimple virtuální zařízení v Azure


##<a name="overview"></a>Základní informace
Virtuální zařízení řady StorSimple 8000 je další funkce, která je součástí vašeho řešení Microsoft Azure StorSimple. Virtuální zařízení StorSimple běží na virtuálního počítače v síti Microsoft Azure virtuální a můžete ji použijete k obecnějším údajům a kopírovat data z vašeho hostitele. Tento kurz popisuje, jak nasazením a správou virtuální zařízení v Azure a je k dispozici všechny virtuální zařízení verzi softwaru aktualizace 2 a malá písmena.


#### <a name="virtual-device-model-comparison"></a>Porovnání modelů virtuální zařízení

Virtuální zařízení StorSimple je k dispozici ve dvou modelů, standardní 8010 (dříve označované jako 1100) a premium 8020 (zavedený aktualizace 2). Porovnání dvou modelů se: tabulkový níže.


| Model zařízení          | 8010<sup>1</sup>                                                                     | 8020                                                                                                                               |
|-----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Maximální kapacity**      | 30 TB                                                                     | 64 TB                                                                                                                                |
| **Azure OM**              | Standard_A3 (4 jádra, 7 GB paměti)                                                                      | Standard_DS3 (4 jádra, 14 GB paměti)                                                                                                                          |
| **Kompatibilita verzí** | Verzí předem aktualizace 2 nebo novější                                             | Spuštění aktualizace 2 nebo novější verze                                                                                                  |
| **Dostupnost oblast**   | Všechny oblasti Azure                                                         | Azure oblastí, které podporují Premium úložiště<br></br>Seznam oblastí najdete v tématu [podporované oblasti 8020](#supported-regions-for-8020) |
| **Typ úložiště**          | Použití Azure standardní prostor úložiště pro místní disků<br></br> Zjistěte, jak [vytvořit účet standardní úložiště]() | Používá Azure Premium úložiště pro místní disků<sup>2</sup> <br></br>Zjistěte, jak [vytvořit účet Premium úložiště](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)                                                               |
| **Pokyny pro pracovní zátěž**     | Položky úrovně načtení souborů systému pomocí zálohy                                              | Odchylka v cloudu a otestujte scénáře, zhoršeným latence, vyšší výkon úloh <br></br>Sekundární zařízení havárie obnovení                                                                                            |
 
<sup>1</sup> *dřív označoval jako 1100*.

<sup>2</sup> *8010 a 8020 pomocí standardní úložiště Azure osy cloudu. Rozdíl existuje pouze v místním osy v zařízení*.

#### <a name="supported-regions-for-8020"></a>Podporovaných oblastí pro 8020

Podporované jsou aktuálně pro 8020 oblastí Premium úložiště jsou: tabulkový níže. Tento seznam průběžně aktualizují jako Premium úložiště bude dostupný ve více oblastech. 

| S. Ne.                                                  | Aktuálně podporované v oblasti |
|---------------------------------------------------------|--------------------------------|
| 1                                                       | Centrální USA                     |
| 2                                                       |  Východní USA                       |
| 3                                                       |  Východní USA 2                     |
| 4                                                       | Západ USA                        |
| 5                                                       | Severní Evropě                   |
| 6                                                       | Západní Evropě                    |
| 7                                                       | Jihovýchodní Asie                 |
| 8                                                       | Japonsko východ                     |
| 9                                                       | Japonsko západní                     |
| 10                                                      | Austrálie východ                 |
| 11                                                      | Austrálie jihovýchodní *           |
| 12                                                      | Východní Asie *                     |
| 13                                                      | Jižní ústřední USA *              |

* Premium úložiště byl spuštěn naposledy v těchto geos.

Tento článek popisuje postup nasazení StorSimple virtuální zařízení v Azure. Po přečtení v tomto článku, udělejte toto:

- Vysvětlení, jak virtuální zařízení se liší od fyzické zařízení.

- Možné vytvořit a nakonfigurovat virtuální zařízení.

- Připojení k virtuální zařízení.

- Informace o práci s virtuální zařízení.

Tento kurz platí pro všechny StorSimple virtuální zařízení spuštění aktualizace 2 nebo novější. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Jak se liší od fyzické zařízení virtuální zařízení

Virtuální zařízení StorSimple je jen software verze StorSimple, které se spouští v jediném v Microsoft Azure virtuálního počítače. Virtuální zařízení podporuje havárie obnovení scénáře, ve kterých zařízení fyzicky není k dispozici a je vhodné pro použití v načítání úrovni položek z zálohování, obnovení havárie místní a cloudu vývojáře test příklady použití.

#### <a name="differences-from-the-physical-device"></a>Rozdíly mezi fyzické zařízení

Následující tabulka zobrazuje určité důležité rozdíly mezi virtuální zařízení StorSimple a zařízení fyzicky StorSimple.

|                             | Pole fyzicky zařízení                                          | Virtuální zařízení                                                                            |
|-----------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Umístění**                    | Je umístěn v datacentra.                               | Spustí v Azure.                                                                            |
| **Rozhraní sítě**          | Má šest rozhraní sítě: DATA 0 až DATA 5.                  | Obsahuje pouze jeden rozhraní sítě: DATA 0.                                        |
| **Registrace**                | Registrovaná během krok konfigurace.                | Registrace je samostatných úkolů.                                                          |
| **Služba dat šifrovacího klíče** | Obnovit fyzického zařízení a pak aktualizujte virtuální zařízení s novým klíčem.           | Nejde obnovit z virtuální zařízení. |


## <a name="prerequisites-for-the-virtual-device"></a>Požadavky pro virtuální zařízení

Následující části popisují konfigurace požadavcích na zařízení s virtuální StorSimple. Před nasazením virtuální zařízení, přečtěte si [otázky bezpečnosti pro používání virtuální zařízení](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Požadavky na Azure

Před zřízení virtuální zařízení potřebujete udělat následující přípravy v Azure prostředí:

- Virtuální zařízení [Konfigurace virtuální síti Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Pokud používáte Premium úložiště, musíte vytvořit virtuální sítě v Azure oblasti, který podporuje Premium úložiště. Další informace o [oblastech, které jsou aktuálně podporované pro 8020](#supported-regions-for-8020).
- Je vhodné použít výchozí server DNS poskytovanou Azure místo určení vlastní název serveru DNS. Pokud není platný název vašeho serveru DNS nebo pokud DNS server není správně vyřešit IP adresy, vytváření virtuálních zařízení se nezdaří.
- Bod webů a webů web jsou volitelný, ale není potřeba. Pokud chcete, můžete nakonfigurovat tyto možnosti pokročilejší scénářích. 
- Vytvoření [Azure virtuálních počítačích](../virtual-machines/virtual-machines-linux-about.md) (hostitelské servery) v virtuální síti, ve které můžete použít svazky zveřejněné příslušným virtuální zařízení. Těchto serverech musí splňovat následující kritéria:                            
    - Odpovídat na iSCSI vyzývající softwaru nainstalovaného Windows nebo Linux VMs.
    - Běžet ve stejné síti virtuální jako virtuální zařízení.
    - Moct připojit k cíle iSCSI virtuální zařízení přes interní IP adresu virtuální zařízení.

- Zkontrolujte, že jste nakonfigurovali podpora iSCSI a cloudu přenosy ve stejné síti virtuální.


#### <a name="storsimple-requirements"></a>Požadavky na StorSimple

Tak, abyste tyto aktualizace služby Azure StorSimple před vytvořením virtuální zařízení:


- Přidání [záznamů kontrol přístup](storsimple-manage-acrs.md) pro VMs, které budou hostitelské servery virtuální zařízení.

- Použití [účtu úložiště](storsimple-manage-storage-accounts.md#add-a-storage-account) ve stejné oblasti jako virtuální zařízení. Účty úložiště v různých oblastech může způsobit snížení výkonu. Můžete použít standardní nebo Premium úložiště účet s virtuální zařízení. Další informace o tom, jak vytvořit [standardní úložiště účet]() nebo [účet Premium úložiště](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)

- Použití různých úložiště účtu pro vytváření virtuálních zařízení ze použitým pro vaše data. Použitím jednoho účtu úložiště, můžete mít snížení výkonu.

Zkontrolujte, jestli tyto informace, než začnete:

- Azure klasické portálu účtu pomocí přihlašovacích údajů aplikace access.

- Kopie šifrovací klíč služby data ze zařízení fyzicky.


## <a name="create-and-configure-the-virtual-device"></a>Vytváření a konfigurace virtuální zařízení

Před provedením těchto postupů zkontrolujte, zda jsou splněny [požadavcích na virtuální zařízení](#prerequisites-for-the-virtual-device). 

Po vytvoření virtuální sítě, nakonfigurované StorSimple Správce služby a registrovaný zařízení fyzicky StorSimple se službou, můžete vytvořit a nakonfigurovat virtuální zařízení StorSimple podle těchto kroků. 

### <a name="step-1-create-a-virtual-device"></a>Krok 1: Vytvoření virtuální zařízení

Provedení následujících kroků můžete vytvořit virtuální zařízení StorSimple.

[AZURE.INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Vytváření virtuálních zařízení selže v tomto kroku, pravděpodobně máte připojení k Internetu. Další informace najdete v tématu [Poradce při potížích s chyby připojení k Internetu](#troubleshoot-internet-connectivity-errors) při creatig virtuální zařízení.


### <a name="step-2-configure-and-register-the-virtual-device"></a>Krok 2: Nakonfigurovat a zaregistrovat virtuální zařízení

Před zahájením tohoto postupu zkontrolujte, jestli kopii šifrovací klíč dat služby. Šifrovací klíč dat služby byl vytvořen, když jste nakonfigurovali první StorSimple zařízení a dostali pokyn ji chcete uložit na zabezpečeném místě. Pokud nemáte kopii šifrovací klíč dat služby, musíte kontaktovat Microsoft Support požádejte o pomoc.

Provedení následujících kroků můžete nakonfigurovat a zaregistrovat zařízení virtuální StorSimple.
[AZURE.INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Krok 3: (Volitelné) změnit nastavení zařízení

Následující části jsou uvedeny nastavení konfigurace zařízení potřeby StorSimple virtuální zařízení, pokud chcete použít CHAP, správce StorSimple snímku nebo změna hesla správce zařízení.

#### <a name="configure-the-chap-initiator"></a>Konfigurace vyzývající CHAP

Tento parametr obsahuje přihlašovací údaje, které očekává virtuální zařízení (cíl) z iniciátory (servery), které se pokusí o přístup svazky. Iniciátory poskytne CHAP uživatelské jméno a heslo CHAP identifikovat do zařízení během tohoto ověřování. Podrobný postup přejděte na [Konfigurovat CHAP pro svoje zařízení](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>Nakonfigurovat cílovou CHAP

Tento parametr obsahuje používané zařízení virtuální vyzývající CHAP s podporou požádá o ověření vzájemné nebo obousměrné přihlašovací údaje. Virtuální zařízení použije Převrátit CHAP uživatelské jméno a heslo Převrátit CHAP identifikuje vyzývající během tohoto procesu ověření. Všimněte si, že nastavení cíle CHAP globálních nastavení. Když tyto soubory platí, použije všechny svazky připojené k úložné zařízení virtuální CHAP ověřování. Podrobný postup přejděte na [Konfigurovat CHAP pro svoje zařízení](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Konfigurace StorSimple snímek správce hesel

Software StorSimple snímek Manager je umístěn na hostitele Windows a umožňuje správcům spravovat zálohy StorSimple zařízení ve formuláři místních i cloudových snímky.

>[AZURE.NOTE] Virtuální zařízení je hostitele Windows Azure virtuálního počítače.

Při konfiguraci zařízení ve snímku správci StorSimple, budete vyzváni k StorSimple zařízení IP adresa a heslo k ověření úložné zařízení. Podrobný postup přejděte na [Správce konfigurace snímek StorSimple heslo](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-the-device-administrator-password"></a>Změna hesla správce zařízení 

Při použití rozhraní prostředí Windows PowerShell pro přístup k virtuální zařízení budete vyzváni k zadání hesla správce zařízení. Zabezpečení dat je nutné před virtuální zařízení lze změnit toto heslo. Podrobný postup přejděte na [Konfigurovat heslo správce zařízení](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-to-the-virtual-device"></a>Připojení vzdálených virtuální zařízení
Ve výchozím nastavení není povolený vzdálený přístup k virtuální zařízení prostřednictvím rozhraní prostředí Windows PowerShell. Budete muset povolit vzdálená správa na virtuální zařízení nejdřív a potom ho na klienta, který bude sloužit k přístupu k virtuální zařízení.

Krocích připojení vzdálených jsou uvedeny níže.

### <a name="step-1-configure-remote-management"></a>Krok 1: Konfigurace Vzdálená správa

Proveďte následující kroky pro nastavení Vzdálená správa StorSimple virtuální zařízení.

[AZURE.INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Krok 2: Vzdálený přístup k virtuální zařízení

Po povolení Vzdálená správa na stránce konfigurace StorSimple můžete použít Vzdálená prostředí Windows PowerShell pro připojení k virtuální zařízení z jiného počítače virtuální do stejné virtuální sítě; například se můžete připojit z hostitele OM, které jste nakonfigurovali připojuje iSCSI. Ve většině implementací jste se už otevřeli veřejné koncový bod pro přístup k hostiteli OM, můžete použít pro přístup k virtuální zařízení.

>[AZURE.WARNING] **Zvýšit úroveň zabezpečení doporučujeme při připojování k koncové body používat HTTPS a potom odstraňte koncové body, pokud jste dokončili vzdálenou relaci Powershellu.**

Postupujte podle postupy v [připojení vzdáleně do zařízení StorSimple](storsimple-remote-connect.md) nastavit Vzdálená virtuální zařízení.

## <a name="connect-directly-to-the-virtual-device"></a>Přímé připojení k virtuální zařízení

Taky můžete připojit přímo do virtuálního zařízení. Pokud chcete připojit přímo do virtuálního zařízení z jiného počítače mimo virtuální síť nebo mimo prostředí Microsoft Azure, je potřeba vytvořit další koncové body podle následujícího postupu. 

Proveďte následující postup vytvoření veřejné koncového bodu na virtuální zařízení.

[AZURE.INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Doporučujeme, protože tento postup slouží k minimalizaci číslo veřejné koncové body v síti virtuální připojit z jiného počítače virtuální do stejné virtuální sítě. Když použijete tento způsob, jednoduše připojit k počítači virtuální prostřednictvím relaci vzdálené plochy a nakonfigurujte virtuální počítače pro použití jako kdybyste jakéhokoli Windows klienta v místní síti. Přidání veřejné port číslo portu se již známo, že nepotřebujete.

## <a name="work-with-the-storsimple-virtual-device"></a>Práce s StorSimple virtuální zařízení

Teď, když jste již vytvořili a nakonfigurovali virtuální zařízení StorSimple, jste připraveni začít pracovat s ním. Můžete na pracujete s hlasitost kontejnerů, svazky a zálohování zásady virtuální zařízení stejným způsobem na fyzické StorSimple zařízení; jediný rozdíl je třeba zajistit, v seznamu zařízení vyberte virtuální zařízení. Podívejte se do [používat službu StorSimple správce pro správu virtuální zařízení](storsimple-manager-service-administration.md) podrobné postupy různé úlohy správy pro virtuální zařízení.

Následujících částech jsou uvedeny některé rozdíly, ke kterým se dojít při práci s virtuální zařízení.

### <a name="maintain-a-storsimple-virtual-device"></a>Udržovat StorSimple virtuální zařízení

Protože je jen software zařízení, je minimální ve srovnání s údržba zařízení fyzicky údržbu virtuální zařízení. Máte k dispozici následující možnosti:

- **Aktualizace softwaru** – můžete zobrazit data, které se naposledy aktualizovaly software, spolu s některou aktualizovat zprávy o stavu. V dolní části na stránce můžete pomocí tlačítka **Prohledat aktualizace** provést ruční vyhledávání, pokud chcete vyhledat nové aktualizace. Pokud jsou k dispozici aktualizace, klikněte na **Instalovat aktualizace** . Vzhledem k tomu, že existuje pouze jeden rozhraní na virtuální zařízení, znamená to, že bude přerušení lehké služby při použití aktualizací. Virtuální zařízení se vypnout a restartujte (v případě potřeby) provádět všechny aktualizace vydané. Podrobný postup najdete v tématu [aktualizovat zařízení](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
- **Podpora balíček** – můžete vytvořit a nahrát balíček podpory k řešení problémů s virtuální zařízení Microsoft Support. Podrobný postup najdete v můžete [vytvořit a spravovat balíčku podpory](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Účty úložiště pro virtuální zařízení

Vytvoření účtů úložiště pro použití službu StorSimple správce, virtuální zařízení a fyzické zařízení. Když vytvoříte účtů úložiště, doporučujeme použít identifikátor oblasti do pole popisný název pomůže zajistit konzistentní celé součásti systému oblasti. Virtuální zařízení je důležité, aby všechny prvky ve stejné oblasti, kterou chcete předejít problémům s výkonem.

Podrobný postup přejděte na [Přidat účet úložiště](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Deaktivace StorSimple virtuální zařízení

Deaktivace virtuální zařízení odstraní OM a zdroje vytvořen, pokud byla zřízení. Po deaktivaci virtuální zařízení nejde obnovit původní stav. Než deaktivovat virtuální zařízení měli zastavit nebo odstranění klienty a hosts, které jsou závislé na.

Deaktivace virtuální zařízení výsledkem následujících akcí:

- Virtuální zařízení se odebere.

- Operační systém disku a data disků vytvoří virtuální zařízení se odeberou.

- Hostitelská služba a virtuální sítě vytvořen během zřizování se zachovají. Pokud nepoužíváte je, je nutné je odstranit ručně.

- Snímky cloudu vytvořené pro virtuální zařízení se zachovají.

Podrobný postup najdete v tématu [deaktivovat a odstranění zařízení StorSimple](storsimple-deactivate-and-delete-device.md).

Hned virtuální zařízení se zobrazuje, jak deaktivovat na stránce Správce StorSimple služby, můžete odstranit virtuální zařízení ze seznamu zařízení na stránce **zařízení** .


### <a name="start-stop-and-restart-a-virtual-device"></a>Spustit, zastavit a restartujte virtuální zařízení
Na rozdíl od fyzické zařízení StorSimple je žádné power zapnuto nebo výkonu vypnout nabízená na zařízení virtuální StorSimple s cílem. Však může existovat příležitosti místo, kam potřebujete zastavení a nové spuštění virtuální zařízení. Například některé aktualizace může vyžadovat, aby OM restartovat dokončete proces aktualizace. Nejjednodušším způsobem můžete začít, ukončete a restartujte virtuální zařízení je používat konzolu Správa virtuálních počítačích.

Když se podíváte na konzole pro správu, virtuální zařízení stav je **spuštěný** vzhledem k tomu, že je spuštěn ve výchozím nastavení je vytvořený. Můžete spustit, zastavit a restartujte počítač virtuální kdykoli.

[AZURE.INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Obnovit výchozí nastavení

Pokud se rozhodnete, že chcete začít s virtuální zařízení, jednoduše deaktivujte a odstraňte ji a potom vytvořte novou. Stejně jako při je zařízení fyzicky obnovit nebude nové virtuální zařízení máte nainstalované; všechny aktualizace proto zkontrolujte, že zkontrolovat aktualizace: než ho začnete používat.


## <a name="fail-over-to-the-virtual-device"></a>Převzetí virtuální zařízení

Obnovení havárie (DR) je taková uvedené hlavní příklady situací navržené virtuální zařízení StorSimple pro. V tomto scénáři nemusí být k dispozici fyzické StorSimple zařízení nebo celý datacentra. Naštěstí můžete virtuální zařízení obnovení do jiného umístění. Během DR kontejnerů hlasitost ze zdrojového zařízení změnit vlastnictví a jsou převedeny na virtuální zařízení. Požadavky pro web DR jsou, že virtuální zařízení byly vytvořeny a nakonfigurovali, všechny svazky v kontejneru hlasitost jste byli přesměrováni offline a kontejneru hlasitost má přidruženou cloudu snímek.

>[AZURE.NOTE] 
>
> - Pokud chcete použít virtuální zařízení jako sekundární zařízení pro web DR, mějte na paměti, že 8010 má 30 TB úložiště ve standardní a 8020 64 TB úložiště ve Premium. Vyšší výkon 8020 virtuální zařízení může být více hodí pro situace DR.
> - Nelze převzetí nebo klonovat ze zařízení s aktualizace 2 zařízení 1 softwarem před aktualizace. Můžete však selhání zařízení spuštění aktualizace 2 na zařízení s aktualizace 1 (1.1 nebo 1.2)

Podrobný postup přejděte na [převezme virtuální zařízení](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).
 

## <a name="shut-down-or-delete-the-virtual-device"></a>Vypnutí nebo odstranění virtuální zařízení

Jestliže jste dříve nakonfigurované a použili StorSimple virtuální zařízení ale teď chcete přestat vyplývající výpočetním poplatky za jeho použití, můžete vypnout virtuální zařízení. Vypnutí virtuální zařízení neodstraníte jeho operačního systému nebo disků dat v úložišti. Ukončení poplatky vyplývající vašeho předplatného, ale úložiště poplatky za disků OS a data zůstanou.

Pokud odstraníte nebo vypnout virtuální zařízení, se zobrazí jako **Offline** na stránce zařízení StorSimple Správce služby. Je možné deaktivace nebo odstranění zařízení Pokud taky chcete odstranit zálohy vytvořil virtuální zařízení. Další informace najdete v tématu [deaktivovat a odstranit StorSimple zařízení](storsimple-deactivate-and-delete-device.md).

[AZURE.INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[AZURE.INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

   
## <a name="troubleshoot-internet-connectivity-errors"></a>Poradce při potížích připojení k Internetu 

Při vytváření virtuálních zařízení Pokud je bez připojení k Internetu, krok vytvoření se nezdaří. Pokud chcete odstranit, pokud selže kvůli připojení k Internetu, proveďte následující kroky Azure klasické portálu:

1. Vytvoření systému Windows server 2012 virtuálního počítače v Azure. Tento virtuální počítač používejte stejný účet úložiště, VNet a podsítě používaného virtuální zařízení. Pokud už máte existující hostitele systému Windows Server v Azure pomocí účtu úložiště, Vnet stejných podsítě, můžete taky ji použijete k řešení potíží s připojením k Internetu.
2. Remote log do virtuálního počítače vytvořili v předchozím kroku. 
3. Otevřete okno příkazového řádku do virtuálního počítače (Windows + R a začněte psát `cmd`).
4. Spusťte následující cmd příkazového řádku.

    `nslookup windows.net`

5. Pokud `nslookup` nepovede, pak selhání připojení k Internetu brání virtuální zařízení registrace ke službě StorSimple správce. 
6. Proveďte požadované změny virtuální sítě a ujistěte se, že je virtuální zařízení mít přístup k serverům Azure například "windows.net".

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [používat službu StorSimple správce pro správu virtuální zařízení](storsimple-manager-service-administration.md).
 
- Porozumět tomu, jak [Obnovit StorSimple hlasitost ze záložní sady](storsimple-restore-from-backup-set.md). 

