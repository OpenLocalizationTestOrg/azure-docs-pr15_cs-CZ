<properties
   pageTitle="Sledovat činnosti, událostí a čítače pro vyrovnávání zatížení | Microsoft Azure"
   description="Zjistěte, jak povolit oznámení událostí a probe zdravotní stav protokolování pro vyrovnávání zatížení Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="log-analytics-for-azure-load-balancer-preview"></a>Protokol analýz pro vyrovnávání zatížení Azure (verze Preview)

Různé druhy protokoly v Azure umožňuje spravovat a řešení potíží s Vyrovnávání zatížení. Některé z těchto protokoly můžete k nim získat přístup prostřednictvím portálu. Všechny protokoly můžete extrahovaných z úložiště objektů blob Azure a zobrazit v různých nástrojů, jako je Excel a PowerBI. Další informace o různých typů protokolů z následujícího seznamu.

- **Protokolů auditování:** Chcete-li zobrazit všechny operace při odesílání do vašich Azure předplatných a jejich stav můžete [Protokolů auditování Azure](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) (dříve označované jako provozní protokoly). Protokolů auditování jsou standardně a můžete zobrazit v portálu Azure.
- **Upozornit protokoly událostí:** Použijte tento protokol zobrazíte aktivována jaké upozornění pro vyrovnávání zatížení. Stav pro vyrovnávání zatížení se shromažďují každých pět minut. Tento protokol je zapsat pouze pokud je umocněné na událost upozornění Vyrovnávání zatížení.
- **Stavu zkušební protokolů:** Tento protokol umožňuje vyhledat zkušební Kontrola stavu, kolik instancí jsou online v Vyrovnávání zatížení back-end a procento virtuálních počítačích doručování v síti z vyrovnávání zatížení. Tento protokol zapisuje na zkušební stav událostí změnit.

>[AZURE.IMPORTANT] Protokolování analýzy aktuálně funguje jenom pro internetové Vyrovnávání zatížení. Protokoly jsou dostupné jenom pro zdroje nasazenou v modelu nasazení Správce prostředků. Protokoly nelze použít pro zdroje v modelu klasické nasazení. Další informace o modelů nasazení najdete v článku [Správce prostředků Principy nasazení a klasické nasazení](../../articles/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Povolení protokolování

Protokolování auditu je automaticky povoleno pro každý zdroj správce prostředků. Budete muset povolit událostí a stavu zkušební protokolování zahájíte shromažďování dat prostřednictvím těchto protokoly. Pomocí následujících kroků povolit protokolování.

Přihlaste se k [portálu Azure](http://portal.azure.com). Pokud ještě nemáte při vyrovnávání zatížení, [vytvořte Vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) až potom pokračujte.

1. Na portálu klikněte na **Procházet**.
2. Vyberte **Vyrovnávání zatížení**.

    ![portál - Vyrovnávání zatížení](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Vyberte stávající Vyrovnávání zatížení >> **Všechna nastavení**.
4. Na pravé straně obrazovky s dialogem pod názvem Vyrovnávání zatížení přejděte na možnost **Sledování**, klikněte na **Diagnostika**.

    ![portál – nastavení Vyrovnávání zatížení](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. **V podokně **Diagnostika** v části **Stav**vyberte.**
6. Klepněte na **účet úložiště**.
7. V části **protokolování**zaškrtněte existujícího účtu úložiště nebo vytvořte nový účet. Chcete-li zjistit, kolik dnů události dat bude nacházet v protokolu událostí pomocí posuvníku. 8. Klikněte na **Uložit**.

    ![Portál - protokolování diagnostiky](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

>[AZURE.INFORMATION] protokolů auditování nevyžadují účet samostatný úložiště. Použití úložiště pro události a stavu zkušební protokolování vzniknou poplatky.

## <a name="audit-log"></a>Protokol auditování

Ve výchozím nastavení generování protokolu auditování. Protokoly zachovají 90 dnů v úložišti Azure na protokoly událostí. Další informace o těchto protokoly článek [Zobrazit události a protokolů auditování](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="alert-event-log"></a>Upozornění v protokolu událostí

Tento protokol vytváří pouze, pokud jste ho povolili na na základě Vyrovnávání zatížení. Události jsou přihlášení k lyncu ve formátu JSON a uložené na účtu úložiště, který jste zadali při povolíte protokolování. Následující obrázek je příkladem události.

    {
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }

Výstup JSON zobrazuje *eventname* vlastnost, která bude popisu důvodu Vyrovnávání zatížení vytvořili upozornění. V tomto případě upozornění generovaných byl kvůli vyčerpání port TCP způsobená zdroj překladu síťových IP adres limity (SNAT).

## <a name="health-probe-log"></a>Stav zkušební protokolu

Tento protokol vytváří pouze, pokud jste ho povolili na jednoho zatížení vyrovnávání základna popsané výše. Uložení dat v okně úložiště účet, který jste zadali při povolíte protokolování. Vytvoření kontejneru s názvem "přehledy loadbalancerprobehealthstatus protokoly" a zaznamenané následujícími údaji:

    {
        "records":
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 1,
                "healthPercentage": 50.000000
            }
        },
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 0,
                "healthPercentage": 100.000000
            }
        }
    }

Výstup JSON zobrazí v poli vlastnosti základní informace o stavu zkušební. Vlastnost *dipDownCount* zobrazuje celkový počet výskytů na back-end, které nechodí v síti kvůli selhalo zkušební odpovědi.

## <a name="view-and-analyze-the-audit-log"></a>Prohlížení a analýze protokol auditování

Můžete zobrazit a analýza dat protokolu auditování pomocí kterékoli z těchto způsobů:

- **Azure nástroje:** Získat informace z protokolů auditování pomocí prostředí PowerShell Azure, Azure rozhraní příkazového řádku (rozhraní příkazového řádku), rozhraní REST API Azure nebo portálu Azure náhled. Podrobné pokyny pro každou metodu jsou uvedené v článku [auditování operace s správce prostředků](../../articles/resource-group-audit.md) .
- **Power BI:** Pokud už nemáte účet [Power BI](https://powerbi.microsoft.com/pricing) , můžete ho zdarma. Pomocí [protokolů auditování Azure obsah pack pro Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), analýza dat pomocí předkonfigurovaná řídicí panely, nebo si můžete přizpůsobit zobrazení tak, aby vyhovovaly vašim požadavkům.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Prohlížení a analýze zkušební zdraví a protokolu událostí

Budete muset připojit ke svému účtu úložiště a načíst položky protokolu JSON pro zkušební protokoly událostí a stavu. Po stažení JSON soubory, které převádět CSV a zobrazení v aplikaci Excel, PowerBI nebo jiné vizualizace nástroje data.

>[AZURE.TIP] Pokud znáte Visual Studia a základní koncepty změny hodnoty konstanty a proměnných v jazyce C#, můžete použít [protokol převaděče nástroje](https://github.com/Azure-Samples/networking-dotnet-log-converter) Github k dispozici.

## <a name="additional-resources"></a>Další zdroje informací

- Příspěvek na blog [vizualizace protokolů auditování Azure pomocí Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Zobrazení a analýza protokolů auditování Azure v Power BI a dělat spoustu dalších](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) příspěvek na blog.

## <a name="next-steps"></a>Další kroky

[Princip sond Vyrovnávání zatížení](load-balancer-custom-probe-overview.md)
