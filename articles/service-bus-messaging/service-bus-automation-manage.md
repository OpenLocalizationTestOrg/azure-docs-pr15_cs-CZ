<properties
    pageTitle="Správa Bus služby Azure pomocí Azure automatizace | Microsoft Azure"
    description="Naučte se spravovat Bus služby Azure pomocí služby Azure automatizaci."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Správa Bus služby Azure pomocí automatizace Azure

Tento průvodce vám představí služby Azure automatizaci a jak ji můžete použít k zjednodušení správy Bus služby Azure.

## <a name="what-is-azure-automation"></a>Co je Azure automatizaci?

[Automatizace Azure](../automation/automation-intro.md) je služby Azure zjednodušení správy cloudu pomocí automatizaci procesů a konfigurace požadovaný stav. Automatické Azure, ručně, opakovat, dlouhodobě spuštěných a chyby chybám úkoly můžete automatizovat větší spolehlivosti, efektivity a hodnotu time to pro vaši organizaci.

Azure automatizaci poskytuje modul spuštění vysoce spolehlivé, která je velmi dostupná pracovního postupu, který změní podle svých potřeb. V Azure automatizaci procesů můžete být vykazuje postavu ručně, systémů 3rd výrobců nebo plánované intervalech tak, aby úkoly z toho důvodu přesně potřeby.

Snížení režijních provozní a uvolnit tak IT a DevOps personálu zaměření na práci, která přidá firmy hodnotu přesunutím úkoly správy cloudu, aby automaticky spouštět Azure automatizaci.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Jak lze Azure automatické plánování Bus služby Azure?

Správa služby Bus s automatizaci Azure pomocí [Služby Bus REST API](https://msdn.microsoft.com/library/azure/mt639375.aspx). V rámci Azure automatizace spuštěním skriptů Powershellu k provádění mnoha úkolů služby Bus pomocí rozhraní REST API. Můžete taky dvojici tyto rozhraní REST API hovorů v Azure automatizaci pomocí rutin pro další služby Azure, k automatizaci složitých úkolů přes Azure službami a 3 systémy stran.

Tady je několik příkladů použití Powershellu ke správě Bus služby Azure:

* [Vlastní rutiny prostředí PowerShell pro správu fronty Bus služby Azure](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [Jak vytvořit služby Bus fronty, témata a předplatných pomocí skriptu prostředí PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Vytvoření Azure služby Bus obory názvů pomocí prostředí PowerShell](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

Modul Powershellu pro práci s Azure služby bus v automatizaci runbooks si můžete stáhnout z [Galerie Powershellu](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0).


## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy Azure automatizace a jak ji můžete použít ke správě Bus služby Azure, tyto odkazy vedou na další informace o Azure automatizaci.

* V tématu [Začínáme výuková](../automation/automation-first-runbook-graphical.md) Azure automatizaci
* V tématu Správa [Služby Bus pomocí prostředí PowerShell](service-bus-powershell-how-to-provision.md)
