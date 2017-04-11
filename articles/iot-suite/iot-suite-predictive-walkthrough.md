<properties
 pageTitle="Návod prediktivní údržbu | Microsoft Azure"
 description="Návod prediktivní údržby Azure IoT automaticky předem nakonfigurovaná řešení."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
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

# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Návod prediktivní údržbu automaticky předem nakonfigurovaná řešení

## <a name="introduction"></a>Úvod

Řešení prediktivní údržbu automaticky předem nakonfigurovaná IoT sady je začátku do konce řešení pro situace business s předpovídá bod při chyba se může dojít. Můžete toto předkonfigurované řešení včasným za například optimalizace údržbu. Řešení sloučí klíčové služby Azure IoT sadu, včetně [Azure počítače výukové] [ lnk_machine_learning] pracovního prostoru. Tento pracovní prostor obsahuje pokusy založené na veřejné ukázkové sady dat, předpovídání zbývající užitečné životnost (RUL) stroje letadla. Scénář IoT obchodní řešení plně používá jako výchozí bod pro plánování a implementaci řešení, které odpovídá vašim požadavkům specifickým obchodním.

## <a name="logical-architecture"></a>Logická architektura

Následující schéma shrnuje logické součásti předkonfigurované řešení:

![][img-architecture]

Modrá položky jsou Azure služby, které jsou v umístění, které vyberete když zřizujete předkonfigurované řešení zřízení. Můžete vytvořit předkonfigurované řešení ve východoasijských USA, severní Evropě nebo jihovýchodní Asie oblast.

V oblasti, kde zřízení předkonfigurované řešení nejsou dostupné některé zdroje informací. Oranžová položky v diagramu představují Azure služby zřízení nejblíže k dispozici oblasti (Jižní centrální nás, západní Evropě nebo jihovýchodní Asie) zadaných vybranou oblast.

Zelené položka je simulovaný zařízení, které představuje letadla engine. Je další informace o těchto simulovaný zařízeních v následující části.

Šedé položky představují součástí, které implementovat funkce *správy zařízení* . Aktuální verzi řešení prediktivní údržbu automaticky předem nakonfigurovaná není zřízení tyto materiály. Další informace o správě zařízení, podívejte se do [vzdálené sledování předkonfigurovaná řešení][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Simulovaný zařízení

V předem řešení představuje simulovaný zařízení letadla engine. Řešení máte k dispozici dva stroje, které mapují na jeden letadla. Každý modul posílá čtyři typy telemetrie: senzor 9, 11 senzor, senzor 14 a senzor 15 poskytují data potřebná pro počítač výukové modelu pro výpočet zbývající užitečné životnost (RUL) pro modul. Každý simulovaný zařízení odešle tyto zprávy telemetrie IoT centrální:

*Počet obrázku*. Cyklus představuje dokončení letu proměnné délky mezi 2 10 hodin, ve kterých se telemetrickými daty zaznamenává každé půl hodiny během letu.

*Telemetrie*. Existují čtyři senzory, které představují engine atributy. Čidel označené obecně senzor 9, 11 senzor, senzor 14 a senzor 15. Tato 4 čidla představují telemetrie dostatečné zobrazíte užitečné výsledky z počítače výukové modelu pro RUL. Tento model vytvořený z veřejné k sadám dat, který obsahuje data snímače reálnou engine. Další informace o vytvoření modelu z původního uvedenou množinu dat, najdete v článku [Cortana Galerie prediktivní údržbu šablona Intelligence][lnk-cortana-analytics].

Simulovaný zařízení můžete dělat následující příkazy odesílaným z rozbočovači IoT:

| Příkaz | Popis |
|---------|-------------|
| StartTelemetry | Určuje stav simulace.<br/>Spustí zařízení odesílání telemetrie     |
| StopTelemetry  | Určuje stav simulace.<br/>Ukončí zařízení odesílání telemetrie |

Rozbočovač IoT poskytuje potvrzení příkaz zařízení.

## <a name="azure-stream-analytics-job"></a>Azure úlohy toku analýzy

**Úlohy: Telemetrie** pracuje na příchozí proudu telemetrie zařízení pomocí dvou příkazů. První vybere všechny telemetrie ze zařízení a odešle tato data do objektů blob úložiště ze kdy je znázorněn ve web appu. Další použití příkazu vypočítá průměr senzor hodnoty nad posuvná okna dvě minuty a odešle tato data pomocí rozbočovače události **Procesor událostí**.

## <a name="event-processor"></a>Procesor událostí

**Procesor událostí** vezme hodnoty průměr senzor pro dokončení obrázku. Ho předá tyto hodnoty k rozhraní API, které poskytuje výuky počítače školení modelu pro výpočet RUL modul.

## <a name="azure-machine-learning"></a>Výukové Azure počítače

Další informace o vytvoření modelu z původního uvedenou množinu dat, najdete v článku [Cortana Galerie prediktivní údržbu šablona Intelligence][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>Začněme procházení

Tato část vás provede součásti řešení, popisuje případu zamýšlené použití a jsou uvedeny příklady.

### <a name="predictive-maintenance-dashboard"></a>Řídicí panel prediktivní údržby

Tato stránka ve webové aplikaci používá ovládací prvky PowerBI JavaScript (viz [PowerBI vizuální prvky úložiště][lnk-powerbi]) vizualizovat:

- Výstupní data z toku analýzy úlohy v úložišti objektů blob.
- Počet RUL a obrázku za letadla engine.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Sledování chování cloudové řešení

Na portálu Azure přejděte skupina zdroje s názvem řešení, které jste se rozhodli, abyste zobrazili zřizování zdroje.

![][img-resource-group]

Když zřizujete předkonfigurované řešení, dostanou e-mail s odkazem na pracovní prostor výukové počítače. Můžete taky přejít do pracovního prostoru výukové počítače z [azureiotsuite.com] [ lnk-azureiotsuite] stránky pro zřizování řešení po **připravena** .

![][img-machine-learning]

Na portálu řešení uvidíte, že se čtyřmi simulovaný zařízeními představující dvě letadla dva stroje za letadla, oba objekty mají čtyři senzory máte k dispozici vzorku. Když nejdřív přejděte na portál řešení, simulace ukončit.

![][img-simulation-stopped]

Klikněte na tlačítko **Start simulace** zahájíte simulace, ve kterém najdete v článku historii senzor RUL, cyklů, a historie RUL naplnění řídicího panelu.

![][img-simulation-running]

Menší než 160 (libovolného mezní zvolili pro účely ukázky) po RUL portálu řešení zobrazí upozornění symbol vedle zobrazení RUL a zvýrazní modul letadla ve žluté. Všimněte si, jak hodnoty RUL mít obecné sestupný trend celkové, ale mají za následek odraz nahoru a dolů. Toto chování výsledky z různých délky cyklu a přesnost modelu.

![][img-simulation-warning]

Úplné simulace 148 cyklů trvá asi 35 minut. 160 mezní RUL došlo ke splnění poprvé přibližně 5 minut a modulů pro obě řeč narazit je mezní hodnota rychlostí asi 8 minut.

Simulace projde úplnou sadu pro 148 cykly a vyrovná na výsledných hodnot RUL a obrázku.

Ukončení simulace kdykoli, ale kliknutím na **Spustit simulace** replays simulace od začátku datové sady.

## <a name="next-steps"></a>Další kroky

Teď se dostanete prediktivní údržbu automaticky předem nakonfigurovaná řešení můžete chtít změnit, najdete v článku [Pokyny k úpravám předkonfigurované řešení][lnk-customize].

Příspěvek na blog [Prediktivní údržbu IoT sadu - pod pokličkou -](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNetu obsahuje další podrobné informace o řešení prediktivní údržbu předem.

Můžete také zkoumat některých dalších funkcí a možností řešení IoT sadu automaticky předem nakonfigurovaná:

- [Nejčastější dotazy IoT sadu][lnk-faq]
- [Zabezpečení IoT od základu nahoru][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
