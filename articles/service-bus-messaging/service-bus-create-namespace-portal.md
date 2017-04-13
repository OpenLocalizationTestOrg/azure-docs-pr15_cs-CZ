<properties
    pageTitle="Vytvoření obor Bus služby Azure portálu | Microsoft Azure"
    description="Abyste mohli začít pracovat s Bus službu, budete potřebovat obor. Tady je postup, jak vytvořit pomocí portálu Azure."
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="jotaub"/>

# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Vytvoření obor Bus služby Azure portálu

Obor názvů je kontejner běžné všech zpráv součástí. Více fronty témata mohou být uloženy a v jedné názvů a obory názvů často sloužit jako kontejnery aplikace. Aktuálně dvěma různými způsoby vytvoření obor Bus služby.

1.  Azure portál (Tento článek)

2.  [Správce prostředků šablony][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Vytvořit obor názvů na portálu Azure

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Blahopřejeme! Teď jste vytvořili obor zasílání zpráv Bus služby.

## <a name="next-steps"></a>Další kroky

Podívejte se na našeho [GitHub vzorky] [ github-samples] které zobrazují některé rozšířené funkce zasílání zpráv Bus služby Azure.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples