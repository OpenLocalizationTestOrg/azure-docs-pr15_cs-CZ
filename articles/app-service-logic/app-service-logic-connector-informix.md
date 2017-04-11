<properties
   pageTitle="Pomocí konektoru Informix Microsoft Azure aplikace služby | Microsoft Azure"
   description="Jak konektoru Informix pomocí aktivačních událostí použití logických operátorů aplikace a akce"
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

# <a name="informix-connector"></a>Zprostředkovatel Informix spojnice
>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2014 12 01 náhled schématu verze.

Konektor aplikace Microsoft pro Informix je aplikace rozhraní API pro připojení aplikace přes aplikaci služby Azure uložených v databázi IBM Informix prostředků. Spojnice obsahuje Microsoft Client připojení vzdálených počítačů serveru Informix TCP/IP síťovým připojením, včetně Azure hybridní připojení k místní servery Informix pomocí předávací Bus služby Azure. Spojnice podporuje tyto operace databáze:

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
Spojnice podporuje následující aktivace aplikace logiku a akce:

Aktivace | Akce
--- | ---
<ul><li>Hlasování dat</li></ul> | <ul><li>Vložení hromadné</li><li>Vložení</li><li>Hromadné aktualizace</li><li>Aktualizace</li><li>Volání</li><li>Hromadné odstranění</li><li>Odstranění</li><li>Vyberte</li><li>Podmíněné aktualizace</li><li>Příspěvek na entit</li><li>Podmíněné odstranit</li><li>Vyberte jeden entitu</li><li>Odstranění</li><li>Upsert entit</li><li>Vlastní příkazy</li><li>Složené operace</li></ul>


## <a name="create-the-informix-connector"></a>Vytvoření databáze Informix spojnice
Spojnice v rámci logiky aplikace nebo z webu Azure Marketplace můžete definovat jako v následujícím příkladu:  

1. V Azure startboard vyberte **Marketplace**.
2. V zásuvné **všechno** **informix** zadejte do pole **Prohledat vše** a potom klepněte na klávesu enter.
3. Do pole Hledat vše podokna výsledků, vyberte **Informix spojnice**.
4. V popisu zásuvné Informix spojnice vyberte **vytvořit**.
5. Do balíčku zásuvné Informix spojnice zadejte název (například "InformixConnectorNewOrders"), plán služeb aplikací a další vlastnosti.
6. Vyberte **nastavení balíčku**a zadejte následující nastavení balíčku.

    Jméno | Povinné |  Popis
--- | --- | ---
ConnectionString | Ano | Zprostředkovatel Informix klienta připojovací řetězec (například "síťovou adresu = webu název serveru, Síťový Port = 9089; ID uživatele = uživatelské jméno; Heslo = heslo; počátečního katalogu = nwind; výchozí schématu = informix ").
Tabulky | Ano | Čárkou oddělený seznam tabulky, zobrazení a název alias operace OData a generovat swagger si přečtěte následující dokumentaci s příklady (například "NEWORDERS").
Postupy | Ano | Hodnoty s oddělovači seznam jmen procedury a funkce (například "SPORDERID").
Místní edici | Ne | Pomocí Azure Bus Service Relay v místním nasazení.
ServiceBusConnectionString | Ne | Azure Service Bus Relay připojovací řetězec.
PollToCheckData | Ne | Vyberte počet údajů používat s Spouštěč aplikací logiku (například "Vyberte počet (\*) z NEWORDERS WHERE SHIPDATE IS NULL").
PollToReadData | Ne | Příkaz SELECT pomocí služby Spouštěč aplikací logiky (například "vyberte \* z NEWORDERS SHIPDATE-li hodnota NULL pro aktualizaci").
PollToAlterData | Ne | AKTUALIZACE nebo odstranění fakturu pomocí služby Spouštěč aplikací použití logických operátorů (například "DATUMODESLÁNÍ nastavení aktualizace NEWORDERS = aktuální datum WHERE aktuální OF &lt;kurzor&gt;").

7. Vyberte **OK**a pak vyberte **vytvořit**.
8. Až budete hotovi, nastavení balíčku vypadat takto:  
![][1]


## <a name="logic-app-with-informix-connector-action-to-add-data"></a>Použití logických operátorů aplikace s Informix spojnice akcí pro přidání dat ##
Je možné definovat akci aplikace použití logických operátorů k přidání dat do tabulky Informix pomocí rozhraní API Insert nebo příspěvku operace Entity OData. Například, můžete vložit nový záznam objednávky zákazníků zpracováním SQL INSERT dotaz proti tabulce určené se sloupcem identity vrací hodnotu identity nebo řádky vliv do aplikace použití logických operátorů (vyberte ORDID z VÝSLEDNÁ tabulka (vložit do NEWORDERS (CUSTID NÁZEVZÁSILKY, SHIPADDR, MĚSTODODÁNÍ, SHIPREG, SHIPZIP) hodnoty (?,?,?,?,?,?))).

> [AZURE.TIP] Připojení databáze Informix "*vystavit entit*" vrátí hodnotu sloupce identity a vrátí "*Vložení rozhraní API*" vliv na řádky

1. V Azure startboard vyberte **+** (symbol plus), **webového + Mobile**a potom **logiky aplikace**.
2. Zadejte jméno (například "NewOrdersInformix"), plán služeb aplikací, další vlastnosti a pak vyberte **vytvořit**.
3. V Azure startboard vyberte logiky aplikaci, kterou jste právě vytvořili, **Nastavení**a potom **aktivačními událostmi a akce**.
4. V zásuvné aktivačními událostmi a akce vyberte **vytvořit přímo** v aplikaci logiky šablony.
5. Na panelu rozhraní API aplikace vyberte **opakování**, nastavit počet_plateb a interval a pak **zaškrtnutí**.
6. Na panelu rozhraní API aplikace vyberte **Informix spojnici**, rozbalte položku seznamu operace vyberte **Vložit do NEWORDER**.
7. Rozbalení seznamu parametry a zadejte tyto hodnoty:  

    Jméno | Hodnota
--- | --- 
CUSTID | 10042
ČÍSLOPŘEPRAVCE | 10000
NÁZEVZÁSILKY | Pustí K Kountry úložiště 
SHIPADDR | 12 orchestr terasovité
MĚSTODODÁNÍ | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Zaškrtněte **políčko** uložit nastavení akce a pak **Uložit**.
9. Nastavení by měl vypadat takto:  
![][3]  
10. V seznamu **všechny běží** v seznamu **operace**vyberte položku uvedené první (poslední spustit). 
11. V zásuvné **logiky aplikaci spustit** vyberte položku **Akce** **informixconnectorneworders**.
12. V zásuvné **logiku aplikace akce** vyberte **Vstupů odkaz**. Zprostředkovatel Informix spojnice používá vstupů zpracuje parametry příkaz INSERT.
13. V zásuvné **logiky aplikace akce** vyberte **Výstupy odkaz**. Vstupní hodnoty by měl vypadat takto:  
![][4]

#### <a name="what-you-need-to-know"></a>Co je potřeba vědět

- Spojnice zkrátí názvy tabulek Informix při tvořící logiky aplikace akce názvy. Třeba operace **vložte do NEWORDERS** zkrácen **do NEWORDER**vložit.
- Po uložení aplikaci logiky **aktivačními událostmi a akce**, zpracuje logiky aplikace operaci. Může být zpoždění většího počtu sekund (například 3 až 5 sekundy) před logiky aplikace zpracuje operaci. Pokud chcete můžete kliknete na **Spustit** a provedení operace.
- Zprostředkovatel Informix spojnice definuje entit členy s atributy, včetně toho, jestli člen odpovídá Informix sloupec s výchozí nebo vytvořit sloupcích (například identita). Použití logických operátorů aplikace se zobrazí červená hvězdička vedle názvu ID člena entit k označení Informix sloupce, které vyžadují hodnoty. Pro ORDID člena, který odpovídá sloupec identity Informix neměli zadejte hodnotu. Můžete zadat hodnoty u ostatních volitelné členů (položky, ORDDATE, REQDATE, ČÍSLOPŘEPRAVCE, DOPRAVNÉ, SHIPCTRY), které odpovídají Informix sloupce s výchozími hodnotami. 
- Spojnice Informix vrátí logiky aplikace odpověď na příspěvek na entit obsahující hodnoty pro sloupce identity odvozeným z SQLDARD DRDA (dat odpověď oblast dat SQL) na připravený příkaz INSERT jazyka SQL. Zprostředkovatel Informix serveru nevrací vložené hodnoty pro sloupce s výchozími hodnotami.  


## <a name="logic-app-with-informix-connector-action-to-add-bulk-data"></a>Použití logických operátorů aplikace s Informix spojnice akcí pro hromadné přidání dat ##
Je možné definovat akci aplikace logiky pro přidání dat do databáze Informix tabulky pomocí rozhraní API hromadné vložení operaci. Například, můžete vložit dva nové pořadí záznamy zákazníků, kliknutím na zpracování příkaz Vložit SQL pomocí matici hodnot v řádku proti tabulce určené se sloupcem identity vrácení řádky vliv do aplikace použití logických operátorů (vyberte ORDID z VÝSLEDNÁ tabulka (vložit do NEWORDERS (CUSTID NÁZEVZÁSILKY, SHIPADDR, MĚSTODODÁNÍ, SHIPREG, SHIPZIP) hodnoty (?,?,?,?,?,?))).

1. V Azure startboard vyberte **+** (symbol plus), **webového + Mobile**a potom **logiku aplikace**.
2. Zadejte jméno (například "NewOrdersBulkInformix"), plán služeb aplikací, další vlastnosti a pak vyberte **vytvořit**.
3. V Azure startboard vyberte logiky aplikaci, kterou jste právě vytvořili, **Nastavení**a potom **aktivačními událostmi a akce**.
4. V zásuvné aktivačními událostmi a akce vyberte **vytvořit přímo** v aplikaci logiky šablony.
5. Na panelu rozhraní API aplikace vyberte **opakování**, nastavit počet_plateb a interval a pak **zaškrtnutí**.
6. V panelu rozhraní API aplikace vyberte **Informix spojnici**, rozbalte položku seznamu operace vyberte **Hromadné vložení do nového**.
7. Zadejte hodnotu **řádků** v matici. Například zkopírujte a vložte následující:  

    ```
    [{"custid":10081,"shipid":10000,"shipname":"Trail's Head Gourmet Provisioners","shipaddr":"722 DaVinci Blvd.","shipcity":"Kirkland","shipreg":"WA","shipzip":"98034"},{"custid":10088,"shipid":10000,"shipname":"White Clover Markets","shipaddr":"305 14th Ave. S. Suite 3B","shipcity":"Seattle","shipreg":"WA","shipzip":"98128","shipctry":"USA"}]
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

- Spojnice zkrátí názvy tabulek Informix při tvořící logiky aplikace akce názvy. Třeba operace **Hromadné vložení do NEWORDERS** zkrácen **hromadné**vložit do nového.
- Databáze Informix může být malá a velká písmena na názvy tabulek a sloupců. Názvy sloupců hromadné vložení pole operace možná muset být zadán napsaných malými písmeny ("custid") místo na velká písmena ("CUSTID").
- Vynecháním identity sloupce (například ORDID), s možnou hodnotou NULL sloupce (například DATUMODESLÁNÍ) a sloupce s výchozími hodnotami (například ORDDATE REQDATE, ČÍSLOPŘEPRAVCE, DOPRAVNÉ, SHIPCTRY) databáze Informix vygeneruje hodnoty.
- Zadáním "dnes" a "zítra" Informix spojnice vygeneruje "Aktuální datum" a "Aktuální datum plus 1 den" funkce (například REQDATE). 


## <a name="logic-app-with-informix-connector-trigger-to-read-alter-or-delete-data"></a>Použití logických operátorů aplikace s Informix spojnice aktivační události pro čtení, změnit nebo odstranit data ##
Můžete definovat logiky aplikace aktivační události hlasování a číst data z tabulky databáze Informix pomocí složené operace rozhraní API hlasování Data. Například může číst jednu nebo více nové pořadí záznamy zákazníků, načtení záznamů v aplikaci použití logických operátorů. Nastavení připojení Informix balíček/aplikace by měl vypadat takto:

    Nastavení aplikace | Hodnota
--- | --- | ---
PollToCheckData | Vyberte počet (\*) z NEWORDERS, kde je SHIPDATE NULL
PollToReadData | Vyberte \* z NEWORDERS, kde je NULL aktualizace SHIPDATE
PollToAlterData | <no value specified>


Také můžete definovat Spouštěč aplikací logiku hlasování, číst a měnit data v tabulce Informix myší Data hlasování rozhraní API složené. Například může číst jednu nebo více nové pořadí záznamy zákazníků, aktualizovat hodnoty řádku vrácení vybrané (před aktualizací) záznamů v aplikaci použití logických operátorů. Nastavení připojení Informix balíček/aplikace by měl vypadat takto:

    Nastavení aplikace | Hodnota
--- | --- | ---
PollToCheckData | Vyberte počet (\*) z NEWORDERS, kde je SHIPDATE NULL
PollToReadData | Vyberte \* z NEWORDERS, kde je NULL aktualizace SHIPDATE
PollToAlterData | AKTUALIZACE NEWORDERS sadu SHIPDATE = aktuální datum kde aktuální VOLNOSTI &lt;kurzor&gt;


Kromě toho můžete definovat logiky aplikace aktivační události hlasování, přečtěte si a odebrání dat z databáze Informix tabulky pomocí operaci složené rozhraní API hlasování Data. Například může číst jednu nebo více nové pořadí záznamy zákazníků, odstranit řádky vrácení vybrané (před odstraněním) záznamů v aplikaci logiku. Nastavení připojení Informix balíček/aplikace by měl vypadat takto:

    Nastavení aplikace | Hodnota
--- | --- | ---
PollToCheckData | Vyberte počet (\*) z NEWORDERS, kde je SHIPDATE NULL
PollToReadData | Vyberte \* z NEWORDERS, kde je NULL aktualizace SHIPDATE
PollToAlterData | Odstranit NEWORDERS kde aktuální VOLNOSTI &lt;KURZORU&gt;

V tomto příkladu použití logických operátorů aplikace bude hlasování, číst, aktualizovat a znovu číst data v tabulce Informix.

1. V Azure startboard vyberte **+** (symbol plus), **webového + Mobile**a potom **logiky aplikace**.
2. Zadejte jméno (například "ShipOrdersInformix"), plán služeb aplikací, další vlastnosti a pak vyberte **vytvořit**.
3. V Azure startboard vyberte logiky aplikaci, kterou jste právě vytvořili, **Nastavení**a potom **aktivačními událostmi a akce**.
4. V zásuvné aktivačními událostmi a akce vyberte **vytvořit přímo** v aplikaci logiky šablony.
5. Na panelu rozhraní API aplikace vyberte **Informix spojnice**, nastavit počet_plateb a interval a pak **zaškrtnutí**.
6. V panelu rozhraní API aplikace vyberte **Informix spojnici**, rozbalte položku seznamu operace vyberte **vybírat NEWORDERS**.
7. Zaškrtněte **políčko** uložit nastavení akce a pak **Uložit**. Nastavení by měl vypadat takto:  
![][10]
8. Klikněte na Zavřít zásuvné **aktivačními událostmi a akce** a klikněte na Zavřít zásuvné **Nastavení** .
9. V seznamu **všechny běží** ve skupinovém rámečku **operace**klikněte na položku uvedené první (poslední spustit).
10. V zásuvné **logiky aplikaci spustit** klikněte na položku **Akce** .
11. V zásuvné **logickou aplikace akci** klikněte na **Odkaz výstupy**. Výstupy by měl vypadat takto:  
![][11]


## <a name="logic-app-with-informix-connector-action-to-remove-data"></a>Použití logických operátorů aplikace s Informix spojnice akce: Odeberte data ##
Je možné definovat akci aplikace použití logických operátorů: Odeberte data z tabulky databáze Informix pomocí rozhraní API Delete nebo příspěvku operace Entity OData. Například, můžete vložit nový záznam objednávky zákazníků zpracováním SQL INSERT dotaz proti tabulce určené se sloupcem identity vrací hodnotu identity nebo řádky vliv do aplikace použití logických operátorů (vyberte ORDID z VÝSLEDNÁ tabulka (vložit do NEWORDERS (CUSTID NÁZEVZÁSILKY, SHIPADDR, MĚSTODODÁNÍ, SHIPREG, SHIPZIP) hodnoty (?,?,?,?,?,?))).

## <a name="create-logic-app-using-informix-connector-to-remove-data"></a>Vytvoření aplikace logiku pomocí Informix Connectoru: Odeberte data ##
Můžete vytvořit novou aplikaci použití logických operátorů z v rámci webu Azure Marketplace a pak pomocí konektoru Informix jako akce odebrat objednávky zákazníků. Například, můžete použít Informix spojnice podmíněné operace odstranění se nezobrazuje zpracuje příkaz SQL DELETE (odstranit z NEWORDERS kde ORDID > = 10000).

1. V nabídce centrální vývěsky Azure **Start** , klikněte na **+** (symbol plus), klikněte na **Web + Mobile**a potom klikněte na **logiku aplikace**. 
2. Do zásuvné **vytvořit logiky aplikace** zadejte **název**, například **RemoveOrdersInformix**.
3. Vyberte nebo definovat hodnoty pro ostatní nastavení (například plán služeb, skupina zdroje).
4. Nastavení by měl vypadat takto. Klikněte na **vytvořit**:  
![][12]
5. V **Nastavení** zásuvné klikněte na **aktivační události a akce**.
6. V zásuvné **aktivačními událostmi a akce** v seznamu **aplikací logiky šablony** klikněte na **vytvořit od začátku**.
7. V zásuvné **aktivačními událostmi a akce** na panelu **Rozhraní API aplikace** v rámci skupiny zdrojů klikněte na **opakování**.
8. Na návrhové ploše logiky aplikace klikněte na položku **opakování** , nastavte **Četnost** a **Interval**, například **dny** a **1**a klikněte na **políčko** uložit nastavení opakování položky.
9. V zásuvné **aktivačními událostmi a akce** na panelu **Rozhraní API aplikace** v rámci skupiny zdrojů klikněte na **Informix spojnice**.
10. Na návrhové ploše logiky aplikace klikněte na položku Akce **Informix spojnice** , klikněte na tři tečky (****...) rozbalte seznam operace a klikněte na **podmíněné zabránění N**.
11. Na úkol Informix spojnice zadejte **ordid stránku 10000** pro **výraz, který označuje podmnožinu položky**.
12. Klikněte na **políčko** uložit nastavení akce a klepněte na tlačítko **Uložit**. Nastavení by měl vypadat takto:  
![][13]
13. Klikněte na Zavřít zásuvné **aktivačními událostmi a akce** a klikněte na Zavřít zásuvné **Nastavení** .
14. V seznamu **všechny běží** ve skupinovém rámečku **operace**klikněte na položku uvedené první (poslední spustit).
15. V zásuvné **logiky aplikaci spustit** klikněte na položku **Akce** .
16. V zásuvné **logiky aplikace akce** klikněte na **Odkaz výstupů**. Výstupy by měl vypadat takto:  
![][14]

**Poznámka:** Návrhář aplikace logiku zkrátí názvy tabulek. Třeba operace **podmíněné zabránění NEWORDERS** zkrácen **podmíněné zabránění N**.


> [AZURE.TIP] K vytvoření ukázkové tabulky a uložené procedury použijte následující příkazy SQL. 

Můžete vytvořit v ukázkové tabulce NEWORDERS pomocí následujících příkazů Informix SQL DDL:
 
    create table neworders (  
        ordid serial(10000) unique ,  
        custid int not null ,  
        empid int not null default 10000 ,  
        orddate date not null default today ,  
        reqdate date default today ,  
        shipdate date ,  
        shipid int not null default 10000 ,  
        freight decimal (9,2) not null default 0.00 ,  
        shipname char (40) not null ,  
        shipaddr char (60) not null ,  
        shipcity char (20) not null ,  
        shipreg char (15) not null ,  
        shipzip char (10) not null ,  
        shipctry char (15) not null default ''USA'' 
        )


Můžete vytvořit postup SPORDERID uložené vzorku pomocí následujícího příkazu Informix DDL:
 
    create procedure sporderid ( ord_id int)  
        returning int, int, int, date, date, date, int, decimal (9,2), char (40), char (60), char (20), char (15), char (10), char (15)  
        define xordid, xcustid, xempid, xshipid int;  
        define xorddate, xreqdate, xshipdate date;  
        define xfreight decimal (9,2);  
        define xshipname char (40);  
        define xshipaddr char (60);  
        define xshipcity char (20);  
        define xshipreg, xshipctry char (15);  
        define xshipzip char (10);  
        select ordid, custid, empid, orddate, reqdate, shipdate, shipid, freight, shipname, shipaddr, shipcity, shipreg, shipzip, shipctry  
            into xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry  
            from neworders where ordid = ord_id;  
        return xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry;  
    end procedure; 


## <a name="hybrid-configuration-optional"></a>Hybridní konfigurace (volitelné)

> [AZURE.NOTE] Tento krok je vyžadován jenom v případě, že používáte DB2 spojnice místní za brány firewall.

Služba aplikace pomocí Správce konfigurace hybridního zabezpečené připojení k místní systém. Pokud spojnice používá místním IBM DB2 serveru pro systém Windows, hybridní Connection Manager je vyžadován.

V tématu [použití hybridního Connection Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Další možnosti s na spojnici
Teď, když se vytvoří spojnice, můžete ho přidat do pracovního postupu firmy pomocí aplikace logiky. V tématu [Jaké aplikace logiky?](app-service-logic-what-are-logic-apps.md).

Vytvářejte rozhraní API aplikace pomocí rozhraní REST API. V tématu [spojnic a rozhraní API aplikace odkaz](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Můžete taky zkontrolovat Statistika a řízení jistoty ke spojnici připojit. V tématu [Správa a sledování předdefinované rozhraní API aplikace a spojnic](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-informix/ApiApp_InformixConnector_Create.png
[2]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Create.png
[3]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_TriggersActions.png
[4]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Outputs.png
[5]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Create.png
[6]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_TriggersActions.png
[7]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Inputs.png
[8]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Outputs.png
[9]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Create.png
[10]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_TriggersActions.png
[11]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Outputs.png
[12]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Create.png
[13]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_TriggersActions.png
[14]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Outputs.png


