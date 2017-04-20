# <a name="internet-of-things-security-from-the-ground-up"></a>Zabezpečení Internetu věcí od prvopočátku

Internet věcí (IoT) představuje jedinečné výzvy zabezpečení, ochrany osobních údajů a dodržování předpisů společnostem po celém světě. Na rozdíl od tradiční počítačové technologie, kde tyto problémy základem software a jak jsou implementovány IoT se týká, co se stane, když internetovým a fyzickém světě konvergovat. Ochrana IoT řešení vyžaduje zajištění zabezpečeného zřizování zařízení, zabezpečené připojení mezi tyto zařízení a cloudu a ochranu zabezpečení dat v cloudu během zpracování a skladování. Práce proti takové funkce, jsou však zařízení s omezenými zdroji, zeměpisné rozšíření nasazení a velký počet zařízení v rámci řešení.

Tento článek popisuje, jak Microsoft Azure IoT Suite poskytuje zabezpečené a soukromé cloud řešení Internetu věcí. Sady Azure IoT Suite nabízí kompletní řešení end-to-end zabezpečení integrované v každém stadiu od prvopočátku. Společnost Microsoft se vývoj softwaru pro zabezpečení je součástí praxe software engineering, zakořeněné v našem desetiletí dlouhé zkušenosti s vývoj softwaru pro zabezpečení. Zajistit, je Security Development Lifecycle (SDL) základní rozvoj metodiky, spolu s hostitelem zabezpečení na úrovni infrastruktury služby jako zajištění provozní zabezpečení (OSA) a digitální jednotkou zločinů Microsoft, Microsoft Security Response Center a Microsoft Malware Protection Center. 

Sadu Azure IoT Suite nabízí jedinečné funkce které značka zřizování, připojením a ukládání dat ze zařízení IoT jednoduché a průhledné a nejbezpečnější. V této knize jsme prozkoumat funkce zabezpečení Azure IoT Suite a hromadnému nasazení k zajištění zabezpečení, ochrany osobních údajů a soulad výzev popsaných. 

## <a name="introduction"></a>Úvod

Internet věcí (IoT) je vlna budoucnosti, nabízí okamžité firmy a reálné příležitosti snižovat náklady, zvyšovat výnosy a transformovat jejich podnikání. Mnoho podniků se však nebude svém bankovním spojení k nasazení IoT v jejich organizacích z důvodu obav týkajících se zabezpečení, ochrany osobních údajů a dodržování předpisů. Hlavní bod zájmu pochází z jedinečnosti infrastruktury IoT, která slučuje cyber a fyzickém světě společně úroková jednotlivá rizika plynoucí z těchto dvou světů. Zabezpečení IoT se vztahuje k zajištění integrity kódu spuštěného na zařízení, poskytování zařízení a ověření uživatele, definování jasné vlastnictví zařízení (stejně jako data generovaná zařízení) a jsou odolné k internetovým a fyzickými útoky. 

Potom je problém ochrany osobních údajů. Společnosti mají transparentnost týkající se shromažďování dat, jako v co nejsou shromažďovány a proč, kteří mohli zobrazit, kdo řídí přístup a tak dále. Nakonec jsou obecné bezpečnostní problémy a lidé jim provozní zařízení a problémy zachování průmyslové standardy dodržování předpisů.

Vzhledem zabezpečení, ochrany osobních údajů, průhlednost a jde o shodu, volba práva poskytovatele řešení IoT zůstává výzvou. Ve hřbetu dohromady jednotlivé kusy IoT software a služby poskytované různými dodavateli přináší mezery v zabezpečení, ochrany osobních údajů, transparentnosti a dodržování předpisů, které může být obtížné zjistit, let alone opravit. Volba pravého IoT software a poskytovatel metadat je založeno na vyhledávání zprostředkovatelů, které mají rozsáhlé zkušenosti se službami, které přes pocházejícími a zeměpisných, ale mohou také změnit velikost zabezpečeného a průhledným způsobem. Podobně pomáhá pro vybraného zprostředkovatele desetiletí zkušeností s rozvojovými zabezpečený software spuštěný v miliardách strojů po celém světě a mají schopnost oceňovat krajiny hrozba představovaná tento nový svět Internetu věcí.

## <a name="secure-infrastructure-from-the-ground-up"></a>Od prvopočátku zabezpečenou infrastrukturu 

Infrastruktura [Cloudu společnosti Microsoft](https://www.microsoft.com/enterprise/microsoftcloud/default.aspx#fbid=WzBsRQi6aGk) podporuje více než jednu miliardu zákazníků ve 127 zemích. Kreslení na naše zkušenosti dlouhé desetiletí vytváření podnikového softwaru a spouštění mezi největší online služby na světě, nabízíme vyšší úrovně rozšířeného zabezpečení, ochrany osobních údajů, dodržování předpisů a ohrožení postupy ke zmírnění než většina zákazníků by mohla dosáhnout na své vlastní.

Naše [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/) poskytuje povinné celopodnikové vývojový proces, který vloží požadavky na zabezpečení do softwaru celého životního cyklu. Chcete-li zajistit, aby provozní činnosti postupujte podle stejných postupů zabezpečení používáme přísné bezpečnostní pokyny podle našeho procesu zajištění provozní zabezpečení (OSA). Spolupracujeme také s firmami auditu třetích stran pro průběžné ověření, že jsme splňují naše dodržování povinností a můžeme zapojit do úsilí o důkladné zabezpečení prostřednictvím vytvoření Center excelence, včetně zločinů jednotka digitální Microsoft, Microsoft Security Response Center a Microsoft Malware Protection Center.

## <a name="microsoft-azure---secure-iot-infrastructure-for-your-business"></a>Microsoft Azure – zabezpečení IoT infrastrukturu pro vaše podnikání

Microsoft Azure nabízí kompletní cloud řešení, který kombinuje stále rostoucí kolekce integrovaný cloud služeb, analytics, strojovém učení, ukládání, zabezpečení, sítě a web – s své špičkové snahy o ochranu a soukromých údajů. Naše strategie [předpokládat porušení](https://azure.microsoft.com/blog/red-teaming-using-cutting-edge-threat-simulation-to-harden-the-microsoft-enterprise-cloud/) používá vyhrazený "červený tým" software zabezpečení odborníků, kteří simulovat útoky, možnost testování Azure ke zjišťování, ochranu před nově vznikajícími hrozbami a zotavit z porušení. Náš tým [globální incidentu](https://www.microsoft.com/TrustCenter/Security/DesignOpSecurity) po celý den pracuje s cílem zmírnit účinky útoky a škodlivým aktivitám. Tým takto stanovené postupy pro krizové řízení, komunikace a obnovy a používá rozhraní jasný a předvídatelný s interními a externími partnery.

Naše systémy poskytují kontinuální vniknutí a prevenci zabránění útoku služby, pronikání pravidelné testování a forenzní nástroje, které pomáhají identifikovat a zmírnění hrozeb. [Vícenásobné ověření](../articles/multi-factor-authentication/multi-factor-authentication.md) poskytuje další vrstvu zabezpečení pro koncové uživatele pro přístup k síti. A pro aplikace a hostitele zprostředkovatele nabízíme řízení přístupu, monitorování, anti-malware, chyba skenování, opravy a správa konfigurace.

Sada Microsoft Azure IoT Suite využívá zabezpečení a integrované do platformy Azure a naše SDL a OSA postupy pro vývoj bezpečných a fungování veškerého softwaru společnosti Microsoft o ochraně osobních údajů. Tyto postupy poskytují ochranu infrastruktury, ochrana sítě a identity a řízení funkce základní zabezpečení jakékoli řešení. 

[Rozbočovač pro IoT Azure](../articles/iot-hub/iot-hub-what-is-iot-hub.md) v rámci [IoT Suite](../articles/iot-suite/iot-suite-what-is-azure-iot.md) nabízí plně spravovaná služba, která umožňuje spolehlivou a zabezpečenou obousměrná komunikace mezi IoT zařízení a služby Azure jako [Azure Machine Learning](../articles/machine-learning/machine-learning-what-is-machine-learning.md) a [Azure Stream Analytics](../articles/stream-analytics/stream-analytics-introduction.md) pomocí licence vázané na zařízení pověření zabezpečení a řízení přístupu.  

Nejlepší komunikace zabezpečení a ochrany osobních údajů funkce integrována do sady Azure IoT Suite, jsme jste rozdělená do tří oblastí primární zabezpečení v sadě. 

![Azure IoT Suite](media/iot-security-ground-up/securing-iot-ground-up-fig2.png)

### <a name="secure-device-provisioning-and-authentication"></a>Zabezpečené ověřování a zřizování zařízení

Sadu Azure IoT Suite chrání zařízení, když jsou mimo pole tím, že poskytuje jedinečnou identitu klíč pro každé zařízení, který může sloužit ke komunikaci se zařízením během operace infrastruktura IoT. Proces je rychlý a snadný k instalaci. Vygenerovaný klíč s ID uživatele vybrané zařízení tvoří základ tokenu používán veškerou komunikaci mezi zařízením a rozbočovač IoT Azure.

ID zařízení lze přidružit k zařízení během výroby (tj. rozsvítilo v modulu hardwarového zabezpečení) nebo použít existující pevné identity jako server proxy (například sériová čísla procesoru). Vzhledem k tomu, že není jednoduché změny tento identifikační údaje zařízení, je důležité zavést ID logického zařízení v případě, že základní změny hardwaru zařízení, ale logické zařízení zůstává stejné. V některých případech může dojít přidružení zařízení identit v době nasazení zařízení (tedy inženýr ověřeného pole fyzicky nastaví nové zařízení při komunikaci s back-end řešení IoT). V [registru identit Azure IoT rozbočovač](../articles/iot-hub/iot-hub-devguide.md) poskytuje bezpečné úložiště identit zařízení a zabezpečení klíčů pro řešení. Jednotlivce nebo skupiny identit zařízení lze povolit seznam nebo seznam blokování umožňuje úplnou kontrolu nad přístupem k zařízení.
 
Zásady řízení přístupu Azure IoT rozbočovač v cloudu povolení aktivace a zákaz jakékoli identity zařízení poskytuje způsob, jak zrušit přidružení zařízení z nasazení IoT v případě potřeby. Toto přidružení a zrušení přidružení zařízení je založen na identitu každého zařízení.

Další zařízení bezpečnostní funkce zahrnují následující:

- Zařízení nepřijímá nevyžádané síťové připojení. Vytvoří všechna připojení a směrování odchozího způsobem. Zařízení pro příjem příkaz z back-end musí zařízení iniciovat připojení ke kontrole všechny příkazy čekající na zpracování. Po připojení mezi zařízením a IoT rozbočovač bezpečně usazen, zasílání zpráv z cloudu na zařízení a zařízení v cloudu lze odeslat transparentně.  
- Zařízení pouze připojit nebo vytvořit trasy na známé služby, které jsou peered, například rozbočovači Azure IoT.
- Ověřování a autorizaci na úrovni systému pomocí identity licence vázané na zařízení, vytváření pověření pro přístup a oprávnění u-okamžitě odvolatelný.

### <a name="secure-connectivity"></a>Zabezpečené připojení 

Životnost pro zasílání zpráv je důležitým aspektem jakéhokoli řešení IoT. Potřebu trvale doručit příkazy nebo přijímat data ze zařízení je podtržen skutečností, že IoT zařízení jsou připojena prostřednictvím Internetu nebo jiných podobných sítí, které mohou nespolehlivé. Azure IoT rozbočovač nabízí trvanlivost zasílání zpráv mezi cloud a zařízení prostřednictvím systému potvrzování v odpovědi na zprávy. Další trvanlivosti pro zasílání zpráv se dosáhne sedmi dnů pro telemetrické a dva dny pro příkazy ukládání do vyrovnávací paměti zpráv v rozbočovači IoT.
 
Účinnost je důležité zajistit zachování zdrojů a provoz v prostředí s omezenými zdroji. HTTPS (HTTP Secure), standardní zabezpečenou verzí protokolu http populární podporuje Azure IoT rozbočovač, umožňující efektivní komunikaci. Advanced Message Queuing Protocol (AMQP) a zprávy služby Řízení front zpráv telemetrické dopravy (MQTT), podporovaný rozbočovač IoT Azure, jsou určeny nejen pro účinnost z hlediska využívání zdrojů, ale také doručování zpráv spolehlivé. 

Škálovatelnost vyžaduje schopnost bezpečně spolupracovat s širokou škálu zařízení. Azure IoT rozbočovač umožňuje zabezpečené připojení k zařízení s podporou i bez IP povoleno. Zařízení s podporou mohou přímo připojit a komunikovat s rozbočovačem IoT pomocí zabezpečeného připojení. Povolen protokol IP není s omezenými zdroji a zařízení připojit pouze přes krátkou vzdálenost komunikačních protokolů, jako jsou Zwave, ZigBee a Bluetooth. Brána pole se používá k agregaci těchto zařízení a provádí překlad protokolu povolit zabezpečené obousměrné komunikaci s cloudu.

Připojení další funkce zabezpečení patří následující:

- Cesta komunikace mezi zařízeními a Azure IoT rozbočovač nebo mezi bránami a rozbočovač IoT Azure, je zabezpečena pomocí standardní zabezpečení TLS (Transport Layer) s Azure IoT rozbočovač ověřování pomocí protokolu X.509.
- Za účelem ochrany zařízení před nevyžádaná příchozí připojení, rozbočovač IoT Azure neotevře žádné připojení k zařízení. Zařízení zahájí všechna připojení. 
- Azure IoT rozbočovače trvale ukládá zprávy pro zařízení a zařízení pro připojení čeká. Tyto příkazy jsou uloženy dva dny povolení zařízení připojující sporadicky, z důvodu obavy o napájení nebo připojení, přijímat tyto příkazy. Azure IoT rozbočovač udržuje frontu na zařízení pro každé zařízení.

### <a name="secure-processing-and-storage-in-the-cloud"></a>Bezpečné zpracování a ukládání v cloudu 

Šifrovaná komunikace pro zpracování dat v cloudu sadu Azure IoT Suite pomůže zabezpečit data. Poskytuje možnost uplatňovat další šifrování a Správa klíčů zabezpečení. Pomocí Azure Active Directory (Poplašné) pro ověření a autorizace uživatele, Azure IoT Suite lze poskytnout na základě zásad autorizace model dat v cloudu, povolení správy snadný přístup, který může být auditovány a přezkoumány. Tento model umožňuje téměř okamžité odvolání přístupu k datům v cloudu a zařízení připojených k sadě IoT Azure.

Jakmile se data v cloudu, může zpracování a uloženy v všechny pracovní postupy definované uživatelem. Přístup pro každou část dat, je řízen pomocí služby Active Directory Azure v závislosti na použité úložiště služby.
   
Všechny klíče používaná infrastruktura IoT jsou uloženy v cloudu v zabezpečené úložiště, umožňující přejít v případě, že klíče musí být znovu vytvořena. Data mohou být uložena v [DocumentDB](../articles/documentdb/documentdb-introduction.md) nebo [databáze SQL](../articles/sql-database/sql-database-faq.md), povolení definice úroveň zabezpečení požadovanou. Kromě toho Azure poskytuje způsob, jak sledovat a auditovat všechny přístup k datům vás upozorní na jakékoli vniknutí nebo neoprávněnému přístupu.

## <a name="conclusion"></a>Závěr

Internet věcí začíná vaše věci – věci Většina firem. IoT přináší neuvěřitelné hodnoty obchodní nebo snížení nákladů, zvýšení výnosů a transformace podniku. Úspěch této transformace do značné míry závisí na výběru správné IoT software a poskytovatel metadat. To znamená, že hledání poskytovatele, který nejen catalyzes této transformace porozumění potřebám a požadavkům, ale také poskytuje služby a integrované zabezpečení, ochrany osobních údajů, transparentnosti a shody jako hlavní faktory ovlivňující použití softwaru. Společnost Microsoft má rozsáhlé zkušenosti s vývojem a nasazení zabezpečených softwaru a služeb a je stále vedoucí postavení v této nové věku Internetu věcí. 

Sada Microsoft Azure IoT Suite vytvoří v bezpečnostních opatření návrhu, povolení zabezpečení sledování majetku pro zlepšení efektivity provozní výkon jednotky povolit inovace a využívat pokročilé datové analytiky k transformaci firmy. S vrstvami přístup k zabezpečení, více funkcí zabezpečení a návrhové vzory Azure IoT Suite pomáhá zavedení infrastruktury, které lze důvěřovat transformace každého podnikání. 

## <a name="additional-information"></a>Další informace

Každý předkonfigurovaná řešení Azure IoT Suite vytvoří instance služeb Azure, jako je následující:

- [**Rozbočovač pro IoT Azure**](https://azure.microsoft.com/services/iot-hub/): Brána spojující cloud k "věci". Je možné přizpůsobit pro miliony připojení v rámci procesu a rozbočovač obrovské objemy dat podporující ověřování na zařízení pomáhá zabezpečit vaše řešení.
- [**Azure DocumentDB**](https://azure.microsoft.com/services/documentdb/): škálovatelné, plně indexované databáze služby částečně strukturovaných dat, která spravuje metadata pro zařízení je zřídit, jako jsou například atributy, konfigurace a vlastnosti zabezpečení. DocumentDB nabízí vysoký výkon a Vysoká propustnost zpracování, bez ohledu na schéma indexování dat a bohaté rozhraní SQL dotazu.
- [**Azure Stream Analytics**](https://azure.microsoft.com/services/stream-analytics/): v reálném čase proudu zpracování v cloudu, který umožňuje rychle vyvinout a nasadit řešení analytics levné odkrýt poznatky v reálném čase ze zařízení, senzorů, infrastruktury a aplikací. Data z tohoto plně spravované služby lze škálovat pro svazek při stále dosáhnout vysoké propustnosti, nízké latence a odolnost.
- [**Služby Azure App**](https://azure.microsoft.com/services/app-service/): cloud platform k vytvoření výkonné webové a mobilní aplikace, které se připojují k datům kdekoli; v cloudu nebo místně. Sestavení, vykonávající mobilní aplikací pro iOS, Android a Windows. Integraci se Software jako služba (SaaS) a aplikací v rozlehlé síti s připojením out-of-the-box na desítky cloudové služby a podnikových aplikací. Kód v Oblíbené jazykové a IDE – .NET, NodeJS, PHP, Python nebo Java – k vytvoření webové aplikace a rozhraní API rychleji než kdy dříve.
- [**Logic Apps**](https://azure.microsoft.com/services/app-service/logic/): funkce The Logic Apps služby Azure App Service pomáhá integrovat řešení IoT pro vaše stávající obchodní systémy a automatizovat pracovní postupy. Logic Apps umožňuje vývojářům návrhu pracovních postupů, které začíná od aktivační události a pak provést řadu kroků – pravidla a akce, které používají výkonné konektory pro integraci s podnikovými procesy. Logic Apps nabízí připojení out-of-the-box k obrovské ekosystému SaaS, založené na cloudu a místní aplikace.
- [**Azure objektů blob storage**](https://azure.microsoft.com/services/storage/): spolehlivé, hospodárné cloud úložiště pro data, která vaše zařízení odeslat do cloudu.

