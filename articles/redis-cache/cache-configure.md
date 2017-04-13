<properties 
    pageTitle="Jak konfigurovat Azure Redis mezipaměť | Microsoft Azure"
    description="Princip výchozí konfigurace Redis Azure Redis mezipaměti a zjistěte, jak konfigurovat Azure Redis mezipaměti instance"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/25/2016"
    ms.author="sdanie" />

# <a name="how-to-configure-azure-redis-cache"></a>Jak konfigurovat mezipaměť Redis Azure

Toto téma popisuje, jak zkontrolujte a upravte nastavení mezipaměti Redis Azure instance a popisuje výchozí konfigurace serveru Redis pro instance Azure Redis mezipaměti.

>[AZURE.NOTE] Další informace o konfiguraci a používání prémiových funkcí mezipaměti najdete v článku [jak nakonfigurovat trvalé pro mezipaměť Redis Azure Premium](cache-how-to-premium-persistence.md), [jak nakonfigurovat clusterů pro mezipaměť Redis Azure Premium](cache-how-to-premium-clustering.md)a [jak nakonfigurovat virtuální sítě podpory pro mezipaměť Redis Azure Premium](cache-how-to-premium-vnet.md).

## <a name="configure-redis-cache-settings"></a>Konfigurace nastavení mezipaměti Redis

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis mezipaměti poskytuje následující nastavení na zásuvné **Nastavení** .

![Redis nastavení mezipaměti](./media/cache-configure/redis-cache-settings.png)



-   [Podporu a odstraňování problémů s nastavením](#support-amp-troubleshooting-settings)
-   [Obecná nastavení](#general-settings)
    -   [Vlastnosti](#properties)
    -   [Přístupových kláves](#access-keys)
    -   [Upřesňující nastavení](#advanced-settings)
    -   [Redis Advisor mezipaměti](#redis-cache-advisor)
-   [Nastavení](#scale-settings)
    -   [Ceny osy](#pricing-tier)
    -   [Redis velikost obrázku](#cluster-size)
-   [Nastavení správy dat](#data-management-settings)
    -   [Redis trvání dat](#redis-data-persistence)
    -   [Import nebo Export](#importexport)
-   [Nastavení správy](#administration-settings)
    -   [Restartujte počítač](#reboot)
    -   [Plán aktualizace](#schedule-updates)
-   [Nastavení diagnostických nástrojů](#diagnostics-settings)
-   [Nastavení sítě](#network-settings)
-   [Nastavení správy zdrojů](#resource-management-settings)

## <a name="support--troubleshooting-settings"></a>Podporu a odstraňování problémů s nastavením

Nastavení v části **Podpora + Poradce při potížích s** nabízí možnosti pro řešení problémů s mezipaměť.

![Podpora + řešení potíží](./media/cache-configure/redis-cache-support-troubleshooting.png)

Klikněte na **diagnostikování a řešení problémů** obdržet běžné problémy a strategie pro jejich řešení.

Klikněte na **protokolu činnosti** zobrazíte akce provádějí mezipaměť. Můžete také filtrování rozbalte toto zobrazení zahrnout další zdroje informací. Další informace o práci s protokolů auditování najdete v článku [zobrazení událostí a protokolů auditování](../monitoring-and-diagnostics/insights-debugging-with-events.md) a [auditování operace s správce prostředků](../resource-group-audit.md). Další informace o sledování událostí Azure Redis mezipaměti najdete v článku [operace a upozornění](cache-how-to-monitor.md#operations-and-alerts).

**Stav zdroje** sleduje zdroj a zjistíte, jestli je spuštěný podle očekávání. Další informace o stavu služby Azure prostředku najdete v článku [Přehled stavu Azure prostředků](../resource-health/resource-health-overview.md).

>[AZURE.NOTE] Stav zdroje nedokáže aktuálně zprávu o stavu instance Azure Redis mezipaměti použitý ve virtuální sítě. Další informace najdete v tématu [všechny funkce mezipaměti fungují hostování mezipaměti v VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

Klikněte na tlačítko **Nová žádost o podporu** otevřete žádost o podporu pro mezipaměť.

## <a name="general-settings"></a>Obecná nastavení

Nastavení v části **Obecné** umožňují přístup a konfigurovat tato nastavení mezipaměti.

![Obecná nastavení](./media/cache-configure/redis-cache-general-settings.png)

-   [Vlastnosti](#properties)
-   [Přístupových kláves](#access-keys)
-   [Upřesňující nastavení](#advanced-settings)
-   [Redis Advisor mezipaměti](#redis-cache-advisor)

### <a name="properties"></a>Vlastnosti

Klikněte na **Vlastnosti** k zobrazení informací o mezipaměť, včetně koncový bod mezipaměti a porty.

![Redis vlastnosti mezipaměti](./media/cache-configure/redis-cache-properties.png)

### <a name="access-keys"></a>Přístupových kláves

Klikněte na **přístupových kláves z verze** k zobrazení nebo obnovit přístupových kláves pro mezipaměť. Klávesy používají spolu s název hostitele a porty z zásuvné **Vlastnosti** připojení klientů k mezipaměť.

![Redis mezipaměti přístupových kláves](./media/cache-configure/redis-cache-manage-keys.png)






### <a name="advanced-settings"></a>Upřesňující nastavení

Tato nastavení jsou nakonfigurovat na zásuvné **Upřesnit nastavení** .

-   [Porty přístup](#access-ports)
-   [Zásady Maxmemory a vyhrazená maxmemory](#maxmemory-policy-and-maxmemory-reserved)
-   [Oznámení keyspace (Upřesnit nastavení)](#keyspace-notifications-advanced-settings)


### <a name="access-ports"></a>Porty přístup

Ve výchozím nastavení není k dispozici přístup protokol SSL pro nové mezipaměti. Povolit port a protokol SSL, klikněte na **Ne** **Povolit přístup jenom přes SSL** na **zásuvné upřesňující nastavení** a klikněte na tlačítko **Uložit**.

![Porty redis mezipaměti](./media/cache-configure/redis-cache-access-ports.png)

### <a name="maxmemory-policy-and-maxmemory-reserved"></a>Zásady Maxmemory a vyhrazená maxmemory

Nastavení **zásad Maxmemory** a **vyhrazená maxmemory** na zásuvné **Pokročilé nastavení** konfigurace zásad paměti mezipaměti. Nastavení **zásad maxmemory** nakonfiguruje vyřazení zásad pro ukládání do mezipaměti a **vyhrazená maxmemory** nakonfiguruje paměti vyhrazená pro procesy není mezipaměti.

![Redis zásady Maxmemory mezipaměti](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Zásady Maxmemory** umožňuje vybrat z následujících zásad vyřazení.

-   stálých lru – to je výchozí hodnota.
-   allkeys lru
-   náhodné stálých
-   náhodné allkeys
-   ttl stálých
-   noeviction

Další informace o maxmemory zásad najdete v tématu [zásady vyřazení](http://redis.io/topics/lru-cache#eviction-policies).

Nastavení **vyhrazená maxmemory** nakonfiguruje paměti v Megabajtech vyhrazené pro není mezipaměti operací, jako je replikace při selhání. Jej lze také obsahujících poměr vysoké fragmentace. Nastavení této hodnoty umožňuje jednotnější prostředí serveru Redis při zatížení se liší. Tato hodnota je třeba nastavit vyšší úloh, které jsou psaní těžké. Když paměti rezervováno pro tyto operace není k dispozici pro ukládání dat uložených v mezipaměti.

>[AZURE.IMPORTANT] Nastavení **vyhrazená maxmemory** je dostupný jenom u standardní a ukládání do mezipaměti Premium.

### <a name="keyspace-notifications-advanced-settings"></a>Oznámení keyspace (Upřesnit nastavení)

Redis keyspace oznámení je nastavený na zásuvné **Upřesnit nastavení** . Oznámení keyspace klientům umožnit, aby zasílat oznámení ve formě při určitých událostech výskytu.

![Redis mezipaměti upřesňující nastavení](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Keyspace oznámení a nastavení **oznámení událostí keyspace** jsou dostupné jenom pro standardní a ukládání do mezipaměti Premium.

Další informace najdete v tématu [Redis Keyspace oznámení](http://redis.io/topics/notifications). Ukázkový kód najdete v článku souboru [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) ve výběru [Vítáme](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) .

<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis Advisor mezipaměti

**Doporučení** zásuvné zobrazí doporučení pro mezipaměť. Během normální operací zobrazí se žádná doporučení. 

![Doporučení](./media/cache-configure/redis-cache-no-recommendations.png)

Pokud podmínek dojít během operace mezipaměť například velkého využití paměti, šířka pásma nebo zatížení serveru, zobrazí se upozornění na zásuvné **Redis mezipaměti** .

![Doporučení](./media/cache-configure/redis-cache-recommendations-alert.png)

Další informace najdete na zásuvné **doporučení** .

![Doporučení](./media/cache-configure/redis-cache-recommendations.png)

Můžete sledovat tyto metriky v části [Sledování](cache-how-to-monitor.md#monitoring-charts) a [používání grafy](cache-how-to-monitor.md#usage-charts) zásuvné **Redis mezipaměti** .

Každý ceny osy různých omezuje k připojení klientů, paměti a šířky pásma. Pokud mezipaměť blíží maximální kapacita pro tyto metriky po dobu delší dobu, vytvoří se doporučení. Další informace o metriky a omezení nástrojem **doporučení** najdete v následující tabulce.

| Redis míru mezipaměti      | Další informace naleznete v                                                  |
|-------------------------|---------------------------------------------------------------------------|
| Využití šířky pásma sítě | [Výkon mezipaměti – dostupné šířky pásma](cache-faq.md#cache-performance) |
| Připojení klientů       | [Výchozí konfigurace serveru Redis - maxclients](#maxclients)            |
| Zatížení serveru             | [Použití grafů – Redis zatížení serveru](cache-how-to-monitor.md#usage-charts)  |
| Využití paměti            | [Výkon mezipaměti – velikost](cache-faq.md#cache-performance)                |

Upgradovat mezipaměť, klikněte na **upgradovat** na změnit [ceny osy](#pricing-tier) a měřítka mezipaměť. Další informace o výběru ceny osy, najdete v článku [Jaké nabízení Redis mezipaměti a velikost mám použít?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-settings"></a>Nastavení

Nastavení v části **Měřítko** umožňují přístup a konfigurovat tato nastavení mezipaměti.

![Sítě](./media/cache-configure/redis-cache-scale.png)

-   [Ceny osy](#pricing-tier)
-   [Redis velikost obrázku](#cluster-size)

### <a name="pricing-tier"></a>Ceny osy

Klikněte na tlačítko **vrstvy ceny** si prohlédněte nebo změňte ceny osy mezipaměť. Další informace o měřítko najdete v článku [jak měřítko Azure Redis mezipaměti](cache-how-to-scale.md).

![Redis mezipaměti ceny osy](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### <a name="redis-cluster-size"></a>Redis velikost obrázku

Klikněte na **Velikost obrázku Redis (verze PREVIEW)** můžete změnit velikost obrázku pro průběžný premium mezipaměti s podporou clusterů.

>[AZURE.NOTE] Všimněte si, že při vydání osy Azure Redis mezipaměti Premium všeobecně dostupná funkce Redis velikost obrázku aktuálně v náhledu.

![Redis velikost obrázku](./media/cache-configure/redis-cache-redis-cluster-size.png)

Chcete-li změnit velikost obrázku, pomocí posuvníku nebo zadejte číslo od 1 do 10 do textového pole **počet Shard** a kliknutím na **OK** uložte.

>[AZURE.IMPORTANT] Redis clusterů je dostupný jenom u Premium mezipaměti. Další informace najdete v tématu [jak nakonfigurovat clusterů pro mezipaměť Redis Azure Premium](cache-how-to-premium-clustering.md).


## <a name="data-management-settings"></a>Nastavení správy dat

Nastavení v části **Správa dat** umožňují přístup a konfigurovat tato nastavení mezipaměti.

![Správa dat](./media/cache-configure/redis-cache-data-management.png)

-   [Redis trvání dat](#redis-data-persistence)
-   [Import nebo Export](#importexport)

### <a name="redis-data-persistence"></a>Redis trvání dat

Klikněte na tlačítko **Redis dat trvalé** povolení, zakázání nebo konfigurace trvání dat pro mezipaměť premium.

![Redis trvání dat](./media/cache-configure/redis-cache-persistence-settings.png)

K povolení Redis doby trvání, klikněte na možnost **Povolit** povolit zálohování RDB (Redis databáze). Pokud chcete zakázat Redis trvalé, klikněte na **Zakázané**.

Pokud chcete konfigurovat interval zálohování, vyberte z rozevíracího seznamu **Záložní četnosti** . Možnosti zahrnují **15 minut**, **30 minut**, **60 minut**, **6 hodin**, **12 hodin**a **24 hodin**. Tento interval počítá po předchozím zálohování úspěšném dokončení a když ji uplyne zálohu zahájení.

Klikněte na **Úložiště účtu** vyberte účet úložiště, který chcete použít a vyberte buď **primárního klíče** nebo **vedlejší klíč** z rozevíracího seznamu **Klíč úložiště** . Účet úložiště musí zvolte ve stejné oblasti jako mezipaměti a účet **Úložiště Premium** se nedoporučuje, protože úložiště premium má vyšší výkon. Pokaždé, když obnovených klávesu úložiště pro váš účet trvalé musí znovu vyberete požadovanou klíč z rozevíracího seznamu **Klíč úložiště** .

Kliknutím na **OK** uložte Konfigurace trvalé.

>[AZURE.IMPORTANT] Trvání redis dat je dostupný jenom u Premium mezipaměti. Další informace najdete v tématu [jak nakonfigurovat trvalé pro mezipaměť Redis Azure Premium](cache-how-to-premium-persistence.md).

### <a name="importexport"></a>Import nebo Export

Import nebo Export je operaci správy Azure Redis mezipaměti dat, která umožňuje importovat data do mezipaměti Redis Azure nebo exportovat data z mezipaměti Redis Azure import a Export snímku Redis mezipaměti databáze (RDB) z mezipaměti premium objektů blob stránky v účet Azure úložiště. To umožňuje migrace mezi jiné instance Azure Redis mezipaměti nebo vyplnění mezipaměti se data před použitím.

Import mohou sloužit k přenesení Redis kompatibilní RDB soubory z Redis serveru v cloudu nebo prostředí, včetně Redis na Linux, systému kteréhokoliv poskytovatele cloudu například Amazon webovým službám a další. Import dat je snadný způsob, jak vytvořit mezipaměť přednastavený daty. Při importu Azure Redis mezipaměť načte RDB soubory z Azure úložiště do paměti a vloží klávesy do mezipaměti.

Export umožňuje exportovat data uložená v mezipaměti Redis Azure k Redis kompatibilní soubory RDB. Tato funkce slouží k přesunutí dat z Azure Redis mezipaměti instancemi do jiného nebo jiné Redis server. Při exportu vytvořené v OM dočasný soubor, který hostuje instanci server Azure Redis mezipaměti a soubor odeslat k tomuto účtu určené úložiště. Po dokončení operace exportu se stavem úspěch nebo selhání dočasný soubor se odstraní.

>[AZURE.IMPORTANT] Import nebo Export je dostupný jenom u Premium osy mezipaměti. Další informace a pokyny najdete v tématu [Import a Export dat do mezipaměti Redis Azure](cache-how-to-import-export-data.md).


## <a name="administration-settings"></a>Nastavení správy

Nastavení v části **Správa** umožňuje provádět následující úkoly správy pro mezipaměť premium. 

![Správa](./media/cache-configure/redis-cache-administration.png)

-   [Restartujte počítač](#reboot)
-   [Plán aktualizace](#schedule-updates)

>[AZURE.IMPORTANT] Nastavení v této části jsou dostupné jenom pro Premium osy mezipaměti.

### <a name="reboot"></a>Restartujte počítač

**Restartujte** zásuvné umožňuje restartujte jeden nebo více uzlů mezipaměť. Umožňují testování aplikace odolnosti v případě se nepovede.

![Restartujte počítač](./media/cache-configure/redis-cache-reboot.png)

Pokud máte premium mezipaměti s podporou clusterů, můžete vybrat které shards mezipaměti restartujte počítač.

![Restartujte počítač](./media/cache-configure/redis-cache-reboot-cluster.png)

Restartujte jedné nebo více uzlů mezipaměť, vyberte požadované uzly a klikněte na **Restartovat**. Pokud máte premium mezipaměti s podporou clusterů, vyberte shard(s) restartujte počítač a pak klikněte na **Restartovat**. Po několika minutách vybrané uzly restartujte počítač a jsou zpět do stavu online později několik minut.

>[AZURE.IMPORTANT] Restartujte počítač je dostupný jenom u Premium osy mezipaměti. Další informace a pokyny najdete v článku [Správa mezipaměti Redis Azure – restartujte počítač](cache-administration.md#reboot).

### <a name="schedule-updates"></a>Plán aktualizace

**Plán aktualizace** zásuvné umožňuje určit údržby aktualizací Redis serveru pro mezipaměť. 

>[AZURE.IMPORTANT] Všimněte si, že okno Údržba platí pouze pro Redis serveru aktualizace a pozor, abyste všechny Azure aktualizuje nebo aktualizace, které operační systém VMs hostujících mezipaměti.

![Plán aktualizace](./media/cache-configure/redis-schedule-updates.png)

Chcete-li údržby, zaškrtněte požadovaná dnů a zadat hodiny úvodní okno údržbu pro každý den a klikněte na tlačítko **OK**. Všimněte si, že čas okno údržbu UTC. 

>[AZURE.IMPORTANT] Aktualizace plánu je dostupný jenom u Premium osy mezipaměti. Další informace a pokyny naleznete v tématu [Správa mezipaměti Redis Azure – plán aktualizace](cache-administration.md#schedule-updates).

## <a name="diagnostics-settings"></a>Nastavení diagnostických nástrojů

V části **Diagnostics** umožňuje nastavení diagnostických nástrojů pro mezipaměť Redis.

![Diagnostika](./media/cache-configure/redis-cache-diagnostics.png)

Klikněte na **Diagnostika** [Konfigurace účtu úložiště](cache-how-to-monitor.md#enable-cache-diagnostics) využít k ukládání diagnostických nástrojů mezipaměti.

![Redis diagnostiky mezipaměti](./media/cache-configure/redis-cache-diagnostics-settings.png)

Kliknutím na **Redis metriky** [zobrazení metriky](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) mezipaměti a **pravidla výstrah** nastavení [upozornění pravidel](cache-how-to-monitor.md#operations-and-alerts).

Další informace o diagnostiky Azure Redis mezipaměti najdete v článku [jak sledovat Azure Redis mezipaměti](cache-how-to-monitor.md).


## <a name="network-settings"></a>Nastavení sítě

Nastavení v části **síť** umožňují přístup a konfigurovat tato nastavení mezipaměti.

![Sítě](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Nastavení virtuální sítě jsou dostupné jenom pro premium mezipaměti, které byly nakonfigurovány s podporou VNET při vytváření mezipaměti. Další informace o vytvoření premium mezipaměti s VNET podporu a aktualizace nastavení, přečtěte si, [jak nakonfigurovat virtuální sítě podpory Premium Azure Redis mezipaměti](cache-how-to-premium-vnet.md).

## <a name="resource-management-settings"></a>Nastavení správy zdrojů

![Správa zdrojů](./media/cache-configure/redis-cache-resource-management.png)

V části **značky** vám pomůže uspořádat zdroje. Další informace najdete v tématu [použití značek k uspořádání Azure zdroje](../resource-group-using-tags.md).

V části **uzamčení** umožňuje uzamčení předplatného, skupina zdroje nebo zdroje zabránit ostatním uživatelům ve vaší organizaci mohli omylem odstranit nebo úprava důležitých prostředků. Další informace najdete v tématu [Lock zdroje pomocí Správce prostředků Azure](../resource-group-lock-resources.md).

V části **Uživatelé** podporuje řízení přístupu na základě rolí (RBAC) na portálu Azure pomůže organizace požadavkům jejich přístup správy jednoduše a přesně. Další informace najdete v tématu [řízení přístupu na základě rolí v portálu Azure](../active-directory/role-based-access-control-configure.md).

Klikněte na tlačítko **Exportovat šablonu** k vytvoření a export šablony nasazeném prostředků pro budoucí nasazení. Další informace o práci se šablonami tématech [nasazení se šablonami správce prostředků Azure](../resource-group-template-deploy.md).

## <a name="default-redis-server-configuration"></a>Výchozí konfigurace serveru Redis

Nové instance Azure Redis mezipaměti budou nakonfigurována následujících výchozích hodnot konfigurace Redis.

>[AZURE.NOTE] Nemůžete už změnit nastavení v této části pomocí `StackExchange.Redis.IServer.ConfigSet` metody. Pokud tato metoda nazývá s jeden z příkazů v této části, podobně jako následujícím příkladu je výjimka:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Všechny hodnoty, které jsou, která dokáže nahradit, například **max paměti zásad**, jsou neupravují Azure portál nebo příkazového řádku nástroje pro správu například rozhraní příkazového řádku Azure nebo Powershellu.

|Nastavení|Výchozí hodnota|Popis|
|---|---|---|
|databáze|16|Výchozí počet databáze je 16, ale můžete nakonfigurovat jinou hodnotu podle ceny osy. <sup>1</sup> výchozí databáze je DB 0, můžete zvolit jiný název na základě jednotlivá připojení pomocí `connection.GetDatabase(dbid)` kde dbid je v rozmezí `0` a `databases - 1`.|
|MaxClients|Závisí na ceny osy<sup>2</sup>|Toto je maximální počet připojení klientů povolených ve stejnou dobu. Jakmile je limitu Redis zavřete všechny nové připojení odesílání chyba "maximální počet klientů dosáhl".|
|maxmemory zásad|stálých lru|Zásady Maxmemory je nastavení pro Redis jak vybereme odeberte při maxmemory (velikost mezipaměti nabízející, že jste vybrali, když jste vytvořili mezipaměti). S Azure Redis mezipaměti je ve výchozím nastavení stálých lru, která odebere klávesy s platnosti sadu pomocí LRU algoritmus. Toto nastavení je možné konfigurovat Azure portálu. Další informace najdete v tématu [Maxmemory zásady a vyhrazená maxmemory](#maxmemory-policy-and-maxmemory-reserved).|
|maxmemory vzorky|3|LRU a minimální hodnota TTL algoritmů nejsou přesné algoritmů, ale budou přibližně algoritmy (za účelem uložení paměti), takže můžete taky vybrat velikost hodnoty ke kontrole. Například pro výchozí Redis zkontroluje tři klíče a vyberte požadovanou použitý méně naposledy.|
|LUA časový limit|5 000|Maximální čas spuštění skriptu Lua v milisekundách. Pokud je maximální dobu, po spuštění zaznamená Redis, skript stále probíhá spuštění po maximální povolenou dobu a začne odpovědi na dotazy, zobrazí se chybová zpráva.|
|omezení počtu LUA události|500|Toto je maximální velikost fronty skript události.|
|Klient výstup rezervy limit normalclient výstup rezervy limit pubsub|0 0 032mb 8mb 60|Limity vyrovnávací paměť výstup klienta mohou sloužit k vynucení odpojení klienti, které nejsou čtení dat ze serveru dost rychle z nějakého důvodu (běžným důvodem je, že Pub/Sub klienta nelze používat zprávy rychle jako vydavatel můžete předložit je). Další informace najdete v tématu [http://redis.io/topics/clients](http://redis.io/topics/clients).|

<a name="databases"></a>
<sup>1</sup> Limit `databases` je jiné u každé Azure Redis mezipaměti ceny osy a můžete rozmístěné po vytvoření mezipaměti. Pokud ne `databases` nastavení zadané při vytváření mezipaměti, výchozí hodnota je 16.

-   Základní a standardní mezipaměti
    -   C0 mezipaměti (250 MB) - až 16 databází
    -   C1 mezipaměti (1 GB) - až 16 databází
    -   C2 mezipaměti (2,5 GB) - až 16 databází
    -   C3 (6 GB) mezipaměti - až 16 databází
    -   C4 mezipaměti (13 GB) - až 32 databází
    -   C5 mezipaměti (26 GB) - až 48 databází
    -   C6 mezipaměti (53 GB) - až 64 databází
-   Premium mezipaměti
    -   P1 (6 GB - 60 GB) - až 16 databází
    -   P2 (13 GB - 130 GB) - až 32 databází
    -   P3 (26 GB - 260 GB) - až 48 databází
    -   P4 (53 GB - 530 GB) - až 64 databází
    -   Všechny mezipaměti premium s Redis clusteru povolené - Redis clusteru podporuje pouze použití databáze 0 proto `databases` omezení pro všechny premium mezipaměti s podporou clusterů Redis je efektivně 1 a příkazu [Select](http://redis.io/commands/select) není povolená. Další informace najdete v tématu [muset proveďte změny Moje klientské aplikaci používat clusterů?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] `databases` Nastavení může být nakonfigurováno pouze při vytváření mezipaměti a pouze pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiných správy klientů. Příklad konfigurace `databases` při vytváření mezipaměti pomocí Powershellu najdete v článku [AzureRmRedisCache nový](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a>
<sup>2</sup> `maxclients` je jiné u každé Azure Redis mezipaměti ceny osy.

-   Základní a standardní mezipaměti
    -   C0 mezipaměti (250 MB) - až 256 připojení
    -   C1 mezipaměti (1 GB) - až 1 000 připojení
    -   C2 mezipaměti (2,5 GB) - až 2 000 připojení
    -   C3 (6 GB) mezipaměti - až 5 000 připojení
    -   C4 mezipaměti (13 GB) - až 10 000 připojení
    -   C5 mezipaměti (26 GB) - až 15 000 připojení
    -   C6 mezipaměti (53 GB) - až 20 000 připojení
-   Premium mezipaměti
    -   P1 (6 GB - 60 GB) - až 7500 připojení
    -   P2 (13 GB - 130 GB) - až 15 000 připojení
    -   P3 (26 GB - 260 GB) - až 30 000 připojení
    -   P4 (53 GB - 530 GB) - až 40 000 připojení

## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis příkazy nejsou podporované v mezipaměti Redis Azure

>[AZURE.IMPORTANT] Následující příkazy jsou neaktivní, protože konfigurace a Správa mezipaměti Redis Azure instancí spravuje Microsoft. Pokud se pokusíte je vyvolat obdržíte podobná chybová zpráva `"(error) ERR unknown command"`.
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  KONFIGURACE
>-  LADĚNÍ
>-  MIGRACE
>-  ULOŽENÍ
>-  VYPNUTÍ
>-  SLAVEOF
>-  CLUSTER - clusteru zápisu příkazy jsou neaktivní, ale jen pro čtení clusteru příkazy jsou povoleny.

Další informace o příkazech Redis najdete v článku [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Redis konzoly

Bezpečné vydávat příkazy vašich instancí mezipaměti Redis Azure pomocí **Redis konzoly**, který je dostupný pro normu a ukládá Premium.

>[AZURE.IMPORTANT] Konzole Redis nefunguje s VNET clusterů a databáze jiné než 0. 
>
>-  [VNET](cache-how-to-premium-vnet.md) – Pokud mezipaměť je součástí VNET, pouze klienty v VNET přístup k mezipaměti. Protože konzole Redis používá klienta redis cli.exe hostitelem VMs, které nejsou součástí vašeho VNET, nemůže připojit k mezipaměť.
>-  [Clusterů](cache-how-to-premium-clustering.md) - konzola Redis využívá redis cli.exe klienta, který nepodporuje clusterů v současné době. Nástroj redis rozhraní příkazového řádku v [nestabilní](http://redis.io/download) větvi Redis úložiště na GitHub implementuje základní podpory při spuštění systému `-c` přepnout. Další informace najdete v článku [přehrávání s clusteru](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) na [http://redis.io](http://redis.io) v [Redis kurz obrázku](http://redis.io/topics/cluster-tutorial).
>-  Konzole Redis vytvoří nové připojení k databázi pokaždé, když odešlete příkazu 0. Nelze použít `SELECT` příkaz Vybrat jinou databázi, protože databáze se změní na 0 s jednotlivé příkazy. Informace o spuštění Redis příkazů, včetně změny na jinou databázi, najdete v článku [Jak lze spustit Redis příkazy?](cache-faq.md#how-can-i-run-redis-commands)

Přístup k konzole Redis, klikněte na **konzole** z zásuvné **Redis mezipaměti** .

![Redis konzoly](./media/cache-configure/redis-console-menu.png)

O vystavení příkazy proti instanci mezipaměti, jednoduše zadejte do požadovaného příkazu do konzoly.

![Redis konzoly](./media/cache-configure/redis-console.png)

Seznam Redis příkazy, které jsou zakázány Azure Redis mezipaměti najdete v předchozí části [Redis příkazy nejsou podporované v Azure Redis mezipaměti](#redis-commands-not-supported-in-azure-redis-cache) . Další informace o příkazech Redis najdete v článku [http://redis.io/commands](http://redis.io/commands). 

## <a name="move-your-cache-to-a-new-subscription"></a>Přesunutí mezipaměť k novému předplatnému

Mezipaměť můžete přesunout do nové předplatné kliknutím na **Přesunout**.

![Přesunutí Redis mezipaměti](./media/cache-configure/redis-cache-move.png)

Informace o přesunutí zdroje z jedné skupině zdrojů do druhého a od jedno předplatné do jiného najdete v článku [přesunutí zdrojů do nové skupiny prostředků nebo předplatného](../resource-group-move-resources.md).

## <a name="next-steps"></a>Další kroky
-   Další informace o práci s příkazy Redis najdete v tématu [Jak lze spustit Redis příkazy?](cache-faq.md#how-can-i-run-redis-commands).
