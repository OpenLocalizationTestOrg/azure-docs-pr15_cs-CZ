<properties
    pageTitle="Technologie pro analýzu platformy: Apache bouře porovnání analýzy toku | Microsoft Azure"
    description="Pokud potřebujete pokyny pro výběr platformy analýzy cloudu pomocí Apache bouře porovnání s toku analýzy. Vysvětlení funkcí a rozdíly."
    keywords="technologie pro analýzu platformy platformách technologie pro analýzu, cloudu analýzy platformy, bouře porovnání"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Nápověda výběr streamování platformy analýzy: Apache bouře porovnání analýzy toku Azure

Pokud potřebujete pokyny pro výběr platformy analýzy cloudu pomocí tohoto Apache bouře porovnání analýzy toku Azure. Porozumět tomu, že případy použití hodnotu tvrzení toku analýzy versus Apache bouře jako služba spravovaných na Azure Hdinsightu, abyste mohli zvolit správné řešení ve vašem podniku.

Jak technologie pro analýzu platformách poskytují výhody PaaS řešení, existuje několik hlavní rozlišující možnosti odlišit. Funkce stejně jako omezení z těchto služeb jsou vypsané dole pomůže vám má na řešení, budete muset dosažení své cíle.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Bouřka porovnání analýzy toku: Obecné funkce ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure toku analýzy</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache bouře na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Otevřít zdroj</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ne, je obchodní Microsoft Azure toku analýzy nabízející.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ano, Apache bouře je technologie Apache licenci.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Microsoft podporované</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ano </p>
            </td>
            <td width="246" valign="top">
                <p>
Ano </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Požadavky na hardware</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Existuje žádné hardwarové požadavky. Azure analýzy toku je služba Azure.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Existuje žádné hardwarové požadavky. Apache bouře je služba Azure.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Nejvyšší úrovně jednotku</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Se zákazníky Azure toku analýzy nasazení a sledovat streamování úlohy.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
S Apache bouře na HDInsight zákazníci, nasazení a sledovat celého obrázku, které můžete hostovat více bouře úloh, jakož i ostatní úloh (včetně dávku).
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Price</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Technologie pro analýzu toku účtovány podle objemu dat zpracovaných a počet datových proudů jednotky (/ hod spuštěné úkoly) nutné.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">Ceny Další informace najdete tady.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Apache bouře na HDInsight jednotky nákup je založený na obrázku a bude účtováno založené na čase clusteru je spuštěná, nezávisle na úlohy nasazení.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">Ceny Další informace najdete tady.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Vytváření na každou platformu analýzy##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure toku analýzy</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache bouře na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Možnosti: DSL SQL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ano, snadno se použije podpora jazyků SQL je k dispozici.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ne, uživatelé musí kódu v Java C# nebo použití rozhraní API Trident.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Možnosti: Časový operátory</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Zobrazením agregace a časový spojení jsou podporovány mimo pole.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Časový operátory, musíte provést uživatelem.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Vývojové prostředí</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Interaktivní pro vytváření a ladění experience prostřednictvím portálu Azure vzorová data.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Vývoj, ladění a sledování prostředí je pomocí aplikace Visual Studio .NET uživatelů pro Java a další jazyky pro vývojáře používají integrovaném vývojovém prostředí podle svého výběru.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ladění podpory</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Toku analýzy nabízí stav základní úlohy a operace protokoly jako způsob ladění, ale nenabízí aktuálně pružnost při co a jak velká je součástí protokoly ie: režim s komentářem.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Podrobné protokoly jsou k dispozici pro účely ladění. Existují dva způsoby, jak povrchový protokoly pro uživatele, pomocí aplikace visual Studio nebo uživatel může RDP do obrázku pro přístup k protokoly.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Podpora pro UDF (funkce definované uživatelem)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Momentálně je nepodporuje funkce definované uživatelem.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Funkce definované uživatelem můžete napsaný v jazyce C# Java a jazyka podle svého výběru.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Rozšiřitelný - vlastního kódu</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nejsou podporovány extensible kódem v toku analýzy.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ano, je dostupnost a napište vlastní kód v jazyce C#, Java nebo jiných jazyků podporovaných na bouře.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Zdroje dat a výstupy##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure toku analýzy</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache bouře na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Zdroje zadávání dat</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Podporované vstupní zdroje jsou Azure události rozbočovače a objektů BLOB Azure.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Spojnice nejsou k dispozici pro událost rozbočovače Bus služby, Kafka, atd. Nepodporované spojnic mohou být prováděna prostřednictvím vlastního kódu.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Formáty zadávání dat</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Podporované formáty vstupní jsou Avro, JSON, CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Jakýkoli formát mohou být prováděna prostřednictvím vlastního kódu.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Výstupy</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Streamování úlohy může mít více výstupů. Podporované výstupy: Rozbočovače Azure události, úložišti objektů Blob Azure, Azure tabulky, Azure SQL databáze a PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Podpora pro mnoho výstupy topologie každý výstup může mít vlastní logiky pro příjem zpracování. Mimo pole patří bouře spojnic PowerBI, rozbočovače události Azure, úložiště objektů Blob Azure, Azure DocumentDB, SQL a HBase. Nepodporované spojnic mohou být prováděna prostřednictvím vlastního kódu.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Kódování formátů dat</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Technologie pro analýzu toku vyžaduje UTF-8 formát dat ve sloupcích aby bylo možné.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Data, která formát kódování mohou být použity prostřednictvím vlastního kódu.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Správa a provoz##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure toku analýzy</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache bouře na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Úlohy nasazení modelu</strong>
                </p>
                <p>
                    - <strong>Azure portálu</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>Prostředí PowerShell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nasazení je implementovaná prostřednictvím portálu Azure, prostředí PowerShell a rozhraní REST API.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment implementovaná prostřednictvím portálu Azure Powershellu, Visual Studiu a rozhraní REST API.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Sledování (stat, čítače atd.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Sledování prováděna prostřednictvím portálu Azure a rozhraní REST API.
                </p>
                <p>
Uživatel může taky nakonfigurovat Azure upozornění.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Sledování prováděna prostřednictvím uživatelského rozhraní bouře a rozhraní REST API.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Škálovatelnost:</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Počet jednotek, datových proudů pro jednotlivé úlohy. Jednotlivé datové proudy jednotky zpracuje až 1 MB/s. Maximální počet jednotek 50 ve výchozím nastavení. Přepojit hovor na zvýšit limit.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Počtu uzlů v HDI bouře obrázku. Bez omezení počtu uzlů (horní limit definovaný Azure kvóty). Přepojit hovor na zvýšit limit.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Limity zpracování dat.</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Uživatele můžete změnit velikost nahoru nebo dolů počet jednotek streamování zvětšete zpracování dat nebo optimalizace náklady.
                </p>
                <p>
Změna velikosti až 1 GB/s </p>
            </td>
            <td width="246" valign="top">
                <p>
Uživatele můžete změnit velikost nahoru nebo dolů velikost obrázku podle potřeby.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Zastavit/životopisu</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ukončení a obnovit z posledního místa zastaveno.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ukončení a obnovení z posledního umístěte přestal podle vodoznak.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Aktualizace služby a framework</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automatická oprava s žádné výpadek služeb.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automatická oprava s žádné výpadek služeb.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Nepřerušený provoz prostřednictvím vysoce dostupné s zaručené SLA</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
SLA provozu 99,9 % </p>
                <p>
Automatické obnovení z k chybám </p>
                <p>
Obnovení stavové časový operátorů je předdefinované.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
SLA 99,9 % provozu bouře obrázku. Apache bouře je odolnost proti chybám streamování platformy však zákazníků odpovídá zajistit, aby že jejich streamování úloh spustit bez přerušení.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Pokročilé funkce##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure toku analýzy</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache bouře na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Nejpozději možné příletu a zpracování mimo pořadí událostí</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Předdefinované Konfigurovatelné zásady chcete změnit pořadí, přetáhněte události nebo upravte čas události.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Uživatel musí implementace logiky pro zpracování tento scénář.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Přehled dat</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Odkaz nejsou k dispozici data objektů BLOB Azure maximální velikosti 100 MB v paměti vyhledávání mezipaměti. Aktualizace z referenčních dat se spravuje službu.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Bez omezení velikosti data. Spojnice umožňující HBase DocumentDB, SQL Server a Azure. Nepodporované spojnic mohou být prováděna prostřednictvím vlastního kódu.
                </p>
                <p>
Aktualizace z referenčních dat musí uskutečněných jednotlivými vlastního kódu.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Integrace se službou výukové počítače</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nakonfigurováním publikovat Azure počítače výukové modely jako funkce při vytváření práce ASA <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(soukromé verze preview)</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Dostupné prostřednictvím bouře prvky.
                </p>
            </td>
        </tr>
    </tbody>
</table>
