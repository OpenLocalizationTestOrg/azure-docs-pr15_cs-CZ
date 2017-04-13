<properties 
   pageTitle="Doporučené postupy pro virtuální pole StorSimple | Microsoft Azure"
   description="Popisuje doporučené postupy pro nasazení a správu StorSimple virtuální matice."
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
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-best-practices"></a>Virtuální pole StorSimple doporučené postupy

## <a name="overview"></a>Základní informace

Microsoft Azure StorSimple virtuální matice je integrované úložiště řešení, které spravuje úložiště úkolů mezi virtuální zařízení s místním systémem hypervisoru a Microsoft Azure cloudového úložiště. Virtuální pole StorSimple je efektivně, efektivní alternativy pole fyzicky 8000 řady. Virtuální pole mohlo by umožnit spuštění na stávající infrastrukturu hypervisoru, podporuje iSCSI a protokolů pro plány SMB a je vhodné scénáře office remote office/větev. Další informace o řešení StorSimple přejděte na [Microsoft Azure StorSimple přehled](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Tento článek popisuje doporučené postupy implementovaná během počáteční instalaci, nasazení a správu StorSimple virtuální matice. Tyto doporučené postupy obsahují ověřený pokyny k nastavení a Správa virtuální pole. Tento článek je určený jenom ke správci IT, kteří nasazením a správou virtuální matice v jejich datacentrech.

Doporučujeme pravidelně seznámit s doporučené postupy pro zajistil, že zařízení stále probíhá dodržování předpisů při změně nastavení nebo operace tok. Má při narazíte na problémy implementaci tyto doporučené postupy pro své virtuální pole, aby vám [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) .

## <a name="configuration-best-practices"></a>Konfigurace doporučené postupy 

Tyto doporučené postupy najdete pokyny, které musí být zahájen během počáteční instalace a nasazení virtuální matic. Tyto doporučené postupy zahrnout možností týkajících se zřizování virtuálního počítače nastavení zásad skupiny pro změnu velikosti, nastavení sítí, konfigurace účtů úložiště a vytvoření sdílené složky a svazky virtuální pole. 

### <a name="provisioning"></a>Zřízení 

Serveru hostitele je argument matice virtuální StorSimple virtuálního počítače (OM) zřízení na hypervisor (Hyper-V nebo VMware). Když zřizujete virtuální počítač, zajistěte, aby byl váš hostitel moci určit dostatečné zdroje. Další informace najdete [minimální požadavky na zdroje](storsimple-ova-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements) zřízení matici. 

Implementace následující doporučené postupy při vytváření virtuálních pole:


|                        | Hyper-V                                                                                                                                        | VMware                                                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Typ virtuálního počítače**   | **Analytický nástroj Generátor 2** OM pro použití s Windows Server 2012 nebo novější a *.vhdx* obrázek. <br></br> **Analytický nástroj Generátor 1** OM pro použití systému Windows Server 2008 nebo novějším a *VHD* obrázek.                                                                                                              | Používejte virtuální počítač verzi 8-11 *.vmdk* obrázek.                                                                      |
| **Typ paměti**            | Konfigurace jako **statické paměti**. <br></br> Nepoužívejte možnost **dynamické paměti** .            |                                                    |
| **Datový typ disku**         | Zřízení jako **dynamicky rozbalení**.<br></br> **Pevnou velikost** trvá příliš dlouho. <br></br> Nepoužívejte možnost **derivací** .                                                                                                                   | Pomocí možnosti **tenké poskytování** .                                                                                      |
| **Změna disku dat** | Rozšíření nebo zmenšením není povolená. Pokus o k tomu nevyzve vede ke ztrátě místních dat na zařízení.                       | Rozšíření nebo zmenšením není povolená. Pokus o k tomu nevyzve vede ke ztrátě místních dat na zařízení. |

### <a name="sizing"></a>Pro změnu velikosti

Když pro změnu velikosti své StorSimple virtuální pole, uvažte následující faktory:

- Místní rezervace svazky nebo jejich počet. U místní osy u jednotlivých zřizování vrstvené hlasitost a sdílet je rezervován přibližně 12 % mezery. Přibližně 10 % mezery také vyhrazena svazku místně připojených k systému souborů.
- Nároky snímek. Zhruba 15 % místo na místní osy vyhrazená pro snímky.
- Nutnost obnoví. Pokud obnovení jako nová operace, by měl prostor potřebné pro obnovení účtu pro změnu velikosti. Obnovení je Hotovo do sdílené složky nebo objemu stejně velká nebo větší.
- Neočekávané růstu mají přidělit některé vyrovnávací paměť.

Podle předchozího faktory požadavky pro změnu velikosti představovat následující rovnice:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`


Následující příklady ukazují, jak můžete měnit velikost virtuální pole svým požadavkům.

#### <a name="example-1"></a>Příklad 1:
Na své virtuální pole chcete mít možnost 

- zřízení 2 TB vrstvené hlasitost nebo sdílet.
- zřízení 1 TB vrstvené hlasitost nebo sdílet.
- zřízení 300 GB místně připnuté hlasitost nebo sdílet.


Předchozí svazky nebo sdílené složky dejte nám vypočte požadavky na místo na místní osy. 

Nejdřív pro každou vrstvené hlasitost nebo sdílet místní rezervace by rovná 12 % velikosti hlasitost nebo sdílet. Místně připnuté hlasitost/sdílené položky místní rezervace je 10 % velikosti místně připnuté hlasitost nebo sdílet (kromě zřizování velikost). V tomto příkladu je třeba

- Místní rezervace 240 GB (2 TB vrstveny hlasitost nebo sdílet)
- Místní rezervace 120 GB (1 TB vrstveny hlasitost nebo sdílet)
- 330 GB pro místně připnuté hlasitost nebo do sdílené položky (přidání 10 % místní rezervace velikost 300 GB zřízení)

Celkové požadované místo na místní osy, pokud je: 240 GB + 120 GB + 330 GB = 690 GB.

Za druhé potřebujeme aspoň neodeberete místo na místní osy jako největší jednoho rezervace. Tato navíc částka slouží v případě, že je potřeba obnovit z cloudu snímek. V tomto příkladu je největší místní rezervace 330 GB (včetně rezervace systému souborů), takže byste přidali, že 690 GB: 690 GB + 330 GB = 1020 GB.
Pokud nám provést následující další obnoví, jsme vždy uvolnění místa z předchozí obnovení.

Třetí potřebujeme 15 % celkového místní prostoru zatím k uložení místní snímky, aby byla k dispozici pouze 85 %. V tomto příkladu by kolem 1020 GB = 0.85&ast;zřizování dat disk TB. Ano, bude disku zřizování dat (1020&ast;(1/0.85)) = 1 200 GB = 1,20 TB ~ 1,25 TB (zaokrouhlení na nejbližší kvartilu)

Řešení v neočekávané roste a nové obnoví, by měl zřizujete místní disk kolem 1,25-1,5 TB.

> [AZURE.NOTE] Doporučujeme také, že je na místním disku tence zřízení. Tato doporučení se vzhledem k tomu obnovení místa není potřeba pouze když chcete obnovit data, která je starší než 5 dní. Obnovení položky úrovni umožňuje obnovení dat pro posledních pět dnů bez nutnosti zvětšovat mezeru pro obnovení.

#### <a name="example-2"></a>Příklad 2: 
Na své virtuální pole chcete mít možnost 

- zřízení 2 TB vrstveny hlasitosti
- Vytvoření 300 GB místně připnuté svazku

Podle 12 % rezervace místním prostoru pro vrstvené svazky/sdílené položky a 10 % pro místně připnuté svazky/sdílené složky, potřebujeme

- Místní rezervace 240 GB (2 TB vrstveny hlasitost nebo sdílet)
- 330 GB pro místně připnuté hlasitost nebo do sdílené položky (přidání 10 % místní rezervace 300 GB zřízení mezery)

Je celková požadované místo na místní úroveň: 240 GB + 330 GB = 570 GB

Minimální místním prostoru potřebná k obnovení je 330 GB. 

15 % celkového disku slouží k uložení snímků tak, že je k dispozici pouze 0.85. Ano, je velikost disku (900&ast;(1/0.85)) = 1.06 TB ~ 1,25 TB (zaokrouhlení na nejbližší kvartilu) 

Řešení neočekávané nárůstu, můžete vytvořit místní disk 1,25-1,5 TB.


### <a name="group-policy"></a>Zásady skupiny

Zásady skupiny je infrastrukturu, která umožňuje implementaci konkrétní konfigurací uživatelů a počítačů. Nastavení zásad skupiny jsou součástí zásad skupiny objektů, které jsou propojené na následující kontejnery Active Directory Domain Services (AD DS): weby, domén nebo organizačních jednotek (OU). 

Pokud je vaše virtuální pole doméně, objekty zásad skupiny lze použít k němu. Tyto objekty zásad skupiny můžete nainstalovat aplikací, jako je antivirový software, které mohou nepříznivě ovlivnit operace virtuální pole StorSimple.

Proto doporučujeme, aby se:

-   Zajistěte, aby byl virtuální pole ve své vlastní organizační jednotce (OU) služby Active Directory. 

-   Ujistěte se, že žádný objekty zásad skupiny (objekty zásad skupiny), použijí se virtuální matici. Můžete blokovat dědičnosti zajistit, že virtuální matice (podřízeného uzlu) nepřejímá automaticky všechny objekty zásad skupiny z nadřazeného webu. Další informace přejděte na [Blokování dědičnosti](https://technet.microsoft.com/library/cc731076.aspx).


### <a name="networking"></a>Sítě

Konfigurace sítě pro virtuální pole probíhá prostřednictvím místní web uživatelského rozhraní. Virtuální síťové aktivované prostřednictvím hypervisoru, ve kterém máte k dispozici virtuální matice. Na stránce [Nastavení sítě](storsimple-ova-deploy3-fs-setup.md) pro nastavení virtuální sítě rozhraní IP adresu, podsítě a brány.  Můžete taky nakonfigurovat primárních a sekundárních DNS server, nastavení času a nastavení volitelné proxy pro svoje zařízení. Většina konfiguraci sítě je jednorázové instalace. Projděte si [sítě požadavky StorSimple](storsimple-ova-system-requirements.md#networking-requirements) před nasazením virtuální matice.

Když nasadíte virtuální matice, doporučujeme použijte tyto doporučené postupy:

-   Zkontrolujte, že v síti, ve kterém je virtuální matice nasazený vždy nainstalovaný kapacita zajistíte 5 šířky pásma Internetu MB / (a víc). 

    -   Potřebujete šířky pásma Internetu se liší v závislosti na vlastnosti pracovního vytížení a sazbu dat změnit.

    -   Změna data, která máte je přímo odpovídat šířky pásma Internetu. Jako příklad při přijímání zálohy vejdou 5 šířky pásma MB / ke změně dat asi 18 GB 8 hodin. S čtyřikrát větší šířku pásma (20 MB /) můžete zpracovat čtyřikrát další změnu dat (72 GB). 

-   Zajistěte, aby vždy je k dispozici připojení k Internetu. Připojení k Internetu Občasná nebo nedůvěryhodných zařízení může mít za následek ztrátu přístup k datům v cloudu a může mít za následek Nepodporovaná konfigurace.

-   Pokud plánujete nasazení zařízení jako iSCSI server: 
    -   Doporučujeme vypnout možnost **získat IP adresu automaticky** (DHCP). 
    -   Konfigurace statické IP adres. Musíte nakonfigurovat primární a sekundární server DNS.

    -   Pokud definující více síťových rozhraní na vaše virtuální pole, pouze první rozhraní sítě (ve výchozím nastavení je **Ethernet**rozhraní) zobrazíte v cloudu. Určit typ přenosu, můžete vytvořit více virtuální sítě rozhraní na své virtuální pole (nakonfigurování jako iSCSI serveru) a připojit těchto rozhraní na různé podsítě.

-   Omezení šířky pásma cloudu pouze (používané virtuální matice), konfigurace omezení na směrovači nebo bránu firewall. Když definujete omezení ve vaší hypervisoru, bude omezení všechny protokoly včetně iSCSI SMB místo jenom šířku pásma cloudu. 

-   Je povolena hypervisory zajistit, aby tuto dobu synchronizace. Pokud pomocí Hyper-V, vyberte virtuální pole ve Správci Hyper-V, přejděte na **Nastavení &gt; Integration Services**a ujistěte se, že je zaškrtnuté políčko **synchronizace času** .

### <a name="storage-accounts"></a>Účty úložiště

Virtuální pole StorSimple může být přidružené k účtu jednoho úložiště. Tento účet úložiště může být účtu automaticky generované úložiště účet v rámci stejného předplatného jako službu nebo účet úložiště souvisejících s jiné předplatné. Další informace najdete v tématu Správa [úložiště účty pro virtuální pole](storsimple-ova-manage-storage-accounts.md).

Použijte následující doporučení pro účty úložiště přidružené virtuální pole.

-   Při propojování více virtuální matice pod svým účtem ukládání jediné, faktor maximální kapacita (64 TB) pro virtuální matice a maximální velikosti (500 TB) účtu úložiště. To omezuje počet virtuální matice plné velikosti, které mohou být přidružené k tomuto účtu úložiště přibližně 7.

-   Při vytváření nového účtu úložiště
    -   Doporučujeme, aby ho vytvoříte v oblasti nejblíže k vzdálené office/pobočkové kde své StorSimple virtuální pole používaný minimalizovat čekacích dob.

    -   Mít na paměti, že nelze přesunout účet úložiště mezi různými oblastmi. Také nelze přesunout do služby u předplatných.

    -   Použití účtu úložiště implementující redundance mezi datacentrech. GEO přebytečné úložiště (GRS), zóny nadbytečné úložiště (ZRS) a místně nadbytečné úložiště (LRS) podporují pro použití s virtuální pole. Další informace o různých typů účtů úložiště přejděte na [replikace Azure úložiště](../storage/storage-redundancy.md).


### <a name="shares-and-volumes"></a>Sdílené položky a svazky

Pro pole virtuální StorSimple můžete zřizujete akcie po nakonfigurování jako souborovém serveru a svazky po nakonfigurování jako iSCSI serveru. Doporučené postupy pro vytváření sdílené položky a svazky souvisejí velikost a zadejte nakonfigurované.

#### <a name="volumeshare-size"></a>Velikost hlasitost nebo sdílet

Virtuální matici můžete zřizujete akcie po nakonfigurování jako souborovém serveru a svazky po nakonfigurování jako iSCSI serveru. Doporučené postupy pro vytváření sdílené položky a svazky týkající se velikost a zadejte nakonfigurované. 

Mějte na paměti následující doporučené postupy při zřizování složky sdílené složky nebo svazky na virtuální zařízení.

-   Velikost souboru relativní velikost zřizování vrstvené sdílené složky můžete mít vliv na výkon tiering. Práce s velkých souborů může vést k pomalé osy se. Při práci s velkých souborů, doporučujeme, aby největší soubor je menší než 3 % velikosti sdílet.

-   Než 16 svazky/sdílené složky se dají vytvořit virtuální matici. Pokud místně připnuté svazky sdílené položky lze mezi 50 GB až 2 TB. Pokud vrstveny, svazky/akcie musí být mezi 500 GB 20 TB. 

-   Při vytváření objem, faktor spotřebu očekávaná data, jakož i budoucí růst. Během hlasitost nelze rozbalit později, můžete kdykoli obnovit větší hlasitost.

-   Po vytvoření hlasitost nelze zmenšit velikost hlasitost na StorSimple.
   
-   Při psaní vrstvené objem na StorSimple, data hlasitost dosažení určitou prahovou hodnotu (relativní místní místo vyhrazené pro hlasitost), je omezený vstupu a výstupu. Pokračovat v psaní k tomuto svazku zpomaluje vstupu a výstupu výrazně. Přestože můžete zapisovat do vrstvené hlasitost nad jeho zřizování kapacitu (jsme není stop aktivně uživateli psaní nad zřizování kapacitu), zobrazí oznámení o tom, které mají oversubscribed. Jakmile se zobrazí oznámení, je naprosto hlasitost data jsou získána nápravné míry například odstranit nebo obnovit hlasitost větší objem (hlasitost rozšíření aktuálně nepodporuje).

-   Pro případy použití obnovení havárie jako počet povolenou akcie/svazky je 16 a maximální počet akcie/svazky zpracovaných souběžně také 16 počet akcie/svazky nemá vliv na operace RPO a RTOs. 

#### <a name="volumeshare-type"></a>Typ hlasitost nebo sdílení

StorSimple podporuje dva typy hlasitost nebo sdílet založen na používání: místně připnuté a vrstveny. Místně připnuté svazky/akcie jsou thickly zřízení že vrstvené svazky/akcie jsou tence zřízení. 

Doporučujeme provádět následující doporučené postupy při konfiguraci StorSimple svazky/sdílené složky:

-   Určení typu hlasitost podle úloh, které chcete nasadit před vytvořením svazku. Použití místně připnuté svazky úloh, které vyžadují místní záruky dat (i během výpadku cloud) a čekacích dob zhoršeným cloudu, které vyžadují. Po vytvoření svazku na virtuální pole nelze změnit typ hlasitost z místně připnuté na vrstvené nebo *naopak*. Jako příklad místně připnuté svazky vytvořte při nasazení SQL úloh nebo pracovního vytížení hostingu virtuálních počítačích (VMs); Použijte vrstvené svazky pro úloh sdílet soubor.

-   Zaškrtněte možnost pro méně častých archivace data týkají s velké soubory. Větší deduplication bloku o velikosti 512 kB se používá při zapnuté funkci tuto možnost můžete urychlit přenos dat do cloudu.

#### <a name="volume-format"></a>Formát hlasitosti

Po vytvoření StorSimple svazky na serveru iSCSI potřebujete inicializace, připojení a formátovat svazky. Tento operace na hostiteli připojené k StorSimple zařízení. Při připojování a formátování svazky na hostiteli StorSimple, doporučuje se následující doporučené postupy.

-   Rychlé formátování proveďte všechny svazky StorSimple.

-   Při formátování StorSimple hlasitost, používejte přidělení velikost jednotky (Austrálie) 64 KB (výchozí hodnota je 4 KB). 64 KB Austrálie podle testování sami pro běžné StorSimple pracovního vytížení a jiných úloh.

-   Při použití virtuální pole StorSimple nakonfigurování jako serveru iSCSI, nepoužívejte rozložené svazky nebo dynamických disků jako tyto svazky nebo disků nepodporuje StorSimple.

#### <a name="share-access"></a>Sdílení aplikace access

Při vytváření sdílené složky na souborovém serveru virtuální matice, postupujte podle následujících pokynů:

-   Při vytváření sdílené, přiřaďte skupinu uživatelů jako správce sdílet místo jednoho uživatele.

-   Oprávnění NTFS můžete spravovat po vytvoření sdílet úpravou sdílené složky pomocí Průzkumníka Windows.

#### <a name="volume-access"></a>Přístup ke svazku

Při konfiguraci svazky iSCSI na své StorSimple virtuální pole, je důležité řízení přístupu kdykoli je to nutné. Které hostitele servery pro přístup k svazky, vytvořit a přidružit StorSimple svazky záznamů kontrol přístup (ACRs).

Při konfiguraci ACRs pro StorSimple svazky, použijte následující doporučené postupy:

-   Nejméně jeden ACR vždy přidružit svazku.

-   Definování více ACRs pouze v prostředí skupinový.

-   K přiřazení víc ACR svazku, ujistěte se, že hlasitost, nebude vystaven způsobem, kde můžete souběžně přístup podle víc než jednoho hostitele neseskupené. Pokud jste přiřadili víc ACRs svazku, zpráva s upozorněním uvidí ke kontrole konfiguraci.

### <a name="data-security-and-encryption"></a>Zabezpečení dat a šifrování

Virtuální pole vaše StorSimple má funkce zabezpečení a šifrování dat, které zajišťují utajení a integrity dat. Pokud chcete použít tyto funkce, je vhodné použijte tyto doporučené postupy: 

-   Definujte cloudové úložiště šifrovací klíč pro generování šifrování AES 256 data se odesílá z virtuální pole do cloudu. Tento klíč není povinný, pokud data musí být zašifrovaný na začátku. Klíč můžete být generovaného a bezpečné použití systému správy klíčů například [Azure klíčové trezoru](../key-vault/key-vault-whatis.md).

-   Při konfiguraci účtu úložiště pomocí službu StorSimple správce, ujistěte se, povolit režim SSL k vytvoření zabezpečené kanálu pro síťová komunikace mezi StorSimple zařízení a cloudu.

-   Obnovit klávesy pro účty úložiště (přístupem službu Azure úložiště) pravidelně účtu změny pro přístup k založené na seznamu změněné správců.

-   Údaje o virtuální matice je komprimované a deduplicated před odesláním Azure. Nedoporučujeme pomocí služby role Deduplication dat na hostitele systému Windows Server.


## <a name="operational-best-practices"></a>Provozní doporučené postupy

Provozní doporučené postupy jsou pravidla, která by měla být zahájen během operace virtuální pole či každodenní řízení. Tyto postupy zahrnovat úkoly správy konkrétních například přijímat zálohování, obnovení ze zálohy sady provádění selhání, deaktivace nebo odstranění matice, sledování systém využití a stavu a systémem antivirová kontroluje virtuální matici.

### <a name="backups"></a>Zálohování

Údaje o virtuální pole zálohovala do cloudu na dva způsoby, výchozí automatické denní zálohování celé zařízení začínají na 22:30 nebo prostřednictvím ruční zálohování na vyžádání. Ve výchozím nastavení zařízení automaticky vytvoří denní snímky cloudu všech dat umístěných na něj. Další informace přejděte k [obecnějším údajům své StorSimple virtuální pole](storsimple-ova-backup.md).

Četnost a uchovávání informací přidružený k zálohování výchozí nemohli měnit, ale můžete nakonfigurovat čas jakou denní zálohování zahájí každý den. Při konfiguraci čas spuštění automatické zálohy, doporučujeme, aby:

-   Naplánujte zálohování největšího. Zálohování počáteční čas neměli shoduje s mnoha hostitele vstupů/výstupů.

-   Spusťte ruční zálohování na vyžádání při plánování provádět zařízení převzetí nebo předchozího okna údržbu k ochraně dat virtuální matici.

### <a name="restore"></a>Obnovení

Můžete obnovení ze zálohy nastavit dvěma způsoby: obnovení do jiného hlasitost nebo sdílet nebo obnovení úrovni položek (dostupné jenom u virtuální pole nakonfigurování jako souborovém serveru). Obnovení položky úrovni umožňuje provádět podrobného obnovení souborů a složek ze zálohy cloudu všechny sdílené složky na StorSimple zařízení. Další informace najdete v tématu [obnovení ze zálohy](storsimple-ova-restore.md).

Při provádění obnovení, mějte na paměti následující pravidla:

-   Virtuální pole vaše StorSimple nepodporuje obnovení na místě. Může to však být snadno dosáhnout dvou krocích: přidáte místo na virtuální pole a pak obnovíte do jiného hlasitost nebo sdílet.

-   Po obnovení z místního svazku, mějte na paměti obnovení budou dlouhodobé operace. Když hlasitost může rychle přechodu do online data nadále HYDRATOVANÝ na pozadí.

-   Typ svazku nezmění během obnovení. Vrstvené hlasitost se obnoví do jiného vrstvené svazku a místně připnuté hlasitost do jiného místně připnuté hlasitost.

-   Při pokusu o obnovení ze zálohy svazku nebo sdílené když úlohy obnovení nepovede, nastavte hlasitost cílové nebo sdílet můžou být pořád vytvoření na portálu. Je důležité odstranit tento nepoužitý cílové hlasitost nebo sdílet na portálu minimalizovat všechny budoucí problémy vyplývající z tohoto prvku.

### <a name="failover-and-disaster-recovery"></a>Převzetí a havárie obnovení

Přepojení zařízení umožňuje přenést data ze *zdroje* zařízení v datacentra na jiné zařízení *cílové* umístěné ve stejném nebo jiném geografické polohy. Převzetí zařízení je určený pro celé zařízení. Při selhání data cloudu pro zařízení zdroj změní vlastnictví, cílového zařízení.

Pro pole virtuální StorSimple můžete se jenom převzetí k jinému virtuální poli spravuje službu stejné StorSimple správce. Není povoleno přepojení zařízení 8000 řady nebo matici spravuje jiné službě StorSimple správce (než zdrojového zařízení). Další informace o aspektech překlopení, přejděte na [požadavcích na převzetí zařízení](storsimple-ova-failover-dr.md).

Při provádění selhání přes virtuální pole, mějte na paměti následující:

-   Plánované selhání je doporučený osvědčený postup umožní všem svazky/akcie offline před zahájením záložní. Postupujte podle pokynů specifické pro operační systém svazky/sdílené složky v offline režimu na hostiteli první a proveďte můžou být offline v virtuální zařízení.

-   Na serveru havárie obnovení souborů (DR) doporučujeme spojte cílové zařízení stejné domény jako zdroj, aby se automaticky přeložena oprávnění ke sdílení. V této verzi je podporována pouze přepnutí do cílové zařízení ve stejné domény.

-   Po úspěšném dokončení DR zdrojového zařízení se automaticky odstraní. V případě, že zařízení už není dostupná, virtuální počítač, který zřízení na hostitelském systému stále spotřebovává zdroje. Doporučujeme odstranit tento virtuální počítač ze systému host jakékoli poplatky zabráníte vyplývající.

-   Všimněte si, že i když záložní neproběhne úspěšně, **data jsou vždy bezpečí v cloudu**. Zvažte tyto tři scénáře, ve kterých záložní nebylo dokončeno:

    -   V počáteční fázi převzetí například kdy se před kontroly DR provádí došlo k chybě. V tomto případě je pořád použitelné cílové zařízení. Převzetí na stejné zařízení cílové můžete zopakovat.

    -   Během procesu skutečné převzetí došlo k chybě. V tomto případě je označen cílové zařízení nelze použít. Musí zřizovat a konfigurace jiné cílové virtuální pole a použít pro překlopení.

    -   Záložní byl dokončen, po které odstranil zařízení zdroje, ale cílové zařízení má problémy a nemáte přístup ke všechna data. Data sice dál v cloudu a můžete snadno získat vytvořením jiného virtuální pole a potom s ním pracovat jako cílové zařízení pro DR.

### <a name="deactivate"></a>Deaktivace

Po deaktivaci virtuální pole StorSimple server připojení mezi zařízení a odpovídající StorSimple Správce služby. Deaktivace je **trvalé** a nelze je vrátit zpět. Deaktivovaný zařízení nelze registrovaný u službu StorSimple správce znovu. Další informace klikněte na [deaktivovat a odstranit své StorSimple virtuální pole](storsimple-deactivate-and-delete-device.md).

Po deaktivaci virtuální pole, mějte na paměti následující doporučené postupy:

-   Snímek cloudu všechna data před deaktivace virtuální zařízení. Po deaktivaci virtuální pole všechna data místním zařízením bude ztracen. Pořizování snímku cloudu vám umožní obnovení dat později.

-   Před deaktivovat StorSimple virtuální pole zkontrolujte, zastavit nebo odstranění klienty a hosts, které jsou závislé na toto zařízení.

-   Odstranění deaktivovaný zařízení, pokud už používáte tak, aby neměl nabíhání nákladů.

### <a name="monitoring"></a>Sledování

Zajistěte, aby byl váš StorSimple virtuální pole v souvislé správný stavu, musíte sledovat matice a ujistěte se, získáte informace ze systému včetně upozornění. Ke sledování celkové virtuálních matice, provádět následující doporučené postupy:

- Konfigurace sledování ke sledování využití disku disku virtuální pole data a také OS disku. Pokud pro Hyper-V, slouží ke sledování virtualizace hosts kombinace System Center virtuálního počítače správce (SCVMM) a System Center operace správce (SCOM).   

- Nastavení e-mailová oznámení na virtuální pole Odeslat upozornění na určité úrovni použití.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Index vyhledávání a viry skenování aplikací

Virtuální pole StorSimple můžete automaticky úroveň data z místního osy do cloudu společnosti Microsoft Azure. Až aplikaci třeba indexu vyhledávání nebo virů slouží k prohledávání dat uložených na StorSimple, bude potřeba starat, že data cloudu neobsahuje získat přístup a doplněné zpátky do místního osy.

Doporučujeme provádět následující doporučené postupy při konfiguraci indexu vyhledávání nebo antivirová vyhledávání na virtuální pole:

-   Zakážete všechny operace automaticky konfigurované úplnou kontrolu.

-   Vrstvené svazky konfigurace indexu vyhledávání nebo antivirová prohledávání aplikace provést přírůstková naskenovaný obrázek. To by skenování pouze na nová data pravděpodobně umístěných na místní osy. Během operace přírůstková není přístupný data, která je vrstveny do cloudu.

-   Nakonfigurujte filtry správného vyhledávání a nastavení tak, aby získat naskenovaný určené typy souborů. Například, obrázek (JPEG, GIF a TIFF) a konstrukce výkresů neměli naskenovaný během opětovného vytvoření přírůstková nebo celé indexu.

Pokud používáte Windows indexování proces, postupujte podle následujících pokynů:

-   Nepoužívejte indexování systému Windows pro vrstvené svazky jako vyvolá velké objemy dat (TBs) z cloudu, pokud je potřeba znovu sestaví často index. Opětovné vytvoření indexu by načíst všechny typy souborů pro index obsahu.

-   Použití funkcí Windows indexování proces pro místně připnuté svazky jako tím byste měli jenom přístup k datům na místní úrovně můžete vytvořit index (cloud data nebudou k nim získat přístup).

### <a name="byte-range-locking"></a>Uzamčení oblast bajt

Aplikace můžete uzamknout dané oblasti bajtů v souborech. Pokud aplikace, které jsou zápisu do svého StorSimple aktivované uzamčení bajt oblasti, pak vrstvení nefunguje virtuální matici. Vrstvení pracovat všechny oblasti souborů k nim získat přístup třeba odemknout. Uzamčení oblast bajt nepodporuje vrstvené svazky virtuální matici.

Doporučené opatření bajt oblast uzamčení patří:

-   Vypněte rozsah bajtů uzamčení logiky aplikace.

-   Použití místně připnuté svazky (ne vrstveny) pro údaje související s touto aplikací. Místně připnuté svazky není úroveň do cloudu.

-   Při použití místně připnuté svazky s bajt oblast uzamčení povoleno, hlasitost můžete přechodu do online před dokončením na obnovit. V těchto případech musí čekat na obnovit úplnou.

## <a name="multiple-arrays"></a>Více polí

Více virtuální matice může být nutné být nasazené na účtu v rostoucí pracovní množiny dat, která může přepadového do cloudu, tedy vliv na výkon zařízení. V těchto případech je vhodné měřítko zařízení narůstající velikostí pracovní sada. Tento postup vyžaduje jeden nebo více zařízeních má být přidán v datovém centru místní. Při přidávání zařízení, můžete:

-   Rozdělení aktuální sadu dat.
-   Nasazení nového pracovního vytížení nové nahlas.
-   Pokud nasazení více virtuální matice, doporučujeme, aby ze služby Vyrovnávání zatížení pohledu distribuování matice v různých hypervisoru tabulkami hosts.

-  Více virtuální matice (po nakonfigurování jako souborový server nebo iSCSI serveru) můžete nasazenou v Distributed Namespace systému souborů. Podrobný postup přejděte na [Rozdělením soubor systémové Namespace řešení s Průvodce nasazením hybridní cloudové úložiště](https://www.microsoft.com/download/details.aspx?id=45507). Distributed File System Replication se nedoporučuje, aktuálně pro použití s virtuální matice. 


## <a name="see-also"></a>Viz taky
Zjistěte, jak [Spravovat své StorSimple virtuální pole](storsimple-ova-manager-service-administration.md) prostřednictvím služby StorSimple správce.
