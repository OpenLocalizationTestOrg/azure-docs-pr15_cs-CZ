### <a name="prerequisites"></a>Zjistit předpoklady pro

Musí mít účet [Služby Bus](https://azure.microsoft.com/services/service-bus/) .  

Než budete moct použít účtu Bus služby Azure v aplikaci použití logických operátorů, musí povolit aplikaci logiky pro připojení k účtu služby bus. Naštěstí dosáhnete snadno z v rámci logiky aplikace na portálu Azure.  

Tady je postup povolit připojení k účtu služby Bus aplikace použití logických operátorů:  

1. Vytvoření připojení služby Bus v návrháři aplikace použití logických operátorů, vyberte v rozevíracím seznamu **Zobrazit Microsoft spravované rozhraní API** . Do vyhledávacího pole zadejte **bus služby** . Vyberte aktivační událost nebo akci, kterou chcete použít.  
    ![Vzhled spojovacího služby Bus 1](./media/connectors-create-api-servicebus/servicebus-1.png)  

2. Pokud jste nevytvořili všechna připojení služby Bus před, budete vyzváni k zadání přihlašovacích údajů Bus služby. Těchto přihlašovacích údajů se používají k povolit aplikace použití logických operátorů k připojení a přístup k účtu služby Bus data. Konektor služby Bus musí připojovací řetězec pro službu Bus názvů. Vyžaduje taky **Spravovat** oprávnění. Vhodná k vědět, pokud připojovací řetězec je pro obor nebo konkrétní entity pokud obsahuje `EntityPath` parametr. Pokud ano, není vpravo připojovací řetězec z logických aplikace.  
    ![Služba Bus připojovacího řetězce](./media/connectors-create-api-servicebus/connectionstring.png)

1. Poté, co jste obdrželi připojovací řetězec pro obor, můžete pro připojení k rozhraní API v aplikacích pro použití logických operátorů.  
    ![Vzhled spojovacího služby Bus 2](./media/connectors-create-api-servicebus/servicebus-2.png)  

3. Všimněte si vytvořil připojení a měli byste být bezplatné pokračujte dalšími kroky v aplikaci použití logických operátorů.  
    ![Vzhled spojovacího služby Bus 3](./media/connectors-create-api-servicebus/servicebus-3.png)   
