<properties 
    pageTitle="Operační systém funkce v aplikaci služby Azure" 
    description="Další informace o funkci OS k dispozici pro webové aplikace, mobilní aplikace back-end a rozhraní API aplikace na aplikaci služby Azure" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2016" 
    ms.author="cephalin"/>

# <a name="operating-system-functionality-on-azure-app-service"></a>Operační systém funkce v aplikaci služby Azure #

Tento článek popisuje běžné podle směrného plánu operační systém funkci, která je dostupná pro všechny aplikace spuštěné v [Aplikaci služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714). Tato funkce obsahuje soubor, sítě a přístup k registru a protokolování diagnostiky a události. 

<a id="tiers"></a>
## <a name="app-service-plan-tiers"></a>Aplikace služby plán úrovní

Aplikace služby spustí zákazníka aplikací v prostředí hostingu více klienta. Aplikací nasazena v úrovní **zdarma** a **sdílené** spustit v pracovních postupů sdílené virtuálních počítačích při spuštění aplikace nasazenou v **standardních** a úrovní **Premium** na virtuální počítače vyhrazené speciálně pro aplikace přidružené jediného zákazníka.

Protože aplikace služby podporuje bezproblémovou práci měřítka mezi různých úrovní, nastavení zabezpečení pro aplikaci služby aplikace se nezmění. Zajistíte tím, že aplikace není odlišným neočekávaně, selhání neočekávané způsoby, když plán služeb aplikací kombinace kláves vymění z jedné úrovně do jiného.

<a id="developmentframeworks"></a>
## <a name="development-frameworks"></a>Vývojové rámce

Ceny úrovní aplikaci služby řídit množství prostředků výpočetním (procesoru místa na disku, paměti a výstupním síť) k dispozici pro aplikace. Šířka rámce funkcemi k aplikacím však zůstává stejná bez ohledu na změny měřítka úrovní.

Služba aplikace podporuje spoustu rámce vývoj, včetně prvků ASP.NET, klasické ASP, node.js PHP a Python – všechny spouští jako rozšíření v rámci služby IIS. Abyste zjednodušit a normalizovat nastavení zabezpečení, aplikaci služby aplikace obvykle spustit různých rámce vývoj výchozího nastavení. Jedním ze způsobů konfiguraci aplikace mohlo být přizpůsobit rozhraní API plochu a funkce pro jednotlivé jednotlivé vývoj rámce. Aplikace služby místo toho, která bude obecnější přístup povolením funkčnosti operační systém bez ohledu na aplikaci pro vývoj framework běžné podle směrného plánu.

V následujících částech souhrn obecné druhy operační systém funkce k dispozici pro aplikaci služby aplikace.

<a id="FileAccess"></a>
##<a name="file-access"></a>Přístup k souborům

Existují různé jednotky aplikaci služby včetně místní jednotky a jednotky sítě.

<a id="LocalDrives"></a>
### <a name="local-drives"></a>Místní jednotky

V jeho základní aplikaci služby je služba spuštěná nad infrastruktury Azure PaaS (platformu jako služba). Místní jednotky, připojených "" do virtuálního počítače jako výsledek, jsou k dispozici pro jakoukoli pracovníka roli spuštěné v Azure stejné jednotka typy. Platí to i jednotky operačního systému (jednotku D:\), aplikace jednotku, která obsahuje Azure cspkg souborů používaný výhradně v aplikaci služby (a nedostupné pro zákazníky) a "uživatel" jednotka (jednotku C:\), jejichž velikost se liší v závislosti na velikosti OM.

<a id="NetworkDrives"></a>
### <a name="network-drives-aka-unc-shares"></a>Síťových jednotkách (označovaná taky jako UNC sdílí)

Jedinečný aspektů aplikaci služby, díky kterému nasazení aplikace a údržba jednoduchých reprodukujte v množině podílů UNC uložený obsah pro všechny uživatele. Tento model mapuje velmi dobře běžné vzorek ukládání obsahu používané v místním prostředí, které mají víc Vyrovnávání zatížení servery pro hostování webů. 

V aplikaci služby existuje počet sdílených položek UNC vytvořené v jednotlivých datacentra. Přidělit procenta obsahu uživatele pro všechny zákazníky v jednotlivých datacentra pro každou UNC sdílet. Kromě toho celému obsahu pro jeden zákaznického předplatného vždy umístěny na stejné UNC soubor sdílet. 

Všimněte si, že kvůli jak cloud services práce, zodpovědný za hostitelem sdílené UNC konkrétní virtuální počítač bude v průběhu času mění. Je zaručit, že sdílené složky UNC připojen podle různých virtuálních počítačích, jako při běžném operací cloudu načtením nahoru a dolů. Z tohoto důvodu aplikace připomínat nikdy pevně předpoklady, aby informace o počítače v souboru cesty UNC zůstane stabilní určitou dobu. Místo toho měli použít pohodlný *umělé* absolutní cesta **D:\home\site** poskytující aplikaci služby. Cesta k umělé absolutní poskytuje portable, aplikace a uživatele – bez ohledu na způsob odkazující na aplikace vlastního. Pomocí **D:\home\site**jednu přenášet sdílené soubory aplikací z aplikace bez nutnosti nakonfigurovat nové absolutní cesty pro každý přenos.

<a id="TypesOfFileAccess"></a>
### <a name="types-of-file-access-granted-to-an-app"></a>Typy souborů přístupu k aplikaci

Jednotlivé zákazníky předplatného nastaly struktuře rezervovaná adresářů ve sdílené složce konkrétní UNC v datovém centru. Zákazník může mít více aplikací vytvořené v rámci konkrétní datovém centru, tak všechny adresáře patřící do jediného odběratele předplatného vytvořené ve stejném UNC sdílet. Příkaz sdílet může obsahovat adresářů třeba můžou být pro obsah, chyby a protokoly pro diagnostiku a starších verzích aplikaci vytvořil ovládacího prvku zdroje. Podle očekávání adresáře aplikace zákazníka jsou k dispozici pro čtení a zápis za běhu kódem aplikace v aplikaci.

Na místní jednotky připojené k virtuální počítač, který spustí aplikace pro aplikaci služby si vyhrazuje bloku místa na disku C:\ pro dočasné místní úložiště specifické pro aplikaci. Ačkoli aplikace obsahuje celou pro čtení i zápis přístup k vlastním dočasné místní úložiště, úložiště skutečně není určená k přímo kódem aplikace. Místo toho záměru je stanovit úložiště dočasných souborů služby IIS a webové aplikace rámce. Aplikaci služby taky omezení počtu dočasné místní úložiště k dispozici pro jednotlivé aplikace zabráníte jinými nadbytečné množství místního úložiště souborů jednotlivých aplikací.

Dva příklady použití dočasné místní úložiště služby aplikace jsou v adresáři dočasných souborů ASP.NET a v adresáři služby IIS komprimované soubory. Systém kompilace ASP.NET používá adresáři "Dočasných souborů ASP.NET" jako dočasné kompilace umístění mezipaměti. Služby IIS používá k ukládání výstupu mu tuhle zkomprimovanou odpověď directory "Dočasně komprimovanými soubory IIS". Oběma typy souborů použití (i ostatní) jsou znovu namapovat v aplikaci služby k za aplikace dočasné místním úložišti. Tento aktualizaci informací o umístění zaručuje, že funkce pokračuje podle očekávání.

Jednotlivé aplikace v aplikaci služby běží jako identitu náhodné jedinečné minimum oprávněními pracovního procesu s názvem "identity fondu aplikací", zde popsané dál: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Kód aplikace používá tuto identitu základní jen pro čtení přístupu pro operační systém jednotka (D:\ jednotky). To znamená kódu aplikace můžete seznam běžných struktury adresářů a čtení běžné souborů na disku operační systém. I když to může zdát poněkud obecných úroveň přístupu, stejné adresáře a soubory jsou dostupné, když zřizujete pracovního role ve Azure hostované služby a číst obsah jednotky. 

<a name="multipleinstances"></a>
### <a name="file-access-across-multiple-instances"></a>Přístup k souborům napříč několika instancích spuštěných

Adresáři obsahuje aplikace pro obsah a kód aplikace můžete psát do ní. Pokud aplikace získáte v několika instancích spuštěných adresáři je sdíleny všechny instance tak, aby všechny instance vidět stejného adresáře. Ano například pokud aplikace uloží nahrané soubory adresáři, tyto soubory jsou okamžitě dostupné pro všechny instance. 

<a id="NetworkAccess"></a>
## <a name="network-access"></a>Přístup k síti
Kód aplikace můžete použít TCP/IP a na základě protokoly aby odchozí síťové připojení k přístupných osobám s postižením koncové body Internet, které poskytují externí služby. Aplikace můžete použít tyto stejné protokoly pro připojení k služby Azure & #151, například vytvořením HTTPS připojení k databázi SQL.

Je také omezené možnosti pro aplikace jednoho připojení místní smyčky a mít aplikace naslouchají relacím na této socket místní smyčky. Tato funkce existuje především pro povolení aplikace, které naslouchají relacím na místní smyčky sockets jako součást jejich funkce. Všimněte si, že každé aplikaci uvidí "Soukromé" smyčky připojení. aplikace "A" nelze poslouchat místní smyčky socket zřídit podle aplikace "B".

Pojmenované kanály jsou taky podporované jako mechanismus komunikaci mezi procesy (IPC) mezi různých procesů, které souhrnně spustit aplikaci. Například modulu služby IIS FastCGI závisí na pojmenované kanály ke koordinaci jednotlivé procesy, které stránky PHP spustit.


<a id="Code"></a>
## <a name="code-execution-processes-and-memory"></a>Spuštění kódu, procesy a paměti
Jak je uvedeno dříve, aplikace spustit uvnitř minimum oprávněními pracovní procesy pomocí identity fondu náhodné aplikací. Kód aplikace má přístup k paměť přidružené pracovní proces, jakož i všechny podřízené procesy, které mohou být vytvořený procesy CGI nebo jiných aplikacích. Však jedné aplikaci nemáte přístup ke data z jiné aplikace či paměti i v případě, že je na stejném počítači virtuální.

Aplikace můžete spustit skripty nebo stránky napsané s podporované webové vývojové rámce. Aplikaci služby nemá nastavení libovolný web framework na omezenější režimy. Například v "" úplná namísto omezenější režimu zabezpečení spustit ASP.NET aplikace spuštěné v aplikaci služby. Web rámce, obsahující překlad prováděný klasické ASP i ASP.NET, můžete volat součástí COM v procesu (ale není mimo součástí COM proces) jako ADO (ActiveX Data Objects), které jsou registrované ve výchozím nastavení v operačním systému Windows.

Aplikace můžete spustit a spusťte libovolného kódu. Je povolený aplikace provádět věci, jako vytváření příkazového prostředí nebo spustit skript Powershellu. I když je libovolný kód a procesy můžete vytvořený z aplikace, programy a skripty jsou ale stále omezen na oprávnění udělena do fondu aplikací nadřazené. Například aplikace se mohou provést spustitelný soubor, díky kterému odchozí hovory HTTP, ale že stejný spustitelný nelze pokus o odpojení IP adresu virtuálního počítače z jeho NIC. Uskutečnit hovor odchozí síťové bude moct kód minimum oprávněními, ale pokusíte překonfigurovat síťové nastavení počítače virtuální vyžaduje oprávnění správce.


<a id="Diagnostics"></a>
## <a name="diagnostics-logs-and-events"></a>Událostí a protokolování diagnostiky
Informace v protokolu je jinou sadu dat, která některé aplikace pokusí o přístup k. Typy informací protokolu k dispozici pro kód spuštěný v aplikaci služby včetně diagnostických a zaznamenávat informace generované aplikace, která je také snadný přístup k aplikaci. 

Například nejsou k dispozici, buď na adresář s protokolem v umístění v síti sdílet vytvořené pro aplikaci nebo k dispozici v úložišti objektů blob Pokud zákazník nastavila protokolování W3C k základnímu úložišti W3C HTTP protokoly generovaný aplikace aktivní. Tato možnost umožňuje velké množství protokoly shromažďování bez rizika dokonce překračují limity úložiště souborů přidružené sdílené síťové složky.

V souvislosti podobně jako v reálném čase diagnostické informace z aplikace .NET můžete taky Zaprotokolují .NET trasování a Diagnostika infrastrukturu pomocí možnosti zapsat sledovat informace o buď aplikaci na sdílené síťové složky, případně na úložiště objektů blob.

Oblasti diagnostiky protokolování a trasování, které nejsou k dispozici pro aplikace jsou událostí Windows trasování událostí pro Windows a běžných protokoly událostí systému Windows (například systém, aplikace a zabezpečení protokoly událostí). Protože trasování událostí pro Windows Sledování informací potenciálně lze zobrazit celého (plus pár doprava ACL), jsou blokovány pro čtení a zápis události trasování událostí pro Windows. Vývojáři může se stát, že volání rozhraní API pro čtení a zápis trasování událostí pro Windows zobrazí události a běžných protokoly událostí Windows pro práci, ale je proto, že aplikace služby je "faking" volání tak, aby se nezdaří. Ve skutečnosti nemá kód aplikace přístup k těmto datům události.

<a id="RegistryAccess"></a>
## <a name="registry-access"></a>Přístup k registru
Aplikace využívat jen pro čtení hodně (i když není vše) registru virtuálního počítače se systémem. Ve skutečnosti to znamená, že jsou přístupné aplikace klíče registru přístupu skupiny místních uživatelů jen pro čtení. Jedné části registru, která není aktuálně podporován pro čtení a zápis je HKEY\_aktuální\_podregistru uživatele.

Zápis v registru jsou blokovány, včetně přístup k žádné klíče registru jednotlivé uživatele. Z pohledu aplikace na přístup pro zápis registru by nikdy by se mohly spolehnout v Azure prostředí od aplikace můžete (a dělat) získat poštovním v různých virtuálních počítačích. Pouze trvalého zapisovatelné úložiště, který může být závislé na aplikace je struktura adresář s obsahem za aplikace ve sdílené složky aplikace služby UNC uložený. 

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 
