
<properties
    pageTitle="Seznam porty a adresy URL povolených pro Azure RemoteApp používaný v síti virtuální zákazníka | Microsoft Azure"
    description="Informace, které porty adresy URL a bude potřeba nakonfigurovat komunikaci prostřednictvím Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Seznam porty a adresy URL k povolení přístupu pro Azure RemoteApp používaný v zákazníka virtuální sítě 

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Platí následující pravidla Azure RemoteApp cloudu nebo hybridní kolekce Pokud nasazujete v síti virtuální (VNET). Další informace o virtuálních sítí přečtěte si prosím [Virtuální přehled sítě](../virtual-network/virtual-networks-overview.md). Pokud jste vytvořili skupinu zabezpečení sítí (NSG) omezení přenosy k prostředkům virtuální sítě, které jste vybrali pro Azure RemoteApp, přejděte prosím zkontrolujte, že následují přístupných osobám s postižením a povolené prostřednictvím zásad zabezpečení virtuální síti se systémem. Další informace o skupiny zabezpečení sítě přečtěte si [Co je skupina zabezpečení sítě? (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure podsítě RemoteApp potřebuje přístup k těmto koncové body a adresy URL: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.NET  
*    Odchozí: TCP: 443, TCP: 10101 10175 Document 
*    Volitelné – UDP: 10201 10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure klientů RemoteApp potřebují mít přístup k těmto koncové body a adresy URL: 

Klienti můžu nechtěli plochy zařízení atd pracovníky připojení k pomocí aplikací nasazena v kolekci Azure RemoteApp.

-  https://telemetry.RemoteApp.windowsazure.com  
-  https://*.RemoteApp.windowsazure.com (volitelné UDP portů jsou pro tuto adresu) 
-  https://Login.Windows.NET  
-  https://Login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.NET  
-  Odchozí: TCP: 443  
-  Volitelné – UDP: 3391 