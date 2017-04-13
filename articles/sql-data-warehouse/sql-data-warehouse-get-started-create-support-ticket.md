<properties
   pageTitle="Jak vytvořit lístek podpora pro systém SQL Data Warehouse | Microsoft Azure"
   description="Jak vytvořit požadavek podpory můžete v Azure SQL datový sklad."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess"/>

# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Jak vytvořit lístek podpora pro systém SQL Data Warehouse
 
Pokud máte potíže s SQL datový sklad, přejděte prosím vytvoříte vstupenek technické podpory, aby vám mohou pomoci náš tým inženýrské.

## <a name="create-a-support-ticket"></a>Vytvoření požadavek podpory můžete

1. Otevřete [portál Azure][].

2. Na úvodní obrazovce klikněte na dlaždici **nápovědy + podpory** .

    ![Nápověda k + podpory](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)

3. Na stránce Pomozte + zásuvné podpory, klepněte na tlačítko **vytvořit žádost o podporu**.

    ![Nová žádost o podporu](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
    
    <a name="request-quota-change"></a> 

4. Vyberte možnost **požádat o typ**.

    ![Požádat o typ](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
    
    >[AZURE.NOTE]  Ve výchozím nastavení obsahuje každý SQL server (například myserver.database.windows.net) **DTU kvóty** 45,000. Tento kvóta je jednoduše limit zabezpečení. Zvýšit kvótu vytváření požadavek podpory můžete a výběrem *kvóty* psaní žádost. K výpočtu vaší DTU potřebuje, vynásobte 7.5 souhrn, který [DWU][] potřeby. Třeba byste chtěli hostovat dvou DW6000s na jednom SQL serveru a pak požadujete DTU kvóta 90,000.  Vaše aktuální DTU spotřebu zásuvné SQL serveru můžete zobrazit na portálu. Pozastavené a rušení pozastaveném databází počítání směrem k DTU kvóty. 

5. Vyberte **předplatné** , který je hostitelem databáze se problém, který chcete ohlásit.

    ![Předplatné](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)

6. Vyberte **SQL datový sklad** jako zdroje.

    ![Zdroje](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)

7. Vyberte svůj [Azure podpory plánu][].

    - Podpora **Fakturace, Správa kvót a předplatné** je k dispozici na všech úrovních podpory.
    - Podporu **Konec fix** je k dispozici [Karta Vývojář][], [Standardní][], [Professional přímé][] nebo [Premier][] support. Konec opravit problémy se problémy, kterým zákazníky při používání Azure kde není rozumné očekávání Microsoft příčinou problému.
    - Jsou dostupné na úrovni [Professional přímý][] a [Premier][] support **vedení vývojář** a **poradní služby** . 
    
    Pokud máte Premier support plánu, můžete nahlásit taky v SQL datový sklad otázek na [online portál Microsoft Premier][].  V tématu [Azure podporují plány][Azure podpory plánu] Další informace o různých plánech podpory, včetně obor doby odezvy, ceny, atd.  Nejčastější dotazy týkající se Azure podpory najdete v tématu [Azure podporují časté otázky][].  

    ![Plán podpory](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)

8. Vyberte **Typ problému** a **kategorie**.

    ![Kategorie typu problém](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)

9. Popis problému a zvolte požadovanou úroveň obchodních výsledků.

    ![Popis problému](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)

10. **Kontaktní informace** pro tento požadavek podpory můžete bude předem vyplněné. Změnit v případě potřeby.

    ![Informace o kontaktech](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)

11. Klikněte na **vytvořit** odešlete žádost o podporu.


## <a name="monitor-a-support-ticket"></a>Sledování požadavek podpory můžete

Poté, co jste odeslali žádost o podporu, Azure podpoře vás kontaktovat. Kontrola stavu žádost a podrobnosti, klikněte na **Spravovat požadavky podpory** na řídicím panelu.

![Kontrola stavu](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Další zdroje

Kromě toho se můžete připojit se komunity SQL datový sklad [Přetečení zásobníku][] nebo na [fórum MSDN sklad Data SQL Azure][].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Azure portálu]: https://portal.azure.com/
[Plán podpory Azure]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Karta Vývojář]: https://azure.microsoft.com/support/plans/developer/  
[Standardní]: https://azure.microsoft.com/support/plans/standard/  
[Profesionální přímé]: https://azure.microsoft.com/support/plans/prodirect/  
[Prémiová]: https://azure.microsoft.com/support/plans/premier/  
[Nejčastější dotazy týkající se Azure podpory]: https://azure.microsoft.com/support/faq/
[Online portál Microsoft Premier]: https://premier.microsoft.com/
[Přetečení zásobníku]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Fórum komunity Azure SQL datový sklad MSDN]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

