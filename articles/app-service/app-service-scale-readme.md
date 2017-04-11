<properties
    pageTitle="Azure aplikaci služby: Změna měřítka aplikací aplikace služby"
    description="Další výhody a nevýhody skryté nastavení velikosti aplikace v aplikaci služby."
    keywords="aplikace služby azure aplikace služby měřítko scalable, plán služeb aplikací, aplikací služby náklady"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>Azure aplikaci služby: Změna měřítka aplikací aplikace služby

Aplikace umístěné v aplikaci služby Azure můžete [dosáhnout rozsáhlé měřítko](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Změna měřítka aplikace je však složité problém, který nemá řešení "jeden velikost bude vyhovovat všem". Vaše aplikace jsou správně měřítko 3 klíčové oblasti, které bude přispívají k úspěšnost aplikací:

1. Principy architektuře aplikace a její slabých.
    * Je stavovou aplikace? Příslušnosti?
    * Jaké jsou všechny součásti aplikace?
        * Kde jsou kritické body v aplikaci?
    * Při načítání platí pro aplikace, co přerušíte první?
2. Principy očekávané požadavků zatížení a výkon.
    * Aplikace musím vytisknout faktorem 1000 uživatelů? nebo jednoho milionu?
    * Přenosy se objeví z jednoho zeměpisná místa nebo globálně?
    * Nejdou nějaké sezónní varianty? přenosy píků?
    * Jak rychle by měly odpovídat aplikace? 1 druhé? 1 milisekund?
3. Principy a správně využít platformu hostingu aplikace.
    * Jaké funkce využít k dosažení Moje měřítko cíle?

V této části vám pomůže pochopit faktorů a nápovědy navrhnout strategii využívá potřebné funkce aplikaci služby k dosažení škálovatelnost cílů.

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]
