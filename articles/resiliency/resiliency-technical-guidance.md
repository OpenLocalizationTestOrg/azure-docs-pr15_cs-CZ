<properties
   pageTitle="Index příručku odolnost proti chybám | Microsoft Azure"
   description="Index technické články o principy a navrhování pružné, vysoce dostupné, chybám aplikací, stejně jako plánování kontinuitu havárie obnovení pro podnikatele"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance"></a>Azure odolnost proti chybám příručku

##<a name="introduction"></a>Úvod

Dostupnost a havárie obnovení požadavky vyžaduje dva typy znalostí:

- Podrobné technické informace o platformy cloudové možnosti
- Věděli, jak správně architektonické distribuované služby

Tato řada článků zahrnuje dřívější: funkce a omezení platformy Azure týkající odolnost proti chybám (někdy se jí říká nepřerušený). Pokud vás zajímá ten, najdete v článku řady věnovaných [havárie využití a dostupnost pro Azure aplikace](https://aka.ms/drtechguide). Přestože řady článek dotýká architektura a návrh, není fokus na řadu. Pokyny pro návrh naleznete materiál v části [Další zdroje informací](#additional-resources) .

Informace jsou uspořádány do v následujících článcích:

- [Obnovení z místní selhání](resiliency-technical-guidance-recovery-local-failures.md).
Může selhat fyzických (například jednotky servery a síťových zařízení). Zdroje můžete vyčerpat, pokud špičky načíst. Tento článek popisuje funkce, které Azure poskytuje udržovat dostupnost za těchto podmínek.

- [Obnovení z přerušení služby Azure celé oblasti](resiliency-technical-guidance-recovery-loss-azure-region.md).
Rozšířených pracujících málo, ale jsou teoreticky může. Celé oblasti, může být izolace kvůli selhání sítě nebo může být fyzicky poškozené z přírodní havárie. Tento článek vysvětluje, jak používat Azure vytvářet aplikace, které zahrnují geograficky různých oblastí.

- [Obnovení z místního na Azure](resiliency-technical-guidance-recovery-on-premises-azure.md).
Shluk výrazně změní hospodárnost havárie obnovení, což umožňuje organizacím pomocí Azure můžete vytvořit druhý web pro obnovení. Lze provést na zlomek nákladů vytváření a udržování sekundárním datacentra. Tento článek popisuje funkce, které Azure poskytuje za účelem rozšíření místních datacentra do cloudu.

- [Obnovení z poškození dat nebo náhodného odstranění](resiliency-technical-guidance-recovery-data-corruption.md).
Aplikace můžete mít chyby poškozené data. Operátory nesprávně odstranit důležitá data. Tento článek vysvětluje, Azure poskytuje pro zálohování dat a obnovit předchozí bod času.

##<a name="additional-resources"></a>Další zdroje informací

- [Obnovení havárie a dostupnost pro aplikace založený na Microsoft Azure](resiliency-disaster-recovery-high-availability-azure-applications.md).
Tento článek je podrobný přehled dostupnost a obnovení. Týká se výzva ruční replikace odkaz a transakční data. Konečný části obsahují souhrnné informace o různých typů havárie obnovení topologií, které zahrnují Azure oblastí pro nejvyšší úroveň dostupnosti.

- [Kontrolní seznam vysoké dostupnosti](resiliency-high-availability-checklist.md).
Tento článek je seznam funkcí, službami, a návrhů, které vám pomohou zvýšit odolnost proti chybám a dostupnosti aplikace.

- [Pokyny pro odolnost proti chybám služby Microsoft Azure](resiliency-service-guidance-index.md).
Tento článek je index Azure služeb a obsahuje odkazy na pokyny obnovení havárie a pokyny pro návrh.

- [Přehled: cloudu firmy kontinuitu databáze havárie obnovení a s databáze SQL](../sql-database/sql-database-business-continuity.md).
Tento článek obsahuje databáze SQL Azure technik pro dostupnosti. Zarovná především na strategie pro zálohování a obnovení. Pokud používáte databázi SQL Azure do cloudové služby, přečtěte si tento dokument a související materiály.

- [Dostupnost a obnovení pro systém SQL Server ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).
Tento článek popisuje možnosti dostupnosti, které můžete prozkoumat použijete infrastruktury jako služba (IaaS) hostovat databáze služby. Popisuje skupiny dostupnosti AlwaysOn odrážející strukturu databáze, protokol a zálohování a obnovení. Několik výukové programy pro předvedení použití těchto postupů.

- [Doporučené postupy pro navrhování rozsáhlé služby Azure Cloud Services](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/).
Tento článek se zaměřuje na vývoj architektury vysoce scalable cloudu. V mnoha postupů, které využívají zlepšit škálovatelnost také zlepšit dostupnost. Také pokud aplikace nelze zobrazit vyšší zatížení, škálovatelnost bude problematická dostupnosti.

- [Zálohování a obnovení pro systém SQL Server ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
Tento článek obsahuje příručku zálohování a obnovení spuštěné v Azure virtuálních počítačích Microsoft SQL Server.

- [Bezporuchový: pokyny pro pružné cloudu architektury](https://channel9.msdn.com/Series/FailSafe).
Tento článek obsahuje pokyny k vytváření pružné cloudu architektury pokyny k provádění tyto architektury na technologie Microsoft a recepty k provedení těchto architektury pro konkrétní scénáře.

- [Technické Případová studie: použití technologie cloudu ke zlepšení obnovy havárie](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery).
Tato případová studie zobrazuje použití Azure ke zlepšení obnovy havárie IT společnosti Microsoft.

##<a name="next-steps"></a>Další kroky

Tento článek je součástí řady věnovaných příručku pro Azure odolnost proti chybám. Pokud vás zajímají další články z této série pro čtení, můžete začít s [využitím z místní selhání](resiliency-technical-guidance-recovery-local-failures.md).
