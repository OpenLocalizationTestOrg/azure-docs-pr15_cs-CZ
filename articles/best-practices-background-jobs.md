<properties
   pageTitle="Pokyny pro úlohy na pozadí | Microsoft Azure"
   description="Pokyny pro pozadí úkolů, které spuštění nezávisle na sobě uživatelského rozhraní."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="masashin"/>

# <a name="background-jobs-guidance"></a>Pokyny pro úlohy pozadí

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Základní informace

Mnoho typů aplikací vyžadují úlohy na pozadí spuštěné nezávisle uživatelského rozhraní. Jako příklad lze uvést dávkové úlohy, náročné zpracování úkolů a časově náročné procesů například pracovní postupy. Úloh na pozadí můžete spouštět bez nutnosti interakci uživatele – aplikaci můžete spustit projektu a potom pokračovat ve zpracování interaktivní požadavky od uživatelů. To může pomoci minimalizovat zatížení aplikace uživatelské rozhraní, které můžete zlepšit dostupnost a zmenšení interaktivní odezvy.

Například pokud aplikace se vyžaduje generovat miniatury obrázků, které jsou odeslány uživateli, ho akce jako úloha na pozadí a uložte miniaturu k základnímu úložišti po dokončení – bez uživatele museli počkejte procesu k dokončení. Stejným způsobem můžete zahájit uživatele objednání pozadí pracovního postupu, který procesy směru, zatímco uživatelské rozhraní umožňuje uživateli a pokračujte v prohlížení web appu. Po dokončení úlohy na pozadí můžete aktualizovat data uložená objednávky a odeslat e-mailu pro uživatele, který potvrdí pořadí.

Kdy byste uvážit, zda k provedení úkolu jako úloha na pozadí, hlavní kritéria je zda úkol mohlo by umožnit spuštění bez interakce s uživatelem a v uživatelském rozhraní nutností až práce k dokončení. Úkoly, které vyžadují uživatele nebo uživatelské rozhraní čekat, než jsou dokončeny nemusí být vhodné jako pozadí úlohy.

## <a name="types-of-background-jobs"></a>Typy úloh pozadí

Pozadí úlohy obvykle obsahují jednu nebo více z následujících typů úloh:

- Procesor vyžadující značnou úlohy, třeba výpočty nebo modelu struktury analýzy.
- Můžu/O-značnou úlohy, například provádění řadu úložiště transakce nebo indexování souborů.
- Dávkové úlohy, například aktualizace noční dat nebo plánované zpracování.
- Dlouhé spuštěné pracovní postupy, například vyřizování objednávek a zřízení služby a systémy.
- Pokud úkol je písemný bezpečnější místo pro zpracování zpracování citlivá data. Například nemusí chcete zpracovat citlivá data v rámci do webových aplikací. Místo toho můžete použít vzorek například [Gatekeeper](http://msdn.microsoft.com/library/dn589793.aspx) přenášet data izolace pozadí proces, který má přístup k chráněné úložiště.

## <a name="triggers"></a>Aktivace

Pozadí úlohy se dá spouštět několika různými způsoby. Spadají do jedné z těchto kategorií:

- [**Řízeného událostmi aktivačních událostí**](#event-driven-triggers). Úkol je spuštěn odpověď na událost, obvykle akci, kterou může uživatel nebo kroku v pracovním postupu.
- [**Plánování řízené úsilím aktivačních událostí**](#schedule-driven-triggers). Úkol je vyvolat při plánování podle časovače. Může to být plánu opakované nebo jednorázové vyvolání zadané pro později.

### <a name="event-driven-triggers"></a>Řízeného událostmi aktivačních událostí

Řízeného událostmi vyvolání používá aktivační ke spuštění úlohy pozadí. Příklady použití řízeného událostmi aktivačních událostí:

- Uživatelské rozhraní nebo jiné úlohy umístí zprávy ve frontě. Zpráva obsahuje údaje o akci, která proběhla, například uživatelem objednání. Úkol pozadí sleduje této fronty a zjistí příchod nové zprávy. Přečte zprávu a používá data v něm jako vstupů pro úlohy na pozadí.
- Uživatelské rozhraní nebo jiné úlohy uloží nebo aktualizuje hodnoty v úložišti. Úkol pozadí sleduje úložiště a zjistí, změny. Načte data a používá jako vstupů pro úlohy na pozadí.
- Uživatelské rozhraní nebo jiné úlohy udělá žádost koncový bod, například HTTPS URI nebo rozhraní API, které se zobrazí jako webové služby. Předá data, která je potřebná k dokončení úkolu na pozadí jako součást žádost. Koncový bod nebo webové službě vyvolá pozadí úkol, který používá data jako vstup.

Typické úkoly, které odpovídají řízeného událostmi vyvolání Příklady zpracování pracovní postupy, odesílání informací o vzdálené odesílání e-mailové zprávy a vytváření nových uživatelů v víceklientské aplikací obrázku.

### <a name="schedule-driven-triggers"></a>Plánování řízené úsilím aktivačních událostí

Plánování řízené úsilím vyvolání používá časovač ke spuštění úlohy pozadí. Příklady použití plánování řízené úsilím aktivačních událostí:

- Časovače, na kterém běží místně aplikace nebo jako součást aplikace operační systém vyvolá úlohy na pozadí v pravidelných intervalech.
- Časovače, který běží v jiné aplikaci nebo časovače služby, třeba Azure Plánovač odešle žádost o rozhraní API aplikace nebo webové služby v pravidelných intervalech. Rozhraní API aplikace nebo webové službě vyvolá úkolu pozadí.
- Časovače, který způsobuje úkol pozadí uplatnit ihned po zadanou dobu odložení nebo v daném čase spuštění aplikace nebo samostatný proces.

Typické úkoly, které jsou vhodné pro plánování řízené úsilím vyvolání příklady dávkové zpracování rutin (například aktualizace seznamy souvisejících produktů pro uživatele na základě jejich poslední chování), úkoly pravidelnou zpracování dat (například aktualizace indexy nebo generování souhrnné výsledky), analýza dat pro denní sestav, vyčištění uchovávání dat a kontroly konzistence data.

Pokud používáte plán řízené úsilím úkol, který musíte spustit jako jedna instance, mějte na paměti toto:

- -Li měřítko výpočetním instance, na kterém běží Plánovač (například virtuálního počítače pomocí Windows naplánované úkoly), bude mít víc instancí Plánovač spuštěný. Tyto by mohl začít několika instancích úkolu.
- Úkoly spustíte po dobu delší než období mezi Plánovač události Plánovač začnou další instanci úkolu během spuštění na předchozí snímek.

## <a name="returning-results"></a>Nevrací žádné výsledky

Pozadí úlohy provedení jako samostatný proces a dokonce i do samostatných umístění v uživatelském rozhraní nebo obrázku, který spouštěn úkolu pozadí. V ideálním případě úlohy na pozadí je "spustit a v žádném případě nezapomeňte" operace a daří plnit spuštění nemá žádný vliv na uživatelské rozhraní nebo proces, volající. To znamená, že, volající proces nečeká dokončení úkolů. Proto nelze automaticky rozpoznat, když skončí platnost úkol.

Pokud požadujete úlohy na pozadí komunikovat s volající úkolu k označení průběhu nebo dokončení, je nutné pro tuto implementovat mechanismus. Příklady:

- Napište hodnotu indikátor stavu k základnímu úložišti přístupný uživatelské rozhraní nebo volající úkol, který můžete sledovat nebo zaškrtnout políčka tuto hodnotu v případě potřeby. Jiná data, která úkol pozadí musí vrátit volajícímu mohou být umístěny do stejné úložiště.
- Vytvoření fronta odpovědí, který uživatelské rozhraní nebo volající sleduje. Úlohy pozadí můžete odesílat zprávy do fronty, které označují stav a dokončení. Data, která úkol pozadí musí vrátit volajícímu mohou být umístěny do zprávy. Pokud používáte Bus služby Azure, můžete provádět tuto možnost Vlastnosti **ReplyTo** a **CorrelationId** . Další informace najdete v tématu [srovnávací ve služby Bus Brokered zpráv](http://www.cloudcasts.net/devguide/Default.aspx?id=13029).
- Zveřejnění rozhraní API nebo koncový bod z pozadí úkol, který uživatelské rozhraní nebo volající mají přístup k získání informací o stavu. Data, která úkol pozadí musí vrátit volající může být součástí odpovědi.
- Máte zavoláte zpět na uživatelské rozhraní nebo volající prostřednictvím rozhraní API indikaci stavu předdefinované body minimálně po dokončení úkolu pozadí. Může to být prostřednictvím události aktivované místně nebo mechanismus publikovat a odběr. Data, která úkol pozadí musí vrátit volající může být součástí žádosti o nebo události datové.

## <a name="hosting-environment"></a>Hostitelské prostředí

Úlohy na pozadí můžete hostovat pomocí oblasti služby Azure různé platformy:

- [**Azure Web Apps a WebJobs**](#azure-web-apps-and-webjobs). Můžete WebJobs provést vlastní úlohy podle oblasti různých typů skripty a programy v rámci do webových aplikací.
- [**Role pro web a pracovní azure Cloud Services**](#azure-cloud-services-web-and-worker-roles). Můžete napsat kód v roli, který se spustí jako úlohy na pozadí.
- [**Azure virtuálních počítačích**](#azure-virtual-machines). Pokud máte službu systému Windows nebo chcete použít Plánovač úloh systému Windows, je běžné k hostování vašeho úlohy na pozadí v rámci vyhrazené virtuálního počítače.
- [**Azure dávku**](./batch/batch-technical-overview.md). Je služba platformy, která plánuje náročné práci dělat spravované sady virtuálních počítačích a můžete automaticky měřítko výpočet zdroje podle potřeb své úlohy.

Následující části popisují těhle možností podrobněji a zahrnout aspektech snadněji zvolíte na požadovanou možnost.

## <a name="azure-web-apps-and-webjobs"></a>Azure Web Apps a WebJobs

Azure WebJobs můžete provést vlastní úlohy jako úlohy na pozadí v rámci webovou aplikaci Azure. WebJobs spustili v rámci svojí webové aplikace nepřetržitý proces. WebJobs také spustit odpověď na aktivační událost z Azure Plánovač nebo externí faktorů, jako jsou změny úložiště objektů BLOB a fronty zpráv. Úlohy můžete začít a přerušili na vyžádání a neukončí. Když se trvale pracovního WebJob nepovede, se automaticky nerestartuje. Opakování a chyby akce jsou, která dokáže nahradit.

Když nakonfigurujete WebJob:

- Pokud chcete úloha odpovědět na aktivační události pro řízeného událostmi, byste měli označit jako **Souvislá**. Skript nebo program uložený ve složce s názvem webu/kořenových/app_data/úlohy/nepřetržitý.
- Pokud chcete úloha odpovědět aktivační plánování řízené úsilím, byste měli označit jako **Spustit podle plánu**. Skript nebo program uložený ve složce s názvem webu/kořenových/app_data/úlohy/aktivovaná.
- Pokud zvolíte možnost **Spustit na vyžádání** při konfiguraci úlohy, se spustí kódu stejné jako možnost **Spustit při plánování** , když ho začnete.

Azure WebJobs spustit v rámci izolovaného prostoru ve web appu. To znamená, že můžete zpřístupnit proměnné a sdílení informací, jako jsou řetězce připojení pomocí web appu. Projekt má přístup k jedinečný identifikátor v počítači, na kterém běží projektu. Připojovací řetězec s názvem **AzureWebJobsStorage** poskytuje přístup k fronty Azure úložiště objektů BLOB a tabulek dat aplikace a přístup k Bus služeb pro zasílání zpráv a komunikace. Připojovací řetězec s názvem **AzureWebJobsDashboard** poskytuje přístup k souboru protokolu akce úlohy.

Azure WebJobs splňovat tyto požadavky:

- **Zabezpečení**: WebJobs jsou chráněny pověření nasazení ve web appu.
- **Podporované typy souborů**: můžete definovat WebJobs pomocí příkazů skriptů (CMD), dávku soubory (BAT), skriptů Powershellu (.ps1), flám skriptů prostředí (.sh) PHP skripty (PHP), skripty Python (.py), kód JavaScript (JS) a programy (.exe .jar a další).
- **Nasazení**: nasazením skripty a spustitelné soubory pomocí portálu Azure pomocí [WebJobsVs](https://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) doplňku for Visual Studio nebo [Visual Studio 2013 aktualizace 4](http://www.visualstudio.com/news/vs2013-update4-rc-vs) (můžete vytvořit a nasazení vyberete tuto možnost), pomocí [Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)nebo kopírování přímo do těchto umístění:
  - Pro spuštěná spouštění: webu/kořenových/app_data/úlohy/spouštěný / {úloha název}
  - Nepřetržitý spuštění: webu/kořenových/app_data/úlohy/nepřetržitý / {úloha název}
- **Protokolování**: Console.Out je považováno (označené) informace. Console.Error zpracován jako chyba. Informace o sledování a Diagnostika dostanete pomocí portálu Azure. Stahování souborů protokolu přímo z webu. Uložení do těchto umístění:
  - Pro spuštěná spouštění: Vfs/data/úlohy/spouštěný/název_úlohy
  - Nepřetržitý spuštění: Vfs/data/úlohy/nepřetržitý/název_úlohy
- **Konfigurace**: WebJobs můžete nakonfigurovat pomocí portálu, rozhraní REST API a Powershellu. Konfigurační soubor s názvem settings.job v kořenovém adresáři stejný jako skript úlohy umožňuje poskytovat informace o konfiguraci projektu. Příklad:
  - {"stopping_wait_time": 60}
  - {"is_singleton": PRAVDA}

### <a name="considerations"></a>Co byste měli zvážit

- Ve výchozím nastavení WebJobs měřítko přes web app. Však můžete nakonfigurovat úlohy na spustit jedna instance nastavením vlastnosti konfigurace **is_singleton** **logickou hodnotu PRAVDA**. Jedna instance WebJobs jsou užitečné pro úkoly, které nechcete měřítko nebo jednotlivé spustili souběžné více instance, například přeindexovávat analýza dat a podobné úkoly.
- Minimalizovat důsledek prováděných úloh na výkon web appu, zvažte vytvoření instance prázdné Azure v prohlížeči na nový plán služeb aplikace k hostiteli WebJobs, může se naučíte být spuštění nebo zdroje náročné.

### <a name="more-information"></a>Další informace

- [Azure WebJobs doporučené zdroje](./app-service-web/websites-webjobs-resources.md) obsahuje mnoho užitečné zdroje, stahování a vzorků pro WebJobs.

## <a name="azure-cloud-services-web-and-worker-roles"></a>Azure Cloudovým službám webu a pracovní role

Můžete provést úlohy na pozadí v rámci roli webu nebo v samostatných pracovních roli. Když se rozhodnutí, zda použít roli kolegy, zvažte škálovatelnost a pružnost požadavky, životnost úkolu, uvolněte cadence zabezpečení, odolnost proti chybám, konflikty, složitost a logické architektury. Další informace najdete v článku [Výpočet vzorek sloučení zdroje](http://msdn.microsoft.com/library/dn589778.aspx).

Provádět úlohy na pozadí v roli Cloudovým službám několika způsoby:

- Vytvoření implementaci třídy **RoleEntryPoint** v roli a použijte jeho metody provádět úlohy na pozadí. Úkoly v kontextu WaIISHost.exe spustit. Metoda **GetSetting** třídy **CloudConfigurationManager** použitím načíst nastavení. Další informace najdete v tématu [životního cyklu (cloudové služby)](#lifecycle-cloud-services).
- Pomocí úkolů při spuštění provádět úlohy na pozadí při spuštění aplikace. Chcete-li vynutit úloh, kterými dále běží na pozadí, nastavte vlastnost **taskType** na **pozadí** (Pokud není to uděláte, procesu při spuštění aplikace budou zastavit a počkejte na dokončení úlohy). Další informace najdete v tématu [spuštění úlohy v Azure](./cloud-services/cloud-services-startup-tasks.md).
- Umožňuje provádět úlohy na pozadí jako je WebJobs, která je spuštěná jako úkol při spuštění WebJobs SDK. Další informace najdete v tématu [Začínáme s Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md).
- Pomocí úkolů při spuštění instalace služby Windows, který se spustí jeden nebo více úlohy na pozadí. Tak, aby služba spustí na pozadí, nastavte vlastnost **taskType** na **pozadí** . Další informace najdete v tématu [spuštění úlohy v Azure](./cloud-services/cloud-services-startup-tasks.md).

### <a name="running-background-tasks-in-the-web-role"></a>Spuštění úlohy na pozadí v roli web

Hlavní výhodou spuštění úlohy na pozadí v roli web je ukládání v hostování náklady, protože je povinnost nasazení další role.

### <a name="running-background-tasks-in-a-worker-role"></a>Spuštění úlohy na pozadí v roli kolegy

Spuštění úlohy na pozadí v roli pracovníka má několik výhod:

- Umožňuje spravovat měřítka samostatně u jednotlivých typů role. Například budete potřebovat další instance webu role pro podporu aktuální zatížení, ale méně instancí pracovního role, který se spustí úlohy na pozadí. Roztažením instance výpočetním pozadí úkolů odděleně od role uživatelského rozhraní můžete snížit hostingu náklady a zachovat přijatelného výkonu.
- Offloads zpracovávaných úkolů pozadí z web role. Role webu poskytujícím rozhraní zůstat neodpovídá a může znamenat, že méně instance je potřebný kvůli podpoře daném svazku žádostí o před uživateli.
- Umožňuje provádět oddělení pochybnosti. Každý typ role implementovat určité skupiny jasně definované a související úkoly. Díky navrhování a udržování kód jednodušší, protože je menší vzájemné závislosti kód a funkce mezi každou roli.
- Pomáhají izolovat citlivé procesy a data. Například role web implementující uživatelské rozhraní nemusí mít přístup k datům, která je spravován a řídil pracovní roli. To může být užitečné v posílit zabezpečení, zvlášť když použít vzorek například [Gatekeeper vzorku](http://msdn.microsoft.com/library/dn589793.aspx).  

### <a name="considerations"></a>Co byste měli zvážit

Zvažte následující možnosti při výběru jak a kam nasazení úlohy na pozadí při použití Cloudovým službám webu a pracovníka role:

- Hostitelské úlohy na pozadí v existující web roli můžete uložit náklady spuštění roli samostatných pracovních určené pro tyto úkoly. Je však mohou ovlivnit výkon a dostupnosti aplikace, pokud existuje konflikty pro zpracování a další zdroje. Pomocí samostatných pracovních role zabraňuje dopad úlohy na pozadí – náročné nebo náročné roli web.
- Pokud hostujete úlohy na pozadí s použitím třídy **RoleEntryPoint** , můžete snadno přesunout to do další roli. Třeba když vytvoříte předmětu v roli web a později rozhodnete, že je potřeba spustit úkoly v roli pracovní, můžete přesunout implementaci třídy **RoleEntryPoint** do roli pracovní.
- Spuštění úkoly se mají spustit program nebo skript. Nasazení úloha na pozadí jako spustitelný soubor může být obtížnější, zejména pokud také vyžaduje nasazení závislých sestavení. Může to být snadněji nasazení a informace o definování úlohu pozadí použijete úkoly při spuštění pomocí skriptu.
- Výjimky, které způsobují úlohy na pozadí selhání mají odlišný vliv, podle toho, tak, že jsou hostovány:
  - Pokud používáte přístup třídy **RoleEntryPoint** , selhalo úkolu způsobí roli chcete znovu spustit, aby úkol automaticky restartuje. To může ovlivnit dostupnosti aplikace. Chcete-li předejít, zajistit, aby zahrnout robustní zpracování výjimek v rámci tříd **RoleEntryPoint** a všechny úlohy na pozadí. K opětovnému spuštění úkoly, které se nezdaří, kdy je vhodné použít kód a výjimku restartujte roli jenom v případě, že se nedá obnovit řádně selhání v rámci vašeho kódu.
  - Pokud používáte spuštění úkoly, jste zodpovědní řízení provádění úkolů a kontrola Pokud dojde k chybě.
- Správa a sledování úkolů po spuštění je složitější než používat **RoleEntryPoint** třídy přístup. Však Azure WebJobs SDK obsahuje řídicího panelu snadněji spravovat WebJobs spouštíte projít úvodní úkoly.

### <a name="more-information"></a>Další informace

- [Výpočet vzorek sloučení zdroje](http://msdn.microsoft.com/library/dn589778.aspx)
- [Začínáme s Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)

## <a name="azure-virtual-machines"></a>Azure virtuálních počítačích

Úlohy na pozadí může být implementovaná způsobem, který brání používaný Azure webových aplikací nebo Cloudovým službám nebo tyto možnosti nemusí být užitečné. Typické příklady služby systému Windows a nástroje třetích stran a programy. Jiný příklad může být programy napsané pro spuštění prostředí, které se liší od, který je hostitelem aplikace. Například může být Unix nebo Linux program, který chcete spustit z aplikace pro Windows nebo .NET. Vyberte oblast operační systémy pro Azure virtuální počítač a spusťte službu nebo spustitelný soubor ve počítače virtuální.

Kdy použít virtuálních počítačích zvolit, přečtěte si téma [aplikace služby Azure, cloudovými službami a porovnání virtuálních počítačích](./app-service-web/choose-web-site-cloud-service-vm.md). Informace o možnostech virtuálních počítačích najdete v tématu [virtuálního počítače a cloudové služby velikost Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Další informace o operačních systémů a přednastavených obrázky, které jsou k dispozici pro virtuálních počítačích najdete v tématu [Azure Marketplace virtuálních počítačích](https://azure.microsoft.com/gallery/virtual-machines/).

Zahajte pozadí úkolu v samostatném virtuálního počítače, máte řadu možností:

- Spuštěním úkolu na vyžádání přímo z aplikace tak, že žádost o koncový bod, který poskytuje úkol. To předává v všechna data, která vyžaduje úkol. Tento koncový bod vyvolá úkol.
- Můžete nakonfigurovat spuštění při plánování úlohy pomocí Plánovač nebo časovače, který je k dispozici ve zvoleném operační systém. Například v systému Windows můžete Plánovač úloh systému Windows spustit skripty a úkoly. Nebo pokud máte nainstalované v počítači virtuálního serveru SQL Server, můžete SQL Server Agent spustit skripty a úkoly.
- Plánovač Azure slouží k zahájení úkolu přidáním zprávu do fronty, který daný úkol sleduje nebo tak, že žádost o rozhraní API, které poskytuje úkol.

Části starší [aktivační události](#triggers) pro další informace o tom, jak můžete zahájit úlohy na pozadí.  

### <a name="considerations"></a>Co byste měli zvážit

Při rozhodování, jestli chcete nasadit úlohy na pozadí v Azure virtuálního počítače, zvažte následující možnosti:

- Hostitelské úlohy na pozadí v samostatném Azure virtuálního počítače flexibilitu a umožní přesné publikum nemůže ovládat zahájení, spuštění, plánování a přidělení zdrojů. Však zvětšíte runtime náklady, pokud virtuální počítač musí být nasazené jenom ke spuštění úlohy na pozadí.
- Neexistuje žádná zařízení pro sledování úkolů v portálu Azure a neumožňuje automatizovaných restart selhalo úkoly – sice můžete sledovat stav základní virtuálního počítače a spravovat pomocí [Rutiny pro správu služby Azure](http://msdn.microsoft.com/library/azure/dn495240.aspx). Můžou ale nastat žádné zařízení určit procesy a vláknech ve výpočetním uzlů. Používání virtuálního počítače obvykle budou vyžadovat další úsilí o implementaci mechanismus, který shromáždí data od přístrojového vybavení v úkolu a od operačního systému do virtuálního počítače. Jeden řešení, které může být vhodné je použít [System Center Management Pack pro Azure](http://technet.microsoft.com/library/gg276383.aspx).
- Zvažte vytvoření sledování sond, které jsou k dispozici prostřednictvím protokolu HTTP koncové body. Kód pro tyto sond může provádět kontroly stavu, shromáždit funkční informace a statistiky – nebo zkompletovat informace o chybách a návrat do aplikace pro správu. Další informace najdete v tématu [Vzorek sledování stavu koncového bodu](http://msdn.microsoft.com/library/dn589789.aspx).

### <a name="more-information"></a>Další informace

- [Virtuálních počítačích](https://azure.microsoft.com/services/virtual-machines/) na Azure
- [Nejčastější dotazy týkající se Azure virtuálních počítačích](virtual-machines/virtual-machines-linux-classic-faq.md)

## <a name="design-considerations"></a>Důležité informace o návrhu

Existuje několik základních faktorů, které je třeba zvážit při navrhování úlohy na pozadí. Následující části se zabývají rozdělení konflikty a koordinaci.

## <a name="partitioning"></a>Rozdělení

Pokud se rozhodnete zahrnout úlohy na pozadí v rámci instance stávající výpočetní (například web appu, web role, existující pracovníka nebo virtuální počítač), je nutné zvážit, jak bude to mít vliv atributy kvality stupně výpočetní a samotný úkol pozadí. Následujících skutečností vám pomůže se rozhodnout, jestli umístěním (kolokací) úkoly s existující instance výpočetním nebo oddělte je do samostatných výpočetním instance:

- **Dostupnost**: úlohy na pozadí nemusí potřebujete, měli stejnou úroveň dostupnosti jako jiné části aplikace, zejména v uživatelském rozhraní a dalších částí přímo zapojené interakci uživatele. Úlohy na pozadí může být příjemnější latence chyby opakované připojení a další faktory, které ovlivňují dostupnost, protože operací můžete ve frontě. Avšak musí mít dostatečnou kapacitu k zabránění zálohování požadavky, které by mohl blokovat fronty a ovlivnit aplikaci jako celek.
- **Škálovatelnost**: úlohy na pozadí se pravděpodobně požadavek na různých škálovatelnost než uživatelského rozhraní a interaktivní část aplikace. Měřítko uživatelského rozhraní může být nutné zahájit píků v služba, zatímco úlohy na pozadí zbývající může udělat méně zaneprázdněné době nižší čísla instancí výpočetním.
- **Odolnost proti chybám**: Chyba instance výpočetní jenom hostuje úlohy na pozadí nemusí vliv na smrtelně aplikace jako celek v případě žádosti o těchto úkolů můžete ve frontě nebo odložené, dokud úkol je k dispozici znovu. Pokud výpočetním instance a/nebo úkoly můžete restartovat odpovídající intervalu, nemusí být vliv na uživatele aplikace.
- **Zabezpečení**: úlohy na pozadí může být různé zabezpečení nebo omezení než uživatelské rozhraní nebo jiná část aplikace. Pomocí samostatných výpočetním instanci můžete určit jiné zabezpečení prostředí pro úkoly. Můžete taky vzorků například Gatekeeper vyčlenění instance výpočetním pozadí v uživatelském rozhraní maximalizovat zabezpečení a oddělení.
- **Výkon**: pro úkoly pozadí na konkrétně odpovídající požadavky na výkon úkoly můžete typu výpočetním instance. Může to znamenat pomocí levnější výpočetním možnost, pokud úkoly není nutné zadávat stejné možnosti zpracování jako uživatelské rozhraní nebo větší instance pokud potřebují další funkce a prostředky.
- **Možnosti správy**: úlohy na pozadí může být různé tempo ochranu vývoje a nasazení, jakým z kód hlavní aplikace nebo uživatelského rozhraní. Nasazení samostatné výpočetním instanci můžete zjednodušit aktualizace a správa verzí.
- **Pole náklady**: Přidání výpočet instance provést zvýšení úkoly pozadí hostingu náklady. Zvažte poměr další funkce a tyto další náklady.

Další informace najdete v tématu [Vzorek zvolení vodicí znak](http://msdn.microsoft.com/library/dn568104.aspx) a [Soutěží spotřebitele vzorku](http://msdn.microsoft.com/library/dn568101.aspx).

## <a name="conflicts"></a>Konflikty

Pokud ještě několika instancích úlohy na pozadí je možné, že bude soutěží o přístupu k prostředkům a služby, jako je databáze a úložiště. Přístup k této souběžné může vést přetížené zdroje, které můžou způsobovat konflikty v stavu služeb a integrity dat v úložišti. Vyřešit konflikty prostředků pomocí pesimistické uzamčení přístup. Tím se zabrání soutěží výskyty úkolu ze souběžně přístup ke službě nebo poškození dat.

Další způsob, jak vyřešit konflikty má definovat úlohy na pozadí jako jednoznačné, takže nebude zadaná někdy jedinou instance spuštěna. Díky tomu však spolehlivosti a výkonu výhod, které můžete přidat více instancemi konfigurace. To platí zejména pokud uživatelské rozhraní můžete zadat dostatečné práce zachovat zaneprázdněné víc než jeden úkol pozadí.

Je důležité zajistit, že můžete automaticky restartovat úkolu pozadí a že nejsou dostatečnou kapacitu vyrovnat s vrcholy v služba. Můžete dosáhnout tak, že přidělování instanci výpočetním s dostatečné zdroje implementací mechanismus přidávání do fronty, která mohou být uloženy požadavky na pozdější provedení při služba snižuje nebo pomocí kombinace z těchto postupů.

## <a name="coordination"></a>Koordinaci

Úlohy na pozadí může být složité a může vyžadovat víc jednotlivé úlohy provádět vytvářejí výsledek nebo splňují všechny. Je běžné v podobnému sledu rozdělit úkol na menší skrytého kroky nebo dílčí úkoly, které můžete provádět několik spotřebitele. Zahrnující několik kroků úlohy může být efektivnější a flexibilnější, protože jednotlivé kroky můžou být opakovaně použitelný ve více úloh. Je taky snadno přidat, odebrat nebo změnit pořadí kroků.

Může být náročné koordinaci několika úkoly a kroky, ale existují tři běžné vzorce, které můžete použít k Průvodce vaší implementací řešení:

- **Decomposing úkolu do více opakovaně použitelný kroků**. Aplikace může být nutné provádět různé úkoly různých složitosti informace, které zpracuje. K provedení této zpracování jako modul monolitický může být jednoduchý, ale nepružná přístup k provedení této aplikace. Tento přístup je však pravděpodobně zmenšit příležitostí pro refaktoring kód ho zkusit optimalizovat a opakované použití pokud díly stejné zpracování jsou potřeba jinde v aplikaci. Další informace najdete v tématu [kanály a filtry vzorku](http://msdn.microsoft.com/library/dn568100.aspx).
- **Správa provádění jednoduchých kroků pro daný úkol**. Aplikace může provádět úkoly, které obsahují několik kroků (některé z nich může vyvolat vzdálené nebo přístupu k prostředkům vzdálený). Jednotlivé kroky můžou být nezávisle na sobě, ale jsou orchestrated logikou aplikace implementující úkol. Další informace najdete v tématu [Plánovač Agent správce vzorku](http://msdn.microsoft.com/library/dn589780.aspx).
- **Správa obnovení postup úkolu, které nezdaří**. Aplikace možná budete muset zpět práce, která provádí řadu kroků, (které společně definují operaci postupně konzistentní) Pokud jeden nebo více kroků nezdaří. Další informace najdete v tématu [Kompenzace transakce vzorku](http://msdn.microsoft.com/library/dn589804.aspx).

## <a name="lifecycle-cloud-services"></a>Cyklus (Cloud Services)

 Pokud se rozhodnete provádět úlohy pozadí pro Cloudovým službám aplikace, které využívají web a pracovní role pomocí třídy **RoleEntryPoint** , je důležité pochopit životním cyklu tohoto třídy mohli použít správně.

Role pro web a pracovní absolvovat sadu různých fází při spuštění, spustit a zastavit. Třída **RoleEntryPoint** zpřístupňuje řadu událostí, které označují při tyto fáze dochází. Pomocí těchto inicializace, spustit a zastavit úkolů vlastní pozadí. Dokončení obrázku je:

- Azure načte sestavení rolí a hledá předmětu, která je odvozena z **RoleEntryPoint**.
- Pokud najde této třídy, volá **RoleEntryPoint.OnStart()**. Přepsat tuto metodu inicializace úlohy na pozadí.
- Po dokončení metodu **při spuštění** Azure hovory, které je **Application_Start()** v globální souboru aplikace Pokud prezentace (například Global.asax v roli web systém ASP.NET).
- Azure hovorů **RoleEntryPoint.Run()** na nový podproces popředí, který se spustí souběžně s **OnStart()**. Přepsat tuto metodu spuštění úlohy na pozadí.
- Když skončí platnost metodu spustit, Azure nejdřív hovorů **Application_End()** v globální souboru aplikace, pokud to je k dispozici a potom volá **RoleEntryPoint.OnStop()**. Přepsat metodu **OnStop** ukončíte úlohy na pozadí, vyčistit zdrojů, vyřadit objektů a zavřete připojení, která jste použili úkoly.
- Azure pracovního role hostitele procesu je zastaveno. V tomto okamžiku roli bude z koše a restartuje.

Další informace a příklady pomocí metody třídy **RoleEntryPoint** najdete v tématu [Výpočet vzorek sloučení zdroje](http://msdn.microsoft.com/library/dn589778.aspx).

## <a name="considerations"></a>Co byste měli zvážit

Při plánování jak poběží úlohy na pozadí v roli webu nebo pracovního vzít v úvahu následující skutečnosti:

- Výchozí **Spustit** implementace metody ve třídě **RoleEntryPoint** obsahuje volání **Thread.Sleep(Timeout.Infinite)** , po kterou uchovává roli donekonečna udržovat aktivní. Pokud přepíšete metody **Run** (což je obvykle potřebné ke spuštění úlohy na pozadí), povolíte nesmí kódu ukončíte metodu jestli vám nestačí odpadkového instanci rolí.
- Typická implementace metody **Run** obsahuje kódu pro začátek všechny úlohy na pozadí a konstrukci smyčka, která pravidelně kontroluje stavu úlohy na pozadí. Můžete restartovat kterým nepovede nebo sledovat tokenů zrušení označující, že jste dokončili úlohy.
- Pokud úlohy na pozadí vyvolá neošetřené výjimce, třeba tento úkol z koše zároveň jakékoli jiné úlohy na pozadí v roli pokračovat v práci. Však pokud výjimce tak, že poškození objektů mimo k úkolu, jako je třeba sdílené úložiště výjimku by měl uskutečněných jednotlivými svojí třídě **RoleEntryPoint** , třeba zrušit všechny úkoly a způsob **spuštění** se má povolit ukončit. Azure potom restartujte roli.
- Použijte metodu **OnStop** pozastavit nebo odstranění úlohy na pozadí a vyčištění prostředků. To může zahrnovat zastavení dlouho probíhajících nebo zahrnující několik kroků úkoly. Je důležité zvážit, jak lze to chcete-li předejít nekonzistence data. Pokud instanci rolí přestane z nějakého důvodu než vypnutí inicializovaný uživatelem, musí být dokončen kód spuštěný v metodu **OnStop** v rámci pět minut, než vynutit ukončen. Zajistit, aby kódu můžete udělat v té době nebo nevadí vám neběží k dokončení.  
- Vyrovnávání zatížení Azure spustí odkazující umožnění datových přenosů do instanci rolí metodu **RoleEntryPoint.OnStart** vrátí na hodnotu **true**. Proto zvažte vložení všechny inicializační kódy metodu **při spuštění** tak, aby instance role, které nejsou inicializace úspěšně nebudete dostávat žádné přenosy.
- Můžete použít při spuštění úkoly kromě metody třídy **RoleEntryPoint** . Inicializace nastavení, které je potřeba změnit v Vyrovnávání zatížení Azure, protože tyto úkoly provede před roli obdrží všechny žádosti o byste měli použít při spuštění úkoly. Další informace najdete v tématu [spuštění úlohy v Azure](./cloud-services/cloud-services-startup-tasks.md).
- Pokud dojde k chybě v úkolu spuštění, může vynutit roli neustále restartovat. To může bránit provádění virtuální vyměňovat adresy IP (VIP) zpátky na dříve připravené verze, protože vyměňovat vyžaduje výhradní přístup k roli. To nelze získat během restartováním roli. Chcete-li vyřešit takto:
    -  Přidejte následující kód začátek metody **při spuštění** a **Spustit** v vaše role:

    ```C#
    var freeze = CloudConfigurationManager.GetSetting("Freeze");
    if (freeze != null)
    {
        if (Boolean.Parse(freeze))
        {
            Thread.Sleep(System.Threading.Timeout.Infinite);
        }
    }
    ```

   - Dodejte definici nastavení **ukotvení** jako logická hodnota ServiceDefinition.csdef a ServiceConfiguration. *.cscfg soubory rolí a nastavte ji na* *Nepravda* *. Pokud roli přejde do režimu opakované restartování počítače, můžete změnit nastavení tak, aby * *PRAVDA** ukotvit spuštění rolí a mohla být vyměnit pomocí předchozí verze.

## <a name="resiliency-considerations"></a>Co byste měli zvážit odolnost proti chybám

Úlohy na pozadí musí být pružné za účelem zajištění spolehlivých služeb pro aplikace. Při plánování a návrh úlohy na pozadí, zvažte následující skutečnosti:

- Úlohy na pozadí musíte mít řádně zpracovat role nebo služby restartování bez poškozením dat nebo představení nekonzistence do aplikace. Pro úkoly – náročné nebo zahrnující několik kroků zvažte použití _Zkontrolujte ukazatel ve tvaru_ uložením stavu úloh trvalé úložiště, nebo jako zpráv ve frontě pokud je to vhodné. Můžete třeba přetrvávají informace o stavu ve zprávě ve frontě a postupně aktualizace informací o stavu s průběh úkolu tak, že úkol můžete zpracovány z poslední známé dobré kontroly – místo restartováním od začátku. Pokud chcete použít fronty Bus služby Azure, můžete stejný scénář povolení relace zpráv. Relace umožňují uložit a získat stav zpracování aplikace pomocí metody [SetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.setstate.aspx) a [GetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.getstate.aspx) . Další informace o navrhování spolehlivé zahrnující několik kroků procesy a pracovní postupy najdete v článku [Plánovač Agent správce vzorku](http://msdn.microsoft.com/library/dn589780.aspx).
- Při použití webu nebo pracovního role hostovat více úlohy na pozadí navrhování svého přepsání **Spustit** metodu ke sledování selhalo nebo zablokované úkolů a je znovu spustit. Pokud to není praktické a používáte roli kolegy, vynuťte roli kolegy restartujte ukončením z metody **Spustit** .
- Když použijete fronty komunikovat s úlohy na pozadí, frontách mohou sloužit jako zabezpečení pro ukládání dotazy, které jsou odeslány k úkolům při aplikace je v části vyšší než obvykle načíst. Díky úloh, kterými zorientovat s uživatelským rozhraním méně zaneprázdněné dobu, kdy je. Také znamená to, že recyklace roli neblokuje uživatelského rozhraní. Další informace najdete v tématu [fronty základě vzorku vyrovnání načíst](http://msdn.microsoft.com/library/dn589783.aspx). Pokud některé úkoly jsou důležitější než jiné implementace [Priorita fronty vzorek](http://msdn.microsoft.com/library/dn589794.aspx) zajistit, aby tyto úkoly spusťte před méně důležité z nich.
- Úlohy na pozadí, které jsou spuštěné ve nebo proces zprávy musí navrženy pro zpracování nekonzistence, například zprávy odeslané nefunguje, zprávy, které opakovaně dojít k chybě (označovaná také jako _poškozená zprávy_) a zprávy, které jsou doručeny víckrát. Zvažte následující skutečnosti:
  - Zprávy, které je nutné zpracovat v určitém pořadí, třeba můžou být, které se mění dat na základě existujících dat hodnoty (například přidání hodnoty do existující hodnotu), nemusí se ukládají na původní pořadí, ve kterém byly odeslány. Můžete taky se může uskutečněných jednotlivými jiné instance úlohy na pozadí v jiném pořadí kvůli odlišnými zatížení jednotlivé instance. Zprávy, které je nutné zpracovat v určitém pořadí by měl obsahovat pořadové číslo, klíč nebo některé indikátor jiného, úlohy na pozadí můžete ověřit, že jsou zpracovány ve správném pořadí. Pokud používáte Bus služby Azure, můžete zajistit směru doručení zprávy relace. Je ale obvykle efektivnější, pokud je to možné, navrhnout procesu tak, aby pořadí zpráva není důležité.
  - Úlohy na pozadí se obvykle náhled zpráv, které skryje dočasně od jiných spotřebitele zprávy. Odstraní zprávy po jejich úspěšně zpracování. Pokud úlohy na pozadí, se nezdaří zpracování zprávy, zprávy se znovu zobrazí ve frontě po vypršení časového limitu náhled. Budou zpracovány jinou instancí úkolu nebo při příštím cyklu zpracování této instance. Pokud se zpráva konzistentní způsobuje chybu služby spotřebitele, bude blokovat úlohy ve frontě a nakonec samotnou aplikaci dojde k naplnění fronty. Proto je nezbytné k vyhledání a odstranění poškozená zprávy ve frontě. Pokud používáte Bus služby Azure, zprávy, které dojít k chybě přesouvat ručně nebo automaticky do fronty přidružené nepoužívaných dopisů.
  - Fronty zaručuje na _alespoň jednou_ mechanismy doručování, ale může dodají stejná zpráva víckrát. Kromě toho když pozadí úkol nepovede po zpracování zprávy, ale před odstraněním ve frontě, zprávy budou k dispozici pro zpracování znovu. Úlohy na pozadí by měl být idempotent, což znamená, že zpracování stejná zpráva víckrát nezpůsobuje chyby nebo nekonzistence v datech aplikace. Některé operace se mají přirozený idempotent, například nastavení hodnotu uloženou na konkrétní novou hodnotu. Operace, například přidání hodnoty do existující uložené hodnotu bez kontroly uložená hodnota je stále stejně jako při byla původně odeslané zprávy však způsobí nekonzistence. Automaticky odebrat duplicitní zprávy je možné konfigurovat Azure fronty Bus služby.
  - Některé zpráv systémy, třeba fronty Azure úložiště a Bus služby Azure fronty podporují de-queue počet vlastnost, která označuje, kolikrát četl zprávy ve frontě. To může být užitečné při zpracování zprávy opakované a poškozená. Další informace najdete v tématu [Základy asynchronní zasílání zpráv](http://msdn.microsoft.com/library/dn589781.aspx) a [Idempotenci vzorce](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/).

## <a name="scaling-and-performance-considerations"></a>Změna velikosti a výkonu co byste měli zvážit

Úlohy na pozadí musí nabízejí dostatečné výkon zajistit nebudou blokovat aplikace, ani způsobit nekonzistence kvůli Zpožděné operace při zatížení systému. Obvykle lepší výkon roztažením instance výpočetním hostujících úlohy na pozadí. Při plánování a návrh úlohy na pozadí, zvažte následující body kolem výkon a škálovatelnost:

- Neobsahovaly Azure podporuje text (rozšiřování i měřítka po návratu do) na základě aktuální službu a načíst – nebo plánu předdefinovaného webových aplikacích pro web Cloudovým službám a pracovníka rolí a virtuálních počítačích hostitelem nasazení. Pomocí této funkce můžete zajistit, aby aplikace jako celek dostatečné možnosti výkonu při minimalizace runtime náklady.
- Pokud úlohy na pozadí různých výkonové parametry v další části aplikace typu Cloudovým službám (třeba uživatelské rozhraní nebo součásti například vrstvy přístupu k datům), hostingu úlohy na pozadí společně s rolí samostatných pracovních umožňuje uživatelského rozhraní a pozadí úkolu role zobrazit nezávisle na sobě ke správě načíst. Funkce významně neliší výkonu od sebe máte více úkolů pozadí zvažte dělení do samostatných pracovních rolí a stejné měřítko jednotlivých typů role nezávisle na sobě. Však, že tato možnost může zvětšit runtime náklady ve srovnání s zkombinuje všechny úkoly do méně role.
- Jednoduše měřítka role nemusí postačovat abyste předešli ztrátě výkonu zatížení. Můžete taky muset změnit velikost úložiště fronty a další zdroje pro zabránit v jednom místě celkové zpracování řetězce zabránit tomu, aby kritický bod. Zvažte také další omezení, například maximální výkon úložiště a další služby využívající aplikace a úlohy na pozadí.
- Úlohy na pozadí je určen pro změnu velikosti. Například se mají dynamicky zjistit počet úložiště fronty používá naslouchají relacím na nebo do příslušné fronty odesílat zprávy.
- Ve výchozím nastavení WebJobs měřítko s jejich přidruženou instanci Azure Web Apps. Ale pokud chcete WebJob účtem jenom jedna instance, můžete vytvořit Settings.job soubor, který obsahuje JSON data **{"is_singleton": PRAVDA}**. To přinutí Azure spustit pouze jednou instancí WebJob, i když je jich víc instancí přidružené web appu. Může to být užitečné technika naplánovaných úloh, které musíte spustit jako jenom jedna instance.

## <a name="related-patterns"></a>Související vzorky

- [Asynchronní zpráv základy](http://msdn.microsoft.com/library/dn589781.aspx)
- [Pokyny pro neobsahovaly text](http://msdn.microsoft.com/library/dn589774.aspx)
- [Kompenzace vzorek transakce](http://msdn.microsoft.com/library/dn589804.aspx)
- [Konkurenční spotřebitele vzorek](http://msdn.microsoft.com/library/dn568101.aspx)
- [Výpočet rozdělení pokyny](http://msdn.microsoft.com/library/dn589773.aspx)
- [Výpočet vzorek sloučení zdroje](http://msdn.microsoft.com/library/dn589778.aspx)
- [Gatekeeper vzorek](http://msdn.microsoft.com/library/dn589793.aspx)
- [Zvolení vzorek vodicí znak](http://msdn.microsoft.com/library/dn568104.aspx)
- [Kanály a filtry vzorku](http://msdn.microsoft.com/library/dn568100.aspx)
- [Priority (priorita) fronty vzorek](http://msdn.microsoft.com/library/dn589794.aspx)
- [Na základě fronty zatížení vyrovnání vzorek](http://msdn.microsoft.com/library/dn589783.aspx)
- [Plánovač Agent správce vzorek](http://msdn.microsoft.com/library/dn589780.aspx)

## <a name="more-information"></a>Další informace

- [Změna měřítka Azure aplikací s kolegy role](http://msdn.microsoft.com/library/hh534484.aspx#sec8)
- [Spuštění úlohy na pozadí](http://msdn.microsoft.com/library/ff803365.aspx)
- [Azure Role spuštění životního cyklu](http://blog.syntaxc4.net/post/2011/04/13/windows-azure-role-startup-life-cycle.aspx) (příspěvek na blog)
- [Azure Cloud Services Role cyklus](http://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Windows-Azure-Cloud-Services-Role-Lifecycle) (video)
- [Začínáme s Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- [Azure fronty a služby Bus fronty – porovnání a v porovnání s](./service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Jak povolit Diagnostika v cloudové službě](./cloud-services/cloud-services-dotnet-diagnostics.md)
