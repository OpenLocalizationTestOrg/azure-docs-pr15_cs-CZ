<properties
    pageTitle="Vytvoření a připojení k databázi MySQL v Azure"
    description="Naučte se používat portál Azure k vytvoření databáze MySQL a připojte k němu z web appu PHP v Azure."
    documentationCenter="php"
    services="app-service\web"
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm;cephalin"/>

# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Vytvoření a připojení k databázi MySQL v Azure

Tento kurz se dozvíte, jak vytvořit databázi MySQL [Azure portál](https://portal.azure.com) (poskytovatel je [ClearDB](http://www.cleardb.com/)) a jak se připojit k němu z web appu PHP spuštění v [Aplikaci služby Azure](./app-service/app-service-value-prop-what-is.md). 

> [AZURE.NOTE] Můžete taky vytvořit databázi MySQL jako součást [Šablona aplikace z Marketplace](./app-service-web/app-service-web-create-web-app-from-marketplace.md).

## <a name="create-a-mysql-database-in-azure-portal"></a>Vytvoření databáze MySQL Azure portálu

Vytvoření databáze MySQL v portálu Azure, postupujte takto:

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

2. V nabídce nalevo klikněte na **Nový** > **dat + úložiště** > **Databáze MySQL**.

    ![Vytvoření databáze MySQL v Azure - zahájení](./media/store-php-create-mysql-database/create-db-1-start.png)

2. Nové databáze MySQL [zásuvné](azure-portal-overview.md)nakonfigurovat nové databáze MySQL následujícím způsobem (*zásuvné*: stránku portálu, která se otevře ve vodorovném směru):

    - **Název databáze**: napište název jednoznačně identifikovat
    - **Předplatné**: Zvolte předplatné pro použití
    - **Typ databáze**: vyberte **sdílené** pro minimum náklady či bezplatné úrovní nebo **Dedicated** přesuňte vyhrazené zdroje. 
    - **Pole Skupina zdroje**: přidání databáze MySQL do existující [skupiny zdrojů](../azure-resource-manager/resource-group-overview.md) nebo ji umístit do nové. Zdroje ve stejné skupině je možné snadno spravovat společně.
    - **Umístění**: Vyberte umístění zavřít můžete. Při přidávání do existující skupiny zdrojů, máte uzamčené místo skupina zdroje.
    - **Ceny osy**: klikněte na **Ceny osy**a potom vyberte možnost ocenění (**rtuti** osy je zadarmo) a potom klikněte na **Výběr**. 
    - **Právní podmínky**: klikněte na **Právní podmínky**, zkontrolujte podrobnosti o nákupu a klikněte na **koupit**.
    - **Kód PIN pro řídicí panel**: vyberte, pokud chcete získat přístup přímo z řídicího panelu. Toto je užitečné, pokud nejste obeznámení s ještě portálu navigace.
    
    Následující obrázek je právě příklad konfiguraci databáze MySQL.  
    ![Vytvoření databáze MySQL v Azure – konfigurace](./media/store-php-create-mysql-database/create-db-2-configure.png)

3. Po dokončení konfigurace, klikněte na **vytvořit**.

    ![Vytvoření databáze MySQL v Azure – vytvoření](./media/store-php-create-mysql-database/create-db-3-create.png)

    Zobrazí se odesílat automaticky zobrazované upozornění, které víte, že vaše nasazení spustila.

    ![Vytvoření databáze MySQL v Azure – probíhá](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Zobrazí se vám jiné místní po úspěšném nasazení. Na portálu se taky otevře vaší zásuvné databáze MySQL automaticky.

<a name="connect"></a>
## <a name="connect-to-your-mysql-database"></a>Připojení k databázi MySQL

Pokud chcete zobrazit informace o připojení pro nové databáze MySQL, stačí klikněte **Vlastnosti** ve web appu zásuvné.
    
![Vytvoření databáze MySQL v Azure - zásuvné databáze MySQL](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Teď můžete tuto informace o připojení v libovolné aplikaci, web. Příklad, který ukazuje, jak používat informace o připojení pomocí funkčně jednoduché PHP aplikace je k dispozici [v tomto poli](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Připojení do Laravel webových aplikací (z get PHP Začínáme kurz)

Předpokládejme, že právě jste dokončili kurz [Vytvoření, konfigurace a nasadit web app PHP Azure](./app-service-web/app-service-web-php-get-started.md) a mít systém v Azure do [Laravel](https://www.laravel.com/) webových aplikací. Funkce databáze můžete snadno přidat do aplikace Laravel. Postupujte podle následujících kroků:

>[AZURE.NOTE] Následující postup předpokládá, že jste dokončili kurz [Vytvoření, konfigurace a nasadit web app PHP Azure](./app-service-web/app-service-web-php-get-started.md).

1. Nakonfigurujte aplikaci Laravel ve vaší místní vývojové prostředí: přejděte na databáze MySQL. K tomuto účelu otevřete `.env` z aplikace Laravel kořenový adresář a nakonfigurovat možnosti databáze MySQL.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

    >[AZURE.NOTE] V zásuvné **Vlastnosti** název databáze MySQL může nebo nemusí být takové, jaké vidíte v poli **Název databáze** . Je lepší kontrola parametr databáze v poli **PŘIPOJOVACÍ řetězec** . 
    >
    >![Vytvoření databáze MySQL v Azure – probíhá](./media/store-php-create-mysql-database/connect-db-1-database-name.png)

2. Nejrychlejším způsobem můžete ověřit, že máte přístup MySQL teď je použít [ověřování Laravel na výchozí](https://laravel.com/docs/5.2/authentication#authentication-quickstart). V příkazového řádku terminálu spusťte z kořenového adresáře aplikace Laravel následující příkazy:

         php artisan migrate
         php artisan make:auth

    Prvního příkazu vytvoří tabulky v Azure podle předdefinované migrací v `database/migrations` adresář a druhý příkaz scaffolds základní zobrazení a trasy registrace uživatele a ověřováním.

3. Spusťte vývojového serveru:

        php artisan serve

4. V prohlížeči přejděte na http://localhost:8000 a zaregistrovat nového uživatele, jak je znázorněno:

    ![Připojení k databázi MySQL v Azure - zaregistrovat uživatele](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Postupujte podle úplné uživatelské rozhraní výzva registraci. Po dokončení zápisu se Zaprotokolují.
    
    ![Připojení k databázi MySQL v Azure - zaregistrovat uživatele](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Vyvíjíte aplikace databázi MySQL v Azure.

5. Teď, stačí replikovat vaše `.env` nastavení do Azure webové aplikace. Spusťte následující příkazy Azure rozhraní příkazového řádku:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Zjistěte, jak to funguje v [článku Konfigurace Azure web appu](./app-service-web/app-service-web-php-get-started.md#configure).

6. Potom potvrdit a nabízená Azure místní změny dříve při spuštění `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master

7. Přejděte na Azure web appu.

        azure site browse

8. Přihlaste se pomocí přihlašovací údaje uživatele, který jste vytvořili.

    ![Připojení k databázi MySQL v Azure – procházet Azure web appu](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Při přihlášení, byste měli vidět popisný po přihlašovací obrazovka.
    
    ![Připojení k databázi MySQL v Azure - přihlášení](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Gratulujeme, webovou aplikaci PHP v Azure je teď přístup k datům z databáze MySQL. 

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Středisko pro vývojáře PHP](/develop/php/).
