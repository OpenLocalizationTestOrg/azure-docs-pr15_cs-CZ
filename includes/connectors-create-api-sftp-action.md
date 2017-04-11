Teď, když jste přidali aktivační událost, dostanete zajímavý její čas něco udělat s daty, která je generováno aplikací aktivační událost. Pomocí následujícího postupu přidejte akci **SFTP - extrahovat složky** . Tato akce bude extrahovat obsah souboru, pokud jsou splněny definované podmínky. 

Konfigurace tento akce, budete muset zadat následující informace. Zjistíte, že je snadno se použije data generovaná aktivační událost jako vstup pro některé vlastnosti nového souboru:

|SFTP - extrahovat vlastnosti složky|Popis|
|---|---|
|Cesta k souboru archivu zdroje|Toto je cestu k souboru při rozbalování. Můžete vybírat z tokenů z předchozích akce nebo procházet serveru SFTP zobrazíte cesta k souboru.|
|Cílová složka cesta|Toto je cestu umístění extrahované soubory. Můžete vybírat z tokeny z předchozích akce jako cílové umístění nebo procházet SFTP serveru a vyberte cesty.|
|Přepsat?|Označuje, pokud soubor se stejným názvem jako extrahované soubor nachází v cestu ke složce cíl Pokud existující soubor by měl přepsat, nebo ne.|

Pusťme se do práce přidání akce extrahujte soubory, pokud dříve definované podmínka vyhodnotí jako *PRAVDA*. 

1. Vyberte **Přidat akci**.        
![SFTP akce podmínka obrázek 6](./media/connectors-create-api-sftp/condition-6.png)   
- Vyberte akci **SFTP - extrahovat složky**      
![SFTP akce podmínka obrázek 7](./media/connectors-create-api-sftp/condition-7.png)   
- Vyberte **zdroj cesta k souboru archivu**              
![SFTP akce podmínka obrázek 9](./media/connectors-create-api-sftp/condition-9.png)   
- Vyberte token **cesta k souboru** . Tento údaj označuje, že používáte cesta k souboru archivu zdroj bude cesta k souboru souboru, který nalezené aktivační událost.           
![SFTP akce podmínka obrázek 10](./media/connectors-create-api-sftp/condition-10.png)   
- Vyberte **cílovou složku cestu**           
![SFTP akce podmínka obrázek 11](./media/connectors-create-api-sftp/condition-11.png)   
- Vyberte token **cesta k souboru** . Tento údaj označuje, že použijete cesta k souboru souboru, který nalezené aktivační událost jako cílové umístění pro extrahovanou soubory.   
- Zadejte *\ExtractedFile* v ovládacím prvku **cílovou složku cestu** . To udělejte hned za token cesta k souboru v ovládacím prvku cílovou složku cestu.         
![SFTP akce podmínka obrázek 12](./media/connectors-create-api-sftp/condition-12.png)   
- Zadejte *PRAVDA* v **Přepsat?* ovládací prvek označíte, že by měl přepsat existující soubory, pokud mají stejný název jako extrahované soubory.      
![SFTP akce podmínka obrázek 13](./media/connectors-create-api-sftp/condition-13.png)   
- Uložení změn do pracovního postupu  
