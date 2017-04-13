<properties 
    pageTitle="Vložení MPEG-POMLČKU adaptivní streamování videa v aplikaci HTML5 s DASH.js | Microsoft Azure" 
    description="Toto téma ukazuje, jak vložení videa MPEG-POMLČKU adaptivní datových proudů v HTML5 aplikaci DASH.js." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Vložení MPEG-POMLČKU adaptivní streamování videa v aplikaci HTML5 s DASH.js

##<a name="overview"></a>Základní informace

MPEG-POMLČKU je standard ISO pro adaptivní streamování videa obsahu, který nabízí spoustu značných výhod pro uživatele, kteří chtějí k poskytování videa vysokou kvalitou, adaptivní streamování výstupu. Pomlčkou MPEG datového proudu videa se automaticky přetažením definici nižší případě změní přetížení sítě. Sníží pravděpodobnost prohlížeč zobrazují "pozastavené" videa během přehrávač stáhne další několik sekund, než přehrát (označovaná taky jako vyrovnávací paměť). Jak snižuje přetížení sítě přehrávače videa zase vrátit vyšší toku kvalitu. Tato možnost přizpůsobit šířku pásma požadovanou také výsledkem rychlejší čas zahájení videa. To znamená několik sekund, než první můžete přehrávat v segment nižší kvality rychlé stáhnout a pak byla paměti krok až vyšší jednou dostatečný obsah kvalitu.

Dash.js je otevřít zdroj MPEG-POMLČKU přehrávače videa napsané v JavaScriptu. Jeho cílem je poskytovat přehrávač robustní, různé platformy, který lze volně opakovaně použít v aplikacích, které vyžadují přehrávání videa. Poskytuje MPEG-POMLČKU přehrávání v jakémkoliv prohlížeči, který podporuje W3C médií zdroj přípony (MSE), dnes, která je Chrome, Microsoft Edge a IE11 (jiných prohlížečích uvedli jejich záměr pro podporu MSE). Další informace o DASH.js js najdete v článku úložiště dash.js GitHub.


##<a name="creating-a-browser-based-streaming-video-player"></a>Vytváření prohlížečový streamování přehrávač videa

Vytvoření jednoduchého webovou stránku, která zobrazí přehrávače videa s očekávané ovládacích prvků takové přehrát, pozastavit, převinutí zpět atd., budete muset:

1. Vytvoření stránky HTML
1. Přidání videa značky
1. Přidání přehrávačem dash.js
1. Inicializace přehrávač
1. Přidání určitého stylu šablon stylů CSS
1. Výsledky zobrazit v prohlížeči implementující MSE

Inicializace přehrávač můžete vyplnit jenom hrstku řádky kód v JavaScriptu. Pomocí dash.js, ve skutečnosti je to snadné vložení videa MPEG-přerušované čáry v aplikacích prohlížeče na základě.

##<a name="creating-the-html-page"></a>Vytvoření stránky HTML

Cílem prvního kroku je vytvořit stránku standardní HTML obsahující požadovaný **grafický** prvek uložit tento soubor jako basicPlayer.html, jak ukazuje tento příklad:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##<a name="adding-the-dashjs-player"></a>Přidání přehrávačem DASH.js

Pokud chcete přidat odkaz implementaci dash.js aplikace, musíte vzít dash.all.js soubor z verze 1.0 dash.js projektu. To je vhodné uložit ve složce JavaScript aplikace. Tento soubor je pohodlný soubor, který použije všechny potřebné dash.js kód do jednoho souboru. Pokud jste si kolem dash.js úložiště, bude najít jednotlivé soubory, testování kód a různých dalších věcech, pokud všechny chcete udělat, ale je použití dash.js a pak je soubor dash.all.js co byste měli.

Přehrávač dash.js chcete aplikací, přidáte značku skriptu na hlavní část basicPlayer.html:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Dále vytvořte funkci inicializace přehrávač při načtení stránky. Přidejte následující skript po řádku, ve kterém načtete dash.all.js:

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Tato funkce nejprve vytvoří DashContext. Slouží ke konfiguraci aplikace pro konkrétní runtime prostředí. Z technické hlediska definuje tříd, které framework vkládání závislost použijte při vytváření aplikace. Ve většině případů ale použijete Dash.di.DashContext.

Pak instance primárního třídy dash.js framework MediaPlayer. Tato třída obsahuje základní metody, jako třeba Přehrát pozastavit, spravuje relace videa elementem a také spravuje výklad souboru médií prezentace popis (MPD), který popisuje video přehrát.

Funkce startup() třídy MediaPlayer nazývá a ujistěte se, že je přehrávač připravené k přehrávání videa. Mimo jiné tuto funkci zaručuje, že všechny potřebné třídy (podle kontextu) vloženy. Po přehrávač je připravená, můžete grafický prvek připojte k němu pomocí funkce attachView(). Díky MediaPlayer vloží datového proudu videa do prvku a také ovládat přehrávání podle potřeby.

Předáte adresu URL souboru MPD MediaPlayer tak, že ví o videu, že je očekáván přehrát. Funkce setupVideo() vytvořili muset provést po plně načtení stránky. To udělejte pomocí události při načtení prvku body. Změna vašeho <body> element do:

    <body onload="setupVideo()">

Nakonec nastavte velikost videa prvek pomocí šablon stylů CSS. V prostředí adaptivní streamování totiž zejména důležité velikost videa přehrávaného měnit v závislosti na přehrávání přizpůsobí změnit stav sítě. Tento jednoduchý ukázku jednoduše platných videa prvek být 80 % okna dostupné prohlížeče přidáním následující šablony stylů CSS na hlavní část stránky:
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##<a name="playing-a-video"></a>Přehrávání videa

Přehrát video, přejděte v prohlížeči basicPlayback.html souboru a klikněte na přehrát na přehrávače videa zobrazí.


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Viz taky

[Můžete vyvíjet aplikace přehrávače videa](media-services-develop-video-players.md)

[GitHub dash.js úložiště](https://github.com/Dash-Industry-Forum/dash.js) 
