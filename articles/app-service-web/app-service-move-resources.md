<properties
    pageTitle="Přesunutí webové aplikace prostředky do jiné skupiny zdrojů"
    description="Popisuje scénáře, kde můžete přesunout Web Apps a aplikace služeb z jednoho pole Skupina zdroje do jiného."
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>Podporované přesunout konfiguraci

Můžete přesouvat prostředky Azure Web Appu pomocí rozhraní [Api zdroje přesunout ARM](../resource-group-move-resources.md).

Azure Web Apps v současné době podporuje následující přesunout scénáře:

* Přesunutí celý obsah skupina zdroje (webových aplikací web apps, aplikace služby plány a certifikáty) do jiné skupiny zdrojů 
    * Poznámka: Cílové skupině zdroje nesmí obsahovat Microsoft.Web zdroje v tomto scénáři
* Přesunutí jednotlivých webových aplikací do jiné skupině zdrojů, při pořád hostingu v jejich aktuální plán služeb aplikací (plán služeb aplikací zůstává v původní skupina zdroje)
