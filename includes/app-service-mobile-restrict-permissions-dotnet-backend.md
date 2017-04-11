
Ve výchozím nastavení lze anonymně vyvolat API v mobilní aplikaci back-end. Dál potřebujete omezit přístup k jenom ověřené klienty.  

+ **Back-end Node.js (prostřednictvím portálu)** :  
    
    V mobilní aplikaci **Nastavení**klikněte **Jednoduše tabulek** a vyberte tabulky. Klikněte na **změnit oprávnění**, vyberte **ověřeno přístup jenom** pro všechna oprávnění a pak klikněte na tlačítko **Uložit**. 

+ **.NET back-end (C#)**:  

    V aplikaci project serveru, přejděte na **řadiče** > **TodoItemController.cs**. Přidejte `[Authorize]` atribut třídy **TodoItemController** následujícím způsobem. Pokud chcete omezit přístup jenom pro konkrétní metody, můžete taky použít Tenhle atribut jen na ty metody místo předmětu. Publikujte projekt server.


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Back-end Node.js (přes Node.js kód)** :  
    
    Vyžadovat ověřování pro přístup k tabulce, přidejte skript serveru Node.js následující řádek:


        table.access = 'authenticated';

    Další informace najdete v příručce [jak: Požadovat ověření pro přístup k tabulkám](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Zjistěte, jak stáhnout projekt kódu rychlý úvod z webu, najdete v článku [jak: stažení Node.js back-end rychlý úvod kód projektu pomocí libovolná](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).

