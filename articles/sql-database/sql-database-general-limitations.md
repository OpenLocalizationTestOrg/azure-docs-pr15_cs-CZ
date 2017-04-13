<properties
   pageTitle="Omezení obecné databáze Azure SQL a pokyny"
   description="Tato stránka popisuje některé obecné omezení pro databázi SQL Azure, jakož i oblasti spolupráce a podporu."
   services="sql-database"
   documentationCenter="na"
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/06/2016"
   ms.author="carlrab" />

# <a name="azure-sql-database-general-limitations-and-guidelines"></a>Omezení obecné databáze Azure SQL a pokyny

Toto téma obsahuje obecné omezení a pokyny pro databázi SQL Azure. Úplné pochopení kvóty řízení zdrojů a podporu, najdete [Další zdroje informací](#additional-guidelines) na konci tohoto tématu.

## <a name="connectivity-and-authentication"></a>Připojení a ověřováním

  - Ověřování systému Windows není podporovaná. V tématu [Správa databáze a přihlášení v databázi Azure SQL](sql-database-manage-logins.md). Ověřování služby Azure Active Directory jsou však podporovány pro určitá omezení. Najdete v článku [připojení k databázi SQL Azure Active Directory ověřování](sql-database-aad-authentication.md).

  - Databázi Microsoft Azure SQL podporuje tabulková data toku (Neuvedeno) protokol verzi klienta 7.3 nebo novější.

  - Jsou povoleny pouze TCP/IP připojení.

  - SQL Server 2008 SQL Server prohlížeč není podporovaný, protože databázi Microsoft Azure SQL, které neobsahují dynamické porty pouze port 1433.

## <a name="sql-server-agentjobs"></a>Úlohy Agent serveru SQL

Databázi Microsoft Azure SQL podporuje SQL Server Agent však použijete pružná úlohy ke spuštění úlohy přes jedna ku mnoha databází. Další informace o pružná úlohy najdete v článku [pružná úlohy](sql-database-elastic-jobs-overview.md).

## <a name="sql-server-collation-support"></a>Podpora řazení serveru SQL Server

Výchozí řazení databázi používá databázi Microsoft Azure SQL je **SQL_LATIN1_GENERAL_CP1_CI_AS**, kde **LATIN1_GENERAL** je angličtina (Spojené státy), **CP1** je znaková stránka 1252, **odvětví** je velká a malá písmena, a **jak** diakritikou. Není možné změnit řazení V12 databází. Další informace o tom, jak nastavit řazení najdete v tématu [KOMPLETOVAT (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="naming-requirements"></a>Požadavky na pojmenování

Některé uživatelská jména nejsou povolené z bezpečnostních důvodů. Není možné použít tyto názvy:

 - **Správce**
 - **Správce**
 - **Host**
 - **kořenové**
 - **Správce systému**

Názvy pro všechny nové objekty musí splňovat pravidla serveru SQL Server pro identifikátory. Další informace najdete v tématu [identifikátory](https://msdn.microsoft.com/library/ms175874.aspx).

Kromě toho nesmí obsahovat přihlášení a uživatelského jména \ znak (ověřování systému Windows není podporována).

## <a name="additional-guidelines"></a>Další pokyny

- Kromě obecné omezení uvedené v tomto článku má databáze SQL kvóty určitému zdroji a omezení podle **vrstvy služeb**. Základní informace ze služby úrovní najdete v článku [– úrovně služeb SQL databáze](sql-database-service-tiers.md).

- Ostatní limity SQL databáze v tématu [Limity prostředků databáze SQL Azure](sql-database-resource-limits.md).

- Zabezpečení související pokyny najdete v článku [Pokyny zabezpečení databáze SQL Azure](sql-database-security-guidelines.md).

- Jiné související oblasti kolem kompatibility s databáze SQL Azure s místní verzí systému SQL Server, třeba SQL Server 2014 a SQL serveru 2016. Nejnovější verze V12 databáze SQL Azure připíše spoustu vylepšení v této oblasti. Další informace najdete v tématu [Co je nového v SQL databázi V12](sql-database-v12-whats-new.md).

- Informace o dostupnosti ovladače a podpora pro databáze SQL najdete v tématu [Knihovny připojení databáze SQL a SQL Server](sql-database-libraries.md).
