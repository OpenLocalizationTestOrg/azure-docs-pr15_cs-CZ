<properties 
    pageTitle="Aktivace aplikací služby rozhraní API aplikace | Microsoft Azure" 
    description="Jak implementovat aktivačních událostí v aplikaci pro rozhraní API aplikace služby Azure" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Aktivace aplikací Azure rozhraní API aplikace služby

>[AZURE.NOTE] Tuto verzi článku platí pro rozhraní API aplikace 2014 12 01 náhled schéma verze.


## <a name="overview"></a>Základní informace

Tento článek vysvětluje, jak implementovat rozhraní API aplikace aktivace a používat je z app použití logických operátorů.

Všechny fragmenty kódu v tomto tématu se zkopírují z [příkladu FileWatcher rozhraní API aplikace](http://go.microsoft.com/fwlink/?LinkId=534802). 

Všimněte si, že budete muset stáhnout balíček nuget následující kódy v tomto článku k vytvoření a spuštění: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Co je aktivace rozhraní API aplikace?

Je běžné situace pro rozhraní API aplikace aktivováno událost tak, aby klienti rozhraní API aplikace můžete využít příslušné akce v odpověď na událost. Rozhraní REST API na základě mechanismus, který podporuje tento scénář se nazývá aktivační události pro rozhraní API aplikace. 

Například Řekněme, že kód klienta používá [Twitter konektor rozhraní API aplikace](../app-service-logic/app-service-logic-connector-twitter.md) a kód je potřeba provést akci v závislosti na nové tweety obsahující určitá slova. V tomto případě může nastavíte hlasování nebo nabízených aktivační události pro usnadnění tato potřebná.

## <a name="poll-trigger-versus-push-trigger"></a>Aktivační událost hlasování a aktivační událost nabízené

V současné době podporuje dva typy aktivačních událostí:

- Aktivační událost hlasování - klient odesílá dotaz aplikaci rozhraní API pro oznámení o události, s vyvolala 
- Aktivační událost nabízené - klienta je oznámit, že rozhraní API aplikace se aktivuje, zvláštní události 

### <a name="poll-trigger"></a>Aktivační událost hlasování

Aktivační hlasování implementovaná jako běžný rozhraní REST API a hlasování abyste mohli získávat oznámení očekává svým klientům (například logiky aplikace). Během klienta zachovat státu, hlasování aktivační událost, samotné je příslušnosti. 

Následujících údajů o paketů žádostí a odpovědí ilustrují některé klíčové aspekty smlouvy hlasování aktivační události:

- Žádost o
    - Nastavit informace HTTP metoda: GET
    - Parametry
        - triggerState - tento volitelný parametr umožňuje určit že jejich stav tak, aby aktivační událost hlasování správně rozhodnout, jestli vrátit oznámení nebo ne na základě zadaného stavu.
        - Parametry specifické pro rozhraní API
- Odpověď
    - Stavový kód **200** - žádost o platný a tam je oznámení z aktivační událost. Obsah oznámení bude těle odpovědi. Záhlaví "Opakovat po" odpověď označuje, že musíte další oznámení data načtená hovoru další žádosti o.
    - Stavový kód **202** - žádost je platný, ale je bez oznámení o nové z aktivační událost.
    - Stavový kód **4xx** - požadavek není platný. Klient neměli opakování žádosti.
    - Stavový kód **5xx** – žádost má za následek chyba interním serveru nebo dočasné potíže. Klient by měl opakování žádosti.

Následující fragment kódu je příklad toho, jak implementovat aktivační hlasování.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

Testování tato hlasování aktivační událost, postupujte takto:

1. Nasazení aplikace rozhraní API s nastavení ověřování **anonymní**veřejnosti.
2. Zavolejte na linku operaci **dotykového ovládání** dotyková souboru. Následující obrázek znázorňuje žádosti o ukázkových prostřednictvím pošťák.
   ![Operace dotykového ovládání hovorů přes pošťák](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Zavolejte na aktivační událost hlasování s parametrem **triggerState** nastaveným na časové razítko před kroku #2. Následující obrázek znázorňuje žádost o ukázku prostřednictvím pošťák.
   ![Aktivační událost hlasování volání přes pošťák](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Aktivační událost nabízené

Aktivační událost nabízené implementovaná jako běžný REST API, které posune oznámení klientům, kteří si zaregistrovali Pokud chcete být upozorňováni zvláštní události.

Následujících údajů o paketů žádostí a odpovědí ilustrují některé klíčové aspekty smlouvy nabízených aktivační událost.

- Žádost o
    - Metoda HTTP: umístění
    - Parametry
        - SpuštěcíID: povinné - neprůhledné řetězec (například GUID), který představuje registrace aktivační připínáčku.
        - callbackUrl: povinné - adresa URL zpětné vyvolat při události. Vyvolání je jednoduchý příspěvek HTTP hovoru.
        - Parametry specifické pro rozhraní API
- Odpověď
    - Stav kód **200** - požadavek na registraci klienta úspěšně.
    - Stavový kód **4xx** - požadavek není platný. Klient neměli opakování žádosti.
    - Stavový kód **5xx** – žádost má za následek chyba interním serveru nebo dočasné potíže. Klient má opakovat požadavek.
- Zpětné
    - Metoda HTTP: příspěvku
    - Žádost o textu: obsah oznámení.

Následující fragment kódu je příklad toho, jak implementovat aktivační událost nabízené:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

Testování tato hlasování aktivační událost, postupujte takto:

1. Nasazení aplikace rozhraní API s nastavení ověřování **anonymní**veřejnosti.
2. Přejděte na [http://requestb.in/](http://requestb.in/) vytvořit RequestBin, který bude sloužit jako adresa URL zpětné.
3. Zavolejte na aktivační událost nabízené s GUID jako **SpuštěcíID** a adresu URL RequestBin jako **callbackUrl**.
   ![Aktivační událost nabízené volání přes pošťák](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Zavolejte na linku operaci **dotykového ovládání** dotyková souboru. Následující obrázek znázorňuje žádosti o ukázkových prostřednictvím pošťák.
   ![Operace dotykového ovládání hovorů přes pošťák](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Zaškrtněte políčko RequestBin potvrďte, že vlastnost výstup vyvolání zpětné nabízených aktivační událost.
   ![Aktivační událost hlasování volání přes pošťák](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Popište aktivačních událostí v rozhraní API definici

Po provedení aktivační události a nasazení aplikace rozhraní API Azure, přejděte na zásuvné **Definice rozhraní API** v portálu Azure náhled a uvidíte, že jsou v uživatelském rozhraní, které je řízeno definici Swagger rozhraní API 2.0 rozhraní API aplikace automaticky rozpoznat aktivačních událostí.

![Rozhraní API definice zásuvné](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Pokud tlačítko **Stáhnout Swagger** a otevřete soubor JSON, uvidíte výsledky podobná této:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

Vlastnost rozšíření **x-ms-schedular-aktivační událost** se, jak jsou popsány v rozhraní API definice aktivačních událostí a když automaticky se přidají Brána rozhraní API aplikace požadovat definici rozhraní API prostřednictvím brány, jestliže žádosti na jeden z následujících kritérií. (Můžete taky přidat tuto vlastnost ručně.)

- Aktivační událost hlasování
    - Je-li metodu HTTP **GET**.
    - Pokud vlastnost **operationId** obsahuje řetězec **aktivační událost**.
    - Pokud vlastnost **Parametry** obsahuje parametr pomocí vlastnosti **název** nastavena na **triggerState**.
- Nabízená aktivační událost
    - Pokud je metoda HTTP **umístění**.
    - Pokud vlastnost **operationId** obsahuje řetězec **aktivační událost**.
    - Pokud vlastnost **Parametry** obsahuje parametr pomocí vlastnosti **název** nastavena na **SpuštěcíID**.

## <a name="use-api-app-triggers-in-logic-apps"></a>Pomocí aktivačních událostí rozhraní API aplikace v aplikacích logiku

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Seznam a konfigurace rozhraní API aplikace aktivačních událostí v aplikace Návrhář použití logických operátorů

Pokud vytvoříte v aplikaci použití logických operátorů ve stejné skupině zdroje rozhraní API aplikace, budete moct přidat návrháře plátno jednoduše tak, že na ni kliknete. Následující obrázky ilustrují toto:

![Aktivace aplikací Návrhář použití logických operátorů](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Konfigurace spouštěcí hlasování v návrháři aplikace použití logických operátorů](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Konfigurace aktivační událost nabízená v aplikace Návrhář použití logických operátorů](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>Optimalizace rozhraní API aplikace Spouštěče aplikací logiky

Po přidání aktivačních událostí do aplikace pro rozhraní API tam několik věcí, které můžete dělat zlepšovat možnosti při používání rozhraní API aplikace v aplikaci logiku.

Například parametr **triggerState** aktivačních událostí hlasování nastavte následující výraz v aplikaci použití logických operátorů. Tento výraz mají vyhodnotit poslední vyvolání aktivační událost z aplikace použití logických operátorů a vraťte se daná hodnota.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Poznámka: Vysvětlení funkcí sloužící ve výrazu výše uvedené, naleznete v dokumentaci na [Použití logických operátorů aplikace pracovního postupu Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Uživatelé aplikace logiky musel zadat výraz nad parametru **triggerState** při používání aktivační událost. Je možné mít tuto hodnotu přednastavené návrhářem aplikace logiky prostřednictvím vlastnost rozšíření **x ms Plánovač doporučení**.  Vlastnost rozšíření **x-ms viditelnost** můžete nastavit hodnotu *vnitřní* , takže samotné parametr není uveden v návrháři.  Následující úryvek ukazuje, že.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Aktivace nabízených musí parametr **SpuštěcíID** jednoznačně identifikovat aplikaci logiky. Doporučený osvědčený postup je nastavovaná vlastnost na název pracovního postupu pomocí následující výraz:

    @workflow().name

Používání vlastnosti rozšíření **x ms Plánovač doporučení** a **x-ms viditelnost** v jeho linkové rozhraní API, rozhraní API aplikace můžete sdělit návrháře logiky aplikace automaticky nastavit tento výraz pro uživatele.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Přidání vlastnosti rozšíření v rozhraní API definice

Informace o další metadata – například rozšíření vlastnosti **x ms Plánovač doporučení** **x-ms viditelnost** - lze přidat v rozhraní API definice jedním ze dvou způsobů: statické nebo dynamické.

Statická metadata můžete přímo upravit tento soubor */metadata/apiDefinition.swagger.json* v projektu a ruční přidání vlastnosti.

Rozhraní API aplikací pomocí dynamického metadata můžete upravit tento soubor SwaggerConfig.cs přidání operace filtru, které můžete přidat tyto rozšíření.

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Následující obrázek je příkladem implementaci této třídy usnadnit scénář dynamické metadata.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
