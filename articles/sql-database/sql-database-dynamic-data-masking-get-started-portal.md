<properties
   pageTitle="Začínáme s SQL databáze dynamických dat maskování (klasické portál Azure)"
   description="Jak začít s SQL databáze dynamických dat maskování Classic portálu Azure"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>

# <a name="get-started-with-sql-database-dynamic-data-masking-azure-classic-portal"></a>Začínáme s SQL databáze dynamických dat maskování (klasické portál Azure)

> [AZURE.SELECTOR]
- [Dynamická Data maskování - Azure portálu](sql-database-dynamic-data-masking-get-started.md)

## <a name="overview"></a>Základní informace

Vystavení citlivá data SQL databáze dynamických dat maskování omezuje tak, že maskování neoprávněným uživatelům. Dynamická data maskování je podporovaný pro V12 verzi databáze SQL Azure.

Dynamická data maskování pomáhá zabránili neoprávněnému přístupu citlivá data povolením zákazníci k určení kolik citlivá data zobrazíte s minimálními dopad na vrstvě aplikací. Je funkce zásadami zabezpečení, která slouží ke skrytí citlivá data v sadě výsledků dotazu přes určené databázová pole, zatímco nemění data v databázi.

Například zástupce služby na Centrum odborné stanovit volající několik číslice jejich rodné číslo nebo číslo kreditní karty, ale tyto položky dat neměli zveřejněným plně služeb zákazníkům. Je to možné definovat maskování pravidlo, že masky všechny ale poslední čtyři číslice rodné číslo nebo číslo kreditní karty ve výsledku sada každý dotaz. Jiný příklad masky příslušná data určeny chránit data identifikovatelné osobní údaje, tak, aby vývojáři můžete dotaz provozním prostředí pro odstraňování potíží bez porušení dodržování předpisů.

## <a name="sql-database-dynamic-data-masking-basics"></a>Základy SQL databáze dynamických dat maskování

Nastavení dynamických dat maskování zásad Classic portálu Azure na kartě auditování a zabezpečení databáze.


> [AZURE.NOTE] Dynamická data maskování na portálu Azure, naleznete v tématu [Začínáme s SQL databáze dynamických dat maskování (Azure portál)](sql-database-dynamic-data-masking-get-started.md).


### <a name="dynamic-data-masking-permissions"></a>Dynamická data maskování oprávnění

Dynamická data maskování je možné konfigurovat tak, že databázi Azure správce, správce serveru nebo role Úředník zabezpečení.

### <a name="dynamic-data-masking-policy"></a>Zásady maskování dynamických dat

* **Uživatelé SQL vyloučené ze maskování** - sadu SQL uživatelů a identit AAD, které získá nemaskované data ve výsledcích dotazu SQL. Všimněte si, že uživatelé s oprávněními správce se vždycky vyloučit z maskování a uvidí původní data bez Libovolná maska.

* **Maskování pravidla** - sady pravidel, které definují určené pole, která mají být maskované a maskování funkci, která se použije. Určená pole lze definovat pomocí schématu název databáze, název tabulky a název sloupce.

* **Maskování funkce** - sadu způsoby, kterými se řídí zobrazení dat různých scénářích.

| Maskování (funkce) | Použití logických operátorů maskování |
|----------|---------------|
| **Výchozí**  |**Úplné maskování podle datové typy určené polí**<br/><br/>• Použití XXXX nebo méně křížky je-li velikost pole pro datové typy string (nchar, ntext, nvarchar) menší než 4 znaky.<br/>• Použít nulovou hodnotu pro číselné datové typy (bigint bit, desetinná, int, peněz, číselné, smallint, smallmoney, tinyint, plovoucí, reálné).<br/>• Pomocí 01 01 1900 pro data typu datum a čas (datum, datetime2, data a času, datetimeoffset, smalldatetime, čas).<br/>• SQL varianty, výchozí hodnoty aktuální typ se používá.<br/>• XML dokument <masked/> se používá.<br/>• Určenou zvláštních datových typů prázdnou hodnotu (časové razítko tabulky, ID hierarchie, GUID, binární, obrázek, prostorové typy seskupit).
| **Platební kartou** |**Maskování metody, které poskytuje poslední čtyři číslice určená pole** a přidá řetězec konstantní jako předponu ve formě kreditní karty.<br/><br/>XXXX XXXX XXXX 1234|
| **Rodné číslo.** |**Maskování metody, které poskytuje poslední čtyři číslice určená pole** a přidá řetězec konstantní jako předponu ve formuláři American rodné číslo.<br/><br/>XXX-XX-1234 |
| **E-mailu** | **Maskování metoda která zpřístupňuje první písmeno a nahradí domény se XXX.com** pomocí předponu konstantní řetězce ve formě e-mailovou adresu.<br/><br/>aXX@XXXX.com |
| **Náhodné číslo** | **Maskování metodu, která vygeneruje náhodné číslo** podle vybrané omezení a skutečné datové typy. Pokud určené hranice se liší, funkce maskování budou konstantou.<br/><br/>![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started-portal/1_DDM_Random_number.png) |
| **Vlastního textu** | **Maskování metody, které poskytuje první a poslední znak** a přidá řetězec vlastní odsazení uprostřed. Je-li původní řetězec kratší než vystaveného předponu nebo příponu, bude použita pouze řetězec odsazení.<br/>přípona předponu [odsazení]<br/><br/>![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started-portal/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-classic-portal"></a>Nastavení maskování dynamických dat na portálu Azure klasické databáze

1. Spusťte portálu Azure klasické na [https://manage.windowsazure.com](https://manage.windowsazure.com).

2. Klikněte na databázi, kterou chcete masky a potom klikněte na kartu **AUDITOVÁNÍ a zabezpečení** .

3. V části **dynamických dat maskování**klikněte na **Povolit** povolíte dynamických dat maskování funkce.  

4. Zadejte uživatele serveru SQL nebo AAD identit, které byste měli vyloučit z maskování a mít přístup k nemaskované citlivá data. To by měl být oddělené středníkem seznam uživatelů. Všimněte si, že uživatelé s oprávněními správce vždy mít přístup k původní nemaskované data.

    >[AZURE.TIP] Chcete-li, aby aplikace vrstvy můžete zobrazit citlivá data pro uživatele aplikace privilegované, přidat uživatele SQL nebo AAD identity aplikace používá k vytvoření dotazu v databázi. Důrazně doporučujeme, aby tento seznam obsahoval minimální počet privilegovaných uživatelů k minimalizaci působení citlivá data.

    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started-portal/4_ddm_policy_classic_portal.png)

5. V dolní části stránky v řádku nabídek klikněte na **Přidat MASKU** otevřete maskování okno Konfigurace pravidla.

6. V seznamech rozevíracího seznamu definovat určené polí, které bude maskované vyberte **schéma**, **tabulek** a **sloupců** .

7. Ze seznamu citlivá data maskování kategorie vyberte **MASKOVÁNÍ funkce** .

    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started-portal/5_DDM_Add_Masking_Rule_Classic_Portal.png)

8. V okně data maskování pravidlo okno aktualizovat sadu maskování pravidel v dynamických dat maskování zásady klikněte na tlačítko **OK** .

9. Klepněte na tlačítko **Uložit** uložte zásad nový nebo aktualizovaný maskování.


## <a name="set-up-dynamic-data-masking-for-your-database-using-transact-sql-statements"></a>Nastavení dynamických dat maskování databáze pomocí příkazů jazyce Transact-SQL

V tématu [dynamických dat maskování](https://msdn.microsoft.com/library/mt130841.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Nastavení dynamických dat maskování databáze pomocí rutin prostředí Powershell

V tématu [rutiny pro správu databáze Azure SQL](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Nastavení maskování dynamických dat pro databázi pomocí rozhraní REST API

V tématu [operace pro databáze Azure SQL](https://msdn.microsoft.com/library/dn505719.aspx).
