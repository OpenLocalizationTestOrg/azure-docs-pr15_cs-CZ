<properties
    pageTitle="Vytvořit web appu PHP MySQL v aplikaci služby Azure a nasazení pomocí libovolná"
    description="Kurz, který ukazuje, jak vytvořit web appu PHP, který uchovává data v MySQL a použít libovolná nasazení Azure."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Vytvořit web appu PHP MySQL v aplikaci služby Azure a nasazení pomocí libovolná

Tento kurz se dozvíte, jak vytvořit PHP MySQL web appu a jak ji nasadit [Aplikaci](http://go.microsoft.com/fwlink/?LinkId=529714) služby pomocí libovolná. Použijete [PHP][install-php], nástroj MySQL příkazového řádku (v části [MySQL][install-mysql]) a [Libovolná] [ install-git] na počítači nainstalovaný. Pokyny v tomto kurzu může být zahájen na žádný operační systém, včetně systému Windows, Mac a Linux. Po dokončení tohoto průvodce, budete mít aplikaci Azure do PHP/MySQL webových aplikací.

Naučíte se:

* Jak vytvořit web app a databáze MySQL pomocí [Portálu Azure][management-portal]. Protože PHP je standardně ve [Webových aplikacích pro aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=529714) , žádné zvláštní je potřeba spustit PHP kód.
* Jak publikovat a znova publikovat aplikaci Azure pomocí libovolná.
* Jak povolit koncovku autora automatizovat autora základní úkoly v každé `git push`.

Provedením tohoto kurzu vytvoříte jednoduché registrace web app v PHP. Aplikace se být umístěny ve webových aplikacích. Snímek obrazovky s dokončeným aplikace je níže:

![Azure PHP webu][running-app]

## <a name="set-up-the-development-environment"></a>Nastavení vývojové prostředí:

Tento kurz předpokládá [PHP][install-php], nástroj MySQL příkazového řádku (v části [MySQL][install-mysql]) a [Libovolná] [ install-git] na počítači nainstalovaný.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Vytvořit web appu a nastavit libovolná publikování

Tímto postupem vytvořte web app a databáze MySQL:

1. Přihlášení k [portálu Azure][management-portal].
2. Klikněte na ikonu **Nový** .

3. Klikněte na **Zobrazit všechny** vedle **Marketplace**. 

4. Klikněte na **Web + Mobile**a pak **webové aplikace + MySQL**. Potom klikněte na **vytvořit**.

4. Zadejte platný název skupiny zdrojů.

    ![Název skupiny nastavení zdroje][resource-group]

5. Zadejte hodnoty pro nový web app.

    ![Vytvoření webové aplikace][new-web-app]

6. Zadejte hodnoty pro novou databázi, včetně odsouhlasit právní podmínkami.

    ![Vytvořit novou databázi MySQL][new-mysql-db]

7. Po vytvoření web appu zobrazí se nové zásuvné web app.

7. V **dialogovém okně Nastavení** klikněte na **Nepřetržitý nasazení**a potom klikněte na _konfigurovat požadované nastavení_.

    ![Nastavení libovolná publikování][setup-publishing]

8. Vyberte **Místní úložiště libovolná** pro tento zdroj.

    ![Nastavení libovolná úložiště][setup-repository]


9. Povolit libovolná publikování, je nutné zadat uživatelské jméno a heslo. Poznamenejte si uživatelské jméno a heslo, které vytvoříte. (Pokud jste nastavili úložišti libovolná před, budou vynechány tento krok.)

    ![Vytvoření mapování přihlašovacích publikování][credentials]


## <a name="get-remote-mysql-connection-information"></a>Získat informace o připojení vzdálené MySQL

Připojení k databázi MySQL, na kterém běží ve webových aplikacích, vaše bude potřebovat informace o připojení. Chcete-li získat informace o připojení MySQL, postupujte takto:

1. V rámci skupiny zdrojů klikněte na databázi:

    ![Vyberte databázi][select-database]

2. Z databáze **Nastavení**vyberte **Vlastnosti**.

    ![Vyberte možnost Vlastnosti][select-properties]

2. Poznamenejte si hodnoty pro `Database`, `Host`, `User Id`, a `Password`.

    ![Poznámka: vlastnosti][note-properties]

## <a name="build-and-test-your-app-locally"></a>Vytvořte a otestujte aplikace místně

Teď, když jste vytvořili do webových aplikací, můžete vyvíjet aplikace místně a potom ho nasadit po testování.

Registrace aplikace je jednoduchý PHP aplikace, která umožňuje registrovat události zadáním své jméno a e-mailovou adresu. Zobrazí se informace o předchozí zaregistrované účastníky v tabulce. Registrační informace uložené v databázi MySQL. Aplikace se skládá z jednoho souboru (zkopírujte kód k dispozici následující):

* **index.php**: slouží k zobrazení formuláře pro registraci a tabulkou obsahující osob žádajících o registraci informace.

Vytvořit a spustit aplikaci místně, postupujte následujícím způsobem. Všimněte si, že tyto kroky předpokládají, mají PHP a MySQL nástroj příkazového řádku (součást MySQL) nastavení na místním počítači a že jste povolili [CHOP rozšíření MySQL][pdo-mysql].

1. Připojení k vzdálený server MySQL pomocí hodnoty pro `Data Source`, `User Id`, `Password`, a `Database` , která dříve načítáte:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. MySQL příkazového řádku se zobrazí:

        mysql>

3. Vložit v následujících `CREATE TABLE` příkaz vytvořit `registration_tbl` tabulky v databázi:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. V kořenové složky místní aplikace vytvořte **index.php** soubor.

5. Otevřete soubor **index.php** v textovém editoru nebo v integrovaném vývojovém prostředí a přidejte následující kód a proveďte potřebné změny označené `//TODO:` komentáře.


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
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
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
            // Retrieve data
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
        ?>
        </body>
        </html>

4.  V terminálu přejděte do složky aplikace a zadejte tento příkaz:

        php -S localhost:8000

Teď můžete přecházet na **http://localhost:8000 /** testování aplikace.


## <a name="publish-your-app"></a>Publikování aplikace

Po testování aplikace místně, můžete je Publikujte projekt do webové aplikace pomocí libovolná. Bude inicializace místní úložiště libovolná a publikovat aplikace.

> [AZURE.NOTE]
> Toto jsou stejné kroky zobrazené v portálu Azure na konci vytvořit webovou aplikaci a nastavit si libovolná publikování oddílu výše.

1. (Volitelné)  Pokud jste zapomněli nebo chybně umístěné svoji adresu URL vzdálené repostitory libovolná, přejděte na web app vlastností na portálu Azure.

1. Otevřete GitBash (nebo terminál, pokud je libovolná ve vaší `PATH`), změňte adresáře do kořenového adresáře aplikace a následující příkazy:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    Zobrazí se výzva k zadání hesla, které jste dříve vytvořili.

    ![Počáteční kliknutím Azure prostřednictvím libovolná][git-initial-push]

2. Přejděte do **http://[site name].azurewebsites.net/index.php** začít používat aplikaci (tyto informace se ukládají na řídicí panel účtu):

    ![Azure PHP webu][running-app]

Po publikování aplikace, můžete začít provádění změn a použít libovolná publikovat.

## <a name="publish-changes-to-your-app"></a>Publikování změn do aplikace

Publikování změn aplikace, postupujte takto:

1. Provedení změn v aplikaci místně.
2. Otevřete GitBash (nebo terminálu it libovolná je ve vaší `PATH`), změňte adresáře kořenového adresáře aplikace a následující příkazy:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    Zobrazí se výzva k zadání hesla, které jste dříve vytvořili.

    ![Předání změn webu do Azure pomocí libovolná][git-change-push]

3. Přejděte do **http://[site name].azurewebsites.net/index.php** zobrazíte aplikace a všechny změny, které možná jste provedli:

    ![Azure PHP webu][running-app]

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>Povolení automatické autora s příponou autora

Ve výchozím nastavení procesu nasazení libovolná v aplikaci služby nic se neděje s composer.json, pokud nemáte v PHP projektu. Povolení zpracování během composer.json `git push` povolením koncovku autora.

1. Ve vaší PHP webové aplikace zásuvné [Azure portál][management-portal], klikněte na **Nástroje** > **rozšíření**.

    ![Nastavení rozšíření autora][composer-extension-settings]

2. Klikněte na tlačítko **Přidat**a potom klikněte na **autora**.

    ![Přidání rozšíření autora][composer-extension-add]
    
3. Klikněte na **OK** potvrďte právní podmínky. Klikněte na **OK** znova přidejte rozšíření.

    **Rozšíření instalované** zásuvné teď zobrazí koncovku autora.  
    ![Zobrazení rozšíření autora][composer-extension-view]
    
4. Nyní, provádět `git add`, `git commit`, a `git push` jako v předchozí části. Zobrazí se nyní, že autora instaluje závislosti podle composer.json.

    ![Rozšíření úspěch autora][composer-extension-success]

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Středisko pro vývojáře PHP](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
