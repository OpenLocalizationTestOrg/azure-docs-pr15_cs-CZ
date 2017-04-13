<properties 
   pageTitle="Azure mobilní zapojení uživatelské rozhraní - segmentů" 
   description="Naučte se vytvářet a spravovat segmentů uživatelů k identifikaci vzorů použití pomocí zapojení Mobile Azure" 
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

# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Jak vytvořit a spravovat segmentů uživatelů k identifikaci vzorů použití

Tento článek popisuje kartě **SEGMENTŮ** portálu **Mobile Engagement** . Portál **Mobile zapojení** umožňuje sledovat a spravovat své mobilní aplikace.

Části segmentů uživatelského rozhraní umožňuje pracovat na rozdělíte uživatele na základě jiné chování a technologie pro analýzu, které jste dostali z aplikace, taky zpřístupníte prostřednictvím rozhraní API segmentech. Segmenty jsou vyhodnocované nejdřív 24 hodin od se vytvářejí a jsou přepočítávány každých 24 hodin založené na nejnovější informace o analýzy. Jakmile se vypočítá segment, zobrazí graf "Day s historií den" každý den.


>[AZURE.NOTE] Mnoho oddíly portálu **Mobile zapojení** uživatelského rozhraní včetně tlačítka **Zobrazit NÁPOVĚDU** . Klepnutím na toto tlačítko zobrazíte další kontextové informace o oddílu.

## <a name="create-segments"></a>Vytvoření segmentů
Můžete vytvořit segment podle až 10 kritérií v určitém období nahoru na 60 dní v minulosti z oddílu analýzy. Můžete například vytvořit segment podle lidmi, kteří mají zobrazit určité stránky nebo vyhledávat určitého obsahu v rámci aplikace během posledních 10 dnů. Tyto informace jsou k dispozici v části analýzy. Ano můžete ji použijete k vytváření segmentu a pak nastavení nabízených oznámení jej směrovat tento podmnožinu uživatelé si je vrátit do aplikace. 
 
> Poznámka: Po vypočítána segment ji nelze upravovat; dá se jenom klonovat (zkopírované) nebo odstraněna (odstraněné). Segment můžete klonovat stejné aplikace (plus pár stejné ID aplikace) a můžete klonovat také do jiné aplikace (plus pár různých ID aplikace). 
 
 ![segments1][35] 

## <a name="examples-segments"></a>Příklady segmentů
 ![segments2][36]

Segmentů umožňují segmentech koncoví uživatelé aplikace.
Rozdělíte vaši uživatelé je důležité marketingové strategie. Azure zapojení Mobile vám umožní získat historické data a vytvořit vlastní segmenty. Tento výkonný nástroj umožňuje další informace o zákazníkům v aplikaci. Můžete snadno analyzovat segmenty a použít segmenty jako cíle připínáčku.
Běžné případu použití je, že chcete odeslat nabízená oznámení na podporu vaši koncoví uživatelé postup ohodnocení aplikace v úložišti. Místo odesílání oznámení všem koncovým uživatelům, můžete vytvořit segment, který chcete zadat jenom uživatelé, kteří používají aplikaci každý den minulý měsíc a že jste využili skvělé uživatelského prostředí. Když pošlete méně, vysoce cílových nabízená oznámení, dostanete lepší návratnost investic.
 
 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Segmenty, které můžete vytvořit na základě hlavní Azure Mobile zapojení prvků:
- Události: vytvořte segment dané cílů jeden zvláštní události aplikace, která se stalo více než dvakrát po týdnu. 
- Relace: vytvoření segment uživatelů, kteří používají aplikaci víc než 5 číslem minulý týden.
- Aktivita: vytvořte segment uživatelů, kteří používají jednu stránku nebo obsahu více nebo méně než 10 čas minulý měsíc.
- Úlohy: vytvoření segmentu uživatelů, které jste dokončili úloha přesahem dvakrát den.
- Dojde k chybě: vytvořte segment všech uživatelů, které jste využili zhroucení více než 10krát minulého týdne. (Tohoto segmentu omluvu nebo dokonce kupón může nabízená!)
- Chyba: vytvoření segmentu všech uživatelů, kteří mají došlo k chybě víc než 100 časy posledních 3 dnů.
- Informace o aplikace: vytvoření segment, který se zaměřuje vlastní informace o aplikaci uskutečněných během posledních 25 dnů.
 
 ![segments4][38]

Použít segmentu optimální způsobem, musíte provést vlastní integrace SDK v aplikaci značek plánu "Informace App" značky.
Přejděte na domovskou stránku rozhraní, vyberte požadovanou aplikaci a kliknutím na oddíl "Segmentů".

1. Vyberte požadovaný oddíl "Segmentů".
2. Kliknutím na "nové segmentu" s cílem vytvořte nový segment.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Skutečný příklad života: Vytvořte jednoduchý segment na základě informací o "Relace"
Vytvoření segment všechny koncových uživatelů, které použili aplikace aspoň 50 časy ve minulý týden. Z tohoto umístění najděte pouze koncových uživatelů, které dosud vynaložena nejméně 30 sekund, než v aplikaci relace. Zobrazí všechny koncoví uživatelé, kteří mají kladné prostředí v aplikaci. Potom segment vytvoření může nabízená oznámení tyto koncovým uživatelům Vybídněte je, aby ohodnocení aplikace v úložišti.
 
 ![segments5][39]

1. Pojmenujte segmentu abyste mohli rychle najít v seznamu "Segmentu".
2. Klikněte na tlačítko "Vytvořit".
 
 ![segments6][40]

Vyberte relace.
 
 ![segments7][41]

1. Vyberte období "Minulého týdne".
2. Klikněte na další.
 
 ![segments8][42]

1. Vyberete operátor, která se vztahuje mezi seznamu: =; ≥, ≤.
2. Zadejte počet chcete.
3. Vyberte výskyt chcete. 
4. Klikněte na další.
V tomto příkladu je nastavený tak, aby přes minulého týdne odpovídající uživatelé, které udělali aspoň 50 relace.
 
 ![segments9][43]

Pro segmentace relace můžete vybrat délka relace jako kritéria.

1. Vyberte operátor ze seznamu.
2. Zadejte délku relace.
3. Klikněte na další.
V tomto příkladu s textem, přes všechny relace, která byla segmentována v části výskyt, vyberte jenom uživatelé, kteří mají vynaloženy více než 30 sekund, než relace.
 
 ![segments10][44]

Pojmenujte kritérium k načtení do dokončení filtr a klikněte na Dokončit.
 
 ![segments11][45]

Po dokončení nastavení kritérium se zobrazí v filtr segment.
Protože segment je na základě technologie pro analýzu dat, jsou vyhodnocované segmenty jednou za den.
V tomto příkladu 47,7 % celkového koncových uživatelů spárované kritérium. Měly by být uživatele, kteří mají kvalitní a bude pravděpodobně poskytnout vyšší hodnocení, pokud je nabízená oznámení požádáte postup ohodnocení aplikací ve storu.


## <a name="see-also"></a>Viz taky

- [Koncepty][Link 6]
- [Odstraňování potíží se službou Průvodce][Link 24]

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
