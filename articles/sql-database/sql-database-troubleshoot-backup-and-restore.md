<properties
    pageTitle="Poradce při potížích s zálohování a obnovení databáze SQL Azure"
    description="Zjistěte, jak obnovit databázi cloudu chyby a výpadků pomocí zálohování a repliky v databázi SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="restore-a-database-to-a-previous-point-in-time-restore-a-deleted-database-or-recover-from-a-data-center-outage"></a>Obnovení databáze předchozí bod, obnovit odstraněnou databázi nebo obnovení z výpadku Centrum dat

Databáze SQL pořád kopie databáze, můžete obnovit z výpadků a chyba uživatele. Dostupné možnosti závisejí na vrstvy služeb databáze a možnosti, které zvolíte. V tématu [Přehled kontinuitu obchodní](sql-database-business-continuity.md) informace a pokyny k návrhu.

## <a name="to-restore-a-database-to-a-previous-point-in-time"></a>Obnovení databáze na předchozí bod
1.  Na [Portálu Azure](https://azure.microsoft.com/)klikněte na **SQL databáze**.
2.  V seznamu vyberte databázi a potom klikněte na **Obnovit**.
3.  Zadejte nový název databáze, zvolte datum a čas obnovení na a pak klikněte na **vytvoření.**
4.  Proveďte požadované změny aplikace podle potřeby neodkazuje nové databáze. V tématu [obnovení databáze na bod v čase](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="to-restore-an-accidentally-deleted-database"></a>Chcete-li obnovit databázi omylem odstraněné
1.  Na [Portálu Azure](https://azure.microsoft.com/)klikněte na položku **SQL servery**.
2.  Vyberte server, který je hostitelem databázi ze seznamu.
3.  Na zásuvné serveru přejděte dolů a klikněte na **odstraněno databáze**.
4.  Vyberte databázi, kterou chcete obnovit a klikněte na **vytvořit**.
5.  Proveďte požadované změny aplikace podle potřeby neodkazuje nové databáze. V tématu [Obnovení odstraněných databáze](sql-database-recovery-using-backups.md#deleted-database-restore).

## <a name="to-recover-from-a-regional-datacenter-outage"></a>Jak obnovit z výpadku regionálního datacentra
U standardních a Premium databází Pokud jste nastavili replikovat geo druhotné můžete obnovit pomocí těchto druhotné. To vám možnost obnovit databázi s méně možností ztrátou dat. Podrobnosti najdete v části [Obnovit databázi Azure SQL pomocí zálohy automatické databáze](sql-database-disaster-recovery.md) .

Azure také poskytuje záložní kopie každé databáze v jiné oblasti (geo nadbytečné záložní kopie). Vytvořit novou databázi ze zálohy, která se nazývá Geo obnovit, ale je pravděpodobné, že budete když vám to přinést ztrátou dat spolehnout tato metoda samostatně.

**Obnovení databáze pomocí geo obnovení:**

- Na [Portálu Azure](https://azure.microsoft.com/)klikněte na **Nový**, klikněte na **Data a úložiště**, klikněte na **Databáze SQL**a vyberte **záložní** jako zdroj databáze. Další informace najdete v článku [obnovení databáze Azure SQL z výpadku](sql-database-disaster-recovery.md) .