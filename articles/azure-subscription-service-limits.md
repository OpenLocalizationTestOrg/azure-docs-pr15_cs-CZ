<properties
    pageTitle="Microsoft Azure předplatné a omezení služby, kvót a omezení"
    description="Obsahuje seznam běžných Azure předplatné a omezení služby, kvót a omezení. Jedná se o informace o tom, jak zvýšit limity spolu s maximální hodnoty."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure předplatné a omezení služby, kvót a omezení

## <a name="overview"></a>Základní informace

Tento dokument určuje některé nejčastější limity Microsoft Azure. Tím se nezabývá aktuálně všechny služby Azure. V průběhu času tyto limity rozbalit a aktualizuje tak, aby lépe platformy.

Navštivte [Azure ceny přehled](https://azure.microsoft.com/pricing/) zobrazíte další informace o Azure ceny. Tady odhadu své náklady na použití [Ceny kalkulačky](https://azure.microsoft.com/pricing/calculator/) nebo tak, že na stránce cenových podrobnosti pro službu (například [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).

> [AZURE.NOTE] Pokud chcete zvýšit limit nad **Výchozí Limit**, můžete [Otevřít žádost o konverzaci online Zákaznická podpora zdarma](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Limity není možné zvýšit vyšší než **Maximální** hodnota v následující tabulce. Pokud se žádný sloupec **Maximální Limit** , pak je daný zdroj nemá měnitelné omezení.

## <a name="limits-and-the-azure-resource-manager"></a>Limity a Azure správce zdrojů

Nyní je možné zkombinování několika Azure zdrojů v jedné skupině Azure zdroje. Při použití skupiny zdrojů, budou omezení byly jednou globální spravuje místní úrovni pomocí Správce prostředků Azure. Další informace o skupinách zdroje Azure najdete v článku [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md).

V následujících omezení bylo přidáno novou tabulku tak, aby odrážely rozdíly v omezení při používání správce prostředků Azure. Zde je například tabulky **Předplatné omezení** a **Limity předplatné – správce prostředků Azure** . Když omezení platí pro oba scénáře, se zobrazí pouze v první tabulce. Není uvedeno jinak, jsou ve všech oblastech globální omezení.

> [AZURE.NOTE] Je důležité a zdůraznit tak, že kvóty pro zdroje v Azure skupinách zdroje jsou za oblasti přístupných osobám s postižením tak, že vaše předplatné a ostatní vás za předplatného, jsou služby Správa kvóty. Jako příklad použijeme základní kvóty. Pokud potřebujete s žádostí o zvýšení kvóty s podporou jádra, budete muset rozhodnout, kolik jádra, který chcete použít v oblasti, které a proveďte určitý žádost o kvóty core Azure pole Skupina zdroje pro částky a oblasti, které chcete. Proto, pokud je potřeba použít 30 jádra v západní Evropa ke spuštění aplikace. měli byste konkrétně požadovat 30 jádra v západní Evropy. Ale nebudete mít základní kvóty zvětšení v jakékoli jiné oblasti – pouze západní Evropa bude mít kvóty 30 core.
<!-- -->
Může v důsledku toho užitečné, zvažte rozhodnutí, co Azure pole Skupina zdroje kvóty musí být pro váš pracovní zátěž v jednom regionu a požádejte částky v jednotlivých oblastech, do kterého jsou zvažovat nasazení. V tématu [řešení potíží nasazení](./resource-manager-common-deployment-errors.md) další nápovědu k objevují ostatní aktuální kvóty pro konkrétní oblasti.

## <a name="service-specific-limits"></a>Limity specifických pro službu

- [Služby Active Directory](#active-directory-limits)
- [Rozhraní API správy](#api-management-limits)
- [Aplikace služby](#app-service-limits)
- [Aplikace brány](#application-gateway-limits)
- [Přehledy aplikace](#application-insights-limits)
- [Automatizace](#automation-limits)
- [Azure Redis mezipaměti](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Zálohování](#backup-limits)
- [Dávkové](#batch-limits)
- [BizTalk služby](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Cloud Services](#cloud-services-limits)
- [Data Factory](#data-factory-limits)
- [Jezera analýzy dat](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Rozbočovače události](#event-hubs-limits)
- [Rozbočovač IoT](#iot-hub-limits)
- [Klíčové trezoru](#key-vault-limits)
- [Mediální služby](#media-services-limits)
- [Mobilní zapojení](#mobile-engagement-limits)
- [Mobilní služby](#mobile-services-limits)
- [Sledování](#monitoring-limits)
- [Vícefaktorové ověřování](#multi-factor-authentication)
- [Sítě](#networking-limits)
- [Služba centrální oznámení](#notification-hub-service-limits)
- [Provozní přehledy](#operational-insights-limits)
- [Pole Skupina zdroje](#resource-group-limits)
- [Plánovač](#scheduler-limits)
- [Hledání](#search-limits)
- [Služba Bus](#service-bus-limits)
- [Obnovení webu](#site-recovery-limits)
- [Databáze SQL](#sql-database-limits)
- [Úložiště](#storage-limits)
- [Systém StorSimple](#storsimple-system-limits)
- [Technologie pro analýzu toku](#stream-analytics-limits)
- [Předplatné](#subscription-limits)
- [Přenosy správce](#traffic-manager-limits)
- [Virtuálních počítačích](#virtual-machines-limits)
- [Virtuální počítač měřítko sady](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Limity předplatného
#### <a name="subscription-limits"></a>Limity předplatného
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Limity předplatné – správce prostředků Azure

Platí následující omezení při používání správce prostředků Azure a skupin Azure zdrojů. Limity, které nebyly změněny pomocí Správce prostředků Azure nejsou uvedeny níže. Získáte v předchozí tabulce tyto limity.

Informace o zpracování omezení při žádosti správce prostředků najdete v článku [požadavky omezení správce prostředků](resource-manager-request-limits.md).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Pole Skupina zdroje omezení

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Limity virtuálních počítačích
#### <a name="virtual-machine-limits"></a>Limity virtuálního počítače
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Limity virtuálních počítačích – správce prostředků Azure

Platí následující omezení při používání správce prostředků Azure a skupin Azure zdrojů. Limity, které nebyly změněny pomocí Správce prostředků Azure nejsou uvedeny níže. Získáte v předchozí tabulce tyto limity.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Limity sady měřítko virtuálního počítače

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Limity sítě

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Limity sítě
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Omezení aplikace brány

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Omezení přenosy správce

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>Limity DNS

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Omezení úložiště

Další informace o účtu úložného prostoru najdete v článku [Azure úložiště škálovatelnost a výkonu cílů](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Omezení úložiště služby

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Limity disku virtuálního počítače

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Další informace najdete v článku [velikosti virtuálního počítače](../articles/virtual-machines/virtual-machines-linux-sizes.md) .

**Standardní úložiště účty**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Účty úložiště Premium**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Zprostředkovatele prostředků limitů úložiště

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Omezení služby cloudu

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>Omezení aplikace služby
Tyto limity aplikaci služby zahrnovat limity pro Web Apps, mobilní aplikace, rozhraní API aplikace a logika aplikace.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Plánovač omezení

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Limity dávku

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>Limity BizTalk služby
Následující tabulka zobrazuje limity Biztalk služby Azure.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>Limity DocumentDB

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Kvóty společně s [lze upravit kontaktováním podpory Azure](./documentdb/documentdb-increase-limits.md)hvězdička (*).

### <a name="mobile-engagement-limits"></a>Zapojení Mobile omezení

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Limity hledání

Ceny úrovní určují kapacitu a limity vyhledávací služby. Úrovně patří:

- *Uvolnit* více klienta služby, nasdílel jiných Azure předplatitele určené k vyhodnocení a small vývoje projektech.
- *Základní* obsahuje vyhrazené výpočetní prostředky pro výrobní úloh v menší měřítku s až tři replikami pracovního vytížení vysoce dostupné dotazu.
- *Standardní (S1, S2, S3, vysoké hustoty S3)* je určený pro větší pracovního vytížení výroby. Několik úrovní existují standardní osy tak, že zvolíte konfiguraci zdroje pro konkrétní scénáře.

**Limity jedno předplatné**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Limity za vyhledávací služby serveru**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Podrobnější informace o jiných omezení, včetně velikost dokumentu, dotazů za druhé, klíčů, požadavky a odpovědi najdete v článku [omezení služby Azure hledání](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Limity Media Services

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>Limity CDN

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Limity mobilní služby

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Sledování omezení

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Omezení počtu centrální služby oznámení

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Limity rozbočovače události

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Omezení služby Bus

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>Rozbočovač IoT omezení

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Limity Factory dat

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Limity jezera analýzy dat
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Limity analýzy toku
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Omezení služby Active Directory

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure limity RemoteApp

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>Limity StorSimple systému

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Provozní přehledy omezení

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Zálohování omezení

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Obnovení limitů webů

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Omezení přehledy aplikací

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>Rozhraní API správy omezení

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure limity Redis mezipaměti

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Limity trezoru klíč

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Vícefaktorové ověřování
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automatizace omezení
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Limity SQL databáze

Limity SQL databáze najdete v tématu [Limity prostředků databáze SQL](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Viz taky

[Principy Azure limity a zvyšuje](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtuální počítač a velikosti cloudové služby Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Velikost Cloudovým službám](cloud-services/cloud-services-sizes-specs.md)
