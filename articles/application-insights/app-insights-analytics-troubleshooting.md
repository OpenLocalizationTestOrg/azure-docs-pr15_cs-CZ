<properties 
    pageTitle="Poradce při potížích s analýzy – nástroj výkonné vyhledávání aplikace přehledy | Microsoft Azure" 
    description="Máte problémy s aplikací přehledy analýzy? Začněte tady. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/11/2016" 
    ms.author="awills"/>


# <a name="troubleshoot-analytics-in-application-insights"></a>Poradce při potížích s analýzy v aplikaci přehledy


Máte problémy s [aplikací přehledy analýzy](app-insights-analytics.md)? Začněte tady. Technologie pro analýzu je nástroj výkonné hledání přehledy aplikace Visual Studio.



## <a name="limits"></a>Omezení

* V současné době jsou omezené těsně nad týden minulých dat výsledků dotazu.
* Testování v prohlížečích: nejnovější verze chromu, okraje a Internet Explorer.


## <a name="known-incompatible-browser-extensions"></a>Rozšíření známé kompatibilní prohlížeče

* Ghostery

Zakázání rozšíření nebo použijte jiný prohlížeč.


##<a name="e-a"></a>"Neočekávané chybě"

![Neočekávané chybě obrazovky](./media/app-insights-analytics-troubleshooting/010.png)

Při portálu runtime – neošetřené výjimce došlo k interní chybě.

* Vymazat mezipaměť prohlížeče. 

## <a name="e-b"></a>403... Zkuste znovu načíst

![403... Zkuste znovu načíst](./media/app-insights-analytics-troubleshooting/020.png)

Ověřování souvisejících (během ověřování nebo access token generování) došlo k chybě. Na portálu pravděpodobně způsob, jak obnovit beze změny nastavení prohlížeče.

* Ověřte [Povolit cookies třetích stran](#cookies) v prohlížeči. 


## <a name="authentication"></a>403... ověření zóny zabezpečení

![403.. .verify zóny zabezpečení](./media/app-insights-analytics-troubleshooting/030.png)

Ověřování souvisejících (během ověřování nebo access token generování) došlo k chybě. Na portálu pravděpodobně způsob, jak obnovit beze změny nastavení prohlížeče.

1. Ověřte [Povolit cookies třetích stran](#cookies) v prohlížeči. 

2. Použijete Oblíbené položky, záložku nebo uložený odkaz k otevření portálu technologie pro analýzu? Jste přihlášení pomocí jiného pověření, než se vyvolají jste uložili na odkaz?

2. Zkuste použít v soukromé nebo okno prohlížeče okně (po zavření všechna okna). Budete muset zadat svoje přihlašovací údaje. 

2. Otevřete jiné okno (obyčejný) prohlížeče a přejděte na [Azure](https://portal.azure.com). Odhlaste se. Otevřete odkaz a přihlaste se pomocí správné přihlašovací údaje.

2. Okraje a Internet Explorer uživatelé dostali k této chybě, když důvěryhodné zóny nastavení nejsou podporovaná.

    Ověřte, jestli [portál technologie pro analýzu](https://analytics.applicationinsights.io) a [Azure Active Directory portál](https://portal.azure.com) jsou ve stejném zóny zabezpečení:

 * V Internet Exploreru otevřete **Možnosti Internetu** **zabezpečení**, **Důvěryhodné weby**, **weby**:

    ![Dialogové okno Možnosti Internetu, přidání webu do důvěryhodné weby](./media/app-insights-analytics-troubleshooting/033.png)

    V seznamu webů Pokud některý z následujících adres URL jsou však započítávány, zkontrolujte, že ostatní jsou však započítávány také:

    https://Analytics.applicationinsights.IO<br/>
   https://Login.microsoftonline.com<br/>
   https://Login.Windows.NET


## <a name="e-d"></a>404 … Zdroje nenašlo se.

![404 … zdroje nenašlo se.](./media/app-insights-analytics-troubleshooting/040.png)

Prostředek aplikace odstranila přehledy aplikace a už není dostupná. Tomu může dojít, pokud jste soubor uložili adresu URL na stránku analýzy.


## <a name="e-e"></a>403 … Povolení

![403... neoprávněné](./media/app-insights-analytics-troubleshooting/050.png)

Nemáte oprávnění k otevření aplikace v analýzy.

* Jste získali od někoho jiného na odkaz? Požádejte je, abyste měli jistotu, že se nacházíte v [čteček nebo přispěvatelů pro tuto skupinu zdrojů](app-insights-resources-roles-access-control.md).
* Jste uložili na odkaz pomocí různých přihlašovacích údajů? Otevřete [Azure portálu](https://portal.azure.com)odhlásit a pak to zkuste tento odkaz znovu poskytující správné přihlašovací údaje.

## <a name="html-storage"></a>403... Úložiště HTML5

Náš portálem používá HTML5 localStorage a sessionStorage.

* Chrome: Nastavení ochrany osobních údajů, nastavení obsahu.
* Internet Explorer: Možnosti Internetu, Upřesnit, zabezpečení, povolit úložiště DOM.


![403 … pokusíte povolit úložiště HTML5](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404 … Předplatné nenašlo se.


![404... Předplatné nenašlo se.](./media/app-insights-analytics-troubleshooting/070.png)

Adresa URL je neplatný. 

* Otevřete aplikaci zdroje [Aplikace přehledy](https://portal.azure.com)portálu. Pak můžete pomocí tlačítka analýzy.

## <a name="e-h"></a>404 … neexistuje stránky

![404... Stránka neexistuje](./media/app-insights-analytics-troubleshooting/080.png)

Adresa URL je neplatný.

* Otevřete aplikaci zdroje [Aplikace přehledy](https://portal.azure.com)portálu. Pak můžete pomocí tlačítka analýzy.

## <a name="cookies"></a>Povolit cookies třetích stran

  Přečtěte si, [jak zakázat cookies třetích stran](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), ale Všimněte si, že je třeba **Povolit** je.

## <a name="e-x"></a>Pokud všechno ostatní selže    

[Kontaktujte nás](app-insights-get-dev-support.md).
 
[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

