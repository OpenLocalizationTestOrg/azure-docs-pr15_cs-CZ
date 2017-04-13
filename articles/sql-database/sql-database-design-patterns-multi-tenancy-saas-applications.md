<properties
   pageTitle="Návrh vzorků pro víceklientské SaaS aplikace a databáze SQL Azure | Microsoft Azure"
   description="Tento článek popisuje požadavky a běžných architektura vzorků dat víceklientské databázi, kterou spuštěné v prostředí cloudu vzít do úvahy a různé kompromisy přidružené tyto vzorce. Také vysvětluje, jak databáze SQL Azure, s jejími pružná fondů a pružné nástroje pomoct řešit tyto požadavky způsobem bez narušení zabezpečení."
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-design"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-sql-database"></a>Návrh vzorků pro víceklientské SaaS aplikace a databáze SQL Azure

V tomto článku se naučíte požadavky a společná data architektura vzorků víceklientské software jako databázové aplikace služby (SaaS) spuštěné v prostředí cloudu. Vysvětluje také faktorů, které byste měli myslet a střídání vzorků jiný vzhled. Pružná fondů a pružné nástroje v databázi SQL Azure můžete podle specifických požadavků bez omezení další cíle.

Vývojáři někdy rozhodovat, které fungují před jejich dlouhodobé nejlepší zájmy při návrhu nájmu modely úrovní dat víceklientské aplikací. Nejprve alespoň vývojář může vnímat usnadnění vývoje a nižší náklady poskytovatele služby cloudu jako důležitější než izolace klienta nebo škálovatelnost aplikace. Tato možnost může vést k pochybnosti spokojenosti zákazníků a nákladné oprava kurzu později.

Víceklientské aplikace je hostovaný v prostředí cloudu program, který poskytuje stejnou sadu služeb stovky nebo tisíce klienti, kteří sdílejí nebo zobrazit data dalších uživatelů. Příklad je SaaS aplikace, která poskytuje služby k tenantům v prostředí hostující v cloudu.

## <a name="multitenant-applications"></a>Víceklientské aplikací

V aplikacích víceklientské dat a pracovní zátěž můžete snadno rozdělit na oddíly. Můžete rozdělit dat a pracovní zátěž, například podél hranice klienta, protože nejvíce požadavky dojít v rámci klienta, kterého. Vlastnost je vlastní data a pracovní zátěž a upřednostňuje vzorků aplikace popisované v tomto článku.

Vývojáři použít tento typ aplikace napříč celou formátování dat aplikace cloudového, včetně:

- Partner databázové aplikace, které jsou právě, které přešly do cloudu jako SaaS aplikace
- Aplikace SaaS vytvořené pro cloudu z důvodu
- Přímé zákazníka webových aplikací
- Zaměstnance webových podnikových aplikací

SaaS aplikace navržené pro cloudu a s odmocniny jako obvykle partnera databázové aplikace jsou víceklientské aplikace. Tyto aplikace SaaS předvádění aplikace speciální software jako služba jejich klienti. Klienti lze získat přístup ke službě aplikace a celou vlastnictví přidružené dat uložených jako součást aplikace. Ale umožní využít výhod SaaS musí klienti předáním některé publikum nemůže ovládat svá data. Budou důvěřovat poskytovatele služeb SaaS k synchronizaci související data z jiných klienti dat bezpečných a izolace. Příklady tohoto typu víceklientské SaaS aplikace jsou MYOB, SnelStart a Salesforce.com. Každá z těchto aplikací můžete rozdělit na oddíly podél hranice klienta a podporu aplikace návrh vzorky probereme tato témata v tomto článku.

Aplikace, které obsahují přímé služby zákazníkům nebo se zaměstnanci v rámci organizace (označovaná také jako uživatele, nikoli klienti) jsou jiné kategorie v dokumentech víceklientské aplikace. Zákazníci přihlášení k odběru služby a nevlastníte data, která poskytovatele služeb shromažďuje a ukládá. Poskytovatelé mají snížené požadavky, aby data svým zákazníkům samostatný nezávisle za právní jakákoli ochrany osobních údajů. Tento typ víceklientské aplikace zákazníkem příklady obsahu poskytovatelů médií například Netflix Spotify a Xbox LIVE. Další příklady snadno rozdělený aplikací zákazníkem, aplikace Internet měřítko, nebo Internet věcí (IoT) do kterých jednotlivé zákazníky nebo zařízení může sloužit jako oddíl. Oddíl omezení můžete rozdělit uživatelům a zařízením.

Ne všechny aplikace oddíl snadno podél jednu vlastnost například klienta, zákazníků, uživatele nebo zařízení. Plánování (ERP) aplikace složité podnikových zdrojů má například produkty, objednávky a Zákazníci. Má obvykle složité schéma s tisíce vysoce propojené tabulky.

Žádné strategie jeden oddíl můžete použít na všechny tabulky a práci ve pracovní zátěž aplikace. Tento článek se zaměřuje na víceklientské aplikací, které mají snadno rozdělený dat a úloh.

## <a name="multitenant-application-design-trade-offs"></a>Víceklientské aplikace návrh střídání

Návrh vzorku, který víceklientské vývojář zvolí obvykle se podle potřeba zvážit následující faktory:

-   **Izolace klienta**. Vývojář musí zajistit, aby žádné klienta nežádoucí přístup k datům jiných klientů. Požadavek izolace rozšíří do jiné vlastnosti, například zajistit ochranu z vysokou úroveň šumu sousední, je možné obnovit data do klienta a provádění klienta veškeré vlastní úpravy.
-   **Cloudové zdroje náklady**. Aplikace SaaS musí být konkurenční náklady. Vývojář aplikace víceklientské rozhodnout optimalizace ke snížení nákladů v seznamu použít cloudu zdrojů, jako je třeba úložiště a výpočet nákladů.
-   **Snadnější DevOps**. Vývojář aplikace víceklientské musí zahrnutí izolace ochranu zachovat a sledování stavu jejich aplikace a schématu databáze a Poradce při potížích klienta. Složitosti vývoj aplikací a operace převádí přímo na vyšší náklady a přijal pokusí se spokojenosti klienta.
-   **Škálovatelnost**. Postupně přidávat další klienty a kapacity pro klienty, kteří ho potřebují je nezbytné operace úspěšně SaaS.

Má každá z těchto skutečností střídání porovnání do jiného. Cloud nejnižší náklady nabízející nabízejí nejpohodlnější vývojové prostředí, může se stát. Je důležité pro vývojáře kvalifikovaně o těchto možnostech a jejich střídání během procesu návrhu aplikace.

Princip Oblíbené vývoje je pack víc klientů do jednoho nebo několika databází. Výhody tento přístup je nižší náklady, protože zaplacená za několik databází a relativní jednoduchosti o práci s omezený počet databází. Ale v průběhu času SaaS víceklientské vývojář zjistili, jestli má tato volba podstatné downsides v klientovi izolace a škálovatelnost. Pokud klienta izolace změní důležité, plánování řízené úsilí další potřebná k ochraně klienta dat ve sdíleném úložišti z neoprávněnému přístupu nebo vysokou úroveň šumu sousední. Tato další úsilí může výrazně zvýšit náročnosti a náklady na údržbu izolace. Podobně při přidání klienti povinný, tento způsob návrhu obvykle požaduje odborné znalosti distribuovat klienta dat přes databází správně zobrazit osy dat aplikace.  

Často izolace klienta je základním požadavkem SaaS víceklientské aplikací vyberte pro firmy a organizace. Vývojáři můžou zvážit tak, že zjištěné výhody v jednoduchosti a náklady izolace klienta a škálovatelnost. Tato změna prokázat složité a drahé jako služba roste a budou klienta izolace požadavky důležitých a spravovaných ve vrstvě aplikací. Však v víceklientské aplikace, které obsahují přímé, přístupným zákaznické služby pro zákazníky, klienta izolace pravděpodobně nižší prioritu, než optimalizace pro nákladové zdroje cloudu.

## <a name="multitenant-data-models"></a>Víceklientské datové modely

Běžné návrh postupy pro umístění dat klienta podle tři odlišné modely obrázku 1.


  ![Datové modely víceklientské aplikace](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)
 obrázek 1: běžné návrh postupy pro víceklientské datové modely

-   **Databáze na klienta**. Každého klienta má vlastní databázi. Všechna data specifické pro klienta je omezen na klienta databáze a samostatný z jiných klientů a související data.
-   **Sdílené databázi sharded**. Víc klientů sdílet mezi více databází. Odlišné sadu klienti přiřazen každou databázi pomocí rozdělení strategie například hash rozsahu nebo seznamu rozdělení. Tato data distribuce strategie často se nazývá sharding.
-   **Sdílené databázi jednoduchá**. Jedinou, někdy velké databázi obsahuje data pro všechny klienty, které jsou jednoznačně rozlišit ve sloupci ID klienta.

> [AZURE.NOTE] Vývojář aplikace se rozhodnout umístěte různých klientů schémata jinou databázi a pak pomocí název schématu rozlišit různých klientů. Protože obvykle vyžaduje použití dynamického příkazu SQL a nesmí být efektivní v plánu ukládání do mezipaměti nedoporučujeme tento přístup. Ve zbývající části tohoto článku jsme zaměření se na postup sdílené tabulce u této kategorie víceklientské aplikace.

## <a name="popular-multitenant-data-models"></a>Oblíbené víceklientské datové modely

Je důležité vyhodnocení různé typy víceklientské datové modely z hlediska aplikace návrh střídání, který jste už identifikovat. Následujících skutečností nápovědy určení tři nejběžnější víceklientské datovými modely popsané výše a jejich použití databáze, jak je vidět na obrázku 2

-   **Izolace**. Míru izolace mezi klienty může být míru izolace kolik klienta dosahuje datový model.
-   **Cloudové zdroje náklady**. Částka sdílení zdrojů mezi klienty můžete optimalizovat cloudové zdroje náklady. Zdroj lze definovat jako výpočetním a úložiště nákladů.
-   **DevOps náklady**. Snadnější aplikace vývoj, nasazení a správu snižuje celkové náklady operace SaaS.  

Obrázek 2 s osou Y ukazuje úroveň izolace klienta. Osy X ukazuje úroveň sdílení zdrojů. Šedé, diagonální šipku uprostřed Určuje směr DevOps náklady, které sledují nezvětšíte nebo nezmenšíte.

![Oblíbené víceklientské aplikace provedeních](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png) obrázek 2: Oblíbené víceklientské datové modely

Pravé dolní čtvrtině obrázek 2 zobrazuje aplikace, který používá potenciálně velkou, sdílené jednu databázi a sdílené tabulky (nebo samostatné schéma) přístup. Je vhodné pro sdílení, protože všechny klienty použít stejné zdroje databáze (procesoru, paměti, vstupní a výstupní) v jedné databáze zdrojů. Ale izolace klienta je omezené. Je potřeba provést další kroky k ochraně klienti nezávisle ve vrstvě aplikací. Následující kroky může výrazně zvýšit náklady DevOps vývoje a Správa aplikace. Škálovatelnost je omezen měřítka hardwaru, který je hostitelem databáze.

Levém dolním čtvrtina obrázek 2 ukazuje víc klientů sharded přes více databázím (obvykle jiný hardware měřítko jednotky). Každou databázi hostuje podmnožinu klienti, které adresy škálovatelnost zájem o jiné vzorků. Pokud další kapacita je nutný pro další student, můžete snadno vložit klienti na nové databáze přidělit nové jednotky měřítka hardwaru. Však je snížit množství sdílení zdrojů. Pouze klienti umístěny na stejné měřítko jednotky dialogu sdílet zdroje. Tento přístup poskytuje malé zlepšení klient izolace, protože mnoho klienti jsou pořád použít pro společné umísťování bez automaticky chráněn z ostatních akcí. Aplikace složitost zůstane vysoká.

Čtvrtina levou obrázek 2 je třetí přístup. Umístí každého klienta data v jeho vlastní databázi. Tento přístup je dobré klienta izolace vlastnosti ale omezuje sdílení zdrojů při každé databáze obsahuje vlastní vyhrazené zdroje. Tento přístup je vhodné máte všechny klienty předvídatelná úloh. Pokud klienta úloh stanou méně předvídatelná, zprostředkovatele neumí optimalizovat sdílení zdrojů. Nepředvídatelnost je běžné SaaS aplikací. Zprostředkovatel musí být buď navýšení poskytování požadavkům nebo nižší zdroje. Obě tyto akce výsledkem vyšší náklady nebo nižší spokojenosti klienta. K tomu řešení efektivnější se změní vyšší stupeň sdílení přes klienti zdrojů. Zvýšení počtu databází taky zvyšuje DevOps náklady na nasazení a spravovat aplikace. Bez ohledu na tyto pochybnosti tato metoda poskytuje nejlepší a nejjednodušší izolace pro klienty.

Následujících skutečností také ovlivnit vzorek návrh, který zákazník rozhodne:

-   **Vlastnictví dat klienta**. Aplikace, ve kterém klienti zachovat vlastnictví svá data upřednostňuje vzorek jednu databázi na klienta.
-   **Měřítko**. Aplikaci stovky tisíce nebo milióny klienti upřednostňuje sdílení přístupy například sharding databáze. Požadavky na izolace pořád můžou být z hlediska úkoly.
-   **Hodnotu a obchodní model**. Pokud aplikace výnosy za klienta Pokud malé (nižší než Česká koruna), izolace požadavky se stanou méně kritické a ke sdílené databázi smysl. Pokud výnos jednoho klienta je několik dolary nebo další, model databáze jednoho klienta je vhodnější. Mohlo by pomoci snížit náklady na vývoj.

Možnost návrh střídání obrázku 2, třeba ideální víceklientské datového modelu pro zahrnutí dobré klienta izolace vlastnosti s sdílení mezi klienty optimální zdrojů. Tento model přizpůsobí v kategorii popsané v pravém horním čtvrtině obrázek 2.

## <a name="multitenancy-support-in-azure-sql-database"></a>Multitenancy podpora v databázi SQL Azure

Databáze SQL Azure podporuje všechny vzorky víceklientské aplikace uvedené v obrázek 2. Pružná fondy taky podporuje aplikace vzorku, který kombinuje sdílení užitečný zdroj a izolace výhody databáze na klienta přiblíží (viz čtvrtina pravého horního obrázek 3). Pružná databázové nástroje a funkce v databázi SQL snížit náklady zobrazte vyvíjet a provozovat aplikace, která obsahuje mnoho databází (ukazuje stínovanou oblastí v obrázek 3). Tyto nástroje můžete vytvářet a spravovat aplikace, které používají některý z vzorků více databáze.

![Vzorky v databázi SQL Azure](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png) obrázek 3: vzorky víceklientské aplikace v databázi SQL Azure

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Model databáze na klientovi s pružná fondů a nástroje

Pružná fondů v databázi SQL kombinovat klienta izolace s sdílení mezi databází klienta pro lepší podporu přístupu databáze na klienta zdrojů. Databáze SQL je řešení osy dat pro SaaS poskytovatelů, kteří vytvářejí víceklientské aplikací. Zatížení sdílení mezi klienty zdrojů posune z vrstvy aplikace služby vrstva databáze. Složitost dotazu ve velkém měřítku přes databází a jeho správa zjednodušené s pružná úlohy, pružná dotazu, pružná transakce a knihovně klienta pružná databáze.

| Požadavky aplikace | Funkce databáze SQL |
| ------------------------ | ------------------------- |
| Izolace klienta a sdílení zdrojů | [Pružná fondů](sql-database-elastic-pool.md): přidělovat fondu zdrojů databáze SQL a sdílení zdrojů mezi různých databází. Jednotlivé databází také upoutat tolik zdroje z fondu podle potřeby tak, aby zahrnoval vrcholy pole Služba kapacita kvůli změnám v klientovi úloh. Pružná samotné skupiny můžete podle potřeby zachován nahoru nebo dolů. Pružná fondů taky poskytují usnadnění správy sledování a odstraňování potíží na úrovni fondu. |
| Snadnější DevOps přes databází | [Pružná fondů](sql-database-elastic-pool.md): výše uvedených hodnot.|
||[Pružná dotaz](sql-database-elastic-query-horizontal-partitioning.md): dotazu mezi databázemi pro vytváření sestav nebo více klienta analýzy.|
||[Pružná úlohy](sql-database-elastic-jobs-overview.md): balíčku a problémy se spolehlivým k více databázím nasazení operace údržby databáze nebo změny schématu databáze.|
||[Pružná transakce](sql-database-elastic-transactions-overview.md): obrázku se změní na několik databází atomová a izolace způsobem. Pokud aplikace potřebují "vše nebo nic" záruky přes několik databázových operací, je nutné pružná transakce. |
||[Knihovna klienta pružná databáze](sql-database-elastic-database-client-library.md): Správa distribuci dat a zmapovat tenantů k databázím. |

## <a name="shared-models"></a>Sdílené modely

Jak je popsáno dříve, většina poskytovatelů SaaS sdílený model přístup může představovat problémy s problémy izolace klienta a složitosti s vývoj aplikací a údržba. Však víceklientské aplikací, které jsou zdrojem služby přímo do spotřebitele, nemusí být klienta izolace požadavky na nejvyšší prioritu jako minimalizace náklady. Může být schopni pack klienti v jedné nebo více databází na vysoké hustoty snížit náklady. Modely sdílené databázi pomocí databázi jednoho nebo více sharded databází může způsobit další efektivitu v sdílení a celkové náklady na zdroje. Databáze SQL Azure poskytuje některé funkce, které pomáhají sestavit izolace pro lepší zabezpečení a správa ve velkém měřítku v osy dat.

| Požadavky aplikace | Funkce databáze SQL |
| ------------------------ | ------------------------- |
| Funkce izolace zabezpečení | [Řádek úrovně zabezpečení](https://msdn.microsoft.com/library/dn765131.aspx) |
|| [Schéma databáze](https://msdn.microsoft.com/library/dd207005.aspx) |
| Snadnější DevOps přes databází | [Pružná dotazu](sql-database-elastic-query-horizontal-partitioning.md) |
|| [Pružná úlohy](sql-database-elastic-jobs-overview.md) |
|| [Pružná transakce](sql-database-elastic-transactions-overview.md) |
|| [Pružná databáze klienta knihovny](sql-database-elastic-database-client-library.md) |
|| [Rozdělení pružná databáze a sloučení](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Souhrn

Požadavky na izolace klienta jsou důležité pro většinu SaaS víceklientské aplikací. Nejlepší možností poskytnout izolace leans velkém směrem k databázi jednoho tenanta přístup. Dalších dvou postupů vyžadují investic do složitých aplikací vrstvy, které vyžadují kvalifikovaných vývoj zaměstnance k poskytnutí izolace, která výrazně zvýší náklady a rizika. Požadavky na izolace nejsou připadá brzy v vývoj služby, dodatečného je vybavení může být i nákladnější v prvních dvou modely. Hlavní nevýhody přidružených k modelu databáze na klienta souvisí s náklady na zdroje lepší cloudu kvůli omezená sdílení a udržování a správa mnoho databází. Vývojáři SaaS často čtením při provádění tyto střídání.

Ačkoli střídání může být hlavní překážky s většina poskytovatelů služeb databáze cloudu, databáze SQL Azure eliminuje překážky s jejími pružná fondu a možností pružná databáze. Vývojáři SaaS můžete kombinovat vlastnosti izolace modelu databáze na klienta a optimalizace vylepšení správy mnoho databází a sdílení zdrojů pomocí pružná fondů a související nástroje.

Poskytovatelé víceklientské aplikací, kteří mají nainstalovanou žádné požadavky izolace klienta a kdo pack klienti v databázi vysoké hustoty může se stát, že sdílené datové modely výsledek v dalších efektivity při sdílení prostředků a sníží celkové náklady. Azure SQL databáze pružná databázové nástroje, sharding knihoven a funkce zabezpečení pomoci SaaS poskytovatelů vytvářet a spravovat víceklientské aplikace.

## <a name="next-steps"></a>Další kroky

[Začínáme s pružná databázové nástroje](sql-database-elastic-scale-get-started.md) s aplikací vzorku, který demonstruje knihovnu klienta.

Vytvořte [pružná fondu vlastní řídicí panel pro SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) se v aplikaci vzorku, který používá pružná fondů řešení efektivní, scalable databáze.

Pomocí nástrojů databáze SQL Azure migrace [existující databáze do rozšiřování](sql-database-elastic-convert-to-use-elastic-tools.md).

Zobrazení naše kurz o tom, jak [Vytvořit fond pružná](sql-database-elastic-pool-create-portal.md).  

Zjistěte, jak [sledovat a spravovat pružná fondu](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Další zdroje informací

- [Co je Azure pružná fondu?](sql-database-elastic-pool.md)
- [Rozšiřování s databáze SQL Azure](sql-database-elastic-scale-introduction.md)
- [Víceklientské aplikací s pružná databázové nástroje a řádek úrovně zabezpečení](sql-database-elastic-tools-multi-tenant-row-level-security.md)
- [Ověřování víceklientské aplikace pomocí služby Azure Active Directory a OpenID připojení](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin průzkumy aplikace](../guidance/guidance-multitenant-identity-tailspin.md)
- [Rychlé zahájení práce řešení](sql-database-solution-quick-starts.md)

## <a name="questions-and-feature-requests"></a>Dotazy a poslat žádost o funkci

Dotazy najděte us ve [fóru databáze SQL](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Přidání žádost o funkci ve [fóru názory databáze SQL](https://feedback.azure.com/forums/217321-sql-database/).
