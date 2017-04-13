<properties
   pageTitle="Shromáždění protokolů pomocí Linux Azure diagnostiky | Microsoft Azure"
   description="Tento článek popisuje, jak nastavit Azure diagnostiky shromáždit protokoly od služby struktury Linux clusteru spuštěné v Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Shromáždění protokolů pomocí diagnostických nástrojů Azure

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Pokud používáte struktury služby Azure clusteru, je vhodné shromáždit protokoly ze všech uzlů v centrálním umístění. S protokoly na jednom centrálním místě usnadňuje analyzovat a řešení problémů, ať už jde o ve vašich služeb, aplikace nebo samotného clusteru. Jedním ze způsobů nahrávat a shromáždění protokolů, je použít rozšíření Azure diagnostických nástrojů, které odešlou protokoly úložišti Azure. Můžete číst události z úložiště a, že je umístíte v produktu, například [Pružná vyhledávání](service-fabric-diagnostic-how-to-use-elasticsearch.md) nebo jiné řešení analýza protokolu.

## <a name="log-sources-that-you-might-want-to-collect"></a>Protokol zdroje, které můžete chtít shromáždit
- **Služba struktury protokolů**: vyzařováno platformu prostřednictvím [LTTng](http://lttng.org) a nahrát ke svému účtu úložiště. Protokoly mohou být provozní události nebo runtime události, které posílá platformu. Tyto protokoly jsou uloženy v umístění, které určuje manifest obrázku. (Pokud chcete získat podrobnosti o účtu úložiště, vyhledejte značku **AzureTableWinFabETWQueryable** a vyhledejte **StoreConnectionString**.)
- **Události aplikace**: vyzařováno vaše služba kódu. Můžete použít libovolný protokolování řešení, který data zapisuje textový protokoly – například LTTng. Další informace najdete v dokumentaci LTTng na trasování aplikace.  


## <a name="deploy-the-diagnostics-extension"></a>Nasazení rozšíření diagnostických nástrojů
Cílem prvního kroku v shromáždění protokolů je pro nasazení rozšíření diagnostiky značí VMs clusteru struktury služby. Diagnostika přípony shromažďují protokoly na každý OM a odešle úložiště účtu, který určíte. Kroky lišit v závislosti na tom, jestli používáte Azure portál nebo správce prostředků Azure.

Do kterých se nasadí koncovku diagnostiky VMs clusteru jako součást vytváření clusteru, nastavte **diagnostiky** na **zapnuto**. Po vytvoření clusteru nelze toto nastavení změníte tak, že na portálu.

Nakonfigurujte Linux Azure diagnostiky (LAD) můžete shromažďovat soubory a, že je umístíte do vašeho účtu úložiště. Tento proces se vysvětluje jako scénář 3 ("nahrát soubory protokolu") v článku [Použití LAD ke sledování a Diagnostika Linux VMs](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md). Sledovat tento proces získá přístup k trasování. Nahrajte stop na visualizer podle svého výběru.

Můžete taky nasadíte koncovku diagnostiky pomocí Správce prostředků Azure. Proces se podobá Windows Linux a uvedených clusterů Windows v [článku jak shromáždit protokoly s Azure diagnostiky](service-fabric-diagnostics-how-to-setup-wad.md).

Můžete také operace správy sadu podle popisu v [Operace správy sadu protokolu technologie pro analýzu s Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Po dokončení konfigurace agenta LAD sleduje zadaný protokoly. Pokaždé, když k souboru se přidá nový řádek, vytvoří novou položku syslog odeslaný k základnímu úložišti, kterou jste zadali.


## <a name="next-steps"></a>Další kroky
Podrobnější k jakým událostem měli zvážit při řešení problémů s, najdete v tématu [si přečtěte následující dokumentaci LTTng](http://lttng.org/docs) a [Pomocí LAD](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md).
