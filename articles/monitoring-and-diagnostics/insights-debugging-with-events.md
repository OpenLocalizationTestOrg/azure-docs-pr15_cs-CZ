<properties
    pageTitle="Zobrazit události a protokolů auditování"
    description="Zjistěte, jak chcete zobrazit všechny události, které se provedou předplatné Azure."
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
    ms.date="04/28/2015"
    ms.author="robb"/>

# <a name="view-events-and-audit-logs"></a>Zobrazit události a protokolů auditování

Všechny operace provádějí Azure prostředky, které budou plně auditovány Azure zdroje správcem, vytváření a odstraňování poskytnutí nebo odvolání přístupu. Můžete procházet tyto protokoly Azure portálu a taky můžete [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) nebo [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx) pro přístup k úplnou sadu události programově.

## <a name="browse-the-events-impacting-your-azure-subscription"></a>Procházet události ovlivňující ochranu předplatného Azure

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na **Procházet** a vyberte **protokolů auditování**.  
    ![Procházet centrální](./media/insights-debugging-with-events/Insights_Browse.png)
3. Otevře se nahoru zásuvné zobrazující všechny události, které mají vliv na některou z předplatných posledních 7 dnů. Nahoře je graf znázorňující data podle úrovně a pod úplný seznam protokolů:  ![všechny události](./media/insights-debugging-with-events/Insights_AllEvents.png)

>[AZURE.NOTE] 500 posledních událostí pro dané předplatné mohou jenom zobrazit v portálu Azure.

4. Po klepnutí na položku zobrazíte události, které udělali nahoru. Třeba při nasazení něco skupina zdroje, mnoha různých zdrojů může být vytvořili nebo změnili. Pro každou položku uvidíte:
    * **Úroveň** událost – například to bylo možné by jenom ke sledování (**informační**), nebo když něco nepovede, že byste měli vědět o (**Chyba**).
    * **Stav** - konečný stav obecně bude **byl úspěšný** nebo **Neúspěšný**, ale je také možné **přijaté** pro operace dlouho probíhajících.
    * *Při* události došlo k.
    * *Kdo* provést operaci, pokud všichni. Ne všechny operace provádí uživatele, některé provádí back-end služby, nebude mít **volající**.
    * **ID korelace** události – to je jedinečný identifikátor pro tuto sadu operací.

5. Odtud můžete přejít na zásuvné podrobnosti zobrazíte konkrétní události.

    ![Skupiny zdrojů](./media/insights-debugging-with-events/Insights_EventDetails.png)

    Pro události **se nezdařilo** tuto stránku obvykle obsahuje **dílčí stav** a **Vlastnosti** oddíl obsahující užitečné informace pro účely ladění.

## <a name="filter-to-specific-logs"></a>Filtrování konkrétní protokoly

Abyste mohli vidět události, které se vztahují konkrétní entity nebo určitý typ, můžete filtrovat zásuvné protokolů auditování po kliknutí na příkaz **Filtr** . Pomocí filtru zásuvné rovněž změnit **časového rozsahu** z zásuvné protokolů auditování.

Po kliknutí na příkaz, otevře se nové zásuvné:

![Filtr](./media/insights-debugging-with-events/Insights_EventFilter.png)

Existují čtyři typy filtrů:

1. Podle předplatného
2. **Pole Skupina zdroje**
3. **Pole Typ zdroje**
4. Podle určitého **zdroje** - pro tuto musíte dřívější v celé *Číslo ID zdroje* zdroje, které vás zajímají

Kdo provedl událost nebo podle úrovně události kromě toho můžete filtrovat i události.

Po dokončení výběru, co chcete zobrazit, klikněte na tlačítko **Aktualizovat** v dolní části zásuvné.

## <a name="monitor-events-impacting-specific-resources"></a>Sledování událostí ovlivňující ochranu určitého zdroje

1. Klikněte na **Procházet** a vyhledejte zdroje, které vás zajímají. Můžete taky zobrazit všechny protokoly pro celé **pole Skupina zdroje**.
2. Na zásuvné zdroje přejděte dolů na Najít dlaždici **události** .  
    ![Dlaždice události](./media/insights-debugging-with-events/Insights_EventsTile.png)
3. Klikněte na tuto dlaždici zobrazíte události filtrované jenom prostředek, který jste vybrali. Příkaz **Filtr** umožňuje změnit časový rozsah nebo konkrétnější pomocí filtrů.

## <a name="next-steps"></a>Další kroky

* [Přijímání oznámení](insights-receive-alert-notifications.md) vždy, když nastane události.
* [Sledování služby metriky](insights-how-to-customize-monitoring.md) abyste měli jistotu, že vaše služba je k dispozici a citlivé.
* [Sledování stavu služeb](insights-service-health.md) zjistit, kdy došlo Azure výkonu snížení úrovně nebo službu vyrušování.  
