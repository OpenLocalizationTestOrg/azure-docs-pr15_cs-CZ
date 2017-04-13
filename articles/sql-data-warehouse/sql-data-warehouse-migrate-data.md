<properties
   pageTitle="Přenést data na SQL datový sklad | Microsoft Azure"
   description="Tipy pro migraci dat do Azure SQL datový sklad k vývoji řešení."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="migrate-your-data"></a>Přenést Data
Přesouvat data z různých zdrojů do SQL datový sklad pomocí různých nástrojů.  Kopírování ADF, SSIS a bcp lze použít k tomuto účelu. Však jako množství dat zvýšení by měl by podle vás o rozdělení přenesení dat do kroků. To umožňuje příležitosti optimalizovat každý krok výkon i pro odolnost k zajištění hladkého data migrovat.

Tento článek popisuje nejdřív jednoduchý migrace scénáře ADF kopírovat, SSIS a bcp. Bude pak podobný trochu hlouběji do jak může být optimalizovaný migrace.

## <a name="azure-data-factory-adf-copy"></a>Azure kopírovat Data Factory (ADF)
[Kopírovat ADF][] je součástí [Azure Data Factory][]. Export dat do textových souborů umístěných na místní úložiště, vzdálené textových souborů v úložišti objektů blob Azure nebo přímo do SQL datový sklad můžete kopírovat ADF.

Pokud vaše data spustí v textových souborů a pak je třeba nejprve převést do Azure úložiště objektů blob před zahájením zatížení do SQL datový sklad. Jakmile se převádí se na úložišti objektů blob Azure data můžete znovu použít [Kopírování ADF][] posunout data do SQL datový sklad.

PolyBase také poskytuje možnost vysoký výkon při načítání dat. Však, která znamená, že pomocí dvou nástroje místo jedné. Pokud potřebujete optimálního výkonu potom použijte PolyBase. Pokud chcete setkat i v případě jednoho nástroj (a data nejsou rozsáhlé) ADF je odpověď.

> [AZURE.NOTE] PolyBase vyžaduje datové soubory v UTF-8. Toto je výchozí ADF kopírovat kódování tak nic chcete změnit. Toto je právě připomenutí po změně výchozího chování ADF kopírovat.

Přes sídlo v článku znalostní báze některé skvělé [Příklady ADF][].

## <a name="integration-services"></a>Integrace služby ##
Integration Services (SSIS) je výkonné a flexibilním extrahovat transformace a načíst (ETL) nástroj, který podporuje složitých pracovních postupů, transformace a několik možností načítání dat. Umožňuje jednoduše převod dat Azure nebo jako součást širší migrace SSIS.

> [AZURE.NOTE] SSIS můžete exportovat do UTF-8 bez značku bajt pořadí v souboru. Konfigurace musíte ji nejprve použijte komponentu odvozené sloupce k převodu dat znak v toku dat na stránce 65001 kód UTF-8. Po převedení sloupců na zapisovat data adaptér cíl plochému souboru zajistit, že 65001 taky byla vybrána možnost jako znakové stránky pro soubor.

SSIS se připojí k SQL datový sklad stejně jako se chcete připojit k nasazení serveru SQL Server. Připojení se však nutné používat Správce ADO.NET připojení. By měl taky staráte se pro nastavení "použití hromadné vložit-li k dispozici" nastavení maximalizovat výkon. Naleznete v článku [ADO.NET cíl adaptér][] informace o této vlastnosti

> [AZURE.NOTE] Připojení k datový sklad SQL Azure pomocí OLEDB není podporovaná.

Kromě toho je vždy možné, balíčku může termín docházet k chybám problémy omezení nebo se sítí. Návrh balíčky tak jejich lze obnovit v místě nepovede, bez opakování práci, která dokončen před selháním.

Další informace naleznete v [dokumentaci SSIS][].

## <a name="bcp"></a>BCP
BCP je příkazového řádku nástroj, který slouží k plochému souboru importem a exportem dat. Některé transformace může proběhnout při exportu dat. K provádění jednoduchých transformací pomocí dotazu můžete vybírat a transformace dat. Po exportu, textových souborů můžete pak načíst přímo do cílové databázi SQL datový sklad.

> [AZURE.NOTE] Je často dobré zapouzdřit transformace používaný během export dat v listu ve zdrojovém systému. Zajistíte, že logiku zachovány a proces se opakující.

Výhody bcp jsou:

- Chcete-li jednoduché. BCP příkazy jsou jednoduchá k vytvoření a spuštění
- Proces znovu spustitelná načítání. Jednou exportovaný mohou být načíst spouštět počtu opakování.

Omezení bcp jsou:

- BCP funguje s tabulkovými pouze textových souborů. Se soubory xml ATP JSON nefunguje
- BCP nepodporuje exportu do UTF-8. To můžou bránit programu pomocí PolyBase bcp export dat
- Možnosti transformace dat jsou omezené na oblasti exportovat a jsou jednoduchá v přírodě
- BCP nebyla byly upraveny jako robustní při načítání dat přes internet. Nestability síti může způsobit chybu načíst.
- BCP závisí na schématu právě prezentovat v cílové databázi před zatížení

Další informace najdete v tématu [použití bcp načíst data do SQL datový sklad][].

## <a name="optimizing-data-migration"></a>Optimalizace migraci dat
Proces migrace SQLDW dat se dají efektivně rozdělit do tří samostatných kroků:

1. Export zdrojových dat
2. Přenos dat Azure
3. Načtení do cílové SQLDW databáze

Každý krok můžete jednotlivě optimalizováno pro vytváření robustních, znovu spustitelná a pružné přenesení s maximalizuje výkon při každém kroku.

## <a name="optimizing-data-load"></a>Optimalizace načtení dat
Možnosti pro chvíli; v opačném pořadí načíst data nejrychleji prostřednictvím PolyBase. Optimalizace pro procesu načíst PolyBase umístí požadavky na předchozí kroky, je vhodné toto vysvětlení předem. Jsou:

1. Kódování datových souborů aplikace
2. Formát datových souborů aplikace
3. Umístění datových souborů

### <a name="encoding"></a>Kódování
PolyBase vyžaduje datové soubory být kódování UTF-8. To znamená, že při exportu dat musí vyhovovat tohoto požadavku. Pokud data obsahují pouze základní znaků ASCII (ne rozšířené ASCII) potom tyto mapy přímo na standard kódování UTF-8 a nemusíte bát, příliš kódování. Ale pokud data obsahují žádné speciální znaky, například přehlásky, zvýraznění nebo symboly nebo datům podporuje jazyky s psaním než latinku pak budete muset zajistit, aby vaše soubory exportu správně kódování UTF-8.

> [AZURE.NOTE] BCP nepodporuje export dat do UTF-8. Nejlepší možností tedy použít Integration Services nebo kopírovat ADF pro export dat. Je vhodné poukázání, zda značka pořadí bajtů UTF-8 (BOM) nepožaduje v datovém souboru.

Všechny soubory kódování UTF-16 pomocí muset být znovu ***předchozí*** došlo k zápisu dat přenos.

### <a name="format-of-data-files"></a>Formát datových souborů aplikace
PolyBase vyžaduje ukončení pevné řádku \n nebo nový řádek. Datové soubory aplikace musí odpovídat této standardní. Nejsou k dispozici omezení ukončení řetězec nebo sloupec.

Je třeba definovat každý sloupec v souboru jako součást externí tabulku v PolyBase. Zkontrolujte, jestli podporují všechny exportovaného sloupce a typy shody požadované standardy.

Přejděte zpátky [migrace schématu] článek podrobnosti na podporované datové typy.

### <a name="location-of-data-files"></a>Umístění datových souborů
SQL datový sklad používá PolyBase k načtení dat z úložiště objektů Blob Azure výhradně. Proto data musí mít nejdřív přenesete do úložiště objektů blob.

## <a name="optimizing-data-transfer"></a>Optimalizace přenos dat
Jednou z nejnižší díly migraci dat je přenos dat do Azure. Nejen lze šířka pásma problému, ale spolehlivosti sítě můžete vážně omezovat i průběh. Ve výchozím nastavení migraci dat Azure je tak, aby byly prakticky pravděpodobně pravděpodobnost výskytu chyb přenos přes internet. Tyto chyby však může vyžadovat opětovné odeslání úplně nebo částečně dat.

Naštěstí máte několik možností ke zlepšení rychlosti a odolnost tohoto procesu:

### <a name="expressroute"></a>[ExpressRoute][]
Je vhodné zvažte použití [ExpressRoute][] pro dosažení vyššího přenosu. [ExpressRoute][] poskytuje vytvořeného soukromé připojení k Azure tak připojení nepřekročí veřejné Internetu. Jedná se o ne prostředky povinné krok. Však ho zlepšíte výkon při předání dat Azure z místního nebo společné umístění zařízení.

Výhody používání [ExpressRoute][] jsou:

1. Vyšší spolehlivost:
2. Rychlejší rychlosti sítě
3. Nižší latence sítě
4. vyšší zabezpečení sítě

[ExpressRoute][] je užitečné pro počet scénářích. nejen migrace.

Zúčastněné? Další informace a ceny prosím Navštěvujte blog o [si přečtěte následující dokumentaci ExpressRoute][].

### <a name="azure-import-and-export-service"></a>Azure Import a Export služby
Azure Import a Export služby je proces převodu dat sice pro velké (GB ++) do rozsáhlé (TB ++) přenosy dat do Azure. Zahrnuje zápisu dat do disků a dodávky Azure datacentrem. Obsah disku bude pak načíst do Azure úložiště objektů BLOB vaším jménem.

Souhrnné zobrazení exportu vytvoříte import vypadá takto:

1. Konfigurace kontejneru úložišti objektů Blob Azure importovaná data
2. Export dat do místní úložiště
2. Zkopírujte data do 3,5 SATA II/III pevných discích nástrojem [Azure Import nebo Export]
3. Vytvoření úlohy importu pomocí Azure Import a Export služba poskytující deníku soubory vytvořené pomocí [Azure Import nebo Export nástroj]
4. Dodat disků jmenovaný Azure datovém centru
5. K úložišti objektů Blob Azure kontejneru je převedení dat
6. Načtení dat do SQLDW pomocí PolyBase

### <a name="azcopy-utility"></a>Nástroj [AZCopy][]
Nástroj [AZCopy][] je skvělým nástrojem pro získání dat do objektů BLOB Azure úložiště. To je určená pro malé (MB ++) obrovské přenosy dat (GB ++). [AZCopy] je také určena zajistit dobré pružné výkon při převodu dat Azure a tak se obzvlášť vyplatí pro přenos kroku data. Jednou přenesených načtení dat pomocí PolyBase do SQL datový sklad. Můžete taky začlenit AZCopy do vaší balíčků SSIS pomocí úkol "Spustit proces".

Použití AZCopy se musíte nejdřív si stáhněte a nainstalujte. [Verze výrobního][] a [verze preview][] k dispozici není.

Nahrajte soubor ze systému souborů budete potřebovat příkaz podobné tomuto níže:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Přehled procesu nejvyšší úrovně, může být:

1. Konfigurace kontejneru Azure úložiště objektů blob pro importovaná data
2. Export dat do místní úložiště
3. AZCopy dat v kontejneru úložišti objektů Blob Azure
4. Načtení dat do SQL datový sklad pomocí PolyBase

Plná si přečtěte následující dokumentaci k dispozici: [AZCopy][].

## <a name="optimizing-data-export"></a>Optimalizace export dat
Kromě zajistit, že exportovat vyhovuje požadavkům rozloženy PolyBase můžete taky řešení optimalizace exportovat data, která chcete zlepšit proces dál.

> [AZURE.NOTE] Jak PolyBase vyžaduje data, která mají být ve formátu UTF-8, že je pravděpodobně bude používat bcp proveďte export dat. BCP nepodporuje výstup datové soubory na UTF-8. SSIS nebo zkopírovat ADF jsou mnohem lepší hodí k provádění tohoto typu exportovat data.

### <a name="data-compression"></a>Komprese dat
PolyBase můžete číst data gzip komprimovány. Pokud budete moci komprimovat data soubory gzip bude minimalizovat množství dat stisknuté v síti.

### <a name="multiple-files"></a>Více souborů
Rozdělení rozsáhlých tabulek do více souborů umožňuje nejen kvůli zlepšení export rychlosti, je taky dobré s přenos re-startability a celkové možnosti správy dat jednou v úložišti objektů blob Azure. Jednou z mnoha hodní funkcí PolyBase je, že bude číst všechny soubory ve složce a považovat za jednu tabulku. Proto je vhodné vyčlenění souborů pro každou tabulku do vlastní složky.

PolyBase taky podporuje funkci jmenoval "rekurzivní složky přecházení". Pomocí této funkce můžete dále vylepšit organizace exportovaná data ke zlepšení správy dat.

Další informace o načítání dat s PolyBase najdete v tématu [Použití PolyBase načíst data do SQL datový sklad][].


## <a name="next-steps"></a>Další kroky
Další informace o migraci najdete v článku [migrace řešení pro systém SQL Data Warehouse][].
Další tipy vývoj najdete v tématu [Přehled vývoje][].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/storage-use-azcopy.md
[Kopírovat ADF]: ../data-factory/data-factory-data-movement-activities.md 
[ADF vzorky]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[Přehled vývoje]: sql-data-warehouse-overview-develop.md
[Migrace řešení pro systém SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Načíst data do SQL datový sklad pomocí bcp]: sql-data-warehouse-load-with-bcp.md
[Načíst data do SQL datový sklad pomocí PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute si přečtěte následující dokumentaci]: http://azure.microsoft.com/documentation/services/expressroute/

[verze výrobního]: http://aka.ms/downloadazcopy/
[verze Preview]: http://aka.ms/downloadazcopypr/
[Určení adaptér ADO.NET]: https://msdn.microsoft.com/library/bb934041.aspx
[Přečtěte následující dokumentaci pro SSIS]: https://msdn.microsoft.com/library/ms141026.aspx
