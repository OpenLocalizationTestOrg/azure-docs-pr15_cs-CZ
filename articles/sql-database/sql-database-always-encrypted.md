<properties
    pageTitle="Vždy zašifrovaných: Ochrana citlivá data v databázi SQL Azure pomocí šifrování databáze | Microsoft Azure"
    description="Chrání citlivá data v databázi SQL v minutách."
    keywords="šifrování dat šifrování sql, šifrování databáze, citlivá data, vždy šifrovaná"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Vždy šifrovaná: Ochrana citlivá data do SQL databáze a uložte šifrovacího klíče úložiště certifikátů Windows

> [AZURE.SELECTOR]
- [Azure klíčové trezoru](sql-database-always-encrypted-azure-key-vault.md)
- [Úložiště certifikátů Windows](sql-database-always-encrypted.md)


V tomto článku se dozvíte, jak zajistit citlivá data v databázi SQL pomocí šifrování databáze pomocí [Vždy zašifrovaných Průvodce](https://msdn.microsoft.com/library/mt459280.aspx) v [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Je taky uvidíte, jak ukládat šifrovacího klíče v úložišti certifikátů Windows.

Vždy šifrovaný je novou technologii šifrování dat v databázi SQL Azure SQL serveru, který pomáhá chránit citlivé data na ostatních na serveru, během pohyb mezi klienta a serveru a při data, nikdy zajištění citlivá data se zobrazí ve formátu prostého textu uvnitř systému databáze. Po zašifrujete data, pouze klientské aplikace nebo aplikace servery, kteří mají přístup ke klíči přístup k dat ve formátu prostého textu. Podrobné informace najdete v tématu [Vždy zašifrovaných (databázový stroj)](https://msdn.microsoft.com/library/mt163865.aspx).

Po konfiguraci vždy šifrované databáze, vytvoříte klientské aplikace v jazyce C# aplikace Visual Studio pro práci s šifrovaná data.

Postupujte podle kroků v tomto článku se dozvíte, jak nastavit vždy šifrované databáze Azure SQL. V tomto článku se dozvíte, jak provádět následující úkoly:

- Pomocí průvodce vždy zašifrovaných v SSMS vytvořit [Vždy zašifrovaných klíče](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
    - Vytvořte [Hlavní sloupec klíč (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Vytvoření [sloupce šifrovací klíč (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Vytvořit databázovou tabulku a šifrování sloupců.
- Vytvoření aplikace, která vloží, vybere a slouží k zobrazení dat ze šifrované sloupců.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Pro účely tohoto návodu budete potřebovat:

- Účet Azure a předplatným. Pokud nemáte jeden zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) verze 13.0.700.242 nebo novější.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) nebo novější (klientského počítače).
- [Visual studia](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).



## <a name="create-a-blank-sql-database"></a>Vytvoření prázdné databáze SQL
1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na **Nový** > **dat + úložiště** > **databáze SQL**.
3. Vytvořte **prázdnou** databázi **část** s názvem nového nebo existujícího serveru. Podrobné informace o vytváření databáze na portálu Azure najdete v článku [vytvoření databáze SQL v minutách](sql-database-get-started.md).

    ![Vytvoření prázdná databáze](./media/sql-database-always-encrypted/create-database.png)

Budete potřebovat připojovací řetězec dál v tomto kurzu. Po vytvoření databáze, přejděte na novou databázi část a zkopírujte připojovací řetězec. Kdykoli můžete získat připojovací řetězec, ale není těžké si ji zkopírujte, když jste na portálu Azure.

1. Klikněte na **SQL databáze** > **část** > **Zobrazit řetězců připojení k databázi**.
2. Zkopírujte připojovací řetězec pro **ADO.NET**.

    ![Zkopírujte připojovací řetězec](./media/sql-database-always-encrypted/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Připojení k databázi s SSMS

Otevřete SSMS a připojení k serveru s databázi část.


1. Otevřete SSMS. (Klikněte na **Připojit** > **Databázový stroj** otevřete okno **připojit k serveru** , pokud není otevřené).
2. Zadejte název serveru a přihlašovací údaje. Název serveru se nachází na zásuvné SQL databáze a v připojovacím řetězci, kterou jste si zkopírovali. Zadejte úplný název serveru včetně *database.windows.net*.

    ![Zkopírujte připojovací řetězec](./media/sql-database-always-encrypted/ssms-connect.png)

Pokud se otevře okno **Nové pravidlo brány Firewall** , přihlaste se k Azure a zpřístupněte SSMS vytvořit nové pravidlo brány firewall pro vás.


## <a name="create-a-table"></a>Vytvoření tabulky

V této části vytvoříte tabulku pro uložení data pacienta. Stane se toto normální tabulka původně – nakonfigurujete šifrování v další části.

1. Rozbalte položku **databáze**.
1. Klikněte pravým tlačítkem myši databázi **část** a klikněte na **Nový dotaz**.
2. Vložte následující jazyce Transact-SQL (T-SQL) do okna nového dotazu a **spouštět** ho.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Šifrování sloupců (Konfigurovat vždy šifrovaná)

SSMS obsahuje Průvodce nastavením CMK, CEK a šifrované sloupce můžete snadno konfigurace vždy šifrovaná.

1. Rozbalte položku **databáze** > **část** > **tabulkami**.
2. Klikněte pravým tlačítkem myši tabulku **pacientů** a vyberte **Zašifrovat sloupce** otevřete průvodce vždy zašifrovaných:

    ![Šifrování sloupců](./media/sql-database-always-encrypted/encrypt-columns.png)

Průvodce vždy zašifrovaných obsahuje následující části: **Výběr sloupců**, **Hlavní klíč konfigurace** (CMK), **ověření**a **Souhrn**.

### <a name="column-selection"></a>Výběr sloupce ###

Na **úvodní** stránka a otevře se stránka **Výběru sloupce** klikněte na tlačítko **Další** . Na této stránce můžete vybrat sloupce, které chcete šifrovat, [typ šifrování a jaké sloupce šifrovací klíč (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) používat.

Šifrování **Rodné číslo** a **DatumNarození** informace pro každého pacienta. **Rodné číslo** sloupec použije deterministický šifrování, která podporuje rovnosti vyhledávání, spojení a seskupit podle. **DatumNarození** sloupec použije náhodného šifrování, která nepodporuje operace.

Nastavte **Typ šifrování** ve sloupci **Rodné číslo** **Deterministic** a **DatumNarození** sloupec **Randomized**. Klikněte na tlačítko **Další**.

![Šifrování sloupců](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Konfigurace hlavního klíče###

Na stránce **Konfigurace hlavního klíče** slouží k nastavení vašeho CMK a vyberte poskytovatele úložišti klíčů uložení CMK. V současné době můžete ukládat CMK úložiště certifikátů Windows, trezoru klíč Azure, nebo hardware zabezpečení modulu (HSM). Tento kurz ukazuje, jak ukládat své klíče v úložišti certifikátů Windows.

Zkontrolujte, jestli je vybraná **úložiště certifikátů Windows** a klikněte na tlačítko **Další**.

![Konfigurace hlavního klíče](./media/sql-database-always-encrypted/master-key-configuration.png)


### <a name="validation"></a>Ověření###

Můžete šifrování sloupce nebo uložit skriptu prostředí PowerShell spusťte později. Pro účely tohoto návodu vyberte **pokračovat dokončete teď** a klikněte na tlačítko **Další**.

### <a name="summary"></a>Souhrn###

Ověřte, zda nastavení všechny opravit a klikněte na **Dokončit** dokončete nastavení pro vždy šifrovaná.

![Souhrn](./media/sql-database-always-encrypted/summary.png)


### <a name="verify-the-wizards-actions"></a>Ověřte v Průvodci akce

Po dokončení Průvodce databáze je nastavte si vždy šifrovaná. Průvodce provést následující akce:

- Vytvoření CMK.
- Vytvoření CEK.
- Konfigurace vybranými sloupci pro šifrování. Tabulka **pacientů** aktuálně neobsahuje žádná data, ale některá data ve vybraných sloupcích jsou teď šifrovaná.

Vytvoření klíčů v SSMS můžete ověřit tak, že přejdete na **část** > **zabezpečení** > **Vždy zašifrovaných klíče**. Teď můžete vidět klíči generovaných Průvodce za vás.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Vytvoření aplikace klienta, se kterými spolupracuje šifrovaná data

Teď, když je nastavený vždy zašifrovaných, je možné vytvářet aplikace, která provádí *vloží* a k *výběru* šifrované sloupců. Úspěšně spustit aplikaci vzorku, je třeba spustit ji na stejném počítači místo, kam jste spustili průvodce vždy šifrovaná. Spustit aplikaci na jiném počítači, musí nasadíte certifikátu vždy zašifrovaných k počítači se systémem aplikaci klienta.  

> [AZURE.IMPORTANT] Aplikace musí používat [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objektů při přechodu na server s vždy šifrovaná sloupce dat ve formátu prostého textu. Předávání literálů bez použití SqlParameter objekty bude mít za následek výjimku.


1. Otevřete aplikaci Visual Studio a vytvořte nové aplikace konzoly C#. Ujistěte se, že projektu je nastavený na **.NET Framework 4.6** nebo novější.
2. Název projektu **AlwaysEncryptedConsoleApp** a klikněte na **OK**.

![Nové aplikace konzoly](./media/sql-database-always-encrypted/console-app.png)



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Úprava připojovací řetězec povolit vždy zašifrovaných

Tento oddíl vysvětluje, jak povolit vždy šifrované databáze připojovací řetězec. Budete upravovat konzoly aplikaci, kterou jste právě vytvořili v další části "Vždy zašifrovaných aplikace konzoly vzorku."


Aby vždy zašifrovaných, potřebujete přidat klíčové slovo **Nastavení šifrování sloupce** k připojovacího řetězce a jeho nastavení **povoleno**.

Je možné nastavit přímo v připojovacím řetězci nebo ho můžete nastavit pomocí [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Ukázková aplikace v následující části ukazuje, jak používat **SqlConnectionStringBuilder**.

> [AZURE.NOTE] Toto je pouze změny v klientské aplikaci konkrétní vždy šifrovaná vyžaduje. Pokud máte existující aplikace, která jsou uložená externě jeho připojovací řetězec (to znamená v souboru konfigurace), bude pravděpodobně možné povolit vždy šifrovaná beze změny jakýkoli kód.


### <a name="enable-always-encrypted-in-the-connection-string"></a>Povolení vždy šifrovaná v připojovacím řetězci

Přidejte následující klíčové slovo připojovací řetězec:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Povolit vždy zašifrovaných SqlConnectionStringBuilder

Následující kód ukazuje, jak povolit vždy šifrovaná nastavením [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [povoleno](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Vždy šifrovaný ukázková konzoly aplikace

Tento příklad ukazuje, jak:

- Úprava připojovací řetězec povolit vždy šifrovaná.
- Vložte data do šifrované sloupců.
- Vyberte záznam filtrováním určitou hodnotu ve sloupci zašifrované.

Nahraďte obsah **Program.cs** následující kód. Nahraďte připojovací řetězec proměnné globální connectionString v řádku přímo nad metodu hlavní platný připojovací řetězec z portálu Microsoft Azure. Toto je pouze změny, které bude nutné, abyste tento kód.

Spusťte aplikaci a podívejte se vždy šifrovaná funguje.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
         VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }


## <a name="verify-that-the-data-is-encrypted"></a>Ověřte, že je šifrovaná data

Rychle můžete zkontrolovat, že je tak, že dotazy na data **pacientů** s SSMS šifrovaná vlastních dat na serveru. (Použít aktuální připojení kde nastavení šifrování sloupec není aktivovány.)

Spuštěním následujícího dotazu v databázi část.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Uvidíte, že šifrované sloupce neobsahuje žádná data ve formátu prostého textu.

   ![Nové aplikace konzoly](./media/sql-database-always-encrypted/ssms-encrypted.png)


Použití SSMS pro přístup k datům ve formátu prostého textu, můžete přidat **Nastavení šifrování sloupce = povolené** parametr připojení.

1. V SSMS klikněte pravým tlačítkem na serveru v **Prohlížeči objektů**a potom klikněte na tlačítko **Odpojit**.
2. Klikněte na **Připojit** > **Databázový stroj** otevřete okno **připojit k serveru** a pak klikněte na **Možnosti**.
3. Klikněte na **Další parametry připojení** a typ **Nastavení šifrování sloupce = povolené**.

    ![Nové aplikace konzoly](./media/sql-database-always-encrypted/ssms-connection-parameter.png)

4. Spuštěním následujícího dotazu v databázi **část** .

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Nyní vidíte data ve formátu prostého textu ve sloupci zašifrované.


    ![Nové aplikace konzoly](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [AZURE.NOTE] Pokud se spojit s SSMS (nebo libovolného klienta) z jiného počítače, nebudou mít přístup k šifrovacího klíče a nebudou moct dešifrovat data.



## <a name="next-steps"></a>Další kroky
Jakmile vytvoříte databázi, která používá vždy šifrovaná, je vhodné postupujte takto:

- V tomto příkladu spusťte z jiného počítače. Nebude mít přístup k datům ve formátu prostého textu a nespustí úspěšně ho nebudou mít přístup k šifrovacího klíče.
- [Otočit a vyčistit kódu Key](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migrace dat, která je už zašifrovaných vždy šifrovaná](https://msdn.microsoft.com/library/mt621539.aspx).
- [Nasazování šifrovaná vždy certifikáty další klientské počítače](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (viz oddíl "Nastavení certifikátů k dispozici aplikace a uživatelům").

## <a name="related-information"></a>Související informace

- [Vždy šifrovaná (vývoj klienta)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Šifrování průhledné dat](https://msdn.microsoft.com/library/bb934049.aspx)
- [Šifrování serveru SQL](https://msdn.microsoft.com/library/bb510663.aspx)
- [Vždy šifrované Průvodce](https://msdn.microsoft.com/library/mt459280.aspx)
- [Vždy šifrované blogu](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
