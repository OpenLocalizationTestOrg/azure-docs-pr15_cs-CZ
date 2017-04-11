<properties 
    pageTitle="Připojení k účtu DocumentDB s podporou protokolu MongoDB | Microsoft Azure" 
    description="Zjistěte, jak se připojit k účtu DocumentDB s podporou protokolu MongoDB, teď k dispozici pro náhled. Připojení pomocí MongoDB připojovací řetězec." 
    keywords="mongodb připojovacího řetězce"
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
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>Jak se připojit k účtu DocumentDB s podporou protokolu MongoDB

Zjistěte, jak se připojit k účtu Azure DocumentDB s podporou protokolu MongoDB pomocí standardní formát URI MongoDB připojovacího řetězce.  

## <a name="get-the-accounts-connection-string-information"></a>Získat informace o připojení řetězec účtu

1. V novém okně Přihlaste se k [Portálu Azure](https://portal.azure.com).
2. V **Levém navigačním** panelu zásuvné účtu klikněte na **Připojovací řetězec**. Přejděte na **Zásuvné účtu**v Jumpbar klikněte na **Další služby**, klikněte na **DocumentDB (NoSQL)** a pak vyberte účet DocumentDB s podporou protokolu MongoDB.

    ![Snímek obrazovky s zásuvné všechna nastavení](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. **Informace připojovacího řetězce** zásuvné otevře a obsahuje všechny informace potřebné pro připojení k účtu pomocí ovladače pro MongoDB, včetně předběžně vypočtená připojovacího řetězce.

    ![Snímek obrazovky s zásuvné řetězec připojení](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Požadavky na řetězec připojení

Je důležité mít na paměti, že DocumentDB podporuje standardní MongoDB připojení URI formát řetězce, pomocí několika specifických požadavků: DocumentDB účty vyžadují ověřování a zabezpečená komunikace přes SSL.  Formát připojovacího řetězce tedy:

    mongodb://username:password@host:port/[database]?ssl=true

Kde jsou dostupné v zásuvné připojovací řetězec uveden nad hodnoty tohoto řetězce.

- Uživatelské jméno (povinné)
    - Název účtu DocumentDB
- Heslo (povinné)
    - Heslo účtu DocumentDB
- Host (povinné)
    - Plně kvalifikovaný název domény DocumentDB účtu
- Port (povinné)
    - 10250
- Databáze (volitelné)
    - Výchozí databázi používá připojení
- SSL = true (povinné)

Zvažte například účet podle výše uvedených informace připojovacího řetězce.  Je platný připojovací řetězec:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>Připojení pomocí ovladače C# pro MongoDB
Zmíněná už všechny účty DocumentDB vyžadují ověřování a zabezpečená komunikace přes SSL. Zatímco URI formát MongoDB připojovacího řetězce podporuje ssl = parametr true dotazu, práci s C MongoDB # ovladač vyžaduje použití objektu MongoClientSettings při vytváření MongoClient.  Možnost výše uvedených informací o účtu, následující fragment kódu ukazuje, jak se můžete připojit k tomuto účtu a práce s databází "Úkoly".

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Další kroky


- Zjistěte, jak chcete [použít MongoChef](documentdb-mongodb-mongochef.md) pod svým účtem DocumentDB s protokolem domovské stránce podpory pro MongoDB.
- Prozkoumejte DocumentDB s podporou protokolu MongoDB [vzorky](documentdb-mongodb-samples.md).

 
