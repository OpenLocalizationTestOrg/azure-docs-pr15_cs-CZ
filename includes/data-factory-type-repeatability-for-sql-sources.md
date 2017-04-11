## <a name="repeatability-during-copy"></a>Opakovatelnosti během kopie

Při kopírování dat Azure SQL nebo SQL Server data z jiných ukládá třeba opakovatelnosti mějte na paměti, abyste se vyhnuli před nechtěným výsledky. 

Při kopírování dat do databáze serveru SQL/SQL Azure, ve výchozím nastavení připojit k tabulce jímky uvedenou množinu dat ve výchozím nastavení se kopie aktivity. Při kopírování dat ze zdroje soubor CSV (dat hodnoty oddělené čárkou) obsahující dva záznamy k databázi serveru SQL/SQL Azure, to je například vypadá v tabulce:
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

Předpokládejme nalezeny chyby ve zdrojovém souboru a aktualizované množství dolů trubice 2 až 4 ve zdrojovém souboru. Pokud pro dané období opětovným spuštěním výseč, najdete dva nové záznamy přidaným k databázi serveru SQL nebo SQL Azure. Níže předpokládá žádný ze sloupců v tabulce mít omezení primárního klíče.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Chcete-li této situaci předejít, budete muset zadat UPSERT sémantiku využívání některého nižší než 2 mechanismy uvedeno dále.

> [AZURE.NOTE] Řez lze znovu spustit automaticky v Azure Data Factory podle určeny zásady opakovat.

### <a name="mechanism-1"></a>Mechanismus 1

Můžete využít **sqlWriterCleanupScript** vlastnost nejdřív provádět akce čištění při spuštění řez. 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

Skript vyčištění bude spuštěn první během kopie pro dané výseč, které by odstranit data z tabulky SQL odpovídající této výsečí. Aktivity následně vložte data do tabulky SQL. 

Jestliže řez je teď znovu spustit, bude budou nalezeny že množství se aktualizuje jako žádoucí.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Předpokládejme, že záznam ploché ostřikovač se odebere z původního souboru csv. Opětovným spuštěním řez vytvoří následující výsledek: 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

Nic nového dosáhl zbývá dokončit. Aktivity kopírovat spustil vyčištění skript k odstranění odpovídající data pro tuto řez. Číst vstupní ze souboru csv (což je obsaženy jenom 1 záznam) a vložit ho do tabulky. 

### <a name="mechanism-2"></a>Mechanismus 2
> [AZURE.IMPORTANT] sliceIdentifierColumnName není podporována pro Azure SQL datový sklad v současné době. 

Jiný mechanismus k dosažení opakovatelnosti je tím, že vyhrazené sloupce (**sliceIdentifierColumnName**) v cílové tabulce. V tomto sloupci byste měli používat Azure Data Factory zajistit že zdrojová i cílová zachová synchronizaci. Tento postup lze použít po pružnost při změně nebo definující cíle schéma tabulka serveru SQL. 

Tento sloupec použije Azure Data Factory pro účely opakovatelnosti a v procesu Azure Data Factory nebudou žádné změny schématu v tabulce. Způsob, jak tuto možnost používat:

1.  Definování sloupců binárního typu (32) v cílové tabulce SQL. By měl být bez omezení podle tohoto sloupce. Pojďme název tohoto sloupce jako "ColumnForADFuseOnly" v tomto příkladu.
2.  Použijte ji kopírovat aktivity následujícím způsobem:

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure Data Factory naplní tento sloupec podle svých potřeb zajistit že zdrojová i cílová zachová synchronizaci. Hodnoty v tomto sloupci není vhodné používat mimo kontext uživatelem. 

Podobně jako mechanismus 1 kopírovat aktivity bude automaticky nejdřív vyčistit data pro dané výsečí v cílové tabulce SQL a spusťte aktivity kopírovat obvykle chcete vložit data ze zdroje do cíle pro tuto řez. 
