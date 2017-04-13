<properties 
    pageTitle="Azure RemoteApp – testování přístupnosti šířku pásma s některé běžné scénáře | Microsoft Azure"
    description="Zjistěte, jak asi obvyklé scénáře použití, které vám pomohou zjistit potřeb šířky pásma pro Azure RemoteApp."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp – testování přístupnosti šířku pásma s některé běžné scénáře

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Jak můžeme popisované v [Azure RemoteApp odhad využití šířky pásma sítě](remoteapp-bandwidth.md), testuje, nejlepší způsob, jak zjistit, co vliv Azure RemoteApp na vaší síti, je spuštění některé použití. Spusťte tyto testy nastavené časového období a měření pásma potřebného pro jednotlivé scénáře. Pokud máte možnost, můžete taky změřit paketu ztráty a síťové kolísání sítě pochopit vytvořené v prostředí konkrétní vzorků sítě.

    
Při vyhodnocování využití šířky pásma, mějte na paměti, že použití liší různí uživatelé ve vaší společnosti. Příklad textu čtení a zápis obvykle používat menší šířku pásma než uživatelé, které spolupracují s video. Nejlepších výsledků dosáhnete zkoumá potřeb uživatele a vytvořte kombinaci následujících scénářích, který nejlépe vystihuje uživatelů ve vaší společnosti. Mějte na paměti, [zkontrolujte faktory, které využití šířky pásma dopad a uživatelského prostředí](remoteapp-bandwidthexperience.md) –, která vám pomůže identifikovat ideální testů.

Nejdřív přečtěte si o testů, vyberte svůj kombinaci a znovu spusťte. Následující tabulka slouží ke sledování výkonu.

>[AZURE.NOTE] Pokud nemůžete dělat vlastní testování sítě nebo nemáte čas na to, přečtěte si náš [Základní síťové šířky pásma odhady/doporučení](remoteapp-bandwidthguidelines.md). Vaše vzdálenost se může lišit, ale, *můžete* spustit vlastní testy, měli byste.


## <a name="the-usage-tests"></a>Použití testů
Každého z těchto testů spuštění pro různé množství času a otestujte různé funkce a funkce, které používání šířka pásma. Nezapomeňte zvolte kombinaci test nejlépe odpovídá uživatelé jednotlivé společnosti.
 
### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>PowerPoint vedoucími/komplexní – spuštění 900 1000 sekund

Uživatel představuje orientace 45 až 50 vysokou věrností pomocí aplikace Microsoft Office PowerPoint v režimu celé obrazovky. Snímky smí obsahovat obrázky, přechody (s animací) a pozadí s přechodem barvy, které jsou typické pro vaši firmu. Uživatel by měl utratit za minimálně 20 s každým snímkem.
    
Tento scénář vytvoří shlukovým přenosem dat, když do snímku přechody na další snímek v prezentaci.
    
### <a name="simple-powerpoint---run-for-620-seconds"></a>Jednoduchý PowerPoint – spuštění ~ 620 sekund

Uživatel představuje simple Powerpointového souboru s přibližně 30 snímky pomocí aplikace Microsoft Office PowerPoint v režimu celé obrazovky. Snímky mají další text nároky než ve scénáři vedoucími/komplexního PowerPoint a mít jednodušší pozadí a obrázky (černé diagramy). 
    
### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer – spuštění pro 250 sekund

Uživatel přejde na webu pomocí aplikace Internet Explorer. Uživatel přejde a posunuje kombinaci text, přirozené obrázky a některé schémata. Webové stránky uložené na místním disku serveru vzdálené plochy relace hostitele (RD relací) jako. Soubor MHT. Uživatel se posunuje Page Up, Page Down, nahoru a dolů klávesy, pomocí odlišnými intervaly pro každý klíč/druh posuvník:
    
    - Dolů – 250 stisknutím kláves velmi 500 ms
    - Page Up - 36 stisknutím kláves každé 1000 ms
    - Dolů – 75 stisknutí kláves každé 100 ms
    - Page Down - 20 stisknutí kláves každé 500 ms
    - Až - 120 kláves každé 300 ms
    
### <a name="pdf-document---simple---run-for-610-seconds"></a>Dokument PDF – jednoduché – spuštění ~ 610 sekund
Uživatel bude číst a pomocí Adobe Acrobat Reader prohledá dokument PDF různými způsoby. Dokument skládat z tabulek, jednoduché grafů a více písma textu. Dokument je 35-40 stránky dlouhé. Uživatel zpětná posunuje na dva různé sazby a předá na čtyři různé zvětšení velikosti (přizpůsobit stránky, přizpůsobit šířku, 100 % a jinou e). Zvětšení zobrazení zaručuje, že se vykreslují pomocí v různých velikostech textu (písmo). Posouvání je vypnutý Page Up, Page Down, nahoru a dolů klávesy, pomocí odlišnými intervaly pro každou posuvníku.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF dokumentů – smíšených - spustit ~ 320 sekund
Uživatel bude číst a hledá dokument PDF různými způsoby pomocí Adobe Acrobat Reader. Dokument se skládá z vysoce kvalitní obrázky (včetně fotografie), tabulek, jednoduché grafů a více písma textu. Uživatel zpětná posunuje na dva různé sazby a předá na čtyři různé zvětšení velikosti (přizpůsobit stránky, přizpůsobit šířku, 100 % a jinou e). Zvětšení zobrazení zaručuje, že se vykreslují pomocí v různých velikostech textu (písmo). Posouvání je vypnutý Page Up, Page Down, nahoru a dolů klávesy, pomocí odlišnými intervaly pro každou posuvníku.

### <a name="flash-video-playback---run-for-180-seconds"></a>Přehrávání videa ve formátu Flash – spuštění ~ 180 sekund
Uživatel zobrazí Adobe Flash kódovaný video vložené do webové stránky. Webové stránky je uložen v místním pevném disku serveru RD relace. Bude video přehrávat v aplikaci Internet Explorer pomocí vloženého přehrávače modulu plug-in.

Tento scénář napodobuje uživateli, kteří zobrazují bohaté webové stránky obsahu obsahující multimediální. Většina data by měla bo prostřednictvím VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Word vzdálené psát – spuštění ~ 1800 sekund
Uživatel zadá dokumentu prostřednictvím RDP relace. Stisknutím kláves jsou odeslány na straně klienta přes relaci RDP dokumentu v aplikaci Microsoft Word aplikaci vzdálenou relaci. Psaní sazba je jeden znak každé 250 ms (celkový 7050 znaků). 

To je jedna nejběžnější scénáře pro znalostí. Tento scénář testuje citlivosti uživatele zadáním do procesor moderní práce. Tento scénář se rozlišují malá písmena na sudých malé změny využití šířky pásma.

## <a name="tracking-the-test-results"></a>Sledování výsledků testu

V následující tabulce můžete použít k vyhodnocení scénáře ve vašem prostředí. Data uvedeny níže jsou určené pro obrázek: může být výrazně liší od můžete sledovat. 

Pro zjednodušení jsme předpokládá, že jsou všechny scénáře testováno pomocí stejné rozlišení obrazovky 1920 × 1080 pixelů a TCP přenosy v síti s latence (zpoždění) pod 200 ms a síťové kolísání označení 120 ms + o 1 %.

Informace o tabulce:
- **Zaznamenat průměr** obsahuje šířka pásma kam produktivity uživatelů není ovlivněn výrazně ale není vyloučit občas poruch videoklipu nebo zvukového. Systém je možné rychle obnovit využitím dynamických logiky. Síťové šířky pásma odhady pokus o zaručit kvalitu uživatelského prostředí.
 - **Noticeable problémy (zarážky)** obsahuje šířka pásma uživatelé všimnout významné problémy v jejich prostředí, kde jejich produktivity dopad pro možné měřit časová období. V tomto okamžiku algoritmů RDP jsou usilující a nezaručuje uživatele kvalitu zkušenosti vzhledem nedostatečná šířka pásma.
 - **Doporučené** obsahuje šířka pásma pro dobré nebo pracovníků prostředí. Je obvykle postupně po jednotlivých krocích vyšší než hodnota ve sloupci odpovídající **průměr dojít** .
 - **Poznámky** obsahují připomínek a poznámek.
 
| Test                  | Průměrná prostředí | Výrazná problémy (zarážky) | Doporučené šířka pásma | Poznámky                                                              |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Vedoucími/komplexního PPT | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s upřednostňované    | Na 1 MB/s budou ztraceny mnoho animace                                   |
| Jednoduchý PPT            | 5 MB/s              | 256 KB/s                         | 10 MB/s                        | Na 256 KB/s snímky načíst s výrazná zpoždění                   |
| Internet Explorer     | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s upřednostňované    | Na 1 MB/s videí web rozmazaně a trhané, rychlý pohyb mezi listy. problémy |
| Jednoduchý PDF            | 1 MB/s              | 256 KB/s                         | 5 MB/s                         | Na 256 KB/s trvá při načtení stránky                       |
| Smíšené PDF             | 1 MB/s             | 256 KB/s                         | 5 MB/s                         | Na 256 KB/s stránky trvá značné množství času načtení    |
| Flash přehrávání videa  | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s upřednostňované    | Na 1 MB/s je zrnitý a některé snímky se nezobrazí.           |
| Vzdálené psaní ve Wordu    | 256 KB/s            | 128 KB/s                         | 1 MB/s                         | Na 256 KB/s uživateli všimnout dobu mezi stisknutím kláves             |

Pokud chcete zjistit šířka pásma na uživatele, vytvořte kombinaci situacích uvedených výše a odpovídající část požadované šířka pásma. Vyberte na nejvyšší číslo potřebné pro vaše scénáře. Vzhledem k tomu, že uživatelé používat téměř nikdy samostatně systém, zvažte některé rezervy uživatelům, kteří současně pracovat na stejné síti.
     
## <a name="learn-more"></a>Víc se uč
- [Odhad využití šířky pásma sítě Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp – jak šířka pásma nebo kvalitu dojít práce pohromadě?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp šířku pásma sítě – obecné pokyny (když nemůžete testovat vlastní)](remoteapp-bandwidthguidelines.md)