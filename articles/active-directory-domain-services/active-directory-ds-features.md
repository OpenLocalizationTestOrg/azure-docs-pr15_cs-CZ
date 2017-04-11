<properties
    pageTitle="Azure Active Directory Domain Services: Funkce | Microsoft Azure"
    description="Funkce služby Azure Active Directory Domain Services"
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
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure AD Domain Services

## <a name="features"></a>Funkce
Tyto funkce jsou dostupné v Azure AD Domain Services spravovat domény.

- **Jednoduché nasazení prostředí:** Můžete povolit služby Azure AD domén pro vašeho klienta Azure AD pomocí několika kliknutí. Bez ohledu na to, zda je ke cloudové klientovi vašeho klienta Azure AD nebo nesynchronizuje s místního adresáře můžete rychle zřízení spravované domény.

- **Podpory pro doménu spojení:** Můžete snadno počítačů připojení k doméně v Azure virtuální sítí, ke které spravované domény je dostupný v aplikaci. Připojení k doméně prostředí Windows klienta a serveru operační systémy funguje Bezproblémová proti domény vyřízení tak, že Azure AD Domain Services. Můžete také automatické domény spojení nástroje proti tyto domény.

- **Jednu doménu instance za Azure AD directory:** Můžete vytvořit jednu domény Active Directory pro každý Azure AD adresář.

- **Vytvořit vlastní názvy domén:** Můžete vytvořit domény s vlastní názvy (například "contoso100.com") pomocí Azure AD Domain Services. Můžete použít buď ověřenou nebo neověřený názvů domén. V případě potřeby můžete taky vytvořit doménu s příponou předdefinované domény (to znamená "*. onmicrosoft.com") nabízená Azure AD adresáře.

- **Integrovaný s Azure AD:** Konfigurace nebo spravovat replikace Azure AD Domain Services nepotřebujete. Uživatelské účty a členství ve skupinách, přihlašovací údaje uživatele pro (heslo) z adresáře Azure AD jsou automaticky součástí Azure AD Domain Services. Noví uživatelé, skupiny nebo změn atributů z místního adresáře nebo vašeho klienta Azure AD se automaticky synchronizují Azure AD Domain Services.

- **NTLM a Kerberos ověření:** S podporou NTLM a Kerberos ověřování nástroje můžete nasazovat aplikace, které jsou závislé na ověřování systému Windows.

- **Pomocí podnikové přihlašovací údaje a hesla:** Hesla pro uživatele ve vašem klientovi Azure AD pracovat s Azure AD Domain Services. Uživatele můžete použít svoje podnikové přihlašovací údaje pro připojení k doméně počítačích, přihlásit interaktivně nebo prostřednictvím vzdálené plochy a ověřování spravované domény.

- **Vazbu protokolu LDAP & LDAP přečíst podpory:** Můžete použít aplikace, které jsou závislé na LDAP vazby pro ověřování uživatelů v domény vyřízení tak, že Azure AD Domain Services. Kromě toho aplikace, které využívají operace čtení LDAP atributů počítači uživatele nebo dotazu z adresáře můžete taky spolupracovat proti Azure AD Domain Services.

- **Zabezpečené LDAP (LDAPS):** Můžete povolit k adresáři přístup prostřednictvím zabezpečeného LDAP (LDAPS). Zabezpečený přístup LDAP je k dispozici v rámci virtuální sítě ve výchozím nastavení. Můžete však také volitelně povolit zabezpečený přístup LDAP přes internet.

- **Zásad skupiny:** Můžete jednoho předdefinované objekt zásad skupiny jednotlivých uživatelů a počítačů kontejnery k jejímu vynucení dodržování požadované zásady zabezpečení pro uživatelské účty a doméně počítače.

- **Spravovat DNS:** Členové skupiny "AAD Datacentrum správci" můžete spravovat DNS pro vaši doménu spravovaný pomocí známé nástroje pro správu DNS například modulu snap-in konzoly MMC správy DNS.

- **Vytvořit vlastní organizačních jednotek (OU):** Členové skupiny "AAD Datacentrum správci" můžete vytvořit vlastní organizačních jednotek ve spravovaných domény. Tito uživatelé mít příslušná oprávnění Úplné správce přes vlastní organizačních jednotek, tak, aby se můžete přidat nebo odebrat účtů služeb, počítačů, skupin atd. v rámci tyto vlastní organizačních jednotek.

- **k dispozici ve více oblastech Azure:** Najdete na stránce [služby Azure podle regionů](https://azure.microsoft.com/regions/#services/) znát Azure oblasti, ve kterých je k dispozici Azure AD Domain Services.

- **Dostupnost:** Azure AD Domain Services nabízí dostupnost pro vaši doménu. Tato funkce nabízí záruky vyšší provozu služby a odolnost k chybám. Sledování nabízí předdefinované stavu automatického remediation z selhání tak, že otáčející se nové instance nahradit selhalo instance a další služby pro vaši doménu.

- **Pomocí nástroje pro správu známé:** Známé nástroje pro správu služby Active Directory pro Windows Server například centra pro správu služby Active Directory nebo Active Directory PowerShell slouží ke správě spravované domény.
