<properties
    pageTitle="Služby Azure Government | Microsoft Azure"
    description="Poskytuje a základní informace o dostupných služeb v Azure Government"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc" />


#  <a name="security"></a>Zabezpečení

##  <a name="principles-for-securing-customer-data-in-azure-government"></a>Zásady zabezpečení Data o zákaznících v Azure Government

Azure vláda obsahuje velké množství funkce a služby, které můžete použít k vytvoření cloudové řešení vlastním potřebám upraveno/řízená data. Je kompatibilní se standardem zákazníka řešení ničím efektivní provádění funkce Azure Government mimo pole, doplněný z hlediska zabezpečení souvislá data.

Když hostitelem řešení v Azure Government zpracuje Microsoft spoustu tyto požadavky na úrovni infrastruktury cloudu.

Na následujícím obrázku vidíte Azure obrana v název hloubkové modelu. Microsoft poskytuje například základní cloudové infrastruktury DENIAL, spolu s zákazníka funkce, například zabezpečení zařízení zákaznické aplikace, které potřebuje DENIAL.

![alternativní text](./media/azure-government-Defenseindepth.png)

Tato stránka popisuje základní zásady zabezpečení vašich služeb a aplikací poskytující pokyny a osvědčené postupy v tom, jak použít tyto zásady; jinými slovy jak zákazníci měli využívat inteligentní Azure Government splňovala závazky a povinnosti, které jsou potřeba pro řešení, které zpracovává informace ITAR.

 Obecné zásady zabezpečení data o zákaznících jsou:

- Ochrana dat pomocí šifrování
- Správa tajemství
- Izolace omezit přístup k datům

###  <a name="protecting-customer-data-using-encryption"></a>Ochrana dat zákazníků pomocí šifrování

Zmírnění rizik a žádosti o schůzku zákonné povinnosti jsou řízení rostoucí fokus a důležitost šifrování. Použití implementaci efektivní šifrování k vylepšení aktuální síť a aplikace bezpečnostních opatřeních – a snížit celkového rizika prostředí cloudu.

#### <a name="encryption-at-rest"></a>Šifrování na ostatní
Šifrování data na ostatních platí pro ochranu obsahu zákazníka v místa na disku. K tomu může dojít několika způsoby:

#### <a name="storage-service-encryption"></a>Úložiště služby šifrování

Azure úložiště služby je povoleno šifrování na úrovni účtu úložiště výsledkem objektů BLOB bloku a automaticky šifrovány zápisu Azure úložiště objektů BLOB stránky. Při načtení dat z Azure úložiště ho bude dešifrovat tak, že služba úložiště před vrácením. Slouží k zabezpečení dat, aniž by bylo nutné upravit nebo přidat kód pro všechny aplikace.

#### <a name="client-side-encryption"></a>Šifrování na straně klienta
Šifrování na straně klienta je integrovaná v Java a .NET úložiště klienta knihovny, které můžete využít Azure klíč trezoru rozhraní API to provádění jednoduchých provádět. Pomocí klávesy trezoru Azure získat přístup k tajemství trezoru klíč Azure pomocí služby Azure Active Directory určitým uživatelům.

#### <a name="encryption-in-transit"></a>Šifrování při přenosu šifrovaná

Základní šifrování k dispozici pro připojení k Azure vláda podporuje protokol úroveň zabezpečení TLS (Transport) 1.2 a certifikáty X.509. Federální informace standardní FIPS (Processing) kryptografický algoritmů 140-2 úrovně 1 se také používá ke infrastruktury síťových připojení mezi Azure Government datacentrech.  Windows Server 2012 R2 a Windows 8-plus VMs a Azure sdílené složky můžete použít SMB 3.0 šifrování mezi OM a sdílení souborů. Šifrování data před převádí se na úložiště v klientské aplikaci a data dešifrovat za ni se převede z úložiště pomocí šifrování na straně klienta.

#### <a name="best-practices-for-encryption"></a>Doporučené postupy pro šifrování

- IaaS VMs: Pomocí šifrování Azure disku. Zapnutí úložiště služby šifrování šifrování virtuální pevný disk soubory, které se používají k obecnějším údajům těchto discích v úložišti Azure, ale tento pouze šifruje nově ručně psaných data. To znamená, že když vytvoříte virtuálního počítače a potom povolit úložiště služby šifrování na úložiště, který obsahuje soubor virtuálního pevného disku, pouze změny budou šifrovány, nikoli původní soubor virtuální pevný disk.
- Klientské šifrování: Toto je nejbezpečnější způsob šifrování dat, protože šifruje před dopravní a šifruje data na ostatních. Vyžaduje však přidání kódu pro vaše aplikace pomocí úložiště, která nemusí chcete udělat. V těchto případech můžete HTTPs pro vaše data při přenosu šifrovaná a úložiště služby šifrování šifrování data na ostatních. Šifrování na straně klienta zahrnuje také další zatížení v klientském počítači – je nutné počítat v škálovatelnost plánů, zejména pokud jsou šifrování a přenosu velké množství dat.

###  <a name="protecting-customer-data-by-managing-secrets"></a>Ochrana dat zákazníků pomocí správy tajemství

Zabezpečené správy klíčů je důležité pro ochranu dat v cloudu. Zákazníci měli snažte se navázat zjednodušení správy klíčů udržujete zkratek používaných v cloudu aplikací a služeb šifrování data.

#### <a name="best-practices-for-managing-secrets"></a>Doporučené postupy pro správu tajemství

- Pomocí klávesy trezoru minimalizace rizika tajemství nebyly přístupné prostřednictvím pevně konfigurační soubory, skripty, nebo ve zdrojovém kódu. Azure trezoru klíč šifruje klávesy (například šifrování klíčů pro šifrování disku Azure) a tajemství (například hesla), ukládání do FIPS 140-2 úrovně 2 ověřuje hardwarové zabezpečení moduly (moduly hardwarového zabezpečení). Přidané assurance importovat nebo vygenerování klíčů v těchto moduly hardwarového zabezpečení.
- Kód aplikace a šablon by měl obsahovat pouze URI odkazy tajemství (což znamená, že skutečné tajemství nejsou v kódu, konfigurace nebo zdrojového kódu úložištích). Tím útoky typu phishing klíčové na interní a externí repo, například sklizně roboti v GitHub.
- Využití silných RBAC prvkům klíč trezoru. Pokud důvěryhodných operátor ponechá společnosti nebo převody do nové skupiny v rámci podniku, budou by mělo být zabráněno nebudete mít přístup k tajemství.

Další informace <a href="https://azure.microsoft.com/documentation/services/key-vault">veřejné dokumentace Azure klíč trezoru.</a>

###  <a name="isolation-to-restrict-data-access"></a>Izolace omezit přístup k datům

Izolace je všechny informace o použití omezení, segmentace a kontejnery omezit přístup k datům jenom uživatelé, services a aplikace. Například oddělení mezi klienty je mechanismus základní zabezpečení pro víceklientské cloudu platformy, třeba Microsoft Azure. Logické izolace zabraňuje jednoho klienta konflikt s operacemi jiném klientovi.

#### <a name="environment-isolation"></a>Prostředí izolace
Prostředí Azure Government je fyzické instanci, která je nezávislý před stavebním blokem sítě společnosti Microsoft. Dosahuje se prostřednictvím řadu fyzické a logické ovládacích prvků, které patří:

- Zabezpečení fyzické překážky pomocí Biometrická zařízení a kamery.
- Použití konkrétní přihlašovací údaje a vícefaktorové ověřování zaměstnanců společnosti Microsoft vyžadují logické přístup k provozním prostředí.
- Všechny infrastruktury služby pro státní správu Azure je umístěný ve Spojených státech.

#### <a name="per-customer-isolation"></a>Izolace jednoho zákazníka
Řízení přístupu k síti Azure implementuje a oddělení prostřednictvím VLAN izolace ACL, načíst vyrovnávání a filtry IP

Zákazníci další různých předplatných, skupiny zdrojů, virtuálních sítí a podsítí izolovat jejich zdroje.

## <a name="screening"></a>Blokování

Naposledy oznámená vysoká FedRAMP a oddělení obrana (Dial) dopad úrovni 4 schválení. To obsahuje mocninu panelu zabezpečení a dodržování předpisů v prostředí Azure Government.

Teď můžeme jsou blokování náš operátory na vnitrostátní kontrola subjekt s právními předpisy a platební (NACLC) nadefinovaná na oddíl 5.6.2.2 z DoD cloudu výpočetních zabezpečení požadavky průvodce (SRG):

>[AZURE.NOTE] Minimální pozadí vyšetřování povinné pro pracovníky CSP mají přístup k úrovni 4 a 5 informace k různým "Nekritický citlivé" (například na DoD ADP-2) je vnitrostátní subjekt obraťte se na práva a platební (NACLC) (pro dodavatelů "Nekritický citlivé") nebo středním rizika pozadí vyšetřování (MBI) pro označení "střední rizika" pozice.

Následující tabulka shrnuje naše aktuální blokování pro státní správu Azure operátory:

Azure Gov promítání a kontrolu na pozadí | Popis|
---|---|
Občanství USA |Ověření občanství USA.
Microsoft cloud pozadí kontrolu (za dva roky)|Rodné číslo hledat, zaškrtněte trestné, Office cizí prostředky řízení seznam (OFAC), seznam Bureau of Industry a zabezpečení (BIS), Office obrana Trade ovládací prvky vyloučit osob seznamu.
Kontrola vnitrostátní subjektu s právními předpisy a platební (NACLC) (každých pět let) | Přidá otisku pozadí zaškrtnutí FBI databází. Další informace přejděte na<a href="https://www.opm.gov/investigations/background-investigations/federal-investigations-notices/1997/fin97-02/"> Web správy zaměstnanců Office</a>. | 
<a href="https://www.microsoft.com/en-us/TrustCenter/Compliance/CJIS">Zločineckých informační službu (CJIS)</a> | CJIS je stav a místní a government FBI blokování který procesy otisku záznamů a ověří trestních historie na provozní zaměstnance, kteří může být k dispozici přístup k datům zločineckých důležité informace (CJI).  Tyto stavy znamená vlastní pozadí kontrola a následnému schválení všechny zaměstnance se potenciální přístup k CJI.|

Pro pracovníky Azure operace použít tyto zásady přístupu:

- Úkoly jsou jasně tato pole definovány, samostatný zodpovědnosti pro vyžádání, schvalování a nasazení změn.
- Přístup je prostřednictvím definovaný rozhraní, které obsahují určité funkce.
- Aplikace Access je těsně za běhu (za běhu) a uděleno jenom na jednotlivé případy nebo pro konkrétní údržbě a vždy po omezenou dobu.
- Přístup je na základě pravidel s definované role přiřazené pouze informace o oprávněních požadovaných pro řešení potíží.

Organizace pro standardy blokování zahrnout ověřování americké občanství podpory společnosti Microsoft a provozní zaměstnanců před udělen přístup k hostované Azure Government systémy. Pracovníky podpory, kteří potřebují pro přenos dat pomocí zabezpečené funkcí ve Azure Government. Přenos zabezpečené dat vyžaduje samostatnou sadu přihlašovacích získat přístup. Například pro přístup k systému metadata, operace zaměstnanců pomocí určitý web interním nástroje pro správu, rozhraní API jen pro čtení a za běhu zvýšení.

## <a name="next-steps"></a>Další kroky

Pro doplňující informace a aktualizace Předplaťte si <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure vláda blogu.</a>
