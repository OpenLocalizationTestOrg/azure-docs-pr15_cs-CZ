<properties 
   pageTitle="Azure mobilní zapojení uživatelské rozhraní - Reach kritérium" 
   description="Naučte se používat kritéria nabízených kampaní odešlete vyberte část uživatelů pomocí zapojení Mobile Azure" 
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


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Použití kritéria nabízených kampaní odešlete vyberte část uživatelů

Zaměření na vaší cílovou skupinou podle specifických kritérií s tlačítkem "Nová kritéria" je jedním z nejvýkonnějších koncepty v Azure Mobile zapojení pomáhá odesíláte relevantní nabízená oznámení, že zákazníci odpoví místo odesílání všichni nevyžádané pošty. Můžete omezit posluchačů na základě standardní kritérií a simulovat posune a zjistit, kolik lidí, dostanou oznámení.

**Viz taky:**

- [Uživatelské rozhraní si přečtěte následující dokumentaci - Reach - nabízených kampaní][Link 27]

## <a name="audience-criteria-can-include"></a>Cílové skupiny kritéria lze zahrnout:
- **Technicals:** Je možné zacílit založené na stejné technické informace, zobrazí se v části sledování a technologie pro analýzu. **Viz taky:** [Uživatelské rozhraní si přečtěte následující dokumentaci - analýzy] [ Link 15], [Uživatelské rozhraní si přečtěte následující dokumentaci – sledování][Link 16]
- **Umístění:** Aplikace, které využívají "reálném čase umístění vytváření sestav" s Geo oddělení umožňuje Geo umístění jako kritéria obrázku cílové skupině představované GPS. "Pustí oblasti umístění zasílání zpráv o chybách" volání také použít jej směrovat cílové skupině představované mobilního telefonu ("reálné umístění generování sestav času" a "Pustí oblasti umístění vykazování" je nutné aktivovat z SDK). **Viz taky:** [SDK si přečtěte následující dokumentaci – iOS - integrace] [ Link 5], [SDK si přečtěte následující dokumentaci – Android – integrace][Link 5]
- **Dosáhla zpětné vazby:** Je možné zacílit posluchačů na základě jejich názorů předchozí oznámení reach prostřednictvím zpětné vazby reach z oznámení, hlasování a posune Data. To umožňuje lepší cílové vaší cílovou skupinou po dvě nebo tři dosáhla kampaní než první. Jej lze také k filtrování uživatelů, kteří už dostali oznámení s podobným obsahem a nastavením kampaně nechat zasílat uživatelům, kteří už dostali konkrétní předchozí kampaní. Můžete dokonce vyloučit uživatele kdo jsou zahrnuty konkrétní kampaní, která je stále aktivní příjem nové posune. **Viz taky:** [Uživatelské rozhraní si přečtěte následující dokumentaci - dosáhla - nabízená obsahu][Link 29]
- **Instalace sledování:** Můžete sledovat informace k různým nainstalovanou aplikaci uživatelů. **Viz taky:** [Uživatelské rozhraní si přečtěte následující dokumentaci – nastavení][Link 20]
- **Profilů uživatelů:** Je možné zacílit na základě standardní informace o uživatelích a můžete cílové podle informace vlastní aplikaci, kterou jste vytvořili. Platí to i uživatelé, kteří aktuálně přihlášeni a uživatelů, které jste odpověděli konkrétními otázkami, které jste požádáni, aby nastavit přímo v aplikaci místo jenom způsob jejich odpověděli předchozích kampaní. Všechny informace o aplikaci definované pro prezentaci aplikace nahoru v seznamu.
- Segmentů: Můžete také cílové podle segmenty, které jste vytvořili podle určitého uživatele chování obsahující více kritérií. Všechny segmenty definované aplikace neprojevila se v tomto seznamu. **Viz taky:** [Uživatelské rozhraní si přečtěte následující dokumentaci - segmentů][Link 18]
- **Aplikace informace:** Vlastní značky informace o aplikaci může vytvořit "Nastavení" ke sledování chování uživatele. **Viz taky:** [Uživatelské rozhraní si přečtěte následující dokumentaci – nastavení][Link 20]

## <a name="example"></a>Příklad: 
Pokud chcete nabízená oznámení jenom na dílčí sadu uživatelů, které jste provedli akci v aplikaci nákup.

1. Přejděte na stránku nastavení aplikace, vyberte nabídku "Informace App" a vyberte "Informace o nové aplikace"
2. Registrace novými informacemi Boolean aplikace s názvem "inAppPurchase"
3. Zkontrolujte nastavení tyto informace o aplikaci "true", když uživatel úspěšně provede v aplikaci nákup aplikace (pomocí sendAppInfo ("inAppPurchase",...) funkce)
4. Pokud nechcete provést z aplikace, můžete to udělat z vaší back-end pomocí zařízení rozhraní API)
5. Pak stačí vytvořit oznámení, s kritériem omezení vaší cílovou skupinou uživatelích, kteří mají "inAppPurchase" nastavena na "hodnotu true")
 
> Poznámka: Zacílení na základě kritérií než značky informace o aplikaci vyžaduje Azure Mobile zapojení shromáždění informací ze zařízení vašich uživatelů, před odesláním push a může způsobovat zpoždění. Konfigurace složité nabízených možností (jako jsou aktualizace odznáčky) můžete taky zpoždění posune. Použití "jeden snímek" kampaně z rozhraní API nabízených je absolutní nejrychlejší způsob nabízená v Azure Mobile Engagement. Použití pouze značky informace o aplikaci jako kritérií nabízených Reach kampaně (buď z rozhraní API dosáhla nebo uživatelské rozhraní) je další nejrychlejší způsob od značky informace o aplikaci se ukládají na straně serveru. Pomocí dalších kritéria pro kampaně nabízená je metoda nabízená flexibilní ale nejnižší, protože Azure Mobile zapojení má k vytvoření dotazu zařízení mohli poslat kampaně.
 
![Reach Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Možnosti kritérium platí pro:
- **Technicals**     
- Firmware název: název firmwaru
- Firmware verze: verze firmwaru
- Model zařízení: modelu zařízení
- Výrobci zařízení: výrobce zařízení
- Verze aplikace: verze aplikace
- Carrier název: název Carrier Nedefinováno
- Země carrier: Carrier země, Nedefinováno
- Typ sítě: typ sítě
- Národní prostředí: národního prostředí
- Velikost obrazovky: velikost obrazovky
- **Umístění**      
- Poslední známé oblast: místo země, oblasti,
- Geo oddělení reálném čase: seznam z POIs (jméno, akce), cyklický SMĚŘUJE (jméno, šířky, délky, Radius v metry)
- **Dosažení zpětné vazby**     
- Oznámení názory: oznámení, zpětné vazby
- Dotazování zpětné vazby: hlasování, zpětné vazby
- Dotazování odpovědí názory: hlasování názory odpovědí, otázku, volba
- Data nabízených názory: Data nabízených zpětné vazby
- **Instalace sledování**     
- Úložiště: Úložiště Nedefinováno
- Zdroj: Zdroj Nedefinováno
- **Profilů uživatelů**     
- Gender: Muž a žena, Nedefinováno
- Datum narození: operátor, datum, Nedefinováno
- Určovat, jestli se změnami: PRAVDA nebo NEPRAVDA, Nedefinováno
- **Informace o aplikaci**      
- Řetězec: Řetězec Nedefinováno
- Data: operátor, datum, Nedefinováno
- Celé číslo: operátor, číslo, Nedefinováno
- Logická hodnota: PRAVDA nebo NEPRAVDA, Nedefinováno
- **Segmentu**    
- Název segmentů (z rozevíracího seznamu), vyloučení (cílových uživatelů, které nejsou součástí tohoto segmentu).

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
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
 
