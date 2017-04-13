<properties
    pageTitle="Replikace místní VMware virtuálních počítačích nebo pole fyzicky servery na vedlejší Web | Microsoft Azure"
    description="Replikace VMware VMs nebo Windows/Linux fyzické servery na vedlejší webu s obnovení webu Azure pomocí tohoto článku."
    services="site-recovery"
    documentationCenter=""
    authors="nsoneji"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="nisoneji"/>


# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Replikovat místní VMware virtuálních počítačích nebo pole fyzicky serverů sekundární Web


## <a name="overview"></a>Základní informace

InMage Scout v Azure obnovení webu poskytuje v reálném čase replikace mezi místním VMware weby. InMage Scout je součástí předplatného služby Azure obnovení webu.


## <a name="prerequisites"></a>Zjistit předpoklady pro

**Účet Azure**:, musíte mít účet [Microsoft Azure](https://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny.


## <a name="step-1-create-a-vault"></a>Krok 1: Vytvoření trezoru
1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Klikněte na nový > Správa > Zálohování a obnovení webu (OMS). Můžete taky, klikněte na Procházet > obnovení služby trezoru > Přidat.
3. Do pole **název** zadejte popisný název k identifikaci trezoru. Pokud máte víc předplatných, vyberte jednu z nich.
4. Vytvoření nové skupiny prostředků v **pole Skupina zdroje** nebo vyberte stávající. Zadejte Azure oblast dokončete požadovaná pole. 
5.  Do pole **umístění**vyberte zeměpisná oblast pro trezoru. Kontrola podporovaných oblastí, najdete v článku [Azure webu obnovení ceny](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Pokud chcete rychle přístup trezoru na řídicím panelu klikněte na připnout na řídicím panelu a pak klikněte na vytvořit.
6. Nové trezoru se zobrazí na řídicí panel > všechny zdroje a na hlavním služby Recovery skříňky zásuvné.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Krok 2: Nakonfigurujte trezoru a stáhnout součásti InMage Scout
7. Ve službě obnovení trezorů zásuvné vyberte trezoru a klikněte na nastavení.
8. V **dialogovém okně Nastavení** > **Začínáme** klikněte na **Obnovení webu** > Krok 1: **Příprava infrastruktury** > **cíl zámek**.
9. V **cíli ochrany** vyberte obnovení web a vyberte Ano, VMware vSphere hypervisoru. Klikněte na OK.
10. V dialogu **Nastavení Scout**klepněte na stáhnout pro stažení InMage Scout 8.0.1 GA software a registrace klíč. Soubory nastavení pro všechny požadované součásti jsou v souboru stažený ZIP.


## <a name="step-3-install-component-updates"></a>Krok 3: Aktualizace součásti

Přečtěte si informace o nejnovějších [aktualizacích](#updates). Aktualizace souborů na serverech budete instalovat v následujícím pořadí:

1. PŘÍJEM serveru, pokud existuje
2. Konfigurace serverů
3. Servery obrázku
3. Hlavní cílových serverů
4. servery vContinuum
5. Zdrojový server (Windows a Linux Server)

Nainstalujte aktualizace následujícím způsobem:

1. [Aktualizace](https://aka.ms/asr-scout-update4) ZIP budou moct soubor stáhněte. Tento soubor ZIP obsahuje následující soubory:

    - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz
    - CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
    - UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
    - UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
    - vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
    - Počet bitů UA update4 RHEL5 OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz 
    
2. Extrahujte soubory ZIP.<br>
3. **Pro příjem serveru**: Kopírovat **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** příjmu server a extrahujte ji. Ve složce extrahované spusťte **a nainstalujte**.<br>
4. **Konfigurace serveru/proces serveru**: Kopírovat **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** konfigurační server a proces serveru. Dvojitým kliknutím ho spusťte.<br>
5. **Pro systém Windows předlohy na cílový**: aktualizujte jednotné agent, zkopírujte **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** předlohy cílový server. Dvojím kliknutím ho spusťte. Všimněte si, že jednotné agent také použít ke zdrojovému serveru. Nainstalujte ji na zdrojový server stejně, jak je uvedeno dále v tomto seznamu.<br>
7. **Pro vContinuum server**: zkopírovat **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** vContinuum server.  Ujistěte se, že nezavřete Průvodce vContinuum. Poklikejte na soubor ho spusťte.<br>
8. **Pro Linux předlohy cílový server**: aktualizovat jednotné agent, zkopírujte **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** předlohy cílový server a extrahujte ji. Ve složce extrahované spusťte **a nainstalujte**.<br>
9. **Zdroje pro Windows server**: Chcete-li aktualizovat jednotné agent, zkopírujte **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** ke zdrojovému serveru. Dvojím kliknutím ho spusťte.<br>
10. **Pro Linux zdrojovému serveru**: aktualizovat jednotné agent, zkopírujte odpovídající verze UA souboru na server Linux a extrahujte ji. Ve složce extrahované spusťte **a nainstalujte**.  Příklad: RHEL 6,7 64bitový serveru, zkopírujte **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** na server a extrahujte ji. Ve složce extrahované spusťte **a nainstalujte**.

## <a name="step-4-set-up-replication"></a>Krok 4: Nastavení replikace
1. Nastavte si replikace mezi zdroji a cílové weby VMware.
2. Pokyny použijte InMage Scout stažený s produktem. Můžete taky dostanete dokumentaci následujícím způsobem:

    - [Poznámky k verzi](https://aka.ms/asr-scout-release-notes)
    - [Funkce pro kompatibilitu matice](https://aka.ms/asr-scout-cm)
    - [Uživatelská příručka](https://aka.ms/asr-scout-user-guide)
    - [PŘÍJEM uživatelská příručka](https://aka.ms/asr-scout-rx-user-guide)
    - [Průvodce rychlou instalací](https://aka.ms/asr-scout-quick-install-guide)


## <a name="updates"></a>Aktualizace

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure webu obnovení Scout 8.0.1 aktualizace 4
Aktualizace Scout 4 je kumulativní aktualizace. Obsahuje všechny opravy update1 pokladny update3 a následující nové opravy chyb a vylepšení.

**Nové podpoře platformy** 

- Podpora přidala pro vCenter/vSphere 6.0, 6.1 a 6.2
- Podpora přidala pro následující operační systémy Linux
    - Červené klobouk Enterprise Linux (RHEL) 7.0, 7.1 a 7.2 
    - CentOS 7.0, 7.1 a 7.2
    - Červené klobouk Enterprise Linux (RHEL) 6.8
    - CentOS 6.8

>[AZURE.NOTE]
>
> RHEL/CentOS 7 64-bit **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** je součástí základní Scout GA balíčku **InMage_Scout_Standard_8.0.1 GA.zip**. Stáhněte Scout GA z portálu podle [Krok 1](site-recovery-vmware-to-vmware.md#Step 1: Create a vault).

**Opravy chyb a vylepšení** 

- Vylepšené vypnutí zpracování pro následující operační systémy Linux a klony předejít problémům s nežádoucí znovu synchronizujte.
    - Červené klobouk Enterprise Linux (RHEL) 6.x
    - Oracle Linux (OL) 6.x
- Linux proveďte přístup ke složce, které jsou oprávnění v adresáři instalace jednotné agent teď omezena pouze pro místní uživatele.
- Ve Windows vypršení časového limitu problém při velkém vydání běžné distribuované konzistence záložka na načíst distribuované aplikace, jako je clusterů SQL a sdílet bodu.
- Přidané protokolu související opravný nástroj Instalační služby základní CX.
- Odkaz na stažení vCLI 6.0 VMware přibude základní instalační služby systému Windows předlohy cílové.
- Přidat další kontroly a protokolů pro změny konfigurace sítě během převzetí a DR cvičení.
- Informace o některých případech uchovávání informací není nahlášený CX.  
- Pole fyzicky clusteru hlasitost změnit velikost operace Průvodce vContinuum selhává při zmenšení hlasitost zdroje se stalo.
- Ochranu shluk došlo k chybě "Se nepodařilo najít podpis disku" při clusteru je disk PRDM.
- cxps přenosová zhroucení serveru kvůli výjimce mimo rozsah. 
- Název serveru a IP sloupců jsou teď měnit na stránce instalace nabízených vContinuum průvodce.
- Vylepšení rozhraní API příjmu
    - Poskytuje pět nejnovější dostupné běžné konzistence body (pouze zaručena značky).
    - Poskytuje kapacitu a podrobnosti volného místa pro všechny chráněné zařízení.
    - Poskytuje Scout ovladač stav na zdrojový server. 
    
>[AZURE.NOTE] 
>
>- Základní sada **InMage_Scout_Standard_8.0.1_GA.zip** teď aktualizoval CX Instalační služby základní **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** a Windows předlohy cílové základní instalační program **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Pro všechny nové instalace použijte nové CX a cílové Windows předlohy GA bitů.
>- Aktualizace 4 lze použít přímo na 8.0.1 JM.
>- Konfigurační server a příjmu aktualizací nelze vrácení po se použije v systému.

### <a name="azure-site-recovery-scout-801-update-3"></a>Aktualizace Scout 8.0.1 obnovení Azure webu 3
3 přináší následující opravy chyb a vylepšení:

- Konfigurační server a příjmu nepovede zaregistrovat k obnovení webu trezoru při pozadu proxy server.
- Sestavy stavu začíná neaktualizují počet hodin, které není splněná cíle bodu obnovení (operace RPO).
- Konfigurace serveru se nesynchronizuje s příjmu při ESX hardwaru podrobnosti nebo nastavení sítě obsahovat žádné UTF-8 znaky.
- Řadiče domény Windows Server 2008 R2 nezdaří pro spuštění po obnovení.
- Offline synchronizace nefunguje očekávaným způsobem.
- Po virtuálního počítače (OM) převzetí replikace pár odstranění zasekne v uživatelském rozhraní CX poměrně dlouho a uživatelé nemohou dokončení navrácení nebo obnovení.
- Celkové snímek, který operacích, které se dělají konzistence úloha byla optimalizované objemu aplikace odpojí jako klientům SQL.
- Výkon nástroj konzistence (VACP.exe) jsme vylepšili jejich zmenšením spotřebu paměti, který je potřebný pro vytvářet snímky na Windows.
- Push instalovat služby havaruje, pokud heslo je větší než 16 znaků.
- vContinuum není kontrola a dotazování na nové přihlašovací údaje vCenter při změně pověření.
- Na Linux není správce mezipaměti předlohy cílové (cachemgr) stahování souborů ze serveru proces, jehož výsledkem je replikace pár omezení.
- Případě směru fyzické překlopení obrázku (MSCS) disku, který není ve všech uzlech stejné replikace není nastavená pro některé svazky clusteru.
<br/>Všimněte si, že clusteru musí být reprotected využít tento opravný nástroj.  
- Funkce SMTP nefunguje očekávaným po příjmu Scout 7.1 upgradovaný na verzi Scout 8.0.1.
- V protokolu vrácení operace přehled o čase, přijatých dokončit, protože se byly přidány další stat.
- Podpora přidala Linux operační systémy v počítačích na zdrojovém serveru:
    - Aktualizace červené klobouk Enterprise Linux (RHEL) 6 7
    - CentOS 6 aktualizace 7
- PŘÍJEM uživatelského prostředí a CX teď můžete zobrazit oznámení o pár přejde do režimu rastrový obrázek.
- Tyto opravy zabezpečení byly přidány do příjmu:

**Popis problému**|**Implementace postupy**
---|---
Vyhnout se tak mohli ověřovat pomocí parametrů falšování|Omezený přístup uživatelům bez příslušných.
Žádost o webů padělání|Implementovaná podobnosti stránky token náhodně generuje pro každou stránku. <br/>S tím zobrazí se: <li> Existuje pouze jeden přihlašovací instance pro jednoho uživatele.</li><li>Aktualizace nefunguje – vás přesměruje na řídicí panel.</li>
Odeslání škodlivým souboru|Omezené soubory do určité rozšíření. Rozšíření jsou povoleny: 7z, aiff, asf, avi, bmp, csv, dokumentu, docx, fla, flv, gif, gz, gzip, jpeg, jpg, protokolu část, mov mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, RT, ram, rar, sv, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, vkládání, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml a zip.
Trvalý skriptování mezi | Přidání vstupní ověření.


>[AZURE.NOTE]
>
>-  Všechny aktualizace obnovení webu jsou kumulativní. Aktualizace 3 má tyto opravy aktualizace 1 a 2 aktualizace. Aktualizace 3 lze použít přímo na 8.0.1 JM.
>-  Konfigurační server a příjmu aktualizací nelze vrátit zpět po se použije v systému.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure webu obnovení Scout 8.0.1 aktualizace 2 (03 aktualizovat prosince 15)

Opravy v aktualizaci 2, patří:

- **Konfigurace serveru**: řešení problému, který funkci bezplatné měření 31 dnech zabráněno pracovní podle očekávání, pokud byla konfigurační server registrována v obnovení webu.
- **Jednotné agent**: řešení problému v Update 1, a při aktualizaci zprávou, že Lync na hlavní cílové serveru nainstalovány při upgradu z verze 8.0 k 8.0.1.


### <a name="azure-site-recovery-scout-801-update-1"></a>Azure webu obnovení Scout 8.0.1 aktualizace 1

Aktualizace 1 zahrnuje tyto opravy chyb a nových funkcí:

- 31 dnech bezplatné ochrany za instance serveru. Tímto způsobem lze otestovat funkce nebo nastavení ověření koncepce.
    - Jsou všechny operace na serveru, včetně překlopení a překlopení zpět, bezplatné první 31 dny, počínaje údaj o čase, serveru se po zamknutí nejdřív s Scout obnovení webu.
    - Z 32nd den roku každý chráněné server strhne příslušná sazbou standardní instance obnovení webu Azure ochrana zákazníka vlastnictví webu.
    - Kdykoli je dostupná na stránky řídicího panelu trezoru obnovení webu Azure počet chráněné servery, které jsou aktuálně účtovat.
- Přidat podporu pro vSphere rozhraní příkazového řádku (vCLI) 5,5 aktualizace 2.
- Podpora přidali Linux operační systémy v počítačích na zdrojovém serveru:
    - RHEL 6 aktualizovat 6
    - RHEL 5 aktualizovat 11
    - Aktualizace centOS 6 6
    - Aktualizace centOS 5 11
- Opravy chyb při řešení těchto problémů:
    - Registrace trezoru selže konfigurační server nebo příjmu serveru.
    - Svazky clusteru nezobrazí podle očekávání, pokud jsou skupinový virtuálních počítačích reprotected při jejich obnovení.
    - Navrácení selhává, když server předlohy cílové nachází na různých serveru ESXi z místního výrobní virtuálních počítačích.
    - Konfigurační soubor oprávnění se změní při upgradu na 8.0.1, což ovlivňuje protection a operací.
    - Mezní hodnota synchronizace není vynucené podle očekávání, které vedou k replikace nekonzistentní chování.
    - Nastavení operace RPO nezobrazují správně v rozhraní konfigurace serveru. Hodnoty nekomprimované data nesprávně zobrazuje komprimované hodnoty.
    -  Operace odebrat neodstraníte podle očekávání v Průvodci vContinuum a replikace není odstraněna z rozhraní konfigurace serveru.
    -  V Průvodci vContinuum disk je automaticky při klepnutí na tlačítko **podrobností** v zobrazení disku při ochraně MSCS virtuálních počítačích.
    - Během fyzické do virtuálního scénář (P2V) nejsou požadované HP služby, jako jsou CIMnotify a CqMgHost, přesune do ruční obnovení virtuálního počítače. Výsledkem další spuštění.
    - Ochrana virtuálního počítače Linux selhává, když existuje více než 26 disků předlohy cílového serveru.

## <a name="next-steps"></a>Další kroky

Pokládat dotazy, které máte na [Fórum komunity služby Azure Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
