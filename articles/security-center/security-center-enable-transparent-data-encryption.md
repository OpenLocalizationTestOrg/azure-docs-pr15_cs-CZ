<properties
   pageTitle="Povolit šifrování průhledné dat v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **Povolit průhledný šifrování**."
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

# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Povolit šifrování průhledné dat v Centru zabezpečení Azure

Centrum zabezpečení Azure doporučí, pokud již není povolené TDE povolit šifrování průhledné dat (TDE) na SQL databáze. TDE chrání data a pomůže vám požadavkům dodržování předpisů pomocí šifrování databáze, přidružené zálohování a souborů protokolu transakce v klidu, aniž by bylo změn v aplikaci. Další informace najdete v článku [Průhledné šifrování databáze SQL Azure](https://msdn.microsoft.com/library/dn948096).

Toto doporučení platí pro službu Azure SQL. neobsahuje SQL na virtuálních počítačích.

> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad.  Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. V zásuvné **doporučení** vyberte **Povolit průhledné šifrování**.
![Povolit šifrování průhledné dat][1]

2. Otevře se zásuvné **Povolit průhledné šifrování dat v SQL databáze** . Vyberte databázi SQL povolíte TDE u.
![Vyberte SQL databáze povolíte TDE u][2]
3. Na zásuvné **šifrování průhledné dat** vyberte **zapnuto, pokud** v části šifrování a vyberte **Uložit** na horním pásu karet zásuvné.
![Zapnutí TDE][3]

  Po povolení TDE na vybrané databáze SQL **stavu šifrování** se změní na **šifrovaný**.    

  ![Stav šifrování][4]

## <a name="see-also"></a>Viz taky

Tento článek ukázal, jak implementovat Centrum zabezpečení doporučení "Povolit šifrování průhledné." Další informace o SQL TDE, najdete v těchto článcích:

- [Šifrování průhledné dat s databáze Azure SQL](https://msdn.microsoft.com/library/dn948096)
- [Začínáme s šifrování průhledné dat (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa zabezpečení doporučení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování stavu zabezpečení v Centru zabezpečení Azure](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
- [Zabezpečení azure blog](http://blogs.msdn.com/b/azuresecurity/) – získání nejnovější informace Azure zabezpečení a informací.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
