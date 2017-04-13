<properties 
    pageTitle="Azure implementaci zapojení mobilní aplikace médií"
    description="Scénář aplikace multimédia implementovat zapojení Mobile Azure" 
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

#<a name="implement-mobile-engagement-with-media-app"></a>Implementace mobilní zapojení s aplikací médií

## <a name="overview"></a>Základní informace

Jan je mobilní vedoucí společnost velký média. Tahle naposledy spuštění nové aplikace, která obsahuje počet velmi vysoké stáhnout. Tahle překročil své cíle ke stažení, ale pořád jeho vrátit na Investment(ROI) za uživatele jeho požadavky nesplňuje. 

Jan byl zjištěn už proč jeho návratnost investic je příliš nízká. Uživatelé často nepoužívali jeho aplikace po pouze dva týdny a většina z nich nikdy vrátit. Chce zvětšit uchování jeho aplikace.

Po některé s velkými testování, mu má dozvěděli při mu věnuje jeho uživatelé, kteří mají nabízená oznámení, budou většinou používejte dál jednotné jeho aplikace. Také uživatelů, které byly neaktivní se často vraťte se do aplikace v závislosti na oznámení, která má jim poslala. Jan rozhoduje o investovat v nějakém programu zapojení jeho aplikace využívající Upřesnit cílovou se nabízená oznámení.

Jan naposledy četl [Azure Mobile zapojení - Příručka Začínáme s osvědčené postupy](mobile-engagement-getting-started-best-practices.md) a rozhodla implementovat doporučení z průvodce.

##<a name="objectives-and-kpis"></a>Cílů a klíčových ukazatelů výkonu

Zahájit zodpovědné za aplikace John's. Obchodní je generováno ze služby Active Directory uživatelé používat jeho média. Zvětšením obsah spotřebované množství za uživatele Petr zvyšuje jeho výnosů. Všechny souhlasíte na jeden hlavní cíl: Pokud chcete zvýšit prodeje ze služby Active Directory hodnotou 25 %. Vytvářejí firmy klíčové ukazatele výkonu (KPI) míry a jednotky tohoto cíle

* Počet reklamu kliknutí na uživatele
* Kolik stránek článků navštívili (za uživatele / relace / týden a měsíc...)
* Jaké jsou oblíbený kategorie

Na základě John's schůzky s zodpovědné za osoba je definován své firmy klíčových ukazatelů výkonu. Osoba sleduje část 1 [Azure Mobile zapojení - Příručka Začínáme s doporučené postupy](mobile-engagement-getting-started-best-practices.md). 

Dále si vytvoří následující zapojení klíčových ukazatelů výkonu k zajištění dosažení cíle:

* Sledování uchovávání informací mezi následujícími intervaly: denně, týdně, bi týdenní a měsíční.
* Vrátí aktivní uživatelé
* Uloží hodnocení aplikace v seznamu aplikací

Na základě doporučení od týmu IT, následující technické klíčové ukazatele výkonu se přidala do odpovězte na následující otázky:

* Co je Moje cesta uživatele (navštívenou stránku, která kolik uživatelů čas strávený ho)
* Počet havárie nebo chyby narazili relace?
* Jaké verze operačního systému používáte svoje uživatele?
* Co je průměrná velikost obrazovky pro naše uživatele?
* Jaký druh připojení k Internetu mají svoje uživatele?

Pro každý ukazatel KPI mu klasifikuje požadované údaje a mu záznamů do správného umístění jeho playbook.

## <a name="engagement-program-and-integration"></a>Zapojení program a integrace

Teď dokončení definování jeho klíčové ukazatele výkonu, které Jan má na začátku jeho zapojení strategie fáze tím, že definujete 4 zapojení programy a jejich cíle:    ![][1]

Jan otevře hlubší tak, že s podrobnostmi nabízených oznámení pro jednotlivé aplikace. Nabízená oznámení jsou definovány pěti prvků:

1. Cíl: co cílem je oznámení
2. Jak se dosažení cíle
3. Cíl: kdo, dostanou oznámení?
4. Obsah: Co je text a formát oznámení (v App/Out z aplikace)
5. Kdy: Jaký je nejlepší momentový odeslat toto nabízená oznámení

    ![][2]

Další informace naleznete v [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Podle části 2 dokument white paper Jan používá cílový oddíl, který definuje jaká data má shromažďovat a data zapisuje jeho značku plánování společně s IT týmu implementovat řešení. Po 1 týden implementaci a testování přijetí uživateli Jan nakonec samostatné předplatné jeho programy:

##<a name="program-results"></a>Výsledky programu

4 měsíců, Jan kontroluje výkonů programů. Uvítací Program a týdenního programu jsou schůzky své cíle. Snižuje počet uživatel s jedinou relací, použily dalších funkcí aplikace a má dvojitý počet připojení za týden.

**Neaktivní Program** pomáhá Jan pochopit tendence uživatele. Zdá se, že 15 % neaktivní uživatele vrátit do aplikace. Ale většina z nich nemáte zůstat aktivní víc než 1 měsíc. Jan předpokládá potenciální optimalizace Tato řada s další oznámení a rozbalení jeho obsahu voleb.

**Seznamte se s aplikací** nefunguje správně. Zvyšuje křížové selling ale není dost kontaktovat své cíle. Jan označuje, že osoba nemá dost data tak, aby odpovídající zacílení a navrhnout příslušný obsah. Tahle zastaví tento program a se zaměřuje na odeslání "redakční nabízená oznámení" s Azure Mobile Engagement. Jeho novináři už máte řešení cm odeslat nabízená oznámení a nechcete, pokud chcete změnit.

Jan rozhoduje o použití rozhraní API kontaktovat, což je rozhraní REST API HTTP, umožňující Správa Reach kampaní bez nutnosti použití AZME webového rozhraní. Tento přístup Jan data můžete shromažďovat mu potřebuje a povolit jeho autorům opakujte provádění cm řešení.

Abyste měli jistotu, že tato funkce funguje správně, Jan dotazem IT tým dbá na následující otázky:

1. **Operace systémy** : všechny mají svoje vlastní pravidla ke správě nabízená oznámení, takže Jan rozhoduje o seznam všech případech zkontroluje, pokud rozhraní API zpracovat jeho.
Např: Systém Android nabízených umožňuje celkového, který není v případě s iOS.

2. **Časové rozmezí**: Jan chce rozhraní API, které nastavit časový rámec a nastavte zakončení kampaní. Chce zachovat uživatelů z libovolné bombing vyloučit oznámení.

3. **Kategorie**: týmu Marketing připraví šablony u každého typu upozornění. Jan zeptá IT týmu nastavit kategorie uvnitř rozhraní API.

Některé testů Jan po spokojení. Díky této rozhraní API novináři pořád zasílat nabízená oznámení s jejich cm a využití Mobile Azure shromáždí všech chování dat pro ně

Po těchto 4 nejdřív měsíců, výsledky odrážet dobré celkový výkon a poskytuje spolehlivosti Jan a jeho vývěsky návratnost investic za uživatele přirážky za 15 % a mobil představují 17.5 % celkového prodeje nárůst 7.5 % pouze čtyři měsíce.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
