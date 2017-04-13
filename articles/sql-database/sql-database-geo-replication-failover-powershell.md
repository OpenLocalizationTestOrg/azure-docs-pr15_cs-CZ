<properties 
    pageTitle="Zahájení konverzace přepojení plánované nebo neplánované pro databáze SQL Azure pomocí prostředí PowerShell | Microsoft Azure" 
    description="Zahájení konverzace přepojení plánované nebo neplánované databáze SQL Azure pomocí prostředí PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Zahájení konverzace přepojení plánované nebo neplánované pro databáze SQL Azure pomocí prostředí PowerShell



> [AZURE.SELECTOR]
- [Azure portálu](sql-database-geo-replication-failover-portal.md)
- [Prostředí PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


V tomto článku se dozvíte, jak zahajte přepojení plánované nebo neplánované pro databáze SQL pomocí prostředí PowerShell. Abyste mohli nakonfigurovat Geo replikace, najdete v článku [Konfigurace Geo replikace databáze SQL Azure](sql-database-geo-replication-powershell.md).



## <a name="initiate-a-planned-failover"></a>Zahájení konverzace plánované selhání

Použijte rutinu **Set-AzureRmSqlDatabaseSecondary** s parametrem **– Převzetí** podporovat sekundární databáze osvobozením od nové primární databázi, převedení existujícího primární osvobozením od sekundární. Tato funkce slouží k plánované selhání, jako je třeba během havárie obnovení cvičení a vyžaduje, aby databázi primárního k dispozici.

Příkaz provádí následující pracovního postupu:

1. Replikace dočasně přepněte na režim. To způsobí všechny zbývající transakce na vyprázdnit sekundární.

2. Přepnutí role dvou databází ve partnerství Geo replikace.  

Tato řada zaručuje, že dvě databáze jsou synchronizovány před přepněte role a tedy dojde k žádné ztrátě dat. Je krátké období, po kterou obě databáze nedostupné (pořadí 0 až 25 sekund) během rolím se přechodu na jiný. Celou operaci lze provést méně než jednu minutu dokončete za normálních okolností. Další informace najdete v tématu [Nastavení AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx).




Tato rutina vrátí po dokončení procesu přepnutí primární sekundární databáze.

Následující příkaz kombinace kláves vymění role databáze s názvem "mydb" na server "srv2" v části Skupina zdroje "rg2" na primární. Původní primární, do kterého byla "db2" připojené k přepne sekundárním po synchronizaci je plně dvě databáze.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] V některých případech je možné, že operaci nelze dokončit, může se zobrazit krátkodobě neodpovídá. V tomto případě uživatele můžete volat příkaz převzetí platnost (neplánované převzetí) a přijměte ztrátou dat.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Zahájení neplánované převezme z primární databáze sekundární databáze


Můžete použít rutinu **Set-AzureRmSqlDatabaseSecondary** s parametry **– Převzetí** a **- AllowDataLoss** podporovat sekundární databáze osvobozením od novou databázi primární neplánované způsobem vynucení snížení úrovně existující primární osvobozením sekundární v době, kdy je hlavní databází už není dostupná.

Tato funkce slouží k obnovení havárie při obnovení dostupnost databáze kritické a ztrátu dat přijatelné. Při vyvolání vynuceného převzetí zadaná sekundární databáze okamžitě změní primární databáze a přijímat transakce zapsat, nebude zahájen. Hned, jak je moct připojit se tato nová primární databáze po operaci vynuceného převzetí původní primární databáze, zálohování, se považuje na původní primární databáze a staré primární databáze je volání do vedlejší databáze pro novou databázi primární. následně je pouze otevřené nové primární.

Ale protože čárky obnovit v době není podporován sekundární databází, pokud chcete data obnovení potvrzeného starou primární databázi, které nebyly replikovat na novou primární databázi, by měl zapojit šablony stylů CSS na Obnovit databázi zálohovat známé protokolu.

> [AZURE.NOTE] Pokud je příkaz vydán při online jsou primárních a sekundárních staré primární nepůjde nové sekundární okamžitě bez dat synchronizace. Spouštění transakce, pokud je příkaz vydán ztrátu dat může dojít, pokud je primární.


Pokud primární databázi používá více druhotné proběhne úspěšně částečně příkazu. Sekundární, na kterém ověříte spuštění příkazu stane primárního. Staré primární ale zůstanou primární, tedy dvě základní barvy bude skončily ve nekonzistentní stav připojení propojením pozastavené replikace. Uživatel bude muset ručně opravit tuto konfiguraci pomocí rozhraní API "Odebrat sekundární" na kterékoli z těchto primární databází.


Následující příkaz kombinace kláves vymění role databáze s názvem "mydb" na primární, když není dostupná služba primární. Původní primární, do kterého byla "mydb" připojené k přepne sekundární po návrat do online režimu. V tomto okamžiku synchronizace může mít za následek ztrátou dat.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Další kroky   

- Po selhání zajistěte, aby že požadavky ověřování pro server a databázi nastavené na nové primární. Další informace najdete v tématu [zabezpečení databáze SQL po obnovení havárie](sql-database-geo-replication-security-config.md).
- Další obnovení po selhání pomocí aktivní Geo replikace včetně pre a publikovat obnovení kroky a obnovení podrobností havárie naleznete v tématu [Obnovení cvičení havárie](sql-database-disaster-recovery.md)
- Sasha Nosov příspěvek na blogu o aktivní Geo replikace najdete v článku [zaměření na nové funkce Geo replikace](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informace o navrhování cloudové aplikace používat aktivní Geo replikace najdete v tématu [navrhování cloudu žádosti o nepřerušený pomocí Geo replikace](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informace o používání aktivní Geo replikace fondy pružná databáze najdete v článku [Strategie obnovení havárie pružná fondu](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Přehled firmy continurity naleznete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)
