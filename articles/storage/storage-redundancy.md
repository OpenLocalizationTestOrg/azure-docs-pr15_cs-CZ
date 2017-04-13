<properties
    pageTitle="Azure replikace úložiště | Microsoft Azure"
    description="Životnost a dostupnost replikace dat ve vašem účtu Microsoft Azure úložiště. Možnosti replikace zahrnout místně nadbytečné úložiště (LRS), zóny nadbytečné úložiště (ZRS), geo nadbytečné úložiště (GRS) a geo nadbytečné úložiště přístup pro čtení (Vzdálená pomoc GRS)."
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
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure replikace úložiště

Data ve vašem účtu Microsoft Azure úložiště je vždy replikovat ověřit životnosti a dostupnost. Replikace slouží ke kopírování dat, v rámci stejného datového centra nebo do druhého datovém centru, podle toho, kterou možnost replikace zvolíte. Replikace chrání data a zachová vaše aplikace dobu v případě selhání přechodná hardwaru. Pokud vaše data replikovat na druhý datovém centru, chrání, vaše data před katastrofické Chyba instalace v hlavním pracovišti.

Replikace zaručuje, že účtu úložiště splňuje [Podmínky smlouvy úrovni služeb (SLA) pro ukládání](https://azure.microsoft.com/support/legal/sla/storage/) i při k chybám. Najdete v tématu že SLA informace o úložišti Azure zaručuje životnosti a k dispozici. 

Při vytváření účtu úložiště, můžete vybrat jednu z následujících možností replikace:  

- [Místně nadbytečné úložiště (LRS)](#locally-redundant-storage)
- [Zóny nadbytečné úložiště (ZRS)](#zone-redundant-storage)
- [GEO nadbytečné úložiště (GRS)](#geo-redundant-storage)
- [Čtení geo nadbytečné úložiště (Vzdálená pomoc GRS)](#read-access-geo-redundant-storage)

Čtení geo nadbytečné úložiště (Vzdálená pomoc GRS) je výchozí možnost při vytváření nového účtu úložiště.

Následující tabulka obsahuje rychlý přehled o rozdílech mezi LRS, ZRS, GRS a Vzdálená pomoc-GRS a následujících oddílů adresu každého typu replikační podrobněji.

| Strategie replikace                                                               | LRS | ZRS | GRS | VZDÁLENÁ POMOC GRS |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Replikace dat napříč několika datacentrech.                                     | Ne  | Ano | Ano | Ano    |
| Data můžete číst z sekundární umístění a od primární umístění. | Ne  | Ne  | Ne  | Ano    |
| Počet kopií dat na samostatných uzlů.                             | 3   | 3   | 6   | 6      |

Naleznete v tématu [Azure úložiště ceny](https://azure.microsoft.com/pricing/details/storage/) informace o různých redundance možnosti cenách.

>[AZURE.NOTE] Úložiště Premium podporuje pouze místně nadbytečné úložiště (LRS). Informace o ukládání Premium najdete v tématu [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Místně nadbytečné úložiště

Místně nadbytečné úložiště (LRS) zreplikuje datům třikrát v rámci měřítka jednotka úložiště, který je hostovaný ve datacentru v oblasti, ve které jste vytvořili účtu úložiště. Žádost o zápisu vrátí úspěšně pouze po jeho napsání do tří ostatními. Tyto tři každý replikami v samostatném poruch domén a upgrade domén v rámci jedné úložiště měřítko jednotky. 

Jednotka měřítko úložiště je sada stojany uzlů úložiště. Doména poruch (FD) je skupina uzlů, které představují fyzická jednotka selhání a mohou být považovány uzly patřící do stejné pole fyzicky regálů. Upgrade domény (UD) je skupina uzlů upgradované společně během upgradu služeb (zavedení). Tři repliky jsou rozšířit mezi UDs a FDs v rámci jednu jednotku úložiště měřítko zajistěte, aby byl data k dispozici i v případě chyby hardwaru ovlivňuje jednoho regálů nebo při upgradu uzly během zavedení. 

LRS je nejnižší možnost náklady a nabízí nejnižších životnosti ve srovnání s další možnosti. V případě selhání úrovně datacentra (fire, zaplavení atd.) může být ostatními tři ztracené nebo obnovit. Chcete-li pomoci tato rizika zmírnit je vhodné Geo nadbytečné úložiště (GRS) u většiny aplikací.

Místně nadbytečné úložiště může být pořád žádoucí v některých scénářích: 

- Poskytuje nejvyšší maximální šířky pásma možnosti replikace v úložišti Azure.

- Pokud aplikace ukládá data, která můžete snadno nepřijímal, může se rozhodnout pro LRS.

- Některé aplikace jsou omezeny replikace dat pouze v rámci země kvůli požadavky zásady správného řízení data. Párových oblast může být v jiné zemi; Další informace o oblasti dvojici najdete v článku [Azure oblastí](https://azure.microsoft.com/regions/) .

## <a name="zone-redundant-storage"></a>Zóny nadbytečné úložiště

Zóny nadbytečné úložiště (ZRS) zreplikuje datům asynchronní přes datacentrech v rámci jedné nebo dvou oblastí kromě ukládání tři repliky podobný LRS, tedy poskytující vyšší odolnost než LRS. Data uložená v ZRS se trvalé, i když primární datacentra není k dispozici nebo obnovit.
Zákazníci, kteří plánování používání ZRS měli vědět, který: 

- ZRS je dostupný jenom u objektů BLOB blok v úložišti účty univerzální a je podporována pouze v úložišti služby verzích 2014-02-14 nebo novějším. 

- Protože asynchronní replikace zahrnuje zpoždění, místní havárie bylo možné v případě, že změny, které nebyly dosud replikovat na sekundární se ztratí z primárního možné obnovit data.

- Otevřené nemusí být k dispozici, dokud Microsoft zahájí převezme sekundární.

- Účty ZRS nelze převést na LRS nebo GRS později. Podobně existujícího účtu LRS nebo GRS nelze převést na účet ZRS.

- Účty ZRS nemají metriky nebo funkce protokolování. 

## <a name="geo-redundant-storage"></a>GEO nadbytečné úložiště

GEO nadbytečné úložiště (GRS) zreplikuje dat na vedlejší oblast, která je stovky mil od primární oblast. Pokud váš účet úložiště GRS povoleno, je vaše data trvalá i v případě výpadku dokončení místní nebo selhání, ve kterém není obnovitelné oblasti primární.

Úložiště účtu s GRS povoleno aktualizace nejdřív zaručuje primární oblasti, kde je replikovat třikrát. Potom aktualizace replikovat asynchronní na vedlejší oblasti, kde také replikace je třikrát. 

Pomocí GRS primárních a sekundárních oblasti spravovat repliky v samostatném poruch doménách a upgrade domén v rámci měřítka jednotky úložiště popsané s LRS.

Důležité informace:

- Protože asynchronní replikace zahrnuje zpoždění, místní havárie bylo možné v případě, že změny, které nebyly dosud replikovat na vedlejší oblasti se ztratí možné obnovit data z oblasti primární.

- Otevřené není k dispozici, pokud Microsoft nezahájí přepnutí do oblasti sekundární.

- Pokud aplikace chce číst z oblasti sekundární měli uživatele povolit GRS Vzdálená pomoc. 

Při vytváření účtu úložiště vyberte primární oblast pro účet. Sekundární oblast závisí na primární oblast a nelze změnit. Následující tabulka zobrazuje párování primárních a sekundárních oblast.

| Primární             | Sekundární           |
|---------------------|---------------------|
| Severní centrální USA    | Jižní centrální USA    |
| Jižní centrální USA    | Severní centrální USA    |
| Východní USA             | Západ USA             |
| Západ USA             | Východní USA             |
| Východního USA 2           | Centrální USA          |
| Centrální USA          | Východního USA 2           |
| Severní Evropě        | Západní Evropě         |
| Západní Evropě         | Severní Evropě        |
| Jihovýchodní Asie     | Východní Asie           |
| Východní Asie           | Jihovýchodní Asie     |
| Východní Číně          | Severní Číně         |
| Severní Číně         | Východní Číně          |
| Japonsko východ          | Japonsko západní          |
| Japonsko západní          | Japonsko východ          |
| Brazílie jih        | Jižní centrální USA    |
| Austrálie východ      | Austrálie jihovýchodní |
| Austrálie jihovýchodní | Austrálie východ      |
| Indie jih         | Indie centrální       |
| Indie centrální       | Indie jih         |
| Iowa Gov USA         | Virginie Gov USA     |
| Virginie Gov USA     | Iowa Gov USA         |
| Centrální Kanada      | Kanada východ          |
| Kanada východ         | Centrální Kanada      |
| UK západní             | Jih Spojené království            |
| Jih Spojené království            | Kanada západní             |
| Centrální Německo     | V severovýchodní Německo   |
| V severovýchodní Německo   | Centrální Německo     |
| Západ USA 2           | Centrální západ USA     |
| Centrální západ USA     | Západ USA 2           |

Aktuální informace o nepodporuje Azure oblastí najdete v článku [Azure oblastí](https://azure.microsoft.com/regions/).
 
## <a name="read-access-geo-redundant-storage"></a>Geo nadbytečné úložiště přístup pro čtení

Čtení geo nadbytečné úložiště (Vzdálená pomoc GRS) maximalizuje dostupnost pro váš účet úložiště zadáním jen pro čtení přístup k datům v sekundární umístění kromě replikace přes dvou oblastí poskytovanou GRS. 

Po povolení jen pro čtení přístup k datům v oblasti sekundárním dat je dostupná na vedlejší koncový bod, kromě primární koncový bod pro váš účet úložiště. Sekundární koncový bod se podobá primární koncový bod, ale přidá příponu `–secondary` na název účtu. Pokud se vaše primární koncový bod pro službu objektů Blob například `myaccount.blob.core.windows.net`, bude sekundárním koncový bod `myaccount-secondary.blob.core.windows.net`. Kombinace kláves pro váš účet úložiště jsou stejné pro oba primárních a sekundárních koncové body.

Důležité informace:

- Ke správě který koncový bod se interakce se při použití Vzdálená pomoc GRS má aplikace. 

- Vzdálená pomoc GRS je určená pro účely vysoké dostupnosti. Pokyny pro škálovatelnost Zrevidujte [kontrolního seznamu výkonu](storage-performance-checklist.md).

## <a name="next-steps"></a>Další kroky

- [Ceny Azure úložiště](https://azure.microsoft.com/pricing/details/storage/)
- [Informace o účtech Azure úložiště](storage-create-storage-account.md)
- [Škálovatelnost Azure úložiště a cílů výkonu](storage-scalability-targets.md)
- [Možnosti ukládání redundance Microsoft Azure a přístup pro čtení Geo nadbytečné úložiště](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [SOSP papír - Azure úložiště: Vysoce dostupné službě cloudového úložiště s silných konzistence](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  
