<properties 
   pageTitle="Azure mobilní zapojení Poradce při potížích s Průvodce - SDK" 
   description="SDK integrace řešení problémů s v Azure Mobile zapojení" 
   services="mobile-engagement" 
   documentationCenter="" 
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

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Příručka pro řešení potíží pro problémy s integrací SDK

Následují možné problémy, ke kterým může dojít s jak integruje Azure Mobile zapojení do aplikace.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>SDK chyb selhání v jiné oblasti aplikace

### <a name="issue"></a>Problém
- Uživatelské rozhraní dat kolekce selhala (analýzy, sledování, segmentace nebo řídicí panely).
- Nabízená selhání (posune nefungují v aplikaci mimo aplikaci nebo obojí).
- Pokročilé funkce selhání (sledování zeměpisná poloha a platformě konkrétní posune nefungují).
- Rozhraní API dojde k selhání (rozhraní API selhání často tiše bez chybové zprávy).
- Chyby služby (žádná z Azure Mobile zapojení vyhovovat aplikace).

### <a name="causes"></a>Příčiny

- Většina problémy, které se musí vyřešit s SDK Azure Mobile zapojení budou zjištěny selhání v aplikaci (například selhání kolekce dat uživatelského rozhraní, nabízených selhání, selhání pokročilé funkce, selhání rozhraní API, selhání aplikace nebo výpadku zřejmé služby).  
- Pokud určité funkce přijetí Mobile Azure pracoval nikdy v aplikaci před, musíte dokončit integrace. 
- Pokud určité funkce přijetí Mobile Azure nacházela a zastavit, budete muset upgradovat na poslední verzi s Azure Mobile Engagement SDK. Myslete na to, že je jinou verzi Azure Mobile zapojení SDK pro každou platformu nepodporuje Azure Mobile zapojení (Android iOS, Windows a Windows Phone).

#### <a name="sdk-integration"></a>Integrace SDK

- Azure zapojení Mobile integrované není správně SDK (analýzy).
- Kontaktujte, není správně integrované v SDK (v aplikaci a mimo aplikaci posune).
- Certificate vypršela platnost nebo jsou nesprávná výrobní porovnání vývojáře (pouze v iOS).
- GCM nebo ADM integrované není správně v SDK (Android pouze - posune služby konkrétní).
- Sledování není správně integrované v SDK (sledování úložiště instalovat).
- Pustí umístění nebo GPS integrované není správně v SDK (cílení podle umístění geo).


**Viz taky:**

- [SDK si přečtěte následující dokumentaci - integrace vodítka][Link 5] 
- [Poradce při potížích s Průvodce - připínáčku][Link 23]

#### <a name="sdk-upgrade"></a>SDK Upgrade

- Muset upgradovat SDK k řešení problémů se staršími verzemi SDK (často Příbuzná novějších verzích zařízení OS).
- Odinstalujte všechny předchozí verze aplikace ze svého zařízení a znova nainstalujte nejnovější verzi aplikace znovu zaregistrujte svoje zařízení ID v Azure Mobile zapojení uživatelském rozhraní potvrďte, že zařízení používá nejnovější verzi aplikace.

**Viz taky:**

- [SDK si přečtěte následující dokumentaci - poznámky k verzi](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [SDK si přečtěte následující dokumentaci - upgradu vodítka](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>SDK ostatní

- Chyby "AndroidManifest.xml" manifest aplikace mohou způsobit Azure Mobile zapojení nefunguje (pouze Android).
- Běžné problémy s SDK integrace a použití rozhraní API je si splést SDK klávesy a rozhraní API.

**Viz taky:**

- [Principy – Glosář][Link 6]

## <a name="advanced-coding-issues"></a>Rozšířené možnosti kódování problémy

### <a name="issue"></a>Problém
-  Platformu konkrétní kód přímo související s Azure Mobile zapojení mohou způsobit problémy na iOS, Android a Windows Phone.

### <a name="causes"></a>Příčiny

- V mnoha Upřesnit kódování problémy s Azure Mobile zapojení příčinou nesprávně ručně psaných platformy konkrétní kód přímo související s Azure Mobile Engagement. Je třeba v dokumentaci k specifické pro platformu, který vyvíjíte pro kromě Azure Mobile zapojení dokumentace (Android iOS, Web, Windows a Windows Phone).
- Konfigurace není správně "kategorie", zabrání propojení z upozornění na jiné místo uvnitř a mimo aplikaci (pouze Android). 
- Nastavení "UIKit.framework" "volitelné" v iOS kódu, zobrazuje "Symbolem nebyl nalezen chyba" nebo dojde k chybě na starších zařízení s iOS (pouze v iOS).
- Certifikáty, kterým vypršela platnost nebo pomocí nesprávně zařízením nebo výrobní verze certifikátu příčiny nabízených problémy (pouze v iOS).
- Inherentní platformu Azure Mobile zapojení nemůže ovládat (například k centru systém fungování pro odhlášení z aplikace v iOS a Android posune) platí některá omezení:
- Azure zapojení Mobile publikuje úplný seznam interní balíčků používaný Azure Mobile zapojení pro iOS a Android kdykoliv při ruce. Mějte na paměti, že některé funkce Azure Mobile zapojení nejsou specifické pro platformu (Android iOS, Web, Windows a Windows Phone).

### <a name="see-also"></a>Viz taky

 - [Poradce při potížích s Průvodce - připínáčku][Link 23] 
 - [SDK si přečtěte následující dokumentaci - poznámky k verzi][Link 5]
 - [SDK si přečtěte následující dokumentaci - upgradu vodítka][Link 5]

## <a name="application-crashes"></a>Zhroucení aplikace

### <a name="issue"></a>Problém
- Na zařízení koncoví uživatelé způsobí zhroucení aplikace.

### <a name="causes"></a>Příčiny

- Informacemi o chybách můžete zobrazit v *Uživatelské rozhraní analýzy* nebo rozhraní *API analýzy*
- Můžete najít ID zařízení test zařízení a stejné kroky, který způsobil aplikace pád pro koncového uživatele k identifikaci příčinu váš stav.
- Známé problémy s SDK zapojení Mobile Azure, které způsobují aplikací chybu upgradu na nejnovější verzi sady SDK někdy vyřešit. Projděte poznámky o svoji platformu při zjišťování chyb.

### <a name="see-also"></a>Viz taky

- [SDK si přečtěte následující dokumentaci - poznámky k verzi][Link 5]
- [SDK si přečtěte následující dokumentaci - upgradu vodítka][Link 5]

## <a name="app-store-upload-failures"></a>Selhání odeslání úložiště přihlašovacích údajů aplikace

### <a name="issue"></a>Problém
- Chyb souvisejících s nahrávání nejnovější verzi aplikace Apple, Google nebo aplikace pro Windows store.

### <a name="causes"></a>Příčiny

- Aplikace ukládá někdy blok aplikace s některé funkce povolené (jako je Apple Storu brání použití IDFV v aplikací ve storu a úložišti GooglePlay zabrání sdílení aplikace informací mezi aplikacemi). 
- Ujistěte se, pokud se vám nedaří odeslání aplikace do úložiště zaškrtněte poznámky k verzi svých platformy a aktuální verzi sady SDK.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
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
 
