<properties
    pageTitle="Azure IoT zařízení SDK C - IoTHubClient | Microsoft Azure"
    description="Další informace o používání knihovny IoTHubClient v zařízení Azure IoT SDK c"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Zařízení Microsoft Azure IoT SDK c – Další informace o IoTHubClient

[První článek](iot-hub-device-sdk-c-intro.md) v této řadě zavádí **zařízení Microsoft Azure IoT SDK C**. Tento článek vysvětluje, SDK jsou dvě vrstvy architektury. Základem je **IoTHubClient** knihovny, které přímo spravuje komunikace s IoT centrální. Je také **serializer** knihovnu, která vytvoří v horní části daného pohybu serializace služeb. V tomto článku Nabízíme další podrobnosti o knihovně **IoTHubClient** .

Předchozí článek popisující knihovna **IoTHubClient** slouží k události k rozbočovači IoT odesílání a přijímání zpráv. Tento článek rozšiřuje této diskuse o vysvětlíme, jak přesněji spravovat *Při* odesílání a přijímání dat, úvod k rozhraní **API nižší úrovně**. Budeme se také popisují vlastnosti připojení k události (a načíst je ze zprávy) pomocí vlastnosti zpracování funkcí v knihovně **IoTHubClient** . Nakonec poskytneme další vysvětlení způsobů, jak řešit zprávy přijaté od IoT centrální.

V článku uzavře zahrnutím několik různých témata, včetně více o zařízení pověření a jak lze změnit chování **IoTHubClient** pomocí možnosti konfigurace.

Použijeme ukázky SDK **IoTHubClient** vysvětlit tato témata. Pokud chcete sledovat, přečtěte si téma **iothub\_klienta\_vzorku\_http** a **iothub\_klienta\_vzorku\_amqp** aplikace, které jsou součástí zařízení Azure IoT SDK pro C. všechno, co popsaných v následujících částech je znázorněn v těchto vzorcích.

Můžete najít **Azure IoT zařízení SDK C** v úložišti [Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) GitHub a zobrazit podrobnosti o rozhraní API v [rozhraní API odkaz](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-lower-level-apis"></a>Rozhraní API nižší úrovně

Předchozí článek popsané základní operace **IotHubClient** v rámci **iothub\_klienta\_vzorku\_amqp** aplikace. Například vysvětluje postup inicializace knihovny pomocí tento kód.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Také popisující odeslat události pomocí této funkce volání.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

V článku taky popisující přijímat zprávy zaregistrováním zpětné funkci.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

V článku taky ukázal, jak uvolnit zdrojů pomocí kódu následující.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Existuje ale funkce Průvodce vyhledáváním pro každou z těchto rozhraní API:

-   IoTHubClient\_l\_CreateFromConnectionString

-   IoTHubClient\_l\_SendEventAsync

-   IoTHubClient\_l\_SetMessageCallback

-   IoTHubClient\_l\_Destroy


Tyto funkce všechny zahrnout "L" název rozhraní API. Kromě toho jsou shodné s jejich protějšky l parametry každá z těchto funkcí. Chování těchto funkcí je však různých způsobů důležité.

Když voláte **IoTHubClient\_CreateFromConnectionString**, základní knihoven vytvořit nový podproces, který běží na pozadí. Tento podproces umožňuje události, volání a přijímání zpráv od IoT rozbočovače. Tyto vlákna se vytvoří při práci s rozhraní API "l". Vytvoření vlákna pozadí je snadno ovladatelné funkce pro vývojáře. Nemusíte bát explicitně odesílání událostí a přijímání zpráv od IoT rozbočovače – se to děje automaticky na pozadí. Naopak "L" rozhraní API umožňují explicitní publikum nemůže ovládat komunikace s IoT centrální pokud ho potřebujete.

Pokud chcete zjistit, tím lepší, Podívejme se na příklad:

Když voláte **IoTHubClient\_SendEventAsync**, co skutečně děláte umísťuje události v vyrovnávací paměť. Pozadí vlákna vytvořili při volání **IoTHubClient\_CreateFromConnectionString** automaticky sleduje Tato vyrovnávací paměť a odešle všechna data, která obsahuje IoT centrální. K tomu dojde na pozadí ve stejnou dobu, že hlavní podproces provádíte jinou práci.

Podobně při registraci funkci zpětného zprávy používající **IoTHubClient\_SetMessageCallback**, máte instrukce SDK mít pozadí vlákna vyvolání funkce zpětné po zprávy přijaté, nezávisle na hlavní podproces.

"L" rozhraní API nevytvoříte podprocesem na pozadí. Místo toho je nutné nového rozhraní API volat explicitně odesílat a přijímat data z centrální IoT. To je ukázáno v následujícím příkladu.

**Iothub\_klienta\_vzorku\_http** aplikace, která je součástí SDK ukazuje rozhraní API nižší úrovně. V tomto příkladu jsme odešlete události IoT centrální s kódem třeba takto:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

První tři řádky vytvoření zprávy a poslední řádek odešle událost. Ale jak jsme zmínili dříve, "odesílání" události znamená, že se data jednoduše umístí do vyrovnávací paměť. Nic jsou přenášena v síti při jsme volání **IoTHubClient\_l\_SendEventAsync**. Aby skutečně průniku data, která chcete IoT centrální, musíte zavolat **IoTHubClient\_l\_DoWork**, v tomto příkladu:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Tento kód (z **iothub\_klienta\_vzorku\_http** aplikace) opakovaně volá **IoTHubClient\_l\_DoWork**. Pokaždé, když **IoTHubClient\_l\_DoWork** je s názvem, odešle některé události z vyrovnávací paměť IoT centrální a načte ve frontě zprávu odesílaného do zařízení. Takovém případě znamená, že pokud jsme registrovaná zpětné funkce pro zprávy, pak zpětné vyvolání (za předpokladu, že všechny zprávy jsou ve frontě). By zaregistrovali zpětné funkci s kódem třeba takto:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Důvod, proč, **IoTHubClient\_l\_DoWork** je někdy označovány jako ve smyčce je to pokaždé, když je označená jako, odešle *některé* paměti události IoT centrální a načte *na další* zprávu ve frontě pro zařízení. Volání není možné zaručit odeslat všechny pamětí události nebo k načtení všech ve frontě zprávy. Pokud chcete odeslat všechny události vyrovnávací paměť a pokračujte na další zpracování můžete nahradit Tato smyčka s kódem třeba takto:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Tento kód volá **IoTHubClient\_l\_DoWork** dokud poslali všech událostí v vyrovnávací paměť IoT centrální. Poznámka: Toto taky neznamená, že všechny zprávy ve frontě byla přijata. Důvodem je, aby kontrola "všechny" zpráv není jako deterministický akce. Co se stane, když načtete "všechny" zprávy, ale odeslaný nějaký jiný zařízení bezprostředně za? Lepší způsob řešení, která je s naprogramované časový limit. Funkce zpětné zprávy například může obnovit časovače pokaždé, když je možné. Pak můžete psát logiky pokračovat ve zpracování Pokud například přijaty žádné zprávy v poslední *X* sekund.

Pokud jste dokončili ingressing události a přijímat zprávy, je potřeba volání příslušným funkce vyčištění zdroje.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Je v podstatě jenom jednu sadu rozhraní API pro odesílání a přijímání dat s podprocesem na pozadí a další dvojice rozhraní API, který dělá totéž bez vlákna pozadí. Hodně vývojáři chtít jiných - l rozhraní API, ale rozhraní API nižší úrovně jsou užitečné, pokud vývojář chce explicitní publikum nemůže ovládat sítě přenosy. Příklad některých zařízení shromažďovat data o přes čas a pouze průniku události v zadaných intervalech (například jednou za hodinu nebo jednou za den). Rozhraní API nižší úrovně umožnit můžete ovládacímu prvku explicitně při odesílání a příjem dat z centrální IoT. Ostatní se jednoduše raději zjednodušení poskytující rozhraní API nižší úrovně. Všechno, co se stane s hlavním vlákna spíše než některé později práce na pozadí.

Podle toho, která modelu jste si vybrali, ujistěte se, konzistentní v které rozhraní API používáte. Pokud spustíte tak, že zavoláte **IoTHubClient\_l\_CreateFromConnectionString**, ujistěte se, používáte odpovídající rozhraní API nižší úrovně pouze pro zpracování práce:

-   IoTHubClient\_l\_SendEventAsync

-   IoTHubClient\_l\_SetMessageCallback

-   IoTHubClient\_l\_Destroy

-   IoTHubClient\_l\_DoWork

Opakem platí taky. Pokud začnete s **IoTHubClient\_CreateFromConnectionString**, použít jiných - l rozhraní API pro další zpracování.

V zařízení Azure IoT SDK C, najdete v článku **iothub\_klienta\_vzorku\_http** aplikace kompletní příklad rozhraní API nižší úrovně. **Iothub\_klienta\_vzorku\_amqp** aplikace můžete odkázat úplné například jiných - l rozhraní API.

## <a name="property-handling"></a>Vlastnost zpracování

Pokud při Popsali jsme odesílání dat, jsme jste byla odkazující na textu zprávy. Například zkuste tento kód:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

V tomto příkladu odešle zprávu IoT centrální s textem "Vítáme." Rozbočovač IoT však umožňuje vlastnosti připojí se k každé zprávy. Vlastnosti jsou název/dvojice připojené k zprávu. Například jsme můžete upravit kód předchozí připojit vlastnost na zprávu:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Začneme tak, že zavoláte **IoTHubMessage\_vlastnosti** a předáním úchyt naše zprávy. Co doporučujeme vrátit se **MAPY\_ZPRACOVAT** odkaz, který umožňuje nám do ní začít přidávat vlastnosti. Lze provést voláním **mapy\_AddOrUpdate**, který má odkaz na mapu\_úchyt, název vlastnosti a hodnotu. Pomocí této rozhraní API jsme můžete přidat tolik vlastnosti jsme zachce.

Přečtení událost z **Rozbočovače události**můžete příjemce umožňuje zobrazit výčet vlastností a načíst odpovídající hodnoty. Například v .NET to chcete provést přistupovat ke [kolekci vlastnosti objektu EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

V předchozím příkladu jsme jste připojení k události, která chcete IoT centrální odeslat vlastnosti. Vlastnosti může být připojen také zprávy přijaté od IoT centrální. Pokud nám chcete načíst vlastnosti ze zprávy, můžete použít kód například následující v naší funkce zpětné zprávy:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Volání **IoTHubMessage\_vlastnosti** vrátí **MAPY\_ZPRACOVAT** odkaz. Jsme předejte tento odkaz na **mapy\_GetInternals** získat odkaz na pole název/dvojice (stejně jako počet vlastnosti). V tomto okamžiku je jednoduchá věc výčtu vlastností pro přístup k hodnoty, které chceme.

Nemusíte použít vlastnosti v aplikaci. Ale pokud potřebujete nastavit o událostech nebo načtení ze zprávy, **IoTHubClient** knihovna usnadňuje.

## <a name="message-handling"></a>Zpracování zpráv

Jak je uvedeno v předchozích verzích při příchodu zprávy od IoT rozbočovače knihovnu **IoTHubClient** odpoví vyvoláním registrovaných zpětné funkce. Je zpáteční parametr tuto funkci, která si zaslouží některé další vysvětlení. Tady je výpisem funkce zpětné v **iothub\_klienta\_vzorku\_http** vzorové aplikace:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Všimněte si, že je vráceným typem **IOTHUBMESSAGE\_DISPOZICE\_výsledek** a v tomto případě jsme vrátit **IOTHUBMESSAGE\_PŘIJATÉ**. Existují další hodnoty, které můžete vrácených z funkce, které se mění jak knihovnu **IoTHubClient** reagovat na zpětné zprávy. Tady jsou možnosti.

-   **IOTHUBMESSAGE\_PŘIJATÉ** – úspěšně zpracovat zprávu. Knihovna **IoTHubClient** nebude vyvolání funkce zpětné znovu se bude stejná zpráva.

-   **IOTHUBMESSAGE\_odmítnuto** – zpráva nebyla zpracována a že bez přání k tomu nevyzve v budoucnu. Knihovna **IoTHubClient** neměli vyvolání funkce zpětné znovu se bude stejná zpráva.

-   **IOTHUBMESSAGE\_ABANDONED** – zpráva nebyla zpracována úspěšně, ale knihovnu **IoTHubClient** měli vyvolat funkce zpětné znovu se bude stejná zpráva.

První dvě return kódy knihovnu **IoTHubClient** odešle zprávu IoT centrální označující, že zprávu odstraní ze zařízení fronty a není Doručená znovu. Čistý efekt odpovídá (zprávu se odstraní ze zařízení fronty), ale zda bylo zdroji přijaté nebo odmítnuté zprávy stále nastavené.  Nahrávání tento rozdíl je užitečná odesílatelům zprávy, kteří můžou k získání názorů zjistit, pokud je zařízení přijetí nebo odmítnutí určitou zprávu.

V posledním případě zprávy odešlou se zároveň i k rozbočovači IoT, ale označuje, že zpráva znovu dodány. Budete obvykle opustit zprávy, pokud dojde k některé chyby, ale chcete zkusit zpracuje ji znovu. Naopak odmítnutí zprávy je vhodné když dojde k chybě obnovit (nebo v případě jednoduše zjistíte, že nechcete zpracování zprávy).

V každém případě znát různé zpáteční kódy tak, aby vyvolat chování, které chcete z knihovny **IoTHubClient** .

## <a name="alternate-device-credentials"></a>Přihlašovací údaje alternativní zařízení

Jak je popsáno dříve, první věc, kterou se má stát při práci s knihovnou **IoTHubClient** je získat **IOTHUB\_klienta\_ZPRACOVAT** hovoru například následující:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Argumenty **IoTHubClient\_CreateFromConnectionString** jsou připojovací řetězec naše zařízení a parametr, který označuje protokol používáme komunikovat s IoT centrální. Připojovací řetězec má formát, který se zobrazí takto:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Existují čtyři druhy údajů do tohoto řetězce: IoT centrální název, IoT centrální příponu, ID zařízení a sdílené přístupové klávesy. Získání plně kvalifikovaný název domény (FQDN) IoT rozbočovače při vytváření instanci aplikace IoT centrální Azure portálu – to vám názvu centrální IoT (první část FQDN) nebo centrální příponu IoT (zbytek FQDN). Při registraci zařízení IoT centrální (podle popisu v [Předchozí článek](iot-hub-device-sdk-c-intro.md)) získat ID zařízení a přístupové klávesy sdílené.

**IoTHubClient\_CreateFromConnectionString** umožňuje jedním ze způsobů inicializace knihovnu. Pokud chcete, můžete vytvořit nový **IOTHUB\_klienta\_ZPRACOVAT** pomocí těchto jednotlivé parametry spíše než připojovací řetězec. To je dosáhnout pomocí následující kód:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Tím se musí splnit totéž jako **IoTHubClient\_CreateFromConnectionString**.

To může zdát zřejmé, zda chcete použít **IoTHubClient\_CreateFromConnectionString** místo takto podrobnější inicializace. Mějte na paměti, že registrace zařízení v centrální IoT se zobrazí při ID zařízení a zařízení klíč (ne připojovací řetězec). Nástroj **Správce zařízení** SDK zavedená v [Předchozí článek](iot-hub-device-sdk-c-intro.md) používá knihovny **služby Azure IoT SDK** vytvoření připojovacího řetězce z zařízení ID, klíče zařízení a IoT centrální název hostitele. Tak volání **IoTHubClient\_l\_vytvořit** může být vhodnější, protože uloží krok generování připojovací řetězec. Použijte bez ohledu na způsob je vhodný.

## <a name="configuration-options"></a>Konfigurace možností

Pokud všechno popsané o způsobu práce knihovnu **IoTHubClient** odráží jeho výchozí chování. Můžou ale nastat několik možností, které můžete nastavit, aby změnit knihovnu. To je provést využívání **IoTHubClient\_l\_SetOption** rozhraní API. Zvažte v tomto příkladu:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Existuje několik možností, které se běžně používají:

-   **SetBatching** (logická hodnota) – Pokud **platí**, a pak data odeslaná IoT centrální odeslaný po dávkách. Pokud **false**a potom zprávy se odesílají jednotlivě. Výchozí hodnota je **false**. Všimněte si, že možnost **SetBatching** platí jenom protokolu HTTP a pozor, abyste MQTT nebo AMQP protokoly.

-   **Časový limit** (nepodepsaný int) – Tato hodnota představuje v milisekundách. Při odesílání trvá déle než tentokrát žádost HTTP nebo zvednete odpověď a potom časový limit připojení.

Možnost dávkování je důležité. Ve výchozím nastavení událostí ingresses knihovny jednotlivě (jedné události je něco jiného, co předáte **IoTHubClient\_l\_SendEventAsync**). Při **splnění**dávkování možnost knihovnu shromažďuje tolik události, jak můžete z vyrovnávací paměť (až maximální velikost zprávy, která přijímá IoT centrální).  Dávky událost jsou odeslány IoT centrální HTTP telefonuje jediný (jednotlivé události jsou seskupeny do pole JSON) Povolení dávky obvykle za následek velký výkon vzhledem k tomu, že jste snížení opakované sítě. Protože posíláte jedna skupina záhlaví HTTP s listem událostí a ne sadu záhlaví pro každou jednotlivé událost také významně snižuje šířky pásma. Pokud máte konkrétní důvod, proč se jinak, obvykle je vhodné povolit dávky.

## <a name="next-steps"></a>Další kroky

Tento článek popisuje podrobně chování knihovnu **IoTHubClient** zjištěným v **Azure IoT zařízení SDK C**. Tyto informace byste si měli dobře porozumět výhod knihovnu **IoTHubClient** . [Další článek](iot-hub-device-sdk-c-serializer.md) obsahuje podobné podrobností u této knihovny **serializer** .

Další informace o vytváření pro centrální IoT najdete v tématu [IoT centrální SDK][lnk-sdks].

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Napodobení zařízení s SDK IoT brány][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
