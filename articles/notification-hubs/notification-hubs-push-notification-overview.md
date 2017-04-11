<properties
    pageTitle="Rozbočovače Azure oznámení"
    description="Naučte se používat nabízená oznámení v Azure. Ukázky napsané v jazyce C# pomocí rozhraní API .NET."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Rozbočovače Azure oznámení

##<a name="overview"></a>Základní informace

Azure rozbočovače oznámení poskytují infrastrukturu snadno použitelné s více platformami, diagramů s měřítky mimo nabízených, která vám umožní odeslat mobilní nabízená oznámení ze všech back-end (v cloudu nebo místní) mobilní platformu.

S rozbočovače oznámení můžete snadno poslat různé platformy, přizpůsobených nabízená oznámení abstracting podrobnosti systémů oznámení pro jiné platformy (PNS). Pomocí jednoho volání rozhraní API je možné zacílit jednotlivé uživatele nebo celé publikum segmentů obsahující miliony uživatelů, na všech svých zařízeních.

Můžete oznámení rozbočovače enterprise a spotř scénářích. Příklad:

- Nejnovější příspěvky oznámení odešlete miliony s nízkou latence (oznámení rozbočovače zajišťuje aplikace Bing předinstalované na všech zařízeních Windows a Windows Phone).
- Odešlete na základě umístění kupóny segmentů uživatele.
- Odeslání oznámení o událostech uživatelům nebo skupinám sports/finance/her aplikací.
- Informujte uživatele enterprise události, jako třeba nové zprávy nebo e-mailům a potenciálních.
- Odeslání jednoho-úvazek-požadování hesel pro vícefaktorové ověřování.



##<a name="what-are-push-notifications"></a>Co jsou služby nabízená oznámení?

Smartphonech a tabletech můžete "oznámí" uživatelé výskyt události. Toto oznámení může trvat lišit.

V aplikacích pro Windows Store a Windows Phone, může být oznámení ve formě _oznámením_: Zobrazí se okno s nemodální, se zvukem, signál oznámení o nové. Další typy oznámení, které podporují zahrnout oznámení _dlaždice_ _nezpracovanými_a _oznámení_ . Další informace o typech oznámení podporované na zařízeních s Windows najdete v článku [dlaždice, odznáčky a oznámení](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

Na zařízení s iOS Apple push podobně upozorní na uživatele s dialogovým oknem žádosti uživatele k zobrazení nebo zavření oznámení. Kliknutím na tlačítko **zobrazení** otevřete aplikaci, která se zobrazuje zpráva o. Další informace o iOS oznámení najdete v článku [iOS oznámení](http://go.microsoft.com/fwlink/?LinkId=615245).

Nabízená oznámení pomáhají mobilními zařízeními zobrazit nové informace zůstává efektivně energie. Oznámení můžou posílat back-end systémy mobilními zařízeními i v případě, že odpovídající aplikací na zařízení nejsou aktivní. Nabízená oznámení jsou důležitou součástí aplikace příjemce, které se používají ke zvýšení využití aplikací a použití. Oznámení jsou také užitečné pro podniky, aktuální informace zvyšuje zaměstnance reakce na události business.

Konkrétní mobilní zapojení scénáře příklady:

1.  Aktualizace dlaždice ve Windows 8 nebo Windows Phone s aktuální finančních informací.
2.  Výstrahy uživatele s oznámením, přiřazené některé pracovní položky tomuto uživateli v aplikaci na základě pracovního postupu organizace.
3.  Zobrazení Odznáček s číslem aktuální prodej provede v aplikaci CRM (třeba Microsoft Dynamics CRM).

##<a name="how-push-notifications-work"></a>Nabízená oznámení práce

Nabízená oznámení doručované prostřednictvím specifické pro platformu infrastruktury s názvem _Systémy oznámení platformu_ (PNS). PNS nabízí funkce barebones (to znamená nepodporuje žádný vysílání pro individuální nastavení) a mít bez běžné rozhraní. Například k odeslání oznámení pro aplikace pro Windows Store, musíte kontaktovat vývojář WNS (Windows oznámení služby). Odeslání oznámení na zařízení s iOS, musí stejný vývojář kontaktovat APNS (Apple Push služba oznámení) a odešlete zprávu ještě jednou. Azure rozbočovače oznámení pomáhají zadáním běžné rozhraní, spolu s jiné funkce, které podporují nabízená oznámení na každý platformě.

Na vysoké úrovni ale všechny systémy oznámení platformy postupujte podle stejné vzorce:

1.  Klient aplikace kontaktů PNS k načtení jeho _Zpracovat_. Typ úchyt závisí na systém. WNS, po identifikátor URI nebo "oznámení channel." APNS je token.
2.  Klient aplikace ukládá tento úchyt aplikace _back-end_ pro pozdější použití. Pro WNS je back-end obvykle do cloudové služby. Pro Apple jestli chcete systém je místo toho možnost _poskytovatele_.
3.  Odeslání nabízená oznámení, kontakty aplikace back-end PNS pomocí úchytu jej směrovat instanci aplikace konkrétní klienta.
4.  PNS předá oznámení na zadané zařízení tak, že na úchyt.

![][0]

##<a name="the-challenges-of-push-notifications"></a>Problémy při nabízená oznámení

Když jsou tyto systémy hodně výkonná, budou ponechat množství práce pro vývojáře aplikace k provedení i obvyklé scénáře nabízená oznámení, například vysílání nebo odeslání nabízená oznámení Segmentovaný uživatelům.

Nabízená oznámení jsou jednou z funkcí ve službě cloud services pro mobilní aplikace. Důvodem je, že infrastruktury požadovat, aby fungovaly poměrně složité a převážně vliv na to, hlavní obchodní logiky aplikace. Některé úkoly při vytváření infrastrukturu na vyžádání nabízených jsou:

- **Závislost platformu.** Chcete-li odeslat oznámení zařízení na různé platformy, musíte více rozhraní kódovaný v back-end. Pouze se liší nižší úrovně podrobností, ale prezentace oznámení (dlaždice, uvidí nebo znaku) je také zase závislý platformu. Tyto rozdíly může vést k složité a těžko se snadnou údržbou back-end kód.

- **Měřítko.** Měřítko Tato infrastruktura obsahuje dva aspekty:
    + Podle pokynů PNS musí zařízení tokeny aktualizovat při každém spuštění aplikace. To vede k velké množství přenosů (a přístupy následné databáze) jenom aktuálnost tokeny zařízení. Počet zařízení roste (případně na miliony), náklady vytváření a udržování Tato infrastruktura při nonnegligible.

    + Většina PNSs nepodporují všesměrové vysílání na několika zařízeních. Následuje, se spojí miliony zařízení výsledkem miliony volání PNSs. Možnost zobrazit tyto požadavky totiž jednoduché, obvykle vývojáře aplikace chcete zachovat Celková latence. Například poslední zařízení zpráva neměli upozornění 30 minut po odeslání oznámení, pokud jde o mnoha případech je by připraven účel mít nabízená oznámení.
- **Směrování.** PNSs možnost, jak odeslat zprávu o zařízení. V aplikacích pro většinu však oznámení jsou určené pro uživatele a/nebo úrokové skupiny (například všechny zaměstnance přiřazené k určité účtu zákazníka). K oznámení směrovat správné zařízení, jako takové aplikace back-end vedou registru přiřadí úrok skupiny tokeny zařízení. Tento režijních přidá celkovou dobu market a údržba náklady aplikace.

##<a name="why-use-notification-hubs"></a>Proč používat rozbočovače oznámení?

Oznámení o rozbočovače eliminuje složitost: není nutné spravovat výzvy nabízená oznámení. Místo toho můžete rozbočovači oznámení. Oznámení o rozbočovače pomocí infrastrukturu plně s více platformami a rozšířených nabízená oznámení a značně snížit nabízených specifické kód, který běží v serverové části aplikace. Oznámení o rozbočovače implementovat všechny funkce infrastruktury připínáčku. Zařízení jsou pouze odpovědná za registraci PNS úchytů a back-end odpovídá pro odesílání zpráv nezávislý na platformě uživatelům nebo skupinám zájem, jak je znázorněno na následujícím obrázku:

![][1]


Oznámení o rozbočovače poskytuje infrastrukturu připravené k použití nabízená oznámení s následující výhody:

- **Více platformách.**
    +  Podpora pro všechny hlavní verze platformy mobilních. Oznámení rozbočovače můžete poslat nabízená oznámení pro Windows Store, iOS, Android a Windows Phone.

    +  Oznámení o rozbočovače přináší běžné rozhraní o neodeslání oznámení všechny podporované platformy. Specifické pro platformu protokoly není povinný. Aplikace back-end zasílat oznámení ve formátech specifické pro platformu nebo nezávislý na platformě. Aplikace pouze komunikaci se rozbočovače oznámení.

    +  Správa zařízení úchyt. Oznámení o rozbočovače udržuje úchyt registru a názorů PNSs.

- **Works with back-end**: cloudu a místní, .NET PHP, Java, uzel, atd.

- **Měřítko.** Oznámení o rozbočovače měřítko miliony zařízeními, aniž by potřeba znovu architektonické nebo shard.


- **Rozšířená nastavení vzorků dodání**:

    - *Vysílání*: umožňuje poblíž souběžné vysílání miliony zařízení jednoho rozhraní API hovoru.

    - *Unicast/Multicast*: nabízená značky představující jednotlivé uživatele, včetně všech jeho zařízeních; nebo širší skupinu. například samostatném formuláři faktory (tabletu nebo telefonu).

    - *Segmentace*: Zatlačit složité segmentu definované značku výrazy (například zařízení v New Yorku sledovat Yankees).

    Každý zařízení, když posíláte úchyt k rozbočovači oznámení můžete zadat jeden nebo více _značek_. Další informace o [značky]. Značky nemusíte předem zřízení nebo odstraněny. Značky lze jednoduché o neodeslání oznámení uživatelům nebo skupinám termínech. Protože značky mohou obsahovat žádný identifikátor specifické pro aplikaci (třeba ID uživatele nebo skupinu), jejich použití uvolní back-end aplikace z zátěž s k ukládání a správa zařízení úchyty.

- **Přizpůsobení**: každé zařízení můžete mít jednu nebo více šablon pro dosažení lokalizace na zařízení a přizpůsobení beze změny back-end kód.

- **Zabezpečení**: sdílené federované ověřování nebo přidružení přístup tajná (zabezpečení).

- **Formátovaný telemetrie**: k dispozici v portálu a programově.


##<a name="integration-with-app-service-mobile-apps"></a>Integrace se službou aplikaci služby – mobilní aplikace

Usnadnit setkat i v případě bezproblémové a jednotných přes Azure služby [Aplikace služby mobilní aplikace] podporuje vestavěné nabízených oznámení pomocí rozbočovače oznámení. [Aplikace služby mobilní aplikace] nabízí vývojové platformy vysoce scalable, globálně dostupné mobilní aplikace pro vývojáře organizace a doplňky systému, která přiblíží bohatou sadu funkcí k mobilní vývojáři.

Mobilní aplikace vývojáře využít rozbočovače oznámení s pracovním postupem následující:

1. Načtení popisovače PNS zařízení
2. Zaregistrovat zařízení a [šablony] v oznámení o rozbočovače prostřednictvím praktické API register mobilní aplikace klienta SDK
    + Všimněte si, že mobilní aplikace odstraní pryč všechny značky na registrace z bezpečnostních důvodů. Práce s rozbočovače oznámení z vaší back-end přímo do značky přidružit zařízení.
3. Odeslat oznámení z app backendovou s rozbočovače oznámení

Tady jsou některé výhody přenesený pro vývojáře tento integrací:

- **SDK mobilních klientů aplikace.** Tyto více SDK zadání jednoduchého rozhraní API pro registraci a Promluvte si centru oznámení pomocí mobilní aplikace automaticky propojen s. Vývojáře nemusí dostanete pomocí přihlašovacích údajů rozbočovače oznámení a pracovat s další služby.
    + SDK automaticky označení dané zařízení s aplikací Mobile ověřený ID uživatele povolit nabízených scénář uživatele.
    + SDK automaticky pomocí mobilní aplikace instalace ID jako GUID zaregistrovat oznámení rozbočovače ukládat vývojáře potíže při zachování více GUID služby.
    
- **Instalace modelu.** Mobilní aplikace funguje s modelem nejnovější nabízená oznámení rozbočovače představující všechny nabízených vlastnostech přidružených k zařízení při instalaci JSON zarovná služby nabízená oznámení, která se snadno používá.

- **Pružnost.** Vývojáři vždy možné pracovat s oznámení rozbočovače přímo i s integrací na místě.

- **Integrované prostředí [Azure]portálu.** Stisknutím a možnost představuje vizuálně v mobilních aplikacích vývojáři můžete snadno pracovat s centru přidružené oznámení pomocí mobilní aplikace.



##<a name="next-steps"></a>Další kroky

Další informace o oznámení rozbočovače najdete v těchto tématech:

+ **[Jak zákazníci používají rozbočovače oznámení]**

+ **[Výukové programy pro rozbočovače oznámení a vodítka]**

+ **Výukové programy pro oznámení rozbočovače Začínáme** ([iOS], [Android], [univerzální Windows], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

Důležité .NET spravované rozhraní API odkazy pro nabízená oznámení nacházejí zde:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Jak zákazníci používají rozbočovače oznámení]: http://azure.microsoft.com/services/notification-hubs
  [Výukové programy pro rozbočovače oznámení a vodítka]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Univerzální systému Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [Mobilní aplikace aplikace služby]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [šablony]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure portálu]: https://portal.azure.com
  [značky]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
