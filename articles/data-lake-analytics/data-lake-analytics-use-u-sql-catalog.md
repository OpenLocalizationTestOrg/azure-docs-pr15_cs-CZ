<properties
   pageTitle="Představte katalogu dat Azure jezera analýzy U-SQL | Azure"
   description="Představte katalogu dat Azure jezera analýzy U jazyka SQL"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>Používání katalogu U SQL

Katalog U SQL slouží k vytvoření struktury dat a kód tak, aby se smí zobrazovat skripty U SQL. V katalogu umožňuje možné s daty v Azure dat jezera nejvyšší výkon.

Každý účet Azure dat jezera analýzy má právě jeden katalogu U SQL s ním spojené. Nelze odstranit katalogu U-SQL. U SQL katalogy aktuálně nejde sdílet mezi účty jezera úložiště.

Obsahuje katalog každý U SQL databáze s názvem **hlavní**. Hlavní databázi nelze odstranit.  Každý U SQL katalogu může obsahovat další dalších databází.

U SQL databáze obsahuje:

- Sestavení – sdílení kódu .NET mezi skripty U SQL.
- Funkce hodnotách tabulky – sdílení kódu U SQL mezi skripty U SQL.
- Tabulky – sdílení dat mezi skripty U SQL.
- Schémata – sdílení tabulky schémata mezi skripty U SQL.

## <a name="manage-catalogs"></a>Správa katalogů
Každý účet Azure dat jezera analýzy má výchozí úložiště jezera dat Azure účet spojený s ním. Tento účet úložiště jezera dat se označuje jako výchozí úložiště jezera dat účet. U SQL katalogu je uložená ve výchozí účet úložiště jezera dat ve složce /catalog. Neodstraňujte všechny soubory ve složce /catalog.

### <a name="use-azure-portal"></a>Použití Azure portálu

V tématu [Správa jezera analýzy dat pomocí portálu](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>Pomocí nástrojů jezera datového Visual Studio.

Datové nástroje jezera for Visual Studio slouží ke správě katalogu.  Další informace o nástrojích najdete v článku [Použití datové jezera nástroje for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

**Správa katalogu**

1. Otevřete aplikaci Visual Studio a připojte k azure. Pokyny najdete v tématu [připojení Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure).
1. Otevřete **Průzkumníka serveru** stiskněte **Kombinaci kláves CTRL + ALT + S**.
2. V systému Windows **Server Explorer**rozbalte **Azure**, rozbalte položku **Jezera analýzy dat**, rozbalte účtu jezera analýzy dat, rozbalte **databází**a položku **předlohy**.



    - Pokud chcete přidat novou databázi, klikněte pravým tlačítkem **databáze**a pak klikněte na **Vytvořit databázi**.
    - Pokud chcete přidat nové sestavení, klikněte pravým tlačítkem **sestavení**a potom klikněte na **Zaregistrovat sestavení**.
    - Pokud chcete přidat nové schéma, klikněte pravým tlačítkem **schémata**a klikněte na "vytvořit schématu **.
    - Pokud chcete přidat novou tabulku, klikněte pravým tlačítkem **tabulky**a klikněte na "" vytvořit tabulku **.
    - Pokud chcete přidat novou funkci vracejícími tabulku, najdete v článku [operátory pro analýzy dat jezera úlohy definované uživatelem vyvíjet U-SQL](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![Procházet katalogy U SQL Visual Studio](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Viz taky

- Začínáme
    - [Začínáme s jezera analýzy dat Azure portálu](data-lake-analytics-get-started-portal.md)
    - [Začínáme s jezera analýzy dat pomocí prostředí PowerShell Azure](data-lake-analytics-get-started-powershell.md)
    - [Začínáme s jezera analýzy dat pomocí Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
    - [Můžete vyvíjet U SQL skriptů pomocí Data jezera Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Začínáme s jazykem Azure dat jezera analýzy U SQL](data-lake-analytics-u-sql-get-started.md)

- U SQL a vývoj
    - [Začínáme s jazykem Azure dat jezera analýzy U SQL](data-lake-analytics-u-sql-get-started.md)
    - [Používat funkce okno U SQL Azure dat jezera analýzy projektů](data-lake-analytics-use-window-functions.md)
    - [Můžete vyvíjet operátory definované uživatelem U jazyka SQL pro úlohy jezera analýzy dat](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Správa
    - [Správa Azure dat jezera technologie pro analýzu Azure portálu](data-lake-analytics-manage-use-portal.md)
    - [Správa Azure dat jezera technologie pro analýzu pomocí prostředí PowerShell Azure](data-lake-analytics-manage-use-powershell.md)
    - [Sledování a odstraňování případných problémů jezera analýzy dat Azure úlohy Azure portálu](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- Kurz začátku do konce
    - [Použití interaktivní výukové programy pro analýzy jezera dat Azure](data-lake-analytics-use-interactive-tutorials.md)
    - [Analýza protokoly webu pomocí analýzy jezera dat Azure](data-lake-analytics-analyze-weblogs.md)
