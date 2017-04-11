<properties
    pageTitle="Volba mezi toku, aplikace použití logických operátorů, funkce a WebJobs | Microsoft Azure"
    description="Porovnávat cloudu integrace služby od Microsoftu a rozhodnout, jaký služeb byste měli použít."
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="Microsoft toku, toku, aplikace použití logických operátorů, azure funkcí, mezi které funkcí, mezi které azure webjobs webjobs, události, dynamické výpočetním bez serveru architektuře zpracování"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Volba mezi toku, logiku aplikace, funkce a WebJobs

Tento článek porovnává a porovnání následující služby v cloudu společnosti Microsoft, které můžete všechny řešit problémy integrace a automatizaci obchodních procesů:

- [Microsoft toku](https://flow.microsoft.com/)
- [Použití logických operátorů Azure aplikace](https://azure.microsoft.com/services/logic-apps/)
- [Funkce Azure](https://azure.microsoft.com/services/functions/)
- [WebJobs Azure aplikace služby](../app-service-web/web-sites-create-web-jobs.md)

Tyto služby se hodí "připevnit" pohromadě různými systémy. Můžete všechny definují vstupní, akce, podmínky a výstupu. Každý z nich můžete spustit na plánu nebo aktivační událost. Však každé služby přidá jedinečnou sadu hodnot a jejich porovnání není dotaz "službu nejvhodnějšího?" ale nějaká "službu nejlépe vyhovuje pro tuto situaci?" Kombinace z těchto služeb často, je nejlepší způsob, jak rychle vytvořit scalable, úplné doporučené integrace řešení.

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>Tok porovnání logiku aplikace

Můžete probereme tato Flow Microsoft a Azure logiky aplikace společně protože jsou oba *Konfigurace první* integrace služby, které umožňuje snadno vytvářet procesy a pracovní postupy a integrace s různé SaaS a enterprise aplikace. 

- Tok postavená logiky aplikace
- Mají stejné Návrháře pracovního postupu
- [Spojnice](../connectors/apis-list.md) práci v jednom můžete taky spolupracovat ve druhém

Toky umožňuje každý pracovník office provádět jednoduché integrace (například získat služby SMS důležité e-maily) bez opakovaného vývojáři nebo IT. Použití logických operátorů aplikace na druhé straně můžete povolit upřesňující nebo kritické integrace (například B2B procesů), které jsou potřeba úrovni organizace DevOps a zabezpečení postupy. Je typické pro firmy pracovní postup nárůstu složitost přesčasová. Podle toho můžete začít toku nejdřív a potom podle potřeby ji převést logiky aplikace.

V následující tabulce umožňuje určit, zda je nejvhodnější pro dané integrace toku nebo použití logických operátorů aplikace.

|               | Tok                                                                             | Použití logických operátorů aplikace                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Cílové skupiny      | Office zaměstnanců, podnikoví uživatelé                                                   | V oboru IT, vývojáři                                                                                 |
| Scénáře     | Samoobslužné                                                                     | Důležité                                                                                    |
| Vývojového nástroje   | V prohlížeče pouze uživatelského rozhraní                                                              | V prohlížeči a [Visual Studia](../app-service/logic/app-service-logic-deploy-from-vs.md) [zobrazení Kód](../app-service-logic/app-service-logic-author-definitions.md) k dispozici |
| DevOps        | Ad hoc vývoj ve výrobním                                                    | ovládací prvek zdroje, testování, podpora a automatizace a správy [zdrojů](../app-service-logic/app-service-logic-arm-provision.md) správy Azure|
| Možnosti správy| [https://Flow.microsoft.com](https://flow.microsoft.com)                       | [https://Portal.Azure.com](https://portal.azure.com)                                                |
| Zabezpečení      | Standardní postupy: [zůstala zachovaná svrchovanost data](https://wikipedia.org/wiki/Technological_Sovereignty), [šifrování na zbývající](https://wikipedia.org/wiki/Data_at_rest#Encryption) citlivá data, atd. | Zabezpečení assurance Azure: [Azure zabezpečení](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Centrum zabezpečení](https://azure.microsoft.com/services/security-center/), [Auditovat protokoly](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)a další. |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>Funkce porovnání WebJobs

Můžou diskutovat Azure funkcí a WebJobs aplikace služby Azure společně protože jsou obě integrace služby *kód prvního* jsme sice pro vývojáře. Umožňují v odpověď na různé události, jako je třeba [Nové úložiště objektů BLOB](functions-bindings-storage.md) nebo [WebHook požádat o](functions-bindings-http-webhook.md)spustit skript nebo kód. Tady je jejich podobnosti: 

- Jsou sestaveny v [Aplikaci služby Azure](../app-service/app-service-value-prop-what-is.md) i využívat funkce, jako jsou [zdrojového](../app-service-web/app-service-continuous-deployment.md) [ověřování](../app-service/app-service-authentication-overview.md)a [Sledování](../app-service-web/web-sites-monitor.md).
- Jsou obě zaměřené vývojář služby.
- Podpora standardní skriptování i jazyky.
- Mají NuGet a NPM podpory.

Funkce je přirozený vývoj WebJobs při trvá nejlepších informací o WebJobs a vylepšuje je. Vylepšení patří: 

- Zjednodušené vývoj, otestujte a spusťte kódu přímo v prohlížeči.
- Předdefinované integrace s více Azure služby a služby 3rd strany, jako třeba [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
- Mzdy použití, není potřeba zaplatit Pomocník pro [Plánování aplikaci služby](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
- Automatické, [dynamické měřítka](functions-scale.md).
- U stávajících zákazníků služby aplikace spuštěné v aplikaci služby plán ještě možné (využít-využít zdrojů).
- Integrace s aplikacemi jiných logiky.

Následující tabulka shrnuje rozdíly mezi funkcemi a WebJobs:

|                        | Funkce                                                                                                                                                                | WebJobs                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Změna měřítka                | Configurationless měřítka                                                                                                                                                | Změna velikosti s plán služeb aplikací        |
| Ceny                | Mzdy na použití nebo jeho část plán služeb aplikací                                                                                                                                  | Součástí plánu aplikace služby           |
| Spustit typ               | spouštěný, naplánoval (aktivační událost časovače)                                                                                                                                  | spouštěné, nepřetržitý, naplánované   |
| Aktivační události         | [timer](functions-bindings-timer.md), [Azure DocumentDB](functions-bindings-documentdb.md) [Rozbočovače události Azure](functions-bindings-event-hubs), [Protokolu HTTP/WebHook (GitHub, časová rezerva)](functions-bindings-http-webhook.md), [Aplikací Mobile aplikace služby Azure](functions-bindings-mobile-apps.md), [Rozbočovače oznámení Azure](functions-bindings-notification-hubs.md), [Bus služby Azure](functions-bindings-service-bus.md), [Úložišti Azure](articles/functions-bindings-storage.md) | [Azure úložiště](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) [služby Azure Bus](websites-dotnet-webjobs-sdk-service-bus.md)         |
| Vývojové v prohlížeči | x                                                                                                                                                                        |                                    |
| Okno skriptování       | pokusné                                                                                                                                                             | x                                  |
| Prostředí PowerShell             | pokusné                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| Flám                   | pokusné                                                                                                                                                             | x                                  |
| PHP                    | pokusných                                                                                                                                                             | x                                  |
| Python                 | pokusné                                                                                                                                                             | x                                  |
| JavaScript             | x                                                                                                                                                                        | x                                  |
| Java                   | pokusné                                                                                                                                                             | x                                  |

Použití funkce nebo WebJobs nakonec závisí na tom co už děláte s aplikaci služby. Pokud máte aplikaci pro aplikaci služby, pro kterou chcete spustit fragmenty kódu a chcete spravovat společně ve stejném DevOps prostředí, byste měli použít WebJobs. Pokud chcete spustit fragmenty kódu pro další služby Azure nebo aplikací i 3rd výrobců nebo pokud chcete spravovat fragmenty kódu integrace odděleně od aplikací aplikaci služby nebo pokud chcete zavolat fragmenty kódu z app použití logických operátorů, můžete měli využívat všechna vylepšení ve funkcích.  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>Tok, logiku aplikace a funkce spolupráce

Jako výše uvedené které služba je nejvhodnější pro vás závisí na vaší situaci. 

- Jednoduchý organizační optimalizace pak používají.
- Pokud scénářů integrace příliš rozšířené možnosti pro tok nebo potřebujete DevOps funkcí a compliances zabezpečení, pomocí logiky aplikace.
- Pokud krok nefunguje integrace vyžaduje vysoce vlastní transformace nebo speciální kód, zapište funkce aplikace a aktivovat funkci akce v aplikaci logiku.

Volání v aplikaci logiku v toku. Můžete taky zavoláte funkce v aplikaci logiky a aplikace použití logických operátorů ve funkci. Integrace mezi toku, použití logických operátorů aplikací a funkcí dál zlepšit přesčasovou práci. Můžete vytvořit něco v jedné služby a použít v jiných služeb. Proto je vhodné investice, které provedete tyto tři technologie.

## <a name="next-steps"></a>Další kroky

Začínáme s každým z těchto služeb vytvořením vaše první toku, logiku aplikace, funkce aplikace nebo WebJob. Klikněte na některou z následujících odkazů:

- [Začínáme s Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [Vytvoření aplikace logiku](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Vytvoření první funkce Azure](../azure-functions/functions-create-first-azure-function.md)
- [Nasazení WebJobs pomocí aplikace Visual Studio](../app-service-web/websites-dotnet-deploy-webjobs.md)

Nebo získat další informace o těchto službách integrace s následující odkazy:

- [Využívání Azure funkce a služba Azure aplikací pro scénáře integrace tak, že Kryštofem Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Integrace Charlese Lamanna, které udělali jednoduché](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Použití logických operátorů aplikace Live webové vysílání](http://aka.ms/logicappslive)
- [Microsoft tok časté otázky](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Azure WebJobs si přečtěte následující dokumentaci zdroje](../app-service-web/websites-webjobs-resources.md)
