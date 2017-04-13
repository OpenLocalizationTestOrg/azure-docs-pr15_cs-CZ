<properties
    pageTitle="Důležité informace o výkonu pro správce dopravy Azure | Microsoft Azure"
    description="Princip výkon přenosy správce a jak otestovat výkonu svého webu při použití přenosy správce"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="performance-considerations-for-traffic-manager"></a>Důležité informace o výkonu pro správce dopravy

Tato stránka vysvětluje správce přenosy důležité informace o výkonu. Zvažte následující situaci:

V oblasti WestUS a EastAsia máte instance webu. Kontrola stavu Správce zkušební provoz jednu z instancí se nezdařilo. Přenosy aplikace přesměrován správný oblast. Očekává se tento převzetí ale výkonu může jít o problém podle latence přenosy schůzku cestovat vzdálené oblast.

## <a name="how-traffic-manager-works"></a>Jak funguje přenosy správce

Pouze výkonu obsahujících správce přenosy na váš web je počáteční vyhledávání DNS. Žádost o DNS pro název profilu přenosy správce uskutečněných jednotlivými kořenový server DNS Microsoftu, který hostuje zónu trafficmanager.net. Přenosy správce naplní a pravidelně aktualizuje kořenové servery DNS společnosti Microsoft na základě zásad přenosy správce a výsledky zkušební. Dotazy bez DNS tak i během počáteční vyhledání DNS se odesílají správci přenosy.

Správce přenosy se skládá ze některé komponenty: název serveru DNS servery, rozhraní API služeb, vrstvy úložiště a koncový bod sledování služby. Pokud součást přenosy Správce služby nezdaří, je žádný vliv na název DNS související s profilem přenosy správce. Záznamy v serverů DNS Microsoftu zůstat beze změny. Však koncový bod sledování a aktualizace DNS z toho důvodu. Proto přenosy správce nedokáže aktualizovat DNS tak, aby ukazovaly na web selhání při havaruje primární webu.

Překládá názvy DNS název je rychlý a výsledky jsou uložených v mezipaměti. Rychlost počáteční vyhledávání DNS závisí na serverů DNS klienta používané pro překlad. Obvykle klienta dokončením DNS vyhledávací pole v rámci ~ 50 ms. Výsledky vyhledávání se mezipaměti dobu trvání DNS Time-to-live (TTL). Výchozí hodnota TTL pro správce přenosy je 300 sekund.

Přenosy není tok pomocí Správce přenosy. Po dokončení vyhledávání DNS klienta má IP adresu instance webu. Klient se připojí přímo na tuto adresu a nesplňuje podmínky pomocí Správce přenosy. Přenosy správce zásad, které zvolíte nemá vliv na výkon DNS. Výkon směrování metodu však může negativně ovlivnit prostředí aplikace. Například pokud vaše zásady přesměrovává přenosy z Severní Amerika k instanci hostované v Asii, latence sítě pro tyto relace může být problém s výkonem.

## <a name="measuring-traffic-manager-performance"></a>Měření výkonu přenosy správce

Existuje několik webů, které můžete použít k pochopení očekávaného výkonu a chování profil přenosy správce. Spoustu těchto webů jsou zadarmo, ale může mít omezení. Některé weby nabídnout lepší sledování a vytváření sestav funkce za poplatek.

Nástroje na těchto webech míra DNS čekacích dob a zobrazení přeložit IP adresy pro klienta umístění ve světě. Většinu těchto nástrojů Neukládat do mezipaměti výsledky DNS. Proto nástrojů zobrazit celou vyhledání DNS při každém spuštění testu. Při testování z vlastního klienta pouze celé vyhledávání výkon DNS jednou po dobu trvání TTL zaznamenat.

## <a name="sample-tools-to-measure-dns-performance"></a>Ukázka nástroje pro měření výkonu

- [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS nabízí celou řadu nástrojů výkonu. Nástroj porovnání DNS můžete zobrazit, jak dlouho bude trvat vyřešit názvu DNS a, srovnání s jiných poskytovatelů služby DNS.

- [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Nejjednodušší nástrojů reprodukujte WebSitePulse. Zadejte adresu URL zobrazíte DNS vyřešení času, první bajt, poslední bajt a další údaje o výkonu. Můžete vybírat tři různé zkušební umístění. V tomto příkladě vidíte, že je první spuštění uvedený vyhledávání DNS trvá 0.204 sec.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Protože jsou cached výsledky, druhém testu stejné koncového bodu přenosy správce vyhledávání DNS trvá 0,002 sec.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

- [Sledování aplikací syntetický certifikační Autority](https://asm.ca.com/en/checkit.php)

    Dřív označoval jako nástroj Watchmouse zaškrtnutí webu, tento web vidíte časového rozlišení DNS z několika zeměpisné oblastí současně. Zadejte adresu URL zobrazíte DNS rozlišení čas, dobu připojení a rychlosti z několika zeměpisné lokality. Použijte tento test zobrazíte hostovanou službu budou vráceny různá místa světě.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

- [Pingdom](http://tools.pingdom.com/)

    Tento nástroj poskytuje údaje o výkonu pro každý prvek na webovou stránku. Na kartě analýza stránky se zobrazí procento času stráveného vyhledávání DNS.

- [Co je Moje DNS?](http://www.whatsmydns.net/)

    Tento web znamená vyhledávání DNS z 20 různých místech a zobrazí výsledky na mapě.

- [Dostanete webového rozhraní](http://www.digwebinterface.com)

    Tento web zobrazuje že podrobnější informace služby DNS, včetně mají záznamy CNAME záznam. Ujistěte se, můžete zkontrolovat Kolorovat výstup a "Stat" ve skupinovém rámečku Možnosti seznamu a vyberte "všechny názvové servery.

## <a name="next-steps"></a>Další kroky

[O směrování metod přenosy přenosy správce](traffic-manager-routing-methods.md)

[Otestujte svoje nastavení správce dopravy](traffic-manager-testing-settings.md)

[Operace na přenosy správce (REST API odkazy)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure rutin přenosy správce](http://go.microsoft.com/fwlink/p/?LinkId=400769)
