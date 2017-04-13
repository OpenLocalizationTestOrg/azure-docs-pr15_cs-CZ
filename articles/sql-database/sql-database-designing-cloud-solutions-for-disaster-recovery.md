<properties
   pageTitle="Cloudové řešení obnovení havárie - SQL databáze aktivní Geo replikace | Microsoft Azure"
   description="Zjistěte, jak navrhnout cloudového havárie obnovení řešení pro obchodní kontinuitu plánování pomocí geo replikace pro zálohování dat aplikace s databáze SQL Azure."
   keywords="cloud havárie obnovení řešení obnovení havárie, zálohování dat aplikace, geo replikace, obchodní kontinuitu plánování"
   services="sql-database"
   documentationCenter=""
   authors="anosov1960"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="07/20/2016"
   ms.author="sashan"/>

# <a name="design-an-application-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Návrh žádost o obnovení havárie cloudu pomocí aktivní Geo replikace databáze SQL


> [AZURE.NOTE] [Aktivní Geo replikace](sql-database-geo-replication-overview.md) je teď k dispozici pro všechny databáze ve všech úrovní.



Naučte se používat [Aktivní Geo replikace](sql-database-geo-replication-overview.md) databáze SQL pro návrh databáze aplikace pružné místní selhání a katastrofické výpadků. Pro obchodní kontinuitu plánování, zvažte topologii nasazení aplikace smlouva o úrovni služeb směrujete, přenosy latence a náklady. V tomto článku podívejte se na běžná schémata aplikace jsme diskutovat o výhodách a střídání jednotlivých možností. Informace o aktivní Geo replikace pružná fondy najdete v tématu [Strategie obnovení havárie pružná fondu](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Návrh vzorci 1: aktivní pasivní nasazení cloudu havárie obnovení s společně umístit databází

Tato možnost je nejvhodnější pro aplikace s následujícími vlastnostmi:

+ Aktivní instanci v jedné oblasti Azure
+ Směrový závislost na pro čtení i zápis (RW) přístup k datům
+ Propojení mezi oblast logiky aplikace a databázi není přijatelná kvůli latence a provoz nákladů    

V tomto případě topologie aplikací nasazení je optimalizována pro zpracování místní havárie při všechny součástí aplikace jsou ovlivněny a potřebujete převzetí jako celek. Pro zeměpisné redundance logiky aplikace a databázi replikovat na jiné oblasti, ale nejsou použity pro pracovní zátěž aplikace běžných podmínkách. Aplikace v oblasti sekundární by měl nakonfigurován pro použití připojovací řetězec SQL na databázi sekundární. Přenosy manager je nastavit tak, aby použijte [metodu směrování překlopení](../traffic-manager/traffic-manager-configure-failover-routing-method.md).  

> [AZURE.NOTE] [Správce Azure přenosy](../traffic-manager/traffic-manager-overview.md) slouží v tomto článku obrázku pouze pro účely. Můžete použít řešení vyrovnávání zatížení, který podporuje metodu směrování překlopení.    

Kromě instance hlavní aplikace měli byste zvážit nasazení malá [pracovníka role aplikace](cloud-services-choose-me.md#tellmecs) , která sleduje hlavní databází zadáním pravidelných T-SQL jen pro čtení příkazy (RO). Můžete ho pro automatické spuštění selhání generovat upozornění na konzole pro správu aplikace, nebo obojí. Abyste měli jistotu, že sledování není ovlivněny výpadků celé oblasti, by měl nasazení sledování instancí aplikace pro každou oblast a mít jednotlivé instance primární databáze v oblasti primární a sekundární databáze v oblasti místní připojovat se k. 

> [AZURE.NOTE] Jak sledování aplikací by měl být aktivní a probe primárních a sekundárních databází. Ten lze zjistit selhání sekundární oblast a výstrahy, kdy aplikace není zamčený.     

Na následujícím obrázku vidíte této konfiguraci před výpadku.

![Konfigurace Geo-replikace databáze SQL. Obnovení havárie cloudu.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Po výpadku v oblasti hlavní aplikace pro sledování zjistí, že je hlavní databází není k dispozici a zaregistruje upozornění. V závislosti na vaší SLA aplikace můžete se rozhodnout, kolik po sobě jdoucí sledování sond by měl nepovede před deklaruje výpadku databáze. Abyste dosáhli koordinovaný selhání koncovou aplikace a databázi, byste si měli aplikace pro sledování proveďte následující kroky:

1. Chcete-li spustit selhání koncovou [Aktualizovat stav primárního bod](https://msdn.microsoft.com/library/hh758250.aspx) .
2. Zavolejte na vedlejší databáze zahajte [převzetí databáze](sql-database-geo-replication-portal.md).

Po selhání aplikace zpracuje uživatelských požadavků v oblasti sekundární ale zůstane umístit současně s databází, protože primární databáze teď budou v oblasti sekundární. Tento scénář je znázorněn další diagramem. Ve všech diagramech plné čáry označují aktivní připojení, tečkované čáry označují pozastavené připojení a zastavit znaménka označení Aktivace akce.


![Replikace GEO: Přepnutí sekundární databáze. Zálohování dat aplikace.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

V případě výpadku v oblasti sekundární je pozastavené replikace propojení mezi primární a sekundární databáze a aplikace pro sledování zaregistruje upozornění, že se zobrazí primární databáze. Není dopad na výkon aplikace, ale aplikace funguje vystaveného a tedy vyšší riziko v případě obě oblasti nepodaří odeslat po sobě.

> [AZURE.NOTE] Doporučujeme pouze konfigurace nasazení s jedinou DR oblast. Je to proto většina Azure zeměpisných má dvou oblastí. Tyto konfigurace nebude chránit vaše aplikace z Katastrofální selhání z obou oblastí. V události pravděpodobně se nepovede se dají obnovit databáze ve třetím oblastí pomocí [geo obnovení](sql-database-disaster-recovery.md#recovery-using-geo-restore).

Jakmile zmírnit výpadku sekundární databáze automaticky synchronizovány s primární. Při synchronizaci primární může být trochu dopad na výkon podle objemu dat, která potřebuje k synchronizaci. Následující obrázek znázorňuje výpadku v oblasti sekundární.

![Sekundární databáze sesynchronizovaná s hlavní. Obnovení havárie cloudu.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


Klíčové **výhody** tento způsob návrhu jsou:

+ Připojovací řetězec SQL je nastavena během nasazení aplikace v jednotlivých oblastech a nezmění po překlopení.
+ Není tak, že převzetí jako aplikace dopad na výkon aplikace a databáze nacházejí vždy spoluvytváření.

Hlavní **kompromis** je instance nadbytečné aplikace v oblasti sekundární je pouze pro havárie obnovení.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Návrh vzorek 2: aktivní nasazení aplikace Vyrovnávání zatížení
Tuto možnost obnovení havárie cloudu je nejvhodnější pro aplikace s následujícími vlastnostmi:

+ Vysoký poměr databáze přečte zápisy
+ Latence zápisu databáze nemá vliv na koncovým  
+ Použití logických operátorů jen pro čtení můžete oddělená od logiky pro čtení i zápis pomocí různých připojovacího řetězce
+ Použití logických operátorů jen pro čtení nezávisí na dat není plně synchronizovaný s nejnovějšími aktualizacemi  

Pokud aplikace tyto vlastnosti, připojení koncového uživatele napříč několika instancí aplikace služby v různých oblastech Vyrovnávání zatížení zlepšit výkon a koncovým. Provádět Vyrovnávání zatížení každou oblast měli aktivní instanci aplikace s připojení k databázi primární v oblasti hlavní logickou (RW) pro čtení i zápis. Logika jen pro čtení (RO) měli připojených k databázi sekundární ve stejné oblasti jako instance aplikace. Přenosy Správce by měl nastavit pomocí služby [směrování kruhového](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) nebo [výkonu směrování](../traffic-manager/traffic-manager-configure-performance-routing-method.md) [bod sledování](../traffic-manager/traffic-manager-monitoring.md) povoleno pro každou instanci aplikace.

Jako vzorek #1 měli byste zvážit zavedení podobné aplikace pro sledování. Ale na rozdíl od vzorek #1, aplikace pro sledování zodpovědný za aktivaci převzetí bod.

> [AZURE.NOTE] Během tento postup využívá víc než jednu vedlejší databázi, byste měli použít pouze jeden druhotné pro překlopení důvodů bylo uvedeno dříve. Protože tento způsob vyžaduje přístup k sekundární jen pro čtení, vyžaduje aktivní Geo replikace.

Přenosy Správce by měl nakonfigurován výkon směrování přímé připojení uživatele k instanci aplikace nejblíže k zeměpisné polohy uživatele. Následující obrázek znázorňuje tento konfigurace před výpadku.
![Žádné výpadku: výkonu směrování nejbližší aplikace. Replikace GEO.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Pokud je zjištěno výpadku databáze v oblasti primární, spouštíte převzetí primární databáze na jeden z oblasti sekundární změníte umístění ovládacích primární databáze. Správce přenosy automaticky nezahrnuje offline bod z směrování tabulky, ale dál směrování přenosu koncových uživatelů na zbývající online instance. Protože primární databáze je teď v jiné oblasti, musíte změnit všechny výskyty online jejich pro čtení i zápis SQL připojovací řetězec pro připojení k nové primární. Je důležité provedení této změny před zahájením záložní databáze. Řetězců SQL připojení jen pro čtení by měl zůstat beze změny, jako jsou vždy přejděte na databáze ve stejné oblasti. Převzetí se:  

1. Změna řetězců SQL připojení pro čtení i zápis tak, aby ukazovaly na nové primární.
2. Zavolejte určené sekundární databáze za účelem [převzetí zahájení databáze](sql-database-geo-replication-portal.md).

Následující obrázek znázorňuje nové konfigurace po záložní.
![Konfigurace po překlopení. Obnovení havárie cloudu.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

V případě výpadku v jednom z oblasti sekundární správce přenosy automaticky odebere offline koncového bodu v dané oblasti směrovací tabulky. Replikace kanálu sekundární databáze v dané oblasti se pozastaví. Protože zbývající oblastí získáte další provozu v tomto scénáři je dopad na výkon aplikace během výpadku. Po zmírnit výpadku sekundární databáze v oblasti dotčeném okamžitě sesynchronizovaná s hlavní. Při synchronizaci primární může být trochu dopad na výkon podle objemu dat, která potřebuje k synchronizaci. Následující obrázek znázorňuje výpadku v jednom z oblasti sekundární.

![Výpadku v sekundární oblast. Obnovení havárie - Geo replikace v cloudu.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Nejdůležitější **využít** tento způsob návrhu je, že je možné přizpůsobit pracovní zátěž aplikace napříč několika druhotné dosáhnout koncových uživatelů optimální výkon. **Poměry** tato možnost je:

+ Čtení a zápis připojení mezi instancí aplikace služby a databází k dispozici různé latence a náklady
+ Během výpadku je dopad na výkon aplikace
+ Instancí aplikace je třeba dynamicky měnit připojovací řetězec SQL po převzetí databáze.  

> [AZURE.NOTE] Podobný přístup mohou sloužit k převzít specializované úloh třeba sestav úloh, nástrojů business intelligence nebo zálohy. Obvykle tyto pracovního vytížení využití prostředků významné databáze proto doporučujeme, aby se určuje jednu vedlejší databází, je úrovní výkonu odpovídají očekávané pracovní zátěž.

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Návrh vzorek 3: aktivní pasivní nasazení uchovávání dat  
Tato možnost je nejvhodnější pro aplikace s následujícími vlastnostmi:

+ Ztráty dat jsou vysoké firmy rizika. Převzetí databáze můžete použít jako poslední možnost pouze v případě výpadku je trvalé.
+ Aplikaci můžete pracovat v "režim jen pro čtení" pro určitého časového období.

V tomto vzoru aplikace přepne do režimu jen pro čtení při připojení k databázi sekundární. Logika aplikace v oblasti primárního spoluvytváření nachází s hlavní databází a pracuje v režimu čtení a zápis (RW). Logika aplikace v oblasti sekundární spoluvytváření nachází se sekundární databází a je připraven k ovládání v režimu jen pro čtení (RO).  Přenosy Správce by měl nastavit tak, aby pomocí [směrování selhání](../traffic-manager/traffic-manager-configure-failover-routing-method.md) [koncovou sledování](../traffic-manager/traffic-manager-monitoring.md) povoleno pro obě instance aplikace.

Následující obrázek znázorňuje tento konfigurace před výpadku.
![Aktivní pasivní nasazení před překlopení. Obnovení havárie cloudu.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Pokud správce přenosy zjistí selhání připojení k oblasti primárního, vymění automaticky provozu instanci aplikace v oblasti sekundární. Pomocí tohoto vzorku je důležité, které se **nemají** zahájit databáze převzetí po zjištění výpadku. Aplikace v oblasti sekundární aktivaci a pracuje v režimu jen pro čtení pomocí sekundární databáze. To je znázorněn tak, že na následujícím obrázku.

![Výpadku: Aplikace v režimu jen pro čtení. Obnovení havárie cloudu.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Po zmírnit výpadku v oblasti primární správce přenosy rozpozná obnovení připojení v oblasti hlavní a kombinace kláves vymění provozu zpátky do instance aplikace v oblasti hlavní. Tuto instanci aplikace obnoví a pracuje v režimu čtení a zápis pomocí primárního databáze.

> [AZURE.NOTE] Protože tento způsob vyžaduje jen pro čtení přístup k sekundární vyžaduje aktivní Geo replikace.

V případě výpadku v oblasti sekundární správce přenosy označí bod aplikace v oblasti hlavní sníženou kvalitu a je pozastavené kanálu replikace. Však tento výpadku nemá vliv na výkon aplikace během výpadku. Jakmile zmírnit výpadku sekundární databáze okamžitě sesynchronizovaná s hlavní. Při synchronizaci primární může být trochu dopad na výkon podle objemu dat, která potřebuje k synchronizaci.

![Výpadku: Sekundární databáze. Obnovení havárie cloudu.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Tento způsob návrhu obsahuje několik **výhod**:

+ Zabrání ztrátou dat během dočasné výpadků.
+ Ho nevyžaduje nasazení aplikace pro sledování při obnovení spouštěný kliknutím přenosy správce.
+ Prostoj při závisí pouze jak rychle přenosy správce zjistí chyby připojení, což je možné nakonfigurovat.

**Poměry** jsou:

+ Aplikace musíte mít pracovat v režimu jen pro čtení.
+ Je potřeba dynamicky neho přepnout při připojení k databázi jen pro čtení.
+ Obnovení operací pro čtení i zápis vyžaduje obnovení primární oblasti, což znamená, že úplné datové může být zakázaný hodiny nebo dokonce dny.

> [AZURE.NOTE] V případě výpadku trvalé služby v oblasti musí ručně aktivovat převzetí databáze a přijměte ztrátou dat. Aplikace bude funkční v oblasti sekundární pro čtení i zápis přístup k databázi.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Obchodní kontinuitu plánování: výběrem položky návrh aplikace pro obnovení havárie cloudu

Strategie konkrétní cloudu havárie obnovení můžete sloučit nebo rozšíření tyto provedeních nejlépe potřebám aplikace.  Jak jsme zmínili dříve, strategie, jaká jste podle SLA chcete nabízet vaši zákazníci a nasazení topologie aplikací. Aby průvodce vaše rozhodnutí, v následující tabulce porovnává možností podle ztrátou odhadovaná dat nebo obnovení přejděte cíle (operace RPO) a Předpokládaná doba obnovení (Vložit).

| Vzor | OPERACE RPO | VLOŽIT
| :--- |:--- | :---
| Aktivní pasivní nasazení havárie obnovení s spoluvytváření uložena databáze aplikace access | Přístup pro čtení a zápis < 5 sec | Čas zjištění chyby + rozhraní API převzetí volání aplikace ověření test
| Aktivní nasazení aplikace Vyrovnávání zatížení | Přístup pro čtení a zápis < 5 sec | Čas zjištění chyby převzetí rozhraní API volání + připojovací řetězec SQL změnit + aplikace ověření test
| Aktivní pasivní nasazení uchovávání dat | Jen pro čtení < 5 s přístup pro čtení a zápis = nula | Jen pro čtení = připojení Chyba při zjišťování čas + ověřovací test aplikace <br>Přístup pro čtení a zápis = čas zmírnit výpadek

## <a name="next-steps"></a>Další kroky

- Další informace o databáze SQL Azure automatické zálohy, najdete v článku [automatické zálohy databáze SQL](sql-database-automated-backups.md)
- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)
- Další informace o použití zálohy pro automatické obnovení najdete v tématu [obnovení databáze ze zálohy spuštěná služba](sql-database-recovery-using-backups.md)
- Další informace o rychlejší možnosti obnovení najdete v tématu [Aktivní Geo replikace](sql-database-geo-replication-overview.md)  
- Další informace o použití zálohy pro automatické archivace najdete v tématu [kopie databáze](sql-database-copy.md)
- Informace o aktivní Geo replikace pružná fondy najdete v tématu [Strategie obnovení havárie pružná fondu](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
