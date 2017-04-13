<properties
    pageTitle="Co je databáze SQL? Úvod k SQL databázi | Microsoft Azure"
    description="Úvod k databázi SQL: technické podrobnosti a funkcí aplikace společnosti Microsoft relační databáze systému řízení (RDBMS) v cloudu."
    keywords="Úvod k sql, úvod k sql, co je databáze sql"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="shkurhek"/>

# <a name="what-is-sql-database-introduction-to-sql-database"></a>Co je databáze SQL? Úvod k SQL databázi

Databáze SQL je služba relační databáze v cloudu založený na Microsoft SQL Server trh úvodní mezery modulu s možnostmi kritické. Databáze SQL poskytuje předvídatelná výkonu, škálovatelnost s výpadek služeb, nepřerušený a ochrana dat – pomocí téměř nulové správy. Se můžete zaměřit na vývoj rychlé aplikací a urychlení času na trh, nikoli Správa virtuálních počítačích a infrastrukturu. Vzhledem k tomu, že je založená na modul [SQL serveru](https://msdn.microsoft.com/library/bb545450.aspx) , databázi SQL podporuje existující SQL Server nástroje, knihoven a rozhraní API, což usnadňuje přesunout a rozšíření do cloudu.

Tento článek je Úvod do databáze SQL základní koncepty a funkce související s výkonem, škálovatelnost a možnosti správy s odkazy na získání přehledu. Pokud budete chtít přejít v, můžete [vytvoření první databáze SQL](sql-database-get-started.md) nebo [Vytvoření fondu pružná databáze](sql-database-elastic-pool-create-portal.md) v minutách. Podle potřeby hlubší postupy v tomto videu 30 minut.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## <a name="adjust-performance-and-scale-without-downtime"></a>Úprava výkon a měřítko bez výpadek služeb

SQL databáze je k dispozici v Basic, Standard a Premium *– úrovně služeb*. Jednotlivé vrstvy služeb nabízí [různé úrovně výkon a funkce](sql-database-service-tiers.md) pro podporu lightweight těžké databáze úloh. Je možné vytvářet první aplikace na malé databáze pro několik peněz za měsíc, pak [změnit vrstvy služeb](sql-database-scale-up.md) ať už ručně nebo programově kdykoli podle aplikace, půjde virové světě, bez prostoj aplikace nebo zákazníky.

Pro mnoho firmy a aplikací je možné vytvořit databází a vyžádané výkon jedné databáze nahoru nebo dolů je nestačí, zejména pokud vzorce použití jsou relativně předvídatelná. Ale pokud máte vzorce neočekávané použití ji může být pevných ke správě nákladů a obchodní model.

Tento problém vyřešit [pružná fondů](sql-database-elastic-pool.md) v databázi SQL. Koncept je velmi jednoduché. Přidělovat výkonu do fondu a zaplatit byste výkon fondu spíše než jednu databázi výkonu. Nemusíte vytočit výkon databáze nahoru nebo dolů. Databáze ve fondu s názvem *pružná databází*automaticky měřítko nahoru a dolů splňují požadavek. Pružná databází používat, ale není překročit fondu, tak i v případě použití databáze nemá zůstane předvídatelná náklady. Týmovými můžete [Přidat nebo odebrat databáze do fondu](sql-database-elastic-pool-manage-portal.md), měřítka aplikace ze hrstku databází k tisícům, vše v rámci rozpočtu, která můžete řídit. Další informace o provedeních SaaS aplikací pomocí pružná fondů, najdete v článku [Provedeních více klienta SaaS aplikací se databáze SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

V obou případech přejdete – jeden či pružná – nejsou Uzamčeno v. Prolnutí jedné databáze fondy pružná databáze a změňte služby úrovní jedné databáze a fondů k vytvoření nové návrhů. Kromě toho power a reach Azure, můžete kombinaci a POZVYHLEDAT Azure služeb SQL databáze podle vlastní potřeby návrh jedinečné moderní aplikace, řízení nákladů a zdrojů efektivity a odemknutí nové příležitosti business.

Ale můžete je srovnání relativní výkon databází a databáze fondů? Jak poznáte klikněte pravým tlačítkem myši – zastavit, když se telefonicky nahoru a dolů? Odpověď je jednotka transakce databáze (DTU) pro jednoho databáze a pružná DTU (eDTU) pro pružná databází a databáze fondů. V tématu [Možnosti SQL databáze a výkonu: porozumět tomu, co je k dispozici v každé vrstvy služeb](sql-database-service-tiers.md) podrobnosti.

## <a name="keep-your-app-and-business-running"></a>Zachovat aplikace a bezproblémový firmy

Azure na odvětví počáteční 99,99 % dostupnost smlouva o úrovni služeb [(SLA)](http://azure.microsoft.com/support/legal/sla/)technologii globální síť Microsoft Správa přístupových práv datacentrech pomáhá aplikace pro systém 24/7. Každé databáze SQL můžete využívat ochrany předdefinovaných dat, odolnost proti chybám a ochrana dat, které by jinak musíte návrhu, nákup, vytváření a správa. V závislosti na požadavky vyhovovaly i tak může vyžadovat další vrstvy a zajistit tak, že aplikace a vašeho podniku můžete rychle obnovit v případě selhání chybu nebo něco jiného. Databáze SQL jednotlivé vrstvy služeb nabízí různé nabídku funkcí, u kterých můžete získat a spuštění a pobytu tak. Obnovení v okamžiku umožňuje vrátit databáze předchozího stavu, již 35 dní. Pokud datacentru, který je hostitelem vašich databází dojde výpadku, kromě toho můžete převzetí replikami databáze v jiné oblasti. Nebo můžete použít, repliky inovace nebo přemístění u různých oblastí.

![Geo replikace databáze SQL](./media/sql-database-technical-overview/azure_sqldb_map.png)


Informace o funkcích kontinuitu podniková umožňující jiné službě úrovní najdete v článku [Nepřerušený](sql-database-business-continuity.md) .

## <a name="secure-your-data"></a>Zabezpečení dat
SQL Server má tradici souvislá data cenného papíru, aby databázi SQL zachovává s funkcemi, které omezit přístup, ochrana dat a pomáhají sledovat aktivity. Rychlé rundown možnosti zabezpečení, které máte v databázi SQL najdete v článku [zabezpečení databáze SQL](sql-database-security.md) . Viz [Centrum zabezpečení pro SQL Server databázový stroj a databáze SQL](https://msdn.microsoft.com/library/bb510589) pro komplexnější zobrazení funkce zabezpečení. A navštívit [Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/security/) informace o společnosti Azure platformy zabezpečení.

## <a name="next-steps"></a>Další kroky
Teď, když jste přečtěte si úvod k SQL databázi a zodpovězené otázky "Novinky databáze SQL", budete chtít:

- V tématu [ceny stránky](https://azure.microsoft.com/pricing/details/sql-database/) pro jednu databázi a porovnání náklady pružná databáze a kalkulačky.
- Informace o [pružná fondů](sql-database-elastic-pool.md).
- Začněte s [vytvářením první databáze](sql-database-get-started.md).
- [Připojení a dotaz s SSMS](sql-database-connect-query-ssms.md)
- Vytvoření první aplikace v jazyce C#, Java, Node.js, PHP, Python nebo skutečné: [knihovny připojení databáze SQL a SQL Server](sql-database-libraries.md)
- Zobrazte nadpisy a popis [všech](sql-database-index-all-articles.md)témat služby databáze sql Azure.
