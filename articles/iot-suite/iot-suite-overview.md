<properties
    pageTitle="Přehled Microsoft Azure IoT Suite | Microsoft Azure"
    description="Přehled, jak Azure IoT sadu poskytuje internet předkonfigurované řešení věci shromáždit analyzovat a ukládání dat, zadejte vizualizace a integrace s jiných systémů."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# <a name="what-is-azure-iot-suite"></a>Co je sada IoT Azure?

Azure internetových služeb věcí (IoT) nabízí řadu možností. Tyto služby testu enterprise umožňují:

- Shromáždit data od zařízení
- Analýza dat toky v pohybu
- Ukládání a velkých datových sad dotazu
- Vizualizace dat v reálném čase a historie
- Integrace se systémy office zpět

Předvádění tyto možnosti, sadu IoT Azure balíčků společně více služby Azure pomocí vlastní rozšíření jako *předkonfigurované řešení*. Tyto předem řešení jsou základní implementace vzorů běžné IoT řešení, které pomáhají zkrátit, kterými předvádění IoT řešení. Použití [IoT software development Kit][lnk-sdks], přizpůsobení a rozšíření tato řešení vlastní požadavkům. Taky můžete tato řešení jako příklady nebo šablony při vytváření nových IoT řešení.

Následující video poskytuje přehled Azure IoT sadě:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT služby Azure IoT sady

Předkonfigurované řešení obvykle používají následující služby:

- Základní sadě IoT Azure je [Centrum IoT Azure] [ lnk-iot-hub] služby. Tato služba poskytuje funkce zasílání zpráv zařízení do cloudu a cloudu zařízení a slouží jako brány do cloudu a dalších služeb klíčové IoT sadu. Služba umožňuje přijímat zprávy z vašeho zařízení ve velkém měřítku a příkazy posílat do svých zařízeních.

- [Azure analýzy toku] [ lnk-asa] poskytuje analýza dat v pohybu. Sadu IoT využívá tuto službu zpracovat příchozí telemetrie, provádět agregace a zjistit události. Předkonfigurované řešení také pomocí analýzy toku zpracuje informační zprávy, které obsahují data například metadata nebo příkaz odpovědi ze zařízení. Řešení pomocí analýzy toku zpracování zpráv ze svého zařízení a k předání těchto zprávy jiných služeb.

- [Azure úložiště] [ lnk-azure-storage] a [Azure DocumentDB] [ lnk-document-db] poskytují možnosti ukládání dat. Předkonfigurované řešení pomocí úložiště objektů blob pro ukládání telemetrie a být k dispozici pro analýzy. Řešení DocumentDB umožňuje ukládat metadata zařízení a povolení možností správy zařízení řešení.

- [Azure Web Apps] [ lnk-web-apps] a [Microsoft Power BI] [ lnk-power-bi] poskytují možnosti vizualizaci dat. Flexibilní Power BI umožňuje rychle vytvářet vlastní interaktivní řídicí panely, které využívají data IoT sadu.

Základní informace o architekturu typické IoT řešení, najdete v článku [Microsoft Azure a Internet věcí (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Předkonfigurované řešení

Sadu IoT obsahuje předkonfigurované řešení, které umožňují rychle začít pracovat s a prozkoumání obvyklé scénáře IoT, například *vzdálené sledování* a *prediktivní údržby*, umožní Azure IoT sadu. Můžete nasadit tato řešení k předplatnému Azure a znovu spusťte scénáři IoT dokončeno, začátku do konce.

## <a name="next-steps"></a>Další kroky

Teď, když máte Následuje přehled toho, co můžete dělat IoT sady a jaké hlavní součásti, můžete získat další informace o předkonfigurované řešení sady IoT naleznete v tématu [Co jsou Azure IoT automaticky předem nakonfigurovaná řešení?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
