<properties
   pageTitle="Azure mobilní zapojení uživatelské rozhraní - analýzy"
   description="Zjistěte, jak k analýze historii dat o aplikaci pomocí zapojení Mobile Azure"
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

# <a name="how-to-analyze-historical-data-about-your-application"></a>Jak analyzovat historii dat o aplikaci

Tento článek popisuje kartě **analýzy** portálu **Mobile Engagement** . Portál **Mobile zapojení** umožňuje sledovat a spravovat své mobilní aplikace. Všimněte si, že začít používat portál musíte nejdřív vytvořit účet **Azure Mobile Engagement** .


V části analýzy v uživatelském rozhraní zobrazí souhrnné informace o aplikaci na základě historické dat, které se aktualizuje každých 24 hodin. Informace se zobrazí v různých řídicí panely skládající se ze řádku výsečový graf, tabulky a mapy. Data můžete stáhnout také jako soubory CSV. Většina těchto informací je k dispozici v reálném čase v části Sledování uživatelského rozhraní a taky k němu z rozhraní API analýzy.

>[AZURE.NOTE] Mnoho oddíly portálu **Mobile zapojení** uživatelského rozhraní včetně tlačítka **Zobrazit NÁPOVĚDU** . Klepnutím na toto tlačítko zobrazíte další kontextové informace o oddílu.

## <a name="standard-and-custom-analytics"></a>Standardní a vlastní Analytics

Azure zapojení Mobile k dispozici sadu základní, standardní analytický informace o vaše aplikace, které vypočítána hned, jak integrovat aplikace s SDK. Azure zapojení Mobile také umožňuje shromáždit další vlastní analytics požadované informace o chování vaši koncoví uživatelé. Lze provést vytvořením plánu značku vlastní "značky (informace o aplikaci)", vytvořené ze **stránky nastavení** tak, aby Azure Mobile zapojení získáte tyto další údaje za vás.



## <a name="analytics"></a>Technologie pro analýzu
- Řídicí panel: Zobrazuje obecné informace o nových a aktivující uživatelů a jejich trendy.
- Uživatelé: Uživatelé jsou označeny jejich identifikátor zařízení: Tento identifikátor jsou jedinečné pro každý zařízení (jednoho nového uživatele je skutečně jedné nové zařízení). Uživatel je považován za jako nové zadaného časového intervalu pokud mu provedl jeho první relace během tohoto časového intervalu. Uživatel se považuje za zachovány, pokud má provedl aspoň jednu relaci během posledních 7 dnech. Aktivní uživatelé jsou uživatelé, které udělali aspoň jednu relaci během určitého období. Můžete řadit, týdně, měsíčně denně nebo hodinové časová období. Všechny grafy vypadají podobně jako ale umožňují filtrovat podle různých funkcí, jako třeba verzi aplikace a klikněte na Seřadit podle určitého časového období. Standardní informace shromážděné integrace SDK obsahuje následující řetězec: aktivní uživatelé nového uživatele, počet relací, délka každé relace, technické informace o země, lokální, umístění, carrier jazyk, zařízení, firmwaru, síti (Wi-Fi), verze aplikace a SDK, používá zákazníky. Tyto informace lze zobrazit v reálném čase z části monitor.

> Poznámka: Časové období je na základě data z nastavení zařízení uživatelů, aby mohl uživatele, jehož telefon obsahuje datum nastavené nesprávně zobrazovaly ve špatném časové období.

- Uchovávání informací: Uživatel je považován za zachovány zadaného časového intervalu, pokud má provedl jeho první relace během tohoto časového intervalu. Změna časových intervalů, ve kterých jsou započteny zachovány uživatelům (a nové) hodin, dny, týdny nebo měsíce. Uchovávání informací analýzy uživatele je této technologii postavené kohorta. Kohorty je sada noví uživatelé detekovaný za dané období (tedy několika uživatelům provádění jeho první relaci během tohoto období). Budeme používat kohorty 1 den, 2 den, 4denně, 7 dní nebo 1 měsíc. Možnost kohorty, 1denně, 2denně, 4denně, 7 dnech, nebo 1 měsíc, Azure mobilní zapojení vypočítá sadu všichni uživatelé, kteří patří do kohorty a jsou pořád aktivní (tedy sadu uživatelů, kteří provedených aspoň jednu relaci období). Tuto sadu uživatelů se nazývá kohorty verze. (Azure zapojení Mobile lze zjistit, kolik uživatelů stále používáte aplikaci, ale jenom platformy konkrétní úložišti se dozvíte, kolik uživatele po odinstalaci aplikací – například GooglePlay iTunes, pro Windows Store, atd.).
- Relace: Použití aplikace uživatelem. Relace se vytvářejí z řady aktivit uživatelů (aktivitu obvykle souvisí s použití jednu obrazovku aplikace, ale to se může lišit v závislosti na způsobu SDK byla integrována do aplikace). Uživatel může provádět pouze jedné po jednom: relaci spustí hned, jak uživatel spustí jeho první činnosti a zastavíte mu dokončí svou činnost poslední. Pokud uživatel zůstává víc než několik sekund, než bez provedení činnosti, jeho posloupnost aktivity rozdělit na dvě různých relace.
- Aktivity: Jména každé obrazovky v aplikaci a délka mohou uživatelé výdaje na jednotlivých obrazovkách. Vlastní analytické možnost, která odpovídá "informace app" značky, které jste vytvořili vlastní aplikace pro Tyhle aktivity:
- Cesta uživatele: Ukazuje, jak uživatelé pohybovat aplikace aktivity (obrazovky). Můžete posunutím posuvníku nastavte úroveň podrobností. Modrá uzly představují aplikace aktivity. Jejich velikost je poměrné času uživatelé vynaloženy na něm. Bílý uzly představují relace spuštění a ukončení. Červené uzly představují dojde k chybě. Odkazy představují přechody mezi aktivity aplikace (nebo aktivity a dojde k chybě). Klikněte na uzel nebo odkaz pro zobrazení popisů ovládacích prvků s dalšími informacemi o dat: dobu strávenou v určité obrazovky počet přechody a procento přechody ze zdrojového aktivity aktivitou cíl. (---60 %---> B znamená, že uživatelé na aktivity A přejde na aktivity B 60 % od času.) Můžete změnit uspořádání v grafu požadovaného Upřesnit. polohy se uloží pokaždé, když provedete změnu. Můžete zobrazit nebo skrýt havaruje zesvětlit grafu.
- Události: Specifické akce uživatelem v aplikaci. Výskyt události se zobrazí jako počet události za uživatele za relace. Události představuje rychlé akce, například kliknutí na tlačítko nebo přijímání oznámení. (Význam události závisí na tom, jak byla v SDK integrována do aplikace.) Události může dojít během relace nebo projektu nebo může být samostatně.
- Úlohy: Podobné událostem kromě se zaměřuje na délce akce. Například úlohy může obsahovat technické informace o tom, jak dlouho trvá obsahu načtení nebo volání webové služby. Může taky zobrazit, jak dlouho trvá uživateli ve formuláři, vytvořte účet nebo nákupu. Úlohy představuje doby trvání úkolu, například doby trvání úkolu stahování nebo čas v pruhu se zobrazí na obrazovce. (Význam úlohy závisí na tom, jak byla v SDK integrována do aplikace.) Úlohy jsou obvykle souvisí s úlohy na pozadí, které provádí mimo rozsah relaci (tedy bez jakékoli činnosti uživatelů).
- Technicals: Technické informace o zařízení uživatele aplikace, můžete sledovat, například národní prostředí, Carrier, sítě, zařízení a firmwaru a obrazovky velikost zařízení uživatelů a verzi aplikace a verzi SDK použít v aplikaci.
- Chyby: Informace o technické chybách v aplikaci nezpůsobí aplikace chybu. Chyba představuje rychlé problém, například selhání sítě nebo chybné manipulace. (Význam události závisí na tom, jak byla v SDK integrována do aplikace.) K chybě může dojít během relace nebo projektu nebo může být samostatně.
- Dojde k chybě: Informace o chybách, které způsobí zhroucení aplikace. Selhání je neočekávané podmínku Pokud aplikace přestal jeho funkčnosti a musí být zastavení. Selhání je obvykle kvůli chyb v aplikaci.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Přístup k přehled uchovávání informací
![Analytics3][12]

Přehled uchovávání informací je prezentovaná uprostřed do několika karty jednotlivých zobrazení přehledu určitou dobu uchovávání informací. Doba uchovávání informací 2 den se zobrazí v příkladu. Jiné karty zobrazení období 4- a 7 – den, uchovávání informací.

## <a name="understanding-the-retention-overview-cards"></a>Principy karty Přehled uchovávání informací
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Každé kartě se skládá ze 3 hlavních částí:
1. 1: kohorty a období
2. 2 – 4: uchovávání informací za aktuální období
3. 5: minigrafu historie

### <a name="here-is-detailed-information-about-each-element"></a>Tady najdete podrobné informace o jednotlivé elementy:
1.    Kohorty a období: Tento nadpis zobrazí typ kohorty. Tady "2 dní" znamená, že se podíváme na chování uživatelé delší než dva dny uživatelů, které dny a jestli přišla po dobu 2 připojení v následujících blocích 2 dnů. Z příkladu výše byly použity činnosti uživatelů mezi 21 a 22nd dne.
2.    To vám sazba uchovávání informací myší 21 a 22 listopadu uživatelům příchozí 19 a 20 dne. Tady jsme dosáhl 1 aktivních uživatelů mezi 21 a 22nd, přes 3, které byly noví uživatelé mezi 19th a 20.
3.    Tento indikátor vizuálních vám bude radit stejné informace jako výše graficky. (Třetí kruhu pochází z číslo 33 %). Barvu nabízí další informace: zelené označuje toto číslo roste z předchozí výpočtu odebrány. Žlutá jiný stabilní a červené prostředky zmenšení.
4.    Tento údaj označuje používá pro výpočet hodnoty.
5.    Toto je minigraf historie hodnoty uchovávání informací. Umožňuje zobrazit hodnoty v minulosti mít obecných zobrazení jak evolved.


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
