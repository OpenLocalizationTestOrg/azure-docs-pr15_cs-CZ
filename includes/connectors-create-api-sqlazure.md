### <a name="prerequisites"></a>Zjistit předpoklady pro
- Účet Azure. můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
- Aplikace [Databáze SQL Azure](../articles/sql-database/sql-database-get-started.md) pomocí své informace o připojení, včetně název serveru, název databáze a uživatelského jména a hesla. Tyto informace jsou obsaženy v připojovacím řetězci databáze SQL:
  
    Server = tcp:*yoursqlservername*. database.windows.net,1433;Initial katalogu =*yourqldbname*; Zachování informace o zabezpečení = NEPRAVDA. ID uživatele = {your_username}; Heslo = {your_password}; Atribut MultipleActiveResultSets = NEPRAVDA. Šifrování = PRAVDA a naopak. TrustServerCertificate = NEPRAVDA. Časový limit připojení = 30.

    Další informace o [Databázích SQL Azure](https://azure.microsoft.com/services/sql-database).

> [AZURE.NOTE] Když vytvoříte databázi SQL Azure, můžete taky vytvořit databáze ukázkové zahrnuté v aplikaci SQL. 



Před použitím databáze SQL Azure v aplikaci použití logických operátorů, připojení k databázi SQL. Lze provést jednoduše v rámci logiky aplikace na portálu Azure.  

Připojení k databázi SQL Azure pomocí následujících kroků:  

1. Vytvoření aplikace logiky. V Návrháři logiky aplikace přidat aktivační událost a pak přidejte akce. V rozevíracím seznamu vyberte **zobrazení Microsoft spravované rozhraní API** a do vyhledávacího pole zadejte "sql". Vyberte jednu z akcí:  

    ![Krok vytvoření připojení databáze SQL Azure](./media/connectors-create-api-sqlazure/sql-actions.png)

2. Pokud jste ještě nevytvořili všechna připojení k databázi SQL, zobrazí se výzva k podrobnosti o připojení:  

    ![Krok vytvoření připojení databáze SQL Azure](./media/connectors-create-api-sqlazure/connection-details.png) 

3. Zadejte podrobnosti o SQL databázi. Vlastnosti hvězdičkou jsou potřeba.

    | Vlastnost | Podrobnosti |
|---|---|
| Připojení přes brány | Nechte toto zrušené zaškrtnutí políčka. Používá se při připojení k místní SQL serveru. |
| Název připojení * | Zadejte libovolnou název připojení. | 
| Název serveru SQL Server * | Zadejte název serveru; což je něco jako *servername.database.windows.net*. Název serveru je zobrazena ve vlastnostech databáze SQL Azure portálu a také zobrazí v připojovacím řetězci. | 
| Název databáze SQL * | Zadejte název jste přiřadili databázi SQL. To je uvedený ve vlastnostech SQL databáze v připojovacím řetězci: počátečního katalogu =*yoursqldbname*. | 
| Uživatelské jméno * | Zadejte uživatelské jméno, které jste vytvořili při vytvoření databáze SQL. To je uveden ve vlastnosti databáze SQL Azure portálu. | 
| Heslo * | Zadejte heslo, které jste vytvořili při vytvoření databáze SQL. | 

    Těchto přihlašovacích údajů se používají k povolit aplikaci použití logických operátorů k připojení a přístup k datům na SQL. Po dokončení podrobnosti o připojení vypadat takto:  

    ![Krok vytvoření připojení databáze SQL Azure](./media/connectors-create-api-sqlazure/sample-connection.png) 

4. Vyberte možnost **vytvořit**. 

5. Všimněte si, že byl vytvořen připojení. Teď pokračujte dalšími kroky v aplikaci použití logických operátorů: 

    ![Krok vytvoření připojení databáze SQL Azure](./media/connectors-create-api-sqlazure/table.png)