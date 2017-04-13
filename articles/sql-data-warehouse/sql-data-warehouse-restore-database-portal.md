<properties
   pageTitle="Obnovení Azure SQL datový sklad (portál) | Microsoft Azure"
   description="Azure portálu úkoly při obnovení datový sklad SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-portal"></a>Obnovení Azure SQL datový sklad (portál)

> [AZURE.SELECTOR]
- [Základní informace][]
- [Portál][]
- [Prostředí PowerShell][]
- [ZBÝVAJÍCÍ][]

V tomto článku se dozvíte, jak obnovit sklad dat SQL Azure pomocí portálu Azure.

## <a name="before-you-begin"></a>Než začnete

**Ověřte DTU kapacity.** Každý SQL datový sklad je hostovaný aplikací SQL server (například myserver.database.windows.net), který má výchozí kvóta DTU.  Před obnovením datový sklad SQL, ověřte, že serveru SQL server má dost zbývající DTU kvóty pro databáze obnovena. Zjistěte, jak vypočítat DTU potřeby nebo požádejte o další DTU, najdete v článku [požadavek na změnu DTU kvóty][].


## <a name="restore-an-active-or-paused-database"></a>Obnovení databáze aktivní nebo pozastaveného

Obnovení databáze:

1. Přihlaste se k [portálu Azure][]
2. Na levé straně obrazovky vyberte **Procházet** a pak vyberte **servery SQL**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Přejděte na serveru a vyberte ho
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Najděte datový sklad SQL, který chcete obnovit z a vyberte ho
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. V horní části zásuvné datový sklad klepněte na tlačítko **Obnovit**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Zadejte nový **název databáze**
7. Výběr poslední **Bod obnovení**
    1. Zkontrolujte, že vyberete nejnovější bod obnovení.  Protože body obnovení se zobrazí v UTC, někdy výchozí možnost vidět není nejnovější bod obnovení.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Klikněte na tlačítko **OK**
9. Proces obnovení databáze začne a můžete sledovat pomocí **oznámení**

>[AZURE.NOTE] Po dokončení obnovení můžete nakonfigurovat obnoveného databázi tak, že následující [Konfigurace databáze po obnovení][].


## <a name="restore-a-deleted-database"></a>Obnovení odstraněné databáze

Obnovení odstraněné databáze:

1. Přihlaste se k [portálu Azure][]
2. Na levé straně obrazovky vyberte **Procházet** a pak vyberte **servery SQL**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Přejděte na serveru a vyberte ho
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Přejděte dolů do části operace na zásuvné váš server
5. Klikněte na dlaždici **Odstranit databáze**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Vyberte odstraněné databázi, kterou chcete obnovit
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Zadejte nový **název databáze**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Klikněte na tlačítko **OK**
9. Proces obnovení databáze začne a můžete sledovat pomocí **oznámení**

>[AZURE.NOTE] Po dokončení obnovení nastavení databáze naleznete v tématu [Konfigurace databáze po obnovení][]. 

## <a name="next-steps"></a>Další kroky
Další informace o funkcích business kontinuitu edicí databáze SQL Azure, přečtěte si [databáze SQL Azure obchodní kontinuitu přehled][].

<!--Image references-->

<!--Article references-->
[Základní informace kontinuitu firmy databáze SQL Azure]: ./sql-database-business-continuity.md
[Základní informace]: ./sql-data-warehouse-restore-database-overview.md
[Portál]: ./sql-data-warehouse-restore-database-portal.md
[Prostředí PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[ZBÝVAJÍCÍ]: ./sql-data-warehouse-restore-database-rest-api.md
[Konfigurace databáze po obnovení]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Požadavek na změnu kvóty DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portálu]: https://portal.azure.com/
