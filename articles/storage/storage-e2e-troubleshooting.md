<properties
    pageTitle="Ukončení až do konce Poradce při potížích s pomocí metriky úložišť Azure a protokolování, AzCopy a zprávy Analyzer | Microsoft Azure"
    description="Kurz, které demonstrují začátku do konce Poradce při potížích s Azure úložiště technologie pro analýzu, AzCopy a Microsoft zprávy Analyzer"
    services="storage"
    documentationCenter="dotnet"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Při použití metriky úložišť Azure protokolování, AzCopy a zprávy Analyzer Poradce při potížích začátku do konce

[AZURE.INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## <a name="overview"></a>Základní informace

Diagnostikování a řešení problémů s je klíčové dovedností pro vytváření a podporu klientské aplikace s úložišti tabulek Microsoft Azure. Z důvodu distribuované přírodní Azure aplikace může být složitější než v tradiční prostředí diagnostiky a Poradce při potížích s chybami a problémy s výkonem.

V tomto kurzu budeme ukazují, jak identifikovat klienta některé chyby, které můžou mít vliv na výkon a řešení těchto chyb od začátku do konce nástroje poskytované společností Microsoft a Azure úložiště, pokud chcete optimalizovat klientské aplikaci.

Tento kurz obsahuje praktických průzkum Poradce při potížích scénář začátku do konce. Podrobné koncepční Průvodce pro řešení potíží s aplikací Azure úložiště najdete v tématu [Sledování Diagnostika a řešení potíží s úložišti tabulek Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Nástroje pro řešení potíží s aplikací úložišti Azure

Řešení problémů s klientské aplikace pomocí úložišti tabulek Microsoft Azure, můžete kombinací nástroje rozhodnout, kdy došlo k problému a co může být příčinou problémů. Tyto nástroje patří:

- **Azure úložiště analýzy**. [Azure úložiště analýzy](http://msdn.microsoft.com/library/azure/hh343270.aspx) poskytuje metriky a protokolování pro Azure úložiště.
    - **Metriky úložišť** sleduje transakce metriky a kapacita metriky účtu úložiště. Pomocí metriky, můžete určit, jak funguje aplikace podle spoustu různých míry. Další informace o typech metriky sledována analýzy úložiště najdete v článku [Schématu úložiště analýzy metriky tabulky](http://msdn.microsoft.com/library/azure/hh343264.aspx) .

    - Každý požadavek **úložiště protokolování** zaznamenává do služby Azure úložiště na straně serveru protokol. Protokol zaznamenává podrobná data pro každý požadavek, včetně operace, stav operace a latence informace. Další informace o datech žádostí a odpovědí, která chcete protokoly napsal úložiště analýzy naleznete v tématu [Formát protokolu analýzy úložiště](http://msdn.microsoft.com/library/azure/hh343259.aspx) .

> [AZURE.NOTE] Účty úložiště k typu replikační části Zone nadbytečné úložiště (ZRS) nemají metriky nebo funkce protokolování povoleno v současné době. 

- **Azure portálu**. Konfigurace protokolování a metriky účtu úložiště na [Portál Azure](https://portal.azure.com). Můžete taky zobrazení diagramů a grafů, které ukazují, jak funguje aplikace v čase a konfigurace upozornění upozorníte, pokud aplikace provede zadaný metriky očekávaným způsobem.

    Informace o konfiguraci sledování na portálu Azure naleznete v tématu [sledování úložiště účtu na portálu Azure](storage-monitor-storage-account.md) .

- **AzCopy**. Protokoly server Azure úložištěm uložená jako objekty BLOB, abyste mohli používat AzCopy kopírování objektů BLOB protokolu místního adresáře pro analýzu pomocí analyzátoru zprávy Microsoft. Další informace o AzCopy naleznete v tématu [přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md) .

- **Analýza Microsoft zprávy**. Zpráva Analyzer je nástroj, který využívá soubory protokolu a zobrazí protokolu data ve vizuálně formátu, který usnadňuje filtrování, hledání a seskupení dat protokolu do užitečné sady, které můžete použít k analýze chyb a problémů s výkonem. Příručce [Microsoft zprávy Analyzer provozní](http://technet.microsoft.com/library/jj649776.aspx) Další informace o Analyzer zprávy.

## <a name="about-the-sample-scenario"></a>O scénáři výběru

Pro účely tohoto návodu jsme budete zkoumat situace kde metriky úložišť Azure označuje zhoršeným procentuálně úspěch kurz pro aplikaci, která volá Azure úložiště. Metriky sazba zhoršeným procentuálně success (viz jako **PercentSuccess** [Azure portál](https://portal.azure.com) a v tabulkách metriky) sleduje operace, které úspěšné, ale které vrací HTTP stavový kód, který je větší než 299. Protokoly straně serveru úložiště jsou tyto operace zaznamenané se stavem transakce **ClientOtherErrors**. Podrobné informace o míru zhoršeným procentuálně úspěch najdete v článku [metriky zobrazení zhoršeným PercentSuccess nebo analýzy protokolu položky mají operace s stav ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Skladování Azure může vrátit HTTP stavů větší než 299 jako součást jejich normální funkce. Ale tyto chyby v některých případech označuje, že je možné optimalizovat klientská aplikace pro zvýšení výkonu.

V tomto scénáři popíšeme sazbu zhoršeným procentuálně úspěch je něco nižší než 100 %. Můžete metrických úroveň, ale podle potřeb. Doporučujeme, aby při zkoušení aplikace, vytvoříte odchylky podle směrného plánu pro klíčové měřítka. Například může zjistíte, na základě testování aplikace by měl mít konzistentní procentuálně úspěšnosti 90 % nebo 85 %. Pokud vaše data metriky ukazuje, že aplikace je přesahující toto číslo a pak můžete zjistit, co může způsobovat zvýšení.

Pro naše ukázkový scénář po jsme jste, že sazba míru procentuální úspěch je menší než 100 %, jsme prozkoumá protokoly najdete chyby, které souvisejí s metrikou a používat k zjistit příčinu nižší procentuální úspěšnosti. Podíváme na chyby v oblasti 400. Potom jsme budete lépe prozkoumejte chyby 404 (nebyl nalezen).

### <a name="some-causes-of-400-range-errors"></a>Několik příčin chyb při 400oblast

Následující příklady ukazuje příklady chybami 400oblast pro žádosti o proti úložišti objektů Blob Azure a jejich možných příčin. Některé z těchto chyby, jako chyby v 300 oblast a 500 rozsah dat, můžete přispívají k zhoršeným procentuálně úspěšnosti.

Všimněte si, že následujících seznamů vzdáleném od dokončení. Zobrazení [stavu a kódy chyb](http://msdn.microsoft.com/library/azure/dd179382.aspx) na webu MSDN podrobné informace o obecné chyby Azure úložiště a odstraňování specifické pro jednotlivé úložiště služby.

**Příklady kódu 404 (nebyl nalezen) stavu**

Nastane, když operace čtení proti kontejneru nebo objektů blob selhala, protože není nalezen objektů blob nebo kontejner.

- Dojde, když kontejneru nebo objektů blob odstranil jiného klienta než tohoto požadavku.
- Dojde, když používáte volání rozhraní API, který vytvoří kontejneru nebo objektů blob po kontrole, jestli existuje. Rozhraní API CreateIfNotExists volání vedoucí nejdřív se mají zjišťovat existenci container nebo objektů blob; Pokud není k dispozici, je vrácena chyba 404 a klikněte druhý umístění při volání napište kontejneru nebo objektů blob.

**Stavový kód 409 příklady (konflikt)**

- Dojde, když použití vytvořit rozhraní API pro vytvoření nového kontejneru nebo objektů blob, bez kontroly existenci nejdřív a kontejneru nebo objektů blob s tímto názvem už existuje.
- Dojde, když je odstraněním kontejneru a pokusíte se vytvořit nový kontejner se stejným názvem před dokončením operace odstranění.
- Dojde, když zadáte zapůjčenou na kontejner nebo objektů blob a už je zapůjčenou prezentovat.

**Stavový kód 412 (se nepodařilo předpokladem) příklady**

- Nastane při nesplnila podmínka nastavil podmíněné záhlaví.
- Nastane při zadané ID zapůjčení neodpovídá ID zapůjčení na kontejner nebo objektů blob.

## <a name="generate-log-files-for-analysis"></a>Generování protokoly pro analýzu

V tomto kurzu použijeme Analyzer zprávy pro práci s tři různé typy souborů protokolu, přestože můžete pracovat s některou z těchto článků:

- **Protokol serveru**, která se vytvoří při povolíte protokolování Azure úložiště. Protokol serveru obsahuje údaje o každé operace s názvem proti jedné z Azure úložiště služby – objektů blob, fronty, tabulky a soubor. Protokol serveru označuje, která operace označovalo jako a jaké stavový kód byl vrácené, stejně jako další podrobnosti o žádostí a odpovědí.
- **Protokol .NET klienta**, která je vytvořená při zapnutí protokolování na straně klienta z aplikace .NET. Protokol klienta obsahuje podrobné informace o jak klienta připraví žádost přijme a zpracuje odpověď.
- **Protokol trasování sítě HTTP**, který shromáždí data údajů protokolu HTTP/HTTPS žádostí a odpovědí, včetně pro operace proti Azure úložiště. V tomto kurzu budeme budete generovat trasování sítě pomocí analyzátoru zprávy.

### <a name="configure-server-side-logging-and-metrics"></a>Konfigurace protokolování na straně serveru a metriky

Nejdřív budete potřebujeme konfigurace protokolování Azure úložiště a metriky, takže máme dat z aplikace klienta a analyzujte data. Protokolování a metriky můžete konfigurovat různými způsoby – prostřednictvím [Portálu Azure](https://portal.azure.com)pomocí prostředí PowerShell, nebo programově. V tématu [Povolení metriky úložišť a zobrazení metriky dat](http://msdn.microsoft.com/library/azure/dn782843.aspx) a [Povolení protokolování úložiště a přístup k datům protokolu](http://msdn.microsoft.com/library/azure/dn782840.aspx) na webu MSDN podrobné informace o konfiguraci protokolování a metriky.

**Pomocí portálu Azure**

Konfigurace protokolování a metriky účtu úložiště pomocí [Portálu Azure](https://portal.azure.com), postupujte podle pokynů na [sledování úložiště účtu na portálu Azure](storage-monitor-storage-account.md).

> [AZURE.NOTE] Je možné nastavit minute metriky pomocí portálu Azure. Doporučujeme ale, že je nastavení pro účely tohoto kurzu a vyšetřování problémy s výkonem s aplikací. Můžete nastavit minute metriky pomocí prostředí PowerShell viz dole nebo programově pomocí knihovně úložiště klienta.
>
> Všimněte si, že na portálu Azure nemůže zobrazit minute metriky pouze hodinové metriky.

**Pomocí prostředí PowerShell**

Začínáme s prostředím PowerShell pro Azure, najdete v článku [jak instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

1. Přidání účtu Azure uživatele do okna prostředí PowerShell získáte pomocí rutiny [AzureAccount přidat](http://msdn.microsoft.com/library/azure/dn722528.aspx) :

    ```
    Add-AzureAccount
    ```

2. V okně **Přihlaste se k Microsoft Azure** zadejte e-mailové adresy a hesla přidruženého k vašemu účtu. Azure ověří uloží přihlašovacími údaji a potom slouží k zavření okna.
3. Nastavte výchozí účet úložiště na úložiště účet, který používáte pro kurz spuštěním tyto příkazy v okně Powershellu:

    ```
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. Povolení protokolování úložiště objektů Blob služby:

    ```
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```
5. Povolení metriky úložišť pro službu objektů Blob a ujistěte se, chcete-li nastavit **- MetricsType** `Minute`:

    ```
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>Konfigurace protokolování na straně klienta .NET

Konfigurace protokolování na straně klienta pro aplikaci .NET povolte .NET Diagnostika v souboru konfigurace aplikace (web.config nebo app.config). Další informace najdete na webu MSDN [protokolování na straně klienta s knihovnou .NET úložiště klienta](http://msdn.microsoft.com/library/azure/dn782839.aspx) a na [straně klienta protokolování s Microsoft Azure úložiště SDK jazyka Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) .

Protokol klientských obsahuje podrobné informace o jak klienta připraví žádost přijme a zpracuje odpověď.

Knihovně úložiště klienta ukládá klientských protokolu data v oblasti zadané v souboru konfigurace aplikace (web.config nebo app.config).

### <a name="collect-a-network-trace"></a>Shromáždit trasování v síti

Zpráva Analyzer umožňuje shromáždit trasování sítě protokolu HTTP/HTTPS je spuštěná klientské aplikace. Zpráva Analyzer bude použit [Fiddler](http://www.telerik.com/fiddler) back-end. Před shromažďujete trasování sítě, doporučujeme konfigurace Fiddler pořizovat záznam nešifrovaném přenosy HTTPS:

1. Nainstalujte [Fiddler](http://www.telerik.com/download/fiddler).
2. Spusťte Fiddler.
2. Vyberte nástroje **| Možnosti Fiddler**.
3. V dialogovém okně Možnosti zkontrolujte, zda **Zachytit připojí HTTPS** a **Dešifrování přenosy HTTPS** jsou oba vybrána, jak je ukázáno v následujícím příkladu.

![Konfigurace možností Fiddler](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Výukové shromáždit a nejprve uložit trasování v síti analýza zprávu a pak vytvořit relaci analýzy a analyzujte data sledování a protokoly. Získat informace o trasování v síti analýza zprávy:

1. Analýza zprávy, vyberte **soubor | Rychlé sledování | Bez šifrování HTTPS**.
2. Trasování se spustí hned. Vyberte zastavit sledování můžeme ji nakonfigurovat pro sledování úložiště pouze přenosu **Zastavit** .
3. Zvolte **Upravit** Upravit relaci trasování.
4. Vyberte odkaz **Konfigurovat** napravo od poskytovatele **Microsoft Pef WebProxy** trasování událostí pro Windows.
5. V dialogovém okně **Upřesnit nastavení** klikněte na kartu **poskytovatele** .
6. V poli **Název hostitele filtr** určete koncových bodů úložiště, oddělené mezerami. Například můžete zadat koncových bodů takto: Změna `storagesample` na název účtu úložiště:

    ```
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Zavřete dialogové okno a klikněte na zahájit shromažďování sledování s filtrem hostname na místě, **Restartujte** tak, aby pouze v síti Azure úložiště je součástí trasování.

>[AZURE.NOTE] Po dokončení shromažďování trasování sítě důrazně doporučujeme vrátit nastavení, které můžete měnit v Fiddler dešifrování HTTPS provoz. V dialogovém okně Možnosti Fiddler zrušte zaškrtnutí políčka **Zachytit připojí HTTPS** a **Dešifrování přenosy HTTPS** .

V tématu [použití funkce trasování sítě](http://technet.microsoft.com/library/jj674819.aspx) na webu Technet další podrobnosti.

## <a name="review-metrics-data-in-the-azure-portal"></a>Prohlížení metriky dat na portálu Azure

Po spuštění aplikace pro určitého časového období můžete zkontrolovat metriky grafů, které se zobrazí v [Portálu Azure](https://portal.azure.com) a sledujte, jak byly provádění služby. Nejdřív přejděte ke svému účtu úložiště na portálu Azure a přidání grafu pro **Úspěšné** procentuální.

Na portálu Azure teď uvidíte **Úspěch procento** sledování diagramu spolu s jiné přidaného může metriky. V scénáře jsme budete prozkoumat další analýzou protokoly analýza zprávy, procentuálně pravděpodobnost úspěchu je trochu nižší než 100 %.

Další podrobnosti o přidávání metriky stránky Monitoring najdete v tématu [jak: Přidání metriky tabulky metriky](storage-monitor-storage-account.md#how-to-add-metrics-to-the-metrics-table).

> [AZURE.NOTE] Může trvat delší dobu datům metriky zobrazit v portálu Azure po povolení metriky úložišť. Je to proto hodinové metriky pro předchozí hodiny nejsou zobrazeny v portálu Azure teprve po uplynutí aktuální hodiny. Minuta metriky také nejsou aktuálně zobrazené na portálu Azure. Takže podle toho, kdy povolit metriky, může to trvat až dva hodin zobrazíte metriky data.

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Použít AzCopy pro server protokoly do místního adresáře

Azure úložiště zapíše data protokolu serveru objektů BLOB, zatímco metriky zapíše do tabulky. Objekty BLOB protokolu jsou k dispozici v dobře známé `$logs` kontejneru účtu úložiště. Objekty BLOB protokolu jsou s názvem hierarchicky rok, měsíc, den a hodiny, tak, aby mohli snadno vyhledat oblast dobu, kterou chcete prozkoumat. Například v `storagesample` účet, container pro objekty BLOB protokolu pro 01/02/2015 a od 8 – 9: `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. Jednotlivé objekty BLOB v tomto kontejneru jsou s názvem postupně, počínaje `000000.log`.

Pokud si Pokud chcete stáhnout tyto soubory protokolu serverovou do umístění podle svého výběru na místním počítači můžete nástroj AzCopy příkazového řádku. Například, můžete tento příkaz Stáhnout protokoly pro objektů blob operace, které proběhly v 2 ledna 2015 do složky `C:\Temp\Logs\Server`; nahrazení `<storageaccountname>` s název účtu úložiště a `<storageaccountkey>` s svůj klíč účtu aplikace access:

    AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V

AzCopy je k dispozici ke stažení na stránky [Souborů ke stažení Azure](https://azure.microsoft.com/downloads/) . Podrobnosti o používání AzCopy najdete v tématu [přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md).

Další informace o stahování protokoly na straně serveru najdete v článku [stažení protokolování úložiště dat protokolu](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Pomocí analyzátoru zprávy Microsoft k analýze dat protokolu

Microsoft zprávy Analyzer je nástroj pro zachycení, zobrazení a analýza protokol zpráv přenosy, událostí a ostatní systému nebo aplikace zprávy scénáře diagnostiky a Poradce při potížích. Zpráva Analyzer také umožňuje načíst, agregovat a analýza dat z protokolu a uložili sledování soubory. Další informace o zprávy Analyzer příručce [Microsoft zprávy Analyzer provozní](http://technet.microsoft.com/library/jj649776.aspx).

Analýza zpráva obsahuje prostředky pro Azure úložiště, které vám pomohou při analýze server, klienta a síťové protokoly. V této části probereme používání nástrojů pro řešení problému zhoršeným procentuálně úspěch v protokolech na úložiště.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Stažení a instalace Analyzer zprávy a prostředky úložiště Azure

1. Stáhněte si [Zprávu Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) z Microsoft Download Center a spusťte instalační program.
2. Spuštění analyzátor zprávy.
3. V nabídce **Nástroje** vyberte **Správce materiálů**. V dialogovém okně **Správce materiálů** vybrat **soubory ke stažení**a potom filtrovat podle **Azure úložiště**. Zobrazí se Azure úložiště materiálů, a jak je znázorněno na následujícím obrázku.
4. Klikněte na **Synchronizovat všechny položky zobrazené** nainstalovat prostředky úložiště Azure. Dostupné prostředky, patří:
    - **Azure úložiště barvy pravidla:** Azure úložiště barvy pravidla umožňují určit speciální filtry, které používají barvu, text a řez písma zvýraznit zprávy, které obsahují konkrétních informací v trasování.
    - **Azure úložiště grafy:** Azure úložiště grafy jsou předdefinované grafy a znázornění dat protokolu serveru. Všimněte si, že můžete úložišti Azure grafy v současné době může jenom načtete protokol serveru do mřížky analýzy.
    - **Azure úložiště analyzátory:** Analyzátory úložišti Azure analyzovat Azure úložiště klienta, server a HTTP protokoly pro zobrazení v mřížce analýzy.
    - **Azure úložiště filtry:** Azure filtry úložiště jsou předdefinované kritéria, která slouží k vytvoření dotazu dat v mřížce analýzy.
    - **Azure úložiště zobrazení rozložení:** Azure rozložení zobrazení úložiště jsou předdefinovaného zobrazení sloupců a seskupení v mřížce analýzy.
4. Po instalaci aktiva, restartujte Analyzer zprávy.

![Zpráva Analyzer materiálů správce](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [AZURE.NOTE] Nainstalujte všechny úložišti Azure aktiva uvedená pro účely tohoto kurzu.

### <a name="import-your-log-files-into-message-analyzer"></a>Import souborů protokolu do zprávy analýza

Do jedné relace v Microsoft zprávy Analyzer pro analýzu můžete importovat všechny uložené soubory protokolu (serverovou straně klienta a sítě).

1. V nabídce **soubor** v aplikaci Microsoft zprávy Analyzer klikněte na **Novou relaci**a potom klikněte na **Prázdný relaci**. V dialogovém okně **Novou relaci** zadejte název pro analýzy relaci. V podokně **Podrobností relace** klikněte na tlačítko **soubory** .
1. Načtěte data trasování sítě generovaná Analyzer zprávu, klikněte na **Přidat soubory**, přejděte do umístění pro uložení souboru .matp z relaci sledování webu, vyberte soubor .matp a klikněte na tlačítko **Otevřít**.
1. Načtení dat protokolu straně serveru, klikněte na **Přidat soubory**, přejděte do umístění, kam jste stáhli protokoly na straně serveru, vyberte protokoly pro časový rozsah, který chcete analyzovat a klikněte na **Otevřít**. Pak v podokně **Podrobností relace** nastavte **Konfiguraci protokolu Text** rozevírací seznam pro každý soubor protokolu serverovou na **AzureStorageLog** zajistit, že Microsoft zprávy Analyzer analyzovat soubor protokolu správně.
1. Načtení dat protokolu klienta, klikněte na **Přidat soubory**, přejděte do umístění pro uložení vaší protokoly na straně klienta, vyberte protokoly, které chcete analyzovat a klikněte na **Otevřít**. Pak v podokně **Podrobností relace** nastavte **Konfiguraci protokolu Text** rozevírací seznam pro každý soubor protokolu klienta na **AzureStorageClientDotNetV4** zajistit, že Microsoft zprávy Analyzer analyzovat soubor protokolu správně.
1. Klikněte na tlačítko **Start** v dialogovém okně **Nové relace** zavádění a analyzovat data protokolu. Zobrazení protokolu dat v mřížce zprávy Analyzer analýzy.

Následujícím obrázku je relaci příklad nakonfigurována server, klienta a souborů protokolu trasování sítě.

![Konfigurace zprávy Analyzer relace](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Všimněte si, že zpráva Analyzer načte protokoly do paměti. Pokud máte velký množiny dat protokolu, budete chtít filtrovat abyste mohli získávat optimálního výkonu z analyzátoru zprávy.

Nejprve určit časové rozmezí, které vás zajímají revize a zachovat časové rozmezí nejnižší možnou. V mnoha případech bude chcete provést revizi období minut nebo hodin při nejčastěji. Importujte sady nejmenší protokolů, které můžete přizpůsobit svým potřebám.

Pokud máte pořád ještě velkého množství dat protokolu, pak je vhodné zadat relaci filtr dat protokolu před načíst. V dialogovém okně **Filtr relace** klikněte na tlačítko **knihovny** zvolit předdefinované filtry; například vyberte **globální čas filtr I** filtry úložišti Azure filtr na časový interval. Pak můžete upravit kritéria filtru Chcete-li zadat počáteční a koncové časové razítko pro interval, který chcete zobrazit. Můžete filtrovat i podle určitého stavový kód; můžete třeba načíst protokolu položky, kde stavový kód je 404.

Další informace o importu dat protokolu do Microsoft zprávy Analyzer najdete v článku [Načítání dat zprávy](http://technet.microsoft.com/library/dn772437.aspx) na TechNetu.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Použít ID žádosti klienta ke koordinaci data souboru protokolu

Knihovnu Azure úložiště klienta automaticky vygeneruje jedinečný klientské žádost o ID pro všechny žádosti o. Tato hodnota je aby došlo k zápisu protokolu klienta, protokol serveru a trasování v síti, abyste ho mohli použít ke koordinaci dat přes všechny tři protokoly v rámci Analyzer zprávy. Viz [ID klienta žádost o](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) Další informace o žádosti klienta ID.

Následující oddíly popisují používání předkonfigurovaná a vlastní rozložení zobrazení ke koordinaci a seskupit data podle ID klienta žádost.

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Výběr rozložení zobrazení zobrazíte v mřížce analýzy

Úložiště aktiva pro analýza zprávy zahrnují Azure úložiště zobrazení rozložení, které jsou předkonfigurovaná zobrazení, které můžete použít k zobrazení dat s užitečné seskupení a sloupci různých scénářích. Můžete taky vytvořit vlastní zobrazení rozložení a uložit pro opakované použití.

Na následujícím obrázku nabídce **Zobrazení rozložení** dostupná výběrem **Zobrazení rozložení** na pásu karet panelu nástrojů. Zobrazení rozložení pro úložišti Azure jsou seskupená podle uzel **Azure úložiště** v nabídce. Můžete vyhledávat `Azure Storage` do vyhledávacího pole požadovanou úložišti Azure zobrazit pouze rozložení. Můžete také vybrat na hvězdičku u zobrazení rozložení a jeho nastavení jako oblíbené položky zobrazit v horní části nabídky.

![Nabídka rozložení](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Na začátku vyberte **seskupené podle ClientRequestID a modulu**. Toto rozložení zobrazení skupin dat ze všech tři protokoly přihlášení nejdřív podle ID žádosti klienta a potom soubor protokolu zdroj (nebo **modulu** v Analyzer zprávy). V tomto zobrazení můžete přecházet na podrobnější ID žádost o konkrétní klienta a zobrazit data z všechny tři protokoly pro žádosti klienta ID.

Obrázek ukazuje toto zobrazení rozložení použit protokolu s ukázkovými daty s podmnožinu sloupce zobrazovat. Uvidíte, že žádost o určitého klienta ID mřížce analýzy slouží k zobrazení dat z klienta protokolu, protokol serveru a trasování sítě.

![Azure úložiště zobrazení rozložení](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

>[AZURE.NOTE] Různé protokoly mají různé sloupce, tak po zobrazení dat z víc souborů protokolu v mřížce analýzy některé sloupce nesmí obsahovat žádná data pro dané řádek. Například na obrázku nahoře, řádky protokolu klienta nezobrazovat žádná data pro **časové razítko**, **TimeElapsed**, **zdroje**a **určení** sloupce tyto sloupce v protokolu klienta neexistuje, protože neexistují v trasování sítě. Podobně sloupec časového **razítka** zobrazuje časové razítko data z protokolu serveru, ale nezobrazí se žádná data pro **TimeElapsed**, **zdrojové**a **cílové** sloupce, které nejsou součástí protokol serveru.

Kromě použití zobrazení rozložení Azure úložiště, můžete definovat a uložit vlastní zobrazení rozložení. Můžete vybrat jiné požadovaná pole pro seskupení dat a uložit seskupení jako součást vašeho vlastního rozložení.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Použití barvy pravidla přichycením k mřížce analýzy

Úložiště prostředky se týkají taky barvy pravidla, která poskytují že vizuální prvek prostředky k identifikaci různé typy chyb v mřížce analýzy. Předdefinované barvy pravidla platí pro HTTP chyby, aby se zobrazovaly jenom pro sledování protokolu a sítě serveru.

Pravidlo můžete použít barvy, vyberte **Barvu pravidla** z pásu karet panelu nástrojů. Zobrazí se tato pravidla barvy Azure úložiště v nabídce. Výukové vyberte **Klienta chyby (StatusCode mezi 400 a 499)**, jak je znázorněno na následujícím obrázku.

![Azure úložiště zobrazení rozložení](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Kromě použití pravidel barvy Azure úložiště, můžete definovat a uložit vlastní barvu pravidla.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Skupina a filtrování dat protokolu zobrazíte 400oblast chyby

Potom jsme budete seskupení a filtrování dat protokolu zobrazíte všechny chyby v oblasti 400.

1. Najděte **StatusCode** sloupec v mřížce analýzy, klikněte pravým tlačítkem myši na záhlaví sloupce a vyberte **skupinu**.
2. Další skupiny ve sloupci **ClientRequestId** . Uvidíte, že data v mřížce analýzy je uspořádaný podle stavový kód a podle ID klienta žádost.
1. Zobrazení podokna nástrojů filtru zobrazení, pokud již není zobrazeno. Na pásu karet panelu nástrojů vyberte **Nástroje systému Windows**, pak **Filtru zobrazení**.
2. Filtrování dat protokolu zobrazíte jenom 400oblast chyby, přidejte následující kritéria filtru do okna **Filtru zobrazení** a klikněte na **použít**:

        (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)

Následující obrázek zobrazuje výsledky tohoto seskupení a filtrování. Rozbalení pole **ClientRequestID** pod seskupení stavový kód 409, například zobrazuje operaci, kterou výsledkem této stavový kód.

![Azure úložiště zobrazení rozložení](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Po použití tento filtr, zobrazí se řádky z protokolu klienta vyloučíte, jako klient protokolu nezahrnuje **StatusCode** sloupec. Na začátku: přečtěte server a protokoly trasování sítě vyhledejte chyby 404 a jsme se vraťte do protokolu klienta podívat se operace na straně klienta, které vedly k nim.

>[AZURE.NOTE] Můžete filtrovat podle sloupce **StatusCode** a pořád zobrazení dat z všechny tři protokoly, včetně protokolu klienta, když naopak přidáte výrazu filtru, který obsahuje protokolu položky, kde je null stavový kód. Vytvářet tento výraz filtru, můžete:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
>
> Tento filtr vrátí všechny řádky z klienta protokolu a pouze řádky z protokol serveru a protokolu HTTP kde je větší než 400 stavový kód. Je-li použít rozložení zobrazení seskupené podle ID žádosti klienta a modulu, můžete hledat nebo přejděte prostřednictvím protokolu položky najít fáze, kde všechny tři protokoly reprezentuje.   

### <a name="filter-log-data-to-find-404-errors"></a>Filtrování dat protokolu najdete chyby 404

Úložiště prostředky zahrnout předdefinované filtry, které můžete použít k filtrování dat protokolu najdete chyby nebo trendy, kterou hledáte. Pak doporučujeme použít dva předdefinované filtry: ten, který filtruje serveru a protokoly trasování sítě 404 chyby a ten, který filtruje data na zadaný časový rozsah.

1. Zobrazení podokna nástrojů filtru zobrazení, pokud již není zobrazeno. Na pásu karet panelu nástrojů vyberte **Nástroje systému Windows**, pak **Filtru zobrazení**.
2. V okně Filtr zobrazení vyberte **knihovnu**a vyhledávat `Azure Storage` zobrazíte úložišti Azure filtry. Vyberte filtr **404 (nebyl nalezen)**zpráv ve všech protokolech.
3. Zobrazení nabídky **knihovny** znovu a vyhledejte a vyberte **Globální filtru s časovým filtrem**.
4. Úprava časová razítka vidět ve filtru pro oblasti, kterou chcete zobrazit. To vám pomůže zúžit oblast dat a analyzujte data.
5. Filtr by se měly podobně jako v příkladu níže. Na tlačítko **použít** filtr přichycením k mřížce analýzy.

        ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
        (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)

![Azure úložiště zobrazení rozložení](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Analýza dat protokolu

Teď, když jste seskupili a filtrovat data, můžete zkontrolovat podrobnosti jednotlivé požadavky, které vygenerovalo chyby 404. V zobrazení rozložení na aktuální data seskupená podle ID žádosti klienta, potom podle protokolu zdroje. Protože jsme filtrování na žádosti o kde pole StatusCode obsahuje 404, ukážeme jenom server a data trasování sítě, není dat protokolu klienta.

Na následujícím obrázku žádost o konkrétní místo, kam operace získat objektů Blob vhodné 404, protože neexistuje objektů blob. Všimněte si, že některé sloupce mohly být odebrány z ve standardním zobrazení zobrazí příslušná data.

![Filtrované serveru a trasování sítě](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

Dále jsme budete sladit tento kód žádosti klienta s daty protokolu klienta zobrazíte jaké akce klienta byl provedení při došlo k chybě. Nové zobrazení Analýza tabulky pro tuto relaci zobrazíte data protokolu klienta, který se otevře v druhé kartě můžete zobrazit:

1. Nejdřív zkopírujte hodnotu z pole **ClientRequestId** do schránky. Lze provést výběrem buď řádku, vyhledání pole **ClientRequestId** , pravým tlačítkem myši na hodnotu data a zvolte **Kopírovat "ClientRequestId"**.
1. Na pásu karet panelu nástrojů vyberte **Nový prohlížeč**a potom vyberte **Analýzy mřížky** otevřete novou kartu. Na kartě nové zobrazuje všechna data v souborech protokolu bez seskupení, filtrování a barvy pravidla.
2. Na pásu karet panelu nástrojů vyberte **Zobrazení rozložení**a potom vyberte **Všechny sloupce .NET klienta** v části **Azure úložiště** . Toto zobrazení rozložení protokolu, jakož i protokoly sledování informací o serveru a sítě jsou zobrazena data ze klienta. Ve výchozím nastavení seřazena ve sloupci **MessageNumber** .
3. Pak protokolu klienta vyhledejte žádost o ID klienta. Na pásu karet panelu nástrojů vyberte **Vyhledání zpráv**a potom zadat vlastní filtr ID žádosti klienta do pole **Najít** . Pomocí následující syntaxe pro filtr vlastní identifikátorem žádosti klienta:

        *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"

Zpráva Analyzer najde a vybere první položku protokolu kde kritéria vyhledávání odpovídá ID klienta žádost. V protokolu klienta existuje několik položek pro každou žádost o ID klienta tak můžete chtít seskupit podle pole **ClientRequestId** usnadňuje neuvidíte vůbec. Následující obrázek ukazuje všechny zprávy v protokolu klienta pro zadaný klient žádosti.

![Klient protokolu zobrazující 404 chyby](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Použití dat zobrazených v zobrazení rozložení v těchto dvou karty, můžete analyzovat žádost o data, která chcete určit, co způsobilo chybu. Můžete taky prohlédnout požadavky, které tuto položku zobrazíte, pokud předchozí událost mohou vést chyba 404. Můžete třeba zkontrolovat položky klientů protokolu před ID tohoto klienta žádost o chcete zjistit, zda objektů blob odstranit nebo pokud došlo k chybě kvůli klientské aplikaci volání rozhraní API CreateIfNotExists na kontejner nebo objektů blob. V protokolu klienta můžete najít objektů blob adresu v poli **Popis** . Tyto informace se zobrazí v poli **Souhrn** na serveru a protokoly trasování sítě.

Jakmile znát adresu objektů blob, která je výsledkem chybná chyba 404, můžete prozkoumat další. Pokud Hledat položky protokolu další zprávy související s operacemi na stejné objektů blob můžete zkontrolovat, zda klienta odstraněnou entity.

## <a name="analyze-other-types-of-storage-errors"></a>Analýza jiných typů úložiště chyb

Teď máte zkušenosti s pomocí analyzátoru zprávy k analýze dat protokolu, můžete analyzovat jiné druhy chyb pomocí zobrazení rozložení, barev pravidla a hledání/filtrování. Následující tabulce jsou uvedené některých problémů, ke kterým může dojít a kritérií filtrů, které můžete použít je vyhledat. Další informace o vytváření filtrů a zprávy Analyzer filtrování jazyka najdete v článku [Filtrování dat zprávy](http://technet.microsoft.com/library/jj819365.aspx).

|    Pokud chcete zkontrolovat...                                                                                               |    Použití výrazu filtru...                                                                                                                                                                                                                                        |    Výraz použit k protokolu (klienta, Server, sítě, všechny)    |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|    Neočekávané zpožděním při doručení zprávy ve frontě                                                              |    AzureStorageClientDotNetV4.Description obsahuje "Opakování operace se nezdařila."                                                                                                                                                                                |    Klient                                                        |
|    Nastavit informace HTTP zvýšení PercentThrottlingError                                                                       |    NASTAVIT INFORMACE HTTP. Response.StatusCode == 500 #124; & #124; NASTAVIT INFORMACE HTTP. Response.StatusCode == 503                                                                                                                                                                                          |    Sítě                                                       |
|    Zvýšení PercentTimeoutError                                                                               |    NASTAVIT INFORMACE HTTP. Response.StatusCode == 500                                                                                                                                                                                                                             |    Sítě                                                       |
|    Zvýšení PercentTimeoutError (vše)                                                                         |    * StatusCode == 500                                                                                                                                                                                                                                          |    Všechny                                                           |
|    Zvýšení PercentNetworkError                                                                               |    AzureStorageClientDotNetV4.EventLogEntry.Level < 2                                                                                                                                                                                                          |    Klient                                                        |
|    Nastavit informace HTTP 403 (zakázáno) zprávy                                                                                 |    NASTAVIT INFORMACE HTTP. Response.StatusCode == 403                                                                                                                                                                                                                             |    Sítě                                                       |
|    Nastavit informace HTTP 404 (nebyl nalezen) zprávy                                                                                 |    NASTAVIT INFORMACE HTTP. Response.StatusCode == 404                                                                                                                                                                                                                             |    Sítě                                                       |
|    404 (vše)                                                                                                     |    * StatusCode == 404                                                                                                                                                                                                                                          |    Všechny                                                           |
|    Sdílené problém se tak mohli ověřovat přidružení přístup podpisu (zabezpečení)                                                             |    AzureStorageLog.RequestStatus == "SASAuthorizationError"                                                                                                                                                                                                     |    Sítě                                                       |
|    Nastavit informace HTTP 409 (konflikt) zprávy                                                                                  |    NASTAVIT INFORMACE HTTP. Response.StatusCode == 409                                                                                                                                                                                                                             |    Sítě                                                       |
|    409 (vše)                                                                                                     |    * StatusCode == 409                                                                                                                                                                                                                                          |    Všechny                                                           |
|    Nízká PercentSuccess nebo analýzy protokolu položky mají operace se stavem ClientOtherErrors    |    AzureStorageLog.RequestStatus == "ClientOtherError"                                                                                                                                                                                                         |    Server                                                        |
|    Nagle upozornění                                                                                               |    ((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1,5)) a (AzureStorageLog.RequestPacketSize < 1460) a (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200)        |    Server                                                        |
|    Časového rozsahu ve serveru a síťové protokoly                                                                    |    #Časové razítko > = 2014-10 – 20T16:36:38 a #Timestamp < = 2014-10 – 20T16:36:39                                                                                                                                                                                     |    Server, sítě                                               |
|    Oblast čas na serveru odhlásí                                                                                |    AzureStorageLog.Timestamp > = 2014-10 – 20T16:36:38 a AzureStorageLog.Timestamp < = 2014-10 – 20T16:36:39                                                                                                                                                     |    Server                                                        |


## <a name="next-steps"></a>Další kroky

Další informace o řešení potíží začátku do konce scénáře v úložišti Azure naleznete v následujících zdrojích:

- [Sledování a Diagnostika a odstraňování potíží úložišti tabulek Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md)
- [Úložiště analýzy](http://msdn.microsoft.com/library/azure/hh343270.aspx)
- [Sledování úložiště účtu na portálu Azure](storage-monitor-storage-account.md)
- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
- [Příručka provozní Analyzer Microsoft zprávy](http://technet.microsoft.com/library/jj649776.aspx)
