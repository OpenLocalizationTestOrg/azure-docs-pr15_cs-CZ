<properties
    pageTitle="Akce: v případě výpadku úložišti Azure | Microsoft Azure"
    description="Akce: v případě výpadku úložišti Azure"
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Co dělat, když dojde k úložišti Azure výpadek

Společnost Microsoft pracujeme pevně aby zkontrolovala, jestli že jsou vždy dostupné služeb společnosti Microsoft. V některých případech vynutí mimo naši vliv řízení nám způsoby, které způsobují služby neplánované výpadků v jedné nebo více oblastí. Můžete dělat tyto méně častých výskytů, nabízíme následující pokyny uceleném služby Azure úložiště.

## <a name="how-to-prepare"></a>Jak připravit 

Je důležité pro každý zákazníkovi Příprava vlastní havárie obnovení plánu. Úsilí o obnovení z úložiště výpadku obvykle zahrnuje operace personál a automatické postupy pro opětovnou aktivaci aplikace ve funkční stavu. Získáte Azure si přečtěte následující dokumentaci dole můžete vytvořit vlastní havárie obnovení plán:

-   [Obnovení havárie a dostupnost pro Azure aplikace](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)

-   [Azure odolnost proti chybám příručku](../resiliency/resiliency-technical-guidance.md)

-   [Služby Azure obnovení webu](https://azure.microsoft.com/services/site-recovery/)

-   [Azure replikace úložiště](storage-redundancy.md)

-   [Služby Azure zálohování](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Vyhledání 

Doporučené postupy k určení stav služby Azure, je přihlášení k odběru [Řídicí panel stavu služby Azure](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Co dělat, když výpadek úložiště

Pokud jeden nebo více úložiště služby se dočasně nedostupný v jedné nebo více oblastí, existují dva způsoby trocha zamyšlení nad prezentacemi. Pokud okamžitý přístup k datům, zvažte možnost 2.

### <a name="option-1-wait-for-recovery"></a>Možnost 1: Počkejte obnovení

V tomto případě není nutná žádná akce na druhé straně. Pracujeme pečlivě obnovíte dostupnost Azure služby. Můžete sledovat stav služby na [Řídicí panel stavu služby Azure](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Možnost 2: Zkopírování dat z sekundární

Pokud jste se rozhodli [geo nadbytečné úložiště přístup pro čtení (Vzdálená pomoc GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (doporučeno) pro účty úložiště, bude mít přístup pro čtení k datům z oblasti sekundární. Nástroje můžete použít jako [AzCopy](storage-use-azcopy.md) [Azure PowerShell](storage-powershell-guide-full.md)a [Přesun dat Azure knihovny](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) kopírovat data z oblasti sekundární do jiného účtu úložiště v unimpacted oblasti a potom na položku aplikace k tomuto účtu úložiště pro obě čtení a zápis dostupnosti.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Co můžete očekávat, pokud dojde k selhání úložiště

Pokud jste se rozhodli [Geo nadbytečné úložiště (GRS)](storage-redundancy.md#geo-redundant-storage) nebo [geo nadbytečné úložiště přístup pro čtení (Vzdálená pomoc GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (doporučeno), úložišti Azure zachová data trvalá ve dvou oblastí (primárních a sekundárních). V obou oblastí úložišti Azure neustále udržuje více kopie vaše data.

Když selhání místní blokování automaticky otevíraných primární oblast, nejdřív pokusíme obnovíte službu v dané oblasti. Závisí na druhu ke katastrofě a jeho dopady v některých výjimečně jsme nemusí nebude možné obnovit primární oblast. V tomto okamžiku jsme provede geo přepojení. Replikace křížově oblast dat je asynchronní proces, který bude to vyžadovat zpoždění, takže je možné, že změny, které nebyly dosud replikovat na vedlejší oblast může dojít ke ztrátě. ["Poslední synchronizace" účtu úložiště](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) k získání informací o stavu replikace můžete dotaz.

Několik bodů týkajících se prostředí geo převzetí úložiště:

-   Geo řešení selhání úložiště bude pouze aktivován týmem úložišti Azure – je požadována žádná akce zákazníka.

-   Existující koncových bodů služby úložiště objektů BLOB, tabulek, dotazů a soubory zůstane po překlopení; položka DNS bude třeba aktualizovat Pokud chcete přepnout snímání z oblasti primární sekundární oblasti.

-   Před a při geo selhání nebudete mít přístup pro zápis ke svému účtu úložiště kvůli dopad katastrofě ale můžete pořád číst z sekundární Pokud nakonfiguroval účtu úložiště jako GRS Vzdálená pomoc.

-   Po dokončení geo převzetí a šíření změn DNS, bude pokračovat pro čtení a zápis ke svému účtu úložiště. Můžete dotaz ["Geo převzetí posledního" účtu úložiště](https://msdn.microsoft.com/library/azure/ee460802.aspx) k získání dalších podrobností.

-   Po překlopení účtu úložiště bude plně funkční, ale v "jejich funkčnost bude omezena" stav ZobrazitFormulář.aspx skutečně hostuje v samostatné oblasti s není možné geo replikace. Pokud chcete pomoci tato rizika zmírnit, jsme bude obnovit původní primárního oblasti a poté proveďte geo navrácení obnovit původní stav. Pokud je obnovit původní primární oblast, jsme přidělí jiné sekundární oblasti.
Podrobné informace o infrastruktury replikace geo Azure úložiště získáte v článku na blogu týmu úložiště o [možnostech redundance a Vzdálená pomoc GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

##<a name="best-practices-for-protecting-your-data"></a>Osvědčené postupy při ochraně dat

Existuje několik doporučených postupů k obecnějším údajům ukládání dat v pravidelných intervalech.

-   OM disků – pomocí [služby Azure zálohování](https://azure.microsoft.com/services/backup/) k obecnějším údajům OM disků používaný Azure virtuálních počítačích.

-   Objekty BLOB blok – vytvoření [snímku](https://msdn.microsoft.com/library/azure/hh488361.aspx) každý blok blob nebo kopírování objektů BLOB k základnímu úložišti jiného účtu v jiné oblasti pomocí [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)nebo [Přesun dat Azure knihovny](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).

-   Tabulky – umožňuje [AzCopy](storage-use-azcopy.md) exportu dat tabulky do jiného účtu úložiště v jiné oblasti.

-   Soubory – slouží ke kopírování souborů na jiný účet úložiště v jiné oblasti [AzCopy](storage-use-azcopy.md) nebo [Azure Powershellu](storage-powershell-guide-full.md) .
