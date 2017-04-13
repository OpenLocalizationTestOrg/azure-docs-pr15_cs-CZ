<properties
   pageTitle="Správa Azure zabezpečení a sledování přehled | Microsoft Azure"
   description=" Azure poskytuje mechanismy zabezpečení pro usnadnění řízení a sledování Azure cloudovými službami a virtuálních počítačích.  Tento článek obsahuje přehled těchto základních zabezpečení funkcí a služeb. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-security-management-and-monitoring-overview"></a>Správa Azure zabezpečení a sledování základní informace

Azure poskytuje mechanismy zabezpečení pro usnadnění řízení a sledování Azure cloudovými službami a virtuálních počítačích. Tento článek obsahuje přehled těchto základních zabezpečení funkcí a služeb. Jsou uvedeny odkazy na články, které vám umožní podrobnosti každé tak další informace.

Zabezpečení cloudovým službám společnosti Microsoft se partnerství a sdílené odpovědnosti mezi vámi a Microsoft. Sdílené zodpovědnost znamená Microsoft je zodpovědný za Microsoft Azure a fyzické zabezpečení její data Center (použitím zabezpečení ochrany například uzamčené Odznáček položka dveří, ochranné a chrání). Kromě toho Azure úrovní silných zabezpečení cloudové ve vrstvě software, který vyhovuje zabezpečení, ochrana osobních údajů a dodržování předpisů náročné zákazníků.

Vlastní data a identit, zodpovědnost chránících je zabezpečení místních zdrojů a zabezpečení cloudové součásti vystavujete ovládáte. Microsoft poskytuje ovládací prvky zabezpečení a funkce, které pomáhají chránit vaše data a aplikací. Odpovědnost za zabezpečení se podle typu cloudové služby.

V následující tabulce je uveden souhrn zůstatek zodpovědnost společnosti Microsoft a zákazníka.

![Sdílené odpovědnosti][1]

Hlubší postupy na stránce pro správu zabezpečení najdete v článku [Správa zabezpečení v Azure](azure-security-management.md).

Tady jsou základní funkce vztahovat v tomto článku:

- Řízení přístupu na základě rolí
- Antimalware
- Vícefaktorové ověřování
- ExpressRoute
- Virtuální sítě bran
- Správa privilegovaných identit
- Ochrana identity
- Centrum zabezpečení

## <a name="role-based-access-control"></a>Řízení přístupu na základě rolí

Na základě rolí řízení přístupu (RBAC) obsahuje Správa jemně odstupňovaná přístupu pro Azure zdroje. Použití RBAC, můžete udělit lidé počtu přístup, kterou potřebují k provedení své práce.  RBAC také vám pomůže zajistit, že když lidé opustit organizaci budou ztratíte přístup k zdrojů v cloudu.

Víc se uč:

- [Blog týmu Active Directory na RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
- [Řízení přístupu na základě rolí Azure](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Antimalware

Azure pomocí můžete software antimalware dodavatelům hlavní zabezpečení například Microsoft Symantec, Trend Micro, McAfee a Kaspersky se můžete pomoci chrá virtuálních počítačích z nebezpečného soubory, adware a jiných hrozeb.

Microsoft Antimalware nabízí možnost instalace agenta antimalware PaaS rolí a virtuálních počítačích. Podle System Center Endpoint Protection tuto funkci přináší ověřené místního technologii zabezpečení do cloudu.

Nabízíme rozsáhlá integrace LINTREND [Důkladné zabezpečení](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ a [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ produkty v Azure platformu. DeepSecurity představuje antivirové řešení a SecureCloud je řešení pro šifrování. DeepSecurity má být nasazené uvnitř VMs pomocí rozšíření modelu. Pomocí portálu uživatelského rozhraní a Powershellu, můžete použít DeepSecurity uvnitř nové VMs, které jsou právě nespředený nahoru nebo existující VMs, které jsou již nainstalovali.

Ochrana koncový bod Symantec (září) podporuje i na Azure. Pomocí portálu integrace zákazníci měli možnost určit, že mají v úmyslu použít září do virtuálního počítače. Oddělovač je možné nainstalovat na ještě nepracovali OM prostřednictvím portálu Azure nebo je možné nainstalovat na existující OM pomocí Powershellu.

Víc se uč:

- [Nasazení řešení Antimalware na Azure virtuálních počítačích](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Microsoft Antimalware pro Azure cloudovými službami a virtuálních počítačích](../security/azure-security-antimalware.md)
- [Jak nainstalovat a nakonfigurovat Trend Micro důkladné zabezpečení jako služba ve Windows OM](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Jak nainstalovat a nakonfigurovat aplikaci Symantec Endpoint Protection na Windows OM](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Nové možnosti Antimalware chránících Azure virtuálních počítačích – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Vícefaktorové ověřování

Azure vícefaktorové ověřování (MFA) je metody ověřování, který je potřeba použít více než jednu metodu ověření a uživatel přihlášení, začátky transakce se přidá kritické druhý vrstvy cenného papíru. MFA pomáhá chránit přístup k datům a aplikace během schůzky požádání uživatele pro jednoduchý proces přihlášení. Poskytuje silných ověřovat pomocí celou řadu možností ověření – telefonní hovor, textová zpráva nebo mobilní aplikaci oznámení nebo ověření kód a 3rd stran MÍSTOPŘÍSEŽNÉM tokeny.

Víc se uč:

- [Vícefaktorové ověřování](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Co je Azure Vícefaktorové ověřování?](../multi-factor-authentication/multi-factor-authentication.md)
- [Jak funguje Azure Vícefaktorové ověřování](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute

Microsoft Azure ExpressRoute umožňuje rozšíření místních sítí do cloudu společnosti Microsoft přes vyhrazené soukromé připojení usnadnit poskytovatel připojení. S ExpressRoute můžete vytvořit připojení ke cloudovým službám společnosti Microsoft, jako je Microsoft Azure Office 365 a CRM Online. Připojení může být z libovolného libovolného sítě (IP VPN), v síti Ethernet bod k nebo virtuální křížově připojení prostřednictvím poskytovatele připojení v zařízení společné umístění. ExpressRoute připojení nepřekročí veřejné Internetu. Díky ExpressRoute připojení nabízet další spolehlivost rychlejší rychlosti, malá čekacích dob a vyšší zabezpečení než typický připojení přes Internet.

Víc se uč:

- [ExpressRoute technický přehled](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuální sítě bran

Virtuální privátní sítě bran taky se mu říká Azure virtuální sítě bran se používají k odeslání mezi virtuálních sítí a místní umístění v síti. Používají se také k přenosu mezi více virtuální sítí v Azure (VNet VNet).  VPN bran poskytují zabezpečené křížově místní propojení mezi Azure a infrastrukturu.

Víc se uč:

- [Informace o VPN brány](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Přehled zabezpečení Azure sítě](security-network-overview.md)

## <a name="privileged-identity-management"></a>Správa privilegovaných identit

Uživatelé někdy potřebovat provádět privilegovaných operace v Azure zdrojů nebo jiných aplikacích SaaS. Často znamená to, že organizacemi muset udělte jim přístupová oprávnění trvalé privilegovaných v Azure Active Directory (Azure AD). Totiž rostoucí rizika zabezpečení pro hostované cloudové zdroje organizace nemůžete sledovat dostatečně co dělají tito uživatelé s oprávněním přístup.
Kromě toho pokud ohroženo uživatelský účet s přístup privilegovaných daného jeden porušení můžou mít vliv na celkové zabezpečení cloudové. Správa identit Azure AD oprávněními pomůže vyřešit toto riziko snížit čas oprávnění a zvýšením přehled použití.  

Privilegovaných Správa identit zavádí koncept dočasná správce pro role nebo "pouze v čase" přístupem na úrovni správce, což je uživatel, který dokončit proces aktivace pro přiřazené role. Aktivační proces změny přiřazení uživatele role v Azure AD z neaktivní na aktivní pro zadaný časový období například 8 hodin.

Víc se uč:

- [Správa identit Azure AD oprávněními](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Začínáme s Azure AD privilegovaných Správa identit](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Ochrana identity

Ochrana Identity Azure Active Directory (AD) nabízí konsolidované zobrazení podezřelých přihlašovací aktivit a potenciální chyby se můžete pomoci chrá vaší firmě. Ochrana identity rozpozná podezřelých aktivit uživatelů a identit oprávněními (Správci) podle signály jako hrubou útokům prozrazený přihlašovací údaje a přihlášení z cizí umístění a infekce zařízení.

Zadáním oznámení a doporučené remediation ochrana Identity umožňuje zmírnění rizik v reálném čase. Vypočítá závažnosti rizika uživatelů a konfigurace zásad rizika automaticky pomůže chránit aplikace přístup z budoucích hrozeb.

Víc se uč:

- [Ochrana Identity Azure Active Directory](../active-directory/active-directory-identityprotection.md)
- [Kanál 9: Azure AD a Identity zobrazit: náhled ochranu Identity](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Centrum zabezpečení

Centrum zabezpečení Azure pomáhá zabránit, zjišťování a odpovědět na hrozeb a poskytuje že lepší přehled o a publikum nemůže ovládat, zabezpečení Azure zdroje. Poskytuje integrované zabezpečení sledování a zásady správy přes Azure předplatných, pomáhají zjistit hrozeb, které může jinak všimnout a spolupracuje Rozsáhlá platforma řešení zabezpečení.

Centrum zabezpečení vám pomůže optimalizovat a sledovat zabezpečení Azure prostředků tak, že:

- Umožňuje definovat zásady pro vaše předplatné Azure zdroje podle potřeb vaší společnosti zabezpečení a typ aplikace nebo citlivosti dat do každého předplatného.
- Sledování stavu Azure virtuálních počítačích, sítě a aplikací.
- Poskytování seznam upozornění zabezpečení uspořádaný, včetně upozornění od integrované partnerských řešení, spolu s informací, které potřebujete rychle prozkoumat a doporučení ohledně toho, jak nápravě útok.

Víc se uč:

- [Úvod k Centru zabezpečení Azure](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
