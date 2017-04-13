<properties 
    pageTitle="Nasazení QuickBooks v Azure RemoteApp | Microsoft Azure" 
    description="Naučte se sdílet QuickBooks s Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Jak nasadíte QuickBooks v Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Použijte tyto informace ke sdílení QuickBooks jako aplikace v Azure RemoteApp.


QuickBooks 2015 organizace můžete sdílet s Azure RemoteApp v kolekci hybridní nebo cloudu. Soubor společnost musí být umístěny na OM, na kterém běží QuickBooks databázovém serveru, který je nezávislý na Azure RemoteApp servery. Nikdy společnosti soubor uložit na obrázek Azure RemoteApp - ztrátou dat je očekáván v takovém případě. Pouze QuickBooks organizace podporuje hostingu QuickBooks souboru na externí sdílení s databázovým serverem QuickBooks přístupných standardní Windows sítě.   

> [AZURE.IMPORTANT] Databázový server QuickBooks, který je hostitelem firemního souboru musí být umístěny na samostatném OM v rámci stejné VNET jako kolekci Azure RemoteApp.  

## <a name="steps-to-deploy-quickbooks"></a>Postup pro nasazení QuickBooks

1. Vytvoření OM Azure a nainstalujte QuickBooks QuickBooks databázový server a umístění souboru společnosti na OM Azure.  Je třeba správně nakonfigurovat pravidla brány firewall.
2. Nainstalujte QuickBooks na [vlastní obrázek](remoteapp-imageoptions.md) a vytvořte [kolekci Azure RemoteApp](remoteapp-collections.md), cloudu nebo hybridní v rámci přesně stejné VNET kde jsou uložená OM hostingu QuickBooks databázový server s položkami společnosti. 
3.  [Publikování](remoteapp-publish.md) QuickBooks aplikací uživatelům
4.  Azure RemoteApp hostované QuickBooks klienta, přejděte pomocí standardní Windows sítí pro OM hostingu QuickBooks databázový server a otevřete soubor společnosti. 

## <a name="documentation-references"></a>Přečtěte následující dokumentaci pro odkazy

- QuickBooks [podporované konfigurace](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooks [Možnosti nasazení](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Můžete taky podívat Ignite prezentaci, [týkajících se základů Microsoft Azure RemoteApp správy a správa](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) – rychlé převíjení vpřed na 1:02:45 pro přístup k části QuickBooks.

## <a name="deployment-architecture"></a>Architektura nasazení

![QuickBooks + Azure RemoteApp nasazení](./media/remoteapp-quickbooks/ra-quickbooks.png)