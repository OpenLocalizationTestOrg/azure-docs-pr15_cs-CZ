<properties
   pageTitle="Zakázat, povolení nebo odstranění profilu přenosy správce | Microsoft Azure"
   description="Tento článek vám pomůže pracovat s profily přenosy správce."
   services="traffic-manager"
   documentationCenter="na"
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

# <a name="disable-enable-or-delete-a-profile"></a>Zakázání, povolení nebo odstranění profilu


Existující profil správce přenosy můžete zakázat, takže nebude odkázat koncových uživatelů na jeho nakonfigurované koncové body. Po zákazu profil správce přenosy, samotném profilu a informace obsažené v profilu zůstane nedotčený a může upravovat v rozhraní přenosy správce. Pokud chcete znovu povolit profilu, můžete snadno to v Azure portálem a odkazy obnoví. Při vytváření profil přenosy správce na portálu Azure automaticky zapnutá.

## <a name="to-disable-a-profile"></a>Jak zakázat profilu

1. Upravte záznam o prostředku DNS na server DNS sítě Internet používání vhodné record type a odkaz na jiný název nebo IP adresu na určité místo na Internetu. Jinými slovy změníte záznam o prostředku DNS na Internetu DNS server tak, aby používala už zdroje záznam CNAME, který odkazuje na název domény pro svůj profil správce dopravy.
1. Přenosy se zastaví byli přesměrováni koncové body prostřednictvím nastavení profilu přenosy správce.
1. Vyberte profil, který chcete zakázat. K výběru profilu, na stránce Správce přenosy zvýraznění kliknutím do sloupce vedle název profilu na profil. Jak to přejdete na stránku nastavení profilu je třeba kliknout na název profilu nebo na šipku vedle názvu.
1. Po výběru profilu, klikněte na zakázat v dolní části stránky.

## <a name="to-enable-a-profile"></a>Chcete-li povolit profilu

1. Vyberte profil, který chcete povolit. K výběru profilu, na stránce Správce přenosy zvýraznění kliknutím do sloupce vedle název profilu na profil. Jak to přejdete na stránku nastavení profilu je třeba kliknout na název profilu nebo na šipku vedle názvu.
1. Po výběru profilu, klikněte na povolit v dolní části stránky.
1. Upravte záznam o prostředku DNS na server DNS sítě Internet používat typ záznamu CNAME, který odpovídá názvu domény společnosti název domény pro svůj profil správce dopravy. Další informace najdete v tématu [čárky internetovou doménu společnosti přenosy správce domény](traffic-manager-point-internet-domain.md).
1. Provoz zahájí byli přesměrováni koncové body znovu.

## <a name="delete-a-profile"></a>Odstranění profilu


1. Zajistit, aby záznam o prostředku DNS na Internetu DNS server už používala zdroje záznam CNAME, který odkazuje na název domény pro svůj profil správce dopravy.
1. Vyberte profil, který chcete odstranit. K výběru profilu, na stránce Správce přenosy zvýraznění profilu tak, že
1. Kliknutím do sloupce vedle profilu. Jak to přejdete na stránku nastavení profilu je třeba kliknout na název profilu nebo na šipku vedle názvu.
1. Po výběru profilu, klikněte na Odstranit v dolní části stránky.

## <a name="next-steps"></a>Další kroky

[Přenosy správce – zakázání nebo povolení koncový bod](disable-or-enable-an-endpoint.md)

[Konfigurace metodu směrování překlopení](traffic-manager-configure-failover-routing-method.md)

[Konfigurace metodu směrování kruhového](traffic-manager-configure-round-robin-routing-method.md)

[Konfigurace metodu směrování výkonu](traffic-manager-configure-performance-routing-method.md)

[Poradce při potížích přenosy správce stavu sníženou kvalitu.](traffic-manager-troubleshooting-degraded.md)

