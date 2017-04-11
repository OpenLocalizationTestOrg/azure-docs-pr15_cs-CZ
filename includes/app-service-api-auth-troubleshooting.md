Většinu času chyby ověření vyplynou ze nesprávné nebo nekonzistentní nastavení. Tady je několik návrhů specifické pro zkontrolujte.

* Ujistěte se, že zmeškáte neměli tlačítka **Uložit** kamkoli. Toto je často snadné a výsledek je, že budete se díváte na správné hodnoty pro stránku portálu, ale ve skutečnosti byla uložená v příslušné Azure prostředí nebo aplikace Azure AD.
* Nastavením v **Nastavení aplikace** zásuvné portálu Azure Ujistěte se, že je vybraný správné rozhraní API aplikace nebo webové aplikace při nastavení zadaných.  Taky zkontrolujte, že nastavení zadaných jako **Nastavení aplikace** a ne **připojovací řetězec**je podobný formát dva oddíly.
* Ověřování front-end JavaScript, stáhněte si manifest znovu a ověřte, že `oauth2AllowImplicitFlow` úspěšně změnil na `true`.
* Ověřte použit HTTPS, když jste nakonfigurovali adresy URL:

    * V kódu projectu
    * V CORS
    * V nastavení aplikace Azure prostředí pro každou rozhraní API aplikace a v prohlížeči
    * V nastavení aplikace Azure AD.
    
    Všimněte si, že pokud zkopírujte adresu URL aplikace API z portálu často má `http://` a budete muset ručně ji změnit na `https://`.

* Ujistěte se, že byla úspěšně nasazená žádné změny kódu. Například ve více projektové řešení je možné kód projektu změnit a zvolte jednu z ostatních omylem, když chcete nasadit změnit.
* Ujistěte se, že budete adresy URL HTTPS v prohlížeči není adresy URL HTTP. Ve výchozím nastavení Visual Studio vytvoří profily s adresou URL HTTP publikování a je to, co se otevře v prohlížeči po nasazení projektu.
* Při ověřování front-end JavaScript zkontrolujte, že CORS správně nakonfigurované na rozhraní API aplikace, která volá kód JavaScript. Pokud jste na pochybách o tom, jestli je to související CORS, zkuste "*" jako adresa URL povolené origin. 
* Pro JavaScript se front-end otevřete kartu Vývojář nástroje konzoly prohlížeče získat další informace o chybě a průzkum požadavků HTTP v síti. Chybové zprávy konzoly však může být zavádějící. Pokud se zobrazí zpráva s upozorněním na CORS chybu, může být skutečné problém ověřování. Můžete zkontrolovat, že pokud jde o případ spuštěním aplikace pomocí ověřování dočasně dočasně zakázané.
* .NET rozhraní API aplikace Ujistěte se, že tolik informace se zobrazují v chybových zprávách největšímu nastavením [customErrors režimu vypnutí](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* Aplikace .NET API spusťte [vzdálenou relaci ladění](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)a zkontrolujte hodnotách proměnných, které předávají kód, který používá ADAL získat nosný token nebo kód, který zkontroluje pohledávky vůči ID očekávané služby základní. Všimněte si, že váš kód může vystopovat hodnot konfigurace z mnoha různých zdrojů, tak, aby bylo možné najít překvapení tímto způsobem. Řekněme, že napíšete `ida:ClientId` jako `ida:ClientID` při konfiguraci nastavení aplikaci služby Azure prostředí, může se zobrazit kód `ida:ClientId` hodnotu, která je hledáte z nastavení(Web.config)) ignorování nastavení aplikace služby Azure. 
* Pokud věci nefungují v normálním okně Internet Exploreru, existující protokolu aplikace může interferovat; Zkuste InPrivate a zkuste Chrome a Firefox.
