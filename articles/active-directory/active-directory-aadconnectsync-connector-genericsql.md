<properties
   pageTitle="Obecný SQL spojnice | Microsoft Azure"
   description="Tento článek popisuje, jak nakonfigurovat obecné spojnice SQL společnosti Microsoft."
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

# <a name="generic-sql-connector-technical-reference"></a>Technické informace o obecných spojnice SQL

Tento článek popisuje obecný spojnice SQL. Článek se týká těchto produktů:

- Správce Microsoft identit 2016 (MIM2016)
- Správce identit Forefront 2010 R2 (FIM2010R2)
    -   Musíte použít oprava 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 a FIM2010R2 spojnice je k dispozici ke stažení z [Webu služby Stažení softwaru](http://go.microsoft.com/fwlink/?LinkId=717495).

Tato spojnice v akci zobrazíte najdete v článku [Obecný SQL spojnice podrobné](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) .

## <a name="overview-of-the-generic-sql-connector"></a>Základní informace o konektoru obecný SQL

Obecný spojnice SQL umožňuje integrace služby synchronizace s databáze systému, který nabízí připojení ODBC.  

Z uceleném pohledu tyto funkce jsou podporovány ální spojnice:

Funkce | Podpora
--- | ---
Připojený zdroj dat | Spojnice jsou podporovány pro všechny ovladače ODBC 64bitová verze. Po testování s takto: <li>Microsoft SQL Server a SQL Azure</li><li>Databáze IBM DB2 10.x</li><li>Databáze IBM DB2 9.x</li><li>Oracle 10 a 11 g</li><li>MySQL 5.x</li>
Scénáře   | <li>Správa životního cyklu objektu</li><li>Správa hesel</li>
Operace | <li>Úplný Import a Delta Import, Export</li><li>Pro Export: Přidat, odstranit, aktualizovat a nahradit</li><li>Dialogové okno nastavit heslo, změnit heslo</li>
Schéma | <li>Dynamické zjišťování objektů a atributů</li>

### <a name="prerequisites"></a>Zjistit předpoklady pro
Před použitím spojnice, zkontrolujte, jestli že máte následující na serveru synchronizace:

- 4.5.2 rozhraní Microsoft .NET Framework nebo novější
- 64bitová verze ovladače klienta ODBC

### <a name="permissions-in-connected-data-source"></a>Oprávnění v připojený zdroj dat
Pokud chcete vytvořit nebo proveďte některý z podporovaných úkolů v spojnice obecný SQL, musíte mít:

- db_datareader
- db_datawriter

### <a name="ports-and-protocols"></a>Porty a protokoly
Porty potřebných pro ovladač ODBC pro práci v dokumentaci dodavatele databáze.

## <a name="create-a-new-connector"></a>Vytvoření nové spojnice
Obecný SQL spojnici vytvoříte v **Synchronizační službu** vyberte **Agent pro správu** a **vytvořit**. Vyberte spojnici **Obecný SQL (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Připojení
Spojnice používá soubor zdroj dat ODBC pro připojení k síti. Vytvoření souboru DSN použití **Zdrojů dat ODBC** nachází v nabídce start klikněte v části **Nástroje pro správu**. V nástroji pro správu vytvořte **Soubor DSN** , lze zadat ke spojnici připojit.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Obrazovka připojení je první při vytváření nové obecný spojnice SQL. Nejdřív musíte zadat následující informace:

- Cesta k souboru kódu oznámení o doručení
- Ověřování
    - Uživatelské jméno
    - Heslo

Databáze by měla podporovat jednu z těchto metod:

- **Ověřování systému Windows**: databázi ověřování používá přihlašovací údaje Windows k ověření uživatele. Uživatelské jméno a heslo zadané slouží k ověření s databází. Tento účet vyžaduje oprávnění k databázi.
- **Ověřování serveru SQL**: ověřování používá databáze uživatelské jméno a heslo definované jednu obrazovku připojení pro připojení k databázi. Pokud soubor DSN uložit uživatelské jméno a heslo, pověření zadané v dialogovém okně připojení přednost.
- **Databáze SQL Azure ověření**: Další informace najdete v tématu [připojení k SQL databázi tak, že pomocí Azure Active Directory, ověřování](..\sql-database\sql-database-aad-authentication.md).

**Rozlišující název je ukotvení**: Pokud vyberete tuto možnost, název domény se používá taky jako atribut ukotvení. Slouží k provádění jednoduchých ale taky má následující omezení:

-   Spojnice podporuje typ pouze jeden objekt. Proto atributů odkaz pouze odkazovat na stejného typu objektu.

**Exportovat typ: objekt nahradit**: při exportu, když jenom některé atributy změnily, celý objekt se všemi atributy exportu a nahradí existující objekt.

### <a name="schema-1-detect-object-types"></a>Schéma 1 (typy objektů rozpoznat)
Na této stránce kterou se chystáte nakonfigurovat, jak se bude spojnice najít typy různých objektů v databázi.

Každý typ objektu vyjádřená oddíl a nakonfigurovali další na **Konfigurovat oddíly a hierarchie**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Způsob zjišťování typ objektu**: spojnice podporuje těchto postupů zjišťování typ objektu.

- **Pevná**: poskytnete seznamu typů objektů se seznamem hodnotami oddělenými čárkou. Příklad: `User,Group,Department`.  
![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **Tabulka/zobrazení/uložené procedury**: Zadejte název tabulky, zobrazení nebo uložené procedury a pak název sloupce, který obsahuje seznam typů objektů. Pokud používáte uložená procedura, zadejte také parametry ho ve formátu **[webu název]: [směr]: [hodnota]**. Na samostatném řádku (použijte kombinaci kláves Ctrl + Enter získat nový řádek) zadejte každý parametr.  
![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **Dotaz SQL zobrazený**: Tato možnost umožňuje poskytovat jazyka SQL, který vrátí jediný sloupec s typy objektů, například `SELECT [Column Name] FROM TABLENAME`. Musí být vrácený sloupec typu řetězec (datová).

### <a name="schema-2-detect-attribute-types"></a>Schéma 2 (rozpoznat atribut typy)
Na této stránce kterou se chystáte nakonfigurovat, jak typy a názvy atributů se chystáte zjišťování. Konfigurace možnosti jsou vyjmenované pro každý typ objektu detekovaný z předchozí stránky.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Typ atributu rozpoznávání metoda**: spojnice podporuje tyto atribut typ rozpoznávání metody s typem každé zjištěné objektu na obrazovce schématu 1.

- **Tabulka/zobrazení/uložené procedury**: Zadejte název tabulky, zobrazení nebo uložená procedura použitý k nalezení názvů atribut. Pokud používáte uložená procedura, zadejte také parametry ho ve formátu **[webu název]: [směr]: [hodnota]**. Na samostatném řádku (použijte kombinaci kláves Ctrl + Enter získat nový řádek) zadejte každý parametr. Zjišťování názvy atributů s více hodnotami atributem, zadejte seznam tabulky nebo zobrazení. Nejsou podporovány s více hodnotami scénáře při nadřazené a podřízené tabulky mají stejné názvů sloupců.
- **Dotaz SQL zobrazený**: Tato možnost umožňuje poskytovat jazyka SQL, který vrátí jediný sloupec s názvem atribut, například `SELECT [Column Name] FROM TABLENAME`. Musí být vrácený sloupec typu řetězec (datová).

### <a name="schema-3-define-anchor-and-dn"></a>Schéma 3 (ukotvení definovat a DN)
Tato stránka umožňuje konfigurace ukotvení a atribut DN pro každý typ zjištěnou objektu. Můžete vybrat více atribut aby ukotvení jedinečný.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- Nejsou uvedené atributy s více hodnotami a logické.
- Stejný atribut nelze použít pro název domény a ukotvení, pokud **DN ukotvení** je zaškrtnuté na stránce připojení.
- Pokud **DN ukotvení** je vyberete na stránce připojení, tato stránka vyžaduje pouze atribut DN. Tenhle atribut byste měli použít také jako atribut ukotvení.
![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schéma 4 (definovat typ atributu, odkaz a směr)
Tato stránka umožňuje nastavit typ atribut, například celé číslo, binární, nebo logické hodnoty a směr každé vlastnosti. Všechny atributy stránce **schématu 2** jsou uvedeny včetně s více hodnotami atributy.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **Datový typ**: slouží k mapování typ atributu na tyto typy známé modulem synchronizace. Ve výchozím nastavení je pomocí stejného typu jako zjištěnou schématu SQL, ale data a času a funkce pro odkazy nejsou snadno zjistitelný. Osobám budete muset zadat **data a času** nebo **odkaz**.
- **Směr**: můžete nastavit směr atribut chcete importovat, exportovat nebo ImportExport. ImportExport je výchozí.
![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Poznámky:

- Pokud typ atributu ve spojnici, použije datový typ String.
- **Vnořené tabulky** považovat za jedním sloupcem databázových tabulek. Oracle ukládá řádky vnořené tabulky v určitém pořadí. Však při načítání vnořené tabulky do proměnné PL/SQL řádky jsou uvedeny po sobě jdoucí dolního indexu od 1. Který umožňuje pole profesionálové přístup pro jednotlivé řádky.
- **VARRYS** nepodporuje spojnice.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schéma 5 (definovat oddíl pro odkaz atributy)
Na této stránce můžete nakonfigurovat pro všechny referenční atributy, které oddílu (typ objektu) atribut odkazuje.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Pokud používáte **název domény je ukotvení**, musí použít stejného typu objektu jako tu, kterou odkazujete z. Nelze odkazovat jiný typ objektu.

### <a name="global-parameters"></a>Globální parametry
Na stránce globální parametry slouží ke konfiguraci Delta Import, formát data a času a metodu hesla.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

Obecný spojnice SQL podporuje následujících metod k importu Delta:

- **Aktivace**: najdete v článku [vytváření Delta zobrazeními pomocí aktivačních událostí](https://technet.microsoft.com/library/cc708665.aspx).
- **Vodoznak**: Obecné přístup, který se dá používat se všechny databáze. Vodoznak dotaz, je předem vyplněné podle dodavatele databáze. Sloupec vodoznak musí být v každé tabulce/zobrazení použít. V tomto sloupci musí sledovat vloží a aktualizace tabulky jako a jeho závislé (s více hodnotami nebo zneužívání) tabulky. Hodiny mezi službou synchronizace a databázový server je synchronizovat. Pokud ne, některé položky v poli importovat delta může vynechány.  
Omezení:
    - Strategie vodoznak nemá podpory odstraněné objekty.
- **Snímek**: (funguje jenom s Microsoft SQL Server) [generování zobrazení Delta použití snímků](https://technet.microsoft.com/library/cc720640.aspx)
- **Sledování změn**: (funguje jenom s Microsoft SQL Server) [o sledování změn](https://msdn.microsoft.com/library/bb933875.aspx)  
Omezení:
    - Ukotvení & DN atributu musí být součástí primárního klíče pro vybraného objektu v tabulce.
    - Dotaz SQL zobrazený není podporovaná během importem a exportem s sledování změn.

**Další parametry**: zadat časové pásmo serveru databáze označující, kde je umístěn databázovým serverem. Tato hodnota se používá pro podporu uvedené různé formáty atributy datum a čas.

Spojnice vždy ukládá datum a datum a čas ve formátu UTC. Abyste mohli správně převést datum a čas, časové pásmo databázový server a použitý formát je nutné zadat. Formát by měl být vyjádřena ve formátu .net.

Při exportu musí být každý atribut doby data uvedeny ke konektoru ve formátu času UTC.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Konfigurace heslo**: spojnice poskytuje možnosti synchronizace heslo a podporuje a změnit heslo.

Spojnice lze podporu synchronizace hesel dvěma způsoby:

- **Uložená procedura**: Tato metoda vyžaduje dva uložené procedury pro podporu změnit & Nastavit heslo. Zadejte všechny parametry pro přidání a změna operaci heslo v části **Nastavení SP heslo** a **Změnit heslo SP** parametrů v uvedeném pořadí jako na příkladu.
![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **Rozšíření heslo**: Tato metoda vyžaduje heslo rozšíření DLL (musíte zadat název DLL rozšíření, který je implementující rozhraní [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) ). Sestavení rozšíření heslo musí být umístěny ve složce rozšíření tak, aby spojnice načtením DLL za běhu.
![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Máte taky povolit správu heslo na stránce **Konfigurace rozšíření** .
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurace oddíly a hierarchie
Na stránce oddíly a hierarchie vyberte všechny typy objektů. Každý typ objektu je samostatný oddíl.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Je také možné přepsat hodnot definovaných na stránce **připojení** nebo **Globální parametry** .

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Konfigurace ukotvení
Tato stránka je jen pro čtení, protože definovaný ukotvení. Atribut vybrané ukotvení připojen vždy typu objektu zajistit, že je že stále jedinečné různých typů objektů.

![ukotvení](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Konfigurace parametr spuštění kroku
Tento postup nastavené na spouštěcím profily na spojnici. Tyto konfigurace udělejte skutečné práce pro import a export dat.

### <a name="full-and-delta-import"></a>Úplné a Delta Import
Obecný SQL spojnice podpora celé a Import Delta pomocí těchto postupů:

- Tabulky
- Zobrazení
- Uložená procedura
- Dotaz SQL

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Zobrazení a tabulky**  
K importu s více hodnotami atributy objektu, budete muset názvu hodnotami oddělenými čárkou nebo zobrazení tabulky v **seznamem název tabulky a zobrazení** a odpovídajících spojení podmínky **podmínku spojení** poskytnout nadřazené tabulce.

Příklad: Chcete-li importovat zaměstnance objektu a jeho s více hodnotami atributy. Existují dvě tabulky s názvem zaměstnance (hlavní tabulka) a oddělení (s více hodnotami).
Postupujte takto:

- Typ **zaměstnance** do **Tabulky nebo zobrazení/SP**
- Zadejte oddělení **seznamem název tabulky nebo zobrazení**.
- Podmínka spojení mezi zaměstnanců a oddělení psaním **Spojení podmínky**, například `Employee.DEPTID=Department.DepartmentID`.
![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Uložené procedury**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- Pokud máte velké množství dat, je vhodné implementovat stránkování s uložené procedury.
- Pro váš uložená procedura podporuje stránkování budete muset zadat Index zahájení a ukončení Index. Viz: [efektivní procházení velkých objemů dat](https://msdn.microsoft.com/library/bb445504.aspx).
- @StartIndexa @EndIndex nahrazeny v době provádění příslušných stránky velikost hodnotami nakonfigurovaný na stránce **Konfigurace kroku** . Třeba když spojnice načte na první stránce a velikost stránky je nastavený 500, v takové případě @StartIndex bude 1 a @EndIndex 500. Tyto hodnoty zvětšení při spojnice načte další stránky a změnit @StartIndex & @EndIndex hodnotu.
- Provádět parametry uložená procedura zadání parametrů v `[Name]:[Direction]:[Value]` formát. Zadejte každý parametr na samostatném řádku (podržením klávesy Ctrl + Enter získat nový řádek).
- Obecný spojnice SQL podporuje také operace importu z propojené serverů Microsoft SQL Server. Pokud informace o by měla získat z tabulky propojené serveru, mají tabulky určené ve formátu:`[ServerName].[Database].[Schema].[TableName]`
- Obecný spojnice SQL podporuje jenom objekty, které mají podobný struktury (jak alias názvem a datovým typem) mezi, která spuštění kroky informace a schéma vyhledávání. Pokud vybraného objektu z schématu a zadané informace na spouštěcím krok se liší, spojnice SQL se nepodařilo vizualizaci nepodporujeme scénáře.

**Dotaz SQL**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- Sada více výsledků dotazů nejsou podporované.
- Dotaz SQL zobrazený podporuje na stránkování a zadejte počáteční Index a koncová hodnota jako proměnná, aby podporoval stránkování.

### <a name="delta-import"></a>Delta Import
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Delta Import konfigurace vyžaduje některé další konfiguraci ve srovnání s úplný Import.

- Pokud se rozhodnete aktivační událost nebo snímku přístup ke sledování změn delta, zadejte databázi tabulky historie nebo snímku v **tabulce historie nebo název databáze snímek** pole.
- Budete taky potřebovat podmínku spojení mezi tabulkou historie a nadřazené tabulce třeba`Employee.ID=History.EmployeeID`
- Chcete-li sledovat transakci v nadřazené tabulce v tabulce historie, je nutné zadat název sloupce, který obsahuje informace o operace (přidat, aktualizovat nebo odstranění).
- Pokud se rozhodnete vodoznak ke sledování změn delta, zadejte název sloupce, který obsahuje informace o operace **vodu označit sloupec**název.
- Sloupce **změnit typ atributu** je potřebný pro změnit typ. V tomto sloupci mapy změny, které se vyskytuje změnit typ v zobrazení delta v primární tabulka nebo v tabulce s více hodnotami. V tomto sloupci může obsahovat změnit typ Modify_Attribute úroveň atributu změnu nebo přidat, upravit, nebo odstranit Změna typu procentní úrovni objektů změnit typ hodnoty. Pokud je jiný než nebo rovná hodnotě přidat, změnit nebo odstranit a pak můžete definovat jsou tyto hodnoty pomocí této možnosti.

### <a name="export"></a>Export
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Obecný spojnice SQL dál nebude podporovat Export metodami čtyři podporované jako:

- Tabulky
- Zobrazení
- Uložená procedura
- Dotaz SQL

**Zobrazení a tabulky**  
Pokud vyberete možnost zobrazení a tabulky, spojnice generuje odpovídajících dotazy chcete exportovat.

**Uložené procedury**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Pokud vyberete možnost uložená procedura, Export vyžaduje tři různé uložené procedury k provádění operací, vložení, aktualizace nebo odstranění.

- **Přidat název SP**: Tento SP spustí libovolný objekt dodává konektor pro vložení do příslušné tabulce.
- **Aktualizujte název SP**: Tento SP spustí Pokud libovolný objekt spojnice pro aktualizaci v příslušné tabulce.
- **Odstranění názvu SP**: Tento SP spustí Pokud libovolný objekt spojnice k odstranění v příslušné tabulce.
- Atribut vybraná ze schématu použit jako hodnota parametru uložené procedury. Například `EmployeeName: INPUT: @EmployeeName` (EmployeeName zaškrtnuté ve schématu spojnici a spojnice nahradí odpovídající hodnoty při tom export)
- Parametry uložená procedura spustíte zadání parametrů v `[Name]:[Direction]:[Value]` formát. Zadejte každý parametr na samostatném řádku (podržením klávesy Ctrl + Enter získat nový řádek).

**Dotaz SQL**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Pokud zvolíte možnost dotazu SQL, Export vyžaduje tři různé dotazy k provádění operací, vložení, aktualizace nebo odstranění.

- **Vložení dotazu**: Tento dotaz spustí Pokud libovolný objekt spojnice pro vložení do příslušné tabulce.
- **Aktualizujte dotaz**: Tento dotaz spustí Pokud libovolný objekt spojnice pro aktualizaci v příslušné tabulce.
- **Odstraňovací dotaz**: Tento dotaz spustí Pokud libovolný objekt spojnice k odstranění v příslušné tabulce.
- Atribut vybrali schématu použit jako hodnota parametru dotazu, například`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Řešení potíží

-   Informace o tom, jak povolit protokolování pro řešení potíží s spojnice zjistěte, [jak povolit trasování událostí pro Windows trasování spojnice](http://go.microsoft.com/fwlink/?LinkId=335731).
