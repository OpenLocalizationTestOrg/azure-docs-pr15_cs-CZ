<properties
   pageTitle="Správa koncové body v Azure přenosy správce | Microsoft Azure"
   description="Tento článek vám pomůže přidat odebrat, povolení, zakázání koncové body Azure přenosy ve Správci systému."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Přidání, zakázat, povolit nebo odstranění koncové body

Funkce Web Apps v aplikaci služby Azure už poskytuje převzetí a kruhového přenosy směrování funkcí pro weby v rámci datacentru, bez ohledu na režim webu. Azure přenosy správce vám umožní určit převzetí a kruhového přenosy směrování pro weby a cloudové služby v jiném datacentrech. Cílem prvního kroku nutné zajistit, že funkce přidáte do cloudové služby nebo web koncového bodu správci přenosy.

>[AZURE.NOTE] Jako koncové body na portálu Azure klasické nemůžete přidat externí umístění nebo profilů přenosy správce. Je nutné použít rozhraní REST API [Vytvořit definice](http://go.microsoft.com/fwlink/p/?LinkId=400772) nebo prostředí Windows PowerShell [AzureTrafficManagerEndpoint přidat](http://go.microsoft.com/fwlink/p/?LinkId=400774).

Můžete taky zakázat jednotlivé koncové body, které jsou součástí profil přenosy správce. Koncové body zahrnout cloudovými službami a weby. Zakázání koncový bod zůstane jako součást profilu, ale profilu chová jako koncový bod není zahrnut do něj. Tato akce je velmi užitečné pro dočasně odstranění koncového bodu, který je v režimu údržby nebo opětovném nasazení. Jakmile koncový bod opět začátcích, může být užitečné.

>[AZURE.NOTE] Zakázání koncový bod nesouvisí s stavu nasazení v Azure. Správný koncový bod zůstane nahoru a nedostáváte přenosy i v případě, že zakázané ve Správci přenosy. Kromě toho zakázání koncového bodu v jednom profilu nemá vliv na svůj stav u jiného profilu.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Pokud chcete přidat cloudové služby nebo web koncový bod


1. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje, které chcete změnit nastavení koncového bodu a potom klikněte na šipku vpravo od názvu profilu. Tím se otevře stránka nastavení profilu.
2. V horní části stránky klikněte na **koncové body** zobrazíte koncové body, které jsou již součástí konfiguraci.
3. V dolní části stránky klepněte na tlačítko **Přidat** pro přístup na stránku **Přidat koncové body služby** . Ve výchozím nastavení na stránce zobrazí cloudových služeb v části **Koncové body služby**.
4. Cloudové služby vyberte v seznamu povolit jako koncové body v tomto profilu cloudových služeb. Vymazání název cloudové služby, odebere se ze seznamu koncové body.
5. Weby klikněte na **Typ služby** rozevírací seznam a vyberte **Web appu**.
6. V seznamu a přidat jeho členy jako koncové body pro tento profil vyberte weby. Vymazání název webu odebere ze seznamu koncové body. Všimněte si, že můžete zvolit pouze jeden web na Azure datacentra (neboli oblast). Pokud vyberete web ve datacentru, který je hostitelem více webů, po výběru prvního webu, jiné ve stejném datacentra nedostupné pro výběr. Navíc nezapomeňte, že jsou uvedeny pouze standardní weby.
7. Když vyberete koncové body v tomto profilu, klikněte na zaškrtnutí v pravé dolní uložte provedené změny.

>[AZURE.NOTE] Pokud používáte *převzetí* přenosy metodu směrování, po přidání nebo odebrání koncový bod, je potřeba upravit seznamu priorita převzetí na stránce konfigurace tak, aby odrážely převzetí pořadí, ve kterém chcete použít pro konfiguraci. Další informace najdete v článku [směrování přenosu konfigurace překlopení](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Jak zakázat koncový bod

1. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje, které chcete změnit nastavení koncového bodu a potom klikněte na šipku vpravo od názvu profilu. Tím se otevře stránka nastavení profilu.
2. V horní části stránky klikněte na **koncové body** zobrazíte koncové body, které jsou součástí konfiguraci.
3. Klikněte na koncový bod, který chcete zakázat a potom klikněte na **Zakázat** v dolní části stránky.
4. Přenosy se zastaví toku koncový bod podle DNS čas to-Live (TTL) nakonfigurován pro název domény přenosy správce. Na stránce konfigurace přenosy správce profilu můžete změnit hodnotu TTL.

## <a name="to-enable-an-endpoint"></a>Chcete-li povolit koncový bod

1. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje, které chcete změnit nastavení koncového bodu a potom klikněte na šipku vpravo od názvu profilu. Tím se otevře stránka nastavení profilu.
2. V horní části stránky klikněte na **koncové body** zobrazíte koncové body, které jsou součástí konfiguraci.
3. Klikněte na koncový bod, který chcete povolit a potom klikněte na **Povolit** v dolní části stránky.
4. Přenosy se začne toku ke službě znovu jako nadiktovaná profilu.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Chcete-li odstranit cloudové služby nebo web koncový bod


1. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje, které chcete změnit nastavení koncového bodu a potom klikněte na šipku vpravo od názvu profilu. Tím se otevře stránka nastavení profilu.
2. V horní části stránky klikněte na **koncové body** zobrazíte koncové body, které jsou již součástí konfiguraci.
3. Na stránce koncové body klikněte na název koncového bodu, který chcete odstranit z profilu.
4. V dolní části stránky klikněte na **Odstranit**.

>[AZURE.NOTE] Jako koncové body na portálu Azure klasické nelze odstranit externí umístění nebo profilů přenosy správce. Musíte používat Windows PowerShell. Další informace najdete v tématu [AzureTrafficManagerEndpoint odebrat](https://msdn.microsoft.com/library/dn690251.aspx).

## <a name="next-steps"></a>Další kroky

- [Konfigurace metodu směrování překlopení](traffic-manager-configure-failover-routing-method.md)
- [Konfigurace metodu směrování kruhového](traffic-manager-configure-round-robin-routing-method.md)
- [Konfigurace metodu směrování výkonu](traffic-manager-configure-performance-routing-method.md)
- [Poradce při potížích přenosy správce stavu sníženou kvalitu.](traffic-manager-troubleshooting-degraded.md)
- [Operace na přenosy správce (REST API odkazy)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
