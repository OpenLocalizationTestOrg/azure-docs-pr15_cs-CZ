<properties
    pageTitle="Začínáme s Azure prohledávat NodeJS | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Projděte si postup vytvoření aplikace Vyhledávací služby na hostovanou cloudu vyhledávací služby na Azure pomocí NodeJS jako programovací jazyk."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-nodejs"></a>Začínáme s Azure prohledávat NodeJS
> [AZURE.SELECTOR]
- [Portál](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Zjistěte, jak vytvářet vlastní NodeJS vyhledávání aplikace, která používá Azure vyhledejte jeho vyhledávání. Tento kurz používá [Azure hledání služba REST API](https://msdn.microsoft.com/library/dn798935.aspx) pro vytvoření objektů a činnosti použitý v tomto cvičení.

Jsme použili [NodeJS](https://nodejs.org) a NPM, [Sublime Text 3](http://www.sublimetext.com/3)a prostředí Windows PowerShell ve Windows 8.1 vyvíjet a otestovat tento kód.

V tomto příkladu spustíte musí mít Azure vyhledávací služby, které se můžete přihlásit k [Portálu Azure](https://portal.azure.com). Podrobné pokyny najdete v článku [Vytvoření Azure vyhledávací služby na portálu](search-create-service-portal.md) .

## <a name="about-the-data"></a>Informace o data

Tato ukázková aplikace použije data z [USA geologických služby (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtrované na stav Rhode Island zmenšit velikost datovou sadu. Tyto údaje použijeme k vytvoření aplikace Vyhledávací služby, který vrací orientačních bodů budovy například nemocnice a školy, jakož i geologických funkcí, jako je datových proudů jezera a schůzek.

V této aplikaci **DataIndexer** program vytvoří a načte index pomocí [indexování](https://msdn.microsoft.com/library/azure/dn798918.aspx) konstrukce, načítání filtrované datovou sadu USGS z veřejné databáze SQL Azure. Přihlašovací údaje a připojení ke zdroji dat online je údaje ve programového kódu. Stačí žádnou další konfiguraci.

> [AZURE.NOTE] Společnost Microsoft použije filtru v této sadě dat dodržovat předpisy ve skupinovém rámečku limit 10 000 dokumentů bezplatné ceny osy. Pokud používáte standardní osy, toto omezení, nebudou mít vliv. Podrobnosti o kapacitu pro každou ceny osy najdete v tématu [limity hledání služby](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Najděte název služby a rozhraní api klávesami Azure vyhledávací služby

Po vytvoření službu, vraťte se do portálu pro získání adresy URL nebo `api-key`. Připojení ke službě hledání nutné, abyste měli adresu URL a `api-key` ověření hovor.

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Na panelu odkaz klikněte na možnost **Vyhledávací služby serveru** zobrazíte seznam všech služby Azure hledání zřízení pro vaše předplatné.
3. Vyberte službu, kterou chcete použít.
4. Na řídicím panelu služby uvidíte dlaždice základní informace i na ikonu klíče pro přístup ke klíči správce.

    ![][3]

5. Zkopírujte adresu URL služby, správce klíč a klíč dotazu. Musíte mít všichni tři později, když je přidáte do souboru config.js.

## <a name="download-the-sample-files"></a>Stažení ukázkových souborů

Některá z následujících postupů použijte ke stažení vzorku.

1. Přejděte na [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Klikněte na tlačítko **Stáhnout ZIP**, uložte soubor ZIP a extrahujte všechny soubory, které obsahuje.

Před soubory v této složce budou všechny změny následné souboru a spuštění příkazy.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Aktualizujte config.js. pomocí adresy URL služby hledání a rozhraní api klíč

Použití adresy URL a rozhraní api tlačítky, kterou jste si zkopírovali, zadejte adresu URL, správce a dotaz klíče v souboru konfigurace.

Správu klíčů udělit oprávnění k úplnému řízení služby operace, včetně vytváření nebo odstraňování rejstřík a načítání dokumenty. Naopak dotazu se operace jen pro čtení, obvykle používají v klientských aplikacích, která se připojují k prohledávání Azure.

V tomto příkladu jsme zahrnout klíč dotazu pomoct, jak posílit doporučený postup použití klíč dotazu v klientských aplikacích.

Následující snímek obrazovky zobrazující **config.js** otevřený v textovém editoru s příslušné položky vymezené tak, že můžete zjistit, kde k aktualizaci souboru s hodnotami, které jsou platné pro vyhledávací službu.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>Hostitelské prostředí runtime pro výběru

Vzorku vyžaduje serveru HTTP, které si můžete nainstalovat globálně pomocí npm.

Pomocí prostředí PowerShell okna následující příkazy.

1. Přejděte do složky obsahující soubor **package.json** .
2. Typ `npm install`.
2. Typ `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>Vytvoříte rejstřík a spuštění aplikace

1. Typ `npm run indexDocuments`.
2. Typ `npm run build`.
3. Typ `npm run start_server`.
4. Přímo v prohlížeči`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>Hledání dat USGS

Uvedenou množinu dat USGS obsahuje záznamy, které jsou důležité pro stát Rhode Island. Pokud kliknete na tlačítko **Hledat** na prázdný vyhledávací pole, zobrazí se vám horní 50 položek, což je výchozí.

Zadání hledaný výraz vám umožní vyhledávací něco přejít. Zadejte název místní. "Roger čáslavka" byla první guvernér Rhode Island. Po mu jsou s názvem spoustu parky budovy a školy.

![][9]

Může taky zkusit některou z těchto podmínek:

- Pawtucket
- Pembroke
- husí + standardní


## <a name="next-steps"></a>Další kroky

Toto je první kurz Azure hledání na základě NodeJS a USGS datovou sadu. V průběhu času rozšíříme kurzu, který ukazují další vyhledávací funkce, které můžete chtít použít ve vlastní řešení.

Pokud máte nějaké základní v Azure hledání, můžete v tomto příkladu springboard vyzkoušeli suggesters (automatickým dokončováním nebo funkce Automatické dokončování dotazů), filtry a působí navigace. Na stránce výsledků hledání můžete také zvýšit přidáním počty a dávky dokumentů tak, aby uživatelé můžou procházení výsledků.

Začínáte s Azure hledání? Doporučujeme vyzkoušení jiných výukové programy pro se dají pochopení co můžete vytvořit. Navštivte naše [si přečtěte následující dokumentaci stránky](https://azure.microsoft.com/documentation/services/search/) na Najít další zdroje informací. Můžete také zobrazit odkazy na naše [videa a výuková seznamu](search-video-demo-tutorial-list.md) zobrazíte další informace.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png
