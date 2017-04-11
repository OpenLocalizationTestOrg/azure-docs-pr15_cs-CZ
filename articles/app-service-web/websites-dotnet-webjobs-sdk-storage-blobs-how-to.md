<properties 
    pageTitle="Jak pomocí WebJobs SDK úložišti objektů blob Azure" 
    description="Zjistěte, jak úložišti objektů blob Azure pomocí služby WebJobs SDK. Spustit proces začalo nových objektů blob v kontejneru a obsloužení "poškozená objektů BLOB"." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Jak pomocí WebJobs SDK úložišti objektů blob Azure

## <a name="overview"></a>Základní informace

Tato příručka obsahuje C# ukázek kódu, které ukazují, jak aktivovat proces vytvořila nebo aktualizovala objektů blob Azure. Ukázky kódu používat verzi [WebJobs SDK](websites-dotnet-webjobs-sdk.md) 1.x.

Ukázky, které ukazují, jak vytvořit objekty BLOB, najdete v článku [Použití úložiště Azure fronty s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 
        
Průvodce předpokládá víte, [jak vytvořit projekt WebJob ve Visual Studiu s připojovací řetězec, které přejděte ke svému účtu úložiště](websites-dotnet-webjobs-sdk-get-started.md) nebo k [víc účtům úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Jak aktivovat funkci objektů blob vytvořila nebo aktualizovala

Tato část popisuje použít `BlobTrigger` atribut. 

> [AZURE.NOTE] WebJobs SDK prohledávají protokoly sledovat nové nebo změněné objektů BLOB. Tento proces není v reálném čase; funkce nemusí získat spouštěný do několika minut nebo není po vytvoření objektů blob. Navíc [úložiště, které protokoly vytvořené ve "maximální úsilí"](https://msdn.microsoft.com/library/azure/hh343262.aspx) základna; je možné, že budou zachyceny všechny události. Za určitých podmínek propásli protokoly. Pokud rychlost a spolehlivost omezení objektů blob aktivace nejsou přípustné aplikace, doporučujeme vytvořit fronty zprávu při vytvoření objektů blob a použít atribut [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) místo `BlobTrigger` atribut funkci zpracuje objektů blob.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Jeden zástupný symbol pro název objektů blob s příponou  

Následující příklad kódu slouží ke kopírování objektů BLOB textu, které se zobrazí v kontejneru *zadávání* do kontejneru *výstup* :

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Atribut konstruktor má parametr řetězec určující název kontejneru a zástupný symbol pro objektů blob. V tomto příkladu objektů blob s názvem *Blob1.txt* vytvořeném v kontejneru *vstupní* funkci vytvoří objektů blob s názvem *Blob1.txt* v kontejneru *výstupu* . 

Jak je vidět v následujícím příkladu je možné zadat název vzorek se zástupným symbolem objektů blob název:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Tento kód slouží ke kopírování pouze objektů BLOB, jejichž názvy začínají "původní-". Například *Původní Blob1.txt* v kontejneru *vstupní* zkopírována do *Kopie Blob1.txt* v kontejneru *výstupu* .

Pokud je třeba zadat název vzor pro názvy objektů blob, které mají složené závorky v názvu, dvojité složené závorky. Například, pokud chcete najít objektů BLOB v kontejneru *obrázky* , které mají názvy takto:

        {20140101}-soundfile.mp3

Pomocí této funkce vzorku:

        images/{{20140101}}-{name}

V příkladu bude hodnotu z pole *název* zástupný symbol *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Samostatné objektů blob názvem a příponou zástupné symboly

Následující příklad kódu se mění koncovku souboru zkopíruje objektů BLOB, které se zobrazí v kontejneru *zadávání* do kontejneru *výstupu* . Kód protokoly rozšíření *vstupní* objektů blob a nastaví rozšíření objektů blob *výstup* *txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a>Typy, které můžete vytvořit vazbu s objekty BLOB

Můžete použít `BlobTrigger` atribut na následující typy:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Další typy rekonstruovány [ICloudBlobStreamBinder](#icbsb) 

Pokud chcete pracovat přímo pomocí účtu Azure úložiště, můžete si taky přidat `CloudStorageAccount` parametr podpis metody.

Příklady najdete v článku [Kód vazby objektů blob azure webjobs sdk úložiště na GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Získání text objektů blob obsahu vazbou řetězec

Pokud jsou objekty BLOB textu, `BlobTrigger` lze použít `string` parametr. Následující příklad kódu váže objektů blob text do `string` parametr s názvem `logMessage`. Zapsat obsah objektů blob na řídicí panel WebJobs SDK použije funkce tento parametr. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Získání serializován objektů blob obsahu pomocí ICloudBlobStreamBinder

Následující příklad kódu používá třídu implementující `ICloudBlobStreamBinder` povolit `BlobTrigger` atribut svázat objektů blob k `WebImage` typu.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

`WebImage` Vazba kód je k dispozici v `WebImageBinder` předmětu, která je odvozena z `ICloudBlobStreamBinder`.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Získání cesta objektů blob pro spouštěcí objektů blob

Získání kontejneru název a název objektů blob objektů blob, který má spustil funkci, zahrnout `blobTrigger` řetězec parametrů v podpisu (funkce).

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Postup v případě poškozená objekty BLOB

Když `BlobTrigger` funkce dojde k chybě v SDK volá ho znovu, v případě, že selhání způsobil přechodná chyby. Pokud chyba způsobená obsahu objektů blob, funkce se nezdaří pokaždé, když se pokusí ke zpracování objektů blob. Ve výchozím nastavení v SDK hovorů funkci až 5 číslem pro dané objektů blob. Když na pátý vyzkoušet nepovede, SDK přidá zprávu do fronty s názvem *webjobs blobtrigger poison*.

Maximální počet opakování lze konfigurovat. Stejné nastavení [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) se používá pro zpracování poškozená objektů blob a zpracování zpráv poškozená fronty. 

Zpráva fronty pro poškozená objekty BLOB je JSON objekt, který obsahuje následující vlastnosti:

* FunctionId (ve formátu *{WebJob název}*. Funkce. *{Název funkce}*, například: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" nebo "PageBlob")
* Název_kontejneru
* BlobName
* ETag (identifikátor verze objektů blob, například: "0x8D1DC6E70A277EF")

V následujícím příkladu `CopyBlob` funkce obsahuje kód, který způsobí selhání pokaždé, když je označená jako. Po SDK hovorů pro maximální počet opakování, zprávy se vytvoří ve frontě poškozená objektů blob a zpracována zprávy `LogPoisonBlob` (funkce). 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }
        
        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

V SDK automaticky deserializuje JSON zprávy. Tady je `PoisonBlobMessage` třídy: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>Algoritmus objektů BLOB hlasování

WebJobs SDK kontroluje všechny kontejnery nastavil `BlobTrigger` atributy při spuštění aplikace. Účet velké úložiště kontrola může nějakou dobu trvat, takže chvíli může být před nových objektů BLOB najdou a `BlobTrigger` funkce provádějí.

Zjistit objekty BLOB novými a upravenými po spuštění aplikace v SDK pravidelně bude číst z protokolů úložiště objektů blob. Protokoly objektů blob jsou paměti a získat fyzicky zapsat pouze každých 10 minut nebo tak, takže může existovat významné zpoždění po objektů blob vytvořila se nebo aktualizovala před odpovídající `BlobTrigger` provede (funkce). 

Existuje výjimku pro objekty BLOB vytvářených pomocí `Blob` atribut. Při WebJobs SDK vytváření nových objektů blob, předá nových objektů blob okamžitě všechny odpovídající `BlobTrigger` funkcí. Proto máte řetězec objektů blob vstupy a výstupy SDK můžete zpracovat efektivně. Ale pokud chcete zhoršeným latence spuštění svého objektů blob zpracování funkcí pro objekty BLOB, které jsou vytvořené nebo aktualizován jiným způsobem, doporučujeme vám použít `QueueTrigger` nekoupili `BlobTrigger`.

### <a id="receipts"></a>Potvrzení objektů BLOB

WebJobs SDK zajišťuje, že žádný `BlobTrigger` funkce získá s názvem víckrát pro stejný objektů blob nový nebo aktualizovaný. Je to tak, že správa *objektů blob příjmy* abyste mohli určit, zda byla zpracována verze dané objektů blob.

Potvrzení objektů BLOB jsou uložené v kontejneru s názvem *azure webjobs hosts* v okně účet Azure úložiště nastavil AzureWebJobsStorage připojovací řetězec. Oznámení o objektů blob obsahuje následující informace:

* Funkce, která označovalo jako pro objektů blob ("*{WebJob název}*. Funkce. *{Název funkce}*", například:"WebJob1.Functions.CopyBlob")
* Jméno container
* Typ objektů blob ("BlockBlob" nebo "PageBlob")
* Název objektů blob
* ETag (identifikátor verze objektů blob, například: "0x8D1DC6E70A277EF")

Pokud chcete vynutit opakovaného zpracování z objektů blob, můžete ručně odstranit objektů blob potvrzení pro tuto objektů blob *azure webjobs hosts* kontejneru.

## <a id="queues"></a>Související témata uvedených v článku fronty

Informace o způsobu zpracování objektů blob zpracování spouštěný kliknutím zprávu fronty nebo WebJobs SDK scénáře nejsou specifické pro objektů blob zpracování v tématu [Použití úložiště Azure fronty s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Související témata v tomto článku patřit následující úkoly:

* Funkce asynchronní
* Více instancí
* Vypnutí
* Použít atributy WebJobs SDK v těle funkce
* Nastavení připojení řetězce SDK v kódu.
* Nastavení hodnot pro WebJobs SDK konstruktor parametrů v kódu
* Konfigurace `MaxDequeueCount` poškozená objektů blob zpracování.
* Aktivace funkce ručně
* Zápisu protokolů

## <a id="nextsteps"></a>Další kroky

Tato příručka dodala ukázek kódu, které ukazují, jak řešit obvyklé scénáře pro práci s objekty BLOB Azure. Další informace o používání Azure WebJobs a WebJobs SDK tématech [Azure WebJobs doporučené](http://go.microsoft.com/fwlink/?linkid=390226).
 
