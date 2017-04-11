<properties
    pageTitle="Azure AD Domain Services: Sítě pokyny | Microsoft Azure"
    description="Co byste měli zvážit sítě pro Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Co byste měli zvážit sítě pro službu Azure AD Domain Services

## <a name="how-to-select-an-azure-virtual-network"></a>Postup při výběru Azure virtuální sítě
Následující pokyny vám vyberte virtuální sítě pomocí služby Azure AD Domain Services.

### <a name="type-of-azure-virtual-network"></a>Typ Azure virtuální sítě

- Můžete povolit Azure AD Domain Services v síti klasické Azure virtuální.

- Azure AD doméně služby **nelze povolit ve virtuálních sítí vytvořené pomocí Správce prostředků Azure**.

- Správce prostředků síti založené na virtuální můžete připojit k klasické virtuální sítě, ve kterém je povolený Azure AD Domain Services. Azure AD Domain Services, můžete na základě správce prostředků virtuální sítě. Další informace naleznete v části [připojení k síti](active-directory-ds-networking.md#network-connectivity) .

- **Místní virtuální sítě**: Pokud nebudete chtít použít existující virtuální sítě, zajistit, že jde o místní virtuální sítě.

    - Virtuální sítích, které používají starší verze skupiny spřažení mechanismus nelze použít s Azure AD Domain Services.

    - Použití služby Azure AD domény [migrace starší verze virtuální sítě pro místní virtuální sítě](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).


### <a name="azure-region-for-the-virtual-network"></a>Azure oblast pro virtuální sítě

- Služby Azure AD doménu spravovanou domény používaný v oblasti stejné Azure jako virtuální sítě můžete povolit službu v.

- Vyberte virtuální sítě v Azure oblasti nepodporuje Azure AD Domain Services.

- Najdete na stránce [služby Azure podle regionů](https://azure.microsoft.com/regions/#services/) znát Azure oblasti, ve kterých je k dispozici Azure AD Domain Services.


### <a name="requirements-for-the-virtual-network"></a>Požadavky pro virtuální sítě

- **Blízkosti Azure pracovního vytížení**: vyberte virtuální sítě, které aktuálně hostuje/uspořádá virtuálních počítačích, které potřebují mít přístup ke službám Azure AD Domain.

- **Servery DNS vlastní/přenést e vlastní**: Ujistěte se, že nejsou žádné vlastní servery DNS nakonfigurovány virtuální sítě.

- **Existující domény se stejným názvem domény**: zajistit, že nemusíte stávající domény se stejným názvem domény k dispozici v této virtuální síti. Například se předpokládá, že máte doménu s názvem "contoso.com" už k dispozici na vybraný virtuální sítě. Později pokusíte povolit doménu spravovaný Azure AD Domain Services s stejný název domény (to znamená "contoso.com"), které virtuální síti se systémem. Při používání narazíte na chybu při pokusu o povolení Azure AD Domain Services. Tato chyba je kvůli konflikty jmen pro název domény, které virtuální síti se systémem. V takovém případě musí použijte jiný název pro nastavení domény spravovaných Azure AD Domain Services. Případně můžete zrušit zřízení stávající domény a potom přejděte k povolení Azure AD Domain Services.

> [AZURE.WARNING] Domain Services nelze přejít k jiné síti virtuální po povolení služby.


## <a name="network-security-groups-and-subnet-design"></a>Skupiny zabezpečení sítě a podsítě návrhu
[Skupina zabezpečení síti (NSG)](../virtual-network/virtual-networks-nsg.md) obsahuje seznam pravidel seznam řízení přístupu (ACL), která povolit nebo odepřít v síti na OM instance v síti virtuální. NSGs jde přidružit podsítí nebo jednotlivé instance OM v rámci této podsítě. Po přidružené k podsítě NSG ACL pravidla platí pro všechny instance OM v této podsítě. Umožnění datových přenosů do jednotlivých OM navíc může být omezený další přidružením NSG přímo do této OM.

![Návrhové doporučená podsítě](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>Doporučené postupy pro výběr podsítě
- Nasazení služby Azure AD domény **samostatné vyhrazené podsítě** v rámci Azure virtuální sítě.

- Se nevztahují NSGs vyhrazené podsítě pro vaši doménu spravovaný. Pokud použijete musí NSGs vyhrazené podsítě, zajistit, aby se **nebudou blokovat porty muset služby a spravovat svoji doménu**.

- Neomezovat příliš počet dostupných v rámci vyhrazené podsítě pro vaši doménu spravovaný IP adres. Toto omezení zabrání službu zpřístupnění dva řadiče domény pro vaši doménu spravovaný.

- **Nepovolovat Azure AD Domain služby v podsítě brány** virtuální sítě.


> [AZURE.WARNING] Když přiřadíte NSG s podsítí, ve kterém Azure AD Domain služby je povolena, mohou přerušit společnosti Microsoft možnost služby a spravovat domény. Kromě toho je přerušení synchronizace mezi Azure AD klienta a spravovaných domény. **SLA netýká nasazení kde NSG, které byly použity, která blokuje Azure AD Domain Services z aktualizace a správy domény.**


### <a name="ports-required-for-azure-ad-domain-services"></a>Porty požadovaný pro služby Azure AD domény
Tyto porty jsou potřeba pro služby Azure AD doméně služby a spravovat spravovaná domény. Ujistěte se, že tyto porty se nebudou blokovat podsítě, ve kterém jste povolili spravované domény.

| Číslo portu | Účel |
|---|---|
| 443 | Synchronizace s Azure AD klienta |
| 3389 | Správa vaší domény |
| 5986 | Správa vaší domény |
| 636 | Zabezpečený přístup LDAP (LDAPS) pro vaši doménu spravovaný |



## <a name="network-connectivity"></a>Připojení k síti
Spravované domain Azure AD Domain Services může být užitečné pouze v rámci jedné sítě klasické virtuální v Azure. Virtuální sítě vytvořené pomocí Správce prostředků Azure nejsou podporované.


### <a name="scenarios-for-connecting-azure-networks"></a>Scénáře propojení Azure sítí
Připojení Azure virtuálních sítí používat spravované doménu, v jakémkoli z následujících scénářích nasazení:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Použití spravované domény v víc Azure klasické virtuální sítě
Jiné Azure sítě klasické virtuální můžete připojit k Azure klasické virtuální sítě, ve kterém jste povolili Azure AD Domain Services. Toto připojení VPN umožňuje používat spravované doménu, se vaše úlohami nasazenou v jiných sítích virtuální.

![Klasický virtuální síťové připojení](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Používat spravované doménu, v síti virtuální na základě správce prostředků
Správce prostředků síti založené na virtuální můžete připojit k Azure klasické virtuální sítě, ve kterém jste povolili Azure AD Domain Services. Toto připojení umožňuje používat spravované doménu, se vaše úlohami nasazenou v na základě správce prostředků virtuální sítě.

![Správce prostředků klasické virtuální síťové připojení](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>Možnosti síťových připojení

- **Připojení VNet VNet pomocí připojení VPN k webu**: virtuální sítě připojení k jiné síti virtuální (VNet VNet) je podobný připojení k umístění webu místní virtuální sítě. Oba typy připojení se k zajištění zabezpečené tunelem pomocí IPsec/IKE používají VPN brány.

    ![Připojení k síti virtuální pomocí Brána VPN](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Další informace – připojit pomocí Brána VPN virtuální sítě](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **Připojení VNet VNet pomocí prozkoumávání virtuální sítě**: virtuální sítě prozkoumávání je mechanismus, který propojuje dva virtuální sítí ve stejné oblasti prostřednictvím Azure páteřní síti. Jakmile peered, dvě virtuální sítě se zobrazí jako jeden z důvodů všechna připojení. Je pořád spravováno jako samostatný zdroje, ale virtuálních počítačích v těchto virtuální sítě můžou vzájemně komunikovat přímo pomocí soukromé IP adres.

    ![Připojení k síti virtuální pomocí prozkoumávání](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Další informace: virtuální sítě prozkoumávání](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>Související obsah

- [Prozkoumávání Azure virtuální sítě](../virtual-network/virtual-network-peering-overview.md)

- [Konfigurace připojení VNet VNet pro klasické nasazení modelu](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Skupiny zabezpečení Azure sítě](../virtual-network/virtual-networks-nsg.md)
