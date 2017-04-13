<properties
   pageTitle="Začínáme s SQL databáze dynamických dat maskování (Azure portál)"
   description="Jak začít s SQL databáze dynamických dat maskování na portálu Azure"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>


# <a name="get-started-with-sql-database-dynamic-data-masking-azure-portal"></a>Začínáme s SQL databáze dynamických dat maskování (Azure portál)

> [AZURE.SELECTOR]
- [Dynamická Data maskování - Azure klasické portálu](sql-database-dynamic-data-masking-get-started-portal.md)

## <a name="overview"></a>Základní informace

Vystavení citlivá data SQL databáze dynamických dat maskování omezuje tak, že maskování neoprávněným uživatelům. Dynamická data maskování je podporovaný pro V12 verzi databáze SQL Azure.

Dynamická data maskování pomáhá zabránili neoprávněnému přístupu citlivá data povolením zákazníci k určení kolik citlivá data zobrazíte s minimálními dopad na vrstvě aplikací. Je funkce zabezpečení na základě zásad, která slouží ke skrytí citlivá data v sadě výsledků dotazu přes určené databázová pole, zatímco nemění data v databázi.

Například zástupce služby na Centrum odborné stanovit volající několik číslice jejich rodné číslo nebo číslo kreditní karty, ale tyto položky dat neměli zveřejněným plně služeb zákazníkům. Je to možné definovat maskování pravidlo, že masky všechny ale poslední čtyři číslice rodné číslo nebo číslo kreditní karty ve výsledku sada každý dotaz. Jiný příklad masky příslušná data určeny chránit data identifikovatelné osobní údaje, tak, aby vývojáři můžete dotaz provozním prostředí pro odstraňování potíží bez porušení dodržování předpisů.

## <a name="sql-database-dynamic-data-masking-basics"></a>Základy SQL databáze dynamických dat maskování

Nastavení dynamických dat maskování zásad na portálu Azure tak, že vyberete operaci maskování dynamických dat v SQL databázi konfigurace zásuvné nebo zásuvné nastavení.


### <a name="dynamic-data-masking-permissions"></a>Dynamická data maskování oprávnění

Dynamická data maskování je možné konfigurovat tak, že databázi Azure správce, správce serveru nebo role Úředník zabezpečení.

### <a name="dynamic-data-masking-policy"></a>Zásady maskování dynamických dat

* **Uživatelé SQL vyloučené ze maskování** - sadu SQL uživatelů a identit AAD, které získá nemaskované data ve výsledcích dotazu SQL. Všimněte si, že uživatelé s oprávněními správce se vždycky vyloučit z maskování a uvidí původní data bez Libovolná maska.

* **Maskování pravidla** - sady pravidel, které definují určené pole, která mají být maskované a maskování funkci, která se použije. Určená pole lze definovat pomocí schématu název databáze, název tabulky a název sloupce.

* **Maskování funkce** - sadu způsoby, kterými se řídí zobrazení dat různých scénářích.

| Maskování (funkce) | Použití logických operátorů maskování |
|----------|---------------|
| **Výchozí**  |**Úplné maskování podle datové typy určené polí**<br/><br/>• Použití XXXX nebo méně křížky je-li velikost pole pro datové typy string (nchar, ntext, nvarchar) menší než 4 znaky.<br/>• Použít nulovou hodnotu pro číselné datové typy (bigint bit, desetinná, int, peníze, číselné, smallint, smallmoney, tinyint, plovoucí, reálné).<br/>• Pomocí 01 01 1900 pro data typu datum a čas (datum, datetime2, data a času, datetimeoffset, smalldatetime, čas).<br/>• SQL varianty, výchozí hodnoty aktuální typ se používá.<br/>• XML dokument <masked/> se používá.<br/>• Určenou zvláštních datových typů prázdnou hodnotu (časové razítko tabulky, ID hierarchie, GUID, binární, obrázek, prostorové typy seskupit).
| **Platební kartou** |**Maskování metody, které poskytuje poslední čtyři číslice určená pole** a přidá řetězec konstantní jako předponu ve formě kreditní karty.<br/><br/>XXXX XXXX XXXX 1234|
| **Rodné číslo.** |**Maskování metody, které poskytuje poslední čtyři číslice určená pole** a přidá řetězec konstantní jako předponu ve formuláři American rodné číslo.<br/><br/>XXX-XX-1234 |
| **E-mailu** | **Maskování metoda která zpřístupňuje první písmeno a nahradí domény se XXX.com** pomocí předponu konstantní řetězce ve formě e-mailovou adresu.<br/><br/>aXX@XXXX.com |
| **Náhodné číslo** | **Maskování metodu, která vygeneruje náhodné číslo** podle vybrané omezení a skutečné datové typy. Pokud určené hranice se liší, funkce maskování budou konstantou.<br/><br/>![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Vlastního textu** | **Maskování metody, které poskytuje první a poslední znak** a přidá řetězec vlastní odsazení uprostřed. Je-li původní řetězec kratší než vystaveného předponu nebo příponu, bude použita pouze řetězec odsazení. <br/>přípona předponu [odsazení]<br/><br/>![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>
### <a name="recommended-fields-to-mask"></a>Doporučená pole Maskujte

Modul doporučení DDM příznaky určitých polí z databáze jako potenciálně citlivé pole, které mohou být správnými kandidáty pro maskování. V zásuvné maskování dynamických dat na portálu uvidíte doporučené sloupce pro databázi. Je třeba udělat stačí klikněte na **Přidat masku** pro jeden nebo více sloupců a potom **Uložit** -li použít masku u těchto polí.

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Nastavení maskování dynamických dat na portálu Azure databáze

1. Spusťte portálu Microsoft Azure na [https://portal.azure.com](https://portal.azure.com).

2. Přejděte na zásuvné nastavení databáze, která obsahuje citlivé data, která chcete masky.

3. Klikněte na dlaždici **Maskování dynamických dat** , která otevře zásuvné **Maskování dynamických dat** konfigurace.

    * Můžete taky můžete přejděte dolů do části **operace** a klikněte na **Dynamické maskování Data**.

    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>


4. V konfiguraci zásuvné **Maskování dynamických dat** můžete vidět některé databáze sloupce, které modul doporučení má maskování. Pokud chcete přijmout doporučení, jenom pro jeden nebo více sloupců klikněte na tlačítko **Přidat masku** a maskou vytvoří podle vybraného typu výchozí pro tento sloupec. Funkce maskování můžete změnit tak, že kliknete na pravidlo maskování a úpravy maskování formátu pole do jiného formátu podle svého výběru. Ujistěte se, že klepnutím na tlačítko **Uložit** uložte nastavení.

    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>


5. Přidání masky pro všechny sloupce v databázi, v horní části Konfigurace zásuvné **Maskování dynamických dat** klikněte na **Přidat masku** otevřete zásuvné konfigurace **Přidat pravidlo maskování**

    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>

6. Vyberte **schéma**, **tabulek** a **sloupců** k definování určené pole, které bude maskované.

7. Zvolte **Maskování formátu pole** ze seznamu citlivá data maskování kategorie.

    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>     

8. V datech maskování pravidlo zásuvné aktualizovat sadu maskování pravidel v dynamických dat maskování zásady klikněte na **Uložit** .

9. Zadejte uživatele serveru SQL nebo AAD identit, které byste měli vyloučit z maskování a mít přístup k nemaskované citlivá data. To by měl být oddělené středníkem seznam uživatelů. Všimněte si, že uživatelé s oprávněními správce vždy mít přístup k původní nemaskované data.

    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    >[AZURE.TIP] Chcete-li, aby aplikace vrstvy můžete zobrazit citlivá data pro uživatele aplikace privilegované, přidat uživatele SQL nebo AAD identity aplikace používá k vytvoření dotazu v databázi. Důrazně doporučujeme, aby tento seznam obsahoval minimální počet privilegovaných uživatelů k minimalizaci působení citlivá data.

10. V datech maskování konfigurace zásuvné uložit nový nebo aktualizovaný maskování zásady klikněte na **Uložit** .

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Nastavení dynamických dat maskování databáze pomocí rutin prostředí Powershell

V tématu [rutiny pro správu databáze Azure SQL](https://msdn.microsoft.com/library/azure/mt574084.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Nastavení maskování dynamických dat pro databázi pomocí rozhraní REST API

V tématu [operace pro databáze Azure SQL](https://msdn.microsoft.com/library/dn505719.aspx).
