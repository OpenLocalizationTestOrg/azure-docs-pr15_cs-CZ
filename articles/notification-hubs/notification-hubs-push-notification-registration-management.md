<properties
    pageTitle="Registrace správy"
    description="Toto téma vysvětluje, jak si zaregistrovat zařízení s rozbočovače oznámení pro příjem nabízených oznámení."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="registration-management"></a>Správa registrace

##<a name="overview"></a>Základní informace

Toto téma vysvětluje, jak si zaregistrovat zařízení s rozbočovače oznámení pro příjem nabízených oznámení. Téma popisuje registrace na nejvyšší úrovni a potom přináší dva hlavní vzorků pro registraci zařízení: registrace ze zařízení přímo k rozbočovači oznámení a registrace prostřednictvím serverové části aplikace. 


##<a name="what-is-device-registration"></a>Co je registrace zařízení

Registrace zařízení s rozbočovači oznámení lze provést pomocí **zápisu** nebo **instalace**.

#### <a name="registrations"></a>Registrace
Registrace platformu oznámení služby (PNS) úchyt pro zařízení přidružuje značky a případně šablony. Úchyt PNS může být ChannelURI, token zařízení nebo id registrace GCM. Značky slouží k oznámení směrovat správnou sadu úchyty zařízení. Další informace najdete v tématu [Směrování a značky výrazů](notification-hubs-tags-segment-push-message.md). Šablony slouží k provádění transformací za registraci. Další informace najdete v tématu [šablony](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Instalace
Instalace je vylepšený registraci, která obsahuje od nabízených souvisejících vlastnosti. Je nejnovější a doporučené přístup k registraci svých zařízeních. Však nepodporuje na straně klienta .NET SDK ([SDK centrální oznámení pro back-end operace](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) u příležitosti ještě.  To znamená, že pokud registrujete z klientského počítače přímo, potřebujete použít přístup [Oznámení rozbočovače REST API](https://msdn.microsoft.com/library/mt621153.aspx) pro podporu zařízení. Pokud používáte službu back-end, by měl nebudou moct používat [SDK centrální oznámení pro operace back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Zde jsou některé klíčové výhody použití zařízení:

* Vytvoření nebo aktualizace instalace je plně idempotent. Tak můžete zopakovat bez obav duplicitní registrace.
* Model instalace snadno dělat jednotlivé posune - zacílení požadované zařízení. Systém značku **"$InstallationId: [installationId]"** automaticky přidají i s každé instalace na základě registrace. Tak můžete volat odeslat na tuto značku zaměřit na konkrétní zařízení aniž byste museli dělat jakékoliv další kódování.
* Pomocí zařízení také vám umožní provést aktualizace částečného registrace. Částečné aktualizace instalace žádá metodou oprava pomocí [Standardní JSON opravy](https://tools.ietf.org/html/rfc6902). Toto je užitečné, když chcete aktualizovat značky na registrace. Nemusíte rozbalovací celý registraci a pak znovu odeslat všechny předchozí značky.

Instalace může obsahovat následující vlastnosti. Kompletní seznam instalace vlastnosti najdete v článku, [Vytvoření nebo přepsat instalace pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) nebo [Vlastnosti instalace](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) .

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }

 

Je důležité mít na paměti, že registrace a zařízení ve výchozím nastavení už vypršení platnosti.

Registrace a zařízení musí obsahovat platnou ty PNS pro každý zařízení/kanál. Protože PNS úchyty můžete získat jenom v klientské aplikaci v zařízení, je jeden vzorek zaregistrovat přímo na zařízení s aplikací klienta. Na druhé straně otázky bezpečnosti a obchodní logiky související s značky může vyžadovat ke správě registrace zařízení v aplikaci back-end. 

#### <a name="templates"></a>Šablony

Pokud chcete používat [šablony](notification-hubs-templates-cross-platform-push-messages.md), zařízení podržte všechny šablony spojené s tímto zařízením JSON formátování (viz výše uvedené ukázkové). Názvy šablon pomáhají cílové různých šablon pro stejné zařízení.

Všimněte si, že každý název šablony mapy text šablony a volitelnou sadu značky. Každou platformu navíc může obsahovat další šablony vlastnosti. Pro Windows Store (pomocí WNS) a Windows Phone 8 (pomocí MPNS) další sady záhlaví může být součástí šablony. V případě APNs můžete nastavit vlastnost vypršení platnosti konstanta nebo výraz šablony. Kompletní seznam instalace vlastnosti tématu, [Vytvoření nebo přepsat instalací u ostatních](https://msdn.microsoft.com/library/azure/mt621153.aspx) .

#### <a name="secondary-tiles-for-windows-store-apps"></a>Sekundární dlaždice aplikace pro Windows Store

Pro klientské aplikace pro Windows Store odesílání oznámení na vedlejší dlaždice je stejná jako jim neposlat na primární. Podporuje se taky v zařízení. Všimněte si, že sekundárním dlaždice obsahuje jiné ChannelUri, která SDK na klientské aplikace zpracovává transparentně.

Slovník SecondaryTiles využívá stejné TileId, který slouží k vytvoření SecondaryTiles objektu v aplikaci Windows Store.
Stejně jako u primární ChannelUri můžete změnit ChannelUris sekundární dlaždic okamžiku. Pokud chcete zachovat zařízení v centru oznámení aktualizovat, musí zařízení obnovit je s aktuální ChannelUris sekundární dlaždice.


##<a name="registration-management-from-the-device"></a>Správa registrace ze zařízení

Při správě registrace zařízení klientských aplikacích, back-end odpovídá pouze pro posílání oznámení. Klient aplikace aktuálnost PNS úchytů a zaregistrovat značky. Následující obrázek znázorňuje tento vzorku.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Zařízení nejdřív zkopíruje úchyt PNS z PNS, a pak registruje centru oznámení přímo. Po úspěšném registrace back-end aplikací můžete poslat oznámení zacílení k registraci. Další informace o tom, jak poslat oznámení, najdete v článku [Směrování a značky výrazech](notification-hubs-tags-segment-push-message.md).
Všimněte si, že v takovém případě budete používat jenom přehrávat rights pro přístup k oznámení rozbočovače ze zařízení. Další informace najdete v tématu [zabezpečení](notification-hubs-push-notification-security.md).

Registrace ze zařízení je nejjednodušší způsob, ale má některé nevýhody.
První nevýhodou je, že v klientské aplikaci můžete aktualizovat pouze jeho značky Pokud aplikace nebude aktivní. Například pokud uživatele má dvě zařízení, které registrovat značky související s týmy sport, když první zařízení zaregistruje pro další značku (například Seahawks), druhé zařízení nebudete dostávat oznámení o Seahawks dokud aplikace na druhý zařízení se spustí podruhé. V obecnější rovině značek jsou ovlivněny několika zařízeních, správa značky z back-end při žádoucí možnost.
Druhá nevýhodou registrace správy z aplikace klienta je, protože aplikace můžete hacker, vyžaduje zabezpečení registrace k určité značky zvláštní pozornost způsobem popsaným v části "zabezpečení na úrovni značku."



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Příklad k registraci rozbočovači oznámení z zařízení instalace 

V současné době to je podporována pouze pomocí rozhraní [REST API rozbočovače oznámení](https://msdn.microsoft.com/library/mt621153.aspx).

Můžete taky použít metodu oprava pomocí [Standardní JSON oprava](https://tools.ietf.org/html/rfc6902) aktualizace instalace.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }

   

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Příklad k registraci rozbočovači oznámení ze zařízení, které používá registrace


Tyto metody vytvoření nebo aktualizace registrace pro zařízení, na kterém je nazýván. To znamená, že musí přepsat aktualizujte úchytu nebo značky celý registrace. Nezapomeňte, že jsou registrace přechodná, takže byste měli vždy mít spolehlivé úložiště s aktuální značky, které potřebuje určitému zařízení.


    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }
     
    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Správa registrace z back-end

Správa registrace z back-end vyžaduje psaní dalších kódu. Aplikace ze zařízení musí poskytovat aktualizované PNS zpracovat back-end při každém spuštění aplikace (spolu s značky a šablony) a back-end musíte aktualizovat tento úchyt v centru oznámení. Následující obrázek znázorňuje tento návrh.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Výhody Správa registrace z back-end patří možnost upravovat značky registrací i v případě odpovídající aplikaci na zařízení je aktivní a ověření aplikaci klienta před přidáním značky k zápisu.


#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Příklad k registraci rozbočovači oznámení z back-end instalace

Klientského zařízení pořád získává své PNS úchyt a vlastnosti příslušné instalace jako před a vlastní rozhraní API vyzývá back-end, které můžete provádět registraci a povolte značky apod. Back-end můžete využít [SDK centrální oznámení pro operace back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Můžete taky použít metodu oprava pomocí [Standardní JSON oprava](https://tools.ietf.org/html/rfc6902) aktualizace instalace.
 

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Příklad k registraci rozbočovači oznámení ze zařízení pomocí id registrace

Z vaší aplikaci back-end můžete provádět základní operace CRUDS na registrace. Příklad:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");
            
    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


Back-end musí zpracovat souběžné mezi registrace aktualizace. Služba Bus nabízí optimistické souběžné ovládací prvek pro správu registrace. Na úrovni HTTP to prováděné s použitím ETag na operace správy registrace. Tato funkce slouží transparentně SDKs Microsoft, který výjimku pokud aktualizace odmítnuté souběžné důvodů. Aplikace back-end zodpovídá za zpracování tyto výjimky a opakování aktualizace v případě potřeby.