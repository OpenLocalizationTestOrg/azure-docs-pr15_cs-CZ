# <a name="compute"></a>Výpočet

Azure umožňuje nasazení a sledovat kód aplikace spuštěné v datovém centru Microsoftu. Při vytváření aplikace a spustit v Azure, kód a konfigurace společně se nazývá Azure hostovaná služby. Hostovanou služby se snadno spravovat měřítko nahoru a dolů, nakonfigurovat a aktualizovat s novou verzí aplikace kód. Tento článek se zaměřuje na Azure hostované model aplikace služeb.<a id="compare" name="compare"></a>

## Obsahy<a id="_GoBack" name="_GoBack"></a>

-   [Výhody modelu Azure aplikace][]
-   [Hostovanou služby základní koncepty][]
-   [Důležité informace o návrhu hostovanou službu][]
-   [Navrhování aplikace pro měřítko][]
-   [Definice hostovanou službu a konfigurace][]
-   [Soubor definice služby][]
-   [Konfigurační soubor služby][]
-   [Vytvoření a nasazení hostovanou službu][]
-   [Odkazy][]

## <a id="benefits"> </a>Výhody modelu azure aplikace

Při nasazení aplikace jako hostovanou službu Azure vytvoří jeden nebo více virtuálních počítačích (VMs), které obsahují kód vaše aplikace a spuštění VMs fyzické počítačích umístěných v jednom z Azure datacentrech. Podle žádosti klienta o hostovanou aplikaci zadat datovém centru, Vyrovnávání zatížení distribuuje tyto požadavky rovnoměrně z vnější VMs. Zatímco aplikace je umístěn v Azure, dostane tři klíčové výhody:

-   **Dostupnost.** Dostupnost znamená, že Azure zajišťuje, že aplikace běží co nejvíc a je možné odpověď na žádosti klienta o. Pokud aplikace ukončí (kvůli neošetřené výjimce, například), potom Azure zjistí to a bude automaticky znova spusťte aplikaci. Pokud počítač běží aplikace na prostředí určitý druh selhání hardwaru, pak Azure také reprodukovat a automaticky vytvořit nové OM na jiném počítači fyzické pracovní a spustíte kód odtud. Poznámka: Aby aplikace získat společnosti Microsoft smlouvách úroveň 99.95 % k dispozici, musí mít aspoň dva VMs spuštěním kódu aplikace. Díky jeden OM zpracovat žádosti klienta o během Azure přesune kódu z selhalo OM do nového, dobrý OM.

-   **Škálovatelnost.** Azure umožňuje snadno a dynamické změnit počet zobrazených VMs spuštění aplikace kódu pro zpracování aktuální zatížení uvedení na aplikace. Umožňuje upravit aplikaci pracovní zátěž, který zákazníkům jsou uvedení na něm při placení jenom pro VMs potřebujete, když je potřebujete. Když budete chtít změnit počet zobrazených VMs, Azure odpoví během několika minut a umožňuje dynamicky změnit počet zobrazených VMs spuštěný jako často podle potřeby.

-   **Možnosti správy.** Vzhledem k tomu Azure platformu jako služba (PaaS) nabízející, spravuje infrastruktury (vlastním hardwaru, elektrické a sítě) povinnost uchovávat tyto počítače se systémem. Azure také spravuje platformy, zajištění aktuální operační systém s všechny správné opravy a aktualizace zabezpečení, stejně jako součást aktualizace například .NET Framework a Internet Information Server. Protože všechny VMs běží Windows Server 2008, Azure poskytuje další funkce, jako jsou diagnostiky sledování Podpora vzdálené plochy, bránách firewall a konfigurace úložiště certifikátů. Tyto funkce jsou k dispozici bez extra nákladů. Ve skutečnosti při spuštění aplikace v Azure licence systému Windows Server 2008 operačního systému (OS) je však započítávány. Protože všechny VMs běží Windows Server 2008, jakýkoli kód, který běží v systému Windows Server 2008 funguje jenom při spuštění v Azure.

## <a id="concepts"> </a>Hostované služby základní koncepty

Při nasazení aplikace jako hostovanou službu v Azure běží jako jeden nebo více *role.* *Role* jednoduše odkazuje soubory aplikace a konfigurace. Definovat role jeden nebo více aplikace, oba objekty mají vlastní sadu soubory aplikace a konfigurace. Pro každou roli v aplikaci můžete zadat počet VMs a *rolí instance*spustit. Následující obrázek zobrazit dva jednoduché příklady modelovat jako hostovanou službu pomocí rolí a rolí instancí aplikace.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Obrázek 1: Jeden role s tři instance (VMs) spuštěné v centru Azure dat

![Obrázek][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Obrázek 2: Dvě role, oba objekty mají dvě instance (VMs) spuštěné v centru Azure dat

![Obrázek][1]

Role instance obvykle zpracovávat žádosti klienta Internet zadávání datového centra takzvanou *vstupní koncový bod*. Jeden role může mít 0 nebo další vstupní koncové body. Každý koncový bod označuje protokol (HTTP, HTTPS nebo TCP) a port. Je běžné konfigurace role mít dvě vstupní koncové body: HTTP listening na porty 80 a HTTPS listening na port 443. Následující obrázek znázorňuje příklad dva různé role pomocí různých vstupní koncové body přesměrování požadavků klientů na ně.

![Obrázek][2]

Při vytváření hostovanou službu v Azure přidělený veřejně s možností zadání IP adresy, kterou klienti můžou pomocí k ní přístup. Při vytváření hostovanou službu je nutné zaškrtnout předponu adresu URL, která je namapovaných na danou IP adresu. Tuto předponu musí být jedinečné, jako jsou v podstatě rezervaci *předponu*. cloudapp.net URL tak, že nikdo jiný měli ho. Klienti komunikovat s rolí instance pomocí adresy URL. Obvykle, nebude distribuce nebo publikovat Azure *předponu*. cloudapp.net URL. Místo toho bude koupit název DNS od svého registrátora DNS podle výběru a konfigurace názvu DNS přesměrovat požadavků klientů na adresu URL Azure. Další informace najdete v tématu [konfigurace vaší vlastní doménou v Azure][].

## <a id="considerations"> </a>Hostované služby navrhování

Při návrhu aplikace pro spuštění v prostředí cloudu, je několik tipů k myslete například latence, vysoké dostupnosti a škálovatelnost.

Umístění kód aplikace je důležitým aspektem, pokud běží hostovanou službu Azure. Je běžné nasadit aplikaci datacentrech, které jsou nejblíže k klienty snížit latence a získat možné nejlepší možný výkon.
Však můžete zvolit datovém centru blíž k vaší společnosti nebo blíž k datům, pokud máte nějaký příslušnosti nebo právní pochybnosti týkající se vašich dat a kde se nachází. Může být hostitelem kód aplikace se šesti datacentrech po celém světě. Následující tabulka ukazuje dostupná umístění:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr>
<td style="width: 100px;">
**Země/oblasti**

</td>
<td style="width: 200px;">
**Dílčí oblastí**

</td>
</tr>
<tr>
<td>
Spojené státy americké

</td>
<td>
Severní střední a Jižní centrální

</td>
</tr>
<tr>
<td>
Europe

</td>
<td>
Severní & západní

</td>
</tr>
<tr>
<td>
Země Asie

</td>
<td>
Jihovýchod a východ

</td>
</tr>
</tbody>
</table>
Při vytváření hostovanou službu, vyberte dílčí oblast označující umístění, ve kterém chcete spuštění kódu.

K dosažení dostupnost a škálovatelnost, je zásadně důležitá data aplikace uchovávat v centrálním úložišti přístupných osobám s postižením k několika instancích spuštěných role. Pokud chcete s tímto, Azure nabízí několik možností úložiště například objektů BLOB, tabulky a databáze SQL. Najdete v článku [Datový úložiště nabídky v Azure][] Další informace o těchto technologií úložiště. Následující obrázek ukazuje, jak Vyrovnávání zatížení uvnitř Azure datovém centru distribuuje požadavků klientů na jinou roli instance všichni z nich mají přístup ke stejné ukládání dat.

![Obrázek][3]

Obvykle chcete najít kód aplikace a dat v centru stejné dat jako tato funkce umožňuje zhoršeným latence (lepší výkon) při použití kód přistupuje data. Kromě toho můžete nejsou účtován šířky pásma dat je přesunutí v rámci stejného datového centra.

## <a id="scale"> </a>Zvážení měřítko

V některých případech může chcete pořídit jediné aplikaci (třeba jednoduchý webový server) a byl použitý ve Azure. Ale často aplikace může obsahovat více rolí společně. Například na obrázku existuje dvě instance webu role, tři instance role zpracování objednávek a jednou instancí roli Generátor sestav. Tyto role jsou všechny spolupráce a kód všem poznámkám můžete balíčku a nasazené jako jednu jednotku až Azure.

![Obrázek][4]

Hlavní důvod, proč rozdělení aplikace do různé role každý spuštěných pro vlastní sadu instancí role (to znamená VMs) je měřítko role nezávisle na sobě. Například během svátcích, množství zákazníků může být nákupu produkty z vaší společnosti, tak můžete chtít zvýšení počtu role instance spuštěna vaše role webu, stejně jako počet role instance spuštěna vaše role zpracování objednávek. Po svátcích může dostáváte velké produktů vrácených, abyste pořád budete potřebovat spoustu instance webu, ale méně instance zpracování objednávek. Během zbytku roku může stačí pár web a zpracování objednávek instance. V celém to všechno možná budete muset jenom jedna instance Generátor sestav. Flexibility nasazení na základě rolí v Azure umožňuje snadno upravit aplikace potřebám vaší společnosti.

Je běžné mít požadovanou roli instancí v hostovanou službu vzájemné komunikaci. Například roli webu přijme objednávky zákazníků ale pak offloads pořadí zpracování na role instance zpracování objednávek. Nejlepší způsob, jak předat Formulář pracovní jednu sadu instancí role na jinou sadu instancí používá fronty technologii poskytovanou Azure, služba fronty nebo fronty Bus služby. Použití fronty je důležitou součástí článku. Ve frontě umožňuje hostovanou službu zobrazit rolí umožňuje nezávisle na sobě vyrovnat pracovní zátěž proti náklady. Pokud počet zpráv ve frontě zvyšuje určitou dobu, pak můžete změnit telefonní číslo instancí role zpracování objednávek. Pokud časem snižuje počet zpráv ve frontě, pak můžete změnit počet zpracování objednávek instance role. Tímto způsobem můžete jenom platíte instancí vyžadované pro zpracování vytížení.

Ve frontě také poskytuje spolehlivost. Po zmenšení počet zpracování objednávek instancí role Azure rozhoduje o které instance ukončit. Rozhodnout ukončit instance, která je uprostřed zpracování zprávy fronty. Ale protože zpracování zprávy úspěšně nedokončí, zprávu viditelná znovu do jiné zpracování objednávek role instance, který ho vyzvedne a zpracuje ji. Z důvodu viditelnost zpráv fronty je nakonec získat zpracovány zaručena zprávy. Ve frontě taky slouží jako při vyrovnávání zatížení efektivně distribuce své zprávy instancí za veškerá role, které zprávy požádat ho.

Role instance webu, můžete monitorovat data chodit do nich a rozhodnout zobrazit počet je nahoru nebo dolů, stejně. Ve frontě umožňuje zobrazit počet instancí webu role nezávisle na role instance zpracování objednávek. Toto je hodně výkonná která nabízí spoustu flexibility. Samozřejmě pokud vaše aplikace obsahuje další role, můžete přidat další fronty jako přenos komunikaci mezi nimi abyste mohli využít stejné měřítko a náklady výhody.

## <a id="defandcfg"> </a>Hostovaný definici služby a konfigurace

Nasazení hostovanou službu Azure vyžaduje také soubor definice služby a služby konfigurační soubor. Oba tyto soubory jsou soubory XML a umožňují deklarativně zadejte nastavení nasazení hostovanou službu. Soubor definice služby popisuje všechny role, které tvoří hostovanou službu a způsob komunikace. Konfigurační soubor služby popisuje počet instancí pro každou roli a nastavení sloužící ke konfiguraci pokaždé role.

## <a id="def"> </a>Soubor definice služby

Jak můžu jsme zmínili dříve, definice služby (CSDEF) je soubor XML soubor, který popisuje různé role, které tvoří dokončení aplikace. Úplné schéma XML souboru najdete tady: [[http://msdn.microsoft.com/library/windowsazure/ee758711.aspx]].
Soubor CSDEF obsahuje prvek WebRole nebo WorkerRole pro každou roli, která se má v aplikaci. Nasazení role roli web (pomocí WebRole element) znamená, že kód poběží na instanci rolí obsahující Windows Server 2008 a informace o serveru Internetové informační.
Nasazení role podle role pracovníka (pomocí WorkerRole element) znamená, že instanci rolí bude Windows Server 2008 na něm (IIS se nenainstaluje).

Určitě vytvoření a nasazení role kolegy používající určitý mechanismus přijímat příchozí žádosti web (například kód může vytváření a používání .NET HttpListener). Protože všechny instance role běží Windows Server 2008, kódu můžete provádět všechny operace, které jsou dostupné pro spuštění aplikace v systému Windows Server
2008.

Pro každou roli označíte požadovanou velikost OM používaná instancí určitou roli. Následující tabulka ukazuje různých velikostech OM dnes k dispozici a atributy každého:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr align="left" valign="top">
<td>
**Velikost OM**

</td>
<td>
**PROCESOR**

</td>
<td>
**RAM**

</td>
<td>
**Disk**

</td>
<td>
**Pole Špička sítě vstupu a výstupu**

</td>
</tr>
<tr align="left" valign="top">
<td>
**Extra Small**

</td>
<td>
GHz 1 x 1.0

</td>
<td>
768 MB

</td>
<td>
20GB

</td>
<td>
\~5 MB /

</td>
</tr>
<tr align="left" valign="top">
<td>
**Small**

</td>
<td>
1 x 1,6 GHz

</td>
<td>
1,75 GB

</td>
<td>
225GB

</td>
<td>
\~100 MB /

</td>
</tr>
<tr align="left" valign="top">
<td>
**Střední**

</td>
<td>
2 × 1,6 GHz

</td>
<td>
3,5 GB

</td>
<td>
490GB

</td>
<td>
\~200 MB /

</td>
</tr>
<tr align="left" valign="top">
<td>
**Velká**

</td>
<td>
4 × 1,6 GHz

</td>
<td>
7 GB

</td>
<td>
1TB

</td>
<td>
\~400 MB /

</td>
</tr>
<tr align="left" valign="top">
<td>
**Největší**

</td>
<td>
8 × 1,6 GHz

</td>
<td>
14 GB

</td>
<td>
2TB

</td>
<td>
\~800 MB /

</td>
</tr>
</tbody>
</table>
Vám bude účtovaná hodinové pro každou OM použít jako instanci rolí a vám bude taky Účtovaná pro všechna data, že vaše role instancí posílat mimo datovém centru. Nejsou platit pro zadávání dat centra data. Další informace najdete v tématu [[Azure ceny]]. Obecně je vhodné používat mnoha případech malé role namísto několik velké instancí aplikace je další pružné selhání. Nakonec máte míň instance role, více katastrofální a Chyba instalace v jednom z nich celkové aplikace. Také jak je uvedeno před, musí nasadíte nejméně dvě instance pro každou roli abyste mohli získávat smlouva o úrovni služeb 99.95 %, který poskytuje Microsoft.

Soubor definice (CSDEF) služby je také místo, kam zadáte mnoho atributů o každou roli v aplikaci. Tady jsou některé položky zvýšíte jeho přínos k dispozici:

-   **Certifikáty**. Použití certifikáty pro šifrování dat nebo pokud webová služba podporuje protokol SSL. Certifikáty muset nahrát Azure. Další informace najdete v tématu [Správa certifikátů v Azure][]. Toto nastavení používají XML nainstaluje jste předtím nahráli certifikáty do úložiště certifikátů instanci rolí, aby byly použitelné kódem aplikace.

-   **Konfigurace nastavení názvy**. Pro hodnoty, které chcete, aby vaše aplikace číst při spuštění na instanci rolí. Skutečná hodnota nastavení konfigurace nastavenou v souboru konfigurace (CSCFG) služby, které můžete kdykoli aktualizovat aniž by bylo potřeba nasadit kódu. Kromě toho můžete kód aplikace způsobem ke zjištění hodnoty změněné konfigurace bez by všechny prostoje.

-   Při **zadávání koncové body**. Tady můžete zadat všechny HTTP, HTTPS nebo TCP koncové body (plus pár porty), které chcete vystavit do vnější světa prostřednictvím *předponu*. cloadapp.net URL. Když Azure nasadí vaše role, je nastaví její konfiguraci a bránu firewall na instanci rolí automaticky.

-   **Vnitřní koncové body**. Tady můžete zadat všechny HTTP nebo TCP koncové body, které chcete vystaven ostatních rolí instancí nasazené jako součást aplikace. Povolit všechny instance role v aplikaci vzájemné komunikaci interní koncové body, ale nejsou přístupné pro všechny instance role, které jsou mimo aplikaci.

-   **Importovat moduly**. Tyto volitelně nainstalovat užitečné součásti role instance. Součásti existují pro diagnostiku sledování Vzdálená plocha a Azure připojení (umožňující instanci rolí s přístupem k místním zdroje přes zabezpečený kanál).

-   **Místní uložení**. To přidělí podadresáře instance role pro aplikaci použít. To je podrobněji píše v článku [Datový úložiště nabídky v Azure][] .

-   **Spuštění úkoly**. Spuštění úlohy umožňují nainstalovat požadované součásti instanci rolí spuštění nahoru. Úkoly, můžete spustit jako správce v případě potřeby zvýšenými oprávněními.

## <a id="cfg"> </a>Soubor konfigurační služby

Soubor služby konfigurace (CSCFG) je soubor XML, který popisuje nastavení, které bude možné měnit bez znovu nasazení aplikace. Úplné schéma XML souboru najdete tady: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][].
Soubor CSCFG obsahuje prvek Role pro každou roli v aplikaci. Tady je několik položek, které můžete použít v souboru CSCFG:

-   **Operační systém verze**. Tenhle atribut umožňuje vybrat verze operačního systému (OS), který chcete použít pro všechny instance role spuštěním kódu aplikace. Tento OS se označuje jako *Host OS*a každé nové verze zahrnuje nejnovější opravy zabezpečení a aktualizacích dokumentu v době, kdy vydání hosta s operačním systémem. Pokud má argument hodnota atributu osVersion "\*", pak Azure automaticky aktualizuje hosta OS ve všech instancích role dostupné nové verze s operačním systémem Host. Můžete však výslovného nesouhlasu automatické aktualizace tak, že vyberete konkrétní Host operační systém verze. Příklad nastavení atribut osVersion hodnotu "WA hosta OS 2,8\_201109 01" způsobí, že všechny instance role zjistit, co je popsáno na tuto webovou stránku: [http://msdn.microsoft.com/library/hh560567.aspx][]. Další informace o hosta operační systém verze najdete v článku [Správa upgrady OS hostů Azure].

-   **Instance**. : Tento element hodnota označuje počet instancí role, které chcete zřizování spuštění kódu pro konkrétní rolí. Vzhledem k tomu můžete nahrát nový soubor CSCFG na Azure (opětovné nasazení aplikace), je trivially Snadná změna hodnoty pro tento element a odeslat nový soubor CSCFG dynamicky zvětšit nebo zmenšit počet instancí role spuštěním kódu aplikace. Umožňuje snadno zobrazit aplikace nahoru nebo dolů setkat skutečné vytížení při také řízení kolik vám bude účtovaná za spuštěné instance role.

-   **Konfigurace nastavení hodnot**. : Tento element označuje hodnoty pro nastavení (podle definice v souboru CSDEF). Vaše role může číst tyto hodnoty je spuštěná. Tyto hodnoty nastavení konfigurace se obvykle používají řetězců připojení k databázi SQL nebo k základnímu úložišti Azure, ale lze za účelem, které si přejete.

## <a id="hostedservices"> </a>Vytvoření a nasazení hostovanou službu

Vytvoření hostovanou službu vyžaduje, že nejdřív přejděte na [Portálu Správa Azure] a zřízení hostovanou službu zadáním předponu DNS a datovém centru nakonec chcete svůj kód spuštěný v. Pak v vývojové prostředí vytvořit soubor definice (CSDEF) služby, sestavovat kód aplikace a balíčku (PSČ) všechny tyto soubory do služby soubor balíčku (CSPKG). Soubor služby konfigurace (CSCFG) musí taky připravit. Abyste mohli nasadit vaše role, nahrajte soubory CSPKG a CSCFG pomocí rozhraní API správy služby Azure. Po zavedení, Azure, bude poskytování role instancí v datovém centru (na základě konfigurace dat) extrahovat kód aplikace z balíčku, zkopírujte do instance role a spuštění instance. Nyní je váš kód do začátků.

Následující obrázek ukazuje CSPKG a CSCFG soubory, které vytvoříte ve vývojovém počítači. Soubor CSPKG obsahuje soubor CSDEF a kód dvě role. Po odeslání CSPKG a CSCFG soubory s rozhraní API správy služby Azure, vytvoří Azure instance role v datovém centru. V tomto příkladu souboru CSCFG uvedeno, že Azure mají vytvořit tři instance role \#1 a 2 výskyty Role \#2.

![Obrázek][5]

Další informace o nasazení upgrade a změny konfigurace vaše role naleznete v článku [zavedení a aktualizace aplikací Azure][] .<a id="Ref" name="Ref"></a>

## <a id="references"> </a>Odkazy

-   [Vytváření hostovanou službu Azure][]

-   [Správa hostované služby Azure][]

-   [Migrace aplikací Azure][]

-   [Konfigurace aplikace Azure][]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Napsal Jan Richter (Wintellect)</p>

</div>

  [Výhody modelu Azure aplikace]: #benefits
  [Hostovanou služby základní koncepty]: #concepts
  [Důležité informace o návrhu hostovanou službu]: #considerations
  [Navrhování aplikace pro měřítko]: #scale
  [Definice hostovanou službu a konfigurace]: #defandcfg
  [Soubor definice služby]: #def
  [Konfigurační soubor služby]: #cfg
  [Vytvoření a nasazení hostovanou službu]: #hostedservices
  [Odkazy]: #references
  [0]: ./media/application-model/application-model-3.jpg
  [1]: ./media/application-model/application-model-4.jpg
  [2]: ./media/application-model/application-model-5.jpg
  [Konfigurace vaší vlastní doménou v Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
  [Nabídka úložiště dat v Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
  [3]: ./media/application-model/application-model-6.jpg
  [4]: ./media/application-model/application-model-7.jpg
  
  [Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
  [Správa certifikátů v Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
  [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
  [http://msdn.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
  [Správa upgrady Azure hostů OS]: http://msdn.microsoft.com/library/ee924680.aspx
  [Portál Správa Azure]: http://manage.windowsazure.com/
  [5]: ./media/application-model/application-model-8.jpg
  [Nasazení a aktualizace Azure aplikací]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
  [Vytváření hostovanou službu Azure]: http://msdn.microsoft.com/library/gg432967.aspx
  [Správa hostované služby Azure]: http://msdn.microsoft.com/library/gg433038.aspx
  [Migrace aplikací Azure]: http://msdn.microsoft.com/library/gg186051.aspx
  [Konfigurace aplikace Azure]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
