<properties
   pageTitle="Odkaz Azure architektura - IaaS: Provádění DMZ mezi Azure a Internetu | Microsoft Azure"
   description="Jak implementovat zabezpečené hybridní architektura sítě s přístupem k Internetu v Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>Provádění DMZ mezi Azure a na Internetu

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tento článek popisuje doporučené postupy pro implementaci zabezpečené hybridní síti, která slouží k rozšíření místních sítě, který přijme přenos ze sítě internet Azure. Tento odkaz architektura rozšiřuje architektura popsaná v článku [provádění DMZ mezi Azure a místních datacentra][implementing-a-secure-hybrid-network-architecture]. Přečtěte si tento dokument a porozumět tomu, že odkaz architektura před čtením tohoto dokumentu.

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento odkaz architektura používá správce zdroje, který Microsoft doporučuje nové nasazení. 

Typické použití případů pro tuto architekturu patří:

- Hybridní aplikace kde úloh spustit částečně místní a částečně v Azure.

- Azure infrastruktury, který směruje příchozích z místním prostředím a na Internetu.

## <a name="architecture-diagram"></a>Diagram architektury

V následujícím diagramu jsou uvedeny důležitou součástí tato architektura:

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento diagram je na stránce "DMZ – veřejné".

[! [0]][0]

- **Veřejnou IP adresu (PIP).** Toto je IP adresa veřejného koncového bodu. Externí uživatelé připojení k Internetu přístup k systému pomocí této adresy.

- **Síť virtuální zařízení (NVA).**  NVA je obecný termín, který popisuje OM provádění úloh, jako je povolení nebo odepření přístupu jako bránu firewall, optimalizace sítě WAN operace (včetně komprese sítě), vlastní směrování nebo jiných funkcí sítě.

- **Vyrovnávání zatížení Azure.** Všechny příchozí žádosti z Internetu přes tento Vyrovnávání zatížení a jsou úměrně NVAs ve veřejných DMZ příchozí podsítě.

- **Veřejné DMZ příchozí podsítě.** Tento podsítě přijímá žádosti z vyrovnávání zatížení Azure. Předávají se příchozí žádosti na jeden z NVAs v DMZ.

- **Veřejné DMZ odchozí podsítě.** Požadavky, které jsou schválené NVA Procházet tento podsítě Vyrovnávání zatížení interní pro web osy.

## <a name="recommendations"></a>Doporučení

Azure nabízí mnoho různých zdrojů a typů zdrojů, takže může být tento odkaz architektura zřízení mnoha různými způsoby. Správce prostředků Azure šablonu nainstalovat architektura odkaz, který následuje těmito doporučeními uvádíme. Pokud budete chtít vytvořit vlastní odkaz architektura Pokud požadujete konkrétní příliš velký doporučení by měl těmito doporučeními.

### <a name="nva-recommendations"></a>Doporučení NVA

Implementujte jednu sadu NVAs pro přenos pocházející na Internetu a jiné pro přenos pocházející místní. Je rizika zabezpečení použít pouze jednu sadu NVAs oba, protože tento návrh poskytuje žádné zabezpečení obvod mezi dvěma množinami provozu v síti. Je výhoda použít tento návrh, protože snižuje složitost zabezpečení pravidel kontroly a díky kterému je přehlednější která pravidla odpovídají každé příchozí žádosti síť. Například jednu sadu NVAs používá pravidla pro internetový provoz pouze při jinou sadu pravidel implementace NVAs místní pouze pro provoz.

Zahrnout vrstvy 7 NVA chcete ukončit připojení aplikací na úrovni NVA a udržovat spřažen úrovní back-end. Zaručuje, ve kterém odpověď adres úrovní back-end vrátí prostřednictvím NVA symetrickou připojení.  

### <a name="public-load-balancer-recommendations"></a>Doporučení Vyrovnávání zatížení veřejné ###

Abyste zachovali škálovatelnost a dostupnost, nasazení veřejné DMZ příchozí NVAs [Nastavte dostupnost] [ availability-set] a používat [internet protilehlé Vyrovnávání zatížení] [ load-balancer] k distribuci požadavky na Internetu přes NVAs v sadě dostupnosti.  

Konfigurace služby Vyrovnávání zatížení k přijímaní žádostí jenom na porty potřebné pro internetový provoz. Například omezte příchozí HTTP požadavky na port 80 a příchozí HTTPS požadavky na port 443.

## <a name="scalability-considerations"></a>Škálovatelnost: co byste měli zvážit

Navržení infrastruktury s internetové Vyrovnávání zatížení před příchozí veřejné DMZ podsítě od začátku. I když architektura initally vyžaduje jeden NVA, bude snadněji přizpůsobit několika NVAs v budoucnu Pokud nasadili internetové Vyrovnávání zatížení.

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Internetové služby Vyrovnávání zatížení vyžaduje každý NVA ve veřejných DMZ příchozí podsítě implementovat [stavu zkušební][lb-probe]. Zkušební stavu, který přestane odpovídat na tento koncový bod se považuje za není k dispozici a vyrovnávání zatížení bude směrovat požadavky jiných NVAs v sadě stejné dostupnosti. Všimněte si, že pokud všechny NVAs reagovat, aplikace se nepovede, takže je důležité, abyste měli sledování nakonfigurované tak, aby upozornění DevOps-li počet instancí NVA správný pod prahovou hodnotu.

## <a name="manageability-considerations"></a>Důležité informace o možnostech správy

Omezení funkcí monitorování a správu pro příchozí veřejné DMZ NVA na odpovědět na požadavky v poli odkaz v pouze podsítě správy. Jak je uvedeno v [implementaci DMZ mezi Azure a místních datacentra] [ implementing-a-secure-hybrid-network-architecture] dokument, určíte jedné sítě směrování z místní síti prostřednictvím brány do pole odkaz v podsítě správy omezení přístupu.

Pokud je připojení brány z místní síti k Azure dolů, pořád zobrazíte do pole odkaz nasazením PIP ho přidáte do pole odkaz a Vzdálená v z Internetu.

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Tento odkaz architektura implementuje několik úrovní zabezpečení:

- Internetové služby Vyrovnávání zatížení směrovat tak, aby požadavky NVAs příchozí veřejné DMZ podsítě pouze a pouze na porty potřebné pro aplikaci.

- NSG pravidla pro příchozí a odchozí veřejné DMZ podsítě zabránit NVAs ohroženo blokováním požadavky, které spadající mimo NSG pravidla.

- Do směrování konfigurace překladu adres pro NVAs směrovat příchozí požadavky na port 80 a 443 na Vyrovnávání zatížení osy web, ale požadavky na jiné porty ignoruje.

Všimněte si, že má protokolovat všechny příchozí žádosti na všechny porty. Pravidelná kontrola protokoly, přičemž pozornost do žádosti o spadající mimo očekávané parametry tyto mohou poukazovat pokusy o průnik.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Použití NSGs do bloku/průchodu přenosy mezi aplikací vrstev

Všech úrovní webový server, business a data omezit přenosy mezi nimi pomocí NSGs. To znamená osy firmy používá NSG Pokud chcete blokovat všechny přenosy, které nemá pocházejí ve vrstvě web a osy dat pomocí NSG blokovat všechny přenosy, že není pocházejí z firmy osy. Pokud požadujete rozbalte pravidla NSG širší přístupu k tyto vrstvy, zvažte tyto požadavky hlediska zabezpečení rizikové. Každý nový příchozí cesty představuje příležitost náhodné nebo purposeful data únik nebo aplikace škod.

## <a name="solution-deployment"></a>Nasazení řešení

Nasazení pro implementující těmito doporučeními architekturu odkaz je dostupný na Github. Tento odkaz architektura obsahuje virtuální sítě (VNet), skupiny zabezpečení síti (NSG), Vyrovnávání zatížení a dvě virtuálních počítačích (VMs).

Architektura odkaz můžete nasadit buď s Windows nebo Linux VMs podle pokynů níže: 

1. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně":  
[![Nasazení Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. Jakmile otevře na odkaz v portálu Azure, musíte zadat hodnoty některá nastavení: 
    - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte **Vytvořit nový** a zadejte `ra-public-dmz-network-rg` do textového pole.
    - Vyberte oblast z **umístění** rozevíracím seznamu.
    - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
    - Vyberte **Typ Os** v rozevíracím poli, **windows** a **linux**.
    - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
    - Klikněte na tlačítko **koupit** .

3. Počkejte nasazení dokončete.

4. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně":  
[![Nasazení Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. Jakmile otevře na odkaz v portálu Azure, musíte zadat hodnoty některá nastavení: 
    - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte **Vytvořit nový** a zadejte `ra-public-dmz-wl-rg` do textového pole.
    - Vyberte oblast z **umístění** rozevíracím seznamu.
    - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
    - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
    - Klikněte na tlačítko **koupit** .

6. Počkejte nasazení dokončete.

7. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně":  
[![Nasazení Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. Jakmile otevře na odkaz v portálu Azure, musíte zadat hodnoty některá nastavení: 
    - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte možnost **Použít existující** a zadejte `ra-public-dmz-network-rg` do textového pole.
    - Vyberte oblast z **umístění** rozevíracím seznamu.
    - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
    - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
    - Klikněte na tlačítko **koupit** .

9. Počkejte nasazení dokončete.

10. Parametr soubory obsahují pevně uživatelské jméno a heslo správce pro všechny VMs a důrazně doporučujeme okamžitě změnit obojí. Pro každou OM nasazení vyberte Azure portálu a klikněte na **Resetovat heslo** v zásuvné **podpory + Poradce při potížích** . Vyberte **Resetovat heslo** do pole rozevíracího seznamu **režim** a potom vyberte nové **uživatelské jméno** a **heslo**. Klikněte na tlačítko **Aktualizovat** uchovávat.


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "Zabezpečené architektura sítě hybridní"
