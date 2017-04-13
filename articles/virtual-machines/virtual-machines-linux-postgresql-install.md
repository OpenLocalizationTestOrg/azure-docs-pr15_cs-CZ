<properties
    pageTitle="Nastavení PostgreSQL na Linux OM | Microsoft Azure"
    description="Zjistěte, jak nainstalovat a nakonfigurovat PostgreSQL na počítač virtuální Linux v Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


# <a name="install-and-configure-postgresql-on-azure"></a>Nainstalujte a nakonfigurujte PostgreSQL na Azure

PostgreSQL je rozšířené otevřít zdroj databáze, která se podobají Oracle a DB2. Zahrnuje enterprise použité funkce, například úplné kyseliny dodržování předpisů, spolehlivé transakční zpracování a více verzí souběžné řízení. Podporuje také standardy například ANSI SQL a SQL/MED (včetně obálky cizí dat Oracle MySQL, MongoDB a mnoha dalších). Je velmi extensible s podporou nad 12 procedurální jazyky, GIN a GiST indexy, podporu prostorových dat a více NoSQL funkce JSON nebo klíč hodnotu-aplikace založené na.

V tomto článku se dozvíte, jak můžete nainstalovat a nakonfigurovat PostgreSQL v Azure virtuální počítači spuštěná Linux.


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="install-postgresql"></a>Instalace PostgreSQL

> [AZURE.NOTE] Musíte už mít Azure virtuálního počítače, abyste mohli tento kurz spuštěný Linux. Vytvořit a nastavit Linux OM před pokračováním, najdete v tématu [kurz OM Azure Linux](virtual-machines-linux-quick-create-cli.md).

V tomto případě použití portu 1999 jako PostgreSQL port.  

Připojení k Linux OM vytvořený prostřednictvím nátěrové. Pokud používáte OM Linux Azure poprvé, přečtěte si, [jak používat SSH s Linux na Azure](virtual-machines-linux-mac-create-ssh-keys.md) se dozvíte, jak používat nátěrové pro připojení k OM Linux.

1. Spusťte tento příkaz přejděte do kořenové (Správci):

        # sudo su -

2. Některé distribuce mít závislosti, které je třeba nainstalovat před instalací PostgreSQL. Vyhledat distro v tomto seznamu a zadejte příslušný příkaz:

    - Základní Linux červené klobouk:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Debian základní Linux:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. Stáhněte si PostgreSQL do kořenového adresáře a potom rozbalte balíček:

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    Výše uvedené je znázorněn příklad. Podrobnější stažení adresu můžete najít v [Index /pub/zdroj /](https://ftp.postgresql.org/pub/source/).

4. Spusťte sestavení, spusťte tyto příkazy:

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. Pokud budete chtít vytvořit všechno, co je možné sestavit, včetně si přečtěte následující dokumentaci (HTML a člověka stránky) a další modulů (contrib), spusťte tento příkaz:

        # gmake install-world

    Dostanete potvrzení následující:

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>Konfigurace PostgreSQL

1. (Volitelné) Vytvoření symbolického odkazu chcete zkrátit odkaz PostgreSQL tak, aby neobsahoval číslo verze:

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. Vytvořte adresář pro databázi:

        # mkdir -p /opt/pgsql_data

3. Vytvoření uživatele jiné kořenové a změny profilu uživatele. Přepněte do tohoto nového uživatele (nazývané *postgres* v našem příkladu):

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] Z bezpečnostních důvodů pomocí PostgreSQL uživatele jiné kořenové inicializace, zahájení nebo ukončení databáze.


4. Upravte soubor *bash_profile* zadáním následující příkazy. Tyto řádky se přidá na konec *bash_profile* souboru:

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. Spusťte soubor *bash_profile* :

        $ source .bash_profile

6. Ověření vaší instalace pomocí tento příkaz:

        $ which psql

    Pokud je instalace úspěšné, zobrazí se následující odpověď:

        /opt/pgsql/bin/psql

7. Můžete taky zaškrtnout PostgreSQL verze:

        $ psql -V

8. Inicializace databáze:

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    Zobrazí se následující výstup:

![Obrázek](./media/virtual-machines-linux-postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>Nastavení PostgreSQL

<!--    [postgres@ test ~]$ exit -->

Spusťte následující příkazy:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Úprava dvě proměnné v souboru /etc/init.d/postgresql. Tuto předponu nastavena na Instalační cesta PostgreSQL: **/opt/pgsql**. PGDATA nastavena na cestu datový úložiště PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Obrázek](./media/virtual-machines-linux-postgresql-install/no2.png)

Změna snažíme usnadnit jeho spustitelný soubor:

    # chmod +x /etc/init.d/postgresql

Spuštění PostgreSQL:

    # /etc/init.d/postgresql start

Zkontrolujte, jestli koncového bodu PostgreSQL jde podle:

    # netstat -tunlp|grep 1999

Měli byste vidět výstupu takto:

![Obrázek](./media/virtual-machines-linux-postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Připojení k databázi Postgres

Ještě jednou přepněte na postgres uživatele:

    # su - postgres

Vytvoření databáze Postgres:

    $ createdb events

Připojení k databázi události, kterou jste právě vytvořili:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Vytváření a odstraňování Postgres tabulky

Teď, když jste připojení k databázi, můžete vytvořit tabulek v nich.

Například vytvořte novou tabulku Postgres příklad pomocí tento příkaz:

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

Teď jste nastavili čtyř sloupců tabulky pomocí následující názvy sloupců a omezení:

1. Sloupec "název" má omezenou příkazem datová být pod 20 znaků.
2. Sloupec "jídla" označuje položku jídla, přinese jednotlivým uživatelům. Datová omezuje tento text v části 30 znaků.
3. "Potvrzené" sloupec záznamů, jestli má na společnou večeři RSVP'd osoby. Povolené hodnoty se "používá písmeno A" a "N".
4. Pole "data" sloupec zobrazuje při registraci k události. Postgres vyžaduje, aby se yyyy mm dd. měly zapisovat kalendářních dat

Byste měli vidět následující, pokud úspěšné vytvoření tabulky:

![Obrázek](./media/virtual-machines-linux-postgresql-install/no4.png)

Pomocí následujícího příkazu můžete taky zaškrtnout strukturu tabulky:

![Obrázek](./media/virtual-machines-linux-postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Přidání dat do tabulky

Nejdřív vložte informace do řádku:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Měli byste vidět tento výstup:

![Obrázek](./media/virtual-machines-linux-postgresql-install/no6.png)

Pár dalších lidí můžete přidat i do tabulky. Tady jsou některé možnosti, nebo můžete vytvořit vlastní:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Zobrazení tabulky

Umožňuje znázornit tabulky tento příkaz:

    select * from potluck;

Výstup je:

![Obrázek](./media/virtual-machines-linux-postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Odstranění dat v tabulce

Pomocí následujícího příkazu odstranit data v tabulce:

    delete from potluck where name=’John’;

Tím odstraníte všechny informace na řádku "Petr". Výstup je:

![Obrázek](./media/virtual-machines-linux-postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Aktualizace dat v tabulce

Pomocí následujícího příkazu aktualizace dat v tabulce. Pro tuto položku Sandy potvrzuje, že kontakt je účastnit se jich, takže Změníme své RSVP z "N" k "Y":

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##<a name="get-more-information-about-postgresql"></a>Získat další informace o PostgreSQL
Teď, když jste dokončili instalaci PostgreSQL v OM Linux Azure, můžete využívat s ním pracovat v Azure. Další informace o PostgreSQL, navštivte [Web PostgreSQL](http://www.postgresql.org/).
