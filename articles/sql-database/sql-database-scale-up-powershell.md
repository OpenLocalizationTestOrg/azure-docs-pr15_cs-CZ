<properties 
    pageTitle="Změna úrovně osy a výkonu služby databázi Azure SQL pomocí prostředí PowerShell | Microsoft Azure" 
    description="Změna vrstvy služeb a úroveň výkonu databáze Azure SQL ukazuje, jak změnit velikost databáze SQL nahoru nebo dolů v prostředí PowerShell. Změna ceny osy databáze Azure SQL pomocí prostředí PowerShell." 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>Změna služby osy a výkonu úrovně (cena za osy) databáze SQL pomocí prostředí PowerShell


> [AZURE.SELECTOR]
- [Azure portálu](sql-database-scale-up.md)
- [**Prostředí PowerShell**](sql-database-scale-up-powershell.md)


Služba úrovní výkonu úrovně popisuje funkce a prostředky k dispozici pro databázi SQL a můžete aktualizovat podle potřeby změnit aplikace. Podrobnosti najdete v tématu [– Úrovně služeb](sql-database-service-tiers.md).

Všimněte si, že změna vrstvy služeb a/nebo úroveň výkonu databáze vytvoří kopii původní databáze na nové úrovni výkonu a pak se přepne připojení k otevřené. Během tohoto procesu dojde ke ztrátě žádná data, ale během stručný momentový při jsme přepnutím na otevřené připojení k databázi zakázány, aby některé transakce letu může vrátit zpět. Toto okno se liší, ale je průměrně o ve skupinovém rámečku 4 sekundy a ve více než 99 % případech je menší než 30 sekund. Velmi málo zejména pokud existují velké počtu transakce v letech v okamžiku, kdy připojení jsou zakázány, toto okno může být delší.  

Doba trvání celého procesu měřítko zdola nahoru závisí na velikosti a service osy databázi před a po změně. 250 GB databázi, která se mění do z nebo v rámci standardní služba osy, by například Dokončit 6 hodin. Pro danou databázi velikosti změny výkonu úrovních vrstvy služeb Premium měli dokončit 3 hodin.


- Snížit verzi databáze, musí být menší než maximální povolenou velikost vrstvy služeb cílovou databázi. 
- Při upgradu databáze s [Geo replikace](sql-database-geo-replication-portal.md) povoleno, je třeba nejprve upgradovat sekundární databází na osy požadovanou výkonu před upgradem primární databáze.
- Při přechodu ze vrstvy služeb Premium, musíte nejdřív ukončit všechny relace Geo replikace. Postupujte podle kroků popsaných v tématu [obnovení z výpadku](sql-database-disaster-recovery.md) ukončit proces replikace mezi primární a sekundární aktivní databází.
- Nabízené služby obnovení se liší pro různé úrovně služby. Pokud jsou přechodu se může dojít ke ztrátě možnost obnovit bodu v čase nebo nižší období záložní uchovávání informací. Další informace najdete v článku [zálohy databáze SQL Azure a obnovení](sql-database-business-continuity.md).
- Nové vlastnosti databáze nejsou použity až do dokončení změny.



**Tento článek je potřeba k provedení těchto věcí:**

- Předplatné Azure. Pokud potřebujete předplatné Azure jednoduše klikněte na **Bezplatný účet** v horní části této stránky a pak se vrátit k dokončení tohoto článku.
- Databáze Azure SQL. Pokud nemáte SQL databáze, vytvořte jednu kroků v tomto článku: [vytvoření první databáze SQL Azure](sql-database-get-started.md).
- Azure Powershellu.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>Změna úrovně osy a výkonu služby SQL databáze

Spuštění [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) rutinách a set **-RequestedServiceObjectiveName** výkonu úrovně požadovaný ceny vrstva; například *S0* *S1*, *S2*, *S3*, *P1*, *P2*,...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>Ukázka skriptu prostředí PowerShell změnit úroveň osy a výkonu služby SQL databáze

Nahrazení ```{variables}``` s požadované hodnoty (nezahrnujte složené závorky).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Další kroky

- [Změna velikosti a](sql-database-elastic-scale-get-started.md)
- [Připojení a dotaz SQL databáze pomocí SSMS](sql-database-connect-query-ssms.md)
- [Export databáze Azure SQL](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Další zdroje informací

- [Obchodní kontinuitu přehled](sql-database-business-continuity.md)
- [Dokumentace databáze SQL](http://azure.microsoft.com/documentation/services/sql-database/)
- [Rutiny databáze azure SQL] (http://msdn.microsoft.com/library/azure/mt574084https :/ / msdn.microsoft.com/knihovně/azure/mt619433(v=azure.300\).aspx.aspx)