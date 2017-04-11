<properties
    pageTitle="Přidání spojnice DB2 v aplikacích použití logických operátorů | Microsoft Azure"
    description="Základní informace o DB2 spojnice s parametry rozhraní REST API"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# <a name="get-started-with-the-db2-connector"></a>Začínáme s konektoru DB2
Konektor aplikace Microsoft pro DB2 připojí aplikace použití logických operátorů k uložených v databázi IBM DB2 prostředků. Tato spojnice obsahuje klienta Microsoft komunikovat s vzdálených počítačů DB2 serveru v síti TCP/IP. To obsahuje cloudu databáze IBM Bluemix dashDB ATP IBM DB2 pro systém Windows spuštěné v Azure virtualization a místní databází používat bránu místní data. Podívejte se [podporované seznamu](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2 platformy a verze (v tomto tématu).

>[AZURE.NOTE] Tuto verzi článku platí pro aplikace logiky všeobecně dostupná (GA). 

Konektor DB2 podporuje tyto operace databáze:

- Seznam databázových tabulek
- Pomocí vyberte jeden řádek pro čtení
- Přečtěte si pomocí vyberte všechny řádky
- Přidání jednoho řádku příkazu Vložit
- Příkaz ALTER jeden řádek pomocí aktualizace
- Odebrání jednoho řádku pomocí odstranit

V tomto tématu se dozvíte, jak používat konektoru aplikace logiky operací databázi obrázku.

Další informace o použití logických operátorů aplikacích, najdete v článku [Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="available-actions"></a>Dostupné akce
Konektor DB2 podporuje následující akce aplikace použití logických operátorů:

- GetTables
- GetRow
- GetRows
- InsertRow
- UpdateRow
- DeleteRow


## <a name="list-tables"></a>Seznam tabulek
Vytvoření aplikace logiky pro všechny operace se skládá z několika krocích provést pomocí portálu Microsoft Azure.

V aplikaci logiky můžete přidat akci do seznamu tabulek v databázi DB2. Akce pokyn spojnice tak, aby zpracovat DB2 schématu příkaz, například `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1.  V **Azure start vývěsky**, vyberte **+** (symbol plus), **webového + Mobile**a potom **Logiku aplikace**.
2.  Zadejte **název**, jako například `Db2getTables`, **předplatné** **pole Skupina zdroje**, **umístění**a **Plán služeb aplikací**. Vyberte **Připnout do řídicího panelu**a pak vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1.  V **Aplikace Návrhář použití logických operátorů**vyberte **Prázdné LogicApp** v seznamu **šablony** .
2.  Vyberte v seznamu **aktivační události** **opakování**. 
3.  V **opakování** aktivační událost vyberte **Upravit**, vyberte **Četnost** rozevíracího seznamu vyberte **den**a nastavte **Interval** , který má typ **7**.  
4.  Zaškrtněte políčko **+ nový krok** a pak vyberte **Přidat akci**.
5.  V seznamu **Akce** zadejte `db2` do pole **Vyhledat další akce** textové pole a vyberte **DB2 - dostat tabulek (verze Preview)**.

    ![](./media/connectors-create-api-db2/Db2connectorActions.png)  

6.  V podokně Konfigurace **DB2 - dostat tabulek** vyberte **zaškrtávací políčko** povolit **připojení prostřednictvím místní data brány**. Všimněte si, že nastavení změnit z cloudu pro místní.
    - Zadejte hodnotu pro **Server**ve formě číslo portu dvojtečka adresu nebo alias. Zadejte například `ibmserver01:50000`.
    - Zadejte hodnotu pro **databázi**. Zadejte například `nwind`.
    - Vyberte hodnotu pro **ověřování**. Vyberte například **základní**.
    - Zadejte hodnotu pro **uživatelské jméno**. Zadejte například `db2admin`.
    - Zadejte hodnotu pro **heslo**. Zadejte například `Password1`.
    - Vyberte hodnotu pro **brány**. Vyberte například **datagateway01**.
7. Vyberte **vytvořit**a pak vyberte **Uložit**. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

8.  V **Db2getTables** zásuvné v seznamu **všechny běží** v části **Souhrn**vyberte položku uvedené první (poslední spustit).
9.  V zásuvné **logiky aplikaci spustit** vyberte **Spustit podrobnosti**. V seznamu **Akce** vyberte **Get_tables**. Naleznete v tématu hodnotu **stavu**, které by měl být **byl úspěšný**. Vyberte **odkaz vstupů** zobrazíte vstupní hodnoty. Vyberte **výstupy odkaz**a zobrazit výstupy; které by měl obsahovat seznamu tabulek.

    ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Vytvoření připojení
Tato spojnice podporuje připojení databáze hostované místní a v cloudu pomocí následující vlastnosti připojení. 

Vlastnost | Popis
--- | ---
Server | Povinné. Přijme řetězcovou hodnotu, která představuje TCP/IP adresu nebo alias ve formátu IPv4 nebo IPv6 zahájen (dvojtečka oddělený tabulátory) tak, že číslo portu TCP/IP. 
databáze | Povinné. Přijme řetězec, který představuje DRDA název pro relační databáze (RDBNAM). DB2 z/OS přijme řetězec 16 bajtů (databáze se označuje jako IBM DB2 umístění z/OS). DB2 i5/OS přijme 18 8bajtový typ string (databáze se označuje jako IBM DB2 pro i relační databáze). DB2 LUW přijme 8bajtový typ string.
ověřování | Volitelné. Zpracuje hodnotu položky seznamu, Basic a Windows (kerberos). 
uživatelské jméno | Povinné. Je možné zadat hodnotu řetězce. DB2 z/OS přijme 8bajtový typ string. DB2 pro i přijme 10 8bajtový typ string. DB2 Linux nebo UNIX přijme 8bajtový typ string. DB2 pro Windows přijme řetězec bajt 30.
heslo | Povinné. Je možné zadat hodnotu řetězce.
Brána | Povinné. Přijme hodnotou položky seznamu představující místní brána dat definované k aplikacím použití logických operátorů ve skupině úložiště.  

## <a name="create-the-on-premises-gateway-connection"></a>Vytvoření místní připojení brány
Tato spojnice přístup k databázi DB2 místní pomocí místní brány. Další informace naleznete v tématech brány. 

1. V podokně Konfigurace **brány** zaškrtněte **políčko** povolit **připojení přes bránu**. Všimněte si, že nastavení změnit z cloudu pro místní.
2. Zadejte hodnotu pro **Server**ve formě číslo portu dvojtečka adresu nebo alias. Zadejte například `ibmserver01:50000`.
3. Zadejte hodnotu pro **databázi**. Zadejte například `nwind`.
4. Vyberte hodnotu pro **ověřování**. Vyberte například **základní**.
5. Zadejte hodnotu pro **uživatelské jméno**. Zadejte například `db2admin`.
6. Zadejte hodnotu pro **heslo**. Zadejte například `Password1`.
7. Vyberte hodnotu pro **brány**. Vyberte například **datagateway01**.
8. Výběrem možnosti **vytvořit** pokračovat. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Vytvoření připojení ke cloudu
Tato spojnice můžete připojit ke cloudové DB2 databázi. 

1. V podokně Konfigurace **brány** opustit **zaškrtávacího políčka** zakázat (nepoužité) **Připojit pomocí brány**. 
2. Zadejte hodnotu pro **název připojení**. Zadejte například `hisdemo2`.
3. **Název serveru DB2**ve formě číslo portu dvojtečka adresu nebo alias zadejte hodnotu. Zadejte například `hisdemo2.cloudapp.net:50000`.
3. Zadejte hodnotu pro **název databáze DB2**. Zadejte například `nwind`.
4. Zadejte hodnotu pro **uživatelské jméno**. Zadejte například `db2admin`.
5. Zadejte hodnotu pro **heslo**. Zadejte například `Password1`.
6. Výběrem možnosti **vytvořit** pokračovat. 

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Vzdálené použití pomocí vyberte všechny řádky
Je možné definovat akci aplikace logiky vzdáleně všechny řádky v tabulce DB2. Tím se nastaví spojnice tak, aby zpracovat příkaz DB2 SELECT, jako například `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1.  V **Azure start vývěsky**, vyberte **+** (symbol plus), **webového + Mobile**a potom **Logiku aplikace**.
2.  Zadejte **název**, jako například `Db2getRows`, **předplatné** **pole Skupina zdroje**, **umístění**a **Plán služeb aplikací**. Vyberte **Připnout do řídicího panelu**a pak vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1.  V **Aplikace Návrhář použití logických operátorů**vyberte **Prázdné LogicApp** v seznamu **šablony** .
2.  Vyberte v seznamu **aktivační události** **opakování**. 
3.  V **opakování** aktivační událost vyberte **Upravit**, vyberte **Četnost** rozevíracího seznamu vyberte **den**a potom vyberte **Interval** zadejte **7**. 
4.  Zaškrtněte políčko **+ nový krok** a pak vyberte **Přidat akci**.
5.  V seznamu **Akce** zadejte `db2` do pole **Vyhledat další akce** textové pole a potom vyberte **DB2 - získat řádky (verze Preview)**.
6. V této akci **získat řádky (verze Preview)** vyberte **změnit připojení**.
7. V podokně Konfigurace **připojení** vyberte **vytvořit nový**. 

    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
  
8. V podokně Konfigurace **brány** opustit **zaškrtávacího políčka** zakázat (nepoužité) **Připojit pomocí brány**.
    - Zadejte hodnotu pro **název připojení**. Zadejte například `HISDEMO2`.
    - **Název serveru DB2**ve formě číslo portu dvojtečka adresu nebo alias zadejte hodnotu. Zadejte například `HISDEMO2.cloudapp.net:50000`.
    - Zadejte hodnotu pro **název databáze DB2**. Zadejte například `NWIND`.
    - Zadejte hodnotu pro **uživatelské jméno**. Zadejte například `db2admin`.
    - Zadejte hodnotu pro **heslo**. Zadejte například `Password1`.
9. Výběrem možnosti **vytvořit** pokračovat.

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

10. V seznamu **název tabulky** vyberte **šipku dolů**a potom vyberte položku **oblast**.
11. Pokud chcete klikněte na **Zobrazit upřesňující možnosti** a zadejte nastavení dotazu.
12. Vyberte **Uložit**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)

13. V **Db2getRows** zásuvné v seznamu **všechny běží** v části **Souhrn**vyberte položku uvedené první (poslední spustit).
14. V zásuvné **logiky aplikaci spustit** vyberte **Spustit podrobnosti**. V seznamu **Akce** vyberte **Get_rows**. Naleznete v tématu hodnotu **stavu**, které by měl být **byl úspěšný**. Vyberte **odkaz vstupů** zobrazíte vstupní hodnoty. Vyberte **výstupy odkaz**a zobrazit výstupy; které by měl obsahovat seznam řádků.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Přidání jednoho řádku příkazu Vložit
Můžete definovat logickou aplikace akci přidáte jeden řádek v tabulce DB2. Tato akce nastaví konektoru zpracuje příkazu Vložit DB2, jako například `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1.  V **Azure start vývěsky**, vyberte **+** (symbol plus), **webového + Mobile**a potom **Logiku aplikace**.
2.  Zadejte **název**, jako například `Db2insertRow`, **předplatné** **pole Skupina zdroje**, **umístění**a **Plán služeb aplikací**. Vyberte **Připnout do řídicího panelu**a pak vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1.  V **Aplikace Návrhář použití logických operátorů**vyberte **Prázdné LogicApp** v seznamu **šablony** .
2.  Vyberte v seznamu **aktivační události** **opakování**. 
3.  V **opakování** aktivační událost vyberte **Upravit**, vyberte **Četnost** rozevíracího seznamu vyberte **den**a potom vyberte **Interval** zadejte **7**. 
4.  Zaškrtněte políčko **+ nový krok** a pak vyberte **Přidat akci**.
5.  V seznamu **Akce** **db2** zadejte do textového pole **Hledat další akce** a pak vyberte **DB2 - vložit řádek (verze Preview)**.
6. V této akci **získat řádky (verze Preview)** vyberte **změnit připojení**. 
7. V podokně Konfigurace **připojení** vyberte připojení. Vyberte například **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. V seznamu **název tabulky** vyberte **šipku dolů**a potom vyberte položku **oblast**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červená hvězdička). Zadejte například `99999` **AREAID**, zadejte `Area 99999`a zadejte `102` pro **REGIONID**. 
10. Vyberte **Uložit**.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
 
11. V **Db2insertRow** zásuvné v seznamu **všechny běží** v části **Souhrn**vyberte položku uvedené první (poslední spustit).
12. V zásuvné **logiky aplikaci spustit** vyberte **Spustit podrobnosti**. V seznamu **Akce** vyberte **Get_rows**. Naleznete v tématu hodnotu **stavu**, které by měl být **byl úspěšný**. Vyberte **odkaz vstupů** zobrazíte vstupní hodnoty. Vyberte **výstupy odkaz**a zobrazit výstupy; které by měl obsahovat nový řádek.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Vzdálené použití pomocí vyberte jeden řádek
Je možné definovat akci aplikace logiky vzdáleně jeden řádek v tabulce DB2. Tato akce nastaví konektoru výkaz DB2 vyberte místo, KAM zpracuje jako `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1.  V **Azure start vývěsky**, vyberte **+** (symbol plus), **webového + Mobile**a potom **Logiku aplikace**.
2.  Zadejte **název** (například "**Db2getRow**"), **předplatné**, **Skupina zdroje**, **umístění**a **Plán služeb aplikací**. Vyberte **Připnout do řídicího panelu**a pak vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1.  V **Aplikace Návrhář použití logických operátorů**vyberte **Prázdné LogicApp** v seznamu **šablony** . 
2.  Vyberte v seznamu **aktivační události** **opakování**. 
3.  V **opakování** aktivační událost vyberte **Upravit**, vyberte **Četnost** rozevíracího seznamu vyberte **den**a potom vyberte **Interval** zadejte **7**. 
4.  Zaškrtněte políčko **+ nový krok** a pak vyberte **Přidat akci**.
5.  V seznamu **Akce** **db2** zadejte do textového pole **Hledat další akce** a pak vyberte **DB2 - získat řádky (verze Preview)**.
6. V této akci **získat řádky (verze Preview)** vyberte **změnit připojení**. 
7. V podokně Konfigurace **připojení** vyberte existující připojení. Vyberte například **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. V seznamu **název tabulky** vyberte **šipku dolů**a potom vyberte položku **oblast**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červená hvězdička). Zadejte například `99999` pro **AREAID**. 
10. Pokud chcete klikněte na **Zobrazit upřesňující možnosti** a zadejte nastavení dotazu.
11. Vyberte **Uložit**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)

12. V **Db2getRow** zásuvné v seznamu **všechny běží** v části **Souhrn**vyberte položku uvedené první (poslední spustit).
13. V zásuvné **logiky aplikaci spustit** vyberte **Spustit podrobnosti**. V seznamu **Akce** vyberte **Get_rows**. Naleznete v tématu hodnotu **stavu**, které by měl být **byl úspěšný**. Vyberte **odkaz vstupů** zobrazíte vstupní hodnoty. Vyberte **výstupy odkaz**a zobrazit výstupy; které by měl obsahovat řádku.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Změna jednoho řádku pomocí aktualizace
Je možné definovat akci aplikace logiky změnit jeden řádek v tabulce DB2. Tato akce nastaví konektoru zpracuje příkaz DB2 UPDATE, jako například `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1.  V **Azure start vývěsky**, vyberte **+** (symbol plus), **webového + Mobile**a potom **Logiku aplikace**.
2.  Zadejte **název**, jako například `Db2updateRow`, **předplatné** **pole Skupina zdroje**, **umístění**a **Plán služeb aplikací**. Vyberte **Připnout do řídicího panelu**a pak vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1.  V **Aplikace Návrhář použití logických operátorů**vyberte **Prázdné LogicApp** v seznamu **šablony** .
2.  Vyberte v seznamu **aktivační události** **opakování**. 
3.  V **opakování** aktivační událost vyberte **Upravit**, vyberte **Četnost** rozevíracího seznamu vyberte **den**a potom vyberte **Interval** zadejte **7**. 
4.  Zaškrtněte políčko **+ nový krok** a pak vyberte **Přidat akci**.
5.  V seznamu **Akce** **db2** zadejte do textového pole **Hledat další akce** a pak vyberte **DB2 – aktualizace řádku (verze Preview)**.
6. V této akci **získat řádky (verze Preview)** vyberte **změnit připojení**. 
7. V podokně Konfigurace **připojení** vyberte vybrat existující připojení. Vyberte například **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. V seznamu **název tabulky** vyberte **šipku dolů**a potom vyberte položku **oblast**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červená hvězdička). Zadejte například `99999` **AREAID**, zadejte `Updated 99999`a zadejte `102` pro **REGIONID**. 
10. Vyberte **Uložit**. 

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)

11. V **Db2updateRow** zásuvné v seznamu **všechny běží** v části **Souhrn**vyberte položku uvedené první (poslední spustit).
12. V zásuvné **logiky aplikaci spustit** vyberte **Spustit podrobnosti**. V seznamu **Akce** vyberte **Get_rows**. Naleznete v tématu hodnotu **stavu**, které by měl být **byl úspěšný**. Vyberte **odkaz vstupů** zobrazíte vstupní hodnoty. Vyberte **výstupy odkaz**a zobrazit výstupy; které by měl obsahovat nový řádek.

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Odebrání jednoho řádku pomocí odstranit
Je možné definovat akci aplikace logiku odebrat jeden řádek v tabulce DB2. Tato akce nastaví spojnice tak, aby zpracovat DB2 odstranit příkaz, například `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1.  V **Azure start vývěsky**, vyberte **+** (symbol plus), **webového + Mobile**a potom **Logiku aplikace**.
2.  Zadejte **název**, jako například `Db2deleteRow`, **předplatné** **pole Skupina zdroje**, **umístění**a **Plán služeb aplikací**. Vyberte **Připnout do řídicího panelu**a pak vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1.  V **Aplikace Návrhář použití logických operátorů**vyberte **Prázdné LogicApp** v seznamu **šablony** . 
2.  Vyberte v seznamu **aktivační události** **opakování**. 
3.  V **opakování** aktivační událost vyberte **Upravit**, vyberte **Četnost** rozevíracího seznamu vyberte **den**a potom vyberte **Interval** zadejte **7**. 
4.  Zaškrtněte políčko **+ nový krok** a pak vyberte **Přidat akci**.
5.  V seznamu **Akce** vyberte **db2** do textového pole **Hledat další akce** a pak vyberte **DB2 – odstranění řádku (verze Preview)**.
6. V této akci **získat řádky (verze Preview)** vyberte **změnit připojení**. 
7. V podokně Konfigurace **připojení** vyberte existující připojení. Vyberte například **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. V seznamu **název tabulky** vyberte **šipku dolů**a potom vyberte položku **oblast**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červená hvězdička). Zadejte například `99999` pro **AREAID**. 
10. Vyberte **Uložit**. 

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)

11. V **Db2deleteRow** zásuvné v seznamu **všechny běží** v části **Souhrn**vyberte položku uvedené první (poslední spustit).
12. V zásuvné **logiky aplikaci spustit** vyberte **Spustit podrobnosti**. V seznamu **Akce** vyberte **Get_rows**. Naleznete v tématu hodnotu **stavu**, které by měl být **byl úspěšný**. Vyberte **odkaz vstupů** zobrazíte vstupní hodnoty. Vyberte **výstupy odkaz**a zobrazit výstupy; které by měl obsahovat odstraněný řádek.

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>Podrobné technické informace

## <a name="actions"></a>Akce
Akce je operace prováděné definované v aplikaci logiky pracovního postupu. Databázového konektoru DB2 zahrnuje následující akce. 

|Akce|Popis|
|--- | ---|
|[GetRow](connectors-create-api-db2.md#get-row)|Zkopíruje do jednoho řádku z tabulky DB2|
|[GetRows](connectors-create-api-db2.md#get-rows)|Načte řádků z tabulky DB2|
|[InsertRow](connectors-create-api-db2.md#insert-row)|Vloží nový řádek tabulky DB2|
|[DeleteRow](connectors-create-api-db2.md#delete-row)|Odstranění řádku z tabulky DB2|
|[GetTables](connectors-create-api-db2.md#get-tables)|Načte tabulek z databáze DB2|
|[UpdateRow](connectors-create-api-db2.md#update-row)|Aktualizuje existující řádek v tabulce DB2|

### <a name="action-details"></a>Podrobnosti o akci

V této části najdete v článku nějaké informace o jednotlivých akci, včetně všechny požadované nebo volitelné vstupní vlastnosti a odpovídající výstup přidružené spojnice.

#### <a name="get-row"></a>Získání řádku 
Zkopíruje do jednoho řádku z tabulky DB2.  

| Název vlastnosti| Zobrazované jméno |Popis|
| ---|---|---|
|Tabulka * | Název tabulky |Název tabulky DB2|
|ID * | Identifikátor řádku |Jedinečný identifikátor řádku k načtení|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Položky

| Název vlastnosti | Datový typ |
|---|---|
|ItemInternalId|řetězec|


#### <a name="get-rows"></a>Získání řádků 
Načte řádků z tabulky DB2.  

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Název tabulky|Název tabulky DB2|
|$skip|Přeskočit počet|Počet faktur od přeskočit (výchozí = 0)|
|$top|Získání maximální počet|Maximální počet položek k načtení (výchozí = 256)|
|$filter|Filtrování dotazu|Dotaz filtru ODATA omezení počtu položek|
|$orderby|Řadit podle|Dotaz ODATA řadit podle určete pořadí položek|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
ItemsList

| Název vlastnosti | Datový typ |
|---|---|
|Hodnota|pole|


#### <a name="insert-row"></a>Vložení řádku 
Vloží nový řádek tabulky DB2.  

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Název tabulky|Název tabulky DB2|
|položky *|Řádek|Řádek vložit do zadané tabulky v DB2|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Položky

| Název vlastnosti | Datový typ |
|---|---|
|ItemInternalId|řetězec|


#### <a name="delete-row"></a>Odstranění řádku 
Odstranění řádku z tabulky DB2.  

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Název tabulky|Název tabulky DB2|
|ID *|Identifikátor řádku|Jedinečný identifikátor řádku, až odstranit|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.

#### <a name="get-tables"></a>Získání tabulek 
Načte tabulek z databáze DB2.  

Neexistují žádné parametry pro tento hovor. 

##### <a name="output-details"></a>Podrobnosti výstupu 
TablesList

| Název vlastnosti | Datový typ |
|---|---|
|Hodnota|pole|

#### <a name="update-row"></a>Aktualizace řádku 
Aktualizuje existující řádek v tabulce DB2.  

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Tabulka *|Název tabulky|Název tabulky DB2|
|ID *|Identifikátor řádku|Jedinečný identifikátor řádku Aktualizovat|
|položky *|Řádek|Řádek s aktualizovanými hodnotami|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu  
Položky

| Název vlastnosti | Datový typ |
|---|---|
|ItemInternalId|řetězec|


### <a name="http-responses"></a>Odpovědi na HTTP

Při volání různé kroky, může se zobrazit některé odpovědi. Následující tabulka popisuje odpovědi a jejich popisu:  

|Jméno|Popis|
|---|---|
|200|Ok|
|202|Přijaté|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="supported-db2-platforms-and-versions"></a>Podporované platformy DB2 a verze
Tato spojnice podporuje následující IBM DB2 platformy a verze, stejně jako IBM DB2 kompatibilní produkty (například IBM Bluemix dashDB), které podporují Distributed relační databáze architektura (DRDA) SQL přístup správce (SQLAM) verze 10 a 11:

- Databáze IBM DB2 pro z 11.1 operační systém
- Databáze IBM DB2 pro z 10.1 operační systém
- Databáze IBM DB2 pro i 7.3
- Databáze IBM DB2 pro i 7.2
- Databáze IBM DB2 pro i 7.1
- Databáze IBM DB2 LUW 11
- Databáze IBM DB2 pro LUW 10.5


## <a name="next-steps"></a>Další kroky

[Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md). Prozkoumejte jiných spojnice k dispozici v aplikacích pro použití logických operátorů v našem [seznamu rozhraní API](apis-list.md).

