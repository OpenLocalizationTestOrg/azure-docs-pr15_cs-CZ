Teď, když jste přidali aktivační událost, dostanete zajímavý její čas něco udělat s daty, která je generováno aplikací aktivační událost. Tímto postupem přidání **Služby SharePoint Online – vytvoření souboru** akce. Tato akce vytvoří soubor ve službě SharePoint Online pokaždé, když se aktivuje novou položku aktivační událost. 

Konfigurace tento akce, budete muset zadat následující informace. Jak zadejte tyto informace zjistíte, že je snadno se použije data generovaná aktivační událost jako vstup pro některé vlastnosti nového souboru:

|Vytvoření vlastnosti souboru|Popis|
|---|---|
|Adresa URL webu|Toto je adresa URL webu Sharepointu Online, kde chcete vytvořit nový soubor. Vyberte ze seznamu prezentovat Web.|
|Cestu ke složce|Toto je složka (adrese URL webu vybrané v předchozím kroku) umístění nový soubor. Vyhledejte a vyberte složku.|
|Název souboru|Toto je název souboru vzniku.|
|Obsah souboru|Obsah, který se měly zapisovat do souboru.|

1. Vyberte **+ nový krok** přidat akci.  
![SharePoint online akce obrázek 1](./media/connectors-create-api-sharepointonline/action-1.png)  
- Vyberte odkaz **Přidat akci** . Toto se otevře, ke které chcete vyhledávací pole, kde můžete vyhledávat všech akcí můžete udělat. V tomto příkladu akce služby SharePoint jsou potřebné.    
![SharePoint online akce obrázek 2](./media/connectors-create-api-sharepointonline/action-2.png)    
- Zadejte *sharepoint* a vyhledávat související s SharePoint akce.
- Vyberte **SharePoint Online – vytvoření souboru** jako akce.   **Poznámka**: Zobrazí se výzva k ověření aplikace logiky pro přístup k účtu služby SharePoint, pokud jste připojení k Sharepointu Online dříve nevytvořili.    
![SharePoint online akce obrázek 3](./media/connectors-create-api-sharepointonline/action-3.png)    
- Ovládací prvek **vytvořit soubor** otevře.   
![SharePoint online akce obrázek 4](./media/connectors-create-api-sharepointonline/action-4.png)     
- Vyberte **Adresu URL webu** a vyhledejte na webu, kde chcete vytvořit soubor.     
![SharePoint online akce obrázek 5](./media/connectors-create-api-sharepointonline/action-5.png)  
- Vyberte **cestu ke složce** a vyhledejte složku umístění nový soubor.  
![SharePoint online akce obrázek 6](./media/connectors-create-api-sharepointonline/action-6.png)  
- Vyberte ovládací prvek **název souboru** a zadejte název souboru, který chcete vytvořit. Tady můžete zadat název souboru přímo nebo můžete použít kteroukoli z vlastností z aktivační událost, kterou jste dříve vytvořili. Můžete to udělat tak, že vyberete vlastnosti ze seznamu **výstupy ze kdy se vytvoří novou položku**. Tady je pouze zobrazení po vyberte ovládací prvek **název souboru** . V tomto walkthough můžu zúžený ID (ID novou položku seznamu) název souboru vzniku **SharePoint Online – vytvoření souboru** akcí.    
![SharePoint online akce obrázek 7](./media/connectors-create-api-sharepointonline/action-7.png)  
- Vyberte ovládací prvek **obsahu souboru** a zadejte požadovaný obsah vzniku soubor, který bude vytvořen. Obsah souboru Všimněte si můžete použít kteroukoli z vlastností z aktivační událost, kterou jste dříve vytvořili. V seznamu prezentovány jednoduše vyberte vlastnosti. Můžete taky můžete zadat text **obsah souboru** přímo do ovládacího prvku. V tomto příkladu můžu vybrali některé vlastnosti a přidají mezery a pomlčku jednotlivých vlastností.        
![SharePoint online akce obrázek 8](./media/connectors-create-api-sharepointonline/action-8.png)  
- Uložení změn do pracovního postupu  
- Gratulujeme, teď máte plně funkční použití logických operátorů aplikace, který se spustí při přidání nové položky do seznamu Sharepointu Online. Aplikace vytvoří soubor pomocí některé vlastnosti z novou položku seznamu.  Teď se můžete otestovat tak, že vytvoříte novou položku v seznamu služby SharePoint. 
