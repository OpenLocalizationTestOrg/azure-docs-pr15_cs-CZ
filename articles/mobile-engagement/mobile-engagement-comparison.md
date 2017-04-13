<properties
    pageTitle="Porovnání zapojení Azure Mobile s jinými službami podobné Azure"
    description="Porovnání zapojení Azure Mobile s jinými podobné Azure službami - HockeyApp, AppInsights, rozbočovače oznámení"
    services="mobile-engagement"
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Porovnání zapojení Azure Mobile s jinými službami podobné Azure

Někdy rozbalení seznamu služby nabízených Microsoft Azure a v některých případech může zajímat, jak se liší od tato služba, která jenom číst nebo dozvíme informace o využití Mobile Azure. Tento článek pokusí zrušte zmatky a směrovat tak, aby zvolit zapojení Mobile Azure, když je nejvhodnější pro používání. 
 
Azure zapojení Mobile je služba cílová speciálně **pro digitální obchodníci/CMOs** , ale může **vlastník mobilní aplikace nebo aplikace publisher** kdo chce ke zvýšení využití, uchovávání informací a možnostmi finančního zhodnocení jejich mobilních aplikací. 

*Je software jako služby uživatele zapojení platformu (SaaS), který obsahuje přehledy založených na datech do aplikace použití Segmentace v reálném čase uživatele a umožňuje kontextově podporující nabízená oznámení a zasílání zpráv v aplikaci.* 

Rozdělení tato definice máme následující klíče vlastnosti, které také zvýrazní jeho nabízená jedinečná hodnota:

1.  **Kontextově podporující nabízená oznámení a zasílání zpráv v aplikaci**
        
    Toto je fokus core produktu: provedení cílových a přizpůsobených nabízená oznámení. A tento problémy, jsme shromáždí propracovaných chování analýzy dat. 

2.  **Založených na datech podstatu používání aplikace**

    Připravili jsme křížového platformy SDK získat informace o chování analýzu pro uživatele aplikace. Poznámka: chování analýzy termínů (vůči výkonu analýzy), protože jsme zaměření se na tom, jak uživatelé aplikace jsou v aplikaci. Budeme shromažďovat data o analýzy základní výkonu o chyby, dojde k chybě atd ale tedy není fokus core produktu. 

3.  **Segmentace uživatelů v reálném čase**

    Jakmile jste shromáždili aplikace uživatelé chování technologie pro analýzu dat, jsme vám umožní segment vaší cílovou skupinou podle různých parametry a shromažďování dat můžete spustit cílových nabízených kampaní. 

4.  **Software jako-service (SaaS):**

    Máme nezávislá na portálu Správa Azure, která je optimalizováno pro komunikovat a analýzu bohaté chování o uživatelích, aplikace a spuštění marketingových kampaní nabízených portál. Produkt je zaměřené vám přejdete během chvilky!   
 
Pomocí této sady rozlišení v ruky to, jak jsme porovnání jiných zdánlivě podobné Azure nabídkách – poznámku, v článku neprovádí úroveň porovnání funkcí ale trvá další situace na základě přístup k definování, který produkt funguje nejlépe:
 
1.  [HockeyApp](https://azure.microsoft.com/services/hockeyapp/) je mobilní DevOps řešení společnosti Microsoft. S ním můžete distribuovat buildy beta testeři, shromažďovat data o selhání a získat názory uživatelů. Také lze integrovat s Visual Studio týmovou umožňuje snadno vytvořit nasazení a integrace položku práce. 
    
    Zaměřuje je DevOps a získávání výkonu technologie pro analýzu dat o mobilních aplikací. Můžete dostat pomocí integrace obou HockeyApps a Mobile zapojení do aplikace a že nejsou neobvyklé vzhledem k tomu, i když v řádcích dat shromážděných některé překrývajících se fokus core produktů se liší a pomáhají při dosahování různých cílů za vás.  

2.  [Přehledy aplikace](../application-insights/app-insights-overview.md) Pokud mobilní aplikace na straně serveru, potom použije aplikace přehledy ke sledování na straně serveru webové aplikace, ale pro mobilní aplikace klienta straně byste měli použít HockeyApp. 

3.  [Oznámení o rozbočovače](https://azure.microsoft.com/services/notification-hubs/) poskytuje služby lightweight znamená, že nemusíte SDK integrované v mobilní aplikaci a máte oprávnění k úplnému řízení mobilní aplikace a umožňuje odesílat nabízená oznámení s základních funkcí značek. Toto je ideální pro všechny aplikace vlastníkem dbá menší informace o skupině a další informace o odesílání/sdělování informací okamžitě uživatelům jejich aplikace (vysílání velké základního souboru). 

    Poznámka: zaměřuje na odeslání svěží rychlé oznámení s možností základní segmentace. 

Podívejme se některé scénáře:

1.  TIM je součástí digitální uvedeni v obchodě Adventure Works, který publikuje mobilních aplikací pro uživatele. Na TIM cílem je zajistit, aby uživatelé, kteří mobilní aplikace stáhnout ji nadále používat a často. Toto nejen zvýší značky připojit pomocí aplikace uživatelé, ale taky zvyšuje možnostmi finančního zhodnocení, pokud uživatelé aplikace nakupovat pomocí mobilní aplikace. Pro tento Tim muset poslat uživatelům aplikace, které jsou užitečné, povzbudit Poznámky otevřete aplikaci a vraťte se často cílových oznámení. Tady najdete Tim bude Mobile zapojení užitečné. 

2.  Joann je součástí týmu vývoje mobilních aplikací na Adventure Works a hledáte produkt protokolování podrobnosti o havaruje, výjimek, a získat výkonu telemetrie z aplikace. Tady najdete Joann bude HockeyApp užitečné. Joann může začlenit HockeyApp své vývojář zaměření scénáře i Azure Mobile zapojení pro Tim ve stejném aplikace na vytěžit maximum obou. 

3.  Petra je součástí týmu vývoje mobilních aplikací na síť Contoso novinky a všechny po něm chce je odeslat zrušení upozornění na nové zprávy všem uživatelům bez mnohem segmentace hned, jak je výstraha příspěvky. Tady najdete Petra bude oznámení rozbočovače užitečné. V tomto scénáři je možné ale, že během existence mobilní aplikace je požadavek na mnohem lepší segmentace a získat informace o chování aplikace uživatele. V současné době budou muset Petra vyhodnocení Azure Mobile Engagement. 
 
Recap, účel zapojení Mobile není právě shromažďovat analýzy – není "ještě další analýzy produkt od společnosti Microsoft". Jedná se o odeslání cílových nabízená oznámení a pro tento cílovou jsme shromažďovat data o chování analýzy ale fokus zůstává na odeslání nabízená oznámení, které jsou nejvhodnější uživatelům aplikace tak, aby nepochází za nevyžádanou poštu. 

Podrobné informace – podívejte se na toto [krátké video](mobile-engagement-overview.md) o využití Mobile ve zkratce. 

