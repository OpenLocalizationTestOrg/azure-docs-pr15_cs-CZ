<properties 
   pageTitle="Azure mobilní zapojení uživatelské rozhraní - Reach postup"
   description="Přehled uživatelského rozhraní pro Azure mobilní zapojení" 
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

# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Jak začít používání a správa posune kontaktujte koncovým uživatelům

Jakmile v SDK je plně integrovaný aplikace, mohli rovnou začít používat části Reach uživatelského rozhraní pro nabízených oznámení pro uživatele aplikace.  

## <a name="do-your-first-push-notification-campaign"></a>Proveďte první kampaně nabízená oznámení
-    Potvrďte, že vaše Reach je integrována do aplikace sady SDK. 
-    Vyberte aplikaci
 
![First1][1]

-    Přejděte oddíl "Reach" a klikněte na "oznámení narození"
 
![First2][2]

-    Vytvoření nové kampaně a nazvěte ji
 
 ![First3][3]

-    Vyberte, jak by měl doručeno oznámení, jako v aplikace pouze
 
![First4][4]

-    Vytvořte zprávu, kterou chcete posunout
 
![First5][5]

-    Můžete zadat název na oznámení (volitelné).
-    Zapsat obsah zprávy připínáčku.
-    Můžete nahrát obrázek. Mějte na paměti, že velikost souboru nesmí překročit 32 768 bajtů.
-    Máte taky možnost vybrat další možnosti, ale pro výběr tento kurz se vidíme, později.

-    Vyberte typ obsahu, jako oznámení pouze
 
![First6][6]

-    Vytvoření kampaně nabízených a se zobrazí v seznamu kampaní.
 
![First7][7]

## <a name="test-your-push-notification-campaign"></a>Testování kampaně nabízená oznámení
![Test1][8]

-    Zaregistrujte zařízení.
-    Klikněte na zaškrtávací políčko zařízení, které chcete posunout.
-    Klikněte na tlačítko "Test" stisknutí odešlete zařízení.
 
![Test2][9]

-    Aktivace kampaň
 
![Test3][10]

-    Teď, když jste vytvořili kampaně stačí ho pro oznámení posune uživatelům aktivujete.
 
## <a name="send-personalized-pushes"></a>Odesílání přizpůsobených posune
-    Tento příklad vytvoří nabízených místo, kam se zadává vlastní slevě kód do nabízená oznámení.
 
![Personalize1][11]

Přizpůsobení funguje nahrazením z značku informace o aplikaci značku tak, že Ano, budete muset zkontrolujte, jestli že má uživatel správnou aplikaci info definován jako první. V tomto příkladu cílových uživatelé budou mít značku informace o aplikaci s názvem rebate_code definované.
Jak se zobrazí nad obsah nabízená oznámení zahrnuje značku ${rebate_code}, což se označuje, že ho nahrazuje skutečný obsah značku informace o aplikaci.

> Upozornění: Pokud značku informace o aplikaci není definované pro uživatele, nebudou uživatel push.

-    Výsledek
 
![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>Můžete upravit text svého oznámení
![Personalize3][13]

-    Včetně názvu oznámení
-    A obsah zprávy.
-    Zvolte typ oznámení (zobrazení textu nebo zobrazení webové stránky)
 
![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>Část oznámení mohou také přizpůsobit:
-    Adresa URL akce by měl chcete upravit cílovou stránku
-    Název,
-    Text zprávy.
 
 
## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Rozlišení Your nabízená oznámení (nebo oddálit aplikace)
-    Zvolte typ oznámení nabízená, vyberte aplikaci, přejděte k části "Dosáhla", vyberte nebo vytvoříte nabízených kampaní a přejděte k části "Oznámení".
 
-    Klikněte na "režim doručení" mají.
-    Kliknutí na zaškrtávací políčko "Omezit aktivity" Chcete oznámení dojde na určité činnosti (obrazovky).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Mimo aplikaci jenom" režim doručení
![Differentiate2][16]

"Mimo aplikaci jenom" doručení režim poskytuje nabízená oznámení, když máte zavřený aplikace. Toto je standardní nabízená oznámení.
Když vyberete "mimo aplikaci jenom", je potřeba již zadali certifikáty z platformy, který vytváří aplikace na (APNS nebo GCM).

### <a name="see-also"></a>Viz taky
-  [Apple služba nabízených oznámení – certifikáty](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), Google Cloud zasílání zpráv – certifikát] (http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"v aplikace jenom" doručení režimu
![Differentiate3][17]

"V-aplikace jenom" doručení režim poskytuje nabízená oznámení při spuštění aplikace.
Pro toto oznámení se nemusí projít APNS a GCM systému.
Kontaktovat svého koncovým uživatelům můžete systém doručování v aplikaci.
Plně můžete přizpůsobit oznámení a rozhodnout, ve kterém se zobrazí aktivity (obrazovky) oznámení.

### <a name="anytime-delivery-mode"></a>"Kdykoli" doručení režimu
Můžete zvolit režimu doručení "Kdykoli" a zajistíte, abyste dosáhla koncovému uživateli, jestli je aplikace spuštěna nebo ne.
Když vyberete "Kdykoli", je potřeba již zadali certifikáty z platformy, který vytváří aplikace na (APNS nebo GCM). 
 
## <a name="schedule-a-push-campaign"></a>Naplánování nabízených kampaň
### <a name="plan-to-start-a-campaign"></a>Plánování spuštění kampaň
![Shedule1][18]

Je 21 z března a máte oznámení provádět a plánované pro 22 z března půlnoci. Nemáte dodržovat předpisy před rozhraní push! Můžou plánovat předem přesné minutu, kterou oznámení odešle.
-    Zrušením zaškrtnutí políčka "Žádná" zaškrtávacího políčka a vyberte čas zahájení 
-    Vyberte datum a čas, budete chtít spustit kampaně připínáčku.

### <a name="plan-to-end-a-campaign"></a>Plánování ukončit kampaň
![Shedule2][19]

Chcete kampaně ukončíte na 25 z března na 3. hodině pm, ale víte, že tam nebude na to.
Nemáte dodržovat předpisy před rozhraní pro nabízená! Můžou plánovat předem přesné minutu, kterou kampaně se zastaví.
-    Klikněte na "Žádná" zaškrtávacího políčka nebo vyberte koncový čas
-    Vyberte datum a čas má funkce dokončit kampaně připínáčku.

### <a name="end-a-campaign-manually"></a>Ruční konec kampaň
![Shedule3][20]

Ve výchozím nastavení "" zaškrtávací políčka jsou vybranou položkou žádná.
To znamená, že kampaně se spustí hned, jak aktivovat v části přístup a skončit při přestane v části reach.
 
> Poznámka: Kampaní vytvořená bez koncové datum ukládání stisknutí místně na zařízení a jeho vyjádření při příštím otevření aplikace i v případě kampaně ručně ukončen.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Vylepšení nabízená oznámení s zobrazení textu
### <a name="what-is-a-text-view"></a>Co je zobrazení textu?
![TextView1][21]

Zobrazení textu se automaticky otevírané okno s obsahem text. Tento automaticky otevírané okno se zobrazí po klepnutí koncový uživatel nabízená oznámení o.
Zobrazení textu umožňuje prezentovat další obsah koncovému uživateli. Toto je také příležitost k zobrazení hovoru na akci například přechod na stránku aplikace přesměrování na úložiště, otevření webové stránky, odeslání e-mailu od lokalizované geo hledání, atd.

### <a name="example-text-view"></a>Příklad: Zobrazení textu
-    V části "Reach" vytvářet kampaně nabízená oznámení a pojmenujte kampaň
 
![TextView2][22]

-    Napište zprávu, která se zobrazí na oznámení.
-    Vyberte typ obsahu oznámení "text"
 
![TextView3][23]

> Poznámka: po stisknutí zobrazení textu, je vždy je součástí oznámení nejdřív. 

- Definování text (po výběru textového oznámení obsahu dílčí část zobrazí ostatním uživatelům umožňuje definovat text, který chcete zobrazovat.)
 
![TextView4][24]

-    Napište název, který bude zobrazen v horní části zprávy.
-    Napište hlavním obsahem zobrazení textu.
-    Zapište obsah, který se zobrazí na tlačítko akce (tlačítko akce umožňuje aplikaci provést určité akce například otevřením stránky aplikací, přesměrování na App store nebo jakýkoli druh zdroje, které můžete přidat).
-    Zapsat obsah, který se zobrazí na tlačítko Ukončit (kliknutím na tlačítko Ukončit zobrazení textu by měl zmizet text.)
 
-    Vytvoření kampaně nabízená oznámení a zobrazí se v seznamu kampaně.
 
![TextView5][25]

-    Aktivace kampaně nabízená oznámení o odeslání zobrazení textu uživatelům.
 
![TextView6][26]

-    Výsledek
 
![TextView7][27]

-    Uživatel dostane oznámení a klepněte na něj.
-    Zobrazení textu se zobrazí jako automaticky otevírané okno umožňující uživatelům pracovat s ním.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Vylepšení nabízená oznámení s webovém zobrazení
### <a name="what-is-a-web-view"></a>Co je zobrazení webového?
![WebView1][28]

Zobrazení webového je automaticky otevírané okno s obsahem webu. Tento automaticky otevírané okno se zobrazí, když koncový uživatel klikl nabízená oznámení o.
Zobrazení webového umožňuje mít další interakce s koncovým uživatelem.
Toto je také příležitost k zobrazení hovoru na akci jako je přesměrování na App Store otevření webové stránky, odeslání e-mailu, spuštění lokalizované geo hledání atd …

### <a name="example-web-view"></a>Příklad: Webovém zobrazení
-    Vytvoření kampaně nabízená v části "Reach" a kampaně pojmenovat.
 
![WebView2][29]

-    Napište zprávu, která se zobrazí na oznámení.
-    Vyberte typ obsahu, oznámení jako "web"
 
![WebView3][30]

### <a name="about-announcement-types"></a>O typech oznámení:
- Pouze oznámení: je jednoduchý standardní oznámení. Znamená to, že pokud uživatel klikne na něm, žádné další zobrazení se zobrazí, ale pouze akce přidružené k němu dojde.
- Text oznámení: je upozornění, které provede uživatel měl si téma zobrazení textu.
- Web oznámení: je upozornění, které provede uživatel měl si téma zobrazení webového.
Vyberte obsah "Web oznámení".

> Poznámka: Po stisknutí zobrazení webového, je vždy je součástí oznámení nejdřív.

- Definování webového obsahu (po výběru oznámení webový obsah dílčí části se zobrazí, což umožňuje definovat zobrazení obsahu webu, který chcete zobrazovat.)

 
![WebView4][31]

-    Napište název, který bude zobrazen v horní části zprávy (volitelné).
-    Zadejte kód HTML, tady.
-    Klikněte na úpravy režimu s cílem přepnout edition a zjistěte, jak to vypadá zdrojového.
-    Zapište obsah, který se zobrazí na tlačítko akce (tlačítko akce umožňuje aplikaci provést určité akce například otevřením stránky aplikací, přesměrování úložiště nebo jakýkoli druh zdroje, které můžete přidat).
-    Zapsat obsah, který se zobrazí na tlačítko Ukončit (kliknutím na tlačítko Ukončit zobrazení webového by měl zmizet text).
 
-    Výsledek
 
![WebView5][32]

-    Uživatel upozornění a klikněte na ni.
-    Zobrazení textu se zobrazí jako automaticky otevírané okno umožňující uživatelům pracovat s ním.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
