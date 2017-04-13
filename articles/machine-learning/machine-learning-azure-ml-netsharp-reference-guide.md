<properties 
    pageTitle="Průvodce čistého # Neural sítí specifikace jazyka | Microsoft Azure" 
    description="Syntaxe čistého číslo neural sítě specifikace jazyka, spolu s příklady, jak vytvořit vlastní neural sítě model v aplikaci Microsoft Azure ML pomocí sítě#" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jeannt" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="jeannt"/>



# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Příručka k čistého # neural sítě specifikace jazyka pro vzdělávání počítače Azure

## <a name="overview"></a>Základní informace
Čistého # je jazyk vyvinutý společností Microsoft, který se používá k definování neural síťové architektury neural sítě modulů Microsoft Azure počítač přečíst. V tomto článku se dozvíte:  

-   Základní pojmy týkající neural sítích
-   Požadavky na neural sítě a jak definovat primární součásti
-   Syntaxe a klíčových slov specifikace jazyka čistého #
-   Příklady vlastní neural sítě vytvořené pomocí sítě# 
    
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

## <a name="neural-network-basics"></a>Základy neural sítě
Struktura neural sítě se skládá z ***uzly*** , která jsou uspořádána ve ***vrstvy***a váženého ***připojení*** (nebo ***okraje***) mezi uzly. Připojení směrová a u každého připojení má ***zdroj*** uzel a uzel ***cíl*** .  

Jeden nebo více ***připojení svazky***má každá ***trainable vrstvy*** (skrytých nebo výstup layer). Sada připojení se skládá z zdrojové vrstvy a specifikace připojení z dané vrstvě zdroje. Všechna připojení v dané příruček sdílení stejné ***zdroje vrstvy*** a stejné ***cílové vrstvě***. V čistého # příruček připojení se považuje za které patří do skupiny cílové vrstvě.  
 
Čistého # podporují různé druhy připojení sady, které umožňuje přizpůsobit tak, jak vstupní hodnoty jsou namapované skryté vrstvy a namapované výstupy.   

Výchozí nebo standardní sada je **Úplná sada**, ve kterém je každý uzel ve vrstvě zdroj připojené k všech uzlů v cílové vrstvě.  

Kromě toho čistého # podporuje následující čtyři druhy svazky připojení:  

-   **Balíčky zjednodušený formát**. Uživatele můžete definovat predikátem pomocí umístění zdrojový uzel vrstvy a cílový uzel vrstvy. Uzly připojeni kdykoli predikát hodnotu PRAVDA.
-   **Convolutional sady**. Uživatele můžete definovat malé sousedství uzlů ve vrstvě zdroje. Každý uzel v cílové vrstvy připojení k jedné složce uzlů ve vrstvě zdroje.
-   **Fond sady** a **sady normalizace odpověď**. Tyto podobají convolutional sady v tom, že uživatel definuje malé sousedství uzlů ve vrstvě zdroje. Rozdíl je, že gramáží okrajů v těchto svazky trainable. Místo toho předdefinované funkce se použije pro zdrojové hodnoty uzel určit cílový hodnotu.  

Použití sítě # k definování struktura neural sítě umožňuje definovat složité struktury tmavě neural sítí ATP convolutions libovolného dimenzí, které označují zlepšit výukové dat jako je obrázek, zvuk nebo video.  

## <a name="supported-customizations"></a>Podporované vlastní nastavení
Architektura modely neural sítě, které vytvoříte Azure počítač přečíst můžete značně přizpůsobenému pomocí čistého #. Můžeš:  

-   Vytvoření skryté vrstvy a řídit počtu uzlů v každé vrstvy.
-   Určete, jak mají být připojeni k sobě vrstvy.
-   Definování struktury zvláštní připojení, například convolutions a tloušťky sdílení sady.
-   Určení jiné aktivaci funkce.  

Podrobné informace o specifikace syntaxe jazyka najdete v článku [Specifikace strukturu](#Structure-specifications).  
 
Příklady definování neural sítě pro některé běžné počítač výukové úkoly z simplex: k složité najdete v článku [Příklady](#Examples-of-Net#-usage).  

## <a name="general-requirements"></a>Obecné požadavky
-   Musí být jeden výstup vrstvě, alespoň jeden zadávání a žádný nebo více skrytých vrstvy. 
-   Jednotlivé vrstvy má pevného počtu uzlů entity jsou uspořádané v obdélníkové pole libovolného dimenze. 
-   Zadávání vrstvy mít žádný související školení parametry a představují místa, kde instance data do sítě. 
-   Trainable vrstev (vrstvy skryté a výstupní) přidruženými školení parametry jmenoval gramáží a odchylek. 
-   Uzly zdrojové a cílové musí být v samostatných vrstev. 
-   Připojení musí být Acyklické; jinými slovy nemůže být řetězce připojení vést zpátky na počáteční zdrojový uzel.
-   Výstup vrstvy nemůžou být vrstvě zdroj balíku připojení.  

## <a name="structure-specifications"></a>Specifikace struktury
Specifikace strukturu neural sítě se skládá ze tří částí: **deklarace konstanty**, **deklarace vrstvy**, **deklarace připojení**. Je také oddíl volitelné **sdílet deklarace** . Oddíly můžete zadat v libovolném pořadí.  

## <a name="constant-declaration"></a>Deklarace konstanty 
Deklarace konstanty je nepovinný krok. To umožňuje definovat hodnoty jinde použita při definici neural sítě. Příkaz deklarace se skládá z identifikátoru následovaný symbol rovná se a hodnota výrazu.   

Následující příkaz například definuje konstanty **x**:  


    Const X = 28;  

Pokud chcete definovat dva nebo více konstanty současně, uzavřete identifikátor názvy a hodnoty do složených závorek a oddělte je pomocí středníků. Příklad:  

    Const { X = 28; Y = 4; }  

Na pravé straně jednotlivých přiřazení výraz může být celé číslo, reálné číslo, logickou hodnotou (True nebo False) nebo matematický výraz. Příklad:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Deklarace vrstvy
Požaduje deklaraci vrstvy. Definuje formát a zdroj vrstvy, včetně jejího svazky připojení a atributy. Příkaz deklarace začíná název vrstvy (při zadávání, skrýt nebo výstup) a za ním uveďte rozměry vrstvy (řazené kolekce členů kladných celých čísel). Příklad:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

-   Součin rozměry je počtu uzlů ve vrstvě. V tomto příkladu jsou dvojrozměrné [5,20], což znamená, že je 100 uzlů ve vrstvě.
-   Vrstvy lze deklarovat v libovolném pořadí s jednou výjimkou: Pokud je definován více vstupní vrstvám, pořadí, ve kterém jsou deklarovány musí odpovídat pořadí funkcí jednotkou vstupních dat..  


Chcete-li automaticky určit počtu uzlů v vrstvy, použijte klíčové slovo **automaticky** . Klíčové slovo **auto** má různých efektů, v závislosti na vrstvu:  

-   V deklarace vstupní vrstvy je počtu uzlů řadu funkcí jednotkou vstupních dat..
-   Počtu uzlů v deklaraci skryté vrstvy je číslo, které nastavil hodnota parametru pro **Počet skrytých uzlů**. 
-   Počtu uzlů v deklarace vrstvy výstup je pro dvě třídy klasifikace, 1 pro regresní a počtu uzlů výstup pro multiclass klasifikace se rovná 2.   

Například následující definice síti umožňuje velikost všechny vrstvy stanovit automaticky:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Deklarace vrstvy trainable vrstvy (vrstvy skryté nebo výstupní) můžete volitelně můžete zahrnout výstup funkci (nazývané také funkci aktivace), která ve výchozím nastavení **sigmoid** pro modely klasifikace a **Lineární** regresní modelů. (I když použijete výchozí nastavení, můžete výslovně neuvedete aktivaci funkce, pokud budete chtít pro přehlednost.)

Jsou podporované tyto funkce výstup:  

-   sigmoid
-   lineární
-   softmax
-   rlinear
-   čtverec
-   Odmocnina
-   srlinear
-   Abs
-   TGH 
-   brlinear  

Například následující deklaraci používá funkci **softmax** :  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Deklarace připojení
Ihned po definování trainable vrstvy, je třeba deklarovat připojení mezi vrstvy, kterou jste definovali. Deklarace příruček připojení začíná klíčové slovo **z**, a za ním uveďte název vrstvy zdroj do skupiny a druh příruček připojení k vytvoření.   

V současné době podporuje pět druhy svazky připojení:  

-   **Úplné** sady podle klíčových slov **všechny**
-   Sady **filtrované** podle klíčové slovo **kde**následované predikátu výrazem
-   Balíčky **Convolutional** označen klíčové slovo **convolve**, následovaný konvoluce atributy
-   Balíčky **Pooling** podle klíčových slov **maximální** nebo **mysli fondu**
-   **Normalizace odpověď** svazky podle klíčových slov **normu odpověď**      

## <a name="full-bundles"></a>Úplné sady  

Úplné připojení příruček obsahuje připojení ze všech uzlů ve vrstvě zdroj každý uzel cílové vrstvy. Toto je výchozí typ připojení.  

## <a name="filtered-bundles"></a>Filtrované sady
Specifikace příruček filtrované připojení obsahuje predikátu, vyjádřené syntakticky, velmi jako lambda výraz C#. Následující příklad definuje dva filtrované svazky:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

-   V predikát pro _ByRow_ **s** je parametr představující rejstřík do pole obdélníkové uzlů vstupní vrstvy _pixelů_a **d** je parametr představující rejstřík do pole uzly skryté vrstvy _ByRow_. Typ **s** a **d** se řazené kolekce členů s celými čísly délku dva. Entity **s** oblastí přes všechny dvojice hodnot search_column celých čísel s _0 < = s [0] < 10_ a _0 < = s[1] < 20_, a **d** oblasti přes všechny dvojice hodnot search_column celá čísla, s _0 < = [0] d < 10_ a _0 < = d[1] < 12_. 
-   Na pravé straně predikátu výraz je podmínka. V tomto příkladu pro každou hodnotu **Ne** a **d** tak, aby podmínky PRAVDA, je okraj ze skupiny vrstvy zdroje na cílový uzel vrstvy. Tento výraz filtru tedy znamená, že do skupiny zahrnuje připojení ze skupiny definované **s** uzel definované **d** ve všech případech, kdy je rovno [0] d s [0].  

Pokud chcete můžete určit sadu gramáží filtrované příruček. Hodnota atributu **gramáží** musí být řazené kolekce členů z hodnoty s plovoucí desetinnou s délkou, která odpovídá počtu připojení definován do skupiny. Ve výchozím nastavení vygenerují náhodně gramáží.  

Weight (váha) hodnoty jsou seskupená podle cílový uzel index. To znamená pokud první cílový uzel je připojené k uzly zdroj K, první _K_ prvky n-tici **gramáží** jsou gramáží pro první cílový uzel v pořadí index zdroje. Totéž platí pro zbývající uzly cíl.  

Je možné určit gramáží přímo jako konstanty. Například pokud jste se naučili dříve hmotnost, můžete je zadat jako konstanty pomocí následující syntaxe:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional sady
Strukturu homogenní po dat školení convolutional připojení se běžně používají další uceleném funkcí data. Příklad obrázku, zvuk nebo video, prostorové nebo časový dimenzionalitu lze data poměrně jednotné.  

Convolutional svazky využívat obdélníkový **jádra** , která jsou posouvá prostřednictvím rozměry. V podstatě každý jádra definuje sadu gramáží použité v místním sousedství označovaný taky jako **jádra aplikací**. Každá jádra aplikace odpovídá uzel ve vrstvě zdroje, které se označuje jako **centrální uzel**. Gramáží jádra jsou sdíleny mnoho připojení. Ve svazku convolutional každý jádra je obdélníkový a všechny aplikace jádra mají stejnou velikost.  

Convolutional svazky dál nebude podporovat následujícími atributy:

**InputShape** definuje dimenze vrstvy zdroje pro účely tohoto convolutional příruček. Hodnota musí být řazené kolekce členů kladných celých čísel. Součin celá čísla musí být rovna počtu uzlů ve vrstvě zdroje, ale jinak, nemusí podle dimenzionalitu deklarované vrstvy zdroje. Délka tento n-tici změní hodnotu **Arita** convolutional příruček. (Obvykle Arita označuje počet argumentů nebo se může trvat funkce.)  

Pokud chcete definovat obrazec a umístění jádra, použijte atributy **KernelShape**, **mezery**, **Odsazení**, **LowerPad**a **UpperPad**:   

-   **KernelShape**: (požadováno) definuje dimenze každý jádra convolutional příruček. Hodnota musí být řazené kolekce členů kladných celých čísel s délkou, která je rovno aritu do skupiny. Jednotlivé součásti tohoto n-tici nesmí být větší než odpovídající součást **InputShape**. 
-   **Mezery**: (volitelné) definuje posuvné velikost kroku konvoluce (postupně po jednotlivých krocích velikost pro každou dimenzi), která je vzdálenost mezi centrální uzlů. Hodnota musí být řazené kolekce členů kladných celých čísel s délkou, která je aritu do skupiny. Jednotlivé součásti tohoto n-tici nesmí být větší než odpovídající součást **KernelShape**. Výchozí hodnota je řazené kolekce členů se všechny komponenty roven jedné. 
-   **Sdílení**: (volitelné) definuje tloušťky sdílení pro každou dimenzi konvoluce. Hodnota může být jeden logická hodnota nebo řazené kolekce členů logických hodnot s délkou, která je aritu do skupiny. Logická hodnota je rozšířenými být řazené kolekce členů správnou délku s všechny komponenty rovno určité hodnoty. Výchozí hodnota je řazené kolekce členů skládající se ze všech skutečné hodnoty. 
-   **MapCount**: (volitelné) definuje počet funkce mapy pro convolutional příruček. Hodnota může být jeden kladné celé číslo nebo řazené kolekce členů kladných celých čísel s délkou, která je aritu do skupiny. Jeden celočíselnou hodnotu se rozšíří být řazené kolekce členů správnou délku první součástí rovno zadaná hodnota a všechny ostatní složky se rovná jedné. Výchozí hodnota je taková. Celkový počet funkce mapy je produkt komponent n-tici. Řešení tohoto celkový počet součásti určuje způsob seskupení hodnoty funkce mapy v cílové uzlů. 
-   **Gramáží**: (volitelné) definuje počáteční gramáží do skupiny. Hodnota musí být řazené kolekce členů pohyblivé čárky hodnoty s délkou, která je počet jádra počet gramáží za jádra, nadefinovaná dál v tomto článku. Vygenerují náhodně gramáží výchozí.  

Existují dvě sady vlastností, kterými se řídí odsazení, právě vzájemně se vylučujících vlastnosti:

-   **Odsazení obsahu**: (volitelné) určuje zda by měly vstupní doplněné pomocí **výchozí odsazení schéma**. Hodnota může být jeden logická hodnota, nebo může být řazené kolekce členů logických hodnot s délkou, která je aritu do skupiny. Logická hodnota prodlužuje být řazené kolekce členů správnou délku s všechny komponenty rovno určité hodnoty. Pokud rozměry hodnotu PRAVDA, zdroji logicky doplněné v této dimenzi s nulovou hodnotou buněk pro podporu další jádra aplikací tak, aby centrální uzly první a poslední jádra v tomto rozměru první a poslední uzlů v dané dimenze ve vrstvě zdroje. Tak, počet "formální" uzlů v každé dimenzi, je určený automaticky přizpůsobí přesně _(InputShape [d] - 1) / mezery [d] + 1_ jádra do vrstvy čalouněný zdroje. Pokud rozměry hodnotu NEPRAVDA, jsou definované jádra tak, aby počtu uzlů po stranách zůstanou se stejná (až rozdílu druhých 1). Výchozí hodnota Tenhle atribut je řazené kolekce členů se všechny komponenty rovna hodnotě NEPRAVDA.
-   **UpperPad** a **LowerPad**: (volitelné) poskytovat získat větší kontrolu nad množství výplně používat. **Důležité:** Tyto atributy je to možné definovat a pouze v případě vlastnost **Odsazení** nad je ***Nedefinováno*** . Hodnoty by měl být vracející celé číslo n-ticové s délky, které jsou aritu do skupiny. Když jsou tyto atributy jazyku, "formální" uzly se přidají do horní a dolní konce každou dimenzi vstupní vrstvy. Počtu uzlů přidána do horní a dolní končí v každé dimenzi je určený podle **LowerPad**[i] a **UpperPad**[i]. Abyste měli jistotu, že jádra odpovídat jenom "skutečné" uzly a pozor, abyste "formální" uzly, musí být splněny následující podmínky:
    -   Jednotlivé součásti **LowerPad** musí být sporná, i když menší než KernelShape [d] / 2. 
    -   Jednotlivé součásti **UpperPad** nesmí být větší než KernelShape [d] / 2. 
    -   Výchozí tyto atributy hodnotu řazené kolekce členů se všechny komponenty roven 0. 

Nastavení **Odsazení** = true umožňuje tolik odsazení v případě potřeby je chcete zachovat "střed" jádra uvnitř "skutečné" při zadávání. Tím se změní matematické trochu pro výpočet velikost výstupu. Obecně vzato výstupní velikost _D_ počítá jako _D = (I - K) / S + 1_, kde _můžu_ velikost vstupní argument _K_ není velikost jádra, _S_ je rozteč, a _/_ je celé číslo dělení (zaokrouhlit směrem k nule). Pokud jste nastavili UpperPad = [1, 1], zadávání velikost, _můžu_ je efektivně 29 a tím _D = (29-5) / 2 + 1 = 13_. Ale, když **Odsazení** = PRAVDA, v podstatě, _můžu_ získá bumped _K - 1_; Proto _D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14_. Zadáním hodnoty pro **UpperPad** a **LowerPad** získáte mnohem víc publikum nemůže ovládat odsazení než nastavíte jenom **Výplň** = true.

Další informace o convolutional sítí a jejich využití najdete v těchto článcích:  

-   [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html )
-   [http://Research.microsoft.com/pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
-   [http://People.csail.MIT.edu/jvb/papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Fond sady
**Fond příruček** platí geometrie podobný convolutional připojení, ale používá předdefinovaných funkcí zdrojové hodnoty uzel odvodit cíl hodnotu. Proto sdružování svazky mít žádný stav trainable (gramáží nebo odchylek). Sdružování svazky podporují všechny convolutional atributy kromě **sdílení**, **MapCount**a **tloušťky**.  

Obvykle nepřekrývá jádra shrnuté podle sousedních sdružování jednotky. Pokud rozteč [d] je rovno KernelShape [d] v každé dimenzi, vrstvy, získáte je tradiční místní sdružování vrstvy, které se běžně používané v convolutional neural sítě. Každý cílový uzel vypočítá maximální počet nebo průměr aktivity jeho jádra ve vrstvě zdroje.  

Následující příklad ukazuje sdružování příruček: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

-   Aritu do skupiny je 3 (délka n-ticové **InputShape** **KernelShape**a **mezery**). 
-   Počet uzlů ve vrstvě zdroje je _5 *24* 24 = 2880_. 
-   Důvodem je tradiční místní sdružování vrstvy **KernelShape** a **mezery** jsou stejné. 
-   Počet uzlů v cílové vrstvy je _5 *12* 12 = 1440_.  
    
Další informace o sdružování vrstvy najdete v těchto článcích:  

-   [http://www.cs.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Oddíl 3.4)
-   [http://cs.Nyu.edu/~koray/publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
-   [http://cs.Nyu.edu/~koray/publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)
    
## <a name="response-normalization-bundles"></a>Balíčky normalizace odpověď
**Normalizace odpověď** je místní normalizace schéma, které byl nejdříve podle Geoffrey Hinton a další v knize [ImageNet Classiﬁcation s hloubkové Convolutional Neural sítě](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Normalizace odpověď slouží k podpoře Generalizace v neural sítí. Když jeden neuron aktivaci úrovni velmi vysoké aktivaci, vrstvy normalizace místní odpověď potlačí úroveň aktivace okolní neurons. Důvodem je pomocí tři parametry (***α***, ***β***a ***k***) a convolutional struktury (nebo obrazec Okolní počítače). Každý neuron ve vrstvě cíl ***y*** odpovídá neuron ***x*** ve vrstvě zdroje. Aktivace úroveň ***y*** je dán formulací, kde ***f*** je úroveň aktivace neuron a ***Nx*** je jádra (nebo sadu obsahující neurons v prostředí ***x***), způsobem definovaným ve struktuře convolutional následující:  

![][1]  

Odpověď normalizace svazky podporují všechny convolutional atributy kromě **sdílení**, **MapCount**a **tloušťky**.  
 
-   Obsahuje-li jádra neurons ve stejné mapy ve ***tvaru x***, schématu normalizace označovány jako **stejné mapy normalizace**. Pokud chcete definovat stejné mapy normalizace, musí mít první souřadnice v **InputShape** hodnotu 1.
-   Pokud jádra obsahuje neurons ve stejné prostorové pozici ve ***tvaru x***, ale neurons jsou v ostatních mapy, schématu normalizace nazývá **přes normalizace mapy**. Tento typ odpovědi normalizace používá formuláře boční zabránění vycházející typ součástí skutečné neurons vytváření soutěže pro velké aktivace úrovně mezi neuron výstupy vypočítanou na různých mapy. Pokud chcete definovat přes normalizace mapy, první souřadnice musí být celé číslo větší než jedna a ne větší než počet mapy a zbytek souřadnice musí mít hodnotu 1.  

Protože odpověď normalizace svazky použít předdefinované funkce zdrojové hodnoty uzel uzel hodnotu cíl určíte, mají žádný stav trainable (gramáží nebo odchylek).   

**Upozornění**: uzlů v cílové vrstvy odpovídají neurons, které jsou centrální uzly jádra. Řekněme, že KernelShape [d] je liché, pak _KernelShape [d] / 2_ odpovídá jádra centrální uzel. Pokud ještě není _KernelShape [d]_ , centrální uzel je na _KernelShape [d] / 2-1_. Proto pokud **Odsazení**[d] NEPRAVDA, první a poslední _KernelShape [d] / 2_ uzly nemají odpovídající uzlů v cílové vrstvě. Chcete-li této situaci předejít, definujte **Odsazení** jako [true, hodnotu PRAVDA,..., hodnotu true].  

Kromě čtyři atributy popsané výše odpověď normalizace svazky také podporují následující atributy:  

-   **Alfa**: (požadováno) určuje s plovoucí desetinnou čárkou, který odpovídá ***α*** v předchozím vzorcem. 
-   **Beta**: (požadováno) určuje s plovoucí desetinnou čárkou, který odpovídá ***β*** v předchozím vzorcem. 
-   **Posun**: (volitelné) určuje s plovoucí desetinnou čárkou, které odpovídá ***k*** předchozímu vzorci. Ve výchozím nastavení 1.  

Následující příklad definuje příruček normalizace odpovědí pomocí tyto atributy:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

-   Zdrojové vrstvy obsahuje pět mapování, oba objekty mají aof rozměru 12 x 12, celkové součty v 1440 uzlů. 
-   Hodnota **KernelShape** označuje, že se jedná o stejné mapy normalizace vrstvě, kde servery jsou obdélník 3 × 3. 
-   Výchozí **Odsazení** hodnotu NEPRAVDA, tedy cílové vrstvy obsahuje jenom 10 uzlů v každé dimenzi. Zahrnout jeden uzel cíl vrstvu, která odpovídá každý uzel ve vrstvě zdroj, přidejte odsazení = [true, hodnotu PRAVDA, pravdivé]; a změňte velikost RN1 na [5, 12; 12].  

## <a name="share-declaration"></a>Sdílení deklarace 
Čistého # volitelně podporuje definování více svazky s sdílených gramáží. Gramáží dvě sady možné je sdílet, pokud jejich struktury jsou stejné. Následující syntaxi definuje svazky s sdílených gramáží:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }
    
    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name
    
    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec
    
    bundle-spec:
       layer-name    =>     layer-name
    
    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec
    
    bias-spec:
        1    =>    layer-name
    
    layer-name:
        identifier  

Například následující deklarace sdílet určuje názvy vrstev označující, že by měl sdílený gramáží a chyb:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

-   Zadávání funkce jsou rozdělit na dvě stejné velikosti vstupní vrstvy. 
-   Skryté vrstvy potom výpočet vyšší úroveň funkcí na dvě vstupní vrstvy. 
-   Deklaraci sdílet Určuje _H1_ a _H2_ musí být vypočteny stejným způsobem z jejich odpovídajících vstupů.  
 
Můžete taky to může být zadán s dvě samostatná prohlášení sdílet následujícím způsobem:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

Zkratka můžete použít pouze v případě, že vrstvy obsahují jednu příruček. Obecně je možné sdílení pouze v případě odpovídající struktura je stejné, což znamená, že mají stejné velikosti, stejné convolutional geometrie a podobně.  

## <a name="examples-of-net-usage"></a>Příklady použití čistého #
Tato část obsahuje několik příkladů použití čistého # přidat skryté vrstvy, definujte způsobu, jakým skryté vrstvy ovládat ostatní vrstvy a vybudovat convolutional sítě.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Definování jednoduchá vlastní neural síť: "Ahoj světe" Příklad
Tento jednoduchý příklad ukazuje, jak vytvořit si model neural sítě, který obsahuje jeden skryté vrstvě.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

Příklad ukazuje některé základní příkazy následujícím způsobem:  

-   První řádek definuje vstupní vrstvy (s názvem _Data_). Pokud použijete klíčové slovo **auto** , neural síť automaticky obsahuje všechny sloupce funkce v zadávání příklady. 
-   Na druhém řádku vytvoří skryté vrstvě. Název _H_ přiřazeny k vrstvě skryté, tato role má 200 uzlů. Tato vrstva plně připojení k zadávání vrstvy.
-   Třetím řádku definuje výstup vrstvu (pojmenovali _O_), která obsahuje 10 výstup uzlů. Pokud neural sítě se používá pro klasifikaci, je jeden uzel výstup podle předmětu. Klíčové slovo **sigmoid** označuje, že funkce výstup se použije k vrstvě výstup.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Definování více skryté vrstvy: Příklad zrakem počítače
Následující příklad ukazuje, jak definovat trochu složitější neural sítě s více vlastní skryté vrstvy.  

    // Define the input layers 
    input Pixels [10, 20];
    input MetaData [7];
    
    // Define the first two hidden layers, using data only from the Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
    
    // Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }
    
    // Define the output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

Tento příklad ukazuje několik funkcí specifikace jazyka neural sítě:  

-   Struktura má dvě vstupní vrstvy, _pixelů_ a _MetaData_.
-   _Pixelů_ vrstvy je vrstva zdroje pro dvě sady připojení s cílovou vrstvy, _ByRow_ a _ByCol_.
-   Vrstvy _shromažďovat_ a _výsledek_ jsou cíl vrstvy do více balíčků připojení.
-   Výstup vrstva, _výsledek_je vrstva cíl do dvou připojení balíčků; jedna z nich druhé úrovně skryté (shromažďovat) jako cíle vrstvy a druhý vstupní vrstvy (MetaData) jako cílové vrstvě.
-   Skryté vrstvy _ByRow_ a _ByCol_zadat filtrované připojení pomocí predikátu výrazech. Přesněji na uzel _ByRow_ na [argumenty x, y] je připojen k uzlů v _pixelech_ , které mají rovno první souřadnice na uzel první index souřadnice x. Podobně na uzel _ByCol na [argumenty x, y] je připojen k uzlů v _pixelech_ druhý indexem koordinaci do jedné z druhé souřadnice na uzel y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Definování convolutional síť pro multiclass klasifikace: Příklad rozpoznávání číslice
Definice následující sítě je určen rozpoznatelný čísel a ukazuje některé pokročilé techniky pro přizpůsobení neural sítě.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


-   Struktura má vstupní vrstvy, _Obrázek_.
-   Klíčové slovo **convolve** označuje, že vrstvy s názvem _Conv1_ a _Conv2_ convolutional vrstvy. Všechny tyto vrstvy deklarace následuje seznam atributů konvoluce.
-   Sítě má třetí skryté vrstvy, _Hid3_, které je úplně připojené k druhé skryté vrstvy _Conv2_.
-   Výstup vrstvy _číslice_připojení jenom pro třetí skryté vrstvy _Hid3_. Klíčové slovo **všechny** označuje, že vrstvy výstup plně propojen _Hid3_.
-   Aritu konvoluce je tři (délka n-ticové **InputShape** **KernelShape**, **mezery**a **sdílení**). 
-   Počet gramáží za jádra je _1 + **KernelShape**\[0] * * *KernelShape**\[1]* **KernelShape**\[2] = 1 + 1 *5* 5 = 26. Nebo 26 * 50 = 1300_.
-   Můžete vypočítat uzlů v každé skryté vrstvě následujícím způsobem:
    -   **NodeCount**\[0] = (5 - 1) / 1 + 1 = 5.
    -   **NodeCount**\[1] = [13-5] / 2 + 1 = 5. 
    -   **NodeCount**\[2] = [13-5] / 2 + 1 = 5. 
-   Celkový počet uzlů můžete vypočítat pomocí deklarované dimenze vrstvy, [50, 5, 5], následujícím způsobem: _ **MapCount** * * *NodeCount**\[0]* **NodeCount**\[1] * * *NodeCount**\[2] = 10* 5 *5* 5_
-   Protože **sdílení**[d] je NEPRAVDA jenom pro _d == 0_, je číslo jádra _ **MapCount** * * *NodeCount**\[0] = 10* 5 = 50_. 


## <a name="acknowledgements"></a>Potvrzení

Jazyk čistého # pro přizpůsobení architektura neural sítí vyvinutý konsorciem Shon Katzenberger (tvůrce, výukové počítače) a Alexey Kamenev (Software zpětnou analýzu, Microsoft Research) společnosti Microsoft. Pracovní postup slouží interně počítače výukové projekty a od zjišťování obrázek do analýzy text aplikace. Další informace najdete v tématu [Neural sítí v Azure ML – Úvod do čistého #](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)


[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif
 
