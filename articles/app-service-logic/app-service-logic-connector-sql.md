<properties
   pageTitle="Pomocí konektoru SQL v aplikacích pro použití logických operátorů | Aplikace služby Microsoft Azure"
   description="Jak vytvořit a nakonfigurovat aplikaci SQL spojnice nebo rozhraní API a použít v aplikaci logiky v aplikaci služby Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Začínáme s Microsoft SQL Connector a přiřadit ji někomu aplikace pro použití logických operátorů
>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2014 12 01 náhled schématu verze. Azure SQL 2015-08: 01-schématu verze preview klikněte na [Rozhraní API SQL Azure](../connectors/connectors-create-api-sqlazure.md).

Připojení k místního serveru SQL Server nebo databáze SQL Azure, vytvořit a změnit informace nebo data. Spojnice lze použít v aplikacích pro použití logických operátorů k načtení, procesu nebo nabízená dat jako součást "pracovního postupu". Pokud používáte konektoru SQL v pracovním postupu, můžete dosáhnout řadu možností. Například máte tyto možnosti:

- Zveřejnění části dat umístěný v databázi SQL pomocí webu nebo mobilní aplikaci.
- Vložení data do tabulky databáze SQL pro úložiště. Můžete třeba zadat záznamy zaměstnanců, aktualizovat prodejní objednávky a podobně.
- Načtení dat z SQL a použít v obchodních procesů. Můžete třeba získat záznamy o zákaznících a umístit ty záznamy o zákaznících služby SalesForce.

Přidejte konektoru SQL pro obchodní data pracovního postupu a proces jako součást tento pracovní postup v aplikaci logiku. 

## <a name="triggers-and-actions"></a>Aktivace a akce
*Aktivace* jsou událostí, ke kterým dojde. Třeba při aktualizaci smysluplném pořadí nebo nový zákazník přibude. *Akce* je výsledkem aktivační událost. Třeba při aktualizaci objednávku zaslat upozornění prodejce. Nebo když se přidá nový zákazník, odešlete uvítací e-mail nového zákazníka.

Konektor SQL mohou sloužit jako aktivační událost nebo akce v logických aplikace a podporuje data ve formátu JSON a XML. U každé tabulky zahrnuté do balíčku nastavení (podrobnosti o této dál v tomto tématu) je sady akcí JSON a sady akcí XML.

Konektor SQL obsahuje následující aktivačními událostmi a dostupné akce:

Aktivace | Akce
--- | ---
Hlasování dat | <ul><li>Vložte do tabulky</li><li>Update tabulka</li><li>Vyberte z tabulky</li><li>Odstranit z tabulky</li><li>Volání uložená procedura</li></ul>

## <a name="create-the-sql-connector"></a>Vytvoření spojnice SQL

Spojnice lze vytvořit v aplikaci logiky nebo vytvořit přímo z webu Azure Marketplace. Vytvoření spojnice z Marketplace:  

1. V Azure startboard vyberte **Marketplace**.
2. Vyhledejte "SQL spojnice", vyberte ji a vyberte **vytvořit**.
3. Zadejte název aplikace služby plánování a jiné vlastnosti.
4. Zadejte následující nastavení balíčku:

    Jméno | Povinné |  Popis
--- | --- | ---
Název serveru | Ano | Zadejte název serveru SQL Server. Zadejte třeba *SQL Server nebo sqlexpress* nebo *SQLserver.mydomain.com*.
Port | Ne | Výchozí hodnota je 1433.
Uživatelské jméno | Ano | Zadejte uživatelské jméno, které se může přihlásit k serveru SQL. Pokud připojení k místní SQL Server, zadejte přihlašovací údaje ověřování serveru SQL.
Heslo | Ano | Zadejte uživatelské jméno, heslo.
Název databáze | Ano | Zadejte databázi, ke kterému se připojujete. Můžete třeba zadat *Zákazníci* nebo *dbo/objednávky*.
Místní | Ano | Výchozí hodnota je False. Pokud připojení k databázi Azure SQL, zadejte hodnotu False. Zadejte hodnotu True, pokud připojení k místní SQL Server.
Služba Bus připojovacího řetězce | Ne | Pokud se připojujete k místním zadejte připojovací řetězec relay Service Bus.<br/><br/>[Použití hybridního Connection Manager](app-service-logic-hybrid-connection-manager.md)<br/>[Služby Bus ceny](https://azure.microsoft.com/pricing/details/service-bus/)
Název serveru partner | Ne | Pokud je k dispozici primární server, můžete zadat serveru partner jako alternativní nebo záložní server.
Tabulky | Ne | Seznam databázových tabulek, které mohou být aktualizovány spojnice. Zadejte například *OrdersTable* nebo *EmployeeTable*. Pokud jsou zadány žádné tabulky, lze použít všechny tabulky. Platných tabulek a/nebo uložené procedury nutné použít tento konektor akce.
Uložené procedury | Ne | Zadejte existující uložené procedury, můžete volat spojnice. Zadejte například *sp_IsEmployeeEligible* nebo *sp_CalculateOrderDiscount*. Platných tabulek a/nebo uložené procedury nutné použít tento konektor akce.
Dotaz na data k dispozici | Podpora aktivační událost | Zjistit, zda je k dispozici pro dotazování tabulky databáze SQL serveru všechna data, příkaz SQL. To měli vrácení číselné hodnoty představující počet řádků dat k dispozici. Příklad: Vyberte COUNT(*) table_name.
Dotaz na Data hlasování | Podpora aktivační událost | Příkaz SQL hlasování tabulky databáze SQL serveru. Můžete zadat libovolný počet příkazy SQL oddělené středníkem. Tento příkaz je spouštět využití transakce a pouze potvrzeného při bezpečné uložení dat v aplikaci použití logických operátorů. Příklad: Vyberte *table_name; ODSTRANIT z table_name. <br/>**Note**<br/>Je nutné zadat výraz hlasování, který předchází nekonečné smyčce odstraněním, přesunutí nebo aktualizace vybraného data a ujistěte se, že není znovu dotazování stejná data.

5. Až budete hotovi, nastavení balíčku vypadat takto:  
![][1]  

6. Vyberte možnost **vytvořit**. 


## <a name="use-the-connector-as-a-trigger"></a>Pomocí konektoru jako aktivační událost
Podívejme se na použití logických operátorů jednoduché aplikace zjišťuje data z tabulky SQL, přidá do data v jiné tabulce, který aktualizuje data.

Použít konektoru SQL jako aktivační událost, zadejte hodnoty **Dotaz na Data k dispozici** a **Dotaz na Data hlasování** . **Dotaz na data k dispozici** probíhá podle plánu, zadejte a určuje, zda je k dispozici žádná data. Protože tento dotaz vrátí jenom skalární čísla, můžete správně zapnuli a optimalizované pro časté spuštění.

**Dotaz na Data hlasování** je spuštěn pouze, když Data k dispozici dotazu označuje, že data k dispozici. Tento příkaz spustí v rámci transakce a je jen potvrzeného Pokud extrahovaných dat trvale uložené v pracovním postupu. Je důležité, abyste tak předešli nekonečně znovu extrahování stejná data. Transakční přírodní tohoto spuštění lze odstranit nebo aktualizovat data a ujistěte se, že není shromažďované při příštím data zpracovávat.

> [AZURE.NOTE] Schéma vrácené tento příkaz identifikuje vlastnosti dostupné v na spojnici. Musí mít název všechny sloupce.

#### <a name="data-available-query-example"></a>Příklad data k dispozici dotazu

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>Příklad dotazu dat hlasování

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>Přidání aktivační událost
1. Při vytváření nebo úpravy v aplikaci použití logických operátorů, vyberte spojnici SQL jste vytvořili jako aktivační událost. Seznam dostupných aktivačních událostí: **Hlasování dat (JSON)** a **Hlasování dat XML**:  
![][5]

2. Vyberte aktivační událost **Hlasování dat (JSON)** , zadejte požadovanou frekvenci a klikněte ✓:  
![][6]

3. Aktivační událost se teď zobrazuje nakonfigurovaná v aplikaci použití logických operátorů. Output(s) aktivační událost jsou vidět a mohou sloužit jako vstupy všechny následující akce:  
![][7]

## <a name="use-the-connector-as-an-action"></a>Pomocí konektoru akce
Pomocí naše jednoduchých logických aplikace scénáře, které zjišťuje data z tabulky SQL přidá do data v jiné tabulce a aktualizuje data.

Jako akce použít konektoru SQL, zadejte název tabulky a/nebo uložené procedury jste zadali při vytvoření konektoru SQL:

1. Po aktivační událost (nebo vyberte "ruční spuštění této logiky"), přidání konektoru SQL jste vytvořili v galerii. Vyberte jednu z vložení akcí, jako je *Vložte do TempEmployeeDetails (JSON)*:  
![][8]

2. Zadání vstupních hodnot záznam, který chcete vložit a klikněte na ✓:  
![][9]

3. V galerii vyberte stejný konektor SQL, který jste vytvořili. Akce vyberte akce aktualizovat na stejné tabulce, jako je *EmployeeDetails aktualizace*:  
![][11]

4. Zadání vstupních hodnot pro akce aktualizovat a klikněte na ✓:  
![][12]

Použití logických operátorů aplikaci můžete otestovat přidáním nového záznamu do tabulky, kterým dojde.

## <a name="what-you-can-and-cannot-do"></a>Co můžete a nemůžete dělat

Dotaz SQL | Podporované | Nepodporovaná funkce
--- | --- | ---
Kde klauzule | <ul><li>Operátory: A, případně = <> <, < =, >, > = a LÍBÍ.</li><li>Možné kombinování víc podmínek sub "(" a")"</li><li>Řetězec znaků, data a času (uzavřený do jednoduchých uvozovek) čísel (by měl obsahovat pouze číslice)</li><li>Sporná, i když jsou ve formátu binární výraz třeba ((operand operator operand) a/nebo (operand operátor operand)) *</li></ul> | <ul><li>Operátory: Mezi, v</li><li>Všechny předdefinované funkce, jako je ADD(), MAX() NOW(), POWER() a tak dále</li><li>Matematické operátory, jako *, -, +, a tak dále</li><li>Zřetězení řetězců pomocí +.</li><li>Všechny spojení</li><li>NULL ani Null</li><li>Všechna čísla s znaky není číselného typu, jako hexadecimální čísla</li></ul>
Pole (výběrový dotaz) | <ul><li>Názvy sloupců platné oddělte je čárkami. Bez předpony název tabulky povolené (funguje spojnice na jedné tabulky vždy).</li><li>Názvy lze uvést zpětným ' ["a"] "</li></ul> | <ul><li>Klíčová slova jako horní, DISTINCT a tak dále</li><li>Zástupný jako ulice + Zip jako adresa a Město</li><li>Všechny předdefinované funkce, například ADD() MAX() NOW(), POWER() a tak dále</li><li>Matematické operátory, jako *, -, +, a tak dále</li><li>Zřetězení řetězců pomocí +</li></ul>

#### <a name="tips"></a>Tipy

- Rozšířené dotazy měli byste vytváření uložené procedury a spouštět způsobem spouštět uložené rozhraní API.
- Pokud chcete použít vnitřní dotazů, je používáte v rámci uložené procedury.
- Pro připojení ke více podmínek, použijete "A" a "Nebo" operátory.

## <a name="hybrid-configuration-optional"></a>Hybridní konfigurace (volitelné)

> [AZURE.NOTE] Tento krok je vyžadován jenom v případě, že používáte SQL Server místní za brány firewall.

Služba aplikace pomocí Správce konfigurace hybridního zabezpečené připojení k místní systém. Pokud jste spojnice využití místního SQL serveru, hybridní Connection Manager není potřeba.

> [AZURE.NOTE] Pokud chcete začít s aplikacemi jiných logiky Azure před registrací účet Azure, přejděte na [Zkuste aplikaci použití logických operátorů](https://tryappservice.azure.com/?appservice=logic), kde můžete okamžitě vytvořit aplikaci logiky krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

V tématu [použití hybridního Connection Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Další možnosti s na spojnici
Teď, když se vytvoří spojnice, můžete ho přidat do pracovního postupu firmy pomocí aplikace použití logických operátorů. V tématu [Jaké aplikace logiky?](app-service-logic-what-are-logic-apps.md).

Zobrazte odkaz Swagger rozhraní REST API [spojnicemi a rozhraní API aplikace odkaz](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Můžete taky zkontrolovat Statistika a řízení jistoty ke spojnici připojit. V tématu [Správa a sledování předdefinované rozhraní API aplikace a spojnic](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png
