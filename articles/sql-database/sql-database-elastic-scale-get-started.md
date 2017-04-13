<properties 
    pageTitle="Začínáme s pružná databázové nástroje" 
    description="Základní vysvětlení funkce nástroje pružná databáze databáze SQL Azure, včetně Snadné spuštění aplikace vzorku." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Začínáme s pružná databázové nástroje

Tento dokument vás seznámí s prostředí developer spuštěním aplikaci vzorku. Vzorek vytvoří jednoduchou sharded aplikaci a prozkoumá klíčové funkce pružná databázové nástroje. Vzorku ukazuje funkce [knihovny klienta pružná databáze](sql-database-elastic-database-client-library.md)

Pokud chcete nainstalovat knihovnu, přejděte na [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Poznámka: nainstalované knihovny s aplikací ukázkové píše níže.

## <a name="prerequisites"></a>Zjistit předpoklady pro

1. Vyžaduje se Visual Studio 2012 a novějších verzích se C#. Stažení bezplatné verze na [Visual Studio stáhne](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. Nuget 2.7 nebo vyšší. Získání nejnovější verzi, naleznete v článku [Instalace NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Stáhnout a spustit aplikaci ukázka

**Pružná databázi Azure SQL – Začínáme** ukázková aplikace vysvětluje nejdůležitější aspekty vývojové prostředí pro sharded aplikací pomocí nástrojů pružná databáze Azure SQL. Se zaměřuje na klíčové použití balení [shard mapování správy](sql-database-elastic-scale-shard-map-management.md), [data závislá směrování](sql-database-elastic-scale-data-dependent-routing.md) a [více shard dotazování](sql-database-elastic-scale-multishard-querying.md). Chcete-li stáhnout a spustit vzorku, postupujte takto: 

1. Otevřete aplikaci Visual Studio a vyberte **Soubor -> Nový -> projektu**.
2. V dialogovém okně klikněte na tlačítko **Online**.

    ![Nový Projekt > Online][2]
3. Klepněte na **Visual Basic** klikněte v části **Ukázky**.

    ![Klikněte na tlačítko Visual C#][3]
4. Do pole Hledat zadejte **pružná db** vyhledat vzorku. Název pružná DB pro SQL Azure – Začínáme se zobrazí karta **Nástroje** .

    ![Vyhledávací pole][1]
 
5. Vyberte výběru, zvolte název a místo nového projektu a klikněte na **OK** vytvořte projekt.
6. Otevřete **konfiguračního** souboru v řešení pro projekt vzorku a postupujte podle pokynů v souboru a přidejte název svého serveru databáze Azure SQL a vaše přihlašovací údaje (uživatelské jméno a heslo).
7. Vytvoření a spuštění aplikace. Pokud se zobrazí dotaz, může trvat Visual Studio obnovit NuGet balíčků řešení. Z NuGet to bude stáhnout nejnovější verzi klienta knihovny pružná databáze.
8. Přehrát s různé možnosti zobrazíte další informace o možnosti knihovny klienta. Poznámka: kroky, které aplikace, která bude v konzole výstup a neváhejte, které můžete prozkoumat kódem na pozadí.

    ![průběh][4]

Blahopřejeme – úspěšně vytvořené a spusťte aplikaci první sharded pomocí pružná databázové nástroje na databázi SQL Azure. Podívejte se snadné shards vytvořené vzorku připojením pomocí aplikace Visual Studio nebo SQL Server Management Studio na databázový Server Azure. Zjistíte nové databáze ukázkové shard a shard správce databáze mapy vytvořenou vzorku.

> [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>Klíčové použitelné části ukázka kódu

1. **Správa Shards a Shard mapy**: Tento kód ukazuje, jak pracovat s shards, oblastí, a zařaďte **ShardMapManagerSample.cs**mapování. Můžete najít další informace o tomto tématu: [Správa mapování Shard](http://go.microsoft.com/?linkid=9862595).  
2. **Data závislá směrování**: směrování transakce na pravém shard se zobrazují v **DataDependentRoutingSample.cs**. Další informace najdete v tématu [Závislá směrování Data](http://go.microsoft.com/?linkid=9862596). 
3. **Dotazování na více Shards**: dotazování přes shards je znázorněn v souboru **MultiShardQuerySample.cs**. Další podrobnosti najdete v tématu [Více Shard dotazu](http://go.microsoft.com/?linkid=9862597).
4. **Přidání prázdné shards**: iterativních přidání nového prázdného shards provádí kód v souboru **AddNewShardsSample.cs**. Podrobnosti o tomto tématu se vztahují tady: [Správa mapování Shard](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Další operace pružná měřítko

1. **Rozdělení existující shard**: možnost rozdělení shards je k dispozici pomocí **nástroje sloučení rozdělení**. Můžete najít další informace o tomto nástroji: [Přehled nástroje rozděleného sloučení](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Sloučení existující shards**: Shard sloučí provádí také pomocí **nástrojů korespondence rozdělení**. Další informace najdete v příručce: [Přehled nástrojů korespondence rozdělení](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Pole náklady

Pružná databázové nástroje jsou zdarma. Pružná databázové nástroje neukládá dalším poplatkům nad ceny Azure použití. 

Například ukázková aplikace vytvoří nové databáze. Náklady závisí na verzi databáze databáze SQL Azure, které vyberete a Azure použití aplikace.

Ceny informace v tématu [SQL databáze ceny podrobnosti](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Další kroky
Další informace o pružná databázové nástroje najdete tady:

* [Mapa dokumentace nástroje pružná databáze](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Ukázky: 
    -    [Pružná DB s Azure SQL – Začínáme](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Pružná DB s Azure SQL - integrace s Entity Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Shard pružnost na Centrum skriptů](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blog: [pružná měřítko oznámení](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Kanál 9: [pružná měřítko přehled Video](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Diskusní fóra: [Fórum komunity databáze SQL Azure](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Měření výkonu: [výkonnosti pro správce shard mapy](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
