<properties
    pageTitle="Zálohování databáze roztáhnout s podporou | Microsoft Azure"
    description="Naučte se obecnějším údajům roztáhnout\-povolené databází."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2016"
    ms.author="douglasl"/>

# <a name="backup-stretch-enabled-databases"></a>Zálohování databáze roztáhnout s podporou

Zálohování databáze vám pomohou při obnovení z mnoho typů k chybám, chyb a havárie.  

-   Máte k obecnějším údajům vaší roztáhnout\-povolené databáze systému SQL Server.  

-   Microsoft Azure automaticky zálohovat Vzdálená data, která roztáhnout databáze obsahuje poštovním, ze serveru SQL Server Azure.  

>    [AZURE.NOTE] Zálohování bylo jedinou část dokončení dostupnost a obchodního kontinuitu řešení. Další informace o dostupnost najdete v článku [Vysoké dostupnost řešení](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="back-up-your-sql-server-data"></a>Obecnějším údajům data SQL serveru  

K obecnějším údajům vaší roztáhnout\-povolené databáze systému SQL Server, můžete dál používat záložní metody SQL serveru, které používáte. Další informace najdete v tématu [zálohování a obnovení z databáze SQL Server](https://msdn.microsoft.com/library/ms187048.aspx).

Zálohování databáze SQL serveru roztáhnout s podporou obsahovat pouze místních dat a datové využívat migrace v bodě v čase při spuštění zálohování. \(Možný data jsou data, která není zatím poštovním, ale se přesunou do Azure na základě nastavení migrace tabulek.\) Tím se označuje jako zálohu **omezené** a neobsahuje data už migruje se na Azure.  

## <a name="back-up-your-remote-azure-data"></a>Zálohování dat aplikace vzdálené Azure   

Microsoft Azure automaticky zálohovat Vzdálená data, která roztáhnout databáze obsahuje poštovním, ze serveru SQL Server Azure.  

### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure snižuje riziko ztrátou dat pomocí automatického zálohování  
Služba roztáhnout databáze systému SQL Server na Azure chrání Vzdálená databáze se automatické ukládání snímků minimálně každých 8 hodin. Ponechává každý snímek dobu 7 dní k poskytování oblasti bodů možné obnovit.  

### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure snižuje riziko ztrátou dat s geo\-redundance  
Zálohování databáze Azure jsou uložené na geo\-nadbytečné Azure úložiště (Vzdálená pomoc\-GRS), a proto jsou geo\-nadbytečné ve výchozím nastavení. GEO\-nadbytečné úložiště zreplikuje dat na vedlejší oblast, která je stovky mil od primárního oblast. V oblasti primárních a sekundárních je třikrát každý replikovat dat přes samostatné poruch domén a upgrade domén. Zajistíte, že je vaše data trvalá i v případě havárie, který se vykreslují pomocí jedné z Azure oblastí není k dispozici a dokončení místní výpadku.

### <a name="stretchRPO"></a>Roztažení databáze snižuje riziko ztrátou dat pro typ vašich dat Azure dočasně zachováním migrované řádků
Po roztáhnout databáze migruje měl řádků ze serveru SQL Server Azure, zachová tyto řádky v tabulce pracovní dobu nejméně 8 hodin. Při obnovení záložní databáze Azure, roztáhnout databázi používá řádky uložené v tabulce pracovní odsouhlasit SQL serveru a Azure databází.

Po obnovení záložní kopie Azure dat je třeba spustit uložená procedura [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) se znovu připojit roztáhnout\-povolené databáze SQL serveru k vzdálené databázi Azure. Když spustíte **sys.sp_rda_reauthorize_db**, roztáhnout databáze sloučí SQL serveru a Azure databází.

Zvýšit počet hodin migrované data, která databázi roztáhnout zachová dočasně v tabulce pracovní, spusťte uložená procedura [sys.sp_rda_set_rpo_duration](https://msdn.microsoft.com/library/mt707766.aspx) a zadejte číslo větší než 8 hodin. Při rozhodování, kolik uchovávání dat, zvažte následující faktory:
-   Četnost automatické Azure zálohování (minimálně každých 8 hodin).
-   Dobu potřebnou po problém rozpoznat problému a rozhodnete obnovit zálohy.
-   Doba trvání Azure obnovení.

> [AZURE.NOTE] Zvýšení množství dat, která databázi roztáhnout zachová dočasně v tabulce pracovní zvýší požadované místo na serveru SQL Server.

Pokud chcete zjistit počet hodin dat, roztáhnout databáze právě dočasně zachová v tabulce pracovní, spusťte uložená procedura [sys.sp_rda_get_rpo_duration](https://msdn.microsoft.com/library/mt707767.aspx).

## <a name="see-also"></a>Viz taky

[Správa a řešení potíží s roztáhnout databáze](sql-server-stretch-database-manage.md)

[Sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Zálohování a obnovení databáze SQL serveru](https://msdn.microsoft.com/library/ms187048.aspx)
