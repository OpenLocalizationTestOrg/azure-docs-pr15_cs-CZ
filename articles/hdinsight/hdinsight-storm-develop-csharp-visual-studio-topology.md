<properties
   pageTitle="Topologie Apache bouře s Visual Studia a C# | Microsoft Azure"
   description="Naučte se vytvářet bouře topologií v jazyce C# tak, že vytvoříte topologii počet jednoduché word ve Visual Studiu pomocí nástroje HDInsight for Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>Můžete vyvíjet C# topologie pro Apache bouře na HDInsight pomocí nástroje Hadoop for Visual Studio

Naučte se vytvářet C# bouře topologie pomocí nástroje HDInsight for Visual Studio. Tento kurz provede proces vytvoření nového projektu bouře ve Visual Studiu, testování místně a nasazení Apache bouře clusteru HDInsight.

Můžete taky dozvíte, jak vytvořit hybridní topologií používající C# a součástí jazyka Java.

> [AZURE.IMPORTANT] Během kroků uvedených v tomto dokumentu spolehnout vývojové prostředí Windows s Visual Studiu, zkompilované projektu můžete zaslat Linux nebo serveru s Windows HDInsight clusteru. Na základě Linux clusterů vytvořit pouze po 10/28/2016 podpory SCP.NET topologií.
>
> Topologie C# pomocí Linux clusteru, musíte aktualizovat balíček Microsoft.SCP.Net.SDK NuGet používá společnost projektu verzi 0.10.0.6 nebo vyšší. Verze balíčku odpovídat také hlavní verze bouře nainstalovaným HDInsight. Bouře na HDInsight verze 3.3 a 3.4 ji například používat verzi bouře 0.10.x, zatímco HDInsight 3.5 používá bouře 1.0.x.
> 
> Topologie C# na základě Linux clusterů musí používat .NET 4.5 a používejte Mono spustit clusteru HDInsight. Většinu věcí, které budou fungovat, ale byste měli zkontrolovat [Kompatibilitu Mono](http://www.mono-project.com/docs/about-mono/compatibility/) dokument potenciální kompatibilitou.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Jedna z těchto verzí aplikace Visual Studio

    - Visual Studio 2012 s [Aktualizovat 4](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 s [aktualizace 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 komunity](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio 2015 nebo [Visual Studio 2015 komunity](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure SDK 2.9.5 nebo novější

- HDInsight Tools for Visual Studio: najdete v článku [Začínáme s používáním HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) můžete nainstalovat a nakonfigurovat nástroje HDInsight for Visual Studio.

    > [AZURE.NOTE] HDInsight Tools for Visual Studio nejsou podporovány v aplikaci Visual Studio Express

-   Apache bouře HDInsight clusteru: najdete v článku [Začínáme s Apache bouře na HDInsight](hdinsight-apache-storm-tutorial-get-started.md) postup vytvoření clusteru.

## <a name="templates"></a>Šablony

Nástroje HDInsight for Visual Studio obsahují následující šablony:

| Typ projektu | Ukazuje |
| ------------ | ------------- |
| Bouře aplikace | Prázdný projekt topologie bouře |
| Ukázka Redaktor bouře Azure SQL | Jak psát k databázi SQL Azure |
| Ukázka bouře DocumentDB Readeru | Jak číst z Azure DocumentDB |
| Ukázka DocumentDB Redaktor bouře | Jak psát Azure DocumentDB |
| Ukázka bouře EventHub Readeru | Jak číst z Azure rozbočovače události |
| Ukázka EventHub Redaktor bouře | Jak psát rozbočovače události Azure |
| Ukázka bouře HBase Readeru | Jak číst z HBase na HDInsight clusterů |
| Ukázka HBase Redaktor bouře | Jak psát HBase na HDInsight clusterů |
| Ukázka hybridní bouře | Jak používat součást Java |
| Ukázka bouře | Topologie počet základní Wordu |

> [AZURE.NOTE] Ukázky čtečka a Autor HBase pomocí rozhraní REST API HBase komunikovat s HBase HDInsight clusteru, ne rozhraní API Java HBase.

Kroky v tomto dokumentu použijete základní typ bouře aplikace project k vytvoření nové topologie.

## <a name="create-a-c-topology"></a>Vytvoření topologie C#

1. Pokud jste nenainstalovali nejnovější verzi nástroje HDInsight for Visual Studio, přečtěte si článek [Začínáme s používáním HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Otevřete aplikaci Visual Studio, vyberte **soubor** > **Nový**a potom **projekt**.

3. Na obrazovce **Nový projekt** rozbalte **Instalované** > **šablony**a vyberte **HDInsight**. V seznamu šablony vyberte **Bouře aplikace**. V dolní části obrazovky zadejte **WordCount** jako název aplikace.

    ![Obrázek](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. Po vytvoření projektu byste si měli následující soubory:

    - **Program.cs**: Tato možnost definuje topologii plánu projektu. Všimněte si, že je ve výchozím nastavení vytvoří výchozí topologii skládající se z jednoho hubičky a jeden šroub.

    - **Spout.cs**: hubičky příklad, který posílá náhodná čísla.

    - **Bolt.cs**: blesku příklad, který uchovává počet čísel, které hubičky.

    Jako součást vytvářeného projektu bude nejnovější [SCP.NET balíčků](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) stahovat z NuGet.

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

V dalších částech změníte tohoto projektu do aplikace základní WordCount.

### <a name="implement-the-spout"></a>Implementace hubičky

1. Otevřete **Spout.cs**. Spouts slouží k načtení dat v topologii z externího zdroje. Hlavní součásti pro hubičkou jsou:

    - **NextTuple**: volat bouře při hubičky bude moct posílat nových záznamů.

    - **Potvrzení** (pouze transakční topologie): zpracovává potvrzení zahájil: po jiných komponent topologie pro n-ticové odeslané z tohoto hubičky. Potvrdil řazené kolekce členů umožňuje hubičky vědět, že byl úspěšně zpracován podřízené součástmi.

    - **Selhání** (pouze transakční topologie): zpracovává záznamů, které jsou selhání zpracování jiných komponent topologie. Tato funkce poskytuje možnost znovu posílat n-tici tak, že můžete znovu zpracovány.

2. Nahraďte obsah třídy **Spout** takto. Tím vytvoříte hubičkou náhodně posílá věty do topologii.

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    Přečetli komentáře chcete porozumět tomu, jaký dopad tento kód chvíli trvat.

### <a name="implement-the-bolts"></a>Implementace prvky

1. Odstranění existující soubor **Bolt.cs** z projektu.

2. V **Okně Průzkumník**projektu klikněte pravým tlačítkem myši a vyberte **Přidat** > **Nová položka**. V seznamu vyberte **Bouře blesku**a zadejte **Splitter.cs** jako název. Opakujte tento postup vytvoření druhého pevným pojmenované **Counter.cs**.

    - **Splitter.cs**: implementuje role, které rozdělí vět na jednotlivá slova a posílá nové toku slova.

    - **Counter.cs**: implementuje role, který počítá každého slova a posílá nové proudu slov a počtu u každého slova.

    > [AZURE.NOTE] Tyto prvky jednoduše čtení a zápis do datových proudů, ale můžete také role komunikovat s zdrojům, jako jsou databáze nebo služby.

3. Otevřete **Splitter.cs**. Poznámka: obsahuje pouze jednu metodu ve výchozím nastavení: **Spustit**. Je místo toho šroubu obdrží řazené kolekce členů pro zpracování. Tady si můžete přečíst zpracovat příchozí n-ticové a generuje výstupních záznamů.

4. Obsah tříd **rozdělovač** nahraďte následující kód:

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    Přečetli komentáře chcete porozumět tomu, jaký dopad tento kód chvíli trvat.

5. Otevřete **Counter.cs** a nahraďte obsah tříd takto:

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    Přečetli komentáře chcete porozumět tomu, jaký dopad tento kód chvíli trvat.

### <a name="define-the-topology"></a>Definování topologii

Spouts a prvky jsou uspořádány do grafu, který definuje, jak data přetékat mezi součástmi. Pro tento topologii graf vypadá takto:

![Obrázek uspořádání součásti](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Věty jsou vyzařováno hubičky, které jsou úměrně výskyty blesku rozdělovač. Rozdělovač blesku rozdělí věty na slova, které jsou úměrně blesku čítač.

Protože slov v instanci čítače směřuje místně, chceme, abyste měli jistotu, že určitá slova flow stejné instanci blesku čítače, takže máme jenom jedna instance udržování přehledu o určité slovo. Ale pro blesku rozdělovač skutečně nezáleží na které blesku obdrží které věty, můžeme jednoduše chcete načíst zůstatek vět všech těchto instancí.

Otevřete **Program.cs**. Metoda důležité je **GetTopologyBuilder**, které se používá k definování topologie, která se odešle bouře. Obsah **GetTopologyBuilder** nahraďte následující kód implementovat topologii popsáno dříve:

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

Přečetli komentáře chcete porozumět tomu, jaký dopad tento kód chvíli trvat.

## <a name="submit-the-topology"></a>Odeslání topologii

1. V **Okně Průzkumník**projektu klikněte pravým tlačítkem myši a vyberte **Odeslat bouře na HDInsight**.

    > [AZURE.NOTE] Pokud se zobrazí výzva, zadejte přihlašovací údaje pro předplatné Azure. Pokud máte víc předplatných, přihlaste se k ten, který obsahuje vaše bouře clusteru HDInsight.

2. Vyberte bouře HDInsight clusteru z rozevíracího seznamu **Bouře obrázku** a pak vyberte **Odeslat**. Můžete sledovat při odeslání úspěšné pomocí v okně **výstupu** .

3. Po úspěšném odeslání topologii by se měly **Bouře topologií** clusteru. Vyberte ze seznamu zobrazíte informace o topologii pracovního topologii **WordCount** .

    > [AZURE.NOTE] Můžete taky zobrazit **Bouře topologií** z **Průzkumníka serveru**: rozbalte **Azure** > **HDInsight**bouře HDInsight clusteru, pravým tlačítkem vyberte **Zobrazení bouře topologií**.

    Odkazy pro spouts nebo prvky slouží k zobrazení informací o tyto součásti. Otevře se nové okno pro každé vybrané položky.

4. **Topologie souhrnné** zobrazení klikněte na **Ukončit** ukončíte topologii.

    > [AZURE.NOTE] Topologie bouře dál spustit, dokud se nebo byl deaktivovaný nebo odstraněný clusteru.

## <a name="transactional-topology"></a>Transakční topologie

Předchozí topologie je transakční. Součásti uvnitř topologii implementovat ne všechny funkce pro přehrání zprávy, když zpracování nepovede součástí v topologii. Příklad transakční topologie vytvoření nového projektu a vyberte **Ukázkový bouře** jako typ projektu.

Transakční topologií implementace podle následujících pokynů podporují přehrání dat:

- **Ukládání do mezipaměti metadat**: hubičky musí ukládat metadata o data vyzařovaného tak, aby data můžete načíst a znovu vyzařovaného dojde k chybě. Protože dat, které vzorku je malý, původní data pro každou n-tici uložený ve slovník pro přehrání.

- **ACK**: každý blesku topologie upoutat `this.ctx.Ack(tuple)` k potvrzení, že byla úspěšně zpracovány řazené kolekce členů. Všechny prvky obsahujících acked n-tici, `Ack` metoda hubičky. Díky hubičky odebrat data uložená v mezipaměti pro přehrávání, protože úplně zpracovat data.

- **Selhání**: každý blesku upoutat `this.ctx.Fail(tuple)` označíte, že zpracování selže pro řazené kolekce členů. Selhání rozšíří do `Fail` metoda hubičky, kde můžete pomocí přehrány n-tici cached metadata.

- **Pořadové číslo**: když výstupu řazené kolekce členů, může být zadán ID posloupnosti. To by měl být hodnota, která určuje n-tici pro zpracování znova se přehrávat (Ack a selhání). Například hubičky v **Ukázkové bouře** projektu pomocí následujícího při vysílání dat:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    To posílá nové řazené kolekce členů obsahující věty do datového proudu výchozí hodnotu ID posloupnost obsažené v **lastSeqId**. V tomto příkladu je **lastSeqId** jednoduše zvýšen pro každých n-tici vyzařovaného.

Jak je ukázáno v projektu **Bouře vzorku** , zda je transakční komponentu lze nastavit za běhu, podle konfigurace.

## <a name="hybrid-topology"></a>Topologie hybridní

HDInsight nástroje for Visual Studio lze také vytvořit hybridní topologií, kde jsou některé součásti C# a ostatní Java.

Pro hybridní příklad topologie vytvoření nového projektu a vyberte **Bouře hybridního výběru**. Tím vytvoříte plně skrytými v komentářích vzorku, který obsahuje několik topologií, které ukazují následující:

- **Java spout** a **C# šroub**: podle **HybridTopology_javaSpout_csharpBolt**

    - Transakční verze je definována v **HybridTopologyTx_javaSpout_csharpBolt**

- **C# spout** a **Java šroub**: podle **HybridTopology_csharpSpout_javaBolt**

    - Transakční verze je definována v **HybridTopologyTx_csharpSpout_javaBolt**

        > [AZURE.NOTE] Tato verze taky demonstruje použití kódu Clojure z textového souboru jako součást Java.

Pokud chcete přepnout mezi topologie, která se používá při odeslání projektu, jednoduše přesunout `[Active(true)]` údajů topologie, který chcete použít před jeho odesláním ke clusteru.

> [AZURE.NOTE] Všechny soubory Java, které jsou potřeba jsou k dispozici v rámci tohoto projektu ve složce **JavaDependency** .

Je potřeba zvážit následující při vytvoření a odeslání hybridní topologie:

- **JavaComponentConstructor** musí být použity k vytvoření nové instance třídy Java hubičky nebo blesku.

- **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** bude použito serializovat dat v a odhlášení z součástí jazyka Java z Java objektů do formátu JSON.

- Při odesílání topologii na serveru, musíte pomocí **dalších konfigurace** možnost můžete určit **cesty k souboru Java**. Zadaná cesta by měl být adresář, který obsahuje SKLENICE soubory, které obsahují tříd jazyka Java.

### <a name="azure-event-hubs"></a>Rozbočovače Azure události

Verze SCP.Net 0.9.4.203 uvádí nové třídy a metody speciálně pro práci s událost centrální Spout (Java hubičkou, který bude číst z centrální událost.) Při vytváření topologie, která používá tento hubičky, postupujte takto:

- Třídy **EventHubSpoutConfig** : vytvoří objekt, který obsahuje konfiguraci komponentu hubičky

- Metoda **TopologyBuilder.SetEventHubSpout** : přidá komponentu události centrální Spout topologii

> [AZURE.NOTE] Když tyto usnadňují práci s Spout události centrální než jiné součásti Java, musíte dál používat CustomizedInteropJSONSerializer serializovat dat vytvořené pomocí hubičky.

## <a id="configurationmanager"></a>Použití ConfigurationManager

Nepoužívat ConfigurationManager k načtení hodnot konfigurace z blesku a spout součástí; To vede k výjimce ukazatele null. Místo toho postup nastavení projektu je předaným topologii bouře jako pár klíč/hodnota v kontextu topologie. Jednotlivé součásti, která závisí na hodnoty konfigurace musí načíst je v kontextu během inicializace.

Následující kód ukazuje, jak získat tyto hodnoty:

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

Pokud používáte `Get` metodu pro návrat instance komponenty, musíte se ujistit obě předá `Context` a `Dictionary<string, Object>` parametry konstruktoru. V následujícím příkladu je základní `Get` metodu, která správně předá tyto hodnoty:

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>Jak aktualizovat SCP.NET

Poslední verzích SCP.NET domovské stránce podpory upgradu balíčku prostřednictvím NuGet. Pokud je k dispozici nová aktualizace, zobrazí se oznámení o upgradu. Pro upgrade zkontrolovat ručně, postupujte takto:

1. V **Okně Průzkumník**projektu klikněte pravým tlačítkem myši a vyberte **Spravovat balíčků NuGet**.

2. Z balíčku správce vyberte možnost **aktualizace**. Pokud je k dispozici aktualizace, budou uvedené. Klikněte na tlačítko **Aktualizovat** balíčku případně ji nainstalujte.

> [AZURE.IMPORTANT] Pokud váš projekt byl vytvořený pomocí jedna z předchozích verzí SCP.NET, který nepoužili NuGet balíček aktualizace, musíte proveďte následující kroky k aktualizaci na novou verzi:
>
> 1. V **Okně Průzkumník**projektu klikněte pravým tlačítkem myši a vyberte **Spravovat balíčků NuGet**.
> 2. Pomocí pole **Hledat** , Hledat a potom přidejte **Microsoft.SCP.Net.SDK** projektu.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="null-pointer-exceptions"></a>Výjimky ukazatele Null.

Při použití topologie C# na základě Linux HDInsight clusteru, šroubu a spout součástí, které můžete přečíst v tématu Nastavení konfigurace za běhu může vrátit hodnotu null ukazovátko výjimky ConfigurationManager. To je způsobeno konfigurace načtené domény není z sestavení, která obsahuje váš projekt.

Postup nastavení projektu předaným topologii bouře jako pár klíč/hodnota v kontextu topologie a můžete načtená z kontingenčního seznamu objekt slovník, které je váš komponent při.

Následující příklad ukazuje načítání hodnoty konfigurace z topologie kontext, naleznete v části [ConfigurationManager](#configurationmanager) tohoto dokumentu.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Při topologie C# pomocí obrázku na základě Linux HDInsight, se může zobrazit chybová zpráva:

    System.TypeLoadException: Failure has occurred while loading a type.

To obvykle zemře při použití binární, která není kompatibilní s verzi .NET, který podporuje mono.

Na základě Linux HDInsight clusterů nezapomeňte, že váš projekt používá pro .NET 4.5 kompilovaný binární soubory.


### <a name="test-a-topology-locally"></a>Testování topologii místně

I když není těžké si nasazení topologii cluster v některých případech, budete muset testování topologii místně. Pomocí následujících kroků spustit a otestovat topologii příklad v tomto kurzu místně ve vývojovém prostředí.

> [AZURE.WARNING] Místní testování jde použít pouze pro základní, C# pouze topologií. Nepoužívejte místní testování hybridní topologií nebo topologií, které používají více datových proudů jako chybové zprávy.

1. V **Okně Průzkumník**projektu klikněte pravým tlačítkem myši a vyberte **Vlastnosti**. V dialogovém okně Vlastnosti projektu změňte **Typ výstupu** do **Konzoly aplikace**.

    ![Typ výstupu](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] Upozorňujeme, že chcete změnit **Typ výstupu** zpět **Knihovna tříd** před nasazením topologii clusteru.

2. V **Okně Průzkumník**projektu klikněte pravým tlačítkem myši a potom klikněte na **Přidat** > **Nová položka**. Vyberte **předmětu** a zadejte **LocalTest.cs** název třídy. Nakonec klikněte na **Přidat**.

3. Otevřete **LocalTest.cs** a přidejte následující příkaz **pomocí** nahoře:

        using Microsoft.SCP;

4. Obsah tříd **LocalTest** použijte následující:

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    Přečetli komentáře kódu chvíli trvat. Tento kód používá **LocalContext** spuštění součásti vývojové prostředí a trvá toku dat mezi komponenty pro soubory ve formátu RTF na místní disk.

5. Otevřete **Program.cs** a na metodu **hlavní** přidejte následující text:

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. Uložte změny, a pak klikněte na **F5** nebo vyberte **ladění** > **Spustit ladění** zahájíte projekt. Okno konzoly by měl zobrazit a zaznamenávat stav průběh testů. Po **že dokončení testů** zobrazení stisknutím libovolné klávesy zavřete okno.

7. Pomocí **Průzkumníka Windows** přejděte v adresáři, která obsahuje váš projekt, například **C:\Users\<vaše_uživatelské_jméno > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. V této složce otevřete **Koš**a potom klikněte na **ladění**. Byste měli vidět textové soubory, které byly vytvořeny při spuštění testů: sentences.txt counter.txt a splitter.txt. Otevřete každý textového souboru a Kontrola data.

    > [AZURE.NOTE] Řetězec data uložena jako maticový desetinná čísla v těchto souborů. Například \[[97,103,111]] v **splitter.txt** je soubor na slovo "a".

Ačkoli testování základní slova počítat místně aplikace je poměrně důvodu že skutečné hodnoty dosahuje při máte složité topologie, která informuje uživatele o s externím zdrojům dat nebo provede analýza komplexních dat. Při práci s projektem, budete muset nastavit zarážky a pokaždé procházet všemi kroky kód v komponentách izolovat problémy.

> [AZURE.NOTE] Ujistěte se, a nastavte **Typ projektu** zpět na **Knihovna tříd** před nasazením bouře clusteru HDInsight.

### <a name="log-information"></a>Informace v protokolu

Můžete snadno protokolovat informace z komponent topologie pomocí `Context.Logger`. Například následující vytvoří informační záznam:

    Context.Logger.Info("Component started");

Protokolované informace může zobrazit z **Protokolu Hadoop služby**, který je umístěn v **Průzkumníku serveru**. Rozbalte položku pro bouře HDInsight clusteru a potom rozbalte položku **Hadoop služby protokolu**. Nakonec vyberte soubor protokolu zobrazíte.

> [AZURE.NOTE] Protokoly jsou uložené v úložišti Azure účet, který používá svůj cluster. Pokud je to jiné předplatné než tu, kterou jste se přihlásili k aplikaci Visual Studio, budete muset přihlásit k odběru obsahující úložiště účet, který chcete tyto informace zobrazit.

### <a name="view-error-information"></a>Zobrazení informací o chybě

Chyby, ke kterým došlo v pracovního topologii zobrazíte pomocí následujících kroků:

1. Z **Průzkumníka serveru**klikněte pravým tlačítkem myši bouře HDInsight clusteru a vyberte **zobrazení bouře topologií**.

2. Pro **Spout** a **Bolts**sloupci **Poslední chyba** obsah bude na poslední chyby, ke kterým došlo.

3. Vyberte **Spout Id** **Blesku Id** pro součást s chybou uvedené. Na stránce Podrobnosti, která se zobrazí další informace o chybě zobrazí v části **chyb** v dolní části stránky.

4. Získat další informace, vyberte **Port** **vykonavatelů** část stránky zobrazíte protokol bouře pracovní dobu poslední několik minut.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak vyvíjet a nasazení bouře topologií nástrojích HDInsight for Visual Studio se dozvíte, jak [Proces události z Azure události centrální s bouře na HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).

Příklad C# topologie rozdělí více datových proudů toku dat najdete v článku [C# bouře příklad](https://github.com/Blackmist/csharp-storm-example).

Pokud chcete zjistit další informace o vytváření C# topologií, navštěvujte blog o [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Další způsoby, jak pracovat s HDInsight a další bouře na HDinsight vzorcích najdete v těchto článcích:

**Microsoft SCP.NET**

* [Průvodce programováním spojovací bod služby](hdinsight-storm-scp-programming-guide.md)

**Apache bouře na HDInsight**

- [Nasazení a sledovat topologií s Apache bouře na HDInsight](hdinsight-storm-deploy-monitor-topology.md)

- [Příklad topologie pro bouře na HDInsight](hdinsight-storm-example-topology.md)

**Apache Hadoop na HDInsight**

- [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

- [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

- [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

**Apache HBase na HDInsight**

- [Začínáme s HBase na HDInsight](hdinsight-hbase-tutorial-get-started.md)
