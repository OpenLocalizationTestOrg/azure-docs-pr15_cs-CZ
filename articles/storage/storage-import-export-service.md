<properties
    pageTitle="Použití import nebo export pro přenos dat k úložišti objektů Blob | Microsoft Azure"
    description="Naučte se vytvořit import a export úlohy v portálu pro přenos dat k úložišti objektů blob Azure klasické."
    authors="renashahmsft"
    manager="aungoo"
    editor="tysonn"
    services="storage"
    documentationCenter=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="renash"/>


# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>Pomocí služby Microsoft Azure Import nebo Export pro přenos dat k úložišti objektů Blob

## <a name="overview"></a>Základní informace

Služba Azure Import nebo Export umožňuje bezpečně přenášet velké objemy dat k úložišti objektů blob Azure odesíláním pevných discích Azure datacentrem. Můžete také tuto službu přenášet data z úložiště objektů blob Azure jednotky pevného disku a dodání na svůj místní web. Tato služba je vhodné v situacích, kdy chcete přenést několik TBs dat z Azure nebo, ale není možné kvůli omezené šířka pásma nebo Vysoká sítí náklady nahrávání nebo stahování v síti.

Služba vyžaduje, aby pevných discích by měl bit schránka zašifrovaných zabezpečení data. Služba podporuje klasické úložiště účty prezentovat ve všech oblastech veřejné Azure. Je třeba nabízet pevných discích na jeden z podporovaných umístění uvedené dále v tomto článku.

V tomto článku se další informace o službu Azure Import nebo Export a jak dodat jednotky ke kopírování dat do a z úložiště objektů Blob Azure.

> [AZURE.IMPORTANT] Můžete vytvořit a spravovat import a export úlohy pomocí portálu Classic nebo [Import nebo Export služba REST API](http://go.microsoft.com/fwlink/?LinkID=329099)– klasické úložiště. Správce prostředků úložiště účty nejsou v současné době podporované.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Kdy mám používat službu Azure Import nebo Export?

Můžete použít služby Azure Import nebo Export při nahrávání nebo stahování dat v síti je pomalý nebo získání dalších šířka pásma je náklady překážku.

Tuto službu v scénáře můžete například:

- Migraci dat do cloudu: Přesunutí velké objemy dat Azure rychle a efektivně nákladů.
- Rozdělení obsahu: rychle odeslat data k webům zákazníka.
- Zálohování: Prohlédněte zálohy místních dat ukládat v úložišti objektů blob Azure.
- Obnovení dat: obnovení velké množství dat uložených v úložišti objektů blob a nastavit doručení místního pracoviště.

## <a name="pre-requisites"></a>Předpoklady

Předpoklady nutné používat tuto službu jsme mít uvedené v této části. Zkontrolujte je pečlivě před dodací jednotky.

### <a name="storage-account"></a>Úložiště účtu

Musí mít stávajícímu předplatnému Azure a jeden nebo více **klasické** účtů úložiště používat službu Import nebo Export. Každý úlohu může použít pro přenos dat nebo z jen s jedním klientem klasické úložiště. Jinými slovy úlohy jednoho import nebo export nemůže zahrnovat u více účtů úložiště. Další informace o vytvoření nového účtu úložiště zjistěte, [jak vytvořit účet úložiště](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>Typy objektů BLOB

Zkopírujte data do objektů blob **blok** nebo **stránku** objektů BLOB můžete pomocí služby Azure Import nebo Export. Naopak lze exportovat pouze objektů blob **blok** , objekty BLOB **stránky** nebo objekty BLOB **Připojit** z Azure úložiště pomocí této služby.

### <a name="job"></a>Úlohy

Spusťte import nebo export z úložiště objektů Blob, nejdřív vytvořit úlohu. Úlohy může být úlohy importu nebo exportu úlohy:

- Vytvoření úlohy importu, když chcete přenést data, máte pro objekty BLOB místní ve vašem účtu Azure úložiště.
- Vytvoření projektu exportovat, když chcete přenést data aktuálně uložená jako objekty BLOB ve vašem účtu úložiště na pevných discích dodávané pro vás.

Při vytváření úlohy oznámit, že budete službu Import nebo Export, že se bude mít dodací jeden nebo více pevných discích Azure datacentrem.

- Import projektu bude dodací pevných discích obsahujícím data.
- Export projektu bude dodací prázdné pevných discích.
- Dodáte až 10 pevných discích na projekt.

Můžete vytvořit operaci importu nebo exportu úlohy pomocí [portálu Classic](https://manage.windowsazure.com/) nebo [Azure úložiště Import nebo Export REST API](http://go.microsoft.com/fwlink/?LinkID=329099).

### <a name="client-tool"></a>Nástroj klienta

Cílem prvního kroku při vytváření **import** projektu je připravit jednotku, která mají být dodání pro import. Příprava jednotky, musíte připojit k místní server a spustit nástroj Azure Import nebo Export klienta místního serveru. Tento nástroj klienta usnadňuje kopírování dat na jednotku, šifrování dat na disku pomocí nástroje BitLocker a vytvářet soubory deníku jednotky.

Deníku soubory obsahují základní informace o projektu a jednotky, jako je jednotka pořadové číslo a název účtu úložiště. Tento soubor deníku není uložena na disku. Používá se při vytváření úlohy importovat. Podrobnosti o vytváření úlohy krok za krokem jsou uvedeny dál v tomto článku.

Nástroj klienta je pouze kompatibilní se službou 64bitová verze operačního systému. V části [operační systém](#operating-system) pro určitý operační systém verze podporované.

Stáhněte si nejnovější verzi [nástroj klienta Azure Import nebo Export](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409). Podrobné informace o používání nástroje pro Import nebo Export Azure najdete v článku [Odkazy nástrojů Azure Import nebo Export](http://go.microsoft.com/fwlink/?LinkId=329032).

### <a name="hard-disk-drives"></a>Pevné jednotky

Pouze 3,5 SATA II/III interní pevných discích jsou podporovány pro použití se službou Import nebo Export. Můžete použít pevných discích až 10TB.
Pro importovat projekty budou zpracovány pouze první hlasitost dat na disku. Hlasitost dat zformátujte systémem.
Při kopírování dat na pevný disk, můžete ho přímo pomocí konektoru SATA připojit nebo ji externě pomocí externí adaptér SATA II/III USB připojit. Doporučujeme použít jednu z následujících externí adaptéry USB SATA II/III:

- Anker 68UPSATAA - 02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Sharkoon QuickPort XT HC

Pokud máte převaděče, které tady není uvedené, můžete zkusit spuštění nástroje pro Import nebo Export Azure pomocí svého převaděče pro přípravu jednotku a zjistěte, jestli funguje před nákupem převaděče podporované.

> [AZURE.IMPORTANT] Externí jednotky pevného disku, které jsou součástí předdefinované adaptér USB nepodporuje tuto službu. Navíc nelze použít disk uvnitř velikost písmen externí pevný disk. Pošlete nám prosím není externí pevných disků.

### <a name="encryption"></a>Šifrování

Údaje o změně jednotky musí šifrovaná pomocí nástroje BitLocker jednotka šifrování. To chrání data při přenosu.

Pro importovat projekty jsou dva způsoby, jak provádět šifrování. První tak, jak je používat / šifrovat parametr při spuštění nástroje pro klienta při přípravě jednotky. Druhý způsob je povolit šifrování nástroje BitLocker ručně na disku a zadání šifrovacího klíče do příkazového řádku nástroj klienta při přípravě jednotky.

Pro export úlohy po zkopírování dat do jednotky, služba bude jednotku zašifrovat pomocí nástroje BitLocker před dodací zpět. Šifrovací klíč bude pro vás poskytovat pomocí portálu klasické.  

### <a name="operating-system"></a>Operační systém

Použijte jeden z následující operační systémy 64-bit Příprava pevný disk pomocí nástroje pro Import nebo Export Azure před dodací jednotku, která má Azure:

Windows 7 podniku, Windows 7 Ultimate Windows 8 Pro Windows 8 Enterprise, Windows 8.1 Pro Windows 8.1 Enterprise Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Tyto operační systémy nepodporují šifrovacím nástroje BitLocker.

> [AZURE.IMPORTANT] <sup>1</sup> Pokud používáte Windows 10 počítače Příprava pevného disku, stáhněte si prosím nejnovější verzi nástroj Azure Import nebo Export.

### <a name="locations"></a>Umístění

Služba Azure Import nebo Export podporuje kopírování data mezi libovolnými všechny účty úložiště veřejného Azure. Dodáte pevných discích na jednu z následujících umístění. Pokud je váš účet úložiště ve veřejných Azure umístění, které tady není uvedené, alternativní dodávky umístění bude poskytována při vytváření úlohy pomocí portálu Classic nebo rozhraní REST API Import nebo Export.

Podporované dodávky umístění:

- Východní USA

- Západ USA

- Východní USA 2

- Centrální USA

- Severní centrální USA

- Jižní centrální USA

- Severní Evropě

- Západní Evropě

- Východní Asie

- Jihovýchodní Asie

- Austrálie východ

- Austrálie jihovýchodní

- Japonsko západní

- Japonsko východ

- Centrální Indie


### <a name="shipping"></a>Expedice

**Námořní vede k centru dat:**

Při vytváření import nebo export projektu, bude poskytována adresu dodávky jeden z podporovaných umístění dodat jednotky. Námořní adresu poskytnutou závisí na umístění účtu úložiště, ale nemusí být stejné jako svého účtu úložiště.

Můžete dopravců jako FedEx, DHL, UPS nebo poštovní služby nám dodat vašich jednotkách adresy příjemce.

**Námořní jednotky z centra dat:**

Při vytváření import nebo export projektu, je nutné zadat zpáteční adresu pro Microsoft používat, když dodací jednotky zpět po dokončení práce. Ujistěte se, že zadání platná zpáteční adresy do vyhnout zpožděním při zpracování.

Také je nutné zadat platné FedEx nebo DHL dopravce číslo určené společnost Microsoft dodací jednotky zpět účtu. Číslo účtu FedEx je potřebný pro dodací jednotky zpět z USA a Europe umístění. Číslo účtu DHL je potřebný pro dodací jednotky zpět z země Asie a Austrálie umístění. Pokud nemáte jeden můžete vytvořit [FedEx](http://www.fedex.com/us/oadr/) (pro USA a Evropa) nebo účtu dopravce [DHL](http://www.dhl.com/) (země Asie a Austrálie). Pokud už máte carrier číslo účtu, ověřte, zda je platný.

V dodávky balíčky, musí splňovat číslech [Podmínky služby Microsoft Azure](https://azure.microsoft.com/support/legal/services-terms/).

> [AZURE.IMPORTANT] Dejte pozor, aby fyzické média, která se muset křížové mezinárodní ohraničení. Zodpovídáte za zajistit, že fyzické média a data jsou importovat a/nebo exportovat v souladu s rozhodným právem. Před dodávky médium, obraťte se na poradci pro ověření, že média a dat můžete ze zákona dodání identifikované datovém centru. To vám pomůže zajistit, že nedosáhnete Microsoft včas. Například balíčku, který protínají mezinárodní ohraničení musí faktury spolu s balíček (kromě Pokud přes ohraničení v Evropské unii). Může vytisknout výplní kopii faktury z webu carrier. Příklad obchodní faktury jsou [DHL obchodní faktura] (http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) nebo [Obchodní faktura FedEx](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Ujistěte se, zda nebyla Microsoft označené jako vývozce.

## <a name="how-does-the-azure-importexport-service-work"></a>Fungování služby Azure Import nebo Export

Přenášet data mezi místních webů a úložišti objektů blob Azure pomocí služby Azure Import nebo Export vytvořením úlohy a dodávky pevných discích Azure datacentrem. Každého pevného disku, které doplníte souvisí s jednu úlohu. Jednotlivé úlohy je přidružené k účtu jednoho úložiště. Přečtěte si [část předpoklady](#pre-requisites) pečlivě se naučit používat konkrétní tento typy objektů blob služeb, jako podporované typy disku, umístění a dodávky.

V této části jsme popisují na vysoké úrovni kroky při importu a exportu úlohy. Dále v [části úvodní](#quick-start)poskytneme podrobné pokyny k vytvoření operaci importu a exportu projektu.

### <a name="inside-an-import-job"></a>Uvnitř úlohy importu

Import projektu na vysoké úrovni zahrnuje tyto kroky:

- Určete data, která chcete importovat a počet jednotek, které budete potřebovat.
- Označte cíl objektů blob pro vaše data v úložišti objektů Blob.
- Použijte nástroj Import nebo Export Azure a zkopírujte data do jednoho nebo více pevný diskových jednotek zašifrovat pomocí nástroje BitLocker.  
- Vytvoření úlohy importu ve vašem účtu klasické úložiště cílové pomocí portálu Classic nebo rozhraní REST API Import nebo Export. Pokud portálu klasické nahrajte soubory deníku jednotky.
- Zadejte zpáteční adresu a číslo účtu dopravce určené dodací jednotky zpět.
- Dodat jednotky pevného disku na adresu příjemce k dispozici při vytváření projektu.
- Aktualizujte doručení číslo v podrobnostech úlohy import sledování a odeslání úlohy importu.
- Jednotky jsou přijetí a zpracování uprostřed Azure data.
- Jednotky jsou odeslané pomocí svého účtu carrier na zpáteční adresu uvedenou v úlohy importu.

    ![Obrázek 1:Import úlohy toku](./media/storage-import-export-service/importjob.png)


### <a name="inside-an-export-job"></a>Ve exportovat projektu

Export projektu na vysoké úrovni zahrnuje tyto kroky:

- Zjistit, data, která chcete exportovat a počet jednotek, které budete potřebovat.
- Označte objektů BLOB zdroj nebo kontejneru cesty dat v úložišti objektů Blob.
- Vytvoření projektu exportovat ve vašem účtu úložiště zdroj pomocí portálu Classic nebo rozhraní REST API Import nebo Export.
- Zadejte objektů BLOB zdroj nebo kontejneru cesty dat ve exportovat projektu.
- Poskytovat zpáteční adresu a číslo účtu dopravce pro se dá použít pro dodací jednotky zpět.
- Dodat jednotky pevného disku na adresu příjemce k dispozici při vytváření projektu.
- Aktualizace doručení sledování Číslo ve exportovat podrobnosti projektu a odesílat úlohy export.
- Jednotky jsou přijetí a zpracování uprostřed Azure data.
- Jednotky jsou šifrovány pomocí nástroje BitLocker; klávesy je dostupné prostřednictvím portálu klasické.  
- Jednotky jsou odeslané pomocí svého účtu carrier na zpáteční adresu uvedenou v úlohy importu.

    ![Obrázek 2:Export úlohy toku](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-status"></a>Zobrazení stavu projektu

Můžete sledovat stav importu nebo exportu úlohy z portálu klasické. Přejděte ke svému účtu úložiště na portálu klasické a klikněte na kartu **Import nebo Export** . Na stránce se zobrazí seznam úloh. Můžete filtrovat seznam na stav úlohy, název projektu, typ projektu nebo číslem pro sledování.

Zobrazí se jednu z následujících úloh stavů podle toho, kde je jednotka v procesu.

| Stav úlohy   | Popis                                                                                                                                                                                                                                                                                                                                                                        |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vytvoření     | Vaše úloha byla vytvořena, ale ještě nezadali jste informace o způsobu dodání.                                                                                                                                                                                                                                                                                                |
| Expedice     | Vytvořil práce a dodávky informací. **Poznámka**: při doručení jednotku Azure datacentrem stav může nadále zobrazovat "Dodávky" určitou dobu. Po spuštění služby kopírování dat, stav se změní na "Přenos". Pokud chcete zobrazit konkrétnější stav jednotky, můžete použít rozhraní REST API Import nebo Export. |
| Přenos | Vaše data přenášena z pevného disku (pro úlohy importu) nebo na pevný disk (pro export úloha).                                                                                                                                                                                                                                                                 |
| Sbalení    | Přenos data dokončení a pevného disku je připraveno k odeslání zpět.                                                                                                                                                                                                                                                                             |
| Dokončení     | Pevný disk byly odeslané zpět.                                                                                                                                                                                                                                                                                                                                      |

### <a name="time-to-process-job"></a>Čas k obrázku projektu

Časový úsek trvá proces import nebo export projektu liší se podle různých faktorů, jako je času dodávky projektu typ, typ a velikost dat kopírovaný a velikost disků k dispozici. Import nebo Export služba nemá SLA. Rozhraní REST API můžete lépe sledovat průběh projektu. Podívejte se parametr procento dokončení v operaci úloh seznam, který indikuje průběh kopírovat. Kontaktujte nás v případě potřeby odhad dokončit úkol s časem kritické import nebo export.

### <a name="pricing"></a>Ceny

**Jednotka zpracování poplatků**

Existuje poplatek zpracování jednotka pro každou jednotku zpracovány jako součást importu nebo exportu projektu. Zobrazit podrobnosti o [Cenách Azure Import nebo Export](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Dopravné**

Pokud dodáte jednotky Azure, můžete zaplatit náklady dodávky dopravců. Když Microsoft vrátí jednotky, náklady dodávky účtovat na carrier účtu, který jste zadali při vytváření úlohy.

**Náklady transakce**

Při importu dat do úložiště objektů blob jsou žádné transakce náklady. Při exportu dat z úložiště objektů blob poplatky standardní výstupní platí. Další podrobnosti o nákladech transakce najdete v tématu [přenosu dat ceny.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Představení aplikace

V této části poskytneme podrobné pokyny k vytváření operaci importu a exportu projektu. Ujistěte se, jestli že splňujete všechny [předpoklady](#pre-requisites) před chovaly.

## <a name="how-to-create-an-import-job"></a>Jak vytvořit úlohy importu?

Vytvoření úlohy importu ke kopírování dat ke svému účtu Azure úložiště od pevných discích odesíláním jednu nebo více jednotek s daty do centra pro zadaná data. Import projektu vyjadřuje podrobnosti o jednotky pevného disku, data, která chcete zkopírovat, zacílit účtu úložiště a dodávky informací do služby Azure Import nebo Export. Vytváření úlohy importu je třech krocích. Nejdřív připravte se na vašich jednotkách nástrojem pro klienta Azure Import nebo Export. Druhý odeslání úlohy importu pomocí portálu klasické. Třetí dodat jednotek pro dodávky adresu poskytnutou při vytváření úlohy a aktualizujte informace o dodávky v podrobností projektu.   

> [AZURE.IMPORTANT] Můžete odeslat jenom jednu úlohu jednoho účtu úložiště. Každou jednotku, které doplníte lze importovat do jednoho účtu úložiště. Například Řekněme, že chcete importovat data do dva účty úložiště. Musí používat zvláštní pevných discích pro každý účet úložiště a vytvořte samostatné úlohy každého účtu úložiště.

### <a name="prepare-your-drives"></a>Příprava jednotky

Cílem prvního kroku při importu dat pomocí služby Azure Import nebo Export Příprava vašich jednotkách nástrojem pro klienta Azure Import nebo Export. Postupujte podle pokynů k přípravě jednotky.

1.  Zjistěte, data, která chcete importovat. Může to být adresářů a samostatné soubory na místního serveru nebo sdílené síťové složky.  

2.  Určete počet jednotek, které budete potřebovat v závislosti na celkovou velikost data. Získávají požadovaný počet 3,5 SATA II/III pevných discích.

3.  Označení cílového úložiště účtu, kontejner, virtuálních nebo objekty BLOB.

4.  Určení adresářů a/nebo samostatné soubory, které budou zkopírovány do každého pevného disku.

5.  Pomocí [Nástroje služby Azure Import nebo Export](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) zkopírujte data do jednoho nebo více pevných discích.

    - Nástroj Azure Import nebo Export vytvoří kopii relace ke kopírování dat ze zdroje jednotky pevného disku. V relaci jednu kopii můžete nástroj Kopírovat jediném adresáři spolu s jeho podadresáře nebo tvořená jedním souborem.

    - Možná budete muset více kopií relace Pokud zdrojová data zahrnuje mnoho adresářů.

    - Každého pevného disku, které připravujete vyžaduje aspoň jednu kopii relace.

6.  Můžete zadat / šifrování parametr Povolit šifrování nástroje Bitlocker na pevném disku. Můžete taky může povolit šifrování nástroje Bitlocker ručně na pevném disku a zadejte klíč při spuštění nástroje.

7.  Azure Import nebo Export vygeneruje souboru deníku jednotka pro každou jednotku jako je připraven. Je jednotka deníku soubor uložený na místním počítači, ne na vlastní jednotce. Při vytváření úlohy importu, budou se nahrávat soubor deníku. Soubor deníku jednotka obsahuje ID jednotky a klíč BitLocker, jakož i další informace o změně jednotky.
**Důležité**: každého pevného disku je poštou bude mít za následek souboru deníku. Při vytváření úlohy importu pomocí portálu klasické, třeba uložit všechny soubory deníku jednotek, které jsou součástí této úlohy importu. Jednotky bez deníku, který nebude budou zpracovány soubory.

8.  Po dokončení Příprava disku neupravujte data na jednotky pevného disku nebo soubor deníku.

V následující tabulce jsou příkazy a příklady pro přípravu jednotku pevného disku nástrojem pro klienta Azure Import nebo Export.

Azure Import nebo Export klienta PrepImport příkaz nástroj pro první relaci Kopírovat zkopírujte v adresáři:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Příklad:**

V následujícím příkladu jsme kopírujete všechny soubory a sub adresáře z H:\Video jednotku pevného disku připojeny X:. Data budou importovány účet cílového úložiště zadaný klíč účtu úložiště a do kontejneru úložiště s názvem video. Pokud kontejneru úložiště není k dispozici, bude vytvořena. Tento příkaz také formátovat a šifrování cílové pevném disku.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:storageaccountkey /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure Import nebo Export klienta PrepImport příkaz nástroj pro relace přenosu následné Kopírovat zkopírujte v adresáři:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Pro následné kopírovat relace stejné jednotku pevného disku zadejte deníku souboru se stejným názvem a zadejte nové ID relace; je není nutné zajistit jednotce spolu s cílovou úložiště účtu znovu ani formátovat nebo šifrování jednotky. V tomto příkladu jsme kopírujete složce H:\Photo a jeho podadresáře na stejnou cílové jednotku, do kontejneru úložiště s názvem fotku.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure Import nebo Export nástroj klienta PrepImport příkaz pro první relace Kopírovat zkopírujte soubor:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Azure Import nebo Export nástroj klienta PrepImport příkaz relací dalších Kopírovat zkopírujte soubor:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Zapamatovat**: ve výchozím nastavení dat bude importován jako objekty BLOB blokovat. Použít parametr /BlobType importovat data jako objekty BLOB stránky. Například pokud importujete virtuální pevný disk soubory, které bude připojen jako disků na Azure OM, je nutné importovat je jako objekty BLOB stránky. Pokud si nejste jisti, které objektů blob typ použít, můžete zadat /blobType:auto a můžeme vám pomůže určit typ vpravo. V tomto případě všechny soubory virtuálního pevného disku a vhdx bude importován jako objekty BLOB stránky a zbytek bude importován jako objekty BLOB blokovat.

Zobrazit další podrobné informace o používání nástroje klienta Azure Import nebo Export v [Příprava pevné jednotky pro Import](https://msdn.microsoft.com/library/dn529089.aspx).

Také v nápovědě k [Výběru pracovní postup Příprava pevné jednotky úlohy importu](https://msdn.microsoft.com/library/dn529097.aspx) další podrobné pokyny.  

### <a name="create-the-import-job"></a>Vytvoření úlohy importu

1.  Po předem připravených jednotky, přejděte ke svému účtu úložiště na [portál klasické](https://manage.windowsazure.com) a zobrazení na řídicím panelu. V části **Rychlé přehledně uspořádané**klikněte na **vytvořit úlohy importu**. Zjistit, jakým způsobem a zaškrtněte políčko označíte, že jste připraveni na disku a máte k dispozici soubor deníku jednotky.

2.  V kroku 1 poskytnutí kontaktních informací někoho pověřit této úlohy importu a platnou zpáteční adresu. Pokud chcete uložit úplné protokolování data úlohy importu, zaškrtněte možnost **Uložit úplné protokolování v kontejneru objektů blob Moje "waimportexport"**.

3.  V kroku 2 nahrajte soubory deníku jednotky, které jste získali během krok Příprava jednotka. Je třeba nahrání jednoho souboru pro každý jednotku, kterou jste připraveni.

    ![Vytvoření úlohy importu - krok 3](./media/storage-import-export-service/import-job-03.png)

4.  V kroku 3 zadejte popisný název úlohy importu. Všimněte si, že název, který zadáte může obsahovat pouze malá písmena, čísla, pomlčky a podtržítka, musí začínat písmenem a nesmí obsahovat mezery. Název, který chcete sledovat úlohami době, kdy budou v průběhu a po dokončení budou používat.

    Potom vyberte oblast centra dat ze seznamu. Oblast dat centra výskyt znamená datové centrum a adresu, na které je třeba nabízet balíčku. V tématu Nejčastější dotazy k pod další informace.

5.  V kroku 4 vyberte zpáteční carrier ze seznamu a zadejte číslo svého účtu carrier. Použijeme tento účet dodat jednotky zpět po dokončení importu práce.

    Pokud máte číslem pro sledování ze seznamu vyberte svého operátora doručení a zadejte číslo svého sledování.

    Pokud budete mít s číslem pro sledování dosud, vyberte **I will provide zadám údaje o mém dodávky pro tento projekt importovat až jste poslali Můj balíček**, pak dokončete import.

6. Zadejte číslo svého sledování poté, co jste poslali balíčku, vraťte se na stránce **Import nebo Export** účtu úložiště na portálu klasické ze seznamu vyberte práce a zvolte **Dodací informace**. Procházení průvodce a zadejte číslo svého sledování v kroku 2.

    Pokud není číslem pro sledování do 2 týdnů vytváření úlohy, vyprší platnost projektu.

    Pokud projektu je ve stavu vytváření a dodání přenos, můžete taky aktualizovat vaše carrier číslo účtu v části Krok 2 průvodce. Když projektu je ve stavu balení, nelze aktualizovat číslo účtu dopravce pro tento projekt.

7. Sledování průběhu práce na řídicím panelu portálu. Najdete v článku co tyto stavy úlohy v předchozí části znamenají zobrazením [stavu projektu](#viewing-your-job-status).

## <a name="how-to-create-an-export-job"></a>Jak vytvořit úlohy exportu?

Vytvoření projektu exportovat upozornit službu Import nebo Export, že je budete být dodací jednu nebo více prázdné jednotek datacentrem tak, aby data můžete vyexportovaný z účtu úložiště jednotky a jednotky potom dodání.

### <a name="prepare-your-drives"></a>Příprava jednotky

Následující před kontroly doporučujeme Příprava projektu exportovat vašich jednotkách:

1. Zaškrtněte políčko číslo discích potřebné pomocí příkazu PreviewExport nástroj Azure Import nebo Export. Další informace najdete v tématu [Náhled jednotka použití pro Export úlohu](https://msdn.microsoft.com/library/azure/dn722414.aspx). Umožňuje zobrazit náhled využití jednotky pro objekty BLOB, které jste vybrali, v závislosti na velikosti jednotek, které budete používat.

2. Zkontrolujte, že je můžete pro čtení i zápis na pevný disk dodané projektu exportovat.

### <a name="create-the-export-job"></a>Vytvoření úlohy Export

1.  Vytvoření projektu exportovat, přejděte ke svému účtu úložiště na [portál klasické](https://manage.windowsazure.com)a zobrazit řídicího panelu. V části **Rychlé přehledně uspořádané**klikněte na **vytvořit úlohy exportu** a pokračujte podle pokynů průvodce.

2.  V kroku 2 poskytnutí kontaktních informací někoho pověřit pro tento projekt exportovat. Pokud chcete uložit úplné protokolování data projektu exportovat, zaškrtněte možnost **Uložit úplné protokolování v kontejneru objektů blob Moje "waimportexport"**.

3.  V kroku 3 zadejte objektů blob data, která chcete exportovat ze svého účtu úložiště na prázdné jednotky a jednotky. Je možné exportovat všechna data v účtu úložiště objektů blob nebo můžete určit, které objekty BLOB nebo sady objektů BLOB export.

    Chcete-li zadat objektů blob exportovat, používat voliče **Rovno** a zadejte relativní cestu k blob začínající jméno container. Pomocí *$root* můžete určit kořenový kontejner.

    Chcete-li všechny objekty BLOB počínaje předponu, pomocí voliče **Začíná** a zadejte tuto předponu začínající lomítko "/". Tuto předponu může být předponu jméno container, dokončeno container jméno nebo název dokončení kontejneru následovaný předponu názvu objektů blob.

    Následující tabulka zobrazuje příklady platné objektů blob cesty:

    Výběr|Cesta objektů BLOB|Popis
    ---|---|---
    Spouštění pracovních|/|Exportuje všechny objekty BLOB účtu úložiště
    Spouštění pracovních|/$root /|Exportuje všechny objekty BLOB v kontejneru kořenové
    Spouštění pracovních|/Book|Exportuje všechny objekty BLOB v kontejneru začínající předponu **knihy**
    Spouštění pracovních|/Music/|Exportuje všechny objekty BLOB v kontejneru **hudbu**
    Spouštění pracovních|/ hudbu/láska|Exportuje všechny objekty BLOB v kontejneru **Hudba** , které začínají na předponu **používat**
    Nerovná se|$root/logo.bmp|Export objektů blob **logo.bmp** v kontejneru kořenové
    Nerovná se|videos/Story.MP4|Export objektů blob **story.mp4** v kontejneru **videa**

    Jak ukazuje tento obrázek je nutné zadat objektů blob cesty v platné formáty k chybám při zpracování.

    ![Vytvoření projektu exportovat - krok 3](./media/storage-import-export-service/export-job-03.png)


4.  V kroku 4 zadejte popisný název úlohy export. Název, který zadáte může obsahovat pouze malá písmena, čísla, pomlčky a podtržítka, musí začínat písmenem a nesmí obsahovat mezery.

    Oblast centra dat bude označovat datovém centru, do kterého je třeba nabízet balíčku. V tématu Nejčastější dotazy k pod další informace.

5.  V kroku 5 vyberte zpáteční carrier ze seznamu a zadejte číslo svého účtu carrier. Použijeme tento účet dodat vašich jednotkách zpět po dokončení exportu práce.

    Pokud máte číslem pro sledování ze seznamu vyberte svého operátora doručení a zadejte číslo svého sledování.

    Pokud budete mít s číslem pro sledování dosud, vyberte **I will provide zadám Moje dodávky informace o projektu exportovat po jste poslali Můj balíček**, proveďte exportu vytvoříte.

6. Zadejte číslo svého sledování poté, co jste poslali balíčku, vraťte se na stránce **Import nebo Export** účtu úložiště na portálu klasické ze seznamu vyberte práce a zvolte **Dodací informace**. Procházení průvodce a zadejte číslo svého sledování v kroku 2.

    Pokud není číslem pro sledování do 2 týdnů vytváření úlohy, vyprší platnost projektu.

    Pokud projektu je ve stavu vytváření a dodání přenos, můžete taky aktualizovat vaše carrier číslo účtu v části Krok 2 průvodce. Když projektu je ve stavu balení, nelze aktualizovat číslo účtu dopravce pro tento projekt.

    > [AZURE.NOTE] Pokud objektů blob k exportu se používá při kopírování na pevný disk, služby Azure Import nebo Export udělejte si snímek objektů blob a zkopírujte snímek.

7.  Sledování průběhu práce na řídicí panel na klasické portálu. V tématu co tyto stavy projektu znamená, že v předchozí části na "zobrazení stavu úlohy".

8.  Jakmile dostanete jednotek s exportovaná data, můžete zobrazit a zkopírujte klíčů nástroje BitLocker generované službou jednotky. Přejděte ke svému účtu úložiště na portálu klasické a klikněte na kartu Import nebo Export. Vyberte export práce v seznamu a klikněte na tlačítko Zobrazit klíče. Jak je ukázáno v následujícím příkladu jsou zobrazeny nástroje BitLocker klíče:

    ![Klávesové zkratky nástroje BitLocker zobrazení pro export projektu](.\media\storage-import-export-service\export-job-bitlocker-keys.png)

Podívejte se prosím prostřednictvím v části Nejčastější dotazy pod jako popisuje nejčastější dotazy, které zákazníci dojít při pomocí této služby.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy ##


**Jak dlouho trvá zkopírovat data, když Moje jednotkách dosáhne datovém centru?**

Čas potřebný ke kopírování se liší v závislosti na různých faktorů, jako je typ projektu, typ a velikost dat kopírovaný, velikost disků k dispozici s existujícím zátěží na projektu. Můžete měnit od několika dnů k několika týdnů v závislosti na následujících skutečností. Je tedy obtížné poskytnout obecné odhad. Optimalizace práce zkopírováním více jednotek současně, pokud je to možné službu se snaží. Pokud máte úlohu kritické import nebo export čas, kontaktujte nás odhad.

**Kdy mám používat Import nebo Export služby Azure?**
Jednu byste měli vědět, že pomocí Azure Import Export v případě nahrávání nebo stahování přes síť trvá zhruba jako odhad více než 7 dnů. Můžete vypočítat, jak dlouho bude trvat pomocí online kalkulačky nebo [stahování](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/archive/master.zip) umístěn ve Azure Import Export REST API výběru v Azure ukázky úložiště na [Kalkulačky rychlost přenést Data](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html). Toto není přesný výpočet ale hrubé označení.

**Můžu používat službu Azure Import nebo Export pod svým účtem správce prostředků úložiště?**

Ne, nemůžete zkopírování dat nebo z účtu správce prostředků úložiště přímo pomocí služby Azure Import nebo Export. Služba podporuje pouze klasické úložiště účty. Podpora účtu správce prostředků úložiště bude brzy. Jako alternativu můžete importovat data do účtu klasické úložiště a migrace ke svému účtu úložiště správce prostředků pomocí [AzCopy](storage-use-azcopy.md) nebo CopyBlob.

**Můžete zkopírovat soubory Azure pomocí služby Azure Import nebo Export?**

Ne, služby Azure Import nebo Export podporuje pouze objektů BLOB bloku a objekty BLOB stránky. Všechny ostatní typy úložiště včetně Azure soubory, tabulky a fronty nejsou podporované.

**Neexistuje služby Azure Import nebo Export CSP předplatného?**

Ne, služba Azure Import nebo Export nepodporuje CSP předplatná. Podpora se přidají v budoucnu.

**Můžete přejít krok Příprava jednotka úlohy importu, nebo je můžete připravit jednotku nezkopíruje?**

Jednotku, který chcete odeslat při importu dat musí být připravena pomocí nástroje pro klienta Azure Import nebo Export. Zkopírujte data do jednotka musí pomocí nástroje klienta.

**Je potřeba provést libovolný Příprava disku při vytváření úlohy exportu?**

Doporučuje se žádné, ale některé před kontroly. Zaškrtněte políčko číslo discích potřebné pomocí příkazu PreviewExport nástroj Azure Import nebo Export. Další informace najdete v tématu [Náhled jednotka použití pro Export úlohu](https://msdn.microsoft.com/library/azure/dn722414.aspx). Umožňuje zobrazit náhled využití jednotky pro objekty BLOB, které jste vybrali, v závislosti na velikosti jednotek, které budete používat. Zkontrolujte taky, můžete číst a psát na pevný disk dodané projektu exportovat.

**Co se stane, pokud omylem odeslat pevného disku, který není v souladu s požadavky na podporované?**

Azure datovém centru vrátí jednotku, která není v souladu s požadavky na podporované pro vás. Pokud pouze některé jednotky v okně balíček nesplňuje požadavky podpory, budou zpracovány tyto jednotky a jednotky, které nesplňuje požadavky na budou vráceny pro vás.

**Můžete zrušit Moje úlohy?**

Při vytváření nebo dodávky svůj stav můžete zrušit projektu.

**Jak dlouho můžete zobrazit stav dokončení úloh klasické portálu?**

Zobrazení stavu pro dokončené práce za posledních 90 dní. Po 90 dnů se odstraní dokončené práce.

**Pokud chci import nebo export více než 10 jednotky, co má můžu udělat?**

Jeden import nebo export projektu můžete odkázat jenom 10 jednotek v jedné úlohy pro službu Import nebo Export. Pokud chcete odeslat více než 10 jednotky, můžete vytvořit více úloh. Jednotky, které jsou přidružené k stejnou úlohu musí společně odeslané ve stejném balíčku.

**Můžete použít adaptér USB SATA, která není v seznamu doporučená?**

Pokud máte převaděče, které tady není uvedené, můžete zkusit spuštění nástroje pro Import nebo Export Azure pomocí svého převaděče pro přípravu jednotku a zjistěte, jestli funguje před nákupem převaděče podporované.

**Službu formátovat jednotky před jejich vrácení?**

Ne. Všechny jednotky jsou šifrovány pomocí nástroje BitLocker.

**Možnost si zakoupit jednotek pro import nebo export úlohy od Microsoftu?**

Ne. Bude muset dodáte vlastní jednotek pro obě import a export úlohy.

**Až se dokončí úloha importu, jak budou moje data vypadá v účtu úložiště? Moje hierarchii adresářů se zachová?**

Při přípravě na pevný disk úlohy importu, cíle nastavil /dstdir: parametr. Toto je kontejner cílového úložiště účtu, ke kterému je zkopírována data z pevného disku. V tomto kontejneru cíl virtuálních vytvořené pro složky z pevného disku a objekty BLOB vytvořené pro soubory.

**Pokud má jednotka soubory, které už v účtu úložiště, služba přepíše existující objektů BLOB v účtu úložiště?**

Při přípravě disk, můžete určit, zda by měl přepíšou cílové soubory nebo ignorované parametr s názvem /Disposition: < přejmenovat | přepsat bez | přepsat >. Ve výchozím nastavení služby bude přejmenovat nové soubory spíše než přepsat existující objektů BLOB.

**Je kompatibilní se službou 32bitová verze operačních systémů Azure Import nebo Export klientské nástroje?**
Ne. Nástroj klienta je pouze kompatibilní se službou 64bitové operační systémy Windows. Získáte operační systémy v části [předpoklady](#pre-requisites) úplný seznam podporovaných verzí operačního systému.

**By měla obsahovat vše kromě jednotku pevného disku Moje balíček?**

Prosím dodat pouze pevných discích. Nezahrnujte položky, jako jsou power napájecí kabely nebo USB.

**Je potřeba dodat Moje jednotky pomocí FedEx nebo DHL?**

Dodáte jednotky datacentrem pomocí známé carrier jako FedEx, DHL, UPS nebo nám poštovní služby. Pro dodávky jednotky zpět v datovém centru, musí však zadat číslo účtu FedEx ve Spojených státech a EU nebo číslo účtu DHL v oblasti země Asie a Austrálie.

**Existují omezení s dodací jednotka mezinárodní?**

Dejte pozor, aby fyzické média, která se muset křížové mezinárodní ohraničení. Zodpovídáte za zajistit, že fyzické média a data jsou importovat a/nebo exportovat v souladu s rozhodným právem. Před dodávky médium, obraťte se na poradci pro ověření, že média a dat můžete ze zákona dodání identifikované datovém centru. To vám pomůže zajistit, že nedosáhnete Microsoft včas.

**Při vytváření projektů, adresy příjemce je umístění, do kterého se liší od nacházím účtu úložiště. Co mám dělat?**

Některé úložišť účtu jsou namapované na alternativní dodávky umístění. Dříve dostupná umístění doručovací lze také dočasně namapovat na alternativní umístění. Kontrola dodávky adresu poskytnutou při vytváření úlohy před dodací jednotky.

**Proč stav projektu v klasické portálu přivítejte "dodací" při Carrier webu zobrazuje Můj balíček doručení?**

Stav na portálu klasické změní z dodávky přenosu při zpracování spuštění jednotky. Pokud jednotky dosáhl zařízení, ale ještě nezačala zpracování, stavu úlohy se zobrazí jako dodávky.

**Když dodávky jednotka, dopravce zeptá dat centra jméno kontaktu a telefonní číslo. Co by měl obsahovat?**

Telefonní číslo je k dispozici pro vás při vytváření projektu. Pokud potřebujete jméno kontaktu, kontaktujte nás v waimportexport@microsoft.com a bude vám poskytneme těchto informací.

**Můžete pomocí služby Azure Import nebo Export zkopírovat PST poštovní schránky a dat služby SharePoint k O365?**

Získáte [jde o soubory PST importovat nebo SharePoint dat do Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Můžete pomocí služby Azure Import nebo Export zkopírovat Moje záloh offline zálohování služby Azure?**

Získáte [Offline zálohování pracovní postup v Azure zálohování](../backup/backup-azure-backup-import-export.md).

## <a name="see-also"></a>Viz taky:

- [Nastavte si nástroj klienta Azure Import nebo Export](https://msdn.microsoft.com/library/dn529112.aspx)

- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)

- [Azure Import Export REST API vzorku](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)
