<properties
   pageTitle="Pomocí rutin prostředí PowerShell s Azure RemoteApp | Microsoft Azure"
   description="Informace o používání rutin prostředí Windows PowerShell v Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Pomocí rutin prostředí Windows PowerShell Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

 Rutiny prostředí RemoteApp PowerShell Azure slouží ke správě a udržovat vaše kolekce. Začněte pomocí následující informace.

## <a name="get-the-cmdlets"></a>Získání rutiny 
-------------
Nejdřív rutiny prostředí Powershell Azure Stáhnout [tady](http://go.microsoft.com/?linkid=9811175)RemoteApp rutin najdete v něm. 

Najdete v článku [nápovědy rutina Azure RemoteApp](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Konfigurace Azure rutiny, aby se pomocí svého předplatného
------------------
Postupovat podle [tohoto průvodce](../powershell-install-configure.md) , abyste mohli používat rutiny proti Azure předplatné.

Tento postup slouží k rychlý start:

1.  Stáhněte a nainstalujte [rutiny prostředí PowerShell Azure](http://go.microsoft.com/?linkid=9811175).
2.  Spusťte Microsoft Azure Powershellu.
3.  Spusťte **Přidat AzureAccount** ověření k předplatnému Azure. Po zobrazení výzvy zadejte stejné uživatelské jméno a heslo, které používáte pro přihlášení k portálu Azure.  
4.  Spusťte **Get-AzureSubscription** seznam předplatných spojený s vaším účtem. 
5.  Spusťte **AzureSubscription vyberte** a zadejte název předplatného nebo ID pro použití v konzole Powershellu.

Gratulujeme, je konzola Azure PowerShell nakonfigurován a je připraven k použití. Mějte na paměti, že budete muset repeate kroky 2 až 5 pokaždé, když začnete konzole Azure Powershellu.  

## <a name="create-a-cloud-collection"></a>Vytvoření kolekce cloudu
--------------------
Je jednoduché, spusťte tento příkaz:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Výše uvedený příkaz automaticky publikuje aplikace Microsoft Office 365 (Excel, OneNote, Outlooku, PowerPointu, Visio a Word).

Vytvoření kolekce může trvat 30 minut nebo déle. Proto tento příkaz vrátí ID sledování, které můžete použít následujícím způsobem:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Po dokončení kolekci můžete přidat uživatele do kolekce pomocí následujícího příkazu:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

A budete mít? Tento uživatel by měla připojit k aplikaci pomocí klienta Azure RemoteApp najdete [tady](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>K dispozici rutiny
Mnoho dalších příkazů, které máme, dokumentace pro ně budou přijít krátce:

Rutiny pro základní RemoteApp kolekce: 

- Nové AzureRemoteAppCollection
- Get-AzureRemoteAppCollection
- Nastavení AzureRemoteAppCollection
- Aktualizace AzureRemoteAppCollection
- Odebrat AzureRemoteAppCollection
- Přidání AzureRemoteAppUser
- Get-AzureRemoteAppUser
- Odebrat AzureRemoteAppUser
- Get-AzureRemoteAppSession
- Odpojení AzureRemoteAppSession
- Vyvolání AzureRemoteAppSessionLogoff
- Odeslat AzureRemoteAppSessionMessage
- Get-AzureRemoteAppProgram
- Get-AzureRemoteAppStartMenuProgram
- Publikování AzureRemoteAppProgram
- Zrušení publikování AzureRemoteAppProgram
- Get-AzureRemoteAppCollectionUsageDetails
- Get-AzureRemoteAppCollectionUsageSummary
- Get-AzureRemoteAppPlan

Rutiny pro RemoteApp virtuální sítě:

- Nové AzureRemoteAppVNet
- Get-AzureRemoteAppVNet
- Nastavení AzureRemoteAppVNet
- Odebrat AzureRemoteAppVNet
- Get-AzureRemoteAppVpnDevice
- Get - AzureRemoteAppVpnDeviceConfigScript
- Obnovení AzureRemoteAppVpnSharedKey

Rutiny pro obrázek šablony RemoteApp:

- Nové AzureRemoteAppTemplateImage
- Get-AzureRemoteAppTemplateImage
- Přejmenování AzureRemoteAppTemplateImage
- Odebrat AzureRemoteAppTemplateImage

Další RemoteApp rutiny:

- Get-AzureRemoteAppLocation
- Get-AzureRemoteAppWorkspace
- Nastavení AzureRemoteAppWorkspace
- Get-AzureRemoteAppOperationResult
 
