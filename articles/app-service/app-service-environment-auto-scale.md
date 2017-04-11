<properties
    pageTitle="Neobsahovaly text a prostředí aplikace služeb | Microsoft Azure"
    description="Neobsahovaly text a prostředí aplikace služby"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"
/>

# <a name="autoscaling-and-app-service-environment"></a>Neobsahovaly text a prostředí aplikace služby

Azure aplikaci služby prostředí podporují možnosti *neobsahovaly text*. Můžete automatické měřítko jednotlivé pracovní skupiny na základě metriky nebo plánu.

![Automatické měřítko možnosti pracovní skupiny.][intro]

Neobsahovaly text optimalizuje využití prostředků automaticky Kvetoucí a zmenšením prostředí aplikace služby a přizpůsobit rozpočtu a načíst profil.

## <a name="configure-worker-pool-autoscale"></a>Konfigurace automatické pracovníka fondu měřítko

Máte přístup k funkci Automatické měřítko na kartě **Nastavení** fondu kolegy.

![Karta nastavení fondu kolegy.][settings-scale]

V ní má být rozhraní ovládat od jeho budou mít stejné možnosti, že se při změně velikosti plán služeb aplikací. 

![Ruční nastavení.][scale-manual]

Můžete taky nakonfigurovat automatické měřítko profilu.

![Nastavení automatické měřítko.][scale-profile]

Automatické měřítko profily jsou užitečné můžete nastavit omezení pro použít měřítko. Tímto způsobem může mít konzistentní výkon dojít nastavením hodnota násobku dolní mez (1) a předvídatelná výdaji zakončení nastavením horní mez (2).

![Nastavení profilu.][scale-profile2]

Po definování profilu můžete přidat pravidla automatické měřítko zobrazit nahoru nebo dolů počet instancí ve fondu pracovníka v rozmezí definované profilem. Automatické měřítko pravidla vycházejí metriky.

![Pravidlo měřítko.][scale-rule]

 Pracovní fond nebo front-end metriky lze definovat automatické měřítko pravidla. Tyto metriky jsou stejné metriky můžete sledovat v grafech zásuvné zdrojů nebo nastavit upozornění pro.

## <a name="autoscale-example"></a>Příklad automatické měřítko

Automatické měřítko z prostředí aplikace služby lze ilustrovat nejlépe tak, že rozbor situace.

Tento článek vysvětluje všechny potřebné aspektech při nastavení automatické měřítko. Tento článek vás provede interakcí, které uplatnit při faktor v neobsahovaly text aplikaci služby prostředí, které jsou hostované v prostředí aplikace služeb.

### <a name="scenario-introduction"></a>Scénář Úvod

ADAM je členem Enterprise, kdo má nemigruje část úloh, které spravuje do prostředí aplikace služby.

Prostředí aplikace služby je nakonfigurovaný na ruční měřítko následujícím způsobem:

* **Přední konce:** 3
* **Fond pracovníka 1**: 10
* **Fond pracovníka 2**: 5
* **Pracovní fondu 3**: 5

Fond pracovníka 1 se používá pro výrobní úloh, během pracovních fondu 2 a pracovní fondu 3 se používají pro jakosti (q & a) a vývoj úloh.

Plány aplikaci služby pro q & a a vývoj nakonfigurovaná na ruční měřítko. Výrobní plán služeb aplikací nastavenou automatické měřítko pro práci s různými variantami načítání a přenos.

ADAM je povědomý s aplikací. Ví Špička dobu zatížení se 9:00:00 až 6:00 odp protože tento se s aplikací (LOB –) řádek obchodní využívající zaměstnanců době, kdy budou v office. Použití vynechává po, když uživatelé dělají pro daný den. Mimo pole Špička hodin přetrvává některé zatížení vzhledem k tomu, že uživatelé měli přístup k aplikaci vzdáleně pomocí jejich mobilní zařízení nebo domácí PC. Výrobní plán služeb aplikací už nakonfigurovaný tak, aby automatické měřítko podle využití procesoru s těmito pravidly:

![Nastavení aplikace LOB.][asp-scale]

|   **Automatické měřítko profil – dny v týdnu – plán služeb aplikací**     |   **Automatické měřítko profil – víkendy – plán služeb aplikací**     |
|   ----------------------------------------------------    |   ----------------------------------------------------    |
|   **Název:** Profil den v týdnu                               |   **Název:** Víkendu profilu                               |
|   **Měřítko:** Plán a výkonu pravidel            |   **Měřítko:** Plán a výkonu pravidel            |
|   **Profil:** Dny v týdnu                                   |   **Profil:** Víkendu                                    |
|   **Typ:** Opakování                                    |   **Typ:** Opakování                                    |
|   **Cílovou oblastí:** instance 5 až 20                     |   **Cílovou oblastí:** instance 3 až 10                     |
|   **Dnů:** Pondělí, úterý, středa, čtvrtek, pátek  |   **Dnů:** Sobota, neděle                              |
|   **Spuštění:** 9:00 dop.                                 |   **Spuštění:** 9:00 dop.                                 |
|   **Časové pásmo:** UTC 08                                   |   **Časové pásmo:** UTC 08                                   |
|                                                           |                                                           |
|   **Automatické měřítko pravidlo (měřítko nahoru)**                           |   **Automatické měřítko pravidlo (měřítko nahoru)**                           |
|   **Zdroje:** Výrobní (aplikaci služby prostředí)      |   **Zdroje:** Výrobní (aplikaci služby prostředí)      |
|   **Míru:** VYUŽITÍ PROCESORU %                                       |   **Míru:** VYUŽITÍ PROCESORU %                                       |
|   **Operace:** Větší než 60 %                         |   **Operace:** Vyšší než 80 %                         |
|   **Doby trvání:** 5 minut                                 |   **Doby trvání:** 10 minut                                |
|   **Čas agregace:** Průměr                           |   **Čas agregace:** Průměr                           |
|   **Akce:** Zvýšení počtu číslem 2                         |   **Akce:** Zvýšení počtu 1                         |
|   **Zajímavých dolů (v minutách):** 15                             |   **Zajímavých dolů (v minutách):** 20                             |
|                                                           |                                                           |
  	|   **Automatické měřítko pravidlo (měřítko dolů)**                     |   **Automatické měřítko pravidlo (měřítko dolů)**                         |
|   **Zdroje:** Výrobní (aplikaci služby prostředí)      |   **Zdroje:** Výrobní (aplikaci služby prostředí)      |
|   **Míru:** VYUŽITÍ PROCESORU %                                       |   **Metriky:** VYUŽITÍ PROCESORU %                                       |
|   **Operace:** Menší než 30 %                            |   **Operace:** Nižší než 20 %                            |
|   **Doby trvání:** 10 minut                                |   **Doba trvání:** 15 minut                                |
|   **Čas agregace:** Průměr                           |   **Čas agregace:** Průměr                           |
|   **Akce:** Zmenšit počet 1                         |   **Akce:** Zmenšit počet 1                         |
|   **Zajímavých dolů (v minutách):** 20                             |   **Zajímavých dolů (v minutách):** 10                             |

### <a name="app-service-plan-inflation-rate"></a>Sazba inflační plánu aplikace služby

Aplikace služby plánuje nakonfigurované automatické měřítko provést maximální sazbou za hodinu. Tento kurz se dají zpracovat založené na hodnoty zadané pravidla automatické měřítko.

Principy a výpočty *sazba inflační plánu aplikace služby* je důležité pro automatické aplikaci služby prostředí měřítko, protože změny měřítka fondu pracovníka nejsou okamžité.

Sazba aplikaci služby plánu inflační se vypočítá následujícím způsobem:

![Výpočet míry inflace plánu aplikace služby.][ASP-Inflation]

Založené na automatické měřítko – pravidla měřítko nahoru profilu den v týdnu výrobní plán služeb aplikací:

![Aplikace služby sazba inflační plánu pro pracovní dny podle automatické měřítko – měřítko nahoru pravidlo.][Equation1]

V případě automatické měřítko – pravidla měřítko nahoru profilu víkendu výrobní plán služeb aplikací, vzorec by překládal:

![Aplikace služby plán inflační sazbu víkendu podle automatické měřítko – měřítko nahoru pravidlo.][Equation2]

Tato hodnota můžete taky vypočítat pro operace měřítko shora dolů.

Založené na automatické měřítko – měřítko dolů pravidlo pro den v týdnu profil výrobní plán služeb aplikací, to bude vypadat takto:

![Aplikace služby sazba inflační plánu pro pracovní dny podle automatické měřítko – měřítko dolů pravidlo.][Equation3]

V případě automatické měřítko – měřítko dolů pravidlo považováno za víkendové profil výrobní plán služeb aplikací, vzorec by překládal:  

![Aplikace služby plán inflační sazbu víkendy podle automatické měřítko – měřítko dolů pravidlo.][Equation4]

Výrobní plán služeb aplikací rozrůstat na maximální rychlost osm instance/hodiny v týdnu, čtyři instance/hodinu víkendu. Můžete uvolnit instance maximální rychlostí čtyři instance/hodiny v týdnu a šest instance/hodinu víkendy.

Pokud více plány aplikaci služby hostitelském do fondu kolegy, budete muset vypočítat *celkové míry inflace* jako součet míry inflace u všech plánů aplikaci služby, které jsou hostingu v tomto fondu kolegy.

![Výpočet celkové inflační kurzu pro více plány aplikaci služby použitý ve fondu pracovníka.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Umožňuje definovat pracovní fondu automatické měřítko pravidla sazba inflační plánu aplikace služby

Pracovní fondy, které hostují aplikaci služby plánuje nakonfigurované automatické měřítko potřebovat přidělená vyrovnávací paměť kapacity. Vyrovnávací paměť umožňuje operace automatické měřítko zvětšovat a zmenšovat plán služeb aplikace podle potřeby. Minimální vyrovnávací paměť by Počítané celkové aplikace služby plánování inflační sazbu.

Protože aplikace služby prostředí měřítko operace chvíli trvat použít, změny by měl účtu pro další změny službu, ke kterým může dojít v průběhu operace měřítko. Tak, aby zahrnoval tento latence, doporučujeme použít počítané celková aplikace služby plánování inflační rychlost jako minimální počet instance, které se přidají pro každou akci automatické měřítko.

Tyto informace lze definovat ADAM automatické měřítko profil a pravidla:

![Automatické měřítko profilu pravidla LOB – třeba.][Worker-Pool-Scale]

|   **Automatické měřítko profil – dny v týdnu**                        |   **Automatické měřítko profil – víkendy**                |
|   ----------------------------------------------------    |   --------------------------------------------    |
|   **Název:** Profil den v týdnu                               |   **Název:** Víkendu profilu                       |
|   **Měřítko:** Plán a výkonu pravidel            |   **Měřítko:** Plán a výkonu pravidel    |
|   **Profil:** Dny v týdnu                                   |   **Profil:** Víkendu                            |
|   **Typ:** Opakování                                    |   **Typ:** Opakování                            |
|   **Cílovou oblastí:** instance 13 až 25                    |   **Cílovou oblastí:** instance 6 až 15             |
|   **Dnů:** Pondělí, úterý, středa, čtvrtek, pátek  |   **Dnů:** Sobota, neděle                      |
|   **Spuštění:** 7:00 dop.                                 |   **Spuštění:** 9:00 dop.                         |
|   **Časové pásmo:** UTC 08                                   |   **Časové pásmo:** UTC 08                           |
|                                                           |                                                   |
|   **Automatické měřítko pravidlo (měřítko nahoru)**                           |   **Automatické měřítko pravidlo (měřítko nahoru)**                   |
|   **Zdroje:** Fond pracovníka 1                             |   **Zdroje:** Fond pracovníka 1                     |
|   **Míru:** WorkersAvailable                            |   **Míru:** WorkersAvailable                    |
|   **Operace:** Méně než 8                              |   **Operace:** Menší než 3                      |
|   **Doby trvání:** 20 minut                                |   **Doba trvání:** 30 minut                        |
|   **Čas agregace:** Průměr                           |   **Čas agregace:** Průměr                   |
|   **Akce:** Zvýšení počtu 8                         |   **Akce:** Zvýšení počtu vynásoben 3                 |
|   **Zajímavých dolů (v minutách):** 180                            |   **Zajímavých dolů (v minutách):** 180                    |
|                                                           |                                                   |
|   **Automatické měřítko pravidlo (měřítko dolů)**                         |   **Automatické měřítko pravidlo (měřítko dolů)**                 |
|   **Zdroje:** Fond pracovníka 1                             |   **Zdroje:** Pracovní fond 1                     |
|   **Míru:** WorkersAvailable                            |   **Míru:** WorkersAvailable                    |
|   **Operace:** Větší než 8                           |   **Operace:** Větší než 3                   |
|   **Doby trvání:** 20 minut                                |   **Doba trvání:** 15 minut                        |
|   **Čas agregace:** Průměr                           |   **Čas agregace:** Průměr                   |
|   **Akce:** Zmenšit počet číslem 2                         |   **Akce:** Zmenšit počet vynásoben 3                 |
|   **Zajímavých dolů (v minutách):** 120                            |   **Zajímavých dolů (v minutách):** 120                    |

Cílový rozsah definované v profilu vypočítáme minimální instance definované v profilu pro plán služeb aplikací + rezervy.

Oblast maximální bude součet všech maximální oblasti u všech plánů aplikaci služby hostované ve fondu pracovníka.

Zvýšení počtu měřítko pravidla by měl nastavit minimálně 1 X sazba inflační plánu služby aplikace měřítko.

Snížení počtu lze upravit na něco mezi 1/2 X nebo 1 X sazba aplikace služby plánu inflační měřítko dolů.

### <a name="autoscale-for-front-end-pool"></a>Automatické měřítko fondu front-end

Pravidla pro automatické front-end měřítko jsou jednodušší než pro pracovní skupiny. Především měli byste  
Ujistěte se, dobu trvání měření a časovače cooldown zvažte operace měřítko na některý z plánů aplikaci služby nejsou okamžité.

V tomto scénáři ADAM ví, že míru chyb zvyšuje po front-end dosáhla 80 % využití procesoru a nastaví automatické měřítko pravidlo zvětšíte instance následujícím způsobem:

![Nastavení automatické měřítko pro fondu front-end.][Front-End-Scale]

|   **Automatické měřítko profil – přední konce**              |
|   --------------------------------------------    |
|   **Název:** Automatické měřítko – přední konce                |
|   **Měřítko:** Plán a výkonu pravidel    |
|   **Profil:** Každý den                           |
|   **Typ:** Opakování                            |
|   **Cílovou oblastí:** instance 3 až 10             |
|   **Dnů:** Každý den                              |
|   **Spuštění:** 9:00 dop.                         |
|   **Časové pásmo:** UTC 08                           |
|                                                   |
|   **Automatické měřítko pravidlo (měřítko nahoru)**                   |
|   **Zdroje:** Fondu front-end                    |
|   **Míru:** VYUŽITÍ PROCESORU %                               |
|   **Operace:** Větší než 60 %                 |
|   **Doby trvání:** 20 minut                        |
|   **Čas agregace:** Průměr                   |
|   **Akce:** Zvýšení počtu vynásoben 3                 |
|   **Zajímavých dolů (v minutách):** 120                    |
|                                                   |
|   **Automatické měřítko pravidlo (měřítko dolů)**                 |
|   **Zdroje:** Fond pracovníka 1                     |
|   **Míru:** VYUŽITÍ PROCESORU %                               |
|   **Operace:** Menší než 30 %                    |
|   **Doby trvání:** 20 minut                        |
|   **Čas agregace:** Průměr                   |
|   **Akce:** Zmenšit počet vynásoben 3                 |
|   **Zajímavých dolů (v minutách):** 120                    |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
