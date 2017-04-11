<properties
    pageTitle="Jak nastavit připojení VPN správy rozhraní API Azure"
    description="Zjistěte, jak nastavit připojení k síti VPN v Azure rozhraní API správu a přístup k webovým službám přes něj."
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>Jak nastavit připojení VPN správy rozhraní API Azure

Podpora VPN rozhraní API řízení umožňuje připojit vaše Brána pro správu rozhraní API k síti virtuální Azure (klasický). Díky rozhraní API správy zákazníkům zabezpečené připojení k jejich back-end webové služby, které jsou místní nebo jinak nedostupné pro veřejné Internetu.

>[AZURE.NOTE] Správa Azure rozhraní API spolupracuje klasické VNETs. Informace o vytváření klasické VNET najdete v tématu [vytvoření virtuální sítě (klasické) pomocí portálu Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Informace o připojení klasické VNETs ARM VNETS najdete v tématu [připojení klasické VNets do nového VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="enable-vpn"> </a>Povolit sítě VPN

>Připojení k síti VPN je dostupná jenom v úrovní **Premium** a **Vývojář** . Přejděte k němu otevřete služby správy rozhraní API [Klasické portál Azure][] a pak otevřete kartu **Měřítko** . V části **Obecné** vyberte Premium vrstvy a klikněte na Uložit.

Povolit připojení k síti VPN, otevřete rozhraní API Správa služby [Azure klasické portál][] a přejděte na kartu **Konfigurovat** . 

V části VPN přejděte **připojení prostřednictvím sítě VPN** **na**.

![Konfigurace kartu instance rozhraní API správy][api-management-setup-vpn-configure]

Zobrazí se nyní zobrazí seznam všech oblastí, kde máte k dispozici správy rozhraní API služeb.

Vyberte VPN a podsítě pro každý oblast. Seznam VPN je vyplněné podle virtuální součástí předplatného Azure sítí, které jsou nastavení v oblasti, které chcete nakonfigurovat.

![Vyberte VPN][api-management-setup-vpn-select]

Klepněte na tlačítko **Uložit** v dolní části obrazovky. Nebude moct provádět jiné operace na službu pro správu rozhraní API z portálu Microsoft Azure klasické aktualizace. Služba brány zůstanou k dispozici a runtime hovory byste neměli mít vliv na.

Všimněte si, že adresa VIP brány se změní pokaždé, když VPN povolit nebo zakázat.

## <a name="connect-vpn"> </a>Připojení k webové službě za VPN

Po služby správy rozhraní API připojení k síti VPN, přístup k webovým službám v rámci virtuální sítě neliší od přístup ke službám veřejnosti. Stačí zadat místní adresy nebo název hostitele (Pokud DNS server nakonfigurovaný pro virtuální sítě Azure) webové služby do pole **Adresa URL webové služby** při vytváření nového rozhraní API nebo úpravy existující úrovně.

![Přidání rozhraní API z VPN][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>Požadované porty pro rozhraní API správy VPN podpory

Když instanci služby správy rozhraní API je hostovaný ve VNET, používá porty v následující tabulce. Pokud tyto porty blokovány, službu nemusí fungovat správně. Máte jednu nebo více tyto porty blokované problém nejběžnější stav při používání rozhraní API správy s VNET.

| Porty                      | Směr        | Transport Protocol (protokol) | Účel                                                          | Zdrojový nebo cílový              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| 80, 443                      | Příchozí          | TCP                | Komunikace klienta do rozhraní API správy                           | INTERNET / VIRTUAL_NETWORK        |
| 80,443                       | Odchozí         | TCP                | Rozhraní API Správa závislostí na Azure úložiště a služby Azure Bus | VIRTUAL_NETWORK NEBO INTERNET        |
| 1433                         | Odchozí         | TCP                | Rozhraní API Správa závislostí na SQL                               | VIRTUAL_NETWORK NEBO INTERNET        |
| 9350 9351, 9352, 9353, 9354 | Odchozí         | TCP                | Rozhraní API Správa závislostí na Bus služby                       | VIRTUAL_NETWORK NEBO INTERNET        |
| 5671                         | Odchozí         | AMQP               | Rozhraní API Správa závislostí protokolu událost centrální zásad            | VIRTUAL_NETWORK NEBO INTERNET        |
| 6381, 6382, 6383             | Příchozí nebo odchozí | UDP                | Rozhraní API Správa závislostí na Redis mezipaměti                       | VIRTUAL_NETWORK / VIRTUAL_NETWORK |
| 445                          | Odchozí         | TCP                | Rozhraní API Správa závislostí na Azure sdílené složky pro libovolná            | VIRTUAL_NETWORK NEBO INTERNET        |

## <a name="custom-dns"> </a>Instalační program vlastní DNS server

Správa rozhraní API závisí na počet Azure služeb. Když instanci služby správy rozhraní API je hostovaný ve VNET použití vlastní DNS server, je třeba vyřešit názvy hostitelů tyto služby Azure. Postupujte podle [návod](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) na vlastní nastavení služby DNS.  

## <a name="related-content"> </a>Související obsah


* [Vytvořit virtuální sítě s připojení VPN k webu na portálu Azure klasické][]
* [Použití rozhraní API inspektor trasování volá správy rozhraní API Azure][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure klasické portálu]: https://manage.windowsazure.com/

[Vytvořit virtuální sítě s připojení VPN k webu na portálu Azure klasické]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[Použití rozhraní API inspektor trasování volá správy rozhraní API Azure]: api-management-howto-api-inspector.md
