<properties
    pageTitle="Služby Azure Government | Microsoft Azure"
    description="Informace o správě předplatného v Azure Government"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Správa a připojení k vašemu předplatnému v Azure Government

Azure vláda jedinečné adresy URL a koncové body pro správu prostředí. Je důležité používat správné připojení ke správě prostředí prostřednictvím portálu nebo Powershellu. Jakmile jste připojeni k prostředí Azure Government normálně pro správu služby funguje v případě byly nasazeny komponentu.

## <a name="connecting-via-the-portal"></a>Připojení pomocí portálu
Na portálu je primární tak, aby většina lidí připojení k Azure Government.  Pokud chcete připojit, přejděte na portál na [https://portal.azure.us](https://portal.azure.us).  Starší verzi Azure portál můžete k nim získat přístup prostřednictvím [https://manage.windowsazure.us](https://manage.windowsazure.us).

Předplatné můžete vytvořit pro váš účet připojení k [https://account.windowsazure.us](https://account.windowsazure.us).

## <a name="connecting-via-powershell"></a>Připojení pomocí prostředí PowerShell

Jestli používáte Azure Powershellu ke správě velké předplatné prostřednictvím skriptu nebo access funkce, které nejsou aktuálně dostupné na portálu Azure, které potřebujete k připojení k Azure Government místo Azure veřejné.  Pokud používáte Powershellu v Azure veřejné je stejný.  Rozdíly v Azure Government jsou:

+ Připojení účtu
+ Názvy oblastí

>[AZURE.NOTE] Pokud zatím nepoužijete Powershellu, přečtěte si [Úvod do prostředí PowerShell Azure](../powershell-install-configure.md).

Při spuštění prostředí PowerShell musíte zjistit Azure Powershellu pro připojení k Azure Government zadáním parametr prostředí.  Parametr zaručuje, že prostředí PowerShell se připojuje k správné koncové body.  Když spojíte přihlásit ke svému účtu, je určený kolekce koncové body.  Různé rozhraní API vyžadují různé verze přepínačem prostředí:

Typ připojení | Příkaz
---|----
Příkazy pro [Správu služby](https://msdn.microsoft.com/library/dn708504.aspx) | `Add-AzureAccount -Environment AzureUSGovernment`
Příkazy pro [Správu zdrojů](https://msdn.microsoft.com/library/mt125356.aspx) | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
Příkazy [Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) | `Connect-MsolService -AzureEnvironment UsGovernment`
[Příkaz v2 Azure Active Directory](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

Použijte přepínač prostředí při připojení k účtu úložiště pomocí nový AzureStorageContext, kde můžete určit AzureUSGovernment.

### <a name="determining-region"></a>Určení oblast

Jakmile jste připojeni, je jeden další rozdíl – oblasti slouží k obrázku do služby.  Každý Azure cloudu má různých oblastí.  Zobrazíte je uvedená na stránce dostupnost služby.  Běžným způsobem použijete oblast v parametru umístění příkazu.

Je jeden skutečné.  Oblasti Azure Government je nutné naformátovat jinak než jejich běžných názvů:

Běžné název | Příkaz
---|----
Virginie Gov USA | USGov Virginie
Iowa Gov USA | USGov Iowa

>[AZURE.NOTE] Při použití parametr umístění je bez mezer mezi USA a Gov.

Pokud chcete ověřit oblasti k dispozici v Azure Government, můžete následující příkazy a vytisknout aktuální seznam:

    Get-AzureLocation

Pokud jste přes Azure zajímá vás, k dispozici prostředí, můžete spustit:

    Get-AzureEnvironment

## <a name="next-steps"></a>Další kroky

Pokud hledáte další informace, které najdete v tématech:

+ [Prostředí PowerShell dokumenty na GitHub](https://github.com/Azure/azure-powershell)
+ [Podrobné pokyny k připojení k řízení zdrojů](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [Azure prostředí PowerShell dokumenty na webu MSDN](https://msdn.microsoft.com/library/mt619274.aspx)

Doplňující informace a aktualizace se přihlásit k odběru [Blog aplikace Microsoft Azure Government] (https://blogs.msdn.microsoft.com/azuregov/)
