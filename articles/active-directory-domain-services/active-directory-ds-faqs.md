<properties
    pageTitle="Časté otázky – Azure Active Directory Domain Services | Microsoft Azure"
    description="Nejčastější dotazy k Azure Active Directory Domain Services"
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
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: Nejčastější dotazy

Na této stránce najdete odpovědi na nejčastější dotazy týkající se Azure Active Directory Domain Services. Zachování kontroly další aktualizace.

### <a name="troubleshooting-guide"></a>Příručka pro řešení potíží
V nápovědě k naše [Poradce při potížích Průvodce](active-directory-ds-troubleshooting.md) pro řešení běžných problémů při konfiguraci a správě Azure AD Domain Services.


### <a name="configuration"></a>Konfigurace

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Můžete vytvořit víc domén pro jeden Azure AD adresáře?
Ne. Můžete vytvořit jenom jednu doménu vyřízení služby Azure AD Domain pro jeden Azure AD adresář.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Můžete povolit Azure AD Domain Services v síti virtuální správce prostředků Azure?
Ne. Azure AD Domain Services můžete povolit pouze v síti klasické Azure virtuální. Klasický virtuální sítě můžete připojit k správce prostředků virtuální sítě pomocí virtuální sítě prozkoumávání aby používala vaši doménu spravovaný v síti virtuální správce prostředků.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Můžu někomu Azure AD Domain Services k dispozici ve více virtuální sítí v rámci předplatného?
Služba sama přímo tento scénář nepodporují. Azure AD Domain Services je k dispozici v jedinou virtuální sítě po druhém. Však můžete nakonfigurovat propojení mezi více virtuální sítí vystavit služby Azure AD Domain pro jiné virtuální sítě. Tento článek popisuje, jak se dá [připojit virtuální sítě Azure](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Můžete povolit služby Azure AD domény pomocí prostředí PowerShell?
Prostředí PowerShell/automatizované nasazení služby Azure AD domény momentálně není dostupná.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Je k dispozici v novém portálu Azure Azure AD Domain Services?
Ne. Pouze v [Azure klasické portál](https://manage.windowsazure.com)je možné konfigurovat Azure AD Domain Services. Jsme očekávají, že v budoucnu rozšíření podpora [Azure portálu](https://portal.azure.com) .

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Můžete přidat řadiče domény spravovaných doménu Azure AD Domain Services?
Ne. Spravované doménu je doména poskytovanou Azure AD Domain Services. Není potřeba zřízení, konfigurace nebo jinak spravovat řadiče domény pro tuto doménu – tyto činnosti správy jsou k dispozici jako služba společnosti Microsoft. Proto nemůžete přidat další řadiče domény (pro čtení i zápis nebo jen pro čtení) u spravovaných domény.

### <a name="administration-and-operations"></a>Správa a provoz

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Můžete se připojit k řadiče domény pro svoji doménu spravovaný pomocí vzdálené plochy?
Ne. Nemáte oprávnění k připojení k řadiče domény pro doménu spravovaný pomocí vzdálené plochy. Členové skupiny "AAD Datacentrum správci" můžete spravovat spravovaná domény pomocí nástroje pro správu AD například Centrum správy Active Directory (ADAC) nebo AD Powershellu. Tyto nástroje nainstalovaných pomocí funkce "Nástroje pro správu vzdálený Server" v systému Windows server připojen k spravované domény.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Můžu jste povolili Azure AD Domain Services. Jaký účet se používá do počítače spojení domény pro tuto doménu?
Uživatele, které jste přidali do skupiny pro správu (například "AAD Datacentrum správci") můžete připojení k doméně počítačích. Uživatelé v této skupině navíc měli přístup ke vzdálené ploše pro počítače, kteří se připojili k doméně.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>Můžete používejte oprávnění správce domény u domény poskytovanou Azure AD Domain Services?
Ne. Nejsou udělena administrativním oprávněním pro spravované domény. Oprávnění pro doménu správce a podnikový správce nejsou k dispozici pro použití v rámci domény. Existující správce domény nebo podnikový správce skupiny v adresáři Azure AD nejsou také poskytuje oprávnění správce domény nebo enterprise pro tuto doménu.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>Můžete změnit členství ve skupinách pomocí LDAP nebo další nástroje pro správu AD na domény poskytovanou Azure AD Domain Services?
Ne. Členství ve skupinách nelze upravit, na vyřízení tak, že Azure AD Domain Services domény. Totéž platí pro atributy uživatele. Můžete ale změnit členství ve skupinách nebo atributy uživatele v Azure AD nebo ve vaší místní doméně. Tyto změny se automaticky synchronizují Azure AD Domain Services.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>Můžete rozšířit schématu domény poskytovanou Azure AD Domain Services?
Ne. Schéma spravuje Microsoft spravované domény. Rozšíření schématu nepodporuje Azure AD Domain Services.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Je možné upravit nebo přidat záznamy DNS v spravovaných doménu?
Ano. Uživatelé, kteří patří do skupiny "AAD Datacentrum správci" mít příslušná oprávnění DNS správce, aby se upravit záznamy DNS v seznamu spravované doména. Těmto uživatelům umožňuje konzole DNS správce v počítači spuštěný Windows Server připojen k spravované domény spravovat DNS. Použití konzoly Správce DNS, nainstalujte "Nástroje serveru DNS", která je součástí "Nástroje pro správu vzdálený Server" volitelná funkce na serveru. Další informace o [Nástroje pro správu, sledování a odstraňování potíží DNS](https://technet.microsoft.com/library/cc753579.aspx) je k dispozici na TechNetu.


### <a name="billing-and-availability"></a>Fakturace a dostupnosti

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Je že Domain Azure AD služeb placené služba?
Ano. Další informace najdete v tématu [ceny stránky](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-the-service"></a>Existuje bezplatnou zkušební verzi pro službu?
Tato služba je součástí bezplatnou zkušební verzi Azure. Můžete zaregistrovat [bezplatnou zkušební verzi Azure jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>Můžete získat Azure AD Domain Services jako součást aplikace Enterprise mobilita sady (EMS)?
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Je potřeba Azure AD Premium použití služby Azure AD doménu?
Ne. Azure AD Domain Services je přislíbený Azure služby a není součástí EMS. Azure AD Domain Services jsou dostupné pro všechny edice Azure AD (bezplatnou základní a, Premium) a fakturované za hodinu, v závislosti na použití.

#### <a name="what-azure-regions-is-the-service-available-in"></a>K čemu Azure oblastí neexistuje službu v?
Podívejte se na stránku [Služby Azure podle regionů](https://azure.microsoft.com/regions/#services/) zobrazíte seznam Azure oblastí, kde je k dispozici Azure AD Domain Services.
