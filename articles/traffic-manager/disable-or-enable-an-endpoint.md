<properties
   pageTitle="Zakázání nebo povolení koncový bod přenosy správce | Microsoft Azure"
   description="Tento článek vám pomůže zakázání nebo povolení koncové body váš správce přenosy profilu."
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

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>Zakázání nebo povolení koncový bod přenosy správce

Můžete taky zakázat jednotlivé koncové body, které jsou součástí profil přenosy správce. Koncové body zahrnout cloudovými službami a weby. Zakázání koncový bod zůstane jako součást profilu, ale profilu chová jako koncový bod není zahrnut do něj. Tato akce je velmi užitečné pro dočasně odstranění koncového bodu, který je v režimu údržby nebo opětovném nasazení. Jakmile koncový bod opět začátcích, lze povolit

>[AZURE.NOTE] **Zakázání koncový bod nesouvisí s stavu nasazení v Azure. Správný koncový bod zůstane nahoru a nedostáváte přenosy i v případě, že zakázané ve Správci přenosy. Kromě toho zakázání koncového bodu v jednom profilu nemá vliv na svůj stav u jiného profilu.**

## <a name="to-disable-an-endpoint"></a>Jak zakázat koncový bod

1. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje, které chcete změnit nastavení koncového bodu a potom klikněte na šipku vpravo od názvu profilu. Tím se otevře stránka nastavení profilu.
1. V horní části stránky klikněte na **koncové body** zobrazíte koncové body, které jsou součástí konfiguraci.
1. Klikněte na koncový bod, který chcete zakázat a potom klikněte na **Zakázat** v dolní části stránky.
1. Přenosy se zastaví toku koncový bod podle DNS čas to-Live (TTL) nakonfigurován pro název domény přenosy správce. Na stránce konfigurace přenosy správce profilu můžete změnit hodnotu TTL.

## <a name="to-enable-an-endpoint"></a>Chcete-li povolit koncový bod


1. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje, které chcete změnit nastavení koncového bodu a potom klikněte na šipku vpravo od názvu profilu. Tím se otevře stránka nastavení profilu.
1. V horní části stránky klikněte na **koncové body** zobrazíte koncové body, které jsou součástí konfiguraci.
1. Klikněte na koncový bod, který chcete povolit a potom klikněte na **Povolit** v dolní části stránky.
1. Přenosy se začne toku ke službě znovu jako nadiktovaná profilu.

## <a name="next-steps"></a>Další kroky

[Vysílání povolit správce – zakázat, nebo odstranění profilu](disable-enable-or-delete-a-profile.md)

[Poradce při potížích přenosy správce stavu sníženou kvalitu.](traffic-manager-troubleshooting-degraded.md)

[Důležité informace o výkonu přenosy správce](traffic-manager-performance-considerations.md)