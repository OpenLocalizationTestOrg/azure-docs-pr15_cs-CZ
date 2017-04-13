<properties
    pageTitle="Shromáždit protokoly a metriky dat pomocí analýzy úložiště | Microsoft Azure"
    description="Úložiště analýzy umožňuje sledovat metriky data pro všechny služby úložiště a shromáždit protokoly pro úložiště objektů Blob fronty a tabulky."
    services="storage"
    documentationCenter=""
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

# <a name="storage-analytics"></a>Úložiště analýzy

## <a name="overview"></a>Základní informace

Azure technologie pro analýzu úložiště provádí protokolování a poskytuje metriky data účtu úložiště. Tato data umožňuje sledovat žádosti o analýze trendů použití a Diagnostika problémů s účtem úložiště.

Použití analýzy úložiště, musíte povolit ho jednotlivě pro každou službu, kterou chcete sledovat. Můžete ji povolit z [Portálu Azure](https://portal.azure.com). Další informace najdete v tématu [sledování úložiště účtu na portálu Azure](storage-monitor-storage-account.md). Můžete taky povolit úložiště analýzy programově prostřednictvím rozhraní REST API nebo do jiné knihovny klienta. Umožňuje povolit analýzu úložiště pro každou službu operace [Získat vlastnosti služby objektů Blob](https://msdn.microsoft.com/library/hh452239.aspx), [Získat vlastnosti služby fronty](https://msdn.microsoft.com/library/hh452243.aspx), [Získat tabulku služba vlastnosti](https://msdn.microsoft.com/library/hh452238.aspx)a [Získat vlastnosti souboru služby](https://msdn.microsoft.com/library/mt427369.aspx) .

Souhrnné údaje je uložený ve známý objektů blob (pro protokolování) a dobře známé tabulkách (metriky), které může se otevřít prostřednictvím služby objektů Blob a tabulku služba rozhraní API.

Úložiště analýzy má omezení 20 TB na množství uložená data, která jsou nezávislé na celkové limit úložiště účtu. Další informace o fakturaci a zásady uchovávání informací dat najdete v článku [technologie pro analýzu úložiště a fakturaci](https://msdn.microsoft.com/library/hh360997.aspx). Další informace o omezeních úložiště účtu najdete v článku [Azure úložiště škálovatelnost a výkonu cílů](storage-scalability-targets.md).

Podrobný průvodce k používání technologie pro analýzu úložiště a další nástroje určit, Diagnostika a řešení potíží s problémy související s Azure úložiště najdete v tématu [sledování, Diagnostika a řešení potíží s úložišti tabulek Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="about-storage-analytics-logging"></a>Informace o protokolování úložiště analýzy

Úložiště analýzy protokoly podrobné informace o požadavcích úspěšné a neúspěšné ke službě úložiště. Tyto informace slouží ke sledování jednotlivé požadavky a diagnostikovat potíže se službou úložiště. Žádosti o přihlášení k lyncu na základě nejlepší dostupné.

Položky protokolu vzniká jenom v případě, že je aktivity úložiště služby. Například pokud úložiště účet aktivity v jeho službě objektů Blob, ale ne v tabulce nebo fronty služeb, pouze protokoly vztahující se ke službě objektů Blob se vytvoří.

Zapnout protokolování analýzy úložiště není k dispozici pro službu Azure soubor.

### <a name="logging-authenticated-requests"></a>Žádosti o protokolování ověření

Následující typy ověřené žádosti o přihlášení k lyncu:

- Požadavky na úspěšné.

- Nejde požadavků, včetně časový limit, omezení, sítě, se tak mohli ověřovat a další chyby.

- Požadavky na používání sdílené přístup podpisu (přidružení zabezpečení), včetně žádosti o nezdařeném uložení a úspěšných.

- Požadavky na technologie pro analýzu dat.

Požadavky na úložiště technologie pro analýzu, například protokolu vytvoření nebo odstranění nejste přihlášení k lyncu. Úplný seznam dat protokolu jsou popsány v tématech [přihlášení k Lyncu skladování technologie pro analýzu a zprávy o stavu](https://msdn.microsoft.com/library/hh343260.aspx) a [Formát protokolu analýzy úložiště](https://msdn.microsoft.com/library/hh343259.aspx) .

### <a name="logging-anonymous-requests"></a>Protokolování anonymní požadavky
Následující typy anonymní žádosti o přihlášení k lyncu:

- Požadavky na úspěšné.

- Chyby serveru.

- Chyby vypršení časového limitu pro klienta a serveru.

- Selhalo požadavky GET s kódem chyby 304 (změněno).

Všechny ostatní selhalo anonymní žádosti o nejste přihlášení k lyncu. Úplný seznam dat protokolu jsou popsány v tématech [přihlášení k Lyncu skladování technologie pro analýzu a zprávy o stavu](https://msdn.microsoft.com/library/hh343260.aspx) a [Formát protokolu analýzy úložiště](https://msdn.microsoft.com/library/hh343259.aspx) .

### <a name="how-logs-are-stored"></a>Ukládání protokolů
Všechny protokoly jsou uloženy v bloku objektů BLOB v kontejneru s názvem $logs, který se automaticky vytvoří při zapnuté funkci analýzy úložiště účtu úložiště. Kontejneru $logs se nachází v názvů objektů blob účtu úložiště, třeba: `http://<accountname>.blob.core.windows.net/$logs`. Tento kontejner nelze odstranit po povolení analýzy úložiště, když obsah se dá odstranit.

>[Azure.NOTE] Kontejner $logs nezobrazuje při provádění kontejneru výpis operace, například metodu [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) . Je nutné přistupovat přímo. Například, použijte metodu [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) pro přístup k objektů BLOB v `$logs` kontejner.
Podle žádosti o přihlášení, úložiště analýzy nahrajte intermediate výsledky jako bloky. Technologie pro analýzu úložiště pravidelně, bude potvrdit tyto bloky a zpřístupnit je tak jako objekt blob.

Duplicitní záznamy může existovat protokolů vytvořených v stejné hodinu. Můžete určit, pokud záznamu je duplicitní kontrolou číslo **ID žádosti** a **funkce** .

### <a name="log-naming-conventions"></a>Přihlaste se konvence pro zadávání názvů
Každý protokolu odesílán, v následujícím formátu.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

Následující tabulka popisuje jednotlivé atributy do pole název protokolu.

| Atribut         | Popis                                                                                                                                                                                   |
|----------------   |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   |
| < název služby >    | Název služba úložiště. Příklad: objektů blob, tabulku nebo fronty.                                                                                                                          |
| YYYY              | Čtyři číslice roku protokol. Příklad: 2011.                                                                                                                                           |
| MM                | Měsíc Dvojmístné protokolu. Příklad: 07.                                                                                                                                             |
| DD                | Měsíc Dvojmístné protokolu. Příklad: 07.                                                                                                                                             |
| [hh                | Dvojmístné hour, která označuje počáteční hodinu u protokolů ve 24hodinovém formátu UTC. Příklad: 18.                                                                                     |
| mm                | Dvojmístné číslo označující počáteční minuty protokolů. Tato hodnota není podporována v aktuální verzi úložiště technologie pro analýzu a jeho hodnotu vždycky 00.     |
| <counter>         | Od nuly čítač se šesti číslic, která označuje počet objektů BLOB protokol vytvořený pro službu úložiště za hodinu časové období. Čítač začíná 000000. Příklad: 000001.     |

Následující obrázek je celá ukázková protokolu název, který kombinuje předchozích příkladů.

    blob/2011/07/31/1800/000001.log

Následuje příklad identifikátor URI, který lze použít pro přístup k předchozí protokolu.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Při přihlášení žádost úložiště odpovídá názvu výsledné protokolu hodinu dokončení požadovanou operaci. Například, pokud žádost o GetBlob byl dokončen na 6:30 odp. na 7 31 / / 2011 by měly zapisovat protokol s předponou takto:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Metadat protokolu
Všechny objekty BLOB protokolu jsou uloženy s metadaty, který umožňuje určit protokolování data, která obsahuje objektů blob. Následující tabulka popisuje jednotlivé atribut metadat.

| Atribut     | Popis                                                                                                                                                                                                                                                   |
|------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| LogType       | Popisuje, zda protokol obsahuje informace, které se týkají pro čtení, psaní nebo odstraňování. Tato hodnota můžete zahrnout jeden typ nebo kombinací všechny tři oddělte je čárkami. Příklad 1: psaní; Příklad 2: Číst, zapsat; Příklad 3: Číst, zapisovat, odstranit.    |
| Čas spuštění     | Počáteční čas položky v protokolu ve formě YYYY-MM-DDThh:mm:ssZ. Příklad: 2011-07-31T18:21:46Z.                                                                                                                                             |
| Čas_ukončení       | Poslední čas položky v protokolu ve formě YYYY-MM-DDThh:mm:ssZ. Příklad: 2011-07-31T18:22:09Z.                                                                                                                                               |
| LogVersion    | Verze formát protokolu. Aktuálně podporované hodnota je pouze 1.0.                                                                                                                                                                                     |

Následující seznam zobrazuje metadata celá ukázková pomocí předchozích příkladů.

- LogType = zápis

- Čas spuštění = 2011-07-31T18:21:46Z

- Čas_ukončení = 2011-07-31T18:22:09Z

- LogVersion = 1.0

### <a name="accessing-logging-data"></a>Přístup k datům protokolování

Všechna data v `$logs` kontejner můžete přistupovat pomocí služby objektů Blob rozhraní API, včetně rozhraní API .NET poskytovanou Azure spravovaných knihovny. Účet správce úložiště můžete číst a odstranit protokoly, ale nelze vytvořit nebo aktualizovat. Metadata do protokolu a název protokolu lze při dotazování na protokol. Je možné pro zadané hodiny protokoly zobrazit nefunguje, ale metadata vždy určuje timespan položky protokolu v protokol. Kombinace názvy protokolu a metadata proto, můžete při hledání určité protokol.

## <a name="about-storage-analytics-metrics"></a>Informace o metriky úložišť analýzy

Úložiště analýzy mohou být uloženy metrik zahrnující agregovanou transakce Statistika a kapacita dat o žádosti ke službě úložiště. Transakce jsou uvedeny na úrovni operace rozhraní API i na úrovni úložiště služby a kapacita vykázaného na úrovni úložiště služby. Metriky dat lze analýza využití úložiště služby, Diagnostika problémů s žádosti proti služba úložiště a aby se zvýšil výkon aplikace pomocí služby.

Použití analýzy úložiště, musíte povolit ho jednotlivě pro každou službu, kterou chcete sledovat. Můžete ji povolit z [Portálu Azure](https://portal.azure.com). Další informace najdete v tématu [sledování úložiště účtu na portálu Azure](storage-monitor-storage-account.md). Můžete taky povolit úložiště analýzy programově prostřednictvím rozhraní REST API nebo do jiné knihovny klienta. Umožňuje povolit analýzu úložiště pro každou službu operace **Získat vlastnosti služby** .

### <a name="transaction-metrics"></a>Metriky transakce

Robustní sadu dat nastavené intervalech hodinové nebo minut pro každou službu úložiště požadované rozhraní API operace, včetně průniku/výstupní, dostupnost, chyby a zařazené do kategorií žádost o procent. Můžete podívat na celý seznam transakce informace v tématu [Schématu tabulky metriky analýzy úložiště](https://msdn.microsoft.com/library/hh343264.aspx) .

Data transakce nastavené na dvou úrovních – úroveň služeb a rozhraní API operace úroveň. Na úrovni služby statistiky shrnutí všechna požadovaná rozhraní API operace jsou došlo k zápisu tabulky entity každou hodinu i v případě provedené žádné žádosti o služby. Na úrovni operace rozhraní API statistiky jsou pouze došlo k zápisu entita Pokud operace bylo požadováno do této hodiny.

Například pokud provedení operace **GetBlob** službě objektů Blob metriky úložišť analýzy protokolu žádosti a zahrnout do souhrnná data pro službu objektů Blob jak operaci **GetBlob** . Ale pokud žádná **GetBlob** operace žádá za hodinu, entita nesmí být došlo k zápisu `$MetricsTransactionsBlob` pro tuto operaci.

Metriky transakce se zaznamenávají pro koncových uživatelů a žádosti úložiště analýzy samotné. Například se zaznamenávají požadavky analýzy úložiště k zápisu protokolů a tabulky entity. Další informace o tom, jak je faktura tyto požadavky najdete v článku [technologie pro analýzu úložiště a fakturaci](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Metriky kapacity

>[AZURE.NOTE] V současné době kapacita metriky jsou k dispozici pouze pro službu objektů Blob. Kapacity metriky pro tabulku služba a služba fronty budou k dispozici v budoucích verzích úložiště analýzy.

Kapacity dat nastavené denně účtu úložiště objektů Blob službě a napsali dvě tabulky entity. Jedna osoba obsahuje statistiky pro uživatelská data, a druhý statistických údajů o `$logs` používané technologie pro analýzu úložiště objektů blob kontejner. `$MetricsCapacityBlob` Tabulka obsahuje následující Statistika:

- **Kapacita**: velikost úložiště použita službou účtu úložiště objektů Blob, v bajtech.

- **ContainerCount**: počet objektů blob kontejnery účtu úložiště objektů Blob služby.

- **ObjectCount**: počet potvrzené a nepotvrzené blok nebo stránku objektů BLOB účtu úložiště objektů Blob služby.

Další informace o metriky kapacita najdete v tématu [Úložiště technologie pro analýzu metriky tabulky schéma](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Uložení metriky

Všechna data metriky pro každou z úložiště služby uložený ve tři tabulky vyhrazená pro službu: jedné tabulky transakce informace, jednu tabulku minute transakce informace a druhou tabulku pro informace o kapacity. Informace o transakce transakce a minuty se skládá z žádostí a odpovědí data a informace o kapacity se skládá z úložiště použití zásad správy informací. Metriky hodiny, minute metriky a kapacita účtu úložiště objektů Blob službě můžete k nim získat přístup v tabulkách, které jsou s názvem popsané v následující tabulce.

| Metriky úrovně                         | Názvy tabulek                                                                                                                   | Podporované verze                                                                                                                        |
|------------------------------------   |-----------------------------------------------------------------------------------------------------------------------------  |---------------------------------------------------------------------------------------------------------------------------------------------- |
| Každou hodinu metriky primární umístění      |  $MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue                                                  | Verze před 2013-08-15 pouze. Když tyto názvy nadále podporuje, je vhodné přechod k používání níže uvedené tabulky.  |
| Každou hodinu metriky primární umístění      | $MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue               | Všechny verze, včetně 2013-08-15.                                                                                                               |
| Minuta metriky primární umístění      | $MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue         | Všechny verze, včetně 2013-08-15.                                                                                                               |
| Každou hodinu metriky sekundární umístění    | $MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue         | Všechny verze, včetně 2013-08-15. Musí být povolený geo nadbytečné replikace přístup pro čtení.                                                    |
| Minuta metriky sekundární umístění    | $MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue   | Všechny verze, včetně 2013-08-15. Musí být povolený geo nadbytečné replikace přístup pro čtení.                                                    |
| Kapacita (pouze objektů Blob služba)          | $MetricsCapacityBlob                                                                                                          | Všechny verze, včetně 2013-08-15.                                                                                                               |


V této tabulce jsou vytvářeny automaticky při zapnuté funkci analýzy úložiště účtu úložiště. K nim prostřednictvím obor názvů klienta úložiště, třeba:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Přístup k datům metriky

Všechna data v tabulkách metriky můžete přistupovat pomocí tabulku služba rozhraní API, včetně rozhraní API .NET poskytovanou Azure spravovaných knihovny. Účet správce úložiště můžete číst a odstranit tabulku entity, ale nelze vytvořit nebo aktualizovat je.

## <a name="billing-for-storage-analytics"></a>Fakturace pro analýzy úložiště

Technologie pro analýzu úložiště je povolený účtu vlastníkem úložiště; ve výchozím nastavení není povoleno. Všechna data metriky píše služby účet úložiště. Každý zápisu operace analýzy úložiště je fakturaci. Kromě toho je velikost úložiště používaný metriky data také fakturaci.

Jsou tyto akce provádí úložiště analýzy fakturaci:

- Požadavky na vytvoření objektů blob pro přihlášení. 

- Požadavky na vytvoření tabulky entity metriky.

Pokud jste nakonfigurovali zásady uchovávání informací, můžete nejsou účtovaná za odstranit transakce úložiště analýzy odstraní původní protokolování a metriky data. Odstranit transakce z klienta se však fakturaci. Další informace o zásady uchovávání informací najdete v tématu [nastavení zásad uchovávání informací úložiště technologie pro analýzu dat](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Principy žádosti o fakturaci

Každý žádost účtu úložiště služby je fakturaci nebo Nefakturovatelný. Úložiště analýzy protokoly každý jednotlivé žádost do služby, včetně stav zprávu, která označuje zpracování požadavku. Podobně úložiště analýzy ukládá metriky pro službu a rozhraní API operace tuto službu, včetně procentní sazby a počtu určité zprávy o stavu. Dohromady tyto funkce můžete analyzovat žádostech o fakturaci, vylepšování na aplikace a Diagnostika problémů s žádostí o služby. Další informace o fakturaci najdete v článku [Principy Azure úložiště fakturace - šířky pásma, transakce a kapacity](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Při pohledu úložiště technologie pro analýzu dat, můžete zjistit, jaké požadavky jsou fakturaci tabulek v tématu [přihlášení k Lyncu skladování technologie pro analýzu a zprávy o stavu](https://msdn.microsoft.com/library/azure/hh343260.aspx) . Pak můžete porovnat protokoly a metriky dat do zprávy o stavu zobrazíte, pokud se vám účtují konkrétního požadavku. Můžete také tabulek v předchozím tématu zjistit dostupnost úložiště služby nebo jednotlivé rozhraní API operace.

## <a name="next-steps"></a>Další kroky

### <a name="setting-up-storage-analytics"></a>Nastavení technologie pro analýzu úložiště
- [Sledování úložiště účtu na portálu Azure](storage-monitor-storage-account.md)
- [Povolení a konfigurace úložiště analýzy](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Protokolování analýzy úložiště  
- [Informace o protokolování analýzy úložiště](https://msdn.microsoft.com/library/hh343262.aspx)
- [Formát protokolu analýzy úložiště](https://msdn.microsoft.com/library/hh343259.aspx)
- [Úložiště analýzy přihlášení operací a zprávy o stavu](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Metriky úložišť analýzy
- [Informace o metriky úložišť analýzy](https://msdn.microsoft.com/library/hh343258.aspx)
- [Úložiště analýzy metriky tabulky schématu](https://msdn.microsoft.com/library/hh343264.aspx)
- [Úložiště analýzy přihlášení operací a zprávy o stavu](https://msdn.microsoft.com/library/hh343260.aspx)  
