<properties
   pageTitle="Ochrana služby Azure SQL v Centru zabezpečení Azure | Microsoft Azure"
   description="Tento dokument adresy doporučení v Centru zabezpečení Azure, které pomáhají chránit služby Azure SQL a ponechání souladu s zásady zabezpečení."
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
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-azure-sql-service-in-azure-security-center"></a>Ochrana služby Azure SQL v Centru zabezpečení Azure

Centrum zabezpečení Azure analyzuje stav zabezpečení Azure prostředků. Když Centrum zabezpečení identifikuje potenciální zabezpečením, vytvoří doporučení, které by vás provede procesem konfigurace potřebných ovládacích prvků.  Doporučení použít u typů Azure zdroje: virtuálních počítačích (VMs), připojení k síti SQL a aplikace.

Tento článek se zaměřuje na doporučení, která platí pro službu Azure SQL.  Centrum doporučení služby Azure SQL okolo povolení auditování Azure SQL servery a databází a povolení šifrování databáze SQL.  Použijte v následující tabulce jako odkaz na vám pomůže pochopit dostupné doporučení služby SQL a co každý z nich bude dělat, když ho použijete.

## <a name="available-sql-service-recommendations"></a>K dispozici doporučení služby SQL

|Doporučení|Popis|
|-----|-----|
|[Povolení auditování SQL serveru](security-center-enable-auditing-on-sql-servers.md)|Doporučuje, zapněte auditování Azure SQL servery (služba Azure SQL pouze; nezahrnuje SQL na virtuálních počítačích).|
|[Povolení databáze SQL auditování](security-center-enable-auditing-on-sql-databases.md)|Doporučuje, zapněte auditování pro databáze Azure SQL (služba Azure SQL pouze; nezahrnuje SQL na virtuálních počítačích).|
|[Povolit průhledné šifrování dat v SQL databáze](security-center-enable-transparent-data-encryption.md)|Doporučuje povolit šifrování pro SQL databáze (jenom služba Azure SQL).|

## <a name="see-also"></a>Viz taky

Další informace o doporučení, které se vztahují na jiné typy Azure zdroje, najdete v těchto článcích:

- [Ochrana virtuálních počítačích v Centru zabezpečení Azure](security-center-virtual-machine-recommendations.md)
- [Ochrana vašich aplikací v Centru zabezpečení Azure](security-center-application-recommendations.md)
- [Ochrana sítě v Centru zabezpečení Azure](security-center-network-recommendations.md)

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
