<properties
    pageTitle="Povolit sledování a Diagnostika v Microsoft Azure | Microsoft Azure "
    description="Zjistěte, jak nastavit diagnostiky zdrojů v Azure."
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

# <a name="enable-monitoring-and-diagnostics"></a>Povolit sledování a diagnostických nástrojů

Na [Portálu Azure](https://portal.azure.com)můžete nakonfigurovat bohaté a časté, sledování a Diagnostika dat o zdrojích. Můžete také [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) nebo [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx) pro nastavení diagnostických nástrojů programově.

Diagnostika, sledování a míru dat v Azure se uloží do účtu úložiště podle svého výběru. Umožňuje použít jakékoli nástrojů, který chcete číst data, z Průzkumníka úložiště k Power BI na nástrojů jiných výrobců.

## <a name="when-you-create-a-resource"></a>Když vytvoříte zdroj

Většinou služeb umožňuje diagnostiky po vytvoření je [Portál Azure](https://portal.azure.com).

1. Přejděte na **Nový** a zvolte zdroje, které vás zajímají.

2. Vyberte **volitelné konfiguraci**.
    ![Diagnostika zásuvné](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. Vyberte **diagnostických nástrojů**a klepněte **na**. Je třeba zvolte účet úložiště má diagnostiky uložit do. Budete vybírá normální dat sazby pro ukládání a transakce při odeslání diagnostiky účet úložiště.

4. Klikněte na **OK** a vytvořte zdroje.

## <a name="change-settings-for-an-existing-resource"></a>Změna nastavení pro existující zdroj

Pokud jste již vytvořili zdroje a chcete změnit nastavení diagnostických nástrojů (Pokud chcete změnit úroveň shromažďování dat, například), můžete udělat, které přímo na portálu Azure.

1. Přejděte na daný zdroj a klikněte na tlačítko **Nastavení** .

2. Vyberte **diagnostických nástrojů**.

3. **Diagnostika** zásuvné obsahuje všechny možné Diagnostika a sledování kolekce dat pro daný zdroj. Pro některé zdroje můžete taky zvolit zásad **uchovávání informací** u dat si vyčistit e-maily ze svého účtu úložiště.
    ![Diagnostika úložiště](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. Jakmile jste zvolili nastavení, klikněte na příkaz **Uložit** . Může to trvat trochu během k monitorování dat objevit, pokud povolíte ho poprvé.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Kategorie shromažďování dat pro virtuálních počítačích
Pro virtuálních počítačích všech metriky a protokoly bude zaznamenán v jednu chvíli intervalech, takže máte můžete vždy nejaktuálnější informace o vašem počítači.

- **Základní metriky** : Stav metriky o virtuálního počítače například procesoru a paměti
- **Síť a web metriky** : metriky o síťových připojení a webové služby
- **Metriky .NET** : metriky .NET a ASP.NET aplikace spuštěné v počítači virtuální
- **SQL metriky** : Pokud je spuštěná služba Microsoft SQL, jeho měřítka
- **Aplikace protokoly událostí Windows** : událostí systému Windows, které jsou odeslané na kanál, aplikace
- **Protokoly událostí systému Windows** : událostí systému Windows, které jsou odeslány systémový kanál. Obsahuje taky všechny události z [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
- **Zabezpečení protokoly událostí Windows** : událostí systému Windows, které jsou odeslány kanál zabezpečení
- **Protokolování diagnostiky infrastruktury** : protokolování informace o infrastruktury kolekce diagnostických nástrojů
- **Protokoly služby IIS** : protokoly o serveru IIS

Všimněte si, že v současné době nejsou podporované některé distribuce Linux a agenta hosta musí být nainstalovaná v počítači virtuální.

## <a name="next-steps"></a>Další kroky

* [Přijímání oznámení](insights-receive-alert-notifications.md) pokaždé, když dojde k provozní událostem nebo metriky křížové prahové hodnoty.
* [Sledování služby metriky](insights-how-to-customize-monitoring.md) abyste měli jistotu, že vaše služba je k dispozici a citlivé.
* Aby použít měřítko služby založené na vyžádání [Měřítko počet instancí automaticky](insights-how-to-scale.md) .
* [Sledování aplikací výkonu](../application-insights/app-insights-azure-web-apps.md) chcete se dozvědět, jak je přesně kódu provádění v cloudu.
* [Zobrazit události a protokolů auditování](insights-debugging-with-events.md) další všechno uskutečněných v služby.
* [Sledování stavu služeb](insights-service-health.md) zjistit, kdy došlo Azure výkonu snížení úrovně nebo službu vyrušování.
