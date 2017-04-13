<properties
    pageTitle="Azure zajímavých úložiště objektů BLOB | Microsoft Azure"
    description="Úložiště úrovní k úložišti objektů Blob Azure nabízejí náklady efektivně úložiště pro data objektu vámi přístup. Vrstvy zajímavých úložiště je optimalizována pro data, která se méně často pracuje."
    services="storage"
    documentationCenter=""
    authors="michaelhauss"
    manager="vamshik"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="mihauss"/>


# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Úložiště objektů Blob Azure: Aktivní a ochladí úrovní úložiště

## <a name="overview"></a>Základní informace

Azure úložiště teď nabízí dvě úrovně úložiště k úložišti objektů Blob (objektu úložiště), takže mohou být uloženy datům nejčastěji levně podle toho, jak používat. Azure **osy žádanou úložiště** je optimalizováno pro ukládání data, která se často pracuje. Azure **osy zajímavých úložiště** je optimalizováno pro ukládání data, která jsou k nim přistupovat a dlouhodobé zřídka. Data ve vrstvě zajímavých úložiště nevadí vám něco nižší dostupnost, ale pořád vyžaduje vysoké životnosti a podobné času pro přístup k a výkon vlastnosti jako aktivní data. Zajímavých dat mírně nižší dostupnost SLA a vyšší náklady přístup jsou přijatelné střídání pro mnoho nižší náklady úložiště.

Data uložená v cloudu v současné době roste exponenciální tempem. Ke správě nákladů pro vaše potřeby zvětšující úložiště, je užitečné pro uspořádání dat na základě atributů jako četnost přístup a plánované uchovávání informací období. Data uložená v cloudu může být docela různých z hlediska jak je generováno, zpracovávané a získat přístup prostřednictvím životnosti. Některá data aktivně přístupem a změny v celém životnosti. Některá data přistupuje často brzy v jeho životnost s přístupem přetažením významně jako věku data. Některá data nečinný v cloudu a je jen zřídka, pokud vůbec, přistupovat jednou uložené.

Každý z těchto dat přístup k scénáře jsme je popsali výše výhody odlišné vrstva úložiště, která je optimalizovaná pro vzorek konkrétní přístup. Zavedením úrovní žádanou a zajímavých úložiště objektů Blob Azure úložiště teď adresy vyžadují odlišné úložiště úrovní s oddělte ceny modely.

## <a name="blob-storage-accounts"></a>Účty úložiště objektů BLOB

**Účty úložiště objektů BLOB** jsou specializované úložiště účty pro ukládání Nestrukturovaná data jako objekty BLOB (objekty) v úložišti Azure. U účtů úložiště objektů Blob teď můžete mezi úrovní žádanou a zajímavých úložiště pro ukládání méně často používané zajímavých data v úložišti nižší náklady a uložit častěji přístup žádanou data v dolním přístup nákladů. Účty úložiště objektů blob se podobají stávajících účtů univerzální úložiště a sdílení všech skvělé životnosti, dostupnost, škálovatelnost a výkonu funkce, které můžete použít dnes, včetně 100 % konzistence rozhraní API pro objekty BLOB bloku a připojit objektů BLOB.

> [AZURE.NOTE] Účty úložiště objektů BLOB podporují pouze bloku a přidat objektů BLOB a ne objektů BLOB stránky.

Účty úložiště objektů BLOB vystavit atribut **Úroveň přístupu** , které umožňují určit úroveň úložiště jako **aktivní** nebo **zajímavých** podle data uložená v okně účet. Pokud dojde ke změně použití obchodních dat, můžete také přepínat mezi tyto úložiště úrovní kdykoli.

> [AZURE.NOTE] Změna úrovně úložiště může způsobit další poplatky. Zkontrolujte naleznete v části [ceny a fakturace](storage-blob-storage-tiers.md#pricing-and-billing) další podrobnosti.

Příklady použití scénářů žádanou úložiště osy zahrnout:

- Data, která je aktivně používán nebo podle očekávání měly být přístupný (čtení od a do ručně psaných) často.
- Data, která je připraven k zpracování a případné migrovat na osy zajímavých úložiště.

Příklady použití scénářů zajímavých úložiště osy zahrnout:

- Zálohování, archivace a obnovení havárie datové sady.
- Starší obsahu médií není zobrazit často už ale očekává se, aby byl k dispozici okamžitě při přístupu.
- Velkých sad dat, které je potřeba mít uložené náklady efektivně, zatímco je víc dat shromážděných pro další zpracování. (*například*dlouhodobé úložiště matematickém dat, jako nezpracovaná telemetrickými daty z výrobních zařízení)
- Původní (nezpracovaná) data, která musí být zachovány, i když nezpracuje konečné použitelné formulář. (*například*nezpracovanými mediální soubory po převod kódování do jiných formátů)
- Dodržování předpisů a archivace dat, která musí být uloženy poměrně dlouho a téměř nikdy pracuje. (*například*záběru fotoaparátu zabezpečení staré X-Rays/MRIs zdravotní péče organizace, zvukových záznamů a přepisy zákazníka volá pro finanční služby)

Další informace o účtech úložiště najdete v článku [o Azure úložiště účty](storage-create-storage-account.md) .

Aplikace vyžadující pouze blokovat nebo přidat úložiště objektů blob, doporučujeme používat účty úložiště objektů Blob umožní využít výhod odlišné ceny modelu vrstveného úložiště. Však nám jasné, že to není možné za určitých okolností kde pomocí účtů univerzální úložiště bude umožňuje vrátit, jako například:

- Budete muset pomocí tabulek, dotazů nebo soubory a chcete svůj objektů BLOB uložený ve stejném účtu úložiště. Všimněte si, že je žádné technické využít k ukládání do jednoho účtu než mají stejnou sdílené klíče.
- Potřebujete pomocí klasického nasazení modelu. Účty úložiště objektů BLOB jsou dostupné, jenom pomocí modelu nasazení Správce prostředků Azure.
- Je potřeba použít objektů BLOB stránky. Účty úložiště objektů BLOB nepodporují objektů BLOB stránky. Obecně doporučujeme, abyste pomocí objektů BLOB blok, pokud máte konkrétní potřebu objektů BLOB stránky.
- Pomocí verze [Rozhraní REST API úložiště služby](https://msdn.microsoft.com/library/azure/dd894041.aspx) , které předchází 2014-02-14 nebo v knihovně klienta verzí nižší než 4.x a nedá se upgradovat aplikaci.

> [AZURE.NOTE] Účty úložiště objektů BLOB aktuálně podporuje většinou Azure oblasti s více ke sledování. Můžete najít aktualizovaný seznam dostupných oblastí na stránce [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services) .

## <a name="comparison-between-the-storage-tiers"></a>Srovnání jednotlivých úrovní úložiště

V následující tabulce jsou uvedeny srovnání dvě úložiště úrovně:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Aktivní úložiště osy</center></strong></td>
    <td><strong><center>Vytvoření skvělých úložiště osy</center></strong></td
</tr>
<tr>
    <td><strong><center>Dostupnost</center></strong></td>
    <td><center>99,9 %</center></td>
    <td><center>99 %</center></td>
</tr>
<tr>
    <td><strong><center>Dostupnost<br>(Vzdálená pomoc GRS čtení)</center></strong></td>
    <td><center>99,99 %</center></td>
    <td><center>99,9 %</center></td>
</tr>
<tr>
    <td><strong><center>Poplatky za použití</center></strong></td>
    <td><center>Vyšší náklady úložiště<br>Nižší náklady přístup a transakce</center></td>
    <td><center>Nižší náklady úložiště<br>Vyšší náklady přístup a transakce</center></td>
</tr>
<tr>
    <td><strong><center>Velikost minimální objektů<center></strong></td>
    <td colspan="2"><center>NENÍ K DISPOZICI</center></td>
</tr>
<tr>
    <td><strong><center>Doba trvání minimální úložiště<center></strong></td>
    <td colspan="2"><center>NENÍ K DISPOZICI</center></td>
</tr>
<tr>
    <td><strong><center>Latence<br>(První bajt času)<center></strong></td>
    <td colspan="2"><center>milisekund</center></td>
</tr>
<tr>
    <td><strong><center>Výkon a škálovatelnost cíle<center></strong></td>
    <td colspan="2"><center>Stejně jako univerzální úložiště účty</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] Účty úložiště objektů BLOB podporovat stejný výkon a škálovatelnost cílů jako univerzální úložiště účty. Další informace najdete v článku [Azure úložiště škálovatelnost a výkonu cílů](storage-scalability-targets.md) .

## <a name="pricing-and-billing"></a>Ceny a fakturace

Účty úložiště objektů BLOB použijte nový model ceny pro úložiště objektů blob založené na úroveň úložiště. Pokud používáte účet úložiště objektů Blob platí následující omezení fakturace:

- **Úložiště nákladů**: kromě množství dat uložených jako nákladů ukládání dat liší se podle osy úložiště. Za GB náklady jsou nižší osy zajímavých úložiště než osy žádanou úložiště.
- **Přístup k datům nákladů**: data z osy zajímavých úložiště, můžete se bude účtovat poplatku za GB dat přístup pro čtení a zápis.
- **Náklady transakce**: existuje každou transakci částka za obou úrovní. Náklady na transakce osy zajímavých úložiště je však vyšší než osy žádanou úložiště.
- **Data GEO replikace převést náklady**: To platí jenom pro účty s geo replikace nakonfigurován, včetně GRS a Vzdálená pomoc GRS. Přenos dat GEO replikace poněkud poplatku za GB.
- **Výstupní data převést náklady**: přenosy výstupní data (data, která se převede z Azure oblast) vzniknou fakturace využití šířky pásma na základě za GB konzistentní s účty univerzální úložiště.
- **Změna úrovně úložiště**: změňte úroveň úložiště z skvělé za běhu je možné za poplatek rovno čtení všechna data stávající účtu úložiště pro každý přechod. Na druhé straně změňte úroveň úložiště z aktivní na skvělé budou zdarma.

> [AZURE.NOTE] Pokud chcete uživatelům povolit vyzkoušet nové úrovně úložiště a ověřte funkce příspěvek spuštění náklady pro změnu úložiště je bude osy z skvělé klávesových od vypnout až 30. června 2016. Spuštění 1st červenec 2016, náklady se použije na všechny přechody z skvělé klávesových. Podrobné informace o cenách modelu doplňku pro úložiště objektů Blob účty zobrazíte [Azure úložiště ceny](https://azure.microsoft.com/pricing/details/storage/) stránky. Podrobné informace o odchozí data přenos poplatky zobrazíte stránku [Podrobnosti ceny přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/) .

## <a name="quick-start"></a>Představení aplikace

V této části jsme ukazují tyto scénáře Azure portálu:

- Postup pro vytvoření účtu úložiště objektů Blob.
- Jak spravovat účet úložiště objektů Blob.

### <a name="using-the-azure-portal"></a>Pomocí portálu Azure

#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Vytvoření účtu úložiště objektů Blob Azure portálu

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

2. V nabídce centrální vyberte **Nový** > **dat + úložiště** > **účtu úložiště**.

3. Zadejte název účtu úložiště.

    Tento název musí být jedinečné; pracovní postup slouží jako součást adresy URL použít pro přístup k objektům v účtu úložiště.  

4. Vyberte **Správce prostředků** jako modelu nasazení.

    Vrstveného úložiště lze použít pouze s oprávněními správce prostředků úložiště; Toto je doporučené nasazení modelu doplňku pro nové zdroje. Další informace najdete v článku [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md).  

5. V rozevíracím seznamu Typ účtu vyberte **Úložiště objektů Blob**.

    Toto je vyberete typ účtu úložiště. Vrstveného úložiště není k dispozici v univerzální úložiště; je k dispozici pouze v typ účtu úložiště objektů Blob.    

    Všimněte si, že pokud vyberete, osy výkonu je nastavený na standardní. Vrstveného úložiště není k dispozici s osy výkonu Premium.

6. Výběrem možnosti replikace účtu úložiště: **LRS**, **GRS**nebo **GRS Vzdálená pomoc**. Výchozí hodnota je **Vzdálená pomoc GRS**.

    LRS = místně nadbytečné úložiště; GRS = geo nadbytečné úložiště (2 oblastí); Vzdálená pomoc GRS je geo nadbytečné úložiště přístup pro čtení (2 oblasti s oprávněním ke druhé čtení).

    Podrobné informace o možnosti replikace Azure úložiště podívejte se na [replikace Azure úložiště](storage-redundancy.md).

7. Vyberte vrstvy správné úložiště pro vaše potřeby: nastavte **úroveň přístupu** k **Vytvoření skvělých** nebo **aktivní**. Výchozí hodnota je **aktivní**.

8. Vyberte předplatné, ve kterém chcete vytvořit nový účet úložiště.

9. Zadat nové skupiny prostředků nebo vyberte existující skupiny zdrojů. Další informace o skupinách zdroje najdete v článku [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md).

10. Vyberte oblast pro svůj účet úložiště.

11. Klikněte na **vytvořit** vytvořte účet úložiště.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Změna úrovně úložiště účtu úložiště objektů Blob Azure portálu

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

2. Pokud chcete přejít ke svému účtu úložiště, vyberte všechny zdroje a pak vyberte svůj účet úložiště.

3. V nastavení zásuvné klikněte na **konfigurační** k zobrazení nebo změna konfigurace účtu.

4. Vyberte vrstvy správné úložiště pro vaše potřeby: nastavte **úroveň přístupu** k **Vytvoření skvělých** nebo **aktivní**.

5. Klikněte na Uložit v horní části zásuvné.

> [AZURE.NOTE] Změna úrovně úložiště může způsobit další poplatky. Zkontrolujte naleznete v části [ceny a fakturace](storage-blob-storage-tiers.md#pricing-and-billing) další podrobnosti.

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Vyhodnocení a migrace k účtům úložiště objektů Blob

Této části účel umožňuje uživatelům a přitom zajistit hladký přechod do pomocí účtů úložiště objektů Blob. Existují dva scénáře uživatele:

- Máte existující účet univerzální úložiště a chcete vyhodnotit na změnu účtu úložiště objektů Blob s osy doprava úložiště.
- Jste se rozhodli použití účtu úložiště objektů Blob nebo už máte a chcete zjistit, jestli byste měli použít osy aktivní nebo zajímavých úložiště.

V obou případech je první obchodní volnosti pořadí odhadu nákladů ukládání a přístup k datům uložené na účtu úložiště objektů Blob a porovnání, a aktuální nákladů.

### <a name="evaluating-blob-storage-account-tiers"></a>Vyhodnocení úrovní účtu úložiště objektů Blob

K odhadu nákladů ukládání a přístup k datům uložené na účtu úložiště objektů Blob, budete potřebovat k vyhodnocení existující použití vzorku nebo aproximaci počtu očekávané použití vzorku. Obecně vás bude určitě zajímat:

- Úložiště spotřeba – jak množství dat, které je uloženo a jak tím změní měsíčně?
- Access vzorku úložiště - množství dat, která má být číst a zapisovat k tomuto účtu (včetně nové datové)? Kolik transakcí se používají pro přístup k datům a jaké druhy transakce, že jsou?

#### <a name="monitoring-existing-storage-accounts"></a>Sledování stávajících účtů úložiště

Sledování stávajících účtů úložiště a shromáždění tato data, můžete můžete využít analýzy úložiště Azure, která provádí protokolování a poskytuje metriky datový úložiště účtu.
Úložiště analýzy mohou být uloženy metrik zahrnující agregované transakce Statistika a kapacity data o požadavcích služby úložiště objektů Blob pro účty univerzální úložiště jak účty úložiště objektů Blob.
Tato data je uložená v tabulkách známý ve stejném účtu úložiště.

Další informace najdete v článku [O metriky úložišť technologie pro analýzu](https://msdn.microsoft.com/library/azure/hh343258.aspx) a [Schématu tabulky metriky analýzy úložiště](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] Účty úložiště objektů BLOB vystavit koncový bod služby tabulky pouze pro ukládání a přístup k datům metriky tohoto účtu.

Sledovat spotřebu úložiště pro službu úložiště objektů Blob, bude třeba povolit metriky kapacity.
Tuto funkci povolíte zaznamenání dat kapacita denní účtu úložiště objektů Blob službě a zaznamenané jako položky seznamu vzniku k tabulce *$MetricsCapacityBlob* v rámci jednoho účtu úložiště.

Sledování dat aplikace access vzor pro službu úložiště objektů Blob, bude třeba povolit hodinové metriky transakce úrovni rozhraní API.
Tuto funkci povolíte za rozhraní API transakce jsou agregované každou hodinu a zaznamenané jako položky seznamu vzniku k tabulce *$MetricsHourPrimaryTransactionsBlob* v rámci jednoho účtu úložiště. V tabulce *$MetricsHourSecondaryTransactionsBlob* záznamů transakce sekundární koncový bod pro nečekané Vzdálená pomoc GRS úložiště účty.

> [AZURE.NOTE] V případě, že máte účet univerzální úložiště, ve kterém jsou uloženy objekty BLOB stránky a virtuálního počítače disků vedle bloku a připojit objektů blob data, tento proces odhad se nepoužívá. Je to proto bude obsahovat nijak rozlišující kapacity a metriky transakce pouze v závislosti na typu objektů blob pro blokovat a připojte objektů BLOB, které můžete do účtu úložiště objektů Blob migrovat.

Získat dobré sbližování dat spotřeby a přístup vzorku, doporučujeme vyberte období uchování metriky, které je zástupce běžná použití a odvodit.
Jednou z možností je uchovávání dat metriky dobu 7 dní a Automatický sběr dat každý týden, analýzy na konci měsíce.
Další možností je uchovávání dat metriky posledních 30 dnů a shromažďovat a analyzovat data na konci 30 dní.

Další informace o povolení shromažďování a zobrazení dat metriky najdete, [metriky úložišť povolení Azure a zobrazení dat metriky](storage-enable-and-view-metrics.md).

> [AZURE.NOTE] Ukládání, otevření a stažení technologie pro analýzu dat je také účtovaná stejně jako běžný uživatel data.

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Využití použití metriky k odhadu nákladů

##### <a name="storage-costs"></a>Náklady úložiště

Poslední položku v kapacita metriky tabulky *$MetricsCapacityBlob* klíčové řádku *"data za rok"* zobrazuje kapacitu úložiště využívané uživatelská data.
Poslední položku v kapacita metriky tabulky *$MetricsCapacityBlob* s klíčové řádku *"analýzy"* zobrazuje kapacitu úložiště využívané protokoly technologie pro analýzu.

Tento celkovou kapacitu využívané obou uživatelských dat a analýzy protokoly (Pokud je povoleno) lze pak k odhadu nákladů ukládání dat do účtu úložiště.
Stejným způsobem lze také pro vytvoření odhadu nákladů úložiště pro blok ani připojit v účty univerzální úložiště objektů BLOB.

##### <a name="transaction-costs"></a>Náklady transakce

Součet *"TotalBillableRequests"*, přes všechny položky pro rozhraní API v tabulce metriky transakce označuje celkový počet transakce u této konkrétní rozhraní API. *například*celkový počet transakce *"GetBlob"* v dané období můžete vypočítat součtem celkový počet fakturaci požadavků pro všechny položky s klávesou řádku *"uživatele. GetBlob "*.

K odhadu nákladů transakce pro účty úložiště objektů Blob, budete potřebovat které rozdělí transakce do tří kategorií, protože jsou cenami jinak.

- Napište transakce například *"PutBlob"*, *"PutBlock"*, *"PutBlockList"*, *"AppendBlock"*, *"ListBlobs"*, *"ListContainers"*, *"CreateContainer"*, *"SnapshotBlob"*a *"CopyBlob"*.
- Odstranění transakce například *"DeleteBlob"* a *"DeleteContainer"*.
- Všechny ostatní transakce.

K odhadu nákladů transakce pro účty univerzální úložiště, budete muset agregovat všechny transakce bez ohledu na to operace rozhraní API.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Data aplikace access a geo replikace dat převést náklady

Během analýzy úložiště neposkytuje množství dat číst a zapisovat do účtu úložiště, ho můžete zhruba odhadnout pohledem tabulce metriky transakce.
Součet *"TotalIngress"* přes všechny položky pro rozhraní API v tabulce metriky transakce označuje celkovou kapacitu průniku dat v bajtech u této konkrétní rozhraní API.
Podobně součet *"TotalEgress"* označuje celkovou kapacitu výstupní data, v bajtech.

Abyste mohli odhad nákladů dat aplikace access pro účty úložiště objektů Blob, musíte se rozdělí transakce do dvou skupin.

- Částka data načtená z účtu úložiště můžete odhadovanou pohledem součet *"TotalEgress"* pro primárně operace *"GetBlob"* a *"CopyBlob"* .
- Množství dat napsali k tomuto účtu úložiště můžete odhadovanou vyhledáním na součet *"TotalIngress"* především *"PutBlob"*, *"PutBlock"*, *"CopyBlob"* a *"AppendBlock"* operace.

Náklady geo replikace přenos dat u účtů úložiště objektů Blob můžete taky nevypočítávají pomocí odhad množství dat napsané v případě GRS nebo Vzdálená pomoc GRS účtu úložiště.

> [AZURE.NOTE] Podrobnější příklad výpočet nákladů na použití osy aktivní nebo zajímavých úložiště přečtěte si téma časté otázky s názvem *"co jsou aktivní a skvělé úrovní přístupu a jak mají určují který z nich budete use?"* v [Azure úložiště ceny stránky](https://azure.microsoft.com/pricing/details/storage/).

### <a name="migrating-existing-data"></a>Přenesení existujících dat

Účet úložiště objektů Blob specializované pro ukládání pouze blok ani připojit objektů BLOB. Existující univerzální úložiště účty, které umožňují uložení tabulek, dotazů, soubory a disků, stejně jako objekty BLOB, nelze převést na účty úložiště objektů Blob. Použít úrovní úložiště, musíte vytvořit nový účet úložiště objektů Blob a přenesení existujících dat do nově vytvořený účty.
Pomocí následujících metod k přenesení existujících dat do účty úložiště objektů Blob z místní úložiště zařízení, od jiných výrobců cloudové úložiště poskytovatelů nebo ze stávajících účtů univerzální úložiště v Azure:

#### <a name="azcopy"></a>AzCopy

AzCopy je určený pro výkonné kopírování dat z Azure úložiště a příkazového řádku nástroj Windows. AzCopy slouží ke kopírování dat do vašeho účtu úložiště objektů Blob ze stávajících účtů univerzální úložiště nebo odeslání dat ze zařízení místní úložiště ke svému účtu úložiště objektů Blob.

Další podrobnosti najdete v tématu [přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Přesun dat

Azure úložiště pohyb dat pro .NET vychází z framework pohyb základní data, která zajišťuje AzCopy. Knihovně je určený pro výkonné spolehlivý a snadno předávání údajů podobný AzCopy. Vám umožní všech výhod funkce poskytované AzCopy v aplikaci nativně aniž byste museli zabývat spuštění a sledování externích výskyty AzCopy.

Další podrobnosti najdete v tématu [Azure úložiště pohyb dat pro .net](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>Rozhraní REST API nebo knihovny klienta

Můžete vytvořit vlastní aplikaci pro migraci dat do účtu úložiště objektů Blob pomocí jedné z knihoven Azure klienta nebo služby Azure úložiště rozhraní REST API. Azure úložiště poskytuje knihovny klienta RTF pro více jazyků a platformách například .NET Java, C++, Node.JS, PHP, skutečné a Python. Knihoven klienta nabízejí pokročilé funkce, například Logika opakování, protokolování a paralelní nahrávání. Můžete taky vytvořit přímo proti rozhraní REST API, nazývaný tak, že libovolný jazyk, který zajišťuje žádosti o protokolu HTTP/HTTPS.

Další informace najdete v tématu [Začínáme s úložiště objektů Blob Azure](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] Objekty BLOB šifrovaná pomocí šifrování na straně klienta obsahují související šifrování metadata uložená s objektů blob. Je naprosto důležité, že všechny kopírovat mechanismus by měl zajistit, aby se zachová metadata objektů blob a zejména související šifrování metadata. Pokud zkopírujete objektů BLOB bez tato metadata, obsah objektů blob nebudou zobrazitelné ve výsledcích vyhledávání znovu. Podrobné informace o šifrování související metadata najdete v článku [šifrování na straně klienta Azure úložiště](storage-client-side-encryption.md).

## <a name="faqs"></a>Nejčastější dotazy

1. **Existují stále dostupné úložiště účtů?**

    Ano, stávajících účtů úložiště jsou pořád dostupné a jsou beze změny v ceny nebo funkce.  Nemají možnost zvolit úložiště osy a nebudou mít vrstvení funkcí v budoucnu.

2. **Proč a kdy by měly začít používat účty úložiště objektů Blob?**

    Účty úložiště objektů BLOB specializované pro ukládání objektů BLOB a umožňují představující nových objektů blob zaměřené na zpracování funkcí. Přecházející do dalšího období, účty úložiště objektů Blob jsou doporučené postupy pro uložení jako další funkce, například hierarchické úložiště objektů BLOB, a vrstvení zavede založené na tento typ účtu. Je ale až vás, když chcete migrovat podle vašim požadavkům pro firmy.

3. **Můžete převést existující účtu úložiště k účtu úložiště objektů Blob?**

    Ne. Potřebujete vytvořit nové a přenést data, jak je vysvětleno účtu úložiště objektů blob je jiný druh úložiště účtu.

4. **Můžete ukládat objekty v obou úrovní úložiště ve stejném účtu?**

    Atribut *Přístup osy* , který označuje osy úložiště je nastavený na úrovni účtu a platí pro všechny objekty v tomto účtu. Na úrovni objektů se nedá nastavit atribut úroveň přístupu.

5. **Můžete změnit úroveň úložiště účtu úložiště objektů Blob?**

    Ano. Bude moct změnit úroveň úložiště nastavením atribut *Přístup osy* na účtu úložiště. Změna úrovně úložiště bude používat pro všechny objekty uložené na účtu. Je možné změnit úroveň úložiště z aktivní na skvělé nepřejímá všechny poplatky při změně z skvělé za běhu za za GB náklady pro čtení všech dat do účtu.

6. **Jak často můžete změnit úroveň úložiště účtu úložiště objektů Blob?**

    Když není jsme vynutit omezení frekvence bude možné měnit osy úložiště, uvědomte si prosím, změnit úroveň úložiště z skvělé za běhu je možné významné poplatky za. Nedoporučujeme často Změna osy úložiště.

7. **Bude objektů BLOB ve vrstvě zajímavých úložiště se chovají jinak než ve vrstvě žádanou úložiště?**

    Ve vrstvě žádanou úložiště objektů BLOB mít stejnou latence jako objekty BLOB univerzální úložiště účty. Ve vrstvě zajímavých úložiště objektů BLOB mít podobné latence (v milisekundách) jako objekty BLOB univerzální úložiště účty.

    Ve vrstvě zajímavých úložiště objektů BLOB bude mít poněkud menší dostupnost úroveň služeb (SLA) než uložené ve vrstvě žádanou úložiště objektů BLOB. Další podrobnosti najdete v tématu [SLA úložiště](https://azure.microsoft.com/support/legal/sla/storage).

8. **Můžete ukládat objekty BLOB stránky a disků virtuálního počítače v účty úložiště objektů Blob?**

    Účty úložiště objektů BLOB podporují pouze bloku a přidat objektů BLOB a ne objektů BLOB stránky. Azure virtuálního počítače disků jsou podporovaným objektů BLOB stránky a jako výsledek účty úložiště objektů Blob se nedají použít k ukládání disků virtuálního počítače. Ale je možné k ukládání záložní kopie disků virtuálního počítače jako objekty BLOB blok v účtu úložiště objektů Blob.

9. **Bude potřeba změnit existující aplikace použít účty úložiště objektů Blob?**

    Účty úložiště objektů BLOB jsou konzistentní s účty univerzální úložiště pro blok rozhraní API 100 % a připojit objektů BLOB. Dokud aplikace a to pomocí blokovat objektů BLOB nebo přidat objektů BLOB a používáte 2014-02-14 verzi z [Úložiště služby REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) nebo vyšší a aplikace by měly jenom fungovat. Pokud používáte starší verzi protokolu, pak se potřebujete aktualizovat aplikaci používat novou verzi tak, aby Bezproblémová práce prostřednictvím obou typů účtů úložiště. Obecně vždy doporučená používat nejnovější verze bez ohledu na to, který účet úložiště zadejte je používáme.

10. **Bude k dispozici ke změně uživatelské prostředí?**

    Účty úložiště objektů blob se hodně podobají univerzální úložiště účty pro ukládání blok připojení objektů BLOB a dál nebude podporovat klíčové funkce Azure úložiště, včetně vysoké životnosti a dostupnost, škálovatelnost, výkon a zabezpečení. Kromě funkcí a omezení specifické pro účty úložiště objektů Blob a jeho vrstev úložiště, které byly vyznačenými nad všechno, co dalšího se nezmění.

## <a name="next-steps"></a>Další kroky

### <a name="evaluate-blob-storage-accounts"></a>Vyhodnocení účty úložiště objektů Blob

[Kontrola dostupnosti účtů úložiště objektů Blob podle regionů](https://azure.microsoft.com/regions/#services)

[Vyhodnocení použití účtů aktuální úložiště povolením metriky úložišť Azure](storage-enable-and-view-metrics.md)

[Kontrola cena za úložiště objektů Blob podle regionů](https://azure.microsoft.com/pricing/details/storage/)

[Přenosy dat zaškrtněte ceny](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Začínáme používat účty úložiště objektů Blob

[Začínáme s úložiště objektů Blob Azure](storage-dotnet-how-to-use-blobs.md)

[Přesunutí dat z Azure úložiště a](storage-moving-data.md)

[Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)

[Vyhledejte a prozkoumání účtů úložiště](http://storageexplorer.com/)
