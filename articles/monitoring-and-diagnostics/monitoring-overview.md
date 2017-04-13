<properties
    pageTitle="Základní informace o sledování v Microsoft Azure | Microsoft Azure"
    description="Nejvyšší úrovně základní informace o sledování a Diagnostika v Microsoft Azure včetně upozornění, webhooks, automatické měřítko apod."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"l
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="robb"/>

# <a name="overview-of-monitoring-in-microsoft-azure"></a>Základní informace o sledování v Microsoft Azure

Tento článek obsahuje přehled sledování Azure zdroje. Poskytuje odkazy na informace na určité typy zdrojů.  Uceleném informace týkající se sledování aplikace z hlediska není Azure najdete v tématu [sledování a diagnostice pokyny](../best-practices-monitoring.md).

Cloudové aplikace jsou složité s mnoha proměnné části. Sledování obsahuje data a ujistěte se, že aplikace zůstane nahoru a spuštění v pořádku stavu. Také pomáhá stave vypnout potenciálních problémů a řešení potíží s dřívější fáze. Kromě toho můžete monitorování dat získat hloubkové Další informace o aplikaci. Tento knowledge můžete můžete zlepšit výkonu aplikace nebo udržovatelnost nebo automatizovat akce, které by jinak vyžadovat ruční zásah.

Na následujícím obrázku vidíte koncepční zobrazení aplikace Azure sledování, včetně typu protokolů, které můžete shromažďovat a co můžete dělat s tato data.   

![Logické modelu doplňku pro monitorování a diagnostice bez využití zdrojů](./media/monitoring-overview/MonitoringAzureResources-non-compute_v3.png)

Obrázek 1: Koncepční modelu doplňku pro monitorování a diagnostice bez využití zdrojů

<br/>

![Logické modelu doplňku pro monitorování a diagnostiku výpočetních zdrojů](./media/monitoring-overview/MonitoringAzureResources-compute_v3.png)

Obrázek 2: Koncepční modelu doplňku pro sledování a diagnostiku výpočetních zdrojů


## <a name="monitoring-sources"></a>Sledování zdrojů
### <a name="activity-logs"></a>Protokoly aktivity
Protokol aktivity (dříve nazývanou funkční nebo protokolů auditování) můžete vyhledávat informace o zdroj zobrazené Azure infrastruktury. Protokol obsahuje informace o například čas při vytváření nebo odstraněna zdroje.  

### <a name="host-vm"></a>Host (hostitel) OM
**Výpočet pouze**


Některé výpočet zdrojů, jako jsou cloudové služby, virtuálních počítačích, stav a struktury služby vyhrazené OM hostitele interakci s. Host (hostitel) OM odpovídá kořenové OM v modelu hypervisoru Hyper-V. V tomto případě získáte metriky na jenom OM hostitele kromě OS Host.  

Pro další služby Azure není nutně 1:1 mapování mezi zdroj a konkrétní OM hostitele tak hostitele OM metriky nejsou k dispozici.


### <a name="resource---metrics-and-diagnostics-logs"></a>Zdroj - metriky a protokolování diagnostiky
Collectable metriky lišit v závislosti na typu zdroje. Například virtuálních počítačích obsahuje statistiky/v disku a procento využití procesoru. Ale tyto stat neexistují fronty Bus služby, místo toho poskytující metriky jako fronty velikost a zprávy pro výkon.

Pro využití prostředků můžete získat metriky na moduly hostovaného OS a diagnostice jako Azure diagnostiky. Azure diagnostiky pomáhá shromažďovat a směrování shromážděte dat diagnostiky do jiných umístění, včetně Azure úložiště.

Seznam aktuálně collectable metriky je k dispozici [podporované metriky](monitoring-supported-metrics.md).

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Aplikace – protokolování diagnostiky protokoly aplikací a metriky
**Výpočet pouze**

Aplikace je možné spouštět nad hostovaného OS v modelu výpočetním. Budou posílat vlastní sadu metriky a protokolování.

Typy metriky

- Výkonnosti
- Protokoly aplikace
- Protokoly událostí systému Windows
- .NET zdroj události
- Protokoly služby IIS
- Manifestu na základě trasování událostí pro Windows
- Výpisy
- Protokoly chyb zákazníka


## <a name="uses-for-monitoring-data"></a>Použití monitorování dat

### <a name="visualize"></a>Vizualizace
Vizualizace sledování dat v grafy vám pomůže najít trendů úplně rychleji než procházení samotná data.  

Několik způsobů vizualizace, patří:

- Používat portál Azure
- Směrování dat pro přehledy aplikace Azure
- Směrovat data k Microsoft PowerBI
- Směrování dat do jiných výrobců vizualizace nástroje pomocí některého z živé streamování nebo tím, že nástroje pro čtení z archivu v Azure úložiště

### <a name="archive"></a>Archivace
Sledování dat je obvykle zapisovat Azure úložiště a k dispozici tam dokud neodstraníte ho.

Několik možností, jak používat tyto údaje:

- Jakmile napsali, můžete mít dalších nástrojů v rámci nebo mimo ni Azure číst a jeho zpracování.
- Stáhnout data místně pro místní archivace nebo změňte svoje zásady uchovávání informací v cloudu, aby uchovávat data pro dlouhou dobu.  
- Můžete nechat data v Azure úložiště, když budete muset zaplatit Azure úložiště podle objemu dat, vytvořit si.
-

### <a name="query"></a>Dotaz
Azure Monitor rozhraní REST API, křížové platformy rozhraní příkazového rozhraní příkazového řádku (řádku) příkazů, rutiny prostředí PowerShell a .NET SDK můžete použít pro přístup k datům v systému nebo Azure úložiště

Jako příklad lze uvést:

-  Získání dat pro vlastní aplikace pro sledování, kterou jste napsali
-  Vytváření vlastních dotazů a posílají tato data do aplikace jiných výrobců.

### <a name="route"></a>Směrování
Můžete vysílat datovými proudy monitorování dat do jiných umístění v reálném čase.

Jako příklad lze uvést:

- Odešlete interpretace aplikace, abyste mohli používat nástroje vizualizace tam.
- Odešlete rozbočovače událost, můžete směrovat nástrojů jiných výrobců v reálném čase analýza.

### <a name="automate"></a>Automatizace
Můžete použít sledování data na aktivační události nebo dokonce celé procesy jako příklad lze uvést:

- Použití dat na automatické měřítko výpočetním instance nahoru nebo dolů na základě při zavedení aplikace.
- Posílat e-maily, když metriky ve výkresu protínají předem mezní hodnota.
- Volání webové adresy URL (webhook) provedení akce v systému mimo Azure
- Začněte postupu runbook v Azure automatizaci provádět libovolnou řadu úkolů

## <a name="methods-of-use"></a>Způsoby použití
Obecně můžete pracovat s dat sledování směrování a načítání pomocí jedné z následujících metod. Ne všechny metody jsou dostupné pro všechny akce nebo datové typy.

- [Azure portálu](https://portal.azure.com)
- [Prostředí PowerShell](insights-powershell-samples.md)  
- [Různé platformy rozhraní příkazového řádku (rozhraní příkazového řádku)](insights-cli-samples.md)
- [ROZHRANÍ REST API](https://msdn.microsoft.com/library/dn931943.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/dn802153.aspx)

## <a name="azures-monitoring-offerings"></a>Sledování nabídky na Azure
Azure má nabídky dostupné pro sledování svých služeb z čistý počítač infrastruktury telemetrie aplikace. Nejlepší sledování strategie kombinuje využívání všechny tři získat komplexní, podrobný přehled o stavu služby.

- [Sledování azure](http://aka.ms/azmondocs) – nabízí vizualizace, dotazu, směrování, výstrahy, automatické měřítko a automatizaci na datech jak z Azure infrastruktury (protokolu činnosti) a každý konkrétního zdroje Azure (protokoly pro diagnostiku). Tento článek je součástí dokumentace Azure Monitor. Název Azure sledování byla vydána září 25 na Ignite 2016.  Předchozí název byl "Azure přehledy."  
- [Přehledy aplikace](https://azure.microsoft.com/documentation/services/application-insights/) – poskytuje bohaté zjišťování a diagnostice problémy ve vrstvě aplikací služby, dobře integrované nad dat z Azure sledování. Jedná se o výchozí diagnostiky platformu pro aplikaci služby Web Apps.  Můžete směrovat dat z jiných služeb, do ní.  
- [Protokol analýzy](https://azure.microsoft.com/documentation/services/log-analytics/) součást [Operace správy sadu](https://www.microsoft.com/cloud-platform/operations-management-suite) – poskytuje jedné komplexní řešení pro správu IT pro místním prostředím a jiných výrobců cloudové infrastruktury (například AWS) kromě Azure zdroje.  Dat z Azure Monitor mohou být směrovány přímo do protokolu analýzy, abyste viděli metriky a protokolů pro celé prostředí na jednom místě.     


## <a name="next-steps"></a>Další kroky
Další informace o

- [Azure Monitor do videa z Ignite 2016](https://myignite.microsoft.com/videos/4977)
- [Začínáme s Azure monitoru](monitoring-get-started.md)
- [Diagnostika azure](../azure-diagnostics.md) při pokusu diagnostikovat potíže v aplikaci cloudové služby, virtuálního počítače nebo struktury služby.
- [Aplikace přehledy](https://azure.microsoft.com/documentation/services/application-insights/) Pokud se pokoušíte Diagnostika problémů v aplikaci služby Web appu.
- [Poradce při potížích s úložišti Azure](../storage/storage-e2e-troubleshooting.md) při použití úložiště objektů BLOB, tabulek nebo dotazů
- [Protokol technologie pro analýzu](https://azure.microsoft.com/documentation/services/log-analytics/) a [operace správy Suite](https://www.microsoft.com/cloud-platform/operations-management-suite)
