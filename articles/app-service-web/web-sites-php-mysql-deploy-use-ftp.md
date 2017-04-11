<properties 
    pageTitle="Vytvořit web appu PHP MySQL v aplikaci služby Azure a nasazení pomocí FTP" 
    description="Kurz, který ukazuje, jak vytvořit web appu PHP, který uchovává data v MySQL a jejich použití nasazení FTP Azure." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Vytvořit web appu PHP MySQL v aplikaci služby Azure a nasazení pomocí FTP

Tento kurz se dozvíte, jak vytvořit PHP MySQL web appu a jak ji pomocí protokolu FTP nasadit. Tento kurz předpokládá [PHP][install-php], [MySQL][install-mysql], webového serveru a klient FTP v počítači nainstalovaný. Pokyny v tomto kurzu může být zahájen na žádný operační systém, včetně systému Windows, Mac a Linux. Po dokončení tohoto průvodce, budete mít do webových aplikací PHP/MySQL spuštěné v Azure.
 
Naučíte se:

* Jak vytvořit web app a databáze MySQL pomocí portálu Azure. Protože PHP je standardně ve webových aplikacích, žádné zvláštní je potřeba spustit PHP kód.
* Jak publikovat aplikaci Azure pomocí protokolu FTP.
 
Provedením tohoto kurzu vytvoříte jednoduché registrace web app v PHP. Aplikace se použitý ve Web Appu. Snímek obrazovky s dokončeným aplikace je níže:

![Azure PHP webu][running-app]

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Bez kreditní karty potřeby žádné závazky. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Vytvořit web appu a nastavit FTP publikování

Tímto postupem vytvořte web app a databáze MySQL:

1. Přihlášení k [portálu Azure][management-portal].
2. Klikněte na ikonu **+ Nový** horní levé části portálu Azure.

    ![Vytvoření nového webu Azure][new-website]

3. Do pole Hledat zadejte **v prohlížeči + MySQL** a klikněte na **Web appu + MySQL**.

    ![Vlastní vytvořit nový web][custom-create]

4. Klikněte na **vytvořit**. Zadejte název jedinečné aplikace služby, platný název skupiny zdrojů a nový plán služeb.

    ![Název skupiny nastavení zdroje][resource-group]


6. Zadejte hodnoty pro novou databázi, včetně souhlasit a právní podmínek.

    ![Vytvořit novou databázi MySQL][new-mysql-db]
    
7. Po vytvoření web appu zobrazí se nové služby zásuvné aplikace.


6. Klikněte na možnost **Nastavení** > **Nasazení pověření**. 

    ![Nasazení nastavit přihlašovací údaje][set-deployment-credentials]

7. Povolit FTP publikování, je nutné zadat uživatelské jméno a heslo. Uložit přihlašovací údaje a poznamenejte si uživatelské jméno a heslo, které vytvoříte.

    ![Vytvoření mapování přihlašovacích publikování][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Vytvořte a otestujte aplikace místně

Registrace aplikace je jednoduchý PHP aplikace, která umožňuje registrovat události poskytnutím vaše jméno a e-mailovou adresu. Zobrazí se informace o předchozí zaregistrované účastníky v tabulce. Registrační informace uložené v databázi MySQL. Aplikace se skládá ze dvou souborů:

* **index.php**: slouží k zobrazení formuláře pro registraci a tabulkou obsahující osob žádajících o registraci informace.
* **CreateTable.php**: vytvoří tabulku MySQL aplikace. Tento soubor se používat jenom jednou.

Vytvořit a spustit aplikaci místně, postupujte následujícím způsobem. Všimněte si, že tyto kroky předpokládají, máte PHP, MySQL, a na webový server nastavení na místním počítači a ke kterým jste povolili [CHOP rozšíření MySQL][pdo-mysql].

1. Vytvoření databáze MySQL s názvem `registration`. Můžete to udělat z příkazového řádku MySQL pomocí tohoto příkazu:

        mysql> create database registration;

2. V kořenovém adresáři webový server, vytvořte složku s názvem `registration` a vytvoříte dva soubory v něm – jeden s názvem `createtable.php` a jeden s názvem `index.php`.

3. Otevřít `createtable.php` soubor v textovém editoru nebo v integrovaném vývojovém prostředí a přidat následující kód. Tento kód se použijí k vytvoření `registration_tbl` tabulku v `registration` databáze.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > Budete muset aktualizovat hodnoty pro <code>$user</code> a <code>$pwd</code> s místní MySQL uživatelské jméno a heslo.

4. Otevřete webový prohlížeč a přejděte k [http://localhost/registration/createtable.php][localhost-createtable]. Tím vytvoříte `registration_tbl` tabulky v databázi.

5. Otevřete soubor **index.php** v textovém editoru nebo v integrovaném vývojovém prostředí a přidejte základní kód HTML a CSS stránky (kód PHP se přidají na pozdější kroky).

        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php

        ?>
        </body>
        </html>

6. V rámci značky PHP přidejte kód PHP pro připojení k databázi.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Znovu byste potřebovali aktualizovat hodnoty pro <code>$user</code> a <code>$pwd</code> s místní MySQL uživatelské jméno a heslo.

7. Následující kód připojení databáze přidejte kód pro vložení registračních údajů do databáze.

        if(!empty($_POST)) {
        try {
            $name = $_POST['name'];
            $email = $_POST['email'];
            $date = date("Y-m-d");
            // Insert data
            $sql_insert = "INSERT INTO registration_tbl (name, email, date) 
                           VALUES (?,?,?)";
            $stmt = $conn->prepare($sql_insert);
            $stmt->bindValue(1, $name);
            $stmt->bindValue(2, $email);
            $stmt->bindValue(3, $date);
            $stmt->execute();
        }
        catch(Exception $e) {
            die(var_dump($e));
        }
        echo "<h3>Your're registered!</h3>";
        }

8. Nakonec za výše uvedených kód přidání kódu pro načítání dat z databáze.

        $sql_select = "SELECT * FROM registration_tbl";
        $stmt = $conn->query($sql_select);
        $registrants = $stmt->fetchAll(); 
        if(count($registrants) > 0) {
            echo "<h2>People who are registered:</h2>";
            echo "<table>";
            echo "<tr><th>Name</th>";
            echo "<th>Email</th>";
            echo "<th>Date</th></tr>";
            foreach($registrants as $registrant) {
                echo "<tr><td>".$registrant['name']."</td>";
                echo "<td>".$registrant['email']."</td>";
                echo "<td>".$registrant['date']."</td></tr>";
            }
            echo "</table>";
        } else {
            echo "<h3>No one is currently registered.</h3>";
        }

Teď můžete přecházet na [http://localhost/registration/index.php] [ localhost-index] otestovat aplikace.

##<a name="get-mysql-and-ftp-connection-information"></a>Získat informace o připojení MySQL a FTP

Připojení k databázi MySQL, na kterém běží ve webových aplikacích, vaše bude potřebovat informace o připojení. Chcete-li získat informace o připojení MySQL, postupujte takto:

1. Z aplikace služby zásuvné web appu klikněte na odkaz skupina zdroje:

    ![Vyberte pole Skupina zdroje][select-resourcegroup]

1. V rámci skupiny zdrojů klikněte na databázi:

    ![Vyberte databázi][select-database]

2. Z souhrn databáze vyberte **Nastavení** > **Vlastnosti**.

    ![Vyberte možnost Vlastnosti][select-properties]
    
2. Poznamenejte si hodnoty pro `Database`, `Host`, `User Id`, a `Password`.

    ![Poznámka: vlastnosti][note-properties]

3. Z web appu klikněte na odkaz **ke stažení publikovat profilu** v pravém dolním rohu stránky:

    ![Stažení publikovat profilu][download-publish-profile]

4. Otevřít `.publishsettings` souboru v XML editoru. 

3. Najděte `<publishProfile >` elementem `publishMethod="FTP"` , která vypadá nějak takto:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Poznamenejte si `publishUrl`, `userName`, a `userPWD` atributy.

##<a name="publish-your-app"></a>Publikování aplikace

Po testování aplikace místně, můžete je Publikujte projekt do webovou aplikaci pomocí protokolu FTP. Ale musíte nejdřív aktualizovat informace o připojení databáze v aplikaci. Podle pokynů připojení databáze jste získali dříve (v MySQL získat informace o připojení FTP části **a** ), aktualizace v **obou** tyto informace `createdatabase.php` a `index.php` soubory s příslušnými hodnotami:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Teď jste připraveni publikovat aplikace pomocí protokolu FTP.

1. Otevřete FTP klienta podle výběru.

2. Zadejte *část názvu hostitele* z `publishUrl` atribut uvedeno do FTP klienta.

3. Zadejte `userName` a `userPWD` atributy poznamenat nad beze změny do FTP klienta.

4. Připojení.

Po připojení budete moct nahrávání a stahování souborů v případě potřeby. Ujistěte se, že ukládáte soubory do kořenové složky, která je `/site/wwwroot`.

Po odeslání obě `index.php` a `createtable.php`, přejděte na **http://[site name].azurewebsites.net/createtable.php** k vytvoření tabulky MySQL aplikace a potom vyhledejte **http://[site name].azurewebsites.net/index.php** začít používat aplikaci.
 
## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Středisko pro vývojáře PHP](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
