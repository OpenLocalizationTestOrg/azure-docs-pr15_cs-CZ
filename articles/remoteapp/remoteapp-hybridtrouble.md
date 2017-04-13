
<properties
    pageTitle="Poradce při potížích s vytvářením kolekce hybridní RemoteApp | Microsoft Azure"
    description="Zjistěte, jak řešit problémy s chyby vytváření kolekce hybridní RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Poradce při potížích s vytváření kolekce hybridní Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Hybridní kolekce je hostovaný ve a uchovává data v Azure cloudu, ale taky umožňuje uživatelům přístup k datům a zdroje informací uložených v místní síti. Uživatelé můžou získat přístup ke aplikace přihlašování pomocí své firemní přihlašovací údaje synchronizované nebo federovaní Azure Active Directory. Můžete nasadit kolekce hybridní využívající existující virtuální sítě Azure nebo můžete vytvořit novou síť virtuální. Doporučujeme vytváříte nebo používáte podsítě virtuální sítě s rozsahem CIDR dostatečně velký k tomu očekávané budoucí růstu pro Azure RemoteApp.

Nevytvořili kolekci ještě? Postup v tématu [Vytvoření hybridní kolekce](remoteapp-create-hybrid-deployment.md) .

Pokud se vám nedaří vytváření kolekci nebo pokud kolekci nefunguje tak, jak si myslíte, že by měl být vidět, podívejte se na následující informace.

## <a name="your-image-is-invalid"></a>Obrázek nejsou platná: ##
Pokud při čekáte Azure zřízení kolekci uvidíte zprávu podobnou "GoldImageInvalid", znamená to, že obrázek šablony nesplňuje [definované požadavky na obrázek](remoteapp-imagereqs.md). Přejděte Ano, přečtěte si tyto [požadavky](remoteapp-imagereqs.md), opravíte obrázek a vytvořte kolekci znovu.



## <a name="does-your-vnet-have-network-security-groups-defined"></a>Má váš VNET definované skupiny zabezpečení sítě? ##
Pokud máte skupiny zabezpečení sítě definovaných ve podsítě používáte pro svou kolekci, zkontrolujte, že tyto [adresy URL a porty](remoteapp-ports.md) jsou dostupné v aplikaci vaší podsítě.

Skupiny zabezpečení další síťové můžete přiřadit VMs uskutečněné v podsítě užší ovládacího prvku.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Používáte serverů DNS? A že jsou přístupné z vaší podsítě VNET? ##
>[AZURE.NOTE] Budete muset zkontrolujte, jestli je servery DNS ve vaší VNET vždy nahoru a vždy možné řešení virtuálních počítačích použitý ve VNET. Nepoužívejte Google DNS pro tuto.


Pro hybridní kolekce použijete serverů DNS. Můžete určit jejich počet ve vaší síti konfigurace schématu nebo prostřednictvím portálu pro správu při vytváření virtuálních sítě. Servery DNS používáme ve směru, aby byla vyplněná způsobem převzetí (na rozdíl od kruhového).  
Získáte [Překlad VMs a rolí instance](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) aby zkontrolovala, jestli že jsou nakonfigurované correcly serverů DNS.

Ujistěte se servery DNS pro vaši kolekci jsou k dispozici a poskytuje společnost podsítě VNET zadaná pro tuto kolekci.

Příklad:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Definování DNS](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Používáte ve vaší kolekci řadiče domény Active Directory? ##
Nyní jen jednu doménu služby Active Directory jde přidružit Azure RemoteApp. Hybridní kolekce podporuje pouze Azure Active Directory účty, které byly synchronizované pomocí nástroje DirSync ze služby Active Directory pro Windows Server nasazení; Konkrétně nebo synchronizují s parametrem synchronizace hesel synchronizují nakonfigurovali federace Active Directory Federation Services (AD FS). Potřebujete vytvořit vlastní doménu, která odpovídá Přípony UPN domény pro místní domény a nastavení integrace adresářů.

Další informace najdete v tématu [Konfigurace Active Directory Azure RemoteApp](remoteapp-ad.md) .

Ujistěte se, že doména podrobnosti k dispozici jsou platné, a řadiče domény se k nim přistupovat z OM vytvořené v podsítě používá aplikací Remote Azure. Také účtu pověření služby oprávnění přidat počítače na zadané doménu a zkontrolujte, že zadaný AD název může být přeložena z uvedených v VNET DNS.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Zadejte název domény určíte, když jste vytvořili kolekci? ##

Název domény, vytvoření nebo přidání musí být vnitřní název_domény (ne názvu domény Azure AD) a možností vyřešení DNS formát (contoso.local) musí odpovídat. Například máte interní název služby Active Directory (contoso.local) a Active Directory Přípony (contoso.com) – je nutné použít název interního když vytvoříte kolekci.
