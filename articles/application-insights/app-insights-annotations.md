<properties
    pageTitle="Uvolněte poznámky pro přehledy aplikace | Microsoft Azure"
    description="Přidání nasazení nebo vytvořit značky a metriky explorer grafů v aplikaci přehledy."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="awills"/>

# <a name="release-annotations-in-application-insights"></a>Uvolněte poznámky v aplikaci přehledy

Uvolněte poznámky na místo, kam jste nainstalovali nové sestavení [Metriky Explorer](app-insights-metrics-explorer.md) graf zobrazuje. Je to snadný způsob podívejte, jestli vaše změny obsahoval žádný vliv na výkon aplikace. Budou můžete automaticky vytvořit tak, že [Visual Studio týmovou vytvořit systém](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs)a můžete taky [vytvořit přímo z prostředí PowerShell](#create-annotations-from-powershell).

![Příklad poznámek s viditelné korelace s časem odezvy serveru](./media/app-insights-annotations/00.png)

Poznámky vydání jsou funkce sestavení cloudové a uvolněním služby Visual Studio týmovou. 

## <a name="install-the-annotations-extension-one-time"></a>Instalace koncovku poznámky (jednou)

Abyste mohli vytvořit vydání poznámek, musíte nainstalovat některý z mnoha rozšíření týmu služeb k dispozici ve Visual Studiu Marketplace.

1. Přihlaste se do [Aplikace Visual Studio týmovou](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projektu.
2. Ve Visual Studiu Marketplace [získat rozšíření vydání poznámky](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations)a přiřadit ji někomu účtu týmovou.

![AT top vpravo od týmu služeb webové stránky, otevřít Marketplace. Vyberte týmovou vizuální a pak v části Tvůrce dotazů a vydání, zvolte Další.](./media/app-insights-annotations/10.png)

Stačí udělat jednou pro váš účet aplikace Visual Studio týmovou. Nyní je možné konfigurovat vydání poznámky pro všechny projekty ve vašem účtu. 

## <a name="get-an-api-key-from-application-insights"></a>Získat rozhraní API klíč z aplikace přehledy

Musíte to udělat pro každé šablony vydání, který chcete vytvořit verzi poznámky.


1. Přihlaste se k [Portálu Microsoft Azure](https://portal.azure.com) a otevřete aplikaci přehledy prostředek, který sleduje aplikace. (Nebo [Vytvořte si ho teď](app-insights-overview.md), pokud jste tak dosud neučinili.)
2. Spusťte **Access rozhraní API**a udělat kopii **Id přehledy aplikace**.

    ![V portal.azure.com otevřete aplikaci přehledy zdroje a zvolte nastavení. Spusťte rozhraní API aplikace Access. Zkopírujte ID aplikace](./media/app-insights-annotations/20.png)

2. V samostatném okně prohlížeče otevřete (nebo vytvořte) vydání šablonu, která spravuje váš nasazení z Visual Studio týmovou. 

    Přidání úkolu a z nabídky vyberte úkol aplikace přehledy vydání poznámky.

    Vložte **Id aplikace** , kterou jste zkopírovali z zásuvné rozhraní API aplikace Access.

    ![Služby Visual Studio týmu otevřete vydání, vyberte definici vydání a klikněte na Upravit. Klikněte na Přidat úkol a vyberte aplikaci přehledy vydání poznámky. Vložte ID aplikace přehledy.](./media/app-insights-annotations/30.png)

3. Nastavte v poli **APIKey** proměnné `$(ApiKey)`.

4. Zpátky ve zásuvné přístup k rozhraní API vytvořte nový klíč rozhraní API a přijmout ji zkopírujte.

    ![V zásuvné přístup k rozhraní API v okně Azure klikněte na vytvořit klíč rozhraní API. Zadejte komentáře, zapsat poznámky a klikněte na Generovat klíče. Zkopírujte nový klíč.](./media/app-insights-annotations/40.png)

4. Otevřete kartu Konfigurace šablony vydání.

    Vytvoření proměnné definici `ApiKey`.

    Vložte kód rozhraní API proměnné definice ApiKey.

    ![V okně týmovou vyberte kartu Konfigurace a klikněte na Přidat proměnnou. Nastavení názvu ApiKey a na hodnotu, vložte klíč, který jste právě vygenerovali.](./media/app-insights-annotations/50.png)


5. Nakonec **Uložit** definici vydání.

## <a name="create-annotations-from-powershell"></a>Vytvoření poznámky z prostředí PowerShell

Poznámky můžete taky vytvořit z obrázku, kterou chcete (bez použití a Team System). 

Pokud potřebujete [skript Powershellu z GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

Používejte takto:

    .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }

Získání `applicationId` a `apiKey` z aplikace přehledy zdroje: Otevřít nastavení, přístup k rozhraní API a kopírovat ID aplikace. Potom klikněte na vytvořit klíč rozhraní API a zkopírujte klávesu. 

## <a name="release-annotations"></a>Uvolněte poznámky

Teď pokaždé, když použijete šablonu vydání pro nasazení nové verzi, poznámky odešle interpretace aplikace. Poznámky se zobrazí v grafech v Průzkumníku metriky.

Klikněte na libovolnou značku poznámky otevřete podrobné informace o verzi, včetně o synchronizaci adresářů zdroj ovládacího prvku větví, uvolněte definice prostředí a další.


![Klikněte na libovolnou značku poznámky vydání.](./media/app-insights-annotations/60.png)
