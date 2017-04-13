<properties
    pageTitle="Šifrování služby Azure úložiště dat u ostatních | Microsoft Azure"
    description="Šifrování úložišti objektů Blob Azure na straně služby při ukládání dat pomocí funkce šifrování úložiště služby Azure a podoby při načítání data."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="robinsh"/>

# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Šifrování služby Azure úložiště dat u ostatních

Azure úložiště služby šifrování (SSE) dat u ostatních vám pomůže zamknout nebo chránit data splňují organizační zabezpečení a dodržování předpisů závazky. Pomocí této funkce úložišti Azure automaticky šifruje data před uchování k základnímu úložišti a dešifruje před načítání. Šifrování, dešifrování a správy klíčů jsou úplně průhledné uživatelům.

Následující části obsahují, že dojde k podrobné pokyny k používání funkcí úložiště služby šifrování i Podporované scénáře a uživatele.

## <a name="overview"></a>Základní informace

Azure úložiště umožňuje komplexní sady možnosti zabezpečení, které dohromady vývojářům umožňuje vytvářet zabezpečené aplikace. Data lze zabezpečit mezi aplikací a Azure pomocí [Šifrování na straně klienta](storage-client-side-encryption.md), HTTPs nebo SMB 3.0. Úložiště služby šifrování poskytuje šifrování na rest, zpracování šifrování, dešifrování a správy klíčů úplně průhledným způsobem. Všechna data je šifrovaná pomocí 256 [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu z nejvyšší blokové šifry k dispozici.

SSE funguje zašifrováním data zapisuje k úložišti Azure a se dá použít pro objekty BLOB blok, objekty BLOB stránky a připojit objektů BLOB. To funguje pro následující:

-   Univerzální úložiště a účty úložiště objektů Blob
-   Standardní úložiště a Premium úložiště 
-   Všechny redundance vyrovnání (LRS ZRS, GRS, Vzdálená pomoc GRS)
-   Azure správce prostředků úložiště účty (ale ne klasické) 
-   Všechny oblasti

Zapnutí nebo vypnutí úložiště služby šifrování úložiště účtu, přihlaste se k [portálu Azure](https://azure.portal.com) a vyberte účet úložiště. Na zásuvné nastavení hledejte části objektů Blob služba uvedeno v této snímek a zaškrtněte políčko šifrování.

![Možnost portálu snímek obrazovky zobrazující šifrování](./media/storage-service-encryption/image1.png)

Po klepnutí na tlačítko Nastavení šifrování, můžete povolit nebo zakázat úložiště služby šifrování.

![Vlastnosti portálu snímek obrazovky zobrazující šifrování](./media/storage-service-encryption/image2.png)

##<a name="encryption-scenarios"></a>Šifrování scénáře

Úložiště služby šifrování může být užitečné úrovni účtu úložiště. Podporuje následující scénáře zákazníka:

-   Šifrování objektů BLOB bloku přidat objektů BLOB a objekty BLOB stránky.

-   Šifrování archivované VHD a šablon dáno Azure z místního.

-   Šifrování podkladového OS a dat disků IaaS VMs vytvořené pomocí svého VHD

SSE platí následující omezení:

-   Šifrování osnovu klasické úložiště nepodporuje.

-   Šifrování klasické úložiště, které účty migruje se na správce prostředků úložiště účty nepodporuje.

-   Existující Data – SSE šifruje nově vytvořený data pouze po povolení šifrování. Pokud například vytvořit nový účet správce prostředků úložiště, ale nechcete zapnout šifrování a pak nahrajte k tomuto účtu úložiště objektů BLOB nebo archivované VHD a zapněte SSE, nebudou tyto objekty BLOB šifrovaná, pokud jsou přepsat nebo zkopírovat.

-   Podpora Marketplace - povolit šifrování VMs vytvořená z pomocí [Azure portál](https://portal.azure.com), prostředí PowerShell a rozhraní příkazového řádku Azure Marketplace. Obrázek základní virtuální pevný disk zůstane nešifrovaném; šifrování bude však všech zápisu provést po OM má nespředený nahoru.

-   Tabulka, fronty, a nebude zašifrovaných souborů data.

##<a name="getting-started"></a>Začínáme

###<a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Krok 1: [Vytvoření nového účtu úložiště](storage-create-storage-account.md).

###<a name="step-2-enable-encryption"></a>Krok 2: Povolit šifrování.

Šifrování pomocí [Azure portál](https://portal.azure.com)lze povolit.

> [AZURE.NOTE] Pokud chcete programově povolit nebo zakázat šifrování služba úložiště na účet úložiště, můžete [Azure úložiště zdroje poskytovatele REST API](https://msdn.microsoft.com/library/azure/mt163683.aspx), [Úložiště zdroje poskytovatele klienta knihovny pro .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](../powershell-install-configure.md)nebo [Azure rozhraní příkazového řádku](storage-azure-cli.md).

###<a name="step-3-copy-data-to-storage-account"></a>Krok 3: Zkopírování dat do účtu úložiště

Pokud povolíte SSE na účet úložiště a zapište objektů BLOB k tomuto účtu úložiště že objekty BLOB bude šifrovaná. Všechny objekty BLOB už součástí tohoto účtu úložiště nebude zašifrovaný, dokud se přepsat. Zkopírujte data z jednoho úložiště účtu jedna z nich SSE šifrované, nebo dokonce povolit SSE a zkopírujte objekty BLOB z jednoho kontejneru do jiného do jistotu, že je šifrovaná data z předchozích. K tomu můžete použít některou z následujících nástrojů.

#### <a name="using-azcopy"></a>Použití AzCopy

AzCopy je nástroj příkazového řádku Windows navržené ke kopírování dat z úložiště objektů Blob Microsoft Azure, soubor a tabulky pomocí jednoduchých příkazů optimální výkon a. Můžete to zkopírujte vaší objektů blob z jednoho účtu úložiště na jiný účet, který má povolené SSE. 

Další informace, navštivte [přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md).

#### <a name="using-the-storage-client-libraries"></a>Použití knihoven úložiště klienta

Kulatý data můžete kopírovat do a z úložiště objektů blob nebo mezi účty úložiště pomocí našich bohatou sadu funkcí včetně .NET C++, Java, Android, Node.js, PHP, Python a skutečné knihoven úložiště klienta.

Další informace, navštivte naše [začít pracovat s úložiště objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Pomocí Průzkumníka úložiště

Průzkumník úložišť slouží k vytvoření účtů úložiště, nahrajte a stažení dat, zobrazení obsahu objektů BLOB a procházet adresáře. Můžete jeden z těchto článků odeslání ke svému účtu úložiště objektů BLOB pomocí šifrování povoleno. S některé Průzkumník úložiště můžete taky zkopírovat data z existující úložiště objektů blob do různých kontejner úložiště účet nebo účet nové úložiště, který má povolené SSE.

Další informace, navštivte [Průzkumník úložiště Azure](storage-explorers.md).

###<a name="step-4-query-the-status-of-the-encrypted-data"></a>Krok 4: Stav šifrovaná data dotazu

Byly nasazeny aktualizovanou verzi knihoven úložiště klienta, který umožňuje dotazu stav objektu a zjistit, pokud je zašifrován nebo ne. Příklady se přidá na tento dokument v blízké budoucnosti.

[Zobrazí vlastnosti účtu](https://msdn.microsoft.com/library/azure/mt163553.aspx) a ověřte, zda účtu úložiště šifrování povoleno zobrazení vlastností účtu úložiště na portálu Azure mezitím můžete volání.

##<a name="encryption-and-decryption-workflow"></a>Šifrování a dešifrování pracovního postupu

Tady je stručný popis šifrování/dešifrování pracovního postupu:

-   Zákazník umožňuje šifrování účtu úložiště.

-   Pokud zákazník zapíše nová data (umístění objektů Blob Vložit blok, umístění stránky, atd.) k úložišti objektů Blob; Každý zápisu musí být zašifrovaný pomocí 256 [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu z nejvyšší blokové šifry k dispozici.

-   Zákazník musí pro přístup k datům (získat objektů Blob, atd.), dat při automaticky dešifrovaný před návratu do uživatele.

-   Pokud je zakázané šifrování nové zápisy jsou už šifrované a existující šifrovaná data zůstane šifrované, dokud přepsat uživatelem. Při aktivované šifrování, bude šifrovaná zápisy k úložišti objektů Blob. Stav data nezmění s uživatelem přepínání mezi povolení nebo zakázání šifrování účtu úložiště.

-   Všechny šifrovacího klíče jsou uloženy zašifrovaných a spravuje Microsoft.

##<a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Nejčastější dotazy k šifrování úložiště služby dat u ostatních

**Otázka: Můžu mít existujícího účtu klasické úložiště. Můžete na něm povolit SSE?**

Odpověď: už SSE je podporována pouze správce úložiště účtů.

**Otázka: Jak můžu zašifrovat dat v účtu klasické úložiště?**

Odpověď: můžete vytvořit nový účet správce úložiště a zkopírujte data pomocí [AzCopy](storage-use-azcopy.md) z vašemu stávajícímu účtu klasické úložiště nově vytvořený účet správce prostředků úložiště. 

Další možností je migrace účtu klasické úložiště k účtu úložiště spravovat zdroje. Další informace najdete v tématu [Platformy podporované migrace z IaaS zdroje z klasického správci zdrojů](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/).

**Otázka: Můžu mít existujícího účtu úložiště správce prostředků. Můžete na něm povolit SSE?**

Odpověď: Ano, ale jenom nově napsali objektů BLOB budou šifrovány. Není vraťte a šifrování data, která byla už prezentovat. 

**Otázka: můžu chcete šifrovat aktuální data do existujícího účtu správce prostředků úložiště?**

Odpověď: můžete povolit SSE kdykoli v účtu úložiště správce prostředků. Objekty BLOB, které již byly prezentovat však nebude zašifrován. Šifrování těchto objektů BLOB, můžete je zkopírovat na jiný název nebo jiný kontejner a odeberte nešifrovaném verze.

**Otázka: používám Premium úložiště; můžete použít SSE?**

Odpověď: Ano SSE podporována ve standardní úložiště a Premium úložiště.

**Otázka: Pokud můžu vytvořit nový účet úložiště a povolení SSE a pak vytvořit nové OM pomocí tohoto účtu úložiště, znamená to, že můj OM šifrovaná?**

Odpověď: Ano. Disků vytvořili, které používají nový účet úložiště, budou šifrovány také jsou vytvořené po SSE aktivované. Pokud OM byl vytvořen pomocí Azure Tržiště, základní obrázek virtuální pevný disk zůstane nešifrovaném; šifrování bude však všech zápisu provést po OM má nespředený nahoru.

**Otázka: můžete vytvářet nové účty úložiště s SSE povoleno pomocí prostředí PowerShell Azure a rozhraní příkazového řádku Azure?**

Odpověď: Ano.

**Otázka: jaká další úložiště Azure nákladů Pokud je povoleno SSE?**

Odpověď: neexistuje žádné další náklady.

**Otázka: kdo má na starosti šifrovacího klíče?**

A: klávesy se spravuje Microsoft.

**Otázka: můžu používat vlastní šifrovacího klíče?**

A: pracujeme na nabízí možnosti pro zákazníky převést legendou šifrování.

**Otázka: můžou odvolat přístup k šifrovacího klíče?**

Odpověď: v tuto chvíli ne klávesy jsou plně spravuje Microsoft.

**Otázka: je standardně zapnutá SSE při vytvoření nového účtu úložiště?**

A: SSE není standardně; pomocí portálu Azure můžete ji povolit. Můžete také programově tuto funkci povolíte pomocí rozhraní REST API úložiště zdroje poskytovatele.

**Otázka: jak se to liší od šifrovacím Azure?**

Odpověď: Tato funkce slouží k šifrování dat v úložišti objektů Blob Azure. Šifrování disku Azure slouží k šifrování OS a dat disků v IaaS VMs. Podrobné informace navštivte naše [Průvodce zabezpečením úložiště](storage-security-guide.md).

**Otázka: Co když mám povolit SSE a potom přejděte a šifrování disku Azure na discích?**

A: to bude fungovat bez problémů. Šifrování dat bude obě metody.

**Otázka: Můj účet úložiště je nastavit tak, aby se replikovat geo redundantně. Pokud SSE si mám povolit, budou si záložní kopii také šifrovány?**

Odpověď: Ano, všechny kopie účtu úložiště jsou šifrovány a všechny možnosti redundance – místně nadbytečné úložiště (LRS) zóny přebytečné úložiště (ZRS), Geo přebytečné úložiště (GRS) a úložiště Geo přebytečné přístup pro čtení (Vzdálená pomoc GRS) – jsou podporovány.

**Otázka: můžu není možné povolit šifrování v mém účtu úložiště.**

Odpověď: to je účtem správce prostředků úložiště? Klasický úložiště účty nejsou podporované. 

**Otázka: je SSE povolen pouze v určité oblasti?**

Odpověď: SSE je k dispozici ve všech oblastech. 

**Otázka: jak se lze obrátit Pokud máte problémy nebo chcete sdělit svůj názor?**

Odpověď: kontaktujte [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) problémy související s úložiště služby šifrování.

##<a name="next-steps"></a>Další kroky

Azure úložiště umožňuje komplexní sady možnosti zabezpečení, které dohromady vývojářům umožňuje vytvářet zabezpečené aplikace. Další informace Navštěvujte blog o [Průvodce zabezpečením úložiště](storage-security-guide.md).
