<properties 
    pageTitle="Společná operace v rozhraní API doporučení výukové počítače | Microsoft Azure" 
    description="Azure ML doporučení ukázková aplikace" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 


# <a name="recommendations-api-sample-application-walkthrough"></a>Doporučení ukázkové rozhraní API aplikace návod

>[AZURE.NOTE]Měli byste začít používat službu kognitivní rozhraní API doporučení místo tato verze. Kognitivní službu doporučení se nahradí tuto službu a s novými funkcemi bude vyvinuté tam. Má nové funkce jako dávky podpory lepší rozhraní API aplikace Explorer, opět dostanete čistší prostředí povrchový, jednotnější registrace/fakturace rozhraní API, atd.
> Další informace o [migraci do nové kognitivní služby](http://aka.ms/recomigrate)

##<a name="purpose"></a>Účel

Tento dokument zobrazuje použití API Azure počítače výukové doporučení prostřednictvím [Ukázková aplikace](https://code.msdn.microsoft.com/Recommendations-144df403).

Tato aplikace není určená k zahrnout plnou funkčnost ani nepoužívá rozhraní API. Ukazuje společná operace, které se mají provést, když chcete nejdřív přehrát s služba strojového výukové. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="introduction-to-machine-learning-recommendation-service"></a>Úvod do služby třídění výukové počítače

Doporučení prostřednictvím služba strojového výukové jsou povoleny při vytváření doporučení model založený na následující data:

* Úložiště položky, kterou chcete doporučit označovaná taky jako katalog
* Data, která představují použití položek pro uživatele nebo relace (to můžete získali v čase prostřednictvím získávání dat, ne jako součást aplikace ukázka)

Po modelu doporučení, můžete ji použijete k předpovídání položky, které uživatel může být zájem, podle předpisů rozhraní sady položek (nebo jedné položky) uživatel vybere.

Do předchozí scénář povolení, udělejte tohle ve počítače výukové služba:

* Vytvoření modelu: Toto je logické kontejner, který obsahuje data (katalogu a použití) a prediktivní vkládání modely. Každý kontejner modelu je určen prostřednictvím jedinečné ID, které je přidělený při vytvoření. Toto ID je označená jako ID modelu a pracovní postup slouží většinou rozhraní API. 
* Nahrát do katalogu: vytvoření kontejneru modelu můžete přiřadit k němu katalogu.

**Poznámka**: vytvoření modelu a nahrávání ke katalogu obvykle provádí jednou pro životním cyklu modelu.

* Nahrání použití: kontejneru modelu se přidá použití zásad správy informací.
* Vytvoření modelu doporučení: když máte dost data, je možné vytvářet modelu doporučení. Tuto operaci používá horní algoritmy výukové počítače k vytvoření modelu doporučení. Každý tvůrce dotazů je přidružená k jedinečné ID. Potřebujete poznamenejte Identifikátor vzhledem k tomu, že je nutné pro fungování některá rozhraní API.
* Sledování proces vytváření: sestavení modelu doporučení je asynchronní operace a může trvat několik minut několik hodin, v závislosti na počtu dat (katalogu a použití) a parametrech Tvůrce dotazů. Proto musíte sledovat sestavení. Doporučení modelu se vytvoří, jenom v případě jeho přidružený sestavení úspěšném dokončení.
* (Volitelné) Zvolte vytvořit modelu aktivní doporučení: Tento krok je nezbytné pouze pokud máte víc než jeden doporučení modelu integrované v kontejneru modelu. Jakékoliv žádosti o získat doporučení bez označující modelu aktivní doporučení přesměruje systému do aktivního sestavení výchozí automaticky. 

**Poznámka**: aktivní doporučení modelu je výrobní připravená a vytvořené pro pracovní zátěž výroby. Tím se liší od modelu neaktivní doporučení, které zůstává v prostředí test profesionálové (někdy se jí říká pracovní).

* Získat doporučení: až budete mít modelu doporučení, můžete aktivovat doporučení pro jednu položku nebo seznam položek, které vyberete. 

Bude obvykle vyvoláte získat doporučení pro určitou dobu. Během tohoto časového období můžete přesměrovat použití zásad správy informací k systému doporučení výukové počítače, čímž tato data do kontejneru zadaný model. Pokud máte dost použití zásad správy informací, můžete vytvořit nový model doporučení, která zahrnuje další využití. 

##<a name="prerequisites"></a>Zjistit předpoklady pro

* Visual Studio 2013
* Přístup k Internetu 
* Předplatné doporučení rozhraní API (https://datamarket.azure.com/dataset/amla/recommendations).

##<a name="azure-machine-learning-sample-app-solution"></a>Azure řešení aplikace ukázkové výukové počítače

Toto řešení obsahuje zdrojového kódu, příklad použití, soubor katalogu a směrnice ke stažení balíčky, které jsou potřeba pro kompilaci.

##<a name="the-apis-used"></a>Rozhraní API používá

Aplikace používá funkci doporučení výukové počítače prostřednictvím podmnožinu dostupných rozhraní API. Následující rozhraní API je znázorněn v aplikaci:

* Vytvoření modelu: vytvoření kontejneru logické pro uložení dat a doporučení modely. Model je určen název a nelze vytvořit víc než jeden model se stejným názvem.
* Odeslat soubor katalogu: k nahrání katalogu dat používat.
* Nahrání souboru použití: použijte k nahrání použití zásad správy informací.
* Aktivace sestavení: slouží k vytvoření modelu doporučení.
* Sledování spuštění Tvůrce dotazů: umožňuje sledovat stav sestavení modelu doporučení.
* Volba vytvořená modelu doporučení: slouží k označení který model doporučení používat jako výchozí pro určité modelu kontejner. Tento krok je nutný jenom v případě, že máte víc než jeden doporučení model a chcete aktivovat neaktivní sestavení jako aktivní doporučení model.
* Získání doporučení: slouží k načtení Doporučené položky podle zadaného jedné položky nebo několika položek. 

Detailní popis rozhraní API najdete v dokumentaci k webu Microsoft Azure Marketplace. 

**Poznámka**: modelu nemůžou mít několik buildy určitou dobu (ne současně). Každý buildu se vytvoří pomocí stejné nebo aktualizované katalogu a další použití zásad správy informací.

## <a name="common-pitfalls"></a>Běžné nástrahy

* Je třeba zadat svoje uživatelské jméno a svůj klíč účtu primární webu Microsoft Azure Marketplace spustit aplikaci vzorku.
* Spuštění aplikace ukázkové postupně se nezdaří. Toku aplikace zahrnuje vytváření odesílání, vytváření monitor a získání doporučení z předdefinovaných modelu; Proto se nezdaří na po sobě jdoucí spuštění Pokud nezměníte název modelu mezi vyvolání.
* Doporučení může vrátit bez data. Ukázka aplikace používá příliš malý katalogu a použití soubor. Některé položky z katalogu proto bude mít žádné doporučené položky.

## <a name="disclaimer"></a>Zřeknutí se záruk
Ukázka aplikace není určená k probíhat v pracovním prostředí. K dispozici v katalogu dat je příliš malý a nebude zajišťovat modelu smysluplné doporučení. Data slouží jako ukázka. 
 
