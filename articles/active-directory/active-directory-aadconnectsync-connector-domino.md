<properties
   pageTitle="Spojnice Domino Lotus | Microsoft Azure"
   description="Tento článek popisuje, jak nakonfigurovat spojnice Domino Lotus společnosti Microsoft."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="lotus-domino-connector-technical-reference"></a>Technické informace o Lotus Domino spojnice
Tento článek popisuje Lotus Domino spojnice. Článek se týká těchto produktů:

- Správce Microsoft identit 2016 (MIM2016)
- Správce identit Forefront 2010 R2 (FIM2010R2)
    -   Musíte použít oprava 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 a FIM2010R2 spojnice je k dispozici ke stažení z [Webu služby Stažení softwaru](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Základní informace o konektoru Lotus Domino
Spojnice Domino Lotus umožňuje integrace služby synchronizace s společnosti IBM Lotus Domino serveru.

Z uceleném pohledu tyto funkce jsou podporovány ální konektoru:

Funkce | Podpora
--- | ---
Připojený zdroj dat | Server: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Klient:<li>Aplikace Lotus Notes 9.x</li>
Scénáře | <li>Správa životního cyklu objektu</li><li>Správa skupiny</li><li>Správa hesel</li>
Operace | <li>Úplné a Delta Import</li><li>Export</li><li>Nastavení a změnit heslo na HTTP heslo</li>
Schéma | <li>Osoba (cestovní uživatel kontaktu (osoby s žádný certifikát))</li><li>Skupina</li><li>Zdroje (zdroje, místnosti schůzky)</li><li>Mail v databázi</li><li>Dynamické zjišťování atributy pro podporované objekty</li>

Spojnice Lotus Domino používá klienta Lotus Notes pro komunikaci s Lotus Domino serveru. V důsledku tuto závislost musí být nainstalovaný podporované klienta Lotus Notes na server synchronizace. Komunikace mezi klientem a server je implementovaná prostřednictvím rozhraní Lotus Notes .NET interoperability (Interop.domino.dll). Rozhraní usnadňuje komunikaci mezi Microsoft.NET platformy a Lotus Notes klienta a podporuje přístup k dokumentům Lotus Domino a zobrazení. K importu delta je také možné, že původní rozhraní C++ využívá (v závislosti na metodě import vybrané delta).

### <a name="prerequisites"></a>Zjistit předpoklady pro
Před použitím spojnice, zkontrolujte, jestli že máte následující na serveru synchronizace:

- 4.5.2 rozhraní Microsoft .NET Framework nebo novější
- Klient Lotus Notes musí být na serveru nainstalovány? synchronizace
- Konektor Domino Lotus vyžaduje výchozí Lotus Domino LDAP schématu databáze (schema.nsf) měly tvořit na serveru Domino adresář. Pokud nenachází, můžete ji nainstalovat tak, že systém nebo restartovat službu LDAP na serveru Domino.

### <a name="connected-data-source-permissions"></a>Připojení zdroje dat oprávnění
Provádět všechny podporované úlohy v Lotus Domino spojnice, musíte být členem skupiny následující:

- Úplný přístup správce
- Správci
- Správci databáze

V následující tabulce jsou uvedeny oprávnění, která jsou potřeba pro každou akci:

Operace | Zadání přístupových práv
--- | ---
Import | <li>Čtení veřejné dokumentů</li><li> Úplný přístup správce (když jste členem skupiny administrators plný přístup, automaticky máte efektivní přístup k v ACL.)</li>
Export a nastavení hesla | Efektivní Access: <li>Vytváření dokumentů</li><li>Odstranění dokumentů</li><li>Čtení veřejné dokumentů</li><li>Psaní veřejné dokumenty</li><li>Replikace nebo zkopírovat dokumenty</li>Pro operace exportu musíte taky následující role: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li>

### <a name="direct-operations-and-adminp"></a>Přímé operací a AdminP
Operace buď přejít přímo do adresáře Domino nebo procesem AdminP. Následující seznam podporované objekty, operace tabulky a v případě potřeby metodu související implementaci:

**Primární adresáře**

Objekt | Vytvoření | Aktualizace | Odstranění
--- | --- | --- | ---
Osoba | AdminP | Přímé | AdminP
Skupina | AdminP | Přímé | AdminP
MailInDB | Přímé | Přímé | Přímé
Zdroje | AdminP | Přímé | AdminP

**Sekundární adresáře**

Objekt | Vytvoření | Aktualizace | Odstranění
--- | --- | --- | ---
Osoba | NENÍ K DISPOZICI | Přímé | Přímé
Skupina | Přímé | Přímé | Přímé
MailInDB | Přímé | Přímé | Přímé
Zdroje | NENÍ K DISPOZICI | NENÍ K DISPOZICI | NENÍ K DISPOZICI

Při zdroje je vytvořeno dokumentu poznámky. Podobně při odstranění zdroje dokument poznámek je odstranit.

### <a name="ports-and-protocols"></a>Porty a protokoly
IBM Lotus Notes klienta a servery Domino komunikovat pomocí poznámky vzdálené řízení volání (NRPC) kde NRPC používejte TCP/IP. Výchozí číslo portu je 1352, ale bude možné měnit správcem Domino.

### <a name="not-supported"></a>Nepodporovaná funkce
Tyto operace nepodporuje ální konektoru Lotus Domino:

- Přesunutí poštovní schránky mezi servery.

## <a name="create-a-new-connector"></a>Vytvoření nové spojnice

### <a name="client-software-installation-and-configuration"></a>Software instalace a konfigurace klienta
Lotus Notes musí být nainstalovaný na serveru **před** nainstalovaným spojnice.

Při instalaci, zkontrolujte, že provedete **Jednu uživatel nainstalovat**. Výchozí nastavení **Více uživatelů instalace** nebude fungovat.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Na stránce funkce nainstalujte jenom požadované funkce Lotus Notes a **Přihlášení jednoho klienta**. Jeden přihlašovací je potřebný pro spojnice tak, aby mohli přihlásit k serveru Domino.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Poznámka:** Na začátku Lotus Notes jednou uživatel, který je umístěn na stejný server jako účet, který použijete jako účet služby spojnice. Ujistěte se také zavřete klienta Lotus Notes na serveru. Nemůžete používat ve stejnou dobu, kterou spojnice pokouší připojit k serveru Domino.

### <a name="create-connector"></a>Vytvoření spojnice
Vytvoření Lotus Domino spojnice, služby **Synchronizace** vyberte **Agent pro správu** a **vytvořit**. Vyberte spojnici **Lotus Domino (Microsoft)** .  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Pokud si verzi synchronizační službu nabízí možnost konfigurace **Architektura**, ujistěte se, že konektoru je nastavený na výchozí hodnotu pro spuštění **procesu**.

### <a name="connectivity"></a>Připojení
Na stránce připojení musí zadat název serveru Lotus Domino a zadejte přihlašovací údaje.  
![Připojení](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Vlastnost Domino Server podporuje dva formáty název serveru:

- Webu název serveru
- Webu název serveru/DirectoryName

Formát **Webu název serveru/DirectoryName** je upřednostňované formát atributu, protože poskytuje rychlejší odpovědi při spojnice kontaktuje Domino Server.

Zadané uživatelské ID soubor uložený v databázi konfigurace služby synchronizace.

K **Importu Delta** máte tyto možnosti:

- **Žádná**. Spojnice nedělá všechny delta importy.
- **Přidání a aktualizace**. Import spojnice slouží delta přidávají a aktualizují operace. Odstranit je potřeba **Úplné** importu. Tuto operaci používá interoperability .net.
- **Přidání, aktualizace nebo odstranění**. Import delta slouží spojnice přidat, aktualizovat a odstraňovat operace. Tuto operaci používá nativní C++ rozhraní.

V dialogovém okně **Možnosti schématu** máte následující možnosti:

- **Výchozí schéma**. Spojnice rozpozná schématu ze serveru Domino. Toto je výchozí možnost.
- **DSML schéma**. Používá pouze v případě Domino server nezobrazuje schéma. Můžete pak vytvořit soubor DSML se schématem a importujte ho. Další informace o DSML najdete v tématu [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Po klepnutí na tlačítko Další ověření se parametry konfigurace uživatelské ID a heslo.

### <a name="global-parameters"></a>Globální parametry
Na stránce globální parametry konfigurace časové pásmo a import a export operace možnost.  
![Globální parametry](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

Parametr **časové pásmo serveru Domino** definuje umístění Domino serveru.

Tuto možnost konfigurace je potřebný pro podporu **delta importovat** operací, protože umožňuje synchronizace služby zjistit změny mezi poslední dvě importy.

#### <a name="import-settings-method"></a>Nastavení importu, metody
**Provést úplné Import tak, že** má tyto možnosti:

- Hledání
- Zobrazení (doporučeno)

**Hledání** používá indexování v Domino, ale je běžné indexy se neaktualizují v reálném čase a data vrácená ze serveru vždy správná není. Režim s mnoha změny tato možnost obvykle nefunguje dobře a poskytuje NEPRAVDA odstraní v některých případech. **Hledání** se však rychlejší než **zobrazení**.

**Zobrazení** je doporučené možnost od poskytuje stavu správná data. Je mírně nižší než vyhledávání.

#### <a name="creation-of-virtual-contact-objects"></a>Vytváření virtuálních kontaktů objektů
**Povolit vytváření \_objekt kontakt** má tyto možnosti:

- Žádná
- Hodnoty-Reference
- Odkazy a -Reference hodnoty

V Domino může obsahovat odkaz atributy mnoha různými formáty jako odkaz na další objekty. Budete moci představují různé varianty, implementuje spojnice \_kontaktujte objektů, nazývaný také **Virtuální kontakty** (VC). Tyto objekty jsou vytvořené, můžete připojení k existující objekty více hodnot nebo promítat jako nových objektů. Tímto způsobem můžete uchovávat atribut odkazy.

Povolením toto nastavení a obsahu atribut odkaz není formátu DN \_objekt kontakt se vytvoří. Atribut člena skupiny, například může obsahovat adresy SMTP. Je také možné shortName a další atributy účastní atributy odkaz. V tomto scénáři vyberte **Hodnoty Non-Reference**. Této konfiguraci je nejběžnější nastavení pro Domino implementace.

Když Lotus Domino nakonfigurovaný mít samostatný adresáře s různými rozlišující názvy představující stejný objekt, je možné vytvořit také \_kontaktovat objekty pro všechny referenční hodnoty, které se nacházejí v adresáři. V tomto scénáři vyberte možnost **odkaz a hodnoty Non-Reference** .

Pokud máte víc hodnot v atributu **FullName** Domino, pak taky chcete povolit vytváření virtuálních kontakty, tak odkazy můžete vyřešit. Tenhle atribut například může obsahovat více hodnot po svatbu nebo rozvodu. Zaškrtněte políčko Povolit **... FullName s více hodnotami** pro tento scénář.

Spojením na správné atributy \_kontakt objekty by být připojen k objektu více hodnot.

Tyto objekty mají VC =\_přidat kontakt do jejich DN.

#### <a name="import-settings-conflict-object"></a>Import nastavení, konfliktu objektu
**Vyloučení konflikt objektu**

V velké implementaci Domino je možné, že stejný název domény kvůli problémům s replikace k více objektů. V těchto případech uvidí spojnice dva objekty různých UniversalIDs ale stejný název domény. Konflikt způsobí přechodná objekt vzniku do pole spojnice. Spojnice můžete ignorovat objekty, které jste vybrali v Domino jako replikace obětí. Doporučujeme zachovat toto políčko zaškrtnuté.

#### <a name="export-settings"></a>Export nastavení
Pokud není vybrána možnost **Použít AdminP aktualizace odkazů** , export atributy odkaz, jako jsou například člena, je přímé volání a nepoužívá AdminP proces. Tuto možnost používejte jenom AdminP není nakonfigurován pro zachování referenční integritu.

#### <a name="routing-information"></a>Informace o směrování
V Domino je možné, že atribut odkaz obsahuje informace o směrování vložené jako příponu názvu domény. Atribut členství ve skupině může obsahovat například **CN=example/organization@ABC**. Přípona @ABC je informace o směrování. Informace o směrování slouží Domino k posílat e-maily do správné Domino systému, které by mohly být systému v různých organizace. V poli směrování informací můžete určit směrování přípony použít v rámci organizace v rozsahu spojnice. Pokud jako příponu jednu z těchto hodnot najdete v atributu odkaz, informace o směrování se odebere z odkazu. Pokud k směrování přípony na hodnotu odkaz na jednu z těchto hodnot zadán, \_objekt kontakt se vytvoří. Tyto \_objekty kontaktu se vytvářejí ** RO=@ ** vložená do názvu domény. U těchto \_kontakt objekty povolit připojení na skutečný objekt v případě potřeby taky přidají následujícími atributy: \_routingName, \_kontakt, \_displayName a UniversalID.

#### <a name="additional-address-books"></a>Další adresáře
Pokud nemáte **telefonních číslech** nainstalovali, který obsahuje název vedlejší adresáře, můžete ručně zadat tyto adresáře.

#### <a name="multivalued-transformation"></a>Transformace s více hodnotami
Mnoho atributů v Lotus Domino jsou s více hodnotami. Odpovídající atributy metaverse jsou obvykle jeden hodnotami. Nakonfigurováním Import a Export operace možnost povolit spojnice tak, aby s požadované překladu problémového atributy pomoct.

**Export**  
Možnost operace exportu podporuje dvěma způsoby:

- Přidat položky
- Nahrazení položky

**Nahradit položku** – Pokud vyberete tuto možnost, konektoru vždy odeberte aktuální hodnoty atributu Domino a nahradit je zadané hodnoty. Poskytované vracejícími může být jedinou hodnotu nebo s více hodnotami.

Příklad: Atribut Pomocník objektu osoba obsahuje následující hodnoty:

- CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Pokud nové Pomocníka s názvem **David Alexandru** přiřazen k objektu uživatele, bude výsledek takto:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Připojit položku** – Pokud vyberete tuto možnost, spojnice zachová existujících hodnot na základě atributu Domino a vložit nové hodnot v horní části seznamu data.

Příklad: Atribut Pomocník objektu osoba obsahuje následující hodnoty:

- CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Pokud nové Pomocníka s názvem **David Alexandru** přiřazen k objektu uživatele, bude výsledek takto:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Import**  
Možnost operace importu podporuje dvěma způsoby:

- Výchozí
- S více hodnotami jediná hodnota

**Výchozí** – Pokud vyberete možnost výchozí, jsou všechny hodnoty všech atributů importovány.

**Multivalued jediná hodnota** – Pokud vyberete tuto možnost, atribut s více hodnotami bude převeden na atribut jedinou hodnotu. Pokud existuje více než jednu hodnotu, bude použita hodnota nahoře (tuto hodnotu obvykle je také poslední).

Příklad: Atribut Pomocník objektu osoba obsahuje následující hodnoty:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Poslední aktualizace Tenhle atribut je **David Alexandru**. Protože možnost operace importu je nastavena na Multivalued jedna hodnota, spojnice **David Alexandru** pouze importuje do místa spojnice.

Logika k převodu s více hodnotami atributy atributy s jednou hodnotou, nebudou mít vliv atribut člena skupiny a atribut fullname osoby.

Je také možné konfigurovat import a export pravidel transformace s více hodnotami atributů každý atribut, výjimečně globální pravidla. Pokud chcete nakonfigurovat tuto možnost, zadejte [vztah mezi typem objektu]. [Název_atributu] do **seznamu vyloučení atribut import** a **export seznamu vyloučení atribut** textových polí. Například pokud zadáte Person.Assistant a globální příznak je nastavený k importu všech hodnot, pouze první hodnotu naimportují pro pomocníka.

#### <a name="certifiers"></a>Udělení licence certifikátorům
Všechny organizace/organizační jednotky jsou uvedeny podle spojnice. Pokud chcete mít možnost Export osoba objekty do primární adresáře, certifier s jeho heslo požaduje sledování.

Máte všechny udělení licence certifikátorům stejné heslo, můžete použít **heslo pro všechny Certifers** . Pak můžete zadat heslo tady a zadat pouze soubor certifier.

Pokud pouze importujete, není nutné zadat libovolný udělení licence certifikátorům.

### <a name="configure-provisioning-hierarchy"></a>Konfigurace vytváření hierarchie
Nakonfigurováním konektoru Lotus Domino přeskočte tento dialogovou stránku. Konektor Lotus Domino nepodporuje hierarchie zřizování.  
![Vytváření hierarchie](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurace oddíly a hierarchie
Při konfiguraci oddíly a hierarchie, je nutné vybrat primární adresář s názvem NAB=names.nsf. Kromě primární adresář, který můžete vybrat sekundární adresáře, pokud existují.  
![Oddíly](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Vyberte atributy
Při konfiguraci vašeho atributy musíte vybrat všechny atributy, které jsou s předponou ** \_MMS\_**. Když zřizujete nové objekty Lotus Domino podporují tyto atributy

![Atributy](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Správa životního cyklu objektu
Tato část obsahuje základní informace o různých objektů v Domino.

### <a name="person-objects"></a>Osoba objektů
Objekt osoba představuje uživatelů v organizaci a organizaci jednotky. Kromě výchozí atributy správce Domino přidat vlastní atributy objektu osoby. Minimálně objekt osoba musí obsahovat všechny povinné atributy. Úplný seznam povinné atributy najdete v článku [Lotus Notes vlastnosti](#lotus-notes-properties). Zaregistrujte objektu uživatele, musí být splněny následující požadavky:

- Adresář (names.nsf) musí být definována a by mělo být primárním adresář.
- Musí mít certifier O/OU Id a heslo k registraci určitému uživateli v organizaci / organizační jednotku.
- Je třeba nastavit určité skupiny Lotus Notes vlastnosti objektu osoby. Tyto vlastnosti se používají pro zřizování objekt osoby. Další informace najdete v části s názvem [Lotus Notes vlastnosti](#lotus-notes-properties) dále v tomto dokumentu.
- Počáteční heslo HTTP osoby je atribut a nastavte během vytváření.
- Objekt osoba musí být jeden z následujících tří podporované typy:
    1. Normální uživatele, který má soubor pošty a soubor id uživatele
    2. Cestovní uživatele (normální, která obsahuje všechny cestovní soubory databáze)
    3. Kontakty (uživatel s žádný soubor id)

Osoby (s výjimkou kontaktů) je možné dál rozdělit na nám uživatelům a mezinárodní podle hodnoty \_MMS\_IDRegType vlastnost. Tyto osoby použijete klientovi poznámek pro přístup k serverům Lotus Domino mají Id poznámek a dokumentu osoby. Pokud používají hromadné poznámky, pak taky mají souboru hromadné. Uživatel musí být registrován k aktivaci. Další informace najdete v tématu:

- [Nastavení uživatelů poznámek](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [Registrace uživatele](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [Správa uživatelů](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [Přejmenování uživatelů](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Tyto operace jsou provést v Lotus Domino a pak importovat do služby synchronizace.

### <a name="resources-and-rooms"></a>Místnosti a prostředky
Kdy je zdroj databáze v Lotus Domino jiného typu. Zdroje mohou být konferenční místnosti pomocí různých typů zařízení, třeba projektory. Existují podtypy prostředků nepodporuje Lotus Domino spojnice, které jsou definovány atribut typ zdroje:

Typ zdroje | Atribut typ zdroje
--- | ---
Místnosti | 1
Zdroje (Další) | 2
Online schůzky | 3

Pro typ objektu zdroje pro práci je potřeba následující akce:

- Databáze rezervace prostředků by měl existovat v připojeném Domino serveru
- Na web je definován zdroje

Databáze rezervaci prostředku obsahuje tři typy dokumentů:

- Profil webu
- Zdroje
- Rezervace

Podrobné informace o nastavení databáze rezervaci prostředku najdete v článku [Nastavení databáze rezervace zdrojů](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Vytvářet, aktualizovat a odstraňovat zdroje**  
Operace vytvoření, aktualizace a odstranění provádí pomocí konektoru Lotus Domino v databázi rezervaci prostředku. Zdroje vytvářejí jako dokumenty v Names.nsf (to znamená primárního adresář). Podrobné informace o úpravy a odstranění materiály najdete v článku [Úpravy a odstraňování zdrojů dokumenty](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Import a Export operace pro zdroje**  
Zdroje můžete importovat do a vyexportovaný z synchronizace služby, jako je jiný typ objektu. Vyberte typ objektu jako zdroj během konfigurace. Pro úspěšné exportu měli byste mít podrobnosti pro typ zdroje, konference databáze a název webu.

### <a name="mail-in-databases"></a>Databáze příchozí pošty
Mail v databázi je databáze, která je určen pro příjem e-mailů. Je Lotus Domino poštovní schránky, který není přidružený k žádné konkrétní uživatelský účet Lotus Domino (to znamená nemá vlastní soubor ID a heslo). Mail v databázi obsahuje jedinečné ID ("krátký název") přidružené a vlastní e-mailovou adresu.

Pokud je potřeba samostatné poštovní schránka s vlastním e-mailovou adresu, kterou můžete sdílet mezi různými uživateli (například group@contoso.com), se vytvoří databázi pošty. Přístup k tuhle poštovní schránku řídí prostřednictvím jeho ovládací prvek seznam přístupu (ACL), která obsahuje názvy uživatelů poznámky, které jsou povoleny k otevření poštovní schránky.

Seznam má požadované atributy najdete v části s názvem [Povinné atributy](#mandatory-attributes) dál v tomto článku.

Když databáze je určen pro příjem Hromadná, vytvoří se dokument hromadné databáze ve Lotus Domino. Tento dokument musí existovat v adresáři Domino každého serveru, který ukládá kopii databáze. Podrobný popis o vytváření dokumentu hromadné v databázi najdete v článku [Vytvoření dokumentu hromadné v databázi](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Před vytvořením pošta v databázi, by měl existovat databáze (by měl vytvořil správce Lotus) na serveru Domino.

### <a name="group-management"></a>Správa skupiny
Podrobný přehled správy Lotus Domino skupiny můžete získat z následujících zdrojů:

- [Používání skupin](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [Vytvoření skupiny](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [Vytváření a úpravy skupiny](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [Správa skupin](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [Přejmenování skupiny](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Správa hesel
Uživatel registrovaných Lotus Domino existují dva typy hesel:

1. Heslo uživatele (uložené v souboru User.id)
2. Internet / HTTP heslo

Konektor Lotus Domino podporuje pouze operace heslem HTTP.

Provádět správu hesel byste měli povolit správu hesel konektoru v Návrháři Agent správy. Chcete-li povolit správu hesel, zaškrtněte políčko **Povolit Správa hesel** na stránce dialogové okno **Konfigurovat rozšíření** .  
![Konfigurace rozšíření](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Podpora spojnice Lotus Domino sledovat operace hesla k Internetu:

- Nastavení hesla: Nastavení hesla nastaví nové heslo protokolu HTTP/Internet uživatele systému Domino. Ve výchozím nastavení je také odemknout. Příznak odemknout se zobrazí v rozhraní WMI modul synchronizace.
- Změna hesla: V tomto scénáři uživatel možná budete chtít změnit heslo nebo se zobrazí výzva ke změně hesla po určité době. Pro tuto operaci mají provést, umístěte, (starou i nové heslo) povinná. Jakmile změníte, nového hesla se aktualizuje v Lotus Domino.

Další informace najdete v tématu:

- [Použití funkce uzamčení Internet](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [Správa hesel Internet](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Referenční informace
Tato část uvádí například atribut popisy a atribut požadavky pro Lotus Domino spojnice.

### <a name="lotus-notes-properties"></a>Vlastnosti aplikace Lotus Notes
Když zřizujete objekty osoby do vašeho adresáře Lotus Domino, objekty musí mít konkrétní sadu vlastností s určitých hodnot vyplněné. Tyto hodnoty jsou pouze potřebných pro vytvoření operací.

Následující tabulka uvádí těchto vlastností a obsahuje popis z nich.

Vlastnost | Popis
--- | ---
\_MMS_AltFullName | Alternativní celé jméno uživatele.
\_MMS_AltFullNameLanguage | Jazyk, který chcete použít pro zadání alternativního celé jméno uživatele.
\_MMS_CertDaysToExpire | Spočítá počet dnů od aktuálního data před certifikát vyprší jejich platnost. Pokud není zadán, je výchozí datum dva roky od aktuálního data.
\_MMS_Certifier | Vlastnost, která obsahuje název organizace hierarchie certifier. Příklad: OU = OrganizationUnit, O = organizační, C = země.
\_MMS_IDPath | Pokud vlastnost je prázdná, žádné identifikační soubor uživatel vytvoří se místně na serveru synchronizace. Obsahuje-li vlastnost název souboru, vytvoří se ve složce madata souboru ID uživatele. Vlastnost mohou také obsahovat úplnou cestu.
\_MMS_IDRegType | Kontakty, nám uživatelé a mezinárodní uživatele je možné rozdělit osoby. Následující tabulka obsahuje možné hodnoty: <li>0 - kontaktu</li><li>1 - US uživatele</li><li>2 – mezinárodní uživatele</li>
\_MMS_IDStoreType | Vlastnost je nutno zadat pro USA a mezinárodní uživatele. Vlastnost obsahuje celočíselnou hodnotu, která určuje, zda je uložen identifikace uživatele jako přílohu v adresáři poznámky nebo v souboru hromadné dané osoby. Pokud je soubor ID uživatele přílohu v adresáři, pokud chcete vytvořením jako soubor s \_MMS_IDPath. <li>Prázdné – ID úložiště souborů v ID trezoru žádné identifikace souboru (kontakty).</li><li> 1 – přílohu v adresáři poznámky. \_Musí být vlastnost MMS_Password nastavená pro uživatele identifikace soubory, které jsou přílohy</li><li>2 – obsahují ID uživatele pošty souboru. \_MMS_UseAdminP musí být nastavena na hodnotu false chcete, aby soubor pošty vytvořili při registraci osoby. \_Musí být vlastnost MMS_Password nastavená pro soubory identifikace uživatele.</li>
\_MMS_MailQuotaSizeLimit | Počet megabajtů, které jsou povoleny pro databázový soubor e-mailu.
\_MMS_MailQuotaWarningThreshold | Počet megabajtů, které jsou povoleny pro databázový soubor e-mailu před vystavením upozornění.
\_MMS_MailTemplateName | Soubor šablony e-mailu, který slouží k vytvoření souboru e-mailu uživatele. Pokud není zadán šablony, vytvoří se soubor pošty pomocí určité šablony. Pokud je zadána žádná šablona, výchozí šablony souboru slouží k vytvoření souboru.
\_MMS_OU | Volitelné vlastnost, která se jmenuje OU ve skupinovém rámečku certifier. Tato vlastnost by měl být prázdný kontaktů.
\_MMS_Password | Vlastnost je nutno zadat pro uživatele. Vlastnost obsahuje heslo pro identifikaci soubor objektu.
\_MMS_UseAdminP | Vlastnost měla být nastavena na hodnotu true, pokud soubor pošty by měla být vytvořili procesem AdminP na serveru Domino (asynchronní exportu vytvoříte). Pokud je nastavena na hodnotu NEPRAVDA, soubor pošty vytvoří se s uživatelem Domino (synchronní při exportu vytvoříte).

Pro uživatele se souborem přidružených identifikačních \_MMS_Password vlastnost musí obsahovat hodnotu. Vlastnosti poštovního serveru a MailFile uživatele pro přístup k e-mailu přes klienta Lotus Notes, musí obsahovat hodnotu.

Přístup k e-mailu pomocí webového prohlížeče, musí obsahovat následující vlastnosti hodnoty:

- MailFile - vlastnost je nutno zadat obsahující cestu na server Lotus Domino, kde je uložený soubor pošty.
- Poštovního serveru - vlastnost je nutno zadat obsahující název serveru Lotus Domino. Tato hodnota je název, který chcete používat při vytváření souborů aplikace Lotus mail na serveru Domino.
- HTTPPassword – volitelné vlastnost, která obsahuje heslo Web access objektu.

Vlastnost HTTPPassword pro přístup k serveru Domino bez možnosti pošty, musí obsahovat hodnotu. Vlastnosti MailFile a poštovního serveru může být prázdné.

S \_MMS_ IDStoreType = 2 (id úložiště v souboru hromadné), je vlastnost MailSystem NotesRegistrationclass nastavena na REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Povinné atributy
Konektor Lotus Domino hlavně nepodporuje tyto typy objektů (typy dokumentu):

- Skupina
- Mail v databázi
- Osoba
- Kontakt (osobě používající bez certifier)
- Zdroje

V této části jsou uvedeny atributy, které jsou pro jednotlivé podporované objekty exportovat do serveru Domino povinné.

Typ objektu | Povinné atributy
--- | ---
Skupina | <li>ListName</li>
Hlavní databáze | <li>FullName</li><li>MailFile</li><li>Poštovního serveru</li><li>MailDomain</li>
Osoba | <li>Příjmení</li><li>MailFile</li><li>ShortName</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li>
Kontakt (osobě používající bez certifier) | <li>\_MMS_IDRegType</li>
Zdroje | <li>FullName</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Web</li><li>DisplayName</li><li>MailFile</li><li>Poštovního serveru</li><li>MailDomain</li>

## <a name="common-issues-and-questions"></a>Běžné problémy a otázky

### <a name="schema-detection-does-not-work"></a>Schéma vyhledávání nefunguje
Pokud chcete mít možnost zjišťování schéma, je nutné, že soubor schema.nsf nachází na serveru Domino. Tento soubor se zobrazí, jenom Pokud LDAP je nainstalovaná na serveru. Pokud není schématu zjišťovat, postupujte následovně:

- Schema.nsf soubor neobsahuje data v kořenové složce Domino serveru
- Uživatel s oprávněním k zobrazení souboru schema.nsf.
- Vynuťte restart serveru LDAP. Spusťte **Konzolu Domino Lotus** a příkazem **Řekněte ReloadSchema LDAP** nové načtení schématu.

### <a name="not-all-secondary-address-books-are-visible"></a>Jsou zobrazeny některé sekundární adresáře.
Konektor Domino závisí na funkci **Telefonních číslech** moct najít sekundární adresáře. Pokud chybí sekundární adresáře, ověřte, pokud [Telefonních číslech](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) povolil a nakonfigurovaný na serveru Domino.

### <a name="custom-attributes-in-domino"></a>Vlastní atributy ve Domino
Existuje několik způsobů v Domino rozšíření schématu tak, že bude jako vlastní atribut spotřební pomocí konektoru.

**Postup 1: Rozšíření Lotus Domino schématu**

1. Vytvoření kopie Domino adresáře šablony {PUBNAMES. NTF} pomocí následujících [kroků](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (neměli přizpůsobíte adresáři IBM Lotus Domino výchozí šablony):
2. Otevření adresáře šablony kopírovat Domino {CONTOSO. Šablona NTF}, který byl vytvořený v Návrháři Domino a postupujte podle těchto kroků:
    - Klikněte na sdílené prvky a rozbalte podformulářů
    - Poklikejte na podformulář InheritableSchema ${Název_objektu} (kde {název_objektu} je název třídy výchozí strukturální objekt, například: osoba).
    - Název atribut, který chcete přidat do schématu {MyPersonAtrribute} a odpovídající atributu. Vytvoření pole podle vyberte nabídku **vytvořit** a pak vyberte **pole** v nabídce.
    - V poli Dodat nastavte tak, že vyberete jeho typ, styl, velikost, písma a dalších související parametrů v okně Vlastnosti pole jeho vlastnosti.
    - Zachovat atribut výchozí hodnota stejný jako název atributu (například-li název atributu MyPersonAttribute, uchovávat nebo rovná hodnotě se stejným názvem).
    - Uložení podformulář InheritableSchema ${název_objektu} s aktualizovanými hodnotami.
3. Nahrazení Domino adresáře šablony {PUBNAMES. NTF} s novou vlastní šablonu {CONTOSO. NTF} podle následujících [kroků](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Zavřete Domino správce a otevřete Domino konzoly služby LDAP a nové načtení schématu LDAP:
    - V konzole Domino vložte příkaz v nabídce **Příkazu Domino** text zařazeno ji spustit LDAP – [Restartujte LDAP úkolu]( http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    - Nové načtení LDAP schématu příkazem řekněte LDAP - řekněte ReloadSchema LDAP
5. Otevřít Domino správce a vyberte uživatelé a skupiny kartě najdete v článku přidaný atribut se projeví v domino přidat uživatele.
6. Otevřete Schema.nsf na kartu **soubory** a najdete v článku přidaný atribut se projeví do dominoPerson LDAP objekt třídy.

**Postup 2: Vytvoření auxClass vlastní atributem a přidružit třídu objektu**

1. Vytvoření kopie Domino adresáře šablony {PUBNAMES. NTF} podle následujících [kroků](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nikdy přizpůsobení adresáři IBM Lotus Domino výchozí šablony):
2. Otevření adresáře šablony kopírovat Domino {CONTOSO. Šablona NTF}, která byla vytvořená v Návrháři Domino.
3. V levém podokně vyberte sdílené kódu a potom podformulářů.
4. Klikněte na nový podformulář
5. Chcete-li zadat vlastnosti nový podformulář následujícím způsobem:
    - Pomocí nový podformulář otevřete, výběrem položky návrh – vlastnosti podformuláře
    - Vedle položky vlastnost Název zadejte název třídy pomocné objekt – například TestSubform.
    - Zachovat vlastnost možnosti "Zahrnout vložení podformulář... dialogové okno" vybrané
    - Zrušte výběr možnosti vlastnost "vykreslení průchodu prostřednictvím HTML v poznámkách."
    - Další vlastnosti chcete nechat stejnou a zavřete okno Vlastnosti podformulář.
    - Uložte a zavřete nový podformulář.
6. Postupujte podle následujících pokynů přidejte pole pro definování třídy pomocné objektu:
    - Otevřete podformuláře, kterou jste vytvořili.
    - Zvolte vytvořit - polí.
    - Vedle názvu na kartě základy v dialogovém okně pole, zadejte libovolný název, například: {MyPersonTestAttribute}.
    - V poli přidané nastavil její vlastnosti výběr jeho typ, styl, velikost, písma a související vlastnosti.
    - Zachovat atribut výchozí hodnota stejný jako název atributu (například-li název atributu MyPersonTestAttribute, uchovávat nebo rovná hodnotě se stejným názvem).
    - Uložte podformulář s aktualizovanými hodnotami a postupujte takto:
        - V levém podokně vyberte sdílené kód a pak podformulářů
        - Vyberte nový podformulář a výběrem položky návrh – vlastnosti návrhu.
        - Klikněte na kartu třetí zleva a vyberte **změnit rozšíření tento zákaz návrh**.
7. Otevřete podformulář ExtensibleSchema ${Název_objektu} (kde {název_objektu} je název třídy výchozí strukturální objekt, například – osoba).
8. Vložení prostředku vyberte podformulář (který jste vytvořili, například – TestSubform) a uložte podformulář ExtensibleSchema ${název_objektu}.
9. Nahrazení Šablona adresáře Domino {PUBNAMES. NTF} s novou vlastní šablonu {CONTOSO. NTF} podle následujících [kroků](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Zavřete Domino správce a otevřete Domino konzoly služby LDAP a nové načtení schématu LDAP:
    - V konzole Domino vložte příkaz v nabídce **Příkazu Domino** text zařazeno ji spustit LDAP – [Restartujte LDAP úkolu](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    - Nové načtení LDAP schématu příkazem řekněte LDAP **Řekněte ReloadSchema LDAP**.
11. Otevřete správce Domino a vyberte kartu uživatelé a skupiny zobrazíte přidaný atribut se projeví v domino přidat uživatele (v části jiné tab).
12. Otevřete Schema.nsf na kartu **soubory** a najdete v článku přidaný atribut se projeví v části TestSubform LDAP pomocné třídy objektu.

**Postup 3: Přidání vlastní atribut třídy ExtensibleObject**

1. Otevřít soubor {Schema.nsf} umístěný v kořenovém adresáři
2. Vyberte třídy LDAP objektů v nabídce nalevo v části **Všechny dokumenty schématu** a klikněte na tlačítko **Přidat objekt třídy** :
3. Zadejte název LDAP formou {zzzExtensibleSchema} (kde zzz je název třídy výchozí strukturální objekt, například osoba). Příklad rozšíření schématu jménem uživatele třídy objektů, zadejte název LDAP {PersonExtensibleSchema}.
4. Zadejte název třídy nadřazený objekt, u kterého chcete rozšířit schéma. Třeba rozšířit schématu třídy objektu uživatele, zadejte název třídy nadřazený objekt {dominoPerson}:
5. Zadejte platný ID objektu odpovídá třídě objektu.
6. Vyberte rozšířený/vlastní atributy ve skupinovém rámečku povinné a nepovinné atribut typy polí podle požadovaného:
7. Po přidání požadované atributy ExtensibleObjectClass, klikněte na **Uložit a zavřít**.
8. ExtensibleObjectClass se vytvoří odpovídajících výchozí třídy objekt s rozšířenými atributy.

## <a name="troubleshooting"></a>Řešení potíží

-   Informace o tom, jak povolit protokolování pro řešení potíží s spojnice zjistěte, [jak povolit trasování událostí pro Windows trasování spojnice](http://go.microsoft.com/fwlink/?LinkId=335731).
