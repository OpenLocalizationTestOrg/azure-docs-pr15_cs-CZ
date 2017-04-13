<properties
    pageTitle="Správa koncové body v Azure přenosy správce | Microsoft Azure"
    description="Tento článek vám pomůže přidat odebrat, povolení, zakázání koncové body Azure přenosy ve Správci systému."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="add-disable-enable-or-delete-endpoints"></a>Přidání, zakázat, povolit nebo odstranění koncové body

Funkce Web Apps v aplikaci služby Azure už poskytuje převzetí a kruhového přenosy směrování funkcí pro weby v rámci datacentru, bez ohledu na režim webu. Azure přenosy správce vám umožní určit převzetí a kruhového přenosy směrování pro weby a cloudové služby v jiném datacentrech. Cílem prvního kroku nutné zajistit, že funkce přidáte do cloudové služby nebo web koncového bodu správci přenosy.

>[AZURE.NOTE]  Tento článek vysvětluje, jak používat portál klasické. Portál Azure klasické podporuje pouze vytvoření a přiřazení cloudovými službami a webových aplikací Web apps jako koncové body. Nový [Azure portál](https://portal.azure.com) je upřednostňované rozhraní.

Můžete taky zakázat jednotlivé koncové body, které jsou součástí profil přenosy správce. Zakázání koncový bod zůstane jako součást profilu, ale profilu chová jako koncový bod není zahrnut do něj. Tato akce je užitečné pro dočasně odstranění koncového bodu, který je v režimu údržby nebo opětovném nasazení. Jakmile koncový bod opět začátcích, může být užitečné.

>[AZURE.NOTE] Zakázání koncový bod nesouvisí s stavu nasazení v Azure. Správný koncový bod zůstane nahoru a nedostáváte přenosy i v případě, že zakázané ve Správci přenosy. Kromě toho zakázání koncového bodu v jednom profilu nemá vliv na svůj stav u jiného profilu.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Pokud chcete přidat cloudové služby nebo web koncový bod

1. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje koncový bod nastavení, které chcete změnit. Otevřete stránku nastavení, klikněte na šipku vpravo od názvu profilu.
2. V horní části stránky klikněte na **koncové body** zobrazíte koncové body, které jsou již součástí konfiguraci.
3. V dolní části stránky klepněte na tlačítko **Přidat** pro přístup na stránku **Přidat koncové body služby** . Ve výchozím nastavení na stránce zobrazí cloudových služeb v části **Koncové body služby**.
4. Cloudové služby vyberte ze seznamu a přidat jeho členy jako koncové body v tomto profilu cloudových služeb. Vymazání název cloudové služby, odebere se ze seznamu koncové body.
5. Weby klikněte na **Typ služby** rozevírací seznam a vyberte **Web appu**.
6. V seznamu a přidat jeho členy jako koncové body pro tento profil vyberte weby. Vymazání název webu odebere ze seznamu koncové body. Můžete vybrat pouze jeden web na Azure datacentra (neboli oblast). Po výběru prvního webu k dispozici pro výběr jiných webů ve stejném datacentra. Navíc nezapomeňte, že jsou uvedeny pouze standardní weby.
7. Když vyberete koncové body v tomto profilu, klikněte na zaškrtnutí v pravé dolní uložte provedené změny.

>[AZURE.NOTE] Po přidání nebo odebrání koncový bod z profilu metodou *převzetí* přenosy směrování seznamu priorita převzetí nemusí být objednali budou tak, jak chcete. Můžete upravit pořadí seznamu priorita převzetí na stránce konfigurace. Další informace najdete v článku [směrování přenosu konfigurace překlopení](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Jak zakázat koncový bod

1. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje koncový bod nastavení, které chcete změnit. Otevřete stránku nastavení, klikněte na šipku vpravo od názvu profilu.
2. V horní části stránky klikněte na **koncové body** zobrazíte koncové body, které jsou součástí konfiguraci.
3. Klikněte na koncový bod, který chcete zakázat a potom klikněte na **Zakázat** v dolní části stránky.
4. Klienti dál posílat přenosy na koncový bod dobu trvání Time-to-Live (TTL). Můžete změnit TTL na stránce konfigurace profilu přenosy správce.

## <a name="to-enable-an-endpoint"></a>Chcete-li povolit koncový bod

1. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje koncový bod nastavení, které chcete změnit. Otevřete stránku nastavení, klikněte na šipku vpravo od názvu profilu.
2. V horní části stránky klikněte na **koncové body** zobrazíte koncové body, které jsou součástí konfiguraci.
3. Klikněte na koncový bod, který chcete povolit a potom klikněte na **Povolit** v dolní části stránky.
4. Klienti jsou směrovány povolené koncový bod podle profilu.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Chcete-li odstranit cloudové služby nebo web koncový bod

1. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje koncový bod nastavení, které chcete změnit. Otevřete stránku nastavení, klikněte na šipku vpravo od názvu profilu.
2. V horní části stránky klikněte na **koncové body** zobrazíte koncové body, které jsou již součástí konfiguraci.
3. Na stránce koncové body klikněte na název koncového bodu, který chcete odstranit z profilu.
4. V dolní části stránky klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

* [Správa profilů přenosy správce](traffic-manager-manage-profiles.md)
* [Konfigurace metody směrování](traffic-manager-configure-routing-method.md)
* [Poradce při potížích přenosy správce stavu sníženou kvalitu.](traffic-manager-troubleshooting-degraded.md)
* [Důležité informace o výkonu přenosy správce](traffic-manager-performance-considerations.md)
* [Operace na přenosy správce (REST API odkazy)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
