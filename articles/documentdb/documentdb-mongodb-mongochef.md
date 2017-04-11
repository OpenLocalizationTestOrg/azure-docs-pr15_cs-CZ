<properties 
    pageTitle="Použití MongoChef pod svým účtem DocumentDB s podporou protokolu MongoDB | Microsoft Azure" 
    description="Naučte se používat MongoChef pod svým účtem DocumentDB s podporou protokolu MongoDB, teď k dispozici pro náhled." 
    keywords="mongochef"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="anhoh"/>

# <a name="use-mongochef-with-a-documentdb-account-with-protocol-support-for-mongodb"></a>Použití MongoChef pod svým účtem DocumentDB s podporou protokolu MongoDB

Připojení k účtu Azure DocumentDB s podporou protokolu MongoDB pomocí MongoChef, musíte:

- Stažení a instalace [MongoChef](http://3t.io/mongochef)
- Máte účet DocumentDB s podporou protokolu MongoDB [připojovací řetězec](documentdb-connect-mongodb-account.md) informace

## <a name="create-the-connection-in-mongochef"></a>Vytvoření připojení v MongoChef  

Přidat svůj účet DocumentDB s podporou protokolu MongoDB vedoucímu MongoChef připojení, proveďte následující kroky.

1. Načtení DocumentDB s podporou protokolu informace o připojení MongoDB postupujte podle pokynů [v tomto poli](documentdb-connect-mongodb-account.md).

    ![Snímek obrazovky s zásuvné řetězec připojení](./media/documentdb-mongodb-mongochef/ConnectionStringBlade.png)

2. Klikněte na **Připojit** k otevření Connection Manager a potom klikněte na **Nové připojení**

    ![Snímek obrazovky s odpovědi MongoChef připojení](./media/documentdb-mongodb-mongochef/ConnectionManager.png)
    
2. V okně **Nové připojení** klikněte na kartě **Server** zadejte hostitele (FQDN) DocumentDB účet s podporou protokolu MongoDB a číslo portu.
    
    ![Snímek obrazovky s kartou MongoChef připojení správce serveru](./media/documentdb-mongodb-mongochef/ConnectionManagerServerTab.png)

3. V okně **Nové připojení** klikněte na kartu **ověřování** vyberte režim ověřování **směrodatnou (MONGODB CR nebo SCARM-SHA-1)** a zadejte uživatelské jméno a heslo.  Přijměte výchozí ověřování databáze (Správci) nebo zadejte vlastní hodnota.

    ![Snímek obrazovky s kartou ověřování MongoChef connection manager](./media/documentdb-mongodb-mongochef/ConnectionManagerAuthenticationTab.png)

4. V okně **Nové připojení** klikněte na kartě **SSL** zaškrtněte políčko **použít protokol SSL protokol sloužící k připojení** a přepínací tlačítko **přijmout podepsaného certifikáty SSL** .

    ![Snímek obrazovky s kartou SSL MongoChef connection manager](./media/documentdb-mongodb-mongochef/ConnectionManagerSSLTab.png)

5. Kliknutím na tlačítko **Testovat připojení** ověřte informace o připojení, klikněte na **OK** se vrátíte do okna nové připojení a pak klikněte na **Uložit**.

    ![Snímek obrazovky s oknem MongoChef test připojení](./media/documentdb-mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a>Vytvoření databáze, shromažďování a dokumentů pomocí MongoChef  

Pokud chcete vytvořit databázi, shromažďování a dokumentů pomocí MongoChef, proveďte následující kroky.

1. Ve **Správci připojení**zvýrazněte připojení a klikněte na **Připojit**.

    ![Snímek obrazovky s odpovědi MongoChef připojení](./media/documentdb-mongodb-mongochef/ConnectToAccount.png)

2. Klikněte pravým tlačítkem myši Host (hostitel) a zvolte **Přidat databázi**.  Zadejte název databáze a klikněte na **OK**.
    
    ![Snímek obrazovky s možností přidat databázi MongoChef](./media/documentdb-mongodb-mongochef/AddDatabase1.png)

3. Klikněte pravým tlačítkem myši databázi a zvolte **Přidat kolekce**.  Zadejte název kolekce a klikněte na **vytvořit**.

    ![Snímek obrazovky s možností přidat kolekce MongoChef](./media/documentdb-mongodb-mongochef/AddCollection.png)

4. Klikněte na položku **kolekce** a potom klikněte na **Přidat dokument**.

    ![Snímek obrazovky s položkou nabídky MongoChef přidat dokument](./media/documentdb-mongodb-mongochef/AddDocument1.png)

5. V dialogovém okně Přidat dokument vložte následující a pak klikněte na **Přidat dokument**.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
            { "firstName": "Thomas" },
            { "firstName": "Mary Kay"}
        ],
        "children": [
        {
            "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
            "pets": [{ "givenName": "Fluffy" }]
        }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }

    
6. Přidání jiného dokumentu, tuto dobu následující obsah.

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                "givenName": "Lisa", 
                "gender": "female", 
                "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }

7. Spuštění dotazu na vzorku. Příklad hledání pro rodiny se příjmení "Andersen" a vraťte se rodičům a pole Stav.

    ![Snímek obrazovky s Mongo Chef výsledky dotazu](./media/documentdb-mongodb-mongochef/QueryDocument1.png)
    

## <a name="next-steps"></a>Další kroky

- Prozkoumejte DocumentDB s podporou protokolu MongoDB [vzorky](documentdb-mongodb-samples.md).

 
