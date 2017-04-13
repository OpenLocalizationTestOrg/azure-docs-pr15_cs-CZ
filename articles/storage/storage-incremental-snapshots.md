<properties
    pageTitle="Použití přírůstková snímků pro zálohování a obnovení Azure virtuálních počítačích | Microsoft Azure"
    description="Vytvořte vlastní řešení pro zálohování a obnovení disků Azure virtuálního počítače pomocí přírůstková snímky."
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
    ms.date="10/18/2016"
    ms.author="aungoo"/>


# <a name="back-up-azure-virtual-machine-disks-with-incremental-snapshots"></a>Obecnějším údajům disků Azure virtuálního počítače s přírůstková snímky

## <a name="overview"></a>Základní informace

Azure úložiště poskytuje možnost pořizovat snímky objektů BLOB. Snímky zachycení stavu objektů blob v daném okamžiku. V tomto článku se popisu scénáři jak zachovat zálohy disků virtuálního počítače pomocí snímky. Tato metoda slouží vyberete nepoužít Azure zálohování a obnovení přihlašovacích údajů a chcete vytvořit vlastní záložní strategie pro disků virtuálního počítače.

Azure virtuálního počítače disků uložená jako objekty BLOB stránky v úložišti Azure. Protože jsme mluvíte o strategie zálohování disků virtuálního počítače v tomto článku, jsme být odkazující na snímky v souvislosti s objekty BLOB stránky. Další informace o snímků najdete v nápovědě k [Vytvoření snímku objektů Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Co je snímek?

Snímek objektů blob je jen pro čtení verze objektů blob zachycené v určitém bodě v čase. Po vytvoření snímek ho můžete číst, zkopírovali, nebo odstranit, ale nebyly změněny. Snímky lze k obecnějším údajům objektů blob zobrazená v časovém okamžiku. Až ZBÝVAJÍCÍ verzi 2015 04 05 jste měli možnost kopírovat celé snímky. S ostatními verze 2015 07 08 a výše, které můžete taky zkopírovat přírůstková snímky.

## <a name="full-snapshot-copy"></a>Kopírovat celé snímku

Snímky můžete zkopírovat na jiný účet úložiště jako objektů blob zachovat zálohy základní objektů blob. Můžete taky zkopírovat snímek přes základní blob, která je podobná obnovení objektů blob do předchozí verze. Kopírování snímku je z jednoho účtu úložiště na jiný, zabere stejné místo jako objektů blob základní stránky. Kopírování celé snímky z jednoho účtu úložiště do jiného proto bude pomalé a bude taky využívat velké množství místo ve schránce cílového úložiště.

>[AZURE.NOTE] Pokud kopírujete základní objektů blob na jiné místo a snímky objektů blob nezkopírují spolu se. Podobně přepíšete základní objektů blob kopii, nemá vliv snímky přidružené základní objektů blob a zůstat beze změny pod názvem základní objektů blob.

### <a name="back-up-disks-using-snapshots"></a>Obecnějším údajům disků použití snímků

Jako záložní strategie pro disků virtuálního počítače můžete pořizovat pravidelné snímky objektů blob disku nebo stránky a zkopírujte do jiného účtu úložiště pomocí nástroje jako [Kopii Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) operace nebo [AzCopy](storage-use-azcopy.md). Snímek můžete zkopírovat do objektů blob cílovou stránku s jiným názvem. Výsledný objektů blob cílovou stránku je objektů blob zapisovatelné stránku a ne snímek. Dál v tomto článku jsme popisuje postup zálohování disků virtuálního počítače pomocí snímky.

### <a name="restore-disks-using-snapshots"></a>Obnovení disků použití snímků

Když je potřeba obnovit předchozí stabilní verzi nezaznamenávají jedním ze záložních snímků disk, můžete zkopírovat snímek přes objektů blob základní stránky. Po snímku propagované na stránku základní objektů blob, zůstane snímek ale pramene se přepíše kopii, může být číst i napsané. Dál v tomto článku jsme popisuje postup obnovit předchozí verzi disku ze své snímku.

### <a name="implementing-full-snapshot-copy"></a>Provádění kopírovat celé snímku

Implementace kopii úplné snímek podle následujících pokynů,

-   Nejdřív udělejte si snímek základní objektů blob myší [Objektů Blob snímek](https://msdn.microsoft.com/library/azure/ee691971.aspx) .
-   Zkopírujte snímku do cílového úložiště účtu pomocí [Objektů Blob kopírovat](https://msdn.microsoft.com/library/azure/dd894037.aspx).
-   Opakujte tento postup udržovat záložní kopie základní objektů blob.

## <a name="incremental-snapshot-copy"></a>Kopírovat přírůstková snímku

Nová funkce v [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) rozhraní API umožňuje mnohem lepší obecnějším údajům snímky objektů BLOB stránky nebo disků. Rozhraní API vrátí seznam změn mezi základní objektů blob a snímků. Sníží využitého místa úložiště na záložní účtu. Rozhraní API podporuje objektů BLOB stránky na Premium úložiště, stejně jako standardní úložiště. Pomocí tohoto rozhraní API, můžete teď vytvoříte rychlejší a efektivnější záložní řešení pro Azure VMs. To bude dostupná ve ZBÝVAJÍCÍ verzi 2015 07 08 nebo novější.

Přírůstková Kopírovat snímek umožňuje z jednoho účtu úložiště zkopírovat do jiného rozdílu mezi

-   Základní objektů blob a jeho snímku nebo
-   Libovolné dva snímky základní objektů blob

Za předpokladu, že jsou splněné následující podmínky,

- Objektů blob byla vytvořená na i-1-2016 nebo novější.
- Objektů blob nebyl přepíše [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) nebo [Kopírování objektů Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) mezi dva snímky.


**Poznámka**: Tato funkce je k dispozici pro Premium a standardní objektů BLOB Azure stránky.

Pokud máte vlastní záložní strategii, který používá snímky, kopírování snímky z jednoho účtu úložiště může být hodně pomalá a zpracuje spoustu úložného prostoru. Nekopírujte celý snímek k úložišti účtu můžete napsat rozdíl mezi po sobě jdoucí snímků objektů blob záložní stránky. Tímto způsobem čas příkazy Kopírovat a místa pro uložení záložní kopie značně sníží.

### <a name="implementing-incremental-snapshot-copy"></a>Provádění kopírovat přírůstková snímku

Implementace přírůstková snímek kopírovat takto,

-   Udělejte si snímek základní objektů blob pomocí [Objektů Blob snímek](https://msdn.microsoft.com/library/azure/ee691971.aspx).
-   Kopírování snímku do cílového úložiště zálohy účtu pomocí [Kopírování objektů Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx). To bude objektů blob záložní stránky. Udělejte si snímek tento objektů blob záložní stránky a uložit záložní účtu.
-   Další snímek základní objektů blob pomocí objektů Blob snímek.
-   Pokud potřebujete rozdíl mezi prvním a druhým snímky základní objektů blob pomocí [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx). Pomocí nové parametr **prevsnapshot** zadání hodnoty data a času na snímek, chcete-li získat rozdíl s. Pokud tento parametr je k dispozici, odpovědi ostatních bude obsahovat pouze stránky, které byly změněny mezi cílové snímek a předchozí snímek včetně vymazat stránek.
-   Umožňuje použít tyto změny u objektů blob záložní stránky [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) .
-   Nakonec udělejte si snímek objektů blob záložní stránky a uložte ho do účtu úložišti.

V části Další jsme se podrobněji popisují jak zachovat zálohování disků přírůstková zkopírováním snímku

## <a name="scenario"></a>Scénář

V této části jsme popisuje scénáře, která zahrnuje strategii vlastní záložní virtuálního počítače disků použití snímků.

Zvažte OM Azure DS řady s diskem P30 úložiště premium připojené. Disk P30 nazývaný *mypremiumdisk* uložený v účtu úložiště premium s názvem *mypremiumaccount*. Standardní úložiště účet s názvem *mybackupstdaccount* se použije pro ukládání zálohování *mypremiumdisk*. Chtěli jsme uchovávat snímek *mypremiumdisk* každých 12 hodin.

Další informace o vytváření účtu úložiště a disků najdete v nápovědě k [účty adresářové služby Azure o úložiště](storage-create-storage-account.md).

Další informace o zálohování Azure VMs najdete v nápovědě k [plánování OM Azure zálohy](../backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Postup Udržovat zálohy disk použití přírůstková snímků

Podle kroků popsaných níže prohlédněte snímky *mypremiumdisk* a Udržovat zálohy v *mybackupstdaccount*. Zálohování budou objektů blob standardní stránku s názvem *mybackupstdpageblob*. Kulatý záložní stránky vždy obsahovat stavu stejný jako poslední snímek *mypremiumdisk*.

1.  Nejprve vytvořte záložní stránky objektů blob disku úložiště premium. Jak postupovat, udělejte si snímek *mypremiumdisk* s názvem *mypremiumdisk_ss1*.
2.  Zkopírujte tento snímek mybackupstdaccount jako objektů blob stránku s názvem *mybackupstdpageblob*.
3.  Udělejte si snímek *mybackupstdpageblob* s názvem *mybackupstdpageblob_ss1*, pomocí [Objektů Blob snímek](https://msdn.microsoft.com/library/azure/ee691971.aspx) a uložte ho do *mybackupstdaccount*.
4.  Během zálohování vytvořit další snímek *mypremiumdisk*vyslovte *mypremiumdisk_ss2*a uložte ho do *mypremiumaccount*.
5.  Pokud potřebujete přírůstková změny mezi dva snímky, *mypremiumdisk_ss2* a *mypremiumdisk_ss1*, pomocí [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) na *mypremiumdisk_ss2* **prevsnapshot** parametr nastaven na časové razítko *mypremiumdisk_ss1*. Zapište do objektů blob záložní stránky *mybackupstdpageblob* v *mybackupstdaccount*tyto přírůstková změny. Pokud v seznamu přírůstková změny jsou odstraněné oblastí, musí být zrušeno z objektů blob záložní stránky. Pomocí [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) zapisovat přírůstková změny objektů blob záložní stránky.
6.  Udělejte si snímek záložní stránky objektů blob *mybackupstdpageblob*, s názvem *mybackupstdpageblob_ss2*. Odstranění předchozí snímek *mypremiumdisk_ss1* z účtu úložiště premium.
7.  Opakujte kroky 4 až 6 každého záložní okna. Tímto způsobem můžete udržovat zálohy *mypremiumdisk* účet standardní úložiště.

![Obecnějším údajům disk pomocí přírůstková snímky](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Postup obnovit disk ze snímků

Podle kroků popsaných níže obnoví premium disku, *mypremiumdisk* dřívějším snímkem z účtu úložišti *mybackupstdaccount*.

1.  Určení bodu v čase, který chcete obnovit premium disk. Řekněme, že se snímek *mybackupstdpageblob_ss2*, který je uložen v úložišti účtu *mybackupstdaccount*.
2.  V mybackupstdaccount zvýší snímek *mybackupstdpageblob_ss2* jako nový *mybackupstdpageblobrestored*objektů blob záložní základní stránky.
3.  Udělejte si snímek tento blob obnovení záložní stránky s názvem *mybackupstdpageblobrestored_ss1*.
4.  Kopírování objektů blob obnovená stránky *mybackupstdpageblobrestored* z *mybackupstdaccount* do *mypremiumaccount* jako nový disku premium *mypremiumdiskrestored*.
5.  Udělejte si snímek *mypremiumdiskrestored*, s názvem *mypremiumdiskrestored_ss1* týkající se uplatňování budoucí přírůstková záložní kopie.
6.  Bod DS řada OM obnovená disk *mypremiumdiskrestored* a odpojení staré *mypremiumdisk* z OM.
7.  Zahajte proces zálohování popisu v předchozí části obnovená disku *mypremiumdiskrestored*, používají *mybackupstdpageblobrestored* objektů blob záložní stránky.

![Obnovení disku ze snímků](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Další kroky

Další informace o vytvoření snímků objektů blob poznámek a plánování infrastrukturu záložní OM pomocí těchto odkazů.

- [Vytvoření snímku objektů Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx)
- [Plánování infrastrukturu záložní OM](../backup/backup-azure-vms-introduction.md)
