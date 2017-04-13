<properties 
   pageTitle="Azure mobilní zapojení uživatelské rozhraní - Reach obsahu" 
   description="Naučte se spravovat jedinečný obsah různých typů nabízená oznámení kampaní v Azure Mobile zapojení" 
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

# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>Jak spravovat jedinečný obsah různých typů kampaní nabízená oznámení
 
Část kampaní reach obsahu umožňuje upravovat obsah oznámení, hlasování, posune dat, a dlaždice (pouze pro Windows Phone). Nastavení obsahu nabízených kampaní si specifické pro typu kampaně. 
 
### <a name="content-types"></a>Typy obsahu:
- Oznámení
- Hlasování
- Posune dat
- Dlaždice (pouze pro Windows Phone)
 
## <a name="content-of-announcements"></a>Obsah oznámení
 ![Reach Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Zvolte typ oznámení:
-    Pouze oznámení: je jednoduchý standardní oznámení. Znamená to, že pokud uživatel klikne na něm, žádné další zobrazení se zobrazí, ale pouze akce přidružené k němu dojde.
-    Text oznámení: je upozornění, které provede uživatel měl si téma zobrazení textu.
-    Web oznámení: je upozornění, které provede uživatel měl si téma zobrazení webového.

### <a name="see-also"></a>Viz taky
- [Dosažení – jak jejich – oznámení][Link 3] 

### <a name="about-web-view-announcements"></a>Informace o webové zobrazení oznámení:
Výskyty průběh "{deviceid}" kód HTML nebo kód v JavaScriptu, který tady zadáte automaticky nahradí identifikátor zařízení zobrazování oznámení. Toto je snadný způsob, jak načíst zapojení Azure mobilní zařízení identifikátory v externí webové služby hostované na zadní office.
Pokud chcete vytvořit zobrazení přes celou obrazovku web (bez výchozí akce a ukončení tlačítka připravili jsme pro) můžete použít následující funkce z kód v JavaScriptu oznámení váš web zobrazení: 

-    provedení akce oznámení: ReachContent.actionContent()
-    opuštění oznámení: ReachContent.exitContent()
 
### <a name="choose-your-action"></a>Zvolení dalšího postupu:

### <a name="about-action-urls"></a>Informace o adresách URL akce:
Všechny adresy URL, která lze interpretovat operačním systémem cílových zařízení mohou sloužit jako adresa URL akce.
Některé vlastní adresu URL, která mohou podporovat aplikace (například chcete, aby uživatelé přechod na určitou stránku) lze také jako adresa URL akce.
Každý výskyt vzorek {deviceid} se automaticky nahrazuje identifikátor zařízení provádění akce. To lze snadno získat zapojení Azure mobilní zařízení identifikátory přes externí webové služby hostované na zadní office.

- **Android + iOS akce**
    - Otevřete webovou stránku
    - http://\[doména webové webu\] 
    - Příklad: http://www.azure.com
    - Odeslání e-mailu
    - mailto:\[e-mailu příjemce\]?subject =\[předmět\]& textu =\[zprávy\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Odeslání služby SMS
    - služby SMS:\[telefonní číslo\] 
    - Příklad: sms:2125551212
    - Vytočit telefonní číslo
    - Telefon:\[telefonní číslo\] 
    - Příklad: tel:2125551212
- **Android pouze akce**
    - Stažení aplikace na Play Store
    - Market://details?id=\[balíčku aplikace\] 
    - Příklad: market://details?id=com.microsoft.office.word
    - Spustit hledání geo lokalizované
    - GEO:0, 0?q =\[vyhledávacího dotazu\] 
    - Příklad: geo:0, 0? q = starbucks, Paříž
- **pouze akce iOS**
    - Stažení aplikace v obchodu App Store
    - /id /app/ [název aplikace] http://iTunes.Apple.com/ [Země] [id aplikace]? mt = 8 
    - Příklad: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
    - Akce Windows
    - Otevřete webovou stránku
    - http://\[doména webové webu\] 
    - Příklad: http://www.azure.com
    - Odeslání e-mailu
    - mailto:\[e-mailu příjemce\]?subject =\[předmět\]& textu =\[zprávy\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Odeslání zprávy (Skypu aplikace pro Store povinné)
    - služby SMS:\[telefonní číslo\] 
    - Příklad: sms:2125551212
    - Vytočit telefonní číslo (Skypu aplikace pro Store povinné)
    - Telefon:\[telefonní číslo\] 
    - Příklad: tel:2125551212
    - Stažení aplikace na Play Store
    - MS-windows – úložiště přihlašovacích údajů: PDP?PFN =\[ID balíčku aplikace\] 
    - Příklad: ms-windows – úložiště přihlašovacích údajů: stránce podrobností projektu? PFN = 4d91298a-07cb-40fb-aecc-4cb5615d53c1
    - Spustit hledání bingmaps
    - bingmaps:?q =\[vyhledávacího dotazu\] 
    - Příklad: bingmaps:? q = starbucks, Paříž
    - Použití vlastní schématu
    - \[schéma\]://\[schéma parametry\] 
    - Příklad: myCustomProtocol://myCustomParams
    - Použití balíčku dat (aplikace pro Store pro rozšíření číst povinné)
    - \[Složka\]\[dat\]. \[rozšíření\] 
    - Example:myfolderdata.txt
 
### <a name="build-a-tracking-url"></a>Vytvoření sledování adresy URL:
-    Najdete v části "Nastavení" <UI Documentation> pro pokyny k vytváření sledování adresy URL, které vám umožní uživatelům stahování některou z jiných aplikací.
 
### <a name="define-the-texts-of-your-announcement"></a>Definování texty oznámení
Zadejte název, obsah a tlačítko znění oznámení. Je možné zacílit cílové skupině představované budoucí kampaně na základě reakcí reach jak uživatelé odpovídal tento kampaní. Cílové skupině můžete na základě reakcí o tom, jestli byla této kampaně jenom posune, odpovědích actioned nebo neopustí.

### <a name="see-also"></a>Viz taky
- [Uživatelské rozhraní si přečtěte následující dokumentaci - Reach – nové nabízených kritérium][Link 28]

## <a name="content-of-polls"></a>Obsah hlasování
![Reach Content2][31] vyplňte nadpis, popis a tlačítko znění oznámení. Potom přidejte otázky a možností pro odpovědi na vaše otázky.
Je možné zacílit cílové skupině představované budoucí kampaně na základě reakcí reach jak uživatelé odpovídal tento kampaní. Cílové skupině můžou být založená na tom, jestli byla této kampaně jenom posune, odpovědích actioned nebo neopustí. Cílové skupině můžete taky být na základě reakcí hlasování odpovědí, kde otázku a odpověď volba slouží jako kritérií.

### <a name="see-also"></a>Viz taky
- [Uživatelské rozhraní si přečtěte následující dokumentaci - Reach – nové nabízených kritérium][Link 28]
 
## <a name="content-of-data-pushes"></a>Posune obsah dat
![Reach Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Zvolte typ dat:
- Text
- Binární data
- Data ve formátu Base 64

### <a name="define-the-content-of-your-data"></a>Definování obsahu dat
- Pokud jste vybrali nabízená textová data, zkopírujte a vložte text do pole "obsah".
- Pokud jste vybrali nabízená binární nebo ve formátu Base 64 data, můžete pomocí tlačítka "umístěte vložit ovládací prvek" nahrajte soubor.
- Je možné zacílit cílové skupině představované budoucí kampaně na základě reakcí reach jak uživatelé odpovídal tento kampaní. Cílové skupině můžou být založená na tom, jestli byla této kampaně jenom posune, odpovědích actioned nebo neopustí.

### <a name="see-also"></a>Viz taky
- [Uživatelské rozhraní si přečtěte následující dokumentaci - Reach – nové nabízených kritérium][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Obsah dlaždic (pouze pro Windows Phone)
![Reach Content4][33]

### <a name="define-the-content-of-your-tile"></a>Definování obsah dlaždici
Dlaždice datové je text, který se má zobrazit v této dlaždici aplikace na zařízeních s Windows Phone.
Dlaždice nabízených je verze Microsoft nabízená oznámení služby (MPNS) nativní push pro Windows Phone. Typ dlaždice nabízená je jediný typ nabízených, ve kterém není odpověď a tak nemůžete cílové skupině představované budoucí kampaní založená na výsledky kampaně nabízených dlaždice. 

### <a name="see-also"></a>Viz taky
- [Rozhraní API si přečtěte následující dokumentaci - Reach API – nativní připínáčku][Link 4]

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
 
