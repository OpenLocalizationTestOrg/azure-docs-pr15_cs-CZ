<properties
    pageTitle="Základní informace o Azure diagnostiky | Microsoft Azure"
    description="Použití Azure Diagnostika pro ladění výkonu, sledování, analýza provozu do cloudové služby, virtuálních počítačích a struktury služby"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Co je diagnostických nástrojů Microsoft Azure


Azure diagnostiky je možnost v Azure, která umožňuje sběr diagnostických informací o nasazení aplikace. Rozšíření diagnostiky můžete z několika různých zdrojů. Aktuálně podporované jsou webové služby Azure cloudu a pracovníka role, systém Microsoft Windows a struktury služby Azure virtuálních počítačích. Ostatní služby Azure mít vlastní samostatné diagnostiky.

## <a name="data-you-can-collect"></a>Data, která můžete shromažďovat

Azure diagnostiky získáte následující typy dat:

Zdroj dat|Popis
---|---
Výkonnosti | Operační systém a vlastní výkonnosti
Protokoly aplikace     | Sledování zpráv napsal aplikace
Protokoly událostí systému Windows   | Informace odesílané do systému protokolování událostí systému Windows
.NET zdroj události    | Kód zápisu událostí pomocí .NET [Zdroj_události](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) třídy
Protokoly služby IIS             | Informace o webů služby IIS
Manifestu na základě trasování událostí pro Windows   | Událost trasování pro Windows události generované rozeběhnutý proces
Výpisy          | Informace o stavu procesu v případě havárie aplikací
Vlastní chybové protokoly    | Protokoly vytvořené pomocí aplikace nebo služby
Azure infrastruktury diagnostické protokolování|Informace o diagnostiky sebe sama

Rozšíření Azure diagnostiky můžete převést tato data do účet Azure úložiště nebo jeho odeslání do služby, jako třeba [Přehledy aplikace](./application-insights/app-insights-cloudservices.md). Data můžete použít pro ladění a odstraňování potíží, měření výkonu, sledování využití prostředků, analýza provozu a plánování kapacity a auditování.


## <a name="versioning"></a>Správa verzí
Zobrazit [historii verzí Azure diagnostiky](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Další kroky
Výběr které služby chcete shromažďovat Diagnostika v a použijte v těchto článcích můžete začít. Pomocí odkazů obecné Azure Diagnostika odkazy u konkrétních úkolů.

## <a name="web-apps"></a>Web Apps
Poznámka: Web Apps nepoužívejte Azure diagnostiky. Vyhledání odpovídající informace na [Web Apps](./app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Cloudové služby, které využívají diagnostiky Azure
- Pokud používáte Visual Studiu, najdete v článku [Použití Visual Studio můžete sledovat aplikace Cloudovým službám](./vs-azure-tools-debug-cloud-services-virtual-machines.md) začít pracovat. V opačném najdete v článku
- [Jak sledovat Cloudovým službám pomocí diagnostických nástrojů Azure](./cloud-services/cloud-services-how-to-monitor.md)
- [Nastavení Azure Diagnostika v aplikaci služby cloudu](./cloud-services/cloud-services-dotnet-diagnostics.md)

Další rozšířené témata naleznete v tématu

- [Použití Azure Diagnostika v aplikaci přehledy pro Cloudovým službám](./application-insights/app-insights-cloudservices.md)
- [Sledování tok aplikace Cloudovým službám pomocí diagnostiky Azure](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [Použití Powershellu ke nastavení diagnostických nástrojů na Cloud Services](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Virtuálních počítačích pomocí diagnostických nástrojů Azure
- Pokud používáte Visual Studiu, najdete v článku [Použití Visual Studio můžete provést sledování Azure virtuálních počítačích](./vs-azure-tools-debug-cloud-services-virtual-machines.md) začít pracovat. V opačném najdete v článku
- [Nastavení diagnostických nástrojů Azure na Azure virtuálního počítače](./virtual-machines-dotnet-diagnostics.md)

Další rozšířené témata naleznete v tématu

- [Nastavení diagnostických nástrojů na virtuálních počítačích Azure pomocí prostředí PowerShell](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [Vytvoření Windows virtuální počítače s sledování a diagnostice pomocí šablony správce prostředků Azure](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Služba struktury pomocí diagnostických nástrojů Azure
Začínáme na [monitoru aplikace služby struktury](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Množství jiných služeb struktury diagnostiky článků, které jsou dostupné v navigačního stromu na levé straně po nastavení tohoto článku.

## <a name="general-azure-diagnostics-articles"></a>Obecné Azure diagnostiky články
- [Konfigurace schématu diagnostiky azure](https://msdn.microsoft.com/library/azure/mt634524.aspx) – zjistěte, jak změnit souboru schématu můžete shromažďovat a směrování diagnostiky data. Všimněte si, že můžete také Visual Studio změnit souboru schématu.
- [Jak Azure diagnostických nástrojů dat uložených v úložišti Azure](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) - znáte názvy tabulek a objekty BLOB kde diagnostiky data zapisuje.
- Naučte se [používat výkonnosti v Azure diagnostiky](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Zjistěte, jak pomocí [postupu Azure diagnostické informace pro přehledy aplikace](./azure-diagnostics-configure-applicationinsights.md)
- Pokud máte potíže s diagnostiky zahájení nebo hledání dat v Azure úložiště tabulek, najdete v článku [Poradce při potížích s diagnostiky Azure](./azure-diagnostics-troubleshooting.md)
