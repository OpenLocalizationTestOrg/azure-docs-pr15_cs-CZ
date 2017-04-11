<properties
   pageTitle="Základní informace o úložiště jezera dat Azure | Microsoft Azure"
   description="Porozumět tomu, co je Azure úložišti jezera a hodnotu, kterou zajišťuje v jiných úložiště dat"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Základní informace o úložiště jezera dat Azure

Úložiště jezera dat Azure je úložiště celopodnikové hyper měřítko pro analytický pracovního vytížení velký data. Jezera dat Azure umožňuje shromáždit informace libovolnou velikost, typ a požití rychlosti jednoho místa pro provozní a průzkumné analýzy.

> [AZURE.TIP] Umožňuje [cesta výuky úložiště jezera dat](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) Seznamte se s ním službu Azure dat jezera úložiště přihlašovacích údajů.

Úložiště jezera dat Azure můžete k nim získat přístup z Hadoop (dostupné u obrázku HDInsight) pomocí REST API kompatibilní s prohlížečem WebHDFS. Konkrétně umožňuje analýzy na uložená data a optimalizaci výkonu scénářích technologie pro analýzu dat. Mimo pole obsahuje všechny možnosti podnikové – zabezpečení správy, škálovatelnost, spolehlivost a dostupnost – základní pro případy použití reálný organizace.


![Jezera dat Azure](./media/data-lake-store-overview/data-lake-store-concept.png)

Některé klíčové funkce jezera dat Azure patří.

### <a name="built-for-hadoop"></a>Vytvořené pro Hadoop

Úložiště jezera dat Azure je systém souborů Apache Hadoop kompatibilní se pracuje s ekosystému Hadoop a Hadoop Distributed soubor systému (HDFS).  Existující HDInsight aplikace nebo služby, které používají rozhraní API WebHDFS můžete snadno integrace s jezera úložiště. Úložiště jezera dat také poskytuje rozhraní REST kompatibilní s prohlížečem WebHDFS aplikací

Dat uložených v úložišti jezera dat lze snadno analyzovat pomocí analytických rámce Hadoop MapReduce ATP podregistru. Microsoft Azure HDInsight clusterů můžete zřízení a nakonfigurované tak, aby přímý přístup k dat uložených v úložišti jezera.

### <a name="unlimited-storage-petabyte-files"></a>Neomezený tarif úložiště, petabyte souborů

Úložiště jezera dat Azure poskytuje neomezený tarif úložiště a je vhodný k ukládání velkého množství dat pro analýzy. Neukládá jakékoli omezení velikostí účtu, velikosti souborů nebo množství dat, která mohou být uloženy v jezera data. Jednotlivé soubory v rozsahu kB až petabytes velikosti a obzvlášť vyplatí uložit libovolný typ dat. Data se ukládají trvale tak, že zobrazíte více kopií a není omezena na dobu trvání, u kterého můžete být data uložena v jezera data.

### <a name="performance-tuned-for-big-data-analytics"></a>Výkon optimalizovaných pro analýzy velký dat

Úložiště jezera dat Azure je vytvořené pro systémy velkém měřítku analytický, které vyžadují rozsáhlé výkon dotazu a analyzovat velké objemy dat. Data jezera rozložení částí souboru přes počtu jednotlivých úložiště serverů To zlepšuje čtení výkon při čtení soubor paralelně k provádění analýzy data.


### <a name="enterprise-ready-highly-available-and-secure"></a>Pole organizace – připravena: Snadno dostupné a zabezpečení

Úložiště jezera dat Azure zajišťuje standardní dostupnost a spolehlivost. Prostředky dat ukládají trvale tak, že záložní kopie pro ochranu před neočekávaným selhání. Podniky můžete použít jezera dat Azure v jejich řešení jako důležitou součástí jejich existující platformy data.

Úložiště jezera dat také poskytuje podnikové zabezpečení uložená data. Další informace najdete v tématu [zabezpečení dat v úložišti jezera dat Azure](#DataLakeStoreSecurity).


### <a name="all-data"></a>Všechna Data

Úložiště jezera dat Azure mohou být uloženy libovolné dat v původním formátu, jako je, aniž by bylo všechny předchozí transformace. Úložiště jezera dat není nutné zadávat schématu a je to možné definovat než načtení dat přímo do jednotlivých analytický framework interpretace dat a definovat schéma v době analýzu. Možnost uložit soubory libovolného velikosti a formáty umožňuje jezera úložiště zpracovat strukturovaná, částečně strukturovaná a Nestrukturovaná data.

Kontejnery úložiště jezera dat Azure pro data jsou v podstatě složek a souborů. Můžete pracovat na uložená data pomocí SDK, Azure portál a Azure Powershellu. Když vložíte data do úložiště pomocí těchto rozhraní a pomocí příslušných kontejnerů, mohou být uloženy libovolné typ dat. Úložiště jezera dat neprovádí žádné zvláštní zpracování dat v závislosti na typu dat, která ukládá.


## <a name="DataLakeStoreSecurity"></a>Zabezpečení dat v úložišti jezera dat Azure

Úložiště jezera dat Azure používá Azure Active Directory pro ověřování a seznamy řízení přístupu (ACL) spravovat přístup k vašim datům.

| Funkce                                 | Popis                              |
|-----------------------------------------|------------------------------------------|
| Ověřování | Úložiště jezera dat Azure integraci s Azure Active Directory (AAD) správy identit a přístup všech dat uložených v úložišti jezera dat Azure. Důsledku integrace jezera dat Azure výhod funkcí AAD včetně vícefaktorové ověřování podmíněného přístupu, řízení přístupu na základě rolí, sledování použití aplikací, zabezpečení sledování a výstrahy, atd. Úložiště jezera dat Azure podporuje protokol OAuth 2.0 k ověření v rozhraní REST. |
| Řízení přístupu                          | Úložiště jezera dat Azure poskytuje řízení přístupu podporou POSIX stylu oprávnění zveřejněné příslušným protokol WebHDFS. V současné verzi může být užitečné ACL na kořenová složka, podsložky, jakož i jednotlivé soubory. ACL, které můžete použít ke kořenové složce se také pro všechny podřízené složky nebo soubory stejně.|

Chcete se dozvědět víc o zabezpečení dat v úložišti jezera. Postupujte podle těchto odkazů.

* Pokyny k zabezpečení dat v úložišti jezera dat najdete v tématu [zabezpečení dat v úložišti jezera dat Azure](data-lake-store-secure-data.md).
* Chcete raději videa? [Podívejte se na toto video](https://mix.office.com/watch/1q2mgzh9nn5lx) o tom, k zabezpečení dat uložených v úložišti jezera.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Kompatibilní se službou Azure úložiště jezera dat aplikace

Úložiště jezera dat Azure je kompatibilní se službou nejčastěji otevřít zdroj součástí ekosystému Hadoop. Také integruje dobře s jinými službami Azure. Díky úložiště jezera dat správný možnost pro vaše potřeby datový úložiště. Postupujte podle těchto odkazů Další informace o úložišti jezera použití jak se otevřít zdroj součásti, jakož i další služby Azure.

* Seznam aplikací otevřít zdroj spolupracovat s úložiště jezera dat najdete v článku [aplikací a služeb kompatibilní se službou Azure dat jezera úložiště přihlašovacích údajů](data-lake-store-compatible-oss-other-applications.md) .
* V tématu [Integrace s jinými službami Azure](data-lake-store-integrate-with-other-services.md) pochopit, jak můžete použít úložiště jezera dat s jinými službami Azure povolit může používat větší okruh scénáře.
* V tématu [scénáře pro používání úložiště jezera dat](data-lake-store-data-scenarios.md) se dozvíte, jak používat úložiště jezera dat v situacích, například ingesting dat zpracování dat, stažení dat a vizualizaci dat.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Co je Azure úložišti jezera systému souborů (adl: / /)?

Úložiště jezera dat můžete k nim získat přístup prostřednictvím nového souborů AzureDataLakeFilesystem (adl: / /), v prostředí Hadoop (dostupné u HDInsight obrázku). Aplikace a služby, které používají adl: / / jsou využít další optimalizaci výkonu, které nejsou ve WebHDFS momentálně dostupné. V důsledku toho úložiště jezera dat vám umožní buď využít optimálního výkonu s parametrem doporučené použití adl: / / nebo používáním rozhraní API WebHDFS přímo k zachování existující kód. Azure HDInsight plně využívá AzureDataLakeFilesystem poskytovat optimálního výkonu v úložišti jezera.

Získat přístup k datům v úložišti jezera pomocí `adl://<data_lake_store_name>.azuredatalakestore.net`. Další informace o tom, jak získat přístup k datům v úložišti jezera dat najdete v článku [zobrazení vlastností uložená data](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Jak začít používat úložiště jezera dat Azure?

V tématu [Začínáme s úložiště jezera dat pomocí portálu Azure](data-lake-store-get-started-portal.md), o tom, jak zřídit úložiště jezera dat na portálu Azure. Jakmile máte zřízení jezera dat Azure, se naučíte, jak nabízené velký data například jezera analýzy dat Azure nebo Azure Hdinsightu pomocí jezera úložiště. Můžete taky vytvořit aplikaci .NET pro vytvoření účtu úložiště jezera dat Azure a provádět operace, například nahrát dat, stáhnout data, atd.

- [Začínáme s Azure dat jezera analýzy](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Začínáme s úložiště jezera dat Azure pomocí .NET SDK](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Videa jezera úložiště dat

V případě potřeby sledování videa a zjistěte, úložiště jezera dat týkající se videa oblasti funkcí.

* [Vytvoření účtu úložiště jezera dat Azure](https://mix.office.com/watch/1k1cycy4l4gen)
* [Použití Průzkumníka dat ke správě dat v úložišti jezera dat Azure](https://mix.office.com/watch/icletrxrh6pc)
* [Připojení dat Azure jezera analýzy úložiště jezera dat Azure](https://mix.office.com/watch/qwji0dc9rx9k)
* [Access Azure jezera úložiště prostřednictvím jezera analýzy dat](https://mix.office.com/watch/1n0s45up381a8)
* [Připojení k úložišti jezera dat Azure Azure HDInsight](https://mix.office.com/watch/l93xri2yhtp2)
* [Access Azure jezera úložiště prostřednictvím podregistru a Prasátko](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Kopírování dat z Azure úložišti jezera a pomocí DistCp (Hadoop Distributed kopii)](https://mix.office.com/watch/1liuojvdx6sie)
* [Přesouvání dat mezi relačním zdroji a úložiště jezera dat Azure pomocí Apache Sqoop](https://mix.office.com/watch/1butcdjxmu114)
* [Průběhu dat pomocí Azure Data Factory pro úložiště jezera dat Azure](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Zabezpečení dat v úložišti jezera dat Azure](https://mix.office.com/watch/1q2mgzh9nn5lx)



