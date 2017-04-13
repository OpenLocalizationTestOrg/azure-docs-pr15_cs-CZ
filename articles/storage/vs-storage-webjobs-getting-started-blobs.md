<properties
    pageTitle="Začínáme s objektů blob úložiště a Visual Studio připojené služby (WebJob projektů) | Microsoft Azure"
    description="Jak začít používat úložiště objektů Blob v projektu WebJob po připojení k Azure úložiště pomocí aplikace Visual Studio připojení služby."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Začínáme s objektů Blob Azure úložiště a Visual Studio připojené služby (WebJob projektů)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Základní informace

Tento článek obsahuje C# ukázek kódu, které ukazují, jak aktivovat proces vytvořila nebo aktualizovala objektů blob Azure. Ukázky kódu používat verzi [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) 1.x. Po přidání účtu úložiště WebJob projektu pomocí dialogového okna aplikace Visual Studio **Přidat připojené služby** příslušný balíček Azure úložiště NuGet je nainstalovaný, odkazy na příslušné .NET se přidají do projektu a připojení k účtu úložiště jsou aktualizované v konfiguračního souboru.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Jak aktivovat funkci objektů blob vytvořila nebo aktualizovala

Tato část popisuje použít atribut **BlobTrigger** .

 **Poznámka:** WebJobs SDK kontroluje protokoly sledovat nové nebo změněné objektů BLOB. Tento proces pomalý podstatě; funkce nemusí získat spouštěný do několika minut nebo není po vytvoření objektů blob.  Pokud potřebujete-li zpracovat objektů BLOB ihned aplikace, doporučujeme vytvořit fronty zprávu při vytvoření objektů blob a použít atribut [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) místo atribut **BlobTrigger** na funkci, která zpracuje objektů blob.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Jeden zástupný symbol pro název objektů blob s příponou  

Následující příklad kódu slouží ke kopírování objektů BLOB textu, které se zobrazí v kontejneru *zadávání* do kontejneru *výstup* :

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Atribut konstruktor má parametr řetězec určující název kontejneru a zástupný symbol pro název objektů blob. V tomto příkladu objektů blob s názvem *Blob1.txt* vytvořeném v kontejneru *vstupní* funkci vytvoří objektů blob s názvem *Blob1.txt* v kontejneru *výstupu* .

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

## <a name="types-that-you-can-bind-to-blobs"></a>Typy, které můžete vytvořit vazbu s objekty BLOB

Můžete použít atribut **BlobTrigger** na následující typy:

* **řetězec**
* **TextReader**
* **Toku**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Další typy rekonstruovány [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Pokud chcete zprovoznit účet Azure úložiště, můžete také přiřadit parametru **CloudStorageAccount** podpis metody.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Získání text objektů blob obsahu vazbou řetězec

Pokud jsou objekty BLOB textu, **BlobTrigger** můžete použije parametru **řetězce** . Následující příklad kódu váže objektů blob text parametru **řetězce** s názvem **logMessage**. Zapsat obsah objektů blob na řídicí panel WebJobs SDK použije funkce tento parametr.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Získání serializován objektů blob obsahu pomocí ICloudBlobStreamBinder

Následující příklad kódu používá třídu implementující **ICloudBlobStreamBinder** povolíte atribut **BlobTrigger** svázat objektů blob typ **WebImage** .

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

Kód vazby **WebImage** je k dispozici ve třídě **WebImageBinder** , která je odvozena z **ICloudBlobStreamBinder**.

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

## <a name="how-to-handle-poison-blobs"></a>Postup v případě poškozená objekty BLOB

Pokud funkce **BlobTrigger** selže, SDK hovorů ho znovu v případě, že selhání způsobil přechodná chyby. Pokud chyba způsobená obsahu objektů blob, funkce se nezdaří pokaždé, když se pokusí ke zpracování objektů blob. Ve výchozím nastavení v SDK hovorů funkci až 5 číslem pro dané objektů blob. Když na pátý vyzkoušet nepovede, SDK přidá zprávu do fronty s názvem *webjobs blobtrigger poison*.

Maximální počet opakování lze konfigurovat. Stejné nastavení [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) se používá pro zpracování poškozená objektů blob a zpracování zpráv poškozená fronty.

Zpráva fronty pro poškozená objekty BLOB je JSON objekt, který obsahuje následující vlastnosti:

* FunctionId (ve formátu *{WebJob název}*. Funkce. *{Název funkce}*, například: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" nebo "PageBlob")
* Název_kontejneru
* BlobName
* ETag (identifikátor verze objektů blob, například: "0x8D1DC6E70A277EF")

V následující ukázce kódu má funkce **CopyBlob** kód, který způsobí selhání pokaždé, když je označená jako. Po SDK vyžaduje je maximální počet opakování, zprávy se vytvoří ve frontě poškozená objektů blob a zpracování zprávy pomocí funkce **LogPoisonBlob** .

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

V SDK automaticky deserializuje JSON zprávy. Tady je třída **PoisonBlobMessage** :

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>Algoritmus objektů BLOB hlasování

WebJobs SDK kontroluje všechny kontejnery nastavil **BlobTrigger** atributy při spuštění aplikace. Účet velké úložiště kontrola může určitou dobu trvat, takže chvíli může být před nových objektů BLOB najdou a spouštět **BlobTrigger** funkce.

Zjistit objekty BLOB novými a upravenými po spuštění aplikace v SDK pravidelně bude číst z protokolů úložiště objektů blob. Protokoly objektů blob jsou paměti a získat fyzicky zapsat pouze každých 10 minut nebo tak, aby může existovat významné zpoždění po objektů blob vytvořila se nebo aktualizovala před odpovídající **BlobTrigger** funkce spustí.

Existuje výjimku pro objekty BLOB vytvářených pomocí atribut **objektů Blob** . Když WebJobs SDK vytvoří nový objektů blob, předá nových objektů blob okamžitě žádné odpovídající **BlobTrigger** funkce. Proto máte řetězec objektů blob vstupy a výstupy SDK můžete zpracovat efektivně. Ale pokud chcete zhoršeným latence spuštění svého objektů blob zpracování funkcí pro objekty BLOB, které jsou vytvořené nebo aktualizován jiným způsobem, doporučujeme vám použít **QueueTrigger** spíše než **BlobTrigger**.

### <a name="blob-receipts"></a>Potvrzení objektů BLOB

WebJobs SDK zajišťuje, že žádná funkce **BlobTrigger** získá s názvem víckrát pro stejný objektů blob nový nebo aktualizovaný. Je to tak, že správa *objektů blob příjmy* abyste mohli určit, zda byla zpracována verze dané objektů blob.

Potvrzení objektů BLOB jsou uložené v kontejneru s názvem *azure webjobs hosts* v okně účet Azure úložiště nastavil AzureWebJobsStorage připojovací řetězec. Oznámení o objektů blob obsahuje následující informace:

* Funkce, která označovalo jako pro objektů blob ("*{WebJob název}*. Funkce. *{Název funkce}*", například:"WebJob1.Functions.CopyBlob")
* Jméno container
* Typ objektů blob ("BlockBlob" nebo "PageBlob")
* Název objektů blob
* ETag (identifikátor verze objektů blob, například: "0x8D1DC6E70A277EF")

Pokud chcete vynutit opakovaného zpracování z objektů blob, můžete ručně odstranit objektů blob potvrzení pro tuto objektů blob *azure webjobs hosts* kontejneru.

## <a name="related-topics-covered-by-the-queues-article"></a>Související témata uvedených v článku fronty

Informace o způsobu zpracování objektů blob zpracování spouštěný kliknutím zprávu fronty nebo WebJobs SDK scénáře nejsou specifické pro objektů blob zpracování v tématu [Použití úložiště Azure fronty s WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Související témata v tomto článku patřit následující úkoly:

* Funkce asynchronní
* Více instancí
* Vypnutí
* Použít atributy WebJobs SDK v těle funkce
* Nastavení připojení řetězce SDK v kódu.
* Nastavení hodnot pro WebJobs SDK konstruktor parametrů v kódu
* Nakonfigurujte **MaxDequeueCount** poškozená objektů blob zpracování.
* Aktivace funkce ručně
* Zápisu protokolů

## <a name="next-steps"></a>Další kroky

Tento článek dodala ukázek kódu, které ukazují, jak řešit obvyklé scénáře pro práci s objekty BLOB Azure. Další informace o používání Azure WebJobs a WebJobs SDK tématech [Azure WebJobs si přečtěte následující dokumentaci](http://go.microsoft.com/fwlink/?linkid=390226).
