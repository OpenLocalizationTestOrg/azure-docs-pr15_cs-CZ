<properties 
    pageTitle="Vytvoření rozhraní API pro použití logických operátorů aplikace" 
    description="Vytvoření vlastní rozhraní API pro práci s aplikací logiky" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>Vytvoření vlastní rozhraní API pro práci s aplikací logiky

Pokud chcete rozšířit platformu aplikace použití logických operátorů, mnoha způsoby, které můžete volat do rozhraní API nebo systémů, které nejsou k dispozici jako jednu z našich řada spojnic mimo pole.  Jedním z těchto způsobů vytvoření aplikace pro rozhraní API můžete zavolat z v rámci pracovního postupu aplikace logiky.

## <a name="helpful-tools"></a>Užitečnými nástroji

Rozhraní API pro práci s aplikacemi jiných logiku nejlépe doporučujeme generování [swagger](http://swagger.io) dokumentu s podrobnostmi podporovaná operace a parametry pro vaše rozhraní API.  Existuje mnoho knihoven (třeba [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)), které automaticky vygeneruje swagger za vás.  Můžete taky [TRex](https://github.com/nihaue/TRex) pomůže pořizovat poznámky swagger pro práci s aplikací logiku (Zobrazit názvy, vlastnosti typy atd.).  Pro některé příklady rozhraní API aplikace integrované aplikace použití logických operátorů nezapomeňte najdete v článku naší [GitHub úložiště](http://github.com/logicappsio) nebo do [blogu](http://aka.ms/logicappsblog).

## <a name="actions"></a>Akce

Základní akce aplikace logiky je řadiče, který bude přijmout žádost HTTP a vraťte se na odpověď (obvykle 200).  Je však k dispozici různé vzorce, které sledujete rozšiřte akce podle vašich potřeb.

Ve výchozím nastavení modul logiky aplikace bude vypršení časového limitu žádost za minutu.  Však může mít rozhraní API provedení akce, které trvat déle a máte modul čekání na dokončení, podle pokynů asynchronní nebo webhook vzorek podrobné pod.

Standardní akce jednoduše napište metodu žádost HTTP vaše rozhraní API, které se zobrazí přes swagger.  Zobrazí se příklady rozhraní API aplikace, které spolupracují s aplikacemi jiných logiku v naší [GitHub úložiště](https://github.com/logicappsio).  Tady vidíte způsoby provádění běžných vzorků s vlastní spojnice.

### <a name="long-running-actions---async-pattern"></a>Dlouhodobé akce - asynchronní vzorek

Systém dlouhé krok nebo úkolů, první věc, kterou je potřeba udělat při Ujistěte se, že modul ví, že jste to ještě vypršel časový limit. Bude potřeba komunikovat modul, jak bude vědět, až budete hotovi s úkolem a nakonec je potřeba relevantních dat vrátit modul mohli pokračovat s tímto pracovním postupem. Můžete provést, prostřednictvím rozhraní API podle pokynů níže toku. Tyto kroky jsou z bodu z zobrazení vlastní rozhraní API:

1. Po přijetí žádosti okamžitě vrátíte na odpověď (před práci). Bude tato odpověď `202 ACCEPTED` odpověď, takže modul vědět jste získali data, přijal datové a teď zpracování. Odpověď na 202 by měl obsahovat záhlaví takto: 
 * `location`záhlaví (povinné): Toto je absolutní cesta k aplikacím logiky adresu URL můžete použít k prohlédnutí stav úlohy.
 * `retry-after`(volitelné, bude výchozí 20 pro akce). Toto je počet sekund, které modul by měl čekat, než dotazování adresy URL umístění záhlaví a zkontrolujte stav.

2. Při kontrole stavu úloh zkontrolovat následující položky: 
 * Pokud probíhá úlohy: vraťte se `200 OK` odpověď s informacemi odpověď.
 * Pokud se stále zpracování úlohy: vrátit jiný `202 ACCEPTED` odpověď s záhlaví stejné jako první odpovědi

Tento způsob umožňuje velmi dlouhý úlohy v vlákna svůj vlastní rozhraní API, ale zachovat aktivní připojení aktivní s modul logiky aplikace tak, aby neměl vypršení časového limitu nebo pokračovat před dokončením práce. Při přidávání to do aplikace pro použití logických operátorů, je důležité mít na paměti, že není třeba provádět v definici aplikace logiky pokračovat hlasování a zkontrolujte stav. Jakmile modul uvidí 202 PŘIJATÉ odpověď se záhlavím platné umístění, bude přijmout asynchronní vzorku a pokračovat hlasování umístění záhlaví, dokud není 202 je vrácena.

Uvidíte ukázka tento způsob GitHub [tady](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Webhook akce

Během pracovního postupu můžete mít aplikaci logiky pozastavit a počkejte "zpětné" pokračujte.  Tento zpětné je ve formuláři HTTP příspěvek.  K provedení tohoto vzorku, je třeba zadat dva koncové body na ovladači: přihlášení k odběru a odběr.

Na "přihlášení k odběru" bude aplikaci logiku vytvořit a zaregistrovat zpětné adresy URL, které mohou být uloženy vaše rozhraní API a zpětné systému připravených jako HTTP příspěvek.  Libovolný obsah/záhlaví bude předán do aplikace logiky a lze použít v zbývající pracovního postupu.  Modul logiky aplikace vyzve spuštění čárky přihlásit k odběru hned narazí tento krok.

Pokud uživatel ho zrušil spustit, modul logiky aplikace budou volání koncový bod "odběr.  Vaše rozhraní API můžete zrušte adresu URL zpětné podle potřeby.

Aktuálně návrháře logiky aplikace, které nejsou podporovány objevují ostatní webhook koncový bod prostřednictvím swagger, takže použít tento typ akce Přidat akci "Webhook" a zadejte adresu URL, záhlaví a textu žádosti o.  Můžete použít `@listCallbackUrl()` funkce pracovního postupu v některém z těchto polí v případě potřeby pro předávání v poli Adresa URL zpětné.

Uvidíte vzorek webhook vzorek v GitHub [tady](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>Aktivace

Kromě akce může mít vlastní rozhraní API aplikace act jako aktivační logiku aplikace.  Existují dva vzorky, které můžete sledovat pod aktivovat aplikaci použití logických operátorů:

### <a name="polling-triggers"></a>Aktivace hlasování

Aktivace hlasování chovají podobně jako výše dlouhé asynchronní spuštění akce.  Modul logiky aplikace zavolá koncový bod aktivační událost po uplynutí určitou dobu (závisí na SKU 15 pro Premium, 1 minuta standardu až 1 hodinu zdarma).

Pokud tam neexistuje žádná data, vrátí aktivační události `202 ACCEPTED` odpověď s `location` a `retry-after` záhlaví.  Však aktivačních událostí se doporučuje `location` záhlaví obsahuje dotazu parametr `triggerState`.  Toto je některé identifikátor API vědět, kdy posledního aplikaci logiky vyvolala.  Pokud je data k dispozici, vrátí aktivační události `200 OK` odpověď s obsahu datové.  Spustí aplikaci logiku.

Pokud například můžu byl dotazování vidět – Pokud bylo k dispozici souboru může vytvořit hlasování aktivační událost, která by postupujte takto:

* Pokud žádost o byla přijata s žádné triggerState rozhraní API vrátí `202 ACCEPTED` s `location` záhlaví, který má triggerState aktuální čas a `retry-after` 15.
* Pokud žádost o byla přijata s triggerState:
 * Zkontrolujte, pokud se po triggerState data a času přidala všechny soubory. 
  * Pokud 1 soubor se vrátíte `200 OK` odpověď se obsahu datové zvýšit triggerState na datový typ DateTime souboru vrátí a nastavte `retry-after` 15.
  * Pokud existuje více souborů, můžu současně s výsledkem 1 `200 OK`, zvýšit Moje triggerState v `location` záhlaví a nastavte si `retry-after` na hodnotu 0.  To vám umožní modul není k dispozici více dat a okamžitě ho bude požadovat na `location` zadáno záhlaví.
  * Pokud jsou k dispozici žádné soubory, vraťte se `202 ACCEPTED` odpověď a nechejte `location` triggerState stejné.  Nastavení `retry-after` 15.

Uvidíte vzorek aktivační hlasování v GitHub [tady](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Aktivace Webhook

Aktivace Webhook chovají podobně jako Webhook akce výše.  Modul logiky aplikace zavolá koncový bod "odběr kdykoli aktivační webhook přidali a uložit.  Vaše rozhraní API můžete zaregistrovat webhook adresu URL a volání přes číslo do příspěvku HTTP pokaždé, když data je k dispozici.  Záhlaví a obsahu datové bude předán do aplikace logiky spustit.

Aktivační webhook někdy odstranili (logiky aplikace zcela nebo jenom aktivační událost webhook), modul budou volání na adresu URL "odhlášení místo, kam můžete svůj rozhraní API unregister adresu URL zpětné a zastavit procesům podle potřeby.

Aktuálně návrháře logiky aplikace, které nejsou podporovány objevují ostatní aktivační webhook prostřednictvím swagger, takže použít tento typ akce se přidávají aktivační událost "Webhook" a zadejte adresu URL, záhlaví a textu žádosti o.  Můžete použít `@listCallbackUrl()` funkce pracovního postupu v některém z těchto polí v případě potřeby pro předávání v poli Adresa URL zpětné.

Uvidíte vzorek aktivační webhook v GitHub [tady](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)