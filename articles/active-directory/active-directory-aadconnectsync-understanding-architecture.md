<properties
   pageTitle="Azure AD Connect synchronizace: Principy architektura | Microsoft Azure"
   description="Toto téma popisuje architekturu Azure AD Connect synchronizace a vysvětluje termíny používané."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/31/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect synchronizace: Principy architektura
Toto téma popisuje základní architektura Azure AD Connect synchronizace. V mnoha aspekty je podobná jeho předchůdců MIIS 2003, 2007 nástroj ILM a FIM 2010. Azure AD Connect synchronizace je vývoj těchto technologií. Pokud máte zkušenosti s některou z těchto dřívějších technologií, obsah tohoto tématu bude známé vám taky. Pokud začínáte synchronizace, toto téma je za vás. Není ale povinné znát podrobnosti o tomto tématu být úspěšný při přizpůsobení synchronizovat Azure AD Connect (s názvem sync engine v tomto tématu).

## <a name="architecture"></a>Architektura
Modul synchronizace vytvoří ucelený přehled objekty, které jsou uložené ve více připojených zdrojů dat a spravuje identity informace v těchto zdrojích dat. Integrované zobrazení je určený podle identity informace načtené z připojených zdrojů dat a sady pravidel, které určují způsob zpracování tyto informace.

### <a name="connected-data-sources-and-connectors"></a>Zdroje dat připojeného a spojnic
Modul synchronizace zpracovává informace identity z úložištích jinými daty, například Active Directory nebo databáze SQL serveru. Každý úložiště dat, která slouží k uspořádání její data ve formátu databáze a poskytující standardní datové metody je potenciální candidate zdroje dat pro modul synchronizace. Úložištích dat, které jsou synchronizované modulem synchronizace se označují jako **připojení zdroje dat** nebo **připojení adresáře** (CD).

Modul synchronizace zapouzdřuje interakci se zdrojem připojená data v modulu s názvem **spojnice**. Každý typ zdroje dat připojeného má konkrétní spojnice. Spojnice převádí požadovanou operaci do formátu, který je připojený zdroj dat rozumí.

Spojnice volat rozhraní API pro výměnu informací identit (čtení i zápis) se zdrojem připojená data. Je také možné přidat vlastní spojnice pomocí rozhraní extensible připojení. Následující obrázek znázorňuje, jak spojnice připojení zdroje dat připojeného k modul synchronizace.

![Arch1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Můžete v obou směrech toku dat, ale ho nemůžete flow v obou směrech současně. Jinými slovy spojnice můžete nakonfigurovat tak, aby data flow z zdroje dat připojeného sync engine nebo sync engine ke zdroji připojená data, ale jenom jednu z těchto operací může dojít v daném čase pro jeden objektů a atributů. Směr může být různé k různým objektům a různé vlastnosti.

Abyste mohli nakonfigurovat spojnici, zadáte typy objektů, které chcete synchronizovat. Určení typy objektů definuje rozsah objekty, které jsou součástí synchronizaci. Dalším krokem je vyberte atributy synchronizovat, která se nazývá seznamu zahrnutí atributů. Tato nastavení lze změnit kdykoli v odpovědi na změny na obchodní pravidla. Při použití Průvodce instalací Azure AD Connect jsou nakonfigurovány tato nastavení.

Export objekty do zdroje dat připojeného seznamu zahrnutí atributu musí obsahovat alespoň atributy minimální potřebná k vytvoření určitého typu objektu ve zdroji připojená data. Atribut **sAMAccountName** například musí být součástí seznamu zahrnutí atributů Export objektu uživatele do služby Active Directory, protože všechny objekty uživatelů ve službě Active Directory musí mít atribut **sAMAccountName** definované. Průvodce instalací znovu provede tuto konfiguraci za vás.

Pokud zdroje dat připojeného používá strukturální součásti, například oddíly nebo kontejnery k uspořádání objektů, můžete nastavit omezení v zdroje dat připojeného oblastech, které se používají pro dané řešení.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Interní sync engine názvů
Oboru engine celý synchronizace se skládá ze dvou obory názvů, které obsahují informace identity. Dva obory názvů jsou:

- Prostor konektoru (CS)
- Metaverse (více hodnot)

**Prostor konektoru** je pracovní oblasti obsahující formátovaná jako určené objekty ze zdroje dat připojeného a atributy určené v seznamu zahrnutí atributů. Modul synchronizace používá místo spojnici a určit, co se změnilo ve zdroji připojená data fáze příchozí změny. Modul synchronizovat taky používá místo spojnice připravení odchozích změn pro export ke zdroji připojená data. Modul synchronizace udržuje různých spojnice mezeru jako pracovní oblasti pro každou spojnici.

Pomocí pracovní oblasti modul Synchronizace zůstává nezávislá připojených zdrojů dat a nebyl ovlivněn jejich dostupnost a přístupnost. V důsledku toho můžete procesu identity kdykoli pomocí data v oblasti pracovní. Modul synchronizace můžete požádat o pouze změny provedené ve zdroji připojená data od posledního relaci komunikace ukončeny nebo nabízených pouze změny identity informace ještě přijato je připojený zdroj dat, která omezuje síťová komunikace mezi modul synchronizace a zdroje dat připojeného.

Kromě toho sync engine ukládá stavové informace o všech objektů, že fáze do pole spojnice. Po přijetí nových dat sync engine vždy vyhodnotí jestli data už synchronizování.

**Metaverse** je úložiště s informacemi agregované identity z různých zdrojů připojená data, poskytnutí jednoho globální, integrované zobrazení všech kombinovanou objektů. Objekty Metaverse vzniká na základě informací identity, který je načtená z kontingenčního seznamu zdroje dat připojeného a sady pravidel, které umožňují přizpůsobení synchronizaci.

Následující obrázek znázorňuje spojnice obor názvů a oboru metaverse v rámci modul synchronizace.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Objekty identity Sync engine
Objekty v modulu synchronizace jsou formátovaná jako některý z objektů ve zdroji připojená data nebo integrované zobrazení, které synchronizovat engine má těchto objektů. Každý objekt sync engine musí mít globálně jedinečný identifikátor (GUID). GUID zajištění integrity dat a express vztahy mezi objekty.

### <a name="connector-space-objects"></a>Spojnice rozmístění objektů
Při synchronizaci engine komunikaci se zdrojem připojená data, bude číst identity informace ve zdroji připojená data a použije tyto informace k vytvoření formátovanými jako objekt identity v prostoru spojnice. Nelze vytvářet ani odstraňovat tyto objekty jednotlivě. Však můžete ručně odstranit všechny objekty v mezerou spojnice.

Všechny objekty v prostor konektoru mít dva atributy:

- Globálně jedinečný identifikátor (GUID)
- Rozlišující název (DN)

Pokud je připojený zdroj dat přiřadí jedinečný atribut na objekt, pak objektů v prostor konektoru mohou také obsahovat atribut ukotvení. Atribut ukotvení jednoznačně identifikuje objektu ve zdroji připojená data. Modul synchronizace používá ukotvení k vyhledání odpovídající znázornění tento objekt ve zdroji připojená data. Sync engine předpokládá, že ukotvení objektu nikdy mění v průběhu životnost objektu.

Známé jedinečný identifikátor spoustu spojnice použít ke generování ukotvení automaticky pro každý objekt při importu. Konektor služby Active Directory například používá atribut **objectGUID** pro na ukotvení. Připojených zdrojů dat, které nejsou jasně definované jedinečný identifikátor můžete určit ukotvení generování jako součást konfiguračního Connector.

Případ, ukotvení je vytvořené z jedné nebo více jedinečné atributy objektu typu ani změny a to jednoznačně identifikuje objekt v spojnice místa (například číslo zaměstnance nebo ID uživatele).

Objekt místo spojnice může být jeden z těchto věcí:

- Přípravná objektu
- Zástupný symbol

### <a name="staging-objects"></a>Pracovní objektů
Objekt pracovní představuje instanci typy určené objektů ze zdroje dat připojeného. Kromě GUID a rozlišující název, který má objekt pracovní vždy hodnotu, která označuje typ objektu.

Objekty pracovní importovaných vždy mít hodnoty pro atribut ukotvení. Pracovní objekty, které byly nově zřízení modulem synchronizace a ho právě vzniku ve zdroji připojená data nemají hodnotu atributu ukotvení.

Objekty pracovní mít také aktuální hodnoty firmy atributy a provozní informace potřebné modulem synchronizace provést synchronizaci. Provozní informace zahrnují příznaky, které označují typ aktualizace, které jsou připravené na pracovní objektu. Pokud objekt pracovní obdržel nové identity informace ze zdroje připojená data, které nebyly dosud zpracovány, objekt označena jako **pole Čekání na importovat**. Pokud má pracovní objekt nové identity informace, které dosud nebyly exportovány ke zdroji připojená data, označena jako **pole Čekání na Exportovat**.

Objekt pracovní může být objekt importu nebo exportu objekt. Modul synchronizace vytvoří objekt importovat pomocí přijatou ze zdroje dat připojeného informace o objektu. Informace o existenci nový objekt, který odpovídá některému z typy objektů vybraná v konektoru přijaté sync engine předtím vytvořil objektu importovat do pole spojnice jako vyjádření objektu ve zdroji připojená data.

Následující obrázek znázorňuje import objekt, který představuje objektu ve zdroji připojená data.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

Modul synchronizace vytvoří objekt exportovat pomocí informace o objektu v metaverse. Export objektů jsou exportovány ke zdroji připojená data během další relace komunikace. Z pohledu modul Synchronizace export objektů není zatím ve zdroji připojená data. Atribut ukotvení pro export objekt proto není k dispozici. Jakmile obdrží objektu z sync engine, je připojený zdroj dat vytvoří jedinečnou hodnotu pro objektu atributu ukotvení.

Následující obrázek znázorňuje, jak exportovat objekt vytvořený s využitím identit informace v metaverse.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

Modul synchronizace potvrdí exportovat objektu tak, že znovu importujte objekt ze zdroje dat připojeného. Export objekty se stanou import objektů přijaté sync engine je při dalším importu z tohoto zdroje připojená data.

### <a name="placeholders"></a>Zástupné symboly
Modul synchronizace používá ploché obor pro uložení objektů. Některé zdroje dat připojeného například Active Directory však použít hierarchické názvů. Transformace informace z hierarchické názvů na plochou názvů, sync engine pomocí zástupných symbolů zachovat hierarchie.

Každý zástupný symbol představuje součástí (například organizační jednotce) objektu hierarchické název nebyla importují do sync engine, ale je nutné k vytvoření hierarchické název. Vyplňování mezery vytvořené odkazy ve zdroji připojená data na objekty, které nejsou přípravu objekty v prostoru spojnice.

Zástupné symboly modul synchronizovat taky používá k ukládání odkazované objekty, které dosud nebyly importovány. Například pokud synchronizace nakonfigurovaný tak, aby atribut správce pro *Abbie Spencer* objektu a přijaté hodnoty je objekt, který nebyla importovaných ještě, jako je třeba *KN = Lee Sperry CN = Uživatelé, Datacentrum = fabrikam Datacentrum = com*, správce informace se uloží jako zástupné symboly do pole spojnice. Pokud správce objekt je později, zástupný objekt přepíše pracovní objekt, který představuje vedoucího.

### <a name="metaverse-objects"></a>Metaverse objektů
Objekt metaverse obsahuje souhrnné zobrazení této sync engine má pracovní objektů do pole spojnice. Synchronizace engine vytvoří metaverse objektů pomocí informací v import objektů. Několik objektů místo spojnice můžou být propojené s metaverse jeden objekt, ale místo objekt připojovací nemohou být propojeny s více než jeden objekt metaverse.

Metaverse objekty nelze ručně vytvořit nebo odstranit. Modul synchronizace automaticky odstraní metaverse objekty, které nemají odkaz na libovolný objekt místo konektor do pole spojnice.

Objekty ve zdroji připojená data odpovídající typu objektu v rámci metaverse namapujete poskytuje sync engine schématem extensible pomocí předdefinovaných sady typů objektů a atributů přidružené. Můžete vytvořit nové typy objektů a atributů metaverse objektů. Atributy může být jeden nebo více hodnot a typy atributů může být řetězce, odkazy, čísla, logické hodnoty.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Relace mezi pracovní a metaverse objekty
V rámci oboru sync engine toku dat povolený podle vztahu propojení mezi pracovní a metaverse objekty. Pracovní objekt, který je propojený objekt metaverse se nazývá **připojen objektu** (nebo **objekt připojovací**). Přípravná objekt, který není propojený objekt metaverse se nazývá **odebrán objektu** (nebo **odpojovače objekt**). Podmínky připojeni a odebrán jsou upřednostňované není zaměnit se spojnicemi zodpovědný za import a export dat z připojeného adresáře.

Zástupné symboly se nikdy připojeného k objektu metaverse

Objekt spojených obsahuje pracovní objektu a jeho propojené vztah k objektu jednoho metaverse. Spojené objekty se používají k synchronizaci hodnoty atributu mezi objektem spojnice místa a metaverse objektu.

Jakmile objekt pracovní spojených objekt při synchronizaci, můžete mezi pracovní objektu a objekt metaverse tok atributy. Atribut toku obousměrnou komunikaci a jsou nakonfigurované pomocí pravidel atribut import a export atribut.

Pouze jeden objekt metaverse jde propojit objekt místo jednu spojnici. Však jednotlivé objekty metaverse jde propojit s více spojnice objekty místa ve stejném nebo v různých spojnice mezery, jak je znázorněno na následujícím obrázku.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

Propojené vztah mezi pracovní objektu a objekt metaverse je trvalý a je možné odstranit pouze pomocí pravidla, které zadáte.

Samostatném jedná se o pracovní objekt, která nepoužívá k libovolným objektům metaverse. Atribut, který samostatném objektu jsou zpracovány některé další v rámci metaverse. Hodnoty atributu odpovídající objekt ve zdroji připojená data se neaktualizují modulem synchronizace.

Pomocí samostatném objektů můžete ukládat informace identity sync engine a zpracovat později. Uchovávání objekt pracovní samostatném ve jako objekt prostor konektoru má mnoho výhod. Protože systém byl již připravili požadovaných informací o tento objekt, není nutné vytvořit podobu tento objekt znovu při dalším importu ze zdroje dat připojeného. Tímto způsobem sync engine obsahuje snímek dokončení zdroje dat připojeného vždy i v případě, že není aktuální připojení ke zdroji připojená data. Samostatném objekty lze převést do spojených objektů a číslování na odrážky, v závislosti na tato pravidla, které zadáte.

Objekt importu se vytvoří jako objekt samostatném. Export objekt musí být spojených objektu. Systém logiku vynucuje toto pravidlo a odstraní každý exportovat objekt, který není spojených objekt.

## <a name="sync-engine-identity-management-process"></a>Proces správy identit engine synchronizace
Proces správy identit Určuje, jak bude aktualizován identity informací mezi různých propojených zdrojů dat. Správa identit se skládá ze tří procesy:

- Import
- Synchronizace
- Export

Během procesu importu sync engine vyhodnocuje příchozí identity informace ze zdroje dat připojeného. Když nejsou nalezeny změny, se vytvoří nové pracovní objekty nebo aktualizuje existující pracovní objekty na spojnici místo pro synchronizaci.

Během procesu synchronizace sync engine aktualizuje metaverse odrážet změny, které se uskutečnily do pole spojnice a prostor konektoru odrážet změny, ke kterým došlo v metaverse se aktualizuje.

Při exportu vytvoříte sync engine posune změny, které jsou připravené na přípravu objekty a, které jsou označeny jako čekající na exportovat.

Následující obrázek znázorňuje místo, kam každá procesů se vyskytuje jako toků identity informace z jednoho zdroje dat připojeného do jiného.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Proces importu
Během procesu importu vyhodnotí sync engine aktualizace identity informace. Synchronizace modul porovnává informace identitě přijatou ze zdroje dat připojeného identity informace o objekt pracovní a určuje, zda pracovní objekt vyžaduje aktualizace. V případě potřeby aktualizujte pracovní objekt novými daty, pracovní objekt označena jako čekající na importovat.

Tak, že pracovní objekty v prostoru spojnice před synchronizací sync engine zpracovat pouze identity informace, které se změnily. Tento proces poskytuje následující výhody:

- **Efektivní synchronizace**. Velikost dat zpracovaných při synchronizaci se zmenší.
- **Efektivní synchronizace**. Jak sync engine zpracovává informace identity bez opětovné připojení modul synchronizace ke zdroji dat můžete změnit.
- **Možnost zobrazit náhled synchronizace**. Můžete zobrazit náhled synchronizace ověřte správnost předpokladů o proces správy identit.

Pro každý objekt podle spojnice modul Synchronizace nejdřív pokusí se najít vyjádření objektu do pole spojnice spojnice. Synchronizace engine kontroluje všechny objekty pracovní prostor konektoru a pokusí se najít odpovídající pracovní objekt, který obsahuje atribut odpovídající ukotvení. Pokud žádný existující objekt pracovní odpovídající ukotvení atribut, synchronizace pokusí se najít odpovídající pracovní objekt se stejným názvem rozlišující.

Když sync engine najde pracovní objekt, který odpovídá rozlišující název, ale ne ukotvení, dojde k následující speciální chování:

- Pokud má objekt umístěn na místo spojnice bez ukotvení, sync engine odebere z prostor konektoru tento objekt a označí metaverse objekt, který je spojený s jako **Opakovat zřizování při další synchronizaci spustit**. Potom vytvoří nový objekt importovat.
- Pokud objektu nachází v prostor konektoru má na ukotvení, sync engine se předpokládá, že tento objekt je buď přejmenovat nebo odstraněný v adresáři připojení. Slouží k přiřazování dočasné, nové rozlišující název objektu místo spojnice tak, aby ho fáze příchozí objekt. Původní objekt pak bude **přechodná**, čeká se na spojnice k přejmenování nebo odstranění řešení situace importovat.

Pokud sync engine najde pracovní objekt, který odpovídá objektu podle spojnice, určuje, jaký druh změny platit. Sync engine může přejmenovat nebo odstranit objektu ve zdroji připojená data nebo ho může aktualizovat pouze hodnoty objektu atributu.

Pracovní objektů s aktualizovaná data jsou označeny jako čekající na importovat. Různé druhy čeká na vyřízení importy jsou k dispozici. V závislosti na výsledek importu objektu pracovní prostor konektoru má jednu z těchto čeká na vyřízení typy importu:

- **Žádná**. Bez změny všech atributů pracovní objektu jsou k dispozici. Synchronizace engine neoznačí tento typ jako čekající na importovat.
- **Přidat**. Přípravná objekt je nový objekt importovat do pole spojnice. Synchronizace engine příznaky tento typ až import pro další zpracování v metaverse.
- **Aktualizace**. Synchronizace modul najde odpovídající objekt pracovní prostor konektoru a označeno tento typ jako čeká na vyřízení import a aktualizace atributů můžete zpracovávají v metaverse. Aktualizace obsahují přejmenování objektu.
- **Odstranit**. Synchronizace modul najde odpovídající objekt pracovní prostor konektoru a označeno tento typ jako čeká na vyřízení import spojených objekt můžete odstranit.
- **Přidat nebo odstranit**. Sync engine najde odpovídající objekt pracovní prostor konektoru, ale typy objektů se neshodují. V tomto případě odstranit přidat připravené poslední změny. A odstranit přidejte změny označuje modulu synchronizace, aby dokončení synchronizace tento objekt musí spadat protože různé sady pravidel použít pro tento objekt při objekt zadejte změny.

Nastavit stav na zjištění stavu úkolů importovat pracovní objektu, je možné výrazně zmenšit velikost dat zpracovaných při synchronizaci, protože tím umožňují systému zpracuje jenom objekty, které jste aktualizovali data.

### <a name="synchronization-process"></a>Proces synchronizace
Synchronizace se skládá ze dvou souvisejících procesů:

- Při aktualizaci obsahu metaverse pomocí data do pole konektor příchozí synchronizace.
- Odchozí synchronizace při aktualizaci obsahu místa spojnice pomocí data v metaverse.

Pomocí informací připravené na místo konektor příchozí synchronizaci vytvoří v metaverse integrované zobrazení data, která je uložena v zdroje dat připojeného. Všechny objekty pracovní nebo jenom ty s informacemi o čekání na import agregován, v závislosti na konfiguraci pravidla.

Proces aktualizace odchozí synchronizace exportovat objekty Pokud dojde ke změně metaverse objekty.

Příchozí synchronizace vytvoří zobrazení integrované v metaverse identity informace, které se dostali ze zdroje dat připojeného. Synchronizace engine můžete procesu identit kdykoli pomocí nejnovější identit informace, které má ze zdroje dat připojeného.

**Příchozí synchronizace**

Příchozí synchronizace zahrnuje následující postupy:

- **Poskytování** (nazývané také **projekce** Pokud je třeba odchozí synchronizace zřizování rozlišit tento proces). Modul synchronizace vytvoří nový objekt metaverse založený na objekt pracovní a jejich spojení. Poskytování je operaci úrovni objektů.
- **Připojit se**. Modul synchronizace odkazuje na existující objekt metaverse objekt pracovní. Spojení je operaci úrovni objektů.
- **Import atribut toku**. Synchronizace engine aktualizovat hodnoty atributu s názvem atribut toku objektu v metaverse. Import atribut tok je úroveň atributu operaci, která vyžaduje propojení mezi pracovní objektu a objekt metaverse.

Poskytování je pouze proces, který vytváří objektů v metaverse. Poskytování ovlivní pouze import objektů, které jsou samostatném objekty. Při poskytování vytvoří sync engine metaverse objekt, který odpovídá typu objektu importovaného objektu a vytvoří vazbu mezi oba objekty, čímž vytvoříte spojených objekt.

Proces připojení také vytvoří vazbu mezi import objektů a metaverse objekt. Rozdíl mezi spojení a poskytování je, že proces připojení vyžaduje importovaného objektu jsou propojené s existující metaverse objektu, kde procesu poskytování vytvoří nový objekt metaverse.

Synchronizace pokusí připojit objekt importu k objektu metaverse pomocí kritéria, která je podle konfigurace synchronizace pravidla.

Během procesu poskytování a připojit se ke sync engine odkazy na samostatném objekt na objekt metaverse organizováním připojen. Po dokončení těchto operací úrovni objektů sync engine můžete aktualizovat hodnoty atributu přidružené metaverse objektu. Tento proces se nazývá toku atribut importovat.

Import atribut tok probíhá na všechny import objektů, které obsahují nová data a jsou propojené s metaverse objektu.

**Odchozí synchronizace**

Aktualizace odchozí synchronizace exportovat objektů, pokud objekt metaverse změnit, ale není odstraněna. Cílem odchozí synchronizace je chcete zjistit, zda změny metaverse objektů vyžadovat aktualizace pracovní objekty do polí spojnice. V některých případech může vyžadovat změny tohoto pracovní objekty v všechny mezery spojnice aktualizovat. Pracovní objekty, které byly změněny budou označena jako čekající na exportovat, a je exportovat objekty. Tyto objekty jsou později použiti na zdroje dat připojeného při exportu vytvoříte exportovat.

Odchozí synchronizace má tří procesy:

- **Zřízení**
- **Zrušení zajišťování**
- **Export atribut toku**

Vytváření a zrušení zajišťování jsou operace úrovni objektů. Zrušení zajišťování závisí na zřizování protože pouze zřizování můžete zahájit ho. Zrušení zajišťování se aktivuje, když zřizujete odebere propojení mezi metaverse objektu a objekt exportovat.

Zřízení vždy dojde, když změny budou provedeny objektů v metaverse. Při změně metaverse objektů, sync engine můžete dělat tyto věci jako součást procesu zřizovací:

- Vytvoření spojených objektů, kde je objekt metaverse připojeného k objektu nově vytvořený exportovat.
- Přejmenování spojených objektu.
- Disjoin propojení mezi objektem metaverse a přípravu objektů, vytváření samostatném objektu.

Vyžaduje-li zřizování sync engine vytvořit nový objekt spojnice, pracovní objekt, ke kterému je propojený objekt metaverse je vždy exportovat objekt, protože objektu ve zdroji připojená data ještě neexistuje.

Pokud zřizování vyžaduje modul Synchronizace disjoin spojených objekt vytváření objektu samostatném zrušení zajišťování se při spuštění aktivují. Proces deprovisioning odstraní objekt.

Během zrušení zajišťování, odstranění objektu export neodstraní fyzicky objektu. Objekt označena jako **odstranili**, což znamená, že je připraven operace odstranění se nezobrazuje na požadovaném objektu.

Export atribut tok taky probíhá během odchozí synchronizace, podobně jako, import atribut tok probíhá během příchozí synchronizace. Export atribut tok probíhá pouze mezi metaverse a export objekty, kteří se připojili.

### <a name="export-process"></a>Operace exportu
Při exportu vytvoříte sync engine kontroluje všechny export objektů, které jsou označeny jako čekající na Exportovat do pole spojnice a potom odešle aktualizace zdroje dat připojeného.

Modul synchronizace můžete určit úspěch export ale nemůže dostatečně určit, dokončení procesu správy identit. Objekty ve zdroji připojená data vždy bude možné měnit jinými postupy. Protože sync engine nemá trvalé připojení ke zdroji připojená data, nestačí aby předpoklady o vlastnosti objektu ve zdroji připojená data založeny pouze na oznámení úspěšný export.

Příklad obrázku ve zdroji připojená data může změnit atributy objektu zpátky na původní hodnoty (tedy zdroje dat připojeného může přepsat hodnoty bezprostředně po použiti modulem synchronizace a úspěšně nastavili v zdroje dat připojeného data).

Úložišti sync engine export a import informací o stavu o jednotlivé pracovní objekty. Pokud od posledního exportu změnily se hodnoty atributů, které jsou uvedené v seznamu zahrnutí atribut, ukládání import a export stav umožňuje sync engine reagovat řádně podporovat. Synchronizace stroj používá importu potvrďte atributů, které byly exportovány ke zdroji připojená data. Srovnání importovaných a exportovat informace, jak je znázorněno na následujícím obrázku zařazují umožňuje synchronizace stroj zjistit, zda byl úspěšný exportovat nebo pokud je třeba opakovat.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Například pokud synchronizace modul exportuje atribut, který má hodnotu 5, ke zdroji připojená data, uloží C = 5 v paměti stav exportovat. Každý další export na tento objekt výsledků při pokusu o C = 5 ke zdroji připojená data data znova vyexportovat, protože sync engine předpokládá, že tato hodnota byla nepoužil trvale objektu (Pokud jinou hodnotu importovaného naposledy ze zdroje dat připojeného). Export paměti je zrušeno po přijetí C = 5 během operace importu na požadovaném objektu.

## <a name="next-steps"></a>Další kroky
Další informace o konfiguraci [Azure AD Connect synchronizovat](active-directory-aadconnectsync-whatis.md) .

Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
