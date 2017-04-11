V tomto walk-through se naučíte použijte aktivační událost **služby Salesforce – vytvoření objektu** zahajte pracovního postupu aplikace logiky při vytváření nové zájemce vaše služby Salesforce.

>[AZURE.NOTE]Se zobrazuje výzva k k účtu služby Salesforce přihlásit, když jste ještě nevytvořili *připojení* k služby Salesforce.  

1. Zadejte *služby salesforce* do vyhledávacího pole v Návrháři logiky aplikace a pak vyberte aktivační událost **služby Salesforce – vytvoření objektu** .  
![Služby Salesforce aktivační událost obrázek 1](./media/connectors-create-api-salesforce/trigger-1.png)   
- Ovládací prvek **Po vytvoření objekt** zobrazený.  
![Služby Salesforce aktivační událost obrázek 2](./media/connectors-create-api-salesforce/trigger-2.png)   
- Vyberte **Typ objektu** a vyberte *vést* ze seznamu objektů. V tomto kroku jsou označující, zda vytváříte aktivační události, které vás upozorní aplikace logiky pokaždé, když se vytvoří nové zájemce v služby Salesforce.   
![Služby Salesforce aktivační událost obrázek 3](./media/connectors-create-api-salesforce/trigger-3.png)   
- To je vše. Jste si vytvořili aktivační událost. Potřebujete vytvořit alespoň jeden akci před uskutečněním to platné logiky aplikace.    
![Služby Salesforce aktivační událost obrázek 4](./media/connectors-create-api-salesforce/trigger-4.png)   

V tomto okamžiku aplikace logiky nakonfigurované s aktivační začne spustit ostatních aktivačních událostí a akce pracovního postupu při vytvoření nové položky ve vaší služby Salesforce.  