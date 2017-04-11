<properties
 pageTitle="Prognostické údržbu předem řešení | Microsoft Azure"
 description="Popis prediktivní údržbu Azure IoT automaticky předem nakonfigurovaná řešení."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Přehled prediktivní údržby automaticky předem nakonfigurovaná řešení

Řešení *prediktivní údržbu* automaticky předem nakonfigurovaná je jednou z [předem řešení] [ lnk_preconfigured_solutions] vydána jako součást sady [Microsoft Azure IoT][lnk_iot_suite]. Toto řešení je integrována v reálném čase zařízení telemetrie kolekce prediktivní model vytvořený pomocí [Azure počítače výukové][lnk_machine_learning].


S Azure IoT sadu podniky můžete rychle a snadno připojit ke sledování majetku a analyzovat data v reálném čase. Řešení prediktivní údržbu automaticky předem nakonfigurovaná zabírá tato data a používá pro firmy poskytnout nové intelligence, který můžete řídit efektivity a vylepšení příjmů bohaté řídicí panely a vizualizace.

## <a name="the-scenario"></a>Scénáře

Fabrikam je místní aerolinka, která se zaměřuje na spuštění skvělé zákazníkem za konkurenční ceny. Jeden zpoždění letů vznikne údržby problémy a údržba engine letadla je zejména náročné. Chyba Engine během letu musí za všech okolností vyhnout tak Fabrikam pravidelně kontroluje jeho moduly řeč a vyhovuje pro plánovanou údržbu programu. Však moduly letadla řeč není vždy používejte stejný. Některé nepotřebných údržbu proběhne na webech. Problémy důležitější vznikají, které můžete světlá letadla, dokud se provádí údržbu. To způsobí, že nákladné zpoždění, zejména pokud letadla na místě, kde na pravém techniky nebo náhradní části nejsou k dispozici.

Moduly řeč společnosti Fabrikam letadla jsou podporovány s senzory, které sledují engine podmínky během letu. Fabrikam umožňuje shromáždit data snímače shromažďované během letu Azure IoT sadu. Po sbírat roků stroje provozní a data selhání mít společnosti Fabrikam dat vědeckých modelovat způsob předpovídání zbývající užitečné životnost (RUL) stroje letadla. Co jste určili je korelace data ze čtyř čidel engine s opotřebení engine vede k případnému selhání. Zatímco Fabrikam provádět pravidelné kontroly zajistit bezpečnost, můžete teď použity modely pro výpočet RUL pro každý modul po každé letu pomocí telemetrie shromážděné z webů během letu. Fabrikam teď můžete odhadu budoucích čárky selhání a plán pro údržbu a opravit předem.

> [AZURE.NOTE] Model řešení používá skutečné engine opotřebení data.

Podle předpovídání bod při údržby je potřeba, můžete optimalizovat Fabrikam jeho operace snížit náklady. Údržby koordinátoři pracovat s plánovače plánovat údržbu shodný s letadla zastavení v určitém umístění a za ověření oprávněnosti dostatek času pro letadla je mimo služby snadnějšímu narušení plánu. Naplánujte Fabrikam techniky. zajištění letadla jsou efektivní vyřízení bez prodleva. Správce ovládací prvek zásob dostávat plány údržby tak můžete optimalizovat jejich objednávání a náhradní části zásob. Všechny informace o umožňuje Fabrikam minimalizovat letadla základu čas a snižuje provozní náklady zároveň zajistit bezpečnost osob a posádky.

Pokud chcete zjistit, jak [Azure IoT sadu] [ lnk_iot_suite] poskytuje funkce zákazníci potřeba uvědomit potenciál prediktivní údržby, zkontrolujte toto [infographic][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Jak sestavují řešení prediktivní údržby

Řešení využití existujícího Azure počítače výukové modelu jsou k dispozici jako šablonu, která se zobrazit tyto možnosti práce ze zařízení telemetrie shromážděných prostřednictvím služby IoT sadu. Vytvořila Microsoft [regresní model] [ lnk_regression_model] stroje letadla a dokončení šablonu dat publikovali<sup>\[1\]</sup>a podrobné pokyny k použití modelu.

Řešení prediktivní údržbu automaticky předem nakonfigurovaná Azure IoT používá regresní model vytvořený pomocí této šablony; je používaný Azure předplatné a vystaven prostřednictvím automaticky generované rozhraní API. Řešení obsahuje podmnožinu testování data, která představují 4 (celkem 100) stroje a 4 (21 celkové) senzor datových proudů, které poskytují přesné výsledky z školení modelu.

*\[1\] A. Saxena a K. Goebel (2008). "Turbofan Engine snížení úrovně simulace sadu dat" NASA Ames Prognostics datový úložiště (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), Centrum zdrojů informací Ames NASA, Moffett pole, CA*

## <a name="next-steps"></a>Další kroky

Další informace o jak Azure IoT umožňuje scénáře prediktivní údržbu najdete v tématu [zachytit hodnotu z Internetu věci][lnk_capture_value].

Prohlédněte [návodu] [ lnk-predictive-walkthrough] prediktivní údržby automaticky předem nakonfigurovaná řešení.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Můžete také zkoumat některých dalších funkcí a možností řešení IoT sadu automaticky předem nakonfigurovaná:

- [Nejčastější dotazy IoT sadu][lnk-faq]
- [Zabezpečení IoT od základu nahoru][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
