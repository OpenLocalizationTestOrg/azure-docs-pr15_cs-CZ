<properties
    pageTitle="Zabezpečení pro rozbočovače oznámení"
    description="Toto téma vysvětluje zabezpečení pro rozbočovače Azure oznámení."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="security"></a>Zabezpečení

##<a name="overview"></a>Základní informace

Toto téma popisuje model zabezpečení rozbočovače oznámení Azure. Vzhledem k tomu rozbočovače oznámení služby Bus entitu, implementaci stejný model zabezpečení jako Bus služby. Další informace najdete v tématech [Ověření Bus služby](https://msdn.microsoft.com/library/azure/dn155925.aspx) .

##<a name="shared-access-signature-security-sas"></a>Sdílený přístup podpis zabezpečení (přidružení zabezpečení) 

Oznámení o rozbočovače implementuje schématu entity úrovně zabezpečení Schvalte přidružení zabezpečení (sdílené přístup podpis). Tohoto režimu umožňuje zpráv entity deklarovat až 12 pravidla se tak mohli ověřovat v jeho popis, která udělit práva na entity.

Každé pravidlo obsahuje název, s hodnotou klíče (sdílené tajná) a sadu práva příjemce, způsobem popsaným v části "Zabezpečení deklarací." Při vytváření rozbočovači oznámení, budou dvě pravidla vytvořeny automaticky: jeden právy poslech (používané aplikaci klienta) a druhý se všemi právy (používané back-end aplikaci).

Při registraci správy z klienta aplikací ty informace odeslané prostřednictvím oznámení nerozlišuje malá písmena (například počasí aktualizací), běžným způsobem pro přístup k rozbočovači oznámení dát hodnoty klíče pravidla pouze poslech přístup k aplikaci klienta a dát hodnoty klíče pravidlo úplný přístup k aplikaci back-end.

Nedoporučujeme vložit hodnoty klíče klientské aplikace pro Windows Store. Způsob, jak nemuseli vkládat hodnoty klíče je aplikaci klienta načíst z app back-end při spuštění.

Je důležité pochopit klávesu s přístupem poslech umožňuje klientské aplikace pro registraci na libovolnou značku. Pokud aplikace nutné omezit registrace ke konkrétní značky konkrétní klientům (například pokud značky představují ID uživatele), musíte provést serverové části aplikace registrace. Další informace najdete v tématu Správa registrace. Všimněte si, že tímto způsobem, nebudou mít klientské aplikace přímý přístup k oznámení rozbočovače.

##<a name="security-claims"></a>Deklarace zabezpečení

Podobně jako jiné entity Oznámení centrální operace je povolený limit pro tři deklarací zabezpečení: poslechněte, Odeslat a spravovat.

| Deklarace | Popis | Povolené operace |
|-------|-------------|--------------------|
| Poslech | Vytvoření nebo aktualizace číst a odstranit jeden registrace | Vytvoření nebo aktualizace registrace<br><br>Registrace pro čtení<br><br>Přečtěte si všechny registrace pro ty<br><br>Odstranění registrace |
| Odeslání | Odesílat zprávy centru oznámení | Odeslání zprávy |
| Správa | CRUDs oznámení rozbočovače (včetně aktualizací PNS přihlašovacích údajů a zabezpečení klíče) a čtení registrace podle značky | Vytvoření, aktualizace nebo pro čtení nebo odstranění oznámení rozbočovače<br><br>Registrace ke čtení podle značky |


Oznámení o rozbočovače přijmout deklarací udělena řízení přístupu k Microsoft Azure tokenů tak podpisu tokeny generovaného s sdílených klíčů nakonfigurované přímo v centru oznámení.