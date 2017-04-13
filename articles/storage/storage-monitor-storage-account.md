<properties
    pageTitle="Sledování úložiště účtu | Microsoft Azure"
    description="Zjistěte, jak sledovat účtu úložiště v Azure pomocí portálu Azure."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Sledování úložiště účtu na portálu Azure

## <a name="overview"></a>Základní informace

Můžete sledovat účtu úložiště z [Portálu Azure](https://portal.azure.com). Při konfiguraci účtu úložiště pro sledování prostřednictvím portálu úložiště Azure pomocí [Analýzy úložiště](http://msdn.microsoft.com/library/azure/hh343270.aspx) sledování metriky pro váš účet a protokolování údajů o žádost.

> [AZURE.NOTE]Další náklady jsou přidružené k posouzení monitorování dat na [Portálu Azure](https://portal.azure.com). Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/azure/hh360997.aspx">technologie pro analýzu úložiště a fakturaci</a>. <br />

> Azure úložiště souborů aktuálně podporuje metriky úložišť analýzy, ale zatím nepodporuje protokolování. Můžete povolit metriky pro ukládání souborů Azure prostřednictvím [Portálu Azure](https://portal.azure.com).

> Účty úložiště k typu replikační části Zone nadbytečné úložiště (ZRS) nemají metriky nebo funkce protokolování povoleno v současné době. 

> Podrobný průvodce k používání technologie pro analýzu úložiště a další nástroje určit, Diagnostika a řešení potíží s problémy související s Azure úložiště najdete v tématu [sledování, Diagnostika a řešení potíží s úložišti tabulek Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="how-to-configure-monitoring-for-a-storage-account"></a>Jak: konfigurace sledování účtu úložiště

1. Na [Portálu Azure](https://portal.azure.com)klikněte na **úložiště**a potom klikněte na název účtu úložiště otevřete řídicího panelu.

2. Klikněte na **Konfigurovat**a posuňte se dolů na **Sledování** nastavení objektů blob, tabulky a služby fronty.

    ![MonitoringOptions](./media/storage-monitor-storage-account/Storage_MonitoringOptions.png)

3. **Monitorování**nastavte úroveň sledování a zásady uchovávání dat pro každou službu:

    -  Pokud chcete nastavit sledování úroveň, vyberte jednu z těchto věcí:

      **Minimální** - shromažďuje metriky například průniku/výstupní, dostupnost, zpoždění a úspěch procent, které jsou agregované objektů blob, tabulky a služby fronty.

      **Podrobné** - současně s minimálními metriky shromažďuje stejnou sadu metriky pro každou akci úložiště v rozhraní API úložiště služby Azure. Podrobný metriky povolit bližší analýzu problémů, ke kterým dochází při operacích aplikace.

      **Vypnutí** - vypne sledování. Existující monitorování dat je zachován až do konce období uchování.

- Pokud chcete nastavit zásady uchovávání dat, **uchovávání informací (ve dnech)**, zadejte počet dnů od data, která chcete zachovat od 1 až 365 dnů. Pokud nechcete nastavit zásady uchovávání informací, zadejte nula. Pokud nejsou žádné zásady uchovávání informací, je můžete odstranit sledování data. Doporučujeme nastavení zásad uchovávání informací založené na tom, jak dlouho chcete zachovat úložiště technologie pro analýzu dat pro váš účet tak, aby starých a nepoužívaných analýzy dat lze odstranit tak, že systém zdarma.

4. Po dokončení konfigurace sledování, klikněte na **Uložit**.

By měly začít zobrazují monitorování dat na stránce **Monitor** po zhruba hodinu a na řídicím panelu.

Dokud nakonfigurujete sledování účtu úložiště, bez sledování údaje a grafy metriky na stránce řídicích panelů a **Sledování** jsou prázdné.

Po nastavení monitorování úrovně a zásady uchovávání informací, které můžete zvolit, které dostupné metrik ke sledování na [Portál Azure](https://portal.azure.com)a které metriky vykreslit na metriky grafy. Výchozí sadu metriky se zobrazí na každé úrovni sledování. **Přidání metriky** slouží k přidání nebo odebrání metriky ze seznamu metriky.

Metriky jsou uložené na účtu úložiště ve čtyřech tabulkách s názvem $MetricsTransactionsBlob, $MetricsTransactionsTable, $MetricsTransactionsQueue a $MetricsCapacityBlob. Další informace najdete v tématu [O metriky úložišť analýzy](http://msdn.microsoft.com/library/azure/hh343258.aspx).


## <a name="how-to-customize-the-dashboard-for-monitoring"></a>Jak se: přizpůsobení řídicí panel pro sledování

Na řídicím panelu můžete až šest metriky vykreslování hodnot v průběhu v diagramu metriky z devět metriky k dispozici. Metriky dostupnost, úspěch procenta i celkový počet požadavků pro každou službu (objektů blob tabulky a fronty), jsou k dispozici. K dispozici na řídicím panelu metriky jsou stejné minimální nebo podrobného sledování.

1. Na [Portálu Azure](https://portal.azure.com)klikněte na **úložiště**a potom klikněte na název účtu úložiště otevřete řídicího panelu.

2. Pokud chcete změnit metriky, které jsou zobrazeny v grafu, proveďte jednu z následujících akcí:

    - Pokud chcete přidat nový metriky do grafu, klikněte na barevné zaškrtnutí políčka vedle metrických záhlaví v tabulce níže grafu.

    - Skrýt míru, která jsou zobrazena v grafu, zrušte zaškrtnutí políčka barevné vedle metrických záhlaví.

        ![Monitoring_nmore](./media/storage-monitor-storage-account/storage_Monitoring_nmore.png)

3. Ve výchozím nastavení zobrazuje grafu trendy zobrazení pouze aktuální hodnotu každý metriky ( **relativní** možnost v horní části grafu). Zobrazit osu Y abyste viděli absolutní hodnoty, zaškrtněte políčko **absolutní**.

4. Pokud chcete změnit časový rozsah vyberte graf zobrazuje metriky 6 hodin, 24 hodin nebo 7 dní v horní části grafu.


## <a name="how-to-customize-the-monitor-page"></a>Jak se: přizpůsobení stránky monitoru

Na stránce **Sledování** můžete zobrazit celou sadu metriky účtu úložiště.

- Pokud váš účet úložiště minimální sledování nakonfigurovali, metriky například průniku/výstupní dostupnost, latence procenta a úspěch agregované z objektů blob, tabulky a služby fronty.

- Pokud váš účet úložiště podrobného sledování nakonfigurovali, metriky jsou k dispozici lepší rozlišení jednotlivé skladování kromě agregace úrovni služeb.

Použijte následující postupy rozhodnout, které metriky úložišť zobrazíte v metriky grafy a tabulky, která se zobrazí na stránce **Sledování** . Tato nastavení nemají vliv kolekce agregace a ukládání monitorování dat do účtu úložiště.

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Postup: Přidání metriky metriky tabulky


1. Na [Portálu Azure](https://portal.azure.com)klikněte na **úložiště**a potom klikněte na název účtu úložiště otevřete řídicího panelu.

2. Klikněte na tlačítko **Sledování**.

    Otevře se stránka **Monitor** . Ve výchozím nastavení metriky tabulka zobrazuje podmnožina metriky, které jsou dostupné pro sledování. Obrázek zobrazuje monitoru výchozí úložiště účtu s komentářem sledování nakonfigurován pro všechny tři služby. **Přidání metriky** slouží k výběru metriky, který chcete sledovat ze všech dostupných metriky.

    ![Monitoring_VerboseDisplay](./media/storage-monitor-storage-account/Storage_Monitoring_VerboseDisplay.png)

    > [AZURE.NOTE] Když vyberete metriky vzít v úvahu náklady. Existuje transakce a výstupním náklady spojené s aktualizací sledování se zobrazí. Další informace najdete v tématu [technologie pro analýzu úložiště a fakturaci](http://msdn.microsoft.com/library/azure/hh360997.aspx).

3. Klikněte na **Přidat metriky**.

    Agregační metriky, které jsou k dispozici v minimální sledování se v horní části seznamu. Pokud je zaškrtnuté políčko, zobrazí se v seznamu metriky metriky.

    ![AddMetricsInitialDisplay](./media/storage-monitor-storage-account/Storage_AddMetrics_InitialDisplay.png)

4. Najeďte myší na pravé straně dialogu zobrazíte posuvník, které můžete přetáhnout další metriky přejděte do zobrazení.

    ![AddMetricsScrollbar](./media/storage-monitor-storage-account/Storage_AddMetrics_Scrollbar.png)


5. Klikněte na šipku dolů tak, že metriky rozbalte seznam operace, které metriky má obor vymezený na zahrnout. Vyberte každou operaci, kterou chcete zobrazit v tabulce metriky [Azure portálu](https://portal.azure.com).

    Na následujícím obrázku zařazují byla rozbalená míru procentuální chyby ověření.

    ![ExpandCollapse](./media/storage-monitor-storage-account/Storage_AddMetrics_ExpandCollapse.png)


6. Když vyberete metriky pro všechny služby, klikněte na OK (zaškrtnutí) aktualizovat konfigurace sledování. Vybrané metriky jsou přidané do tabulky metriky.

7. Odstranit metriky z tabulky, klikněte na míru ho vyberte a pak klikněte na **Odstranit míru**.

    ![DeleteMetric](./media/storage-monitor-storage-account/Storage_DeleteMetric.png)

## <a name="how-to-customize-the-metrics-chart-on-the-monitor-page"></a>Jak se: přizpůsobení grafu metriky na stránce Sledování

1. Na stránce **Sledování** úložiště účtu v tabulce metriky vyberte až 6 metriky vykreslování hodnot v průběhu v diagramu metriky. Chcete-li vybrat metriky, zaškrtněte políčko na levé straně. Metriky odebrat z grafu, zrušte zaškrtnutí políčka.

2. Chcete-li přepnout grafu mezi relativními (zobrazí se pouze konečná hodnota) a absolutní hodnotami (osy Y zobrazí), vyberte **relativní** a **absolutní** v horní části grafu.

3.  Chcete-li změnit oblasti čas zobrazí graf metriky, vyberte **6 hodin**, **24 hodin**nebo **7 dní** v horní části grafu.



## <a name="how-to-configure-logging"></a>Jak: Konfigurace protokolování

Pro každý z úložiště služby dostupné s účtem úložiště (objektů blob tabulky a fronty) můžete uložit protokoly diagnostických nástrojů pro žádosti o čtení, psaní žádosti a/nebo odstranění žádosti o a můžete nastavit zásady uchovávání informací u každé z těchto služeb.

1. Na [Portálu Azure](https://portal.azure.com)klikněte na **úložiště**a potom klikněte na název účtu úložiště otevřete řídicího panelu.

2. Klikněte na **Konfigurovat**a posuňte se dolů na **protokolování**pomocí šipky dolů na klávesnici.

    ![Storagelogging](./media/storage-monitor-storage-account/Storage_LoggingOptions.png)


3. Pro každou službu (objektů blob tabulky a fronty) nakonfigurujte tyto možnosti:

    - Typy žádosti o přihlášení: žádosti o čtení, napsat žádosti a odstranění žádosti o.

    - Počet dnů uchovávání dat protokolu. Zadejte nula je, pokud nechcete nastavit zásady uchovávání informací. Pokud nejsou nastavené zásady uchovávání informací, je odstranit protokoly.

4. Klikněte na **Uložit**.

Protokolování diagnostiky se ukládají v kontejneru objektů blob s názvem $logs ve vašem účtu úložiště. Informace o přístupu k kontejneru $logs najdete v tématu [O úložiště analýzy protokolování](http://msdn.microsoft.com/library/azure/hh343262.aspx).
