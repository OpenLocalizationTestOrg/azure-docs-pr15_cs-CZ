<properties
   pageTitle="Práce se zdroji dat "velký data za rok" | Microsoft Azure"
   description="Zvýraznění vzorků pro používání katalogu dat Azure se zdroji dat "velký data za rok", včetně úložišti objektů Blob Azure, jezera dat Azure a Hadoop HDFS článek s postupy."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Práce se zdroji velký dat v katalogu dat Azure

## <a name="introduction"></a>Úvod
**Katalog dat Microsoft Azure** je plně spravovaných cloudové služby, které bude sloužit jako systém registrace a systémem zjišťování pro zdroje dat organizace. **Katalog dat Azure** je jinými slovy, všechny o lidech nápovědu Seznamte se s principy a použití zdrojů dat a organizace nápovědu ke zjištění další hodnotu z jejich existujícího zdroje dat, včetně velký data.

**Katalog dat Azure** podporuje registrace Azure blogu úložiště objektů BLOB adresářů i souborů Hadoop HDFS a adresáře. Částečně strukturovaná přírodní těchto zdrojů dat obsahuje skvělé flexibilitu, ale taky znamená to, že uživatelé třeba vzít v úvahu uspořádání zdroje dat abyste mohli získávat maximální hodnotu z zaregistrovali **Katalog dat Azure**.

## <a name="directories-as-logical-data-sets"></a>Adresáře jako logické množinách dat.

Běžné vzor pro uspořádání zdroje velký dat se zachází adresáře jako logické datové sady. Nejvyšší úrovně adresáře slouží k definování k sadám dat, zatímco podsložky definovat oddíly a ukládat soubory, které obsahují samotná data.

Příklad tento způsob může být:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

V tomto příkladu vehicle_maintenance_events a location_tracking_events představují logické datové sady. Každá z těchto složek obsahuje datové soubory, které jsou uspořádané podle roku a měsíce do podsložky. Každá z těchto složek mohou obsahovat stovky nebo tisíce soubory.

V tomto vzoru zaregistrovali na jednotlivé soubory **Katalog dat Azure** pravděpodobně smysl. Místo toho zaregistrujte adresáře, které představují výše uvedené množiny dat, které bude dávat smysl uživatelé pracují s daty.

## <a name="reference-data-files"></a>Přehled datových souborů

Doplňková vzorek je obsahují sady dat odkaz jako jednotlivé soubory. Tyto sady dat může považovat za "small" straně velký dat a, jsou často podobné dimenzí v analytical datového modelu. Odkaz datové soubory obsahují záznamy, které se používají k poskytnout kontext pro hromadnou datové soubory uložené kdekoli jinde v úložišti velký data.

Příklad tento způsob může být:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Když analytik nebo dat vědecký pracovník pracuje s údaji ve struktuře větší adresáře, data v těchto souborů odkaz mohou sloužit k obsahují podrobnější informace u entit, které jsou podle pouze podle jména nebo ID větší uvedenou množinu dat.

V tomto vzoru ho bude smysl zaregistrovat referenčního datových souborů v **Katalogu dat Azure**. Každý soubor představuje sadu dat a každého z nich můžete poznámkami a zjistil jednotlivě.

## <a name="alternate-patterns"></a>Alternativní vzorky

Vzorky jsme je popsali výše jenom dvěma způsoby možné uspořádání velké úložiště může být, ale každý implementaci se budou lišit. Bez ohledu na to, jak jsou strukturovány zdroje dat, při registraci zdroje velký dat s **Katalogem dat Azure**se zaměřují na registrace soubory a adresáře, které představují výše uvedené množiny dat, které budou hodnoty ostatním uživatelům ve vaší organizaci. Registrace všechny soubory a adresáře můžete složky nepotřebné katalogu, díky těžší uživatelům hledání, které potřebují.

## <a name="summary"></a>Souhrn
Registrace zdroje dat s **Katalogem dat Azure** usnadňuje a seznamte se s principy. Registrace a poznámky velký datové soubory a adresáře, které představují logické sady dat, můžete pomoc uživatelům vyhledávání a používání zdrojů velký dat, který potřebují.
