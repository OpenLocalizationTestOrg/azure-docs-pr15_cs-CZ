<properties 
   pageTitle="Azure mobilní zapojení průvodce - rozhraní API pro řešení" 
   description="Příručky pro Azure mobilní zasunutí - rozhraní API pro řešení problémů" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="10/04/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-api-issues"></a>Příručka pro řešení potíží pro rozhraní API problémy

Následují možné problémy, ke kterým může dojít s interakce správci s Azure Mobile zapojení prostřednictvím rozhraní API.

## <a name="syntax-issues"></a>Syntaxe problémy

### <a name="issue"></a>Problém
- Chyby syntaxe pomocí rozhraní API (nebo neočekávané chování).

### <a name="causes"></a>Příčiny

- Syntaxe problémy:
    - Projděte syntaxe konkrétní rozhraní API používáte k potvrzení, že možnost je k dispozici.
    - Běžné problémy s použití rozhraní API je si splést rozhraní API dosáhla a rozhraní API nabízená (většinu úkolů souvisejících se provádí pomocí rozhraní API dosáhla místo rozhraní API nabízená). 
    - Jiné běžné problémy s SDK integrace a použití rozhraní API je si splést SDK klávesy a rozhraní API.
    - Skripty, kteří se připojují k rozhraní API muset poslat data minimálně každých 10 minut nebo připojení se časový limit (časté zejména v rozhraní API monitoru skripty naslouchají dat). Nechcete, aby časový limit, máte skript odeslat typu XMPP ping každých 10 minut uchovat relaci aktivní se serverem.

### <a name="see-also"></a>Viz taky
 
- [Rozhraní API si přečtěte následující dokumentaci][Link 4]
- [Informace o typu XMPP Protocol (protokol)]( http://xmpp.org/extensions/xep-0199.html)
 
## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a>Nelze použít rozhraní API provádět k dispozici v uživatelském rozhraní Azure Mobile zapojení stejné akce

### <a name="issue"></a>Problém
- Akce, které funguje v uživatelském rozhraní Azure Mobile zapojení nefunguje z související API zapojení Mobile Azure.

### <a name="causes"></a>Příčiny

- Potvrzení, že můžete provádět stejné akce uživatelského rozhraní Azure Mobile zapojení ukazuje, že jste správně integrovaný tuto funkci Azure Mobile zapojení s SDK.

### <a name="see-also"></a>Viz taky
 
- [Přečtěte následující dokumentaci pro uživatelské rozhraní][Link 1]
 
## <a name="error-messages"></a>Chybové zprávy

### <a name="issue"></a>Problém
- Kódy chyb pomocí rozhraní API zobrazí při spuštění nebo v protokoly.

### <a name="causes"></a>Příčiny

- Tady je složené seznam běžných rozhraní API stav kódy čísla odkaz a předběžné Poradce při potížích:

        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a>Viz taky

- [Rozhraní API si přečtěte následující dokumentaci - podrobné chyby na každý konkrétní rozhraní API][Link 4]
 
## <a name="silent-failures"></a>Pasivní k chybám

### <a name="issue"></a>Problém
- Rozhraní API akce se nezdaří se žádná chybová zpráva zobrazí při spuštění nebo v protokoly.

### <a name="causes"></a>Příčiny

- Více položek přestane být platná v uživatelském rozhraní Azure Mobile zapojení Pokud nejsou integrována správně, se nepovede tiše z rozhraní API, ale takže nezapomeňte test stejnou funkci z uživatelského rozhraní pro vás, pokud to funguje.
- Azure zapojení Mobile a mnoho rozšířené funkce Azure Mobile zapojení se pokoušíte použít, musíte být jednotlivě integrována do aplikace sady SDK jako samostatných kroků než budete moct použít.

### <a name="see-also"></a>Viz taky

- [Poradce při potížích s Průvodce - SDK][Link 25]
 
<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
