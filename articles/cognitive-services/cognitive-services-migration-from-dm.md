
<properties
    pageTitle="Migrace do služby Azure kognitivní doporučení rozhraní API ze služby DataMarket doporučení rozhraní API | Microsoft Azure"
    description="Výukové doporučení – migrace doporučení kognitivní služby Azure počítače"
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
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Migrace do služby Azure kognitivní doporučení rozhraní API ze služby DataMarket doporučení rozhraní API
V tomto článku se dozvíte, jak migrovat z rozhraní [API doporučení Microsoft DataMarket](https://datamarket.azure.com/dataset/amla/recommendations) [Microsoft Azure kognitivní doporučení rozhraní API služeb](https://www.microsoft.com/cognitive-services/en-us/recommendations-api).

Rozhraní API doporučení DataMarket přestane přijímat noví zákazníci 31 prosinec 2016 nebo budou změněny 28 únor 2017.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Jak začít používat Azure kognitivní doporučení rozhraní API služeb?

Migrace do kognitivní rozhraní API služeb doporučení, postupujte takto:

1.  Pokud ještě nemáte předplatné Azure [zaregistrovat](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) za jiný. 

1.  Pokud potřebujete podrobné pokyny od [– Úvodní příručka](cognitive-services-recommendations-quick-start.md) k registraci kognitivní rozhraní API služeb doporučení a používat programově. 

1.  Přejděte [kognitivní rozhraní API služeb doporučení úvodní stránka](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) do obsahuje informace o službě a o si přečtěte následující dokumentaci.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Můžu v uživatelském rozhraní doporučení použita k vytvoření Moje modely. Existuje podobného nástroje pro kognitivní doporučení API služeb?

Absolutní! Adresa URL pro nové [Uživatelské rozhraní doporučení](http://recommendations-portal.azurewebsites.net/) je http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] Vaše přihlašovací údaje DataMarket nefungují tady. Registrace k předplatného na portálu Azure a najděte klíč účtu, třeba na nové [Uživatelské rozhraní doporučení](http://recommendations-portal.azurewebsites.net/).
Pokud nemáte klíč účtu, přečtěte si článek úkolu 1 [– Úvodní příručka](cognitive-services-recommendations-quick-start.md).

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>Je v novém formátu rozhraní API stejné jako rozhraní API doporučení služby DataMarket

Rozhraní API podporuje stejné scénářů a pracovních postupů jako scénářů DataMarket verze podporuje, ale skutečné rozhraní API byl aktualizovaný souladu s [Microsoft REST API pokyny](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). Rozhraní API jsou jednotnější a práce lépe s nástrojů, které podporují Swagger.

Vysvětlení jednotlivých rozhraní API, najdete v tématech [rozhraní API aplikace explorer](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db).
Použití, *zkuste* ho tlačítko zkušební hovor rozhraní API. Formát souborů spotřebované množství rozhraním API doporučení (katalogu a použití souborů) nezměnil, můžete dál používat stejnou soubory nebo infrastruktury jste vytvořili pro vytvoření těchto souborů.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Co patří mezi nové funkce v kognitivní doporučení API služeb?

Za posledních dvou měsíců byl vydán nové funkce pro kognitivní rozhraní API služeb doporučení:
-   Doporučení uživatelského rozhraní pro školení a testování modely bez nutnosti napsat jedním řádkem kódu
-   Dávkové bodování k poskytování tisíce doporučení v celém dokumentu
-   Vytvoření metriky podpory dotazu kvalitu modely doporučení
-   Podpora pro obchodního pravidla
-   Umožňuje vytvořit výčet a stahovat soubory použití a katalogu
-   Hodnocení sestavení podpory k vytvoření dotazu kvalitu funkce položky v modelu doporučení
-   Přidané možnost vyhledat produktu v katalogu

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Kdy Microsoft zastavit podpůrné rozhraní API doporučení DataMarket?

[Rozhraní API doporučení na DataMarket](https://datamarket.azure.com/dataset/amla/recommendations) přestane přijímat noví zákazníci po 31 prosinec 2016 a budou úplně změněny tak, že 28 únor 2017. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Co když nemám data, která potřeba vytvořit Moje modely v kognitivní doporučení API služeb?

Chceme pro snadnou přechod co nejvíce podporovat za vás. Kde vám radíme přesunout starý modely ze svého účtu služby DataMarket do nového předplatného Azure kognitivní rozhraní API doporučení služeb. Kontaktujte nás v [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Nemůžeme vás vyzve k poskytnutí DataMarket ID předplatného a vaše ID předplatného kognitivní služby před jsme modely přidružit k vašemu novému účtu.
