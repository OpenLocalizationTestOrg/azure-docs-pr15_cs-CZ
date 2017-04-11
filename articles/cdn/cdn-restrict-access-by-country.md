<properties
    pageTitle="Omezení přístupu k obsahu Azure CDN podle země | Microsoft Azure"
    description="Zjistěte, jak omezit přístup k obsahu Azure CDN pomocí funkce Geo filtrování."
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="casoper"/>

#<a name="restrict-access-to-your-content-by-country---verizon"></a>Omezení přístupu k obsahu podle země - Verizon

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Standardní Akamai](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>Základní informace

Když uživatel požádá obsahu, ve výchozím nastavení, obsah podávané bez ohledu na to, kde uživatel zadal tento požadavek z. V některých případech může chcete omezit přístup k obsahu podle země. Toto téma vysvětluje, jak používat funkci **Filtrování Geo** Pokud chcete službu povolit nebo zablokovat přístup podle země.

> [AZURE.IMPORTANT] Produkty Verizon a Akamai poskytují stejnou funkci filtrování geo, ale v uživatelském rozhraní se liší. Tento dokument popisuje rozhraní pro **Azure CDN standardní z Verizon** a **Azure CDN Premium z Verizon**. Geo filtrování s **Azure CDN standardní z Akamai**najdete v článku [omezení přístupu k obsahu podle země - Akamai](cdn-restrict-access-by-country-akamai.md).

Informace o aspektech, které se vztahují ke konfiguraci tento typ omezení naleznete v části [Důležité informace](cdn-restrict-access-by-country.md#considerations) na konci tématu.  

>[AZURE.NOTE] Po konfiguraci nastavenou, bude platit pro všechny **Azure CDN z Verizon** koncové body v tomto profilu Azure CDN.

![Filtrování země](./media/cdn-filtering/cdn-country-filtering.png)

##<a name="step-1-define-the-directory-path"></a>Krok 1: Definování adresář

Při konfiguraci filtru země, třeba určit relativní cestu k umístění, do kterého uživatelé povoleno nebo odepření přístupu. Můžete použít země filtrování všechny soubory s "/" nebo zadáním adresářům vybrané složky.

Příklad adresáře cestu filtru:

    /                                 
    /Photos/
    /Photos/Strasbourg

##<a name="step-2-define-the-action-block-or-allow"></a>Krok 2: Definujte akce: blokování nebo povolení

**Blok:** Uživatelům z určité země bude mít přístup odepřen majetku požadované z této rekurzivní cesty. Pokud žádné jiné zemi možnosti filtrování nakonfigurovali pro dané umístění, pak všichni ostatní uživatelé budou mít přístup.

**Povolit:** Pouze z určité země se uživatelé přístup k prostředky požadované z této rekurzivní cesty.

##<a name="step-3-define-the-countries"></a>Krok 3: Definování zemí

Vyberte země, které chcete blokovat nebo povolit pro cestu. Další informace najdete v tématu [Azure CDN z Verizon země kódy](https://msdn.microsoft.com/library/mt761717.aspx).

Pravidlo blokování /Photos/Štrasburku/bude třeba filtrovat souborů, včetně:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>Kódy země

Funkce **Filtrování Geo** používá kódy zemí k definování země, ze kterých žádost povolené nebo blokované zabezpečené adresář. Kód země bude hledat v [Azure CDN z Verizon země kódy](https://msdn.microsoft.com/library/mt761717.aspx). Pokud zadáte "EU" (Evropa) nebo "Onenotových" (země Asie a Tichomoří), bude podmnožinu IP adresy, které pocházejí z jakékoli zemi v této oblasti blokované ani není povolené.


##<a id="considerations"></a>Co byste měli zvážit

- Konfigurace filtrování země se projeví může trvat až 90 minut změny.
- Tato funkce nepodporuje zástupné znaky (například "*").
- Země filtrování konfigurace přidružené relativní cestu budou používána opakovaně na cesty.
- Jenom jedno pravidlo se dají použít stejné relativní cestu (nelze vytvořit více filtrů země, které odkazují na stejnou relativní cestu. Složky však může být více filtrů země. Toto je kvůli rekurzivní druh země filtry. Jinými slovy podsložkou dříve konfigurované složky můžete přidělovat jiné zemi filtru.
