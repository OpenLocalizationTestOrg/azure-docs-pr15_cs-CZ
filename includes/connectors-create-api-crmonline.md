#### <a name="prerequisites"></a>Zjistit předpoklady pro
- Účet Azure. můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
- Účet [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) 

Před použitím účtu Dynamics v aplikaci použití logických operátorů, povolte aplikaci logiky pro připojení k účtu CRM Online. Lze provést jednoduše v rámci logiky aplikace na portálu Azure. 

Povolte aplikaci logiky pro připojení k účtu CRM Online pomocí následujících kroků:

1. Vytvoření aplikace logiky. V Návrháři logiky aplikace vyberte v rozevíracím seznamu **Zobrazit Microsoft spravované rozhraní API** a do vyhledávacího pole zadejte "dynamics". Vyberte jednu z aktivačních událostí nebo akce:  
  ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Pokud jste ještě nevytvořili všechna připojení k Dynamics, se zobrazí výzva k přihlášení pomocí svých přihlašovacích údajů Dynamics:  
  ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Vyberte **přihlásit**a zadejte svoje uživatelské jméno a heslo. Vyberte **přihlásit**. 

    Těchto přihlašovacích údajů se používají k povolit aplikaci použití logických operátorů k připojení a přístup k datům ve vašem účtu Dynamics. 
4. Všimněte si, že byl vytvořen připojení. Teď pokračujte dalšími kroky v aplikaci použití logických operátorů:  
  ![](./media/connectors-create-api-crmonline/dynamics-properties.png)
