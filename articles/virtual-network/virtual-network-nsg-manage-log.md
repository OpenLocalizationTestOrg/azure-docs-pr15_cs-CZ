<properties
   pageTitle="Sledovat činnosti, událostí a čítače NSGs | Microsoft Azure"
   description="Zjistěte, jak povolit čítače, události a provozní protokolování pro NSGs"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/14/2016"
   ms.author="jdial" />

#<a name="log-analytics-for-network-security-groups-nsgs"></a>Protokol analýzy sítě skupin zabezpečení (NSGs)

Různé druhy protokoly v Azure umožňuje spravovat a řešení potíží s NSGs. Některé z těchto protokoly můžete k nim získat přístup prostřednictvím portálu a všechny protokoly extrahovaných z úložiště objektů blob Azure a zobrazit v různých nástrojů, jako jsou [Protokolu analýzy](../log-analytics/log-analytics-azure-networking-analytics.md), Excel a PowerBI. Další informace o různých typů protokolů z následujícího seznamu.

- **Protokolů auditování:** Chcete-li zobrazit všechny operace při odesílání do vašich Azure předplatných a jejich stav můžete [Protokolů auditování Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (dříve označované jako provozní protokoly). Protokolů auditování jsou standardně a můžete zobrazit v portálu Azure náhled.
- **Protokoly událostí:** Tento protokol slouží k zobrazení, jaké NSG pravidla VMs a instance role na základě MAC adresy. Stav tato pravidla se shromažďují každých 60 sekund.
- **Čítač protokolů:** Tento protokol slouží k zobrazení kolikrát byl použitý každé pravidlo NSG odepřít nebo přenosy.

>[AZURE.WARNING] Protokoly jsou dostupné jenom pro zdroje nasazenou v modelu nasazení Správce prostředků. Protokoly nelze použít pro zdroje v modelu klasické nasazení. Pro lepší znalost dvou modelů, referenční článek [Správce prostředků Principy nasazení a klasické nasazení](../resource-manager-deployment-model.md) .

##<a name="enable-logging"></a>Povolení protokolování
Protokolování auditu je automaticky zapnutá u vždy pro každý zdroj správce prostředků. Budete muset povolit událostí a protokolování zahájíte shromažďování dat prostřednictvím těchto protokoly čítač. Chcete-li povolit protokolování, postupujte následujícím způsobem.

1.  Přihlaste se k [portálu Azure](https://portal.azure.com). Pokud ještě nemáte existující skupiny zabezpečení sítě, [Vytvoření NSG](virtual-networks-create-nsg-arm-ps.md) až potom pokračujte.

2.  Na portálu náhledu klikněte na tlačítko **Procházet** >> **skupiny zabezpečení sítě**.

    ![Náhled portálu - skupiny zabezpečení sítě](./media/virtual-network-nsg-manage-log/portal-enable1.png)

3. Vyberte existující skupinu zabezpečení sítě.

    ![Náhled portál – nastavení skupiny zabezpečení sítě](./media/virtual-network-nsg-manage-log/portal-enable2.png)

4. V **Nastavení** zásuvné klikněte na **Diagnostika**a v podokně **diagnostiky** vedle **Stav**, klepněte **na**
5. V **Nastavení** zásuvné, klikněte na **Úložiště účet**a potom vyberte existující úložiště účet nebo vytvořte nový účet.  

>[AZURE.INFORMATION] protokolů auditování nevyžadují účet samostatný úložiště. Použití úložiště pro událostí a protokolování pravidlo je možné za poplatky.

6. V rozevíracím seznamu těsně pod **Úložiště účtu**vyberte, jestli chcete protokolu událostí nebo čítače a potom klikněte na **Uložit**.

    ![Náhled portálu - protokolování diagnostiky](./media/virtual-network-nsg-manage-log/portal-enable3.png)

## <a name="audit-log"></a>Protokol auditování
Protokol (dříve označované jako "provozní protokol") je generováno aplikací Azure ve výchozím nastavení.  Protokoly zachovají 90 dnů v úložišti Azure na protokoly událostí. Další informace o těchto protokoly článek [Zobrazit události a protokolů auditování](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="counter-log"></a>Protokol čítače
Pokud jste ho povolili na základě za NSG uvedených výše pouze generování tento protokolu. Uložení dat v okně úložiště účet, který jste zadali při povolíte protokolování. Každé pravidlo se používá pro zdroje je přihlášena formátu JSON, jak je vidět níže.

    {
        "time": "2015-09-11T23:14:22.6940000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupRuleCounter",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupCounters",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"DenyAllOutBound",
            "direction":"Out",
            "type":"block",
            "matchedConnections":0
            }
    }

## <a name="event-log"></a>Protokol událostí
Pokud jste ho povolili na základě za NSG uvedených výše pouze generování tento protokolu. Uložení dat v okně úložiště účet, který jste zadali při povolíte protokolování. Zaznamenané následující údaje:

    {
        "time": "2015-09-11T23:05:22.6860000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupEvent",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupEvents",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"AllowVnetOutBound",
            "direction":"Out",
            "priority":65000,
            "type":"allow",
            "conditions":{
                "destinationPortRange":"0-65535",
                "sourcePortRange":"0-65535",
                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
            }
        }
    }

## <a name="view-and-analyze-the-audit-log"></a>Prohlížení a analýze protokol auditování
Můžete zobrazit a analýza dat protokolu auditování pomocí kterékoli z těchto způsobů:

- **Azure nástroje:** Získat informace z protokolů auditování pomocí prostředí PowerShell Azure, Azure rozhraní příkazového řádku (rozhraní příkazového řádku), rozhraní REST API Azure nebo portálu Azure náhled.  Podrobné pokyny pro každou metodu jsou uvedené v článku [auditování operace s správce prostředků](../resource-group-audit.md) .
- **Power BI:** Pokud ještě nemáte účet [Power BI](https://powerbi.microsoft.com/pricing) , můžete ho zdarma. Použití [protokolů auditování Azure obsah pack pro Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/) můžete analýza dat pomocí předkonfigurovaná řídicí panely, které můžete použít jako-je nebo upravit.

## <a name="view-and-analyze-the-counter-and-event-log"></a>Prohlížení a analýze čítač a protokolu událostí

Azure [Protokolu analýzy](../log-analytics/log-analytics-azure-networking-analytics.md) získáte čítači v protokolu událostí soubory a z vašeho účtu úložiště objektů Blob obsahuje vizualizace a výkonné možnosti vyhledávání a analyzujte data protokolů.

Také se můžete připojit ke svému účtu úložiště a načítat položky protokolu JSON protokoly událostí a čítač. Po stažení JSON soubory, které převádět CSV a zobrazení v aplikaci Excel, PowerBI nebo jiné vizualizace nástroje data.

>[AZURE.TIP] Pokud znáte Visual Studia a základní koncepty změny hodnoty konstanty a proměnných v jazyce C#, můžete použít [protokol převaděče nástroje](https://github.com/Azure-Samples/networking-dotnet-log-converter) Github k dispozici.

## <a name="next-steps"></a>Další kroky

- Vizualizace čítač a protokoly událostí pomocí [Protokolu analýzy](../log-analytics/log-analytics-azure-networking-analytics.md)
- Příspěvek na blog [vizualizace protokolů auditování Azure pomocí Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Zobrazení a analýza protokolů auditování Azure v Power BI a dělat spoustu dalších](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) příspěvek na blog.
