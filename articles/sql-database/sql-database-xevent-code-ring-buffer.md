<properties 
    pageTitle="XEvent vyrovnávací kód SQL databáze | Microsoft Azure" 
    description="Obsahuje ukázky kód jazyce Transact-SQL, která je nastavená jako snadno a rychle pomocí vyrovnávací cílové databáze SQL Azure." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Vyzvání rezervy představuje cíl kód pro rozšířený události v databázi SQL

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Chcete úplného příkladu kódu pro snadné nejjednodušší zachycení a sestavy informace rozšířené události při zkoušce. Nejjednodušší cílové hodnotě data rozšířené události je [cílový vyrovnávací](http://msdn.microsoft.com/library/ff878182.aspx).


Toto téma popisuje ukázka kódu jazyce Transact-SQL, který:


1. Vytvoří tabulku s daty, která ukazuje s.

2. Vytvoří relace s existující rozšířené události, a to **sqlserver.sql_statement_starting**.
    - Událost se omezí na příkazy SQL, které obsahují určitý řetězec aktualizace: **údajů jako "aktualizace tabEmployee %"**.
    - Vybere odešlete cíl typu vyrovnávací, a to **package0.ring_buffer**výstupu události.

3. Spuštění relace událostí.

4. Problémy několik jednoduchých příkazy SQL aktualizace.

5. Problémy SQL vyberte k načtení akci výstup z vyrovnávací.
    - jsou spojeny **Sys.dm_xe_database_session_targets** a dalších Správa dynamických zobrazení (DMVs).

6. Ukončí relaci události.

7. Vynechává vyrovnávací cíl uvolnění jeho prostředků.

8. Vynechává relace událostí a tabulce ukázku.


## <a name="prerequisites"></a>Zjistit předpoklady pro


- Účet Azure a předplatným. Můžete zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).


- Všechny databáze, můžete vytvořit tabulku.
 - Volitelně můžete [vytvořit databázi ukázkové **AdventureWorksLT** ](sql-database-get-started.md) v minutách.


- SQL Server Management Studio (ssms.exe) v ideálním případě jeho nejnovější měsíční aktualizaci verzi. Stáhněte si nejnovější ssms.exe z:
 - Článku nazvanou [Stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Přímý odkaz na stažení.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Ukázka kódu


Velmi menší úprav v následujícím příkladu kód vyrovnávací poběží na databázi SQL Azure nebo Microsoft SQL Server. Rozdíl je stavu uzel "_databáze" název některých Správa dynamických zobrazení (DMVs) použitý v klauzuli FROM v kroku 5. Příklad:

- Sys.dm_xe**_databáze**_session_targets
- Sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Obsah vyrovnávací paměť vyzvánění


Použijeme ssms.exe ke spuštění ukázka kódu.


Aby se zobrazily výsledky, jsme kliknutí na buňku v části záhlaví sloupce **target_data_XML**.

Pak v podokně výsledků jsme kliknutí na buňku v části záhlaví sloupce **target_data_XML**. Klepněte na tlačítko vytvořit jinou kartu Soubor v ssms.exe ve kterém byl zobrazen obsah výsledné buňky, ve formátu XML.


Výstup se zobrazují v následující blok. Vypadá dlouho, ale lepší je právě dvě **<event>** prvky.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Uvolnění prostředků uskutečňuje vyrovnávací paměť vyzvánění


Po dokončení s vyrovnávací paměť vyzvánět, můžete ho odebrat a uvolnit jeho prostředky vydání **změnit** třeba takto:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Definice události cvičení je aktualizovat, ale není nezobrazí. Později můžete přidat další instanci vyrovnávací relace událostí:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Další informace


Primární téma rozšířené událostí v databázi SQL Azure je:


- [Rozšířený aspektech událostí v databázi SQL](sql-database-xevent-db-diff-from-svr.md), který porovnání některé aspekty rozšířené události, které se liší od databáze SQL Azure versus Microsoft SQL Server.


Další témata kód vzorku rozšířený události jsou k dispozici následující odkazy. Však je třeba zkontrolovat pravidelně kterémkoliv vzorku zobrazíte, zda vzorku zaměřuje Microsoft SQL Server a databázi SQL Azure. Pak můžete se rozhodnout, jestli jsou menší změny potřebné ke spuštění vzorku.


- Ukázka kódu databáze SQL Azure: [kódu cílové soubor události rozšířené událostí v databázi SQL](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
