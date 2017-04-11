<properties
    pageTitle="Připojení k webové služby strojového výukové | Microsoft Azure"
    description="C# nebo Python připojte k Azure počítače výukové webové služby pomocí klávesy se tak mohli ověřovat."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016" 
    ms.author="garye" />


# <a name="connect-to-an-azure-machine-learning-web-service"></a>Připojení k webové služby Azure počítače výuka

Prostředí developer Azure počítače výukové je rozhraní API webových služeb aby předpovědí ze vstupní data v reálném čase nebo v režimu dávku. Azure počítače výukové Studio umožňuje vytvářet předpovědí a nasazení Azure počítače výukové webové služby.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Další informace o tom, jak vytvářet a publikovat počítače výukové webové služby pomocí počítače výukové Studio:

- Návod k vytvoření pokus ve počítače výukové studiu najdete v článku [vytvoření první testu](machine-learning-create-experiment.md).
- Podrobnosti o tom, jak nasazení webové služby najdete v tématu [nasazení počítače výukové webové služby](machine-learning-publish-a-machine-learning-web-service.md).
- Další informace o počítači výukové obecně, navštivte [Výukové centrum si přečtěte následující dokumentaci pro počítače](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Azure počítače výukové webové služby ##

Se službou Azure počítače výukové webové aplikace externí informuje uživatele o s počítače výukové pracovního postupu bodování modelu můžete v reálném čase. Volání počítače výukové webové služby vrátí výsledky předpovědí externí aplikaci. Chcete-li volání počítače výukové webové služby, předáte rozhraní API klíč, který se vytvoří při nasazení předpověď. Počítač výukové webová služba vychází z REST, výběr oblíbených architektura webu programování projekty.

Azure výukové počítač má dva typy služeb:

- Odpovědi na žádosti o službu (RR) – zhoršeným latence, vysoce scalable služba, která poskytuje rozhraní pro příslušnosti modely vytvořené a nasazeny z Studio výukové počítače.
- Dávkové spuštění služby (BES) – asynchronní služby, které skóre a dávka pro záznamy.

Další informace o počítači výukové webové služby najdete v článku [nasazení počítače výukové webové služby](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Získání autorizovaný klíč výukové počítače Azure ##

Při nasazení vaší experiment vytvářejí rozhraní API klíče webové služby. Načtení klíčů z několika umístění.

## <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Z portálu Microsoft Azure počítače výukové webové služby

Přihlaste se k portálu [Microsoft Azure počítače výukové webové služby](https://services.azureml.net) .

K načtení klávesu rozhraní API pro nový počítač výukové webové služby:

1. Na portálu Azure počítače výukové webové služby klikněte na **Webové služby** na začátek nabídku.
2. Klikněte na webové služby, u kterého chcete načíst klávesu.
3. V nabídce horní klikněte na **spotřebě**.
4. Kopírování a uložte **Primární klíč**.


K načtení klávesu rozhraní API pro klasické počítače výukové webové služby:

1. Na portálu Azure počítače výukové webové služby klikněte na **Klasické webovým službám** nabídky nejvyšší.
2. Klikněte na webová služba, se kterým pracujete.
3. Klikněte na koncový bod, u kterého chcete načíst klávesu.
3. V nabídce horní klikněte na **spotřebě**.
4. Kopírování a uložte **Primární klíč**.

## <a name="classic-web-service"></a>Klasický webové služby ##

 Můžete taky načtete klíč pro klasické webové služby z počítače výukové Studio nebo portálu Azure.

### <a name="machine-learning-studio"></a>Počítač výukové Studio ###

1. Ve počítače výukové studiu klikněte na levé straně **Webové služby** .
2. Klikněte na webové služby. **Rozhraní API klíč** je na kartě **řídicího PANELU** .

### <a name="azure-portal"></a>Azure portálu ###

1. Klikněte na **Počítač VÝUKOVÉ** na levé straně.
2. Klikněte na pracovní prostor, ve kterém se nachází webové služby.
3. Klikněte na **WEB služby**.
4. Klikněte na webové služby.
5. Klikněte na koncový bod. "Rozhraní API klíč" je dolů v pravém dolním rohu.

## <a id="connect"></a>Připojení k počítači výukové webové služby

Můžete připojit k počítači výukové webové služby pomocí programovací jazyk, který podporuje protokol HTTP žádostí a odpovědí. Příklady v jazyce C# Python a R uvidí z počítače výukové webové služby nápovědy stránky.

**Rozhraní API výukové počítače Nápověda** Počítač rozhraní API výukové nápovědy se vytvoří při nasazení webové služby. V tématu [Naučná Azure počítače návodu – nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md).
Nápověda k počítači výukové rozhraní API aplikace obsahuje podrobnosti o předpovídání pomocí webové služby.

2. Klikněte na webová služba, se kterým pracujete.
3. Klikněte na koncový bod, u kterého chcete zobrazit na stránce nápovědy rozhraní API.
3. V nabídce horní klikněte na **spotřebě**.
3. Klepněte na **stránku nápovědy rozhraní API** odpovědi na žádosti o nebo koncové body spuštění dávky.

**Do zobrazení pro počítače výukové API Nápověda pro nové webové služby**

V Azure počítač výukové Web služby portálu:

1. **Webové služby** v nabídce klikněte na horní.
2. Klikněte na webové služby, u kterého chcete načíst klávesu.

Klikněte na **spotřebě** zobrazíte URI pro žádost o Reposonse a spuštění služeb dávku ukázkový kód v jazyce C# R a Python.

Klikněte na tlačítko **Swagger rozhraní API** získat Swagger na základě si přečtěte následující dokumentaci pro rozhraní API s názvem z předaném URI.

### <a name="c-sample"></a>Ukázka C# ###

Připojení k počítači výukové webové služby, použijte **HttpClient** předávání ScoreData. ScoreData obsahuje FeatureVector n dimenzionální vektorové číselných funkce, které představuje ScoreData. Ověření pro službu strojového výukové klíčem rozhraní API.

Připojení k počítači výukové webové služby, musí být nainstalovaný balíček NuGet **Microsoft.AspNet.WebApi.Client** .

**Instalace Microsoft.AspNet.WebApi.Client NuGet ve Visual Studiu**

1. Publikování datovou sadu stažení z UC: dospělého 2 třídy sadu webové služby.
2. Klikněte na **Nástroje** > **Správce NuGet balíčků** > **Konzoly balíčku správce**.
2. Zvolte **Microsoft.AspNet.WebApi.Client instalační balíček**.

**Spuštění ukázka kódu**

1. Publikování "Příklad 1: stahování datovou sadu UC: datovou sadu třídy dospělý 2" testu, část kolekce ukázkové výukové počítače.
2. Přiřadíte apiKey s klávesou z webové služby. V tématu **získání autorizovaný klíč Azure počítače výukové** výše.
3. Přiřadíte serviceUri s URI požádat o.


### <a name="python-sample"></a>Ukázka Python ###

Připojení k počítači výukové webové služby, použijte knihovnu **urllib2** předávání ScoreData. ScoreData obsahuje FeatureVector n dimenzionální vektorové číselných funkce, které představuje ScoreData. Ověření pro službu strojového výukové klíčem rozhraní API.


**Spuštění ukázka kódu**

1. Nasazení "Příklad 1: Stáhněte si sadu z UC: datovou sadu třídy dospělý 2" testu, část kolekce ukázkové počítače výukové.
2. Přiřadíte apiKey s klávesou z webové služby. Naleznete v části **získat autorizovaný klíč Azure počítače výukové** poblíž začátku tohoto článku.
3. Přiřadíte serviceUri s URI požádat o.
