<properties
    pageTitle="Změna velikosti počet instancí, ať už ručně nebo automaticky | Microsoft Azure"
    description="Zjistěte, jak zobrazit služby Azure."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="scale-instance-count-manually-or-automatically"></a>Změna velikosti počet instancí, ať už ručně nebo automaticky

Na [Portálu Azure](https://portal.azure.com/), můžete ručně nastavit počet instancí služby nebo, můžete nastavit parametry ji automaticky měřítko podle služba. Tím se obvykle nazývá *měřítko,* případně *měřítko v*.

Před měřítka založená na počtu instance, byste měli zvážit, že měřítko je ovlivněna **ceny osy** kromě počet instancí. Různé ceny úrovní můžou mít různý jádra a paměti a to bude mít lepší výkon pro stejný počet výskytů (což je *Měřítko nahoru* nebo *Měřítko dolů*). Tento článek popisuje konkrétně *velkém měřítku v* a *out*.

Můžete změnit velikost na portálu a taky můžete [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx) nebo [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) upravit měřítko, ať už ručně nebo automaticky.

> [AZURE.NOTE] Tento článek popisuje, jak k vytvoření nastavení automatické měřítko na portálu na [http://portal.azure.com](http://portal.azure.com). Nastavení automatické měřítko vytvořené v tomto portálu se nedají upravovat portálu klasické ([http://manage.windowsazure.com](http://manage.windowsazure.com)).

## <a name="scaling-manually"></a>Ruční změna velikosti

1. [Portál Azure](https://portal.azure.com/)klikněte na tlačítko **Procházet**a potom přejděte na zdroj, který chcete změnit velikost, například na **plán služeb aplikací**.

2. Dlaždice **Měřítko** při **operacích** zjistíte, stav měřítko: **vypnutí** když jsou **na** Pokud jste měřítka podle jednoho nebo několika metrik výkonu ručně, měřítka.
    ![Dlaždice měřítko](./media/insights-how-to-scale/Insights_UsageLens.png)

3. Po kliknutí na dlaždici se vrátíte na zásuvné **Měřítko** . V horní části měřítko zásuvné zobrazíte historii automatické měřítko akce služby.  
    ![Zásuvné měřítko](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)

>[AZURE.NOTE] Pouze akce, které provádí automatické měřítko zobrazí se v tomto grafu. Pokud upravíte ručně počet instancí, změny se projeví v tomto grafu.

4. Můžete ručně upravit číselné **instance** pomocí posuvníku.
5. Klikněte na příkaz **Uložit** a kterou budete měnit jejich velikost na daný počet výskytů téměř okamžitě.

## <a name="scaling-based-on-a-pre-set-metric"></a>Změna měřítka podle předdefinovaných metriky

Podle potřeby počet instancí automaticky upravit podle metriky vyberte v rozevíracím seznamu **měřítko tak, že** míru, které chcete. Například pro **plán služeb aplikací** se můžete zobrazit jako **Procento využití procesoru**.

1. Když vyberete metriky se posuvníkem nebo textová pole pro zadávání počtu instance, kterou chcete změnit velikost mezi:

    ![Měřítko zásuvné se sloupcem procesor procentuální](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)

    Automatické měřítko nikdy nepracují služby pod nebo nad hranice nastavená, bez ohledu na to vaše načíst.

2. Za druhé vyberte oblast cílové metriky. Například pokud jste se rozhodli **Procento využití procesoru**, můžete nastavit cíl pro průměrnou procesor ve všech instancí ve službě. Měřítko, se stane, když průměr CPU, zapněte, které definujete, podobně velkém měřítku v dojde při každém průměr procesoru vynechává nižší než minimální.

3. Klikněte na příkaz **Uložit** . Automatické měřítko kontroloval každých několik minut, abyste měli jistotu, že jste v oblasti instance a cílové vaší metriky. Další přenosy přijaté služby získáte další instance aniž byste museli dělat cokoliv.

## <a name="scale-based-on-other-metrics"></a>Další metrice měřítko

Změna velikosti na základě metriky než přednastavení, která se zobrazí v rozevíracím seznamu **Zobrazit jako** a můžete dokonce máte složité nastavení měřítko a zobrazit v pravidlech.

### <a name="adding-or-changing-a-rule"></a>Přidáním nebo změnou pravidla

1. Zvolte **plán a výkonu pravidla** v rozevíracím seznamu **měřítko tak, že** : ![pravidla výkonu](./media/insights-how-to-scale/Insights_PerformanceRules.png)

2. Pokud jste dříve měli automatické měřítko, na uvidíte zobrazení přesná pravidla, které jste měli.

3. Zobrazit na základě jiné metrických klikněte na řádku **Přidat pravidlo** . Můžete také kliknete na jednu z existujících řádků změnit z míru, které jste dříve museli míru, kterou chcete zobrazit jako.
![Přidání pravidla](./media/insights-how-to-scale/Insights_AddRule.png)

4. Teď je třeba vybrat které míru budete chtít zobrazit jako. Při výběru metrické, zvažte pár věcí:
    * *Zdroje* metrické pochází z. Obvykle to bude stejná jako zdroj, který je měřítko. Pokud chcete zobrazit jako název hloubkové fronty úložiště, je zdroj, který chcete zobrazit jako fronty.
    * Na *metrických název* samotný.
    * *Časová agregace* metriky. Toto je, jak data překročení zkombinovat doby *trvání*.

5. Po výběru vaší metrické zvolíte mezní metrické a operátor. Například řekněte **vyšší než** **80 %**.

6. Vyberte akci, kterou chcete provést. Existuje pár různé typy akcí:
    * Zvětšit nebo zmenšit tak, že – to bude přidání nebo odebrání **hodnotu** čísla instance, které definujete
    * Zvětšete nebo zmenšete procento – tím se změní počet instancí podle procenta. Třeba byste mohli dát 25 v poli **hodnota** a pokud máte aktuálně 8 instance, by měl být přidán 2.
    * Zvětšení nebo zmenšení: Tato možnost nastaví počet instancí **hodnota** definujete.

7. Nakonec můžete skvělé dolů – jak dlouho má toto pravidlo čekat po předchozí akci měřítko znovu zobrazit.

8. Po konfiguraci pravidla stiskněte tlačítko **OK**.

9. Po konfiguraci všechna pravidla, které chcete nezapomeňte narazila na příkaz **Uložit** .

### <a name="scaling-with-multiple-steps"></a>Změna měřítka s několika kroky

Výše uvedené příklady jsou poměrně základní. Pokud chcete mít víc agresivní o měřítka nahoru (nebo dolů), můžete i více pravidel měřítko metriky stejné. Můžete například definovat dvě pravidla měřítko na procento využití procesoru:

1. Rozšiřování instancí 1-li procento využití procesoru vyšší než 60 %
2. Rozšiřování instancemi 3-li procento využití procesoru nad 85 %

![Více pravidel měřítko](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Při použití tohoto pravidla další zatížení překračuje 85 % před měřítko akcí se zobrazí dvě další instance místo jedné.

## <a name="scale-based-on-a-schedule"></a>Měřítko podle plánu


Ve výchozím nastavení při vytváření pravidla měřítko vždy použije. Když kliknete na záhlaví profilu, které uvidíte:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Však můžete mít vyšší hodnotu proměnné velikosti během dne nebo týden, než na víkendu. Může i vypnout služby úplně vypnout pracovní doby.

1. Uděláte to takto: na profil, který máte, vyberte **opakování** místo **vždy** a zvolte časy, které chcete použít profil.

2. Třeba Pokud chcete, aby profil, který slouží k použití v týdnu, v rozevíracím seznamu **dnů** zrušte zaškrtnutí políčka **sobotu** a **neděli**.

3. Profil, který se vztahuje během na denní, nastavte **Počáteční čas** na čas, který chcete začít od.
    ![Výchozí opakování](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)

4. Klikněte na **OK**.

5. Dále budou potřebujete přidat na profil, který chcete použít v jiných dobu. Klikněte na řádek **Přidat profilu** .
    ![Mimo kancelář](./media/insights-how-to-scale/Insights_ProfileOffWork.png)

6. Název vaší nové, druhou, profilu, například může volání **mimo kancelář**.

7. Pak znovu vyberte **opakování** a zvolte oblast počet instance, kterou chcete během této doby.

8. Jako výchozí profil, vyberte **dnů** chcete, aby profil chcete použít a **čas zahájení** během dne.

>[AZURE.NOTE] Automatické měřítko použije letní úspor pravidla pro ten **časové pásmo** vyberete. Však během letní čas časový posun řádku se zobrazí základní časové pásmo odsazení, ne posun letní úspor UTC.

9. Klikněte na **OK**.

10. Teď budou potřebujete přidat jakéhokoliv pravidla průběhu druhý profil chcete použít. Klikněte na položku **Přidat pravidlo**a pak můžou vytvářet stejné pravidlo, které máte během výchozí profil.
    ![Přidání pravidla vypněte práce](./media/insights-how-to-scale/Insights_RuleOffWork.png)

11. Ujistěte se, jak vytvořit obou pravidlo pro měřítko, a měřítka v or else během profilu počet instancí bude pouze postupně se zvětšují (nebo zmenšit).

12. Nakonec klikněte na **Uložit**.

## <a name="next-steps"></a>Další kroky

* [Sledování služby metriky](insights-how-to-customize-monitoring.md) abyste měli jistotu, že vaše služba je k dispozici a citlivé.
* [Povolit sledování a diagnostice](insights-how-to-use-diagnostics.md) shromažďovat podrobné metriky vysokou četnost na služby.
* [Přijímání oznámení](insights-receive-alert-notifications.md) pokaždé, když dojde k provozní událostem nebo metriky křížové prahovou hodnotu.
* [Sledování aplikací výkonu](../application-insights/app-insights-azure-web-apps.md) chcete se dozvědět přesně jak funguje kódu v cloudu.
* [Zobrazit události a protokolů auditování](insights-debugging-with-events.md) další všechno uskutečněných v služby.
* [Dostupnost monitor a rychlostí reakce webové stránky](../application-insights/app-insights-monitor-web-app-availability.md) s přehledy aplikace, kterou můžete zjistit, zda stránku je vypnutý.
