#### <a name="prerequisites"></a>Zjistit předpoklady pro
- Účet Azure. můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
- Účet [Office 365](https://office365.com)  

Před použitím účtu Office 365 v aplikaci použití logických operátorů, povolte aplikaci logiky pro připojení k účtu Office 365. Lze provést jednoduše v rámci logiky aplikace na portálu Azure.  

Povolte aplikaci logiky pro připojení k účtu Office 365 pomocí následujících kroků:

1. Vytvoření aplikace logiky. V Návrháři logiky aplikace vyberte v rozevíracím seznamu **Zobrazit Microsoft spravované rozhraní API** a do vyhledávacího pole zadejte "office 365". Vyberte jednu z aktivačních událostí nebo akce:  
    ![Krok vytvoření připojení Office 365](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  

2. Pokud jste ještě nevytvořili všechna připojení k Office 365, se zobrazí výzva k přihlášení pomocí svých přihlašovacích údajů Office 365:  
    ![Krok vytvoření připojení Office 365](./media/connectors-create-api-office365-outlook/office365-signin.png)  

3. Vyberte **přihlásit**a zadejte svoje uživatelské jméno a heslo. Vyberte **přihlásit**:  
    ![Krok vytvoření připojení Office 365](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)

    Těchto přihlašovacích údajů se používají k povolit aplikaci použití logických operátorů k připojení a přístup k účtu Office 365. 

4. Všimněte si, že byl vytvořen připojení. Teď pokračujte dalšími kroky v aplikaci použití logických operátorů:   
    ![Krok vytvoření připojení Office 365](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  
  