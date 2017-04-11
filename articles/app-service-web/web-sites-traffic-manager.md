<properties
    pageTitle="Řízení Azure webové aplikace přenosy v síti s Azure přenosy správce"
    description="Souhrnné informace pro správce dopravy Azure obsahuje tento článek se týká Azure webových aplikacích."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Řízení Azure webové aplikace přenosy v síti s Azure přenosy správce

> [AZURE.NOTE] Souhrnné informace pro správce přenosy Microsoft Azure obsahuje tento článek se týká Azure aplikace služby Web Apps. Další informace o Azure správce dopravy samotné najdete tak, že návštěva odkazy na konci tohoto článku.

## <a name="introduction"></a>Úvod
Správce přenosy Azure umožňuje určit, jak jsou žádosti o weboví klienti úměrně web apps v aplikaci služby Azure. Když koncové body webové aplikace se přidají do profilu Azure přenosy správce, správce přenosy Azure uchovává informace o stavu webové aplikace (systém, přestal nebo odstraněné) tak, aby ho můžete se rozhodnout, která tyto koncové body mají dostávat přenosy.

## <a name="load-balancing-methods"></a>Načtení vyrovnávání metody
Tři způsoby Vyrovnávání zatížení různých použije Azure přenosy správce. V následujícím seznamu jsou popsány jako souvisí Azure webových aplikacích.

* **Převzetí**: Pokud máte web app klony v různých oblastech, můžete tímto způsobem můžete nakonfigurovat jeden web appu služby všechny přenosy web klienta a nakonfigurovat jiné webové aplikace v jiné oblasti služby která vysílání v případě, že první web appu nebude k dispozici.

* **Kruhové**: Pokud máte web app klony v různých oblastech, můžete tento způsob distribuovat přenosy rovnoměrně přes web apps v různých oblastech.

* **Výkon**: výkonu metoda distribuuje přenosy podle nejkratší dobu odezvy klientům. Metodu výkon můžete použít pro webové aplikace v rámci stejné oblasti nebo v různých oblastech.

##<a name="web-apps-and-traffic-manager-profiles"></a>Web Apps a profily přenosy správce
Konfigurace ovládací prvek přenosů webové aplikace, vytvoření profilu v Azure správce dopravy, že používá, jednu z těchto tří načíst vyrovnávání metody popsané výše a zadejte koncové body (v tomto případě webové aplikace) pro které chcete určit umožnění datových přenosů do profilu. Stavu webové aplikace (systém, zastavit nebo odstranění) je pravidelně oznamují profilu tak, aby Azure přenosy Správce směrování adres příslušným způsobem.

Při použití Azure přenosy správce v Azure, mějte na paměti následující skutečnosti:

* U webové aplikace pouze nasazení ve stejné oblasti Web Apps už nabízí převzetí a funkce kruhového bez ohledu na režim web app.

* Nasazení ve stejné oblasti, které využívají Web Apps ve spojení s jinou službou Azure cloudu můžete kombinovat obou typů koncové body povolit hybridní scénáře.

* V profilu můžete určit pouze jeden koncový bod web app na oblast. Po výběru do webových aplikací jako koncový bod pro jednu oblast zbývající web apps v dané oblasti výběru pro tento profil nedostupné.

* Koncové body webové aplikace, které zadáte v profilu Azure přenosy správce se zobrazí v části **Názvy domén** na stránce konfigurovat pro web app v profilu, ale nebude možné konfigurovat tam.

* Po přidání do webových aplikací do profilu adresu **URL webu** na řídicím panelu stránku portálu web appu se zobrazí vlastní adresu URL domény web appu, pokud jste nastavili jednu. Jinak, zobrazí se adresa URL profilu přenosy správce (například `contoso.trafficmgr.com`). Obě přímé název domény pro web app a adresu URL správce dopravy bude viditelný na stránce konfigurovat web appu v části **Názvy domén** .

* Vaše vlastní názvy domén funguje očekávaným způsobem, ale kromě je přidat na svůj web aplikace, musíte taky nakonfigurovat mapy DNS tak, aby ukazovaly na adresu URL přenosy správce. Informace o tom, jak nastavit vlastní doménu pro Azure web app najdete v tématu [Konfigurace vlastního názvu domény pro web s Azure](web-sites-custom-domain-name.md).

* Možné přidávat jen webových aplikací web apps, které jsou ve standardním režimu Azure přenosy správce profilu.

## <a name="next-steps"></a>Další kroky

Koncepční a technický přehled správce dopravy Azure najdete v článku [Přehled přenosy správce](../traffic-manager/traffic-manager-overview.md).

Další informace o používání správce dopravy pomocí webových aplikací Web Apps najdete že příspěvky blogu [Pomocí správce dopravy Azure s Azure weby](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) a [správce dopravy Azure teď můžete integrovat s Azure weby](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).
