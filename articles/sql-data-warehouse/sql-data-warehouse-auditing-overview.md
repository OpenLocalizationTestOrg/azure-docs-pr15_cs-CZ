<properties
   pageTitle="Sestavy auditování v Azure SQL datový sklad | Microsoft Azure"
   description="Začínáme s auditování v Azure SQL datový sklad"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="auditing-in-azure-sql-data-warehouse"></a>V Azure SQL datový sklad sestavy auditování

> [AZURE.SELECTOR]
- [Sestavy auditování](sql-data-warehouse-auditing-overview.md)
- [Zjišťování hrozbou, že](sql-data-warehouse-security-threat-detection.md)

Sestavy auditování SQL datový sklad umožňuje záznam, který události v databázi s auditem protokolu ve vašem úložišti Azure účtu. Sestavy auditování můžete udržovat dodržování předpisů, pochopit aktivita v databázi a získat přehled o nesrovnalosti a odchylky, které znamenat firmy pochybnosti nebo podezřelé porušení zabezpečení. Sestavy auditování SQL datový sklad taky lze integrovat s Microsoft Power BI pro vytváření sestav a analýzy přechodu na podrobnosti.

Nástroje závislostí povolit a usnadnit dodržování organizace pro standardy dodržování předpisů, ale není zaručit dodržování předpisů. Další informace o Azure programy, které podporují standardy dodržování předpisů najdete v tématu <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Centrum zabezpečení Azure</a>.

+ [Informace o databázích auditování]
+ [Nastavení auditování pro databázi]
+ [Analýza protokolů auditování a sestavy]

##<a id="subheading-1"></a>Základy Azure SQL datový sklad databáze auditování


Sestavy auditování databáze SQL datový sklad umožňuje:

- **Zachovat** revizní vybraných událostí. Můžete definovat kategorie databáze akce, které mají být auditovány.
- **Sestava** aktivita v databázi. Předkonfigurované sestavy a řídicí panel můžete začít rychle pracovat s aktivity a vytváření sestav funkce události.
- **Analýza** sestavy. Můžete najít podezřelé události, neobvyklé aktivity a trendy.

Můžete nakonfigurovat auditování pro kategorii následujících událostí:

**Prostý SQL** a **s parametry SQL** , u kterého shromážděných auditování zaznamenává jsou klasifikované jako  

- **Přístup k datům**
- **Změny schématu (DDL)**
- **Změny dat (DML)**
- **Účty, role a oprávnění (DCL)**
- **Uložená procedura**, **přihlášení** a **správě transakcí**.

Pro každou kategorii událostí auditování **úspěšné** a **selhání** operace nakonfigurovaná samostatně.

Další informace o aktivit a událostí auditovány najdete v článku <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Odkaz formát protokolu auditování (stahování souboru dokumentu)</a>.

Protokolů auditování jsou uložené ve vašem účtu Azure úložiště. Můžete definovat období uchovávání protokolu auditování.

Možné definovat zásad auditování pro určité databáze nebo jako výchozí zásady serveru. Výchozí auditování zásady serveru bude platit pro všechny databáze na serveru, které nemají žádné zvláštní přepsání databáze auditování zásady definované.

Před nastavení auditování nahoru auditování zaškrtněte, pokud používáte ["starších klientů"](sql-data-warehouse-auditing-downlevel-clients.md).


##<a id="subheading-2"></a>Nastavení auditování pro databázi

1. Spusťte <a href="https://portal.azure.com" target="_blank">Azure portálu</a>.

2. přejděte na zásuvné konfigurační databáze SQL datový sklad / SQL serveru, které mají být auditovány. Klikněte na tlačítko **Nastavení** na začátku a potom v zásuvné nastavení a vyberte **auditování**.

    ![][1]

3. V auditování zásuvné konfigurace nejprve zrušit zaškrtnutí položky zaškrtávací políčko **Zdědit nastavení auditování ze serveru** . To vám umožní určit nastavení pro konkrétní databázi.

    ![][2]

4. Povolte pak auditování kliknutím **na** tlačítko.

    ![][3]

5. V auditování zásuvné konfigurace vyberte **Podrobnosti úložiště** otevřete zásuvné úložiště protokolů auditování. Vyberte místo, kam protokolů uložily účet Azure úložiště a doba uchovávání informací. **Tip:** Použijte stejný účet úložiště pro všechny ověřené databáze k maximálnímu využití šablony předkonfigurované sestavy.

    ![][4]

6. Klikněte na tlačítko **OK** uložte konfiguraci podrobností úložiště.


7. V části **Protokolování tak, že události**klikněte **úspěšné** a **NEÚSPĚŠNÉ** protokolovat všechny události nebo klikněte na jednu událost kategorie.


8. Pokud konfigurujete auditování pro danou databázi, budete muset změnit připojovací řetězec klientem a zajistěte, aby data auditování se správně zaznamenává. Zaškrtněte políčko v tématu [Úprava FDQN serveru v připojovacím řetězci](sql-data-warehouse-auditing-downlevel-clients.md) k připojení klientů starších verzí.

9. Klikněte na **OK**.


##<a id="subheading-3">Analýza protokolů auditování a sestavy</a>

Protokolů auditování agregován v kolekci úložiště tabulek s předponou **SQLDBAuditLogs** v Azure úložiště účet, který jste zvolili při instalaci. Můžete zobrazit pomocí nástroje, například <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure úložiště Průzkumníka</a>souborů protokolu.

Šablona sestavy předkonfigurované řídicího panelu je k dispozici <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">ke stažení Excelovou tabulku</a> k rychlé analýze dat protokolu. Pokud chcete použít šablonu na protokolů auditování, budete potřebovat aplikaci Excel 2013 nebo novější a Power Query, kterou si můžete stáhnout <a href="http://www.microsoft.com/download/details.aspx?id=39379">tady</a>.

Šablona má fiktivní ukázková data v něm a Power Query můžete nastavit pro import protokolu auditování přímo z vašeho účtu Azure úložiště.

Další podrobné informace o práci s šablony sestavy najdete v části <a href="http://go.microsoft.com/fwlink/?LinkId=506731">Jak k (ke stažení dokumentu)</a>.

![][5]


##<a id="subheading-4">Postupy pro použití v výrobní</a>
Popis v této části odkazuje na obrazovce zachycení výše. <a href="https://portal.azure.com" target="_blank">Portál Azure</a> nebo <a href= "https://manage.windowsazure.com/" target="_bank">Klasická Azure klasické portál</a> se může použít.


##<a id="subheading-5"></a>Obnovování klíčů úložiště

Ve výrobním budete pravděpodobně pravidelně aktualizovat úložiště klíče. Při aktualizaci vašich klíčů budete muset znovu uložte zásadu. Proces je takto:.


1. V auditování konfigurace zásuvné (jsme je popsali výše ve skupině nastavení auditování oddíl) přepněte **Úložiště přístupová klávesa** z *primární* *sekundární* a **Uložit**.
![][4]
2. Přejděte na zásuvné konfigurace úložiště a **Obnovit** *Primární klíč přístup*.

3. Přejděte zpátky do auditování zásuvné konfigurace přepnout **Úložiště přístupová klávesa** z *sekundární* *primární* a stiskněte klávesu **Uložit**.

4. Přejděte zpět do úložiště uživatelského rozhraní a **Obnovit** *Sekundární přístupové klávesy* (jako příprava další aktualizace cyklus klíče.

##<a id="subheading-6"></a>Automatizace
Existuje několik rutinách Powershellu, které můžete použít pro nastavení auditování v databázi SQL Azure. Přístup k auditování rutiny se musí běžet Powershellu v režimu správce prostředků Azure.

> [AZURE.NOTE] Modul [Azure správce prostředků](https://msdn.microsoft.com/library/dn654592.aspx) momentálně v náhledu. Dát pravděpodobně stejné možnosti správy jako modul Azure.

Pokud jste v režimu správce prostředků Azure, spusťte `Get-Command *AzureSql*` seznam dostupných rutiny.


<!--Anchors-->
[Informace o databázích auditování]: #subheading-1
[Nastavení auditování pro databázi]: #subheading-2
[Analýza protokolů auditování a sestavy]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
