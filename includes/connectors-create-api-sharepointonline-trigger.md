V tomto příkladu se bude vidíte, jak používat **SharePoint Online – při vytvoření nové položky** aktivační událost zahajte pracovního postupu aplikace logiky při vytvoření nové položky do seznamu Sharepointu Online.

>[AZURE.NOTE]Se zobrazuje výzva k přihlaste ke svému účtu SharePoint, pokud jste ještě nevytvořili *připojení* k Sharepointu Online.  

1. Zadejte *sharepoint* do vyhledávacího pole v Návrháři logiky aplikace a pak vyberte **SharePoint Online – při vytvoření nové položky** aktivační událost.  
![SharePoint online aktivační událost obrázek](./media/connectors-create-api-sharepointonline/trigger-1.png)  
- Ovládací prvek **Při vytvoření nové položky** se zobrazí.  
![SharePoint online aktivační událost obrázek 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
- Vyberte **adresu URL webu**. Toto je web Sharepointu online, který chcete sledovat nové položky spustit pracovní postup.  
![SharePoint online aktivační událost obrázek 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
- Vyberte **název seznamu**. Toto je seznam na webu Sharepointu Online, který chcete sledovat nové položky, které se spustí pracovní postup.  
![SharePoint online aktivační událost obrázek 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

V tomto okamžiku aplikace logiky nakonfigurované s aktivační událost, která bude začínat spustit ostatních aktivačních událostí a akce pracovního postupu. Tím se provede pokaždé, když se vytvoří novou položku v seznamu Sharepointu Online, který jste vybrali.  