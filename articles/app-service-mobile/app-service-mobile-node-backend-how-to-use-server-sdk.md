<properties
    pageTitle="Jak pracovat se serverem back-end Node.js SDK k aplikacím Mobile | Azure aplikace služby"
    description="Informace o práci se serverem back-end Node.js SDK Azure aplikace služby mobilních aplikací."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Jak používat v Azure mobilní aplikace Node.js SDK

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Tento článek obsahuje podrobné informace a příklady znázorňující, jak pracovat s back-end Node.js v Azure aplikace služby mobilní aplikace.

## <a name="Introduction"></a>Úvod

Azure aplikace služby mobilní aplikace poskytuje možnost přidání optimalizované mobilní datové rozhraní API webových webové aplikace.  V Azure aplikace služby mobilní aplikace SDK je vynechaný ASP.NET a Node.js webových aplikací.  V SDK poskytuje tyto operace:

- Operace s tabulkou (pro čtení, vložení, aktualizace, odstranění) pro přístup k datům
- Vlastní rozhraní API operace

Obě operace poskytovat ověřování přes všechny Zprostředkovatelé identit jiní povolený aplikace službou Azure včetně poskytovatelé sociální identity jako je Facebook, Twitter, Google a Microsoft i Azure Active Directory pro identitu organizace.

O ke každému případu použití v [adresáři vzorky na GitHub]najdete příklady.

## <a name="supported-platforms"></a>Podporované platformy

Uzel SDK Azure mobilní aplikace podporuje ální l uzlu a v novějších verzích.  Psaní, je nejnovější verze l uzel v4.5.0.  Další verze uzel může to fungovat, ale nejsou podporované.

Uzel SDK Azure mobilní aplikace podporuje dva ovladače databáze: ovladač uzel mssql podporuje SQL Azure a místních instance serveru SQL Server.  Ovladač sqlite3 podporuje SQLite databáze na jenom jedna instance.

### <a name="howto-cmdline-basicapp"></a>Postup: vytvoření základní Node.js back-end pomocí příkazového řádku

Každý back-end Azure aplikace služby mobilní aplikace Node.js spustí jako ExpressJS aplikace.  ExpressJS je nejoblíbenější webové služby rámec umožňující Node.js.  Vytvoříte základní [Express] aplikace takto:

1. V příkazu nebo okna prostředí PowerShell vytvořte adresář plánu projektu.

        mkdir basicapp

2. Spusťte npm inicializace inicializace struktury balíčku.

        cd basicapp
        npm init

    Spuštění příkazu npm zeptá sadu otázek, na které inicializace projektu.  Zobrazit výstup příkladu:

    ![Výstup inicializace npm][0]

3. Nainstalujte express a aplikací azure mobile knihoven v úložišti npm.

        npm install --save express azure-mobile-apps

4. Vytvoření souboru app.js provádět základní mobilní serveru.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Tato aplikace vytvoří WebAPI optimalizované mobilní s jeden koncový bod (`/tables/TodoItem`), která jsou k dispozici neověřených základní úložiště data SQL pomocí dynamického schéma.  Je vhodné pro sledování Snadné spuštění knihovny klienta:

- [Rychlý úvod Android klienta]
- [Rychlý úvod Apache Cordova klienta]
- [iOS rychlý úvod klienta]
- [Rychlý úvod klienta pro Windows Store]
- [Rychlý úvod Xamarin.iOS klienta]
- [Rychlý úvod Xamarin.Android klienta]
- [Rychlý úvod Xamarin.Forms klienta]

Vyhledání kódu pro tuto aplikaci základní v [ukázkové basicapp na GitHub].

### <a name="howto-vs2015-basicapp"></a>Postup: vytvoření back-end uzel s Visual Studio 2015

Visual Studio 2015 vyžaduje rozšíření k vývoji aplikací Node.js v integrovaném vývojovém prostředí.  Zahájíte instalaci [Node.js nástroje 1.1 for Visual Studio].  Po instalaci nástroje Node.js for Visual Studio vytvořte aplikaci 4.x Express:

1. Otevření dialogového okna **Nový projekt** (ze **souboru** > **Nový** > **projekt...**).

2. Rozbalení **šablony** > **JavaScript** > **Node.js**.

3. Vyberte **aplikaci základní Azure Express Node.js 4**.

4. Zadejte název projektu.  Klikněte na *OK*.

    ![Nový projekt Visual Studio 2015][1]

5. Klikněte pravým tlačítkem myši na uzel **npm** a vyberte **nainstalovat npm balíčky s novým …**.

6. Budete muset aktualizovat katalogu npm týkající se vytváření první Node.js aplikace.  Pokud je to potřeba, klikněte na **Aktualizovat** .

7. Do vyhledávacího pole zadejte _aplikací azure mobile_ .  Klikněte na balíček **azure aplikací mobile 2.0.0** a potom klikněte na **Nainstalovat balíček**.

    ![Instalace nové balíčky npm][2]

8. Klikněte na tlačítko **Zavřít**.

9. Otevřete soubor _app.js_ přidání podpory SDK Azure mobilní aplikace.  Na řádku 6 at dole podokně úloh knihovna vyžaduje příkazy, přidejte následující kód:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Přibližně 27 za řádek jiné app.use příkazy přidejte tento kód:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Uložte soubor.

10. Spusťte aplikaci místně (rozhraní API je doručen http://localhost:3000) nebo publikovat Azure.

### <a name="create-node-backend-portal"></a>Postup: vytvoření Node.js back-end pomocí portálu Azure

Vpravo back-end mobilní aplikaci můžete vytvořit [Azure portál]. Můžete postupujte podle následujících kroků nebo vytvoření klienta a serveru společně pomocí následujících kurz [Vytvoření mobilní aplikaci](app-service-mobile-ios-get-started.md) . Kurz obsahuje zjednodušenou verzí tyto pokyny a je nejvhodnější pro doklad koncept projektů.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Po návratu do zásuvné _Začínáme_ v části **vytvořit tabulku rozhraní API**zvolte **Node.js** jako **jazyk back-end**. Zaškrtněte políčko u "**můžu potvrdit, že tímto přepíšete všechny webu obsah.**" klikněte na **vytvořit TodoItem tabulky**.

### <a name="download-quickstart"></a>Postup: Stáhněte si pomocí libovolná projekt kódu rychlý úvod Node.js back-end

Při vytváření back-end Node.js mobilní aplikaci pomocí portálu **úvodní** zásuvné projektu Node.js vytvoří a nasadit na web. Můžete přidat tabulky a rozhraní API a upravovat soubory kódu pro back-end Node.js na portálu. Můžete taky různých nástroje pro nasazení stáhnout projekt back-end tak, že můžete přidat nebo změnit tabulek a rozhraní API a pak znovu publikovat projekt. Další informace najdete v tématu [Průvodce nasazením aplikace služby Azure]. Následující postup používá úložišti libovolná přehrajete rychlý úvod projekt kódu.

1. Nainstalujte libovolná, pokud jste tak již neučinili. Kroky potřebné k instalaci libovolná lišit operační systémy. Naleznete v tématu [Instalace libovolná](http://git-scm.com/book/en/Getting-Started-Installing-Git) specifické pro operační systém rozdělení a pokyny pro instalaci.

2. Postupujte podle pokynů v tématu [Povolení aplikace úložiště služby aplikace](../app-service-web/app-service-deploy-local-git.md#Step3) povolit úložiště libovolná pro web back-end vytvoření poznámky o nasazení uživatelské jméno a heslo.

3. Poznamenejte si nastavení **Libovolná klonovat URL** v zásuvné pro mobilní aplikaci backendovou.

4. Spustit `git clone` příkaz pomocí libovolná klonovat adresy URL, zadávání svoje heslo v případě potřeby jako v následujícím příkladu:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Přejděte do místního adresáře, tedy v předchozím příkladu /todolist a Všimněte si stáhli soubory projektu. Vyhledejte `todoitem.json` souborů v `/tables` adresář.  Tento soubor definuje oprávnění v tabulce.  Taky `todoitem.js` soubor ve stejném adresář, který definuje operace CRUD skripty pro tabulku.

6. Po provedení změn souborů projektů, spusťte následující příkazy chcete přidat, potvrdit a potom uložit změny na web:

        $ git commit -m "updated the table script"
        $ git push origin master

    Když přidáte nové soubory do projektu, musíte nejdřív spustit `git add .` příkaz.

Na web je znovu publikovat pokaždé, když s novou sadou potvrzení se posune na web.

### <a name="howto-publish-to-azure"></a>Postup: publikovat svůj back-end Node.js Azure

Microsoft Azure poskytuje mnoho mechanismy pro publikování Azure aplikace služby mobilní aplikace Node.js backendovou pro službu Azure.  Jedná se o použití nástroje pro nasazení integrována do aplikace Visual Studio, nástroje příkazového řádku a možnosti nepřetržitý nasazení podle ovládacího prvku zdroje.  Další informace o tomto tématu najdete v tématu [Průvodce nasazením aplikace služby Azure].

Azure aplikaci služby obsahuje konkrétní doporučení pro Node.js aplikace, které byste měli zkontrolovat před nasazením:

- Určení [Verze uzel]
- Jak [používat moduly uzel]

### <a name="howto-enable-homepage"></a>Postup: povolení domovskou stránku aplikace

Řada aplikací jsou kombinace web a mobilní aplikace a ExpressJS framework umožňuje sloučení dvou fasetami.  Někdy ale můžete chtít pouze implementace mobilní rozhraní.  Je vhodné zajistit, že je cílová stránka zajistit službu aplikace do začátků.  Můžete zadat vlastní domovské stránce nebo povolení dočasné domovskou stránku.  Povolit dočasné domovské stránky, použijte následující pro vytvoření instance Azure mobilní aplikace:

    var mobile = azureMobileApps({ homePage: true });

Pokud chcete jenom tuto možnost k dispozici při vytváření místně, můžete přidat toto nastavení bude vaše `azureMobile.js` soubor.

## <a name="TableOperations"></a>Operace s tabulkou 

SDK Server azure aplikací mobile Node.js poskytuje mechanismy vystavit tabulek dat uložených v databázi SQL Azure jako WebAPI.  Pět operace jsou k dispozici.

| Operace | Popis |
| --------- | ----------- |
| ZÍSKÁNÍ /tables/_tabulky_ | Zobrazí všechny záznamy v tabulce |
| ZÍSKÁNÍ /:id /tables/_tabulky_ | Získání na určitý záznam v tabulce |
| PŘÍSPĚVEK /tables/_tabulky_ | Vytvořte záznam v tabulce |
| Oprava /:id /tables/_tabulky_ | Aktualizujte záznam v tabulce |
| ODSTRANĚNÍ_tabulky_/:id /tables/ | Odstranění záznamu v tabulce |

Tento WebAPI podporuje [OData] a slouží k rozšíření schématu tabulky pro podporu [dat v režimu offline synchronizace].

### <a name="howto-dynamicschema"></a>Postup: definování tabulek pomocí dynamického schématu

Před použitím tabulky, je třeba definovat.  Pomocí statické schéma (pokud vývojář definuje sloupce v rámci schématu) nebo dynamicky je to možné definovat tabulky (kde SDK ovládacích prvků schématu podle příchozí žádosti). Vývojář kromě toho můžete určit konkrétní aspekty WebAPI přidáním kód v JavaScriptu na definici.

Osvědčený by měl definovat každou tabulku v souboru Javascript v adresáři tabulky a potom použijte metodu tables.import() k importu tabulky.  Rozšíření aplikaci basic, soubor app.js se upraví:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definice tabulky,. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tabulky pomocí dynamického schéma ve výchozím nastavení.  Vypnutí dynamické schématu globálně, nastavte nastavení aplikace **MS_DynamicSchema** NEPRAVDA v Azure portálu.

Dokončení příkladu můžete najít v [ukázkové úkol na GitHub].

### <a name="howto-staticschema"></a>Postup: definování tabulek pomocí statický schématu

Můžete definovat explicitně sloupce, které chcete vystavit prostřednictvím WebAPI.  SDK Node.js aplikací azure mobile automaticky přidá všechny další sloupce potřebných pro data v režimu offline synchronizace do seznamu, který zadáte.  Například klientské aplikace rychlý úvod vyžadovat tabulce se dvěma sloupci: text (řetězec) a dokončení (logická).  
V tabulce lze definovat v tabulce JavaScript souboru definice (umístěné v adresáři tabulek) takto:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Když definujete staticky tabulkami, musíte taky volat metodu tables.initialize() na spuštění vytvořit schématu databáze.  Metoda tables.initialize() vrátí [Promise] tak, aby webová služba neslouží žádosti o před databázi inicializovaný.

### <a name="howto-sqlexpress-setup"></a>Postup: použití SQL Express jako datový úložiště vývoj na místním počítači

V Azure mobilní aplikace AzureMobile aplikace uzel SDK nabízí tři možnosti pro podávání data mimo pole: SDK máte tři možnosti pro podávání data mimo pole:

- K zajištění úložištěm dočasnou příkladu používají ovladači **paměti**
- Použít **mssql** ovladače SQL Express úložiště dat pro vývoj
- Použití ovladače **mssql** stanovit výrobní úložiště dat databáze SQL Azure

V Azure mobilní aplikace Node.js SDK používá [mssql Node.js balíčku] a vytvoření připojení k SQL Express a databáze SQL.  Tento balíček vyžaduje povolit připojení TCP na instanci SQL Express.

> [AZURE.TIP]Ovladač paměti neposkytuje úplná sada střediska pro účely testování.  Pokud chcete otestovat vaše back-end místně, doporučujeme použití úložiště data expresní SQL a ovladač mssql.

1. Stáhněte a nainstalujte [Microsoft SQL Server 2014 Express].  Ujistěte se, že musíte nainstalovat SQL Server 2014 Express edici nástroje.  Pokud požadujete explicitně 64-bit podporu, 32bitová verze spotřebovává méně paměti při spuštění.

2. Spusťte Správce konfigurace systému SQL Server 2014.

  1. Rozbalte uzel **SQL Server – konfigurace sítě** v levém stromové nabídce.
  2. Klikněte na **protokoly pro SQLEXPRESS**.
  3. Klikněte pravým tlačítkem na **TCP/IP** a zaškrtněte políčko **Povolit**.  V dialogovém okně místní klikněte na tlačítko **OK** .
  4. Klikněte pravým tlačítkem na **TCP/IP** a vyberte **Vlastnosti**.
  5. Klikněte na kartu **IP adres** .
  6. Najděte uzel **IPAll** .  V poli **TCP Port** zadejte **1433**.

         ![Configure SQL Express for TCP/IP][3]

  7. Klikněte na **OK**.  V dialogovém okně místní klikněte na tlačítko **OK** .
  8. V levém stromové nabídce na příkaz **Služby SQL Server** .
  9. Klikněte pravým tlačítkem na **SQL Server (SQLEXPRESS)** a vyberte položku **Restartovat**
  10. Ukončete Správce konfigurace systému SQL Server 2014.

3. Spusťte SQL Server 2014 Management Studio a připojit k místní instanci SQL Express

  1. Klikněte pravým tlačítkem myši instanci aplikace Explorer objektu a vyberte **Vlastnosti**
  2. Vyberte stránku **zabezpečení** .
  3. Zajistit, že je vybraná možnost **serveru SQL Server a v režimu ověřování systému Windows**
  4. Klikněte na tlačítko **OK**

        ![Konfigurace ověřování SQL Server Express][4]

  5. Rozbalte položku **zabezpečení** > **přihlášení** v prohlížeči objektů
  6. Klikněte pravým tlačítkem myši **přihlášení** a vyberte **Nové přihlášení...**
  7. Zadejte přihlašovací jméno.  Vyberte **ověřování serveru SQL Server**.  Zadejte heslo a pak zadejte stejné heslo do pole **Potvrzení hesla**.  Heslo musí vyhovovat požadavkům složitost Windows.
  8. Klikněte na tlačítko **OK**

        ![Přidání nového uživatele vyjádřit SQL][5]

  9. Klikněte pravým tlačítkem na nové přihlašovací jméno a vyberte **Vlastnosti**
  10. Vyberte stránku **Role serveru**
  11. Zaškrtněte políčko vedle **budou** role serveru
  12. Klikněte na tlačítko **OK**
  13. Zavřete SQL Server 2015 Management Studio

Zajistěte, aby že záznam uživatelské jméno a heslo, které jste vybrali.  Potřebujete přiřadit další server role nebo oprávnění v závislosti na vašim požadavkům pro konkrétní databáze.

Aplikace Node.js čte proměnnou **SQLCONNSTR_MS_TableConnectionString** prostředí pro připojovací řetězec pro tuto databázi.  Nastavte tuto proměnnou prostředí.  Pomocí prostředí PowerShell můžete například nastavit tuto proměnnou prostředí:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Přístup k databázi přes připojení TCP/IP a zadejte uživatelské jméno a heslo pro připojení.

### <a name="howto-config-localdev"></a>Jak: konfigurace projektu pro místní vývoj

Azure mobilní aplikace přečte soubor JavaScript s názvem _azureMobile.js_ z místního systému souborů.  Nepoužívejte tento soubor konfigurace SDK Azure mobilní aplikace na výrobní-místo toho použít nastavení aplikace v [Azure portálu] .  Soubor _azureMobile.js_ by měl export konfigurace objektu.  Nejběžnější nastavení jsou:

- Nastavení databáze
- Nastavení protokolování diagnostiky
- Alternativní CORS nastavení

Příklad _azureMobile.js_ souboru provádění výše uvedená nastavení databáze takto:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Doporučujeme přidat _azureMobile.js_ _.gitignore_ soubor (nebo jiné zdrojového kódu ignorovat soubor) zabráníte hesla uložený v cloudu.  Vždy nastavení výrobní v nastavení aplikace v [Azure portálu].

### <a name="howto-appsettings"></a>Jak: Konfigurace aplikace pro mobilní aplikaci

Většina nastavení v souboru _azureMobile.js_ mít odpovídající nastavení aplikace v [Azure portálu].  Následující seznam používá ke konfiguraci aplikace v nastavení aplikace:

| Nastavení aplikace                 | Nastavení _azureMobile.js_  | Popis                               | Platné hodnoty                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | Jméno                      | Název aplikace                       | řetězec                                      |
| **MS_MobileLoggingLevel**   | Logging.level             | Úroveň minimální protokolu zpráv k přihlášení      | Chyba, upozornění, informace najdete podrobné ladění, i |
| **MS_DebugMode**            | ladění                     | Povolení nebo zakázání ladění režimu              | PRAVDA, NEPRAVDA                                 |
| **MS_TableSchema**          | data.Schema               | Výchozí schématu název tabulky SQL        | řetězec (výchozí: dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | Povolení nebo zakázání ladění režimu              | PRAVDA, NEPRAVDA                                 |
| **MS_DisableVersionHeader** | verze (nastavena na Nedefinováno)| Zakáže záhlaví X ZUMO serveru verze | PRAVDA, NEPRAVDA                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | Zakáže Kontrola verze klientského rozhraní API     | PRAVDA, NEPRAVDA                                 |

Nastavení prostředí aplikace:

1. Přihlaste se k [portálu Azure].
2. Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název aplikace Mobile.
3. Nastavení zásuvné otevře ve výchozím nastavení. Pokud ne, klikněte na **Nastavení**.
4. V hlavní nabídce na příkaz **Nastavení aplikace** .
5. Přejděte do oddílu nastavení aplikace.
6. Pokud aplikace nastavení už existuje, klikněte na požadovanou hodnotu nastavení aplikace pro úpravu hodnoty.
7. Pokud vaše nastavení aplikací neexistuje, zadejte do pole klíč a hodnotu v poli hodnota nastavení aplikace.
8. Po dokončení klikněte na **Uložit**.

Změna většina nastavení aplikace vyžaduje restartování služby.

### <a name="howto-use-sqlazure"></a>Postup: použití SQL databázi jako úložiště výrobní dat.

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Použití databáze SQL Azure jako datový úložiště je stejné všech typů aplikace aplikaci služby Azure. Pokud jste to ještě neudělali, vytvořte back-end mobilní aplikaci takto:

1. Přihlaste se k [portálu Azure].

2. V horní levé části okna, klikněte na tlačítko **+ Nový** > **Web + Mobile** > **Mobilní aplikaci**a potom zadejte název vašeho back-end mobilní aplikaci.

3. V dialogovém okně **Pole Skupina zdroje** zadejte stejný název jako aplikace.

4. Plán služeb výchozí aplikace zaškrtnuté.  Pokud chcete změnit svůj plán služeb aplikací, lze provést po kliknutí na plán služeb aplikace > **+ vytvořit nový**.  Zadejte název nového plánu aplikace služby a výběr odpovídajícího umístění.  Klikněte na tlačítko vrstvy ceny a vyberte odpovídající ceny vrstvy pro službu. Když vyberete **Zobrazit vše** zobrazíte další možnosti ceny, například **zdarma** a **sdílené**.  Po výběru ceny vrstvy klikněte na tlačítko **Vybrat** .  Po návratu do zásuvné **plán služeb aplikací** klikněte na **OK**.

5. Klikněte na **vytvořit**. Zřízení back-end mobilní aplikace může trvat několik minut.  Po zřízení back-end mobilní aplikaci portálu otevře zásuvné **Nastavení** pro mobilní aplikaci back-end.

Po vytvoření back-end mobilní aplikaci můžete se připojit existující databázi SQL na vaše mobilní aplikaci back-end nebo vytvořte novou databázi SQL.  V této části vytvoříme databázi SQL.

> [AZURE.NOTE]Pokud máte databázi ve stejném umístění jako back-end mobilní aplikaci, můžete místo toho zvolit **použití existující databáze** a vyberte databázi. Použití databáze do jiného umístění se nedoporučuje, protože vyšší čekacích dob.

6. V nové mobilní aplikaci back-end, klikněte na **Nastavení** > **Mobilní aplikaci** > **dat** > **+ Přidat**.

7. Na zásuvné **Přidat datové připojení** , klikněte na **SQL databáze – konfigurace požadovaná nastavení** > **vytvořit novou databázi**.  Do pole **název** zadejte název nové databáze.

8. Klikněte na **Server**.  V zásuvné **Nový server** zadejte název serveru jedinečný v poli **název serveru** a zadejte vhodný **přihlášení správce serveru** a **heslo**.  Zkontrolujte, zda že je zaškrtnuto políčko **Povolit azure služby pro přístup k serveru** .  Klikněte na **OK**.

    ![Vytvoření databáze Azure SQL][6]

9. Na zásuvné **novou databázi** klikněte na **OK**.

10. Zpět na zásuvné **Přidat datové připojení** vyberte **připojovací řetězec**, zadejte přihlašovací jméno a heslo, které jste zadali při vytváření databáze.  Pokud používáte existující databázi, zadejte přihlašovací údaje pro tuto databázi.  Po zadání, klikněte na **OK**.

11. Zpět na zásuvné **Přidat datové připojení** , klikněte na tlačítko **OK** k vytvoření databáze.

<!--- END OF ALTERNATE INCLUDE -->

Vytvoření databáze může trvat několik minut.  Sledování průběhu nasazení pomocí **oznamovací** oblasti.  Není průběhu dokud úspěšně nasazena databáze.  Po úspěšném zavedení připojovací řetězec se vytvoří instanci systému SQL databáze ve vaší mobilní back-end nastavení aplikace.  Zobrazí se toto nastavení aplikace v **Nastavení** > **Nastavení aplikace** > **připojovací řetězec**.

### <a name="howto-tables-auth"></a>Postup: Požadovat ověření pro přístup k tabulkám

Pokud chcete použít aplikaci služby ověřování s koncovým tabulek, musíte nakonfigurovat aplikaci služby ověřování [Azure portál] nejdřív.  Podrobné informace o konfiguraci ověřování v aplikaci služby Azure zkontrolujte Průvodce konfigurací zprostředkovatele identit, který chcete použít:

- [Konfigurace ověřování služby Azure Active Directory]
- [Konfigurace ověřování Facebooku]
- [Konfigurace ověřování Google]
- [Postup při konfiguraci Microsoft Authentication]
- [Postup při konfiguraci Twitter ověřování]

Každá tabulka obsahuje vlastností, které lze použít k řízení přístupu k tabulce.  Následující příklad ukazuje staticky definovaný tabulku s požadováno ověření.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Vlastnost access může trvat jednu ze tří hodnot

  - *anonymní* označuje, že je zobrazíte data bez ověřování povolena klientské aplikace
  - *ověření* indikuje, že klientské aplikaci musí poslat token platný ověřovací s žádostí o
  - *Zakázat* označuje, že v této tabulce je aktuálně vypnuto

Pokud je definován vlastnost přístup, je povolen neověřený přístup.

### <a name="howto-tables-getidentity"></a>Postup: ověřování deklarací pomocí tabulek

Můžete nastavit různým deklarací, které jsou požadovány při nastavení ověřování.  Tato tvrzení nejsou dostupné prostřednictvím `context.user` objektu.  Však je lze získat prostřednictvím `context.user.getIdentity()` metody.  `getIdentity()` Metoda vrátí Promise, jehož výsledkem objektu.  Objekt je nastaven tak, že metody ověřování (facebook, google, twitter, microsoftaccount nebo aad).

Například pokud jste nastavili Account Microsoft ověřování a žádost o převzetí e-mailové adresy, můžete přidat e-mailovou adresu záznam u řadiče následující tabulky:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Zjistěte, jaké nároků jsou k dispozici, pomocí webového prohlížeče můžete zobrazit `/.auth/me` koncový bod webu.

### <a name="howto-tables-disabled"></a>Postup: zakázání přístupu k určité tabulky operace

Kromě zobrazená v tabulce, můžete být vlastnost access slouží k určení jednotlivé operace.  Existují čtyři operace:

  - *Přečtěte si* je RESTful získat operace v tabulce
  - *Vložení* je operace RESTful příspěvek v tabulce
  - *Aktualizovat* je RESTful oprava operace v tabulce
  - *Odstranění* je operace RESTful DELETE na tabulce

Chcete například poskytnout neověřených tabulku jen pro čtení:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Postup: Upravte dotaz, který se používá se operace tabulky

Běžné požadavku na operace tabulky je poskytovat s omezeným přístupem zobrazení dat.  Můžete třeba nastavit tabulky, který je označený ID ověřeného uživatele tak, aby se jenom číst nebo aktualizovat záznamy sami.  Tuto funkci poskytuje následující definice tabulky:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Operacích, které obvykle spuštění dotazu na mají vlastnost dotazu, můžete upravit s klauzuli where klauzule. Vlastnost dotazu je [QueryJS] objekt, který se používá k převodu dotazu OData na jinou hodnotu, kterou můžete zpracovat back-end data.  Pro názvy případů jednoduché rovnosti (podobné tomuto předchozí) mohou sloužit mapy. Můžete také přidat konkrétní používaných klauzulí SQL:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Jak: Konfigurace měkké odstranit v tabulce

Odstranit měkké nedojde k odstranění skutečně záznamy.  Místo toho označí je jako odstraněný v databázi nastavením sloupci odstraněné true (pravda).  Pokud klienta SDK mobilní používá IncludeDeleted() SDK Azure mobilní aplikace automaticky odstraní měkkých odstraní záznamy z výsledků.  Konfigurace tabulku pro měkké odstranit, nastavte u `softDelete` vlastnost v souboru definice tabulky:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Zavede mechanismus při odstranění záznamů – buď z klientské aplikaci prostřednictvím WebJob funkce Azure nebo prostřednictvím vlastní rozhraní API.

### <a name="howto-tables-seeding"></a>Postup: inokula databáze s daty

Při vytváření nové aplikace, můžete pro tabulku s daty.  To možné vykonat během soubor JavaScript definice tabulky takto:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Ohlašovat dat se provádí pouze při tabulce se vytvořil SDK Azure mobilní aplikace.  Pokud existuje v databázi, žádná data vložit do tabulky.  Pokud je zapnutá dynamické schéma, schématu odvozeny od peckami data.

Doporučujeme explicitně volat `tables.initialize()` způsob, jak vytvořit tabulku při spuštění.

### <a name="Swagger"></a>Postup: povolení Swagger podpory

Azure aplikace služby mobilní aplikace je součástí předdefinované [Swagger] podpory.  Povolit Swagger podpory, nejprve nainstalujte rozhraní swagger jako závislostí:

    npm install --save swagger-ui

Po instalaci můžete povolit Swagger podporu konstruktoru Azure mobilní aplikace:

    var mobile = azureMobileApps({ swagger: true });

Budete pravděpodobně jenom chcete povolit Swagger podporu v edicích vývoj.  Můžete to udělat využitím `NODE_ENV` nastavení aplikace:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Koncový bod swagger je umístěný na http://_yoursite_.azurewebsites.net/swagger.  Máte přístup k rozhraní Swagger prostřednictvím `/swagger/ui` koncového bodu.  Pokud se rozhodnete sdělit vyžadují ověřování připojení přes celou aplikaci, vytvoří Swagger chybu.  Nejlepších výsledků dosáhnete, vyberte Povolit neověřené požadavky pomocí ověřování služby Azure aplikace / se tak mohli ověřovat nastavení, pak ovládací prvek pomocí ověřování `table.access` vlastnost.

Můžete také přidat možnost Swagger vaše `azureMobile.js` souboru, pokud chcete, jenom Swagger podpory při vytváření místně.

## <a name="a-namepushpush-notifications"></a><a name="push">Nabízená oznámení

Mobilní aplikace lze integrovat s Azure oznámení rozbočovače umožňující odešlete miliony zařízení cílových nabízených oznámení pro všechny hlavní verze platformy. Pomocí oznámení rozbočovače můžete posílat nabízená oznámení iOS, zařízení Android a Windows. Další informace o oznámení rozbočovače naleznete v tématu [Přehled rozbočovače oznámení](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Postup: odeslání nabízená oznámení

Následující kód ukazuje, jak pomocí objektu nabízených odešlete zařízení s iOS registrovaných vysílání nabízená oznámení:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Vytvořením registrace nabízených šablony z klienta, můžete místo toho odeslat zprávu nabízených šablony zařízení na všechny podporované platformy. Následující kód ukazuje, jak k odeslání oznámení šablony:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="push-user"></a>Postup: odesílání nabízených oznámení pro ověřeného uživatele pomocí značek

Když ověřený uživatel zaregistruje nabízených oznámení, se automaticky přidají značku ID uživatele k registraci. Pomocí této značky můžete poslat nabízená oznámení na všech zařízeních registrovaných určitým uživatelem. Následující kód získá ID zabezpečení uživatele, který tvoří žádosti a pošle nabízená oznámení šablony každé registrace zařízení pro daného uživatele:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Při registraci nabízených oznámení z ověřené klienta, ujistěte se, že dokončením ověřování před pokusem o registraci.

## <a name="CustomAPI"></a>Vlastní rozhraní API

###  <a name="howto-customapi-basic"></a>Postup: definování vlastních rozhraní API


Kromě datových rozhraní API prostřednictvím koncový bod /tables Azure mobilní aplikace můžete pokrytí vlastní rozhraní API.  Vlastní rozhraní API jsou definované obdobně definice tabulky a dostanete stejné zařízení, včetně ověřování.

Pokud chcete aplikaci služby ověřování pomocí rozhraní API vlastní, musíte nakonfigurovat aplikaci služby ověřování [Azure portál] nejdřív.  Podrobné informace o konfiguraci ověřování v aplikaci služby Azure zkontrolujte Průvodce konfigurací zprostředkovatele identit, který chcete použít:

- [Konfigurace ověřování služby Azure Active Directory]
- [Konfigurace ověřování Facebooku]
- [Konfigurace ověřování Google]
- [Postup při konfiguraci Microsoft Authentication]
- [Postup při konfiguraci Twitter ověřování]

Vlastní rozhraní API jsou definované stejným způsobem jako rozhraní API tabulek.

1. Vytvoření **rozhraní api** adresáře
2. Vytvoření souboru rozhraní API definice JavaScript v adresáři **rozhraní api** .
3. Použijte metodu import pro import adresáře **rozhraní api** .

Tady je definici rozhraní api prototypů na základě vzorku basic aplikace, kterou jsme použili dříve.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Podívejme se příklad rozhraní API, který vrací kalendářní datum serveru pomocí metody _Date.now()_ .  Tady je api/date.js soubor:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Každý parametr je jedním ze standardní RESTful slovesa – GET, příspěvek, oprava nebo odstranit.  Metoda je standardní [ExpressJS Middleware] funkci, která odešle požadované výstupní.

### <a name="howto-customapi-auth"></a>Postup: Požadovat ověření pro přístup k rozhraní API vlastní

Azure mobilní aplikace SDK provádí ověřování v stejnou výzvu koncový bod tabulek a vlastní rozhraní API.  Pokud chcete přidat ověřování k rozhraní API vyvinuté v předchozí části, přidejte vlastnosti aplikace **access** :

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Můžete také zadat ověřovací na konkrétní operace:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Stejný token, který se používá pro koncový bod tabulky musí používat pro vlastní rozhraní API, které vyžadují ověřování.

### <a name="howto-customapi-auth"></a>Postup: Zpracovat odesílání velkých souborů

Azure mobilní aplikace SDK používá [textu analyzátor middleware](https://github.com/expressjs/body-parser) přijmout a dekódovat text na váš příspěvek.  Předem můžete nakonfigurovat textu analyzátor potvrďte, že odesílání souborů větších:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Soubor je kódování před transmission base-64.  To zvětšuje velikost skutečného nahrávání (a tedy velikost musíte vzít v úvahu pro).

### <a name="howto-customapi-sql"></a>Postup: spustit vlastní příkazy SQL

SDK Azure mobilní aplikace umožňuje přístup k celé kontextu přes objekt request umožňují snadno provést parametry příkazů SQL s cílem zprostředkovatel definovaný dat:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Ladění, snadno tabulky a snadno rozhraní API

### <a name="howto-diagnostic-logs"></a>Postup: ladění Diagnostika a řešení potíží s aplikací Azure Mobile

Aplikace služby Azure obsahuje několik ladění a řešení problémů s postupy pro Node.js aplikace.
Odkažte na následující články vám pomůžou začít při řešení potíží s vaší back-end Node.js Mobile:

- [Sledování aplikací služby Azure]
- [Povolit diagnostické protokolování v aplikaci služby Azure]
- [Poradce při potížích s služby Azure aplikace ve Visual Studiu]

Aplikace Node.js mají přístup do široké řady nástrojů diagnostickém protokolu.  V Azure mobilní aplikace Node.js SDK interně, používá [Winston] protokolování diagnostiky.  Protokolování je automaticky zapnutá povolení režimu ladění nebo nastavením nastavení aplikace **MS_DebugMode** [Azure portálu]true (pravda). Generované protokoly se zobrazí v protokolech diagnostiky na [Azure portál].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Postup: práce s tabulkami snadno na portálu Azure

Snadno tabulek v portálu umožňují vytváření složek a práce s tabulkami přímo na portálu. Dokonce úpravou operace tabulky pomocí editoru aplikace služby.

Po klepnutí na tlačítko **snadno tabulek** v nastavení webu back-end, můžete přidat, změnit nebo odstranit tabulku. Najdete v části data v tabulce.

![Práce s tabulkami snadno](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Následující příkazy jsou k dispozici na panelu s příkazy pro tabulku:

+ **Změna oprávnění** - Úprava oprávnění pro čtení, vložení, aktualizovat a odstraňovat operace v tabulce. 
  Povolení anonymního přístupu, vyžadují ověřování nebo zakázat všechny přístup k operaci jsou možnosti. 
+ **Úprava skriptu** - soubor skriptu pro tabulku se otevře v editoru služby aplikace.
+ **Správa schématu** - přidat nebo odstranit sloupce nebo změnit index tabulky.
+ **Odstranit tabulku** – zkrátí existující tabulky se odstranění všech řádků dat, ale necháte schématu beze změny.
+ **Odstranit řádky** - odstranit jednotlivé řádky dat.
+ **Zobrazení datových proudů protokoly** - se připojuje ke službě streamování protokolu pro web.

###<a name="work-easy-apis"></a>Postup: spolupráce s snadno rozhraní API v portálu Azure

Snadno rozhraní API v portálu umožňují vytvářet a pracovat s vlastní rozhraní API přímo na portálu. Můžete upravit rozhraní API skriptů pomocí editoru aplikace služby.

Po kliknutí **Snadno rozhraní API** nastavení back-end webu, můžete přidat, změnit nebo odstranit vlastní koncový bod rozhraní API.

![Práce s snadno rozhraní API](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Na portálu můžete Změna přístupových oprávnění pro určitou akci HTTP, upravit soubor skriptu rozhraní API v editoru služby aplikace nebo zobrazení datových proudů protokoly.

###<a name="online-editor"></a>Postup: Upravit kód v editoru služeb aplikací

Portál Azure umožňuje upravit Node.js back-end skript souborů v editoru služby aplikace aniž byste museli stáhnout projektu do místního počítače. Abyste mohli upravovat soubory skript v editoru online:

1. V zásuvné back-end vaše mobilní aplikaci, klikněte na **všechna nastavení** > **snadno tabulek** nebo **Snadno rozhraní API**, klikněte na tabulku nebo rozhraní API a potom klikněte na **Upravit skript**. Soubor skriptu je otevřen v editoru služby aplikace.

    ![Editor aplikace služby](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. Proveďte požadované změny souboru s kódem v editoru online. Změny se ukládají automaticky při psaní.


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Rychlý úvod Android klienta]: app-service-mobile-android-get-started.md
[Rychlý úvod Apache Cordova klienta]: app-service-mobile-cordova-get-started.md
[iOS rychlý úvod klienta]: app-service-mobile-ios-get-started.md
[Rychlý úvod Xamarin.iOS klienta]: app-service-mobile-xamarin-ios-get-started.md
[Rychlý úvod Xamarin.Android klienta]: app-service-mobile-xamarin-android-get-started.md
[Rychlý úvod Xamarin.Forms klienta]: app-service-mobile-xamarin-forms-get-started.md
[Rychlý úvod klienta pro Windows Store]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[synchronizace dat v režimu offline]: app-service-mobile-offline-data-sync.md
[Konfigurace ověřování služby Azure Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Konfigurace ověřování Facebooku]: app-service-mobile-how-to-configure-facebook-authentication.md
[Konfigurace ověřování Google]: app-service-mobile-how-to-configure-google-authentication.md
[Postup při konfiguraci Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Postup při konfiguraci Twitter ověřování]: app-service-mobile-how-to-configure-twitter-authentication.md
[Průvodce nasazením Azure aplikace služby]: ../app-service-web/web-sites-deploy.md
[Sledování aplikací služby Azure]: ../app-service-web/web-sites-monitor.md
[Povolit diagnostické protokolování v aplikaci služby Azure]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Poradce při potížích s služby Azure aplikace ve Visual Studiu]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[určení verze uzel]: ../nodejs-specify-node-version-azure-apps.md
[použití moduly uzel]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure portálu]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[Ukázka basicapp na GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[Ukázka úkol na GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[Ukázky adresáře na GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Nástroje Node.js 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[MSSQL Node.js balíčku]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
