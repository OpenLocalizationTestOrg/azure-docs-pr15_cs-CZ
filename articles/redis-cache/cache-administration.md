<properties 
    pageTitle="Postup při správě Azure Redis mezipaměti | Microsoft Azure"
    description="Zjistěte, jak provádět správu například restartujte počítač a plán aktualizace pro mezipaměť Redis Azure"
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="how-to-administer-azure-redis-cache"></a>Způsob správy mezipaměti Redis Azure

Toto téma popisuje, jak provádět úlohy správy například restartování a plánování zavedli instance Azure Redis mezipaměti.

>[AZURE.IMPORTANT] Nastavení a funkcí popsaných v tomto článku jsou k dispozici pouze pro Premium osy mezipaměti.


## <a name="administration-settings"></a>Nastavení správy

Nastavení mezipaměti Redis Azure **správy** umožňuje provádět následující úkoly správy pro mezipaměť premium. Přístup k nastavení správy, klikněte na **Nastavení** nebo **všechny** z zásuvné Redis mezipaměti a posuňte se v zásuvné **Nastavení** v části **Správa** .

![Správa](./media/cache-administration/redis-cache-administration.png)

-   [Restartujte počítač](#reboot)
-   [Plán aktualizace](#schedule-updates)

## <a name="reboot"></a>Restartujte počítač

**Restartujte** zásuvné umožňuje restartujte jeden nebo více uzlů mezipaměť. Umožňují testování aplikace odolnosti v případě se nepovede.

![Restartujte počítač](./media/cache-administration/redis-cache-reboot.png)

Pokud máte premium mezipaměti s podporou clusterů, můžete vybrat které shards mezipaměti restartujte počítač.

![Restartujte počítač](./media/cache-administration/redis-cache-reboot-cluster.png)

Restartujte jeden nebo více uzlů mezipaměť, vyberte požadovanou uzlů a klikněte na **Restartovat**. Pokud máte premium mezipaměti s podporou clusterů, vyberte shard(s) restartujte počítač a pak klikněte na **Restartovat**. Po několika minutách vybrané uzly restartujte počítač a jsou zpět do stavu online později několik minut.

Dopad na klientské aplikace se liší podle toho, že restartujete uzly.

-   **Hlavní** – po restartování předlohy uzel Azure Redis mezipaměť selhání na uzel otevřené a podporuje předlohy. Při tomto selhání může být krátké interval, ve kterém může selhat připojení do mezipaměti.
-   **Podřízený** – po restartování podřízeného uzlu se obvykle používá žádný vliv klientům mezipaměti.
-   **Hlavní i podřízený** – po restartování uzlech mezipaměti, všechna data dojde ke ztrátě v mezipaměti a připojení k selhání mezipaměti, dokud primární uzel přejde do režimu online. Pokud jste nakonfigurovali [trvání dat](cache-how-to-premium-persistence.md), nejnovější zálohy obnoveny mezipaměti přejde do režimu online. Všimněte si, že budou ztraceny všechny zápisy mezipaměti, ke kterým došlo za posledních záložní kopii.
-   **Uzly premium mezipaměti s podporou clusterů** – po restartování uzly premium mezipaměti s podporou clusterů, chování je stejná jako když restartujete uzly neseskupené mezipaměti.


>[AZURE.IMPORTANT] Restartujte počítač je dostupný jenom u Premium osy mezipaměti.

## <a name="reboot-faq"></a>Restartujte časté otázky

-   [Který uzel restartujte otestovat aplikace?](#which-node-should-i-reboot-to-test-my-application)
-   [Můžete restartovat mezipaměti zrušte připojení klientů?](#can-i-reboot-the-cache-to-clear-client-connections)
-   [Můžu ztratí data z mé mezipaměti pokud udělat restartování počítače?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
-   [Můžete restartujte svůj mezipaměti pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné nástroje pro správu?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
-   [K čemu ceny úrovně můžete použít funkci restartujte počítač?](#what-pricing-tiers-can-use-the-reboot-functionality)


### <a name="which-node-should-i-reboot-to-test-my-application"></a>Který uzel restartujte otestovat aplikace?

Testování odolnost proti chybám aplikace před selháním primární uzlu mezipaměť, restartujte uzel **předlohy** . Testování odolnost proti chybám aplikace před selháním sekundární uzel, restartujte **podřízeného** uzlu. Chcete-li otestovat odolnost proti chybám aplikace proti celkové selhání mezipaměti, restartujte **uzlech** .

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Můžete restartovat mezipaměti zrušte připojení klientů?

Ano, pokud restartujte mezipaměti všechna připojení klientů vymažou. To můžete užitečné v případě všechna připojení klientů použití, například z důvodu logiky chybu nebo chybu v klientské aplikaci. Každý ceny osy obsahuje jiné [omezení připojení klientů](cache-configure.md#default-redis-server-configuration) pro různé formáty a jakmile tyto limitu, žádné další připojení klientů přijaté. Restartování mezipaměti umožňuje vymazat všechna připojení klientů.

>[AZURE.IMPORTANT] Použijete-li připojení klientů kvůli logiky chybu nebo chybu v kód klienta, Všimněte si, že bude po návrat do online režimu uzel Redis StackExchange.Redis automatické připojení. Pokud základní problém nevyřeší, zůstanou připojení klientů použít.

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Můžu ztratí data z mé mezipaměti pokud udělat restartování počítače?

Pokud restartujte **hlavní** a **podřízený** uzly všechna data v mezipaměti (nebo v této shard, pokud používáte premium mezipaměti s podporou clusterů) bude ztracen. Pokud jste nakonfigurovali [trvání dat](cache-how-to-premium-persistence.md), bude nejnovější zálohy obnoven mezipaměti vycházejí návrat do online režimu. Všimněte si, že budou ztraceny všechny zápisy mezipaměti, které se uskutečnily po byla záloha vytvořena.

Pokud restartujete jenom jeden z uzlů, data budou ztracena není obvykle, ale přesto může být. Například pokud po restartování uzel předlohy a zápisu mezipaměti probíhá, data z mezipaměti zápisu je ztraceny. Jiné scénář ztrátou dat bude pokud restartujte jeden uzel a na uzel se stane s přejděte selhání ve stejnou dobu. Další informace o možných příčin ztrátou dat najdete v tématu [Co se stalo s mými daty v Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Můžete restartujte svůj mezipaměti pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné nástroje pro správu?

Ano, prostředí PowerShell pokyny najdete v článku [k restartujte Redis mezipaměti](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>K čemu ceny úrovně můžete použít funkci restartujte počítač?

Restartujte počítač je k dispozici pouze v premium ceny osy.

## <a name="schedule-updates"></a>Plán aktualizace

**Plán aktualizace** zásuvné umožňuje určit údržby pro mezipaměť. Pokud není zadán okno Údržba, všechny aktualizace serveru Redis volání během v tomto okně. Všimněte si, že okno Údržba platí pouze pro Redis serveru aktualizace a pozor, abyste všechny Azure aktualizuje nebo aktualizace, které operační systém VMs hostujících mezipaměti.

![Plán aktualizace](./media/cache-administration/redis-schedule-updates.png)

Chcete-li údržby, zaškrtněte požadovaná dnů a zadat hodiny úvodní okno údržbu pro každý den a klikněte na tlačítko **OK**. Všimněte si, že čas okno údržbu UTC. 

>[AZURE.NOTE] Okno výchozí Údržba aktualizací je pěti hodin. Tato hodnota se konfigurovat z portálu Microsoft Azure, ale můžete ji nakonfigurovat pomocí prostředí PowerShell `MaintenanceWindow` parametr rutinu [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) . Další informace najdete v tématu [dá ovládat plánované aktualizace pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné nástroje pro správu?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Naplánování aktualizace časté otázky

-   [Když aktualizace dojít, pokud nechcete používat funkci plán aktualizace?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
-   [Jaký druh aktualizace jsou určené během plánované údržby?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
-   [Umožňuje spravovat plánované aktualizace pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné nástroje pro správu?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
-   [K čemu ceny úrovně můžete použít funkci plán aktualizace?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Když aktualizace dojít, pokud nechcete používat funkci plán aktualizace?

Pokud nezadáte údržby, můžete kdykoli volání aktualizace.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Jaký druh aktualizace jsou určené během plánované údržby?

Pouze Redis serveru, ke kterému jsou aktualizovány během plánované údržby. Okno Údržba, nebudou mít vliv na Azure aktualizace nebo aktualizace OM operačního systému.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Umožňuje spravovat plánované aktualizace pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné nástroje pro správu?

Ano, můžete spravovat svoje plánované aktualizace pomocí následující rutiny prostředí PowerShell.

-   [Get-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763835.aspx)
-   [Nové AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
-   [Nové AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
-   [Odebrat AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>K čemu ceny úrovně můžete použít funkci plán aktualizace?

Plán aktualizace je k dispozici pouze v premium ceny osy.

## <a name="next-steps"></a>Další kroky

-   Prozkoumání dalších funkcí [Azure Redis mezipaměti premium osy](cache-premium-tier-intro.md) .





