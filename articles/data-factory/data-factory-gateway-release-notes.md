<properties 
    pageTitle="Poznámky k verzi pro Brána pro správu dat | Azure Data Factory" 
    description="Poznámky k verzi text Brána pro správu dat" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>Poznámky k verzi pro Brána pro správu dat
Jednou z výzvy pro integraci moderní dat je Bezproblémová přesunutí dat do a z místní do cloudu. Data Factory zajišťuje toto integrace bezproblémové s Brána pro správu dat, což je agent, že můžete nainstalovat místních povolit přesun dat hybridní.

Najdete podrobné informace o bráně pro správu dat a jak se používá v následujících článcích: 

- [Brána pro správu dat](data-factory-data-management-gateway.md)
- [Přesouvání dat mezi místních i cloudových pomocí Azure Data Factory](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>Aktuální verze (2.2.6072.1)

- Podporuje nastavení brány pomocí Správce konfigurace brány pro proxy pro HTTP. Pokud nakonfigurovány, objektů Blob Azure, tabulky Azure, jezera dat Azure a dokumentu DB přistupovat prostřednictvím protokolu HTTP proxy.
- Podporuje záhlaví zpracování pro formát textu při kopírování dat z/objektů Blob Azure, jezera Azure úložišti místního systému souborů a místním HDFS.
- Kopírování dat z objektů Blob připojení a stránky Blob spolu s už podporované objektů Blob blok podporuje.
- Uvádí nové stav brány **Online (omezené)**, což znamená, že hlavní funkce brány označené jako pracuje s výjimkou interaktivní operace podpora Průvodce kopírováním.
- Zlepšuje odolnosti registrace brány pomocí registračního klíče.

## <a name="earlier-versions"></a>Starší verze

## <a name="2160401"></a>2.1.6040.1

- Ovladač DB2 je součástí instalační balíček brány nyní. Není potřeba nainstalovat samostatně. 
- Ovladač DB2 teď podporuje z/OS DB2 pro můžu (jako / 400) spolu s už podporované platformy (Linux Unix a Windows). 
- Podporuje použití DocumentDB jako zdrojový nebo cílový pro místní úložiště dat
- Úložiště spolu s účtem už podporované univerzální úložiště objektů blob podporuje kopírování dat z/chladírenského/aktivní. 
- Umožňuje připojit k místní SQL Server prostřednictvím brány s oprávněními vzdálené přihlášení.  

## <a name="2060131"></a>2.0.6013.1

- Můžete vybrat jazyk/jazykové verze brány používaný během ruční instalace.
- Pokud brána nefunguje očekávaným způsobem, můžete odeslat protokoly brány posledních sedmi dnů společnosti Microsoft usnadnit řešení problémů s problém. Pokud brána není připojený ke cloudové služby, můžete uložit a archivace protokoly brány.  
- Vylepšení uživatelského rozhraní pro správce konfigurace brány pro:
    - Zviditelníte stav brány na pásu karet aplikace Excel.
    - Ovládací prvky přeorganizované a zjednodušenou.
- Zkopírujte data z úložiště pomocí [kódu bezplatné nástroje Kopírovat náhled](data-factory-copy-data-wizard-tutorial.md). Přečtěte si článek [Připravené kopírování](data-factory-copy-activity-performance.md#staged-copy) podrobné informace o této funkci obecně. 
- Pomocí Brána pro správu dat k datům průniku přímo z databáze SQL serveru místní do výukové Azure počítače.
- Zvýšení výkonu
    - Zlepšení výkonu o prohlížení schématu a náhled před SQL serveru pomocí kopírování kódu bez náhledu nástroje.



## <a name="11259531"></a>1.12.5953.1
- Opravy chyb

## <a name="11159181"></a>1.11.5918.1

- Maximální velikosti doručovaných v protokolu událostí brány se zvýší z 1 MB 40 MB.
- V případě, že během automaticky aktualizován brány není potřeba restartovat počítač, zobrazí se dialog s upozorněním. Můžete přímo potom nebo novější. 
- V případě, že automatické aktualizace nepovede, opakuje brány instalační automatické aktualizace třikrát na maximum.
- Zvýšení výkonu
    - Zvýšení výkonu při načítání rozsáhlých tabulek z místního serveru v případě kopírování bez kódu.
- Opravy chyb

## <a name="11058921"></a>1.10.5892.1

- Zvýšení výkonu
- Opravy chyb

## <a name="1958652"></a>1.9.5865.2

- Nula funkcí Automatické aktualizace dotykového ovládání
- Ikona nové panelu s ukazateli stavu brány
- Možnost "Teď aktualizaci" z klienta
- Možnost nastavit čas plán aktualizace
- Skript Powershellu pro přepínání automaticky aktualizován zapnuto nebo vypnuto
- Podpora formátu JSON  
- Zvýšení výkonu
- Opravy chyb

## <a name="1858221"></a>1.8.5822.1

- Zlepšete možnosti práce při řešení potíží
- Zvýšení výkonu
- Opravy chyb

### <a name="1757951"></a>1.7.5795.1

- Zvýšení výkonu
- Opravy chyb

### <a name="1757641"></a>1.7.5764.1

- Zvýšení výkonu
- Opravy chyb

### <a name="1657351"></a>1.6.5735.1

- Podpora místního HDFS zdroj/jímky
- Zvýšení výkonu
- Opravy chyb

### <a name="1656961"></a>1.6.5696.1

- Zvýšení výkonu
- Opravy chyb

### <a name="1656761"></a>1.6.5676.1

- Podpora diagnostické nástroje na správce konfigurace
- Podpora sloupců v tabulce u tabulkových zdrojů dat Azure Data Factory
- Podpora SQL DW Azure Data Factory
- Podpora Reclusive BlobSource a FileSource Azure Data Factory
- Podpora CopyBehavior – MergeFiles PreserveHierarchy a FlattenHierarchy BlobSink a FileSink binární kopií Azure Data Factory
- Podpora kopírovat aktivitu vykazování průběhu pro Azure Data Factory
- Ověření připojení zdroje dat podpory pro Azure Data Factory
- Opravy chyb


### <a name="1656721"></a>1.6.5672.1

- Název tabulky pro zdroj dat ODBC podpory Azure Data Factory
- Zvýšení výkonu
- Opravy chyb

### <a name="1656581"></a>1.6.5658.1

- Podpora soubor dřez pro Azure Data Factory
- Podpora zachování hierarchie v binární kopírovat Azure Data Factory
- Podpora kopírovat aktivity idempotenci Azure Data Factory
- Opravy chyb

### <a name="1656401"></a>1.6.5640.1

- Podpora 3 více zdrojům dat Azure Data Factory (ODBC, OData, HDFS)
- Podpora znak uvozovky v csv analyzátoru Azure Data Factory
- Podpora komprese (BZip2)
- Opravy chyb

### <a name="1556121"></a>1.5.5612.1

- Podpora pět relační databáze Azure Data Factory (MySQL PostgreSQL, DB2, Teradata a Sybase)
- Podpora komprese (Gzip a Deflate)
- Zvýšení výkonu
- Opravy chyb


### <a name="1455491"></a>1.4.5549.1

- Přidání podpora zdroje dat Oracle pro Azure Data Factory
- Zvýšení výkonu
- Opravy chyb

### <a name="1454921"></a>1.4.5492.1

- Jednotné binární soubor, který podporuje služby Microsoft Azure Data Factory a Office 365 Power BI
- Upřesnění procesu konfigurace uživatelského rozhraní a registrace
- Azure Data Factory – podpora pro zdroje dat systému SQL Server Azure průniku a výstupním

### <a name="1253031"></a>1.2.5303.1

-   Řešení problému vypršení časového limitu pro podporu více připojení ke zdrojům dat časově náročnější. 
    
### <a name="1155268"></a>1.1.5526.8

- Během instalace vyžaduje .NET Framework 4.5.1 jako předpoklad.

### <a name="1051442"></a>1.0.5144.2

- Žádné změny, které ovlivňují Azure Data Factory scénáře. 
