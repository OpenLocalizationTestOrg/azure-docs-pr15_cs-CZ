# <a name="internet-of-things-security-best-practices"></a>Doporučené postupy pro zabezpečení Internetu věcí

Zabezpečení věci z Internetu (IoT) infrastruktury vyžaduje přísné strategie zabezpečení do hloubky. Tato strategie vyžaduje zabezpečení dat v cloudu, ochranu integrity dat při přenosu prostřednictvím veřejné sítě internet a bezpečně zřídit zařízení. Každá vrstva vytváří vyšší záruky zabezpečení v celé infrastruktury.

## <a name="secure-an-iot-infrastructure"></a>Zabezpečení infrastruktury IoT

Tato strategie zabezpečení do hloubky mohou být vyvinuty a provedeny s aktivní účast různých účastníků zapojených do výroby, vývoje a nasazení IoT zařízení a infrastruktury. Následuje stručný popis těchto přehrávačů.  

- **Výrobce hardwaru IoT/integrátor**: obvykle jde o výrobci hardwaru IoT nasazena doplňky montáž hardware od různých výrobců nebo dodavatelů, poskytování hardware pro nasazení IoT vyrobené nebo integrované jinými dodavateli.
- **Vývojář řešení IoT**: vývoj řešení IoT se obvykle provádí vývojář řešení. Tento vývojář může část interní tým nebo specializovanými v této aktivitě integrátor systému (SI). Vývojář řešení IoT můžete vyvíjet různé součásti řešení IoT od začátku, integraci různých součástí dodávaných nebo open-source nebo přijmout předem řešení s menší úpravou.
- **Nasazovatel řešení IoT**: po IoT řešení je vyvíjeno, musí být nasazeny v poli. To zahrnuje nasazení hardwaru propojení zařízení a nasazení řešení v hardwarových zařízeních nebo v cloudu.
- **Operátor řešení IoT**: nasazení po IoT řešení, vyžaduje dlouhodobé operace, sledování, aktualizace a údržba. To lze provést interní tým, který zahrnuje specialisty technologie informace, hardwarových operací a údržby týmy a domény specialistů, kteří sledování správné chování celé infrastruktury IoT.

Následujících částech naleznete doporučené postupy pro každou z těchto přehrávačů umožňuje vyvíjet, implementovat a provozovat zabezpečenou infrastrukturu IoT.

## <a name="iot-hardware-manufacturerintegrator"></a>Výrobce hardwaru IoT/integrátor

Dále jsou uvedeny doporučené postupy pro IoT výrobci hardwaru a hardware doplňky.

- **Minimální požadavky na hardware oboru**: návrh hardwaru by měla zahrnovat minimální funkce potřebné pro fungování hardwaru a nic víc. Příklad je pouze v případě, že je nezbytné pro provoz zařízení patří USB porty. Tyto další funkce otevřít zařízení pro nežádoucí útoky, které by měly být vyloučeny.
- **Proveďte hardwaru manipulovat důkaz**: sestavení do mechanismů ke zjištění fyzické zásahy, například otevírání krytu zařízení nebo část zařízení odstranit. Tyto manipulovat signály mohou být součástí datového proudu, který je odeslán do cloudu, které by mohly upozornit hospodářské subjekty z těchto událostí.
- **Sestavit kolem hardwarového zabezpečení**: Povolí-li náklady na prodané zboží, sestavení funkce zabezpečení, jako například zabezpečené a šifrované úložiště nebo spuštění funkce založené na Trusted Platform Module (TPM). Tyto funkce musíte zařízení více zabezpečit a ochránit celé infrastruktury IoT.
- **Upgrade zabezpečení**: upgrady firmwaru po dobu životnosti zařízení jsou nevyhnutelné. Vytváření zařízení s bezpečné cesty pro inovace a zabezpečování kryptografické verzí firmwaru umožní zařízení je zabezpečený, během a po inovaci.

## <a name="iot-solution-developer"></a>Vývojář řešení IoT

Doporučené postupy pro vývojáře řešení IoT jsou následující:

- **Metodika vývoje zabezpečeného softwaru postupujte**: vývoj softwaru zabezpečení vyžaduje vozovkou nahoru přemýšlení o zabezpečení od zahájení projektu až jeho implementace, testování a nasazení. Možnosti platformy, jazyky a nástroje jsou ovlivněny pomocí této metody. Microsoft Security Development Lifecycle zajišťuje postupného vytváření zabezpečeného softwaru.
- **Zvolte open-source software s péčí**: Open-source software nabízí možnost rychle vyvíjet řešení. Když zvolíte open-source softwaru, zvažte úroveň aktivity Společenství pro každou komponentu, open-source. Aktivní Společenství zajistí, že software je podporován a zjištění a řeší problémy. Případně zakrývají a neaktivní open-source software nemusí být podporovány a budou pravděpodobně nejsou zjištěny problémy.
- **Integrace s péčí**: existuje mnoho chyb zabezpečení softwaru na hranici mezi rozhraní API a knihovny. Funkce, které nemusí být vyžadovány pro aktuální nasazení může být stále k dispozici prostřednictvím vrstvy API. K zajištění celkové zabezpečení, ujistěte se, zkontrolujte všechna rozhraní komponenty jsou integrovány pro chyby zabezpečení.      

## <a name="iot-solution-deployer"></a>Nasazovatel řešení IoT

Doporučené postupy pro pracovníky IoT řešení jsou následující:

- **Bezpečné nasazení hardwaru**: IoT nasazení může vyžadovat hardware má být zaveden v nezabezpečené umístění, například veřejné prostory nebo závadou národní prostředí. V takových situacích, zajistěte, aby hardware nasazení manipulací v maximálním možném rozsahu. USB nebo jiné porty jsou k dispozici na hardware, zajistit, které bezpečně. Mnoho způsobů útoku lze použít jako vstupní body.
- **Zabezpečit ověření klíče**: během nasazení, každé zařízení vyžaduje ID zařízení a ověřování klíčů generovaných službou cloud spojené. Zabezpečit tyto klíče fyzicky i po nasazení. Všechny odhalený klíč lze nebezpečného zařízení jako stávající zařízení.

## <a name="iot-solution-operator"></a>Operátor řešení IoT

Doporučené postupy pro IoT řešení operátory jsou následující:

- **Aktualizovat systém**: zajistit, aby operační systémy zařízení a všechny ovladače zařízení jsou inovovány na nejnovější verze. Pokud zapnete automatické aktualizace v systému Windows 10 (IoT nebo jiných SKU) Microsoft zůstane aktuální, poskytování zabezpečeného operačního systému pro zařízení IoT. Vedení ostatních operačních systémů (například Linux) aktuální pomáhá zajistit, že jsou také chráněny před škodlivými útoky.
- **Ochrana proti škodlivým aktivitám**: Pokud umožňuje operační systém, nainstalujte nejnovější antivirový a antimalwarový program možnosti v každém operačním systému zařízení. To může pomoci zmírnit nejvíce vnějším hrozbám. Většina moderních operačních systémů před hrozbami můžete zabezpečit přijetí vhodných opatření.
- **Často auditování**: auditování IoT infrastrukturu pro problémy související se zabezpečením je klíč, ale reagovat na případy porušení bezpečnosti. Většina operačních systémů poskytují předdefinované události protokolování, které by mělo být přezkoumáno často Ujistěte se, že nedošlo k porušení zabezpečení došlo k chybě. Informace o auditování nelze odesílat jako samostatné telemetrické proud ke službě cloud kde lze analyzovat.
- **Fyzicky chránit infrastruktura IoT**: nejhorší útokům proti infrastruktura IoT jsou spouštěny pomocí fyzický přístup k zařízení. Jeden důležité bezpečnostní praxi je k ochraně proti zneužití porty USB a jiných fyzický přístup. Je jeden klíč k odhalení porušení, které by mohlo dojít k protokolování fyzického přístupu, například pomocí portu USB. Windows 10 (IoT a jiných identifikátorů SKU) opět povolí podrobné protokolování těchto událostí.
- **Ochrana cloudu pověření**: pověření pro ověření cloudu pro konfigurace a provozní nasazení IoT jsou pravděpodobně nejjednodušší způsob, jak získat přístup a ohrozit systém IoT. Často měnit heslo chránit pověření a zdržet se ve veřejných počítačích pomocí těchto pověření.

Závislé na možnostech různých zařízení IoT. Některá zařízení mohou být počítače se systémem běžné operační systémy a některá zařízení mohou být velmi lehký operačním systémem. Doporučené postupy zabezpečení popsané dříve mohou být pro tato zařízení v různých stupních. Je-li k dispozici, následovat další zabezpečení a nasazení doporučené postupy od výrobců těchto zařízení.

Některé starší a omezené zařízení pravděpodobně není byly navrženy výhradně pro nasazení IoT. Tato zařízení mohou chybět možnost šifrování dat, připojení k Internetu nebo poskytují pokročilé auditování. Moderní a bezpečné pole brána v těchto případech můžete agregovat data ze staršího zařízení a zabezpečení potřebné pro připojení těchto zařízení přes Internet. Pole brány může poskytovat zabezpečené ověřování vyjednávání šifrované relace, přijetí příkazů z cloudu a mnoho dalších funkcí zabezpečení.
