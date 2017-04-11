<properties
   pageTitle="Porovnání funkcí pro správu externího identit pomocí služby Azure Active Directory | Microsoft Azure"
   description="Srovnání Azure Active Directory B2B spolupráce, B2C a více klienta aplikace pro podporu a tak mohli ověřovat pro externí identity"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Porovnání funkcí pro správu externího identit pomocí služby Azure Active Directory

Kromě Správa zaměstnanců mobilní přístup k aplikacím SaaS Azure Active Directory (Azure AD) pomáhá organizaci sdílet zdroje s obchodními partnery a předvádění aplikace pro firmy a uživatelé.

## <a name="developing-applications-for-businesses"></a>Vývoj aplikací pro firmy

Poskytuje služby nebo aplikace, například služby mezd pro firmy? Azure AD poskytuje platformu identity, která umožňuje vytvářet aplikace, které Bezproblémová integrace s miliony organizacím, které jste už nakonfigurovali Azure AD jako součást nasazení Office 365 nebo jiné služby enterprise.

**Příklad:** Pharmaceutical distributor poskytuje lékařích dodávky a informace systémy zdravotnictví. Potřebovali pole analýzy aplikace lékařích postupy a požadovaného zákazníkům spravovat svoje vlastní identity. Tato společnost rozhodli Azure AD jako platformu identity pro správu jejich praktická část v aplikaci poskytnout podpoře Azure AD identit svým zákazníkům zavináč nahoru v případě potřeby. Další informace najdete v příručce [pro vývojáře služby Azure Active Directory](active-directory-developers-guide.md).

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Povolení firmy přístupem partnerů k podnikové prostředky

Máte k dispozici obchodní partnery nebo ostatní uživatelé mimo vaši organizaci, kteří potřebují dostat k zdrojích organizace, například Sharepointového webu nebo systému ERP? Azure AD umožňuje správcům udělit, že externí uživatele (může nebo nemusí existovat v Azure AD) jednotné přihlašování pro přístup k podnikové aplikace. To zvyšuje zabezpečení, jak uživatelé ztratíte přístup, když opustí organizaci partnera, zatímco ovládáte zásady přístupu ve vaší organizaci. To také zjednodušuje správu nemusíte spravovat externí partnera adresáře nebo za partnera federace relace.

**Příklad:** Pro zpracování obrázků společnost poskytuje prodejců pro zpracování obrázků služeb a provozuje největší maloobchodní síť tisku místa. Budou se rozhodnout, Azure AD povolit tisíce uživatelů na jejich maloobchodní obchodní partnery používat svoje přihlašovací údaje k stáhnout nejnovější partnera marketingových materiálů a uspořádat fotky zpracování dodávek od společnosti dodavatele extranetové. Další informace najdete v tématu [spolupráce Azure AD B2B](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Vývoj aplikací pro uživatele

Potřebujete bezpečně a s nižšími náklady publikujete online aplikací, například popředí úložiště maloobchodní miliony spotřebitele? Azure AD poskytuje platformu povolení sociální a uživatelského jména a hesla přihlásit, firemních samoobslužnou registraci a samoobslužné resetování hesla pro uživatele aplikace. Této kombinace kláves rozšíří usnadnění pro uživatele při snížení zatížení své vývojové.

**Příklad:** \#1 sportovní franšíza na světě potřeby přímo zapojit s jeho 450 milionů ventilátory. K tomuto účelu jsou vytvořené v mobilní aplikaci pomocí Azure AD pro ukládání ověřování a profilu uživatele. Získání zjednodušené registraci a přihlášení pomocí účty sociálních třeba Facebook nebo můžou použít tradiční uživatelského jména a hesla pro bezproblémovou práci různých iOS a Android a Windows telefony. Budovy založení platformu Azure AD značně snížit vlastního kódu během pojmenování franšíza přizpůsobené branding a boj pochybnosti týkající se zabezpečení, porušení dat a škálovatelnost. Další informace najdete v tématu [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).

## <a name="comparison-of-azure-ad-capabilities"></a>Porovnání funkcí Azure AD

Každá scénáře už popisované v tomto článku je je určena funkcí v Azure AD. Tato tabulka by měla nahlédnutí, které funkce jsou pro vás nejdůležitější:

| **Zvažte tento produkt …**       | [Azure AD více klienta SaaS aplikace](active-directory-developers-guide.md)    | [Azure AD B2B spolupráce](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **Pokud třeba zadat...** | Služba pro firmy | přístupem partnerů k Moje aplikace  | služby zákazníkům |
| **A jsem podobný …**  | Pharma distributor      | Pro zpracování obrázků společnosti            | Sportovní franšíza       |
| **Nasazení aplikace pro...**  | Správa praktické cvičení     | Dodavatel extranet          | Fotbalový ventilátory            |
| **Nasměrování …**        | Lékaře kanceláří        | Schválené obchodní partnery | Každý, kdo má e-mailu      |
| **Dostupné v případě …**      | Souhlasí správu zákazníků | Můj správce pozvánky           | Zaregistruje příjemce      |
