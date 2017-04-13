<properties
    pageTitle="Plánování upgradu na SQL databáze V12 | Microsoft Azure"
    description="Popisuje přípravy a omezení zahrnuté do upgradu na verzi V12 databáze SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genemi"/>


# <a name="plan-and-prepare-to-upgrade-to-sql-database-v12"></a>Plánování a příprava na upgrade na SQL databáze V12


Toto téma popisuje plánování a přípravy je nutné provést upgradovat databáze Azure SQL z verze V11 V12.


Nový [Azure portál](https://portal.azure.com/) neexistuje podpora upgradu na V12.


V následující tabulce jsou uvedeny další témata nápovědy pro V12.


| Nadpis a odkaz | Popis obsahu |
| :--- | :--- |
| [Co je nového v SQL databázi V12](sql-database-v12-whats-new.md) | Popisuje podrobnosti o tom, jak V12 přiblíží blízkosti databáze SQL Azure k úplné dostupná se serverem Microsoft SQL Server. |
| [Vytvoření databáze v SQL databázi V12](sql-database-get-started.md) | Popisuje, jak můžete vytvořit novou databázi Azure SQL u verze V12. Popisuje různé možnosti za právě prázdná databáze. |


## <a name="plan-ahead"></a>Plánování dopředu


Následující pododdílu popisují učení a rozhodování, které je třeba věnovat před zpřístupněním akce směrem k upgradu databáze Azure SQL na V12.

### <a name="version-clarification"></a>Vysvětlení verze


V tomto dokumentu se týká upgradu databázi Microsoft Azure SQL z verze V11 V12. Další dříve čísla verze jsou zavřít těchto dvou hodnot, vykázání příkazu jazyce Transact-SQL **Vyberte @@version; ** :


- 12.0.2000.8 *(nebo vyšší, bit V12)*
- 11.0.9228.18 *(V11)*
 - Někdy V11 byl označovány jako SAWA v2.

### <a name="service-tier-planning"></a>Služby plánování osy


Počínaje V12 databáze SQL Azure podporuje pouze – úrovně služeb s názvem základní, Standard a Premium. Web pro podnikatele vrstvy služeb není podporována ve V12. Proto pokud nebudete chtít upgradovat databáze Azure SQL, která je aktuálně vrstvy služeb Web nebo Business, musíte rozhodnout, co by měl být nové vrstvy služeb.


Podrobné informace o úrovní služby základní, Standard a Premium najdete tady:

- [Databáze SQL – úrovně služeb](sql-database-service-tiers.md)
- [Upgrade webových/firmy databáze SQL databáze na nové – úrovně služeb](sql-database-upgrade-server-portal.md)



### <a name="review-the-geo-replication-configuration"></a>Kontrola konfigurace Geo replikace


Pokud databázi Azure SQL nakonfigurovaný Geo replikace, by měl dokumentu aktuální konfigurace a zastavit Geo replikace, než začnete upgradu Příprava akce. Po dokončení upgradu je třeba překonfigurovat Geo replikace databáze.


Strategie je ponechat zdroji beze změn a testování na kopii databáze.


## <a name="preparation-actions"></a>Příprava akce


Pokud jste dokončili, plánování, můžete provádět akce kroků popsaných v následujících pododdílu Příprava na upgrade závěrečné.


Podrobný popis závěrečné upgradu jsou k dispozici v tématech nápovědy, které jsou propojené s v horní části tohoto tématu nápovědy.


### <a name="service-tier-actions"></a>Akce osy služby


Služba Web a obchodní ceny osy není podporována ve V12.


Pokud databázi V11 Azure SQL je Web nebo Business databáze, procesu upgradu vám nabídne možnost databáze přejděte podporovaná osy. Upgrade doporučuje osy, která historii pracovního vytížení databáze. Však můžete podporované vrstvy, kterou chcete.


Zmenšení jsou kroky nutné během upgradu přepnutím V11 databáze od osy Web a firmy před zahájením upgradu. Lze provést pomocí nového [Azure portálu](https://portal.azure.com/).


Pokud si nejste jisti, které vrstvy služeb přejdete do, S2 úroveň Standardní osy pravděpodobně smysluplné počáteční volbu. Nižší úrovně, bude mít méně zdrojů než osy Web a Business měli.


### <a name="suspend-geo-replication-during-upgrade"></a>Pozastavení Geo replikace během upgradu


Upgrade na V12 nejde spustit Pokud Geo replikace je aktivní v databázi. Je třeba nejprve překonfigurovat přestat používat Geo replikace databáze.


Po dokončení upgradu můžete nakonfigurovat znovu použít Geo replikace databáze.


### <a name="open-ports-on-an-azure-vm-for-client-connectivity"></a>Otevřete porty na Azure OM k připojení klientů


Pokud se klientským programem připojuje k SQL databázi V12 v průběhu svému klientovi na Azure virtuálního počítače (OM), musíte otevřít následující rozsahy portů na OM:

- 11000 11999
- 14000 14999


Klikněte na [zde](sql-database-develop-direct-route-ports-adonet-v12.md) podrobnosti o porty V12 databáze SQL. Porty jsou vyžadovány zvýšení výkonu v SQL databázi V12.


##<a id="limitations"></a>Omezení při a po upgradu na V12


### <a name="portals-for-v12"></a>Portály V12


Existují tři portály Azure a každý z nich má jiný možností týkajících se V12 databáze SQL.


- [http://Portal.Azure.com/](https://portal.azure.com/)<br/>Tento portál Azure nové a je pořád v náhledu stav. Tento portál úplně ještě není v celé obecný dostupnosti (GA). Tento portál:
 - Můžete spravovat V12 server a databázi.
 - Upgradovat databáze V11 V12.


- [http://Manage.windowsazure.com/](http://manage.windowsazure.com/)<br/>Tento Azure klasické portál může postupně postupně. Tento portál:
 - Můžete spravovat V12 server a databázi.
 - Můžete *není* upgrade vašeho V11 databáze V12.


- (http://*název_serveru*. database.windows.net)<br/>Azure SQL databáze klasické portálu:
 - Můžete*není* Správa V12 servery.


Budeme rádi, když připojení k databázím Azure SQL pomocí aplikace Visual Studio 2015 (VS2015). VS2015 se dá použít pro úkoly, například takto:


- Ke spuštění příkazu Transact-SQL.
- Navrhnout schéma.
- Chcete-li vytvořit databázi, online nebo offline.


Nebo místo toho zdarma si můžete stáhnout [Visual Studio 2015 komunity](https://www.visualstudio.com/vs/community/), což je verze plnohodnotně vybaveného VS2015.


Na starší Azure klasické portálu na stránce databáze kliknete na **Otevřít v aplikaci Visual Studio** spuštění VS2015 ve vašem počítači pro připojení k databázi SQL Azure.


Pro další možnost můžete SQL Server Management Studio (SSMS) 2014 s [CU6](http://support.microsoft.com/kb/3031047/) připojení k databázi SQL Azure. Víc informací se na tento příspěvek blogu:<br/>[Klientské nástroje aktualizace z databáze SQL Azure](https://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/).


> [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="limitation-during-upgrade-to-v12"></a>Omezení *Při* upgradu na V12


Při upgradu na V12 zůstávají k dispozici pro přístup k datům V11 databáze. Je ještě pár omezení vzít v úvahu.


| Omezení | Popis |
| :--- | :--- |
| Doba trvání upgradu | Doba trvání upgrade závisí na velikosti, edition a počet databáze na serveru. Proces upgradu spuštěním hodin dnů pro servery zejména servery, které má databáze:<br/><br/>*Větší než 50 GB nebo<br/> * U vrstvy není premium služeb<br/><br/>Vytvoření nové databáze na serveru během upgradu můžete taky zvětšit upgradu doba trvání. |
| Žádné Geo replikace | GEO replikace není podporována na serveru V12, která je aktuálně účastní upgradu z V11. |
| Databáze stručně v není k dispozici závěrečné upgradovat na V12 | Databáze patří k serveru V11 zůstávají k dispozici během upgradu. Připojení k serveru a databáze je však stručně není k dispozici ve fázi konečný při přechodu myší počínaje V11 k připravení V12.<br/><br/>Přepnout období v rozsahu 40 sekund až 5 minut. Pro většinu serverů přepnout přes očekává dokončete 90 vyvolané. Přechod v čase zvyšuje serverech, který máte velký počet databáze nebo při databází máte hodně zápisu úloh. |


### <a name="limitation-after-upgrade-to-v12"></a>Omezení *Po* upgradu na V12


| Omezení | Popis |
| :--- | :--- |
| Nejde vrátit k V11 | Po upgradu na místě nejde vrátit výsledek nebo vrátit zpět. |
| Web nebo Business osy | Po upgradu spuštění serveru pro novou databázi V12 můžete už rozpoznat nebo přijmout vrstvy služeb Web nebo Business. |



### <a name="export-and-import-after-upgrade-to-v12"></a>Export a import *Po* upgradu na V12


Můžete exportovat nebo importovat databázi V12 pomocí [Portálu Azure](https://portal.azure.com/). Nebo můžete exportovat nebo importovat pomocí některého z následujících nástrojů:


- SQL Server Management Studio (SSMS)
- Visual Studio 2015
- Datové vrstvy aplikace Framework (DacFx)


Však pomocí nástrojů, musíte nejdřív nebo nainstalujte jejich zajistit že podporují s novými funkcemi V12:


- [Kumulativní aktualizace 6 pro SQL Server Management Studio 2014](http://support2.microsoft.com/kb/3031047)
- [Aktualizace únor 2015 pro databáze systému SQL Server nástroje ve Visual Studiu 2013](https://msdn.microsoft.com/data/hh297027)
- [Únor 2015 osy dat aplikace rámec (DacFx) pro V12 databáze Azure SQL](http://www.microsoft.com/download/details.aspx?id=45886)


> [AZURE.NOTE] Předchozí texty odkazů na nástroje byly aktualizovány nebo později 2 březnem 2015. Doporučujeme použít tyto novější aktualizace těchto nástrojích.


#### <a name="automated-export"></a>Automatické exportu


Automatické Export i nadále k dispozici jako náhled.  Žádné klientské nástroje aktualizace jsou potřeba při použití automatické exportovat.  Pokud se rozhodnete prohlédněte výsledný soubor a importovat místní server, je třeba aktualizace nástrojů výše uvedené úspěšně naimportovat.


### <a name="restore-to-v12-of-a-deleted-v11-database"></a>Obnovte si V12 odstraněné V11 databáze

Následující postup vysvětluje, že odstraněné databáze V11 Azure SQL obnovit je může do databáze SQL Azure V12 serveru.

1. Předpokládejme, že máte databázi SQL Azure V11 serveru. <br/> Na serveru máte otevřenu databázi na zastaralá vrstvy služeb Web a obchodní.
2. Odstranění databáze.
3. Na V12 upgradovat na server.
4. Další obnovení databáze na serveru. <br/> Databáze se tím upgradovaný na verzi V12 na úrovni S0 vrstvy standardní služeb.
5. Databázi můžete přejít všechny podporované službě osy, nejsou-li S0 své preference.


### <a name="powershell-cmdlets"></a>Rutiny prostředí PowerShell


Rutiny prostředí PowerShell jsou k dispozici spustit, zastavit nebo sledovat upgrade V12 databáze SQL Azure, z V11 nebo jinou verzi pre V12.

- [Upgrade na V12 databáze SQL pomocí prostředí PowerShell](sql-database-upgrade-server-powershell.md)

Si přečtěte následující dokumentaci referenční informace o těchto rutin prostředí najdete v článku:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Zahájení AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Zastavit AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Rutinu zastavit znamená zrušit, není pozastavit. Nejde žádným způsobem pokračujte v upgradu, než počínaje znovu. Rutinu zastavit vyčistí a aktualizací všechny příslušné zdroje.


## <a name="failure-resolution"></a>Chyba při řešení


Když upgrade nepovede liché z nějakého důvodu, zůstane V11 databáze aktivní a k dispozici jako normálně.


## <a name="related-links"></a>Související odkazy


- [Funkce](https://azure.microsoft.com/services/preview/) Microsoft Azure


<!--Anchors-->
[Subheading 1]: #subheading-1
