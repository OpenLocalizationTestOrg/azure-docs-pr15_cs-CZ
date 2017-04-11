<properties 
   pageTitle="Informujte uživatele dat přijaté od senzory nebo jiných systémů | Microsoft Azure"
   description="Popisuje, jak používat události rozbočovače uživatelům data snímače oznamovat."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>Informujte uživatele přijatých z senzory nebo jiných systémů dat.

Předpokládejme, že máte aplikaci, která sleduje data v reálném čase nebo vytváří sestavy podle plánu. Když se podíváte na web, na které se zobrazí tyto nebo v reálném čase grafech, může se zobrazit něco, co vyžaduje akce. Co dělat, když je třeba výstrahy těchto situací, spíše než předávající na pamatovat na podívejte se web? Představte si, že máte light zvětšit v skleníkových a musíte znát okamžitě pokud light přejde. Jedním ze způsobů, který by se light senzoru v skleníkových, uspořádání k odeslání e-mailu Pokud light je vypnuté.

![][1]

Další případ představte, že spustíte domácí naloďovací zařízení a musí být upozorní zhoršeným skladových zásob dodávky. Například mohou uspořádat posílat textové zprávy ze systému ERP Pokud skladové zásoby PSA potravin je odstraněn kritické úrovně. 

![][2]

Problém se dozvíte, jak získat důležité informace, když jsou splněné určité podmínky, není při orientace k rezervování statické sestavy. Pokud používáte [Rozbočovač události Azure][] nebo [Azure IoT Centrum][] pro příjem dat ze zařízení nebo podnikových aplikací například [Dynamics AX][], máte několik možností, jak chcete zpracovat. Můžete tyto informace zobrazit na web je možné analyzovat, můžete je uložit a je můžete používat ke spuštění příkazů, které chcete něco udělat. K tomuto účelu můžete výkonné nástroje například [Azure weby][], [SQL Azure][], [HDInsight][], [Cortana Intelligence sadu][], [IoT sadu][], [Použití logických operátorů aplikace][]nebo [Rozbočovače oznámení Azure][]. Ale někdy vše, které chcete udělat, je někomu s nejméně režijních poslat tato data. Chcete-li vidíte, jak se to, že se tato metoda kódu jsme zadali nový vzorek [AppToNotifyUsers][]. Možnosti součástí jsou e-mailu (SMTP), služby SMS a telefonu.

## <a name="application-structure"></a>Struktura aplikace

Aplikace je napsáno v jazyce C# a soubor readme ve výběru obsahuje všechny informace, které potřebujete k úpravám, vytváření a publikování aplikace. Následující části obsahují podrobný přehled udělá tohle aplikace.

Začneme tím se za předpokladu, že máte důležité události na rozbočovači události Azure nebo IoT rozbočovači. Každý rozbočovač udělá dlouhou, jak máte přístup k němu a znáte připojovací řetězec.

Pokud už nemáte centrální událost nebo IoT rozbočovače, můžete snadno nastavíte rozložené test s Arduino znaku a malin pí, postupujte podle pokynů v aplikaci project [Připojení tečky](https://github.com/Azure/connectthedots) . Snímačem light na znaku Arduino odešle light úrovně prostřednictvím pí k [Centrální události Azure][] (**ehdevices**) a [Analýzy toku Azure](https://azure.microsoft.com/services/stream-analytics/) úlohy posune upozornění k rozbočovači druhý události (**ehalerts**) Pokud light úrovně dostali spadají pod určitou úroveň.

Po spuštění **AppToNotify** přečte konfigurační soubor (App) k získání adresy URL a přihlašovací údaje pro událost centrální příjem upozornění na. Potom založí nepřetržitě sledovat události Centrum pro všechny zprávy, které se zobrazí výzva – dlouhou, jak máte přístup k adresu URL pro centrální událost nebo IoT rozbočovače a platných přihlašovacích údajů, tento kód čtečka události rozbočovače nepřetržitě číst, co je tu procesu. Při spuštění aplikace také bude číst adresy URL a přihlašovací údaje pro zasílání zpráv služby (e-mailů, služby SMS, telefon) chcete použít a jméno nebo adresu odesílatele a seznam příjemců.

Jakmile sledování událostí centrální zjistí zprávu, spustí proces, který odešle zprávy pomocí metody zadané v souboru konfigurace. Všimněte si, že odesláním každé zprávy, které rozpozná. Pokud nastavíte monitor tak, aby ukazovaly k rozbočovači události, která přijímá deseti zprávy sekundu, odesílatel odešle deseti zpráv za sekundu – deseti e-mailů sekundu, zprávy SMS deset sekundu, deseti telefonní hovory za sekundu. Z tohoto důvodu Ujistěte se, sledovat události rozbočovači, který pouze obdrží upozornění, které musí být odesláno, ne rozbočovači události, obdrží původní data z senzory nebo aplikací.

## <a name="applicability"></a>Použitelnost

Kód v tomto příkladu pouze ukazuje, jak sledovat události rozbočovače a jak volat externího zasílání zpráv služby v případě, že chcete aplikaci přidat tuto funkci. Všimněte si, že toto řešení DIY zaměřené vývojář příklad pouze. Nezabývá požadavky organizace jako redundance, před selháním, restartujte po chyba atd. Další úplná a výrobních řešení najdete v těchto článcích:

- Použití spojnice nebo nabízená oznámení pomocí služby [Azure logiky aplikace](../app-service-logic/app-service-logic-connectors-list.md) .
- Pomocí [Rozbočovače oznámení Azure](https://msdn.microsoft.com/library/azure/jj927170.aspx), jak je popsáno blogu [vysílání nabízených oznámení pro miliony mobilní zařízení pomocí rozbočovače oznámení Azure](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs). 

## <a name="next-steps"></a>Další kroky

Je jednoduché vytvořit jednoduchý oznámení služba, která odešle příjemci e-mailů nebo textové zprávy nebo volání, předávací dat dostali centrální událost nebo centrální IoT. Nasazení řešení upozornit uživatele na základě dat dostali tyto rozbočovače, navštěvujte blog o [AppToNotifyUsers][].

Další informace o těchto rozbočovače najdete v následujících článcích:

- [Rozbočovače Azure události]
- [Azure centrální IoT]
- Začínáme s [kurz rozbočovače události].
- Kompletní [Ukázková aplikace, který využívá rozbočovače události].

Abyste mohli nasadit řešení založených na datech dostali tyto rozbočovače uživatelům oznamovat, navštěvujte blog o:

- [AppToNotifyUsers][]

[Kurz rozbočovače události]: event-hubs-csharp-ephcs-getstarted.md
[Azure centrální IoT]: https://azure.microsoft.com/services/iot-hub/
[Rozbočovače Azure události]: https://azure.microsoft.com/services/event-hubs/
[Centrální Azure události]: https://azure.microsoft.com/services/event-hubs/
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AX]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure weby]: https://azure.microsoft.com/services/app-service/web/
[Databáze SQL Azure]: https://azure.microsoft.com/services/sql-database/
[HDInsight]: https://azure.microsoft.com/services/hdinsight/
[Cortana Intelligence Suite]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[IoT Suite]: https://azure.microsoft.com/solutions/iot-suite/
[Použití logických operátorů aplikace]: https://azure.microsoft.com/services/app-service/logic/
[Rozbočovače Azure oznámení]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png