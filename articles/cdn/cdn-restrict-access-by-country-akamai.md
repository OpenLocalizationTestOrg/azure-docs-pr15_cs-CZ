<properties
    pageTitle="Omezení přístupu k obsahu Azure CDN podle země | Microsoft Azure"
    description="Zjistěte, jak omezit přístup k obsahu Azure CDN pomocí funkce Geo filtrování."
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="akucer"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2016"
    ms.author="Lichard"/>

#<a name="restrict-access-to-your-content-by-country---akamai"></a>Omezení přístupu k obsahu podle země - Akamai

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Standardní Akamai](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>Základní informace

Když uživatel požádá obsahu, ve výchozím nastavení, obsah podávané bez ohledu na to, kde uživatel zadal tento požadavek z. V některých případech může chcete omezit přístup k obsahu podle země. Toto téma vysvětluje, jak používat funkci **Filtrování Geo** Pokud chcete službu povolit nebo zablokovat přístup podle země.

> [AZURE.IMPORTANT] Produkty Verizon a Akamai poskytují stejnou funkci filtrování geo, ale v uživatelském rozhraní se liší. Tento dokument popisuje rozhraní pro **Azure CDN standardní z Akamai**. Geo filtrování s **Azure CDN standardní/Premium z Verizon**najdete v článku [omezení přístupu k obsahu podle země - Verizon](cdn-restrict-access-by-country.md).

Informace o aspektech, které se vztahují ke konfiguraci tento typ omezení naleznete v části [Důležité informace](cdn-restrict-access-by-country.md#considerations) na konci tématu.  

![Filtrování země](./media/cdn-filtering/cdn-country-filtering-akamai.png)

##<a name="step-1-define-the-directory-path"></a>Krok 1: Definování adresář

Vyberte koncový bod v rámci portálu a najít kartu Geo filtrování na levém navigačním panelu na tuto funkci najít.

Při konfiguraci filtru země, třeba určit relativní cestu k umístění, do kterého uživatelé povoleno nebo odepření přístupu. Filtrování geo můžete použít pro všechny vaše soubory s "/" nebo vybrané složky zadáním adresářům "/ obrázky /". Můžete taky použít filtrování geo do jednoho souboru tak, že zadání souboru a vynecháním koncové lomítko "/ pictures/city.png".

Příklad adresáře cestu filtru:

    /                                 
    /Photos/
    /Photos/Strasbourg/
    /Photos/Strasbourg/city.png

##<a name="step-2-define-the-action-block-or-allow"></a>Krok 2: Definujte akce: blokování nebo povolení

**Blok:** Uživatelům z určité země bude mít přístup odepřen majetku požadované z této rekurzivní cesty. Pokud žádné jiné zemi možnosti filtrování nakonfigurovali pro dané umístění, pak všichni ostatní uživatelé budou mít přístup.

**Povolit:** Pouze z určité země se uživatelé přístup k prostředky požadované z této rekurzivní cesty.

##<a name="step-3-define-the-countries"></a>Krok 3: Definování zemí

Vyberte země, které chcete blokovat nebo povolit pro cestu. Další informace najdete v tématu [Azure CDN z Akamai země kódy](https://msdn.microsoft.com/library/mt761717.aspx).

Pravidlo blokování /Photos/Štrasburku/bude třeba filtrovat souborů, včetně:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>Kódy země

Funkce **Filtrování Geo** používá kódy zemí k definování země, ze kterých žádost povolené nebo blokované zabezpečené adresář. Kód země bude hledat v [Azure CDN z Akamai země kódy](https://msdn.microsoft.com/library/mt761717.aspx). 

##<a id="considerations"></a>Co byste měli zvážit

- Ji může trvat několik minut, než změny vaše země filtrování konfigurace se projeví.
- Tato funkce nepodporuje zástupné znaky (například "*").
- Konfigurace filtrování geo přidružené relativní cestu budou používána opakovaně na cesty.
- Jenom jedno pravidlo se dají použít stejné relativní cestu (nelze vytvořit více filtrů země, které odkazují na stejnou relativní cestu. Složky však může být více filtrů země. Toto je kvůli rekurzivní druh země filtry. Jinými slovy podsložkou dříve konfigurované složky můžete přidělovat jiné zemi filtru.

