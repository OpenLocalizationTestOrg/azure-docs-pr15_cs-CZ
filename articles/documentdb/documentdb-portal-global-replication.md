<properties
    pageTitle="Replikace databáze globální DocumentDB | Microsoft Azure"
    description="Naučte se spravovat globální replikace účtu DocumentDB prostřednictvím portálu Azure."
    services="documentdb"
    keywords="Globální databáze, replikace"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Jak provádět replikace databáze globální DocumentDB pomocí portálu Azure

Naučte se používat portál Azure replikace dat ve více oblastech globální dostupnost dat v Azure DocumentDB.

Informace o tom, jak globální databáze replikace funguje v DocumentDB najdete v tématu [Rozmístit data globálně DocumentDB](documentdb-distribute-data-globally.md). Informace o provedení replikace databáze globální programově najdete v tématu [rozvojových s účty DocumentDB více oblastí](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Globální distribuční DocumentDB databází je všeobecně dostupná a automaticky povoleno pro všechny nově vytvořený DocumentDB účty. Pracujeme na povolení globálního distribuce pro všechny existující účty, ale mezitím požadovaná globální distribuční povolili na váš účet, [Kontaktujte podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a jsme budete povolte ho můžete nyní.

## <a id="addregion"></a>Přidání oblastech globální databáze

DocumentDB je k dispozici ve většině [Azure oblastí] [azureregions]. Po výběru výchozí úroveň konzistence pro váš účet databáze, můžete propojit jeden nebo více oblastí (v závislosti na svojí volbě Výchozí konzistence úrovně a globální rozdělení potřeb).

1. V [Azure portál](https://portal.azure.com/)v Jumpbar klikněte na **DocumentDB účty**.
2. V zásuvné **DocumentDB účtu** vyberte účet databáze, který chcete upravit.
3. V zásuvné účtu klikněte v nabídce **globálně replikovat data** .
4. V zásuvné **globálně replikovat data** vyberte oblasti, které chcete přidat nebo odebrat a potom klikněte na **Uložit**. Náklady pro přidání oblastí najdete v tématu [ceny stránky](https://azure.microsoft.com/pricing/details/documentdb/) nebo Další informace v článku [Rozmístit data globálně DocumentDB](documentdb-distribute-data-globally.md) .

    ![Klikněte na oblasti v mapování a přidávat nebo odebírat][1]

### <a name="selecting-global-database-regions"></a>Výběr oblasti globální databáze

Při konfiguraci dvě nebo více oblastí, je vhodné, že nejsou vybrány oblastí založené na oblast dvojice podle [firmy kontinuitu a havárie obnovení (BCDR): Azure párovaný oblastí]  [ bcdr] článek.

Konkrétně při konfiguraci do více oblastí, zkontrolujte, že vyberte stejný počet oblastí (+/-sudých a 1) ze všech sloupců párových oblast. Například, pokud chcete nasadit čtyřech oblastech USA, musíte sami vybrat dvou oblastí USA sloupci nalevo a dvě zprava. Ano, následující bude vhodné nastavit: západní USA, východoasijských USA, severní ústředním US a Jižní ústředním US.

Tento návod je důležité postupujte při nastavení pouze dvou oblastí havárie zotavení scénářích. Více než dvou oblastí pokynů Toto je vhodné, ale nejsou kritické, dokud některé z vybraných oblastí dodržovat toto párování.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Další kroky

Naučte se spravovat konzistence účtu globálně replikovanou načtením [konzistence úrovní v DocumentDB](documentdb-consistency-levels.md).

Informace o tom, jak globální databáze replikace probíhá DocumentDB najdete v tématu [Rozmístit data globálně DocumentDB](documentdb-distribute-data-globally.md). Informace o programově replikace dat ve více oblastech najdete v tématu [rozvojových s účty DocumentDB více oblastí](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
