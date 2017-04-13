<properties
   pageTitle="Zálohování databáze SQL – automatické, geo nadbytečné | Microsoft Azure" 
   description="Databáze SQL automaticky vytvoří místní databázi zálohu všech pět minut a používá Azure přístup pro čtení geo nadbytečné úložiště (Vzdálená pomoc GRS) za účelem geo zálohování. "
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/20/2016"
   ms.author="carlrab;barbkess"/>

<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.


Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."
------------------>

<!----------------

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    • Definition of the feature terminology.  i.e., What is a database backup?
    • Characteristics and capabilities of the feature. (How the feature works)
    • Common uses with links to overview topics that recommend when to use the feature.
    • Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    • Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    • How to steps for using the feature (Tasks)
    • How to solve business problems that incorporate the feature (Overviews)
------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
    
    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "Elastic database pools", use a synonym. For example:    "Learn about elastic database pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="learn-about-sql-database-backups"></a>Další informace o zálohování databáze SQL

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:
    
 

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a local database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

Databáze SQL automaticky vytvoří zálohu místní databázi každých několik minut a používá Azure geo nadbytečné úložiště přístup pro čtení pro geo redundance. Zálohování databáze jsou důležitou součástí všech obchodní strategie pro obnovení kontinuitu a havárie, protože jsou ochrana dat náhodné poškozením nebo odstranění. 

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.
    
    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Co je zálohy databáze SQL?  

<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->

Zálohování databáze SQL zahrnuje jak místní databázi a geo nadbytečné zálohování. Tyto zálohy se vytvářejí automaticky a bez dalších poplatků. Nemusíte udělat nic, aby se daly dojít.

<!----------------- 
    Explains first component of the backup feature
------------------>

Pro místní zálohy databáze SQL používá technologii SQL serveru k vytváření záloh [úplné](https://msdn.microsoft.com/library/ms186289.aspx) [rozdílné](https://msdn.microsoft.com/library/ms175526.aspx )a [transakční protokol](https://msdn.microsoft.com/library/ms191429.aspx) . Zálohy protokolu transakce dojít každých pět minut, které umožňuje provádět v okamžiku obnovení stejný server, který je hostitelem databáze. Při obnově databáze služby hodnoty se celé rozdílu a transakce protokolu zálohování muset obnovit.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Použijte zálohu místní databázi, kterou chcete:

- Obnovení databáze v daném okamžiku v období uchování. Zálohování databáze můžete obnovit databázi v daném okamžiku, obnovení odstraněné databáze do doby, kdy došlo k odstranění nebo obnovení databáze pro jinou zeměpisnou oblast. Abyste mohli provést obnovení, najdete v článku [obnovení databáze ze zálohy databáze](sql-database-recovery-using-backups.md).

- Zkopírujte databáze na serveru SQL Server ve stejném nebo jiném oblasti. V kopii odpovídá využití transakce aktuální databáze SQL. Abyste mohli provést kopii, přečtěte si článek [kopírování databáze](sql-database-copy.md).

- Archivace zálohování databáze za období záložní uchovávání informací. Chcete-li provést archiv, [exportovat databázi SQL BACPAC](sql-database-export.md) soubor. Pak můžete archivovat BACPAC k základnímu úložišti dlouhodobé a uložte ji za období uchování. Můžete také použít BACPAC převod kopii databáze SQL serveru, buď místně nebo v Azure virtuálního počítače (OM).

<!----------------- 
    Explains first component of the backup feature
------------------>

Geo nadbytečné záloh používá databáze SQL [Azure úložiště replikace](../storage/storage-redundancy.md). Databáze SQL ukládá záložních souborů místní databázi účet [Geo nadbytečné úložiště přístup pro čtení (Vzdálená pomoc GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) . Azure zreplikuje záložních souborů [párových datacentra](../best-practices-availability-paired-regions.md). 

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Použijte geo nadbytečné zálohy:

- Obnovení databáze pro jinou zeměpisnou oblast v případě, že nebudete mít přístup zálohování databáze z oblast hlavní databází. 

>[AZURE.NOTE] V Azure úložiště termínů *replikace* odkazuje na kopírování souborů z jednoho místa do jiného. *Replikace databáze* SQL odkazuje na uchovávání k více databázím sekundární sesynchronizovaná s hlavní databází. 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Kolik záložní úložiště je součástí zdarma?

Databáze SQL poskytuje až 200 % úložišti maximální zřizování databáze jako úložišti bezplatně. Například pokud máte instanci standardní DB s velikostí zřizování DB 250 GB, máte 500 GB úložiště zálohy bez dalších poplatků. Pokud databázi překročí ujednaných záložní úložiště, můžete zmenšit období uchování kontaktováním podpory Azure. Další možností je zaplatit extra úložišti, která je účtováno standardní sazbou přístup pro čtení geograficky nadbytečné úložiště (Vzdálená pomoc GRS). 

## <a name="how-often-do-backups-happen"></a>Jak často zálohy děje?

Pro místní databázi zálohování zálohování celé databáze dojít, týdně, zálohování rozdílné databáze dojít každou hodinu a transakční protokol, který zálohy dojít každých pět minut. První úplné zálohování naplánován ihned po vytvoření databáze. Obvykle dokončením než 30 minut, ale může trvat déle, když je značné velikosti databáze. Například počáteční zálohování může trvat déle na obnovená databáze nebo kopii databáze. Po prvním úplné zálohování jsou všechny další zálohy automaticky naplánované a spravovat tiše na pozadí. Přesný čas celé a [rozdílné](https://msdn.microsoft.com/library/ms175526.aspx) zálohování databáze je určený podle zůstatky celkové pracovní zátěž systému. 

Geo nadbytečné záloh úplné a rozdílné zálohy zkopírují podle plánu replikace Azure úložiště.

## <a name="how-long-do-you-keep-my-backups"></a>Jak dlouho uchováváte Moje záloh?

Každý zálohování databáze SQL má období uchování založený na [vrstvy služeb](sql-database-service-tiers.md) databáze. Doba uchovávání informací pro danou databázi v:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

- Služby základní osy je sedmi dnů.
- Vrstvy standardní služeb je 35 dní.
- Vrstvy služeb Premium je 35 dní.


Je-li omezit databáze ze standardní či Premium úrovní služby základní zálohy se ukládají sedmi dnů. Všechny stávající záložní kopie starší než 7 dnů už nejsou dostupné žádné. 

Pokud upgradujete databázi ze služby základní osy na standardní nebo Premium, bude databáze SQL tak, aby byly 35 dnů existující zálohy. Uloží novou zálohy při jejich vzniku 35 dnů.
 
Pokud byste odstranili databáze, bude databáze SQL zálohy stejným způsobem, který by pro databázi online. Předpokládejme například, že odstraníte základní databázi, která obsahuje období uchování sedmi dnů. Zálohy, není čtyři dnů se uloží další tři dny.

>[AZURE.IMPORTANT]
    Pokud byste odstranili, která hostuje databáze SQL Azure SQL serveru, odstraní se taky všechny databáze, které patří k serveru a nelze ji obnovit. Nebude možné obnovit odstraněné serveru.

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Další kroky

Zálohování databáze jsou důležitou součástí všech obchodní strategie pro obnovení kontinuitu a havárie, protože jsou ochrana dat náhodné poškozením nebo odstranění. Chcete-li zobrazit jak zálohování databáze do strategii širší najdete v článku [Přehled kontinuitu Business](sql-database-business-continuity.md).


