<properties
   pageTitle="Správa databází v Azure SQL datový sklad | Microsoft Azure"
   description="Základní informace o správě databáze SQL datový sklad. Obsahuje nástroje pro správu, DWUs a škálování výkon Poradce při potížích s výkonu dotazu, vytvoření zásad zabezpečení a obnovení databáze před poškozením dat nebo z místního výpadku."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Správa databází v Azure SQL datový sklad

SQL datový sklad automaticky aspektech správy databáze. Například zobrazit výkonu jenom potřebujete upravit zaplatit správnou úroveň využití prostředků a nechejte SQL datový sklad to postará rozšiřování a měřítka zpět. 

Jistě můžete sledovat vaše pracovní zátěž k určení potřeb výkonu i Poradce při potížích s časově náročné dotazy. Musíte se taky můžou provádět několik zabezpečení ke správě oprávnění pro uživatele a přihlášení.

Tento přehled zahrnuje tyto aspekty Správa SQL datový sklad.

- Nástroje pro správu
- Pro využití měřítko
- Pozastavení a obnovení
- Doporučené postupy výkonu
- Sledování dotazu
- Zabezpečení
- Zálohování a obnovení

## <a name="management-tools"></a>Nástroje pro správu

Různých nástrojů, které slouží ke správě databáze SQL datový sklad. Při správě databáze se naučíte předvoleb nástroje pro každý typ úkolu, které potřebujete provést.

### <a name="azure-portal"></a>Azure portálu
[Azure portál][] je webová portál kde můžete vytvořit, aktualizovat a odstraňte databáze a sledovat databáze prostředky. Tento nástroj je skvělý Pokud teprve začínáte s Azure, Správa malým počtem poštovních datový sklad databáze nebo potřebujete rychle udělat něco.

Začínáme s portálem Azure v tématu [vytvoření SQL datový sklad (Azure portál)][].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools ve Visual Studiu
[SQL Server Data Tools][] (SSDT) ve Visual Studiu umožňuje připojit, Správa a vývoj databáze. Pokud jste vývojář aplikace zkušenosti s Visual Studio nebo jiná integrované vývojové prostředí (IDEs), zkuste použít SSDT ve Visual Studiu.

SSDT obsahuje Průzkumník objektu SQL serveru, která umožňuje vizualizovat, připojení a spustit skripty databázích SQL datový sklad. Pokud chcete rychle připojit k SQL datový sklad, jednoduše kliknete na tlačítko **Otevřít v aplikaci Visual Studio** v panelu příkazů při prohlížení databáze podrobností portálu klasické Azure.  

Začínáme s SSDT ve Visual Studiu, najdete v článku [Dotazu Azure SQL datový sklad s Visual Studio][].

### <a name="command-line-tools"></a>Nástroje příkazového řádku
Přepínač příkazového řádku nástroje jsou ideální pro automatizaci svého pracovního vytížení.  Prostředí PowerShell a sqlcmd dvěma způsoby skvělé automatizace procesů.  Doporučujeme, abyste tyto nástroje pro správu velkého počtu logické servery a nasazení zdroje změn v prostředí výrobní podle potřeby úkoly můžete skriptovány a potom automatické.

### <a name="dynamic-management-views"></a>Správa dynamických zobrazení 

DMVs jsou másla pečivo a správy SQL datový sklad. Skoro všechny informace, které plochy na portálu závisí na DMVs. Pokud chcete zobrazit seznam SQL datový sklad DMVs, zobrazení [SQL datový sklad systému][].

Pokud chcete začít pracovat, najdete v článku [připojení a dotaz se sqlcmd][]a [vytvořit databázi (Powershellu)][].

## <a name="scale-compute"></a>Pro využití měřítko

V SQL datový sklad můžete rychle změnit velikost výkonu, nebo zpět zvětšení nebo zmenšení výpočetním zdrojů procesoru, paměti a šířky pásma vstupu a výstupu. Zobrazit výkonu, je třeba udělat stačí upravte číslo skladové jednotky dat (DWUs) SQL datový sklad přidělí do vaší databáze. SQL datový sklad rychle provede změny a úchyty pro všechny základní změny hardware a software.

Další informace o měřítka DWUs najdete v tématu [výkon měřítko][].

##  <a name="pause-and-resume"></a>Pozastavení a obnovení

Snížení nákladů na, můžete pozastavit a obnovit výpočetním zdroje na vyžádání. Například pokud nebudete používat databázi v noci a o víkendech, můžete pozastaví tyto časy a obnovit během dne. Během databáze se pozastaví se nezapočítávají pro DWUs.

Další informace najdete v tématu [Pozastavit výpočet][]a [Výpočet životopisu][].

## <a name="performance-best-practices"></a>Doporučené postupy výkonu

Pokud začínáte pracovat s novou technologii, objevují ostatní tipy a triky, které fungují nejlepší vpravo od začátku můžete uložit je značné množství času.  Doporučené postupy v celém spoustu naše témata bude hledat.

Mnoho souhrn všech nejdůležitější co byste měli zvážit při vytváření vašeho pracovního vytížení najdete [SQL datový sklad doporučené postupy][].

## <a name="query-monitoring"></a>Sledování dotazu

Někdy je spuštěný dotaz moc dlouhá, ale ještě nejste si jistí které z nich je který. SQL datový sklad má Správa dynamických zobrazení (DMVs), které můžete použít k určení prodejce, dotazu, který trvá příliš dlouho. 

Dlouho probíhajících dotazech najdete v části [Sledování vaše pracovní zátěž pomocí DMVs][].

## <a name="security"></a>Zabezpečení

Abyste zachovali zabezpečeného systému, musí být na upozornění a chránit před libovolný typ neoprávněnému přístupu. Systém zabezpečení musí zkontrolujte, jestli pravidla brány firewall na místě, protože pouze oprávnění IP adresy se můžete připojit. Musí být správné ověřování přihlašovací údaje uživatele. Poté, co uživatel má připojení k databázi, uživatel měli oprávnění k provádění minimální počet akcí. K zabezpečení dat, můžete použít šifrování. Je důležité, abyste měli auditování a sledování tak události můžete vrátit, pokud existuje podezřelé aktivity.

Informace o správě zabezpečení, vedoucí myší na [Přehled zabezpečení][].

## <a name="backup-and-restore"></a>Zálohování a obnovení

Spolehlivé backps dat je důležitou součástí jakékoli výrobní databáze. SQL datový sklad udržuje data bezpečných tak, že automaticky zálohování aktivní databází v pravidelných intervalech. Tyto zálohování umožňují obnovení z situací, které jste poškození dat nebo omylem přetažení dat nebo databáze.  Plán zálohování dat, zásady uchovávání informací a obnovení databáze, najdete v tématu [obnovení ze snímku][].

## <a name="next-steps"></a>Další kroky
Použití dobrý návrh databáze, že zásady budou snadněji spravovat databáze v SQL datový sklad. Další informace, vedoucí myší na [Přehled vývoje][].

<!--Image references-->

<!--Article references-->
[Vytvoření SQL datový sklad (Azure portál)]: sql-data-warehouse-get-started-provision.md
[Vytvoření databáze (Powershellu)]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Dotaz Azure SQL datový sklad aplikace Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Připojení a dotaz s sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Přehled vývoje]: sql-data-warehouse-overview-develop.md
[Sledovat vaše pracovní zátěž pomocí DMVs]: sql-data-warehouse-manage-monitor.md
[Pozastavit výpočetním]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Obnovení ze snímku]: sql-data-warehouse-restore-database-overview.md
[Pro využití životopisu]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Měřítko výkonu]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Přehled zabezpečení]: sql-data-warehouse-overview-manage-security.md
[SQL datový sklad doporučené postupy]: sql-data-warehouse-best-practices.md
[Zobrazení SQL datový sklad systému]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portálu]: http://portal.azure.com/
