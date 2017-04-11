<properties
    pageTitle="Serializovat dat pomocí Microsoft Avro Library | Microsoft Azure"
    description="Zjistěte, jak Azure HDInsight používá Avro serializovat velký data."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Serializovat data v Hadoop s Microsoft Avro Library

Toto téma ukazuje, jak používat <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> serializovat objekty a jiné datové struktury do proudů s cílem zachovat paměti, databáze nebo souboru a taky jak rekonstruovat je obnovit původní objekty.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
<a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Knihovny Microsoft Avro</a> používá systému Apache Avro dat serializace Microsoft.NET prostředí. Apache Avro poskytuje serializace formát výměny kompaktní binárními daty. Pomocí <a href="http://www.json.org" target="_blank">JSON</a> definovat schéma bez ohledu na jazyk, který uzavře interoperability jazyk. Data serializován v jednom jazyce můžete přečíst v jiném. Nyní jsou podporovány C, C++, C#, Java, PHP, Python a skutečné. <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Specifikace Avro Apache</a>najdete podrobné informace o formátu. Všimněte si, že aktuální verzi Microsoft Avro Library nepodporuje součástí tohoto specifikace vzdálené řízení volání (RPC).

Sériové vyjádření objektu v systému Avro tvoří dvě části: schématu a skutečnou hodnotou. Schéma Avro popisuje nezávislá na jazyku datový model sériové dat pomocí JSON. Je prezentovat vedle sebe s binární reprezentaci data. S schématu nezávislá na zápis v binární soustavě umožňuje jednotlivé objekty k zápisu s žádné režijních nákladů na hodnota, díky serializace rychlé a zastoupení small.

##<a name="the-hadoop-scenario"></a>Scénář Hadoop
Formát serializace Apache Avro je rozšířený v Azure HDInsight a jiných Apache Hadoop prostředí. Avro umožňuje snadno představující složité struktury dat v rámci Hadoop MapReduce úlohy. Formát souborů Avro (Avro objekt kontejneru soubor) byla vytvořena pro podporu distribuované MapReduce programovací model. Klíčové funkce, která umožňuje rozdělení je "rozdělit tabulku" znamená, že jeden požádat o jakékoli místo v souboru a spuštění čtení od určitého bloku soubory.

##<a name="serialization-in-avro-library"></a>Serializace v knihovně Avro
Knihovna .NET pro Avro podporuje serializaci objektů dvěma způsoby:

- **odraz** – JSON schématu pro typy je automaticky vytvořené z dat smlouvy atributy serializovat typy .NET.
- **Obecný záznam** – schématu A JSON explicitně podle záznamu představované třídy [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) při prezentování popisují schématu pro data, která chcete serializovat žádné typy .NET.

Po schéma dat je známo Redaktor a reader proudu, bez jeho schématu odesílat data. V případě použijete soubor Avro objektu kontejneru schéma je uloženo v souboru. Další parametry, jako je kodek použitý pro kompresi dat, můžete zadat. Podobnému sledu jsou popsány podrobněji, popsaných v následujících příkladech kódu.


## <a name="install-avro-library"></a>Instalace Avro knihovny

Toto jsou potřeba před instalací knihovny:

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft.NET Framework 4</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 nebo novější)

Všimněte si, že závislost Newtonsoft.Json.dll automaticky stáhnout s instalací Microsoft Avro Library. Postup pro to je uveden v následující části.


Microsoft Avro Library distribuuje jako NuGet balíčku, který je možné nainstalovat z aplikace Visual Studio pomocí následujícího postupu:

1. Zvolte kartu **projektu** -> **Spravovat... NuGet balíčků**
2. Vyhledejte "Microsoft.Hadoop.Avro" do pole **Hledání Online** .
3. Klikněte na tlačítko **nainstalovat** vedle **Microsoft Azure HDInsight Avro knihovny**.

Všimněte si, že Newtonsoft.Json.dll (> = 6.0.4) závislost je taky automaticky stáhnout, s Microsoft Avro Library.

Je vhodné navštivte <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">domovskou stránku knihovny Microsoft Avro</a> číst aktuální poznámky.


Zdrojový kód Microsoft Avro Library je k dispozici na <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">domovskou stránku knihovny Microsoft Avro</a>.

##<a name="compile-schemas-using-avro-library"></a>Kompilace schémata pomocí Avro knihovny

Microsoft Avro Library obsahuje nástroj pro generování kód, který umožňuje vytváření C# typů automaticky na základě dříve definované JSON schématu. Nástroj pro vytváření kódu není distributed jako binární spustitelný soubor, ale je možné snadno sestavit pomocí následujícího postupu:

1. ZIP moct soubor stáhněte nejnovější verzi HDInsight SDK zdrojového kódu z <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">rozhraní Microsoft .NET SDK Hadoop</a>. (Klikněte na ikonu **Stáhnout** , není karta **stáhne** .)

2. Extrahování SDK HDInsight do adresáře v počítači s .NET Framework 4 nainstalovaný a připojení k Internetu pro její stažení potřebné závislost NuGet balíčků. Tady vidíte jsme bude se předpokládá, že zdrojový kód extrakci k C:\SDK.

3. Přejděte do složky C:\SDK\src\Microsoft.Hadoop.Avro.Tools a spusťte build.bat. (Soubor zavolá MSBuild normálním rozdělením 32bitová verze .NET Framework. Pokud chcete používat 64bitovou verzi, upravte build.bat sledovat komentáře do souboru.) Zajistěte, aby byl sestavení úspěšně. (V některých sítě MSBuild může způsobit upozornění. Tato upozornění nemají vliv na nástroj, pokud jsou tu žádné chyby sestavení.)

4. Nástroj zkompilované je umístěn v C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.


Kde se seznámíte s syntaxi příkazového řádku, spusťte následující příkaz ze složky, kde je uložena nástroj pro vytváření kódu:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Chcete-li otestovat nástroj, si můžete vygenerovat třídy C# z ukázkový soubor schématu JSON součástí zdrojového kódu. Spusťte následující příkaz:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

To by měl vytvářet dvěma C# soubory v aktuálním adresáři: SensorData.cs a Location.cs.

Logika, která používá nástroj pro vytváření kódu při převodu schématu JSON typům C#, najdete v tématu soubor, který GenerationVerification.feature vyhledali v C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Dejte pozor, aby obory názvů jsou extrahovaných z schéma JSON pomocí pravidla popsaná v souboru podle předchozího odstavce. Obory názvů extrahovaných z schématu přednost něco jiného, co je součástí parametr /n v nástroj příkazového řádku. Pokud chcete přepsat obory názvů obsažené v rámci schématu, použijte parametr /nf. Například změnit všechny obory názvů z SampleJSONSchema.avsc my.own.nspace, spusťte následující příkaz:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>Vzorky
V tomto tématu šest příklady ilustrují různých scénářích nepodporuje Microsoft Avro Library. Microsoft Avro Library je navržený pro práci s všechny proudu. V těchto příkladech dat pracovat pomocí datových proudů paměti spíše než soubor streamovat nebo databáze jednoduché a konzistence. Přístup použitý v provozním prostředí závisí na přesné scénář požadavku, zdroje dat a objem, omezení výkonu a jiných faktorů.

První dva příklady ukazují, jak serializovat a deserializovat data do datového proudu vyrovnávací paměť pomocí odraz a obecný záznamy. Schéma v obou případech předpokládá se, že sdílet mezi čtení a zápis mimo pásma.

Třetí a čtvrtý příklady ukazují, jak serializovat a deserializovat dat pomocí souborů Avro objekt kontejner. Když data se ukládají v souboru kontejneru Avro, jeho schématu vždy uložený s ním protože schématu musí být sdíleny pro deserializace.

Ukázka obsahující první čtyři příklady můžete stáhnout z webu <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">Azure ukázky</a> .

Pátý příklad ukazuje jak používat vlastní komprese kodek Avro objekt kontejneru soubory. Ukázka obsahující kód v tomto příkladu můžete stáhnout z webu <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">Azure ukázky</a> .

Šestým příklad ukazuje, jak pomocí Avro serializace odeslat data k úložišti objektů Blob Azure a potom ho můžete analyzovat pomocí podregistru s clusteru HDInsight (Hadoop). Můžete stáhnout z webu <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure ukázky</a> .

Tady jsou odkazy na šest vzorků popisované v tématu:

 * <a href="#Scenario1">**Serializace s odraz**</a> – JSON schématu pro typy serializovat je automaticky vytvořené z dat atributy smlouvy.
 * <a href="#Scenario2">**Serializace obecný záznamu**</a> – JSON schématu explicitně v záznamu po zadáno pro odraz neexistuje žádný typ .NET.
 * <a href="#Scenario3">**Serializaci pomocí objektu kontejneru soubory s odraz**</a> – JSON schématu je automaticky vytvořené a sdílené společně s sériové dat přes soubor Avro objektu kontejner.
 * <a href="#Scenario4">**Serializaci pomocí objektu kontejneru soubory s obecný záznam**</a> – JSON schématu je explicitně zadanou před serializace a sdílené společně s daty pomocí soubor Avro objektu kontejner.
 * <a href="#Scenario5">**Serializaci pomocí objektu kontejneru soubory s je kodek vlastní komprese**</a> - příklad ukazuje, jak vytvořit soubor Avro objektu kontejneru s vlastní .NET provádění kodek komprese Deflate data.
 * <a href="#Scenario6">**Použití Avro odeslání dat pro službu Microsoft Azure HDInsight**</a> - příklad ukazuje, interakci Avro serializace ke službě HDInsight. Ke spuštění v tomto příkladu jsou potřeba aktivní Azure předplatné a přístup k clusteru služby Azure HDInsight.

###<a name="Scenario1"></a>Příklad 1: Serializace s odrazů

Schématu JSON pro typy můžete být automaticky vytvořené pomocí Microsoft Library Avro prostřednictvím odraz z dat smlouvy atributy C# objekty, které chcete serializovat. Vytvoří Microsoft Avro Library [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) k identifikaci pole, která mají být serializován.

V tomto příkladu objektů ( **SensorData** třídy s struktura **umístění** člena) jsou serializovány proudu paměti a naopak rekonstruována tomto proudu. Výsledek pak ve srovnání s počáteční instance potvrďte, že objekt **SensorData** obnoveného je shodný s původním.

Schéma v tomto příkladu se dosadí sdíleny čtení a zápis, tak, aby formátu Avro objekt kontejneru není potřeba. Příklad serializovat a deserializovat dat do vyrovnávací paměť pomocí odraz formátu kontejneru objekt při schématu musí být sdíleny s daty najdete v článku <a href="#Scenario3">serializaci pomocí objektu kontejneru soubory s odrazu</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Příklad 2: Serializace k obecný záznamu

Schéma JSON explicitně lze v obecný záznamu při odraz nelze použít, protože data nelze prostřednictvím třídy .NET se smlouvou data. Tento způsob je obecně nižší než používat odrazu. V takovém případě schématu pro data možná by vás dynamické, tedy dál nebyl známé době překladu. Data tvaru hodnoty oddělené čárkami (CSV) soubory, jejichž schématu Neznámý transformovat do formátu Avro za běhu, dokud je znázorněn příklad toto řazení dynamické scénáře.

Tento příklad ukazuje, jak vytvořit a používat [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) a explicitně zadejte schéma, JSON, jak k naplnění s daty a jak serializovat a deserializovat. Výsledek pak ve srovnání s počáteční instance potvrďte, že je stejné do původní podoby záznam obnovit.

Schéma v tomto příkladu se dosadí sdíleny čtení a zápis, tak, aby formátu Avro objekt kontejneru není potřeba. Příklad toho, jak serializovat a deserializovat dat do vyrovnávací paměť pomocí obecný záznamu formátu kontejneru objekt při schématu musí být součástí sériové data podívejte se na příklad <a href="#Scenario4">serializaci pomocí objektu kontejneru soubory s obecný záznamu</a> .


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Ukázka 3: Serializace objekt kontejneru soubory a serializace pomocí odrazů

Tento příklad je podobný scénář v <a href="#Scenario1">prvním příkladu</a>, kde schématu implicitně zadaným odrazu. Rozdíl je tady, schématu není dosadí známé v druhé osobě, která deserializuje ho. **SensorData** objekty, které chcete serializovat a jejich implicitně určenému schématu jsou uloženy v souboru kontejneru objekt Avro představované [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) předmětu.

Data serializován v tomto příkladu s [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) a rekonstruované s [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Výsledek potom je ve srovnání s počáteční instance ověřit identitu.

Data v souboru kontejneru objekt komprimovaná prostřednictvím výchozí [**Deflate**] [ deflate-100] komprese kodek z .NET Framework 4. Podívejte se na <a href="#Scenario5">pátý příklad</a> v tomto tématu se dozvíte, jak používat nejnovější a nadřazené verzi [**Deflate**] [ deflate-110] komprese kodek k dispozici v .NET Framework 4.5.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Ukázka 4: Serializaci pomocí objektu kontejneru soubory a serializace obecný záznamu

Tento příklad je podobný scénář v <a href="#Scenario2">druhém příkladu</a>, kde schématu explicitně zadaným JSON. Rozdíl je tady, schématu není dosadí známé v druhé osobě, která deserializuje ho.

Test uvedenou množinu dat je do seznamu objektů [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) shromážděných schématem určenými JSON a uložena v soubor kontejneru objektu představované [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) předmětu. Tento kontejner soubor vytvoří Redaktor použitém k jeho serializovat data nekomprimované proudu paměti, pak uložený soubor. [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) použitým při vytváření čtenáře Určuje, že tato data nebudou komprimovány.

Data je pak číst ze souboru a převést ze sériového tvaru do kolekce objektů. Tato kolekce porovnáván počáteční seznam záznamů Avro potvrďte, že jsou tyto hodnoty shodné.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Vzorku 5: Serializaci pomocí objektu kontejneru soubory s je kodek vlastní komprese

Pátý příklad ukazuje jak používat vlastní komprese kodek Avro objekt kontejneru soubory. Ukázka obsahující kód v tomto příkladu můžete stáhnout z webu [Azure ukázky](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) .

[Specifikace Avro](http://avro.apache.org/docs/current/spec.html#Required+Codecs) umožňuje použití nepovinný stupeň komprese kodeku (kromě **Null** a **Deflate** výchozí nastavení). V tomto příkladu není implementace úplně novou kodek například Snappy (jak je uvedeno jako podporovaný volitelné kodek [Avro specifikace](http://avro.apache.org/docs/current/spec.html#snappy)). Ukazuje, jak používat .NET Framework 4.5 provádění [**Deflate**] [ deflate-110] správný kodek, který zajišťuje lepší algoritmus komprese založené na knihovně [zlib](http://zlib.net/) komprese než výchozí verze .NET Framework 4.


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Ukázka 6: Pomocí Avro odeslání dat pro službu Microsoft Azure HDInsight

Šestým příklad ukazuje některé programovací postupy týkající se práce se službou Azure HDInsight. Ukázka obsahující kód v tomto příkladu můžete stáhnout z webu [Azure ukázky](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) .

Vzorku dělá toto:

* Připojí ke stávajícímu clusteru služby HDInsight.
* Jsou více souborů CSV a odešle výsledek k úložišti objektů Blob Azure. (Soubor CSV rozloženy společně s vzorku a představují výpis z historických dat příloze burzovního distributed [Infochimps](http://www.infochimps.com/) dobu 1970 2010. Vzorku načte data soubor CSV, převede záznamy na instancí třídy **papír** a jsou jejich pomocí odrazu. Definice burzovní typu se vytvoří ze JSON schématu prostřednictvím nástroj Microsoft Avro Library kód generování.
* Vytvoří nový externí tabulku s názvem **zásob** podregistru a odkazy ho chcete data uložit v předchozím kroku.
* Spustí dotaz prostřednictvím **zásob** tabulku podregistru.

Kromě toho vzorku provede postup vyčištění před a po dokončení operace hlavní. Během vyčištění odeberou se všechny související data objektů Blob Azure a složky a tabulku podregistru nezobrazí. Můžete taky vyvolat postup vyčištění z příkazového řádku vzorku.

Vzorku má následující požadavky:

* Aktivní předplatné Microsoft Azure a jeho ID předplatného.
* Správa certifikátů pro předplatné s odpovídajícím privátním klíčem. Měli byste mít nainstalované certifikátu do aktuálního uživatele soukromé úložiště v počítači použít ke spuštění vzorku.
* Aktivní clusteru HDInsight.
* Účet Azure úložiště clusteru HDInsight propojen z předchozí předpoklad společně s odpovídajícím primární a sekundární přístupové klávesy.

Všechny informace z požadavky se musí zadat na Ukázkový konfigurační soubor před spuštěním vzorku. To udělat dvěma možnými způsoby:

* Upravte soubor app.config v kořenovém adresáři vzorku a potom vytvořit výběru
* Nejdřív Vytvoření ukázky a potom upravit AvroHDISample.exe.config v adresáři Tvůrce dotazů

V obou případech je třeba provádět všechny úpravy **<appSettings>** oddíl nastavení. Postupujte podle komentáře v souboru.
Spuštění výběru z příkazového řádku spuštěním následujícího příkazu (kde souboru ZIP s ukázkou se předpokládá, že extrahovat C:\AvroHDISample; Pokud jinak, použijte cesta k souboru relevantní):

    AvroHDISample run C:\AvroHDISample\Data

Vyčištění clusteru, spusťte tento příkaz:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
