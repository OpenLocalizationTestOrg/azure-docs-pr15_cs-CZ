<properties 
   pageTitle="Azure mobilní zapojení uživatelské rozhraní - Reach kampaně" 
   description="Laern jak vytvářet a spravovat nabízená oznámení kampaně pomocí zapojení Mobile Azure" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Jak vytvořit a spravovat kampaní nabízená oznámení
Části Reach uživatelského rozhraní slouží k vytvoření nové kampaně nabízená složité vzorcem zadáním všechny informace, které budete muset poslat nabízená oznámení. Možnosti nabízených kampaně trochu lišit podle toho, typy čtyři kampaně: oznámení, hlasování, posune dat a dlaždice (pouze pro Windows Phone).

### <a name="option-applies-to"></a>Možnost platí pro:
- Jazyky: Všechny (oznámení, hlasování, Data posune, dlaždice)
- Kampaň: Všechny (oznámení, hlasování, Data posune, dlaždice)
- Oznámení: Oznámení, hlasování
- Obsah: Jedinečné pro každý typ kampaň
- Cílová skupina: Všechny (oznámení, hlasování, Data posune, dlaždice)
- Časové rozmezí: oznámení, hlasování, dlaždic
- Test: Všechny (oznámení, hlasování, Data posune, dlaždice)
 
![Reach Campaign1][20]

## <a name="languages"></a>Jazyky
Můžete použít nabídku rozevírací seznam jazyků jiné verze vaší nabízených odešlete zařízení, která je nastavena na použití různých jazyků. Ve výchozím nastavení se obnoví všechna zařízení stejné nabízených bez ohledu na to, jaký jazyk se nastavit, aby používal. Uživatelé, kteří mají svoje zařízení nastavit jiný jazyk se obnoví výchozí jazyková verze Push. Spoustu možností kampaně nabízených umožňují určit alternativní obsah pro každý další jazyky, které vyberete. 
 
![Reach Campaign2][21]

### <a name="language-differences-apply-to"></a>Použití jazyka rozdíly:
- Jazyky: Jedinečné jazyky mohou být vybrány kromě výchozí jazyk
- Kampaň: to samé platí pro všechny jazyky
- Oznámení: Jedinečné pro každý jazyk kromě výchozí jazyk
- Obsah: Jedinečné pro každý jazyk kromě výchozí jazyk
- Cílová skupina: Může filtrované podle kritérium samostatném jazyka
- Časové rozmezí: stejné pro všechny jazyky
- Test: Mohou být odeslány pro jednotlivé jazyky najednou
 
### <a name="supported-languages"></a>Jazyky podporované:
- Arabština (ar) 
- Bulharština (bg) 
- Katalánština (ca) 
- Čínština (zh) 
- Chorvatština (h) 
- Czech (cs) 
- Dánštinu (da) 
- Holandština (Nizozemsko) 
- Angličtina (en) 
- Finština (fi) 
- Francouzština (fr) 
- Němčina (de) 
- Řečtina (l) 
- Hebrejština (mu) 
- Hindština (skrýt) 
- Maďarština (hu) 
- Indonéština (id) 
- Italština (Pokud se) 
- Japonština (ja) 
- Korejština (ko) 
- Lotyština (lv) 
- Litevština (SV) 
- Malajština (macrolanguage) ([ms) 
- Norština (Bokmål) (Poznámka) 
- Polština (pl) 
- Portugalština (pt) 
- Rumunština (ro) 
- Ruština (ru) 
- Srbština (sr) 
- Slovenština (sk) 
- Slovinština (sl) 
- Španělština (es) 
- Švédština (sv) 
- Tagalogské (tl) 
- Thajské (ář) 
- Turečtina (tr) 
- Ukrajinština (Kanada) 
- Vietnamština (vi) 
 
## <a name="campaign"></a>Kampaň
V části kampaně umožňuje zadejte název a kategorií kampaně stejně jako chcete ignorovat sekci cílová skupina kampaně nabízených a místo toho odeslat tuto kampaň prostřednictvím rozhraní API dosáhla (a některé prvky s nízké nabízená rozhraní API). Kategorie lze pomocí šablony vlastní oznámení pro oznámení v aplikaci ovládací prvek na základě předdefinovaného nastavení. Můžete získat seznam existující "kategorie" prostřednictvím rozhraní API kontaktovat.

> Upozornění: Použijete možnost "Cílová skupina ignorovat nabízených se pošle uživatelů prostřednictvím rozhraní API" v části "Kampaně" Reach kampaně kampaně není odešle automaticky, budete muset poslat ručně pomocí rozhraní API kontaktovat.
 
![Reach Campaign3][22]
 
### <a name="option-applies-to"></a>Možnost platí pro:
- Název: všechny
- Kategorie: Oznámení, hlasování
- Ignorovat cílová skupina nabízených se pošle uživatelů prostřednictvím rozhraní API: všechny
 
## <a name="notification"></a>Oznámení
Část oznámení umožňuje základní nastavení nabízených zahrnutí: název Push, zpráva, obrázek v aplikaci, nebo pokud je dialogových. Mnoho nastavení oznámení jsou specifické pro platformu zařízení. Můžete zvolit, zda vaše nabízených odešle "aplikace" nebo "oddálit aplikace" nebo obojí. (Nezapomeňte, že uživatelé můžou "určovat, jestli se změnami" nebo "vyjádření výslovného nesouhlasu" aplikace"od" posune operačního systému úrovně na svých zařízeních a Azure Mobile zapojení nebude možné přepsat. Nezapomeňte taky, že rozhraní API dosáhla pracuje "v aplikaci" a "mimo aplikaci" posune. Rozhraní API nabízených mohou sloužit k zpracovat "mimo aplikaci" posune příliš.) Posune můžete přizpůsobit s obrázky nebo obsah HTML, včetně hloubkové odkazů pro propojení mimo aplikaci nebo do jiného umístění ve své aplikaci (Android SDK 2.1.0 nebo novější záměru kategorie povinné). Můžete změnit Odznáček ikonu nebo na iOS a odesílat obsah text nebo web (místní s html obsahu, adresu URL do jiného umístění nebo mimo aplikaci). Můžete taky provést vyzvánění zařízení s Androidem nebo vibrace s Push. (Nezapomeňte, že byste potřebovali správný soubor vyzvánění nebo vibrace zařízení manifestu SDK oprávnění v Androidu.) Momentálně č industry standardní pro Android "celkového" rozměrů, protože velikostí obrazovky se liší na všech zařízeních, ale 400 × 100 obrázky dopracování téměř libovolného obrazovku.

### <a name="delivery-types"></a>Typy doručení:
-    Odhlášení z aplikace pouze: oznámení budete dostávat, když uživatel nepoužívá aplikace.
- Mimo aplikaci pouze oznámení vyžaduje certifikát z Apple nebo Google (certifikát APNS nebo GCM).
- V aplikace pouze: oznámení se zobrazí pouze při spuštění aplikace.
- Oznámení se systémem Capptain doručení zastihnout uživatele. Je možné přizpůsobit vizuální rozložení/zobrazení vašeho připínáčku.
- Kdykoli: Tato možnost zajišťuje odeslat oznámení, které je některý z aplikace spuštěna nebo ne.

 
![Reach Campaign4][23]

### <a name="option-applies-to"></a>Možnost platí pro:
- Oznámení: Oznámení, hlasování
 
## <a name="content"></a>Obsah
Části obsahu umožňuje upravovat obsah oznámení, hlasování, posune dat, a dlaždice (pouze pro Windows Phone). Nastavení obsahu nabízených kampaní si specifické pro typu kampaně. 

### <a name="see-also"></a>Viz taky
- [Uživatelské rozhraní si přečtěte následující dokumentaci - dosáhla - nabízená obsahu][Link 29]
 
![Reach Campaign5][24]

## <a name="audience"></a>Cílové skupiny
Sekci cílová skupina lze definovat standardní seznam položek omezit kampaně nebo omezení kampaně založené na vlastní kritéria. Standardní sady možnosti omezit vaší cílovou skupinou umožňuje nabízená nových a starých nebo jenom uživatelům nativní připínáčku. Můžete taky nastavit kvótu omezit počet uživatelů, kteří obdrží push. Můžete ručně upravit výraz pro filtrování kampaně zahrnout jeden nebo více kritérium cílových uživatelů. Můžete ručně zadat výraz cílové skupiny. Tento výraz musí definovat explicitně vztahy mezi kritéria. Kritérium je popsán identifikátor, musíte začít s velkým písmenem a nesmí obsahovat mezery. Vztah mezi kritéria lze popsat pomocí "a", "nebo", "není operátory i"(",")". Příklad: "Criterion1 nebo (Criterion1 a ne Criterion2)".

> Poznámka: S velkou cílovou skupinu součástí kampaní, zacílení naskenovaný obrázek na straně serveru může být pomalé, zejména pokud budete chtít spustit více kampaní ve stejnou dobu.

- Pokud je to možné spouštět pouze jedné kampaně najednou.
- Nejvíce spouštět pouze čtyři kampaní najednou.
- Použít pouze pro aktivní uživatelé (zaškrtávací políčko "zapojit pouze uživatelé, kteří zastižení pomocí nativního nabízených" a "jenom aktivní uživatelé") tak, aby jenom uživatelé pořád nainstalovanou aplikaci a použijte ji bude potřeba zkontrolovat.
Po definování cílové skupině můžete pomocí tlačítka simulovat zjistit, kolik uživatelé dostanou tento připínáčku. To bude vypočítat počet známé uživatele potenciálně směrovat podle této cílové skupiny (Toto je odhad na základě vzorku náhodných uživatelů). Mějte na paměti, že jsou také součástí této cílové skupiny uživatelů, kteří mají odinstalaci aplikace, ale nemůžete získat přístup na stránku.

### <a name="see-also"></a>Viz taky
- [Uživatelské rozhraní si přečtěte následující dokumentaci - Reach – nové nabízených kritérium][Link 28]

![Reach Campaign6][25]

### <a name="edit-expression"></a>Upravit výraz
![Reach Campaign7][26]
 
### <a name="limit-your-audience-option-applies-to"></a>Omezení možností cílové skupiny platí pro:
- Zapojit jen podmnožinu uživatelů: všechny (oznámení, hlasování, posune dat, dlaždice)
- Zapojit pouze staré uživatele: všechny (oznámení, hlasování, posune dat, dlaždice)
- Zapojit jenom nové uživatele: všechny (oznámení, hlasování, posune dat, dlaždice)
- Zapojit pouze nedělá uživatelé: oznámení, hlasování, dlaždic
- Zapojit jenom aktivní uživatelé: všechny (oznámení, hlasování, posune dat, dlaždice)
- Zapojit pouze uživatelé, kteří zastižení pomocí nativního nabízená: oznámení, hlasování
 
## <a name="time-frame"></a>Časové rozmezí:
V části časové rozmezí: slouží k nastavení při stisknutí odešle nebo časový rámec můžou být prázdné okamžitě začít kampaně. Myslete na to, že podle časového pásma koncových začnou kampaně dříve, než očekávat pro vaši koncoví uživatelé v Asii a odeslání malých dávkách posune najednou, dokud všechny časová pásma na světě odpovídají časové rozmezí nastavit kampaně den. Podle časového pásma koncoví uživatelé můžete taky způsobit zpoždění v kampaní protože má požádat o čas z telefonu před spuštěním push.

> Poznámka: Kampaně bez koncové datum můžete mezipaměti posune místně a pořád zobrazily až ručně dokončení kampaní. Chcete-li předejít toto chování konkrétní čas ukončení kampaní.

### <a name="see-also"></a>Viz taky
- [Dosažení – jak jejich – plánování][Link 3] 
 
![Reach Campaign8][27]

### <a name="settings-apply-to"></a>Nastavení platí pro:
- Časové rozmezí: oznámení, hlasování, dlaždic
 
## <a name="test"></a>Test
Pomocí oddílu Test odešlete tento nabízených zařízení test před uložením kampaně. Pokud jste nakonfigurovali vlastní jazyků pro tuto kampaň, můžete otestovat push každý jazyk. Můžete nastavit zkušební zařízení "Můj účet".
> Poznámka: Žádné na straně serveru, který data zaznamenané po pomocí tlačítka "testování" posune, dat pouze přihlásili skutečné nabízených kampaní.

### <a name="see-also"></a>Viz taky
- [Uživatelské rozhraní si přečtěte následující dokumentaci - Můj účet][Link 14]
 
![Reach Campaign9][28]

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
 
