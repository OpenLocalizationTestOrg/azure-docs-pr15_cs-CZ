<properties
    pageTitle="Úvod k základnímu úložišti | Microsoft Azure"
    description="Základní informace o úložišti Azure, společnosti Microsoft online úložný prostor v cloudu. Naučte se používat nejlepším řešením dostupné cloudové úložiště v aplikacích."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="introduction-to-microsoft-azure-storage"></a>Úvod k základnímu úložišti Microsoft Azure

## <a name="overview"></a>Základní informace

Azure úložiště je řešení cloudové úložiště pro moderní aplikace, které jsou závislé na životnost, dostupnost a škálovatelnost podle potřeby svých zákazníků. Tento článek vývojáři, v oboru IT a obchodní rozhodnutí o tvůrci se dozvíte:

- Co je Azure úložiště, a jak můžete převzít výhod ho v cloudu, mobilní serveru a desktopových aplikací
- Jaké druhy data se mohou být uloženy služby Azure úložiště: objektů blob dat (objekt), NoSQL tabulková data, zpráv a sdílení souboru.
- Jak se spravuje přístup k vašim datům v úložišti Azure
- Jak dat Azure úložiště je nastavená jako trvalé prostřednictvím redundance a replikace
- Kam se obrátit další k vytvoření prvního aplikace úložišti Azure

Získáte do začátků s úložištěm Azure rychle najdete v článku [Začínáme s Azure úložiště na pět minut](storage-getting-started-guide.md).

Podrobnosti o nástroje knihovny a další zdroje pro práci s Azure úložiště, najdete v následujících [Další kroky](#next-steps) .

## <a name="what-is-azure-storage"></a>Co je Azure úložiště?

Výpočet umožňuje nové scénáře pro aplikace vyžadující scalable trvalé a vysoce dostupné úložiště pro svoje data – tedy přesně proč vyvinuté Microsoft Azure úložiště v cloudu. Kromě umožňuje vývojářům vytvářet rozsáhlé aplikace podporuje nové scénáře, úložišti Azure také poskytuje úložiště základ pro Azure virtuálních počítačích, další testament k jeho odolnosti.

Azure úložiště je datových scalable mohou být uloženy a proces stovky TB dat pro podporu scénáře velký dat vyžadované vědeckých, finanční analýzy a multimediálních aplikací. Nebo se můžou být uloženy krátkých data potřebná pro web small business. Ať spadají vašim potřebám, můžete zaplatit pouze data, která máte uložený. Azure úložiště aktuálně ukládá desítky trillions objektů jedinečné zákazníka a průměrně zpracovává miliony požadavků na sekundu.

Azure úložiště je pružná, abyste mohli navrhování aplikací pro velkou cílovou skupinu globální a měřítko těchto aplikacích v případě potřeby – jak z hlediska množství dat uložených a počet žádosti u ní. Platíte pouze můžete použít, a pouze v případě, že ho použijete.

Azure úložiště používá rozdělení automatického systém, automaticky zatížení zůstatků data podle přenosy. To znamená, že jako požadavky na zvětšit aplikace úložišti Azure automaticky přidělí vhodné prostředky k jejich splnění.

Azure úložiště přístupný z kdekoli ve světě, z libovolného typu aplikace, zda běží v cloudu, na ploše, místního serveru nebo mobilní telefon nebo tablet zařízení. Můžete Azure úložiště v mobilní scénáře, které aplikace ukládá podmnožinu dat na zařízení a synchronizuje s úplnou sadu dat uložených v cloudu.

Azure úložiště podpora klientů pomocí různých sady operačních systémech (včetně Windows a Linux) a různé jazyky (včetně .NET Java, Node.js, Python, skutečné, PHP a C++ a Mobilní jazyky) pro pohodlný vývoj. Azure úložiště také poskytuje zdroje dat pomocí jednoduchého REST API, které jsou k dispozici klientovi schopné odesílání a přijímání dat prostřednictvím protokolu HTTP/HTTPS.

Azure Premium úložiště poskytuje podpora výkonných maximum, minimum latence disku vstupu a výstupu náročné pracovní vytížení na virtuálních počítačích Azure. S Azure Premium úložiště můžete připojit více disků trvalý dat do virtuálního počítače a nakonfigurovat tak, aby splňovaly vašim požadavkům na výkon. Každý disk dat získáváte disku SSD v úložišti Premium Azure pro maximální výkon vstupu a výstupu. V tématu [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](storage-premium-storage.md) další podrobnosti.

## <a name="introducing-the-azure-storage-services"></a>Představení služby Azure úložiště

Azure úložiště poskytuje následující čtyři služby: objektů Blob úložiště, úložiště tabulek, úložiště fronty a ukládání souborů.

- Úložiště objektů BLOB jsou uložená data nestrukturovaná objektu. Objekt blob může být jakýkoli typ text nebo binární data, jako je dokument, soubor media nebo instalační program aplikace. Úložiště objektů blob se taky nazývá úložiště objektu.
- Tabulky jsou uloženy strukturovaných datové sady. Úložiště tabulek je NoSQL úložiště klíč atribut dat, který umožňuje rychlý vývoj a rychlý přístup k velké objemy dat.
- Úložiště fronty poskytuje spolehlivé zpráv pro zpracování pracovního postupu a komunikaci mezi součástmi cloudovým službám.
- Uložení souboru nabízí sdílené úložiště pro starší verze aplikace, které používají protokol standardní SMB. Azure virtuálních počítačích a cloudovým službám můžete zužitkování datového souboru součástí aplikace prostřednictvím připojené sdílené položky a místní aplikací přístup dat souborů ve sdílené složce prostřednictvím služby souborů rozhraní REST API.

Účet Azure úložiště je zabezpečené účet, který umožňuje přístup ke službám v úložišti Azure. Účtu úložiště poskytuje jedinečných názvů pro ukládání prostředků. Na následujícím obrázku vidíte vztahy mezi prostředky Azure úložiště v úložišti účtu:

![Prostředků Azure úložiště](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Úložiště objektů BLOB

Úložiště objektů Blob uživatelům s velkým množstvím dat nestrukturovaná objekt uložte v cloudu, nabízí efektivní a scalable řešení. Úložiště objektů Blob slouží k ukládání obsahu, jako:

- Dokumenty
- Sociální data například fotky, videa, Hudba a blogů
- Zálohování soubory, počítačů, databází a zařízení
- Obrázky a text webových aplikací
- Konfigurace dat získáte cloudu
- Velký dat, například protokoly a jiných velkých datových sad

Každý objektů blob jsou uspořádány do kontejneru. Kontejnery taky poskytují užitečný způsob, jak přiřadit zásady zabezpečení skupiny objektů. Účet úložiště může obsahovat libovolný počet kontejnery a kontejneru může obsahovat libovolný počet objektů BLOB maximálně 500 TB kapacitu úložiště účtu.  

Kulatý úložiště nabízí tři typy objektů BLOB, zablokovat objektů BLOB, přidávací dotaz objektů BLOB a objekty BLOB stránky (disků).

- Objekty BLOB blok optimalizovaná pro přenos a ukládání cloudu objektů a jsou vhodné pro ukládání dokumentů, mediální soubory, zálohování atd.
- Přidání objektů blob se podobají objektů BLOB blokování, ale jsou optimalizovaná pro operace připojení. Přidat blob aktualizovat lze pouze přidáním nového bloku do konce Přidání objektů BLOB jsou Dobrá volba, pokud scénáře například protokolování, kde je potřeba zapsat pouze na konci objektů blob nová data.
- Objekty BLOB stránky jsou optimalizované pro představující IaaS disků a podporu náhodné zapíše a může být až 1 TB velikost. Síť Azure virtuální počítač připojen IaaS disk je virtuální pevný disk uložených jako objektů blob stránky.

Kde omezeními sítě provést nahrávání nebo stahování dat k úložišti objektů Blob než nerealistické velkých sad dat můžete dodat pevném disku společnosti Microsoft k importu nebo exportu dat přímo z centra data. V tématu [Import nebo Export služby Microsoft Azure pro přenos dat k úložišti objektů Blob](storage-import-export-service.md).

## <a name="table-storage"></a>Úložiště tabulek

Moderní aplikace často požadovat úložiště dat s větší škálovatelnost a flexibilitu než předchozí generace software potřebný. Úložiště tabulek nabízí vysoce dostupné, datových scalable úložiště, takže aplikaci můžete přizpůsobit automaticky odpovídají požadavkům uživatelů. Úložiště tabulek je úložiště klíč/atribut NoSQL společnosti Microsoft – má schemaless návrhu, díky liší od tradiční relační databáze. S úložištěm schemaless dat není těžké si přizpůsobení dat jako potřeb evolve aplikace. Úložiště tabulek je snadno se použije, aby vývojáři můžete rychle vytvořit aplikací. Přístup k datům je rychlé a efektivní pro všechny typy aplikací.  Úložiště tabulek je obvykle výrazně nižší náklady tradiční syntaxe jazyka SQL pro podobné objemy dat.

Úložiště tabulek je klíč atribut úložiště, což znamená uložení všechny hodnoty v tabulce s názvem zadaný vlastnost. Název vlastnosti mohou sloužit k filtrování a zadáním kritérií výběru. Kolekci vlastnosti a jejich hodnot tvoří entity. Protože úložiště tabulek je schemaless, dvě entity ve stejné tabulce může obsahovat různé kolekce vlastnosti a tyto vlastnosti mohou být různých typů.

Úložiště tabulek můžete použít k ukládání flexibilní datové sady, například uživatelská data pro webové aplikace, adresáře, informace o zařízení a jiný typ metadata, která vyžaduje službu.  V tabulce mohou být uloženy libovolné číslo entity a účet úložiště může obsahovat libovolný počet tabulek nahoru limit kapacitu úložiště klienta.

Jako objekty BLOB a fronty vývojáři můžou spravovat a tabulka aplikace access úložiště pomocí standardní ZBÝVAJÍCÍ protokoly, ale úložiště tabulek taky podporuje podmnožinu protokolu OData zjednodušení rozšířené dotazy funkcí a povolení JSON a AtomPub (XML na základě) formáty.

Pro dnešní Internetové aplikace nabídnout NoSQL databáze jako úložiště tabulek Oblíbené alternativy tradiční relační databáze.

## <a name="queue-storage"></a>Úložiště fronty

Při návrhu aplikace pro měřítko, součástí aplikace jsou často oddělené, takže lze přizpůsobit nezávisle na sobě. Úložiště fronty řešením je spolehlivé zpráv asynchronní komunikace mezi součástí aplikace, zda jsou spuštěné v cloudu, na stolním počítači, na místního serveru nebo na mobilním zařízení. Úložiště fronty také podporují správu asynchronní úkoly a vytváření pracovních postupů obrázku.

Účet úložiště mohou obsahovat libovolný počet dotazů. Fronta může obsahovat libovolný počet zpráv až limit kapacitu úložiště klienta. Jednotlivé zprávy může být až 64 KB.

## <a name="file-storage"></a>Ukládání souborů

Azure úložiště souborů nabízí cloudové SMB sdílených souborů, takže můžete migrovat starší verze aplikace využívající sdílené složky na Azure rychle a bez nákladné přepíše. S úložiště souborů Azure spuštěné v Azure virtuálních počítačích nebo cloudovým službám připojit sdílení souborů v cloudu, stejně jako desktopové aplikace připojí Typická SMB sdílet. Libovolný počet součástí aplikace lze připojit a přístup ke sdílené složce úložiště souborů současně.

Protože sdílené složky úložiště je standardní SMB sdílené složky, můžete spuštěné v Azure přístup k datům v dialogu sdílet prostřednictvím systému souborů rozhraní API vstupu a výstupu. Vývojáři proto využít své existující kód a dovednosti, které se mají migrovat stávající aplikace. V oboru IT můžete pomocí rutin prostředí PowerShell vytvářet, připojení a spravovat úložiště sdílených v rámci správy Azure aplikací.

Podobně jako jiné služby Azure úložiště úložiště souborů poskytuje rozhraní REST API pro přístup k datům ve sdílené složce. Místní aplikace můžete volat úložiště souborů rozhraní REST API pro přístup k datům ve sdílené složce. Tímto způsobem licenci enterprise můžete rozhodnout přenést některé starší verze aplikace do Azure a pokračovat v ostatní uživatelé nemohli z jejich organizace. Všimněte si, že připojení sdílené složky je možné pouze pro aplikace spuštěné v Azure; místní aplikace mohou pouze přístup ke sdílené složce soubor prostřednictvím rozhraní REST API.

Distribuované aplikace můžete taky použít úložiště souborů k ukládání a sdílení dat užitečné aplikací a vývoj a nástrojů pro testování. Například aplikace může ukládat soubory konfigurace a diagnostické data například protokoly, metriky a dojde k chybě vypíše v souboru úložiště sdílet tak, aby byly k dispozici na více role nebo virtuálních počítačích. Vývojáři a správci můžou být uloženy nástroje, které budou muset vytvořit nebo spravovat aplikace ve sdílené složky úložiště, který je k dispozici pro všechny komponenty místo instalaci na každém virtuálního počítače nebo instanci rolí.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Přístup k objektů Blob, tabulky, fronty a prostředky souborů

Ve výchozím nastavení můžete jenom vlastník účtu úložiště přístup k prostředkům ve účtu úložiště. Zabezpečení dat musí být ověřeny každé žádost k prostředkům ve vašem účtu. Ověřování závisí na modelu sdílené klíče. Objekty BLOB je možné konfigurovat taky pro podporu anonymní přístup.

Váš účet úložiště přiřazena dvě soukromé přístupové klávesy při vytvoření slouží k ověření. S dva klíče zaručuje, že aplikace zůstávají k dispozici, když pravidelně obnovit klávesy z hlediska běžné zabezpečení správy klíčů.

Pokud přece jen budete potřebovat umožníte uživatelům řídit přístup k prostředkům úložiště a pak můžete vytvořit podpis sdílený přístup. Token, který může být přidán k adresu URL, která umožňuje delegované přístup k prostředku úložiště je sdílený přístup podpis (přidružení zabezpečení). Každý, kdo má tokenu přístup k prostředku, který odkazuje s oprávnění, která určuje, pro časové období, je platný. Začínající verze 2015 04 05 úložišti Azure podporuje dva typy podpisů sdílený přístup: služby přidružení zabezpečení a účet přidružení zabezpečení.

Služba přidružení zabezpečení delegátů přístup k prostředku v jenom jeden z úložiště služby: službu objektů Blob, fronty, tabulku nebo soubor.

Účet přidružení zabezpečení deleguje přístupu k prostředkům ve jednu nebo víc z úložiště služby. Přístup k úrovni služeb operacích, které nejsou k dispozici službou přidružení zabezpečení můžete delegáta. Můžete taky přístup pro čtení, psaní a odstraňování na kontejnery objektů blob, tabulek, dotazů a sdílené složky, které nejsou povoleny službou přidružení zabezpečení delegáta.

Nakonec můžete určit, že jsou k dispozici pro přístup k veřejným kontejneru a jeho objektů BLOB nebo konkrétní objektů blob. Pokud označíte, že je kontejner nebo objektů blob veřejné všichni moct číst anonymně; požaduje ověření.  Veřejné kontejnery a objekty BLOB jsou užitečné pro vystavení zdroje, jako jsou média a dokumenty, které jsou hostované na weby.  Snížit latence sítě pro globální cílovou skupinu, můžete mezipaměti objektů blob data použitá weby s Azure CDN.

Další informace o podpisech sdílený přístup naleznete v tématu [Používání sdílených přístup podpisy (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md) . Další informace o zabezpečený přístup ke svému účtu úložiště najdete v článku [Správa anonymní přístup pro čtení kontejnerů a objektů BLOB](storage-manage-access-to-resources.md) a [ověřování úložiště služby Azure](https://msdn.microsoft.com/library/azure/dd179428.aspx) .

## <a name="replication-for-durability-and-high-availability"></a>Replikace životnosti a dostupnost

Data ve vašem účtu Microsoft Azure úložiště je vždy replikovat ověřit životnosti a dostupnost. Replikace slouží ke kopírování dat, v rámci stejného datového centra nebo do druhého datovém centru, podle toho, kterou možnost replikace zvolíte. Replikace chrání data a zachová vaše aplikace dobu v případě selhání přechodná hardwaru. Pokud vaše data replikovat na druhý datovém centru, chrání, vaše data před katastrofické Chyba instalace v hlavním pracovišti.

Replikace zaručuje, že účtu úložiště splňuje [Podmínky smlouvy úrovni služeb (SLA) pro ukládání](https://azure.microsoft.com/support/legal/sla/storage/) i při k chybám. Najdete v tématu že SLA informace o úložišti Azure zaručuje životnosti a k dispozici. 

Při vytváření účtu úložiště, můžete vybrat jednu z následujících možností replikace:  

- **Místně nadbytečné úložiště (LRS).** Místně nadbytečné úložiště udržuje tři kopie vaše data. LRS je replikovat třikrát v rámci jedné datovém centru v jedné oblasti. LRS chrání data z normální hardwarové chyby, ale ne z neúspěšného jedno datové centrum.  

    LRS je k dispozici se slevou. Maximální životnosti doporučujeme použít geo nadbytečné úložiště píše níže.


- **Zóny nadbytečné úložiště (ZRS).** Zóny nadbytečné úložiště udržuje tři kopie vaše data. ZRS je replikovat třikrát přes dvě nebo tři zařízení, buď v rámci jedné oblasti nebo dvou oblastí, poskytnutí vyšší odolnost než LRS. ZRS zajišťuje, že vaše data trvalé v rámci jedné oblasti.  

    ZRS poskytuje vyšší úroveň životnosti než LRS; však maximální životnosti, doporučujeme použít geo nadbytečné úložiště píše níže.  

    > [AZURE.NOTE] ZRS není momentálně dostupné jenom pro objekty BLOB bloku a pouze je podporována pro verze 2014-02-14 nebo novějším.
    >
    > Po vytvoření účtu úložiště a vybrali ZRS převést nejde bude sloužit jako jiný typ replikace, nebo naopak.

- **Geo nadbytečné úložiště (GRS)**. GRS udržuje šest kopií vaše data. GRS datům replikovat třikrát v rámci oblasti primární a také replikovat třikrát v sekundární oblasti stovky mil od primární oblast poskytující nejvyšší úroveň životnost. V případě selhání v oblasti hlavní Azure úložiště bude přepnutí do oblasti sekundární. GRS zajistí, že vaše data trvalé ve dvou samostatných oblastech.

    Informace o primárních a sekundárních dvojic podle regionů najdete v tématu [Azure oblastí](https://azure.microsoft.com/regions/).

- **Čtení geo nadbytečné úložiště (Vzdálená pomoc GRS)**. Čtení geo nadbytečné úložiště zreplikuje dat na vedlejší zeměpisné polohy a také poskytuje přístup pro čtení k datům v sekundární umístění. Přístup pro čtení geo nadbytečné úložiště umožňuje přístup k datům z primární a sekundární umístění v případě, že nebude k dispozici jednoho místa. Čtení geo nadbytečné úložiště je výchozí možnost účtu úložiště ve výchozím nastavení po jeho vytvoření. 

    > [AZURE.IMPORTANT] Můžete změnit, jak dat je replikovat po vytvoření účtu úložiště, pokud jste zadali ZRS při vytvoření účtu. Však přenosem další jednorázové data nákladů Pokud přecházíte z LRS GRS nebo Vzdálená pomoc GRS mohou být účtovány.

Další informace o možnosti replikace úložiště najdete v článku [replikace Azure úložiště](storage-redundancy.md) .

Informace o účtu replikace úložiště cenách, najdete v článku [Ceny úložiště Azure](https://azure.microsoft.com/pricing/details/storage/). Další informace o tom, jaké služby dostupné v jednotlivých oblastech naleznete v tématu [Azure oblastí](https://azure.microsoft.com/regions/#services) .

Architektonické podrobnosti o životnosti s úložištěm Azure najdete v tématu [SOSP papír - úložišti Azure: A vysoce dostupné službě cloudového úložiště s silné konzistence](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## <a name="transferring-data-to-and-from-azure-storage"></a>Přenos dat z Azure úložiště a

Nástroj příkazového řádku AzCopy slouží ke kopírování objektů blob, soubor a tabulková data v rámci svého účtu úložiště nebo úložiště účty. Další informace najdete v tématu [přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md) .

AzCopy je této technologii postavené [Azure přesun dat](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), která je aktuálně k dispozici ve verzi preview.

Služba Azure Import nebo Export umožňuje vytvářet objektů blob data importovat nebo exportovat data objektů blob ze svého účtu úložiště prostřednictvím disk pevném disku odeslán Azure datacentrem. Další informace o službě Import nebo Export najdete v tématu [použití službu Microsoft Azure Import nebo Export pro přenos dat k úložišti objektů Blob](storage-import-export-service.md).

## <a name="pricing"></a>Ceny

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Rozhraní API úložiště, knihoven a nástroje

Azure prostředků úložiště přístupné libovolný jazyk, který může být žádosti o protokolu HTTP/HTTPS. Azure úložiště navíc nabízí programovací knihoven několik Oblíbené jazyků se zápisem. Tyto knihovny zjednodušit aspektech práce s úložištěm Azure zpracování podrobnosti, například synchronní a asynchronní vyvolání, dávky operace správě výjimek, automatické opakování, provozní chování a tak dále. Knihovny jsou momentálně dostupné pro následující jazyky a platformy, s ostatními v kanálu:

### <a name="azure-storage-data-services"></a>Datové služby Azure úložiště

- [Služba úložiště rozhraní REST API](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Knihovna úložiště klienta pro .NET, Windows Phone a Windows Runtime](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Úložiště klienta v knihovně C++](https://github.com/Azure/azure-storage-cpp)
- [Knihovna úložiště klienta pro Java nebo Android](/develop/java/)
- [Úložiště klienta v knihovně Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [Úložiště klienta v knihovně PHP](/develop/php/)
- [Úložiště klienta v knihovně skutečné](/develop/ruby/)
- [Úložiště klienta v knihovně Python](/develop/python/)
- [Rutiny pro správu úložiště pro PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Správa služby Azure úložiště

- [Úložiště zdroje poskytovatelem REST API odkaz](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [Knihovna klienta poskytovatelem úložiště zdrojů pro .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [Úložiště zdroje poskytovatele rutiny prostředí PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [Úložiště služby správy REST API (klasický)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Pohyb služby Azure úložiště dat

- [Import nebo Export úložiště služby rozhraní REST API](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [Úložiště pohyb klienta dat pro .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Nástroje a nástroje

- [Průzkumník Azure úložiště](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Azure úložiště klientských nástrojích](storage-explorers.md)
- [Nástroje a Azure SDK](https://azure.microsoft.com/tools/)
- [Emulátoru Azure úložiště](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure Powershellu](../powershell-install-configure.md)
- [Nástroj AzCopy příkazového řádku](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Další kroky

Další informace o úložišti Azure, prostudujte si tyto materiály:

### <a name="documentation"></a>Si přečtěte následující dokumentaci

- [Si přečtěte následující dokumentaci Azure úložiště](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>Pro správce

- [Azure Powershellu službou Azure úložiště](storage-powershell-guide-full.md)
- [Azure rozhraní příkazového řádku službou Azure úložiště](storage-azure-cli.md)

### <a name="for-net-developers"></a>Pro vývojáře .NET

- [Začínáme s úložiště objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md)
- [Začínáme s úložiště tabulek Azure pomocí .NET](storage-dotnet-how-to-use-tables.md)
- [Začínáme s úložištěm fronty Azure pomocí .NET](storage-dotnet-how-to-use-queues.md)
- [Začínáme s úložiště souborů Azure v systému Windows](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Pro vývojáře Java nebo Android

- [Použití úložiště objektů Blob z Java](storage-java-how-to-use-blob-storage.md)
- [Použití úložiště tabulek z Java](storage-java-how-to-use-table-storage.md)
- [Použití úložiště fronty z Java](storage-java-how-to-use-queue-storage.md)
- [Použití úložiště souborů z Java](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Pro vývojáře Node.js

- [Použití úložiště objektů Blob z Node.js](storage-nodejs-how-to-use-blob-storage.md)
- [Použití úložiště tabulek z Node.js](storage-nodejs-how-to-use-table-storage.md)
- [Použití úložiště fronty z Node.js](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Pro vývojáře PHP

- [Použití úložiště objektů Blob z PHP](storage-php-how-to-use-blobs.md)
- [Použití úložiště tabulek z PHP](storage-php-how-to-use-table-storage.md)
- [Použití úložiště fronty z PHP](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Pro vývojáře skutečné

- [Použití úložiště objektů Blob z skutečné](storage-ruby-how-to-use-blob-storage.md)
- [Použití úložiště tabulek z skutečné](storage-ruby-how-to-use-table-storage.md)
- [Použití úložiště fronty z skutečné](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Pro vývojáře Python

- [Použití úložiště objektů Blob z Python](storage-python-how-to-use-blob-storage.md)
- [Použití úložiště tabulek z Python](storage-python-how-to-use-table-storage.md)
- [Použití úložiště fronty z Python](storage-python-how-to-use-queue-storage.md)
- [Použití úložiště souborů z Python](storage-python-how-to-use-file-storage.md)
