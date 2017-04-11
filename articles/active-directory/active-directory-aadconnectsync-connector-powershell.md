<properties
   pageTitle="Prostředí PowerShell spojnice | Microsoft Azure"
   description="Tento článek popisuje, jak nakonfigurovat společnosti Microsoft Windows PowerShell spojnice."
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

# <a name="windows-powershell-connector-technical-reference"></a>Technické informace o Windows PowerShell spojnice
Tento článek popisuje konektoru Windows PowerShell. Článek se týká těchto produktů:

- Správce Microsoft identit 2016 (MIM2016)
- Správce identit Forefront 2010 R2 (FIM2010R2)
    -   Musíte použít oprava 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 a FIM2010R2 spojnice je k dispozici ke stažení z [Webu služby Stažení softwaru](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Základní informace o konektoru prostředí PowerShell
Konektor prostředí PowerShell umožňuje integrace služby synchronizace s externí systémy, které nabízejí prostředí Windows PowerShell na základě rozhraní API. Spojnice poskytuje most mezi funkce správy agent založeného na volání extensible připojení 2 (ECMA2) framework a prostředí Windows PowerShell. Další informace o rozhraní ECMA najdete v tématu [Extensible 2.2 správy Agent referenční](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Zjistit předpoklady pro
Před použitím spojnice, zkontrolujte, jestli že máte následující na serveru synchronizace:

- 4.5.2 rozhraní Microsoft .NET Framework nebo novější
- Prostředí Windows PowerShell 2.0, 3.0 nebo 4.0

Zásad spouštění na serveru služby synchronizace musí být nakonfigurované pro povolení spojnice tak, aby spouštěly skripty Windows Powershellu. Pokud skripty spustí spojnice jsou digitálně podepsané, konfigurace zásad spouštění spuštěním tento příkaz:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Vytvoření nové spojnice
Vytvoření prostředí Windows PowerShell spojnice ve službě synchronizace, je nutné zadat řadu skripty Windows Powershellu, které provést kroky požadoval službu synchronizace. V závislosti na zdroj dat, ke kterému se můžete připojit a funkce, které požadujete musíte implementovat skripty liší. Tato část popisuje všech skriptů lze provést, a pokud jsou vyžadovány.

Konektor prostředí Windows PowerShell je určen pro uložení všech skripty do databáze služby synchronizace. Je možné spouštěly skripty, které jsou uložené v systému souborů, je ale jednodušší vložte textu každého skriptu přímo v konfiguraci spojnice.

Prostředí PowerShell spojnici vytvoříte v **Synchronizační službu** vyberte **Agent pro správu** a **vytvořit**. Vyberte spojnici **prostředí PowerShell (Microsoft)** .

![Vytvoření spojnice](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Připojení
Zadejte parametry konfigurace připojení ke vzdálené systému. Tyto hodnoty bezpečně uložené ve službě synchronizace a zpřístupnit skriptů Windows Powershellu při spuštění spojnice.

![Připojení](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Můžete nakonfigurovat následující parametry připojení:

**Připojení**

Parametr | Výchozí hodnota | Účel
--- | --- | ---
Server | <Blank> | Název serveru, který spojnici připojit.
Domény | <Blank> | Doména přihlašovací údaje pro ukládání pro použití při spuštění spojnice.
Uživatel | <Blank> | Uživatelské jméno pověření uložit pro použití při spuštění spojnice.
Heslo | <Blank> | Heslo přihlašovací údaje pro ukládání pro použití při spuštění spojnice.
Zosobnění spojnice účtu | NEPRAVDA | V případě hodnoty true synchronizační službu spustí skripty Windows Powershellu v kontextu pověření. Pokud je to možné, doporučujeme, aby parametr **$Credentials** předán pro každý skript nepoužijí zosobnění. Další informace o dalších oprávnění, která jsou potřebná tuto možnost použít najdete v článku [Další konfiguraci pro zosobnění](#additional-configuration-for-impersonation).
Načtení profilu uživatele při zosobnění | NEPRAVDA | Nastaví Windows během zosobnění načtena profilů uživatelů spojnici přihlašovacích údajů. Pokud zosobněného uživatel cestovní profil, nenačte spojnice cestovní profil. Další informace o dalších oprávnění, která jsou potřebná k používání tento parametr najdete v tématu [Další konfiguraci pro zosobnění](#additional-configuration-for-impersonation).
Typ přihlášení při zosobnění | Žádná | Typ přihlášení během zosobnění. Další informace najdete v příručce [dwLogonType] [ dw] si přečtěte následující dokumentaci.
Podepsané skriptů | NEPRAVDA | True, konektoru prostředí Windows PowerShell ověří, že každý skript má platný digitální podpis. Pokud je hodnota NEPRAVDA, zajistěte, aby byl zásad spouštění prostředí Windows PowerShell serveru služby synchronizace RemoteSigned nebo bez omezení.

**Běžné modul**  
Spojnice umožňuje ukládat sdílené modulu Windows PowerShell v konfiguraci. Skript spuštění spojnice modulu Windows PowerShell tak, že jej lze importovat tak, že každý skript extrakci k systému souborů.

Pro Import, Export a synchronizace hesel skripty společného modulu extrakci do složky MAData spojnice. Schéma, ověřování, hierarchie a oddílu zjišťování skriptů společného modulu extrakci ke složce % TEMP %. V obou případech je název extrahované skriptu společný modul podle nastavení běžných název skriptu modul.

Načtení modulu s názvem FIMPowerShellConnectorModule.psm1 ve složce MAData, použijte následující příkaz:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Načtení modulu s názvem FIMPowerShellConnectorModule.psm1 ze složky % TEMP %, použijte následující příkaz:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Ověření parametrů**  
Skript ověření je nepovinné skriptu prostředí Windows PowerShell, který mohou sloužit k zajištění platnosti parametry konfigurace spojnice poskytnutých správce. Ověřování serveru, pověření připojení a parametrech připojení jsou běžné použití tohoto skriptu ověření. Skript ověření se nazývá po následujících karet a dialogová okna upravují:

- Připojení
- Globální parametry
- Konfigurace oddílu

Skript ověření obdrží následující parametry z konektoru:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | Karta konfigurace nebo dialogové okno, který spustil žádost o ověření.
ConfigParameters | [Kolekci KeyedCollection] [ keyk] [řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.

Skript ověření měly vrátit na jeden objekt ParameterValidationResult kanálu.

**Schéma vyhledávání**  
Skript schématu vyhledávání je povinný. Tento skript vrátí typy objektů, atributy a atribut omezení, která synchronizační služba používá při konfiguraci pravidla toku atribut. Skript schématu vyhledávání se spouští při vytváření spojnice a naplní schématu spojnice. Používá se také aktualizovat schéma akcí v okně Správce služeb synchronizace.

Skript zjišťování schématu obdrží následující parametry z konektoru:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameters | [Kolekci KeyedCollection] [ keyk] [řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.

Skript musí vrátit jediné [schématu] [ schema] objekt kanálu. Objekt schématu je tvořen [SchemaType] [ schemaT] objekty, které představují typy objektů (například: uživatelé a skupiny). Objekt SchemaType obsahuje sadu [SchemaAttribute] [ schemaA] objekty, které představují atributů (například: křestní jméno, příjmení a adresa) typu.

**Další parametry**  
Kromě standardního nastavení můžete definovat další vlastní nastavení, které jsou specifické pro instanci spojnice. Tyto parametry může být zadán v konektoru oddíl, nebo spuštění krok vyrovnání a zobrazená po kliknutí na příslušné skriptu prostředí Windows PowerShell. Vlastní nastavení mohou být uloženy v databázi služby synchronizace ve formátu prostého textu nebo může být zašifrováním. Synchronizační službu automaticky jsou šifrovány a dešifruje zabezpečit nastavení v případě potřeby.

Chcete-li zadat vlastní nastavení, oddělte název každý parametr středník (;).

Vlastní nastavení z scénáře zobrazíte musí přípona názvu podtržítko ( \_ ) a rozsah parametr (globální, oddíl nebo RunStep). Například pro přístup k parametru globální název_souboru, použijte tento fragment kódu:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Funkce
Karta Možnosti návrháře Agent správy definuje chování a funkce spojnice. Výběru na této kartě nelze upravit, když vytvořila spojnice. Tato tabulka uvádí nastavení možností.

![Funkce](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

Funkce | Popis |
--- | --- |
[Rozlišující název stylu][dnstyle] | Označuje, zda spojnice podporuje rozlišující jména a pokud ano, co stylu.
[Exportujte typ][exportT] | Určuje typ objekty, které mají být zobrazena skriptem pro Export. <li>AttributeReplace – obsahuje celou sadu hodnoty pro atribut s více hodnotami při změně atribut.</li><li>AttributeUpdate – obsahuje pouze rozdílů s více hodnotami atribut při změně atribut.</li><li>MultivaluedReferenceAttributeUpdate - obsahuje celou sadu hodnot s více hodnotami atributy-reference a pouze rozdílů atributů s více hodnotami odkaz.</li><li>ObjectReplace – zahrnuje všechny atributy pro konkrétní objekt při žádné změny atributů</li>
[Normalizace dat][DataNorm] | Nastaví synchronizační službu pro normalizovat ukotvení atributy před jsou poskytovány skriptů.
[Potvrzení objektu][oconf] | Konfiguruje nastavení importu na zjištění stavu úkolů ve službě synchronizace. <li>Normální – výchozí chování, které očekává všechny exportovaném změny potvrzeno prostřednictvím importu</li><li>NoDeleteConfirmation – po odstranění objektu je žádná pole Čekání na import generovaného.</li><li>NoAddAndDeleteConfirmation – objektu vytvořila se nebo odstraní, není žádná pole Čekání na import generovaného.</li>
Použití DN jako ukotvení | Pokud LDAP je nastaveno rozlišující název stylu, ukotvení atribut prostor spojnice je také rozlišující název.
Souběžné operace několik spojnic | Pokud zaškrtnete, více spojnic prostředí Windows PowerShell je možné spouštět současně.
Oddíly | Pokud zaškrtnete, spojnice podporuje víc oddílů zjišťování oddíl.
Hierarchie | Při kontrole podporuje konektoru LDAP styl hierarchickou strukturu.
Povolení importu | Když zaškrtnete toto políčko, spojnice k importu dat pomocí skriptů importovat.
Povolení Delta importu | Pokud zaškrtnete, spojnice můžete požádat o rozdílů z importu skriptů.
Povolit Export | Když zaškrtnete toto políčko, spojnice exportuje data pomocí skriptů Exportovat.
Povolit úplné Export | Když zaškrtnete toto políčko, skripty export domovské stránce podpory Export místo celé spojnice. Tuto možnost použít povolit exportovat musí taky zkontrolovat.
Žádné hodnoty odkaz ve exportovat průchodu první | Když zaškrtnete toto políčko, atributy odkaz exportován ve druhém průchodu exportovat.
Povolení přejmenování objektu | Pokud zaškrtnete, rozlišující názvy lze upravit.
Přidat jako nahradit odstranit | Pokud zaškrtnete, odstranit přidáte operace, jsou exportována jako jeden náhradní.
Povolit operace heslo | Když zaškrtnete toto políčko, skripty synchronizace heslo jsou podporovány.
Povolit Export heslo v prvním kroku | Když zaškrtnete toto políčko, heslo během vytváření jsou exportovány při vytvoření objektu.

### <a name="global-parameters"></a>Globální parametry
Karta globální parametry v Návrháři Agent správy umožňuje konfigurace skripty Windows Powershellu, které pracují ve spojnice. Můžete taky nakonfigurovat globální hodnoty pro vlastní nastavení definovaný na kartě připojení.

**Oddíl zjišťování**  
Oddíl je samostatná oboru v rámci jedno sdílené schéma. Každá doména je ve službě Active Directory oddíl jeden doménové. Oddíl je logické seskupení k importu a exportu. Import a Export mít oddíl jako kontextu a všechny operace se stane v tomto kontextu. Oddíly mají představovat hierarchie v LDAP. Rozlišující název oddílu se používá při importu k ověření, že jsou všechny vrácené objekty v rozsahu oddílem. Rozlišující název oddílu slouží také při vytváření z metaverse prostor konektoru k určení oddílu, který chcete objekt by měl být přidružené k při exportu.

Skript zjišťování oddíl přijímání následujících parametrů spojnice:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameters  | [Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.

Skript musí vrátit jeden [oddíl] [ part] objektu nebo seznam [T] objekty oddílu kanálu.

**Zjišťování hierarchie**  
Skript zjišťování hierarchie se používá pouze po funkci rozlišující název stylu LDAP. Skript slouží k umožní procházet a vyberte sadu kontejnery je považován za nebo oddálit obor k importu a exportu. Skript by měl obsahovat pouze seznam uzlů, které jsou přímé podřízené zadaný skriptu uzel kořenové.

Skript zjišťování hierarchie obdrží následující parametry z konektoru:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameters | [Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.
ParentNode | [HierarchyNode][hn] | V kořenovém uzlu hierarchie, pod kterým by měly vrátit skript přímé podřízené.

Skript musí vrátit buď objekt HierarchyNode jediné podřízené nebo seznam [T] podřízené HierarchyNode objekty kanálu.

#### <a name="import"></a>Import
Spojovací čáry, které podporují operace importu musí implementaci tři skriptů.

**Zahajte Import**  
Importovat skript počátečního běží na začátku tohoto kroku spustit import. Během tohoto kroku můžete vytvořit připojení ke zdrojovému systému a proveďte přípravných kroků před importem dat z připojeného systému.

Importovat skript počátečního obdrží následující parametry z konektoru:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameters | [Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skript informuje o typu oddílu spustit import (delta, nebo celé), hierarchie, vodoznak a velikost očekávané stránky.
Typy | [Schéma][schema] | Schéma spojnice prostor, který je importován.

Skript musí vrátit jednoho [OpenImportConnectionResults] [ oicres] objekt kanálu, například:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Import dat**  
Importovat data skript nazývá pomocí konektoru dokud skript značí, že je žádná další data importovat. Konektor prostředí Windows PowerShell velikostí stránky 9 999 objektů. Pokud skript vrátí více než 9 999 objekty pro import, musíte podporu stránkování. Spojnice zpřístupňuje, místo toho vlastnost vlastní data, můžete do úložiště vodoznaku tak, aby pokaždé, když importovat data skript, skript obnoví, import objektů tam, kde ho přestali.

Importovat data skript obdrží následující parametry z konektoru:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameters | [Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.
GetImportEntriesRunStep | [ImportRunStep][irs] | Obsahuje vodoznak (CustomData), který lze použít při stránkování importy a importuje delta.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skript informuje o typu oddílu spustit import (delta, nebo celé), hierarchie, vodoznak a velikost očekávané stránky.
Typy | [Schéma][schema] | Schéma pro naimportují místo na spojnici.

Importovat data skript musíte napsat seznam [[CSEntryChange][csec]] objekt kanálu. Tato kolekce se skládá z CSEntryChange atributy, které představují jednotlivé objekty importovaný. Při spuštění úplného importu této kolekce měli úplnou sadu CSEntryChange objekty, které mají všechny atributy každého objektu. Během importu Delta by měl obsahovat objekt CSEntryChange buď úroveň rozdílů atribut pro jednotlivé objekty k importu nebo formátovanými jako úplný objektů, které se změnily (nahradit režimu).

**Ukončení importu**  
Po dokončení importu spustit spuštění skriptu ukončit importovat. Tento skript by měl provádět vyčištění úlohy potřebné (třeba zavřít připojení k systémům) a odpovědět na k chybám.

Importovat skript koncové obdrží následující parametry z konektoru:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameters | [Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skript informuje o typu oddílu spustit import (delta, nebo celé), hierarchie, vodoznak a velikost očekávané stránky.
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | Skript informuje o důvod, proč import byl ukončen.

Skript musí vrátit jednoho [CloseImportConnectionResults] [ cicres] objekt kanálu, například:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Export
Stejné architektury import spojnice, spojovací čáry, které podporují Export musí implementaci tři skriptů.

**Začněte exportu**  
Skriptem pro export počátečního běží na začátku tohoto kroku exportu. Během tohoto kroku můžete vytvořit připojení ke zdrojovému systému a vedení přípravných kroků před exportem dat připojeného systému.

Skriptem pro export počátečního obdrží následující parametry z konektoru:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameters | [Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skript informuje o typ exportu (delta, nebo celé) oddílu, hierarchie a velikost očekávané stránky.
Typy | [Schéma][schema] | Schéma pro exportu místo na spojnici.

Skript neměli vrátit jakýkoli výstup kanálu.

**Export dat**  
Synchronizace služby hovorů skriptu pro Export dat kolikrát je nutné zpracovat všechna pole Čekání na export. Pokud prostor konektoru má více čekající exporty než velikost stránky spojnice, skriptu pro export dat lze svolat tisknutím a případně tisknutím pro stejný objekt.

Skriptu pro export dat obdrží následující parametry z konektoru:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameters | [Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.
CSEntries | IList[CSEntryChange][csec] | Seznam všechny spojnice místo objekty se čeká na vyřízení exporty zpracovány během této relace.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skript informuje o typ exportu (delta, nebo celé) oddílu, hierarchie a velikost očekávané stránky.
Typy | [Schéma][schema] | Schéma pro exportu místo na spojnici.

Skriptu pro export dat musí vrátit [PutExportEntriesResults] [ peeres] objekt kanálu. Tento objekt nemusí informacemi výsledek pro každou spojnici exportovaného Pokud dojde k chybě nebo změna atribut ukotvení. Chcete-li například vrátí objekt PutExportEntriesResults kanálu:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**End Export**  
Po dokončení exportu spustit, Export koncové skript ke spuštění. Tento skript by měl provádět vyčištění úlohy potřebné (třeba zavřít připojení k systémům) a odpovědět na k chybám.

Ukončení skriptem pro export obdrží následujících parametrů od konektoru:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameters | [Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skript informuje o typ exportu (delta, nebo celé) oddílu, hierarchie a velikost očekávané stránky.
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | Skript informuje o důvod, proč exportovat byl ukončen.

Skript neměli vrátit jakýkoli výstup kanálu.

#### <a name="password-synchronization"></a>Synchronizace hesel
Prostředí Windows PowerShell spojnic mohou sloužit jako cíl pro změny nebo resetování hesla.

Skript heslo obdrží následující parametry z konektoru:

Jméno | Datový typ | Popis
--- | --- | ---
ConfigParameters | [Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] | Tabulka parametrů konfigurace konektoru.
Přihlašovací údaje | [PSCredential][pscred] | Obsahuje žádné přihlašovací údaje zadané správcem na kartě připojení.
Oddíl | [Oddíl][part] | Oddíl adresáře, uložený v CSEntry.
CSEntry | [CSEntry][cse] | Položka spojnice místa pro objekt, který je dostali změnit heslo nebo obnovit.
Typ operace | Řetězec | Určuje, zda je operace obnovit (**SetPassword**) nebo změnu (**Změna hesla byla**).
PasswordOptions | [PasswordOptions][pwdopt] | Příznaky, které určují určené heslo resetovat chování. Tento parametr neexistuje pouze pokud typ operace **SetPassword**.
Původní_heslo | Řetězec | Vyplněné staré heslo objektu pro změnu hesla. Tento parametr neexistuje pouze pokud typ operace je **Změna hesla byla**.
Nové_heslo | Řetězec | Vyplněné objektu nové heslo, které by měl nastavit skript.

Skript heslo není podle očekávání měly být nevrátí žádné výsledky kanálu prostředí Windows PowerShell. Pokud dojde k chybě v skriptu heslo, skript vyvoláním mezi následujícími výjimkami synchronizační službu informovat o tomto problému:

- [PasswordPolicyViolationException] [ pwdex1] – Pokud heslo nesplňuje zásady pro heslo v systému připojeného vyvolá.
- [PasswordIllFormedException] [ pwdex2] – Pokud heslo není přijatelná připojeného systému vyvolá.
- [PasswordExtension] [ pwdex3] – zobrazují jiné chyby skriptu heslo.

## <a name="sample-connectors"></a>Ukázka spojnic
Dokončení základní informace o dostupných ukázkových spojnic, najdete v článku [Windows PowerShell spojnice spojnice odběr][samp].

## <a name="other-notes"></a>Další poznámky

### <a name="additional-configuration-for-impersonation"></a>Další konfiguraci pro zosobnění
Udělte uživatele, který je považováno následující oprávnění na serveru služby synchronizace:

Přístup pro čtení na následujících klíčích registru:

- HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
- HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

K určení zabezpečení identifikátor (ID zabezpečení) synchronizační službu služby účtu, spusťte následující příkazy Powershellu:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Čtení pro následující složky systému souborů:

- %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Nahraďte název konektoru prostředí Windows PowerShell pro tento zástupný symbol {ConnectorName}.

## <a name="troubleshooting"></a>Řešení potíží

-   Informace o tom, jak povolit protokolování pro řešení potíží s spojnice zjistěte, [jak povolit trasování událostí pro Windows trasování spojnice](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
