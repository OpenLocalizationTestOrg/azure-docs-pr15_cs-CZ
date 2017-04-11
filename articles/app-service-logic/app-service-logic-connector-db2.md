<properties
   pageTitle="Pomocí konektoru DB2 Microsoft Azure aplikace služby | Microsoft Azure"
   description="Jak konektoru DB2 pomocí aktivačních událostí použití logických operátorů aplikace a akce"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="db2-connector"></a>Spojnice DB2
>[AZURE.NOTE] Tuto verzi článku platí pro logiku aplikace 2014-12-01-schématu verzi.

Konektor aplikace Microsoft pro DB2 je aplikace rozhraní API pro připojení aplikace přes aplikaci služby Azure uložených v databázi IBM DB2 prostředků. Spojnice obsahuje Microsoft Client připojení vzdálených počítačů serveru DB2 TCP/IP síťovým připojením, včetně Azure hybridní připojení k místní servery DB2 pomocí předávací Bus služby Azure spojnice podporuje tyto operace databáze:

- Vyberte řádky pro čtení pomocí
- Hlasování ke čtení řádků pomocí vyberte počet následovaný vyberte
- Přidání jednoho řádku nebo víc řádků (hromadné) příkazu Vložit
- Změnit jeden řádek nebo víc řádků (hromadné) pomocí aktualizace
- Odebrání jednoho řádku nebo víc řádků (hromadné) pomocí odstranit
- Čtení ke změně řádků pomocí vyberte kurzor a za ním uveďte aktualizace kde od KURZORU
- Čtení odebrání řádků pomocí vyberte kurzor a za ním uveďte aktualizace kde od KURZORU
- Spuštění procedury se vstupní a výstupní parametry, vrátí hodnotu, sadě pomocí HOVORU
- Vlastní příkazy a složené operace pomocí vybrat, vložení, aktualizovat, odstranit

## <a name="triggers-and-actions"></a>Aktivace a akce
Spojnice podporuje následující aktivačních událostí použití logických operátorů aplikace a akce:

Aktivace | Akce
--- | ---
<ul><li>Hlasování dat</li></ul> | <ul><li>Vložení hromadné</li><li>Vložení</li><li>Hromadné aktualizace</li><li>Aktualizace</li><li>Volání</li><li>Hromadné odstranění</li><li>Odstranění</li><li>Vyberte</li><li>Podmíněné aktualizace</li><li>Příspěvek na entit</li><li>Podmíněné odstranit</li><li>Vyberte jeden entitu</li><li>Odstranění</li><li>Upsert entit</li><li>Vlastní příkazy</li><li>Složené operace</li></ul>


## <a name="create-the-db2-connector"></a>Vytvoření DB2 spojnice
Spojnice v rámci logiku aplikace nebo z webu Azure Marketplace můžete definovat jako v následujícím příkladu:  

1. V Azure startboard vyberte **Marketplace**.
2. V zásuvné **vše** **db2** zadejte do pole **Prohledat vše** a potom klepněte na klávesu enter.
3. Do pole Hledat vše podokna výsledků, vyberte **DB2 spojnice**.
4. V popisu zásuvné DB2 spojnice vyberte **vytvořit**.
5. Do balíčku zásuvné DB2 spojnice zadejte název (například "Db2ConnectorNewOrders"), plán služeb aplikací a další vlastnosti.
6. Vyberte **nastavení balíčku**a zadejte následující nastavení balíčku:  

    Jméno | Povinné |  Popis
--- | --- | ---
ConnectionString | Ano | Klient DB2 připojovací řetězec (například "síťovou adresu = webu název serveru, Síťový Port = 50000; ID uživatele = uživatelské jméno; Heslo = heslo; počátečního katalogu = vzorku; Zabalení kolekce = NWIND; výchozí schématu = NWIND ").
Tabulky | Ano | Čárkou oddělený seznam tabulky, zobrazení a název alias operace OData a generovat swagger si přečtěte následující dokumentaci s příklady (například "*NEWORDERS*").
Postupy | Ano | Hodnoty s oddělovači seznam jmen procedury a funkce (například "SPORDERID").
Místní edici | Ne | Pomocí Azure Bus Service Relay v místním nasazení.
ServiceBusConnectionString | Ne | Azure Service Bus Relay připojovací řetězec.
PollToCheckData | Ne | Vyberte počet údajů používat s Spouštěč aplikací logiku (například "Vyberte počet (\*) z NEWORDERS WHERE SHIPDATE IS NULL").
PollToReadData | Ne | Příkaz SELECT pomocí služby Spouštěč aplikací logiky (například "vyberte \* z NEWORDERS SHIPDATE-li hodnota NULL pro aktualizaci").
PollToAlterData | Ne | AKTUALIZACE nebo odstranění fakturu pomocí služby Spouštěč aplikací použití logických operátorů (například "DATUMODESLÁNÍ nastavení aktualizace NEWORDERS = aktuální datum WHERE aktuální OF &lt;kurzor&gt;").

7. Vyberte **OK**a pak vyberte **vytvořit**.
8. Až budete hotovi, nastavení balíčku vypadat takto:  
![][1]


## <a name="logic-app-with-db2-connector-action-to-add-data"></a>Použití logických operátorů aplikace s DB2 spojnice akcí pro přidání dat ##
Je možné definovat akci aplikace logiku přidat data do tabulky DB2 pomocí rozhraní API Insert nebo příspěvku operace Entity OData. Například můžete vložit nový záznam objednávky zákazníků zpracováním SQL INSERT dotaz proti tabulce určené se sloupcem identity vrací hodnotu identity nebo řádky vliv do aplikace logiky (vyberte ORDID z VÝSLEDNÁ tabulka (vložit do NWIND. HODNOTY NEWORDERS (CUSTID NÁZEVZÁSILKY, SHIPADDR, MĚSTODODÁNÍ, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

> [AZURE.TIP] Připojení DB2 "*vystavit entit*" chybovou hodnotu sloupce identity a vrátí "*Vložení rozhraní API*" vliv na řádky

1. V Azure startboard vyberte **+** (symbol plus), **webového + Mobile**a potom **logiku aplikace**.
2. Zadejte jméno (například "NewOrdersDb2"), plán služeb aplikací, další vlastnosti a pak vyberte **vytvořit**.
3. V Azure startboard vyberte logiky aplikaci, kterou jste právě vytvořili, **Nastavení**a potom **aktivačními událostmi a akce**.
4. V zásuvné aktivačními událostmi a akce vyberte **vytvořit přímo** v aplikaci logiky šablony.
5. Na panelu rozhraní API aplikace vyberte **opakování**, nastavit počet_plateb a interval a pak **zaškrtnutí**.
6. Na panelu rozhraní API aplikace vyberte **DB2 spojnici**, rozbalte položku seznamu operace vyberte **Vložit do NEWORDER**.
7. Rozbalení seznamu parametry a zadejte tyto hodnoty:  

    Jméno | Hodnota
--- | --- 
CUSTID | 10042
NÁZEVZÁSILKY | Pustí K Kountry úložiště 
SHIPADDR | 12 orchestr terasovité
MĚSTODODÁNÍ | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Zaškrtněte **políčko** uložit nastavení akce a pak **Uložit**.
9. Nastavení by měl vypadat takto:  
![][3]

10. V seznamu **všechny běží** v seznamu **operace**vyberte položku uvedené první (poslední spustit). 
11. V zásuvné **logiky aplikaci spustit** vyberte položku **Akce** **db2connectorneworders**.
12. V zásuvné **logiky aplikace akce** vyberte **VSTUPY odkaz**. Spojnice DB2 používá vstupů zpracuje parametry příkaz INSERT.
13. V zásuvné **logiky aplikace akce** vyberte **Výstupy odkaz**. Vstupní hodnoty by měl vypadat takto:  
![][4]

#### <a name="what-you-need-to-know"></a>Co je potřeba vědět

- Spojnice zkrátí názvy tabulek DB2 při tvořící logiky aplikace akce názvy. Třeba operace **vložte do NEWORDERS** zkrácen **do NEWORDER**vložit.
- Po uložení aplikaci logiky **aktivačními událostmi a akce**, zpracuje logiky aplikace operaci. Může být zpoždění většího počtu sekund (například 3 až 5 sekundy) před logiky aplikace zpracuje operaci. Pokud chcete můžete kliknete na **Spustit** a provedení operace.
- Spojnice DB2 definuje entit členy s atributy, včetně toho, jestli člen odpovídá DB2 sloupec s výchozí nebo vytvořit sloupcích (například identita). Použití logických operátorů aplikace se zobrazí červená hvězdička vedle názvu ID člena entit k označení DB2 sloupců, které vyžadují hodnoty. ORDID členu, který odpovídá sloupec identity DB2 neměli zadejte hodnotu. Můžete zadat hodnoty pro ostatní volitelné členy (položky, ORDDATE, REQDATE, ČÍSLOPŘEPRAVCE, DOPRAVNÉ, SHIPCTRY), které odpovídají DB2 sloupce s výchozími hodnotami. 
- Spojnice DB2 vrátí logiky aplikace odpověď na příspěvek na entit obsahující hodnoty pro sloupce identity odvozeným z SQLDARD DRDA (dat odpověď oblast dat SQL) na připravený příkaz INSERT jazyka SQL. DB2 server nevrátí vložené hodnoty pro sloupce s výchozími hodnotami.  


## <a name="logic-app-with-db2-connector-action-to-add-bulk-data"></a>Použití logických operátorů aplikace s DB2 spojnice akcí pro hromadné přidání dat ##
Je možné definovat akci aplikace logiky pro přidání dat do tabulky DB2 pomocí rozhraní API hromadné vložení operaci. Například můžete vložit dva nové pořadí záznamy zákazníků, kliknutím na zpracování příkaz Vložit SQL pomocí matici hodnot v řádku proti tabulce určené se sloupcem identity vrácení řádky vliv do aplikace logiky (vyberte ORDID z VÝSLEDNÁ tabulka (vložit do NWIND. HODNOTY NEWORDERS (CUSTID NÁZEVZÁSILKY, SHIPADDR, MĚSTODODÁNÍ, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

1. V Azure startboard vyberte **+** (symbol plus), **webového + Mobile**a potom **logiku aplikace**.
2. Zadejte jméno (například "NewOrdersBulkDb2"), plán služeb aplikací, další vlastnosti a pak vyberte **vytvořit**.
3. V Azure startboard vyberte logiky aplikaci, kterou jste právě vytvořili, **Nastavení**a potom **aktivačními událostmi a akce**.
4. V zásuvné aktivačními událostmi a akce vyberte **vytvořit přímo** v aplikaci logiky šablony.
5. Na panelu rozhraní API aplikace vyberte **opakování**, nastavit počet_plateb a interval a pak **zaškrtnutí**.
6. V panelu rozhraní API aplikace vyberte **DB2 spojnici**, rozbalte položku seznamu operace vyberte **Hromadné vložení do nového**.
7. Zadejte hodnotu **řádků** v matici. Například zkopírujte a vložte následující:

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
    ```

8. Zaškrtněte **políčko** uložit nastavení akce a pak **Uložit**. Nastavení by měl vypadat takto:  
![][6]

9. V seznamu **všechny běží** ve skupinovém rámečku **operace**klikněte na položku uvedené první (poslední spustit).
10. V zásuvné **logiky aplikaci spustit** klikněte na položku **Akce** .
11. V zásuvné **logickou aplikace akci** klikněte na **Odkaz vstupů**. Výstupy by měl vypadat takto:  
[][7]
12. V zásuvné **logiky aplikace akce** klikněte na **Odkaz výstupů**. Výstupy by měl vypadat takto:  
![][8]

#### <a name="what-you-need-to-know"></a>Co je potřeba vědět

- Spojnice zkrátí názvy tabulek DB2 při tvořící logiky aplikace akce názvy. Třeba operace **Hromadné vložení do NEWORDERS** zkrácen **hromadné**vložit do nového.
- Vynecháním identity sloupce (například ORDID), s možnou hodnotou NULL sloupce (například DATUMODESLÁNÍ) a sloupce s výchozími hodnotami (například ORDDATE REQDATE, ČÍSLOPŘEPRAVCE, DOPRAVNÉ, SHIPCTRY) databáze DB2 vygeneruje hodnoty.
- Zadáním "dnes" a "zítra" DB2 spojnice vygeneruje "Aktuální datum" a "Aktuální datum plus 1 den" funkce (například REQDATE). 


## <a name="logic-app-with-db2-connector-trigger-to-read-alter-or-delete-data"></a>Použití logických operátorů aplikace s DB2 spojnice aktivační události pro čtení, změnit nebo odstranit data ##
Můžete definovat logiku aplikace aktivační události hlasování a číst data z tabulky DB2 pomocí operaci složené rozhraní API hlasování Data. Například může číst jednu nebo více nové pořadí záznamy zákazníků, načtení záznamů v aplikaci použití logických operátorů. Nastavení připojení DB2 balíček/aplikace by měl vypadat takto:

    Nastavení aplikace | Hodnota
--- | --- | ---
PollToCheckData | Vyberte počet (\*) z NEWORDERS, kde je SHIPDATE NULL
PollToReadData | Vyberte \* z NEWORDERS, kde je NULL aktualizace SHIPDATE
PollToAlterData | <no value specified>


Také můžete definovat logiky aplikace aktivační události hlasování, číst a měnit data v tabulce DB2 pomocí složené operace rozhraní API hlasování Data. Například může číst jednu nebo více nové pořadí záznamy zákazníků, aktualizovat hodnoty řádku vrácení vybrané (před aktualizací) záznamů v aplikaci použití logických operátorů. Nastavení připojení DB2 balíček/aplikace by měl vypadat takto:

    Nastavení aplikace | Hodnota
--- | --- | ---
PollToCheckData | Vyberte počet (\*) z NEWORDERS, kde je SHIPDATE NULL
PollToReadData | Vyberte \* z NEWORDERS, kde je NULL aktualizace SHIPDATE
PollToAlterData | AKTUALIZACE NEWORDERS sadu SHIPDATE = aktuální datum kde aktuální VOLNOSTI &lt;kurzor&gt;


Kromě toho můžete definovat logiky aplikace aktivační události hlasování, přečtěte si a odebrání dat z tabulky DB2 myší Data hlasování rozhraní API složené. Například může číst jednu nebo více nové pořadí záznamy zákazníků, odstranit řádky vrácení vybrané (před odstraněním) záznamů v aplikaci logiku. Nastavení připojení DB2 balíček/aplikace by měl vypadat takto:

    Nastavení aplikace | Hodnota
--- | --- | ---
PollToCheckData | Vyberte počet (\*) z NEWORDERS, kde je SHIPDATE NULL
PollToReadData | Vyberte \* z NEWORDERS, kde je NULL aktualizace SHIPDATE
PollToAlterData | Odstranit NEWORDERS kde aktuální VOLNOSTI &lt;KURZORU&gt;

V tomto příkladu použití logických operátorů aplikace bude hlasování, číst, aktualizovat a pak znovu si projděte témata data v tabulce DB2.

1. V Azure startboard vyberte **+** (symbol plus), **webového + Mobile**a potom **logiku aplikace**.
2. Zadejte jméno (například "ShipOrdersDb2"), plán služeb aplikací, další vlastnosti a pak vyberte **vytvořit**.
3. V Azure startboard vyberte logiky aplikaci, kterou jste právě vytvořili, **Nastavení**a potom **aktivačními událostmi a akce**.
4. V zásuvné aktivačními událostmi a akce vyberte **vytvořit přímo** v aplikaci logiky šablony.
5. Na panelu rozhraní API aplikace vyberte **DB2 spojnice**, nastavit počet_plateb a interval a pak **zaškrtnutí**.
6. V panelu rozhraní API aplikace vyberte **DB2 spojnici**, rozbalte položku seznamu operace vyberte **vybírat NEWORDERS**.
7. Zaškrtněte **políčko** uložit nastavení akce a pak **Uložit**. Nastavení by měl vypadat takto:  
![][10]  
8. Klikněte na Zavřít zásuvné **aktivačními událostmi a akce** a klikněte na Zavřít zásuvné **Nastavení** .
9. V seznamu **všechny běží** ve skupinovém rámečku **operace**klikněte na položku uvedené první (poslední spustit).
10. V zásuvné **logiky aplikaci spustit** klikněte na položku **Akce** .
11. V zásuvné **logiky aplikace akce** klikněte na **Odkaz výstupů**. Výstupy by měl vypadat takto:  
![][11]


## <a name="logic-app-with-db2-connector-action-to-remove-data"></a>Použití logických operátorů aplikace s DB2 spojnice akce: Odeberte data ##
Je možné definovat akci aplikace použití logických operátorů: Odeberte data z tabulky DB2 pomocí rozhraní API Delete nebo příspěvku operace Entity OData. Například můžete vložit nový záznam objednávky zákazníků zpracováním SQL INSERT dotaz proti tabulce určené se sloupcem identity vrací hodnotu identity nebo řádky vliv do aplikace logiky (vyberte ORDID z VÝSLEDNÁ tabulka (vložit do NWIND. HODNOTY NEWORDERS (CUSTID NÁZEVZÁSILKY, SHIPADDR, MĚSTODODÁNÍ, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

## <a name="create-logic-app-using-db2-connector-to-remove-data"></a>Vytvoření aplikace použití logických operátorů: Odeberte data pomocí DB2 spojnice ##
Můžete vytvořit novou aplikaci použití logických operátorů z v rámci webu Azure Marketplace a pak pomocí konektoru DB2 jako akce odebrat objednávky zákazníků. Například, můžete použít DB2 spojnice podmíněné operace odstranění se nezobrazuje zpracuje příkaz SQL DELETE (odstranit z NEWORDERS kde ORDID > = 10000).

1. V nabídce centrální vývěsky Azure **Start** , klikněte na **+** (symbol plus), klikněte na **Web + Mobile**a potom klikněte na **logiku aplikace**. 
2. Do zásuvné **vytvořit logiky aplikace** zadejte **název**, například **RemoveOrdersDb2**.
3. Vyberte nebo definovat hodnoty pro ostatní nastavení (například plán služeb, skupina zdroje).
4. Nastavení by měl vypadat takto. Klikněte na **vytvořit**:  
![][12]  
5. V **Nastavení** zásuvné klikněte na **aktivační události a akce**.
6. V zásuvné **aktivačními událostmi a akce** v seznamu **aplikací logiky šablony** klikněte na **vytvořit od začátku**.
7. V zásuvné **aktivačními událostmi a akce** na panelu **Rozhraní API aplikace** v rámci skupiny zdrojů klikněte na **opakování**.
8. Na návrhové ploše logiky aplikace klikněte na položku **opakování** , nastavte **Četnost** a **Interval**, například **dny** a **1**a klikněte na **políčko** uložit nastavení opakování položky.
9. V zásuvné **aktivačními událostmi a akce** na panelu **Rozhraní API aplikace** v rámci skupiny zdrojů klikněte na **DB2 spojnice**.
10. Na návrhové ploše logiky aplikace klikněte na položku Akce **DB2 spojnice** , klikněte na tři tečky (****...) rozbalte seznam operace a klikněte na **podmíněné zabránění N**.
11. Pro danou položku Akce spojnice DB2 zadejte **ORDID stránku 10000** **výraz, který označuje podmnožinu položky**.
12. Klikněte na **políčko** uložit nastavení akce a klepněte na tlačítko **Uložit**. Nastavení by měl vypadat takto:  
![][13]  
13. Klikněte na Zavřít zásuvné **aktivačními událostmi a akce** a klikněte na Zavřít zásuvné **Nastavení** .
14. V seznamu **všechny běží** ve skupinovém rámečku **operace**klikněte na položku uvedené první (poslední spustit).
15. V zásuvné **logiky aplikaci spustit** klikněte na položku **Akce** .
16. V zásuvné **logiky aplikace akce** klikněte na **Odkaz výstupů**. Výstupy by měl vypadat takto:  
![][14]

**Poznámka:** Návrhář aplikace logiku zkrátí názvy tabulek. Třeba operace **podmíněné zabránění NEWORDERS** zkrácen **podmíněné zabránění N**.


> [AZURE.TIP] K vytvoření ukázkové tabulky a uložené procedury použijte následující příkazy SQL. 

Můžete vytvořit v ukázkové tabulce NEWORDERS pomocí následujících příkazů DB2 SQL DDL:
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



Můžete vytvořit postup SPOERID uložené vzorku pomocí následujícího příkazu DB2 DDL:
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## <a name="hybrid-configuration-optional"></a>Hybridní konfigurace (volitelné)

> [AZURE.NOTE] Tento krok je vyžadován jenom v případě, že používáte DB2 spojnice místní za brány firewall.

Služba aplikace pomocí Správce konfigurace hybridního zabezpečené připojení k místní systém. Pokud spojnice používá místním IBM DB2 serveru pro systém Windows, hybridní Connection Manager je vyžadován.

V tématu [použití hybridního Connection Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Další možnosti s na spojnici
Teď, když se vytvoří spojnice, můžete ho přidat do pracovního postupu firmy pomocí aplikace logiky. V tématu [Jaké aplikace logiky?](app-service-logic-what-are-logic-apps.md).

Vytvářejte rozhraní API aplikace pomocí rozhraní REST API. V tématu [spojnic a rozhraní API aplikace odkaz](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Můžete taky zkontrolovat Statistika a řízení jistoty ke spojnici připojit. V tématu [Správa a sledování předdefinované rozhraní API aplikace a spojnic](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png

