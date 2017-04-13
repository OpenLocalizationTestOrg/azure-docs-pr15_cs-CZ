<properties
    pageTitle="Migrace k základnímu úložišti Azure Premium | Microsoft Azure"
    description="Migrace stávajícího virtuálních počítačích k úložišti Premium Azure. Úložiště Premium nabízí podpora výkonných maximum, minimum latence disku můžu/O-značnou pracovní vytížení na virtuálních počítačích Azure."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="yuemlu"/>


# <a name="migrating-to-azure-premium-storage"></a>Migrace na Azure Premium úložiště

## <a name="overview"></a>Základní informace

Azure Premium úložiště poskytuje podpora výkonných maximum, minimum latence disku virtuálních počítačích spuštěný můžu/O-značnou úloh. Migrací aplikace OM disků k úložišti Azure Premium může trvat výhod rychlost a výkonu discích.

Této příručce účel usnadňují nové uživatelům Azure Premium úložiště lepší připravit se na změnu přitom zajistit hladký přechod z jejich aktuální systému k základnímu úložišti Premium. Průvodce adresy tři klíčové prvky v tomto procesu: 

  - [Plánování migrace k základnímu úložišti Premium](#plan-the-migration-to-premium-storage)
  - [Příprava a zkopírujte virtuální pevných discích (VHD) k základnímu úložišti Premium](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
  - [Vytvoření Azure virtuálního počítače pomocí Premium úložiště](#create-azure-virtual-machine-using-premium-storage)

Můžete migrovat VMs z dalších platformách k úložišti Premium Azure nebo migrovat stávající VMs Azure ze standardní úložiště k základnímu úložišti Premium. Tato příručka obsahuje kroky pro oba dva scénáře. Postupujte podle kroků uvedených v části důležité podle toho, nefunguje.

>[AZURE.NOTE] Přehled funkcí a ceny Premium úložiště v úložišti Premium můžete najít: [Výkonné prostor úložiště pro Azure virtuálního počítače úloh](storage-premium-storage.md). Doporučujeme migraci všech virtuálního počítače disků vyžadující vysoké procesorů k úložišti Premium Azure pro dosažení nejlepších výsledků pro aplikaci. Pokud disku nevyžaduje vysoké procesorů, můžete omezit náklady na údržbu ve standardní úložiště, který ukládá data disku virtuálního počítače na pevných discích (pevných disků) místo disky SSD.

Dokončení procesu migrace jako celek může vyžadovat další akce před a po pokynů uvedených v této příručce. Jako příklad lze uvést konfiguraci virtuální sítě nebo koncové body nebo provádění změn kódu samotnou aplikaci které může vyžadovat některé prostoje v aplikaci. Tyto akce jsou jedinečné pro jednotlivé aplikace a spolu s pokynů uvedených v této příručce tak, aby úplné přechodu Premium úložiště jako bezproblémové co byste měli dokončit.


## <a name="plan-the-migration-to-premium-storage"></a>Plánování migrace k základnímu úložišti Premium

V této části zajistí, že budete chtít kroky migrace v tomto článku a pomáhá při rozhodování nejlepší na typy OM a disku.

### <a name="prerequisites"></a>Zjistit předpoklady pro
- Budete potřebovat Azure předplatného. Pokud nemáte, můžete vytvořit měsíční [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/) předplatného nebo navštivte [Azure ceny](https://azure.microsoft.com/pricing/) pro další možnosti.
- Spouštět rutiny prostředí PowerShell, musíte modul služeb Microsoft Azure PowerShell. V tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) pro instalovat čárky a pokyny k instalaci.
- Když budete chtít používat Azure VMs spuštěna skladování Premium, budete muset používat může VMs Premium úložiště. Můžete použít standardní a úložiště Premium disků s může VMs Premium úložiště. V budoucnu bude k dispozici více typů OM disků úložiště Premium. Další informace o všech dostupných Azure OM disku typy a formáty najdete v článku [velikosti virtuálních počítačích](../virtual-machines/virtual-machines-windows-sizes.md) a [velikosti služby cloudu](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Co byste měli zvážit

Azure OM podporuje připojení několik disků Premium úložiště tak, aby vaše aplikace může obsahovat až 64 TB úložiště na OM. S úložištěm Premium aplikace dosáhnout 80 000 procesorů (vstupní a výstupní operace sekundu) za OM a 2 000 MB za druhý disku výkon OM s velmi nízký čekacích dob pro operace čtení. Máte možnosti v různých VMs a discích. Tento oddíl je umožňují najít možnost, která se vaše pracovní zátěž nejvíc hodí.

#### <a name="vm-sizes"></a>OM velikosti
Specifikace velikost Azure OM jsou uvedeny v [velikosti virtuálních počítačích](../virtual-machines/virtual-machines-windows-sizes.md). Zkontrolujte vlastnosti výkonu virtuálních počítačích, které fungují s úložištěm Premium a zvolte požadovanou nejvhodnější velikost OM, které nejlépe odpovídá vaší pracovní zátěž. Ujistěte se, že dostatečnou šířku pásma v není k dispozici OM řídit přenosy disku.


#### <a name="disk-sizes"></a>Disk velikosti
Existují tři typy disků, které se dá používat se vaše OM a má každá konkrétní procesorů a výkon omezení. Vzít v úvahu tyto limity při výběru typu disku pro vaše OM podle potřeby aplikace z hlediska kapacitu, výkon a škálovatelnost a načte ve špičce.

|Typ disku úložiště Premium|P10|P20|P30|
|:---:|:---:|:---:|:---:|
|Velikost disku|128 GB|512 GB|1 024 GB (1 TB)|
|Procesorů na disku|500|2300|5000|
|Výkon na disku|100 MB za sekundu|150 MB za sekundu|200 MB za sekundu|

V závislosti na vaší pracovní zátěž zjistit, zda jsou potřebné pro vaše OM disků další data. Několik disků trvalý dat můžete připojit vaší v angličtině. V případě potřeby můžete prokládanou disků větší kapacity a výkonu hlasitost. (V tématu Co je prokládání disků [sem](storage-premium-storage-performance.md#disk-striping).) Pokud prokládanou disků datový úložiště Premium pomocí [Úložiště mezery][4], byste měli označit s jedním sloupcem pro každý disk, který se používá. Celkový výkon prokládané hlasitost jinak, může být nižší než se očekává kvůli lichý rozdělení provozu v discích. Nástroj *mdadm* Linux VMs slouží k dosažení stejné. Najdete v článku [Konfigurace Software RAID na Linux](../virtual-machines/virtual-machines-linux-configure-raid.md) podrobnosti.

#### <a name="storage-account-scalability-targets"></a>Úložiště účtu škálovatelnost cíle
Premium úložiště účty mají následující cílů škálovatelnost kromě [Azure úložiště škálovatelnost a výkonu cílů](storage-scalability-targets.md). Pokud vaše požadavky aplikace překročí cílů škálovatelnost účtu jednoho úložiště, vytvoření aplikace pro použití více účtů úložiště a oddílů dat mezi účty úložiště.

|Účtu Součet kapacita|Celkové šířky pásma pro účet místně nadbytečné úložiště|
|:--|:---|
|Disk kapacita: 35TB<br />Snímek kapacita: 10 TB|Až 50 zajišťuje sekundu pro vstupní + odchozí|

Další informace o specifikace Premium úložiště podívejte se na [škálovatelnost a výkonu při použití Premium úložiště](storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

#### <a name="disk-caching-policy"></a>Diskové mezipaměti zásad
Ve výchozím nastavení je disk ukládání do mezipaměti zásad *Jen pro čtení* pro všechna data Premium disků a *Pro čtení i zápis* disk operačního systému Premium připojené bude v angličtině. Toto nastavení se doporučuje k dosažení optimálního výkonu pro IOs vaše aplikace. Zápis silná nebo určená jen zápisu dat disků (například souborů protokolu SQL serveru) zakážete diskové mezipaměti, takže můžete dosáhnout lepší výkon aplikace. Nastavení mezipaměti pro stávající data disků můžou být aktualizovány pomocí [Portálu Azure](https://portal.azure.com) nebo parametr *- HostCaching* rutinu *Set-AzureDataDisk* .

#### <a name="location"></a>Umístění
Vyberte umístění, kde je k dispozici Azure Premium úložiště. V tématu [Služby Azure podle regionů](https://azure.microsoft.com/regions/#services) aktuální informace o dostupných umístění. VMs umístěny ve stejné oblasti účtem úložiště, že ukládá disků OM získáte mnohem lepší výkon než pokud jsou v samostatných oblastech.

#### <a name="other-azure-vm-configuration-settings"></a>Nastavení dalších OM Azure
Při vytváření Azure OM, se zobrazí výzva ke konfiguraci nastavení určitých OM. Mějte na paměti, několik nastavení řeší dobu trvání OM, zatímco můžete upravit nebo přidat další později. Zkontrolujte tato nastavení konfigurace Azure OM a ujistěte se, že tyto nakonfigurované řádně podporovat podle vašich požadavků zátěží na projektu.

### <a name="optimization"></a>Optimalizace

[Azure Premium úložiště: návrh High Performance](storage-premium-storage-performance.md) obsahuje Rady, vytvářet výkonné aplikace pomocí Azure Premium úložiště. Můžete postupujte podle pokynů v kombinaci s výkonem doporučené postupy pro technologie používané aplikace.


## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Příprava a zkopírujte virtuální pevných discích (VHD) k základnímu úložišti Premium

Následující části jsou uvedeny pokyny pro přípravu VHD z vaší OM a kopírování VHD k základnímu úložišti Azure.

- [Scénář 1: "můžu mi migrace stávajícího Azure VMs na Azure Premium úložiště."](#scenario1)
- [Scénář 2: "můžu mi migrace VMs z dalších platformách Azure Premium úložiště."](#scenario2)

### <a name="prerequisites"></a>Zjistit předpoklady pro

Příprava VHD migraci, musíte:

- Předplatné Azure účet úložiště a kontejneru v tomto účtu úložiště, do kterého můžete zkopírovat vaší virtuální pevný disk. Všimněte si, že cílového úložiště můžete použít standardní nebo Premium úložiště účet v závislosti na požadovanou.
- Nástroj pro generalize virtuálního pevného disku, pokud chcete vytvořit několika instancích spuštěných OM z něho. Například sysprep pro Windows nebo virt.krychle sysprep pro systémem Ubuntu.
- Nástroj pro nahrání souboru virtuální pevný disk k tomuto účtu úložiště. V tématu [přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md) nebo pomocí [Průzkumníka Azure úložišť](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Tato příručka popisuje kopírování vaší virtuální pevný disk pomocí nástroje AzCopy.

> [AZURE.NOTE] Pokud zvolíte možnost synchronní kopie s AzCopy, optimální výkon, zkopírujte vaší virtuální pevný disk jednu z těchto nástrojích spuštěním z Azure OM, která je ve stejné oblasti jako účet cílového úložiště. Pokud kopírujete virtuálního pevného disku z Azure OM v jiné oblasti, může být nižší výkon.
>
> Kopírování velkého množství dat přes omezené šířky pásma, zvažte [použití služby Azure Import nebo Export pro přenos dat k úložišti objektů Blob](storage-import-export-service.md); umožňuje provádět přenášet data podle dodávky pevných discích Azure datacentra. Služby Azure Import nebo Export slouží ke kopírování dat na účet standardní úložiště. Jakmile se data ve vašem účtu standardní úložiště, můžete [Kopírovat objektů Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) nebo AzCopy k převodu dat ke svému účtu úložiště premium.
>
> Všimněte si, že Microsoft Azure podporuje pouze pevnou velikostí souborů virtuální pevný disk. Soubory VHDX nebo dynamické VHD nejsou podporované. Pokud máte dynamické virtuálního pevného disku, převedete ji na pevnou velikost pomocí rutiny [Převést virtuální pevný disk](http://technet.microsoft.com/library/hh848454.aspx) .

### <a name="scenario1"></a>Scénář 1: "můžu mi migrace stávajícího Azure VMs na Azure Premium úložiště."

Při migraci stávajícího VMs Azure zastavit OM, Příprava VHD každého typu se má virtuální pevný disk a zkopírujte virtuální pevný disk s AzCopy nebo Powershellu.

OM musí být úplně dolů k migraci čistého stavu. Nastane prostoje až do dokončení migrace.

#### <a name="step-1-prepare-vhds-for-migration"></a>Krok 1. Příprava VHD migraci

Pokud migrujete existující VMs Azure k základnímu úložišti Premium, může být vaše virtuálního pevného disku:

- Obrázek generalized operační systém
- Disk jedinečné operační systém
- Disk dat

Tady vidíte projdeme scénářů 3 pro přípravu vaší virtuální pevný disk.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Umožňuje vytvořit několika instancích spuštěných OM generalized virtuálního pevného disku operační systém

Pokud ukládáte virtuálního pevného disku, která bude použita k vytvoření několika instancích spuštěných obecný OM Azure, musíte nejdřív generalize nástrojem sysprep virtuální pevný disk. Toto nastavení se projeví do virtuálního pevného disku, který je v místním i v cloudu. Všechny informace specifické pro počítač odebrány z virtuální pevný disk.

>[AZURE.IMPORTANT] Udělejte si snímek nebo zálohování vaší OM před zobecněním ho. Průběžný sysprep zastaví a zrušit OM instance. Postupujte podle kroků pod sysprep virtuálního pevného disku operačního systému Windows. Všimněte si, že spouštět příkaz bude nutné vypnout virtuální počítač. Další informace o Sysprep najdete v článku [Přehled Sysprep](http://technet.microsoft.com/library/hh825209.aspx) nebo [Technické informace o Sysprep](http://technet.microsoft.com/library/cc766049.aspx).

1. Jako správce otevřete okno příkazového řádku.
2. Zadejte tento příkaz Otevřít Sysprep:

        %windir%\system32\sysprep\sysprep.exe

3. V systému nástroj pro přípravu, vyberte prostředí mimo pole zadejte systém (počáteční nastavení počítače), zaškrtněte políčko generalizace, **Vyberte položku**a klikněte na **OK**, jak je znázorněno na následujícím obrázku. Sysprep generalize operačního systému a vypnutí systému.

    ![][1]

Pro systémem Ubuntu OM stejné dosáhnout pomocí virt.krychle sysprep. V tématu [virt.krychle sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) další podrobnosti. Viz také některé otevřít zdroj [Linux serveru zřizování software](http://www.cyberciti.biz/tips/server-provisioning-software.html) pro jiné operační systémy Linux.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Vytvoření jedna instance OM pomocí jedinečné virtuálního pevného disku operační systém

Pokud máte aplikaci OM vyžadující specifická data počítače se systémem, není generalize virtuální pevný disk. Virtuální pevný disk generalized mohou sloužit k vytvoření jedinečných instance OM Azure. Například pokud máte řadiče domény na vaše virtuálního pevného disku, provádění sysprep znamená, že neefektivní jako řadiče domény. Prohlédněte si spuštěné na vaše OM a vlivu spuštěna sysprep je před zobecněním virtuální pevný disk.

##### <a name="register-data-disk-vhd"></a>Registrace dat disku virtuální pevný disk

Pokud máte data disků v Azure k poštovním musí Ujistěte se, že VMs používající discích dat jsou vypnuté.

Postupujte podle kroků popsaných níže zkopírujte virtuální pevný disk k úložišti Premium Azure a zaregistrovat jako disk zřizování data.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Krok 2. Vytvoření cílové umístění svého virtuální pevný disk

Vytvoření účtu úložiště pro zachování svého VHD. Při plánování místa uložení vašeho VHD je potřeba zvážit následující skutečnosti:

- Cíl Premium úložiště účtu.
- Umístění účtu úložiště musí být stejný jako úložiště Premium může Azure VMs vytvoříte v oblasti poslední. Zkopírujete do nového účtu úložiště nebo chcete používat stejný účet úložiště podle vašich potřeb.
- Zkopírujte a uložte klíč účtu úložiště klienta cílového úložiště další fáze.

Disků dat můžete mít disků některá data na standardní úložiště účtu (například disků, které mají chladič úložiště), ale důrazně doporučujeme přesouvat všechna data pro pracovní zátěž výrobní používat premium úložiště.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Krok 3. Zkopírujte virtuální pevný disk s AzCopy nebo prostředí PowerShell

Potřebujete najít svůj kontejneru cesty a úložiště klíč účtu zpracuje jednu z následujících dvou možností. Kontejner cesty a úložiště klíč účtu najdete v **Portálu Azure** > **úložiště**. Adresa URL kontejneru bude třeba "https://myaccount.blob.core.windows.net/mycontainer/".

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Možnost 1: Zkopírování virtuálního pevného disku s AzCopy (asynchronní kopii)

Použití AzCopy, můžete snadno nahrát virtuální pevný disk přes Internet. V závislosti na velikosti VHD může to trvat čas. Nezapomeňte zkontrolovat limitů úložiště účtu průniku/výstupní při použití tuto možnost. Podrobnosti najdete v článku [Azure úložiště škálovatelnost a výkonu cílů](storage-scalability-targets.md) .

1. Stažení a instalace AzCopy odsud: [nejnovější verzi AzCopy](http://aka.ms/downloadazcopy)
2. Otevřete Azure PowerShell a přejděte do složky, kde je nainstalovaný AzCopy.
3. Pomocí následujícího příkazu zkopírujte soubor virtuální pevný disk z "Zdrojový" k "Cílový".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Příklad:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd

    Tady je popis parametry použité v příkazu AzCopy:

 - * */Zdroje: * &lt;zdroj&gt;:*** umístění složky nebo kontejneru adresy URL, která obsahuje virtuální pevný disk.
 - * */SourceKey: * &lt;klíč účtu zdroj&gt;:*** klíč účtu úložiště účtu úložiště zdroje.
 - * */Dest: * &lt;cíl&gt;:*** adresy URL kontejneru úložiště zkopírujte virtuální pevný disk na.
 - * */DestKey: * &lt;klíč účtu cíl&gt;:*** klíč účtu úložiště klienta cílového úložiště.
 - * */Vzorek: * &lt;název souboru&gt;:*** zadejte název souboru virtuální pevný disk na Kopírovat.

Podrobnosti o používání nástroje AzCopy najdete v tématu [přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Možnost 2: Zkopírování virtuálního pevného disku pomocí prostředí PowerShell (Synchronized kopii)

Můžete taky zkopírovat soubor virtuální pevný disk pomocí rutiny prostředí PowerShell Start AzureStorageBlobCopy. Umožňuje tento příkaz na Azure PowerShell zkopírujte virtuální pevný disk. Nahrazení hodnot v <> s odpovídajícími hodnotami z vašeho účtu zdrojového a cílového úložiště. Pomocí tohoto příkazu, musíte mít kontejneru s názvem VHD ve vašem účtu cílového úložiště. Pokud kontejneru neexistuje, vytvořte před spuštěním příkazu.

    $sourceBlobUri = <source-vhd-uri>

    $sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

    $destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

    Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext

Příklad:

    C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

    C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

    C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

    C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext

### <a name="scenario2"></a>Scénář 2: "můžu mi migrace VMs z dalších platformách Azure Premium úložiště."

Při migraci virtuální pevný disk z jiných - Azure cloudového úložiště do Azure, musíte nejdřív exportovat virtuálního pevného disku místního adresáře. Máte úplný zdrojový cestu místního adresáře, kde je uložena virtuální pevný disk po ruce a pak ho nahrajte do úložiště Azure pomocí AzCopy.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Krok 1. Export virtuální pevný disk do místního adresáře

##### <a name="copy-a-vhd-from-aws"></a>Zkopírujte virtuálního pevného disku z AWS

1. Pokud používáte AWS, exportujte do virtuálního pevného disku v bloku Amazon S3 EC2 instance. Postupujte podle kroků popsaných v dokumentaci Amazon k exportu Amazon EC2 instance, které chcete nainstalovat nástroj Amazon EC2 rozhraní příkazového řádku (rozhraní příkazového řádku) a spusťte příkaz Vytvořit instance export úkolech EC2 instance export do souboru virtuální pevný disk. Používejte **virtuálního pevného disku** pro disku #95; OBRÁZEK & #95; Formát proměnná při spuštění příkazu **vytvořit instance – export úkolech** . Exportovaný soubor virtuální pevný disk se uloží do bloku Amazon S3, který jste zadali během tohoto procesu.

        aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
        --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX

2. Virtuální pevný disk moct soubor stáhněte z bloku S3. Vyberte soubor virtuálního pevného disku, pak **Akce** > **Stáhnout**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Zkopírujte virtuálního pevného disku z jiné než Azure cloudu

Při migraci virtuální pevný disk z jiných - Azure cloudového úložiště do Azure, musíte nejdřív exportovat virtuálního pevného disku místního adresáře. Zkopírujte úplný zdrojový cestu místního adresáře, kde je uložena virtuální pevný disk.

##### <a name="copy-a-vhd-from-on-premise"></a>Zkopírujte virtuálního pevného disku z místních

Virtuální pevný disk při migraci v místním prostředí, potřebujete úplný zdrojový cestu, kde je uložena virtuální pevný disk. Cesta ke zdroji může být serveru umístění nebo ve sdílené složce.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Krok 2. Vytvoření cílové umístění svého virtuální pevný disk

Vytvoření účtu úložiště pro zachování svého VHD. Při plánování místa uložení vašeho VHD je potřeba zvážit následující skutečnosti:

- Cílového úložiště účtu může být standardní nebo premium úložiště v závislosti na požadovanou aplikaci.
- Oblast účtu úložiště musí být stejný jako úložiště Premium může Azure VMs vytvoříte v oblasti poslední. Zkopírujete do nového účtu úložiště nebo chcete používat stejný účet úložiště podle vašich potřeb.
- Zkopírujte a uložte klíč účtu úložiště klienta cílového úložiště další fáze.

Důrazně doporučujeme můžete přesouvat všechna data pro pracovní zátěž výrobní používat premium úložiště.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Krok 3. Odeslání virtuálního pevného disku k základnímu úložišti Azure

Teď, když máte v místním adresáři vaší virtuální pevný disk, můžete AzCopy nebo AzurePowerShell nahrát soubor VHD k základnímu úložišti Azure. Obě možnosti jsou k dispozici tady:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Možnost 1: Nahrání souboru VHD pomocí prostředí PowerShell Azure přidat-AzureVhd

    Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>

Příklad <Uri> může být **_"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"_**. Příklad <FileInfo> může být **_"C:\path\to\upload.vhd"_**.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Možnost 2: Použití AzCopy nahrát soubor VHD

Použití AzCopy, můžete snadno nahrát virtuální pevný disk přes Internet. V závislosti na velikosti VHD může to trvat čas. Nezapomeňte zkontrolovat limitů úložiště účtu průniku/výstupní při použití tuto možnost. Podrobnosti najdete v článku [Azure úložiště škálovatelnost a výkonu cílů](storage-scalability-targets.md) .

1. Stažení a instalace AzCopy odsud: [nejnovější verzi AzCopy](http://aka.ms/downloadazcopy)
2. Otevřete Azure PowerShell a přejděte do složky, kde je nainstalovaný AzCopy.
3. Pomocí následujícího příkazu zkopírujte soubor virtuální pevný disk z "Zdrojový" k "Cílový".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Příklad:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd

    Tady je popis parametry použité v příkazu AzCopy:

 - * */Zdroje: * &lt;zdroj&gt;:*** umístění složky nebo kontejneru adresy URL, která obsahuje virtuální pevný disk.
 - * */SourceKey: * &lt;klíč účtu zdroj&gt;:*** klíč účtu úložiště účtu úložiště zdroje.
 - * */Dest: * &lt;cíl&gt;:*** adresy URL kontejneru úložiště zkopírujte virtuální pevný disk na.
 - * */DestKey: * &lt;klíč účtu cíl&gt;:*** klíč účtu úložiště klienta cílového úložiště.
 - **/BlobType: stránky:** Určuje, že je cílová objektů blob stránky.
 - * */Vzorek: * &lt;název souboru&gt;:*** zadejte název souboru virtuální pevný disk na Kopírovat.

Podrobnosti o používání nástroje AzCopy najdete v tématu [přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Další možnosti pro odeslání virtuální pevný disk

Můžete taky nahrajete virtuálního pevného disku ke svému účtu úložiště pomocí jedné z těchto způsobů:

- [Azure úložiště objektů Blob kopírovat rozhraní API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Objekty BLOB Azure úložiště Explorer nahrávání](https://azurestorageexplorer.codeplex.com/)
- [Odkaz na úložiště Import nebo Export službu REST API](https://msdn.microsoft.com/library/dn529096.aspx)

>[AZURE.NOTE] Doporučujeme používat službu Import nebo Export Pokud odhadovaná nahrávání čas je delší než 7 dnů. [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) můžete použít k odhadu času z dat velikost a systémy přepravy jednotky.
>
> Import nebo Export lze použít ke zkopírování do standardní ukládání do účtu. Musíte se ke zkopírování ze standardní ukládání do účtu úložiště premium pomocí nástroje, například AzCopy.


## <a name="create-azure-virtual-machine-using-premium-storage"></a>Vytvoření VMs Azure pomocí Premium úložiště

Když je nahráli ani zkopírování do požadovaného úložiště účtu, postupujte podle pokynů v této části můžete zaregistrovat virtuálního pevného disku jako obrázek s operačním systémem, nebo s operačním systémem disk v závislosti na vaší situaci a pak vytvořte instanci OM z něho. Datový disk virtuální pevný disk může být připojen OM po jeho vytvoření. Ukázka skriptu migrace není uvedený na konci této části. Tento jednoduchý skript neodpovídá všechny scénáře. Budete muset aktualizovat skript podle s konkrétním scénáři. Pokud tento skript platí pro nefunguje, najdete pod [Skript ukázkové migrace](#a-sample-migration-script).

### <a name="checklist"></a>Kontrolní seznam

1.  Počkejte všech discích virtuální pevný disk kopírování je dokončený.
2.  Zkontrolujte, že Premium úložiště je dostupné v oblasti, do kterého migrujete.
3.  Rozhodněte, nové OM řadu, kterou budete používat. Je třeba úložiště Premium může a velikost by měla být v závislosti na dostupnost v oblasti a podle potřeby.
4.  Rozhodněte, přesnou velikost OM, které budete používat. Musí být dostatečně velký k tomu podporuje počet disků dat, ke kterým máte OM velikost. Například Pokud máte 4 disků data, musí mít OM jádra 2 nebo další. Zvažte taky, jestli zpracování power paměti a je třeba šířka pásma.
5.  Vytvořte účet Premium úložiště v cílové oblasti. Toto je účet, který chcete použít pro nová OM.
6.  Máte po ruce, včetně seznamu disků a odpovídající objektů BLOB virtuální pevný disk aktuální OM podrobnosti.

Příprava aplikace výpadku. K provedení migrace vyčistit, budete muset ukončit všechny zpracování v aktuálním systému. Teprve potom můžete získat konzistentního stavu, které můžete migrovat na novou platformu. Doba trvání prostoje závisí na množství dat v disků migrace.

>[AZURE.NOTE] Pokud vytváříte OM správce prostředků Azure z disku specializované virtuálního pevného disku, získáte [Tato šablona](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) pro nasazení Správce prostředků OM pomocí existující disk.

### <a name="register-your-vhd"></a>Zaregistrujte svůj virtuální pevný disk

Vytvoření virtuálního počítače z OS virtuálního pevného disku nebo připojit data disk do nové OM, je nutné je zaregistrovat. Postupujte podle kroků v závislosti na vaší virtuální pevný disk scénáři.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Generalized virtuální pevný disk operační systém vytvořit několika instancích spuštěných OM Azure

Po generalized OS obrázek virtuální pevný disk se k účtu úložiště zaregistrujte jako **Azure OM obrázek** tak, že můžete vytvořit jednu nebo více instancí OM z něho. Pomocí následující rutiny prostředí PowerShell registrovat váš virtuální pevný disk jako obrázek OS OM Azure. Zadejte adresu URL dokončení kontejneru místo, kam se zkopíroval virtuální pevný disk na.

    Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows

Zkopírujte a uložte název nového obrázku OM Azure. Ve výše uvedeném příkladu je *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Jedinečný operační systém virtuální pevný disk vytvořit jednou instancí OM Azure

Po jedinečné virtuální pevný disk OS se k účtu úložiště zaregistrujte jako **Azure OS disku** tak, že vytvoříte instanci OM z něho. Pomocí těchto rutin prostředí PowerShell registrovat váš virtuální pevný disk jako Azure OS Disk. Zadejte adresu URL dokončení container které byl zkopírovány virtuální pevný disk.

    Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"

Zkopírujte a uložte název nové Disk OS Azure. Ve výše uvedeném příkladu je *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Data disk virtuální pevný disk připojí se k nové instance OM Azure

Po datový disk virtuální pevný disk se k účtu úložiště zaregistrujte jako Disk dat Azure tak, aby ho může být připojen nová Pošta řadách, DSv2 řady nebo GS řady Azure OM instance.

Pomocí těchto rutin prostředí PowerShell registrovat váš virtuální pevný disk jako Disk dat Azure. Zadejte adresu URL dokončení kontejneru místo, kam se zkopíroval virtuální pevný disk na.

    Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"

Zkopírujte a uložte název nové Disk dat Azure. Ve výše uvedeném příkladu je *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Vytvoření úložiště Premium může OM

Jednou obrázek s operačním systémem nebo jsou registrované OS disku, vytvořit nové DS řady, DSv2 řady nebo OM GS řady. Operační systém obrázek nebo název disku operační systém, kterou jste registrovali budete používat. Vyberte typ OM osy Premium úložiště. V následujícím příkladu používáme velikost OM *Standard_DS2* .

>[AZURE.NOTE] Aktualizujte velikost disku, aby zkontrolovala, jestli že odpovídá požadavky na výkon a velikosti Azure disku a kapacity.

Postupujte podle níže k vytvoření nové OM rutiny prostředí PowerShell krok za krokem. Nejdřív nastavte běžné parametry:

    $serviceName = "yourVM"
    $location = "location-name" (e.g., West US)
    $vmSize ="Standard_DS2"
    $adminUser = "youradmin"
    $adminPassword = "yourpassword"
    $vmName ="yourVM"
    $vmSize = "Standard_DS2"

Nejprve vytvořte do cloudové služby, ve kterém bude hostitelem vaší nové VMs.

    New-AzureService -ServiceName $serviceName -Location $location

Dále v závislosti na nefunguje, vytvořte instance OM Azure z OS obrázku nebo OS disku, kterou jste registrovali.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Generalized virtuální pevný disk operační systém vytvořit několika instancích spuštěných OM Azure

Vytvořte jeden nebo více nových DS řady Azure OM instancí **Obrázek s operačním systémem Azure** , kterou jste registrovali. Zadejte tento název OS obrázek v konfiguraci OM při vytváření nové OM, jak je ukázáno v následujícím příkladu.

    $OSImage = Get-AzureVMImage –ImageName "OSImageName"

    $vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

    Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

    New-AzureVM -ServiceName $serviceName -VM $vm

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Jedinečný operační systém virtuální pevný disk vytvořit jednou instancí OM Azure

Vytvoření nové DS řady Azure OM instance pomocí **Azure OS disku** , které je zaregistrované. Zadejte název tohoto disku OS v konfiguraci OM při vytváření nové OM, jak je ukázáno v následujícím příkladu.

    $OSDisk = Get-AzureDisk –DiskName "OSDisk"

    $vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

    New-AzureVM -ServiceName $serviceName –VM $vm

Zadejte další Azure OM informace, například cloudové služby, oblasti, účtu úložiště, sadu dostupnost a ukládání do mezipaměti zásad. Všimněte si, že instance OM se musí nacházet společně s přidruženými operačních systémů nebo disků data tak vybrané cloudové služby, oblast a úložiště účet musí být ve stejném umístění jako podkladová VHD tyto disků.

### <a name="attach-data-disk"></a>Připojení dat disku

Nakonec pokud jste si zaregistrovali datový disk VHD, je připojte k nové úložiště Premium může Azure OM.

Pomocí následující rutiny prostředí PowerShell připojte data disk do nové OM a určete zásada ukládání do mezipaměti. V následujícím příkladu je mezipaměti zásady nastaveno *jen pro čtení*.

    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

    Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

    Update-AzureVM  -VM $vm

>[AZURE.NOTE] Doména může obsahovat další kroky potřebné k podpoře aplikace, která je dál nebyl vztahuje tato příručka.

### <a name="checking-and-plan-backup"></a>Kontrola a plánování zálohování

Jakmile nové OM začátcích, přístup pomocí stejné přihlašovací id a heslo je jako původní OM a přesvědčte se, že všechno funguje očekávaným způsobem. Všechna nastavení, včetně prokládané svazky by měly tvořit nové OM.

Posledním krokem je plánování zálohování a údržba harmonogram nové OM podle potřeby aplikace.

### <a name="a-sample-migration-script"></a>Ukázka skriptu migrace

Pokud máte víc VMs migrace, budou automatizaci pomocí skriptů Powershellu užitečné. Toto je ukázkový skript, který umožňuje automatizovat migrace virtuálního počítače. Poznámku pod skript, který je pouze příklad a existuje několik předpoklady o aktuální OM discích. Budete muset aktualizovat skript podle s konkrétním scénáři.

Předpoklady jsou:

- Při vytváření klasické VMs Azure.
- Zdroje s operačním systémem disků a zdrojových dat disků se týkají stejný účet úložiště a stejné kontejner. Není-li disků OS a disků dat na stejném místě, můžete AzCopy nebo Azure PowerShell VHD zkopírovat úložiště účty a kontejnery. Podívejte se na předchozí krok: [Virtuální pevný disk kopie s AzCopy nebo Powershellu](#copy-vhd-with-azcopy-or-powershell). Úprava tento skript ke splnění nefunguje je jinou, ale doporučujeme používat AzCopy nebo prostředí PowerShell takové alternativní řešení je snadněji a rychleji.

Skript automatizace je uveden níže. Nahrazení textu s informacemi a aktualizovat skript podle s konkrétním scénáři.

>[AZURE.NOTE] Použití existujícího skriptu nezachová konfigurace sítě zdroje OM. Je třeba nové konfigurace nastavení sítě na migrované VMs.

    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location “Southeast Asia”

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you’d like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location

#### <a name="optimization"></a>Optimalizace

Aktuální konfigurace OM může být přizpůsobené dobře fungují s standardní discích. Například pro zvýšení výkonu pomocí mnoho disků prokládané objemu. Třeba místo použití 4 disků samostatně úložný prostor Premium, můžete optimalizovat náklady tím, že jeden disk. Optimalizace, jako je tento potřeba přistupovat na základě případ a vyžadují vlastní kroky po migraci. Všimněte si také, že tento proces nemusí taky pro databáze a aplikace, které jsou závislé na disku rozložení definována v nastavení.

##### <a name="preparation"></a>Příprava

1.  Dokončení jednoduché migrace podle popisu v předchozí části. Optimalizace proběhne na nové OM po migraci.
2.  Zadat nové formáty disku potřebné pro optimalizaci konfigurace.
3.  Určení mapování objemu aktuální disků/do nového specifikace disku.

##### <a name="execution-steps"></a>Postup provádění

1.  Vytvoření nových disků s správné velikosti na úložiště OM Premium.
2.  Přihlaste se ke OM a kopírovat data z aktuálně svazku nový disk, který odpovídá svazku. Postup pro všechny aktuální svazky, které je potřeba namapovat na nový disk.
3.  Potom nastavením možností aplikace přepněte do nových disků a odpojení staré svazky.

Optimalizace aplikace pro lepší výkon, získáte pro [Optimalizaci výkonu aplikace](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Aplikací migrací

Databáze nebo jiných aplikacích pro složité může vyžadovat zvláštní kroky definované poskytovatele aplikace pro migraci. Zkontrolujte informace v nápovědě k dané aplikace. Například Obvykle lze migrovat databáze pomocí zálohování a obnovení.

## <a name="next-steps"></a>Další kroky

Najdete v těchto tématech konkrétních situacích pro migraci virtuálních počítačích:

- [Migrace Azure virtuálních počítačích mezi účty úložiště](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
- [Vytvořte a uložte virtuálního pevného disku serveru Windows Azure.](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md)
- [Vytváření a nahrávání virtuální pevný Disk, která obsahuje Linux operační systém](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md)
- [Migrace virtuálních počítačích z Amazon AWS na Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Viz také v následujících zdrojích zobrazíte další informace o úložišti Azure a Azure virtuálních počítačích:

- [Azure úložiště](https://azure.microsoft.com/documentation/services/storage/)
- [Azure virtuálních počítačích](https://azure.microsoft.com/documentation/services/virtual-machines/)
- [Úložiště Premium: Výkonné úložiště pro pracovního vytížení Azure virtuálního počítače](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
