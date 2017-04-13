<properties
   pageTitle="Konfigurace metodu směrování přenosu výkonu | Microsoft Azure"
   description="Tento článek vám pomůže konfigurace metodu směrování přenosu výkonu ve Správci přenosy"
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-performance-traffic-routing-method"></a>Konfigurace metodu směrování přenosu výkonu

Pokud chcete směrovat přenosy v síti pro cloudovými službami a weby (koncové body), které se nacházejí v různých datacentrech po celém světě (označovaná taky jako oblastí), můžete odkázat příchozích koncový bod s nejnižší latence od klienta. Obvykle datacentra s nejnižší latence odpovídá nejbližšího v zeměpisné vzdálenost. Metoda směrování přenosu výkonu vám umožní distribuce založené na nejnižší latence, ale nelze vzít v úvahu v reálném čase změny v konfiguraci sítě nebo načíst. Další informace o směrování metody jiný přenos, které poskytuje Azure přenosy správce najdete v článku [o správce přenosy přenosy směrování metody](traffic-manager-routing-methods.md).

## <a name="route-traffic-based-on-lowest-latency-across-a-set-of-endpoints"></a>Směrovat přenosy v síti založené na nejnižší latence přes sadu koncové body:

1. Azure klasické portálu v levém podokně klikněte na ikonu **Přenosy správce** otevřete podokno přenosy správce. Pokud jste ještě nevytvořili svůj profil správce přenosy, najdete v článku [Správa profilů přenosy správce](traffic-manager-manage-profiles.md) postup se dá vytvořit základní profil správce přenosy.
2. Na portálu v podokně přenosy správce Azure klasické vyhledejte správce přenosy profil, který obsahuje nastavení, které chcete změnit a pak klikněte na šipku vpravo od názvu profilu. Tím se otevře stránka nastavení profilu.
3. Na stránce profilu klikněte na **koncové body** v horní části stránky a ověřit, zda koncové body služby, které chcete zahrnout do konfiguraci. Postup přidání nebo odebrání koncové body ze svého profilu najdete v článku [Správa koncové body ve Správci přenosy](traffic-manager-endpoints.md).
4. Na stránce profilu klikněte na **Konfigurovat** nahoře a otevře se stránka konfigurace.
5. **Způsob nastavení směrování přenosu**zkontrolujte, že metoda směrování přenosu * *výkonu*. Pokud není, klikněte na * *výkonu** v rozevíracím seznamu.
6. Ověřte, že je **Sledování nastavení** nakonfigurované řádně podporovat. Sledování zaručuje, že koncové body, které jsou v režimu offline neodesílají přenosy. Abyste mohli sledovat koncové body, zadejte cestu a název souboru. Všimněte si, že lomítka "/" je platnými položkami pro relativní cestu a znamená, že soubor databáze v kořenovém adresáři (výchozí). Další informace o sledování najdete v tématu [O přenosy správce sledování](traffic-manager-monitoring.md).
7. Po dokončení změny konfigurace, klikněte na **Uložit** v dolní části stránky.
8. Otestujte změny v konfiguraci. Další informace najdete v tématu [Testování nastavení přenosy správce](traffic-manager-testing-settings.md).
9. Po nastavení a práce se svůj profil správce přenosy upravte záznam DNS na autoritativní server DNS tak, aby ukazovaly na název domény přenosy správce názvu domény společnosti. Další informace o tom, jak to udělat najdete v článku [čárky internetovou doménu společnosti přenosy správce domény](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Další kroky


[Internetovou doménu společnosti přejděte na správce přenos domény](traffic-manager-point-internet-domain.md)

[Metody směrování přenosu správce](traffic-manager-routing-methods.md)

[Konfigurace metodu směrování překlopení](traffic-manager-configure-failover-routing-method.md)

[Konfigurace metodu směrování kruhového](traffic-manager-configure-round-robin-routing-method.md)

[Poradce při potížích přenosy správce stavu sníženou kvalitu.](traffic-manager-troubleshooting-degraded.md)

[Vysílání povolit správce – zakázat, nebo odstranění profilu](disable-enable-or-delete-a-profile.md)

[Přenosy správce – zakázání nebo povolení koncový bod](disable-or-enable-an-endpoint.md)

