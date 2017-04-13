<properties
   pageTitle="Povolení auditování na serverech SQL v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat **Povolit auditování na serverech SQL**Azure Centrum zabezpečení doporučení."
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

# <a name="enable-auditing-on-sql-servers-in-azure-security-center"></a>Povolení auditování na serverech SQL v Centru zabezpečení Azure

Centrum zabezpečení Azure doporučí, zapněte auditování pro všechny databáze na serverech Azure SQL, pokud auditování již není povolené. Sestavy auditování můžete udržovat dodržování předpisů, pochopit aktivita v databázi a získat přehled o nesrovnalosti a odchylky, které znamenat firmy pochybnosti nebo podezřelé porušení zabezpečení.

Jakmile jste zapnuli auditování můžete nakonfigurovat nastavení hrozbou, že vyhledávání a e-mailů výstrahy zabezpečení. Zjišťování hrozbou, že rozpozná aktivity neobvyklých databáze označující potenciální hrozeb zabezpečení databáze. To umožňuje zjišťování zpráv a odpovídání na potenciální hrozeb při jejich vzniku.

Toto doporučení platí pro službu Azure SQL. neobsahuje SQL Server na virtuálních počítačích infrastruktury služby Azure (Azure IaaS).

> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad.  Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. **Doporučení** zásuvné vyberte **Povolit auditování na serverech SQL**.  Otevře se zásuvné **Povolit auditování na serverech SQL** .
![Povolení auditování na serverech SQL][1]

2. Výběrem server SQL povolte auditování. Otevře se zásuvné **Nastavení auditování** .
![Nastavení auditování][2]
3. Na zásuvné **Nastavení auditování** vyberte **zapnuto, pokud** v části **auditování**.
![Zapnutí nastavení auditování][3]

4. Postupujte podle pokynů v tématu [Začínáme s SQL databáze auditování](../sql-database/sql-database-auditing-get-started.md) nakonfigurovat úložiště uložení protokolů auditování. Účet předplatného úložiště pro shromažďování dat je výchozí účet úložiště.

5. Postupujte podle pokynů v tématu [Začínáme s SQL databáze hrozbou, že zjišťování](../sql-database/sql-database-threat-detection-get-started.md) zapnout a konfigurace zjišťování hrozbou, že a konfiguraci seznamu e-mailů, které se zobrazí upozornění zabezpečení při zjišťování neobvyklých činností.

## <a name="see-also"></a>Viz taky

Tento článek ukázal, jak implementovat Centrum zabezpečení doporučení "Povolte auditování na serverech SQL." Další informace o zabezpečení databáze SQL, najdete v těchto článcích:

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
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]:./media/security-center-enable-auditing-on-sql-server/enable-auditing.png
[3]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
