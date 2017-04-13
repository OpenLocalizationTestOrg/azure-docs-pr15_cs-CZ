

#<a name="metadata-for-azure-technical-articles"></a>Metadata Azure technické články

Všechny Azure technické články obsahovat dva oddíly metadat - vlastnosti a oddílu značky. V části vlastnosti umožňuje některé automatizaci webu a obsahu SEO části značky umožňuje spoustu interní obsahu sestav. Obou částech jsou potřeba.

- [Syntaxe]
- [Použití]
- [Atributy a hodnot v části Vlastnosti]
- [Atributy a hodnoty v části značky]

##<a name="syntax"></a>Syntaxe

V části Vlastnosti používá následující syntaxi:

    <properties
       pageTitle="Page title that displays in search results and the browser tab | Microsoft Azure"
       description="Article description that will be displayed on landing pages and in most search results"
       services="service-name"
       documentationCenter="dev-center-name"
       authors="GitHub-alias-of-only-one-author"
       manager="manager-alias"
       editor=""
       tags="optional"
       keywords="For use by SEO champs only. Separate terms with commas. Check with your SEO champ before you change content in this article containing these terms."/>

V části značky používá následující syntaxi:

    <tags
       ms.service="required"
       ms.devlang="may be required"
       ms.topic="article"
       ms.tgt_pltfrm="may be required"
       ms.workload="na"
       ms.date="mm/dd/yyyy"
       ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

##<a name="usage"></a>Použití

- Název prvku a názvy atributů jsou malá a velká písmena.
- Na <properties> oddíl musí být první řádek souboru.
- Nechat prázdný řádek na konci každého oddílu metadat a před název stránky, aby na stránce název je správně převést do formátu HTML během procesu publikování.

## <a name="attributes-and-values-for-the-properties-section"></a>Atributy a hodnot v části Vlastnosti

![](./media/article-metadata/checkmark-small.png)**pageTitle**: povinné; důležité SEO. V záložce prohlížeče a jako název výsledek hledání se zobrazí text Tenhle atribut. Použití 55 60 znaků včetně mezer a to včetně identifikátor webu *| Microsoft Azure* (zadali jako: místo kanálu místo Microsoft Azure).  PageTitle by měl být liší od H1.

![](./media/article-metadata/checkmark-small.png)**Popis**: povinné; důležité SEO (relevance) a funkce webu. Popis by měl být aspoň 125 znaky dlouho budou 155 znaků maximální, včetně mezer. Popis účelu obsah tak, aby zákazníkům vědět, jestli se má ji zvolte ze seznamu výsledků hledání. Hodnota je:

- Tento text se může zobrazovat jako popis nebo abstraktní odstavce ve výsledcích hledání na Google.
- Tento text se zobrazí ve [výsledcích článek index](https://azure.microsoft.com/documentation/articles/).

![](./media/article-metadata/checkmark-small.png)**služby**: požadované články, která se týkají služby. Tato hodnota je poskytovat označovány jako "služba popisu". Seznam všech příslušných služeb oddělte je čárkami. První služba, která je seznam bude jednotka navigační navigace s popisem stránky a levém navigačním panelu, který je seznamech na stránce.

V článků, které určují hodnotu služby a documentationCenter hodnotu bude hodnota služby jednotka navigace s popisem cesty. Další hodnoty, které se zobrazí v seznamu jako značky v článku znalostní báze publikované. Hodnoty:

- služby Active directory
- Active directory b2c
- adresářové služby Active directory
- aplikace service\api
- rozhraní API kteří nemají manažerskou
- aplikace služby
- aplikace servic\mobile
- aplikace service\web
- aplikace service\logic
- aplikace brány
- přehledy aplikace
- automatizace
- portál Azure
- Správce prostředků Azure
- Azure zásobníku
- zálohování
- dávkové
- Doporučené postupy
- BizTalk služby
- mezipaměti
- CDN
- cloud services
- katalog dat
- data factory
- data jezera analýzy
- úložiště jezera dat
- devtest laboratoři
- DNS
- documentdb
- expressroute
- rozbočovače události
- hdinsight
- Rozbočovač IOT
- klíč trezoru
- Vyrovnávání zatížení
- výukové počítače
- Marketplace
- Služba Media
- zapojení Mobile
- mobilní služby
- Služba Multi-Factor authentication
- oznámení o rozbočovače
- provozní přehledy
- operace správy suite
- powerapps
- obnovení správce
- redis mezipaměti
- RemoteApp
- Správa práv
- Plánovač
- hledání
- Centrum zabezpečení
- Služba bus
- Služba struktury
- obnovení webu
- databáze SQL
- sklad SQL dat
- vytváření sestav SQL
- úložiště
- úložiště přihlašovacích údajů
- storsimple
- technologie pro analýzu toku
- správce dopravy
- virtuálních počítačích
- virtuální sítě
- vizuální – studio online
- Brána VPN
- webové stránky

![](./media/article-metadata/checkmark-small.png)**documentationCenter**: povinné pro nejlepší vývojáře zaměřené na zpracování články doporučená přes Centrum pro vývojáře. Zadejte Centrum pro vývojáře jedné nebo jazyk, který se vztahuje na článek. Hodnota, kterou můžete seznam bude jednotka navigační navigace s popisem stránky. V článků, které určují hodnotu služby a documentationCenter hodnotu bude hodnota služby jednotka navigace s popisem cesty. Hodnoty:

- **.NET**
- **nodejs**
- **Java**
- **PHP**
- **Python**
- **skutečné**
- **mobilní**: změněny. Nahraďte konkrétní mobilní platformu.
- **IOS**: Verifing tuto novou hodnotu
- **Android**: ověření tuto novou hodnotu
- **Windows**: ověření tuto novou hodnotu
- **xamarin**: ověření tuto novou hodnotu

![](./media/article-metadata/checkmark-small.png)**autoři**: potřeby pouze jednu hodnotu. Seznam na účtu GitHub primární Autor nebo článek systémy. Tenhle atribut jednotek Podtitulek na publikovaný článek. Seznam jediný přes plurální název atributu.

![](./media/article-metadata/checkmark-small.png)**Správci**: povinné, pokud jste si Microsoft přispěvatele. Seznam e-mailový alias s obsahu publikování odpovědi oblasti technologii. Pokud jsou Přispěvatel komunity atribut ale nechejte pole prázdné, abychom mohli vyplňovat.

![](./media/article-metadata/checkmark-small.png)**Editor**: nepoužívá. Nepoužívejte ho pro jiné účely.

![](./media/article-metadata/checkmark-small.png)**značky**: nepovinné. Zahrňte jenom v případě, že chcete povolit odkaz v části navigace s popisem cesty článek stránku článku index (http://azure.microsoft.com/documentation/articles/) na seznam prefiltered článků, které odpovídají jedné ze schválené hodnot. Tyto hodnoty jsou určeny pro lze seskupit obsahu dohromady, když obsahu seskupení není specifických pro službu. Tyto značky můžete taky poskytnout popisků, která udává, které se článek se týká zásobníku technologii. Tato hodnota **nemá** školy volné značky nebo hashtags; značky musí být povolený na webu. Můžete zadat více značek hodnot jednoho článku oddělte je čárkami. Schválené hodnoty jsou:

  - Architektura
  - Správce prostředků Azure
  - Správa služby Azure
  - fakturace
  - MySQL

![](./media/article-metadata/checkmark-small.png)**klíčová slova**: nepovinné. Používat jenom champs SEO. Samostatné podmínky čárkami. **Obraťte se na SEO champ před změna nebo odstranění obsahu v tomto článku obsahující tyto podmínky.** Tenhle atribut záznamů klíčových slov SEO champ má určené a sledování za účelem vylepšení hledání pořadí. Klíčová slova not vykreslovat publikované HTML. Ověření nevyžaduje Tenhle atribut.

## <a name="attributes-and-values-for-the-tags-section"></a>Atributy a hodnoty v části značky

![](./media/article-metadata/checkmark-small.png)**MS.Service**: povinné. Určuje služby Azure, nástroj nebo funkci, která se článek se týká. Jednou hodnotou v každé stránky.

 Pokud stránku platí pro více služeb, vyberte službu, na němž je většina přímo se vztahuje; **služby bus** hodnota, nikoli **webové stránky**, například má příspěvku, který používá aplikaci hostovaný na webech prokázat funkce Bus služby. Pokud stránku platí pro více služeb rovnoměrně z vnější, vyberte **více**. Pokud stránku se nevztahuje na žádnou službu (to bude méně častých), zvolte **NEDEF**.

 - **služby Active directory**
 - **Active directory b2c**
 - **adresářové služby Active directory**
 - **rozhraní API kteří nemají manažerskou**
 - **aplikace služby**: platí jenom pro obecné koncepční informace o aplikaci služby
 - **rozhraní api aplikace služby**
 - **aplikace služby logiky**
 - **aplikace služby mobile**
 - **aplikace služby web**
 - **přehledy aplikace**
 - **aplikace brány**
 - **automatizace**
 - **Správce prostředků Azure**
 - **Azure zabezpečení**
 - **Azure zásobníku**
 - **zálohování**
 - **dávkové**
 - **Doporučené postupy**
 - **BizTalk služby**
 - **fakturace**
 - **mezipaměti**
 - **CDN**
 - **cloud services**
 - **katalog dat**
 - **úložiště jezera dat**
 - **data jezera analýzy**
 - **devtest laboratoři**
 - **expressroute**
 - **hdinsight**
 - **Internet věci**
 - **Rozbočovač IOT**
 - **klíč trezoru**
 - **výukové počítače**
 - **Marketplace**: články o Azure marketplace
 - **Služba Media**
 - **zapojení Mobile**
 - **mobilní služby**
 - **Služba Multi-Factor authentication**
 - **více**: stránky se týká více služeb rovnoměrně z vnější
 - **NEDEF**: na stránce netýká všech služeb (méně častých)
 - **oznámení o rozbočovače**
 - **provozní přehledy**
 - **powerapps**
 - **obnovení správce**
 - **redis mezipaměti**
 - **RemoteApp**
 - **Správa práv**
 - **Plánovač**
 - **Centrum zabezpečení**
 - **Služba bus**
 - **Služba struktury**
 - **obnovení webu**: dřív služby recovery
 - **databáze SQL**
 - **sklad SQL dat**
 - **vytváření sestav SQL**
 - **úložiště**
 - **ukládání**: články o službách dostupné prostřednictvím úložišti Azure
 - **storsimple**
 - **správce dopravy**
 - **virtuálních počítačích**
 - **virtuální sítě**
 - **vizuální – studio online**
 - **Brána VPN**
 - **webové stránky**

![](./media/article-metadata/checkmark-small.png)**MS.devlang**: povinné. Určuje programovací jazyk, který článek se týká. Jedna hodnota na stránku.

 Pokud stránku platí pro oba jazyky rovnoměrně z vnější, vyberte **více**. Pokud stránku je primárně koncepční a její obsah obecně platí pro více jazyků programovacího, vyberte **více**. Pokud stránku není určené pro vývojáře a programovací jazyk použitelnost není důležité, klikněte **NA**. Pomocí **rozhraní rest api** pro identifikaci rozhraní REST API odkazy na témata.

 - **cpp**
 - **DotNet**
 - **Java**
 - **JavaScript**
 - **více**: na stránce platí pro více jazyků programovacího rovnoměrně z vnější.
 - **NEDEF**: stránka není zacílení vývojáři a se nevztahuje na všechny jazyky.
 - **nodejs**
 - **Cíl c**
 - **PHP**
 - **Python**
 - **rozhraní REST api**
 - **skutečné**


![](./media/article-metadata/checkmark-small.png)**MS.topic**: povinné. Zvláštnosti zadejte téma. Většina nové stránky vytvořené od přispěvatelů budou článek nebo odkaz.

 - **článek**: Toto téma, kurz, funkce průvodce nebo jiné než referenční článek

 - **stránka kampaň**: Azure.com pouze.  Na stránku, která je speciálně jako cílovou stránku pro externí kampaní a není součástí primárního webu i a.  Není vhodné používat pro článků si přečtěte následující dokumentaci nebo běžná dokumentu cílové stránky.  Příklady: azure.microsoft.com/develop/net/aspnet/; Azure.microsoft.com/Develop/Mobile/IOS/

 - **Odchylka – Centrum – Domovská stránka**: Azure.com pouze.  Vývoj střed domovské stránky, například/vyvíjet/čistého /

 - **získání začít článkem**: přiřazení na články, které jsou uvedené v části Začínáme nebo Přehled v levém navigačním panelu pro službu.

 - **Obrázek článek**: kurz "obrázek", která slouží k poskytnutí Úvod do služby nebo funkci, kterou je načtena návštěvníci začínáte používat službu rychle a jednotky bezplatné zkušební verze zápisy a aktivací MSDN. Přiřazení tuto hodnotu jen na články, které jsou uvedené v horní části cílovou stránku si přečtěte následující dokumentaci pro službu.

 - **Domovská stránka**: přečtěte následující dokumentaci pro nejvyšší úrovně domovskou stránku. Dvě máme pouze: azure.microsoft.com/documentation/ a msdn.microsoft.com/library/azure/

 - **Stránka indexu**: druhé úrovně úvodní stránky pro programovacím jazyce, služby nebo funkce. Tyto rozšířit mezi Azure.com a knihovny a slouží jako vstupní body konkrétnější, obory informace. Příklady: http://azure.microsoft.com/develop/mobile/resources-wp8/ http://msdn.microsoft.com/library/azure/jj673460.aspx, http://msdn.microsoft.com/library/azure/hh689864.aspx

 - **stránka infographic**: Azure.com pouze. Na stránku, která nabízí procházet infographic nebo plakát, například http://azure.microsoft.com/documentation/infographics/windows-azure/

 - **Přehled**: stránky odkaz rozhraní API (včetně rozhraní REST API) nebo stránku odkaz rutiny prostředí PowerShell

 - **služby – Domovská stránka**: Azure.com pouze.  Domovská stránka služby dokumentu, například /documentation/services/virtual-machines /

 - **Web části domovské stránky**: Azure.com pouze. "Domovské stránky" pro určitý typ obsahu na azure.com příklady: http://azure.microsoft.com/documentation/infographics/ http://azure.microsoft.com/documentation/scripts/, http://azure.microsoft.com/documentation/videos/home/, http://azure.microsoft.com/downloads/

 - **stránku video**: Azure.com pouze.  Na stránku, která obsahuje video, například http://azure.microsoft.com/documentation/videos/azure-webjobs-hosting-testing-net/

![](./media/article-metadata/checkmark-small.png)**MS.tgt_pltfrm**: povinné. Určuje cílové platformy, například Windows Linux, Windows Phone, iOS, Android a speciální mezipaměti platformách. Jednou hodnotou v každé stránky. Tato hodnota bude **NA** většině témata kromě mobile a virtuálních počítačích.

 - **mezipaměť v roli**
 - **více mezipaměti**
 - **redis mezipaměti**
 - **služby mezipaměti**
 - **sdílené mezipaměti**
 - **rozhraní příkazovém řádku**
 - **ibiza**: obsah, který používá portálu Ibiza. Slouží pouze v případech, kdy funkci jsou uvedeny dostupné různých portálu Ibiza a portálu aktuální.
 - **android mobilní telefon**: Azure.com pouze teď
 - **mobilní html**: Azure.com pouze teď
 - **Mobile ios**: Azure.com pouze teď
 - **Mobile kindle**: Azure.com pouze teď
 - **více Mobile**
 - **mobilní telefon nokia x**: Azure.com pouze teď
 - **mobilní phonegap**: Azure.com pouze teď
 - **mobilní sencha**: Azure.com pouze teď
 - **Mobile windows**: Azure.com pouze teď; Univerzální systému Windows
 - **Mobil systému windows**
 - **úložiště windows Mobile**
 - **mobilní xamarin**: Azure.com pouze teď; Xamarin všechny platformy
 - **android mobilní xamarin**: Azure.com pouze teď
 - **mobilní xamarin ios**: Azure.com pouze teď
 - **více**: stránky se týká oprávněnými rovnoměrně z vnější
 - **NEDEF**: specifikátorem platformy neplatí pro tuto stránku
 - **prostředí PowerShell**
 - **linux OM**
 - **více OM**
 - **OM windows**
 - **OM windows sharepoint**
 - **OM windows sql serveru**
 - **a-– Začínáme**: identifikuje skupiny a Začínáme stránek. Značka přidána 12/1/14.
 - **a co-se stalo**: slouží k identifikaci a získání začít příčinách stránky. Značka přidána 12/1/14.

![](./media/article-metadata/checkmark-small.png)**MS.workload**: povinné. Určuje Azure pracovní zátěž, kterým se použije na stránce. Jednu hodnotu pouze ve článku.

**aktualizace 8/6/15** Hodnotu ms.workload vůbec namapovala tak, že xls, nikoli hodnotu v souboru .md. Ms.workload hodnota je pořád potřeba k ověření, dokud tuto funkci lze aktualizovat. Teď je plánován, které fungují.  Stiskněte klávesovou zkratku **"na"** jako hodnota nyní.

![](./media/article-metadata/checkmark-small.png)**MS.Date**: povinné. Určuje datum poslední zkontrolována v článku relevance, přesnost, správný obrazovek a pracovní odkazy. Zadejte datum ve formátu mm/dd/rrrr. Toto datum se bude zobrazovat i na publikovaný článek jako datum poslední aktualizace.

![](./media/article-metadata/checkmark-small.png)**MS.Author**: povinné. Určuje právem přidružený k tématu. Tato hodnota vnitřní sestavy (například aktuálnost) slouží k správné právem přidružit v článku. Chcete-li zadat více hodnot by měl oddělte je středníkem. Jsou přijatelné Microsoft aliasy nebo úplnou e-mailové adresy. Délka může být delší než 200 znaků.


###<a name="contributors-guide-links"></a>Přispěvatelům Průvodce odkazy

- [Článek Základní informace](./../README.md)
- [Index pokyny články](./contributor-guide-index.md)


<!--Anchors-->
[Syntaxe]: #syntax
[Použití]: #usage
[Atributy a hodnot v části Vlastnosti]: #attributes-and-values-for-the-properties-section
[Atributy a hodnoty v části značky]: #attributes-and-values-for-the-tags-section
