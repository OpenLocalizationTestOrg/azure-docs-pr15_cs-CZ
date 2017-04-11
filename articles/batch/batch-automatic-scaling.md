<properties
    pageTitle="Automaticky měřítko výpočet uzlů v Azure dávku fondu | Microsoft Azure"
    description="Povolte automatické měřítko na fond cloudu dynamicky upravte počtu uzlů výpočetním ve fondu."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>Automaticky měřítko výpočet uzlů v fondu dávku Azure

S automatické měřítko službu Azure dávku dynamicky přidat nebo odebrat výpočetní uzlů v fond parametrů, které definujete na. Potenciálně uložením čas i peníze automaticky úpravou množství výpočetním energie aplikace – přidání uzlů jako práce na úkolu požadavky zvýšení a odebírat při jejich snížit.

Povolení automatické měřítko fond uzlů výpočetním přidružením s ním *Automatické měřítko vzorce* , které definujete jako s [PoolOperations.EnableAutoScale] [ net_enableautoscale] metoda v knihovně [Dávku .NET](batch-dotnet-get-started.md) . Službu dávku použije tento vzorec pro určení počtu uzlů výpočetním, které jsou potřebné pro spuštění svého pracovního vytížení. Dávkové odpovídá služby metriky údajům, které byly shromážděny pravidelně a upraví počtu uzlů výpočetním ve fondu na Konfigurovat interval podle vzorce.

Můžete povolit automatické měřítko, když je vytvořeno fond nebo na existující fond. Můžete taky změnit stávající vzorec na skupinu, která je automatické "měřítko" povolené. Dávkové poskytuje možnost vyhodnocení vzorce před jejich přiřazování k fondů, stejně jako sledování stavu automatické změny velikosti spustí.

## <a name="automatic-scaling-formulas"></a>Automatické změny velikosti vzorce

Automatické změny velikosti vzorec je se řetězcem, který určíte, který obsahuje jeden nebo více příkazů a přiřazené k vytvoření fondu [autoScaleFormula] [ rest_autoscaleformula] prvek (dávku ZBYTEK) nebo [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] vlastnosti (dávku .NET). Přiřazeno fond, služba dávku použije vzorec k určení cílové počtu uzlů výpočetním ve fondu pro další interval zpracování (Další na pozdější intervalů). Vzorce řetězec nesmí být delší než 8 KB velikosti, mohou obsahovat až 100 příkazy, které jsou oddělené středníkem a mohou obsahovat konce řádků a komentáře.

Můžete si funkci automatické změny velikosti vzorce jako je použití automatické dávku měřítko "jazyk." Vzorce příkazy jsou formátované uvolnit výrazy, které mohou obsahovat definované služby proměnné (proměnné definované službou dávku) a uživatelem definovaných proměnné (proměnné, které definujete). Mohli dělat různé operace na tyto hodnoty pomocí předdefinovaných typů, operátory a funkce. Výkaz může trvat následující formulář:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Vzorce obvykle obsahují více příkazů, které v předchozí příkazy provádět operace s hodnotami, které jsou získány. Například nejdřív získáváme hodnotu `variable1`, pak jí funkce k naplnění `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

S těmito příkazy ve vzorci k dispozici u řady výpočetním uzlů, které fondu měřítka-cíl **cílové** počet **vyhrazené uzlů**. Toto číslo může být vyšší, malá nebo stejný jako aktuální počtu uzlů v fondu. Dávkové vyhodnotí do fondu automatické měřítko vzorec v určitých intervalech ([automatické změny velikosti intervaly](#automatic-scaling-interval) jsou uvedeny níže). Potom upraví cílové počtu uzlů v fondu číslo určující při vyhodnocení vzorce automatické měřítko.

Rychlý příklad tento vzorec automatické dvě čáry Měřítko určuje, je třeba počtu uzlů upravit podle počet aktivních úkolů, maximální hodnota je 10 výpočetním uzlů:

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

Další několik částech tohoto článku jsou uvedeny různých entit tvořící vzorců automatické měřítko, včetně proměnných, operátory, operací a funkcí. Zjistíte, jak získat různých výpočetním zdroj a úkol metriky v rámci dávku. Pomocí těchto metriky můžete inteligentně upravit počet uzel vašeho fondu podle zdroje využití a stavu úkolů. Potom naučíte vytvořit vzorec a povolit automatické měřítko fond pomocí dávku ZBÝVAJÍCÍ a rozhraní API .NET. Budeme se dokončete nastavení s několika příklady vzorců.

> [AZURE.IMPORTANT] Každý účet Azure dávka se omezí na maximální počet jádra (a tedy výpočetní uzly), který se dá použít pro zpracování. Služba dávku vytvoří uzly pouze až toto omezení core. Proto nemusí být doručena cílové počet výpočetním uzlů zadané ve vzorci. Informace o zobrazení a zvětšení kvóty účtu najdete v článku [kvót a limity pro službu Azure dávku](batch-quota-limit.md) .

## <a name="variables"></a>Proměnné

Jak **Služba definovaná** a **uživatelem definovaných** proměnné můžete použít ve vzorcích automatické měřítko. Služba definovaná proměnné jsou součástí služby dávku – některé pro čtení i zápis a některé jen pro čtení. Uživatelem definované proměnné jsou proměnné *můžete* definovat. Ve vzorci příklad řádku dvou výše uvedená `$TargetDedicated` je služba definovaná proměnné při `$averageActiveTaskCount` je proměnná definované uživatelem.

Následující tabulky zobrazení pro čtení i zápis i jen pro čtení proměnné, které jsou definovány službou dávku.

Je možné **získat** a **Nastavení** hodnoty z těchto služeb definované proměnných ke správě počtu uzlů výpočetním do fondu:

| Proměnné definované služby pro čtení i zápis | Popis |
| --- | --- |
| $TargetDedicated | **Cíl** počet **vyhrazené výpočetním uzly** fondu. Toto je počet výpočetním uzlech, které fondu měřítka k. Jedná se o "cílový" číslo, protože je možné fondu pozor, abyste dosáhla cílové počtu uzlů. K tomu může dojít, pokud cílové počtu uzlů je upravil znovu následné automatické měřítko hodnocení před fondu dosáhl počáteční cílové. Také může dojít, pokud dávku účtu uzel nebo core přiděleného ještě před cílové počtu uzlů. |
| $NodeDeallocationOption | Proces, který bude proveden poté výpočetním uzly budou odebrány z fondu. Možné hodnoty jsou:<ul><li>**requeue**– okamžitě ukončí úkoly a umístí znovu zapněte fronty úloh tak, aby se nemění plán.<li>**ukončení**– okamžitě ukončí úkoly a odebere z fronty úloh.<li>**taskcompletion**– čeká aktuálně spuštěných úkolů k dokončení a pak na uzel odebere z fondu.<li>**retaineddata**– všechny místní data jsou zachovány úkolu čeká na uzel vyčistit před odebráním uzel z fondu.</ul> |

Můžete **získat** hodnotu z těchto služeb definované proměnných úpravy, které jsou založeny na metriky ze služby dávku:

| Jen pro čtení proměnné definované služby | Popis |
| --- | --- |
| $CPUPercent | Průměrné procento využití procesoru. |
| $WallClockSeconds | Počet sekund spotřebované množství. |
| $MemoryBytes | Průměrný počet megabajtů použít. |
| $DiskBytes | Průměrný počet gigabajtů použité na místním disku. |
| $DiskReadBytes | Přečtěte si počet bajtů. |
| $DiskWriteBytes | Počet bajtů napsané. |
| $DiskReadOps | Počet operací čtení disku. |
| $DiskWriteOps | Počet hodnot v zápis disku operací. |
| $NetworkInBytes | Počet bajtů. |
| $NetworkOutBytes | Počet bajtů. |
| $SampleNodeCount | Počet hodnot v výpočetním uzlů. |
| $ActiveTasks | Počet úkolů v aktivního stavu. |
| $RunningTasks | Počet úkolů spuštěna. |
| $PendingTasks | Součet $ActiveTasks a $RunningTasks. |
| $SucceededTasks | Počet úkolů, které byla úspěšně dokončena. |
| $FailedTasks | Počet úkolů, které se nezdařila. |
| $CurrentDedicated | Aktuální počet vyhrazené výpočet uzlů. |

> [AZURE.TIP] Jen pro čtení, služba definovaná proměnné uvedené výše jsou *objekty* , které poskytují různé metody pro přístup k datům spojených s jednotlivými. Další informace najdete v článku [získání ukázkových dat](#getsampledata) pod.

## <a name="types"></a>Typy

Tyto **typy** nejsou podporované ve vzorci.

- dvojité
- doubleVec
- doubleVecList
- řetězec
- časové razítko – časové razítko je víceslovné struktura, která obsahuje následující členy:

    - rok
    - měsíc (1 až 12)
    - den (1 – 31)
    - Funkce Weekday (ve formátu čísla, například 1 pro pondělí)
    - hodina (v 24hodinový formát čísla, například 13 znamená 1 odp.)
    - Funkce MINUTE (00 až 59)
    - druhé (00 až 59)
- TimeInterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>Operace

Tyto **operace** jsou povoleny na typech, které jsou uvedené výše.

| Operace                             | Podporované operátory   | Typ výsledku   |
| ------------------------------------- | --------------------- | ------------- |
| dvojité *operátor* dvojité              | +, -, *, /            | dvojité            |
| dvojité *operátor* timeinterval        | *                     | TimeInterval      |
| doubleVec *operátor* dvojité           | +, -, *, /            | doubleVec         |
| *operátor* doubleVec doubleVec        | +, -, *, /            | doubleVec         |
| TimeInterval *operátor* dvojité        | *, /                  | TimeInterval      |
| *operátor* timeinterval TimeInterval  | +, -                  | TimeInterval      |
| časové razítko TimeInterval *operátor*     | +                     | časové razítko         |
| časové razítko *operátor* timeinterval     | +                     | časové razítko         |
| časové razítko *operátor* časové razítko        | -                     | TimeInterval      |
| *operátor*dvojité                      | -, !                  | dvojité            |
| *operátor*timeinterval                | -                     | TimeInterval      |
| dvojité *operátor* dvojité              | < < =, ==, > =, >,! =  | dvojité            |
| řetězec *operátor* řetězec              | < < =, ==, > =, >,! =  | dvojité            |
| časové razítko *operátor* časové razítko        | < < =, ==, > =, >,! =  | dvojité            |
| *operátor* timeinterval TimeInterval  | < < =, ==, > =, >,! =  | dvojité            |
| dvojité *operátor* dvojité              | & &, & #124; & #124;      | dvojité            |

Při testování typu double s Ternární operátor (`double ? statement1 : statement2`) nenulovou hodnotu **true**a nulovou hodnotu **false**.

## <a name="functions"></a>Funkce

Tyto předdefinované **funkce** jsou dostupné pro použití při definování automatické změny velikosti vzorec.

| (Funkce)                          | Typ vrácené hodnoty   | Popis
| --------------------------------- | ------------- | --------- |
| AVG(doubleVecList)                | dvojité        | Vrátí průměrnou hodnotu pro všechny hodnoty v doubleVecList.
| Len(doubleVecList)                | dvojité        | Vrátí délku vektoru, který je vytvořený ze doubleVecList.
| LG(Double)                        | dvojité        | Vrátí základ 2 dvojitého protokol.
| LG(doubleVecList)                 | doubleVec     | Vrátí základ 2 doubleVecList componentwise protokolu. Vec(double) musí být explicitně předán parametru. V opačném se dosadí dvojité lg(double) verze.
| ln(Double)                        | dvojité        | Vrátí přirozený protokolu dvojitého.
| ln(doubleVecList)                 | doubleVec     | Vrátí základ 2 doubleVecList componentwise protokolu. Vec(double) musí být explicitně předán parametru. V opačném se dosadí dvojité lg(double) verze.
| log(Double)                       | dvojité        | Vrátí protokol základu 10 dvojitého.
| log(doubleVecList)                | doubleVec     | Vrátí protokol componentwise základu 10 doubleVecList. Vec(double) musí být předán explicitně pro jeden dvojité parametr. V opačném se dosadí dvojité log(double) verze.
| Max(doubleVecList)                | dvojité        | Vrátí maximální hodnotu v doubleVecList.
| min(doubleVecList)                | dvojité        | Vrátí minimální hodnotu v doubleVecList.
| Norm(doubleVecList)               | dvojité        | Vrátí normu dvě vektoru, který je vytvořený ze doubleVecList.
| percentil (doubleVec v dvojitých p) | dvojité        | Vrátí hodnotu prvku percentilu vektoru v.
| rand()                            | dvojité        | Vrátí náhodné hodnoty mezi 0,0 a 1,0.
| Range(doubleVecList)              | dvojité        | Vrátí rozdíl mezi minimální a maximální hodnoty v doubleVecList.
| STD(doubleVecList)                | dvojité        | Vrátí výběrová směrodatná odchylka hodnot v doubleVecList.
| stop()                            |               | Zastavení vyhodnocování výrazu neobsahovaly text.
| SUM(doubleVecList)                | dvojité        | Vrátí součet druhých všechny komponenty doubleVecList.
| čas (řetězec, data a času = "")          | časové razítko     | Pokud je předaná vrátí časové razítko aktuální čas, pokud se bez parametrů nebo časového razítka řetězce data a času. Formáty podporované data a času se W3C DTF RFC 1123.
| Funkce Val (doubleVec v dvojitých i)        | dvojité        | Vrátí hodnotu prvku, který je v umístění i ve vektorové v s indexu založeného na počáteční nuly.

Některé funkce, které jsou popsané v předchozí tabulce chvíli přijmout seznam jako argument. Seznam oddělený čárkami je libovolná kombinace *dvojité* a *doubleVec*. Příklad:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

*DoubleVecList* hodnota je převedena na jeden *doubleVec* před hodnocení. Například pokud `v = [1,2,3]`, pak volání `avg(v)` odpovídá k voláte `avg(1,2,3)`. Volání `avg(v, 7)` odpovídá k voláte `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Získání ukázková data

Automatické měřítko vzorce fungují s daty metriky (příklady), která poskytuje společnost službu dávku. Vzorec zvětší nebo zmenší velikost fondu na základě hodnot, které získává ze služby. Proměnné definované služby, které jsou popsané výše jsou objekty, které poskytují různé způsoby pro přístup k datům, která je přidružená k objektu. Následující výraz například ukazuje žádost o Zobrazí posledních pět minut využití procesoru:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Metoda | Popis |
| --- | --- |
| GetSample() | `GetSample()` Metoda vrátí Vektor údajům.<br/><br/>Ukázka je 30 sekund jmění metriky data. Ukázky jinými slovy, jsou získány každých 30 sekund. Ale níže uvedených, zpoždění mezi při výběru získáváme a kdy je k dispozici pro vzorce. Ne všechny vzorky za dané období jako takové může být k dispozici pro vyhodnocení vzorcem.<ul><li>`doubleVec GetSample(double count)`<br/>Určuje číslo ukázky získat z posledních ukázky, které byly shromážděny.<br/><br/>`GetSample(1)`Vrátí poslední dostupné vzorku. Pro metriky jako `$CPUPercent`, ale to není určená k použití protože není možné vědět, *Pokud* byla vybrána vzorku. Může to být poslední nebo kvůli problémům se systému, může to být mnohem starší. Je lepší v takovém případě můžete časový interval, jak je ukázáno v následujícím příkladu.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Určuje časové rozmezí pro shromažďování ukázková data. Volitelně můžete také určuje procento ukázky, které musí být k dispozici v požadované časové rozmezí.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`Vrátí 20 vzorků v případě se účastní historii CPUPercent všechny vzorky poslední deset minut. Pokud poslední chvíli historie není k dispozici, ale jenom 18 vzorky bude vrácen. V tomto případě:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`by nepovede, protože 90 naplánováno vzorků jsou k dispozici.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`by úspěšné.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Určuje časové rozmezí pro shromažďování dat s počáteční čas a koncový čas.<br/><br/>Výše uvedené, je prodleva mezi při výběru získáváme a kdy je k dispozici pro vzorce. To musí být považovány za při použití `GetSample` metody. Viz `GetSamplePercent` níže.|
| GetSamplePeriod() | Vrátí dobu ukázky, které byly odebrány v množině dat historických vzorku. |
| Count() | Vrátí celkový počet vzorky v metrických historie. |
| HistoryBeginTime() | Vrátí časové razítko vzorku od nejstaršího k dispozici data metriky. |
| GetSamplePercent() |Vrátí podíl ukázky, které jsou k dispozici pro dané časový interval. Příklad:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Protože `GetSample` metoda selže, pokud poslaný procento vzorků menší než `samplePercent` zadán, můžete použít `GetSamplePercent` způsob, jak, zkontrolujte nejdřív. Pokud nedostatečná ukázky nacházejí, bez zastavení automatické změny velikosti hodnocení pak můžete provádět alternativní akce.|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Ukázky, procenta vzorku a metodu *GetSample()*

Základní operace automatické měřítko vzorce je získat data metriky úkolu a zdroje a pak upravte velikost fondu podle tato data. Jako takové je důležité, abyste měli vymazat znalost interakce automatické měřítko vzorce s metriky dat nebo "vzorky."

**Vzorky**

Služba dávku pravidelně trvá *Ukázky* úkolu a zdroje metriky a zpřístupňuje automatické měřítko vzorce. Tyto příklady jsou zaznamenány každých 30 sekund službou dávku. Však se obvykle používá některé latence, který způsobuje zpoždění mezi kdy byly zaznamenány tyto příklady a pokud jsou k dispozici (a může být jen pro čtení) automatické měřítko vzorce. Navíc kvůli různých faktorů například sítě nebo jiné problémy infrastruktury ukázky nemusí byly zaznamenány určité intervalu. Výsledkem "chybějící" ukázky.

**Ukázka procento**

Když `samplePercent` předána `GetSample()` metoda nebo `GetSamplePercent()` s názvem metody, "%" odkazuje na srovnání celkové *možné* počet ukázky, které jsou zaznamenány službou dávku a počet ukázky, které jsou ve skutečnosti *k dispozici* pro automatické měřítko vzorec.

Podívejme se na 10 minut timespan jako příklad. Protože vzorky jsou zaznamenány každých 30 sekund, v rámci 10 minut timespan maximální počet ukázky, které jsou zaznamenány dávku bude 20 vzorků (2 za minutu). Ale kvůli inherentní latence podřízenosti mechanismus nebo nějaký jiný problém v rámci Azure infrastruktury doména může obsahovat pouze 15 ukázky, které jsou k dispozici pro automatické měřítko vzorec pro čtení. To znamená, že pro dané období 10 minut pouze **75 %** celkového počtu vzorků zaznamenané skutečně k dispozici do vzorce.

**GetSample() a výběru oblasti**

Automatické měřítko vzorců se chystáte Kvetoucí a zmenšením fondů – uzly přidávání nebo odebírání uzlů. Protože uzly náklady peníze, budete chtít zajistit, že vzorcích používat metodu inteligentní analýzy založený na dostatečné údaje. Proto doporučujeme použít analýza nejoblíbenější typů ve vzorcích. Tento typ zvětšovat a zmenšovat vaší fondů podle *rozsahu* shromážděné vzorky.

Postup použití `GetSample(interval look-back start, interval look-back end)` vrátíte **vektorové** vzorků:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Při vyhodnocování řádku nad tak, že dávka vrátí oblasti vzorků jako vektor hodnot. Příklad:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Jakmile získaných vektorové vzorky, můžete používat funkcí jako `min()`, `max()`, a `avg()` odvodit z oblasti shromážděných smysluplné hodnoty.

Pro větší zabezpečení můžete vynutit vzorců *selhání* menší než určité procento vzorku je k dispozici pro určitého časového období. Při vynucení vzorců selhání pokyn dávku vyhodnocení vzorce přestane dál, pokud není k dispozici – dosažení zadaného procenta ukázky a budou bez změny velikosti fondu. Chcete-li požadované procento vzorků pro vyhodnocení úspěšné, zadejte jako parametr třetí `GetSample()`. Tady je určen povinné 75 procent vzorků:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Také je důležité z důvodu výše uvedených zpoždění v dostupnosti vzorku, vždy stanovit časový rozsah vzhled zpět počáteční čas, který je starší než jednu minutu. Důvodem je, že trvá asi minutu ukázky se rozšíří celém systému, tak v oblasti vzorky `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` často nebudou k dispozici. Znovu použít parametr procento `GetSample()` vynutit požadavek na konkrétní vzorek procentuální hodnotu.

> [AZURE.IMPORTANT] Jsme **důrazně doporučujeme** tuto *pouze *Vyhněte se může ** na `GetSample(1)` ve vaší vzorce automatické měřítko **. Důvodem je, že `GetSample(1)` v podstatě říká ke službě dávku "získávat posledního vzorku máte, bez ohledu na to, jak dlouho nedávno jste získali." Vzhledem k tomu je pouze jeden vzorek a může být starší vzorku, nemusí být zástupce větší vzhledu poslední úkolu nebo zdroje stavu. Pokud používáte `GetSample(1)`, ujistěte se, že je součástí větší údajů a není pouze datový bod, které využívá vzorce.

## <a name="metrics"></a>Metriky

Metriky **zdroj** a **úkol** můžete použít, když definujete vzorec. Upravit cílovou počet vyhrazené uzlů v fondu podle data metriky získat a vyhodnotit. Na každé míru naleznete v části [proměnné](#variables) nad Další informace.

<table>
  <tr>
    <th>Metriky</th>
    <th>Popis</th>
  </tr>
  <tr>
    <td><b>Zdroje</b></td>
    <td><p><b>Metriky zdroje</b> jsou založené na využití procesoru, šířka pásma a paměti výpočetním uzly, jakož i počtu uzlů.</p>
        <p> Tyto služby definované proměnné jsou vhodné k provádění úprav na základě počtu uzlů:</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Tyto služby definované proměnné jsou vhodné k provádění úprav podle uzel používání zdrojů:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Úkol</b></td>
    <td><p><b>Úkol metriky</b> na základě stavu úkolů, jako jsou aktivní, čeká na vyřízení a dokončen. Následující služby definované proměnné jsou vhodné k provádění úprav velikost fondu metrice úkolu:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Napište vzorec automatické měřítko

Příkazy, které používají výše uvedené prvky tvořící sestavte automatické měřítko vzorec a potom kombinovat tyto příkazy do dokončení vzorce. V této části vytvoříme vzorec automatické měřítko příklad, kterého lze provést některé změny měřítka rozhodnutí reálný.

Nejdřív si definovat požadavky na náš nový vzorec automatické měřítko. Vzorec by měl:

1. **Zvětšení** cílové počtu uzlů výpočetní do fondu pokud vytížení procesoru dostatečně vysoká.
2. **Zmenšení** cílové počtu uzlů výpočetním do fondu po nízké využití procesoru.
3. Vždy omezte **maximální** počet uzlů 400.

*Zvýšení* počtu uzlů během vysoké využití procesoru, jsme definování příkazu, který naplní proměnnou definované uživatelem (`$totalNodes`) s hodnotou, která je 110 procent aktuální cílové počtu uzlů, ale jen tehdy, když byl minimální průměrná využití procesoru během posledních 10 minut nad 70 procent. V opačném použijeme aktuální vyhrazené hodnotu.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

*Snížení* počtu uzlů během nízké využití procesoru, nastaví další následný výběr v naší vzorec stejné `$totalNodes` proměnné 90 procent aktuální cílové počtu uzlů v případě průměr využití procesoru za posledních 60 minut byl ve skupinovém rámečku 20 procent. V ostatních případech použít aktuální hodnotu `$totalNodes` , jsme vyplněné v příkazu výše.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Teď počet cílové vyhrazené výpočetním uzlů **maximální** 400:

```
$TargetDedicated = min(400, $totalNodes)
```

Tady je celý vzorec:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Vytvoření fondu automatické měřítko s podporou

Vytvořit nový fond s podporou neobsahovaly text, můžete použít jednu z následujících postupů:

**Dávkové .NET**

1. Vytvoření fondu s [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx).
1. Nastavte vlastnost [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) `true`.
1. Nastavte vlastnost [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) se vzorcem automatické měřítko.
1. (Volitelné) Nastavte vlastnost [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx) (výchozí hodnota je 15 minut).
1. Potvrďte fondu s [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) nebo [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx).

**Dávkové rozhraní REST API**

* [Přidat fond k účtu](https://msdn.microsoft.com/library/azure/dn820174.aspx): Zadejte `enableAutoScale` a `autoScaleFormula` prvky v žádosti o rozhraní REST API pro nastavení automatické změny velikosti fondu po jeho vytvoření.

Následující fragment kódu vytvoří fondu automatické měřítko s podporou pomocí [Dávku .NET] [ net_api] knihovny. Automatické měřítko vzorec do fondu nastaví cílové počtu uzlů až 5 v pondělí a 1 na každý den týdne. [Interval automatické změny velikosti](#automatic-scaling-interval) nastavenou 30 minut. V tomto a jiných C# výstřižky v tomto článku, "myBatchClient" je správně spuštění instancí [BatchClient][net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

Kromě dávku REST API a .NET SDK můžete některou z dalších [SDK dávku](batch-technical-overview.md#batch-development-apis), [rutiny prostředí PowerShell dávku](batch-powershell-cmdlets-get-started.md)a [Rozhraní příkazového řádku dávku](batch-cli-get-started.md) pro práci s neobsahovaly text.

> [AZURE.IMPORTANT] Když vytvoříte fondu podporou automatické měřítko, musíte **není** zadat `targetDedicated` parametr. Také, pokud chcete-li ručně změnit velikost fondu automatické měřítko s podporou (například s [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]), potom musí první **Zakázat** automatické změny velikosti fond, změnit jeho velikost.

### <a name="automatic-scaling-interval"></a>Interval automatické změny velikosti

Ve výchozím nastavení služby dávku upraví velikost fondu podle vzorce jeho automatické měřítko každých **15 minut**. Je tento interval, která dokáže nahradit, ale pomocí následující vlastnosti fondu:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (dávkové .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API)

Minimální interval je pět minut, a maximální hodnota je 168. Pokud není zadán interval mimo tato oblast, službu dávku vrátí chybu chybná požádat o (400).

> [AZURE.NOTE] Neobsahovaly text momentálně není určená k odpovídání na změny za méně než jednu minutu, ale je určená nastavte velikost fondu postupně při spuštění úlohu.

## <a name="enable-autoscaling-on-an-existing-pool"></a>Povolení neobsahovaly text na existující fond

Pokud jste už vytvořili fond číslem sadu uzlů výpočetním pomocí parametru *targetDedicated* , můžete pořád povolit neobsahovaly text na fondu. Každý dávku SDK obsahuje operace "Povolit automatické měřítko", například:

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] (dávkové .NET)
*  [Povolení automatické měřítko na fond] [ rest_enableautoscale] (REST API)

Pokud povolíte neobsahovaly text na existující fond, platí následující:

* Pokud automatické měřítko právě na fondu **Zakázané** při vydání žádost o "Povolit automatické měřítko", je *nutné* zadat vzorec platné automatické měřítko při vydání žádost. *Volitelně* můžete určit interval automatické měřítko hodnocení. Pokud nezadáte interval, bude použita výchozí hodnota 15 minut.

* Pokud automatické měřítko právě **povolený** na fondu, můžete zadat vzorec automatické měřítko a interval hodnocení. Nelze vynechat obě vlastnosti.

  * Pokud chcete zadat nový hodnocení interval automatické měřítko, přerušili existující plán zkušební a začít nový plán. Čas zahájení nového plánu je údaj o čase vydání žádost "Povolit automatické měřítko".

  * Pokud nezadáte funkci Automatické měřítko nebo hodnocení interval, službu dávku nadále používat aktuální hodnotu tohoto nastavení.

> [AZURE.NOTE] Pokud hodnota byla zadané pro parametr *targetDedicated* při vytváření fondu, je ignorován při automatické změny velikosti vzorec Vyhodnocená každá její položka.

Tento fragment kódu C# používá [Dávku .NET] [ net_api] knihovny tak, aby povolit neobsahovaly text na existující fond:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Aktualizace Automatické měřítko vzorec

Použít stejnou "Povolit automatické měřítko" žádost o *aktualizaci* vzorec na existující automatické měřítko s podporou fond (například s [EnableAutoScale] [ net_enableautoscale] v dávku .NET). Neexistuje žádné zvláštní operace "aktualizovat automatické měřítko". Například pokud neobsahovaly text je už povolili na "myexistingpool" při spuštění následující kód, vzorec jeho automatické měřítko nahrazen obsah `myNewFormula`.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Interval automatické měřítko aktualizace

Při aktualizaci vzorec automatické měřítko, použití stejné [EnableAutoScale] [ net_enableautoscale] způsob, jak můžete změnit interval automatické měřítko hodnocení existujícího fondu podporou automatické měřítko. Chcete-li například nastavit interval automatické měřítko hodnocení až 60 minut pro skupinu, která už je, u kterých je povolené automatické měřítko:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Vyhodnocení vzorce automatické měřítko

Před použitím do fondu můžete vyhodnocení vzorce. Tímto způsobem můžete provádět "test směrového" vzorce zobrazit, jak zjistit její příkazy před umístěním vzorec do provozu.

Chcete-li vyhodnocení vzorce automatické měřítko, musíte první **Povolit neobsahovaly text** na fondu **platný vzorec**. Pokud chcete testovat vzorec na skupinu, která ještě nemá povolené neobsahovaly text, můžete použít funkci jednořádkový `$TargetDedicated = 0` nejdřív povolíte neobsahovaly text. Potom pomocí jednu z těchto vyhodnocení vzorce, které chcete testovat:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) nebo [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    Tyto metody dávku .NET vyžadují ID existující fond a řetězec obsahující funkci Automatické měřítko pro hodnocení. Výsledky vyhodnocení jsou obsaženy v vrácené [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) instance.

* [Automatické změny velikosti vzorec vyhodnotit](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    V této pozvánce rozhraní REST API zadejte ID fondu v URI a funkci Automatické měřítko v elementu *autoScaleFormula* požadavku. Odpověď operace obsahuje všechny informace o chybě, která může relaci ve vzorci.

V tomto [Dávku .NET] [ net_api] fragment kódu vyhodnocení vzorce před jeho použití [CloudPool][net_cloudpool]. Pokud fondu nemá povolené neobsahovaly text, můžeme ji povolit nejdřív.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Výstup podobně jako tento způsobí úspěšné vyhodnocení vzorce v tomto fragment kódu:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Získat informace o spuštění automatické měřítko

Abyste měli jistotu, že vzorec funguje očekávaným způsobem, doporučujeme že pravidelně kontrolovat výsledky neobsahovaly text "se spustí" dávku provede vašeho fondu. Ano, získat (nebo aktualizace) odkaz do fondu a zkontrolujte vlastnosti jeho poslední automatické měřítko spustit.

Vlastnost [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) v dávku .NET má několik vlastností poskytuje informace o nejnovějších automatické změny velikosti běžet provedena fondu službou dávku.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

V rozhraní REST API žádost [získat informace o fond](https://msdn.microsoft.com/library/dn820165.aspx) vrátí informace o skupině, která zahrnuje nejnovější automatické změny velikosti spustit informace v [autoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun).

Následující C# fragment kódu používá knihovnu dávku .NET k tisku informací o poslední neobsahovaly text spustit ve fondu "myPool":

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Ukázkový výstup předchozí fragment kódu:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Příklady vzorců automatické měřítko

Podívejme se na několik vzorců, které ukazují různé způsoby nastavení časového využití zdrojů do fondu.

### <a name="example-1-time-based-adjustment"></a>Příklad 1: Úprava založená na čase

Možná budete chtít upravit velikost fondu na základě den týdne a dne, zvětšete nebo zmenšete počtu uzlů v fondu příslušným způsobem.

Tento vzorec první získává aktuální čas. Pokud je den v týdnu (1-5) a v pracovní době (8: 00 do 18: 00) Cílová velikost fondu nastavenou 20 uzlů. V ostatních případech je nastavena na 10 uzlů.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Příklad 2: Úprava založeným na úkolech

V tomto příkladu je upravena velikost fondu na základě počtu úlohy ve frontě. Všimněte si, že jsou přijatelné v vzorce řetězce komentáře a konce řádků.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Příklad 3: Účetnický paralelní úkolů

Toto je jiný příklad, která upraví velikost fondu zadaný počet úkolů. Tento vzorec taky bere ohled [MaxTasksPerComputeNode] [ net_maxtasks] hodnotu, která skupiny. Toto je užitečné v situacích, kdy [spuštění paralelní úkolu](batch-parallel-node-tasks.md) je povolená na váš fond.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Příklad 4: Nastavení počáteční fondu velikosti

Tento příklad ukazuje C# kódu s automatické měřítko vzorec, který nastavuje velikost fondu na počtu uzlů pro počáteční časové období. Potom upraví velikost fondu na základě počtu spuštění a aktivní úkoly po uplynutí počáteční časové období.

Vzorec v následujících fragment kódu:

- Nastaví velikost počáteční fondu čtyři uzlů.
- Neupraví fondu velikost v rámci prvních 10 minut životního cyklu do fondu.
- Za 10 minut získá maximální hodnota argumentu číslo z pracovního a aktivní úkoly během posledních 60 minut.
  - Pokud jsou obě hodnoty 0 (což znamená, že byly žádné úkoly pracovního nebo aktivní za posledních 60 minut), velikost fondu nastavena na hodnotu 0.
  - Pokud některý z hodnota je větší než nula, žádné změny.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Další kroky

* [Maximalizace dávku Azure výpočet používání zdrojů k úkolům souběžné uzel](batch-parallel-node-tasks.md) obsahuje podrobnosti o jak bude možné provést více úkolů současně ve výpočetním uzlech ve vaší fondu. Kromě neobsahovaly text může pomoci tato funkce posunout níž, pracovní dobu některých úloh, ušetříte peníze.

* Jiné efektivity nabíjecích ověřte, že aplikace dávku dotazy služby dávku nejčastěji optimální způsobem. V [dotazu služby Azure dávku efektivně](batch-efficient-list-queries.md)se dozvíte, jak omezit množství dat, protínající lince dotazu stav tisíce výpočetním uzly nebo úkolů.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
