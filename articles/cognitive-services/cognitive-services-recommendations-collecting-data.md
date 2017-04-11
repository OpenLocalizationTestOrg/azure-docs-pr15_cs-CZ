<properties
    pageTitle="Shromažďování dat za účelem školení modelu: počítače výukové doporučení rozhraní API | Microsoft Azure"
    description="Azure počítače výukové doporučení: shromažďování dat za účelem školení modelu"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>Shromažďování dat za účelem školení modelu #

Rozhraní API doporučení učí z minulých transakce najít, co by měl být položky doporučené k určitému uživateli.

Po vytvoření modelu, budou musíte zadat dvě druh informací před provedením všech školení: soubor katalogu a použití zásad správy informací.

>   Pokud jste to ještě neudělali, doporučujeme provést [Rychlý start](cognitive-services-recommendations-quick-start.md).


## <a name="catalog-data"></a>Katalog dat ##

### <a name="catalog-file-format"></a>Formát souboru katalogu ###

Soubor katalogu obsahuje informace o položkách, že budete nabízet zákazníkovi.
Katalog dat měli podle pokynů v tomto formátu:

- Bez funkcí:`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Pomocí funkcí:`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>Ukázka řádků v souboru katalogu

Bez funkcí:

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Pomocí funkcí:

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>Podrobnosti o formátu

| Jméno | Povinné | Typ |  Popis |
|:---|:---|:---|:---|
| Id položky |Ano | [A-z], [a-z], [0 – 9], [_] & #40; Podtržení a #41; [-] & #40; pomlčku & #41;<br> Maximální délka: 50 | Jedinečný identifikátor položky. |
| Název položky | Ano | Alfanumerické znaky<br> Maximální počet znaků: 255 | Název položky. |
| Kategorie položky | Ano | Alfanumerické znaky <br> Maximální počet znaků: 255 | Kategorie, ke kterému patří tuto položku (například vaření publikace obrázkům...); může být prázdné. |
| Popis | Ne, pokud jsou funkce prezentovat (, ale může být prázdný) | Alfanumerické znaky <br> Maximální délka: 4000 | Popis této položky. |
| Seznam funkcí | Ne | Alfanumerické znaky <br> Maximální délka: 4000; Maximální počet funkcí: 20 | Seznam oddělený čárkami názvu funkce = funkce hodnota, která slouží k rozšíření modelu doporučení.|

#### <a name="uploading-a-catalog-file"></a>Nahrání souboru katalogu

Podívejte se na [referenční rozhraní API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1) pro odeslání souboru katalogu.  

Všimněte si, že obsah souboru katalogu mají předané jako požadavku.

Pokud odešlete několik katalogu souborů do stejné modelu s několika voláním jsme vloží jenom nové položky katalogu. Existující položky zůstanou s původní hodnoty. Katalog dat nelze aktualizovat tímto způsobem.

>   Poznámka: Maximální velikost souboru je 200MB.
>   Maximální počet položek v katalogu podporované je 100 000 položek.


## <a name="why-add-features-to-the-catalog"></a>Proč přidávat funkce ke katalogu?

Modul doporučení vytvoří statistické model, který se dovíte položky, které by mohly být přidali, nebo jste si koupili zákazníkem. Pokud máte není možné vytvořit si model na dalších výskytů samotný nového produktu, který má nikdy serveru s ním. Řekněme, že začnete nabízí novou "prospěchu dětí violin" ve vašem úložišti, protože mít nikdy prodané této violin před neumí určit, jaké další položky doporučit s této violin.

Který říká, pokud modul zná informace o této violin (tedy je musical instrument je pro děti věku 7 – 10, není drahé violin, atd.), potom modul můžete čerpat z ostatních produktů s podobné funkce. Například volně violin v minulosti a obvykle osoby, které koupit violins mají za následek koupit klasického hudbu disků CD-ROM s aktualizacemi a stojany hudbu listu.  Systém můžete tyto připojení mezi funkcí najít a poskytují doporučení na základě funkcí během nové violin má malé použití.

Funkce naimportují jako součást katalog dat, a potom jejich pořadí (nebo důležitost funkci v modelu) souvisí po dokončení rank Tvůrce dotazů. Funkce rank můžete změnit podle vzorek použití zásad správy informací a typ položky. Ale konzistentní používání/položek, pořadí by měl pouze malé kolísání. Pořadí funkcí je nezáporné číslo. Číslo 0 znamená, že tato funkce nebyla zařazených jako (se stane, když vyvolat toto rozhraní API před dokončením první rank vytvořit). Datum niž byl atributy pořadí se nazývá aktuálnost skóre.


###<a name="features-are-categorical"></a>Funkce jsou Categorical

To znamená, že byste měli vytvořit funkce, které se podobají kategorie. Například cena = 9.34 není kategorií funkcí. Na druhé straně funkci jako priceRange = Under5Dollars je kategorií funkcí. Jiné běžné chybě, je použít název položky jako funkce. To způsobí název položky jako jedinečné, takže nebude popisují kategorie. Zkontrolujte, že funkce představují druhy položek.


###<a name="how-manywhich-features-should-i-use"></a>Jak n /, které funkce mám použít?


Nakonec doporučení vytvářet podporuje vytvoření modelu až 20 funkcemi. Mohli byste přiřadit víc než 20 funkce položky ve vašem katalogu, ale jsou očekáváte, proveďte hodnocení Tvůrce dotazů a vyberte pouze funkce Vysoký v rozsahu. (Funkci se pořadí zabere nejméně 2.0 je dobře funkce používat!). 


###<a name="when-are-features-actually-used"></a>Pokud funkce skutečně slouží?

Funkce při modelem není dost data transakce poskytnout doporučení na transakce informace samostatně. Funkce tak bude mít největší vliv na "studenou položky" – položky s malým počtem transakcí. Pokud mají všechny položky dostatečné transakce informace nemusí potřebujete rozšíření modelu pomocí funkce.


###<a name="using-product-features"></a>Použití funkce produktu

Použití funkcí jako součást vašeho Tvůrce dotazů, které bude nutné:

1. Zkontrolujte, že katalogu má funkce když ho nahrajte.

2. Aktivace sestavení jejich pořadí. To bude udělat na důležitost/pořadí funkcí.

3. Aktivace sestavení doporučení, nastavení následující sestavit parametry: useFeaturesInModel nastaveno true, allowColdItemPlacement true (pravda), a nastavte modelingFeatureList do seznamu hodnoty s oddělovači funkcí, které chcete použít k vylepšení modelu. Další informace najdete v tématu [doporučení vytvořit typ parametrů](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) .





## <a name="usage-data"></a>Použití zásad správy informací ##
Použití soubor obsahuje informace o použití těchto položek nebo transakce z vašeho podniku.

#### <a name="usage-format-details"></a>Podrobnosti o použití formátu
Použití soubor je soubor CSV (hodnoty oddělené čárkou), kde každý řádek v souboru použití představuje interakce mezi uživateli a položky. Každý řádek zformátována následujícím způsobem:<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| Jméno  | Povinné | Typ | Popis
|-------|------------|------|---------------
|Id uživatele|         Ano|[A-z], [a-z], [0 – 9], [_] & #40; Podtržení & #41; [-] & #40; pomlčku & #41;<br> Maximální počet znaků: 255 |Jedinečný identifikátor uživatele.
|Id položky|Ano|[A-z], [a-z], [0 – 9], [& #95;] & #40; Podtržení & #41; [-] & #40; pomlčku & #41;<br> Maximální délka: 50|Jedinečný identifikátor položky.
|Čas|Ano|Datum ve formátu: YYYY/MM/ddTHH (například 2013/06/20T10:00:00)|Čas data.
|Události|Ne | Jedna z následujících akcí:<br>• Klepnutím na tlačítko<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Nákupu| Typ transakce. |

#### <a name="sample-rows-in-a-usage-file"></a>Ukázka řádků v souboru použití

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>Nahrání souboru použití

Podívejte se na [referenční rozhraní API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) pro nahrávání použití souborů.
Všimněte si, že potřebujete předat obsah souboru použití v těle zprávy HTTP hovoru.

>  Poznámka:

>  Maximální velikost souboru: 200MB. Může odeslat několik použití souborů.

>  Budete muset nahrát soubor katalogu před zahájením přidávání použití zásad správy informací do modelu. Pouze položky v katalogu souboru se použije ve fázi školení. Všechny další položky budou ignorovat.

## <a name="how-much-data-do-you-need"></a>Množství zpracovávaných dat je potřeba?

Kvalita modelu je často závisí na kvalitu a množství dat.
Systém dozví, když uživatelé si různých položek (název tohoto spolupracovat výskytů). Pro FBT sestavení je také důležité vědět položek, které jsou zakoupené ve stejné transakce. 

Je dobré pravidlo mít většiny položek stačí účastnit 20 transakce zabere nejméně, takže pokud jste měli 10 000 položek ve vašem katalogu, doporučujeme, že máte alespoň 20 výskyty určeným počtem transakce nebo asi 200 000 transakce. Ještě jednou jde pravidlo. Je třeba experimentovat s daty.

Po vytvoření modelu můžete provádět [offline hodnocení](cognitive-services-recommendations-buildtypes.md) zjistit, jak je pravděpodobně provádět model služby.
