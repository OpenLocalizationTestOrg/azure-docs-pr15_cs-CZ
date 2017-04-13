<properties
    pageTitle="Co je Azure hledání | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Azure hledání je plně Správa přístupových práv hostovanou cloudu vyhledávací služby. Další informace najdete v tomto přehled funkcí."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="what-is-azure-search"></a>Co je Azure hledání?

Azure hledání se řešení vyhledávání jako služby cloudu deleguje správu informací o serveru a infrastruktury společnosti Microsoft, nechte se službou připravené k použití, kterou můžete naplnit s daty pomocí přidejte hledání na webu nebo mobilní aplikaci. Azure hledání umožňuje snadno přidat robustní vyhledávání aplikace jednoduchého [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) nebo [.NET SDK](search-howto-dotnet-sdk.md) nebo použití bez správy hledání infrastruktury stále odborník na vyhledávání.

## <a name="give-your-users-a-powerful-search-experience"></a>Dát uživatelům výkonné vyhledávání

**Výkonné dotazů** můžete úpravě pomocí [Syntaxe jednoduchého dotazu](https://msdn.microsoft.com/library/azure/dn798920.aspx), který nabízí logické operátory, operátorů vyhledávání frázi, operátory příponu, priorita operátorů. Kromě toho [syntaxi dotazu Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) můžete povolit rozmazaný vyhledávání, blízkost vyhledávání, zvýšení termínů a regulární výrazy. Azure hledání taky podporuje vlastní lexikální analyzátory povolit aplikaci pro zpracování složitých vyhledávacích dotazů pomocí zvukové odpovídající a regulární výrazy.

**Podpora jazyků** se [zahrnuté pro 56 jazyky](https://msdn.microsoft.com/library/azure/dn879793.aspx). Pomocí Lucene analyzátory a Microsoft analyzátory (upřesněno podle roků přirozeného jazyka zpracování v Office a Bing), můžete hledání Azure analyzovat text v poli vyhledávání aplikace inteligentně zpracovávání lingvistické specifické pro určitý jazyk, včetně slovesné časy, pohlaví, Nesouměrné plurální podstatných jmen (například "myš" a "myší"), word zrušte úroková, dělení slov (jazyků se zápisem bez mezer) a další.

**Návrhy hledání** může být užitečné pro autocompleted hledání pruhy a automatickým dokončováním dotazů. [Zobrazí se návrhy dokumentů aktuální ve odkazu](https://msdn.microsoft.com/library/azure/dn798936.aspx) jako uživatele zadejte částečné vyhledávání při zadávání.

**Přístupů zvýraznění** [umožňuje](https://msdn.microsoft.com/library/azure/dn798927.aspx) uživatelům zobrazit fragment textu v jednotlivých výsledek, který obsahuje odpovídající položky pro svůj dotaz. Můžete vybrat pole, která vrátí zvýrazněný fragmenty.

**Působí navigace** je snadno přidat na stránku výsledků vyhledávání pomocí funkce hledání Azure. Použití [pouze jeden dotazu parametr](https://msdn.microsoft.com/library/azure/dn798927.aspx), Azure vyhledá všechny potřebné informace k vytvoření působí vyhledávání v uživatelském rozhraní vaše aplikace umožňuje uživatelům přechodu na podrobnosti a filtrování výsledků hledání (například filtr položky katalogu cena oblasti nebo značku).

**Prostorové GEO** podpory umožňuje inteligentně procesu, filtrování a zobrazení geografické polohy. Azure hledání umožňuje uživatelům, které můžete prozkoumat data založený na blízkost výsledků hledání do zadaného umístění nebo na určitých zeměpisnou oblast. Toto video vám vysvětlí, jak to funguje: [Channel 9: Azure vyhledávání a geografická data](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Filtry** umožňuje snadno začlenit působí navigace do uživatelského rozhraní aplikace, zlepšit formulace dotazu a filtr založený na nebo developer zadané uživatelem kritéria. Vytváření výkonných filtrů pomocí [Syntaxe OData](https://msdn.microsoft.com/library/azure/dn798921.aspx).

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>Posílí vývojáři pomocí služby snadno použitelné

**Dostupnost** zaručuje prostředí příliš spolehlivé vyhledávací služby. Když diagramů s měřítky správně, [Azure hledání nabízí SLA 99,9 %](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

**Plně spravovaných** začátku do konce řešení, Azure vyhledávání vyžaduje opravdu bez správy infrastruktury. Služby lze snadno přizpůsobit potřebám roztažením dvojrozměrné zpracovávání více ukládání dokumentů, vyšší zatížením dotazy nebo obojí.

**Integrace dat** pomocí [indexování](https://msdn.microsoft.com/library/azure/dn946891.aspx) umožňuje Azure vyhledávání a pokusí se automaticky procházení databáze SQL Azure, Azure DocumentDB nebo [úložiště objektů Blob Azure](search-howto-indexing-azure-blob-storage.md) synchronizovat indexu vyhledávání obsahu se vaše primární datový úložiště.

**Dokument výkon** je dostupný (aktuálně ve verze preview) [pro čtení a indexovat hlavní formátech](search-howto-indexing-azure-blob-storage.md) včetně Microsoft Office, stejně jako PDF a dokumentů HTML.

**Hledání přenosy analýzy** mají [snadno shromažďují a analyzovat](search-traffic-analytics.md) odemknout přehledy z co uživatelé píšete do vyhledávacího pole.

**Jednoduchý bodování** je klíčové výhodou Azure vyhledávání. [Hodnocení profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) slouží organizacím modelu relevance jako funkce hodnot v samotné dokumenty. Například může být vhodné novější produkty nebo zlevněného produkty zobrazit vyšší ve výsledcích hledání. Také je možné vytvářet bodování profilů pomocí značek přizpůsobených hodnocení podle hledání preference jste sledovány a uložené odděleně.

**Řazení** je nabízených k více polí pomocí schématu index a potom přepnete v době dotazu s parametrem jedním hledání.

**Stránkování** a omezení výsledků vyhledávání je [jednoduché s ovládacím prvkem pečlivě sestavenou](search-pagination-page-layout.md) nabízející Azure hledání nad výsledky hledání.  

**Hledání Průzkumníka** umožňuje problém dotazů všechny vaše indexy správné z Azure portálu váš účet, můžete snadno testovat dotazy a upřesnění bodování profily.

## <a name="how-it-works"></a>Jak to funguje

### <a name="1-provision-service"></a>1. poskytování služeb
Můžete číselník nahoru služby Azure vyhledávání pomocí [Portálu Azure](https://portal.azure.com/) nebo [Azure rozhraní API správy zdrojů](https://msdn.microsoft.com/library/azure/dn832684.aspx).

Podle toho, jak konfigurace vyhledávací služby serveru které budete používat buď bezplatné osy služba, která jsou sdílené s ostatní účastníci Azure hledání, nebo [zaplacený osy](https://azure.microsoft.com/pricing/details/search/) , která vyhrazuje vlastní zdroje používat jenom služby. Při zřizování služby, můžete taky oblast datovém centru, který je hostitelem služby.

Podle toho, jaký osy služby zvolíte, můžete změnit měřítko služby v dvojrozměrné: 1) přidat repliky rozšířit kapacity ke zpracování zatížením hodně dotazy a 2) přidat oddíly přidání úložiště pro více dokumentů. Zpracováním samostatně výkon úložiště a dotaz dokumentu, můžete přizpůsobit vyhledávací služby pro specifických potřeb.

### <a name="2-create-index"></a>2. vytvoření indexu
Před odesláním obsah služby Azure hledání je třeba definovat indexu vyhledávání Azure. Index je třeba databázovou tabulku, která obsahuje data a jsou přijatelné vyhledávacích dotazů. Definice schématu index přiřadit strukturu dokumenty, které chcete najít, podobně jako pole v databázi.

Schéma těchto indexů buď vytvořením na portálu Azure nebo programově [pomocí .NET SDK](search-howto-dotnet-sdk.md) nebo [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn798941.aspx). Jakmile je definován index, můžete pak nahrajte dat do Azure vyhledávací služby, kde je následně indexována.

### <a name="3-index-data"></a>3. data indexu
Po definování pole a atributy indexu budete chtít nahrát obsah do indexu. Odeslání dat do indexu můžete nabízené nebo vyžádané modelu.

Vložit modelu je k dispozici až indexování, které lze nakonfigurovat pro službu nebo plánované aktualizace (viz [operace indexování (Azure vyhledávací služby rozhraní REST API)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), která umožňují snadno jedí dat a změny dat z Azure DocumentDB, databáze SQL Azure, úložiště objektů Blob Azure nebo použitý ve OM Azure SQL serveru.

Model nabízených je k dispozici až SDK nebo REST API pro odeslání aktualizované dokumentů do indexu. Nabízená dat z libovolných datovou sadu pomocí formátu JSON. Přečtěte si článek [Přidání, aktualizovat, a odstraňovat dokumenty](https://msdn.microsoft.com/library/azure/dn798930.aspx) nebo [používání .NET SDK)](search-howto-dotnet-sdk.md) pokyny k načítání dat.

### <a name="4-search"></a>4. hledání
Po naplnění Azure vyhledávacím odkazu můžete nyní [problém vyhledávacích dotazů](https://msdn.microsoft.com/library/azure/dn798927.aspx) koncový bod služby jednoduchých požadavků HTTP pomocí rozhraní REST API nebo .NET SDK.

## <a name="try-it-now-for-free"></a>Praktická ukázka (zdarma!)
Zkuste hledat Azure dnes! Pokud už máte účet Azure, můžete [poskytování služby v bezplatné osy](search-create-service-portal.md).

Pokud nemáte účet Azure, že můžete zkusit bezplatnou, 60minutové relace bez zaregistrujte povinný. Přejděte do [Zkuste aplikaci služby Azure](http://go.microsoft.com/fwlink/p/?LinkId=618214) a vyberte "Web App." Vyberte šablonu "ASP.NET + Azure hledání" začít pracovat.
