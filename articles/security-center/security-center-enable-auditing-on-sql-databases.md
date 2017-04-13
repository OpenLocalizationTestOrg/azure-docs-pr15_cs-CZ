<properties
   pageTitle="Povolte auditování SQL databáze v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat **povolte auditování databáze SQL**Azure Centrum zabezpečení doporučení."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-auditing-on-sql-databases-in-azure-security-center"></a>Povolte auditování SQL databáze v Centru zabezpečení Azure

Centrum zabezpečení Azure doporučí, zapněte auditování pro všechny databáze SQL Pokud auditování již není povolené. Sestavy auditování můžete udržovat dodržování předpisů, pochopit aktivita v databázi a získat přehled o nesrovnalosti a odchylky, které znamenat firmy pochybnosti nebo podezřelé porušení zabezpečení.

Jakmile jste zapnuli auditování můžete nakonfigurovat nastavení hrozbou, že vyhledávání a e-mailů výstrahy zabezpečení. Zjišťování hrozbou, že rozpozná aktivity neobvyklých databáze označující potenciální hrozeb zabezpečení databáze. To umožňuje zjišťování zpráv a odpovídání na potenciální hrozeb při jejich vzniku.

Toto doporučení platí pro službu Azure SQL. neobsahuje SQL na virtuálních počítačích.

> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad.  Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. **Doporučení** zásuvné vyberte **Povolit auditování na SQL databáze**.  Otevře se zásuvné **Povolit auditování na SQL databáze** .
![Povolte auditování SQL databáze][1]

2. Vyberte databázi SQL povolit auditování. Otevře se zásuvné **auditování & hrozbou, že zjišťování** .
![Sestavy auditování a hrozeb zjišťování][2]
3. Na zásuvné **auditování & hrozbou, že zjišťování** vyberte **zapnuto, pokud** v části **auditování**.
![Zapnutí auditování a hrozeb zjišťování][3]


5. Postupujte podle pokynů v tématu [Začínáme s SQL databáze hrozbou, že zjišťování](../sql-database/sql-database-threat-detection-get-started.md) zapnout a konfigurace zjišťování hrozbou, že a konfiguraci seznamu e-mailů, které se zobrazí upozornění zabezpečení při zjišťování neobvyklých činností.

## <a name="see-also"></a>Viz taky

Tento článek ukázal, jak implementovat Centrum zabezpečení doporučení "Povolte auditování SQL databáze." Další informace o zabezpečení databáze SQL, najdete v těchto článcích:

- [Zabezpečení databáze SQL](../sql-database/sql-database-security.md)

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa zabezpečení doporučení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování stavu zabezpečení v Centru zabezpečení Azure](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
- [Zabezpečení azure blog](http://blogs.msdn.com/b/azuresecurity/) – získání nejnovější informace Azure zabezpečení a informací.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]:./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
