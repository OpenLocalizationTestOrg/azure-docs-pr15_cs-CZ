<properties
   pageTitle="Distribuované transakce přes cloudu databází"
   description="Základní informace o pružná databázové transakce s databáze Azure SQL"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jhubbard"
   editor="torsteng"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="sql-database"
   ms.date="05/27/2016"
   ms.author="torsteng"/>

# <a name="distributed-transactions-across-cloud-databases"></a>Distribuované transakce přes cloudu databází

Pružná databázové transakce databáze SQL Azure (databáze SQL) aby bylo možné spouštět transakce přecházející několik databází v SQL databázi. Pružná databázové transakce pro SQL databáze jsou dostupné pro .NET aplikací pomocí ADO .NET a integrace s známé programovací prostředí pomocí tříd [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) . Získání knihovnu, naleznete v článku [.NET Framework 4.6.1 (Web Installer)](https://www.microsoft.com/download/details.aspx?id=49981).

Místně tato situace zpravidla vyžaduje systém Microsoft Distributed transakce koordinátor MSDTC (). Protože MSDTC není k dispozici pro aplikaci platformy jako služby Azure, možnost koordinaci distribuované transakce teď byla přímo integrována do SQL databáze. Aplikace můžete připojit k jakékoli SQL databáze spuštění distribuované transakce a jednu z databází bude transparentně koordinaci distribuované transakce, jak je znázorněno na následujícím obrázku. 

  ![Distribuované transakce s databáze SQL Azure pomocí pružná databázové transakce ][1]

## <a name="common-scenarios"></a>Obvyklé scénáře

Pružná databázové transakce pro SQL databáze povolit aplikacím atomová měnit data uložená v několika různých SQL databáze. Náhled se zaměřuje na straně klienta vývoj prostředí v jazyce C# a .NET. Straně serveru zkušenosti s používáním T-SQL plánuje na pozdější čas.  
Pružná databázové transakce zaměřuje následujících situacích:

* Více databázové aplikace v Azure: V tomto scénáři je dat ve svislém směru rozdělený napříč několika databází v databázi SQL tak, aby se různých typů dat jsou umístěny na jiné databáze. Některé operace vyžadují změny dat, která bude k dispozici ve dvou nebo více databází. Aplikace používá pružná databázové transakce koordinaci změny mezi databází a zajistit nedělitelnost.

* Sharded databázové aplikace v Azure: V tomto scénáři osy dat pomocí [knihovny klienta pružná databáze](sql-database-elastic-database-client-library.md) nebo sharding sebe ve vodorovném směru oddílů data přes mnoho databází v databázi SQL. Jeden případ použití výrazném je třeba provádět atomová změny sharded aplikace více klienta při změny zahrnují klienti. Přemýšlejte například přenos z jednoho klienta do druhého obou umístěných na jiné databáze. Druhá případu je jemně odstupňovaná sharding tak, aby zahrnoval kapacita úrazovém velké klienta, která zase obvykle znamená, že některé atomová operace vyžaduje Roztáhnout napříč několika databáze použité pro stejném klientovi. Třetí případu je atomová aktualizace neodkazuje data, která jsou replikovat přes databází. Atomová, transakční, operace podél tyto řádky můžete nyní koordinovaný napříč několika databází používat náhled.
Pružná databázové transakce pomocí dvoufázový potvrzovací zajistit transakce nedělitelnost přes databází. Je přesně hodí transakce zahrnující menší než 100 databází v čase v rámci jedné transakce. Tyto limity nejsou nevynucují, ale jednu očekávat výkon a úspěšnosti pro pružná databázové transakce má negativní vliv při překročení tato omezení.


## <a name="installation-and-migration"></a>Instalace a migrace

Možnosti pro pružná databázové transakce v SQL databázi jsou k dispozici prostřednictvím aktualizace knihovny .NET System.Data.dll a System.Transactions.dll. DLL zajistěte, aby že dvoufázový potvrzovací slouží v případě potřeby zajistit nedělitelnost. Vývoj aplikací pomocí pružná databázové transakce zahájíte instalaci [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) nebo jeho novější verzí. Spuštěná v dřívější verzi .NET framework transakce nebude podporovat distribuované transakce a se zvýší výjimku.

Po instalaci můžete distribuované transakce rozhraní API v System.Transactions s připojením k SQL databáze. Pokud máte existující MSDTC aplikací pomocí těchto rozhraní API, stačí znovu vytvořit existující aplikace pro .NET 4.6 po instalaci 4.6.1 Framework. Pokud projektů obrázku .NET 4.6, budou automaticky používat aktualizované DLL v nové verzi Framework a distribuované transakce rozhraní API volání v kombinaci s připojení k SQL databáze se teď nezdaří.

Mějte na paměti, že pružná databázové transakce nevyžadují instalaci MSDTC. Místo toho pružná databázové transakce je spravováno přímo a v SQL databáze. To výrazně zjednodušuje cloudu scénáře od nasazení MSDTC není nutné zadávat distribuované transakce pomocí služby SQL databáze. Část 4 vysvětluje podrobněji nasazení pružná databázové transakce a požadované .NET framework společně s aplikací cloud k Azure.

## <a name="development-experience"></a>Vývojové prostředí

### <a name="multi-database-applications"></a>Více databázové aplikace

Následující ukázkový kód používá známé prostředí programování s .NET System.Transactions. Třídy TransactionScope zavádí okolního transakce v .NET. ("Okolního transakci" je jsou umístěná v aktuálním vlákna.) Všechna připojení otevřeli v TransactionScope zúčastnit transakce. Pokud jinou databází tohoto programu zúčastnit, transakce automaticky vyššími distribuované transakce. Výsledek transakce řídí nastavení oboru dokončete označíte potvrzení.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Sharded databázové aplikace
 
Pružná databázové transakce pro databáze SQL také podporují koordinaci distribuované transakce kde použijte metodu OpenConnectionForKey knihovnu klienta pružná databázi otevřete připojení pro zvětšený se data osy. Zvažte případy, kdy je potřeba zaručit konzistence změny napříč několika různých sharding klíčových hodnot. Připojení k shards hostingu hodnoty klíče různých sharding jsou brokered pomocí OpenConnectionForKey. V případě obecné připojení půjde různých shards tak, aby se zajištění transakční záruk distribuované transakce vyžaduje. Následující příklad kódu znázorňuje tento přístup. Předpokládá, že proměnnou s názvem shardmap slouží k představují mapě shard z knihovny klienta pružná databáze:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }
        
        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>Instalace .NET cloudové služby Azure

Azure obsahuje několik nabídky na hostitele .NET aplikace. Porovnání různých nabídek, je dostupná v [aplikaci služby Azure, cloudovými službami a porovnání virtuálních počítačích](../app-service-web/choose-web-site-cloud-service-vm.md). Host OS nabídky je menší než .NET 4.6.1 potřebných pro pružná transakce, potřebnosti upgradovat hosta OS 4.6.1. 

Aplikace služby Azure nepodporuje aktuálně upgrady pro hosty OS. Pro virtuálních počítačích Azure, jednoduše přihlásit OM a spusťte instalační program pro nejnovější .NET framework. Cloudové služby Azure potřebujete zahrnout instalace novější verzi .NET na spuštění úlohy nasazení. Principy a postupy jsou popsány v [Instalace .NET rolí služby cloudu](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Všimněte si, že instalační program pro .NET 4.6.1 může vyžadovat víc dočasné úložiště během procesu samozaváděcí na Azure cloud services než instalační program pro .NET 4.6. Zajistit úspěšná instalace potřebujete ke zvýšení dočasné úložiště služby Azure cloudu v souboru ServiceDefinition.csdef v části LocalResources a nastavení prostředí spuštění úkolu, jak je vidět v následujícím příkladu:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>
    
## <a name="transactions-across-multiple-servers"></a>Transakce mezi servery

Pružná databázové transakce jsou podporovány na různých logické serverech databáze SQL Azure. Když transakce přes hranice logické serveru, zúčastněných serverů nejprve je nutné zadat do relace vzájemné komunikaci. Jakmile vytvořila relace komunikace jakékoli databáze v některém z dva servery se může účastnit pružná transakce u databází ze na server. S transakce zahrnující více než dvě logických serverů komunikace relace musí být v místo pro všechny dvojice logických serverů.

Správa relací komunikaci mezi servery pro pružná databázové transakce pomocí následující rutiny prostředí PowerShell:

* **Nový AzureRmSqlServerCommunicationLink**: tahle rutina slouží k vytvoření nové relace komunikace mezi dvěma logické servery v databázi SQL Azure. Vztah je symetrickou, což znamená, že oba servery můžete zahájit transakce na serveru.
* **Get-AzureRmSqlServerCommunicationLink**: tahle rutina slouží k načtení existujících relací komunikace a jejich vlastnosti.
* **Odebrat AzureRmSqlServerCommunicationLink**: tahle rutina slouží k odebrání existující relací komunikace. 


## <a name="monitoring-transaction-status"></a>Sledování stavu transakce

Umožňuje dynamická správa zobrazení (DMVs) v databázi SQL monitor stav a průběh probíhající pružná databázové transakce. Všechny DMVs související s transakce se vztahují k distribuované transakce v SQL databázi. Můžete najít na příslušný seznam DMVs tady: [transakce související dynamická správa zobrazení a funkcí (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).
 
Tyto DMVs jsou užitečné zejména:

* **sys.dm\_Nahra\_aktivní\_transakce**: seznamy aktuálně aktivní transakce a jejich stav. Sloupec UOW (jednotky práce) můžete určit jiné podřízené transakce patřícími do stejné distribuované transakce. Všechny transakce v rámci stejné distribuované transakce mít stejnou hodnotu UOW. Najdete v [dokumentaci DMV](https://msdn.microsoft.com/library/ms174302.aspx) další podrobnosti.
* **sys.dm\_Nahra\_databáze\_transakce**: poskytující další informace o transakce, například umístění transakce v protokolu. Najdete v [dokumentaci DMV](https://msdn.microsoft.com/library/ms186957.aspx) další podrobnosti.
* **sys.dm\_Nahra\_zámků webů**: poskytuje informace o zámků webů, které se právě probíhající transakce. Najdete v [dokumentaci DMV](https://msdn.microsoft.com/library/ms190345.aspx) další podrobnosti.

## <a name="limitations"></a>Omezení 

K pružná databázové transakce v SQL databázi aktuálně platí následující omezení:

* Podporuje pouze transakce přes databází v SQL databázi. Další [X / otevřete XA](https://en.wikipedia.org/wiki/X/Open_XA) poskytovatelů zdroje a databází mimo SQL databáze nelze použít v pružná databázové transakce. To znamená, že nelze pružná databázové transakce roztáhnout na místní nasazení serveru SQL Server a databáze SQL Azure. Pro distribuované transakce místně i nadále používat MSDTC. 
* Jenom klient koordinovaný transakce z aplikace .NET jsou podporovány. Serverový podpora T-SQL například začít DISTRIBUTED transakce je plánované, ale zatím není k dispozici. 
* Podporuje pouze databáze na V12 databáze SQL Azure.
* Transakce přes služby WCF nejsou podporované. Máte třeba metoda služby WCF, která provede transakce. Uzavření volání v rozsahu transakce se nepovede jako [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="additional-resources"></a>Další zdroje informací

Dosud pomocí možností pružná databáze pro aplikace Azure? Podívejte se na našeho [Mapa dokumentace](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). Dotazy přejděte prosím kontaktujte nás na [Fórum komunity databáze SQL](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a poslat žádost o funkci, přidejte je do [fórum pro názory databáze SQL](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



