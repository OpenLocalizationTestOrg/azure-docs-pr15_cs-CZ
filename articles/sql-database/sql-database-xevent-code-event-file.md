<properties 
    pageTitle="Soubor události XEvent kód SQL databáze | Microsoft Azure" 
    description="Příklad dvoufázový kód, který ukazuje cíl události souborů v rozšířené událost na databázi SQL Azure poskytuje prostředí PowerShell a jazyce Transact-SQL. Azure úložiště je povinná část tento scénář." 
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


# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Kód cílové soubor události rozšířené událostí v databázi SQL

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Dokončení příkladu požadovaného robustní způsob, jak informace o události rozšířené zachycení a sestavy.


V Microsoft SQL Server, [cílový soubor události](http://msdn.microsoft.com/library/ff878115.aspx) slouží k uložení výstupy událostí do souboru místní jednotku pevného disku. Ale tyto soubory ostatní uživatelé vás databáze SQL Azure. Místo toho používáme Azure úložiště služby pro podporu cílový soubor události.


Toto téma popisuje dvoufázový příkladu:


- Prostředí PowerShell vytvořit kontejner Azure úložiště v cloudu.

- Transact-SQL:
 - Přiřadit kontejneru úložišti Azure cílový soubor události.
 - Vytvoření a spuštění relace události, atd.


## <a name="prerequisites"></a>Zjistit předpoklady pro


- Účet Azure a předplatným. Můžete zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).


- Všechny databáze, můžete vytvořit tabulku.
 - Volitelně můžete [vytvořit databázi ukázkové **AdventureWorksLT** ](sql-database-get-started.md) v minutách.


- SQL Server Management Studio (ssms.exe) v ideálním případě jeho nejnovější měsíční aktualizaci verzi. Stáhněte si nejnovější ssms.exe z:
 - Článku nazvanou [Stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Přímý odkaz na stažení.](http://go.microsoft.com/fwlink/?linkid=616025)


- Musí mít [Azure PowerShell moduly](http://go.microsoft.com/?linkid=9811175) nainstalovaný.
 - Moduly poskytují příkazy například – **Nový AzureStorageAccount**.


## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Fáze 1: Prostředí PowerShell kód pro kontejner Azure úložiště


Toto prostředí PowerShell se fázi 1 ukázka dvoufázový kódu.

Skript začíná příkazy vyčištění po nejnižší předchozí spustit a je rerunnable.



1. Vložení skriptu prostředí PowerShell do editor prostého textu například Notepad.exe a skript uložte jako soubor s příponou **.ps1**.

2. Prostředí PowerShell ISE spusťte jako správce.

3. Na příkazovém řádku zadejte<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>a stiskněte klávesu Enter.

4. V prostředí PowerShell ISE otevřete soubor svojeho **.ps1** . Spusťte skript.

5. Skript prvním spuštění nové okno, ve kterém se přihlásit k Azure.
 - Spusťte skript bez přerušení relaci máte pohodlný možnost komentářích **AzureAccount přidat** příkaz.


![Prostředí PowerShell ISE s Azure modulu nainstalovaný, můžete spustit skriptu.][30_powershell_ise]


&nbsp;


```
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


&nbsp;


Tady je pár poznámek několik pojmenovaných hodnot, které skript Powershellu vytiskne, když skončí jeho platnost. Jsou tyto hodnoty musí úpravy v jazyce Transact-SQL skript, který následuje jako fázi 2.


## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Fáze 2: Transact-SQL kód, který používá kontejner Azure úložiště


- Ve fázi 1 Tato ukázka kódu jste spustili skript Powershellu pro vytvoření kontejneru Azure úložiště.
- V dalším ve fázi 2, musíte tento skript jazyce Transact-SQL pomocí kontejner.


Skript začíná příkazy vyčištění po nejnižší předchozí spustit a je rerunnable.


Skript Powershellu vytisknou několik pojmenovaných hodnot, když ho ukončit. Je třeba upravit použijte tyto hodnoty skript jazyce Transact-SQL. Najděte **úkol** v jazyce Transact-SQL skriptu přejděte na příkaz Upravit body.


1. Otevřete SQL Server Management Studio (ssms.exe).

2. Připojení k databázi SQL Azure databáze.

3. Klepnutím sem otevřete podokno nového dotazu.

4. Tento skript jazyce Transact-SQL vložte do podokna dotazu.

5. Vyhledejte každý **úkol** v skriptu a proveďte odpovídající úpravy.

6. Uložte a znovu spusťte skript.


&nbsp;


> [AZURE.WARNING] Hodnoty klíče přidružení zabezpečení generovaného při předchozím skript Powershellu může začínají "?" (otazník). Při použití klávesy přidružení zabezpečení v tento skript T-SQL musíte *Odebrat úvodní mezery "?"*. V opačném případě mohou být vaše úsilí blokovány zabezpečení.


&nbsp;


```
---- TODO: First, run the PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


&nbsp;


Pokud cíl nepodaří připojit při spuštění, musí zastavte a restartujte relaci událostí:


```
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


&nbsp;


## <a name="output"></a>Výstup


Po dokončení skriptu jazyce Transact-SQL, klikněte na buňku pod záhlavím sloupce **event_data_XML** . Jeden **<event>** prvek zobrazený, který ukazuje jeden příkaz UPDATE.

Tady je jedním **<event>** prvek, který byl vytvořen během testování:


&nbsp;


```
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

&nbsp;


Předchozí skriptů Transact-SQL používají následující funkce systému přečíst event_file:

- [Sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)


Vysvětlení rozšířené možnosti pro zobrazení dat z rozšířené události je k dispozici:

- [Rozšířené zobrazení cílové dat z rozšířené události](http://msdn.microsoft.com/library/mt752502.aspx)

&nbsp;


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Převod výběru kód spustit na serveru SQL Server


Předpokládejme, že jste chtěli spustit v předchozím příkladu jazyce Transact-SQL na Microsoft SQL Server.


- Pro zjednodušení by chcete úplně nahraďte jednoduchý soubor například **C:\myeventdata.xel**využívání kontejneru Azure úložiště. Soubor by zapsat místní jednotku pevného disku počítače, který je hostitelem SQL serveru.


- Jakýkoli druh příkazy jazyce Transact-SQL nemusí pro **Vytvoření hlavního klíče** a **Vytvoření přihlašovací údaje**.


- V příkazu **Vytvořit událost relace** klauzulí **Přidat cíl** , by nahraďte hodnotu Http přiřazené provedené **filename =** řetězcem úplnou cestu jako **C:\myfile.xel**.
 - Žádný účet Azure úložiště potřebujete zapojit.


## <a name="more-information"></a>Další informace


Další informace o účtech a kontejnery služby Azure úložiště najdete v tématu:

- [Použití úložiště objektů Blob z .NET](../storage/storage-dotnet-how-to-use-blobs.md)
- [Pojmenování a odkazování na kontejnery, objekty BLOB a Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx)
- [Práce s kontejneru kořenové](http://msdn.microsoft.com/library/azure/ee395424.aspx)
- [Lekce 1: Vytvoření zásad uložené přístupu a sdílený přístup podpis na kontejner Azure](http://msdn.microsoft.com/library/dn466430.aspx)
    - [Lekce 2: Vytvoření pověření systému SQL Server podpisem sdílený přístup](http://msdn.microsoft.com/library/dn466435.aspx)




<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

