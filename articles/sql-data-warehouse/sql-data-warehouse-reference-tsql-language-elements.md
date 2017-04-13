<properties
   pageTitle="Prvky jazyka SQL datový sklad Transact-SQL | Microsoft Azure"
   description="Seznam odkazů na referenční obsah prvků jazyce Transact-SQL pro SQL datový sklad."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/08/2016"
   ms.author="barbkess;sonyama;kevin"/>

# <a name="language-elements"></a>Prvky jazyka

## <a name="core-elements"></a>Základní prvky

- [konvence syntaxe](https://msdn.microsoft.com/library/ms177563.aspx)
- [pravidla pro pojmenování objektů](https://msdn.microsoft.com/library/ms175874.aspx)
- [vyhrazená klíčová slova](https://msdn.microsoft.com/library/ms189822.aspx)
- [řazení](https://msdn.microsoft.com/library/ff848763.aspx)
- [komentáře](https://msdn.microsoft.com/library/ms181627.aspx)
- [konstanty](https://msdn.microsoft.com/library/ms179899.aspx)
- [datové typy](https://msdn.microsoft.com/library/ms187752.aspx)
- [SPUŠTĚNÍ](https://msdn.microsoft.com/library/ms188332.aspx)
- [výrazy](https://msdn.microsoft.com/library/ms190286.aspx)
- [UKONČENÍ](https://msdn.microsoft.com/library/ms173730.aspx)
- [Řešení: vlastnost IDENTITY](https://msdn.microsoft.com/library/ms186775.aspx)
- [TISK](https://msdn.microsoft.com/library/ms176047.aspx)
- [POUŽITÍ](https://msdn.microsoft.com/library/ms188366.aspx)

## <a name="batches-control-of-flow-and-variables"></a>Listy, řízení toku a proměnných

- [ZAČNĚTE... UKONČENÍ](https://msdn.microsoft.com/library/ms190487.aspx)
- [KONEC](https://msdn.microsoft.com/library/ms181271.aspx)
- [DEKLARACE@local_variable](https://msdn.microsoft.com/library/ms188927.aspx)
- [POKUD... DALŠÍHO](https://msdn.microsoft.com/library/ms182717.aspx)
- [RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx)
- [SET@local_variable](https://msdn.microsoft.com/library/ms189484.aspx)
- [HODIT](https://msdn.microsoft.com/library/ee677615.aspx)
- [ZKUSTE... ZACHYCENÍ](https://msdn.microsoft.com/library/ms175976.aspx)
- [PŘI](https://msdn.microsoft.com/library/ms178642.aspx)

## <a name="operators"></a>Operátory
- [+ (Přidat)](https://msdn.microsoft.com/library/ms178565.aspx)
- [+ (Zřetězení)](https://msdn.microsoft.com/library/ms177561.aspx)
- [-(Zápor)](https://msdn.microsoft.com/library/ms189480.aspx)
- [-(Odečtení)](https://msdn.microsoft.com/library/ms189518.aspx)
- [* (Násobení)](https://msdn.microsoft.com/library/ms176019.aspx)
- [/ (Dělení)](https://msdn.microsoft.com/library/ms175009.aspx)
- [Modulo](https://msdn.microsoft.com/library/ms190279.aspx)

## <a name="wildcard-characters-to-match"></a>Zástupné znaky podle
- [= (Rovná se)](https://msdn.microsoft.com/library/ms175118.aspx)
- [> (Větší než)](https://msdn.microsoft.com/library/ms178590.aspx)
- [< (Menší než)](https://msdn.microsoft.com/library/ms179873.aspx)
- [> = (velké nebo rovno)](https://msdn.microsoft.com/library/ms181567.aspx)
- [< = (menší než nebo rovno)](https://msdn.microsoft.com/library/ms174978.aspx)
- [<> (Není rovno)](https://msdn.microsoft.com/library/ms176020.aspx)
- [! = (Není rovno)](https://msdn.microsoft.com/library/ms190296.aspx)
- [A](https://msdn.microsoft.com/library/ms188372.aspx)
- [MEZI](https://msdn.microsoft.com/library/ms187922.aspx)
- [EXISTUJE](https://msdn.microsoft.com/library/ms188336.aspx)
- [V](https://msdn.microsoft.com/library/ms177682.aspx)
- [NENÍ [] NULL](https://msdn.microsoft.com/library/ms188795.aspx)
- [JAKO](https://msdn.microsoft.com/library/ms179859.aspx)
- [NOT](https://msdn.microsoft.com/library/ms189455.aspx)
- [NEBO](https://msdn.microsoft.com/library/ms188361.aspx)

### <a name="bitwise-operators"></a>Bitové operátory

- [& (Bitové operace AND)](https://msdn.microsoft.com/library/ms174965.aspx)
- [| (Bitové operace OR)](https://msdn.microsoft.com/library/ms186714.aspx)
- [^ (Bitové operace exkluzivní OR)](https://msdn.microsoft.com/library/ms190277.aspx)
- [~ (Bitové ne)](https://msdn.microsoft.com/library/ms173468.aspx)
- [^ = (Bitové operace exkluzivní nebo je ROVNO)](https://msdn.microsoft.com/library/cc627413.aspx)
- [| = (Rovná se bitové operace OR)](https://msdn.microsoft.com/library/cc627409.aspx)
- [& = (rovná se bitové operace AND)](https://msdn.microsoft.com/library/cc627427.aspx)

## <a name="functions"></a>Funkce

- [@@DATEFIRST](https://msdn.microsoft.com/library/ms187766.aspx)
- [@@ERROR](https://msdn.microsoft.com/library/ms188790.aspx)
- [@@LANGUAGE](https://msdn.microsoft.com/library/ms177557.aspx)
- [@@SPID](https://msdn.microsoft.com/library/ms189535.aspx)
- [@@TRANCOUNT](https://msdn.microsoft.com/library/ms187967.aspx)
- [@@VERSION](https://msdn.microsoft.com/library/ms177512.aspx)
- [ABS](https://msdn.microsoft.com/library/ms189800.aspx)
- [ARCCOS](https://msdn.microsoft.com/library/ms178627.aspx)
- [ASCII](https://msdn.microsoft.com/library/ms177545.aspx)
- [ARCSIN](https://msdn.microsoft.com/library/ms181581.aspx)
- [ARCTG](https://msdn.microsoft.com/library/ms181746.aspx)
- [ATN2](https://msdn.microsoft.com/library/ms173854.aspx)
- [BINARY_CHECKSUM](https://msdn.microsoft.com/library/ms173784.aspx)
- [PŘÍPADU](https://msdn.microsoft.com/library/ms181765.aspx)
- [CAST a převést](https://msdn.microsoft.com/library/ms187928.aspx)
- [ZAOKR.NAHORU](https://msdn.microsoft.com/library/ms189818.aspx)
- [ZNAK](https://msdn.microsoft.com/library/ms187323.aspx)
- [CHARINDEX](https://msdn.microsoft.com/library/ms186323.aspx)
- [KONTROLNÍ SOUČET](https://msdn.microsoft.com/library/ms189788.aspx)
- [COALESCE](https://msdn.microsoft.com/library/ms190349.aspx)
- [COL_NAME](https://msdn.microsoft.com/library/ms174974.aspx)
- [COLLATIONPROPERTY](https://msdn.microsoft.com/library/ms190305.aspx)
- [PROPOJIT](https://msdn.microsoft.com/library/hh231515.aspx)
- [COS](https://msdn.microsoft.com/library/ms188919.aspx)
- [COT](https://msdn.microsoft.com/library/ms188921.aspx)
- [POČET](https://msdn.microsoft.com/library/ms175997.aspx)
- [COUNT_BIG](https://msdn.microsoft.com/library/ms190317.aspx)
- [CUME_DIST](https://msdn.microsoft.com/library/hh231078.aspx)
- [CURRENT_TIMESTAMP](https://msdn.microsoft.com/library/ms188751.aspx)
- [CURRENT_USER](https://msdn.microsoft.com/library/ms176050.aspx)
- [DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx)
- [DÉLKA_DAT](https://msdn.microsoft.com/library/ms173486.aspx)
- [FUNKCE DATEADD](https://msdn.microsoft.com/library/ms186819.aspx)
- [FUNKCE DATEDIFF](https://msdn.microsoft.com/library/ms189794.aspx)
- [DATEFROMPARTS](https://msdn.microsoft.com/library/hh213228.aspx)
- [DATENAME](https://msdn.microsoft.com/library/ms174395.aspx)
- [FUNKCE DATEPART](https://msdn.microsoft.com/library/ms174420.aspx)
- [DATETIME2FROMPARTS](https://msdn.microsoft.com/library/hh213312.aspx)
- [DATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213233.aspx)
- [DATETIMEOFFSETFROMPARTS](https://msdn.microsoft.com/library/hh231077.aspx)
- [DEN](https://msdn.microsoft.com/library/ms176052.aspx)
- [DB_ID](https://msdn.microsoft.com/library/ms186274.aspx)
- [DB_NAME](https://msdn.microsoft.com/library/ms189753.aspx)
- [DEGREES](https://msdn.microsoft.com/library/ms178566.aspx)
- [DENSE_RANK](https://msdn.microsoft.com/library/ms173825.aspx)
- [ROZDÍL](https://msdn.microsoft.com/library/ms188753.aspx)
- [EOMONTH](https://msdn.microsoft.com/library/hh213020.aspx)
- [ERROR_MESSAGE](https://msdn.microsoft.com/library/ms190358.aspx)
- [ERROR_NUMBER](https://msdn.microsoft.com/library/ms175069.aspx)
- [ERROR_PROCEDURE](https://msdn.microsoft.com/library/ms188398.aspx)
- [ERROR_SEVERITY](https://msdn.microsoft.com/library/ms178567.aspx)
- [ERROR_STATE](https://msdn.microsoft.com/library/ms180031.aspx)
- [EXP](https://msdn.microsoft.com/library/ms179857.aspx)
- [FIRST_VALUE](https://msdn.microsoft.com/library/hh213018.aspx)
- [ZAOKR.DOLŮ](https://msdn.microsoft.com/library/ms178531.aspx)
- [GETDATE](https://msdn.microsoft.com/library/ms188383.aspx)
- [GETUTCDATE](https://msdn.microsoft.com/library/ms178635.aspx)
- [HAS_DBACCESS](https://msdn.microsoft.com/library/ms187718.aspx)
- [HASHBYTES](https://msdn.microsoft.com/library/ms174415.aspx)
- [INDEXPROPERTY](https://msdn.microsoft.com/library/ms187729.aspx)
- [FUNKCE ISDATE](https://msdn.microsoft.com/library/ms187347.aspx)
- [FUNKCE ISNULL](https://msdn.microsoft.com/library/ms184325.aspx)
- [FUNKCE ISNUMERIC](https://msdn.microsoft.com/library/ms186272.aspx)
- [PRODLEVA](https://msdn.microsoft.com/library/hh231256.aspx)
- [LAST_VALUE](https://msdn.microsoft.com/library/hh231517.aspx)
- [POTENCIÁLNÍ ZÁKAZNÍK](https://msdn.microsoft.com/library/hh213125.aspx)
- [VLEVO](https://msdn.microsoft.com/library/ms177601.aspx)
- [FUNKCE LEN](https://msdn.microsoft.com/library/ms190329.aspx)
- [LOG](https://msdn.microsoft.com/library/ms190319.aspx)
- [LOG10](https://msdn.microsoft.com/library/ms175121.aspx)
- [MALÁ PÍSMENA](https://msdn.microsoft.com/library/ms174400.aspx)
- [FUNKCE LTRIM](https://msdn.microsoft.com/library/ms177827.aspx)
- [MAX](https://msdn.microsoft.com/library/ms187751.aspx)
- [MIN](https://msdn.microsoft.com/library/ms179916.aspx)
- [MĚSÍC](https://msdn.microsoft.com/library/ms187813.aspx)
- [NCHAR](https://msdn.microsoft.com/library/ms182673.aspx)
- [NTILE](https://msdn.microsoft.com/library/ms175126.aspx)
- [HODNOTU NULLIF](https://msdn.microsoft.com/library/ms177562.aspx)
- [OBJECT_ID](https://msdn.microsoft.com/library/ms190328.aspx)
- [NÁZEV_OBJEKTU –](https://msdn.microsoft.com/library/ms186301.aspx)
- [OBJECTPROPERTY](https://msdn.microsoft.com/library/ms176105.aspx)
- [OIBJECTPROPERTYEX](https://msdn.microsoft.com/library/ms188390.aspx)
- [Skalární funkce ODBCS](https://msdn.microsoft.com/library/bb630290.aspx)
- [PŘES klauzule](https://msdn.microsoft.com/library/ms189461.aspx)
- [PARSENAME](https://msdn.microsoft.com/library/ms188006.aspx)
- [PATINDEX](https://msdn.microsoft.com/library/ms188395.aspx)
- [PERCENTILE_CONT](https://msdn.microsoft.com/library/hh231473.aspx)
- [PERCENTILE_DISC](https://msdn.microsoft.com/library/hh231327.aspx)
- [PERCENT_RANK](https://msdn.microsoft.com/library/hh213573.aspx)
- [PI](https://msdn.microsoft.com/library/ms189512.aspx)
- [POWER](https://msdn.microsoft.com/library/ms174276.aspx)
- [QUOTENAME](https://msdn.microsoft.com/library/ms176114.aspx)
- [RADIANS](https://msdn.microsoft.com/library/ms189742.aspx)
- [NÁHČÍSLO](https://msdn.microsoft.com/library/ms177610.aspx)
- [POŘADÍ](https://msdn.microsoft.com/library/ms176102.aspx)
- [NAHRAZENÍ](https://msdn.microsoft.com/library/ms186862.aspx)
- [REPLIKOVAT](https://msdn.microsoft.com/library/ms174383.aspx)
- [OBRÁCENÉ POŘADÍ](https://msdn.microsoft.com/library/ms180040.aspx)
- [VPRAVO](https://msdn.microsoft.com/library/ms177532.aspx)
- [ZAOKROUHLIT](https://msdn.microsoft.com/library/ms175003.aspx)
- [TEXTOVÝ](https://msdn.microsoft.com/library/ms186734.aspx)
- [FUNKCE RTRIM](https://msdn.microsoft.com/library/ms178660.aspx)
- [SCHEMA_ID](https://msdn.microsoft.com/library/ms188797.aspx)
- [SCHEMA_NAME](https://msdn.microsoft.com/library/ms175068.aspx)
- [SERVERPROPERTY](https://msdn.microsoft.com/library/ms174396.aspx)
- [SESSION_USER](https://msdn.microsoft.com/library/ms177587.aspx)
- [SIGN](https://msdn.microsoft.com/library/ms188420.aspx)
- [FUNKCE SIN](https://msdn.microsoft.com/library/ms188377.aspx)
- [SMALLDATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213396.aspx)
- [SOUNDEX](https://msdn.microsoft.com/library/ms187384.aspx)
- [MÍSTO](https://msdn.microsoft.com/library/ms187950.aspx)
- [SQL_VARIANT_PROPERTY](https://msdn.microsoft.com/library/ms178550.aspx)
- [ODMOCNINA](https://msdn.microsoft.com/library/ms176108.aspx)
- [ČTVEREC](https://msdn.microsoft.com/library/ms173569.aspx)
- [STATS_DATE](https://msdn.microsoft.com/library/ms190330.aspx)
- [SMODCH.VÝBĚR](https://msdn.microsoft.com/library/ms190474.aspx)
- [SMODCH](https://msdn.microsoft.com/library/ms176080.aspx)
- [FUNKCE STR](https://msdn.microsoft.com/library/ms189527.aspx)
- [OBSAHU](https://msdn.microsoft.com/library/ms188043.aspx)
- [PODŘETĚZEC](https://msdn.microsoft.com/library/ms187748.aspx)
- [SUMA](https://msdn.microsoft.com/library/ms187810.aspx)
- [SUSER_SNAME](https://msdn.microsoft.com/library/ms174427.aspx)
- [SWITCHOFFSET](https://msdn.microsoft.com/library/bb677244.aspx)
- [SYSDATETIME](https://msdn.microsoft.com/library/bb630353.aspx)
- [SYSDATETIMEOFFSET](https://msdn.microsoft.com/library/bb677334.aspx)
- [SYSTEM_USER](https://msdn.microsoft.com/library/ms179930.aspx)
- [SYSUTCDATETIME](https://msdn.microsoft.com/library/bb630387.aspx)
- [FUNKCE TAN](https://msdn.microsoft.com/library/ms190338.aspx)
- [TERTIARY_WEIGHTS](https://msdn.microsoft.com/library/ms186881.aspx)
- [TIMEFROMPARTS](https://msdn.microsoft.com/library/hh213398.aspx)
- [TODATETIMEOFFSET](https://msdn.microsoft.com/library/bb630335.aspx)
- [TYPE_ID](https://msdn.microsoft.com/library/ms181628.aspx)
- [TYPE_NAME](https://msdn.microsoft.com/library/ms189750.aspx)
- [TYPEPROPERTY](https://msdn.microsoft.com/library/ms188419.aspx)
- [UNICODE](https://msdn.microsoft.com/library/ms180059.aspx)
- [VELKÁ](https://msdn.microsoft.com/library/ms180055.aspx)
- [UŽIVATEL](https://msdn.microsoft.com/library/ms186738.aspx)
- [UŽIVATELSKÉ_JMÉNO](https://msdn.microsoft.com/library/ms188014.aspx)
- [VAR](https://msdn.microsoft.com/library/ms186290.aspx)
- [VAR](https://msdn.microsoft.com/library/ms188735.aspx)
- [ROK](https://msdn.microsoft.com/library/ms186313.aspx)
- [XACT_STATE](https://msdn.microsoft.com/library/ms189797.aspx)

## <a name="transactions"></a>Transakce

- [transakce](https://msdn.microsoft.com/library/mt204031.aspx)

## <a name="diagnostic-sessions"></a>Diagnostické relací

- [VYTVOŘENÍ RELACE DIAGNOSTICKÝCH NÁSTROJŮ](https://msdn.microsoft.com/library/mt204029.aspx)

## <a name="procedures"></a>Postupy

- [sp_addrolemember](https://msdn.microsoft.com/library/ms187750.aspx)
- [sp_columns](https://msdn.microsoft.com/library/ms176077.aspx)
- [sp_configure](https://msdn.microsoft.com/library/ms188787.aspx)
- [sp_datatype_info_90](https://msdn.microsoft.com/library/mt204014.aspx)
- [sp_droprolemember](https://msdn.microsoft.com/library/ms188369.aspx)
- [sp_execute](https://msdn.microsoft.com/library/ff848746.aspx)
- [sp_executesql](https://msdn.microsoft.com/library/ms188001.aspx)
- [sp_fkeys](https://msdn.microsoft.com/library/ms175090.aspx)
- [sp_pdw_add_network_credentials](https://msdn.microsoft.com/library/mt204011.aspx)
- [sp_pdw_database_encryption](https://msdn.microsoft.com/library/mt219360.aspx)
- [sp_pdw_database_encryption_regenerate_system_keys](https://msdn.microsoft.com/library/mt204033.aspx)
- [sp_pdw_log_user_data_masking](https://msdn.microsoft.com/library/mt204023.aspx)
- [sp_pdw_remove_network_credentials](https://msdn.microsoft.com/library/mt204038.aspx)
- [sp_pkeys](https://msdn.microsoft.com/library/ms189813.aspx)
- [sp_prepare](https://msdn.microsoft.com/library/ff848808.aspx)
- [sp_spaceused](https://msdn.microsoft.com/library/ms188776.aspx)
- [sp_special_columns_100](https://msdn.microsoft.com/library/mt204025.aspx)
- [sp_sproc_columns](https://msdn.microsoft.com/library/ms182705.aspx)
- [sp_statistics](https://msdn.microsoft.com/library/ms173842.aspx)
- [sp_tables](https://msdn.microsoft.com/library/ms186250.aspx)
- [sp_unprepare](https://msdn.microsoft.com/library/ff848735.aspx)



## <a name="set-statements"></a>NASTAVENÍ příkazy

- [NASTAVENÍ ANSI_DEFAULTS](https://msdn.microsoft.com/library/ms188340.aspx)
- [NASTAVENÍ ANSI_NULL_DFLT_OFF](https://msdn.microsoft.com/library/ms187356.aspx)
- [NASTAVENÍ ANSI_NULL_DFLT_ON](https://msdn.microsoft.com/library/ms187375.aspx)
- [NASTAVENÍ ANSI_NULLS](https://msdn.microsoft.com/library/ms188048.aspx)
- [NASTAVENÍ PARAMETRU ANSI_PADDING](https://msdn.microsoft.com/library/ms187403.aspx)
- [NASTAVENÍ ANSI_WARNINGS](https://msdn.microsoft.com/library/ms190368.aspx)
- [NASTAVENÍ ARITHABORT](https://msdn.microsoft.com/library/ms190306.aspx)
- [NASTAVENÍ ARITHIGNORE](https://msdn.microsoft.com/library/ms184341.aspx)
- [NASTAVENÍ CONCAT_NULL_YIELDS_NULL](https://msdn.microsoft.com/library/ms176056.aspx)
- [NASTAVENÍ DATEFIRST](https://msdn.microsoft.com/library/ms181598.aspx)
- [NASTAVENÍ FORMÁT_DATA](https://msdn.microsoft.com/library/ms189491.aspx)
- [NASTAVENÍ FMTONLY](https://msdn.microsoft.com/library/ms173839.aspx)
- [NASTAVENÍ IMPLICIT_TRANSACITONS](https://msdn.microsoft.com/library/ms187807.aspx)
- [NASTAVENÍ LOCK_TIMEOUT](https://msdn.microsoft.com/library/ms189470.aspx)
- [NASTAVENÍ NUMBERIC_ROUNDABORT](https://msdn.microsoft.com/library/ms188791.aspx)
- [NASTAVENÍ QUOTED_IDENTIFIER](https://msdn.microsoft.com/library/ms174393.aspx)
- [NASTAVENÍ ROWCOUNT](https://msdn.microsoft.com/library/ms188774.aspx)
- [NASTAVENÍ VELIKOST TEXTU](https://msdn.microsoft.com/library/ms186238.aspx)
- [NASTAVIT ÚROVEŇ IZOLACE TRANSAKCE](https://msdn.microsoft.com/library/ms173763.aspx)
- [NASTAVENÍ XACT_ABORT](https://msdn.microsoft.com/library/ms188792.aspx)


## <a name="next-steps"></a>Další kroky
Další informace najdete v článku [Přehled SQL datový sklad][].

<!--Image references-->

<!--Article references-->
[Přehled SQL datový sklad]: sql-data-warehouse-overview-reference.md

<!--MSDN references-->
