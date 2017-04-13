<properties
    pageTitle="Jak odeslat plánované oznámení | Microsoft Azure"
    description="Toto téma popisuje naplánované oznámení pomocí rozbočovače oznámení Azure."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="nabízená oznámení, nabízená oznámení, plánování nabízená oznámení"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Jak: Odeslání plánované oznámení


##<a name="overview"></a>Základní informace

Pokud máte scénáři, ve kterém chcete odeslat oznámení v budoucnu nastane okamžik, ale nemusíte snadný způsob, jak aktivovat kód back-end k odeslání oznámení. Standardní osy oznámení rozbočovače podporuje funkci, která umožňuje plánování oznámení až 7 dní v budoucnosti.

Při odesílání oznámení, jednoduše používejte [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) třídu v SDK rozbočovače oznámení jak je vidět v následujícím příkladu:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Také můžete zrušit dřív plánované oznámení pomocí jeho notificationId:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Nejsou žádná omezení počtu plánované upozornění, které můžete poslat.