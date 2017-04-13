<properties 
   pageTitle="Určení nastavení DNS v souboru konfigurace služby | Microsoft Azure"
   description="Nastavení vlastních DNS pomocí konfiguračního souboru služby pro virtuální sítě"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Určení nastavení DNS v souboru konfigurace služby

## <a name="dns-elements"></a>Prvky DNS

Konfigurační soubor služby může obsahovat prvek DnsServers se seznamem adres protokolu IPv4 pro servery systému DNS (Domain Name), která budou používat službu. Nastavení v souboru konfigurace služby mají přednost před nastavením v souboru konfigurace sítě. Další informace najdete v tématu [Konfigurace schématu služby Azure (.cscfg soubor)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration prvek**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] **Název** atributu v elementu **Server_dns** se používá pouze jako název odkazu. Název hostitele DNS server nepředstavuje. Každá hodnota atributu **Server_dns** musí být jedinečný napříč celou předplatného Microsoft Azure.

## <a name="see-also"></a>Viz taky

[Schéma konfigurace služby Azure (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Schéma konfigurace Azure virtuální sítě](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfigurace virtuální sítě pomocí síťové konfigurace soubory](http://go.microsoft.com/fwlink/?LinkId=248094)

[Informace o nastavení virtuální sítě v portálu pro správu](http://go.microsoft.com/fwlink/?LinkId=248092)

