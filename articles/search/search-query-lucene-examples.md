<properties
    pageTitle="Příklady dotazu Lucene mají být vyhledávány Azure | Hledání Microsoft Azure"
    description="Lucene syntaxí dotazu pro rozmazaný hledání blízkost vyhledávání, zvýšení termínů, regulárních výrazů hledání a hledání pomocí zástupných znaků."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="liamca"
/>

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Příklady syntaxe dotazu Lucene pro vytváření dotazů v Azure hledání

Při vytváření dotazů mají být vyhledávány Azure, můžete použít výchozí [Syntaxe jednoduchého dotazu](https://msdn.microsoft.com/library/azure/dn798920.aspx) nebo alternativní [Lucene analyzátor dotazu v Azure vyhledávání](https://msdn.microsoft.com/library/azure/mt589323.aspx). Analyzátor dotazu Lucene podporuje složitější dotaz konstrukce, například s rozsahem pole dotazů, rozmazaný vyhledávání, blízkost vyhledávání, zvýšení termínů a reqular výraz vyhledávání.

V tomto článku můžete přecházet mezi příklady, které se zobrazí Lucene syntaxi dotazu a výsledky vedle sebe. Příklady kontrolovat předinstalovaná indexu vyhledávání v [JSFiddle](https://jsfiddle.net/), editoru online kódu pro testování skriptu a HTML. 

Klikněte pravým tlačítkem myši na příklady adres URL dotazu zobrazíte JSFiddle v samostatném okně prohlížeče.

> [AZURE.NOTE] Následující příklady využít indexu vyhledávání tvořené dostupné projekty založené na datovou sadu poskytovanou iniciativa [OpenData New York město](https://nycopendata.socrata.com/) . Tato data nelze považovat za aktuální nebo dokončeno. Index je službě izolovaného prostoru poskytované společností Microsoft. Nepotřebujete Azure předplatného nebo Azure hledání můžete vyzkoušet tyto dotazy.

## <a name="viewing-the-examples-in-this-article"></a>Zobrazení v příkladech v tomto článku

Některé příklady v tomto článku zadejte analyzátor Lucene dotazu pomocí parametrů hledání**queryType** . Pokud chcete použít analyzátor Lucene dotazu v kódu, budete zadávat **queryType** na každou žádost.  Platné hodnoty jsou **jednoduché**|**úplné**s **jednoduché** jako výchozí a **celou** pro analyzátor Lucene dotazu. Další informace o zadávání parametry dotazu najdete v článku [Hledání dokumentů (Azure vyhledávací služby rozhraní REST API)](https://msdn.microsoft.com/library/azure/dn798927.aspx) .

**Příklad 1** – klikněte pravým tlačítkem myši následující dotaz fragment kódu a otevřete ji v novou stránku prohlížeče, která načte JSFiddle a spustí dotaz:
- [& queryType úplné = & hledání = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Tento dotaz vrátí dokumenty z našich úlohy indexu (načíst službě izolovaného prostoru)

V novém okně prohlížeče zobrazí se do něj zdrojovou JavaScript a výstup vedle sebe. Skript odkazuje na dotaz, který není uvedený příklad adresy URL v tomto článku. Například v **příkladu 1**, v podkladovém dotazu vypadá takto:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Oznámení dotaz používá předkonfigurované indexu vyhledávání Azure s názvem nycjobs. Parametr **searchFields** omezení hledání jenom do pole název firmy. **QueryType** nastavenou **úplné**, která nastaví Azure vyhledávání a pokusí se pomocí analyzátoru Lucene dotazu pro tento dotaz.

### <a name="fielded-query-operation"></a>Operace fielded dotazu

Příklady v tomto článku můžete změnit tím, že zadáte **fieldname:searchterm** konstrukce definovat operace fielded dotazu, kde je pole pro jedno slovo a hledaný termín je rovněž jednoho slova nebo fráze, pokud chcete s logické operátory. Několik příkladů patřit následující úkoly:

- business_title:(senior NOT junior)
- Stav: ("New York" a "Nové Jersey")

Nezapomeňte vložit víc řetězec v uvozovkách požadovaná obou řetězců má být vyhodnocen jako jedna entita, stejně jako v tomto případě vyhledáte různých měst v poli místo konání. Taky zajistit, aby je operátor velkým vidět s není a AND.

Pole vybrané v **fieldname:searchterm** musí být vyhledávání pole. Podrobné informace o použití atributy indexu v poli Definice najdete v článku [Vytvoření indexu (Azure služba REST rozhraní API pro hledání)](https://msdn.microsoft.com/library/azure/dn798941.aspx) .

## <a name="fuzzy-search"></a>Rozmazaný hledání

Najde rozmazaný hledání shody řečeno, které mají podobný konstrukce. Za [si přečtěte následující dokumentaci Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)rozmazaný hledání vycházejí [Damerau Levenshtein vzdálenost](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

K provedení rozmazaný hledání, použijte tilda "~" symbol na konci jednoho slova s volitelný parametr, hodnota mezi 0 a 2, která určuje vzdálenost upravit. Například "modré ~" nebo "modré ~ 1" vrátí modré, modré a připevnit.

**Příklad 2** – kliknutí pravým tlačítkem myši následující dotaz úryvek vyzkoušejte si to. Tento dotaz vyhledává firmy názvy s vyšších termínů je, ale ne méně:

- [& queryType úplné = & hledání = business_title:senior není méně](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="proximity-search"></a>Hledání blízkost

Hledání v blízkosti slouží k vyhledání podmínek, které jsou vedle sebe v dokumentu. Vložte tilda "~" symbol na konci věty následovaný počet slov, která blízkost okraj. Například "hotelu letiště" ~ 5 vyhledá hotelu podmínky a letiště v rámci 5 slov v dokumentu.

**Příklad 3** – po kliknutí pravým tlačítkem fragment následující dotaz. Tento dotaz vyhledává úlohy s přidružit termínů (kde to je špatně napsaný):

- [& queryType úplné = & hledání = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Příklad 4** – klikněte pravým tlačítkem myši na dotaz. Vyhledání úlohy se termín "exkurzí analytik" kde je oddělená víc než jednoho slova:

- [& queryType úplné = & hledání = business_title: "exkurzí analytik" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Příklad 5** – zkuste ho znova odeberete slov mezi termín "exkurzí analytik".

- [& queryType úplné = & hledání = business_title: "exkurzí analytik" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting"></a>Doba splatnosti zvýšení

Zvýšení termínů odkazuje hodnocení dokumentu vyšší, pokud ho obsahuje boosted termín vzhledem k dokumentům, které neobsahují termín. Tím se liší od bodování profilů v tom, že bodování profily pomoct určitých polí, nikoli konkrétními termíny. Následující příklad pomáhá ilustrují rozdíly.

Zvažte bodování profil, který zvyšuje odpovídá v některých pole, například **Žánr** v příkladu musicstoreindex. Zvýšení termínů může k dalšímu zesílení určitých vyhledávací podmínky vyššími, než ostatní. Například "rock ^ 2 elektronické" bude pomoct dokumenty, které obsahují hledaných slov v poli **Žánr** vyšší než dalších polí s možností vyhledávání v indexu. Dokumenty, které obsahují hledaný termín "rock" dále budou hodnotit vyšší než "elektronické" důsledku hodnotu zesílení termínů (2) hledaný termín.

Chcete pomoct termínu, použijte stříška, "^", symbol faktor zesílení (číslo) na konci období, kterou hledáte. Vyšší faktor zesílení, tím důležitější termín budou vzhledem k jiné hledaných slov. Ve výchozím nastavení je faktor zesílení 1. I když faktor zesílení musí být kladná, může být menší než 1 (například 0,2).

**Příklad 6** – klikněte pravým tlačítkem myši na dotaz. Vyhledejte úlohy se termín "počítač analytik" vidíme, kde nejsou žádné výsledky s počítačem slova a analytik, ale analytik úlohy se v horní části výsledky.

- [& queryType úplné = & hledání = business_title:computer analytik](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Příklad 7** – akci opakujte, tento čas zvýšení výsledků s počítačem termínů přes analytik termínů Pokud neexistuje obě tato slova.

- [& queryType úplné = & hledání = business_title:computer ^ 2 analytik](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression"></a>Regulárních výrazů

Hledání regulárních výrazů najde shodu podle obsahu mezi lomítka "/", jako je popsaný v [předmětu RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Příklad 8** – klikněte pravým tlačítkem myši na dotaz. Vyhledejte úlohy pomocí termín vyšších nebo střední.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Adresu URL v tomto příkladu nebude správně zobrazeny na stránce. Jako alternativu zkopírujte následující adresu URL a vložte adresu URL prohlížeče:    `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## <a name="wildcard-search"></a>Hledání pomocí zástupných znaků

Obecně rozpoznaný syntaxi můžete použít pro více (\*) nebo jeden hledání zástupných znaků (?). Poznámka: analyzátor dotazu Lucene podporuje použít tyto symboly pomocí jednoho slova a ne frázi.

**Příklad 9** – klikněte pravým tlačítkem myši na dotaz. Vyhledejte úloh, které obsahují předpona "programové", který by obsahoval firmy názvy s podmínkami programování programmer v nich.

- [& queryType = celé & $select = business_title & hledání = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

Nelze použít * nebo? symbol jako první znak hledání.


## <a name="next-steps"></a>Další kroky

Zadejte analyzátor Lucene dotazu v kódu. Nastavení vyhledávacích dotazů .NET a rozhraní REST API popisují následující odkazy. Propojení syntaxi výchozí jednoduché, budete muset použít, co jste se naučili v tomto článku můžete určit **queryType**.

- [Dotaz vyhledávacím odkazu Azure pomocí .NET SDK](search-query-dotnet.md)
- [Dotaz vyhledávacím odkazu Azure pomocí rozhraní REST API](search-query-rest-api.md)


 