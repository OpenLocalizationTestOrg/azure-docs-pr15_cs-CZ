
<properties
    pageTitle="Ověření VNET Azure pomocí služby Azure RemoteApp | Microsoft Azure"
    description="Naučte se ujistěte se, že je připravená k použití s Azure RemoteApp Azure VNET"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Ověření VNET Azure pomocí služby Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Před použitím Azure VNET s Azure RemoteApp můžete ověřit VNET. Zabráníte problémy s připojením.

Ověřte Azure VNET, postupujte takto:

1. Vytvoření Azure virtuálního počítače uvnitř podsítě VNET Azure, který chcete použít s Azure RemoteApp.

2. Připojte se k této OM pomocí možnost **Připojit** v portálu pro správu.
3. Spojení virtuálního počítače a samou doménu, kterou chcete použít s Azure RemoteApp. Pokud vytváříte kolekce hybridní, ke kterému je připojen k síti místní připojení virtuálního počítače do vaší místní domény.

Pokud je to úspěšné, je připravená k použití s RemoteApp Azure VNET.

Další informace o pracovním postupu kolekce hybridní začátku do konce naleznete v následujících článcích:

- [Plánování sítě virtuální Azure RemoteApp](remoteapp-planvnet.md)
- [Vytvoření kolekce hybridní](remoteapp-create-hybrid-deployment.md)
- [Nasazení Azure RemoteApp kolekce k síti Azure virtuální (s podporou ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
