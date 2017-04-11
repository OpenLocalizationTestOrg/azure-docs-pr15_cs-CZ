<properties
    pageTitle="Azure přihlašovacích údajů aplikace, virtuálních počítačích, služby struktury a Cloudovým službám porovnání | Microsoft Azure"
    description="Zjistěte, jak zvolit mezi službou Azure aplikace, virtuálních počítačích, služby struktury a Cloud Services pro hostování webových aplikací."
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure porovnání přihlašovacích údajů aplikace, virtuálních počítačích, struktury služby a služby cloudu

## <a name="overview"></a>Základní informace

Azure nabízí několik způsobů, jak weby Host (hostitel): [Aplikaci služby Azure][], [virtuálních počítačích][], [Služby struktury][]a [Cloudovým službám][]. Tento článek vám pomůže porozumět možnostem a ten správný webové aplikace.

Azure aplikaci služby je nejlepší volbou pro většinu webových aplikacích. Nasazení a správu integrovaný platformu, weby můžete změnit velikost rychle zpracovávání vysoký provoz zatížení a Správce služby Vyrovnávání a provozu předdefinované zatížení zadejte dostupnost. Je mohli přesouvat stávajících webů služby Azure aplikace snadno pomocí [Nástroje pro migraci online](https://www.migratetoazure.net/), použijte aplikaci otevřít zdroj z Galerie webové aplikace a vytvoření nového webu pomocí framework a nástrojů pro podle svého výběru. Funkce [WebJobs][] umožňuje snadno sčítat úlohu pozadí zpracování do webové aplikace aplikaci služby.

Služba struktury je vhodné použít, pokud vytváříte novou aplikaci nebo znovu psaní existující aplikace používat architekturu microservice. Aplikace, které se spouštějí na sdíleném fondu strojů, můžete začít small a postupně se zvětšují rozsáhlé měřítko s stovky nebo tisíce počítačích podle potřeby. Stavová služba usnadnění konzistentní a problémy se spolehlivým uložení stavu aplikace a služby struktury automaticky spravuje služby rozdělení měřítko a dostupnosti za vás.  Služby struktury taky podporuje WebAPI s otevřít webového rozhraní pro .NET (OWIN) a základní ASP.NET.  Ve srovnání s aplikací služby struktury také poskytuje další publikum nemůže ovládat nebo přímý přístup k základní infrastruktury. Můžete do svých serverech vzdálené nebo konfigurace serveru spuštění úkoly. Cloudové služby je podobný struktury služby v stupeň ovládací prvek a snadnější použití, ale je teď starší verze služby a služby struktury je vhodné pro vývoj.

Pokud máte existující aplikace, které vyžadují podstatné změny spuštění služby aplikaci nebo službu struktury, mohli byste virtuálních počítačích za účelem zjednodušení migrace do cloudu. Ale správně konfigurace zabezpečení a zachovat VMs vyžaduje mnohem víc času a IT znalosti ve srovnání s Azure aplikaci služby a služby struktury. Chcete-li instalovat virtuálních počítačích Azure, musíte že vzít v úvahu probíhající Údržba vyžaduje opravu, aktualizovat a správy prostředí OM. Azure virtuálních počítačích je infrastruktury jako-Service (IaaS), zatímco aplikace služby a služby struktury jsou platformu jako-Service (Paas). 

## <a name="features"></a>Porovnání funkcí

Následující tabulka porovnává možnosti přihlašovacích údajů aplikace, cloudové služby, virtuálních počítačích a struktury služby pro vám pomůže zajistit nejlepší varianta. Aktuální informace o SLA jednotlivých možnostech najdete v tématu [Smlouvy o úrovni služeb Azure](/support/legal/sla/).

Funkce|Aplikace služby (webové aplikace)|Cloud Services (role web)|Virtuálních počítačích|Služba struktury|Poznámky
---|---|---|---|---|---
V blízkosti rychlého nasazení|X|||X|Nasazení aplikace nebo aktualizace aplikace do cloudové služby, nebo vytváření OM, trvá několik minut aspoň; nasazení aplikace do webových aplikací trvá sekund.
Přejít na větší počítačích bez opětovném nasazení|X|||X|
Webový server instance sdílet obsah a konfigurace, což znamená, že nemusíte přeinstalujte nebo překonfigurovat při změně velikosti.|X|||X|
Prostředí s více nasazení (výrobní a pracovní)|X|X||X|Služba struktury umožňuje mít víc prostředí pro aplikace nebo nasadit různé verze aplikací vaše aplikace vedle sebe.
Automatická aktualizace Správa OS|X|X|||Automatické aktualizace OS plánujeme pro budoucí struktury služby.
Přepínání bezproblémové platformu (snadno přejít mezi 32bitový a 64bitový)|X|X|||
Nasazení kódu s libovolná, FTP|X||X||
Nasazení kódu s nasazení webu|X||X||Cloud Services podporuje použití nasazení webu instalovat aktualizace na jednotlivé role instance. Však nelze ji použijete počáteční nasazení roli a použijete nasazení webu k aktualizaci budete muset nasazení samostatně každý výskyt roli. Abychom měli nárok pro SLA služby cloudu pro provozním prostředí jsou vyžadovány několika instancích spuštěných.
Podpora WebMatrix|X||X||
Přístup ke službám jako Bus služby, úložiště, databáze SQL|X|X|X|X|
Host (hostitel) webu nebo na web služby vrstvy architektury vícevrstvého|X|X|X|X|
Host (hostitel) střední vrstvy architektury vícevrstvého|X|X|X|X|Aplikaci služby web apps můžete snadno hostovat střední úroveň rozhraní REST API a funkci [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) můžete hostovat pozadí zpracování úloh. Spusťte WebJobs vyhrazené webu dosáhnout nezávislé rozšiřitelnost pro osy. Funkce [rozhraní API aplikace](../app-service-api/app-service-api-apps-why-best-platform.md) náhled poskytuje ještě víc funkcí pro hostitelské služby ZBÝVAJÍCÍ.
Integrované MySQL jako služby podpory|X|X|X||Cloud Services můžete integrovat MySQL jako služby prostřednictvím společnosti ClearDB nabídky, ale ne jako součást Azure portál pracovního postupu.
Podpora technologie ASP.NET klasické ASP Node.js, PHP, Python|X|X|X|X|Služba struktury podporuje vytváření webových front-end pomocí [technologie ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) nebo je můžete nasadit libovolný typ aplikace (Node.js, Java atd.) jako [Host spustitelný](../service-fabric/service-fabric-deploy-existing-app.md).
Rozšiřování do několika instancích spuštěných bez opětovném nasazení|X|X|X|X|Můžete do několika instancích spuštěných rozšiřování virtuálních počítačích, ale služeb spuštěných na nich musí být došlo k zápisu zpracovat tento škálování. Budete muset konfigurace služby Vyrovnávání zatížení směrování požadavky na různých počítačích a vytvořit skupinu spřažení bránící souběžné restartování všechny instance kvůli selhání údržbu nebo hardwaru.
Podpora pro SSL|X|X|X|X|Aplikaci služby webových aplikacích pro protokol SSL pro vlastní názvy domén je podporována pouze základní a standardní režimu. Informace o používání SSL pomocí webových aplikací web apps najdete v tématu [Konfigurace certifikátu SSL webu Azure](../app-service-web/web-sites-configure-ssl-certificate.md).
Integrace aplikace Visual Studio|X|X|X|X|
Vzdálené ladění|X|X|X||
Nasazení kódu s TFS|X|X|X|X|
Izolace sítě s [Azure virtuální sítě](/services/virtual-network/)|X|X|X|X|Viz taky [integraci virtuální sítě Azure weby](/blog/2014/09/15/azure-websites-virtual-network-integration/)
Podpora pro [Správce Azure dopravy](/services/traffic-manager/)|X|X|X|X|
Sledování integrované koncový bod|X|X|X||
Vzdálené plochy přístup k serverům||X|X|X|
Nainstalujte všechny vlastní MSI||X|X|X|Služby struktury můžete hostovat libovolnou spustitelný soubor jako [Host spustitelný](../service-fabric/service-fabric-deploy-existing-app.md) nebo nějakou aplikaci můžete nainstalovat na VMs.
Definování/spouštění po spuštění úkoly||X|X|X|
Můžete poslouchat události trasování událostí pro Windows||X|X|X|

## <a name="scenarios"></a>Scénáře a doporučení

Tady jsou některé běžné scénáře aplikace s doporučení, které Azure možnost pro hostování webů pravděpodobně nejvhodnější pro jednotlivá pole.

- [Potřebuji webových front-end s pozadí zpracování a databáze back-end spustit integrovaný s na místní prostředky obchodních aplikací.](#onprem)
- [Potřebujete spolehlivé způsob, jak hostovat svůj web podnikové, která upraví dobře a dosáhnout globální nabídky.](#corp)
- [Mám aplikace služby IIS 6 v systému Windows Server 2003.](#iis6)
- [Jsem vlastníkem malé firmy a je nutné levný způsob, jak hostovat svůj web, ale s budoucí nárůst paměti.](#smallbusiness)
- [Jsem na web nebo grafický Návrhář a chcete navrhování a vytváření webů jménem zákazníka.](#designer)
- [Aplikace vícevrstvého s webových front-end se mi migraci do cloudu.](#multitier)
- [Aplikace závisí na vysoce přizpůsobený Windows nebo Linux prostředí a můžu ji chcete přesunout do cloudu.](#custom)
- [Osobní web používá otevřít zdroj software a chcete ho hostovat v Azure.](#oss)
- [Mám řádku obchodní aplikací, kterou je potřeba se připojit k podnikové síti.](#lob)
- [Chci hostovat rozhraní REST API nebo webové služby pro mobilní klienty.](#mobile)


### <a id="onprem"></a>Potřebuji webových front-end s pozadí zpracování a databáze back-end spustit integrovaný s na místní prostředky obchodních aplikací.

Azure aplikaci služby je skvělý vyřešíte složité podnikových aplikací. Umožňuje můžete vyvíjet aplikace, které měřítko automaticky rovnováha platformy zatížení, jsou správně zapojené se službou Active Directory a připojení k místním zdrojům. Převede na Správa těmito aplikacemi snadno prostřednictvím celosvětovými portálu a rozhraní API a umožňuje získat přehled o jak zákazníci, kteří používají je s aplikací přehled nástroje. Funkce [Webjobs][] vám umožní spustit procesy na pozadí a úkoly v rámci webu osy, při hybridní připojení a funkce VNET usnadňují připojení zpátky do místního zdroje. Azure aplikaci služby nabízí tři 9 SLA pro webové aplikace a umožňuje:

* Spuštění aplikace problémy se spolehlivým na vlastní retušovacího automatické opravy cloudu platformě.
* Změna velikosti automaticky globální sítí datacentrech.
* Zálohování a obnovení havárie obnovení.
* Být kompatibilní se standardem ISO SOC2 a PCI.
* Integrace se službou Active Directory

### <a id="corp"></a>Můžu potřebují spolehlivé hostovat svůj web společnosti, která upraví dobře a dosáhnout globální nabídky.

Azure aplikace služba je skvělý řešení pro hostování podnikové weby. Umožňuje zobrazit rychle a snadno plnit služba globální sítí datacentrech webových aplikací web apps. Nabízí místní reach odolnost proti chybám a řízení inteligentní provozu. Všechny na platformě, která poskytuje prvotřídní správy nástroje což umožňuje získat přehled o stavu webů a webů přenosy rychle a snadno. Azure aplikaci služby nabízí tři 9 SLA pro webové aplikace a umožňuje:

* Spusťte vaše weby problémy se spolehlivým na vlastní retušovacího automatické opravy cloudu platformě.
* Změna velikosti automaticky globální sítí datacentrech.
* Zálohování a obnovení havárie obnovení.
* Správa protokoly a pomocí integrovaného nástroje.
* Být kompatibilní se standardem ISO SOC2 a PCI.
* Integrace se službou Active Directory

### <a id="iis6"></a>Mám aplikace služby IIS 6 v systému Windows Server 2003.

Azure aplikaci služby usnadňuje vyhnout infrastruktury náklady spojené s migraci starším aplikací služby IIS 6. Společnost Microsoft vytvořila [Nástroje pro migraci snadno se použije a migrace podrobné pokyny](https://www.movemetowebsites.net/) , které umožňují identifikovat žádné změny, které je potřeba provést a zkontrolovat kompatibilitu. Integrace se službou Visual Studiu, TFS a běžných nástrojů cm usnadňuje nasazení aplikace služby IIS 6 přímo do cloudu. Po zavedení, portálu Azur poskytuje robustní řídicí nástroje, které vám umožní Neomezovat ke správě nákladů a až splňují požadavek podle potřeby. Pomocí nástroje pro migraci máte tyto možnosti:

* Rychle a snadno migrujte starší verze webové aplikace Windows Server 2003 do cloudu.
* Určovat, jestli se opustit vaší připojených SQL databáze místní hybridní aplikaci.
* Automaticky přesouvat databázi SQL spolu s starší verze aplikace.

### <a id="smallbusiness"></a>Jsem vlastníkem malé firmy a je nutné levný způsob, jak hostovat svůj web, ale s budoucí nárůst paměti.

Azure aplikace služba je skvělý řešení pro tento scénář vzhledem k tomu, že ho začnete používat zdarma a potom přidejte další možnosti, když je potřebujete. Každý bezplatné web appu je součástí domény které Dal Azure (*your_company*. azurewebsites.net), včetně platformu integrované nástroje pro nasazení a správu, jakož i Galerie aplikace, která usnadňuje seznámení. Existuje mnoho jiných služeb a měřítka možnosti, které umožňují webu vyvíjet s služba zvýšená uživateli. Aplikace služby Azure máte tyto možnosti:

- Začněte s bezplatné osy a potom škálování podle potřeby.
- Pomocí Galerie aplikace rychle nastavit oblíbených webových aplikací, jako je WordPress.
- Podle potřeby přidejte další Azure služeb a funkcí aplikaci.
- Zabezpečený web app s HTTPS.

### <a id="designer"></a>Jsem na web nebo grafický Návrhář a chcete návrh a sestavení weby jménem zákazníka

Pro vývojáře webů a návrháři aplikaci služby Azure integruje snadno řadou rámce a nástroje podporuje nasazení libovolná a FTP a nabízí integraci s nástroji a služeb, jako je Visual Studia a databáze SQL. S aplikací služby máte tyto možnosti:

- Pomocí nástrojů příkazového řádku pro [Automatické úkoly][scripting].
- Práce s Oblíbené jazycích, třeba [.net][dotnet], [PHP][], [Node.js][nodejs]a [Python][].
- Vyberte tři různé úrovně měřítka pro změnu velikosti k velmi vysoké kapacity.
- Integrace s dalšími službami Azure, jako jsou [Databáze SQL][sqldatabase], [Služby Bus] [ servicebus] a [úložiště][]nebo partnera nabídka z [Azure úložiště][azurestore], například MySQL a MongoDB.
- Integrace s nástrojů, jako jsou Visual Studio libovolná, WebMatrix, WebDeploy, TFS a FTP.

### <a id="multitier"></a>Mám migraci aplikace vícevrstvého s webových front-end do cloudu

Pokud používáte aplikaci vícevrstvého, třeba na webový server, ke kterému je připojen k databázi, aplikace služby Azure je dobré možnost, která nabízí integraci s databáze SQL Azure. A použijete funkci WebJobs výpočetnímu back-end procesů.

Zvolte struktury služby pro jeden nebo více ze svého úrovní potřebujete-li větší kontrolu nad prostředí serveru, například možnost vzdálené do vašeho serveru nebo konfigurace serveru spuštění úkoly.

Pokud chcete použít vlastní obrázek počítače nebo po spuštění serverový software nebo služby, které nelze konfigurovat na struktury služby, zvolte virtuálních počítačích pro jeden nebo více ze svého úrovní.

### <a id="custom"></a>Aplikace závisí na vysoce přizpůsobený Windows nebo Linux prostředí a můžu ji chcete přesunout do cloudu.

Pokud aplikace vyžaduje složité instalace a konfigurace software a operační systém, je nejlepším řešením pravděpodobně virtuálních počítačích. Virtuálních počítačích můžete:

- Pomocí Galerie virtuálního počítače začít s operačním systémem, například Windows nebo Linux a pak ho upravit požadavky aplikace.
- Vytvořte a uložte vlastní obrázek stávajícího místního serveru ke spuštění v počítači virtuální v Azure.

### <a id="oss"></a>Osobní web používá otevřít zdroj software a chcete ho hostovat v Azure

Pokud otevřít zdroj framework je podporované v aplikaci služby, jazyky a rámce potřeby tak, že aplikace jsou nakonfigurované za vás automaticky. Služba aplikace umožňuje:

- Použití mnoho oblíbených otevřít zdroj jazyků, například [.NET][dotnet], [PHP][], [Node.js][nodejs]a [Python][].
- Nastavte si WordPress Drupal, Umbraco, DNN a mnoho jiných výrobců webových aplikací.
- Migrace existující aplikace nebo vytvořte nový účet z Galerie aplikace.

Když otevřít zdroj framework není podporovaný v aplikaci služby, můžete ho spusťte k některé z dalších Azure možnosti pro hostování webů. S virtuálních počítačích, nainstalujte a nakonfigurujte softwaru ve počítače obrázek, který může být Windows nebo Linux.

### <a id="lob"></a>Mám řádku obchodní aplikací, kterou je potřeba se připojit k podnikové síti

Pokud chcete vytvořit řádek obchodní aplikací, váš web může vyžadovat přímý přístup k služeb nebo dat v podnikové síti. To je možné aplikaci služby, služby struktury a virtuálních počítačích pomocí [služby Azure virtuální sítě](/services/virtual-network/). V aplikaci služby můžete použít [funkce integrace VNET](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), která umožňuje Azure aplikace jako kdyby to byl ve vaší podnikové síti.

### <a id="mobile"></a>Chci hostovat rozhraní REST API nebo webové služby pro mobilní klienty

Na základě HTTP webovým službám umožňují podporují širokou škálu klientů včetně mobilní klienty. Rámce jako rozhraní API webových ASP.NET integrace s Visual Studio usnadňuje vytváření a používání služba REST.  Tyto služby jsou přístupné z webu koncový bod, tak, aby bylo možné použít libovolnou postup na Azure pro hostování webů pro podporu tento scénář. Aplikace služby však se obzvlášť vyplatí pro hostování rozhraní REST API. S aplikací služby máte tyto možnosti:

- Rychle vytvořte [mobilní aplikace](../app-service-mobile/app-service-mobile-value-prop.md) nebo [rozhraní API aplikace](../app-service-api/app-service-api-apps-why-best-platform.md) hostovat webové služby protokolu HTTP v jednom z Azure na globálně distribuované datacentrech.
- Přenesení existujících služeb ani vytvářet nové.
- Dosažení SLA dostupnosti s jednou instancí nebo rozšiřování do více počítačů vyhrazené.
- Použijte web resetování publikované rozhraní REST API poskytovat HTTP klienty, včetně mobilní klienty.

> [AZURE.NOTE]
> Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet, přejděte na <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, které můžete okamžitě vytvořit v aplikaci krátkodobý starter v aplikaci služby Azure zdarma. Žádné platební kartou potřeby žádné závazky.

## <a id="nextsteps"></a>Další kroky

Další informace o tři webového hostingu možnosti najdete v tématu [Představení Azure](../fundamentals-introduction-to-azure.md).

Začít s možností, které zvolíte aplikace, najdete v následujících zdrojích:

* [Azure aplikace služby](/documentation/services/app-service/)
* [Služby Azure Cloud Services](/documentation/services/cloud-services/)
* [Azure virtuálních počítačích](/documentation/services/virtual-machines/)
* [Služba struktury](/documentation/services/service-fabric)

<!-- URL List -->

[Azure aplikace služby]: /services/app-service/
[Cloud Services]: http://go.microsoft.com/fwlink/?LinkId=306052
[Virtuálních počítačích]: http://go.microsoft.com/fwlink/?LinkID=306053
[Služba struktury]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[Úložiště]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
