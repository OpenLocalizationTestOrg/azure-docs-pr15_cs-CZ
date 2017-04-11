<properties
    pageTitle="Co je Azure události rozbočovače? | Microsoft Azure"
    description="Přehled a popis události rozbočovače Azure"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="sethm"/>

# <a name="what-is-azure-event-hubs"></a>Co je Azure události rozbočovače?

Azure rozbočovače událostí je služba průniku vysoce scalable data, která můžete jedí miliony události sekundu tak, aby se daly procesu a analyzovat větší dat vytvořené pomocí připojení zařízení nebo aplikací. Událost rozbočovače funguje jako "dveří přední" pro událost kanálu a po údaje rozbočovači událost, můžete transformovat a uložené kteréhokoliv poskytovatele v reálném čase analýzy nebo adaptéry dávky/úložiště. Událost rozbočovače zruší párování výrobní toku události z spotřebu z těchto skutečností tak, aby příjemci události můžete získat přístup k události vlastní plánu. Další informace a technické podrobnosti najdete v tématu [Přehled rozbočovače události](event-hubs-overview.md).

## <a name="event-hubs-capabilities"></a>Funkce rozbočovače události

Událost rozbočovače je události zpracování služba, která poskytuje událostí a zpracování telemetrie ve velkém měřítku obrovské, s nízkou latence a vysoký spolehlivost. Tato služba je užitečné především pro:

- Aplikace přístrojového vybavení
- Uživatelské prostředí nebo zpracování pracovního postupu
- Scénáře Internet věcí (IoT)

Některé další možnosti klíčových událostí rozbočovače zahrnovat chování sledování v mobilních aplikacích, přenosu informací z webové serverové farmy, zachycení v hra událostí v konzole her nebo telemetrie odebrané průmyslového strojů nebo připojeného vozidla.

## <a name="next-steps"></a>Další kroky

Podrobné informace o události rozbočovače naleznete v následujících tématech.

- [Přehled rozbočovače událostí](event-hubs-overview.md)
- [Událost rozbočovače programování Průvodce](event-hubs-programming-guide.md)
- [Událost rozbočovače dostupnost a podporu – nejčastější dotazy](event-hubs-availability-and-support-faq.md)
- Začínáme s [kurz rozbočovače události][]
- Kompletní [Ukázková aplikace, který využívá rozbočovače události][]

[Kurz rozbočovače události]: event-hubs-csharp-ephcs-getstarted.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
