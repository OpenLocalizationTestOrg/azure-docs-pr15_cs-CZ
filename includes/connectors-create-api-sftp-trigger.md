Přidejte do něj aktivační událost.

1. Zadejte *sftp* do vyhledávacího pole v Návrháři logiky aplikace a pak vyberte aktivační událost **SFTP – při přidání nebo změny souboru**   
![Obrázek aktivační událost SFTP 1](./media/connectors-create-api-sftp/trigger-1.png)  
- Ovládací prvek **Při přidání nebo změny souboru** se objeví  
![Obrázek aktivační událost SFTP 2](./media/connectors-create-api-sftp/trigger-2.png)  
- Vyberte **...** nachází na pravé straně ovládacího prvku. Otevře se ovládací prvek Výběr složky  
![Obrázek aktivační událost SFTP 3](./media/connectors-create-api-sftp/action-1.png)  
- Vyberte **SFTP** kořenové složky vyberte složku, kterou chcete sledovat nové nebo změněné soubory. Všimněte si, že kořenové složky se nyní zobrazí v ovládacím prvku **složky** .  
![Obrázek aktivační událost SFTP 4](./media/connectors-create-api-sftp/action-2.png)   

V tomto okamžiku aplikace logiky nakonfigurované s aktivační začne spustit ostatních aktivačních událostí a akce pracovního postupu se po změnili nebo vytvořené v požadované složce SFTP souboru. 

>[AZURE.NOTE]Použití logických operátorů aplikace je funkční musí obsahovat alespoň jeden aktivační události a jednu akci. Postupujte podle pokynů v následující části chcete-li přidat akci.  