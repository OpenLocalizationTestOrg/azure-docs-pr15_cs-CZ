## <a name="repeatability-during-copy"></a>Opakovatelnosti během kopie

Při kopírování dat od a do relační úložiště, budete muset opakovatelnosti mějte na paměti, abyste se vyhnuli před nechtěným výsledky. 

Řez může být znovu spustit automaticky v Azure Data Factory podle určeny zásady opakování. Doporučujeme nastavit opakování zásady ochrany před přechodná k chybám. Proto opakovatelnosti je důležitým aspektem starat o během přesun dat. 

**Když chcete jako zdroj:**

> [AZURE.NOTE] Následující příklady jsou pro Azure SQL, ale platí pro všechny úložiště dat, který podporuje obdélníkový datové sady. Možná bude potřeba nastavit **Typ** zdroje a vlastnosti **dotazu** (například: dotazu místo sqlReaderQuery) data obsahují.   

Obvykle při čtení z relační úložiště, je vhodné zobrazíte jen tu část dat odpovídající této výsečí. Způsob, jak to bude pomocí WindowStart a WindowEnd proměnných k dispozici v Azure Data Factory. Přečtěte si o proměnné a funkcí v Azure Data Factory v článku [plánování a provádění](../articles/data-factory/data-factory-scheduling-and-execution.md) . Příklad: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

Tento dotaz načte data z 'tabulka"určitém v oblasti výseč doby trvání. Znovu spustit tento výseč také vždy zajistí toto chování. 

V ostatních případech se můžete chtít přečíst celou tabulku (Předpokládejme pro jednorázové přesunutí pouze) a může sqlReaderQuery definovat následujícím způsobem:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
