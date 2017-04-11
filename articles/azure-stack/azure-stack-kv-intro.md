<properties
    pageTitle="Azure Úvod zásobníku klíč trezoru | Microsoft Azure"
    description="Zjistěte, jak Azure zásobníku klíč trezoru spravuje klíče a tajemství"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Úvod do klíčové trezoru v Azure zásobníku #

## <a name="before-you-start"></a>Než začnete

V tomto článku se předpokládá takto:

- Nástroj pro čtení obsahu mají přístup k předplatnému, který má povolené trezoru klíč Azure.
- Prostředí PowerShell SDK Azure je nakonfigurované a k dispozici.
- Verzi TP2 všechny klienta přístupným operace může dělat jenom z prostředí PowerShell.

## <a name="key-vault-basics"></a>Základní informace o trezoru klíč

Klíč trezoru ve vrstvě Azure pomáhá chránit klíče a použijte tajemství, které cloudu aplikací a služeb. Pomocí klávesy trezoru dá zašifrovat klíče a tajemství (například kláves ověřování, úložiště účet kláves, data šifrovacího klíče, .pfx soubory a hesla).

Klíč trezoru zjednodušuje proces správy klíčů a umožňuje spravovat ovládací prvek zkratek, které se přístup a šifrování vaše data. Vývojáři mohli vytvářet klávesové zkratky pro vývoj a testování v minutách a potom Bezproblémová jejich migraci výrobní klíče. Správci zabezpečení můžete udělit (a odvolání) oprávnění k klávesy, podle potřeby.

Kdokoli s předplatným Azure zásobníku vytváření a používání klíčových trezorů. I když klíč trezoru výhody vývojáři a správci zabezpečení, můžete implementovaná a spravuje správce, který má na starosti další služby Azure zásobníku pro organizaci. Například pro tohoto uživatele přihlásit pomocí předplatné Azure zásobníku vytvořte trezoru pro organizaci, ve kterém chcete ukládat klíčů a potom zodpovědný za těchto provozní úkolů:

- Možnost vytvořte nebo importujte klíče nebo tajná
- Odvolání nebo odstranění klíče nebo tajná
- Povolit uživatelům nebo aplikace pro přístup k klíčové trezoru, abyste mohli potom spravovat a jeho použít a pomocí kláves tajemství
- Konfigurace použití klíče (třeba podepsat nebo šifrování)

Tento správce můžete vývojáři poskytnout URI zavolat z jejich aplikací a poskytnout správce zabezpečení informací o protokolování použití klíče.

Vývojáři můžou taky správu klíčů přímo, pomocí rozhraní API. Další informace najdete v příručce vývojář klíč trezoru.

## <a name="scenarios"></a>Scénáře

Následující tabulka popisuje některé scénáře, kde klíč trezoru pomůže potřebám vývojářů a správci zabezpečení:


### <a name="developer-for-an-azure-stack-application"></a>Vývojář aplikace Azure zásobníku

**Problém**: Chci vytvořit aplikaci pro zásobníku Azure, který používá klíče pro podpis a šifrování, ale chci tyto jako externí z aplikace tak, aby řešením je vhodné pro aplikace geograficky distribuovaný.

**Příkaz**: klávesy jsou uložené v trezoru a vyvolat URI v případě potřeby.


### <a name="developer-for-software-as-a-service-saas"></a>Karta Vývojář softwaru jako služba (SaaS)

**Problém:** Nechci, aby zodpovědnost nebo potenciální odpovědnost Moje zákazníci klienta klíče a tajemství.

**Údajů:** Zákazníci můžete importovat vlastní klíče do zásobníku Azure a spravovat. Chci zákazníkům vlastníte a spravovat legendou tak, aby se můžete Soustřeďte se na udělat, co mám dělat nejlépe, které základní poskytuje funkce softwaru.


### <a name="chief-security-officer-cso"></a>Hlavní bezpečnostní úředník (CSO)

**Problém:** Chci, abyste měli jistotu, že organizaci pod kontrolou klíčové životní cyklus a můžete sledovat použití klíče.

**Příkaz** Klíč trezoru je navržen tak, že Microsoft najdete v článku nebo extrahovat klíče.  Pokud aplikace potřebuje k provádění operací, cryptographic pomocí kláves zákazníků, klíč trezoru to dělá jménem aplikace. Aplikace nezobrazuje zákazníků klíče.  I když používáme více služby Azure zásobníku a zdroje, budu chtít správu klíčů z jednoho místa ve vrstvě Azure. Trezoru poskytuje jediné rozhraní, bez ohledu na to, kolik trezorů máte ve vrstvě Azure, oblasti, které budou podporu a aplikace, které používají.

## <a name="next-steps"></a>Další kroky

[Začínáme s klíčové trezoru](azure-stack-kv-getting-started.md)
