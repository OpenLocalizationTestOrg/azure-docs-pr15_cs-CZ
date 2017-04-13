<properties 
    pageTitle="Úvod k osy Azure Redis mezipaměti Premium | Microsoft Azure" 
    description="Zjistěte, jak vytvořit a spravovat Redis trvalé, Redis clusterů a VNET podpora instance Azure Redis mezipaměti vašeho Premium osy" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Úvod k osy Azure Redis mezipaměti Premium
Azure Redis mezipaměť je distribuované spravovaných mezipaměti, který vám pomůže vytvořit vysoce scalable a neodpovídá aplikací zadáním velice rychlý přístup k datům. 

Nové osy Premium je Enterprise připravení osy obsahující všechny funkce jako standardní osy a informace, například lepší výkon, zvětšit zatížení, havárie obnovení, import nebo export a lepší zabezpečení. Pokračujte ve čtení Další informace o dalších funkcích osy mezipaměti Premium.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Lepší výkon ve srovnání s standardní nebo Basic osy
**Lepší výkon přes standardní nebo základní osy.** Mezipaměti ve vrstvě Premium jsou nasazené na hardware, který má rychlejší procesorů a poskytuje lepší výkonu srovnatelného Basic nebo standardní osy. Premium osy mezipaměti mít vyšší výkon a nižší čekacích dob. 

**Výkon stejné velikosti mezipaměti je vyšší v Premium ve srovnání s standardní osy.** Například výkon 53 GB P4 mezipaměť (Premium) je 250K žádosti o sekundu ve srovnání s 150 K pro C6 (standardní).

Další informace o velikosti výkon a šířky pásma s premium mezipaměti, najdete v článku [Azure Redis mezipaměti nejčastější dotazy týkající se](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis trvání dat
Vrstvy Premium umožňuje uchovávat data v mezipaměti v úložišti Azure účet. V mezipaměti Basic/standardní všechna data uložená jenom v paměti. V případě podkladového infrastruktury problémy tam je možné možnou ztrátu dat. Doporučujeme používat funkci Redis dat trvalé ve vrstvě Premium zvětšíte odolnost před ztrátou dat. Azure Redis mezipaměti RDB a AOF (už brzo) nabízí možnosti v [Redis trvalé](http://redis.io/topics/persistence). 

Pokyny ke konfiguraci trvalé najdete v článku [jak nakonfigurovat trvalé pro mezipaměť Redis Azure Premium](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Redis obrázku
Pokud chcete vytvořit mezipaměti větší než 53 GB nebo chcete shard dat v několika Redis uzlech, můžete Redis clusterů, která je dostupná ve vrstvě Premium. Každý uzel se skládá z mezipaměti pár primární/otevřené spravuje Azure vysoké dostupnosti. 

**Redis clusterů vám maximální měřítko a výkon.** Výkon zvyšuje lineárně jako zvýšení počtu shards (uzlů) v clusteru. Např. Pokud vytvoříte P4 clusteru 10 shards, je k dispozici výkon 250 kB * 10 = 2,5 milionů požadavky sekundu. Přečtěte si téma [Azure Redis mezipaměti nejčastější dotazy týkající se](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) Další informace o velikosti, výkon a šířky pásma s mezipamětí premium.

Začínáme s clusterů, najdete v článku [jak konfigurovat clusterů pro mezipaměť Redis Azure Premium](cache-how-to-premium-clustering.md).

##<a name="enhanced-security-and-isolation"></a>Rozšířené zabezpečení a izolace

Mezipaměti vytvořené ve vrstvě Basic nebo standardní jsou dostupné na veřejné Internetu. Přístup k mezipaměti je omezený podle přístupové klávesy. S osy Premium další zajistíte pouze klienty v rámci zadané síti přístup k mezipaměti. Můžete nasadit Redis mezipaměti v [Azure virtuální sítě (VNet)](https://azure.microsoft.com/services/virtual-network/). Můžete používat všechny funkce VNet například podsítí, zásady řízení přístupu a další funkce pro další omezení přístupu k Redis.

Další informace najdete v tématu [jak nakonfigurovat virtuální sítě podpory pro mezipaměť Redis Premium Azure](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Import nebo Export

Import nebo Export je operaci správy Azure Redis mezipaměti dat, která umožňuje importovat data do mezipaměti Redis Azure nebo exportovat data z mezipaměti Redis Azure import a Export snímku Redis mezipaměti databáze (RDB) z mezipaměti premium objektů blob stránky v účet Azure úložiště. To umožňuje migrace mezi jiné instance Azure Redis mezipaměti nebo vyplnění mezipaměti se data před použitím.

Import mohou sloužit k přenesení Redis kompatibilní RDB soubory z Redis serveru v cloudu nebo prostředí, včetně Redis na Linux, systému kteréhokoliv poskytovatele cloudu například Amazon webovým službám a další. Import dat je snadný způsob, jak vytvořit mezipaměť předem vyplněné daty. Během procesu importu Azure Redis mezipaměť načte RDB soubory z Azure úložiště do paměti a vloží klávesy do mezipaměti.

Export umožňuje exportovat data uložená v mezipaměti Redis Azure k Redis kompatibilní soubory RDB. Tato funkce slouží k přesunutí dat z Azure Redis mezipaměti instancemi do jiného nebo jiné Redis server. Při exportu vytvoříte dočasný soubor je vytvořen na OM hostujícího instanci server Azure Redis mezipaměti a soubor odeslat k tomuto účtu určené úložiště. Po dokončení operace exportu se stavem úspěch nebo selhání dočasný soubor se odstraní.

Další informace najdete v tématu [jak importovat data do a export dat z Azure Redis mezipaměti](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Restartujte počítač

Vrstvy premium umožňuje restartujte jeden nebo více uzlů vaší mezipaměti na vyžádání. Umožňuje provádět test aplikace odolnosti v případě se nepovede. Restartujte počítač následujících uzlů.

-   Hlavní uzel mezipaměť
-   Podřízeného uzlu mezipaměť
-   Hlavní a podřízený uzly mezipaměť
-   Pokud chcete použít premium mezipaměti s clusterů, restartujte předlohy, podřízeného nebo oba uzly pro jednotlivé shards v mezipaměti

Další informace najdete v tématu [Restartujte počítač](cache-administration.md#reboot) a [Nejčastější dotazy týkající se restartováním počítače](cache-administration.md#reboot-faq).

## <a name="schedule-updates"></a>Plán aktualizace

Funkce plánované aktualizace umožňuje určit údržby pro mezipaměť. Pokud není zadán okno Údržba, všechny aktualizace serveru Redis volání během v tomto okně. K označení údržby, vyberte požadované dny a zadejte zachování okno spustit hodinu pro každý den. Všimněte si, že čas okno údržbu UTC. 

Další informace najdete v tématu [plán aktualizace](cache-administration.md#schedule-updates) a [plán aktualizace časté otázky](cache-administration.md#schedule-updates-faq).

>[AZURE.NOTE] Pouze Redis serveru, ke kterému jsou aktualizovány během plánované údržby. Okno Údržba, nebudou mít vliv na Azure aktualizace nebo aktualizace OM operačního systému.

## <a name="to-scale-to-the-premium-tier"></a>Přejít premium osy

Zobrazit úroveň premium, jednoduše vyberte jednu z premium úrovní v zásuvné **změnit ceny osy** . Můžete také změnit velikost mezipaměť osy premium pomocí prostředí PowerShell a rozhraní příkazového řádku. Podrobné pokyny najdete v tématu [jak měřítko Azure Redis mezipaměti](cache-how-to-scale.md) a [jak k automatizaci operace změny měřítka](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Další kroky

Vytvořit mezipaměť a projděte si nové osy prémiových funkcí.

-   [Postup při konfiguraci trvalé pro mezipaměť Redis Premium Azure](cache-how-to-premium-persistence.md)
-   [Postup při konfiguraci virtuální sítě podpory pro mezipaměť Redis Premium Azure](cache-how-to-premium-vnet.md)
-   [Postup při konfiguraci clusterů pro mezipaměť Redis Premium Azure](cache-how-to-premium-clustering.md)
-   [Jak importovat data do a export dat z Azure Redis mezipaměti](cache-how-to-import-export-data.md)
-   [Způsob správy mezipaměti Redis Azure](cache-administration.md)
  

