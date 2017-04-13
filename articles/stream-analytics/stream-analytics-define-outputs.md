<properties
    pageTitle="Technologie pro analýzu výstupy můžete vysílat datovými proudy: možnosti pro úložiště, analýzy | Microsoft Azure"
    description="Přečtěte si o zaměření možnosti výstupy toku analýzy dat včetně Power BI pro výsledky analýzy."
    keywords="transformace, výsledky analýzy, možnosti ukládání dat."
    services="stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage"
    documentationCenter="" 
    authors="jeffstokes72"
    manager="jhubbard" 
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Technologie pro analýzu výstupy můžete vysílat datovými proudy: možnosti pro úložiště, analýzy

Při vytváření toku analýzy projektu, zvažte, jak bude potřeba Výsledná data. Jak se zobrazí výsledky analýzy toku úlohy a místo, kam se uloží jej?

Aby řadu aplikací vzorů má Azure toku analýzy různé možnosti pro ukládání výstupu a prohlížení analýzy výsledků. To umožňuje snadno zobrazit výstup projektu a nabídne vám flexibilitu spotřeby a ukládání výstupu projektu pro jiné účely a data skladování. Výstup konfigurovat úlohy musí existovat před zahájení úlohy a události začněte toku. Například pokud používáte úložiště objektů Blob jako výstup, úlohy nevytvoří účet úložiště automaticky. Je třeba vytvořit tak, že uživatel před tím, než ASA projektu.

## <a name="azure-data-lake-store"></a>Úložiště jezera dat Azure

Technologie pro analýzu toku podporuje [Úložiště jezera dat Azure](https://azure.microsoft.com/services/data-lake-store/). Toto úložiště umožňuje ukládat data z libovolné velikost, typ a požití rychlost provozní a průzkumné analýzy. V současné době vytváření a konfigurace výstupy úložiště jezera dat je podporováno pouze v portálu klasické Azure. Kromě toho analýzy toku musí oprávnění k přístupu k úložišti jezera dat. Informace o povolení a jak si zaregistrovat si náhled dat jezera úložiště přihlašovacích údajů (v případě potřeby) jsou uvedeny v [článku výstup jezera Data](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Povolit úložiště jezera dat Azure

Při ukládání dat jezera jako výstup na portálu Správa Azure se výzva k povolit připojení k existující úložiště jezera Data.  

![Povolte jezera úložiště dat](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Potom vyplňte vlastnosti úložiště jezera dat výstupu jak je vidět níže:

![Povolte jezera úložiště dat](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

Následující tabulka obsahuje názvy vlastností a jejich popis potřebných pro vytvoření úložiště jezera dat výstupu.

<table>
<tbody>
<tr>
<td><B>NÁZEV VLASTNOSTI</B></td>
<td><B>POPIS</B></td>
</tr>
<tr>
<td>Výstup Alias</td>
<td>Toto je používána v dotazech ke směrování výstupu dotazu pro toto úložiště jezera popisný název.</td>
</tr>
<tr>
<td>Název účtu</td>
<td>Název účtu úložiště jezera dat kde odesíláte výstup. Zobrazí se rozevírací seznam účtů jezera úložiště pro které má uživatel přihlášení k portálu přístup k.</td>
</tr>
<tr>
<td>Cesta předpona vzorek [<I>volitelné</I>]</td>
<td>Cesta k souboru použít k vytvoření souborů v rámci zadaný účtu úložiště jezera Data. <BR>{date}, {čas}<BR>Příklad 1: složka1/protokoly / {dat} / {čas}<BR>Příklad 2: složka1/protokoly / {dat}</td>
</tr>
<tr>
<td>Formát Datum [<I>volitelné</I>]</td>
<td>Pokud token datum používá v parametru path předponu, můžete vybrat formát data, ve kterém jsou uspořádány souborů. Příklad: Rrrr/MM/DD</td>
</tr>
<tr>
<td>Formát času [<I>volitelné</I>]</td>
<td>Pokud je token čas použitý v parametru path předponu, zadejte formát času, ve kterém jsou uspořádány souborů. Aktuálně podporované hodnota je pouze HH.</td>
</tr>
<tr>
<td>Formát serializace události</td>
<td>Formát serializace výstupní data. JSON, CSV a Avro jsou podporovány.</td>
</tr>
<tr>
<td>Kódování</td>
<td>Pokud formátu CSV nebo JSON kódování je nutné zadat. Jediný podporovaný formát kódování UTF-8 je v současné době.</td>
</tr>
<tr>
<td>Oddělovače</td>
<td>Platí pouze pro serializaci CSV. Technologie pro analýzu toku podporuje několik běžných oddělovače serializaci data ve formátu CSV. Podporované hodnoty jsou čárku, středník, místa, tab a svislá čára.</td>
</tr>
<tr>
<td>Formát</td>
<td>Platí pouze pro serializaci JSON. Čára oddělené označuje, že výstup zformátuje tím, že každý objekt JSON odděleni nový řádek. Pole určuje výstup bude naformátovat jako pole objektů JSON.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Obnovení povolení jezera úložiště dat

Musíte se znovu ověření účtu úložiště jezera dat, pokud jeho heslo změnila vaše úloha byla vytvořená nebo poslední ověření.

![Povolte jezera úložiště dat](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  


## <a name="sql-database"></a>Databáze SQL

[Databáze SQL Azure](https://azure.microsoft.com/services/sql-database/) mohou sloužit jako výstup pro data, která jsou relační povahy nebo aplikace, které jsou závislé na obsah je hostovaný v relační databázi. Úlohy analýzy toku zapíše do existující tabulky v databázi SQL Azure.  Všimněte si, že schéma tabulky se musí přesně shodovat polí a jejich typů vytváří výstup z práce. [Azure SQL datový sklad](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) lze také zadat jako výstup prostřednictvím databáze SQL výstup možnost i (Toto je funkce Náhled). Následující tabulka obsahuje názvy vlastností a jejich popis pro vytváření výstup databáze SQL.

| Název vlastnosti | Popis |
|---------------|-------------|
| Výstup Alias | Toto je popisný název použitý v dotazech směrování výstupu dotazu k této databázi. |
| Databáze | Název databáze, které odesíláte výstup |
| Název serveru | Název databáze SQL serveru |
| Uživatelské jméno | Uživatelské jméno, který má oprávnění k zápisu do databáze |
| Heslo | Heslo pro připojení k databázi |
| Tabulky | Název tabulky, kde se měly zapisovat výstupu. Název tabulky je velká a malá písmena a schématu tato tabulka by měla přesně shodovat počet polí a jejich typů použitým výstup projektu. |

> [AZURE.NOTE] Nabízení databáze SQL Azure je podporována pro výstup projektu v toku analýzy. Však Azure virtuální počítač serveru SQL Server s databází připojené nepodporuje. Toto je vyměřené poplatky za změnu v budoucích verzích.

## <a name="blob-storage"></a>Úložiště objektů BLOB

Úložiště objektů BLOB nabízí efektivní a scalable řešení pro ukládání velkých objemů Nestrukturovaná data v cloudu.  Úvod do úložiště objektů Blob Azure a jeho použití najdete v dokumentaci na [používání objektů BLOB](../storage/storage-dotnet-how-to-use-blobs.md).

Následující tabulka obsahuje názvy vlastností a jejich popis pro vytváření objektů blob výstupu.

<table>
<tbody>
<tr>
<td>NÁZEV VLASTNOSTI</td>
<td>POPIS</td>
</tr>
<tr>
<td>Výstup Alias</td>
<td>Toto je používána v dotazech ke směrování výstupu dotazu pro toto úložiště objektů blob popisný název.</td>
</tr>
<tr>
<td>Úložiště účtu</td>
<td>Název účtu úložiště kde odesíláte výstup.</td>
</tr>
<tr>
<td>Klíč účtu úložiště</td>
<td>Řeknu klávesu přidružené k účtu úložiště.</td>
</tr>
<tr>
<td>Kontejner úložiště</td>
<td>Kontejnery obsahují logické seskupení pro objekty BLOB uložené ve službě objektů Blob Microsoft Azure. Když nahrajete objektů blob ke službě objektů Blob, je nutné zadat kontejner pro tento objekt.</td>
</tr>
<tr>
<td>Cesta předpona vzorek [volitelné]</td>
<td>Cesta k souboru slouží k psaní svého objektů BLOB v zadaném kontejneru.<BR>V rámci cesty můžete pomocí jedné nebo více výskyty následující 2 proměnné Určete frekvenci vzniku objektů blob:<BR>{date}, {čas}<BR>Příklad 1: cluster1/protokoly / {dat} / {čas}<BR>Příklad 2: cluster1/protokoly / {dat}</td>
</tr>
<tr>
<td>Formát Datum [volitelné]</td>
<td>Pokud token datum používá v parametru path předponu, můžete vybrat formát data, ve kterém jsou uspořádány souborů. Příklad: Rrrr/MM/DD</td>
</tr>
<tr>
<td>Formát času [volitelné]</td>
<td>Pokud je token čas použitý v parametru path předponu, zadejte formát času, ve kterém jsou uspořádány souborů. Aktuálně podporované hodnota je pouze HH.</td>
</tr>
<tr>
<td>Formát serializace události</td>
<td>Formát serializace výstupní data.  JSON, CSV a Avro jsou podporovány.</td>
</tr>
<tr>
<td>Kódování</td>
<td>Pokud formátu CSV nebo JSON kódování je nutné zadat. Jediný podporovaný formát kódování UTF-8 je v současné době.</td>
</tr>
<tr>
<td>Oddělovače</td>
<td>Platí pouze pro serializaci CSV. Technologie pro analýzu toku podporuje několik běžných oddělovače serializaci data ve formátu CSV. Podporované hodnoty jsou čárku, středník, místa, tab a svislá čára.</td>
</tr>
<tr>
<td>Formát</td>
<td>Platí pouze pro serializaci JSON. Čára oddělené označuje, že výstup zformátuje tím, že každý objekt JSON odděleni nový řádek. Pole určuje výstup bude naformátovat jako pole objektů JSON.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Centrální události

[Událost rozbočovače](https://azure.microsoft.com/services/event-hubs/) je velmi scalable publikovat-přihlášení k odběru událostí ingestor. Můžete shromáždit miliony události sekundu.  Jeden rozbočovači událost jako výstup používá při výstup projektu toku analýzy budou zadání jiného streamování úlohy.

Existuje několik parametrů, které jsou potřebné pro nastavení události centrální datových proudů jako výstup.

| Název vlastnosti | Popis |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Výstup Alias | Toto je popisný název použitý v dotazech směrování výstupu dotazu k rozbočovači této události. |
| Služba Bus Namespace | Obor názvů Bus služby je kontejner pro sadu zpráv entity. Po vytvoření nové události centrální vytvořené obor Bus služby |
| Centrální události | Název události centrální výstup |
| Název zásady centrální události | Zásady sdílený přístup, který lze vytvořit na kartě událost centrální konfigurace. Každého zásadu sdílené přístupu budou mít název, oprávnění, která nastavíte, a přístupové klávesy |
| Klíč zásad centrální události | Sdílené přístupové klávesy slouží k ověření přístup k oboru Bus služby |
| Oddíl sloupec [volitelné] | Tento sloupec obsahoval klíč oddílu jako výstupního centrální události. |
| Formát serializace události | Formát serializace výstupní data.  JSON, CSV a Avro jsou podporovány. |
| Kódování | CSV a JSON UTF-8 je jediný podporovaný formát kódování v současné době |
| Oddělovače | Platí pouze pro serializaci CSV. Technologie pro analýzu toku podporuje několik běžných oddělovače serializaci dat ve formátu CSV. Podporované hodnoty jsou čárku, středník, místa, tab a svislá čára. |
| Formát | Platí pouze pro typ JSON. Čára oddělené označuje, že výstup zformátuje tím, že každý objekt JSON odděleni nový řádek. Pole určuje výstup bude naformátovat jako pole objektů JSON. |

## <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com/) mohou sloužit jako výstup projektu toku analýzy stanovit bohaté vizualizace zkušenosti s výsledky analýzy. Tuto funkci lze použít pro provozní řídicí panely, generování sestav a míru řízený úsilím vytváření sestav.

### <a name="authorize-a-power-bi-account"></a>Povolte účet Power BI

1.  Power BI vybrán jako výstup na portálu Správa Azure, zobrazí se výzva, abyste autorizovali stávajícího uživatele Power BI nebo vytvořte nový účet Power BI.  

    ![Povolte Power BI uživatele](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2.  Vytvořte nový účet není ještě jednu a nakonec klikněte na povolit schůzku.  Na obrazovce, jako jsou uvedeny následujících kroků.  

    ![Účet Azure Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3.  V tomto kroku zadání pracovní nebo školní účet pro povolení výstupu Power BI. Pokud nejsou už registraci k Power BI, zvolte přihlásit nahoru. Pracovní nebo školní účet, který používáte pro Power BI může lišit od Azure předplatné účtu, který jste právě přihlášeni se.

### <a name="configure-the-power-bi-output-properties"></a>Konfigurace vlastností výstup Power BI

Až budete mít účet Power BI ověřen, lze konfigurovat vlastnosti pro výstup Power BI. Následující tabulce je seznam názvů vlastností a jejich popis konfigurace výstup Power BI.

| Název vlastnosti | Popis |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Výstup Alias | Toto je popisný název použitý v dotazech směrování výstupu dotazu do této PowerBI výstupu. |
| Skupina pracovního prostoru | Povolte sdílení dat s ostatními uživateli Power BI můžete kliknutím na skupiny do svého účtu Power BI nebo zvolte "Můj pracovní prostor", pokud nechcete, aby zapisovat do skupiny.  Aktualizace existující skupiny vyžaduje obnovování ověřování Power BI. | 
| Název sady dat | Zadejte název sady dat, které je požadován pro výstup Power BI používat |
| Název tabulky | Zadejte název tabulky v části datovou sadu výstupu Power BI. V současné době výstup Power BI z toku analýzy úlohy může obsahovat pouze jednu tabulku v datové sadě |

Walk-through konfigurace výstup Power BI a řídicích panelů naleznete v článku [Azure toku technologie pro analýzu a Power BI](stream-analytics-power-bi-dashboard.md) .

> [AZURE.NOTE] Nevytvářejte explicitně datovou sadu a tabulka na řídicím panelu Power BI. Datová sada a tabulky se automaticky zobrazí při spuštění úlohy a zahájení úlohy čerpacího výstup do Power BI. Všimněte si, že pokud dotaz úlohy negeneruje žádné výsledky, datovou sadu a tabulky nevytvoří. Také mějte na paměti, že pokud Power BI už měli datovou sadu a tabulky se stejným názvem jako jeden uvedených v této úlohy toku analýzy, existující data přepíše.

### <a name="renew-power-bi-authorization"></a>Obnovení se tak mohli ověřovat Power BI

Musíte se znovu ověření účtu Power BI, pokud jeho heslo změnila vaše úloha byla vytvořená nebo poslední ověření. Pokud Multi-Factor Authentication (MFA) je nakonfigurovaný na vašeho klienta Azure Active Directory (AAD), budete taky muset obnovovat každých 2 týdnů tak mohli ověřovat Power BI. Příznakem tohoto problému je žádné výstupu projektu a "ověřit uživatele chyba" v protokolech operace:

  ![Chyba tokenu aktualizace Power BI](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Tento problém můžete vyřešit, zastavte spuštěná úloha a přejděte do výstupu Power BI.  Klikněte na odkaz "Prodloužit se tak mohli ověřovat" a restartujte práce z zastavit naposledy Chcete-li předejít ztrátě dat.

  ![Povolení obnovení Power BI](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Úložiště tabulek

[Úložiště tabulek Azure](../storage/storage-introduction.md) nabízí vysoce dostupné, datových scalable úložiště, takže aplikaci můžete přizpůsobit automaticky odpovídají požadavkům uživatelů. Úložiště tabulek je společnosti Microsoft NoSQL klíč/atribut úložiště, který z nich můžete využít pro strukturovaná data s méně omezení schéma. Úložiště tabulek Azure mohou sloužit k ukládání dat z hlediska trvalé a efektivně načítání.

Následující tabulka obsahuje názvy vlastností a jejich popis pro vytváření výstup tabulky.

| Název vlastnosti | Popis |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Výstup Alias | Toto je popisný název použitý v dotazech směrování výstupu dotazu pro toto úložiště tabulek. |
| Úložiště účtu | Název účtu úložiště kde odesíláte výstup. |
| Klíč účtu úložiště | Přístupová klávesa přidružené k účtu úložiště. |
| Název tabulky | Název tabulky. Pokud není k dispozici získat vytvoří tabulku. |
| Klíč oddílu | Název ve sloupci výstup obsahující klíč oddílu. Oddíl key je jedinečný identifikátor oddíl v rámci dané tabulku, která je součástí první entity primární klíč. Je řetězec, který může být až do velikosti 1 KB. |
| Klíče řádku | Název ve sloupci výstup obsahující klíč řádku. Řádek key je jedinečný identifikátor entity v daném oddílu. Vytváří druhé části entity primární klíč. Klíč řádku je řetězec, který může být až do velikosti 1 KB. |
| Velikost dávky | Počet záznamů pro operaci dávku. Obvykle výchozí hodnota je dostatečně pro většinu projekty, podívejte se do [operace dávku tabulka specifikace](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) podrobné informace o úpravách toto nastavení. |

## <a name="service-bus-queues"></a>Service Bus fronty

[Služba Bus fronty](https://msdn.microsoft.com/library/azure/hh367516.aspx) nabízejí první aplikace, první mimo FIFO doručení zprávy pro jednoho nebo více konkurenční spotřebitele. Obvykle zprávy se podle očekávání měly být přijetí a zpracování tak, že příjemce v časový pořadí, ve kterém se přidala do fronty a každá zpráva přijetí a zpracování klientem pouze jedné zprávy.

Následující tabulka obsahuje názvy vlastností a jejich popis pro vytváření fronty výstupu.

| Název vlastnosti | Popis |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Výstup Alias | Toto je používána v dotazech ke směrování výstupu dotazu do této služby Bus fronty popisný název. |
| Služba Bus Namespace | Obor názvů Bus služby je kontejner pro sadu zpráv entity. |
| Název fronty | Název Bus frontě. |
| Název zásady fronty | Při vytváření fronty můžete taky vytvořit zásady sdílené aplikace access na kartě fronty konfigurace. Každého zásadu sdílené přístupu budou mít název, oprávnění, která nastavíte, a přístupové klávesy. |
| Klíč zásad fronty | Sdílené přístupové klávesy slouží k ověření přístup k oboru Bus služby |
| Formát serializace události | Formát serializace výstupní data.  JSON, CSV a Avro jsou podporovány. |
| Kódování | CSV a JSON UTF-8 je jediný podporovaný formát kódování v současné době |
| Oddělovače | Platí pouze pro serializaci CSV. Technologie pro analýzu toku podporuje několik běžných oddělovače serializaci dat ve formátu CSV. Podporované hodnoty jsou čárku, středník, místa, tab a svislá čára. |
| Formát | Platí pouze pro typ JSON. Čára oddělené označuje, že výstup zformátuje tím, že každý objekt JSON odděleni nový řádek. Pole určuje výstup bude naformátovat jako pole objektů JSON. |

## <a name="service-bus-topics"></a>Služba Bus témata

Během fronty Bus službu poskytovat na jednom komunikace metodě od odesílatele sluchátko, [služby Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) tématech-n forma komunikace.

Následující tabulka obsahuje názvy vlastností a jejich popis pro vytváření výstup tabulky.

| Název vlastnosti | Popis |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Výstup Alias | Toto je popisný název použitý v dotazech směrování výstupu dotazu na toto téma Bus služby. |
| Služba Bus Namespace | Obor názvů Bus služby je kontejner pro sadu zpráv entity. Po vytvoření nové události centrální vytvořené obor Bus služby |
| Název tématu | Témata jsou zpráv entity, podobně jako rozbočovače událostí a fronty. Získat informace o datových proudů událost z mnoha různých zařízeních a služeb jsou určeny. Po vytvoření téma ho je rovněž určitý název. Zprávy zasílané do tématu nebudou dostupné, pokud nevytváříte předplatné, tak zajistěte, aby jeden nebo víc předplatných v části tématu |
| Název zásady téma | Když vytvoříte téma, můžete taky vytvořit zásady sdílené přístupu na kartě tématu Konfigurace. Každého zásadu sdílené přístupu budou mít název, oprávnění, která nastavíte, a přístupové klávesy |
| Téma klíčem zásad | Sdílené přístupové klávesy slouží k ověření přístup k oboru Bus služby |
| Formát serializace události | Formát serializace výstupní data.  JSON, CSV a Avro jsou podporovány. |
| Kódování | Pokud formátu CSV nebo JSON kódování je nutné zadat. Jediný podporovaný formát kódování UTF-8 je v současné době |
| Oddělovače | Platí pouze pro serializaci CSV. Technologie pro analýzu toku podporuje několik běžných oddělovače serializaci dat ve formátu CSV. Podporované hodnoty jsou čárku, středník, místa, tab a svislá čára. |

## <a name="documentdb"></a>DocumentDB

[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) je plně Správa přístupových práv NoSQL dokumentu databáze služba, která nabízí dotazu a transakce schématu bez dat, spolehlivý a dosáhnete výkonu a rychlý vývoj.

Následující tabulka obsahuje názvy vlastností a jejich popis pro vytváření DocumentDB výstupu.

<table>
<tbody>
<tr>
<td>NÁZEV VLASTNOSTI</td>
<td>POPIS</td>
</tr>
<tr>
<td>Název účtu</td>
<td>Název účtu DocumentDB.  Může to být také koncový bod pro účet.</td>
</tr>
<tr>
<td>Klíč účtu</td>
<td>Sdílené přístupová klávesa DocumentDB účtu.</td>
</tr>
<tr>
<td>Databáze</td>
<td>Název databáze DocumentDB.</td>
</tr>
<tr>
<td>Kolekce maskou</td>
<td>Vzorek název kolekce Collections se nemusí používat. Formát názvu kolekce lze vytvořit pomocí tokenu volitelné {oddíl}, kde oddíly začít od 0.<BR>Například Jsou platné zadávání těchto hodnot:<BR>Kolekce {oddíl}<BR>Kolekce<BR>Všimněte si, že kolekce musí existovat před úloha toku analýzy je spuštěn a nevytvoří se automaticky.</td>
</tr>
<tr>
<td>Klíč oddílu</td>
<td>Název pole v výstup události slouží k určení klíč pro rozdělení výstup v kolekcích.</td>
</tr>
<tr>
<td>ID dokumentu</td>
<td>Název pole v výstup události slouží k určení primární klíč, které chcete vložit nebo aktualizovat operace založené na.</td>
</tr>
</tbody>
</table>


## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
Jste byla zavedená do analýzy toku, služba spravovaných datového proudu technologie pro analýzu dat z Internetu věci. Další informace o této službě najdete v tématu:

- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
