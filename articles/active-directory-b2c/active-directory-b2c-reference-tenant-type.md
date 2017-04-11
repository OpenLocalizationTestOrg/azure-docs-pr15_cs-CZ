<properties
    pageTitle="Azure Active Directory B2C: Výrobní měřítko porovnání náhled B2C klientů | Microsoft Azure"
    description="Téma typy Azure Active Directory B2C klientů"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure Active Directory B2C: Výrobní měřítko porovnání náhled B2C klientů

Pokud se chystáte psát výrobní aplikace na B2C Azure Active Directory (Azure AD), musíte mít jistotu, že máte správné klienta "typ", přejděte na živá. Chcete-li zjistit, jaká máte, tyto kroky přejděte [na zásuvné funkce B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) na portálu Azure a ve skupinovém rámečku **Typ klienta**.

## <a name="summary"></a>Souhrn

Azure AD B2C podporuje pouze na **výrobní měřítko** B2C klientů v Severní Americe výrobní aplikace.

| Typu klienta | Země/oblasti | Obecně dostupných? |
| ----------- | -------------- | --------------------- |
| **Výrobní měřítko klienta** | Severní Americe země/oblasti | Ano |
| **Výrobní měřítko klienta** | Země kromě Severní Ameriku | Ne |
| **Náhled klienta** | Obsahují země/oblasti | Ne |

> [AZURE.NOTE]
Azure AD B2C klienti (pro příjemcům) jsou momentálně není k dispozici v několika zemí nebo oblastí, kde jsou k dispozici Azure AD klienti (pro zaměstnance). Přečtěte si další informace v následujících částech.

## <a name="production-scale-b2c-tenant-in-north-america"></a>Výrobní měřítko B2C klienta v Severní Americe

Pokud jste [vytvořili B2C klienta](active-directory-b2c-get-started.md) v Severní Americe, tedy v některém z následujících zemí nebo oblastí: Spojených států, Kanady, Kostarika, Dominikánská republika, l Salvador, Guatemala, Mexiko, Panama, Portoriko a Trinidad a Tobago a **Typ klienta** na B2C Admin UI říká **výrobní měřítko**, vašeho klienta mohou sloužit k aplikacím výroby.

> [AZURE.NOTE]
Nastavení velikosti 100s milionů spotř identit jednoho klienta mohou klienti výrobní měřítko.

![Snímek obrazovky s výrobní měřítko klienta](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>Zobrazení náhledu B2C klienta v jakékoli zemi/oblasti

Pokud jste vytvořili B2C klienta Azure AD B2C náhled období, je pravděpodobné, že vaše **klienta zadejte** říká **Náhled klienta**. Pokud jde o případ, je nutné použít vašeho klienta pouze pro účely vývoje a testování a ne pro výrobní aplikace.

> [AZURE.IMPORTANT]
Neexistuje žádné cestu migrace z náhledu B2C klienta do výrobní měřítko B2C klienta. Všimněte si, že jsou známé problémy při odstranění náhled B2C klienta a opětovné vytvoření klienta B2C výrobní měřítko se stejným názvem domény. Budete muset vytvoření klienta B2C výrobní měřítko s jiným názvem domény.

![Snímek obrazovky s náhled klienta](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>Výrobní měřítko B2C klienta mimo Severní Ameriku

Azure AD B2C není momentálně obecně dostupných mimo Severní Ameriku. Ale můžete vytvářet a používat klienti výrobní měřítko pro vývoj a testování účely v jednom z těchto věcí zemí nebo oblastí: Alžírsko, Rakousko, Ázerbájdžán, Bahrajn, Bělorusko, Belgie, Bulharsko, Chorvatsko, Kypr, Česká republika, Dánsko, Egypt, Estonsko, Finsko, Francie, Německo, Řecko, Maďarsko, Island, Irsko, Izrael, Itálie, Jordánsko, Kazachstán, Keni, Kuvajt, Lotyšska, Libanon, Lichtenštejnsko, Lituania, Lucembursko, Makedonie – bývalá republika Jugoslávie, Malta, Černá Hora, Maroko, Nizozemsko, Nigérie, Norsko , Omán, Pákistán, Polsko, Portugalsko, Katar, Rumunsko, ruština, Saúdská Arábie, Srbsko, Slovensko, Slovinsko, Jižní Afrika, Španělsko, Švédsko, Švýcarsko, Tunisko, Turecko, Ukrajina, Spojené arabské emiráty a Spojené království.

Jakmile Azure AD B2C oznamuje všeobecně dostupná v nad zemí nebo oblastí, můžete dál používat tyto klienti výrobní měřítko a přejít live s aplikací výrobní bez ztráty dat.

## <a name="availability-of-b2c-tenants"></a>Dostupnost B2C klientů

Klienti B2C jsou momentálně není k dispozici v následujících zemí nebo oblastí: Afghánistán, Argentina, Austrálie, Brazílie, Chile, Kolumbie, Ekvádor, Hong Kong SAR, Indie, Indonésie, Irák, Japonsko, Korea, Malajsie, Nový Zéland, Paraguay, Peru, Filipíny, Singapur, Srílanská, Tchaj-wan, Thailand (ไทย), Uruguay a Venezuela. Plánujeme zahrnutí v budoucnu.
