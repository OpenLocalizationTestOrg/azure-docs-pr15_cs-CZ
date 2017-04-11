<properties
    pageTitle="Azure AD Connect synchronizace: Principy výchozí konfigurace | Microsoft Azure"
    description="Tento článek popisuje výchozí konfigurace v Azure AD Connect synchronizovat."
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
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect synchronizace: Principy výchozí konfigurace
Tento článek vysvětluje mimo pole konfigurační pravidla. Dokumenty pravidla a vliv konfigurace následujícími pravidly. Je taky vás provede výchozí konfigurace Azure AD Connect synchronizace. Cílem je, že čtenáře rozumí práci model konfigurace s názvem deklarativní zřizování v reálný příklad. V tomto článku se předpokládá, že jste již nainstalovali a konfigurace synchronizace Azure AD Connect pomocí Průvodce instalací.

Pokud chcete zjistit podrobnosti o model konfigurace, přečtěte si [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Pravidla mimo pole z místního Azure AD
Následující výrazy najdete v části krabice konfiguraci.

### <a name="user-out-of-box-rules"></a>Uživatele mimo pole pravidel
Tato pravidla platí taky typu objektu iNetOrgPerson.

Objekt uživatele musí být splněny následující k synchronizaci:

- Musí mít sourceAnchor.
- Po vytvoření objektu v Azure AD nelze změnit sourceAnchor. Pokud je argument hodnota změněné místního, objekt přestane synchronizace až sourceAnchor zpátky na původní hodnotu.
- Musí mít atribut accountEnabled (Řízení účtu uživatele) vyplněné. S místní služby Active Directory, Tenhle atribut je vždy prezentovat a vyplněna.

Následující objekty uživatele **nejsou** synchronizovány Azure AD:

- `IsPresent([isCriticalSystemObject])`. Zajištění mnoho mimo pole objekty ve službě Active Directory, třeba předdefinovaný účet správce, nejsou synchronizovány.
- `IsPresent([sAMAccountName]) = False`. Zajistěte, aby uživatel objektů s žádný atribut sAMAccountName nejsou synchronizovány. Tento případ by jenom prakticky dojít v doméně upgrade z systému Windows NT 4.
- `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Nesynchronizují účet služby používaný Azure AD Connect synchronizace a v dřívějších verzích.
- Nesynchronizují účtů Exchange, které nebude fungovat v Exchange Online.
    - `[sAMAccountName] = "SUPPORT_388945a0"`
    - `Left([mailNickname], 14) = "SystemMailbox{"`
    - `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
    - `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
- Nesynchronizují objekty, které nebude fungovat v Exchange Online.
`CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
Tento maskování bitů (a H21C07000) by odfiltrovat následující objekty:
    - Podporou poš veřejné složky
    - Telefonických poštovní schránky systému
    - Poštovní schránka poštovní schránky databáze (poštovní schránky systému)
    - Univerzální skupiny zabezpečení (by vhodná pro uživatele, ale je k dispozici důvodů starší verze)
    - Skupina non-univerzální (by vhodná pro uživatele, ale je k dispozici důvodů starší verze)
    - Plán poštovní schránky
    - Schránky Discovery Mailbox
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Všechny objekty obětí replikace není synchronizováno.

Platí následující pravidla atribut:

- `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. Atribut sourceAnchor není přispěla z propojené poštovní schránky. Předpokládá se, že pokud našel propojené poštovní schránky skutečný účet připojen později.
- Exchange související atributy se synchronizují pouze pokud atribut **mailNickName** obsahuje hodnotu.
- Pokud existuje více strukturami, atributy spotřebované v následujícím pořadí:
    1. Související s přihlašovací atributy (například userPrincipalName) jsou přispěla z struktuře s účtem povolené.
    2. Atributy, které najdete v GAL Exchange (globální adresář) jsou přispěla z struktuře s poštovní schránkou Exchange.
    3. Pokud najdete žádné poštovní schránky, tyto atributy můžou pocházet z libovolné doménové.
    4. Exchange související atributy (technická atributy skryté z globálního seznamu adres) jsou přispěla z struktuře kde `mailNickname ISNOTNULL`.
    5. Pokud existuje více strukturami, které splňují jedno z těchto pravidel, vytvoření pořadí (datum a čas) spojnic (strukturami) slouží k určení, které doménové přispívá atributy.

### <a name="contact-out-of-box-rules"></a>Obraťte se na mimo pole pravidel
Objekt kontaktu musí být splněny následující k synchronizaci:

- Kontakt musí být poštovní. Ověření pomocí následujících pravidel:
    - `IsPresent([proxyAddresses]) = True)`. Musí být vyplněno atributu proxyAddresses.
    - Primární e-mailovou adresu najdete v atributu proxyAddresses nebo atribut pošta. Informace o stavu z @ slouží k ověření, že obsah je e-mailovou adresu. Jednu z těchto dvou pravidla musí být vyhodnocen jako True.
        - `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Existuje položku s "SMTP:" a pokud existuje, můžete @ nalézt v řetězci?
        - `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Atribut hromadné vyplněné a pokud ano, můžete @ nalézt v řetězci?

Následující kontaktů objekty **nejsou** synchronizovány Azure AD:

- `IsPresent([isCriticalSystemObject])`. Zajistit, aby se synchronizují žádné kontaktů objekty označeny jako důležité. Nesmí být v kterémkoliv s výchozí konfigurace.
- `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
- `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Tyto objekty nebude fungovat v Exchange Online.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Všechny objekty obětí replikace není synchronizováno.

### <a name="group-out-of-box-rules"></a>Seskupení mimo pole pravidel
Objektu skupiny musí být splněny následující k synchronizaci:

- Musí být menší než 50 000 členy. Počet je počet členů ve skupině místní.
    - Pokud je nějaký další členové před prvním spuštění synchronizace, se nesynchronizuje skupiny.
    - Pokud počet členů postupně se zvětšují z ho původně vytvoření, potom ho dosáhne-li více než 50 000 členy ukončení synchronizace nastavte počet členství nižší než 50 000 znovu.
    - Poznámka: Počet více než 50 000 členství je také tak, že Azure AD vynucené. Nejste schopni synchronizace skupin s víc členů, i když upravit nebo odebrat toto pravidlo.
- Pokud je skupina **Distribuční skupiny**, pak musí být také hromadné povolené. Najdete v tématu že nevynucují [kontakt mimo pole pravidla](#contact-out-of-box-rules) pro toto pravidlo.

Následující objekty skupiny **nejsou** synchronizovány Azure AD:

- `IsPresent([isCriticalSystemObject])`. Zajištění mnoho mimo pole objekty ve službě Active Directory, jako jsou předdefinované skupiny administrators, nejsou synchronizovány.
- `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Používá DirSync starší verze skupiny.
- `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Skupiny rolí.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Všechny objekty obětí replikace není synchronizováno.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal mimo pole pravidel
FSP jsou spojeny se"" (\*) objekt v metaverse. Ve skutečnosti tento spojení pouze neděje pro uživatele a skupiny zabezpečení Konfigurace zajišťuje, že v rámci struktury členství jsou vyřešit a představující správně v Azure AD.

### <a name="computer-out-of-box-rules"></a>Pravidla krabice z počítače
Objekt počítače musí být splněny následující k synchronizaci:

- `userCertificate ISNOTNULL`. Pouze Windows 10 počítače naplnění Tenhle atribut. Všechny objekty počítače s hodnotou v tomto atributu jsou synchronizovány.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Principy scénář mimo pole pravidel
V tomto příkladu používáme nasazení pomocí jednoho účtu struktury (A) jeden zdroj struktury (R) a jeden adresář Azure AD.

![Obrázek s popisem scénářů](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

V této konfiguraci předpokládá se, že je účet povolené ve struktuře účtu a zakázaný účet ve struktuře zdroje s poštovní schránkou propojené.

S nastavením výchozích Naším cílem je:

- Atributů souvisí přihlášení se synchronizují z struktuře s povolenými účtu.
- Atributy, které najdete v GAL (globální adresář) se synchronizují z struktuře s poštovní schránkou. Pokud najdete žádné poštovní schránky, použije se jiných doménové.
- Pokud nachází se propojené poštovní schránky, musí pro objekt, který chcete exportovat do Azure AD nalezeny připojený účet povolené.

### <a name="synchronization-rule-editor"></a>Editor pravidel synchronizace
Konfigurace můžete zobrazit a upravovat pomocí nástroje pro synchronizaci pravidla editoru (SRE) a na jeho zástupce najdete ji v nabídce start.

![Ikona synchronizace Editor pravidel](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

SRE je nástroj resource kit a hned po instalaci používat s Azure AD Connect synchronizace. Abyste mohli začít ji, musí být členem skupiny ADSyncAdmins. Při spuštění, vypadá nějak takto:

![Příchozí pravidla synchronizace](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

V tomto podokně uvidíte všechna synchronizace pravidla pro konfiguraci vašeho vytvořit. Každý řádek v tabulce je jedno pravidlo synchronizace. Nalevo v části typy pravidel jsou uvedeny dva různé typy: příchozí a odchozí. Příchozí a odchozí se ze zobrazení metaverse. Hlavně přecházíte na fokus na příchozí pravidla v tomto přehledu. Skutečné seznamu pravidel synchronizace závisí na schématu zjištěné ve službě Active Directory. Na obrázku nahoře struktuře účtu (fabrikamonline.com) nemá žádné služby, například Exchange a Lync, a vytvořili žádné synchronizace pravidlo pro tyto služby. Však ve struktuře zdroje (res.fabrikamonline.com) nenajdete synchronizace pravidla pro tyto služby. Obsah pravidla se liší v závislosti na verzi detekovaný. Například v nasazení s Exchange 2013 existuje více atribut toků konfigurovat než Exchange 2010 a 2007.

### <a name="synchronization-rule"></a>Synchronizace pravidla
Pravidlo synchronizace se objekt konfigurace sadou atributy toku při splněny. Slouží také k tomu, jak objektu v prostoru spojnice souvisí s objektu v metaverse jmenoval **spojení** nebo **odpovídat**. Pravidla synchronizace mají přednost před hodnotu označující, jak se vztahují k sobě. Pravidlo synchronizace s hodnotou nižší číselné má vyšší prioritu a v toku konfliktem atribut vyšší prioritu wins vyřešení konfliktů.

Jako příklad, podívejte se na toto pravidlo synchronizace **v z AD – uživatele AccountEnabled**. Označte tento řádek v SRE a vyberte **Upravit**.

Protože tento se jedná o pravidlo mimo pole, zobrazí se upozornění při otevření pravidlo. Všechny [změny v pravidlech mimo pole](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)neprovádějte tak, aby se zobrazí dotaz, jaké jsou vaše úmyslech. V tomto případě jenom chcete zobrazit pravidlo. Vyberte možnost **Ne**.

![Synchronizace pravidel upozornění](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Synchronizace pravidla má čtyři části konfigurace: popis definování rozsahu filtru, pravidla spojení a transformace.

#### <a name="description"></a>Popis
První část obsahuje základní informace, jako je název a popis.

![Popis karta editor synchronní pravidel ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Můžete taky najdete informace o připojení systém, který toto pravidlo souvisí, které typ spojení systému, který je určený pro a typ objektu metaverse objektů. Typ objektu metaverse vždycky je osoba bez ohledu na to po typ zdrojového objektu uživatele, iNetOrgPerson nebo kontakt. Typ objektu metaverse by nikdy změnit tak, aby se vytvoří jako typu obecný. Typ vazby můžete nastavit spojení, StickyJoin nebo poskytování. Toto nastavení funguje společně s části připojení pravidla a spadá později.

Uvidíte taky, že toto pravidlo synchronizace se používá k synchronizaci hesel. Pokud je uživatel v rozsahu pravidla synchronizace heslo synchronizují z místní cloudu (za předpokladu, že jste zapnuli automatický přístup synchronizační funkce heslo).

#### <a name="scoping-filter"></a>Definování rozsahu filtru
Části definování rozsahu filtr se používá ke konfiguraci, kdy se má použít pravidlo synchronizace. Protože název pravidla synchronizace prohlížíte označuje by měl použít pouze pro povolení uživatelé, obor nakonfigurovaný tak, aby atribut AD **Řízení účtu uživatele** nesmí mít bit 2 nastavení. Modul synchronizace najde uživatele ve službě Active Directory, platí toto pravidlo synchronizace při **Řízení účtu uživatele** je nastavena na hodnotu desítkové 512 (povolené normální uživatel). Pokud má uživatel **Řízení účtu uživatele** nastavit 514 (zakázané normální uživatel) nevztahuje pravidlo.

![Definování rozsahu kartu v editoru pravidlo synchronizace ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

Oboru filtr obsahuje skupiny a věty, které může být vnořeno do. Všechny klauzule uvnitř skupiny musí být splněny pro synchronizaci pravidlo použít. Když jsou definovány více skupin, aspoň jedna skupina musí být splněny pro pravidlo použít. To znamená logickým operátorem nebo Vyhodnocená každá její položka mezi skupinami a logické a Vyhodnocená každá její položka uvnitř skupiny. Příklad této konfiguraci najdete v odchozího pravidla synchronizace **na AAD – připojení ke skupině**. Existuje několik skupin synchronizace filtr, například jednu skupin zabezpečení (`securityEnabled EQUAL True`) a jedno pro distribuční skupiny (`securityEnabled EQUAL False`).

![Definování rozsahu kartu v editoru pravidlo synchronizace ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Pokud chcete toto pravidlo umožňuje definovat skupiny, které by měl být zřízení Azure AD. Distribuční skupiny, které musí být povolena pro synchronizaci s Azure AD poštovní, ale skupiny zabezpečení e-mailu není potřeba.

#### <a name="join-rules"></a>Připojit se ke pravidel
Třetí oddíl se používá ke konfiguraci vztah objekty v prostoru spojnice k objektům v metaverse. Pravidlo jste dříve prohlédli nemá žádnou konfiguraci pro připojení ke pravidla, tak místo toho přecházíte visiové **v z AD – uživatel připojit**.

![Připojení kartu pravidla v editoru pravidlo synchronizace ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

Obsah pravidlo spojení závisí na odpovídající možnost v Průvodci instalací. Pravidlo pro příchozí připojení hodnocení začíná objektu do pole zdroj spojnice a každá skupina v pravidlech spojení je vyhodnocován v pořadí. Pokud zdrojový objekt je vyhodnocen jako odpovídají právě jeden objekt v metaverse jeden pravidel spojení, jsou spojeny objekty. Pokud byla vyhodnocena všechna pravidla a nedošlo ke shodě, se používá typ vazby na stránce popis. Pokud toto nastavení je nastavený k **poskytování**, se vytvoří nový objekt v cílové metaverse. Zřízení nový objekt, který metaverse je označovaná taky jako **projektu** objekt, který bude metaverse.

Pravidla spojení jsou vyhodnoceny jenom jednou. Objekt spojnice místa a objekt metaverse jsou spojeny, budou platit Pokud jste se připojili, dokud je oborem pravidlo synchronizace pořád spokojení.

Při vyhodnocování pravidel synchronizace, jenom jedno pravidlo synchronizace s pravidly spojení definované musí být v rozsahu. Pokud více pravidel synchronizace s pravidly spojení najdete jeden objekt, vyvolá se chyba. Z toho důvodu je doporučený postup se spojení definované více pravidel synchronizace se obor pro konkrétní objekt, pokud máte jenom jedno pravidlo synchronizace. V konfiguraci mimo pole pro synchronizaci Azure AD Connect následujícími pravidly nalezen zobrazením názvu a najděte můžou být s wordem na konci název **připojení** . Pravidlo synchronizace bez všechna připojení pravidla definované platí toků atribut při další synchronizaci pravidlo propojených objektů nebo zřízení nového objektu v cílovém.

Když se podíváte na obrázku nahoře, zobrazí se, že pravidlo pokouší připojit **objectSID** s **msExchMasterAccountSid** (Exchange) a **Atribut msRTCSIP-OriginatorSid** (Lync), tedy očekáváme v topologii doménové účtu zdroje. Najděte stejné pravidlo pro všechny strukturami. Za předpokladu je, že každá struktura může být doménové k účtu nebo zdroje. Toto nastavení funguje taky v případě mají účty, které bloky v jedné stromové struktury a nemusíte připojit.

#### <a name="transformations"></a>Transformace
V části transformace definuje všechny atribut jsou přenášena vztahující se na cílový objekt, když jsou spojeny objekty a splněny filtru rozsahu. Přechod zpět **v z AD – uživatele AccountEnabled** synchronizace pravidlo nenajdete následující transformace:

![Transformace karta editor synchronní pravidel ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Vložit tuto konfiguraci v kontextu, ve struktuře nasazení účtu zdroje očekává se zobrazíte účet povolené ve struktuře účtu a zakázaného účtu ve struktuře zdroje s nastavením Exchange a Lync. Synchronizace pravidlo prohlížíte obsahuje atributy vyžadované pro přihlášení a tyto atributy by měl tok struktuře kde není povolené účtu. Tyto atribut toky jsou sestavit v jedno pravidlo synchronizace.

Transformace může obsahovat různé typy: Konstanta, přímý a výraz.

- Konstantní toku vždy přecházel pevně kódovaná hodnotu. V případě výše vždy nastaví hodnotu **True** v metaverse atribut s názvem **accountEnabled**.
- Přímé toku vždy přecházel hodnotu atributu ve zdroji na cílový atribut jako-je.
- Třetí typ toku je výraz a umožňuje pro pokročilejší konfigurace.

Výraz jazyka VBA (Visual Basic for Applications), takže osoby, které se prostředí Microsoft Office nebo VBScript rozpozná formátu je. Atributy jsou uzavřeny v hranatých závorek, [Název_atributu]. Atribut názvy a názvy funkcí jsou malá a velká písmena, ale editoru pravidla synchronizace vyhodnotí výrazy a poskytnout upozornění, pokud zadaný výraz není platný. Všechny výrazy jsou vyjádřeny na jednom řádku s vnořenými funkcemi. Zobrazíte power jazyka konfigurace tady je tok pwdLastSet, ale s přidat víc komentářů vložit:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Další informace o výraz jazyka atribut toků najdete v článku [Principy deklarativní zřizování výrazů](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) .

### <a name="precedence"></a>Priorita
Teď jste prohlédli některé jednotlivá pravidla synchronizace, ale tato pravidla společná práce v konfiguraci. V některých případech hodnotě atributu přispěla z více pravidel synchronizace stejný atribut cíl. V tomto případě přednost atributu slouží k určení, který atribut wins. Jako příklad podívejte se na sourceAnchor atribut. Tenhle atribut je důležité atribut mají být k přihlášení k Azure AD. Atribut toku můžete najít atributu ve dvou různých pravidla synchronizace **v z AD – AccountEnabled uživatele** a **v z AD – uživatele běžné**. Z důvodu priorit pravidel synchronizace atribut sourceAnchor je přispěla z struktuře s účtem povolené nejdřív po několika objekty připojen k objektu metaverse. Pokud nejsou povolené účty a potom modul Synchronizace používá pravidlo synchronizace skutečné all **v z AD – uživatele běžné**. Konfigurace zajišťuje, aby i pro účty, které jsou neaktivní, tam pořád sourceAnchor.

![Příchozí pravidla synchronizace](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Priority pravidel synchronizace nastavenou ve skupinách pomocí Průvodce instalací. Všechna pravidla ve skupině mají stejný název, ale jsou připojeny k různých připojeného adresářů. Průvodce instalací nabízí pravidlo **v z AD – uživatel připojit** nejvyšší prioritu a provádí iterace přes všechna připojení AD adresáře. Potom pokračovat s další skupiny pravidel v předem daném pořadí. Do skupiny se přidají pravidla v pořadí, ve kterém spojnice byly přidány v průvodci. Pokud jiné spojnice je přidána procházet všemi kroky průvodce, pravidla synchronizace se změněným pořadím a nové spojnice pravidel jsou vložené poslední v jednotlivých skupin.

### <a name="putting-it-all-together"></a>Vložení vůbec
Teď víme, dostatečně o pravidlech synchronizace mohli pochopit, jak funguje konfiguraci s jinými pravidly synchronizace. Když se podíváte na uživatele a atributy, které jsou přispěla k metaverse, použijí se pravidla v následujícím pořadí:

Jméno | Komentář
:------------- | :-------------
V z AD – uživatele spojení | Pravidla pro připojení ke spojnici rozmístění objektů s metaverse.
V z AD – uživatelské účty povolena | Atributy potřebných pro přihlášení k Azure AD a Office 365. Chceme tyto atributy z účtu povolené.
V z AD – společný uživatele ze serveru Exchange | V globálním adresáři nalezeny atributy. Budeme se předpokládá, že kvality dat je nejlepší ve struktuře kde nalezeny poštovní schránku uživatele.
V z AD – společný uživatele | V globálním adresáři nalezeny atributy. V případě, že nebyly nalezeny poštovní schránky jiného spojených objektu do ní můžete přidávat hodnota atributu.
V z AD – uživatele Exchange | Existuje pouze pokud byl zjištěn Exchange. Toky všechny infrastruktury Exchange atributy.
V z AD – uživatele Lyncu | Existuje pouze pokud byl zjištěn Lyncu. Toky všech atributů infrastruktury Lync.

## <a name="next-steps"></a>Další kroky

- Další informace o konfiguraci modelu v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Další informace o jazyce výrazu ve [Výrazech Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Pokračujte ve čtení fungování konfigurace mimo pole v [Principy uživatelů a kontakty](active-directory-aadconnectsync-understanding-users-and-contacts.md)
- Postup provádění změn na praktické pomocí deklarativní zřizování v [tom, jak změnit výchozí konfigurace](active-directory-aadconnectsync-change-the-configuration.md).

**Přehled témat**

- [Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace](active-directory-aadconnectsync-whatis.md)
- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
