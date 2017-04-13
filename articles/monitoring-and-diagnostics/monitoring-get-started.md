<properties
    pageTitle="Začínáme s Azure Monitor | Microsoft Azure"
    description="Začínáte používat Azure Monitor můžete získat přehled o operaci prostředků a proveďte akce založen na data."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-azure-monitor"></a>Začínáme s Azure monitoru

Azure Monitor je služba platformy, která poskytuje jeden zdroj pro sledování Azure zdroje. S Azure monitoru můžete vizualizace, dotazu, směrování, archivace a akce na metriky a protokoly pocházejících z zdrojů v Azure. Můžete pracovat s těmito daty pomocí portálu zásuvné Monitor [Rutiny prostředí PowerShell monitoru](./insights-powershell-samples.md), [Různé platformy rozhraní příkazového řádku](insights-cli-samples.md)nebo [Azure Monitor REST API](https://msdn.microsoft.com/library/dn931943.aspx). V tomto článku projdeme několik klíčových komponent Azure Monitor, na portálu ukázku.

1. Na portálu přejděte na **Další služby** a najděte požadovanou možnost **Sledování** . Klikněte na ikonu hvězdičky tuto možnost přidáte do seznamu oblíbených tak, aby byla vždy snadno dostupné v levém navigačním panelu.

    ![Sledovat v seznamu služby](./media/monitoring-get-started/monitor-more-services.png)

2. Klikněte na možnost **Sledování** otevřete zásuvné **Monitor** . Tento zásuvné spojuje všechny svoje sledování nastavení a data do jedné konsolidované zobrazení. Prvním otevření do oddílu **protokol činnosti** .

    ![Sledování zásuvné navigace](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] Možnosti **oznámení služby** a **oznámení skupiny** zobrazené nad se zobrazí, pouze uživatelům, kteří se připojili k soukromé náhled těchto funkcí.

    Azure Monitor má tři základní kategorie monitorování dat: **protokol činnosti**, **metriky**a **protokoly pro diagnostiku**.

3. Klikněte na možnost **protokolování aktivity** zajistit, že se zobrazí v části log aktivity.

    ![Aktivity protokolu zásuvné](./media/monitoring-get-started/monitor-act-log-blade.png)

    [**Protokol činnosti**](./monitoring-overview-activity-logs.md) popisuje všechny operace provádějí zdrojů v předplatného. Pomocí protokolu činnosti, můžete určit "co, kdo a kdy" pro všechny vytvořit, operace aktualizace nebo odstranění zdrojů ve vašem předplatném. Například protokolu činnosti se dovíte, kdy byl ukončen do webových aplikací a kdo zastavit. Aktivity události protokolu jsou uložené v platformu k vytvoření dotazu pro 90 dní.
   
    Můžete vytvořit a uložit dotazy pro běžné filtry a potom připnout nejdůležitější dotazů na portálu řídicí panel, takže uvidíte vždy pokud došlo k události, které vyhovují vašim kritériím.

4. Filtrování zobrazení pro určité skupiny prostředků přes minulý týden a potom klikněte na tlačítko **Uložit** .

    ![Uložení aktivity protokolu dotazu](./media/monitoring-get-started/monitor-act-log-save.png)

5. Teď klikněte na tlačítko **připínáčku** .

    ![Klikněte na připnout protokolu aktivity](./media/monitoring-get-started/monitor-act-log-pin.png)

    Většina zobrazení v tomto návodu mohou Připne řídicího panelu. To vám pomůže vytvořit jediný zdrojem informací pro provozní data na svých služeb. 

6. Vraťte se do řídicího panelu. Teď můžete vidět, že dotazu (a počet výsledků) se zobrazí v řídicím panelu. To je užitečné, pokud chcete rychle zobrazit všechny vysoký profil akce, které se uskutečnily naposledy předplatné, např. novou roli přidělené nebo došlo k odstranění virtuálního počítače.

    ![Protokol činnosti připnuté řídicího panelu](./media/monitoring-get-started/monitor-act-log-db.png)

7. Vraťte se do dlaždici **Monitor** a klikněte na oddíl **metriky** . Nejdřív budete muset vyberte zdroj filtrování a výběrem pomocí rozevíracího seznamu možnosti v horní části zásuvné.

    ![Filtrování zdrojů pro metriky](./media/monitoring-get-started/monitor-met-filter.png)

    Všechny Azure zdroje posílat [**metriky**](./monitoring-overview-metrics.md). Toto zobrazení spojují všechny metriky do jednoho podokna sklenice můžete snadno pochopit, jak funguje zdroje.

8. Po výběru zdroje se zobrazí všechny dostupné metriky na levé straně zásuvné. Můžete graf několika metrik najednou tak, že vyberete metriky a změnit typ a čas oblast grafu. Můžete taky zobrazit všechna upozornění metrických nastavení pro tento zdroj.

    ![Metriky zásuvné](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Některé metriky jsou k dispozici pouze povolením [Přehledy aplikace](../application-insights/app-insights-overview.md) a/nebo Windows nebo Linux Azure Diagnostika na zdroj.

9. Až budete spokojení s grafu, můžete připnout do řídicího panelu na tlačítko **Připnout** .

10. Vraťte se do zásuvné **Monitor** a klikněte na **protokoly pro diagnostiku**.

    ![Protokoly pro diagnostiku zásuvné](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Protokoly pro diagnostiku**](monitoring-overview-of-diagnostic-logs.md) jsou protokoly vyzařovaného *tak, že* zdroje, které jsou zdrojem dat o operace danému zdroji. Například čítače pravidlo skupiny zabezpečení sítě a protokoly pracovního postupu aplikace použití logických operátorů jsou oba typy protokoly pro diagnostiku. Tyto protokoly můžete uložené na účtu úložiště, streamují k rozbočovači událost nebo posílat [Protokolu analýzy](../log-analytics/log-analytics-overview.md). Technologie pro analýzu protokolu je produkt provozní intelligence společnosti Microsoft pro rozšířeného vyhledávání a výstrahy.
   
    Na portálu můžete zobrazit a filtrovat seznam všech zdrojů v předplatném zjistit, zda mají povoleno protokoly pro diagnostiku.

11. Klikněte na zdroj v zásuvné diagnostické protokoly. Pokud jsou protokoly pro diagnostiku jsou uloženy v účtu úložiště, zobrazí se seznam hodinové protokolů, které si můžete stáhnout přímo.

    ![Protokoly pro diagnostiku pro jeden zdroj](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    Klikněte na tlačítko **Diagnostické nastavení**, která umožňuje nastavit nebo změnit nastavení pro archivaci k účtu úložiště, datových proudů rozbočovače událost nebo odeslání do pracovního prostoru protokolu analýzy.

    ![Povolení protokoly pro diagnostiku](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Pokud jste nastavili diagnostické protokoly, aby protokolu analýzy, je můžete hledat v části **Log vyhledávání** na portálu.

12. Přejděte do části **upozornění** zásuvné Monitor.

    ![upozornění na zásuvné veřejnosti](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Tady můžete spravovat všechna [**upozornění**](./monitoring-overview-alerts.md) na Azure prostředky. Platí to i pro oznámení na metriky, aktivity události protokolu (v soukromé verze preview), testů web aplikaci přehledy (umístění) a diagnostice přehledy aplikace aktivní. Upozornění můžete aktivovat e-mailu k odeslání nebo HTTP příspěvek na adresu URL webhook.
   
13. Klikněte na **Přidat metrických upozornění** k vytvoření upozornění.

    ![Přidání metrické oznámení](./media/monitoring-get-started/monitor-alerts-add.png)

    Můžete pak připnete upozornění na řídicí panel stavu snadno zorientujete v tom kdykoli.

14. V části Sledování taky obsahuje odkazy na [Přehledy aplikace](../application-insights/app-insights-overview.md) aplikací a řešení pro správu [Protokolu analýzy](../log-analytics/log-analytics-overview.md) . Tyto další produkty společnosti Microsoft mít hloubkové integrace se službou Azure Monitor.

15. Pokud nepoužíváte přehledy aplikace nebo protokolu analýzy, je pravděpodobné, že Azure Monitor má partnerství s aktuální sledování, protokolování a výstražných produkty. V tématu našeho [partnery stránky](./monitoring-partners.md) úplný seznam a pokyny, jak integrovat.

Pomocí tohoto návodu a Připnutí všechny relevantní dlaždice řídicího panelu, můžete vytvořit komplexní zobrazení aplikace a infrastruktury tohoto typu:

![Řídicí panel Azure monitoru](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Další kroky
- Přečtěte si [Přehled Azure monitoru](./monitoring-overview.md)
