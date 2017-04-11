<properties
   pageTitle="Vytvoření kontejneru služby cloudu pomocí prostředí PowerShell | Microsoft Azure"
   description="Tento článek vysvětluje, jak vytvořit kontejner služby cloudu pomocí prostředí PowerShell. Kontejner hostuje web a pracovní role."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Umožňuje vytvořit kontejner prázdné cloudové služby Azure PowerShell command
Tento článek vysvětluje, jak můžete rychle vytvořit kontejner Cloudovým službám pomocí rutin prostředí PowerShell Azure. Postupujte podle následujících kroků:

1. Nainstalujte rutiny prostředí PowerShell Microsoft Azure ze stránky [souborů ke stažení prostředí PowerShell Azure](http://aka.ms/webpi-azps) .
2. Otevřete okno příkazového prostředí PowerShell.
3. Přihlaste se pomocí [AzureAccount přidat](https://msdn.microsoft.com/library/dn495128.aspx) .

    > [AZURE.NOTE] Další informace o instalaci rutiny prostředí PowerShell Azure a připojení k předplatnému Azure nápovědě k [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

4. Umožňuje vytvořit kontejner prázdné Azure cloudové služby rutinu **New-AzureService** .

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. Postupujte následovně vyvolat rutinu:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Další informace o vytváření Azure cloudové služby spusťte:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Další kroky

 * Správa nasazení cloudové služby, najdete pod odkazy příkazy [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Odebrat AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)a [Nastavení AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) . Můžete také použít odkaz na [Postup při konfiguraci cloudovým službám](cloud-services-how-to-configure.md) Další informace.

 * Publikování projektu cloudové služby Azure, najdete pod odkazy ukázka kódu **PublishCloudService.ps1** z [Nepřetržitý doručení ke cloudové službě v Azure](cloud-services-dotnet-continuous-delivery.md).
