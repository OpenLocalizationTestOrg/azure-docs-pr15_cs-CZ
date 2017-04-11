<properties
    pageTitle="Ukládání a zobrazení dat diagnostiky Azure úložiště | Microsoft Azure"
    description="Načtení dat Azure diagnostiky do úložiště Azure a jeho zobrazení"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="cloud-services"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Ukládání a zobrazení diagnostiky dat v úložišti Azure

Diagnostické data nejsou uložena trvale, pokud ho přepojte emulátoru úložiště Microsoft Azure nebo k Azure úložiště. Jednou v úložišti jej lze zobrazit jednoho několik dostupných nástrojů.

## <a name="specify-a-storage-account"></a>Zadejte účet úložiště

Zadáte úložiště účet, který chcete použít v souboru ServiceConfiguration.cscfg. Informace o účtu je definována jako připojovací řetězec v nastavení konfigurace. Následující příklad ukazuje výchozí připojovací řetězec vytvoří nový projekt Cloudová služba ve Visual Studiu:


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Můžete změnit tento připojovací řetězec k poskytování informací o účtu pro účet Azure úložiště.

V závislosti na typu diagnostiky data, která shromážděny Azure diagnostiky používá službu objektů Blob nebo tabulku služba. Následující tabulka zobrazuje zdroje dat, které jsou trvalé a jejich formátu.

|Zdroj dat|Formát ukládání|
|---|---|
|Azure protokoly|Tabulky|
|Protokoly IIS 7.0|Objektů BLOB|
|Azure infrastruktury protokolování diagnostiky|Tabulky|
|Nepodařilo požádat o sledování protokoly|Objektů BLOB|
|Protokoly událostí systému Windows|Tabulky|
|Výkonnosti|Tabulky|
|Výpisy|Objektů BLOB|
|Vlastní chybové protokoly|Objektů BLOB|

## <a name="transfer-diagnostic-data"></a>Přenos diagnostiky dat

Pro SDK 2,5 a v novějších verzích žádost o přenos dat diagnostiky může dojít pomocí konfiguračního souboru. Přenos dat diagnostiky plánované každých jak je uvedeno v konfiguraci.

Pro SDK 2.4 předchozích nebo dalších můžete požádat o pro přenos dat diagnostiky pomocí konfiguračního souboru jako programově. Programový přístup taky vám umožní dělat okamžitou přenosy.


>[AZURE.IMPORTANT] Při převodu dat diagnostiky účet Azure úložiště se vynakládá u zdrojů úložiště, která využívají data diagnostické.

## <a name="store-diagnostic-data"></a>Ukládání dat diagnostiky

Protokol data se ukládají v úložišti objektů Blob nebo tabulce s následujícími názvy:

**Tabulky**

- **WadLogsTable** - protokoly napsané v kódu pomocí posluchače trasování.

- **WADDiagnosticInfrastructureLogsTable** - diagnostiky monitor a konfigurace změní.

- **WADDirectoriesTable** – adresářů, které je sledování diagnostiky monitor.  Jedná se o protokoly služby IIS, IIS failed protokoly žádost a vlastní adresáře.  Umístění souboru protokolu objektů blob zadané v poli kontejner a název objektů blob je v poli RelativePath.  Pole Absolutní_cesta označuje umístění a název souboru tak, jak byly na Azure virtuálního počítače.

- **WADPerformanceCountersTable** – výkonnosti.

- **WADWindowsEventLogsTable** – protokoly událostí systému Windows.

**Objekty BLOB**

- **kontejner wad ovládacího prvku** – (jenom pro SDK 2.4 předchozích nebo dalších) obsahuje konfigurační soubory XML, které řídí Azure diagnostice.

- **wad iis failedreqlogfiles** – obsahuje informace ze služby IIS se nezdařilo požádat o protokoly.

- **wad iis logfiles** – obsahuje informace o službě IIS protokoly.

- **"vlastní"** – vlastní kontejner podle konfigurace adresářů, které jsou sledovány diagnostiky sledování.  Název této objektů blob kontejneru bude zadané v WADDirectoriesTable.

## <a name="tools-to-view-diagnostic-data"></a>Nástroje diagnostiky data můžete zobrazit
Několik nástrojů jsou dostupné pro zobrazení dat je po přenosu k základnímu úložišti. Příklad:

- Průzkumník serveru ve Visual Studiu – Pokud jste nainstalovali Azure nástroje pro Microsoft Visual Studio můžete uzel Azure úložiště v Průzkumníku serveru zobrazíte objektů blob jen pro čtení a tabulku dat z Azure úložiště účtů. Zobrazení dat z vašeho účtu emulátoru místní úložiště a taky z úložiště účtů nevytvoříte pro Azure. Další informace najdete v tématu [procházení a Správa zdrojů úložiště v Průzkumníkovi serveru](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).

- [Microsoft Azure úložiště Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatné aplikace, který umožňuje snadno pracovat s daty Azure úložiště v systému Windows, OSX a Linux.

- [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) obsahuje správce Azure diagnostických nástrojů, který umožňuje zobrazení, stáhněte si a správa diagnostiky data shromážděná spuštěné v Azure.


## <a name="next-steps"></a>Další kroky

[Sledování tok aplikaci Cloudovým službám pomocí diagnostiky Azure](cloud-services-dotnet-diagnostics-trace-flow.md)
