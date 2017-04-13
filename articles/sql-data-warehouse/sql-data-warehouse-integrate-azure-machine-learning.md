<properties
   pageTitle="Pomocí datový sklad SQL Azure počítače výukové | Microsoft Azure"
   description="Kurz pro používání Azure počítače učení s Azure SQL datový sklad k vývoji řešení."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Pomocí datový sklad SQL Azure počítače výukové

Azure výukové počítače je služba plně spravovaných prognostické analýzy, můžete použít k vytváření prediktivní modelů proti dat v SQL datový sklad a pak publikovat jako jste připravení používat webové služby. Se dozvíte základní informace o prediktivní technologie pro analýzu a automatické učení v tématu [Úvod do počítače výukové na Azure][].  Můžete pak zjistěte, jak můžete vytvářet, školení, skóre a otestujte počítače výukové modelu pomocí [experimentovat kurz vytvoření][].

V tomto článku se dozvíte, jak provádět následující akce pomocí [Azure počítače výukové Studio][]:

- Načtení dat z databáze můžete vytvářet, školení a skóre prediktivní modelu
- Zápis dat do databáze


## <a name="read-data-from-sql-data-warehouse"></a>Načtení dat z SQL datový sklad

Jsme bude číst data z tabulky Product v databázi AdventureWorksDW.

### <a name="step-1"></a>Krok 1

Zahájení nového experiment kliknutím na + nový v dolní části okna počítače výukové Studio vyberte testu a pak vyberte prázdný experimentovat. Vyberte výchozí název experiment v horní části plátno a přejmenujte ho na určité akce, například na kolo cena předpovědí.

### <a name="step-2"></a>Krok 2

Vyhledejte modulů na levé straně plátno testu a modulu čtečka palety datové sady. Přetáhněte modul plátno testu.
![][drag_reader]

### <a name="step-3"></a>Krok 3

Vyberte modul čtečka a vyplňte podokna Vlastnosti.

1. Vyberte databázi Azure SQL jako zdroj dat.
2. Název databázového serveru: Zadejte název serveru. [Azure portál][] umožňuje najít takto.

![][server_name]

3. Název databáze: Zadejte název databáze na serveru jste zadali.
4. Název účtu uživatele serveru: Zadejte uživatelské jméno účtu, který má oprávnění pro databázi.
5. Hesla uživatelského účtu serveru: Zadejte heslo pro zadaný uživatelský účet.
6. Přijmout všechny certifikát serveru: Tato možnost (méně bezpečné) použijte, pokud chcete přeskočit revizí certifikát serveru před další data.
7. Databázových dotazů: Zadejte příkaz SQL, který popisuje dat si chcete přečíst. V tomto případě jsme bude číst data z tabulky Product pomocí následujícího dotazu.


```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Krok 4

1. Spusťte testu tak, že kliknete na spustit v části plátno testu.
2. Po dokončení testu modulu čtečka bude mít zelená značka zaškrtnutí tohoto políčka označíte, že byla úspěšně dokončena. Všimněte si také, dokončeno stav spuštění v pravém horním rohu.

![][run]

3. Pokud chcete zobrazit importovaná data, klikněte na výstupní port v dolní části automobilů datovou sadu a vyberte vizualizace.


## <a name="create-train-and-score-a-model"></a>Vytvoření, školení a skóre modelu

Nyní můžete pomocí této sadě dat k:

- Vytvoření modelu: zpracování dat a definovat funkce
- Školení modelu: výběr a jeho aplikování algoritmu výuka
- Počet bodů a testování modelu: předpověď nová cena na kolo


![][model]

Další informace o tom, jak vytvořit, školení, skóre a otestujte počítače výukové použití modelu [vytvořit experimentovat kurz][].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Zápis dat do datový sklad SQL Azure

Jsme zapíše sadu ProductPriceForecast tabulky v databázi AdventureWorksDW výsledků.

### <a name="step-1"></a>Krok 1

Vyhledejte zapisovací modul palety datové sady a moduly na levé straně plátno testu. Přetáhněte modul plátno testu.

![][drag_writer]

### <a name="step-2"></a>Krok 2

Vyberte zapisovací modul a vyplňte podokna Vlastnosti.

1. Vyberte databázi Azure SQL jako cíle Data.
2. Název databázového serveru: Zadejte název serveru. [Azure portál][] umožňuje najít takto.
3. Název databáze: Zadejte název databáze na serveru jste zadali.
4. Název účtu uživatele serveru: Zadejte uživatelské jméno účtu, který má oprávnění k zápisu pro databázi.
5. Hesla uživatelského účtu serveru: Zadejte heslo pro zadaný uživatelský účet.
6. Přijmout všechny certifikát serveru (zabezpečený): tuto možnost vyberte, pokud nechcete zobrazit certifikát.
7. Čárkou oddělený seznam sloupců uložit: Zadejte seznam datovou sadu nebo výsledek sloupců, které chcete vytvořit.
8. Název tabulky dat: Zadejte název tabulky dat.
9. Čárkou oddělený seznam sloupců datové tabulky: Zadejte názvy sloupců použít v nové tabulce. Názvy sloupců se může lišit od těch, které jsou v sadě zdroj dat, ale musí obsahovat stejný počet sloupců tady, které jste určili výstupní tabulce.
10. Počet řádků napsali na SQL Azure operaci: můžete nakonfigurovat počet řádků, které jsou napsali k databázi SQL najednou.

![][writer_properties]

### <a name="step-3"></a>Krok 3

1. Spuštění testu kliknutím spustit ve skupinovém rámečku plátno testu.
2. Po dokončení testu názevprojektu bude mít zelená značka zaškrtnutí označíte, že budou byla úspěšně dokončena.

## <a name="next-steps"></a>Další kroky

Další tipy vývoj najdete v tématu [Přehled vývoje SQL datový sklad][].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[Přehled vývoje SQL datový sklad]: ./sql-data-warehouse-overview-develop.md
[Vytvoření experiment kurz]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Úvod do počítače výukové na Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure počítače výukové Studio]: https://studio.azureml.net/Home
[Azure portálu]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
