<properties
    pageTitle="Ukázková data v tabulkách Azure HDInsight podregistru | Microsoft Azure"
    description="Dolů odběr dat v tabulkách podregistru Azure HDInsight (Hadopop)"
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Ukázková data v tabulkách podregistru Azure HDInsight

V tomto článku jsme popisují dolů ukázková data uložená v tabulkách podregistru Azure HDInsight pomocí podregistru dotazů. Doporučujeme zabývat těmito oblastmi tři způsoby popularly použité odběr:

* Jednotné výběrová
* Výběrová podle skupin
* Analytický nástroj vzorkování vrstveného

**Proč ukázková data?**
Pokud datovou sadu, které chcete analyzovat velký, není vhodné dolů ukázková data, která chcete zmenšit velikost menší, ale zástupce a lépe. To usnadňuje Principy dat, průzkum a inženýrské funkce. Jeho role v procesu týmu dat pro výzkum je to povolíte rychlé vytváření prototypů z funkcí zpracování dat a automatické učení modely.

**Nabídka** pod odkazů na témata, která popisují, jak vzorová data z různých prostředích úložiště.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Analytický nástroj vzorkování úkol je kroku v [Týmu dat pro výzkum obrázku (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>Způsob odeslání podregistru dotazů
Z konzoly Hadoop příkazového řádku na uzel hlavy clusteru Hadoop lze odeslat podregistru dotazů. K tomuto účelu přihlásit do hlavního uzlu Hadoop clusteru, spusťte konzolu Hadoop příkazového řádku a odesílání dotazů podregistru odtud. Pokyny pro odesílání podregistru dotazů v konzole Hadoop příkazového řádku najdete v článku [jak odeslat podregistru dotazů](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Jednotné výběrová
Jednotné výběrová znamená, že má každý řádek v uvedenou množinu dat rovnou příležitost z právě odběr. To lze provést přidáním navíc pole rand() uvedenou množinu dat vnitřní dotaz "select" a ve vnějším "select" dotazu zadaná podmínka na náhodné pole.

Tady je příklad dotazu:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Tady `<sample rate, 0-1>` určuje část záznamy, které uživatelé přehrajte.

## <a name="group"></a>Výběrová podle skupin

Když odběr kategorií dat, můžete zahrnout nebo vyloučit všechny instance některé určitou hodnotu kategorií proměnné. Toto je pojem "odběr skupinou".
Pokud máte kategorií proměnnou "Krajů", která obsahuje hodnoty NY MA, CA, NJ, PA, atd, chcete záznamy stejného státu vždy dohromady, zda jsou vzorky nebo ne.

Tady je příklad dotazu, které vzorky podle skupin:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Analytický nástroj vzorkování vrstveného

Výběrová je si týkající proměnnou kategorií získané vzorky si hodnoty v případě, že kategorií, které jsou ve stejném poměru jako základního souboru nadřazenou odkud vzorky získaných. V příkladu stejný jako výše Předpokládejme, že vaše data obsahují podřízené populace států, Řekněme NJ má 100 pozorování NY má 60 pozorování a WA má 300 pozorování. Pokud chcete zadat sazbu vrstveného odběr být 0,5, pak výběru získáte měli přibližně 50, 30 a 150 pozorování NJ NY a WA v tomto pořadí.

Tady je příklad dotazu:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Informace o pokročilejší odběr metody, které jsou k dispozici v podregistru najdete v článku [LanguageManual odběr](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).
