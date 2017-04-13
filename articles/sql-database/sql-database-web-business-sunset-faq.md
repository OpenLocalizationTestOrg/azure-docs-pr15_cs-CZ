<properties
   pageTitle="Azure SQL databáze Web a Business Edition Jahoda nejčastější dotazy týkající se | Microsoft Azure"
   description="Zjistíte, kdy databáze Azure SQL Web a Business se už nepoužívá a seznamte se s funkcí a možností nové úrovně služby."
   services="sql-database"
   documentationCenter="na"
   authors="stevestein"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/08/2016"
   ms.author="sstein" />

# <a name="web-and-business-edition-sunset-faq"></a>Web a Business Edition Jahoda – nejčastější dotazy

Azure SQL Web a Business databáze jsou teď už nepoužívá. Vrstvách Basic, Standard, Premium a ohebné nahraďte retiring Web a obchodní databází.

K pomůžou při upgradu webu a Business databází, doporučuje služby SQL databáze příslušné služby osy a výkonu úroveň (cena za úrovně) na každou databázi podle historických zátěž.

**Chcete-li získat ceny osy doporučení:**

- [Upgrade na V12 databáze SQL Azure portálu](sql-database-upgrade-server-portal.md)
- [Upgrade na V12 databáze SQL pomocí prostředí PowerShell](sql-database-upgrade-server-powershell.md)
- [Změna ceny úrovně webu nebo na obchodní databáze](sql-database-service-tier-advisor.md)



## <a name="why-does-the-azure-portal-show-my-web-and-business-edition-databases-as-retired"></a>Proč portálu Azure zobrazit svůj Web a Business edition databází už nepoužívá?

Protože Web a Business edition databáze nebude dostupná po 2015 dne, na portálu zobrazuje Web a Business databází už nepoužívá. Vyřazené popisek obsahuje také připomenutí, že Web a Business databáze by měl být upgradovaný na verzi Standard, základní nebo Premium. Podrobné informace o upgradu existující Web nebo Business databáze na nové úrovně služeb najdete v článku [Upgrade na V12 databáze SQL Azure](sql-database-upgrade-server-portal.md).

## <a name="which-new-service-tier-is-the-best-choice-to-upgrade-my-existing-web-or-business-database-to"></a>Které nové osy služby je nejlepší volbou upgradovat existující Web nebo Business databáze aplikace na?

Vyberete požadovanou nové služby osy a výkonu úroveň existující Web nebo Business databáze závisí na určité funkce a výkonu požadavků na aplikace.

Pomocí ceny osy doporučení popsané výše a podrobné informace pomůžou při výběru odpovídající nové vrstvy služeb najdete v článku [Upgrade V12 databáze SQL Azure](sql-database-upgrade-server-portal.md).

## <a name="why-is-microsoft-introducing-new-service-tiers"></a>Proč je Microsoft úvodní informace o nové – úrovně služeb?

Na základě reakcí zákazníků, databáze SQL Azure je úvodní informace o nové – úrovně služeb pomůže další snadno podporují pracovního vytížení relační databáze. Nové vrstvy jsou navrženy pro předvídatelná výkonu v dokumentech sedm úrovní šedé na transakční aplikace zobrazené – požadavky. Kromě toho nové úrovně nabízejí velké firmy kontinuitu funkcí silnější SLA provozu větších databáze pro menší peněz a vylepšení fakturační prostředí.

## <a name="where-can-i-learn-more-about-the-new-service-tiers"></a>Kde se dají najít další informace o nové úrovně služeb?

Podrobné informace o nové služby úrovní a výkonu modelu najdete v článku [– úrovně služeb](sql-database-service-tiers.md). Podrobné informace o nové služby úrovní cenách, najdete v článku [ceny databáze SQL](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="what-features-or-functionality-will-not-be-available-in-basic-standard-and-premium"></a>Jaké funkce a funkce nebudou k dispozici v Basic, Standard a Premium?

Funkce organizace bude vyřadit s edicemi Web pro podnikatele. Zákazníci, kteří potřebují škálování jejich databáze se doporučuje namísto použití nebo migrace do [pružná databázové nástroje](sql-database-elastic-scale-get-started.md) pro [Databázi SQL Azure](sql-database-elastic-scale-get-started.md), což usnadňuje vytváření a správě aplikace, která používá sharding. Knihovny klienta .NET umožňuje aplikací určit, jak data namapovala ke shards a trasy OLTP požadavků na příslušný databází. K podpoře operace správy, které překonfigurovat rozdělování dat mezi shards, je součástí budete moct hostovat v Azure předplatné šablonu služby Azure cloudu. Kromě [pružná databázové nástroje](sql-database-elastic-scale-get-started.md)zůstanou Microsoft vytvářet a publikovat [vlastní sharding vzorků a postupy pokyny](https://msdn.microsoft.com/library/azure/dn764977.aspx) podle learnings z závazky hloubkové zákazníka. Nové zákazníkům, kteří potřebují měřítko, funkce by měly najdete v článku [pružná databázové nástroje](sql-database-elastic-scale-get-started.md) a/nebo kontaktovat Microsoft Support k vyhodnocení jejich možnosti.

Microsoft je i změna prostředí kopii databáze s databázemi Premium. Dříve premium databáze kvóty bylo nutné omezit velikost vytvořte databázi... JAKO KOPII v vytvořili T-SQL databázi pozastavené Premium bez vyhrazené prostředky, které účtována sazbou stejný jako databázi Business. Stejně jako premium kvóty teď více volně dostupné, se už nepodporuje pozastavené Premium. Kopie databáze se vytvoří teď se stejným edition a úroveň výkonu jako zdroj a se vám nebudou účtovat poplatky příslušným způsobem. Zákazníci si mohou vybrat chcete zkopírovaný databází na jinou službu osy nebo výkonu úroveň snížit náklady, pokud budete chtít přejít na. Existující databáze pozastavené Premium se převedou na Business edition jako součást této verzi. Se předpokládá, že zavedení [Obnovit v okamžiku](sql-database-recovery-using-backups.md#point-in-time-restore) sníží nutné vytvořit záložní kopii databáze.

## <a name="how-does-basic-standard-and-premium-improve-my-billing-experience"></a>Jak Basic, Standard a Premium zlepšit fakturační prostředí?

Základní, standardní, databáze Premium Azure SQL je faktura vystavená hodiny a máte možnost zobrazit každou databázi nahoru nebo dolů. Je faktura pevných sazbou podle nejvyšší úrovně a výkonu úrovní služeb určenou pro každou hodinu. Navíc výkonu úrovně (Příklad: Basic, S1 a P2) jsou rozdělen na faktuře usnadňuje zobrazíte databáze dny/čas v hodinách vynaložené na jeden měsíc pro každou úroveň výkonu. Web pro podnikatele databází dál vám nebudou účtovat poplatky pomocí jednotek databáze podle velikost databáze. Navštivte [databáze SQL ceny stránky](https://azure.microsoft.com/pricing/details/sql-database/) zobrazíte další informace o ceny a rozdíly mezi nové úrovně služby.


## <a name="see-also"></a>Viz taky

[Databáze Azure SQL](https://azure.microsoft.com/documentation/services/sql-database/)

[Web a obchodní ceny](https://azure.microsoft.com/pricing/details/sql-database/web-business/)

[– Úrovně služeb](sql-database-service-tiers.md)

[Upgrade na V12 databáze Azure SQL](sql-database-upgrade-server-portal.md)
