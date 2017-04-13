<properties 
   pageTitle="SQL databáze havárie obnovení cvičení | Microsoft Azure" 
   description="Přečtěte si pokyny a osvědčené postupy pro používání databáze SQL Azure provádět cvičení obnovení havárie, které vám pomohou zachovat vaše velice důležité podnikových aplikací pružné selhání a výpadků." 
   services="sql-database" 
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Provádění havárie obnovení podrobností

Je vhodné pravidelně za ověření připravenosti aplikace pro obnovení pracovního postupu. Ověření chování aplikace a důsledky ztrátou dat a/nebo narušení této převzetí zahrnuje je vhodné technickým. Je taky požadavek tak, že většina oborové standardy jako součást Certifikační kontinuitu business.

Obnovení podrobností havárie je tvořen:

- Simulace výpadku osy dat
- Obnovení 
- Ověření obnovení příspěvek integrity aplikace

V závislosti na tom, jak jste [navržený aplikace pro nepřerušený](sql-database-business-continuity.md), pracovní postup spustit přejít se může lišit. Tady vidíte jsme jsou popsány doporučené postupy provádění havárie obnovení přechod v rámci databáze SQL Azure. 

##<a name="geo-restore"></a>Obnovení GEO

Při přechodu obnovení havárie nechcete, aby možnou ztrátu dat, doporučujeme provést podrobností pomocí vytvoření kopie provozním prostředí a jeho použití k ověření pracovního postupu aplikace převzetí testovacím prostředí.
 
####<a name="outage-simulation"></a>Simulace výpadek

Tak, aby napodobily výpadku můžete odstranit nebo přejmenovat zdrojové databáze. Dojde k chybě připojení aplikace. 

####<a name="recovery"></a>Obnovení

- Zastupování Geo obnovení databáze do jiného serveru popsaných [v tomto poli](sql-database-disaster-recovery.md). 
- Změna konfigurace aplikace pro připojení k obnoveného databází a postupujte podle průvodce [konfigurací databáze po obnovení](sql-database-disaster-recovery.md) k dokončení obnovení.

####<a name="validation"></a>Ověření

- Dokončení přejít ověřením integrity příspěvek obnovení aplikace (tedy připojovací řetězec, přihlášení, základní funkce testování nebo jinou část ověření standardní aplikace signoffs postupy).

##<a name="geo-replication"></a>Replikace GEO

Pro databázi, která je chránit pomocí Geo replikace cvičení podrobností bude zahrnovat plánované převezme sekundárním databáze. Plánované převzetí zaručuje, že primární a sekundární databází zachovány synchronní při přepnutí role. Na rozdíl od neplánované převzetí operaci nebude mít za následek ztrátou dat tak podrobné lze provést v provozním prostředí. 

####<a name="outage-simulation"></a>Simulace výpadek

Tak, aby napodobily výpadku můžete zakázat webové aplikace nebo virtuální počítač připojen k databázi. Výsledkem bude problémů s připojením u webových klientů.

####<a name="recovery"></a>Obnovení

- Ujistěte se, konfiguraci aplikace v oblasti DR odkazuje na bývalého sekundární, které se změní na plně přístupné nové primární. 
- Provedení [plánované převzetí](sql-database-geo-replication-powershell.md#initiate-a-planned-failover) pro vytvoření nové primární sekundární databáze
- Postupujte podle průvodce [konfigurací databáze po obnovení](sql-database-disaster-recovery.md) k dokončení obnovení.

####<a name="validation"></a>Ověření

- Dokončení přejít ověřením integrity příspěvek obnovení aplikace (tedy připojovací řetězec, přihlášení, základní funkce testování nebo jinou část ověření standardní aplikace signoffs postupy).


## <a name="next-steps"></a>Další kroky

- Další informace o pokračování v organizacích, najdete v článku [kontinuitu scénáře](sql-database-business-continuity.md)
- Další informace o databáze SQL Azure automatické zálohy, najdete v článku [automatické zálohy databáze SQL](sql-database-automated-backups.md)
- Další informace o použití zálohy pro automatické obnovení najdete v tématu [obnovení databáze ze zálohy spuštěná služba](sql-database-recovery-using-backups.md)
- Další informace o rychlejší možnosti obnovení najdete v tématu [Aktivní Geo replikace](sql-database-geo-replication-overview.md)  
