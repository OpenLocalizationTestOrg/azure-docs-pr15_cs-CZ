<properties
 pageTitle="Přehled správy zařízení IoT centrální | Microsoft Azure"
 description="Tento článek obsahuje přehled správy zařízení v Azure IoT centrální: životního cyklu enterprise zařízení, restartujte počítač, factory resetovat, aktualizaci firmwaru, konfigurace, twins zařízení, dotazy, úlohy"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Základní informace o Azure IoT centrální správy zařízení (verze preview)

## <a name="introduction"></a>Úvod

Azure IoT Centrum poskytuje funkce a rozšíření modelu, umožňující zařízení a back-end vývojářům vytvářet výkonné IoT řešení pro správu zařízení. IoT zařízení sahají od omezené čidly a jeden účel microcontrollers výkonné bran, které směrování komunikace pro skupiny zařízení.  Kromě toho případy použití a požadavky pro IoT operátory lišit výrazně napříč odvětví.  Bez ohledu na tuto variantu Azure IoT centrální správy zařízení poskytuje funkce, vzorek a knihovny kódu pro řešení různého zařízení a koncoví uživatelé.

Důležité část vytváření úspěšné enterprise IoT řešení stanovit strategii jak operátory zpracovat průběžné správě jejich kolekce zařízení. Operátory IoT vyžadují jednoduchý a spolehlivé nástrojů a aplikací, které umožní fokus na další strategic aspekty své práce. Tento článek obsahuje:

- Stručný přehled centrální IoT Azure přístup ke správě zařízení.
- Popis běžných zásady správy zařízení.
- Popis životním cyklu zařízení.
- Základní informace o běžných vzorků správy zařízení.

## <a name="iot-device-management-principles"></a>Zásady správy zařízení IoT

IoT přináší s ním jedinečnou sadu úkoly správy zařízení a každý rozsáhlých řešení musí řešení následující zásady:

![Azure IoT Centrum zařízení správy zásad obrázek][img-dm_principles]

- **Měřítko a automatizace**: řešení IoT vyžaduje jednoduché nástroje, které můžete Automatizace běžných úkolů a povolit blok pro pedagogy relativně malým operace ke správě miliony zařízení. Každodenní, operátory očekávají, že vzdáleně zpracování zařízení operací, hromadně a pouze upozornit, když nastanou problémy, které vyžadují jejich přímé pozornost.

- **Průhlednost a funkce pro kompatibilitu**: ekosystému zařízení IoT je neobvykle různorodého. Nástroje pro správu musí přizpůsobit tak, aby zahrnoval velký počet třídy zařízení, platformy a protokoly. Operátory, musíte mít na podporu mnoho typů zařízení, od největší omezené vložený hoblinách jednoho obrázku, výkonné a plně funkční počítačích.

- **Plánování informovanosti kontextu**: IoT prostředí jsou dynamické a proměnlivých. Spolehlivosti služby je prvořadá. Operace správy zařízení musí faktor SLA Údržba windows stavy sítě a power, podmínky použití v a zeměpisná poloha zařízení zajistit této údržby prostoje nemá vliv na kritické podnikání nebo vytvořit nebezpečného podmínky.

- **Služby mnoho role**: podpora jedinečné pracovní postupy a procesy IoT rolí je důležité. Operace pedagogy musí obchodu pracovat s dané omezení interní oddělení IT.  Musíte zjistit taky trvalého způsoby povrchový reálném čase zařízení operace informace vedoucí a další organizační role vedoucím.

## <a name="iot-device-lifecycle"></a>IoT zařízení cyklus

Je sada fází správy Obecné zařízení, která jsou společná pro všechny projekty organizace IoT. V Azure IoT existuje pět fází v rámci životního cyklu IoT zařízení:

![Pět fází životního cyklu zařízení Azure IoT: plánování, zřízení, konfiguraci, monitorovat, vyřadit][img-device_lifecycle]

V každé z těchto pěti fází existuje několik požadavků operátor zařízení, které musí splňovat poskytnout úplné řešení:

- **Plánování**: povolení operátory schéma metadat zařízení, které umožňují snadno přesně vyhledat a cílová skupina zařízení pro operace správy hromadné vytvoření. Zařízení dvojitých slouží k ukládání tato zařízení metadata v podobě značky a vlastnosti.

    *Další materiály ke čtení*: [Začínáme s zařízení twins][lnk-twins-getstarted], [Vysvětlení informací na zařízení twins][lnk-twins-devguide], [jak používat dvojitých vlastnosti][lnk-twin-properties]

- **Poskytování**: bezpečně zřízení nového zařízení k rozbočovači IoT a povolení operátory a okamžitě objevovat možnosti zařízení.  Pomocí registru zařízení IoT centrální vytvářet flexibilní zařízení identit a přihlašovací údaje a tuto operaci hromadně pomocí projektu. Vytvoření zařízení a jejich funkce a podmínek pomocí zařízení vlastností v dvojitých zařízení sestav.

    *Další materiály ke čtení*: [Správa zařízení identit][lnk-identity-registry], [hromadné správy identit zařízení][lnk-bulk-identity], [jak používat dvojitých vlastnosti][lnk-twin-properties]

- **Konfigurace**: usnadnění hromadné změny konfigurace a aktualizace firmwaru zařízení a zachovat zdraví a zabezpečení. Proveďte tyto operace správy zařízení hromadně pomocí požadované vlastnosti nebo pomocí přímého metody a vysílání úlohy.

    *Další materiály ke čtení*: [použít přímý metody][lnk-c2d-methods], [vyvolat metodu přímý na zařízení][lnk-methods-devguide], [použití vlastností dvojitých][lnk-twin-properties], [plán a vysílání úlohy][lnk-jobs], [plánování úloh na několika zařízeních][lnk-jobs-devguide]

- **Sledování**: sledování stavu kolekce celkové zařízení, stav jednotlivých plánů, a upozornit operátory na problémy, které může vyžadovat jejich pozornost.  Použití dvojitých zařízení umožňuje zařízení a sestavy vyhledávání v reálném provozní podmínky a stavu operace aktualizace. Vytváření sestav výkonné řídicích panelů, které surface bezprostředně problémy pomocí dotazů dvojitých zařízení.

    *Další materiály ke čtení*: [použití vlastností dvojitých][lnk-twin-properties], [je dotazovací jazyk pro úlohy a twins][lnk-query-language]

- **Vyřadit**: nahrazení nebo vyřadit zařízení po selhání, upgrade obrázku, nebo na konci životnost služby.  Pomocí dvou zařízení můžete spravovat informace o zařízení, pokud je zařízení fyzicky nahradit nebo archivují Pokud je už nepoužívá. Pomocí registru IoT Centrum zařízení pro bezpečné odvolání identit zařízení a přihlašovací údaje.

    *Další materiály ke čtení*: [použití vlastností dvojitých][lnk-twin-properties], [Správa identit zařízení][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>Rozbočovač IoT zařízení správy vzorky

Rozbočovač IoT povolí následující sadu vzorků správy zařízení.  [Výukové programy pro správu zařízení] [ lnk-get-started] ukazuje podrobněji, jak prodloužit tyto vzorků na tvar přesné nefunguje a jak navrhnout nový vzorek podle těchto základních šablon.

- **Restartujte** - aplikaci back-end informuje o zařízení přímé metodou ji spustil restartovat počítač.  Zařízení používá zařízení dvojitých vykázaného vlastnosti aktualizovat stav restartujte počítač zařízení.

    ![Obrázek vzorku restartujte Azure IoT centrální správy zařízení][img-reboot_pattern]

- **Obnovení factory** - back-end aplikaci informuje o tom, zařízení přímé metodou ji spustil factory obnovit.  Zařízení používá dvojitých zařízení nahlášeného vlastnosti k aktualizaci výroby obnovit stav zařízení.

    ![Azure factory správy zařízení IoT centrální Obnovit obrázek vzorku][img-facreset_pattern]

- **Konfigurace** – aplikaci back-end používá ke konfiguraci spuštěná na zařízení se vlastnosti dvojitých žádoucí.  Zařízení používá zařízení dvojitých vykázaného vlastností, které lze aktualizovat stav konfigurace zařízení.

    ![Azure IoT Centrum zařízení správy konfigurace vzorek obrázek][img-config_pattern]

- **Aktualizaci firmwaru** - back-end aplikaci informuje o zařízení přímé metodou ji spustil aktualizaci firmwaru.  Zařízení zahájí proces Stáhnout balíček firmwaru, použijte balíček firmware a nakonec opětovné připojení ke službě IoT centrální.  Během procesu koef krok zařízení používá zařízení dvojitých vykázaného vlastnosti aktualizace průběhu a stav zařízení.

    ![Azure firmware správy zařízení IoT centrální aktualizovat vzorek obrázek][img-fwupdate_pattern]

- **Vykazování průběhu a stav** - aplikace back-end spustí zařízení dvojitých dotazy v sadě zařízení vykázat stav a průběh akcí systém na zařízeních.

    ![Azure IoT centrální správy zařízení vykazování průběhu a stavu obrázek vzorku][img-report_progress_pattern]

## <a name="next-steps"></a>Další kroky

Můžete použít funkce, vzorků a kód knihoven, které poskytuje Azure IoT centrální správy zařízení k vytváření IoT aplikací, které odpovídají požadavkům operátor IoT enterprise v rámci v každé fázi životního cyklu zařízení.

Přečtěte si víc o funkcích správy zařízení centrální IoT Azure, najdete v článku [začít používat správu zařízení Azure IoT centrální] [ lnk-get-started] kurz.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md