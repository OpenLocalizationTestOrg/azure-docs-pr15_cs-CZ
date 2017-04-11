<properties 
    pageTitle="Co je Azure WebJobs SDK" 
    description="Úvod do Azure WebJobs SDK. Vysvětluje, co v SDK je, typické scénáře je užitečné pro a kód vzorky." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Co je Azure WebJobs SDK

## <a id="overview"></a>Základní informace

Tento článek vysvětluje, co je WebJobs SDK, kontroluje některé běžné scénáře je užitečné pro která poskytuje Následuje přehled toho, jak ho používat v kódu.

[WebJobs](websites-webjobs-resources.md) je funkce služby Azure aplikace, která umožňuje spustit program nebo skript v rámci stejné jako v prohlížeči, rozhraní API aplikace nebo mobilní aplikaci. Má [WebJobs SDK](websites-webjobs-resources.md) účel zjednodušit kód, který napíšete pro běžné úkoly, můžete provést, například zpracování obrázků, fronty zpracování WebJob RSS agregace, soubor údržby a odesílání e-mailů. WebJobs SDK má integrované funkce pro práci s Azure úložiště a služby Bus pro plánování úkolů a zpracování chyb a pro další obvyklé scénáře. Kromě toho je navržen tak, aby byl extensible. [WebJobs SDK je otevřít zdroj](https://github.com/Azure/azure-webjobs-sdk/), a to není [otevřete úložiště zdrojů pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

WebJobs SDK zahrnuje tyto prvky:

* **Balíčky NuGet**. Balíčky NuGet přidané do projektu aplikace Visual Studio konzoly poskytují rámec, který používá váš kód tak, že architekturu své způsoby s WebJobs SDK atributy.
  
* **Řídicí panel**. Část WebJobs SDK je součástí aplikace služby Azure a poskytuje bohaté sledování a diagnostice programy, které používají balíčků NuGet. Nemusíte kódu použití těchto funkcí sledování a diagnostice.

## <a id="scenarios"></a>Scénáře

Tady jsou některé obvyklé scénáře, které můžete snadněji zpracovat s Azure WebJobs SDK:

* Obrázek zpracování nebo jiné procesoru vyžadující značnou práce. Běžné funkce webů je umožňuje ukládat obrázky nebo videa. Často chcete pracovat s obsahem po nahráním, ale nechcete nastavit uživatele počkejte to udělat.

* Zpracování fronty. Běžným způsobem pro web frontend komunikovat s back-end služby, je použít fronty. Potřebujete-li práci na webu, přesune zprávu do fronty. Služba back-end použije zprávy z fronty a provádí. Můžete použít fronty pro zpracování obrázků: například, když uživatel odešle počet souborů, umístit názvy souborů fronty zprávu, že mají být převzít back-end pro zpracování. Nebo můžete použít fronty zlepšit rychlostí reakce serveru. Třeba místo psaní přímo k databázi SQL, napište do fronty dát uživatelům, které budete mít, a zpřístupněte back-end úchyt vysokou latencí relační databáze služeb práce. Příklad fronty zpracování s procesu najdete v tématu [kurz WebJobs SDK Začínáme](websites-dotnet-webjobs-sdk-get-started.md).

* Agregace RSS. Pokud máte web, který udržuje seznam informačních kanálů RSS, může ho naimportovat ve všech článků z informační kanály v pozadí obrázku.

* Soubor údržby, například agregaci nebo čištění protokoly.  Bude pravděpodobně nutné vytvářených pomocí několika weby nebo rozsahy samostatnou dobu, které chcete zahrnout do sloučeného ke spuštění analýzy úlohy na nich souborů protokolu. Nebo můžete chtít naplánovat úlohy na spustit týdenní vyčištění staré souborů protokolu.

* Průniku do Azure tabulek. Může mít soubory uložené a objekty BLOB a chcete je analyzovat a uložení dat v tabulkách. Funkce průniku může psaní spoustou řádků (miliony v některých případech) a WebJobs SDK umožňuje snadno implementovat tuto funkci. V SDK obsahuje také v reálném čase sledování indikátory průběhu například počet řádků v tabulce.

* Jiné – náročné úkoly, které chcete spustit podproces pozadí, například [odesílání e-mailů](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 

* Úkoly, které chcete spustit na plán, jako jsou operace zálohování každou noc.

V mnoha podobnému sledu můžete změnit velikost do webových aplikací pro spuštění na více VMs, které chcete spustit více WebJobs současně. V některých případech to může mít za následek stejná data zpracovány tisknutím, ale není problém při použití předdefinované fronty, objektů blob a služby Bus aktivačních událostí SDK WebJobs. V SDK zaručuje, že vaše funkce dojde ke zpracování pouze jednou pro každou zprávu nebo objektů blob.

WebJobs SDK taky snadno zpracovávání běžné zpracování chyb ve vzorcích scénáře. Můžete nastavit upozornění o neodeslání oznámení při funkci úspěšné a časových limitů můžete nastavit tak, že funkce se automaticky zruší, pokud není dokončena v rámci zadaný časový limit.

## <a id="code"></a>Ukázky

Kód pro zpracování typické úkoly, které spolupracují s Azure úložiště je velmi jednoduché. V aplikaci konzoly `Main` metoda vytvoříte `JobHost` objekt, který souřadnic volání metody píšete. Framework WebJobs SDK věděli, kdy volat své způsoby a co hodnoty parametrů používat podle WebJobs SDK atributy, které používáte v nich. V SDK obsahuje *aktivační události* pro, zadáte co podmínky způsobit, že funkce volat a *pojiva* zadejte, jak získat informace do a od parametry metody.

Například atribut [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) způsobí, že funkce volat po přijetí zprávy z fronty, a pokud formátu JSON bajt matice nebo vlastní typ, zpráva automaticky rekonstruované. Atribut [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) spustí proces při vytvoření nových objektů blob v úložišti Azure účet.

Tady je jednoduchý program, který zjišťuje ve frontě a vytvoří objektů blob pro každou fronty se zpráva:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

`JobHost` Objekt je kontejner pro sadu funkcí pozadí. `JobHost` Objekt sleduje funkce výrobky událostí, ke kterým aktivovat, a při výskytu spouštěcí události spustí funkce. Volání `JobHost` způsob, jak označuje, zda chcete procesu kontejneru spuštění v aktuální podproces nebo podprocesem na pozadí. V tomto příkladu `RunAndBlock` metoda spustí proces nepřetržitě na aktuální vlákna.

Protože `ProcessQueueMessage` metoda v tomto příkladu má `QueueTrigger` atribut, aktivační události pro tuto funkci je přijetí novou zprávu fronty. `JobHost` Objekt sleduje pro nové zprávy fronty na zadané frontě ("webjobsqueue" v tomto příkladu) a nalezený jednu volá `ProcessQueueMessage`. 

`QueueTrigger` Atribut vazby `inputText` parametr hodnotu fronty zprávy. A `Blob` atribut vazby `TextWriter` objektů blob s názvem "blobname" v kontejneru s názvem "název_kontejneru".  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

K zápisu hodnotu fronty zprávy objektů blob použije funkce tyto parametry potom:

        writer.WriteLine(inputText);

Funkce aktivační události a pořadače WebJobs SDK výrazně zjednodušit kód, který máte k zápisu. Nižší úrovně kódu potřebného pro zpracování dotazů, objekty BLOB nebo soubory nebo zahajte naplánovaných úkolů, se děje rámcem WebJobs SDK. Například rámec vytvoří fronty, které nejsou zatím, otevře se fronty čtení fronty zprávy, odstraníte fronty zprávy při zpracování dokončení, vytvoří kontejnery objektů blob, které ještě zapíše do objektů BLOB atd.

Následující příklad ukazuje řadu aktivačních událostí v jedné WebJob: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, a `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Plánování

`TimerTrigger` Atribut umožňuje funkcí aktivační události pro spuštění na plán. Plánujete WebJob jako celek prostřednictvím Azure nebo plánu jednotlivých funkcí WebJob pomocí WebJobs SDK `TimerTrigger`. Tady je ukázka kódu.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

Další ukázkové kód najdete v článku [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) v úložišti azure webjobs sdk rozšíření na GitHub.com.

## <a name="extensibility"></a>Rozšíření

Nejste omezené na předdefinované funkce – WebJobs SDK slouží k psaní vlastní aktivačními událostmi a pojiva.  Můžete například vytvořit aktivační události pro události mezipaměti a pravidelných plány. [Otevřete zdrojový úložiště](https://github.com/Azure/azure-webjobs-sdk-extensions) obsahuje [podrobné pokyny na WebJobs SDK rozšiřitelnost](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) a ukázkové kód, který vám pomůžou začít pracovat tvorbu vlastních aktivačních událostí a pojiva.

## <a id="workerrole"></a>Použití SDK WebJobs mimo WebJobs

Program, který používá WebJobs SDK je aplikace standardní konzoly a mohlo by umožnit spuštění kamkoli – nemusí spustit jako WebJob. Program můžete otestovat místně vývojového počítače a ve výrobním spuštěním v roli pracovní cloudové služby nebo služba Windows podle potřeby jednu z těchto prostředí. 

Však řídicího panelu je k dispozici pouze jako rozšíření pro webovou aplikaci služby Azure aplikace. Pokud chcete spustit mimo WebJob a dál používat na řídicím panelu, můžete nakonfigurovat web appu používat stejný účet úložiště odkazující připojovací řetězec WebJobs SDK řídicích panelů a řídicích panelů WebJobs web appu, zobrazí se data o funkci spuštění z aplikace, na kterém běží jinam, postupujte takto. Se dostanete do řídicího panelu pomocí.scm.azurewebsites.net/azurejobs/#/functions*{webappname}*https:// adresu URL. Další informace najdete v tématu [získání řídicího panelu pro místní vývoj s WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), ale Všimněte si, že příspěvku na blogu zobrazuje původní název připojovacího řetězce. 

## <a id="nostorage"></a>Funkce řídicích panelů

WebJobs SDK nabízí několik výhod i v případě, že nepoužíváte WebJobs SDK aktivačních událostí nebo pojiva:

* Můžete vyvolání funkce z řídicího panelu.
* Nahrajete funkcí na řídicím panelu.
* Zobrazení protokolů v řídicím panelu propojená s konkrétní WebJob (protokoly aplikací, který napsal pomocí Console.Out Console.Error, sledování, atd.) nebo propojená s vyvolání konkrétní funkcí, které vygenerovalo je (protokoly, který napsal pomocí `TextWriter` objekt, který v SDK předává funkci jako parametr). 

Další informace najdete v tématu [Jak ručně vyvolání funkce](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) a [jak psát protokoly](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Další kroky

Další informace o WebJobs SDK tématech [Azure WebJobs doporučené](http://go.microsoft.com/fwlink/?linkid=390226).

Informace o nejnovějších vylepšení WebJobs SDK najdete v tématu [Poznámky k verzi](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).
 
