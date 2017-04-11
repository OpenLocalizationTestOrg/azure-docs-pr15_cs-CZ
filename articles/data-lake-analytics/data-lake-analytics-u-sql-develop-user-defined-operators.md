<properties 
   pageTitle="Můžete vyvíjet operátory definované uživatelem U SQL Azure dat jezera analýzy úloh | Azure" 
   description="Zjistěte, jak se dají uživatelem definované operátory používají a opakovaně použít v analýzy dat jezera úlohy. " 
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


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>Můžete vyvíjet operátory definované uživatelem U SQL Azure dat jezera analýzy úloh

Zjistěte, jak se dají uživatelem definované operátory používají a opakovaně použít v analýzy dat jezera úlohy. Naučíte se vlastní operátor převést země názvy.

##<a name="prerequisites"></a>Zjistit předpoklady pro

- Visual Studio 2015, Visual Studio 2013 aktualizovat 4 nebo Visual Studio 2012 nainstalované Visual C++ 
- Microsoft Azure SDK pro .NET verze 2,5 nebo vyšší.  Instalace pomocí webové platformy.
- Analýzy dat jezera účet.  V tématu [Začínáme s Azure dat jezera technologie pro analýzu portálu Azure](data-lake-analytics-get-started-portal.md).
- Absolvovat kurz [Seznámení s Studio U SQL Azure dat jezera analýzy](data-lake-analytics-u-sql-get-started.md) .
- Připojení k Azure najdete v článku [Začínáme s Studio U SQL Azure dat jezera analýzy](data-lake-analytics-u-sql-get-started.md#connect-to-azure). 
- Nahrání zdrojových dat najdete v článku [Začínáme s Studio U SQL Azure dat jezera analýzy](data-lake-analytics-u-sql-get-started.md#upload-source-data-files). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Definice a použití operátoru definované uživatelem v U SQL

**Vytvoření a odeslání úlohy U SQL** 

1. V nabídce **soubor** klikněte na **Nový**a potom klikněte na **projekt**.
2. Vyberte typ **Projektu U-SQL** .

    ![Nový projekt Visual Studio U SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klikněte na **OK**. Visual studio vytvoří řešení se souborem Script.usql.
4. V **Okně Průzkumník**rozbalte Script.usql a potom poklepejte **Script.usql.cs**.
5. Vložte tento kód do souboru:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. Otevřete Script.usql a vložte následující skript U SQL:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. V **Okně Průzkumník** **Script.usql**klikněte pravým tlačítkem a pak klikněte na **Vytvořit index**.
6. V **Okně Průzkumník** **Script.usql**klikněte pravým tlačítkem a pak klikněte na **Odeslat index**.
7. Pokud ještě připojit k předplatnému Azure, bude výzva k zadání přihlašovacích údajů účet Azure.
7. Klikněte na **Odeslat**. Odeslání výsledků a odkaz úlohy jsou k dispozici v okně Výsledky dokončení podávání.
8. Musí kliknutím na tlačítko Aktualizovat v tématu nejnovější stavu úlohy a aktualizace obrazovky.

**Chcete-li zobrazit výstup projektu**

1. V systému Windows **Server Explorer**rozbalte **Azure**, rozbalte **Jezera analýzy dat**, rozbalte účtu jezera analýzy dat, rozbalte **Úložiště účty**, klikněte pravým tlačítkem myši výchozí úložiště a klepněte na položku **Průzkumník**. 
2. Rozbalení ukázky, rozbalte výstupy a potom poklepejte **ovladače.csv**.


##<a name="see-also"></a>Viz taky

- [Začínáme s jezera analýzy dat pomocí prostředí PowerShell](data-lake-analytics-get-started-powershell.md)
- [Začínáme s jezera analýzy dat na portálu Azure](data-lake-analytics-get-started-portal.md)
- [Nástroje pro Data jezera for Visual Studio pro vývoj aplikací U SQL](data-lake-analytics-data-lake-tools-get-started.md)
