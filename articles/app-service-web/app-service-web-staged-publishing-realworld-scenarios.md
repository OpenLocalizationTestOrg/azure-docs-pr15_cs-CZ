<properties
  pageTitle="Efektivní použití DevOps prostředí pro webovou aplikaci"
  description="Naučte se používat sloty nasazení se dají vytvořit a spravovat několik vývojové prostředí aplikace"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Efektivní použití DevOps prostředí pro webové aplikace

V tomto článku se dozvíte, se dají vytvořit a spravovat webové aplikace nasazení pro více verzích aplikace například vývoj, přípravu, q & a a výroby. Každý verzi aplikace mohou být považovány vývojové prostředí pro konkrétní potřeby v rámci procesu nasazení. Příklad prostředí q & a lze tak, že váš tým vývojáři otestování kvality aplikace před použít změny pro výroby.
Nastavení více vývojové prostředí může být obtížné úkol podle potřeby můžete sledovat, přidávání a používání zdrojů (výpočetním webové aplikace, databáze, mezipaměti atd.) a nasazení kódu v prostředích.

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>Nastavení testovacím prostředí (fázi, vývoj, q & a)
Až budete mít do webových aplikací výrobní nahoru a spuštěnou, dalším krokem je vytvoření testovacím prostředí. Pokud chcete použít sloty nasazení zkontrolujte, jestli že používáte v režimu plán **Standardní** nebo **Premium** aplikaci služby. Nasazení sloty jsou skutečně živou web apps s vlastní názvy hostitelů. Web app obsah a konfigurace prvky můžete vyměnit mezi dvěma sloty nasazení, včetně úsek výroby. Nasazení aplikace pro nasazení úsek má následující výhody:

1. Ověřit web app změnami pracovní pozici nasazení před výměna s úsek výroby.
2. S nasazováním ve web appu pozici první a výměna do provozu zaručuje, že všechny výskyty úsek jsou provozní teplotu před vyměnit do provozu. Díky prostoj při nasazení webovou aplikaci. Přesměrování přenosy je bezproblémové a z důvodu vyměňovat operace se nezobrazí žádné žádosti o. Celý pracovní postup můžete automatizovat pomocí konfigurace [Zaměnit automaticky](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app) při ověření před vyměňovat není potřeba.
3. Po odkládací úsek dříve fázované webovou aplikaci teď bude předchozí web appu výroby. Pokud jsou tyto změny vyměnit do úsek výrobní není podle očekávání, můžete provést stejné vyměňovat okamžitě zobrazíte "poslední známé dobré webovou aplikaci" zpět.

Pokud chcete nastavit pracovní pozici nasazení, najdete v článku [Nastavení pracovní prostředí pro web apps v aplikaci služby Azure](web-sites-staged-publishing.md). Každý prostředí by měl obsahovat vlastní sadu zdrojů, pro Pokud webové aplikace příkladu je databáze a výrobních a pracovní web app měl použít jiné databáze. Přidejte pracovní zdroje vývojové prostředí, jako jsou databáze, úložiště nebo mezipaměti pro nastavení vaší pracovní vývojové prostředí.

## <a name="examples-of-using-multiple-development-environments"></a>Příklady použití více vývojové prostředí

Jakýkoli projekt, řiďte se správa zdrojového kódu s nejméně dvě prostředí, vývoj a na provozním prostředí, ale při použití správy obsahu systems aplikace rámce atd jsme může nastat problémy kde aplikace tento scénář nepodporují mimo pole. To platí pro některé oblíbené rámců popsána níže. Velké množství otázky být paměti při práci s cm/rámce jako

1. Jak rozdělí ho zmenšit různých prostředích
2. Které soubory můžu změnit a nebude mít vliv na aktualizace verzí framework
3. Jak spravovat konfigurace podle prostředí
4. Jak spravovat moduly a moduly plug-in verze aktualizace, základní framework verze aktualizace

Nastavení prostředí projektu s více mnoha způsoby a následující příklady jsou jeden jediným takový způsob svých aplikací.

### <a name="wordpress"></a>WordPress
V této části se dozvíte, jak nastavit pracovní postup nasazení pomocí sloty pro WordPress. WordPress jako většina řešení cm nepodporuje práci s několika vývojové prostředí mimo pole. Služba aplikací Web Apps obsahuje několik funkcí, které usnadňují uložit nastavení mimo kódu.

Před vytvořením pracovní pozici, nastavte pro podporu více prostředí kód aplikace. Na podporu více prostředí ve WordPress potřebujete upravit `wp-config.php` na svoji webovou aplikaci pro místní rozvoj přidat následující kód na začátku tohoto souboru. Díky aplikaci vyberte správné konfiguraci podle vybraného prostředí.

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

Vytvoření složky v části kořenový web app s názvem `config` a přidejte soubor dvě soubory: `wp-config.azure.php` a `wp-config.local.php` představující azure a místních prostředí v tomto pořadí.

Zkopírujte následující v `wp-config.local.php` :

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

Nastavení klávesy zabezpečení můžete pomoct zabránit právě hacker webovou aplikaci, aby používat jedinečné hodnoty. Pokud potřebujete ke generování řetězce klíčů zabezpečení výše uvedené, můžete přejít na automatické generátor k vytvoření nové klíčů/hodnot pomocí [odkaz] (https://api.wordpress.org/secret-key/1.1/salt)

Zkopírujte následující kód v `wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Použití relativní cesty
Jeden co je třeba nakonfigurovat aplikaci WordPress používat relativní cesty. WordPress ukládá adresy URL informací v databázi. Díky přesunutí obsahu z jednoho prostředí do druhého obtížnější jako byste potřebovali aktualizovat databázi pokaždé, když přesunete z místního fázi nebo fázi provozním prostředí. Snížení rizika problémy způsobující s nasazením databáze pokaždé, když nasadíte z jednoho prostředí do druhého můžete [modul plug-in kořenové relativní odkazy](https://wordpress.org/plugins/root-relative-urls/) , které je možné nainstalovat řídicím panelu Správce WordPress nebo ruční stažení z [tady](https://downloads.wordpress.org/plugin/root-relative-urls.zip).


Přidejte tyto záznamy na vaše `wp-config.php` soubor před `That's all, stop editing!` poznámek:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktivace prostřednictvím modulu plug-in `Plugins` nabídky na řídicím panelu Správce WordPress. Uložení nastavení trvalý odkaz pro WordPress aplikace.

#### <a name="the-final-wp-configphp-file"></a>Poslední `wp-config.php` souboru
Všechny aktualizace WordPress Core neovlivní vaše `wp-config.php`, `wp-config.azure.php` a `wp-config.local.php` soubory. Na konci tohoto postupu `wp-config.php` soubor bude vypadat takto

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Nastavení pracovní prostředí
Za předpokladu, že jste již do WordPress webových aplikací na Web Azure, přihlaste se k [portálu Správa náhledu Azure](http://portal.azure.com) a přejděte na webovou aplikaci WordPress. Pokud aplikace není můžete vytvořit jednu z marketplace. Další informace, [klikněte sem](web-sites-php-web-site-gallery.md).
Klepněte na Nastavení -> nasazení sloty -> Přidat a vytvořte úsek nasazení se název dílčí fáze. Nasazení úsek je sdílení stejné zdroje jako primární web app vytvořili nad jiné webové aplikace.

![Vytvoření úsek nasazení fáze](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

Přidání jiné databáze MySQL přivítejte `wordpress-stage-db` do skupiny zdrojů `wordpressapp-group`.

 ![Přidání databáze MySQL do pole Skupina zdroje](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

Aktualizace jsou řetězce připojení pro vaše fázi nasazení úsek tak, aby ukazovaly na nově vytvořený databáze `wordpress-stage-db`. Poznámka: výrobní webové aplikace, `wordpressprodapp` a pracovní web app `wordpressprodapp-stage` musí odkazovat na jiné databáze.

#### <a name="configure-environment-specific-app-settings"></a>Konfigurace nastavení specifické prostředí aplikace
Vývojáři můžou být uloženy dvojice řetězců klíč hodnota v Azure jako součást informací o konfiguraci přidružený k web appu s názvem nastavení aplikace. Za běhu aplikací služby Web Apps automaticky načte tyto hodnoty pro vás a díky kterému je dostupný pro kód spuštěný ve web appu. Cenného perspektivy, která je hodní straně využívat od citlivé informace jako řetězce připojení databáze pomocí hesla nikdy zobrazovaly jako prostý text v souboru jako `wp-config.php`.

Tento proces definované pod je užitečná, když provedete zahrnuje změny souboru a změny v databázi aplikace WordPress:
- Upgrade WordPress verze
- Přidat novou nebo upravit nebo upgrade moduly plug-in
- Přidat novou nebo upravit nebo upgrade motivy

Nastavení aplikace:

- informace o databázi
- zapnutí/vypnutí protokolování WordPress
- Nastavení zabezpečení WordPress

![Nastavení aplikace pro Wordpress web app](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Zkontrolujte, že jste přidali následující nastavení aplikace pro úsek vaše výrobní web app a dílčí fáze. Poznámka: výrobní web app a pracovní web appu používat jiné databáze.
Zrušte zaškrtnutí políčka **Úsek nastavení** pro všechny parametry nastavení kromě WP_ENV. To bude zaměnit konfigurace pro webovou aplikaci, spolu s obsah a souboru databáze. **Checked**při **Nastavení úsek** nastavení aplikace web appu a konfigurace řetězec připojení se nebude přesouvat v prostředích při provádění operace VYMĚŇOVAT a tedy pokud existují změny databáze to neporušíte webovou aplikaci výroby.

Nasazení místní vývojové prostředí web appu do oblasti v prohlížeči a databáze pomocí WebMatrix nebo nástroje podle svého výběru ATP FTP, libovolná PhpMyAdmin.

![Web publikování matice dialogu pro WordPress web app](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

Vyhledejte a otestujte vaše pracovní web appu. Vzhledem k tomu situace místo, kam má aktualizovat motiv web appu tady je pracovní web app.

![Vyhledejte pracovní web appu před záměna sloty](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 Pokud všechny vypadá všechno dobře, klikněte na tlačítko **Zaměnit** na pracovní webovou aplikaci chcete přesunout obsah provozním prostředí. V tomto případě střídáte web appu a databází v prostředích během operace každé **vyměňovat** .

![Záměna náhled změn pro WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >Pokud máte situace, kdy je potřeba pouze nabízených soubory (bez aktualizace databáze), potom **Zkontrolujte** **Úsek nastavení** všechny databáze týkající se *Nastavení připojení řetězce* ve webové aplikaci nastavení zásuvné v portálu Azure náhled a *Nastavení aplikace* předtím ODKLÁDACÍ. V tomto případu DB_NAME, DB_HOST, DB_PASSWORD, DB_USER řetězec nastavení připojení výchozí neměli objeví v náhled změn při provádění **Zaměnit**. AT tentokrát se po dokončení operace **Zaměnit** web appu WordPress bude mít aktualizací **jenom**soubory.

Před prováděním ODKLÁDACÍ, tady je web app WordPress výrobní ![výrobní v prohlížeči před výměna sloty](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

Po provedení operace VYMĚŇOVAT byl aktualizován motiv na webovou aplikaci výroby.

![V prohlížeči výrobní po přepnutí na sloty](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

V případě, že když potřebujete **vrátit zpět**, můžete přejít na nastavení výrobní web app a klikněte na tlačítko **vyměňovat** zaměnit web app a databáze z výroby na pracovní pozici. Důležité si zapamatovat je, že pokud změny v databázi jsou součástí operace **Zaměnit** v daném okamžiku, pak při příštím znovu nasadíte pracovní webovou aplikaci potřebujete nasazení databáze se změní na aktuální databáze svojí pracovní webové aplikace, které by mohly být předchozí výrobní nebo fáze databázi.

#### <a name="summary"></a>Souhrn
Chcete-li generalize obrázku pro libovolnou aplikaci s databází

1. Instalace aplikací na místního prostředí
2. Zahrnout prostředí konkrétní konfigurace (místního a Azure Web App)
3. Nastavení prostředí v aplikaci služby Web Apps – pracovní, výrobní
4. Pokud máte praxi spuštěná na Azure, synchronizujte výrobní obsahu (soubory nebo kód + databáze) místní a pracovní prostředí.
5. Můžete vyvíjet aplikace na místního prostředí
6. Umístěte webovou aplikaci výrobní v části Údržba nebo uzamčené režimu a synchronizace databáze obsah z výroby pracovní a vývoj prostředí
7. Pracovní prostředí a zkušební nasazení
8. Nasazení provozním prostředí
9. Opakujte kroky 4 až 6

### <a name="umbraco"></a>Umbraco
V této části se dozvíte, jak cm Umbraco používá vlastní modul nasazení z napříč několika DevOps prostředí. Tento příklad uvádí různé přístupu ke správě více vývojové prostředí.

[Umbraco cm](http://umbraco.com/) je taková popular.NET cm řešení používají mnoho vývojáři, které poskytuje [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul nasadit od vývoje pracovní provozním prostředí. Můžete snadno vytvořit místní vývojové prostředí pro webovou aplikaci Umbraco cm pomocí aplikace Visual Studio nebo WebMatrix.

1. Vytvořte webovou aplikaci Umbraco s Visual Studiu, [klikněte sem](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget).
2. Pokud chcete vytvořit webovou aplikaci Umbraco s WebMatrix, [klikněte sem](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix).

Nezapomeňte vždy odebrat `install` složek v části aplikace a nikdy ho nahrajte do oblasti nebo výrobní webové aplikace. Pro účely tohoto návodu můžu pomocí WebMatrix

#### <a name="set-up-a-staging-environment"></a>Nastavení pracovní prostředí
- Vytvoření nasazení úsek výše uvedené pro Umbraco cm v prohlížeči, za předpokladu, že už máte web appu Umbraco cm a systémem. V opačném případě můžete vytvořit jednu z marketplace.

- Aktualizujte připojovací řetězec pro vaše fázi nasazení úsek tak, aby ukazovaly nově vytvořený databáze, **umbraco fázi db**. Výrobní web appu (umbraositecms-1) a pracovní web appu (umbracositecms 1fáze) **musí** odkazovat na jiné databáze.

![Aktualizace připojovací řetězec pro přípravu web app s novou pracovní databázi](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- Klikněte na **nastavení získat publikování** úsek nasazení **fáze**. To bude stažení souboru nastavení publikování, do které uloží všechny požadované informace Visual Studio nebo matici Web publikování aplikace z místního rozvoje web appu do Azure web appu.

 ![Získání publikovat nastavení pracovní web appu](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- Otevřete webovou aplikaci místní rozvoj v **WebMatrix** nebo **Visual Studio**. V tomto kurzu používám Web matice a je třeba nejprve importovat nastavení publikování pro pracovní web app

![Import nastavení publikování pro Umbraco pomocí Web matice](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- Revize změn v dialogovém okně a nasazení místní webovou aplikaci Azure webovou aplikaci, *umbracositecms, 1fáze*. Při nasazení soubory přímo webovou aplikaci pracovní vynecháte všech souborů v `~/app_data/TEMP/` složky jako to bude obnoven při prvním fázi web appu začít. By měl taky vynecháte `~/app_data/umbraco.config` soubor jako toho příliš, bude obnoven.

![Revize změn publikovat v matici web](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Po úspěšném publikování Umbraco místní web appu pro přípravu v prohlížeči, přejděte do svojí pracovní webové aplikace a spuštění několik testů vylučte jakékoli problémy.

#### <a name="set-up-courier2-deployment-module"></a>Nastavení modulu Courier2 nasazení
Pomocí modulu [Courier2](http://umbraco.com/products/more-add-ons/courier-2) můžete nabízená obsah, šablony stylů, vývoj moduly a informace s jednoduchou pravým tlačítkem z pracovní web appu výrobní web app pro další bezplatné nasazení problém a snížení rizika meze webovou aplikaci výrobní při nasazení aktualizace.
Koupit licence pro Courier2 pro doménu `*.azurewebsites.net` a vaši vlastní doménu (například http://abc.com) po zakoupení licencí umístit stažené licence (. Soubor LIC) v `bin` složky.

![Uvolnění licence soubor ve složce Koš](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

Stáhněte balíček Courier2 z [tady](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Přihlaste se do webové aplikace fázi, vyslovte http://umbracocms-site-stage.azurewebsites.net/umbraco a klikněte na **Vývojář** nabídky a vyberte **balíčků**. Klikněte na **nainstalovat** místní balíčku

![Instalační program Umbraco balíčku](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

Nahrání balíčku courier2 pomocí instalačního programu.

![Nahrání balíčku pro courier modul](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

Konfigurace je potřeba aktualizovat soubor courier.config složce **Konfigurace** svojí webové aplikace.

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

V části `<repositories>`, zadejte informace o výrobní webu adresy URL a uživatel. Pokud používáte výchozího poskytovatele Umbraco členství, zadejte Identifikátor pro správu uživatelů ve <user> oddíl. Pokud používáte vlastní poskytovatele členství Umbraco, použijte `<login>`,`<password>` Courier2 modulu vědět, jak se připojit k webu výroby. Další informace prohlédněte [si přečtěte následující dokumentaci](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) pro Courier modul.

Podobně nainstalujte modul Courier na webu výrobní a nakonfigurujte ji, přejděte na web appu fázi v souboru odpovídajících courier.config znázorněná na obrázku

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

Klikněte na kartu Courier2 na řídicím panelu Umbraco cm web app a vyberte umístění. Název úložiště měli vidět, jak je uvedeno v `courier.config`. Použijte svůj výrobní a přípravu webových aplikací web apps.

![Zobrazení cílový web app úložiště](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

Nyní umožňuje nasazení určitý obsah z webu pracovní web výroby. Přejděte na obsah a vyberte existující stránku nebo vytvořte novou stránku. Můžu se vyberte stávající stránky Můj web appu, kde se nadpis stránky změní na **Začínáme – nové** a teď klikněte na **Uložit a publikovat**.

![Změna názvu stránky a publikování](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

Když vyberete možnost upravené stránky a *klikněte pravým tlačítkem myši* a zobrazit všechny možnosti. Klikněte na **Courier** zobrazíte dialogové okno nasazení. Klikněte na **Deploy** zahajte nasazení

![Dialogové okno Courier modul nasazení](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

Zkontrolujte změny a klikněte na pokračovat.

![Courier modul nasazení dialogové okno revidovat změny](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

Nasazení protokolu zobrazí, pokud nasazení byl úspěšný.

 ![Zobrazení protokolů nasazení z modulu Courier](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

Vyhledejte webovou aplikaci výrobní zobrazíte-li změny se projeví.

 ![Procházet výrobní web appu](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Další informace o používání Courier najdete v dokumentaci.

#### <a name="how-to-upgrade-umbraco-cms-version"></a>Jak upgradovat Umbraco cm verze

Courier nepomůže, nasazení s upgradu z jedné verze Umbraco cm. Při upgradu Umbraco cm verze, je třeba zkontrolovat pro kompatibilitou s vlastní moduly nebo moduly třetích stran a knihovnami Umbraco Core. Osvědčený

1. Před zahájením upgrade vždy zálohujte web app a databáze. Na Azure v prohlížeči, můžete nastavit automatické zálohování funkce vaše weby pomocí zálohování a obnovení webu v případě potřeby pomocí obnovit funkci. Další informace najdete v tématu [jak obecnějším údajům web appu](web-sites-backup.md) a [Jak obnovit webovou aplikaci](web-sites-restore.md).

2. Zkontrolujte, jestli jsou kompatibilní při upgradu na verzi softwaru jiných výrobců, které používáte. Na funkci balíček stáhněte stránky, podívejte se projektu kompatibilní s Umbraco cm verzi.

Podrobné informace o tom, jak aktualizovat webovou aplikaci místně postupujte podle pokynů jako uvedené [v tomto poli](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general).

Po upgradu webu místní rozvoj publikujte změny pracovní web appu. Testovat aplikace a pokud všechny vypadá všechno dobře, použijte **Zaměnit** s cílem **Zaměnit** svůj pracovní web a výrobní v prohlížeči. Při provádění operací **Zaměnit** , můžete zobrazit změny, které budou mít vliv na v konfiguraci web appu. Pomocí této operace **Zaměnit** jsme jsou výměna webových aplikací web apps a databází. To znamená, když VYMĚŇOVAT výrobní web appu se teď přejděte umbraco fázi db databáze a pracovní web appu se umbraco výrobní db databáze.

![Záměna náhledu pro nasazení Umbraco cm](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Výhodou výměna web app a databáze:
1. Umožňuje vrátit k předchozí verzi webové aplikace pomocí jiného **Zaměnit** , pokud jsou všechny problémy s aplikací.
2. Upgrade potřebujete nasazení soubory a databáze z pracovní web app výrobní web app a databáze. Máte řadu možností, které můžete přejít nepovedlo při nasazení soubory a databáze. Pomocí funkce **vyměňovat** sloty jsme omezit prostoje během upgradu a snížit riziko chyby, ke kterým může dojít při nasazení změn.
3. Umožňuje provádět **A a B testování** pomocí funkce [testování ve výrobním](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)

Tento příklad ukazuje, flexibilitu platformu kde můžete vytvořit vlastní moduly podobný Umbraco Courier modul ke správě nasazení prostředí.

## <a name="references"></a>Odkazy
[Aktivní software development aplikace službou Azure](app-service-agile-software-development.md)

[Nastavení pracovní prostředí pro web apps v aplikaci služby Azure](web-sites-staged-publishing.md)

[Jak zablokovat k nasazení testovacím sloty web access](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
