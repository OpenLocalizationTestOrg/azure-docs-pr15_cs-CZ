<properties
    pageTitle="Azure Monitor neobsahovaly text běžné metriky. | Microsoft Azure"
    description="Zjistěte, jaký metriky se běžně používají pro neobsahovaly text cloudové služby, virtuálních počítačích a webových aplikací Web Apps."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitor neobsahovaly text běžné metriky

Azure neobsahovaly text Monitor umožňuje zobrazit počet spuštěné instance nahoru nebo dolů, podle telemetrickými daty (metriky). Tento dokument popisuje běžné metriky, který chcete použít. Na portálu Azure pro cloudovými službami a serverové farmy můžete metrické zdroje zobrazit jako. Však můžete také všechny metriky z různých zdrojů zobrazit jako.

Tady jsou podrobnosti o tom, jak najít a seznam, který chcete zobrazit jako metriky. Platí pro změnu velikosti sady měřítko virtuálního počítače i.

## <a name="compute-metrics"></a>Výpočet metriky
Ve výchozím nastavení Azure OM v2 součástí diagnostiky rozšíření nakonfigurované a mají následující metriky zapnutá.

- [Host metriky v2 Windows OM](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Host metriky v2 Linux OM](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Můžete použít `Get MetricDefinitions` rozhraní API/PoSH/rozhraní příkazového řádku zobrazíte metriky umožňující VMSS zdroje. 

Pokud používáte OM měřítko sady a nevidíte konkrétní metrické uvedené, bude pravděpodobně *Zakázané* v linky diagnostických nástrojů.

Pokud nevytváříte příslušné metrice vzorky nebo převedeny na požadovanou frekvenci chcete, můžete aktualizovat konfigurace diagnostiky.

Při splnění obou případech výše zkontrolujte [Pomocí prostředí PowerShell povolit Azure Diagnostika v virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) o prostředí PowerShell ke konfiguraci a aktualizovat linky Azure OM diagnostiky povolte metriky. Tento článek obsahuje taky ukázkový soubor konfigurace diagnostiky.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Výpočet metriky pro Windows OM v2 jako Host OS

Při vytváření nové OM (v2) v Azure diagnostiky aktivované pomocí diagnostických nástrojů rozšíření.

Seznam metriky si můžete vygenerovat pomocí následujícího příkazu Powershellu.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Můžete vytvořit upozornění pro následující metriky.

|Metriky název|   Jednotky|
|---|---|
|\Processor(_Total)\% Procesor času    |V procentech|
|\Processor(_Total)\% privilegovaného času   |V procentech|
|\Processor(_Total)\% uživatelského času |V procentech|
|Četnost \Processor \Processor informace (_Total) |Počet|
|\System\Processes| Počet|
|Počet \Thread \Process (_Total)| Počet|
|Počet \Handle \Process (_Total)  |Počet|
|\Memory\% potvrzeného bajtů používán   |V procentech|
|\Memory\Available bajtů|   Bajtů|
|\Memory\Committed bajtů    |Bajtů|
|\Memory\Commit limit|  Bajtů|
|\Memory\Pool stránkování bajtů|  Bajtů|
|\Memory\Pool fondu bajtů|   Bajtů|
|\PhysicalDisk(_Total)\% čas na disku| V procentech|
|\PhysicalDisk(_Total)\% čtení disku času|    V procentech|
|\PhysicalDisk(_Total)\% disk čas zápisu|   V procentech|
|\PhysicalDisk (_Total) \Disk převody/sec   |CountPerSecond|
|Čtení \Disk \PhysicalDisk (_Total) / sec   |CountPerSecond|
|\PhysicalDisk (_Total) \Disk zápisy/s  |CountPerSecond|
|\PhysicalDisk (_Total) \Disk bajty/s   |BytesPerSecond|
|Přečtěte si bajty/s \Disk \PhysicalDisk (_Total)| BytesPerSecond|
|Zápis \Disk \PhysicalDisk (_Total) bajtů/sec |BytesPerSecond|
|\Avg \PhysicalDisk (_Total). Délka fronty disku|  Počet|
|\Avg \PhysicalDisk (_Total). Délka fronty čtení disku| Počet|
|\Avg \PhysicalDisk (_Total). Délka fronty zápisu disku |Počet|
|\LogicalDisk(_Total)\% volného místa| V procentech|
|Megabajtů \Free \LogicalDisk (_Total)|   Počet|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Výpočet metriky Linux OM v2 jako Host OS

Při vytváření nové OM (v2) v Azure diagnostiky aktivované ve výchozím nastavení pomocí diagnostických nástrojů rozšíření.

Seznam metriky si můžete vygenerovat pomocí následujícího příkazu Powershellu.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Můžete vytvořit upozornění pro následující metriky.

|Metriky název                            |Jednotky|
|---|---|
|\Memory\AvailableMemory                |Bajtů|
|\Memory\PercentAvailableMemory         |V procentech|
|\Memory\UsedMemory                     |Bajtů|
|\Memory\PercentUsedMemory              |V procentech|
|\Memory\PercentUsedByCache             |V procentech|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Bajtů|
|\Memory\PercentAvailableSwap           |V procentech|
|\Memory\UsedSwap                       |Bajtů|
|\Memory\PercentUsedSwap                |V procentech|
|\Processor\PercentIdleTime             |V procentech|
|\Processor\PercentUserTime             |V procentech|
|\Processor\PercentNiceTime             |V procentech|
|\Processor\PercentPrivilegedTime       |V procentech|
|\Processor\PercentInterruptTime        |V procentech|
|\Processor\PercentDPCTime              |V procentech|
|\Processor\PercentProcessorTime        |V procentech|
|\Processor\PercentIOWaitTime           |V procentech|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Sekund|
|\PhysicalDisk\AverageWriteTime         |Sekund|
|\PhysicalDisk\AverageTransferTime      |Sekund|
|\PhysicalDisk\AverageDiskQueueLength   |Počet|
|\NetworkInterface\BytesTransmitted     |Bajtů|
|\NetworkInterface\BytesReceived        |Bajtů|
|\NetworkInterface\PacketsTransmitted   |Počet|
|\NetworkInterface\PacketsReceived      |Počet|
|\NetworkInterface\BytesTotal           |Bajtů|
|\NetworkInterface\TotalRxErrors        |Počet|
|\NetworkInterface\TotalTxErrors        |Počet|
|\NetworkInterface\TotalCollisions      |Počet|




## <a name="commonly-used-web-server-farm-metrics"></a>Běžně používaných metriky Web (serverové farmy)

Taky můžete provádět běžné webový server metrice například délka fronty Http automatické měřítko. Metriky název je **HttpQueueLength**.  V následující části jsou uvedeny dostupné serverové farmy (Web Apps) metriky.

### <a name="web-apps-metrics"></a>Metriky webové aplikace

Seznam webových aplikací Web Apps metriky si můžete vygenerovat pomocí následujícího příkazu Powershellu.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Upozornění na nebo změna velikosti tak, že tyto metrik.

|Metriky název        |Jednotky|
|---                |---|
|CpuPercentage      |V procentech|
|MemoryPercentage   |V procentech|
|DiskQueueLength    |Počet|
|HttpQueueLength    |Počet|
|BytesReceived      |Bajtů|
|BytesSent          |Bajtů|


## <a name="commonly-used-storage-metrics"></a>Běžně používaných metriky úložišť
Můžete změnit tak, že délka fronty úložiště, což je počet zpráv ve frontě úložiště. Délka fronty úložiště je speciální metrické a mezní hodnota použita bude počet zpráv na instance. To znamená, pokud existují dvě instance a když je mezní hodnota nastavené na 100, bude měřítko 200 po celkový počet zpráv. Například 100 zpráv v rámci instance.

Můžete nakonfigurovat jde v portálu Azure ve zásuvné **Nastavení** . U sady měřítko OM je možné aktualizovat nastavení automatické měřítko v šabloně ARM používat *metricName* jako *ApproximateMessageCount* a předejte ID fronty úložiště jako *metricResourceUri*.

Pod svým účtem klasické úložiště metricTrigger nastavení automatické měřítko by například:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

Pro účet úložiště (ne klasický), který by obsahoval metricTrigger:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Běžně používaných metriky Bus služby

Můžete změnit tak, že délka fronty Bus služby, což je počet zpráv ve frontě Bus služby. Délka fronty Bus služby je speciální metrické a prahové hodnoty zadané použité bude počet zpráv na instance. To znamená, pokud existují dvě instance a když je mezní hodnota nastavené na 100, bude měřítko 200 po celkový počet zpráv. Například 100 zpráv v rámci instance.

U sady měřítko OM je možné aktualizovat nastavení automatické měřítko v šabloně ARM používat *metricName* jako *ApproximateMessageCount* a předejte ID fronty úložiště jako *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] Pro službu Bus neexistuje koncept skupina zdroje, ale správce prostředků Azure vytvoří výchozí skupina zdroje jednotlivých oblastech. Skupina zdroje je obvykle ve formátu "Výchozí - ServiceBus-[Oblast]". Například 'Výchozí EastUS ServiceBus' "Výchozí WestUS ServiceBus", "Výchozí-ServiceBus-AustraliaEast" atd.
