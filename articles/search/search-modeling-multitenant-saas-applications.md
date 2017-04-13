<properties
    pageTitle="Modelování Multitenancy v Azure hledání | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Informace o běžných provedeních víceklientské SaaS aplikací při používání Azure vyhledávání."
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
    ms.date="10/26/2016"
    ms.author="ashmaka"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Návrh vzorků víceklientské SaaS aplikací a hledání Azure

Aplikace víceklientské je taková, která obsahuje stejné služeb a funkcí na libovolný počet klientů, kteří se nezobrazí nebo sdílet data jiném klientovi. Tento dokument popisuje klienta izolace strategie pro víceklientské aplikace vytvořené pomocí funkce hledání Azure.

## <a name="azure-search-concepts"></a>Azure hledání koncepty
Jako řešení vyhledávání jako služby Azure hledání vývojáři přidáte prostředí bohaté hledání aplikací bez správy všechny infrastruktury nebo stává odborník na vyhledávání. Data jsou nahrál(a) na server služby a potom uložený v cloudu. Pomocí jednoduchých požadavků pro rozhraní API pro hledání Azure, data lze potom upravit a prohledávat. V [tomto článku](http://aka.ms/whatisazsearch)najdete základní informace o službu. Před projednávat provedeních, je důležité některé principech v Azure vyhledávání.

### <a name="search-services-indexes-fields-and-documents"></a>Vyhledávací služby, indexy, polí a dokumenty
Při hledání Azure jednu přihlásí _Vyhledávací služby_. Při odeslání dat do hledání Azure je uložený v _indexu_ v rámci vyhledávací služby. Může být několik indexy v rámci jedné služby. Použití známých koncepcí databází, lze vyhledávací služba připodobnit k databázi během indexy v rámci služby lze připodobnit tabulkami v databázi.

Každý index v rámci vyhledávací služby obsahuje vlastní schéma, které je definován několika upravitelných _polí_. Data se přidá do indexu vyhledávání Azure ve formě jednotlivé _dokumenty_. Každý dokument musí nahráli do určitého indexu a musí přizpůsobit tento index schéma. Při hledání dat pomocí vyhledávání Azure, jsou fulltextové vyhledávacích dotazů vystavený konkrétní index.  Pokud chcete porovnat koncepce těm, databáze, pole lze připodobnit do sloupců v tabulce a dokumenty lze připodobnit řádků.

### <a name="scalability"></a>Škálovatelnost:
Všechny Azure vyhledávací služby v standardní [ceny osy](https://azure.microsoft.com/pricing/details/search/) můžete změnit velikost v dvojrozměrné: ukládání a dostupnosti.
* _Oddíly_ můžete přidat ke zvýšení úložiště vyhledávací služby.
* _Repliky_ lze přidat do služby, pokud chcete zvýšit výkon požadavky, které můžete dělat vyhledávací služby.

Přidávání a odebírání oddíly a repliky na vám umožní kapacita vyhledávací služba roste, pomocí množství dat a provoz požadavky aplikace. Aby vyhledávací služby dosáhnout čtení [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)vyžaduje dva repliky. Aby služba dosáhnout pro čtení i zápis [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)vyžaduje tři repliky.


### <a name="service-and-index-limits-in-azure-search"></a>Služba a index limity hledání Azure
Obsahuje několik různých [ceny úrovní](https://azure.microsoft.com/pricing/details/search/) v Azure vyhledávání, má každá úrovně rozdílnými [limity a kvóty](search-limits-quotas-capacity.md). Některé z těchto omezení jsou na úrovni služeb, některé jsou úrovni index, a některé úrovni oddíl.


|                                  | Základní     | Standard1   | Standard2   | Standard3   | Standard3 HD  |
|----------------------------------|-----------|-------------|-------------|-------------|---------------|
| Maximální repliky za služby     | 3         | 12          | 12          | 12          | 12            |
| Maximální oddíly za služby   | 1         | 12          | 12          | 12          | 1             |
| Maximální hledání jednotky (repliky * oddíly) za služby | 3         | 36          | 36          | 36          | 36 (max 3 oddíly)            |
| Maximální dokumentů za služby    | 1 milion | 180 milionů | 720 milionů | 1.4 miliardy | 600 miliónů   |
| Maximální úložiště na služby      | 2 GB      | 300 GB      | 1.2 TB      | 2.4 TB      | 600 GB        |
| Maximální dokumenty na oddíl  | 1 milion | 15 milionů  | 60 milionů  | 120 milionů | 200 milionů   |
| Maximální velikosti úložiště na oddíl    | 2 GB      | 25 GB       | 100 GB      | 200 GB      | 200 GB        |
| Maximální indexy za služby      | 5         | 50          | 200         | 200         | 3000 (max 1000 indexy/oddílu)          |


#### <a name="s3-high-density"></a>S3 Vysoký hustoty "
V Azure hledání S3 ceny osy není možnost režimu Vysoký hustoty (HD) navrženy speciálně pro víceklientské scénáře. V mnoha případech je nezbytné k podpoře velkého počtu menší student v rámci jedné služby dosáhnout výhody efektivity jednoduché a náklady.

S3 HD umožňuje mnoho malé indexy na zabalit v části Správa jednoho vyhledávací služby tak, že obchodování možnost rozšiřování indexů pomocí oddílů pro možnost hostovat indexy v jedné služby.

Služby S3 namítají, může mít mezi 1 až 200 indexy hostujících společně může až 1.4 miliardy dokumenty. S3 HD na druhou stranu umožní jednotlivé indexy pouze přejdete až 1 milion dokumenty, ale zvládne až 1000 indexy na oddíl (až 3000 za service) s celkový dokumentu počet 200 milion oddílu (až 600 milion služby).



## <a name="considerations-for-multitenant-applications"></a>Co zvážit při víceklientské aplikací
Víceklientské aplikací efektivně smíte zdrojů mezi klientů při zachování některé úroveň ochrany osobních údajů mezi různých klientů. V úvahu několik při návrhu architektura pro tyto aplikace:

* _Klient izolace:_ Aplikace vývojáři muset udělat vhodná opatření, že žádný klienti neoprávněné nebo nežádoucí přístup k datům jiných klientů. Za pohledu ochrany osobních údajů dat je nutné klienta izolace strategie efektivní nakládání s sdílených a ochrana před vysokou úroveň šumu sousední.
* _Cloudu náklady na zdroje:_ Stejně jako u jakékoli jiné aplikace, musí zůstat softwarových řešení náklady konkurenční jako součást aplikace víceklientské.
* _Usnadnění operací:_ Při vytváření víceklientské architektura, jejich dopad na operace a složitosti aplikace je důležitá poznámka. Azure vyhledávání obsahuje [SLA 99,9 %](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* _Globální stopu:_ Víceklientské aplikací může být nutné efektivně vytisknout klienti, které jsou po celém světě.
* _Škálovatelnost:_ Vývojáři aplikací vzít do úvahy, jak budou odsouhlasit zachování dostatečně vysoké úrovně složitost aplikace a navrhování aplikaci zobrazit počet klientů s velikostí dat a pracovní zátěž klienti účastníků.

Azure hledání nabízí několik omezení, které můžete použít k izolovat dat a pracovní zátěž klienti účastníků.

## <a name="modeling-multitenancy-with-azure-search"></a>Modelování multitenancy pomocí funkce hledání Azure
V případě scénáři víceklientské vývojář aplikace využívá jedné nebo víc služeb hledání a dělit jejich klienti mezi službami a indexy. Azure vyhledávání obsahuje několik běžných vzorků při modelování víceklientské situace:

1. _Index na klienta:_ Každého klienta má vlastní index v rámci vyhledávací služba, která jsou sdílené s další student.
1. _Služby jednoho klienta:_ Každého klienta má samostatném vyhrazené Azure vyhledávací služby nabízející nejvyšší úroveň dat a pracovní zátěž oddělení.
1. _Kombinaci obou:_ Větší, aktivní více klientů přiřazené vyhrazené služby, zatímco menší klienti přiřazené jednotlivé indexy v rámci sdílených služeb.

## <a name="1-index-per-tenant"></a>1. indexovat na klienta
![Portrétu modelu index na klienta](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

V modelu index jednoho klienta víc klientů zaplnit jednoho Azure vyhledávací služby, kde každého klienta má vlastní index.

Klienti dosáhnout izolace dat, protože všechny žádosti o hledání a dokumentu operace vydáním úrovni indexu v Azure vyhledávání. Ve vrstvě aplikací je potřeba povědomí směrování adres různých klientů správné indexy při také řízení zdrojů na úrovni služby přes všechny klienty.

Atribut klíčové modelu index jednoho klienta je možnost vývojář aplikace oversubscribe kapacity mezi klienty aplikace Vyhledávací služby. Máte klienti lichý výskyt zátěží na projektu, můžete optimální kombinací klienti rozvržena vyhledávací služba indexy tak, aby zahrnoval počet vysoce aktivní, náročné klientů při souběžné obsluze fronty méně aktivní klienti. Nejvhodnější poměr je nelze modelu řešit situacích, kdy každého klienta souběžně vysoce aktivní.

Model index jednoho klienta je základem modelu variabilních nákladů, kde je zakoupili celý Azure vyhledávací služba věcí a následně vyplní s klienty. Tato funkce umožňuje nevyužitá kapacita označené pro u zkušebních verzí a bezplatné účty.

Pro práci s globální půdorys nemusí být modelu index jednoho tenanta co nejefektivněji. Pokud se po celém světě aplikace student, třeba samostatná služba pro každou oblast, kterou lze duplikovat náklady na každý z nich.

Azure hledání umožňuje škále jednotlivé indexy i celkový počet indexy naroste. Pokud odpovídající ceny je vybrán osy, oddíly a repliky lze přidat do celého vyhledávací služba při jednotlivé indexu v rámci služby roste z hlediska úložiště nebo přenosy je příliš velký.

Pokud celkový počet indexy roste do jednoho služby je příliš velký, musí zřízení tak, aby zahrnoval nové klienti jiné služby. Pokud indexy budete muset přesunout mezi službami hledání při přidávání nové služby, data z indexu obsahuje ručně zkopírovat z jedné indexu druhý při hledání Azure neumožňuje rejstříku, které se mají přesunout.


## <a name="2-service-per-tenant"></a>2. služba na klienta
![Portrétu modelu služby na klienta](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

V architekturu služby jednoho tenanta každého klienta má vlastní vyhledávací služby serveru.

V tomto modelu dosahuje aplikace nejvyšší úroveň izolace pro jeho klienti. Každou službu má snaží o ukládání a výkon pro zpracování požadavku hledání, jakož i různé rozhraní API klíče.

U aplikací, kde každého klienta obsahuje velké nároky nebo pracovní zátěž malé variabilitě z klienta na klienta model služby jednoho klienta je efektivní výběr jako zdroje nejsou sdíleny různých klientů úloh.

Služba jednoho tenanta modelu nabízí taky výhodou modelu předvídatelná, pevné náklady. Dokud není k dispozici klienta, kterého chcete vyplnit, ale náklady na jednoho klienta je vyšší než datového modelu pro index jednoho klienta je žádné předem investic do celého vyhledávací služby.

Model služby jednoho klienta je efektivně volba aplikací se globální stopu. S geograficky distributed klientů není těžké si službu každého klienta v příslušné oblasti.

Úkoly v měřítka tento způsob vznikají při jednotlivé klienti odrůst své služby. Azure hledání aktuálně nepodporuje upgradu ceny osy vyhledávací služby, aby všechna data by měl ručně zkopírovala do nové služby.

## <a name="3-mixing-both-models"></a>3. oba modely společným
Jiné vzor pro modelování multitenancy je společným strategie index jednoho klienta a služeb na klienta.

Podle společným dva vzorky, můžete největší klientů aplikace zaplnit vyhrazené služby během fronty méně aktivní, menší klientů můžete zaplnit indexy ve sdílených služeb. Tento model zajišťuje největší klienti konzistentní vysoký výkon ze služby zároveň zabezpečit menší klienti z vysokou úroveň šumu sousední.

Provádění této strategie však využívá prognostické předpovídat, které klienti budou vyžadovat vyhrazené služby versus indexu v sdílených služeb. Aplikace složitost zvyšuje se potřebujete ke správě obě tyto multitenancy modely.

## <a name="achieving-even-finer-granularity"></a>Dosažení i lepší rozlišení
Výše uvedené provedeních k modelování víceklientské scénáře v Azure hledání předpokládá obor jednotné, kde je každý klient celé instance aplikace. Však aplikací může někdy úchyt pro mnoho menší oborů

Není-li modely služby jednoho klienta a index jednoho klienta dostatečně malé obory, je možné model indexu dosáhnout ještě podrobnějšího granularity.

Pokud chcete, aby jeden rejstřík s odlišným chováním pro koncové body jiného klienta, můžete přidat pole rejstříku, což znamená, že určitou hodnotu pro každou možnou klienta. Pokaždé, když klienta volá Azure vyhledávacího dotazu nebo změnit index, kód v klientské aplikaci určuje odpovídající hodnotu tohoto pole pomocí možnosti [Filtr](https://msdn.microsoft.com/library/azure/dn798921.aspx) Azure vyhledávání v době dotazu.

Tato metoda slouží k dosažení funkce samostatné uživatelských účtů, úrovně samostatné oprávnění a i úplně samostatné aplikace.

## <a name="next-steps"></a>Další kroky
Azure hledání se atraktivní vyplatí pro mnoho aplikací, [Přečtěte si další informace o výkonné možnosti služby](http://aka.ms/whatisazsearch). Při vyhodnocování různých provedeních víceklientské aplikací, zvažte [různých úrovní ceny](https://azure.microsoft.com/pricing/details/search/) a odpovídajících [omezení služeb](search-limits-quotas-capacity.md) pro nejlepší přizpůsobení Azure vyhledávání tak, aby odpovídal aplikace pracovního vytížení a architektury všech velikostí.

Lze směrovat všechny dotazy týkající se Azure vyhledávání a víceklientské scénáře azuresearch_contact@microsoft.com.
