<properties
   pageTitle="Dostupnost pro Azure aplikace | Microsoft Azure"
   description="Technický přehled a podrobné informace o navrhování a vytváření aplikací pro dostupnost na Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-for-applications-built-on-microsoft-azure"></a>Dostupnost pro aplikace založený na Microsoft Azure

Vysoce dostupné aplikace pohlcuje kolísání dostupnost, načítání a dočasné k chybám v závislé služby a hardwaru. Aplikace nadále pracovat na přijatelný uživatele a úrovni systémové odpověď, jak je definován obchodní požadavky nebo aplikace smlouvy o úrovni služeb (rozsahu).

##<a name="azure-high-availability-features"></a>Azure vysoká dostupnost funkcí

Azure obsahuje mnoho předdefinované platformy, které podporují vysoce dostupných aplikací. Tato část popisuje některé z těchto klíčové funkce. Komplexnější analýza platformy najdete v článku [příručku Azure odolnost proti chybám](./resiliency-technical-guidance.md).

###<a name="fabric-controller"></a>Řadiče domény struktury

Správce Azure struktury předpisy a sleduje podmínka instancí Azure výpočetním. Správce struktury zkontroluje stav hardware a software počítače instancí Host (hostitel) a Host. Když zjistí se nepovede, vynutí rozsahu automaticky přemístíte OM instance. Koncepce dalších domén poruch a upgrade podporuje SLA výpočetním.

Při zavedení několika instancí role cloudové služby Azure nasadí tyto instance na různých poruch domény. Hranice domény poruch je v podstatě jiný hardware regálů ve stejné oblasti. Poruchy domény snížit pravděpodobnost, že přerušíte selhání lokalizované hardwaru služby aplikace. Počet poruch domény, které mají přiřazovány role webu nebo pracovního spravovat nedají. Správce struktury používá vyhrazené prostředky, které jsou odděleně od Azure hostovaný aplikací. Nemá provozu ze 100 %, protože slouží jako jádro Azure systému. Sleduje a spravuje role instance mezi poruch doménami.

Na následujícím obrázku vidíte, že řadiče struktury nasazuje a spravuje přes různé poruch domény Azure sdíleného zdroje.

![Zjednodušené zobrazení izolace domény poruch](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

Upgrade domény se podobají poruch domény ve funkci, ale podporují inovace spíše než k chybám. Upgrade doména je logické jednotky instance oddělení, která určuje, které instancí služby konkrétní upgradují se v určitém bodě v čase. Ve výchozím nastavení pro nasazení hostovanou službu jsou definované pěti upgradu domény. Však můžete změnit hodnotu do souboru definice služby. Předpokládejme například, že máte osm výskyty vaše role web. Nastane dvě instance ve tři upgradu domény a dvě instance v jedné upgradu doméně. Azure definuje pořadí update, ale je založená na počtu upgradu domén. Další informace o upgradu domény najdete v článku [aktualizace do cloudové služby](../cloud-services/cloud-services-update-azure-service.md).

###<a name="features-in-other-services"></a>Funkce v jiných služeb

Kromě toho, které podporují vysoké výpočetním dostupnost platformy Azure vloží vysoká dostupnost funkcí do svých služeb. Například úložišti Azure udržuje tři kopie všech objektů blob tabulce a data fronty. Umožňuje taky možnost geo replikace pro ukládání záložní kopie objektů BLOB a tabulek v sekundární oblasti. Síť doručení obsahu Azure umožňuje objektů BLOB do mezipaměti ve světě redundance a škálovatelnost. Databáze SQL Azure udržuje i více repliky.

Kromě [odolnost proti chybám příručku](https://aka.ms/bctechguide) řadu článků najdete v článku [Doporučené postupy pro návrh ve velkém měřítku služby Azure Cloud Services na](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) papír. Tyto papírů poskytují hlubší diskuse Azure platformu dostupnost funkcí.

Ačkoli Azure nabízí několik funkcí, které podporují dostupnost, je důležité pochopit jejich omezení:

- Pro využití Azure zaručuje, že vaše role jsou k dispozici a spuštěnou, ale nebudou vědět, pokud je vaše aplikace spuštěné nebo přetížený.
- Databáze SQL Azure replikace dat synchronní v oblasti. Můžete aktivní geo replikace, které umožňuje až čtyři kopie další databáze ve stejné oblasti (nebo různé regiony a). Tyto repliky databáze nejsou v okamžiku zálohy. Databáze SQL poskytují možnosti zálohování v daném okamžiku. Další informace o funkcích SQL databáze v daném okamžiku najdete v tématu [Čárky databáze SQL Azure v době obnovení](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/).
- Pro Azure úložiště tabulky a objektů blob dat replikovat ve výchozím nastavení alternativní oblasti. Ale nebudete mít přístup replik, dokud Microsoft zvolí přenesou do alternativního webu. Pouze v případě přerušení delší oblast služby dojde k selhání oblast a je žádné SLA geo převzetí přihlášení. Také je důležité mít na paměti, že omezí se možné poškození dat rychle dvojstránky replikám.

Z těchto důvodů třeba doplnit dostupnost funkcí platformy s funkcemi dostupnost specifické pro aplikaci. Funkce specifické pro aplikaci dostupnost obsahují funkci snímek objektů blob vytvořit zálohování dat objektů blob.

###<a name="availability-sets-for-azure-virtual-machines"></a>Dostupnost nastaví pro virtuálních počítačích Azure

Většinou tento článek se zaměřuje na cloudové služby, které používají platformu modelu služby (PaaS). Můžou ale nastat také konkrétní dostupnost funkcí pro Azure virtuálních počítačích, které používá infrastrukturu jako modelu služby (IaaS). Abyste dosáhli dostupnost s virtuálních počítačích, je nutné použít dostupnost sady. Sady dostupnost slouží podobně jako funkce poruch a upgrade domény. V rámci sady dostupnost Azure umístí virtuálních počítačích způsobem, který brání uvedení dolů všech počítačů v dané skupině poruch lokalizované hardware a činnosti spojené s údržbou. Dostupnost sady jsou potřebné k dosažení SLA Azure dostupnost virtuálních počítačích.

Následující diagram poskytuje vyjádření dvou množinách dostupnost tento web skupiny a SQL Server virtuálních počítačích, respektive.

![Dostupnost nastaví pro virtuálních počítačích Azure](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] Na předchozím obrázku je SQL Server nainstalovanou a spuštěnou na virtuálních počítačích. Tím se liší od předchozího diskusní databáze SQL Azure, která nabízí databáze jako služba spravovaných.

##<a name="application-strategies-for-high-availability"></a>Aplikace strategie pro dostupnost

Většina aplikace strategie pro dostupnost zahrnují redundance a odebrání pevných závislostí mezi součástí aplikace. Návrh aplikace by měl podporují odolnost proti chybám během Občasná prostoje Azure nebo služeb třetí strany. Následující oddíly popisují aplikace vzorků pro zvýšení použitelnosti cloudovým službám.

###<a name="asynchronous-communication-and-durable-queues"></a>Asynchronní komunikaci a trvalé fronty

Zvažte asynchronní komunikaci mezi volně vázaný služby zvětšíte dostupnost v Azure aplikací. V tomto vzoru zaznamenávat zprávy do úložiště zpráv nebo Bus služby Azure fronty pro pozdější zpracování. Při psaní zprávy do fronty okamžitě opět odesílateli zprávy. Jiné vrstvě aplikace zpracování zprávy zpracování, obvykle implementovaná roli kolegy. Pokud roli pracovníka přejde, nahromadit zprávy ve frontě, dokud se obnoví služby zpracování. Dokud fronty je k dispozici, neexistuje žádná přímé závislost mezi front-end odesílatele a zpracování zpráv. Díky požadavku na synchronní volání služby, které se dají výkon kritický distribuované aplikace.

Změna tohoto používá Azure úložiště (objektů BLOB, tabulek, dotazů) nebo služby Bus fronty jako převzetí umístění pro volání nezdařeném uložení databáze. Například synchronní volání v aplikaci na jinou službu (například databáze SQL Azure) opakovaně selže. Je možné serializuje tato data do trvalé úložiště. Nastane okamžik novější po návrat do online režimu, služby nebo databáze aplikace znovu odesláním žádosti o z úložiště. Rozdíl v tomto modelu je, že intermediate umístění konstantní součástí pracovního postupu aplikace. Se používá pouze v selhání scénáře.

V obou případech asynchronní komunikaci a pomocná úložiště zabránit vypnutých služby back-end uvedení dolů celou aplikaci. Fronty sloužit jako logické zprostředkovatele. Další výběr správné fronty služby, najdete v článku [Azure fronty Bus služby Azure fronty – porovnání a v porovnání s](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###<a name="fault-detection-and-retry-logic"></a>Poruchy rozpoznávání a opakování použití logických operátorů

Klíčových bodů v návrhu vysoce dostupné aplikace, je použít Logika opakování kódu řádně zpracovávání služba dočasně dolů. [Přechodná poruch zpracování blok aplikace](https://msdn.microsoft.com/library/hh680934.aspx), vyvinutý společností Microsoft Patterns a postupy týmu usnadní vývojáře aplikace v tomto procesu. Slovo "přechodové" znamená dočasné podmínku, která trvá jenom relativně krátké čas. V rámci tohoto článku zpracování přechodná selhání je součástí vysoce dostupných aplikací. Přechodné podmínky příklady chyby chybovému sítě a ztratilo připojení k databázi.

Přechodné blok poruch zpracování aplikace je zjednodušené způsob zpracování chyb v rámci vašeho kódu bezproblémové způsobem. Ho můžete zlepšit dostupnost aplikace přidáním robustní přechodná logika poruch zpracování. Ve většině případů Logika opakování zpracovává stručný přerušování a znovu připojí odesílatele a příjemce za jeden nebo více neúspěšné pokusy. Pokus o úspěšné opakovat obvykle půjde nezpozorována uživatele aplikace.

Vývojáři máte tři možnosti pro správu jejich opakování použití logických operátorů: přírůstková, pevných interval a exponenciální. Přírůstková čeká již před každý opakovat rostoucí lineární způsobem (například 1, 2, 3 a 4 sekundy). Pevné interval nečeká stejné časový úsek mezi jednotlivými opakováními (například 2 sekund). Další náhodné možnost exponenciální back vypnutí čeká již mezi opakování. Však používá exponenciální chování (například 2, 4, 8 a 16 sekundy).

Uceleném strategie v rámci vašeho kódu je:

1. Definování strategie opakování a zásady.
1. Akci, která může mít za následek chybu přechodná.
1. Pokud se vyskytne přechodná poruch vyvolejte zásad opakovat.
1. Pokud se nezdaří všechna opakování, zachycení konečný výjimek.

Otestujte logiky opakovat v simulovaný selhání zajistit, že opakování na po sobě jdoucí operace za následek neočekávané dlouhé zpoždění. To udělejte před rozhodnutím selhání obecný úkol.

###<a name="reference-data-pattern-for-high-availability"></a>Přehled dat vzor pro dostupnost

Referenčních dat je jen pro čtení dat aplikace. Tato data poskytuje firmy kontext, ve které aplikaci generuje transakční data v průběhu operace business. Transakční data jsou v okamžiku funkci referenční data. Proto jeho integritu závisí na snímku referenčních dat v čase transakce. Toto je poněkud volné definice, ale měli stačit pro naše účel.

Referenční data v rámci aplikace je nutné pro fungování aplikace. Příslušné aplikace vytvářejí a spravují referenčních dat; systémy řízení (MDM) hlavních dat často této funkce. Tyto systémy jsou zodpovědní za životního cyklu referenční data. Přehled dat jako příklad lze uvést katalogu produktů, zaměstnanců předlohy, hlavní části a vybavení předlohy. Referenční data můžete taky pocházejí z mimo vaši organizaci, například poštovní směrovací čísla nebo daň sazeb. Strategie pro zvýšení použitelnosti referenčních dat obtížně obvykle menší než u transakční data. Referenční data má tu výhodu je většinou neměnný.

Můžete pak dělat Azure web a pracovní role, které používání referenčních dat samostatného za běhu nasazením referenční data spolu s aplikací. Pokud rozměry místní úložiště umožňují na nasazení, to je ideální stav. Vložený databáze (SQL, NoSQL) nebo soubory XML používaný systém místní souborů vám pomůže s samostatné Azure výpočetním jednotkách. Však byste si měli mechanismus aktualizace dat v jednotlivých rolích bez nutnosti nového nasazení. K tomuto účelu umístěte všechny aktualizace referenčních dat cloudu úložiště koncový bod (například úložiště objektů Blob Azure nebo databáze SQL). Přidání kódu pro každou roli, která stahuje aktualizace dat do uzlů výpočetním při spuštění role. Můžete taky přidáte kód, který umožňuje správcům dělat Vynucené stahování do instance role.

Pokud chcete zvýšit dostupnost, role obsahovala také množiny dat odkaz v případě úložiště je dolů. Díky role budete chtít začít základní množiny dat odkaz, dokud úložiště zdroj dostupný pro aktualizace.

![Dostupnost aplikace prostřednictvím samostatného výpočetním uzlů](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

Jeden úvahu pro tento způsob je nasazení a rychlosti spuštění pro vaše role. Pokud jsou nasazení nebo stahování velkého množství dat odkaz na spuštění, může zvětšit dobu potřebnou k číselník nové nasazení nebo role instance. Může to být přijatelné kompromis pro samostatné s okamžitě k dispozici referenčních dat na každou roli, nikoli v závislosti na externí úložiště služby.

###<a name="transactional-data-pattern-for-high-availability"></a>Vzor transakční dat pro dostupnost

Transakční data jsou data, která generuje aplikace v rámci firmy. Transakční dat je tvořen kombinací sadu obchodních procesů, které aplikace implementuje a referenčních dat, která podporuje tyto procesy. Příklady transakční dat může obsahovat objednávek, Upřesnit dodací oznámení, faktury a zákazníka relací správu (CRM) příležitostí. Transakční informace shromážděné tedy bude zkrmování externí systémy uchovávání záznamů nebo pro další zpracování.

Mějte na paměti tento odkaz, který můžete změnit data v rámci systémů, které odpovídají tato data. Z tohoto důvodu musí transakční data uložit kontext dat v okamžiku odkaz tak, aby měl minimální externí závislosti sémantického konzistence. Zvažte například odebrání produkt společnostmi katalogu několik měsíců po splnění smysluplném pořadí. Nejlepší je vložit tolik kontext odkaz dat jako je to vhodné do transakce. To i v případě odkazu data se mění po nezaznamenávají transakce zachová sémantiku přidružený k transakci.

Jak jsme zmínili dříve, půjčovat sami architektury, které používají volné spojování a asynchronní komunikace na vyšší úroveň dostupnosti. To platí pro i transakční data, ale je složitější. Tradiční transakční názory obvykle spolehnout na databázi pro zajištění transakce. Při zavedení přechodné vrstvy kód aplikace pracuje správně data na jednotlivé vrstvy ověřit dostatečné konzistenci a životnost.

Následující postup popisuje pracovního postupu, který oddělí zachytávání transakční dat z jeho zpracování:

1. Web výpočetní uzly: prezentovat odkazují na data.
1. Externí úložiště: uložte intermediate transakční data.
1. Web výpočetní uzly: dokončení transakce koncových uživatelů.
1. Web výpočetní uzly: dokončené transakční data spolu s kontextu dat odkaz poslat dočasné trvalých úložiště, které je zaručena dát předvídatelná odpověď.
1. Web výpočetní uzly: signál dokončení koncových uživatelů transakce.
1. Pozadí výpočet uzel: extrahování transakční dat, po zpracování v případě potřeby a odeslat brožuru konečný úložišti v aktuálním systému.

Na následujícím obrázku vidíte případné provádění tento návrh v Azure hostované cloudové službě.

![Dostupnost prostřednictvím volné spojování](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

Přerušované šipky na předchozím obrázku označují asynchronní zpracování. Role front-end webového nemá žádné informace o této asynchronní zpracování. To vede k základnímu úložišti transakce na cílového s odkazem na aktuální systém. Z důvodu latence, který představuje tento asynchronní model není transakční data okamžitě k dispozici pro dotaz. Proto každé jednotky transakční data má být uložené v mezipaměti nebo relaci uživatele ke splnění okamžité uživatelského rozhraní.

Web role je samostatného před stavebním blokem infrastrukturu. Jeho dostupnost profil je tvořen kombinací roli web a Azure fronty a ne celou infrastrukturu. Kromě dostupnost tento přístup vám umožňuje roli webových měřítko vodorovné nezávisle na úložiště back-end. Tento model vysoká dostupnost mohou mít vliv na hospodárnost operací. Dodatečný jako Azure fronty a pracovní role může ovlivnit měsíční náklady na použití.

Všimněte si, že předchozí diagram znázorňuje jednu provádění tento oddělené přístup k datům transakční. Existují další možné implementace. Následující seznam uvádí několik možností:

 * Mezi role webového a fronty úložiště může umístit roli kolegy.
 * Služba Bus fronty lze použít místo fronty Azure úložiště.
 * Konečný cílový může být úložišti Azure nebo poskytovatele jinou databázi.
 * Azure mezipaměti lze ve vrstvě web poskytují okamžité mezipaměti požadavky po transakce.

###<a name="scalability-patterns"></a>Škálovatelnost: vzorky

Je důležité mít na paměti, že škálovatelnost cloudovou službu přímo ovlivní k dispozici. Pokud lepší zatížení způsobí služby být reagovat, dojem uživatele je aplikace dolů. Používaly osvědčené postupy pro škálovatelnost na základě vaší očekávané aplikace načítání a očekávaných. Nejvyšší měřítko zahrnuje mnoho co byste měli zvážit, jako je použití jedné a více účtů úložiště sdílení ve více databází a ukládání do mezipaměti strategie. Podrobnější pohled na tyto vzorce najdete v článku [Doporučené postupy pro návrh ve velkém měřítku služby na Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

##<a name="next-steps"></a>Další kroky

Tento článek je součástí řady článků věnovaných [havárie využití a dostupnost pro aplikace založený na Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Další článek v této řadě je [obnovení pro aplikace založený na Microsoft Azure](./resiliency-disaster-recovery-azure-applications.md).
