<properties
    pageTitle="Přesuňte databází mezi servery, mezi předplatná a a odhlášení z Azure."
    description="Rychlé kroky chcete kopírovat, přesunout a migraci dat a databází v databázi SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Přesuňte databází mezi servery, mezi předplatná a a odhlášení z Azure

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Přesunutí databáze na jiný server v rámci stejného předplatného
- Na [Portálu Azure](https://portal.azure.com)klikněte **SQL databáze**, v seznamu vyberte databázi a potom klikněte na **Kopírovat**. Podrobnější informace v tématu [kopírování databáze Azure SQL](sql-database-copy.md) .

## <a name="to-move-a-database-between-subscriptions"></a>Přesunutí databáze mezi předplatná
- Na [Portálu Azure](https://portal.azure.com)klikněte na položku **SQL servery** a potom vyberte server, který je hostitelem vaší databáze ze seznamu. Klikněte na příkaz **Přesunout**a vyberte předplatného přejdete k a zdroje, které chcete přesunout.

## <a name="to-migrate-a-sql-database-into-azure"></a>Migrovat databázi SQL do Azure
- Určení kompatibility databázi a potom vyberte metodu správné migrace podle vašich potřeb. Postupujte podle pokynů a možnosti v [Migrace databáze SQL serveru](sql-database-cloud-migrate.md).

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>Chcete-li vytvořit kopii databáze pro použití mimo Azure
- [Exportujte do souboru BACPAC.](sql-database-export.md)
