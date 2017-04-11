<properties 
   pageTitle="Použití zobrazení spuštění vrchol obrazce v nástrojích jezera dat for Visual Studio | Microsoft Azure" 
   description="Zjistěte, jak můžete pomocí zobrazení spuštění vrcholu k projektům analýzy dat jezera zkušebních." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/13/2016"
   ms.author="jgao"/>

# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Použití na vrcholu spuštění zobrazit v Data jezera Tools for Visual Studio

Zjistěte, jak můžete pomocí zobrazení spuštění vrchol na zkoušku analýzy dat jezera projekty.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Některé základní znalost pomocí nástroje jezera datového Visual Studio se dají skript U SQL.  V tématu [kurz: vývoj U SQL skriptů pomocí Data jezera Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Otevřete zobrazení spuštění vrchol obrazce

Pro určitou práci kliknete na odkaz "Vrchol spuštění zobrazení" v levém dolním. Můžete být vyzváni k první načtení profily a může trvat dlouho v závislosti na připojení k síti.

![Analýzy dat jezera nástroje zobrazení spuštění vrchol obrazce](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Principy zobrazení spuštění vrchol obrazce

Po zadání zobrazení spuštění vrchol obrazce, existují tři části:

![Analýzy dat jezera nástroje zobrazení spuštění vrchol obrazce](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

- Výběr vrchol: na levé straně je voliče vrchol obrazce.  Můžete zvolit vrcholy funkcemi (například prvních 10 dat přečíst nebo si vyberte podle fáze).

    Jednou z nejběžnější použité filtry je vrcholy na kritické cestě. Kritická cesta je delší cestu úlohu U SQL. Je vhodné pro optimalizaci úlohami kontrolou, které vrchol trvá delší dobu.

- Začátek středovém podokně:

    ![Analýzy dat jezera nástroje zobrazení spuštění vrchol obrazce](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

    Toto zobrazení ukazuje také pracovního stav všech vrcholy. Převede čas proto do místního počítače a zobrazí různé stavy v různých barev.

- Středovém podokně dolní:

    ![Analýzy dat jezera nástroje zobrazení spuštění vrchol obrazce](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

    - Proces název: Název instance vrchol obrazce. Se skládá z různých částí v StageName | VertexName | VertexRunInstance. Například vrchol [62] .v1 SV7_Split zastupuje druhý instanci (.v1, index od 0) vrchol čísla 62 ve fázi SV7_Split.
    - Celková dat pro čtení a zápis: Byla data pro čtení a který napsal tento vrchol obrazce.
    - Stav stát/konec: Konečný stav po ukončení na vrchol obrazce.
    - Typ kód nebo selhání konec: Chyby na vrchol obrazce se nezdařila.
    - Vytvoření důvod: Proč vrchol byl vytvořen.
    - Zdroje latence/proces latence/ČD fronty latence: doba, za vrchol potřeba počkat, zdrojů, zpracuje dat a dodržovat předpisy ve frontě.
    - Identifikátor GUID proces/poznámkové bloky pro školy: Identifikátor GUID aktuálního pracovního vrchol nebo jeho poznámkové bloky pro školy.
    - Verze: N-tého instanci pracovního vrchol (systém naplánovat novými instancemi vrchol pro mnoho důvodů, třeba převzetí výpočet redundance atd.)
    - Verze vytvoří čas.
    - Zpracování vytvořit počáteční čas/proces ve frontě čas/proces Start čas/proces dokončeno čas: při vytvoření, spuštění procesu vrchol obrazce zahájení procesu vrchol obrazce do fronty; zahájení procesu určitých vrchol obrazce, Po dokončení určitých vrchol obrazce.

## <a name="next-steps"></a>Další kroky

- Získejte přehled o jezera analýzy dat, najdete v článku [Přehled analýzy jezera dat Azure](data-lake-analytics-overview.md).
- Začínáme s vývojem aplikací U SQL, najdete v článku [pomocí nástroje jezera datového Visual Studio skripty vyvíjet U-SQL](data-lake-analytics-data-lake-tools-get-started.md).
- Další U SQL najdete v tématu [Začínáme s jazykem dat Azure jezera analýzy U-SQL](data-lake-analytics-u-sql-get-started.md).
- Úlohy správy najdete v článku [Správa jezera analýzy dat Azure Azure portálu](data-lake-analytics-manage-use-portal.md).
- Protokolování diagnostiky informací, najdete v článku [protokolech diagnostiky přístup k jezera analýzy dat Azure](data-lake-analytics-diagnostic-logs.md)
- Složitější dotaz najdete v tématu [protokoly analyzovat webu pomocí analýzy jezera dat Azure](data-lake-analytics-analyze-weblogs.md).
- Zobrazit podrobnosti projektu, najdete v článku [použití úlohy prohlížeče a zobrazení projektu pro úlohy jezera analýzy dat Azure](data-lake-analytics-data-lake-tools-view-jobs.md)