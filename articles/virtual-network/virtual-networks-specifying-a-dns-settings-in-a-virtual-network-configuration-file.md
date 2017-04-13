<properties 
   pageTitle="Nastavení DNS v souboru konfigurace virtuální sítě | Microsoft Azure"
   description="Jak lze změnit nastavení serveru DNS v síti virtuální pomocí konfiguračního souboru virtuální sítě v modelu klasické nasazení"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Nastavení DNS v souboru konfigurace virtuální sítě

Konfigurační soubor sítě obsahuje dva prvky, které můžete použít k určení nastavení systému DNS (Domain Name): **DnsServers** a **DnsServerRef**. Přidání seznamu serverů DNS tím, že zadáte jeho IP adresy a názvy odkaz do prvku **DnsServers** . Pak můžete prvek **DnsServerRef** můžete určit, které DNS server položky z DnsServers element slouží k jiné síti weby v rámci virtuální sítě.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení.

Konfigurační soubor síti může obsahovat tyto prvky. Název každého element je propojený na stránku, která obsahuje další informace o nastavení hodnotu prvku.

>[AZURE.IMPORTANT] Informace o tom, jak konfigurovat konfiguračního souboru sítě najdete v tématu [Konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md). Informace o jednotlivých prvků obsažené v souboru konfigurace sítě najdete v tématu [Azure virtuální sítě konfigurace schéma](https://msdn.microsoft.com/library/azure/jj157100.aspx).

[DNS Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] **Název** atributu v elementu **Server_dns** se používá pouze jako odkaz pro prvek **DnsServerRef** . Název hostitele DNS server nepředstavuje. Každá hodnota atributu **Server_dns** musí být jedinečný napříč celou předplatného Microsoft Azure

[Virtuální sítě weby prvek](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Chcete-li určit toto nastavení pro prvek virtuální sítě weby, ho musí být v dříve definované DNS element. DnsServerRef *název* v elementu virtuální sítě webů musí odkazovat na hodnoty název zadané v prvku DNS pro Server_dns *název*.

## <a name="next-steps"></a>Další kroky

- Princip [schéma konfigurace Azure virtuální sítě](http://go.microsoft.com/fwlink/?LinkId=248093).
- Princip [schématu konfigurace služby Azure](https://msdn.microsoft.com/library/windowsazure/ee758710).
- [Nakonfigurujte virtuální síť pomocí síťové konfigurace soubory](virtual-networks-using-network-configuration-file.md).
