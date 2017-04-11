<properties
   pageTitle="Principy faktuře | Microsoft Azure"
   description="Zjistěte, jak čtení a interpretaci použití a účtu u předplatného Azure"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/31/2016"
   ms.author="erihur;genli"/>


# <a name="understand-your-bill-for-microsoft-azure"></a>Vysvětlení informací na faktuře za Microsoft Azure

> [AZURE.NOTE] Pokud potřebujete další pomoc kdykoli v tomto článku najdete [kontaktovat podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat problém vyřešit rychle.

Poplatky za Microsoft Azure předplatného se liší podle plánu sazba. Některé sazba plánů, například předplatitele Visual Studio Enterprise (MPN) zahrnuje měsíční přeplatky, které můžete použít na všechny Azure služeb na základě potřeb.

Všimněte si, že si 24 hodin skryté použití od předchozího fakturační období můžete vykázat za aktuální fakturační období.

Další informace o spotřebu a sazba plány najdete v článku [Možnosti nákupu Microsoft Azure stránky](https://azure.microsoft.com/pricing/purchase-options/).

<!-- The below links cover a complete list of all Microsoft Azure services.

<!-- - [Service Details list (csv1)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
<!-- - [Service Details list (csv2)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)

<!-- *NOTE: The **csv1** link refers to the column header names for csv version 1 and **csv2** link refers to the new column header names for csv version 2.  These files are updated monthly.*-->

### <a name="view-or-download-a-bill-for-microsoft-azure"></a>Zobrazení nebo stažení směnky pro Microsoft Azure:

1. Přihlaste se k [Účtu Centrum](https://account.windowsazure.com/subscriptions) pomocí účtu Microsoft Account nebo ID organizace.

2. Klikněte na předplatné, ve kterém chcete zobrazit podrobnosti a použití.

3. Klikněte na **Historie fakturace**

    ![Shrnutí – historie -1 fakturace](./media/billing-understand-your-bill/ContentViewaBillforMA1.png)

4. V části **Historie fakturace** uvádí váš příkazy pro předchozí fakturační období plus stávající nefakturovaný období. Příkaz za aktuální období je odhad nákladů od okamžiku, kdy byl vytvořen odhad. Tyto informace pouze aktualizovanou denně a nesmí obsahovat všechny použití vzniklé k aktuálnímu datu. Měsíční najdete na faktuře se může lišit od tohoto odhadu.  

    ![Historie fakturace souhrn 2](./media/billing-understand-your-bill/ContentViewaBillforMA2.png)

5. Klikněte na **Zobrazit aktuální prohlášení** zobrazíte odhad nákladů od okamžiku, kdy byl vytvořen odhad. Tyto informace pouze aktualizovanou denně a nesmí obsahovat všechny použití vzniklé k aktuálnímu datu. Měsíční najdete na faktuře se může lišit od tohoto odhadu.

    ![Historie fakturace souhrn 3](./media/billing-understand-your-bill/ContentViewaBillforMA3.png)

    ![Historie fakturace souhrn 4](./media/billing-understand-your-bill/ContentViewaBillforMA4.png)

6. Klikněte na tlačítko **Stáhnout faktury** zobrazit kopii vaší předchozí faktury.

    ![Historie fakturace souhrn 5](./media/billing-understand-your-bill/ContentViewaBillforMA5.png)


> [AZURE.NOTE] Náklady uvedená na fakturace příkazy pro mezinárodní zákazníci jsou pro účely odhad pouze bank mají různé náklady pro převod sazby.

Zde jsou některé ukázkové příkazy pro dvě různé nabídek dostupných na Microsoft Azure.

 Nabídka Typ | Popis | Ke stažení |
 :--------- |:-------- | :-------|
Přislíbený | Platíte měsíčně zpětně | [Ukázkový soubor](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_ccinvoice_Sample.pdf)
Nabídka potvrzení | Stráví odečtená od předplacený potvrzení | [Ukázkový soubor](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_invoice_Sample.pdf)

## <a name="account-information"></a>Informace o účtu

Oddíl informace o účtu identifikuje relevantní informace týkající se použití a profilu.

![záhlaví](./media/billing-understand-your-bill/Header.png)

| Termín                | Popis                                                                                         |
|---------------------|-----------------------------------------------------------------------------------------------------|
| Číslo faktury         | Faktury jedinečný identifikátor pro účely sledování                                                   |
| Fakturace obrázku       | Časový rámec, ve kterém použití proběhla                                                       |
| Datum faktury        | Datum, které vygenerovalo faktury                                                                 |
| Způsob platby      | Typ platby na účtu (fakturou nebo kreditní kartou)                                   |
| Pro fakturaci             | Adresa platby Microsoft Azure                                                                    |
| Nabídku předplatného  | Typ nabídku předplatného, kterou jste si koupili (přislíbený BizSpark Plus, průchodu Azure, atd.) |
| Účet Vlastník e-mailu | Účet e-mailovou adresu registrovanou v části účet Microsoft Azure                      |

## <a name="understand-the-invoice-summary"></a>Princip souhrn faktury

Oddíl **Faktury souhrn** směnky shrnuje transakce od vaší poslední faktury a aktuální poplatky za použití.

![Souhrn faktury](./media/billing-understand-your-bill/InvoiceSummary.png)

Předchozí zůstatek, plateb a zůstatek část rozpisu shrnuje transakce od vaší poslední faktury.

| Termín                                              | Popis                                                                              |
|---------------------------------------------------|------------------------------------------------------------------------------------------|
| Předchozí zůstatek                                  | Splatná částka celkem od vaší poslední faktury                                                 |
| Platby                                          | Celkové platby u vaší poslední faktury                                                 |
| Zůstatek (z předchozí fakturačním cyklu) | Faktury úpravy (přeplatky nebo zůstatků) použitý k vašemu účtu od vaší poslední faktury  |

## <a name="understand-the-current-charges"></a>Princip aktuálních poplatků
V části aktuální poplatky rozpisu obsahuje podrobnosti o měsíční poplatky. Propojení jsou uspořádány do následujících pododdílu.

| Termín          | Popis                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Poplatky za použití | Poplatky za použití jsou měsíční celkové náklady na předplatné. Je faktura zpětně pro použití uplynulém měsíci.                                                                                                                                                                                                                                                                                                                                       |
| Slevy     | Služba slevy použité na aktuální najdete na faktuře se projeví v tomto řádkové položky.                                                                                                                                                                                                                                                                                                                                              |
| Úpravy   | Různé úpravy jsou různé přeplatky nebo zbývající poplatků faktuře aktuální. Například pokud máte Visual Studio Enterprise nabídnout MSDN, uvidí měsíční platební v tuto položku řádku. Pokud chcete předplatné zrušit, uvidí náklady na měsíční použití více než měsíční platební součástí vaši nabídku od začátku aktuální fakturační období datum zrušení předplatného.|

## <a name="footer-information"></a>Informace ze zápatí
![zápatí](./media/billing-understand-your-bill/footerinformation.png)

## <a name="understand-the-additional-information"></a>Vysvětlení doplňkové informace
Další informace stránka poskytuje odkazy na jiné zdroje porozumět faktury a odkazy na zobrazení používání a další relevantní informace u vašeho účtu.

![Další informace](./media/billing-understand-your-bill/AdditionalInformation.png)

### <a name="detailed-usage"></a>Podrobné použití
Odkaz v popisu v části **Podrobné použití** vás přesměruje centra účet, kde můžete zobrazit podrobné použití pro toto předplatné.  Jsou u jablek dvě verze k dispozici ke stažení: **.csv verze 1** obsahuje pole staré pojmenování a použití **.csv verze 2** obsahuje zákazníka popisných názvů pro každou kategorii a dalších polí, které vám pomohou porozumět tomu, co služby používají na Microsoft Azure. Všimněte si, že v souboru .csv verze 1, že nejsou žádné podrobnosti Správce prostředků Azure. Azure správce prostředků informace najdete v souboru .csv verze 2.

### <a name="additional-information-and-useful-resources"></a>Další informace a užitečné zdroje
Tato část obsahuje odkazy na jednoduché otázky týkající se výpočetním instance velikosti, poplatky SQL databáze a užitečné odkazy týkající se dalších zodpovědět.

| Termín                 | Popis                                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Prodané k              | To je zaplní adresu profilu účtu                                                                           |
| Pokyny pro platbu | Tento oddíl je pokyny pro platbu kam chcete odeslat kontroly, drátěný přenosy nebo přes noc kontroly, pokud je váš způsob platby fakturou |

## <a name="understand-detailed-usage-charges"></a>Princip podrobné nadbytečným poplatkům

Jako součást projektech průběžné pro zákazníky snadno spravovat jejich Azure použití jsme jste rozšířeného stažený soubor použití, který sestavy o využití služby Azure a náklady.  Odkaz ke stažení obsahuje dvě verze souboru použití:

- **Verze 1** ve formátu stávajících

- **Verze 2** obsahuje další informace a aktualizovat názvy sloupců v části denní použití.  

Poplatky za použití jsou celkové **měsíční** poplatky předplatného méně kreditní nebo sleva. Je faktura zpětně pro použití uplynulém měsíci.  Horní část souboru zobrazí podrobnosti o služby, které je právě faktura pro během fakturačním cyklu na předchozí měsíc.  V předchozí tabulce jsou uvedeny názvy sloupců pro každý z verze souborů CSV.

Verze 1 |  Verze 2  |  Popis|
:---------------| :---------------- | --------|
Fakturační období | Fakturační období | Fakturační období, kdy byla spotřebované množství zdroje.
Jméno | Kategorie metr | Určuje služby nejvyšší úrovně, pro který toto využití patří.
Typ | Podkategorie metr | Služby Azure může další definované podle typu v tomto sloupci, což může ovlivnit sazbu.
Zdroje | Název metr | Určuje měrnou jednotku zdroje je spotřebované množství.
Oblast | Oblast metr | Určuje umístění datacentra pro určité služby, které jsou účtovány podle datacentra místa.
SKU | SKU | Určuje jedinečný systémový identifikátor pro jednotlivé Azure zdroje.
Jednotky | Jednotky | Označuje jednotku, které bude účtováno službu v. Například GB, hodiny, 10,000s.
Spotřebované množství | Spotřebované množství | Obsahuje částku prostředek, který má spotřebované fakturační období.
Součástí | Součástí množství | Obsahuje částku prostředek, který je součástí bez poplatků za aktuální fakturační období.
Fakturaci | Průměrných množství | Pokud množství spotřebováno přesáhne však započítávány, tento sloupec zobrazuje rozdíl. Je faktura pro tuto hodnotu. Pro přislíbený nabídky obsahovat žádné hodnotu zahrnutý v sadě nabídky tento součet budou shodovat s názvem spotřebováno množství.
V rámci potvrzení | V rámci potvrzení | Obsahuje náklady zdroje, které jsou snížena z vašich potvrzení částek přidružený k vaší 6 nebo 12 měsíců nabídky. Vaše platby zdroje jsou snížena od množství potvrzení seřazených chronologicky.
Měny | Měny | Určuje měny promítnou i ve vaší aktuální fakturační období.
Nadsazení | Nadsazení | Obsahuje náklady zdroje, které překročit vašich potvrzení částek přidružený k vaší 6 nebo 12 měsíců nabídky.
Potvrzení sazba | Potvrzení sazba | Obsahuje kurz potvrzení založený na vaší částka celkem potvrzení přidružený k vaší 6 nebo 12 měsíců nabídky.
Sazba | Sazba | Sazba zobrazuje sazbu, který vám bude účtovaná za fakturaci jednotku.
Hodnota | Hodnota | Zobrazí výsledek vynásobí fakturaci sloupce podle sloupce sazba. Pokud spotřebováno nedosahuje však započítávány množství, bude zdarma v tomto sloupci.

## <a name="analyze-daily-usage-data"></a>Analýza denní použití zásad správy informací
V závislosti na vaší použití může být tisíce řádků dat s denními použití. Pokud chcete k analýze dat, klikněte na **Stáhnout použití** a vybrat verzi proměnné souboru čárkami (.csv) budu zobrazíte denní použití dat pro příslušný fakturační období.  Jako odkaz můžete si stáhnout ukázkový soubor CSV pro každou verzí dole.

 Jméno | Ke stažení |
 :----------:| :-------: |
  Podrobné použití .csv verze 1|  [Ukázkový soubor](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
  Podrobné použití .csv verze 2 | [Ukázkový soubor](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)



![csv2screenshot](./media/billing-understand-your-bill/csv2screenshot.png)



V souboru .csv položky jsou rozdělené podle zobrazit přehled o tom, jaká část každému zdroji byla spotřebované množství v rámci aktuální fakturační období.

![snímek CSV](./media/billing-understand-your-bill/csvsnapshotportal.png)

Následující sloupce zobrazte podrobnosti, které ovlivňují sazby na začátku fakturační období:

Verze 1 |   Verze 2   |  Popis |
:---------------| :----------------| -----|
Použití kalendářních dat | Použití kalendářních dat | Datum, kdy byla vyzařovaného zdroje.
Jméno | Kategorie metr | Určuje služby nejvyšší úrovně, pro který toto využití patří.
GUID zdroje | ID měřiče | Identifikátor fakturované metr.  Používá se jako identifikátor používaný k cena fakturační použití.
Typ | Podkategorie metr | Služby Azure může další definované podle typu v tomto sloupci, což může ovlivnit sazbu.
Zdroje | Název metr | Určuje měrnou jednotku zdroje je spotřebované množství.
Oblast | Oblast metr | Určuje umístění datacentra pro určité služby, které jsou účtovány podle datacentra místa.
Jednotky | Jednotky | Označuje jednotku, které bude účtováno službu v. Například GB, hodiny, 10,000s.
Spotřebované množství | Spotřebované množství | Obsahuje částku prostředek, který má spotřebované pro daný den.
Oblast Sub | Umístění zdroje | Určuje datacentru, kde je spuštěný zdroje.
Služba | Spotřebované služby | V tomto sloupci využít ke sledování jednotlivé Azure platformu služba, která nemusí být konkrétně uveden ve sloupci Název. Se vztahuje tato služba, sloupec označuje, jaké zvláštní služby použití.
NENÍ K DISPOZICI | Pole Skupina zdroje | _**Přidání nového sloupce.**_ Skupina zdroje, ve kterém nasazeném zdroje běží. Podívejte se na [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md)
Součásti | Instance ID | Identifikátor pracovního zdroje. Identifikátor obsahuje název, který určíte zdroje, když byl vytvořen.
NENÍ K DISPOZICI | Značky | _**Přidání nového sloupce.**_ Nové typy zdrojů v Azure vám umožní značku zdroje. V nápovědě k [uspořádání Azure zdroje se značkami](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/)
Další informace | Další informace | Další metadata vztahující se ke službě.
Informace o službě 1 | Informace o službě 1 | Tento sloupec obsahuje název projektu, který službu patří do vašeho předplatného.
Informace o službě 2 | Informace o službě 2 | Toto je starší verze pole, který popisuje volitelné specifických pro službu metadata.

Kromě některých nových polí a název se změní na csv verze 2 tam bude platí formátování dat v pod pole:

- **Instance ID**: uživatel zadal identifikátor pro službu zřízení představuje pole Instance ID. V současné době existují dva formáty, které představuje Instance ID: je název zdroje, nebo plně kvalifikovaný číslo ID zdroje. Jsou služby Microsoft Azure přechodem na představuje ID Instance ve formátu standardizovaným pole číslo ID zdroje plně kvalifikovaný _**(/subscriptions/<subscription id>/resourcegroups/<resourcegroupname>/providers/<providername>/<resourcename>)**_. Jak služba přechodu na nový formát vidíte datové pole ID instanci změnit pouze název zdroje na číslo ID zdroje. Číslo ID zdroje je formát používaný rozhraní [API Správce prostředků Azure](https://msdn.microsoft.com/library/azure/dn790567.aspx) k identifikaci zdroje v předplatné.

![ID instance](./media/billing-understand-your-bill/instanceid.png)

- **Další informace**: Další informace o sloupce v souboru .csv použití určuje specifických pro službu metadat. Například typ obrázku pro virtuálního počítače. V současné době službu posílá specifických pro službu metadat ve více sloupcích: Další informace, Info1 služby a služby informace 2. Služby Microsoft Azure bude standardizaci výstupu specifických pro službu metadat ve sloupci Další informace.  V tématu pod snímky standardní formát:

![additionalinfo_csv2](./media/billing-understand-your-bill/AdditionaInfo_csv2.png)

- **Značky**: Tento sloupec obsahoval uživatele zadaného značky zdroje. Značky mohou sloužit k seskupení fakturační záznamy. Například můžete značky distribuce nákladů podle oddělení pomocí služby. Další informace o [používání značek k uspořádání Azure zdroje](../resource-group-using-tags.md). Jsou služby, které podporují vysílání značky:  

    - Virtuálních počítačích

    - Ukládání a

    - Síť služby zřízení pomocí rozhraní [API Správce prostředků Azure](https://msdn.microsoft.com/library/azure/dn790567.aspx)

![značky](./media/billing-understand-your-bill/tags.png)


## <a name="next-steps"></a>Další kroky

- [Nastavení fakturace upozornění](../billing-set-up-alerts.md)

- [Spravovat své způsoby platby](../billing-how-to-change-credit-card.md)

- [Princip vaše platby z Azure Marketplace](../billing-understand-your-azure-marketplace-charges.md)

- [Nejčastější dotazy týkající se Azure fakturace a předplatného](../billing-subscription-faq.md)

> [AZURE.NOTE] Pokud máte pořád ještě další otázky, přejděte prosím [kontaktovat podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat problém vyřešit rychle.

<!--
OLD MSDN Articles
- [What do I do if my Azure subscription become disabled?](https://msdn.microsoft.com/library/azure/dn736049.aspx)
- [Edit payment information for an existing credit card](https://msdn.microsoft.com/library/azure/dn736053.aspx)
- [Add a new credit card to use as a payment method](https://msdn.microsoft.com/library/azure/dn736057.aspx)
- [Change the credit card on your Microsoft Azure account](https://msdn.microsoft.com/library/azure/dn736050.aspx)
- [Manage your payment method](https://msdn.microsoft.com/library/azure/dn736054.aspx)
-->



<!--Image references-->
