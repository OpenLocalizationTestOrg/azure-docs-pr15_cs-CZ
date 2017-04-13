<properties
   pageTitle="Analýza dat pomocí Azure počítače výukové | Microsoft Azure"
   description="Pomocí Azure počítače učení a prediktivní počítače výukové modelu podle data uložená v Azure SQL datový sklad."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/14/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="analyze-data-with-azure-machine-learning"></a>Analýza dat pomocí výukové počítače Azure

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Výukové Azure počítače](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SqlCmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Tento kurz pomocí Azure počítače výukové vytváří prediktivní počítače výukové modelu podle data uložená v Azure SQL datový sklad. Konkrétně to vytvoří cílené marketingové kampaně pro Adventure Works pracoviště bike předpovídání Pokud zákazník je pravděpodobně koupit kolo nebo ne.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]


## <a name="prerequisites"></a>Zjistit předpoklady pro
Můžete přecházet mezi tohoto kurzu, budete potřebovat:

- Datový sklad SQL předem s ukázkovými daty AdventureWorksDW načíst. Zřízení to, najdete v tématu [Vytvoření datový sklad SQL][] a zvolte načtení ukázkových dat. Pokud už máte datový sklad, ale nebudete mít ukázková data, můžete [načtení ukázkových dat ručně][].

## <a name="1-get-data"></a>1. načtení dat
Data jsou v dbo.vTargetMail zobrazení v databázi AdventureWorksDW. Číst tyto údaje:

1. Přihlaste se k [Azure počítače výukové studio][] a klikněte na můj pokusy.
2. Klikněte na **+ Nový** a vyberte **Prázdný testu**.
3. Zadejte název vašeho experiment: určený jenom marketingové.
4. Přetáhněte modulu **čtečka** z podokna moduly do plátno.
5. Zadejte podrobnosti databáze SQL datový sklad v podokně Vlastnosti.
6. Určete databázi **dotazu** zobrazíte požadovaná data.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Spuštění testu kliknutím na tlačítko **Spustit** ve skupinovém rámečku plátno testu.
![Spuštění testu][1]


Po dokončení testu úspěšným klikněte výstupní port v dolní části modulu čtečka a vyberte **vizualizace** zobrazíte importovaná data.
![Zobrazení importovaná data][3]


## <a name="2-clean-the-data"></a>2. čištění dat
Odstranění dat, přetáhněte některé sloupce, které se nevztahují na modelu. Akce:

1. Přetáhněte modulu **Projektu sloupců** na plátno.
2. Klikněte na **volič spuštění** do podokna Vlastnosti zadejte sloupce, které chcete zrušit.
![Sloupce projektu][4]

3. Vyloučení dvou sloupců: CustomerAlternateKey a řetězec GeographyKey.
![Odebrání zbytečných sloupců][5]


## <a name="3-build-the-model"></a>3. vytvoření modelu
Jsme rozdělí data 80-20: 80 % školení počítače výukové modelu a % 20 k testování modelu. Bude používat algoritmů "Dvě třídy" pro tento problém binární klasifikace.

1. Přetáhněte modulu **Rozdělit** na plátno.
2. Zadejte 0,8 zlomek řádků v první sadě výstup dat v podokně Vlastnosti.
![Rozdělení data na školení a otestujte nastavení][6]
3. Modul **Třídy dvě zesílen rozhodovacího stromu** přetáhněte na plátno.
4. Přetáhněte modulu **Vlaku modelu** na plátno a zadejte vstupní hodnoty. Potom klikněte na **Spustit volič** v podokně Vlastnosti.
      - Nejdřív při zadávání: ML algoritmus.
      - Druhé při zadávání: Data, která chcete školení algoritmu na.
![Připojení modulu vlaku modelu][7]
5. Vyberte sloupec **BikeBuyer** jako sloupec odhadnout.
![Vyberte sloupec, který předpovídání][8]


## <a name="4-score-the-model"></a>4. model skóre
Teď můžeme testovat jak modelu provede testovací data. Jsme bude porovnávat algoritmus naše volba pomocí různých algoritmu zobrazíte, která provede půjde vám ještě snadněji.

1. Přetáhněte **Skóre modelu** modul na plátno.
    Nejdřív při zadávání: školení druhý vstupní modelu: otestujte dat ![skóre modelu][9]
2. Přetáhněte **Dvě třídy Bayes čárky počítače** do plátno testu. Budeme se slouží k porovnání tento algoritmus ve srovnání s dvěma třídy zesílen rozhodovacího stromu.
3. Kopírování a vkládání moduly vlaku Model a skóre Model plátno.
4. Přetáhněte modulu **Vyhodnocení modelu** na plátno a umožňuje porovnávat dva algoritmy.
5. **Spuštění** testu.
![Spuštění testu][10]
6. Klikněte výstupní port v dolní části modulu vyhodnocení Model a vizualizace.
![Zobrazení výsledků hodnocení][11]

Metriky k dispozici jsou ROC křivky, přesnost odvolání diagramu a výtah křivka. Tyto metriky prohlížíte, vidíme, půjde vám ještě snadněji druhý provedených v prvním modelu. Se podívat, co v prvním modelu předpovídanými, klikněte na výstupní port skóre modelu a klikněte na vizualizace.
![Zobrazení výsledků skóre][12]

Zobrazí se dvě další sloupce přidána do datovou sadu testu.

- Dosáhne pravděpodobnosti: pravděpodobnost, že je zákazník kupní bike.
- Dosáhne popisky: klasifikace provedenou modelu – bike kupní (1) nebo ne (0). Tento mezní hodnota pravděpodobnosti pro označení je nastavený na 50 % a lze upravit.

Porovnání sloupci BikeBuyer (aktuální) s popisky dosáhne (předpovědí), uvidíte, jak dobře modelu provedl. Jako další kroky můžete tento model předpovědí pro nové zákazníky a publikovat tento model jako webové služby nebo zápisu výsledků zpět SQL datový sklad.

## <a name="next-steps"></a>Další kroky

Další informace o sestavování prediktivní počítače výukové modelů najdete v nápovědě k [Úvod do počítače výukové na Azure][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure počítače výukové studio]:https://studio.azureml.net/
[Úvod do počítače výukové na Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[načtení ukázkových dat ručně]: sql-data-warehouse-load-sample-databases.md
[Vytvořit datový sklad SQL]: sql-data-warehouse-get-started-provision.md
