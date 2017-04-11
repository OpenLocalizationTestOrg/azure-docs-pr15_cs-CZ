<properties
   pageTitle="Protokolování diagnostiky Azure dávku | Microsoft Azure"
   description="Nahrávání a analyzovat diagnostickém protokolu událostí pro Azure dávku účtu zdrojů, jako jsou fondů a úkoly."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Azure dávku diagnostické protokolování

Stejně jako u mnoha Azure služeb službu dávku posílá protokolu událostí pro některé zdroje po dobu platnosti zdroje. Povolení protokoly pro diagnostiku Azure dávku záznamu událostí pro zdrojů, jako jsou fondů a úkoly a potom použijte protokoly pro diagnostiku hodnocení a sledování. Vytvoření události, jako třeba fondu, fondu odstranit zahájení úkolu, dokončení úkolu a ostatní jsou součástí protokoly pro diagnostiku dávku.

>[AZURE.NOTE] Tento článek popisuje protokolování událostí pro zdroje účtu dávku sami, není projektu a úkolu výstupní data. Informace o ukládání výstupní data projektů a úkolů najdete v článku [přetrvávají dávku Azure výstupu projektu a úkolu](batch-task-output.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

* [Účet Azure dávku](batch-account-create-portal.md)

* [Účet Azure úložiště](../storage/storage-create-storage-account.md#create-a-storage-account)

  Zachování protokoly pro diagnostiku dávku, musíte vytvořit účet Azure úložiště kde Azure ukládat protokoly. Zadejte tento účet úložiště při [protokolování diagnostiky povolte](#enable-diagnostic-logging) pro váš účet dávku. Úložiště účet, který určíte, když povolíte kolekce protokolů není stejný jako účet propojené úložiště uvedených v článku [balíčků aplikací](batch-application-packages.md) a [uchovávání výstup úkolu](batch-task-output.md) .

  >[AZURE.WARNING] Jste **Účtovaná** za dat uložených v úložišti Azure účtu. Jedná se o protokolech diagnostiky popisované v tomto článku. Mějte na paměti při návrhu svoje [zásady uchovávání informací protokolu](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).

## <a name="enable-diagnostic-logging"></a>Povolení protokolování diagnostiky

Ve výchozím nastavení účtu dávku není povoleno diagnostické protokolování. Musíte výslovně povolit diagnostické protokolování pro každý účet dávku, kterou chcete sledovat:

[Jak povolit kolekce protokoly pro diagnostiku](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Doporučujeme, abyste v celé článku [základní informace o Azure protokoly pro diagnostiku](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) se porozuměli nejen jak povolit protokolování, ale kategorie protokolu různé služby Azure. Například Azure dávku aktuálně podporuje jednu kategorii protokolu: **Protokoly služby**.

## <a name="service-logs"></a>Protokoly služby

Protokoly služby Azure dávku obsahovat události vyzařovaného službou Azure dávku po dobu platnosti dávku zdroje jako fondu nebo úkolů. Každou událost, které dávku uložený v zadaném účtu úložiště ve formátu JSON. To je například textu ukázkové **fondu vytvoření události**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Každý textu události je umístěn v souboru .json v zadaném účet Azure úložiště. Pokud chcete protokoly přímý přístup, můžete chtít zkontrolovat [schématu protokoly pro diagnostiku účtu úložiště](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Služba protokolu událostí

Služba dávku aktuálně posílá následujících služby protokolu událostí. Tento seznam nemusí být vyčerpávající, protože dalších událostí může byly přidány od naposledy aktualizovaly tohoto článku.

| **Služba protokolu událostí** |
| ------------------ |
| [Vytvoření fondu][pool_create] |
| [Fond odstranit start][pool_delete_start] |
| [Odstranit fondu dokončení][pool_delete_complete] |
| [Zahájení fondu změny velikosti][pool_resize_start] |
| [Dokončení změny velikosti fondu][pool_resize_complete] |
| [Zahájení úkolu][task_start] |
| [Dokončený úkol][task_complete] |
| [Selhalo: úkolu][task_fail] |

## <a name="next-steps"></a>Další kroky

Kromě ukládání diagnostickém protokolu událostí v úložišti Azure účet, můžete taky vysílat dávku služby protokolu událostí k [Centrální události Azure](../event-hubs/event-hubs-what-is-event-hubs.md)a odešlete [Azure protokolu analýzy](../log-analytics/log-analytics-overview.md).

* [Azure diagnostické protokoly událostí rozbočovače můžete vysílat datovými proudy](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Můžete Streamovat dávku diagnostických událostí ke službě průniku vysoce scalable dat rozbočovače události. Událost rozbočovače můžete jedí miliony události sekundu, které lze transformace a ukládat používat jakéhokoli poskytovatele v reálném čase analýzy.

* [Analýza Azure protokoly pro diagnostiku pomocí protokolu analýzy](../log-analytics/log-analytics-azure-storage-json.md)

  Odešlete diagnostické protokoly protokolu analýzy, kde můžete analyzovat na portálu Správa operace sady (OMS) nebo exportovat pro analýzy v Power BI nebo Excel.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
