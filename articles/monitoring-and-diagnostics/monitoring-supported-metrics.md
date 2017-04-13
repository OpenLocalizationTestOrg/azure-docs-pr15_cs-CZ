<properties
    pageTitle="Azure metriky Monitor – podporované metriky za typ zdroje | Microsoft Azure"
    description="Seznam metriky k dispozici pro každý typ zdroje s Azure Monitor."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="supported-metrics-with-azure-monitor"></a>Podporované metriky s Azure monitoru

Azure Monitor nabízí několik způsobů chcete provést interakci s metriky, včetně grafů v portálu, přístupu k nim prostřednictvím rozhraní REST API nebo dotazování je pomocí prostředí PowerShell nebo rozhraní příkazového řádku. Pod kompletní seznam všech metriky momentálně neexistuje s metrických profilace Azure Monitor.

> [AZURE.NOTE] Jiné metriky může být k dispozici v portálu nebo pomocí starší verze rozhraní API. Tento seznam obsahuje jenom veřejné náhled metriky dostupné s použitím veřejného náhled konsolidované Azure Monitor metrických kanálem k odesílání zpráv.

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|CoreCount|Počet základních|Počet|Součet|Celkový počet jádra dávku účtu|
|TotalNodeCount|Počet uzel|Počet|Součet|Celkový počet uzlů v okně dávku účet|
|CreatingNodeCount|Vytvoření počtu uzlů|Počet|Součet|Počet uzlů vzniku|
|StartingNodeCount|Počáteční hodnoty uzel|Počet|Součet|Počet uzlů spuštění|
|WaitingForStartTaskNodeCount|Čekání pro zahájení úkolu uzel počet|Počet|Součet|Počet uzlů čeká se na zahájení úkolů k dokončení|
|StartTaskFailedNodeCount|Zahájení úkolu se nepodařilo počet uzel|Počet|Součet|Počet uzlů místo, kam má spustit úkol se nezdařila.|
|IdleNodeCount|Nečinné počet uzel|Počet|Součet|Počet nedělá uzlů|
|OfflineNodeCount|Offline počet uzel|Počet|Součet|Počet offline uzlů|
|RebootingNodeCount|Restartování počet uzel|Počet|Součet|Počet restartování uzlů|
|ReimagingNodeCount|Počet reimaging uzel|Počet|Součet|Počet reimaging uzlů|
|RunningNodeCount|Count průběžný uzel|Počet|Součet|Počet spuštěných uzlů|
|LeavingPoolNodeCount|Nechte fondu uzel počet|Počet|Součet|Počet uzlů byste museli odcházet z fondu|
|UnusableNodeCount|Nelze použít počet uzel|Počet|Součet|Počet nelze použít uzlů|
|TaskStartEvent|Události zahájení úkolu|Počet|Součet|Celkový počet úkoly, které jste zahájili|
|TaskCompleteEvent|Události dokončení úkolu|Počet|Součet|Celkový počet úkoly, které jste dokončili|
|TaskFailEvent|Úkol selhání události|Počet|Součet|Celkový počet úkoly, které jste dokončili ve stavu selhání|
|PoolCreateEvent|Fond vytvářet události|Počet|Součet|Celkový počet skupin, které byly vytvořeny|
|PoolResizeStartEvent|Fond změny velikosti Start události|Počet|Součet|Celkový počet fondu změní, která jste zahájili|
|PoolResizeCompleteEvent|Dokončení událostech fondu změny velikosti|Počet|Součet|Celkový počet fondu změní, které jste dokončili|
|PoolDeleteStartEvent|Fond odstranit Start události|Počet|Součet|Celkový počet fondu odstraníte, která jste zahájili|
|PoolDeleteCompleteEvent|Fond odstranit dokončení události|Počet|Součet|Celkový počet fondu odstraníte, které jste dokončili|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|connectedclients|Připojení klientů|Počet|Maximální||
|totalcommandsprocessed|Celkové operace|Počet|Součet||
|mezipaměti|Přístupy do mezipaměti|Počet|Součet||
|cachemisses|Neúspěšné přístupy do mezipaměti|Počet|Součet||
|getcommands|Získá|Počet|Součet||
|setcommands|Sady|Počet|Součet||
|evictedkeys|Odstraněný klíče|Počet|Součet||
|totalkeys|Celkový počet klíčů|Počet|Maximální||
|expiredkeys|Vypršela platnost klíče|Počet|Součet||
|usedmemory|Použité paměti|Bajtů|Maximální||
|usedmemoryRss|Použité paměti RSS|Bajtů|Maximální||
|serverLoad|Zatížení serveru|V procentech|Maximální||
|cacheWrite|Zápis mezipaměti|BytesPerSecond|Maximální||
|cacheRead|Čtení mezipaměti|BytesPerSecond|Maximální||
|percentProcessorTime|PROCESOR|V procentech|Maximální||
|connectedclients0|Připojení klientů (Shard 0)|Počet|Maximální||
|totalcommandsprocessed0|Celkové operace (Shard 0)|Počet|Součet||
|cachehits0|Přístupy do mezipaměti (Shard 0)|Počet|Součet||
|cachemisses0|Neúspěšné přístupy do mezipaměti (Shard 0)|Počet|Součet||
|getcommands0|Získá (Shard 0)|Počet|Součet||
|setcommands0|Sady (Shard 0)|Počet|Součet||
|evictedkeys0|Odstraněný klávesy (Shard 0)|Počet|Součet||
|totalkeys0|Celkový počet klíčů (uzly 0)|Počet|Maximální||
|expiredkeys0|Vypršela platnost klávesy (Shard 0)|Počet|Součet||
|usedmemory0|Použité paměti (Shard 0)|Bajtů|Maximální||
|usedmemoryRss0|Použité paměti RSS (Shard 0)|Bajtů|Maximální||
|serverLoad0|Zatížení serveru (Shard 0)|V procentech|Maximální||
|cacheWrite0|Zápis mezipaměti (Shard 0)|BytesPerSecond|Maximální||
|cacheRead0|Mezipaměti pro čtení (Shard 0)|BytesPerSecond|Maximální||
|percentProcessorTime0|Procesor (Shard 0)|V procentech|Maximální||
|connectedclients1|Připojení klientů (Shard 1)|Počet|Maximální||
|totalcommandsprocessed1|Celkové operace (Shard 1)|Počet|Součet||
|cachehits1|Přístupy do mezipaměti (Shard 1)|Počet|Součet||
|cachemisses1|Neúspěšné přístupy do mezipaměti (Shard 1)|Počet|Součet||
|getcommands1|Získá (Shard 1)|Počet|Součet||
|setcommands1|Sady (Shard 1)|Počet|Součet||
|evictedkeys1|Odstraněný klávesy (Shard 1)|Počet|Součet||
|totalkeys1|Celkový počet klíčů (uzly 1)|Počet|Maximální||
|expiredkeys1|Vypršela platnost klávesy (Shard 1)|Počet|Součet||
|usedmemory1|Použité paměti (Shard 1)|Bajtů|Maximální||
|usedmemoryRss1|Použité paměti RSS (Shard 1)|Bajtů|Maximální||
|serverLoad1|Zatížení serveru (Shard 1)|V procentech|Maximální||
|cacheWrite1|Zápis mezipaměti (Shard 1)|BytesPerSecond|Maximální||
|cacheRead1|Mezipaměti pro čtení (Shard 1)|BytesPerSecond|Maximální||
|percentProcessorTime1|Procesor (Shard 1)|V procentech|Maximální||
|connectedclients2|Připojení klientů (Shard 2)|Počet|Maximální||
|totalcommandsprocessed2|Celkové operace (Shard 2)|Počet|Součet||
|cachehits2|Přístupy do mezipaměti (Shard 2)|Počet|Součet||
|cachemisses2|Neúspěšné přístupy do mezipaměti (Shard 2)|Počet|Součet||
|getcommands2|Získá (Shard 2)|Počet|Součet||
|setcommands2|Sady (Shard 2)|Počet|Součet||
|evictedkeys2|Odstraněný klávesy (Shard 2)|Počet|Součet||
|totalkeys2|Celkový počet klíčů (uzly 2)|Počet|Maximální||
|expiredkeys2|Vypršela platnost klávesy (Shard 2)|Počet|Součet||
|usedmemory2|Použité paměti (Shard 2)|Bajtů|Maximální||
|usedmemoryRss2|Použité paměti RSS (Shard 2)|Bajtů|Maximální||
|serverLoad2|Zatížení serveru (Shard 2)|V procentech|Maximální||
|cacheWrite2|Zápis mezipaměti (Shard 2)|BytesPerSecond|Maximální||
|cacheRead2|Mezipaměti pro čtení (Shard 2)|BytesPerSecond|Maximální||
|percentProcessorTime2|Procesor (Shard 2)|V procentech|Maximální||
|connectedclients3|Připojení klientů (Shard 3)|Počet|Maximální||
|totalcommandsprocessed3|Celkové operace (Shard 3)|Počet|Součet||
|cachehits3|Přístupy do mezipaměti (Shard 3)|Počet|Součet||
|cachemisses3|Neúspěšné přístupy do mezipaměti (Shard 3)|Počet|Součet||
|getcommands3|Získá (Shard 3)|Počet|Součet||
|setcommands3|Sady (Shard 3)|Počet|Součet||
|evictedkeys3|Odstraněný klávesy (Shard 3)|Počet|Součet||
|totalkeys3|Celkový počet klíčů (uzly 3)|Počet|Maximální||
|expiredkeys3|Vypršela platnost klávesy (Shard 3)|Počet|Součet||
|usedmemory3|Použité pamětí (Shard 3)|Bajtů|Maximální||
|usedmemoryRss3|Použité paměti RSS (Shard 3)|Bajtů|Maximální||
|serverLoad3|Zatížení serveru (Shard 3)|V procentech|Maximální||
|cacheWrite3|Zápis mezipaměti (Shard 3)|BytesPerSecond|Maximální||
|cacheRead3|Mezipaměti pro čtení (Shard 3)|BytesPerSecond|Maximální||
|percentProcessorTime3|Procesor (Shard 3)|V procentech|Maximální||
|connectedclients4|Připojení klientů (Shard 4)|Počet|Maximální||
|totalcommandsprocessed4|Celkové operace (Shard 4)|Počet|Součet||
|cachehits4|Přístupy do mezipaměti (Shard 4)|Počet|Součet||
|cachemisses4|Neúspěšné přístupy do mezipaměti (Shard 4)|Počet|Součet||
|getcommands4|Získá (Shard 4)|Počet|Součet||
|setcommands4|Sady (Shard 4)|Počet|Součet||
|evictedkeys4|Odstraněný klávesy (Shard 4)|Počet|Součet||
|totalkeys4|Celkový počet klíčů (uzly 4)|Počet|Maximální||
|expiredkeys4|Vypršela platnost klávesy (Shard 4)|Počet|Součet||
|usedmemory4|Použité paměti (Shard 4)|Bajtů|Maximální||
|usedmemoryRss4|Použité paměti RSS (Shard 4)|Bajtů|Maximální||
|serverLoad4|Zatížení serveru (Shard 4)|V procentech|Maximální||
|cacheWrite4|Zápis mezipaměti (Shard 4)|BytesPerSecond|Maximální||
|cacheRead4|Mezipaměti pro čtení (Shard 4)|BytesPerSecond|Maximální||
|percentProcessorTime4|Procesor (Shard 4)|V procentech|Maximální||
|connectedclients5|Připojení klientů (Shard 5)|Počet|Maximální||
|totalcommandsprocessed5|Celkové operace (Shard 5)|Počet|Součet||
|cachehits5|Přístupy do mezipaměti (Shard 5)|Počet|Součet||
|cachemisses5|Neúspěšné přístupy do mezipaměti (Shard 5)|Počet|Součet||
|getcommands5|Získá (Shard 5)|Počet|Součet||
|setcommands5|Sady (Shard 5)|Počet|Součet||
|evictedkeys5|Odstraněný klávesy (Shard 5)|Počet|Součet||
|totalkeys5|Celkový počet klíčů (uzly 5)|Počet|Maximální||
|expiredkeys5|Vypršela platnost klávesy (Shard 5)|Počet|Součet||
|usedmemory5|Použité paměti (Shard 5)|Bajtů|Maximální||
|usedmemoryRss5|Použité pamětí RSS (Shard 5)|Bajtů|Maximální||
|serverLoad5|Zatížení serveru (Shard 5)|V procentech|Maximální||
|cacheWrite5|Zápis mezipaměti (Shard 5)|BytesPerSecond|Maximální||
|cacheRead5|Mezipaměti pro čtení (Shard 5)|BytesPerSecond|Maximální||
|percentProcessorTime5|Procesor (Shard 5)|V procentech|Maximální||
|connectedclients6|Připojení klientů (Shard 6)|Počet|Maximální||
|totalcommandsprocessed6|Celkové operace (Shard 6)|Počet|Součet||
|cachehits6|Přístupy do mezipaměti (Shard 6)|Počet|Součet||
|cachemisses6|Neúspěšné přístupy do mezipaměti (Shard 6)|Počet|Součet||
|getcommands6|Získá (Shard 6)|Počet|Součet||
|setcommands6|Sady (Shard 6)|Počet|Součet||
|evictedkeys6|Odstraněný klávesy (Shard 6)|Počet|Součet||
|totalkeys6|Celkový počet klíčů (uzly 6)|Počet|Maximální||
|expiredkeys6|Vypršela platnost klávesy (Shard 6)|Počet|Součet||
|usedmemory6|Použité paměti (Shard 6)|Bajtů|Maximální||
|usedmemoryRss6|Použité pamětí RSS (Shard 6)|Bajtů|Maximální||
|serverLoad6|Zatížení serveru (Shard 6)|V procentech|Maximální||
|cacheWrite6|Zápis mezipaměti (Shard 6)|BytesPerSecond|Maximální||
|cacheRead6|Mezipaměti pro čtení (Shard 6)|BytesPerSecond|Maximální||
|percentProcessorTime6|Procesor (Shard 6)|V procentech|Maximální||
|connectedclients7|Připojení klientů (Shard 7)|Počet|Maximální||
|totalcommandsprocessed7|Celkové operace (Shard 7)|Počet|Součet||
|cachehits7|Přístupy do mezipaměti (Shard 7)|Počet|Součet||
|cachemisses7|Neúspěšné přístupy do mezipaměti (Shard 7)|Počet|Součet||
|getcommands7|Získá (Shard 7)|Počet|Součet||
|setcommands7|Sady (Shard 7)|Počet|Součet||
|evictedkeys7|Odstraněný klávesy (Shard 7)|Počet|Součet||
|totalkeys7|Celkový počet klíčů (uzly 7)|Počet|Maximální||
|expiredkeys7|Vypršela platnost klávesy (Shard 7)|Počet|Součet||
|usedmemory7|Použité paměti (Shard 7)|Bajtů|Maximální||
|usedmemoryRss7|Použité pamětí RSS (Shard 7)|Bajtů|Maximální||
|serverLoad7|Zatížení serveru (Shard 7)|V procentech|Maximální||
|cacheWrite7|Zápis mezipaměti (Shard 7)|BytesPerSecond|Maximální||
|cacheRead7|Mezipaměti pro čtení (Shard 7)|BytesPerSecond|Maximální||
|percentProcessorTime7|Procesor (Shard 7)|V procentech|Maximální||
|connectedclients8|Připojení klientů (Shard 8)|Počet|Maximální||
|totalcommandsprocessed8|Celkové operace (Shard 8)|Počet|Součet||
|cachehits8|Přístupy do mezipaměti (Shard 8)|Počet|Součet||
|cachemisses8|Neúspěšné přístupy do mezipaměti (Shard 8)|Počet|Součet||
|getcommands8|Získá (Shard 8)|Počet|Součet||
|setcommands8|Sady (Shard 8)|Počet|Součet||
|evictedkeys8|Odstraněný klávesy (Shard 8)|Počet|Součet||
|totalkeys8|Celkový počet klíčů (uzly 8)|Počet|Maximální||
|expiredkeys8|Vypršela platnost klávesy (Shard 8)|Počet|Součet||
|usedmemory8|Použité pamětí (Shard 8)|Bajtů|Maximální||
|usedmemoryRss8|Použité paměti RSS (Shard 8)|Bajtů|Maximální||
|serverLoad8|Zatížení serveru (Shard 8)|V procentech|Maximální||
|cacheWrite8|Zápis mezipaměti (Shard 8)|BytesPerSecond|Maximální||
|cacheRead8|Mezipaměti pro čtení (Shard 8)|BytesPerSecond|Maximální||
|percentProcessorTime8|Procesor (Shard 8)|V procentech|Maximální||
|connectedclients9|Připojení klientů (Shard 9)|Počet|Maximální||
|totalcommandsprocessed9|Celkové operace (Shard 9)|Počet|Součet||
|cachehits9|Přístupy do mezipaměti (Shard 9)|Počet|Součet||
|cachemisses9|Neúspěšné přístupy do mezipaměti (Shard 9)|Počet|Součet||
|getcommands9|Získá (Shard 9)|Počet|Součet||
|setcommands9|Sady (Shard 9)|Počet|Součet||
|evictedkeys9|Odstraněný klávesy (Shard 9)|Počet|Součet||
|totalkeys9|Celkový počet klíčů (uzly 9)|Počet|Maximální||
|expiredkeys9|Vypršela platnost klávesy (Shard 9)|Počet|Součet||
|usedmemory9|Použité pamětí (Shard 9)|Bajtů|Maximální||
|usedmemoryRss9|Použité paměti RSS (Shard 9)|Bajtů|Maximální||
|serverLoad9|Zatížení serveru (Shard 9)|V procentech|Maximální||
|cacheWrite9|Zápis mezipaměti (Shard 9)|BytesPerSecond|Maximální||
|cacheRead9|Mezipaměti pro čtení (Shard 9)|BytesPerSecond|Maximální||
|percentProcessorTime9|Procesor (Shard 9)|V procentech|Maximální||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|NumberOfCalls|Celkový počet rozhraní API volání|Počet|Součet|Celkový počet rozhraní API volání.|
|NumberOfFailedCalls|Celkový počet neúspěšných rozhraní API volání|Počet|Součet|Celkový počet neúspěšných rozhraní API volání.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|Procento využití procesoru|Procento využití procesoru|V procentech|Průměr|Procento přidělené výpočetních jednotky, které jsou v současnosti nepoužívá virtuální počítače|
|V síti|V síti|Bajtů|Součet|Počet bajtů dostali na všechna síťová rozhraní virtuální počítače (příchozích)|
|Sítě se|Sítě se|Bajtů|Součet|Počet bajtů na všechna síťová rozhraní tak, že virtuální počítače (odchozí přenosy dat)|
|Čtení bajtů disku|Čtení bajtů disku|Bajtů|Součet|Celkový počet bajtů číst z disku během sledování období|
|Disk zapsat bajtů|Disk zapsat bajtů|Bajtů|Součet|Celkový počet bajtů zapisuje na disk během sledování období|
|Čtení operace a s disku|Čtení operace a s disku|CountPerSecond|Průměr|Disku čtení procesorů|
|Zápis disku operace/Sec|Zápis disku operace/Sec|CountPerSecond|Průměr|Zápis disku procesorů|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|Procento využití procesoru|Procento využití procesoru|V procentech|Průměr|Procento přidělené výpočetních jednotky, které jsou v současnosti nepoužívá virtuální počítače|
|V síti|V síti|Bajtů|Součet|Počet bajtů dostali na všechna síťová rozhraní virtuální počítače (příchozích)|
|Sítě se|Sítě se|Bajtů|Součet|Počet bajtů na všechna síťová rozhraní tak, že virtuální počítače (odchozí přenosy dat)|
|Čtení bajtů disku|Čtení bajtů disku|Bajtů|Součet|Celkový počet bajtů číst z disku během sledování období|
|Disk zapsat bajtů|Disk zapsat bajtů|Bajtů|Součet|Celkový počet bajtů zapisuje na disk během sledování období|
|Čtení operace a s disku|Čtení operace a s disku|CountPerSecond|Průměr|Disku čtení procesorů|
|Zápis disku operace/Sec|Zápis disku operace/Sec|CountPerSecond|Průměr|Zápis disku procesorů|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|Procento využití procesoru|Procento využití procesoru|V procentech|Průměr|Procento přidělené výpočetním jednotky, které jsou v současnosti nepoužívá virtuální počítače|
|V síti|V síti|Bajtů|Součet|Počet bajtů dostali na všechna síťová rozhraní virtuální počítače (příchozích)|
|Sítě se|Sítě se|Bajtů|Součet|Počet bajtů na všechna síťová rozhraní tak, že virtuální počítače (odchozí přenosy dat)|
|Čtení bajtů disku|Čtení bajtů disku|Bajtů|Součet|Celkový počet bajtů číst z disku během sledování období|
|Disk zapsat bajtů|Disk zapsat bajtů|Bajtů|Součet|Celkový počet bajtů zapisuje na disk během sledování období|
|Čtení operace a s disku|Čtení operace a s disku|CountPerSecond|Průměr|Disku čtení procesorů|
|Zápis disku operace/Sec|Zápis disku operace/Sec|CountPerSecond|Průměr|Zápis disku procesorů|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetrie zprávu odeslat neúspěšných pokusech o|Počet|Součet|Pokus o počtu zařízení do cloudu telemetrie zprávy zasílané do vaší IoT centrální|
|d2c.telemetry.ingress.Success|Telemetrie zprávy|Počet|Součet|Počet zařízení ke cloudové telemetrie zprávám odeslaným úspěšně k rozbočovači IoT|
|c2d.Commands.egress.COMPLETE.Success|Příkazy dokončení|Počet|Součet|Počet Cloud k příkazům zařízení byla úspěšně dokončena zařízením|
|c2d.Commands.egress.Abandon.Success|Příkazy Opuštěné|Počet|Součet|Počet Cloud k příkazům zařízení opuštěné zařízením|
|c2d.Commands.egress.Reject.Success|Příkazy odmítnutí|Počet|Součet|Počet Cloud k příkazům zařízení odmítnuto zařízením|
|devices.totalDevices|Celková zařízení|Počet|Součet|Počet zařízení registrovaných rozbočovače IoT|
|devices.connectedDevices.allProtocol|Připojení zařízení|Počet|Součet|Počet zařízení připojené k rozbočovači IoT|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|INREQS|Příchozí žádosti|Počet|Součet|Událost centrální výkon na příchozí zprávu obory názvů|
|SUCCREQ|Požadavky na úspěšné|Počet|Součet|Celkový počet úspěšných požadavků pro obor|
|FAILREQ|Žádosti o nezdařeném uložení|Počet|Součet|Celkový počet neúspěšných požadavků pro obor|
|SVRBSY|Zaneprázdněn chyby serveru|Počet|Součet|Zaneprázdněn chyby celkové serveru pro obor názvů|
|INTERR|Chyby interní Server|Počet|Součet|Celková interní server chyby obor názvů|
|MISCERR|Jiné chyby|Počet|Součet|Celkový počet neúspěšných požadavků pro obor|
|INMSGS|Příchozí zprávy|Počet|Součet|Celková příchozích zpráv pro obor názvů|
|OUTMSGS|Odchozích zpráv|Počet|Součet|Součet odchozí zprávy pro obor názvů|
|EHINMBS|Příchozí bajty za sekundu|BytesPerSecond|Součet|Událost centrální výkon na příchozí zprávu obory názvů|
|EHOUTMBS|Odchozí bajty za sekundu|BytesPerSecond|Součet|Součet odchozí zprávy pro obor názvů|
|EHABL|Archivace zpráv rezervu|Počet|Součet|Událost centrální archiv zpráv v rezervu obory názvů|
|EHAMSGS|Archivace zpráv|Počet|Součet|Událost centrální archivované zprávy v oboru názvů|
|EHAMBS|Archivace zpráv výkon|BytesPerSecond|Součet|Událost centrální výkon archivované zprávy v oboru|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|RunsStarted|Začínáme se spustí|Počet|Součet|Počet pracovní postup spuštěn Začínáme.|
|RunsCompleted|Spustí dokončení|Počet|Součet|Počet pracovní postup spuštěn dokončený.|
|RunsSucceeded|Spustí úspěšně|Počet|Součet|Počet pracovní postup spuštěn úspěchu.|
|RunsFailed|Spustí se nezdařila.|Počet|Součet|Počet pracovní postup spuštěn selhalo.|
|RunsCancelled|Spustí zrušeno|Počet|Součet|Počet pracovní postup spuštěn zrušené.|
|RunLatency|Spuštění latence|Sekund|Průměr|Latence dokončené pracovní postup spuštěn.|
|RunSuccessLatency|Spuštění latence úspěch|Sekund|Průměr|Latence úspěchu pracovní postup spuštěn.|
|RunThrottledEvents|Spouštět omezené události|Počet|Součet|Počet akce pracovního postupu nebo aktivační událost omezena události.|
|RunFailurePercentage|Spuštění selhání procentech|V procentech|Součet|Procento pracovní postup spuštěn nezdařeném uložení.|
|ActionsStarted|Spuštění akce |Počet|Součet|Počet spuštění akce pracovního postupu.|
|ActionsCompleted|Akce dokončení |Počet|Součet|Počet akce pracovního postupu dokončit.|
|ActionsSucceeded|Akce úspěšně |Počet|Součet|Počet akce pracovního postupu byla úspěšně dokončena.|
|ActionsFailed|Akce se nezdařila.|Počet|Součet|Počet akce pracovního postupu se nezdařilo.|
|ActionsSkipped|Akce přeskočilo |Počet|Součet|Číslo akcí pracovních postupů vynechány.|
|ActionLatency|Latence akce |Sekund|Průměr|Latence akcí dokončené pracovních postupů.|
|ActionSuccessLatency|Latence úspěch akce |Sekund|Průměr|Latence akcí úspěchu pracovních postupů.|
|ActionThrottledEvents|Akce omezena události|Počet|Součet|Počet akce pracovního postupu omezena události.|
|TriggersStarted|Aktivace Začínáme |Počet|Součet|Počet aktivační události pracovní postup spuštěn.|
|TriggersCompleted|Aktivace dokončení |Počet|Součet|Počet aktivační události pracovní postup dokončen.|
|TriggersSucceeded|Aktivace bylo úspěšné |Počet|Součet|Počet aktivační události pracovního postupu byla úspěšně dokončena.|
|TriggersFailed|Aktivace se nezdařila. |Počet|Součet|Počet aktivační události pracovního postupu se nezdařilo.|
|TriggersSkipped|Aktivace přeskočilo|Počet|Součet|Počet aktivační události pracovního postupu vynechány.|
|TriggersFired|Aktivace aktivována |Počet|Součet|Počet aktivační události pracovního postupu aktivoval.|
|TriggerLatency|Latence aktivační událost |Sekund|Průměr|Latence aktivačních událostí dokončení pracovního postupu.|
|TriggerFireLatency|Latence Fire aktivační událost |Sekund|Průměr|Latence aplikace aktivována aktivační události pracovního postupu.|
|TriggerSuccessLatency|Latence úspěch aktivační událost |Sekund|Průměr|Latence aktivačních událostí úspěchu pracovního postupu.|
|TriggerThrottledEvents|Omezené události|Počet|Součet|Počet aktivační události pracovního postupu omezena události.|
|BillableActionExecutions|Spuštění fakturaci akce|Počet|Součet|Počet spuštění akce pracovního postupu získání vám účtovat poplatky.|
|BillableTriggerExecutions|Spuštění fakturaci aktivační událost|Počet|Součet|Počet spuštění pracovního postupu aktivační událost začíná vám účtovat poplatky.|
|TotalBillableExecutions|Celková fakturaci spuštění|Počet|Součet|Počet spuštění pracovního postupu získání vám účtovat poplatky.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|Výkon|Výkon|BytesPerSecond|Průměr||

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|SearchLatency|Latence vyhledávání|Sekund|Průměr|Latence průměr vyhledávání pro vyhledávací službu|
|SearchQueriesPerSecond|Vyhledávací dotazy za sekundu|CountPerSecond|Průměr|Vyhledávací dotazy sekundu pro službu hledání|
|ThrottledSearchQueriesPercentage|Omezené vyhledávací dotazy procentech|V procentech|Průměr|Procento vyhledávací dotazy, které byly omezena pro službu hledání|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|CPUXNS|Využití procesoru za obor názvů|V procentech|Maximální|Služba bus premium názvů procesoru použití míru|
|WSXNS|Použití velikosti paměti za obor názvů|V procentech|Maximální|Služba bus premium názvů paměti použití míru|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|cpu_percent|Procento využití procesoru|V procentech|Průměr|Procento využití procesoru|
|physical_data_read_percent|Procento vstupu a výstupu dat|V procentech|Průměr|Procento vstupu a výstupu dat|
|log_write_percent|Procento vstupu a výstupu protokolu|V procentech|Průměr|Procento vstupu a výstupu protokolu|
|dtu_consumption_percent|Procento DTU|V procentech|Průměr|Procento DTU|
|úložiště|Celkovou velikost databáze|Bajtů|Maximální|Celkovou velikost databáze|
|connection_successful|Úspěšné připojení|Počet|Součet|Úspěšné připojení|
|connection_failed|Selhalo připojení|Počet|Součet|Selhalo připojení|
|blocked_by_firewall|Blokována bránou Firewall|Počet|Součet|Blokována bránou Firewall|
|zablokování|Zablokování|Počet|Součet|Zablokování|
|storage_percent|Velikost databáze v procentech|V procentech|Maximální|Velikost databáze v procentech|
|xtp_storage_percent|Percent(Preview) úložiště OLTP v paměti|V procentech|Průměr|Percent(Preview) úložiště OLTP v paměti|
|workers_percent|Procento zaměstnanců|V procentech|Průměr|Pracovníci v procentech|
|sessions_percent|Relace v procentech|V procentech|Průměr|Relace v procentech|
|dtu_limit|DTU limit|Počet|Průměr|DTU limit|
|dtu_used|DTU používá|Počet|Průměr|DTU používá|
|service_level_objective|Služba úrovně cíle databáze|Počet|Součet|Služba úrovně cíle databáze|
|dwu_limit|dwu limit|Počet|Maximální|dwu limit|
|dwu_consumption_percent|Procento DWU|V procentech|Průměr|Procento DWU|
|dwu_used|DWU používá|Počet|Průměr|DWU používá|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|cpu_percent|Procento využití procesoru|V procentech|Průměr|Procento využití procesoru|
|physical_data_read_percent|Procento vstupu a výstupu dat|V procentech|Průměr|Procento vstupu a výstupu dat|
|log_write_percent|Procento vstupu a výstupu protokolu|V procentech|Průměr|Procento vstupu a výstupu protokolu|
|dtu_consumption_percent|Procento DTU|V procentech|Průměr|Procento DTU|
|storage_percent|Procento úložiště|V procentech|Průměr|Procento úložiště|
|workers_percent|Pracovníci v procentech|V procentech|Průměr|Pracovníci v procentech|
|sessions_percent|Relace v procentech|V procentech|Průměr|Relace v procentech|
|eDTU_limit|eDTU limit|Počet|Průměr|eDTU limit|
|storage_limit|Omezení úložiště|Bajtů|Průměr|Omezení úložiště|
|eDTU_used|eDTU používá|Počet|Průměr|eDTU používá|
|storage_used|Úložiště|Bajtů|Průměr|Úložiště|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|ResourceUtilization|Využití % SH|V procentech|Maximální|Využití % SH|
|InputEvents|Zadávání události|Počet|Součet|Zadávání události|
|InputEventBytes|Při zadávání bajtů události|Bajtů|Součet|Při zadávání bajtů události|
|LateInputEvents|Nejpozději možné vstupní události|Počet|Součet|Nejpozději možné vstupní události|
|OutputEvents|Výstup události|Počet|Součet|Výstup události|
|ConversionErrors|Chybám v převodu|Počet|Součet|Chybám v převodu|
|Chyby|Chyby za běhu|Počet|Součet|Chyby za běhu|
|DroppedOrAdjustedEvents|Pořadí událostí|Počet|Součet|Pořadí událostí|
|AMLCalloutRequests|Požadavky na (funkce)|Počet|Součet|Požadavky na (funkce)|
|AMLCalloutFailedRequests|Žádosti o nezdařeném uložení (funkce)|Počet|Součet|Žádosti o nezdařeném uložení (funkce)|
|AMLCalloutInputEvents|Funkce události|Počet|Součet|Funkce události|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|CpuPercentage|Procento využití procesoru|V procentech|Průměr|Procento využití procesoru|
|MemoryPercentage|Procento paměti|V procentech|Průměr|Procento paměti|
|DiskQueueLength|Délka fronty disku|Počet|Součet|Délka fronty disku|
|HttpQueueLength|Délka fronty http|Počet|Součet|Délka fronty http|
|BytesReceived|Data v|Bajtů|Součet|Data v|
|BytesSent|Data se|Bajtů|Součet|Data se|

## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|CpuTime|Procesor času|Sekund|Součet|Procesor času|
|Požadavky|Požadavky|Počet|Součet|Požadavky|
|BytesReceived|Data v|Bajtů|Součet|Data v|
|BytesSent|Data se|Bajtů|Součet|Data se|
|Http2xx|Nastavit informace HTTP 2xx|Počet|Součet|Nastavit informace HTTP 2xx|
|Http3xx|Nastavit informace HTTP 3xx|Počet|Součet|Nastavit informace HTTP 3xx|
|Http401|Nastavit informace HTTP 401|Počet|Součet|Nastavit informace HTTP 401|
|Http403|Nastavit informace HTTP 403|Počet|Součet|Nastavit informace HTTP 403|
|Http404|Nastavit informace HTTP 404|Počet|Součet|Nastavit informace HTTP 404|
|Http406|Nastavit informace HTTP 406|Počet|Součet|Nastavit informace HTTP 406|
|Http4xx|Nastavit informace HTTP 4xx|Počet|Součet|Nastavit informace HTTP 4xx|
|Http5xx|HTTP Server chyby|Počet|Součet|HTTP Server chyby|
|MemoryWorkingSet|Pracovní sada paměti|Bajtů|Součet|Pracovní sada paměti|
|AverageMemoryWorkingSet|Průměrná pracovní sada paměti|Bajtů|Průměr|Průměrná pracovní sada paměti|
|AverageResponseTime|Průměrná doba odezvy|Sekund|Průměr|Průměrná doba odezvy|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metriky|Metriky zobrazované jméno|Jednotky|Typ agregace|Popis|
|---|---|---|---|---|
|CpuTime|Procesor času|Sekund|Součet|Procesor času|
|Požadavky|Požadavky|Počet|Součet|Požadavky|
|BytesReceived|Data v|Bajtů|Součet|Data v|
|BytesSent|Data se|Bajtů|Součet|Data se|
|Http2xx|Nastavit informace HTTP 2xx|Počet|Součet|Nastavit informace HTTP 2xx|
|Http3xx|Nastavit informace HTTP 3xx|Počet|Součet|Nastavit informace HTTP 3xx|
|Http401|Nastavit informace HTTP 401|Počet|Součet|Nastavit informace HTTP 401|
|Http403|Nastavit informace HTTP 403|Počet|Součet|Nastavit informace HTTP 403|
|Http404|Nastavit informace HTTP 404|Počet|Součet|Nastavit informace HTTP 404|
|Http406|Nastavit informace HTTP 406|Počet|Součet|Nastavit informace HTTP 406|
|Http4xx|Nastavit informace HTTP 4xx|Počet|Součet|Nastavit informace HTTP 4xx|
|Http5xx|HTTP Server chyby|Počet|Součet|HTTP Server chyby|
|MemoryWorkingSet|Pracovní sada paměti|Bajtů|Součet|Pracovní sada paměti|
|AverageMemoryWorkingSet|Průměrná pracovní sada paměti|Bajtů|Průměr|Průměrná pracovní sada paměti|
|AverageResponseTime|Průměrná doba odezvy|Sekund|Průměr|Průměrná doba odezvy|

## <a name="next-steps"></a>Další kroky

- [Přečtěte si o metriky v nástroji Sledování Azure](./monitoring-overview.md#monitoring-sources)
- [Vytvořit upozornění metrice](./insights-receive-alert-notifications.md)
- [Export metriky úložiště, centrální událostí nebo protokolu analýzy](./monitoring-overview-of-diagnostic-logs.md)
