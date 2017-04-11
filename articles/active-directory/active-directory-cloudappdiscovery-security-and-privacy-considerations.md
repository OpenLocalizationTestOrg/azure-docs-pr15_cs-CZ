<properties
    pageTitle="Cloud aplikace zjišťování zabezpečení a soukromí | Microsoft Azure"
    description="Toto téma popisuje zabezpečení a ochrana osobních údajů důležité informace týkající se aplikace zjišťování cloudu."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloudových aplikace zjišťování zabezpečení a soukromí

Microsoft velmi dbá na ochranu osobních údajů a zabezpečení dat, předvádění softwaru a služeb, které vám pomůžou se správou zabezpečení vaší organizace. <br>
Jsme rozpozná, pověřit datům ostatním, že vyžaduje zabezpečení investic náročné zabezpečení a odborných informací na pozadí je.
Microsoft vyhovuje striktně dodržování předpisů a pokyny pro zabezpečení z zabezpečené software vývojového životního cyklu postupy provozním službu. <br>
Zabezpečení a ochrana dat je nejvyšší prioritou společnosti Microsoft.

Toto téma vysvětluje, jak Shromažďované, zpracovávané a zabezpečená v Azure Active Directory cloudu aplikace zjišťování dat




##<a name="overview"></a>Základní informace

Zjišťování aplikace cloudu je funkce Azure AD a je hostovaný v Microsoft Azure. <br>
Agent koncový bod cloudu aplikace zjišťování slouží k shromažďovat data o zjišťování aplikace z IT spravovaných počítačů. <br>
Shromážděná data se pošle bezpečně šifrované kanálem služby Azure AD cloudu aplikace zjišťování. <br>
Zjišťování aplikace cloudu data pro organizaci bude viditelný na portálu Azure. <br>


<center>![Jak funguje aplikace zjišťování cloudu](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png)</center> <br>


V následujících částech sledovat tok informací a popisují, jak je zabezpečená pohyb z vaší organizace ve službě Cloud aplikace zjišťování a nakonec na portál zjišťování aplikace cloudu.



## <a name="collecting-data-from-your-organization"></a>Shromažďování dat z vaší organizace

Pomocí funkce zjišťování cloudu aplikace Azure Active Directory a dozvědět se víc do aplikace používané zaměstnanci ve vaší organizaci, budete muset nejdřív nasazení agent koncový bod Azure AD cloudu aplikace zjišťování počítačích ve vaší organizaci.

Správce klienta služby Azure Active Directory (nebo jejich delegát) můžete stáhnout instalační balíček agent z portálu Microsoft Azure. Agent můžete být ručně nainstalovaná nebo nainstalovaná na víc počítačů v organizaci pomocí SCCM nebo zásad skupiny.

Další informace o možnostech příručce [cloudu aplikace zjišťování skupiny zásad nasazení](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).
<br>

### <a name="data-collected-by-the-agent"></a>Data shromážděná agent

Informace uvedené v následujícím seznamu se shromažďují agentem po připojení k webové aplikace. Informace se shromažďují jen pro tyto aplikace, které správce nakonfiguroval pro zjišťování. <br>
Úprava seznamu cloudu aplikací, které agent sleduje prostřednictvím zásuvné cloudu aplikace zjišťování na Microsoft [Azure portál](https://portal.azure.com/), klikněte v části **Nastavení**->**Shromažďování dat**->**seznam aplikací kolekcí**. Další podrobnosti najdete v tématu [Získání začít s cloudu aplikace zjišťování](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
<br>
**Kategorie informací**: informace o uživatelích <br>
**Popis**: <br>
Uživatelské jméno systému Windows procesu, která vyrobila žádost byste do cíle webové aplikace (například: doména\uživatelské jméno) a Windows zabezpečení identifikátor (ID zabezpečení) uživatele.


**Kategorie informací**: informace zpracovat <br>
**Popis**: <br>
Název procesu, který žádost byste do cíle webové aplikace (například: "iexplore.exe")

**Kategorie informací**: informace pro počítač <br>
**Popis**: <br>
Název NetBIOS počítače, na kterém je nainstalovaná agent.

**Kategorie informací**: informace přenosy aplikace <br>
**Popis**: <br>

Následující informace o připojení:

- Zdroj (místní počítač) a cílové IP adresy a čísla portů

- Veřejnou IP adresu přes který žádost přejde mimo organizaci.

- Čas žádosti

- Objem provozu odesílání a přijímání

- IP verze (4 nebo 6)

- TLS pouze pro připojení: název hostitele cílové z koncovku označení název serveru nebo certifikát serveru.

Tyto informace HTTP:

- Metoda (GET, příspěvek atd.)

- Protocol (protokol HTTP/1.1 atd.)

- Identifikační řetězec

- Název hostitele

- Cílový identifikátor URI (s výjimkou řetězce dotazu)

- Informace o typu obsahu

- Informace o adresách URL odkazující (s výjimkou řetězce dotazu)



> [AZURE.NOTE] Výše uvedené HTTP informace se shromažďují všech – šifrované připojení.
TLS připojení jsou tyto informace pouze zachyceny při nastavení "Hloubkovou kontrolu" zapnuté na portálu. Nastavení je ve výchozím nastavení "ZAP".
Další podrobnosti, najdete níže a [Začínáme pracovat s cloudu aplikace zjišťování](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


Kromě data, která agent shromažďuje informace o činnosti sítě taky shromažďuje anonymní informace o software a konfigurace hardwaru, zpráv o chybách a informace o využití agent.

<br><br>
### <a name="how-the-agent-works"></a>Jak funguje agent

Instalace agent obsahuje dvě složky:

- Součásti režimu uživatele

- Součást jádra ovladače (ovladač filtrování platformu Windows)



Při první instalaci agent ukládá důvěryhodnou certifikační specifické pro počítače na počítač, která je pak používá k zabezpečené připojení ke službě Cloud aplikace zjišťování. <br>
Agent pravidelně načte konfigurace zásad služby cloudu aplikace zjišťování přes toto zabezpečené připojení. <br>
Zásady obsahuje informace o aplikace, které cloudu monitor a jestli automatické aktualizace by měla být povolená, mimo jiné.

Web přenosy odesílané a přijímané v počítači v aplikaci Internet Explorer a chromu, agent cloudu aplikace zjišťování analyzuje přenos a extrahuje relevantních metadat (viz **Data shromážděná agent** výše). <br>
Každou minutu agent odešlou shromážděných metadata služby cloudu aplikace zjišťování šifrované kanálem.

Součást ovladače zachytí šifrovaný přenos a vloží přímo do šifrované proudu. Další informace v části **Intercepting data z šifrované připojení (hloubkovou kontrolu)** .


### <a name="respecting-user-privacy"></a>Zachování soukromí uživatelů

Naším cílem je poskytovat správci nástroje nastavíte zůstatek mezi podrobné optika do aplikace použití a uživatel ochrany osobních údajů podle potřeby pro svou organizaci. K tomuto účelu nabízíme následující knoflíky na stránce nastavení v portálu:

- **Shromažďování dat**: správce můžete určit, které aplikace nebo kategorií aplikací chtějí načtení dat zjišťování.

- **Hloubkovou kontrolu**: Správci můžete se rozhodnout, můžete určit, pokud agent shromažďuje přenosy protokolu HTTP pro připojení SSL/TLS (označovaná taky jako **"hloubkovou kontrolu"**). Další informace o v další části.

- **Svolení možnosti**: mohou správci portálu cloudu aplikace zjišťování můžete zvolit, zda uživatelům shromažďování dat agentem oznamovat a jestli chcete vyžadovat, aby uživatel svolení před spuštěním agent shromažďování dat uživatele.

Agent koncový bod cloudu aplikace zjišťování pouze shromažďuje informace podle **Data shromážděná agent** výše.


### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Zachycení dat z šifrované připojení (hloubkovou kontrolu)
Jak jsme zmínili o tom, mohou správci konfigurovat agent sledování dat z šifrované připojení ("hloubkovou kontrolu"). TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) je nejběžnější protokoly používaných na Internetu dnes. Zašifrováním komunikace s TLS klienta můžete vytvořit zabezpečené a privátních komunikaci s webovým serverem; TLS poskytuje základní ochranu pro předávání přihlašovacími údaji a před únikem citlivé informace.

Když začátku do konce zabezpečeného šifrované kanálu poskytovanou TLS umožňuje důležité zabezpečení a ochrana osobních údajů, protokolu často překročen za účelem škodlivým nebo nefarious. Tolik Ano ve skutečnosti této TLS je označovaná také jako "univerzální brány firewall přemostění protokol". Kořenové problém je, že většina bránách firewall nelze zkontrolovat TLS komunikace, protože data aplikace vrstev zašifrován SSL. To, útočníků často využít TLS předvádění škodlivým datové části jistotu, že i nejčastěji inteligentní aplikace vrstvy brány firewall úplně nevidomé se TLS a musí jednoduše přenášet TLS komunikaci mezi tabulkami hosts uživatele. Koncoví uživatelé často využít TLS nepoužili přístup k ovládacím prvkům vynucení podnikových bran a proxy servery, využití k připojení do veřejné proxy servery a tunneling není TLS protokoly přes bránu firewall, která mohou být jinak blokovány zásadami.

Hloubkovou kontrolu umožňuje zjišťování aplikace cloudu agent má fungovat jako důvěryhodného člověka v – uprostřed. Při klienta žádosti o přístup k prostředku HTTPS chráněné ovladač koncový bod Agent zachytí připojení a zjistí, že nové připojení k serveru cíl načte jeho certifikát SSL jménem klienta. Agent zkontroluje, aby certifikát může být důvěryhodný (Kontrola, zda nebyla odvolán a provádění jiné kontroly certifikát), a pokud tyto průchodu Agent koncový bod potom zkopíruje informace z certifikát serveru a vytvoří vlastní certifikát serveru – jmenoval certifikátu zachycení – pomocí těchto informací. Zachycení certifikát je přihlášený na průběžně agentem koncový bod pomocí kořenového certifikátu, která je nainstalovaná v úložišti důvěryhodnou certifikační systému Windows. Certifikát podepsaný svým držitelem označen není možné exportovat a ACL museli správci. Je určen nikdy opustit počítače, na kterém byla vytvořená. Zachycení certifikát přijaté koncového uživatele klientské aplikaci ho bude důvěřovat protože úspěšně ověřením řetěz certifikátů úplně ke kořenovému certifikátu. Tento postup je většinou průhledné z koncového uživatele hlediska s několika upozornění podle níže uvedeného popisu.

Povolením hloubkovou kontrolu agenta cloudu aplikace zjišťování koncový bod dešifrování a zkontrolovat TLS zašifrovaných komunikace povolení služby ke snížení šumu a poskytují další informace o použití aplikace šifrované cloudu.

#### <a name="a-word-of-caution"></a>Word upozornění
Před povolením hloubkovou kontrolu, důrazně doporučujeme komunikovat zamýšlených právnické a oddělení HR a získat souhlas. Podrobná soukromé šifrované komunikace koncový uživatel může být citlivé předmět zjevných důvodů. Před výrobní zavádění hloubkovou kontrolu Ujistěte se, že vaše firemní zabezpečení a zásad přijatelného použití jsme aktualizovali označíte, že budou kontrolovat šifrované komunikace. Upozornění a osvobození od daně považované za citlivé webů (například bankovní a lékařích weby) také třeba pokud konfigurace zjišťování aplikace cloudu je sledování. Výše uvedené, mohou správci portálu cloudu aplikace zjišťování zvolte, jestli uživatelům shromažďování dat agentem oznamovat a jestli se má požadovat souhlasu uživatele před spuštěním agent shromažďování dat uživatele.

### <a name="known-issues-and-drawbacks"></a>Známé problémy a nevýhody
Jsou několik případy, kdy TLS zachycení mohou ovlivnit činnosti koncového uživatele:

- Rozšířené certifikáty ověření (EV) vykreslování do adresního řádku prohlížeče zelené má fungovat jako vizuální potvrzením, že jsou návštěvě důvěryhodného webu. TLS kontroly nelze duplikovat EV v certifikát, který problémy k desktopovému klientovi, weby, které používají EV certifikáty budou pracovat obvykle ale panelu Adresa nezobrazí zelená.  

- Veřejné klíčové Připnutí (označované také jako certifikát Připnutí) usnadňují ochrana uživatelů před útoky člověka v – uprostřed a škodlivý certifikačních autoritách. Když kořenového certifikátu připnuté lokality neodpovídá žádné známé dobré CA, prohlížeči odmítne připojení se k chybě. Připojení se nepovede, protože TLS zachycení, ve skutečnosti člověka v – uprostřed.

- Pokud uživatelé klikněte na ikonu zámku v prohlížeči panelu Adresa prohlížeče kontrolovat informace o webu, se nezobrazí řetěz končící certifikační autorita slouží k podpisu certifikátu webu, ale místo toho řetěz certifikátů končící znakem okna důvěryhodné úložiště certifikátů.

Snížit výskyty těchto problémů, doporučujeme udržení přehledu o cloudovými službami a klientské aplikace známé pomocí rozšířeného ověření nebo veřejných klíčové Připnutí a sdělte agenta koncový bod, chcete-li předejít zachycení dotčeném připojení. I v těchto případech dostanete sestav použití tyto aplikace cloudu a objemu převáděných dat však od nejsou hloubkové zkontrolovat, žádné informace o použití aplikace byly budou k dispozici.


## <a name="sending-data-to-cloud-app-discovery"></a>Odeslání dat do cloudu aplikace zjišťování

Jakmile metadat shromáždil agentem, do mezipaměti v počítači pro jednu minutu nebo dokud data uložená v mezipaměti dosáhne velikosti 5MB. Je pak komprimované a odesílaná zabezpečené připojení ke službě Cloud aplikace zjišťování.

Při nemůžete komunikovat ke službě Cloud zjišťování aplikace z nějakého důvodu agent shromážděných metadata uložené v mezipaměti místního souborů, které můžete jenom k nim získat přístup privilegovaných uživatelů v počítači (například skupiny Administrators). <br>
Agent automaticky pokouší odeslání metadata v mezipaměti, dokud úspěšně byla přijata službou zjišťování aplikace cloudu.



## <a name="receiving-the-data-at-the-service-end"></a>Příjem dat na konci služby

Zástupci ověřování aplikace zjišťování cloudové služby pomocí ověřovací certifikát konkrétního klientského počítače výše uvedené a bude předávat data šifrované kanálem. <br>
Zjišťování aplikace cloudové služby technologie pro analýzu kanálem k odesílání zpráv zpracovávání metadat pro jednotlivé zákazníky samostatně logicky rozdělení všechny fázemi kanálu analýzy.
Analyzované metadata jednotek různé sestavy na portálu.

Nezpracovaná metadat a analyzovat metadata jsou uloženy 180 dní. Zákazníci kromě toho můžete k zaznamenání analyzované metadata v účtu úložiště objektů blob Azure jejich výběru.
To je užitečné pro analýza offline metadat taky delší uchování data.

## <a name="accessing-the-data-using-the-azure-portal"></a>Přístup k datům pomocí portálu Azure

V úsilí zabezpečit metadata shromažďované ve výchozím nastavení jenom globální Správci klienta mít přístup k funkci cloudu aplikace zjišťování Azure portálu. <br>
Správci však můžete zvolit jiné uživatelům nebo skupinám tento přístup delegáta.



> [AZURE.NOTE] Další podrobnosti najdete v tématu [Začínáme pracovat s cloudu aplikace zjišťování](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

<br>
Všichni uživatelé přístup k datům na portálu musí mít licenci Azure AD Premium.



##<a name="additional-resources"></a>Další zdroje informací


* [Jak lze zjistit unsanctioned cloudu aplikace, které se používají v rámci naší organizace](active-directory-cloudappdiscovery-whatis.md)
* [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
