<properties 
    pageTitle="Azure hledání Developer Případová studie: Jak funguje WhatToPedia založený na Microsoft Azure portál infomedia | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu" 
    description="Naučte se vytvářet informace portálem a meta vyhledávacího webu pomocí vyhledávání Azure, vyhledávací služby cloudu hostované pro vývojáře." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Azure hledání vývojář Případová studie

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Jak [WhatToPedia.com](http://whattopedia.com/) sestavují portál infomedia na Microsoft Azure

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">Skvělý nápad</font> 


Náš myšlence je sestavení informační portál, která vám pomůže zákazníci spojit s prodejců prostřednictvím velmi důležité, rozsah hledání setkat i v případě, podobně jako jak cestování portály POZVYHLEDAT turisty nahoru s hotelů, restaurace a zábavu v uncharted oblastí. 

Na portálu jsme výsledcích dodá vyhledávání výjimečně vysoce kvalitní nad daty obchodě a bude v daném trhu, pomáhají zákazníci ukládá podle umístění a jiných zařízení prodejce, od obsahuje. Jsme ohlašovat vyhledávací s počáteční sadu dat, ale hlubší hodnota bude vytvořen určitou dobu, pomocí tohoto prodejce účastníky, kteří zveřejňují informace o své firmě. Propagace nové zboží, oblíbených značek, podnikových speciální technické – – všechny jsou příklady data, která přidá hodnotu registrovat. Tato data je vlastním vykázaného a do souhrnu hledání po prodejce, od zaregistruje jako účastníka. Zobrazování reklam, iniciály modelu předplatné poskytovat proudu výnosy našeho nového podnikání.

Hledání bude modelem interakce nejvíce uživatele na platformě čistého cloudu. Pro účely měřítko a nízké nákladů téměř všechno, co děláme z práce s portálem ovládacímu prvku zdroje, budou až server nějaké online služby. Pomocí vyhledávacího webu, které obsahuje funkce, které potřebujeme mimo pole jsme rychlé vytvoření aplikace Vyhledávací služby, aniž byste museli vytváření a správě hledání modulu označována.

## <a name="what-we-built"></a>Jsme vytvořili

WhatToPedia je společnost infomedia po spuštění. Jsme vytvořili [WhatToPedia.com](http://whattopedia.com/) – – aktuálně v testu market v severní Evropě s live přejít splnění 2 února 2015. Náš zákaznické základny je primárně prodejnách cihel a Malta potřebují dostupnou online stavu, snadno spravovat a udržovat.

Náš úkolu je třeba zajistit zákazníci prostřednictvím prostředí skvělé online hledání, zvýšení výsledky založené na město nebo složce, značky obsahují názvy nebo ukládat typy. Získání zákazníci má dominový efekt motivačních prodejců pro přihlášení k odběru naše portálu. Předplatné je placených, dostupnou sazbou.

 ![][7] 

Po registraci pro předplatné v maloobchodě převezme jejich profil (vytvořil původně nám z placené dat), aktualizace pomocí ostatní údaje o propagační akce, doporučené značky nebo oznámení. Podnikových funkce, jako jsou jazyky používané, hodnoty s měnou potvrdili, bez daně nákupní, může být vlastním nahlášeného lépe upoutání pozornosti zákazníci, kteří pracují na hledané těchto zařízení.

## <a name="who-we-are"></a>Kdo jsme

Je mé jméno Kutěj Segato (Microsoft Consulting) a můžu spolupracovali Jesperu Boelling vést vývojář na WhatToPedia navrhnout řešení. 

WhatToPedia je po spuštění test marketing jeho nové portálu obchodní Švédsko, kde jsou nejčastěji 60 000 prodejců cihel a Malta podniky (malé a střední podniky). Protože víme, že osoba nákupní v Evropě speaks více jazyků a provede více měny, abychom vytvářet řešení, která pokryly vícejazyčné zákazník. Jsme potřeby ale zjistili jste, vyhledávacího webu, který podporuje naše požadavky pro více jazyků v Azure vyhledávání.

Azure hledání byla hra měnič naše projektu. Před dostupnost Azure hledání jsme použito značné energie práce prostřednictvím kinks sestavování naší vyhledávacího webu. S Azure Search Server nějaké online služby odebrali naše řešení největších mezní technické a správy, které chtěli, začíná na trh rychleji a s robustnější vyhledávání.  

## <a name="how-we-did-it"></a>Jak můžeme nebyla

Náš zrakem bylo vytvořit infrastrukturu dokončení založené na cloud services pouze. Microsoft byla vybrána jako strategic platformy, protože byla zprostředkovateli nabízených takové služby (pro spolupráci a vývoj), měřítko na vyžádání a dostupnou ceny.
 
### <a name="high-level-components"></a>Součástí na vysoké úrovni

Jsme vytvořili business, nikoli pouze na webu. Podpora celý úsilí potřebná široká řada nástrojů a aplikací. Jsme přijatou Visual Studio Visual Studio týmovou pro vývoj, Online služby Team Foundation (TFS) pro ovládací prvek zdroje a scrum správu, Office 365 pro komunikaci a spolupráci a samozřejmě Microsoft Azure pro všechny operace týkající se sítí a úložiště. Pomocí aplikace Visual Studio integrovaném vývojovém prostředí k dispozici přímé vytváření Azure, integrací TFS online poskytuje zesílení další produktivity.

Následující diagram znázorňuje uceleném komponenty použité v infrastruktury WhatToPedia.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Jak používáme Microsoft Azure

Prohlížíte zelená pole v předchozím obrázku, uvidíte, že řešení WhatToPedia jsou založeny na těchto služeb:

- [Azure hledání](https://azure.microsoft.com/services/search/)
- [Azure webů pomocí MVC 4](https://azure.microsoft.com/services/websites/)
- [Azure WebJobs naplánovaných úkolů](../app-service-web/websites-webjobs-resources.md)
- [Databáze Azure SQL](https://azure.microsoft.com/services/sql-database/)
- [Úložiště objektů BLOB Azure](https://azure.microsoft.com/services/storage/)
- [Doručování e-mailů SendGrid](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

Velmi srdce řešení je dat a hledat. Tok dat od prodejce, od kterého poskytovatele koncovému zákazníkovi je znázorněno níže:

  ![][9]

Primární datový úložiště je prodejce a účetní data v databázi SQL Azure. Tím se skládá z počáteční sadu dat a data specifické pro prodejce přidaná určitou dobu. Pracujeme s programem Azure WebJob publikovat aktualizace z databáze SQL souhrnu hledání v hledání Azure.

### <a name="presentation-layer"></a>Prezentace vrstvy

Na portálu je web na Azure implementovaná v MVC 4 a [Twitter zavádění](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29). Zvolili jsme MVC, protože nabízí mnoho opět dostanete čistší přístup do formátu HTML než vývoj založené na formulářích ASP.NET. Abyste nemuseli vytvoření aplikace pro víc zařízeních a udržovat více mobilní platformy, byla Twitter zavádění rozhodli podporují zařízení a platformách.

### <a name="authentication-permissions-and-sensitive-data"></a>Ověření, oprávnění a důvěrné údaje

Zákazníci anonymně přejděte na web. Jako takové neexistují žádné přihlašovací požadavky pro zákazníci ani se nám uložení dat příjemce. 

Prodejců jsou na jinou část textu. Tady jsme obsahují informace z veřejného profilu, fakturačních údajů a mediální obsah, který chcete vystavit na webu. Každého prodejce, kdo se přihlásí k webu potřebujete slouží k ověření uživatele před prováděním aktualizace profilu účastníka přihlašovací jméno uživatele.  Jsme bezpečně ukládá všechny odběratele dat v databázi SQL Azure a objektů BLOB Azure úložiště.
Jsme zvolil datového modelu pro ověřování na základě .NET založené na formulářích ověření. Zvolili jsme tento přístup pro zjednodušení; Potřebujeme neměli role, uživatelské rozhraní podpory a dalších nadbytečné funkcí, které jsou součástí jiných přístupů. 

Abyste měli jistotu, že prodejců zobrazit pouze data, která je součástí, jsme vytvořili v maloobchodě ID pro každého prodejce, následně používanou ve všech čtení a zápis operace týkající se data specifická pro prodejce. Tento způsob jsme našli není potřeba udělit oprávnění k databázi jednotlivé prodejců. Všechny prodejci systém interaktivně ovládat v části role jednu databázi, s ID prodejce jako postup izolace naše data.

Protože naše firmy všechny informace o podřízené efekty (řízení další firmy prodejci, vytváření pobídky inzerce a přihlášení k odběru) jsme upoutat čára na zpracování nákup prostřednictvím webu. Jako takové nenajdete nákupní košík na webu, což zjednodušuje požadavky na naše zabezpečení. 

Jiné zjednodušení, které jsme použili byl externí naše splatných operace fakturace a účty. Pomocí směrování údaje o platbě odběratele přímo do jiných výrobců ([SveaWebPay](http://www.sveawebpay.se/)), doporučujeme snížení rizik přidružení k ukládání a chrání citlivá data v naší ukládají data. 

### <a name="search-engine"></a>Vyhledávacího webu

Základní naše řešení je vyhledávací založená na Azure vyhledávací služby serveru. Původně jsme vytvořili vlastní vyhledávacího webu, ale během tohoto procesu jsme realizují složitost plánování řízené úsilí byl velmi vysoké skutečně a, zobrazí výzva, abychom vám zvažte další možnosti. 

Základní funkce, které byly nejdůležitější námi zahrnout:

- Filtry
- Působí navigace
- Zvýšení výsledků
- Procházení AJAX

Hledání internet přepnout do nám následující videa, která daly podnět, abychom vám Azure vyhledávání vyzkoušejte: [Hloubkové postupy v TechEd Europe](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Po sledování videa, byly připravená k vytváření prototypů podle jsme viděli. Protože jsme zařazený do MVC datový model, byl vytváření prototypů jednoduché, protože dat obsažených vyhledávání podmínky a jsme pracovali už požadavky pro jak chtěli jsme seřadit, fasetový a filtrujte data. 

Je to, jak jsme vytvořili prototypů.

**Konfigurace služby Azure hledání**

1. Přihlášení k portálu Classic Azure a přidat vyhledávací služby na naše předplatné. Jsme použili sdílené verze (zdarma s naše předplatné).
2. Vytvoření indexu. Pro typ. jsme použili portálu uživatelského rozhraní definovat vyhledávacího pole a vytvoření bodování profily. Náš bodování profil je založeno na datech umístění: země | Město | adresu (viz: Přidání hodnocení profily).
3. Kopírování adresy URL služby a správu klávesami rozhraní api pro naše soubory konfigurace. Tento klíč je na stránce vyhledávací služby na portálu a pracovní postup slouží k ověření služby.
    
**Můžete vyvíjet úlohy indexování hledání – konzoly Windows**

1. Přečtěte si všechny prodejci z databáze.
2. Volání Azure služby rozhraní API pro hledání nahrát prodejců jeden po druhém (viz: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Nastavení vlastností v databázi, která reseller indexováno přírůstková indexování. Jsme nebyla přidáním pole "indexování", která ukládá stav indexování každého profilu (indexovat nebo ne). 

Naleznete v dodatku k fragment kódu, který vytváří úlohy indexování.

**Můžete vyvíjet webového portálu hledání – MVC**

1. Volání vyhledávací služby Azure zobrazíte všechny dokumenty z vyhledávání (viz: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. Extrahovat sledovat ze odpovědi služby hledání (pomocí json.net http://james.newtonking.com/json)
   - Výsledky
   - Fasetami
   - Počty výsledků
   - Vytvořte uživatelské rozhraní pro zobrazování výsledků vyhledávání, fasetami a počty (jsme už měli toto).

Toto je kód, který jsme použili výsledků dosáhnete z Azure vyhledávání:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Zvýšení podle umístění**

Pravděpodobně nejdůležitější požadavek ověřit prototypů zahrnuté přidání klíčové slovo pro hledání umístění pro dotaz. Je důležité náš portálem, pokud uživatel zadá město název do pole Hledat dotaz, který by prodejci v dané město vyšší než prodejců s klíčového slova Město v popisu seřadit. Pro tento požadavek jsme použili bodování profilu řadit pole Město vyšší než dalších polí.

**Podpora více jazyků**

Potřebujeme a po zobrazení správné výsledky hledání v správné jazycích, nabízí možnost pro stejných výsledků hledání v různých jazycích. Byly sociálními tohoto problému: 

- Hledání slov ve více jazyků
- Zobrazovat výsledky hledání v správný jazyk.

Část prezentace jsme vyřešit přidáním jednotlivých jazyků lokalizované textem a vlastnost pomocí jazyka v dokumentu. Pokud uživatel zadá hledaný termín, jsme uživatele `$filter` výrazů filtrovat podle jazyka uživatel zvolil.

Každý z dokumentů má skryté vlastnost s názvem "cities" založený na typu kolekce. Tato vlastnost ukládá názvy měst všechny jazyky, což umožňuje uživateli hledání ve víc jazycích.

###<a name="data-storage"></a>Ukládání dat

Všechna data (profilu, předplatné a účetní) je uložen v databázi SQL. Všechny soubory médií jsou uložené v úložišti objektů BLOB Azure, včetně obrázků a videí poskytovanou prodejce, od. Pomocí samostatných úložiště objektů BLOB izoluje efekty odesílání souborů. soubory jsou spoluvytváření mingled s webem, nikdy jsme nemuseli znovu vytvořit web kdykoli přičteme soubory.

Důležité výhodou naše návrh úložiště je, že více vývojářů můžete sdílet jediné vývojové úložiště. Požadavky na projektu WhatToPedia určitým mohli vytvářet vývojové prostředí v rámci 15 minut, včetně dat prodejce, obrázky a videa. Získání nejnovější data ze TFS Online, skript SQL a spuštěním úlohy importu, dokončeno prostředí můžou být platnosti během chvilky vůbec. Tento postup také zlepšuje pracovní proces.

###<a name="webjobs"></a>WebJobs

Aktualizace dat do indexu používáme Azure WebJobs. Vytvořením úlohy indexování hledání byl indexování část velmi snadné integrovat do naše řešení. Pouze změny kódu jsme byl tak, aby zahrnoval indexování úloha byla přidáte `Indexed` pole do naší datového modelu a označují stav indexování. Kdykoli přidat nebo aktualizovat, nový profil `Indexed` pole je nastavena na hodnotu false. To platí Pokud prodejce, od změnou přihlásil data profilu prostřednictvím portálu.  

Úlohy vyhledá všechny řádky s `Indexed` nastavena na hodnotu false. Pokud nalezne řádku, dokument publikovaný na Azure vyhledávání a potom `Indexed` je nastaveno true (pravda). Máme neměli plán pro přidání a aktualizace dat, protože Azure vyhledávací služba skutečně má na starosti to. Pokud chcete přidat dokument, který již existuje, udělá službu aktualizace automaticky.

Všechny webové úlohy byla vytvořili jako konzoly aplikace, které můžou být nahráli Azure webů jako soubory ZIP rozbaleny a stiskněte naplánovaná.

Plánované každých 5 minut jako naplánovaný úkol. Jsme vypočítána, že službě trvá, než přibližně tři minuty odesílat 3000 dokumenty, která byla v rámci naší požadavky: 

> [AZURE.NOTE] Je funkce indexování prototypů, která byla naposledy zavedená v Azure vyhledávání. Tato funkce dostali pozdě abychom ho použít v naší první vydání, ale zobrazuje se problém vyřešit stejné, kterou jsme použili naše indexování úlohu, což je k automatizaci operací načítání dat.


###<a name="backup-strategy"></a>Strategie zálohování

Jsme navržený vícevrstevného záložní strategii obnovit z oblasti scénáře z Katastrofální selhání dolů obnovení jednotlivé transakce. Prostředky k ochraně zahrnout tři typy dat (webu odběratele dat a mediální soubory). 

Nejdřív přijetím webu zdrojového kódu v TFS Online nám vědět, že pokud webu přejde, jsme můžete vytvořit znovu ho změny provedené od TFS. 

Odběratele dat v databázi SQL Azure je většina citlivé materiálů. Jsme zpět tento pomocí předdefinované funkce (najdete v článku [zálohy databáze SQL Azure a obnovení](http://msdn.microsoft.com/library/azure/jj650016.aspx)). Plán zálohování je úplnou zálohu databáze jednou týdně, zálohování rozdílné databáze jednou za den a zálohy protokolu transakce každých 5 minut.  Možnost velikost dat, toto řešení je víc než odpovídající pro naše svazky okamžité a očekávaná data.

Třetí jsme ukládat soubory obrázků a videa v úložišti objektů BLOB Azure. Jsme pořád vyhodnocují ultimate záložní plánu tato data zvažovat Cloudberry Explorer pro Azure jako možných řešení. Prozatím používáme WebJob zkopírovat obrázků a videí do jiného umístění.

##<a name="what-we-learned"></a>Co jste se dozvěděli

Protože jsme už máte data, byla snadno vytvořit ověření koncepce. Hodin jsme měli prototypů s podmínkami údaje, stránkování, zařazených jako profily a výsledků hledání. Výsledky hledání byly tak přesné že Rozhodli jsme odebrání některých filtry prezentovány koncovému zákazníkovi. 

Největší překvapení nám byl jsme rychlosti další Azure vyhledávání a kolik jsme máte zpět. Doslova jsme podle ověření koncepce několik hodin (viz poznámku dole), nahrazení 500 řádky kódu 3 řádky kódu v aplikaci front-end (plus nové WebJob) a získání lepších výsledků při. 

Dříve náš kód implementovaná stránkování, počty a dalších prvků, které jsou standardní hledání. Pomocí vyhledávání Azure, výsledky, které jsme vrátit zahrnout výsledků vyhledávání fasetami stránkování data, vrátí – všechny položky potřeby jsme byly s zadat označována. Zahrnout také zvýšení a předdefinovaných působí navigace, které jsme neměli mít v našem původní řešení.

Největší ověřovací kód při provádění se, že ho byla verze Preview a hledání informací a sdílené prostředí obtížně srozumitelný. Jakmile jsme spojí několik bodů, jsme našli, že pomocí vyhledávací služby Azure byl poměrně jednoduché vzhledem k jeho rozhraní REST API a JSON formát dat ve sloupcích. Rámci jsme můžou volat přímo z nejčastěji otevřít moduly plug-in zdroje jako JQuery JSON.Net a může používáme nástroje jako Fiddler pro rychlé zkoušení a ladění. 

> [AZURE.NOTE] Kromě s údaji prepped, pomohl můžou být nás ostatní vytváření prototypů už rozumí jak funguje technologie, díky nám produktivnější a další appreciative předdefinované funkce hledání. Pokud potřebujete ním vyhledávacího dotazu konstrukcí, působí navigace, filtry, atd. můžete očekávat při vytváření prototypů bude trvat delší dobu. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>Řízení fasetami na stránce prezentace vyhledávání

Jednu z našich learnings během ověření z koncepce bylo plánování fasetami pečlivě předem. Po načtení velké množství dat do řešení, jsme viděli, že byl velkému množství fasetami moc vysoké při prezentaci uživatelům. 

Jsme vyřešit tak, že omezení parametr počet podmínka. Parametr počet ukládá pevný limit počtu fasetami uživateli vrácena. Odkaz, který obsahuje popis parametr počet najdete [tady](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>WebJobs při plánování úkolů

Azure hledání nebyl pouze příjemný překvapení nám. Jsme zjištění, že byl výrazně vyšší než náš předchozí přístup vyplývající pomocí vyhrazené OM systém Windows Plánovač s plánovanými úkoly aktualizace indexu vyhledávání pomocí WebJobs k automatizaci náš provoz načítání dat Azure hledání. WebJobs bylo jednodušší ke konfiguraci a snadněji a ladění samozřejmě hodně levnější než museli zaplatit vyhrazené OM.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure Explorer úložiště objektů BLOB aktualizace obrázky

Které jsme našli pomocí [Průzkumníka úložiště objektů BLOB Azure](https://azurestorageexplorer.codeplex.com/) (dostupné na webu codeplex) je velmi užitečné při správě aktualizace obrázek a videa na web. Ji používáme jako prostředek vývojář ručně aktualizovat obrázky a videa, které jsou součástí naše hlavní web. Najít, aby byl flexibilnější než nasazení změn na portál jsme eliminuje Úplný zkušební iterace pokaždé, když je potřeba aktualizovat obrázek. 

##<a name="a-few-final-words"></a>Konečný krátké

Díky skvělé osoby v WhatToPedia pro abychom mohli sdílet své postupu!  

Ať jsme, že jste našli toto Případová studie užitečné. Pokud přejdete na pomocí pole Hledat Azure, můžu doporučujeme několik zdrojů k dosažení můžete podél:

- [Fórum MSDN snaží o hledání Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow má taky značku](http://stackoverflow.com/questions/tagged/azure-search)
- [Stránka si přečtěte následující dokumentaci na Azure.com](https://azure.microsoft.com/documentation/services/search/)
- [Azure hledání si přečtěte následující dokumentaci na webu MSDN](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>Dodatek: WebJob indexování hledání

Následující kód vytvoří indexování uvedené v části o vytváření prototypů.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 
