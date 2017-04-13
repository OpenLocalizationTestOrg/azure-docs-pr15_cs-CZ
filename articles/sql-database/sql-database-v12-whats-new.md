<properties
    pageTitle="Co je nového v SQL databázi V12 | Microsoft Azure"
    description="Popisuje, proč bude využívat upgradu na verzi V12 systémy firmy, které používají databáze SQL Azure v cloudu."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="genemi"/>


# <a name="whats-new-in-sql-database-v12"></a>Co je nového v SQL databázi V12


Toto téma popisuje mnoho výhody, které má nový V12 verzi databáze SQL Azure přes verzi V11.


Budeme dál přidávat funkce k V12. Proto doporučujeme navštivte naše webové stránky aktualizací služeb pro Azure a použití filtrů:


- Filtrované [služby SQL databáze](https://azure.microsoft.com/updates/?service=sql-database).
- Filtrované všeobecně dostupná [(GA) oznámení](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) týkající se funkcí databáze SQL.


Nejnovější informace o limitech zdroje pro databáze SQL uvedených na:<br/>[Limity prostředků databáze azure SQL](sql-database-resource-limits.md).


## <a name="increased-application-compatibility-with-sql-server"></a>Lepší aplikaci kompatibilní se SQL serveru


Klíčové hledání pro SQL databáze V12 bylo vylepšení kompatibility s Microsoft SQL Server 2014 a udržovat kompatibilitu vydána nová verze SQL serveru. Mezi jiné oblasti dosahuje V12 dostupná se serverem SQL Server důležité oblasti programovacích. Příklad:

- [Předdefinované JSON podpory](https://msdn.microsoft.com/library/dn921897.aspx)

- [Okno funkcí](http://msdn.microsoft.com/library/ms189798.aspx), pomocí [myši](http://msdn.microsoft.com/library/ms189461.aspx)

- [Indexy XML](http://msdn.microsoft.com/library/bb934097.aspx) a [Výběrové indexy XML](http://msdn.microsoft.com/library/jj670104.aspx)

- [Sledování změn](http://msdn.microsoft.com/library/bb933875.aspx)

- [VYBERTE... DO](http://msdn.microsoft.com/library/ms188029.aspx)

- [Hledání v celém textu](http://msdn.microsoft.com/library/ms142571.aspx)

- [ZMĚNIT OBOR konfigurace databáze (Transact-SQL)](http://msdn.microsoft.com/library/mt629158.aspx)

Najdete [tady](sql-database-transact-sql-information.md) kratších seznamů funkce ještě není podporované v databázi SQL.


### <a name="compatibility-level-130"></a>Funkce pro kompatibilitu úroveň 130


> [AZURE.IMPORTANT] Spuštění v **červnu 2016**, *nově* vytvořený databází na V12 databáze SQL Azure mají úroveň kompatibility začátek 130, která odpovídá Microsoft SQL serveru 2016 JM.
> 
> Můžete použít `ALTER DATABASE YourDatabase SET COMPATIBILITY_LEVEL = 120` podle potřeby.
> 
> Databáze vytvořené před června 2016 nemají na úroveň svého kompatibility měnit tato změna výchozí. Ani úroveň databáze měnit upgradu z V11 na V12.



Vysvětlení, jak je možné porovnávat dotazech nejdůležitější mezi nejnovější oproti předchozím úroveň kompatibility najdete v článku:

- [Vylepšené dotazu výkonu s úroveň kompatibility 130 v databázi Azure SQL](sql-database-compatibility-level-query-performance-130.md)



## <a name="more-premium-performance-new-performance-levels"></a>Další premium výkonu, nové úrovně výkonu


V V12 jsme zvyšuje jednotek databázové transakce (DTUs) přidělit všechny úrovně výkonu Premium hodnotou 25 % bezplatně. Ještě větší zlepšení výkonu můžete dosáhnout s novými funkcemi jako:


- Podpora v paměti [columnstore indexy](http://msdn.microsoft.com/library/gg492153.aspx).
- [Tabulka rozdělení řádků](http://msdn.microsoft.com/library/ms187802.aspx) s související vylepšení [Tabulky vymazat data](http://msdn.microsoft.com/library/ms177570.aspx).
- Dostupnost Správa dynamických zobrazení [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx) umožňuje sledovat a ladění výkonu.


### <a name="reliable-performance"></a>Spolehlivé výkonu


Pokud se klientským programem připojuje k SQL databázi V12 v průběhu svému klientovi na Azure virtuálního počítače (OM), musíte otevřít následující rozsahy portů na OM:

- 11000 11999
- 14000 14999


Klikněte na [zde](sql-database-develop-direct-route-ports-adonet-v12.md) podrobnosti o porty V12 databáze SQL. Porty jsou vyžadovány zvýšení výkonu v SQL databázi V12.


## <a name="better-support-for-cloud-saas-vendors"></a>Lepší podpora pro cloudu SaaS dodavatelé


Pouze v V12 vydala novou úroveň Standardní výkonu S3 a veřejné Náhled [fondů pružná databáze](sql-database-elastic-pool.md). Pružná databáze se, že sice řešení pro cloudu SaaS Dodavatelé.  Pružná databáze fondy máte tyto možnosti:


- Sdílení DTUs mezi databází ke snížení nákladů na vysoké počty databází.
- Spusťte [úlohy pružná databáze](sql-database-elastic-jobs-overview.md) pro správu databáze ve velkém měřítku.


## <a name="security-enhancements"></a>Zvýšení zabezpečení


Zabezpečení je především pro všechny, kdo pracuje své firmě v cloudu. Nejnovější funkce zabezpečení vydána v V12 patří:


- [Řádek úrovně zabezpečení](http://msdn.microsoft.com/library/dn765131.aspx) (RLS)
- [Maskování dynamických dat](sql-database-dynamic-data-masking-get-started.md)
- [Obsažené databází](http://msdn.microsoft.com/library/ff929188.aspx)
- Spravovat [role aplikací](http://msdn.microsoft.com/library/ms190998.aspx) pomocí GRANT, ODMÍTNOUT, odvolání
- [Šifrování průhledné dat](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) (TDE)
- [Připojení k databázi SQL Azure Active Directory ověřováním](sql-database-aad-authentication.md)
 - Databáze SQL Azure Active Directory authentication mechanismus připojení k databázi SQL pomocí identit v Azure Active Directory (Azure AD) nyní podporuje. S Azure Active Directory authentication můžete centrální správy identit uživatelů databáze a další služby od Microsoftu na jednom centrálním místě.
- [Vždy zašifrovaných](https://msdn.microsoft.com/library/mt163865.aspx) (v náhledu) umožňuje šifrování průhledné aplikací a umožňuje klientům zašifrovat citlivá data v klientských aplikacích bez šifrovacího klíče nasdílet databáze SQL.


## <a name="increased-business-continuity-when-recovery-is-needed"></a>Lepší nepřerušený, když je potřeba obnovení


Odhad využití neustále (ERTs) a v12 nabízí Vylepšený obnovení čárky cíle (RPOs):


| Funkce kontinuitu podnikové | Starší verze | V12 |
| :-- | :-- | :-- |
| Obnovení GEO | • Operace RPO < 24 hodin.<br/>• Vložit < 12 hodin. | • Operace RPO < 1 hodinu.<br/>• Vložit < 12 hodin. |
| Aktivní Geo replikace | • Operace RPO < 5 minut.<br/>• Vložit < 1 hodinu. | • Operace RPO < 5 sekund.<br/>• Vložit < 30 sekund. |


Další informace najdete v tématu [nepřerušený provoz databáze SQL](sql-database-business-continuity.md) .


## <a name="more-reasons-to-upgrade-now"></a>Další důvodů, proč upgradovat


Existují mnoho dobré důvody, proč zákazníci měli upgradovat teď V12 databáze SQL Azure z V11:


- SQL databáze V12 má dlouhý seznam funkcí za funkce V11.
- Budeme dál přidávat nové funkce V12, ale žádné nové funkce se přidají do V11.
- Většina nových funkcí uvolnění na SQL databáze V12 před jejich vydání pro Microsoft SQL Server.


## <a name="are-you-using-v12-already"></a>Používáte V12 už?


Jeden snadný způsob, jak chcete zobrazit, pokud máte databázi nebo logické serveru v dřívější verzi aplikace služby SQL databáze je postupujte takto:


1. Přejděte na [portál Azure](https://portal.azure.com/).
2. Klikněte na **Procházet**.
3. Klikněte na položku **SQL servery**.
4. Ikona vedle serveru nebo databáze říká textu:
 - ![Ikona pro v12 server](./media/sql-database-v12-whats-new/v12_icon.png) **logické serveru V12**
 - ![Ikona pro starší verze serveru](./media/sql-database-v12-whats-new/earlier_icon.png) **starší verze logické server**


Jiný postup zjistit verzi je spustit `SELECT @@version;` následný výběr v databázi a zobrazit podobné výsledky:


- **12**.0.2000.10 &nbsp; *(verze V12)*
- **11**.0.9228.18 &nbsp; *(verze V11)*


V12 databáze může být hostitelem pouze V12 logické serveru. A V12 serveru můžete hostovat pouze V12 databází.


Pokud nejste ještě na V12, můžete upgradovat logické serveru pomocí kroků uvedených v části [Upgrade k SQL databázi V12 na místě](sql-database-v12-plan-prepare-upgrade.md).


## <a name="V12AzureSqlDbPreviewGaTable"></a>Obecné dostupnost oblastí


- Podle 31 červenec 2015 vám to byl všech oblastí propagované na obecný dostupnosti (GA).
- V12 byla vydána do prosince 2014, ale jenom na stav náhled.

[Doplňkové podmínky použití aplikace Microsoft Azure náhledy](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
