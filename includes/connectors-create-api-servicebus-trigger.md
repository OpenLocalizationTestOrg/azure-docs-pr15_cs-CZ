Tady je postup, jak pomocí aktivační události **Služby Bus – po přijetí zprávy ve frontě** zahajte pracovního postupu aplikace logiky při odeslání nové položky do fronty Bus služby.  

>[AZURE.NOTE]Zobrazí se výzva k Pokud jste ještě nevytvořili připojení služby Bus Přihlaste se pomocí služby Bus připojovací řetězec.  

1. Do pole Hledat v aplikace Návrhář logiky zadejte **bus služby**. Klikněte na aktivační událost **Služby Bus – po přijetí zprávy ve frontě** .  
![Služba Bus aktivační událost obrázek 1](./media/connectors-create-api-servicebus/trigger-1.png)   
- Zobrazí se dialogové okno **Po přijetí zprávy ve frontě** .  
![Služba Bus aktivační událost obrázek 2](./media/connectors-create-api-servicebus/trigger-2.png)   
- Zadejte název fronty Bus služby rádi byste aktivační události pro sledování.   
![Služba Bus aktivační událost obrázek 3](./media/connectors-create-api-servicebus/trigger-3.png)   

V tomto okamžiku aplikace logiky nakonfigurované s aktivační událost. Nová položka odeslaná ve frontě, který jste vybrali, začne se aktivační událost spustit ostatních aktivačních událostí a akce pracovního postupu.    
