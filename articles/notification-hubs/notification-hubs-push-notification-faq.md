<properties
    pageTitle="Azure oznámení rozbočovače – nejčastější dotazy"
    description="Nejčastější dotazy na návrh a implementace řešení oznámení rozbočovače"
    services="notification-hubs"
    documentationCenter="mobile"
    authors="ysxu"
    manager="erikre"
    keywords="nabízená oznámení nabízená oznámení, iOS nabízená oznámení, android nabízená oznámení, nabízených ios, android připínáčku"
    editor="" />

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu" />

#<a name="push-notifications-with-azure-notification-hubs---frequently-asked-questions"></a>Nabízená oznámení s Azure oznámení rozbočovače – nejčastější dotazy

##<a name="general"></a>Obecné
###<a name="1---what-is-the-price-model-for-notification-hubs"></a>1. co je cena modelu doplňku pro rozbočovače oznámení?
Oznámení o rozbočovače je k dispozici v tři úrovně:

* **Uvolnit** - získat až 1 milion posune jedno předplatné za měsíc.
* **Základní** - získat 10 milion posune jedno předplatné za měsíc jako Směrný plán s možnostmi LOGLINTREND kvóty.
* **Standardní** - dostanete 10 milion posune jedno předplatné za měsíc jako Směrný plán, s kvóty zvětšit možnosti společně s formátovaným telemetrie capabilties.

Nejnovější informace najdete na stránce [Ceny rozbočovače oznámení] . Cen zřídit na úrovni předplatné a je zadaný počet zahájení nabízená oznámení, nezáleží na kolik obory názvů nebo oznámení rozbočovače nevytvoříte Azure předplatné.

**Uvolnit** osy nabídnuta za účelem vývoj s zaručit SLA. Během této osy může být vhodné výchozí bod pro ty, které chcete prozkoumat možnosti nabízená oznámení prostřednictvím rozbočovače oznámení Azure, nemusí být nejlepší volbou pro střední aplikacím velkém měřítku.

**Základní** & **Standardní** úrovní nabídnuta výrobní použití má následující klíčové funkce povolena *pouze pro standardní úroveň*:

- *Rich telemetrie* – nabídka oznámení rozbočovače celou řadu možností pro export dat telemetrie i nabízená oznámení registrační informace pro zobrazení offline a analýza.
- *Víceklientská* - ideální Pokud vytváříte v mobilní aplikaci pomocí oznámení rozbočovače podporuje víc klientů. To umožňuje nastavit přihlašovací údaje nabízená oznámení služby (PNS) na úrovni obor názvů centrální oznámení pro aplikace a pak oddělit klienti poskytovat jim jednotlivých rozbočovače v části tohoto běžné oboru názvů. Díky usnadnění údržby při zachování klávesy přidružení zabezpečení pro odesílání a příjem nabízených oznámení z rozbočovače oznámení odděleny pro každého klienta zajištění překrytí není křížově klienta.
- *Plánovaná nabízených* – umožňuje plánování nabízená oznámení, která má být následně ve frontě a odesláno.
- *Hromadného importu* – umožňuje importovat registrace hromadně.

###<a name="2---what-is-the-notification-hubs-sla"></a>2. co je SLA rozbočovače oznámení?
Pro **základní** a **Standardní** oznámení rozbočovače úrovní jsme zaručit, že aspoň 99,9 % čas správně nakonfigurovaném aplikací bude moct posílat nabízená oznámení a provádění operací správy registrace týkající rozbočovači oznámení používaný v rámci podporované osy. Další informace o naše SLA, navštivte stránce [SLA rozbočovače oznámení] .

> [AZURE.NOTE] Protože rozbočovače oznámení, závisí na externích platformy poskytovatelů předvádění nabízená oznámení zařízení jsou záruky SLA žádné nohy mezi službou oznámení platformy a zařízení.

###<a name="3---which-customers-are-using-notification-hubs"></a>3. které zákazníci používají rozbočovače oznámení?
Máme velkého počtu zákazníkům, kteří používají rozbočovače oznámení s několika důležitých ty následující:

* Sochi 2014 – 100s úrok skupiny, 3 + milionů zařízení, 150 + milionů oznámení odeslané ve dvou týdnů. [CaseStudy - Sochi]
* Skanska - [CaseStudy - Skanska]
* Olomouc třikrát - [CaseStudy - Seattlu časy]
* Mural.ly - [CaseStudy - Mural.ly]
* 7Digital - [CaseStudy - 7Digital]
* Bing aplikace – 10s milionů zařízeních a odesílání 3 miliony oznámení/den.

###<a name="4-how-do-i-upgrade-or-downgrade-my-notification-hubs-to-change-my-service-tier"></a>4. jak upgrade nebo omezit Moje oznámení rozbočovače změnit Moje vrstvy služeb?
Přejděte na [Portál klasického Azure], klikněte na službu Bus a klepněte na obor názvů potom rozbočovače oznámení. Na kartě Měřítko budou moci měnit vrstvy služeb rozbočovače oznámení.

![](./media/notification-hubs-faq/notification-hubs-classic-portal-scale.png)

##<a name="design--development"></a>Návrh a vývoj
###<a name="1---which-server-side-platforms-do-you-support"></a>1. Jaký serverovou platformách vámi podporované?
Připravili jsme SDK a [dokončení ukázky] pro .NET, Java PHP, Python Node.js tak, aby aplikace backendovou nastavení sdělovat rozbočovače oznámení pomocí kterékoli z těchto platformách. Rozhraní API rozbočovače oznámení vycházejí rozhraní REST, můžete přímo Promluvte si s můžou být místo toho pokud nechcete, aby pro přidání dalších závislosti. Další informace najdete na stránce [NH - rozhraní REST API] .

###<a name="2---which-client-platforms-do-you-support"></a>2. Jaký klientský platformách vámi podporované?
Podporujeme odesílající nabízená oznámení [systémem Apple iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Univerzální Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android Čína (přes Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) & [Androidem](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Aplikací Chrome](notification-hubs-chrome-push-notifications-get-started.md) a [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) platformách. Úplný seznam výukové lekce boji odesílání nabízená oznámení na těchto platformách Začínáme navštivte naše [NH - začíná začít výukové programy pro] stránku.

###<a name="3---do-you-support-smsemailweb-notifications"></a>3. můžete podporují oznámení služby SMS/e-mailu nebo webu?
Oznámení o rozbočovače určena především o neodeslání oznámení na mobilní aplikace pomocí platformách výše uvedené. Není zatím nabízíme možnost odeslat e-mailu nebo služby SMS upozornění; ale platformách třetích stran, které poskytují tyto funkce lze integrovat spolu s rozbočovače oznámení o odeslání nativní nabízená oznámení pomocí [Mobilní aplikace Azure].

Oznámení o rozbočovače taky nenabízejí v prohlížeči nabízená oznámení o doručení služby mimo z předdefinovaných. K provedení pomocí SignalR nad podporované platformy serverovou rozhodnout, zákazníky. Pokud hledáte o neodeslání oznámení prohlížeče aplikace v karanténě Chrome, přečtěte si [kurz aplikací Chrome].

###<a name="4---what-is-the-relation-between-azure-mobile-apps-and-azure-notification-hubs-and-when-do-i-use-what"></a>4. co je vztah mezi aplikací Mobile Azure a Azure oznámení rozbočovače a kdy co použít?
Pokud máte existující mobilní aplikace back-end a chcete přidat možnost odeslat nabízená oznámení můžete rozbočovače oznámení Azure. Pokud chcete nastavit back-end vaše mobilní aplikace od začátku měli zvážit, použití Azure mobilních aplikací. Mobilní aplikace Azure automaticky ustanovení rozbočovači oznámení pro vás mohli snadno odeslat nabízená oznámení z back-end mobilní aplikace. Ceny pro Azure mobilní aplikace zahrnuje základní poplatky rozbočovače oznámení a pouze platíte při přechodu mimo však započítávány posune. Další informace o nákladech jsou dostupné na stránce [Aplikace služby ceny] .

###<a name="5---how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>5. kolik zařízení může podporovat odeslání nabízená oznámení prostřednictvím rozbočovače oznámení?
Získáte na stránce [Oznámení rozbočovače ceny] podrobné informace o počet podporovaná zařízení.

Pro některé scénáře, pokud potřebujete podporu více než 10 000 000 registrovaných v případě zařízení prosím [nás kontaktovat](https://azure.microsoft.com/overview/contact-us/) přímo a můžeme vám pomůže měřítko řešení.

###<a name="6---how-many-push-notifications-can-i-send-out"></a>6. kolik nabízená oznámení můžete odeslat?
V závislosti na vybrané osy Azure bude automaticky škálování na základě počtu oznámení prochází systému.

>[AZURE.NOTE] Použití celkové náklady můžete přejít na základě počtu nabízená oznámení právě podávané množství. Ujistěte se, že si byli vědomi existující vrstva omezení přehledem na stránce [Ceny rozbočovače oznámení] .

Stávající zákazníci používají oznámení rozbočovače denně odesílat milióny nabízená oznámení. Nemusíte provádět žádné zvláštní zobrazit svůj nabízených, které oznámení dosáhnout tak dlouho, dokud používáte rozbočovače oznámení Azure.

###<a name="7---how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>7. jak dlouho trvá pro odeslání nabízená oznámení kontaktovat svého zařízení?
Azure rozbočovače oznámení je možné zpracuje minimálně **1 milion nabízená oznámení odešle na minutu** v běžném použít scénáři, kde je poměrně konzistentní a není spikey v přírodě příchozí načíst. Tento kurz se může lišit v závislosti na počtu značky, druh příchozí odešle a jiných externích faktorů.

V době dodací služba není možné vypočítat cílů za platformy a směrování zprávy služby odpovídajících nabízená oznámení o doručení založené na výrazů registrovaných značky a značky. Tady na odpovídá nabízená oznámení služby (PNS) k odeslání oznámení zařízení.

PNS nezaručuje žádné SLA pro doručování oznámení; avšak obvykle většina nabízená oznámení doručované do cílové zařízení objevit během pár minut (obvykle v rámci 10 minut) od okamžiku, kdy se odesílají naše platformu. Může být několik odlehlé hodnoty, které může trvat delší dobu.

>[AZURE.NOTE] Azure rozbočovače oznámení má zásadu na místě přetáhnout nabízená oznámení, které nejsou možné nedoručila ji do PNS za 30 minut. Toto zpoždění můžete příčiny pro spousta důvodů, nejčastěji běžně PNS omezení aplikace.

###<a name="8---is-there-any-latency-guarantee"></a>8. je k dispozici žádné záruku latenci?
Z důvodu přírodní nabízená oznámení (doručení tak, že externí, specifické pro platformu služba nabízených oznámení) není nijak zaručené latence. Obvykle většinou nabízená oznámení o doručení během několika minut.

###<a name="9---what-are-the-considerations-i-need-to-take-into-account-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>9. co jsou důležité informace potřebné vzít v úvahu při návrhu řešení s obory názvů a rozbočovače oznámení?

####<a name="mobile-appenvironment"></a>Mobilní aplikace/prostředí

* By měl být středovém oznámení na mobilní aplikace na prostředí.
* Ve scénáři více klienta měli každého klienta rozbočovači samostatné.
* Můžete třeba nikdy sdílet centru oznámení mezi test a výrobní prostředí to může způsobit problémy řádku při odesílání oznámení. například Apple nabízí izolovaného prostoru a výrobní nabízená koncové body s každým s samostatné přihlašovací údaje.
* Ve výchozím nastavení můžete odeslat oznámení test registrovaných zařízení prostřednictvím portálu Azure nebo komponentu Azure integrované ve Visual Studiu. Mezní hodnota je nastavena na 10 zařízení, které jsou vybrány náhodně z fondu registrace.

>[AZURE.NOTE] Pokud centru byla původně nakonfigurována certifikát izolovaného prostoru Apple a potom nakonfigurovat tak, použijte certifikát výrobní Apple, by staré tokeny zařízení stanou neplatnými pomocí nového certifikátu a způsobit posune selhání. Je vhodné oddělit výrobní testovacím prostředí a pomocí různých rozbočovače na jiném prostředí.

####<a name="pns-credentials"></a>Přihlašovací údaje PNS

Když v mobilní aplikaci máte zaregistrované v rámci portál pro vývojáře na platformu (například Apple nebo Google atd) získáváte aplikace identifikátor a tokeny zabezpečení, které serverové části aplikace, musí poskytovat služby nabízená oznámení platformy mají být k odeslání nabízená oznámení na zařízeních. Těchto tokenů zabezpečení, které mohou být ve formě certifikátů (například systémem Apple iOS a Windows Phone) nebo klíče zabezpečení (Google Android a Windows Další) muset možné konfigurovat rozbočovače oznámení. Obvykle probíhá na úrovni Centrální oznámení, ale také se teď dá na úrovni názvů ve scénáři více klienta.

####<a name="namespaces"></a>Obory názvů

Obory názvů mohou sloužit k seskupení nasazení.  Je taky lze znázornit všechny rozbočovače oznámení pro všechny klienty stejné aplikace ve scénáři více klienta.

####<a name="geo-distribution"></a>GEO rozdělení.

Rozdělení GEO nebývá vždycky kritické v nabízená oznámení scénáře. Je třeba upozornit rovnoměrně nejsou různé nabízená oznámení služby (například APNS, GCM atd), které jsou nakonec nabízená oznámení na zařízeních, distributed.

Pokud máte aplikaci, která se používá opačném konci světa, můžete vytvořit několik rozbočovače v různé obory názvů využijete dostupnost rozbočovače oznámení služby v různých oblastech Azure po celém světě.

>[AZURE.NOTE] To zvětšíte správy náklady - zejména kolem registrace, takže to se nedoporučuje skutečně musí udělat jenom pokud existuje explicitní potřeba.

###<a name="10--should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>10. mám udělat, registrace z app back-end nebo přímo z klientského zařízení?
Registrace z app back-end jsou užitečné, pokud je potřeba udělat ověření klienta před vytvořením registrace nebo pokud máte značky, které musí být vytvořené nebo změněné na základě některých aplikace postupu back-end aplikace. Další informace najdete podrobnější na stránkách [back-end registrace pokyny] a [pokyny pro registraci back - 2] .

###<a name="11--what-is-the-push-notification-delivery-security-model"></a>11. co je model zabezpečení nabízená oznámení o doručení?
Azure rozbočovače oznámení pomocí [Přidružení sdílené podpis aplikace Access (zabezpečení)](../storage/storage-dotnet-shared-access-signature-part-1.md)-na základě model zabezpečení. Tokeny přidružení zabezpečení můžete použít na kořenové úrovni názvů nebo na úrovni podrobného rozbočovače oznámení. Těchto tokenů přidružení zabezpečení se dá nastavit jiné ověřovací pravidla například oprávnění zprávu odeslat, poslech oznámení oprávnění atd. Další informace jsou k dispozici v dokumentu [model NH zabezpečení] .

###<a name="12--how-should-i-handle-sensitive-payload-in-the-push-notifications"></a>12. jak zacházet citlivá data v nabízená oznámení?
Všechny oznámení doručované cílová zařízení tak, že platformu nabízená oznámení služby (PNS). Když odesílatele pošle oznámení Azure oznámení rozbočovače potom jsme procesu a předejte oznámení odpovídajících PNS.

Všechna připojení od odesílatele rozbočovače Azure oznámení a potom PNS pomocí HTTPS.

>[AZURE.NOTE] Azure rozbočovače oznámení nezaznamenává datové zprávy žádným způsobem.

Odeslání citlivé datové části však doporučujeme vzorek zabezpečené nabízená kde odesílatele poskytuje "ping oznámení s identifikátorem zprávu do zařízení bez citlivé datové a aplikaci v zařízení obdrží tato data, je možné zavolat zabezpečené rozhraní API přímo k načítání podrobností zprávy. Na stránce [NH – kurz zabezpečené nabízená] neexistuje vodítko o tom, jak implementovat vzorek jsme je popsali výše.

##<a name="operations"></a>Operace
###<a name="1---what-is-the-disaster-recovery-dr-story"></a>1. co je text havárie obnovení (DR)?
Připravili jsme pokrytí havárie obnovení metadat na naše konci (jméno centrální oznámení, připojovacího řetězce a dalších důležitých informací). Při aktivaci scénáři DR se registrace data **jenom segmentu** infrastruktury rozbočovače oznámení, které budou ztraceny. Je třeba implementovat řešení znovu vyplňte tato data do nové po obnovení centrální.

- *Krok 1* : vytvoření rozbočovači sekundární oznámení v různých datacentra. Můžete vytvořit to rychlé úpravy v době DR událost nebo můžete vytvořit jednu z get přejít. Ho není zkontrolujte moc rozdíl zvolená možnost protože oznámení centrální zřizování je poměrně rychlé proces v pořadí několik sekund, než. Máte jednu od začátku vás chrání před událostí DR ovlivňující ochranu možnostech správy, takže je možnost důrazně doporučujeme.

- *Krok 2* – hydrát sekundární centrální oznámení s registrací z centra primární oznámení. Nedoporučujeme pokusíte Udržovat registrace na obou rozbočovače a pokusíte se zůstal je synchronizovaná rychlé úpravy registrace pocházejí v – obvykle, které nefungovalo dobře kvůli inherentní tendence registrace platnost na straně PNS. Oznámení o rozbočovače vyčistit je jako jsme získávat názory ostatních PNS o registracích vypršela platnost nebo je neplatný.  

Doporučujeme používat serverové části aplikace, které buď:

- Udržuje danou sadu registrace jeho konci tak, aby ho můžete dělat hromadné vložení rozbočovači sekundární oznámení v případě DR

**NEBO**

- Získá do běžné výpis registrací z centra primární jako zálohu a nemá hromadné vložte do vedlejší NH.

>[AZURE.NOTE] Registrace pro Export a Import funkce dostupné ve standardní vrstvy jsou popsány v dokumentu [Registrace pro Export a Import] .

Pokud nemáte back-end, pak při spuštění aplikace systému u cílové zařízení, se provede novou registraci v centru sekundární oznámení a nakonec sekundární oznámení centrální bude mít všechna aktivní zařízení registrované.

Nevýhodou je, že bude časové období, kdy zařízení, kde se aplikace neotevřeli nahoru nebudete dostávat oznámení.

###<a name="2---is-there-any-audit-log-capability"></a>2. je k dispozici všechny možnosti protokolu auditování?
Všechny operace oznámení rozbočovače správy přejděte na protokoly operace, které jsou zveřejněným v [Azure klasické portálu].

##<a name="monitoring--troubleshooting"></a>Sledování a odstraňování potíží
###<a name="1---what-troubleshooting-capabilities-are-available"></a>1. Poradce při potížích jaké funkce jsou dostupné?
Azure rozbočovače oznámení obsahují několik funkcí, které se běžné řešení potíží s přihlášením, zejména nejběžnější scénář kolem zamítnuté oznámení. Informace o na naše [NH – Poradce při potížích s] dokument.

###<a name="2---what-telemetry-features-are-available"></a>2. jaké funkce telemetrie jsou dostupné?
Azure rozbočovače oznámení umožňuje prohlížení telemetrickými daty v [Azure klasické portálu]. Podrobnosti o dostupných metriky jsou dostupné na stránce [NH - metriky] .

>[AZURE.NOTE] Upozornění na úspěšné pouze znamená, že nabízená oznámení byla Doručená do externí služby nabízených oznámení (například APNS pro Apple, GCM pro Google atd). Je PNS předvádění oznámení cílová zařízení. Obvykle PNS nezobrazuje doručení metriky třetím stranám.  

Připravili jsme taky možnost export telemetrickými daty programově (ve **Standardní** vrstvě). V tématu [NH - metriky ukázka] podrobnosti.

[Azure klasického portálu]: https://manage.windowsazure.com
[Oznámení o rozbočovače ceny]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Oznámení o rozbočovače SLA]: http://azure.microsoft.com/support/legal/sla/
[CaseStudy - Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[CaseStudy - Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[CaseStudy - časy ze Seattlu]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[CaseStudy - Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[CaseStudy - 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH - rozhraní REST API]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH - výukové programy Začínáme]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Kurz aplikací Chrome]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Pokyny pro registraci back-end]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Pokyny pro registraci back - 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Model NH zabezpečení]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[NH – kurz zabezpečené připínáčku]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[NH – řešení potíží]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[NH - metriky]: https://msdn.microsoft.com/library/dn458822.aspx
[NH - metriky ukázka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Registrace pro Export a Import]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[dokončení vzorky]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobilní aplikace]: https://azure.microsoft.com/en-us/services/app-service/mobile/
[Aplikace služby ceny]: https://azure.microsoft.com/en-us/pricing/details/app-service/
