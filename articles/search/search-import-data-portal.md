<properties
    pageTitle="Import dat do Azure vyhledávání pomocí indexování na portálu Azure | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Pomocí Průvodce Azure hledání importem dat na portálu Azure procházení dat z úložiště objektů Blob Azure, stroage tabulky, databáze SQL a SQL Server na Azure VMs."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# <a name="import-data-to-azure-search-using-the-portal"></a>Import dat do hledání Azure pomocí portálu

Portál Azure poskytuje průvodce **Importem dat** na řídicím panelu hledání Azure pro načítání dat do indexu. 

  ![Import dat na panelu s příkazy][1]

Průvodce interně nakonfiguruje a vyvolá *indexování*automatizace několik kroků procesu indexování: 

- Připojení k externímu zdroji dat v aktuální Azure předplatného
- Automaticky vytvořit schématem index podle struktuře zdrojových dat
- Vytvoření dokumenty založené na řádků načtené ze zdroje dat.
- Odeslání dokumentů do indexu vyhledávací služby

Postup při používání ukázkových dat v DocumentDB můžete vyzkoušet. Navštěvujte blog o [začít pracovat s Azure hledání na portálu Azure](search-get-started-portal.md) pokyny.

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Zdroje dat podporované pomocí Průvodce importem dat

Průvodce importem dat podporuje následující zdroje dat: 

- Databáze Azure SQL
- SQL Server relačních dat na OM Azure
- Azure DocumentDB
- Úložiště objektů Blob Azure (ve verze preview)
- Úložiště tabulek Azure (ve verze preview)

Sloučený datovou sadu je povinný vstup. Lze importovat pouze z jedné tabulky, zobrazení databáze nebo rovnocenný datová struktura. Datová struktura měli vytvořit před spuštěním průvodce.

Všimněte si, že se stále nacházejí v náhledu, což znamená, že definici indexování podporovaným verzi preview rozhraní API některých indexování. Další informace a odkazy v tématu [Přehled indexování](search-indexer-overview.md) .

## <a name="connect-to-your-data"></a>Připojení k datům

1. Přihlaste se k [Portálu Azure](https://portal.azure.com) a řídicí panel otevřít služeb. **Služby hledání** můžete kliknout na panelu odkaz zobrazíte existujících služeb v aktuálního předplatného. 

2. **Import dat** klikněte na panelu s příkazy k snímku otevřít zásuvné importovat Data.  

3. Klikněte na **připojit k datům** k určení definice zdroje dat používají indexovací člen. Pro zdroje dat uvnitř předplatné průvodce obvykle zjišťování a přečtěte si informace o připojení, minimalizace požadavky na celkové konfiguraci.

| | |
|--------|------------|
|**Existujícího zdroje dat** | Pokud už máte indexování podle vyhledávací služby, můžete vybrat existující definice zdroje dat pro import jiné.|
|**Databáze Azure SQL** | Na stránce nebo prostřednictvím připojovací řetězec ADO.NET lze zadat název služby, přihlašovacích údajů pro databázi uživatel s oprávněním ke čtení a název databáze. Zvolte možnosti připojovacího řetězce chcete zobrazit nebo upravit vlastnosti. <br/><br/>Na stránce musí být zadán tabulky nebo zobrazení, které poskytuje řádků. Tato možnost se zobrazí po úspěšném připojení pojmenování rozevíracího seznamu tak, aby se dalo výběru.|
|**SQL Server na Azure OM** | Zadejte plně kvalifikovaný služby jméno, uživatelské ID a heslo a databáze jako připojovací řetězec. Pokud chcete použít tento zdroj dat, třeba jste již nainstalovali certifikát v místním úložišti, které jsou šifrovány připojení. <br/><br/>Na stránce musí být zadán tabulky nebo zobrazení, které poskytuje řádků. Tato možnost se zobrazí po úspěšném připojení pojmenování rozevíracího seznamu tak, aby se dalo výběru.
|**DocumentDB** |Požadavky na zahrnout účtu databáze a kolekce. Všechny dokumenty v kolekci zahrne do indexu. Můžete definovat dotazu můžete sloučit nebo filtrovat řádků nebo zjišťování změněné dokumenty pro operace aktualizace další data.|
|**Úložiště objektů Blob Azure** | Požadavky na zahrnout účtu úložiště a kontejner. Volitelně Pokud názvy objektů blob sledovat virtuální konvence pro účely seskupení, můžete určit virtuální adresář část jména jako do složky v kontejneru. Další informace najdete v tématu [Indexování úložiště objektů Blob (verze preview)](search-howto-indexing-azure-blob-storage.md) . |
|**Úložiště tabulek Azure** | Požadavky na zahrnout úložiště účtu a název tabulky. Pokud chcete můžete zadat dotaz načíst podmnožinu tabulky. Další informace najdete v tématu [Indexování úložiště tabulek (verze preview)](search-howto-indexing-azure-tables.md) . |

## <a name="customize-target-index"></a>Úprava cílové indexu

Předběžná index je obvykle odvozeno z datové sady. Přidání, úprava nebo odstranění pole k dokončení schéma. Kromě toho nastavit atributy na úrovni pole k určení jeho chování dalších hledání.

1. **Upravit cílovou index**zadejte jméno a **klíč** použitý k jednoznačně identifikovat každý dokument. Klíč musí být řetězec. Pokud hodnoty polí obsahovat mezery nebo čárky nezapomeňte nastavení dalších možností v tématu **Import dat** potlačí ověření pro tyto znaky.

2. Zkontrolujte a upravte zbývající pole. Pole název a typ jsou obvykle za vás vyplní. Můžete změnit datový typ.

3. Nastavte atributy indexu u každého pole:

 - Zobrazitelné ve výsledcích vyhledávání vrátí pole ve výsledcích hledání.
 - Filtrovat umožňuje pole do výrazy filtru, odkazovat.
 - Řaditelnou umožňuje pole se nemusí používat v řazení.
 - Facetable umožňuje pole pro působí navigaci.
 - Vyhledávání umožňuje fulltextové vyhledávání.
  
4. Pokud chcete zadat analyzer jazyk na úrovni pole, klikněte na kartu **Analýza** . Pouze jazyk analyzátory může být zadán v tomto okamžiku. Použití vlastní analyzer nebo jiné jazykové analyzer jako klíčové slovo, vzorku a tak dále budou vyžadovat kód.

 - Klikněte na **vhodné pro prohledávání** jmenovat fulltextové hledání na základě pole a povolení Analyzer rozevíracího seznamu.
 - Klikněte na analýza, které chcete. Další informace v tématu [Vytvoření indexu u dokumentů do více jazyků](search-language-support.md) .

5. Klikněte na tlačítko **Suggester** povolit návrhů automatickým dokončováním dotazů na vybraných polí.


## <a name="import-your-data"></a>Import dat

1. **Import dat**zadejte název pro službu indexování. Odvolat, součin Průvodce importem dat se vypočítá následujícím indexování. Dál Pokud chcete zobrazit nebo upravovat, vyberete ji z portálu Microsoft a ne spuštěním průvodce. 

2. Určete plán, který je založený na místní časové pásmo, ve kterém máte k dispozici službu.

3. Nastavení dalších možností můžete určit mezní hodnoty na zda indexování můžete pokračovat Pokud dokumentu se nezobrazí. Kromě toho můžete určit, zda **klíč** polí mohou obsahovat mezery a lomítka.  

## <a name="edit-an-existing-indexer"></a>Upravit existující indexování

Na řídicím panelu služby poklikejte na dlaždici indexování snímků se na seznam všech indexování vytvořené pro vaše předplatné. Poklikejte na některou indexování spustit, upravit nebo odstranit. Nahrazení rejstřík s jinou existující, změnit zdroj dat a nastavení možností pro prahové hodnoty chyby při indexování.

## <a name="edit-an-existing-index"></a>Upravit existující index

Do pole hledání Azure strukturální aktualizace rejstříku vyžaduje znovu vytvořit tuto index, které sestává z odstranění indexu znovu vytvářeli index a opětovné načtení dat. Konstrukce aktualizace zahrnují Změna datového typu a přejmenování nebo odstranění pole.

Úpravy, které nevyžadují nové vytvoření zahrnout přidáním nového pole, změna bodování profily, změně suggesters nebo změně analyzátory jazyk. Další informace najdete v tématu [Aktualizovat rejstřík](https://msdn.microsoft.com/library/azure/dn800964.aspx) .

## <a name="next-step"></a>Další krok

Přečtěte si tyto odkazy a další informace o indexování:

- [Indexování databáze Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [Indexování DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Indexování úložiště objektů Blob (verze preview)](search-howto-indexing-azure-blob-storage.md)
- [Indexování úložiště tabulek (verze preview)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

