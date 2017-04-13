<properties
    pageTitle="Přenosy správce – provoz metody směrování | Microsoft Azure"
    description="Tento článek vám pomůže pochopit jiný přenos směrování metodách správcem přenosy"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-traffic-routing-methods"></a>Směrování přenosu metod přenosy správce

Azure správce přenosy podporuje tři způsoby směrování přenosu a zjistěte, jak chcete síť provoz směrovat na různých koncové body služby. Správce přenosy se týká metodu směrování přenosu obou dotazů DNS, které obdrží. Metoda směrování přenosu Určuje, který koncový bod vrátil v odpovědi na DNS.

Správce prostředků Azure podpora pro správce přenosy používá jiná terminologie, než modelu klasické nasazení. V následující tabulce jsou uvedeny rozdíly mezi správce zdrojů a klasické podmínek:

| Správce prostředků termínů | Klasický termínů |
|-----------------------|--------------|
| Metody směrování přenosu | Metody vyrovnávání zatížení |
| Metoda priority (priorita) | Metoda překlopení |
| Váženého metody | Metoda kruhového |
| Metoda výkonu | Metoda výkonu |

Na základě reakcí zákazníků jsme změnili terminologie pro zvýšení přehlednosti a zkrácení běžné nedorozuměním. Není žádný rozdíl funkčnosti.

Třemi způsoby přenosy směrování k dispozici ve Správci přenosy:

- **Priority (priorita):** Když budete chtít použít pro všechny přenosy koncový bod primární služby a zadejte zálohy v případě primární nebo záložní koncové body nejsou k dispozici, vyberte "Prioritu".
- **Váha:** Vyberte "váha" když budete chtít distribuce přenosy přes sadu koncové body, buď rovnoměrně nebo podle gramáží, které můžete definovat.
- **Výkonu:** "Výkonu" vyberte, pokud se koncové body v různých zeměpisné lokality a chcete koncoví uživatelé používat "nejbližší" koncový bod z hlediska nejnižší latence sítě.

Všechny přenosy správce profily zahrnují sledování stavu koncového bodu a převzetí automatické koncového bodu. Další informace najdete v tématu [Sledování koncový bod přenosy správce](traffic-manager-monitoring.md). Jeden profil správce přenosy můžete použít pouze jednu metodu směrování přenosu. Kdykoli můžete vybrat metodu směrování jiný přenos pro svůj profil. Změny se projeví v rámci jednu minutu a vzniklé žádné výpadek služeb. Směrování přenosu metody můžete slučují pomocí vnořené profily přenosy správce. Vnoření umožňuje sofistikované a flexibilním směrování přenosu konfigurace, které splňují potřeby větší složitých aplikací. Další informace najdete v tématu [vnořené profily přenosy správce](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Metoda směrování přenosu priority (priorita)

Často organizace chce poskytnout spolehlivosti pro své služby nasazením jednu nebo více záložní služeb v případě, že jejich primární služby havaruje. Metodu směrování přenosu "Priorita" umožňuje snadno implementovat tento způsob převzetí Azure zákazníkům.

![Azure přenosy správce "Priorita" metody směrování přenosu][1]

Profil správce přenosy obsahuje uspořádaný seznam koncové body služby. Ve výchozím nastavení přenosy správce rozešle primární koncového bodu (nejvyšší prioritu) všechny přenosy. Pokud primární koncový bod není k dispozici, přenosy správce přesměrovává přenosy na druhý koncový bod. Pokud primární a sekundární koncovém nejsou k dispozici, půjde přenos třetí a tak dál. Dostupnost koncového bodu vychází z nakonfigurované stavu (povolit nebo zakázat) a sledování probíhající koncového bodu.

### <a name="configuring-endpoints"></a>Konfigurace koncové body

Pomocí Správce prostředků Azure nakonfigurujete priority koncový bod explicitně pomocí vlastnost "priority" pro každý koncový bod. Tato vlastnost je hodnota mezi 1 a 1 000. Nižší hodnoty představují vyšší prioritu. Koncové body nejde sdílet priority (priorita) hodnoty. Nastavení vlastnosti je nepovinný krok. Při vynechání, bude použita výchozí priorita založená na pořadí koncového bodu.

Pomocí rozhraní klasické Priorita koncový bod nakonfigurován implicitně. Priorita je založena na pořadí, ve kterém jsou uvedeny koncové body v definici profilu.

## <a name="weighted-traffic-routing-method"></a>Váha metody směrování přenosu

Metodu "Vážený" směrování přenosu umožňuje rovnoměrné rozmístění přenosy nebo použít předdefinované váhu.

![Azure přenosy "Vážený" směrování přenosu metody správce][2]

V váženého směrování přenosu metodě přiřadíte tloušťku každý koncový bod v konfiguraci profilu přenosy správce. Hmotnost je celé číslo od 1 do 1 000. Tento parametr je nepovinný krok. Pokud není zadáno, používá přenosy správci tloušťku výchozí '1'.

Pro každý DNS dotaz dostali přenosy správce vybírá náhodně koncový bod k dispozici. Pravděpodobnost Výběr koncového vychází z gramáží přiřazená všechny dostupné koncové body. Použití stejné tloušťky všechny koncové body za následek i přenosy rozdělení. Používání vyšších a nižších gramáží na konkrétní koncové body způsobí, že tyto koncové body budou vráceny více nebo méně často v odpovědích DNS.

Váženého metoda umožňuje některé scénáře užitečné:

- Upgrade postupné aplikace: určité procento provoz směrovat na nový koncový bod a postupně zvyšovat přenosů na 100 %.
- Aplikací migrací do Azure: vytvoření profilu s Azure i externí koncové body. Upravte tloušťky koncových bodů raději nové koncové body.
- Shluk roztržení další kapacitu: rychle vložením za profil správce přenosy rozbalte místního nasazení do cloudu. Když budete potřebovat další kapacita v cloudu, můžete přidat nebo povolení další koncové body a určit, jakou část přenosy přejde na každý koncový bod.

Nový Azure portál podporuje konfigurace váženého přenosy směrování. Na portálu klasické se nedají konfigurovat gramáží. Můžete taky nakonfigurovat gramáží pomocí klasické verze Azure PowerShell rozhraní příkazového řádku a rozhraní REST API a správce prostředků.

Je důležité pochopit, odpovědi DNS ukládány klienty a servery DNS rekurzivní klientů používajících jmen DNS. Tento ukládání do mezipaměti mohou mít vliv na váženého přenosy rozdělení. Po velký počet klientů a servery DNS rekurzivní přenosy rozdělení očekávaným způsobem. Však po malých počet klientů nebo servery DNS rekurzivní ukládání do mezipaměti může výrazně zkosení přenosy rozdělení.

Běžné případy použití patří:

- Testování prostředí a vývoj
- Použití aplikace komunikace
- Aplikace zaměřené na úzký uživatele – základ, sdílet běžné infrastruktury DNS rekurzivní (například zaměstnance společnosti připojení prostřednictvím proxy serveru)

Tyto efekty mezipaměť DNS jsou společná pro všechny přenosy založené na DNS systémů směrování nejen Azure přenosy správce. V některých případech může explicitně vymazat mezipaměť DNS poskytuje řešení. V ostatních případech může být vhodnější alternativní metody směrování přenosu.

## <a name="performance-traffic-routing-method"></a>Metoda směrování přenosu výkonu

Nasazení koncové body ve dvou nebo víc umístěních po celém světě můžete vylepšit citlivosti mnoho aplikací směrování přenosů do umístění, které je nejblíž vám. Metoda směrování přenosu "Výkonu" poskytuje tuto možnost.

![Azure přenosy správce "Výkonu" metody směrování přenosu][3]

"Nejbližší" koncový bod je nutně nejblíž měřeno zeměpisné vzdálenost. Místo toho metoda směrování přenosu "Výkonu" určuje nejbližší koncového bodu měřením latence sítě. Přenosy správce udržuje tabulku Internet latence přehled o čase přenosu mezi rozsahy IP adres a každý Azure datacentra.

Přenosy správce vyhledá IP adresu zdrojového příchozí žádosti DNS v tabulce latence Internet. Přenosy správce vybere koncový bod k dispozici v Azure datacentru, která má nejnižší latence pro tuto rozsah IP adres, a pak vrátí tohoto koncového bodu v odpovědi DNS.

Způsobem popsaným v tématu [Jak funguje přenosy správce](traffic-manager-how-traffic-manager-works.md), správce přenosy přímo z klientů dotazů DNS neobdrží. Místo toho dotazy DNS pocházet z služba DNS rekurzivní klienty nakonfigurované pro použití. Proto IP adresu požívaný k určení "nejbližší" koncový bod není IP adresa klienta, ale je IP adresa rekurzivní služby DNS. Tuto IP adresu ve skutečnosti je dobré proxy serveru pro klienta.

Přenosy správce pravidelně aktualizuje tabulce Internet latence kontrolujte změny v globální Internet a nové Azure oblasti. Však výkon aplikace se liší v závislosti na data v reálném kolísání zatížení všude na Internetu. Směrování přenosu výkonu nesleduje zatížení koncový bod dané služby. Ale pokud koncový bod nebude k dispozici, správce přenosy znamená není v odpovědi na dotazy DNS.

Ukazatel je potřeba pamatovat:

- Pokud váš profil obsahuje více koncové body ve stejné oblasti Azure, správce přenosy distribuuje přenosy mezi dostupné koncové body v dané oblasti rovnoměrně. Pokud raději používáte jiný přenos rozdělení v oblasti, můžete [vnořené profily přenosy správce](traffic-manager-nested-profiles.md).

- Pokud jsou sníženou kvalitu. všechny povolené koncové body v dané oblasti Azure, rozdělí přenosy správce přenosy všechny ostatní koncové body k dispozici místo na nejbližší koncový bod. Tato logika zabrání selhání CSS výskytu tak, že není přetížení nejbližší koncového bodu. Pokud chcete k určení pořadí upřednostňované selhání, pomocí [vnořené profilů přenosy správce](traffic-manager-nested-profiles.md).

- Při použití metodu výkonu směrování přenosu v externí koncové body nebo vnořené koncové body, budete muset zadat umístění tyto koncové body. Výběr oblasti Azure nejblíže k nasazení. Těchto umístěních jsou podporovány Internet latence hodnoty.

- Algoritmus zvolí koncový bod je deterministický. Opakované dotazy DNS ze stejného klienta jsou směrovány stejný koncový bod. Obvykle klienti pomocí různých rekurzivní servery DNS při cestování. Klient může být směrovány různých koncový bod. Směrování můžete taky ovlivněna aktualizace tabulky latence Internet. Proto metodu směrování přenosu výkonu nezaručuje, že klienta směrován vždy stejné koncový bod.

- Při změně tabulce latence Internet, mohou být v některých klientech se přesměrují do různých koncového bodu. Tato změna směrování je přesnější podle aktuálního data latence. Tyto aktualizace jsou nutné zachovat přesnost směrování přenosu výkonu neustále vývoj na Internetu.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak se dají pomocí [Správce přenosy koncový bod sledování](traffic-manager-monitoring.md) aplikací s vysokou dostupností

Zjistěte, jak [vytvořit profil správce dopravy](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png
