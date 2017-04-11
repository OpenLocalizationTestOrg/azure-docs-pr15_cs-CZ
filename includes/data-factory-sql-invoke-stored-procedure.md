## <a name="invoking-stored-procedure-for-sql-sink"></a>Vyvolání uložené procedury pro dřez SQL

Při kopírování dat do databáze serveru SQL/SQL Azure nebo SQL Server, uživatel zadat uložená procedura může být nakonfigurované a vyvolání další parametry. 

Uložená procedura můžete využít při integrovanou mechanismy neslouží účel. Obvykle to je využít při extra zpracování (sloučení sloupců vyhledaná další hodnoty, vložení do více tabulek …) je potřeba provést před poslední vložení zdrojová data do cílové tabulky. 

Uložená procedura volba může vyvolat. Následující příklad ukazuje, jak pomocí uložené procedury můžete provádět jednoduché kurzor do tabulky v databázi. 

**Výstup datové sady**

V tomto příkladu typ je nastavený: SqlServerTable. Nastavte AzureSqlTable pomocí služby databáze Azure SQL. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
Definujte části SqlSink kopírovat činnosti JSON následujícím způsobem. Volání uložené procedury při vkládání dat, jsou potřebné SqlWriterStoredProcedureName a SqlWriterTableType vlastnosti.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

V databázi definujte uložená procedura se stejným názvem jako SqlWriterStoredProcedureName. Zpracuje vstupní data ze zadaného zdroje a vložit do výstupu tabulky. Všimněte si, že název parametru uložené procedury by měl být stejný jako název tabulky definice v souboru JSON tabulky.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

V databázi Definujte typ tabulka se stejným názvem jako SqlWriterTableType. Všimněte si, že schéma typ tabulky by měl být stejný jako schématu vrácené vstupní data.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

Funkce Uložená procedura využívá [Table-Valued parametry](https://msdn.microsoft.com/library/bb675163.aspx).