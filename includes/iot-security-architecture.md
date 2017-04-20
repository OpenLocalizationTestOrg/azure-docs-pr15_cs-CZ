# <a name="internet-of-things-security-architecture"></a>Architektura zabezpečení Internetu věcí

Při návrhu systému, je důležité pochopit potenciální ohrožení tohoto systému a přidejte odpovídající obranu podle toho, jak je systém navržen a určen. Je zejména důležité navrhnout produkt od počátku s ohledem na bezpečnost, protože vysvětlení, jak může být útočník schopen napadnout počítač zvyšuje jistotu odpovídající skutečnosti snižující závažnost rizika jsou v platnosti od začátku. 

## <a name="security-starts-with-a-threat-model"></a>Začíná na modelu ohrožení zabezpečení
 
Microsoft dlouho používané modely hrozbu pro své výrobky a provedl veřejných k dispozici proces modelování ohrožení společnosti. Zkušenosti společnosti ukazuje, že modelování má mimo bezprostřední pochopení co ohrožení jsou nejvíce neočekávané výhody týkající se. Například vytvoří také avenue otevřená diskuse s ostatními uživateli mimo vývojový tým, což může vést k nové nápady a vylepšení v produktu.
  
Modelování ohrožení cílem je pochopit, jak může být útočník schopen napadnout počítač a pak ověřte, zda jsou příslušné skutečnosti snižující závažnost rizika. Nasazení sil modelování ohrožení týmu návrh zvážit skutečnosti snižující závažnost rizika, jako je systém určen spíše než po systému. Tuto skutečnost je životně důležité, protože dovybavení obranu zabezpečení do velkého počtu zařízení v poli je nemožné, náchylný a opustí zákazníci ohroženi.

Mnoho týmů vývoje dělat vynikající práci funkční požadavky na systém, které přináší zákazníkům výhody digitalizace. Upřesnění způsobů není zřejmé, že někdo může zneužít systém je však náročnější. Modelování ohrožení může pomoci pochopit, co může útočník provést vývojové týmy a proč. Modelování ohrožení je strukturovaný proces, který vytvoří návrh rozhodnutí v diskusi o zabezpečení v systému, jakož i změny návrhu, které jsou na cestě k zabezpečení tohoto vlivu. Jednoduše dokumentu při ohrožení modelu této dokumentace také představuje ideální způsob k zajištění kontinuity znalostí, retenční lekcí se dozvěděli a nápovědy nového týmu palubě rychle. Nakonec je povolit, je třeba zvážit další aspekty zabezpečení, například jaké zabezpečení závazků, které chcete poskytnout zákazníkům výsledku modelování ohrožení. Tyto závazky v souvislosti s hrozbou modelování bude informovat a jednotka testování vašeho řešení Internetu věcí (IoT).
 

### <a name="when-to-threat-model"></a>Pokud model ochrany proti novým ohrožením

[Modelování ohrožení](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) nabízí nejvyšší hodnotu, pokud je začleněna do fáze návrhu. Při navrhování, máte nejvyšší flexibilitu provádět změny odstranit. Odstranění hrozby podle návrhu je požadovaný výsledek. Je mnohem jednodušší než přidání skutečnosti snižující závažnost rizika, jejich testování a zajištění zůstávají aktuální a navíc toto odstranění není vždy možné. Je stále těžší udržet odstranit produkt stane zralejší a zase bude nakonec vyžadují více práce a poměry mnohem těžší než hrozba modelování počáteční fázi ve vývoji.

### <a name="what-to-threat-model"></a>Co do modelu ohrožení

Má vlákna model řešení jako celek a také zaměřit v následujících oblastech:

- Funkce zabezpečení a ochrany osobních údajů
- Funkce, jejichž selhání jsou důležité zabezpečení
- Funkce, které se dotýkají hranice důvěry 

### <a name="who-threat-models"></a>Kdo ochrany proti novým ohrožením modely

Modelování ohrožení je stejně jako jakýkoli jiný proces.  Je vhodné považovat hrozba vzor dokumentu jako součást řešení a ověřit. Mnoho týmů vývoje dělat vynikající práci funkční požadavky na systém, které přináší zákazníkům výhody digitalizace. Upřesnění způsobů není zřejmé, že někdo může zneužít systém je však náročnější. Modelování ohrožení může pomoci pochopit, co může útočník provést vývojové týmy a proč.

### <a name="how-to-threat-model"></a>Jak model ohrožení

Proces modelování ohrožení se skládá ze čtyř kroků; Tyto kroky jsou:

- Model aplikace
- Výčet hrozeb
- Zmírnění hrozeb
- Ověření skutečnosti snižující závažnost rizika

#### <a name="the-process-steps"></a>Kroky procesu

Tři zásady třeba vzít v úvahu při vytváření modelu ohrožení:

1. Vytvoření diagramu z referenční architektury. 
2. Šířka první Start. Získat přehled a pochopit systém jako celek, před potápěním hluboko.  To pomáhá zajistit, že můžete nabídnout detailní na správných místech.
3. Řídit proces, nenechte drive je proces. Pokud najít problém ve fázi modelování a vyzkoušet ji, jít pro něj!  Není cítit, je třeba provést následující kroky slavishly.  

#### <a name="threats"></a>Ohrožení

Čtyři základní prvky modelu ohrožení jsou:

- Procesy (webové služby, služby Win32 * nix procesy daemon, atd. Všimněte si, že některé složité subjekty (např. pole brány a snímače) lze abstrakci procesu při technickým podrobnostem v těchto oblastech není možné.
- Data ukládá (všude, kde jsou uložena data, například databáze nebo konfigurační soubor)
- Tok dat (Pokud data přesunuta mezi jinými prvky aplikace)
- Externí entity (cokoli, který spolupracuje se systémem, ale není pod kontrolou aplikace, příklady zahrnují uživatele a satelitní kanály)

Všechny elementy v diagramu architektury se vztahují různé hrozby; použijeme STRIDE symbolické. Číst [Ohrožení modelování opět mezery](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) o ROZTEČ prvků.

Různých prvků diagramu aplikace podléhají některé hrozby ROZTEČ:

- Procesy se řídí STRIDE
- Datové toky jsou předmětem TID
- Úložiště dat podléhají TID a někdy R, pokud úložiště dat jsou soubory protokolu.
- Externí entity se SRD

## <a name="security-in-iot"></a>Zabezpečení IoT

Připojená zařízení speciální mají značný počet ploch potenciální interakce a interakce vzory, které je nutno poskytnout rámec pro zabezpečení digitální přístup k těmto zařízením. Pojem "Digitální přístup" se zde používá k odlišení od jakékoli operace, které jsou prováděny prostřednictvím přímé zařízení interakce kde přístup zabezpečení je zajištěno prostřednictvím řízení fyzického přístupu. Například uvedení zařízení do místnosti s zámek na dveřích. Při použití softwaru a hardwaru, nemůže být odepřeno fyzický přístup, můžete zabránit fyzický přístup vedoucí k rušení systému přijata opatření. 

Jak jsme prozkoumat interakce vzory, se podíváme na "řídící" a "datové zařízení" se stejnou úrovní pozornost. "Řídící" mohou být klasifikovány jako jakékoli informace, které je k dispozici na zařízení stranou s cílem změny nebo ovlivňující jeho chování vůči stavu nebo stavu jeho prostředí. "Datové zařízení" mohou být klasifikovány jako jakékoli informace, které zařízení vysílá do druhé smluvní strany o jeho stavu a pozorovaných stát jeho prostředí.
   
Pokud chcete optimalizovat zabezpečení – doporučené postupy, je vhodné rozdělit typické architektury IoT do několika komponenty/zón v rámci výkonu modelování ohrožení. Tyto zóny jsou plně popsány v této části a zahrnují:

-   Zařízení,
-   Pole brána
-   V cloudu brány, a
-   Služby.

Zóny jsou široké až segmentu řešení; Každá zóna má často své vlastní požadavky dat a ověřování a autorizace. Zóny můžete také použít k poškození izolace a omezit dopad nízké zabezpečení zón na vyšší důvěryhodnosti zóny.

Jednotlivé zóny jsou odděleny hranice vztahu důvěryhodnosti, která bude zaznamenána jako Tečkovaná červená čára na obrázku níže. Představuje přechod údaje/informace z jednoho zdroje na druhý. Během tohoto přechodu může být dat informace o Spoofing, Tamperingem, popření původu, přístup k informacím, odmítnutí služby a zvýšení úrovně oprávnění (STRIDE).

![Zóny zabezpečení IoT](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Součásti použité v ukázkách v rámci každé hranice také podroben STRIDE, umožňující úplné 360 zobrazení řešení modelování ohrožení. Následující části obsahují dále rozvíjejí každý z komponenty a obavy o zabezpečení a řešení, která mají být vloženy do místa.

Oddíly, které následuje popisuje standardní součásti obvykle najít v těchto zónách.

### <a name="the-device-zone"></a>Zařízení zóny

Zařízení prostředí je okamžité fyzické místo kolem zařízení fyzického přístupu a/nebo "místní síť" peer-to-peer programu Digitální přístup k zařízení je možné. Síť, která jsou odlišné a tepelně izolovaného od – ale potenciálně přidat do mostu k – veřejného Internetu a zahrnuje jakékoli technologie krátkého dosahu bezdrátového vysílače, který umožňuje komunikaci peer-to-peer zařízení považován za "Místní síť". Ano *Ne* zahrnovat jakékoli technologie virtualizace sítě vytvoření iluze místní sítě a také neobsahuje veřejný operátor sítě, které vyžadují žádné dvě zařízení pro komunikaci v prostoru veřejné sítě, kdyby vstupovat do vztahu komunikaci peer-to-peer.

### <a name="the-field-gateway-zone"></a>Pole brány zóna

Pole brána je zařízení/zařízení nebo software některých serverů univerzální počítač, který funguje jako aktivátor komunikace a i jako zařízení kontrolní systém a centrální zpracování dat zařízení. Brána zóny pole obsahuje pole brána sama a všech zařízení, která jsou k němu připojené. Jak již název napovídá, pole brány jednat mimo vyhrazené výpočetní zařízení, jsou obvykle vázány umístění, potenciálně podléhají fyzické vniknutí a bude mít omezenou funkční redundance. Vše do říkají, že je brána pole obvykle věc jeden touch a sabotáž, přičemž budete vědět, co je jeho funkce. 

Brána pole se liší od pouhé provoz směrovač měl aktivní roli při řízení přístupu a toku informací, což znamená, že je žádost adresována entity a síťové připojení nebo relace Terminálové. Zařízení NAT nebo brány firewall, naopak se nekvalifikují jako brány pole vzhledem k tomu, že se nejedná o explicitní připojení nebo relace terminály, ale spíše trasu (nebo bloku) připojení nebo prostřednictvím jejich relace. Brána pole má dvě odlišné oblasti povrchu. Jedno zařízení, které jsou připojeny k němu směřuje a představuje vnitřní zóny a ostatní čelí všechny vnější strany a okraji zóny.   

### <a name="the-cloud-gateway-zone"></a>Shluk brány zóna

Shluk brány je systém, který umožňuje vzdálenou komunikaci od a do zařízení a brány pole z několika různých serverů přes veřejné sítě místa, obvykle k cloudové řízení a systém analýzy dat, federace těchto systémů. V některých případech brány cloud může okamžitě usnadnění přístupu pro speciální zařízení z terminálů jako tablety nebo telefony. V souvislosti se zde popsané "cloud" je míněny vyhrazené zpracování dat systému, která není vázána na stejném webu jako připojené zařízení nebo pole brány. V oblasti Cloud také operativní opatření zabránit cílené fyzický přístup a není nutně vystaveni infrastruktury "veřejný cloud".  

Shluk brány může potenciálně mapovány do překrytí virtualizace sítě izolovat shluk brány a všech připojených zařízení nebo pole bránami z dalších síťových přenosů. Shluk brány, sama se ani kontrolní systém zařízení ani zpracování nebo zařízení úložiště pro data zařízení; zařízení rozhraní pomocí cloudu brány. Shluk brány zóna obsahuje brány cloud všechny brány pole a zařízení k němu připojené přímo nebo nepřímo. Okraji zóny je odlišné povrchové plochy, kde všechny vnější strany komunikovat prostřednictvím.

### <a name="the-services-zone"></a>Oblasti služeb

"Služba" je definován pro tento kontext jako všechny součásti softwaru nebo modul, který je propojení se zařízeními pomocí pole nebo shluk brány pro sběr dat a analýzy, jakož i pro příkazy a ovládání.  Služby jsou zprostředkovatelé. Se jednat podle jejich identity, brány a jiné podsystémy, ukládat a analyzovat data, samostatně vydat příkazy zařízení na základě dat poznatky nebo plány a vystavit informace a řídit možnosti oprávněným koncovým uživatelům.

### <a name="information-devices-vs-special-purpose-devices"></a>Informace o zařízení a speciální zařízení

Počítače, telefony a tablety jsou především informace o interaktivní zařízení. Telefony a tablety jsou explicitně optimalizováno maximalizuje životnost baterie. Jsou pokud možno vypnout částečně není okamžitě interakci s osobou nebo pokud není poskytování služeb, jako je přehrávání hudby nebo vedení jejich majitel na určité místo. Z hlediska systémů tyto informace technologie zařízení působí hlavně jako servery proxy směrem k uživateli. Jsou to "lidé válcích" návrh akce a "lidé snímače" shromažďováním vstupu. 

Speciální zařízení z jednoduché Tepelné senzory pro složité výroby výrobních linek s tisíci komponenty uvnitř, se liší. Tato zařízení mají mnohem větší rozsah v účelu a to i v případě, že poskytují některé uživatelské rozhraní, jsou z velké části zaměřen propojení s nebo integrovat do majetku ve fyzickém světě. Jejich měření a zprávu environmentálních podmínek, zapnutí ventilů, řídit servos, zvukové signály, přepínání světel a provádět mnoho dalších úkolů. Práce pomáhají, u kterých informace zařízení je příliš obecné, příliš drahé, příliš velký nebo příliš křehká. Jejich technické provedení konkrétní účel okamžitě určí jako dobře dostupné měnové rozpočet pro jejich výrobu a provoz plánované životnosti. Kombinace těchto dvou klíčových faktorů omezuje dostupné provozním rozpočtu energie, fyzické nároky na paměť a tedy k dispozici úložiště, výpočetní a zabezpečovací funkce.  

Pokud něco "přejde nesprávné" automatizované nebo vzdálené kontrolovatelné zařízeními, například fyzické vady nebo řídicí logiky vady úmyslného neoprávněné vniknutí a manipulace. Výrobní šarže mohou být zničeny budovy mohou být looted nebo vypálit a lidé mohou být poškozené nebo dokonce die. To je samozřejmě zcela odlišné třídy poškození než někdo maxing mimo limit ukradené kreditní karty. Na panelu zabezpečení zařízení přesunout věci a pro data snímače, který nakonec vede k příkazy, které způsobují věci, které chcete přesunout, musí být vyšší než v e-commerce nebo použití v bankovnictví. 


### <a name="device-control-and-device-data-interactions"></a>Ovládání zařízení a zařízení data interakce

Připojená zařízení speciální mají značný počet ploch potenciální interakce a interakce vzory, které je nutno poskytnout rámec pro zabezpečení digitální přístup k těmto zařízením. Pojem "Digitální přístup" se zde používá k odlišení od jakékoli operace, které jsou prováděny prostřednictvím přímé zařízení interakce kde přístup zabezpečení je zajištěno prostřednictvím řízení fyzického přístupu. Například uvedení zařízení do místnosti s zámek na dveřích. Při použití softwaru a hardwaru, nemůže být odepřeno fyzický přístup, můžete zabránit fyzický přístup vedoucí k rušení systému přijata opatření. 
 
Jak jsme prozkoumat interakce vzory, se podíváme na "řídící" a "datové zařízení" se stejnou úrovní pozornost při modelování ohrožení. "Řídící" mohou být klasifikovány jako jakékoli informace, které je k dispozici na zařízení stranou s cílem změny nebo ovlivňující jeho chování vůči stavu nebo stavu jeho prostředí. "Datové zařízení" mohou být klasifikovány jako jakékoli informace, které zařízení vysílá do druhé smluvní strany o jeho stavu a pozorovaných stát jeho prostředí. 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Referenční architektura Azure IoT modelování ohrožení

Společnost Microsoft používá rámec výše uvedených do ochrany proti novým ohrožením modelování pro Azure IoT. V části níže používáme proto konkrétní příklad architektury Azure IoT odkaz k prokázání toho, jak přemýšlet o ohrožení modelování pro IoT a jak řešit hrozby identifikovány. V našem případě můžeme identifikovat čtyři hlavní oblasti zaměření:

-   Zařízení a zdroje dat
-   Přenosem dat
-   Zařízení a zpracování události a
-   Prezentace

![Hrozba modelování pro Azure IoT](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Následující diagram poskytuje zjednodušené zobrazení architektury IoT společnosti Microsoft pomocí modelu diagramu toku dat, který používá nástroj pro modelování ohrožení systému Microsoft:

![Pro IoT Azure pomocí nástroje Modelování ohrožení MS modelování ohrožení](media/iot-security-architecture/iot-security-architecture-fig3.png)

Je důležité si uvědomit, že architektura odděluje funkce zařízení a brány. To umožňuje uživateli využívat zařízení brány, jež jsou bezpečnější: jsou schopny komunikovat s cloud bránu pomocí zabezpečených protokolů, která obvykle vyžaduje větší zátěže, že nativní zařízení - například termostat - může poskytnout vlastní. V oblasti služeb Azure předpokládáme brány Cloud je reprezentován služby Azure IoT rozbočovač.

### <a name="device-and-data-sourcesdata-transport"></a>Zařízení a zdrojů/datový přenos dat

Tato část popisuje architekturu výše uvedených rozptylovým sklem modelování ohrožení a poskytuje přehled jak některé podstatné zájmy můžeme dosáhnout. Můžeme se soustředí na základní prvky modelu ohrožení:

- Procesy (ty podle našeho ovládacího prvku a externí položky)
- Komunikace (nazývané také toky dat)
- Úložiště (také nazývané úložiště dat)

#### <a name="processes"></a>Procesy

V každé z kategorií uvedených v architektura Azure IoT, pokusíme se snížit počet různých ohrožení přes data/informace existuje v různých fázích: proces komunikace a úložiště. Dále nabízíme přehled nejobvyklejší z nich pro kategorii "proces", následuje přehled jak to může být nejlépe zmírnit: 

**Falšování identity (S)**: útočník může extrahovat kryptografické klíče materiálu ze zařízení na úrovni hardwaru nebo softwaru, a následně byly převzaty přístup pomocí systému s identitou jiné fyzické nebo virtuální zařízení zařízení materiálu klíče z. Dobrou ilustraci je dálkové ovládání, které můžete zapnout všechny televizní a jsou populární prankster nástroje.

**Odmítnutí služby (D)**: zařízení lze vykreslit nemůže fungovat nebo sdělování zasahování rádiových frekvencí nebo řezání drátů. Například dozor fotoaparát, který měl jeho napájení nebo síťové připojení záměrně vykrojí nebude dat sestavy, vůbec.

**Falšování (T)**: útočník může zcela nebo zčásti nahradit softwaru spuštěné v zařízení, což může nahrazené softwaru využívat pravé identity zařízení, pokud materiál klíče nebo kryptografických zařízení obsahující klíčové materiály byly nedovolené programu. Útočník může například využít extrahované materiál klíče pro zachycení a potlačit data ze zařízení na cesty pro komunikaci a nahraďte chybné údaje, které jsou ověřeny pomocí ukradeného materiálu klíče.

**Přístup k informacím (I)**: Pokud zařízení běží software s ní manipulováno, takový zpracovatelné software může potenciálně dochází k úniku dat neoprávněnými osobami. Útočník může například využít extrahované materiál klíče k samotné se vstříkne do komunikační cesta mezi zařízení a řadiče nebo pole brány nebo brány cloud siphon vypnout informace.

**Zvýšení úrovně oprávnění (E)**: zařízení, které provádí specifické funkce může zapříčinit udělat něco jiného. Například může být ventil, který je naprogramován tak, aby polovina způsob otevření svých úplně otevřít.

| **Součást** | **Ohrožení** | **Ke zmírnění**                                                                                                                                                | **Riziko**                                                                                                                                                                                                    | **Implementace**                                                                                                                                                                                                                                                                                                                                     |
|---------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zařízení        | S          | Přiřadit zařízení identity a ověřování zařízení                                                                                                | Nahrazení zařízení nebo část zařízení s jiným zařízením. Jak jsme si vědomi, že jsme mluvení do správné zařízení?                                                                                           | Ověřování zařízení, použitím zabezpečení TLS (Transport Layer) nebo protokol IPSec. Infrastruktury by měly podporovat používání předsdíleného klíče (PSK) v těchto zařízeních, která nemůže zpracovat zcela asymetrické šifrování. Využití Azure AD [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt)                             |
|               | TRID       | Například použít mechanismy tamperproof zařízení tím, že velmi náročná na možné extrahovat klíče a jiného kryptografického materiálu ze zařízení. | Rizikem je, pokud někdo je falšování zařízení (fyzické rušení). Jak jsme se, zda nebyl zfalšován zařízení.                                                                                 | Nejúčinnější řešení je schopnost důvěryhodné platform module (TPM), umožňující ukládání klíčů do speciální obvody-chip ze kterého klíče nelze číst, ale lze použít pouze pro kryptografické operace, které používá, ale nikdy zveřejnit klíče. Šifrování paměti zařízení. Správa klíčů pro zařízení. Podepisování kódu. |
|               | E          | S řízení přístupu k zařízení. Schéma ověřování.                                                                                                    | Když zařízení umožňuje jednotlivé akce mají být provedeny podle příkazů z vnějšího zdroje nebo dokonce napaden senzory, umožňuje útok k provedení operace není jinak přístupný. | Schéma ověřování pro zařízení s                                                                                                                                                                                                                                                                                                             |
| Pole brána | S          | Ověření pole brána do cloudu brány (cert založena, PSK, námitka založena,..)                                                                           | Pokud někdo může nahradit falešným pole brána, pak ji může zobrazit jako jakékoli zařízení.                                                                                                                               | RSA/PSK, IPSe, TLS [RFC 4279](https://tools.ietf.org/html/rfc4279). Všechny klíče úložiště a potvrzení se týká zařízení obecně – nejlepší varianta je pomocí čipu TPM. 6LowPAN rozšíření protokolu IPSec podporují bezdrátové snímače sítí (WSN).                                                                                                              |
|               | TRID       | Ochrana Gateway pole proti manipulaci (TPM)?                                                                                                            | Falšování útoky, které přimět přemýšlení brány cloud, který je mluvení do pole brána může vést k informacím a falšováním dat                                                             | Paměť šifrování TPM společnosti, ověření.                                                                                                                                                                                                                                                                                                              |
|               | E          | Mechanismem řízení přístupu pro pole brána                                                                                                                    |                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                        |

Zde jsou některé příklady hrozby v této kategorii:

Podvodná identifikace: Útočník může extrahovat kryptografické klíče materiálu ze zařízení buď na software nebo hardware úrovni a následně přístup přijatých pomocí systému s identitou jiné fyzické nebo virtuální zařízení zařízení materiálu klíče z.

**Odmítnutí služby**: lze vykreslit zařízení nemůže fungovat nebo sdělování zasahování rádiových frekvencí nebo řezání drátů. Například dozor fotoaparát, který měl jeho napájení nebo síťové připojení záměrně vykrojí nebude dat sestavy, vůbec.

**Tamperingem**: útočník může zcela nebo zčásti nahradit softwaru spuštěné v zařízení, což může nahrazené softwaru využívat pravé identity zařízení, pokud materiál klíče nebo kryptografických zařízení obsahující klíčové materiály byly nedovolené programu.

**Tamperingem**: dozor fotoaparát, který je zobrazen obrázek viditelného spektra prázdné hallway by zaměřené na fotografie těchto hallway. Snímač kouře nebo požáru může hlášení někdo drží zapalovač pod ním. V obou případech zařízení může být technicky směrem k systému plně důvěryhodný, ale hlásí informace zpracovatelné.

**Tamperingem**: útočník může využít extrahované materiál klíče pro zachycení a potlačit data ze zařízení na cesty pro komunikaci a nahraďte chybné údaje, které jsou ověřeny pomocí ukradeného materiálu klíče.

**Tamperingem**: útočník může částečně nebo úplně nahradit softwaru spuštěné v zařízení, což může nahrazené softwaru využívat pravé identity zařízení, pokud materiál klíče nebo kryptografických zařízení obsahující klíčové materiály byly nedovolené programu.
   
**Zpřístupnění informací**: Pokud zařízení běží software s ní manipulováno, takový zpracovatelné software může potenciálně dochází k úniku dat neoprávněnými osobami.

**Přístup k informacím**: útočník může využít extrahované materiálu klíče, která sám vloží do cesty komunikace mezi zařízením a řadič nebo pole brány nebo brány cloud siphon vypnout informace.

**Odmítnutí služby**: zařízení lze vypnout nebo do režimu zapnuto, pokud komunikace není možná (což je úmyslně v mnoha průmyslových počítačů).

**Tamperingem**: zařízení lze překonfigurovat pracovat ve stavu neznámé k systému řízení (mimo známých kalibračních parametrů) a poskytnout tak data, která může být špatně interpretován

**Zvýšení úrovně oprávnění**: zařízení, které provádí specifické funkce může zapříčinit udělat něco jiného. Například může být ventil, který je naprogramován tak, aby polovina způsob otevření svých úplně otevřít.

**Odmítnutí služby**: zařízení lze zapnout do státu, kde není možná komunikace.

**Tamperingem**: zařízení lze překonfigurovat pracovat ve stavu neznámé k systému řízení (mimo známých kalibračních parametrů) a poskytnout tak data, která může být špatně interpretován.
 
**Spoofing, Tamperingem/odmítnutí**: Pokud není zabezpečen (což je zřídka případ spotřebitele dálková ovládání) Útočník může manipulovat s stav zařízení anonymně. Dobrou ilustraci je dálkové ovládání, které můžete zapnout všechny televizní a jsou populární prankster nástroje.

#### <a name="communication"></a>Komunikace

Ohrožení kolem cesty komunikace mezi zařízeními, zařízeními a pole brány a brány zařízení a cloudu. Následující tabulka obsahuje některé pokyny kolem otevřít sokety na zařízení sítě VPN:

| **Součást**               | **Ohrožení** | **Ke zmírnění**                                      | **Riziko**                                                                                                      | **Implementace**                                                                                                                                                                                                                                                                                                                                                               |
|-----------------------------|------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Rozbočovač zařízení IoT              | TID        | (D) TLS (PSK/RSA) pro šifrování přenosů             | Odposlouchávání nebo rušení komunikace mezi zařízením a brány                             | Zabezpečení na úrovni protokolu. S vlastní protokoly potřebujeme zjistit, jak je chránit. Ve většině případů komunikace probíhá od zařízení k rozbočovači IoT (připojení iniciuje zařízení).                                                                                                                                                                 |
| Zařízení, zařízení               | TID        | (D) TLS (PSK/RSA) pro šifrování přenosů.            | Čtení dat při přenosu mezi zařízeními. Manipulaci s daty. Přetížení zařízení se nová připojení | Zabezpečení na úrovni protokolu (MQTT/AMQP/HTTP/CoAP. S vlastní protokoly potřebujeme zjistit, jak je chránit. Snížení rizika pro hrozbu DoS je druhé strany zařízení prostřednictvím brány cloud nebo pole a jejich jediným aktem jako klienti směrem k síti. Prozkoumávání může mít za následek přímé připojení mezi partnery za bránou s byla brokered |
| Externí entita zařízení      | TID        | Silné párování zařízení externí entity | Odposlouchávání připojení k zařízení. Rušení komunikace se zařízením                     | Externí entita NKS/Bluetooth LE zařízení bezpečně párování. Řízení panelu provozní zařízení (fyzické)                                                                                                                                                                                                                                                  |
| Pole brána shluk brány | TID        | TLS (PSK/RSA) pro šifrování přenosů.               | Odposlouchávání nebo rušení komunikace mezi zařízením a brány                             | Zabezpečení na úrovni protokolu (MQTT/AMQP/HTTP/CoAP). S vlastní protokoly potřebujeme zjistit, jak je chránit.                                                                                                                                                                                                                                                       |
| Zařízení brány Cloud        | TID        | TLS (PSK/RSA) pro šifrování přenosů.               | Odposlouchávání nebo rušení komunikace mezi zařízením a brány                             | Zabezpečení na úrovni protokolu (MQTT/AMQP/HTTP/CoAP). S vlastní protokoly potřebujeme zjistit, jak je chránit.                                                                                                                                                                                                                                                       |

Zde jsou některé příklady hrozby v této kategorii:

**Odmítnutí služby**: omezené zařízení jsou obecně ohrožené DoS při jejich aktivně naslouchat příchozí připojení nebo nevyžádaných datagramy v síti, protože útočník může otevřít mnoho připojení současně a jejich služby ani služby je velmi pomalu, nebo zařízení může být zaplaven nevyžádané přenosy. V obou případech zařízení může skutečně být nefunkčnost v síti.

**Spoofing, přístup k informacím**: jeden pro všechny zabezpečení zařízení jako je heslo nebo PIN protection mají často omezené a speciální zařízení nebo zcela spoléhají na důvěřování sítě, což znamená bude udělit přístup k informacím, když je zařízení ve stejné síti a sítě je často pouze sdílený klíč chráněn. To znamená, že pokud je sdělí sdílený tajný klíč k zařízení nebo k síti, je možné zařízení řídit nebo sledovat data vyzařovaného ze zařízení.  

**Spoofing**: útočník může zachytit nebo částečně potlačit vysílání a falešná identita původce (muž uprostřed)

**Tamperingem**: útočník může zachytit nebo částečně potlačit vysílání a odeslat nepravdivé informace 

**Přístup k informacím:** útočník může odposlouchávat vysílání a získat informace bez souhlasu **odmítnutí služby:** útočník může zablokování vysílání signálu a distribuci informace odepřít

#### <a name="storage"></a>Úložiště

Každé zařízení a pole brána má určitou formu úložiště (dočasné služby Řízení front data, úložiště bitové kopie operačního systému).

| **Součást**                            | **Ohrožení** | **Ke zmírnění**                       | **Riziko**                                                                                                                                                                                                                                                                                                                | **Implementace**                                                                                                                                                     |
|------------------------------------------|------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zařízení úložiště                           | TRID       | Skladování, šifrování, podepisování protokoly | Čtení dat z úložiště (PII data), manipulaci s daty telemetrické. Manipulace s ve frontě nebo příkazu ovládací prvek dat v mezipaměti. Manipulace s balíčky aktualizace konfigurace nebo firmware v režimu s mezipamětí nebo místně do fronty, může vést k součásti operačního systému nebo systém bude ohrožen                                         | Šifrování kódu ověřovací zprávy (MAC) nebo digitální podpis. Pokud řízení přístupu k možné, silné prostřednictvím přístupu k prostředkům řízení seznamy (ACL) nebo oprávnění. |
| Bitové kopie OS zařízení                          | TRID       |                                      | Manipulace s OS / nahrazení komponent OS                                                                                                                                                                                                                                                                         | Jen pro čtení oddíl OS podepsané bitové kopie operačního systému, šifrování                                                                                                                    |
| Brána pole úložiště (queuing data) | TRID       | Skladování, šifrování, podepisování protokoly | Čtení dat z úložiště (PII data), manipulaci s daty telemetrické falšování ve frontě nebo příkazu ovládací prvek dat v mezipaměti. Manipulace s balíčky aktualizace konfigurace nebo firmware (určeno pro zařízení nebo pole brána) v režimu s mezipamětí nebo místně do fronty, může vést k součásti operačního systému nebo systém bude ohrožen | Nástroj BitLocker                                                                                                                                                              |
| Obrázek pole brána OS                   | TRID       |                                      | Manipulace s OS / nahrazení komponent OS                                                                                                                                                                                                                                                                          | Jen pro čtení oddíl OS podepsané bitové kopie operačního systému, šifrování                                                                                                                    |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Zařízení a událost zpracování/shluk brány zóna

Shluk brány je systém, který umožňuje vzdálenou komunikaci od a do zařízení a brány pole z několika různých serverů přes veřejné sítě místa, obvykle k cloudové řízení a systém analýzy dat, federace těchto systémů. V některých případech brány cloud může okamžitě usnadnění přístupu pro speciální zařízení z terminálů jako tablety nebo telefony. V souvislosti se zde popsané "cloud" je míněny vyhrazené zpracování dat systému, která není vázána na stejném webu jako připojené zařízení nebo pole, brány a kde provozní opatření zabrání cílová fyzický přístup, ale není nutně "veřejný cloud" infrastruktury.  Shluk brány může potenciálně mapovány do překrytí virtualizace sítě izolovat shluk brány a všech připojených zařízení nebo pole bránami z dalších síťových přenosů. Shluk brány, sama se ani kontrolní systém zařízení ani zpracování nebo zařízení úložiště pro data zařízení; zařízení rozhraní pomocí cloudu brány. Shluk brány zóna obsahuje brány cloud všechny brány pole a zařízení k němu připojené přímo nebo nepřímo.

Shluk brány je převážně vlastní sestavený kus softwaru spuštěna jako služba s exponované koncové body, které pole brána a zařízení připojit. Jako takový musí být navrženy s ohledem na bezpečnost. Postupujte podle procesu [SDL](http://www.microsoft.com/sdl) pro navrhování a vytváření této služby. 

#### <a name="services-zone"></a>Oblasti služeb

Systém řízení (nebo řadič) je softwarové řešení, které souvisí se zařízením, nebo pole brána nebo shluk brány pro řízení jednoho nebo více zařízení nebo chcete-li shromažďovat nebo ukládat a analyzovat data zařízení pro prezentaci nebo následné kontrolní účely. Řídicí systémy jsou pouze subjekty v oblasti působnosti této diskuse, která by mohla usnadnit okamžité interakce s lidmi. Výjimkou jsou vnitřní povrchy fyzické ovládací prvek na zařízeních, jako je přepínač, který umožňuje uživateli vypnout zařízení nebo změnit další vlastnosti a pro které není žádný funkční ekvivalent, který je přístupný digitálně. 

Vnitřní povrchy fyzické ovládací prvek jsou ty, kde správní logiku řazení omezuje funkci fyzické povrchu ovládacího prvku tak, že lze vzdáleně spustit ekvivalentní funkce nebo vstupní konflikty s vzdálené vstupy lze předejít – intermediated ovládacích ploch jsou koncepčně připojené k místní kontrolní systém, který využívá stejné základní funkce jako ostatní systém dálkového ovládání, které zařízení může být připojen k paralelně. Největšími ohroženími cloud computing naleznete na stránce [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) .


## <a name="additional-resources"></a>Další zdroje

Naleznete další informace v následujících článcích:

- [Nástroj pro modelování ohrožení SDL](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
- [Referenční Architektura Microsoft Azure IoT](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
 
