<properties 
    pageTitle="Funkce Factory dat a systémových proměnných | Microsoft Azure" 
    description="Obsahuje seznam funkcí Azure Data Factory a proměnných systému" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure Data Factory – funkce a systémových proměnných
Tento článek obsahuje informace o funkcích a proměnných nepodporuje Azure Data Factory.
  
## <a name="data-factory-system-variables"></a>Data Factory systém proměnné

Název proměnné | Popis | Rozsah objektu | Rozsah JSON a případy použití
------------- | ----------- | ------------ | ------------------------
WindowStart | Začátek časový interval pro aktuální činnosti spustit okna | Aktivita | <ol><li>Určete výběr dotazy na data. Najdete v článcích spojnice odkazuje ve článku [Dat pohyb aktivity](data-factory-data-movement-activities.md) .</li><li>Předáte parametry podregistru skript (ukázka najdete ji výše).</li>
WindowEnd | Konec časový interval pro aktuální činnosti spustit okna | Aktivita | stejný jako výše
SliceStart | Začátek časový interval výseč adresami vytvořené | Aktivita<br/>datové sady | <ol><li>Zadejte názvy souborů a dynamické složky cesty při práci s [Objektů Blob Azure](data-factory-azure-blob-connector.md) a [datové sady systému souborů](data-factory-onprem-file-system-connector.md).</li><li>Zadejte vstupní závislosti se funkce factory dat v kolekci vstupů aktivity.</li></ol>
SliceEnd | Konec časový interval pro aktuální výseč adresami vytvořené | Aktivita<br/>datové sady | stejně jako nahoře. 

> [AZURE.NOTE] Aktuálně factory dat vyžaduje plán, podle aktivity přesně odpovídají plánu podle dostupnosti datovou sadu výstup. To znamená, že WindowStart WindowEnd a SliceStart a SliceEnd vždy mapování na stejné časového období a jeden výstup výsečí.
 
## <a name="data-factory-functions"></a>Funkce Factory dat 

Funkce v factory data spolu s výše uvedenými systémové proměnné slouží k těmto účelům:

1.  Zadání dat výběru dotazů (najdete v článcích spojnice odkazováno článek [Dat pohyb aktivity](data-factory-data-movement-activities.md) .

    Syntaxe pro vyvolání funkce factory dat: ** $$ ** pro výběr dotazy na data a další vlastnosti aktivity a datové sady.  
2. Určení vstupní závislosti pomocí funkce factory dat v aktivitě vstupů kolekce (viz výše uvedené ukázkové).

    $ není potřeba určete vstupní závislost výrazů.   

V následujícím příkladu je vlastnost **sqlReaderQuery** v souboru JSON přiřazen k hodnoty vrácené funkcí **Text.Format** . V tomto příkladu taky používá proměnnou systému s názvem **WindowStart**, což představuje čas zahájení aktivity spustit okno.
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>Funkce

Následující tabulka uvádí všechny funkce Azure Data Factory:

Kategorie | (Funkce) | Parametry | Popis
-------- | -------- | ---------- | ----------- 
Čas | AddHours(X,Y) | X: data a času <br/><br/>Y: funkce int | Přidá hodiny Y k dané čas X. <br/><br/>Příklad: 9/5/2013 12:00:00 odp + 2 hodin = 9/5/2013 2:00:00 odp.
Čas | AddMinutes(X,Y) | X: data a času <br/><br/>Y: funkce int | Přidá Y minut x.<br/><br/>Příklad: 9/15/2013 12:00:00 odp + 15 minut = 9/15/2013 12:15:00 odp.
Čas | StartOfHour(X) | X: data a času | Získá počáteční čas hodiny představované hodinu součástí X. <br/><br/>Příklad: StartOfHour 9/15/2013 05:10:23 PM je 9/15/2013 05:00:00 odp.
Datum | AddDays(X,Y) | X: data a času<br/><br/>Y: funkce int | Přidá Y dnů x.<br/><br/>Příklad: 9/15/2013 12:00:00 odp + 2 dnů = 9/17/2013 12:00:00 odp.
Datum | AddMonths(X,Y) | X: data a času<br/><br/>Y: funkce int | Přidá Y měsíců x.<br/><br/>Příklad: 9/15/2013 12:00:00 odp + 1 měsíc = 10/15/2013 12:00:00 odp. 
Datum | AddQuarters(X,Y) | X: data a času <br/><br/>Y: funkce int | Přidá Y * 3 měsíce x.<br/><br/>Příklad: 9/15/2013 12:00:00 odp + 1 čtvrtletí = 12/15/2013 12:00:00 odp.
Datum | AddWeeks(X,Y) | X: data a času<br/><br/>Y: funkce int | Přidá Y * 7 dnů x<br/><br/>Příklad: 9/15/2013 12:00:00 odp + 1 týden = 9/22/2013 12:00:00 odp.
Datum | AddYears(X,Y) | X: data a času<br/><br/>Y: funkce int | Přidá Y let x.<br/><br/>Příklad: 9/15/2013 12:00:00 odp + 1 rok = 9/15/2014 12:00:00 odp.
Datum | Day(X) | X: data a času | Získá den součástí X.<br/><br/>Příklad: Den 9/15/2013 12:00:00 odp je 9. 
Datum | DayOfWeek(X) | X: data a času | Získá den týdne component x.<br/><br/>Příklad: DayOfWeek 9/15/2013 je neděle 12:00:00 odp.
Datum | DayOfYear(X) | X: data a času | Získá na den roku představovanou rok součástí X.<br/><br/>Příklady:<br/>12/1/2015: den 335 2015<br/>31/12/2015: den 365 2015<br/>31/12/2016: den 366 2016 (přestupných rok)
Datum | DaysInMonth(X) | X: data a času | Získá dny v měsíci představované měsíc součástí parametr X.<br/><br/>Příklad: DaysInMonth 9/15/2013 jsou 30, protože jsou během dne měsíce 30 dní.
Datum | EndOfDay(X) | X: data a času | Načte data a času, které představuje dokončení dne (součást den) X.<br/><br/>Příklad: EndOfDay 9/15/2013 05:10:23 PM je 9/15/2013 11:59:59 odp.
Datum | EndOfMonth(X) | X: data a času | Získá Konec měsíce představované měsíc součástí parametr X. <br/><br/>Příklad: EndOfMonth 9/15/2013 05:10:23 PM je 9/30/2013 11:59:59 odp (datum čas, který představuje konce dne měsíce)
Datum | StartOfDay(X) | X: data a času | Získá počáteční den představované den součástí parametr X.<br/><br/>Příklad: StartOfDay 9/15/2013 05:10:23 PM je 9/15/2013 12:00:00 dop.
Data a času | From(X) | X: řetězec | Analyzovat řetězec X datum čas.
Data a času | Ticks(X) | X: data a času | Získá značky vlastnost parametru X. Jedna značka – výsledek je 100 nanosekundách. Hodnota této vlastnosti představuje počet značek, které uplynuly od půlnoci 12:00:00, 1. ledna 0001. 
Text | Format(X) | X: proměnné řetězec | Formáty text.

#### <a name="textformat-example"></a>Příklad Text.Format

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

Přečtěte si téma [vlastní datum a čas formátovací řetězce](https://msdn.microsoft.com/library/8kb3ddd4.aspx) téma popisuje různé možnosti formátování můžete použít (například: RR porovnání rrrr). 

> [AZURE.NOTE] Při použití funkce do jiné funkce, se nemusí používat **$$** předpony pro funkci vnitřní. Příklad: $$Text.Format ("PartitionKey eq \\" my_pkey_filter_value\\"a stránku RowKey \\" {0:yyyy-MM-dd hh: mm:}\\", Time.AddHours (SliceStart -6)). V tomto příkladu Všimněte si, že **$$** předponu nepoužívá pro funkci **Time.AddHours** . 
  

