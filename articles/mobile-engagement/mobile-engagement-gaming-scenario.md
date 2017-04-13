<properties 
    pageTitle="Azure implementaci zapojení mobilní aplikace herní"
    description="Scénář aplikace herní implementovat zapojení Mobile Azure" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-gaming-app"></a>Implementace mobilní zapojení s herní aplikací

## <a name="overview"></a>Základní informace

Po spuštění herní má spustit novou aplikaci herní play role, strategie rybaření založena. Hra byl zařídit i po 6 měsíců. Tato hra se velké úspěch, má miliony stahování a uchovávání informací je velmi vysoké ve srovnání s jiných aplikací herní po spuštění. Na schůzce čtvrtletní revize zúčastněných stran souhlasíte, že je nutné zvětšit průměrný výnos na uživatele (ARPU). Balíčky v hra Premium jsou k dispozici jako speciální nabídky. Tyto herní sady umožňují uživatelům upgrade vzhled a výkonu svého rybaření řádky a lures nebo tackles hry. Balíček prodejů jsou však velmi nízký. Takže rozhodnou nejdřív k analýze prostředí zákazníků pomocí nástrojů pro analýzu a pak se dají zaměstnání program, který chcete zvětšit prodeje pomocí pokročilé segmentace.

Založené na [Azure Mobile zapojení - Příručka Začínáme s doporučené postupy](mobile-engagement-getting-started-best-practices.md) vytvářejí efektivní strategii engagement.

##<a name="objectives-and-kpis"></a>Cíle a klíčových ukazatelů výkonu

Zodpovědné za hry setkání. Všechny souhlasíte na jeden hlavní cílem je – zvýšení premium balíčku prodeje o 15 %. Vytvářejí firmy klíčové ukazatele výkonu (KPI) míry a jednotky tohoto cíle

* Na úrovni hry jsou tyto balíčky jste si koupili?
* Co je výnosy za uživatele za relace, za týden a za měsíc?
* Jaké typy Oblíbené zakoupit?

1 součást [Příručky Začínáme](mobile-engagement-getting-started-best-practices.md) vysvětluje, jak k definování cílů a klíčových ukazatelů výkonu. 

Správce produktu Mobile s firmy klíčové ukazatele výkonu teď definované, vytvoří zapojení klíčových ukazatelů výkonu k určení trendů nového uživatele a uchovávání informací.

* Sledování uchovávání informací a používání různých následující intervalů: denně, za dva dny, týdně, měsíčně a každé 3 měsíce
* Počet aktivních uživatelů
* Hodnocení aplikací ve storu

Na základě doporučení od týmu IT, následující technické klíčové ukazatele výkonu se přidala do odpovězte na následující otázky:

* Co je Moje cesta uživatele (navštívenou stránku, která kolik uživatelů čas strávený ho)
* Počet havárie nebo chyby narazili relace
* K čemu OS svoje uživatele verzí?
* Co je průměrná velikost obrazovky pro naše uživatele?
* Jaký druh připojení k Internetu mají svoje uživatele?

Pro každý klíčový ukazatel výkonu určuje správce produktů mobilní data potřebuje a kde se nachází ve svém playbook.

## <a name="engagement-program-and-integration"></a>Zapojení program a integrace

Před vytvořením aplikace pokročilé využití, měli ředitele projektu Mobile zodpovědní za projektu s hloubkové znalostí toho, jak a kdy produkty jsou využívané uživatelů.

Za tři měsíce shromáždil ředitele projektu Mobile dost dat k vylepšení jeho v aplikaci nabízená oznámení prodej. Dozví, který:

* První nákup obecně se stane, na úrovni 14. Nákup 90 % případy, trvá nové Legendární zbraní $3.
* V 80 % případy uživatelé, kteří provedli nákupu, pokračujte s produktem a udělejte další nákup.
* Uživatelé, kteří uplynulo úroveň 20 spustit věnovat více než 10/ týden.
* Uživatelé obvykle koupit balíčků premium na úrovni 16, 24 a 32.

Díky této analýzy ředitele projektu Mobile rozhodl vytvořit konkrétní nabízená oznámení řady zvětšíte v aplikaci sales. Vytváření tři nabízených řad, které zavolá: Vítá program, prodej Program a neaktivní Program. Další informace naleznete v [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)
    ![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
