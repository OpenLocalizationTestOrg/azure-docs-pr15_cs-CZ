<properties
    pageTitle="Azure IoT zařízení SDK C - Serializer | Microsoft Azure"
    description="Další informace o používání knihovny Serializer v zařízení Azure IoT SDK c"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-serializer"></a>Microsoft Azure IoT zařízení SDK C – Další informace o převodník do sériového tvaru

[První článek](iot-hub-device-sdk-c-intro.md) v této řadě zavedená **Azure IoT zařízení SDK C**. Další článek k dispozici podrobnější popis [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md). Tento článek dokončí pokrytí SDK zadáním podrobnější popis zbývající komponenty: **serializer** knihovny.

Úvodní článek popisující knihovna **serializer** slouží k události odesílání a přijímání zpráv od IoT rozbočovače. V tomto článku rozšíříme jednání zadáním podrobnější vysvětlení způsobu k modelování dat pomocí jazyka **serializer** makra. V článku taky obsahuje další podrobnosti o knihovně jak jsou zprávy (a v některých případech, jak můžete nastavit chování serializace). Budete taky popisu některé parametry, které můžete upravit, jež určují velikost modely, které vytvoříte.

Nakonec v článku znovu navštíví některé témata v předchozí článcích například zprávy a zpracování vlastnost. Jako jsme budete najdete tyto věcí funguje stejně jako pomocí knihovny **serializer** stejně jako s knihovnou **IoTHubClient** .

Všechno, co popisované v tomto článku je založena na vzorky SDK **serializer** . Pokud chcete sledovat, podívejte se **simplesample\_amqp** a **simplesample\_http** aplikace zahrnuté v zařízení Azure IoT SDK pro C.

Můžete najít **Azure IoT zařízení SDK C** v úložišti [Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) GitHub a zobrazit podrobnosti o rozhraní API v [rozhraní API odkaz](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-modeling-language"></a>Jazykové modelování

[Úvodní článek](iot-hub-device-sdk-c-intro.md) v této řadě zavádí **Azure IoT zařízení SDK C** modelování jazyk prostřednictvím v příkladu v **simplesample\_amqp** aplikace:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Jak vidíte, jazykové modelování vychází z C makra. Je třeba nejprve definice s **POČÁTEČNÍHO\_obor názvů** a vždy konci **END\_obor názvů**. Je běžné pojmenování oboru ve svojí společnosti nebo jako v následujícím příkladu projekt, který právě pracujete.

Co přejde do obor názvů jsou definice modelu. V tomto případě je jediný model pro anemometer. Ještě jednou modelu můžete pojmenovat, ale obvykle to jmenuje pro zařízení nebo typu dat, která chcete exchange s IoT centrální.  

Modely obsahují definici události můžete provést tyto akce průniku k rozbočovači IoT ( *data*) i zprávy, které dostáváte od IoT centrální ( *Akce*). Jak vidíte v příkladu, events have jsou typem a název; Akce mít jméno a volitelné parametry (oba objekty mají typu).

Co není uvedeno v tomto příkladu jsou další datové typy podporované v SDK. Budeme se zabývat těmito oblastmi, které další.

> [AZURE.NOTE] Rozbočovač IoT odkazuje na data, která zařízení odešle jako *události*, zatímco jazykové modelování na něj odkazuje jako *data* (definované pomocí **WITH_DATA**). Podobně IoT centrální odkazuje na data, která odešlete zařízení jako *zprávy*, zatímco jazykové modelování na něj odkazuje jako *Akce* (definované pomocí **WITH_ACTION**). Mějte na paměti, že tyto termíny lze místo v tomto článku.

### <a name="supported-data-types"></a>Podporované datové typy

Modely vytvořili s knihovnou **serializer** podporuje následující datové typy:

| Typ                    | Popis                            |
|-------------------------|----------------------------------------|
| dvojité                  | nastavení dvojitých přesnost číslo s plovoucí desetinnou |
| Funkce INT                     | 32bitové celé číslo                         |
| uvolnit                   | číslo s plovoucí desetinnou jednoho přesností |
| dlouhé                    | dlouhé celé číslo                           |
| int8\_t                 | 8 bitů celé číslo                          |
| Int16\_t                | 16bitovou celočíselnou                         |
| Int32\_t                | 32bitové celé číslo                         |
| Int64\_t                | celé číslo 64-bit                         |
| Logická hodnota                    | Logická hodnota                                |
| ASCII\_znak\_ptr        | Řetězec znaků ASCII                           |
| EDM\_datum\_čas\_posun | Datum časový posun                       |
| EDM\_GUID               | IDENTIFIKÁTOR GUID                                   |
| EDM\_binární             | binární.                                 |
| DEKLAROVAT\_struktury         | komplexní datový typ                      |

Začněme s posledního datového typu. **DECLARE\_struktury** umožňuje definovat typy komplexních dat, které jsou seskupení další základní typy. Tato seskupení umožňují definovat modelu, který vypadá takto:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Náš model obsahuje událost nastavit jako jednoho datového typu **TestType**. **TestType** je komplexní typ, který obsahuje několik členy, které souhrnně ukazují základní typy podporované v aplikaci **serializer** modelování jazyk.

S modelem takto jsme napsali kód odeslání dat do centrální IoT, která se zobrazí následujícím způsobem:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

V podstatě jste přiřazení hodnoty pro každý člen struktuře **Test** jsme volání **SendAsync** odeslat události **Test** dat do cloudu. **SendAsync** je pomocná funkce, která odešle událost nastavit jako jednu datovou IoT centrální:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Tato funkce jsou události dané dat a odešle IoT centrální pomocí **IoTHubClient\_SendEventAsync**. Toto je stejný kód popisované v předchozí články (**SendAsync** zapouzdřuje logiky do pohodlný funkci).

Jednu funkci helper použili v předchozím kódu je **GetDateTimeOffset**. Tato funkce převede dané čas na hodnotu typu **EDM\_datum\_čas\_posun**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Pokud spustíte tento kód, následující zpráva se pošle IoT centrální:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Všimněte si, že serializace JSON, což je generováno aplikací knihovnu **serializer** formát. Navíc nezapomeňte, že každý člen sériové objekt JSON shoduje členy **TestType** , která byla definována v naší modelu. Hodnoty taky přesně odpovídají používána v kódu. Však Všimněte si, že binární data kódováním Base 64: "AQID" je ve formátu Base 64 kódování {0x01, 0x02, 0x03}.

Tento příklad ukazuje výhodou používání knihovnu **serializer** – umožňuje nám chcete poslat JSON do cloudu, aniž byste museli explicitně zacházet s serializace v aplikaci. Všechny, které máme starat o to je nastavení hodnot události datového v naší model a potom volání jednoduché rozhraní API odesílat tyto události do cloudu.

Tyto informace jsme můžete definovat modely, které obsahují oblast podporované datové typy včetně komplexní (jsme může i zahrnout složité typů v rámci jiné složité typů). Však mu serializován JSON generovaných z příkladu výše se vyvolá důležitá. *Jak* pošleme dat s knihovnou **serializer** určuje přesně jak vytvořené ve formátu JSON. Určitém místě je co budeme se zabývat těmito oblastmi Další.

## <a name="more-about-serialization"></a>Další informace o serializace

V předchozí části zvýrazní příkladem výstup generovaný knihovnou **serializer** . V této části jsme budete popisují, jak knihovnu jsou data a jak můžete určit toto chování pomocí serializace rozhraní API.

Pokud chcete další informace o serializace, budete Pracujeme s nový model podle termostatu. Nejdřív si na scénář jsme se snažíte adresa zadejte některé základní informace.

Chceme model termostatu zobrazující teploty a vlhkosti. Každou část dat bude předán IoT centrální jinak. Ve výchozím nastavení ingresses termostatu událost nastavit jako teploty každých 2 minut; událost nastavit jako vlhkost je ingressed každých 15 minut. Po ingressed buď události musí obsahovat časového razítka, který zobrazuje dobu, aby byl měří odpovídající teploty nebo vlhkost.

Možnost Tento scénář, jsme budete ukazují dvěma různými způsoby pro modelování dat a jsme budete vysvětlování efekt, že modelování má sériové výstup.

### <a name="model-1"></a>Model 1

Tady je první verzi modelu, který podporuje předchozí situaci:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Všimněte si, že modelu zahrnuje dva události datového: **teplotní** a **vlhkostní**. Na rozdíl od předchozích příkladů je typu každou událost struktuře definované pomocí **DECLARE\_struktury**. **TemperatureEvent** obsahuje měření teploty a časové razítko; **HumidityEvent** obsahuje vlhkost měření a časové razítko. Tento model dostaneme přirozený způsob modelování dat pro scénář jsme je popsali výše. Při pošleme událost do cloudu, doporučujeme buď uživateli odešleme teploty/časové razítko nebo pár vlhkost/časové razítko.

Pošleme můžete událost nastavit jako teploty do cloudu pomocí kódu třeba takto:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Doporučujeme používat pevně hodnoty pro teploty a vlhkosti v ukázkovém kódu, ale Představte si, že jsme vyhledáváte skutečně data tyto hodnoty tak, že odběr odpovídající senzory na termostatu.

Kód výše používá **GetDateTimeOffset** Pomocník, která byla zavedená dříve. Z důvodu, které budou zrušte zaškrtnutí políčka novější Tento kód explicitně odděluje úkolu serializaci a odesílání události. Předchozí kód jsou teploty událostí do vyrovnávací paměť. Potom **sendMessage** je pomocná funkce (součástí **simplesample\_amqp**), která odešle událost IoT centrální:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Tento kód tvoří podmnožinu helper **SendAsync** popisu v předchozí části, proto jsme nebude přejděte nad ním znovu sem.

Při jsme spustíte kód předchozí odeslat události teploty tento sériové formulář události se pošle k rozbočovači IoT:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Jsme odesíláte teplotu, která je typu **TemperatureEvent** a tuto strukturu obsahuje členem **teploty** a **času** . Přímo se projeví v sériové data.

Podobně pošleme můžete událost nastavit jako vlhkost s Tento kód:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Sériové formulář, který se neodesílají IoT centrální vypadat takto:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

To je opět podle očekávání.

V tomto modelu můžete představit jak další události můžete snadno přidat. Definování další struktury pomocí **DECLARE\_struktury**a obsahovat odpovídající událost do modelu pomocí **s\_dat**.

Teď Pojďme změnit model tak, aby zahrnovala stejná data, ale s jinou strukturu.

### <a name="model-2"></a>Model 2

Zvažte toto alternativní model výše uvedené:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

V tomto případě jsme vyloučili **DECLARE\_struktury** makra a jednoduše definování položky dat z našich scénáře použití jednoduchého typů z jazykové modelování.

Pouze v dané chvíli Pojďme Ignorovat **časové** události. S zrušení tady je kód průniku **teplotní**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Tento kód odešle následující sériové událost IoT centrální:

```
{"Temperature":75}
```

A kód pro odesílání události vlhkost vypadat takto:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Tento kód tyto údaje odešle IoT centrální:

```
{"Humidity":45}
```

Pokud pořád existují žádné překvapení. Teď Pojďme změnit, jak používáme SERIALIZOVATELNOU makra.

Makro **SERIALIZOVATELNOU** může trvat několik události datového jako argumenty. Abychom vám serializovat **teplotní** a **vlhkostní** akce společně a odešlete je IoT centrální v jednom volání umožňuje:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Může uhodnout, že tento kód vyhodnotí odeslání dvěma datovými události k rozbočovači IoT:

[

{"Teploty": 75},

{"Vlhkost": 45}

]

Jinými slovy může očekávat, že tento kód je stejná jako odesílání **teploty** a **vlhkosti** samostatně. Je právě pohodlí předat obou událostí **SERIALIZOVATELNOU** ve stejném volání. Však, který není v případě. Místo toho kódu výše rozešle tato událost jednu datovou IoT centrální:

{"Teploty": 75, "vlhkost": 45}

To může zdát nestandardní, protože našeho modelu definuje **teploty** a **vlhkosti** jako dvou *samostatných* událostí:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Další do místa, jsme neměli model tyto události, kde jsou **teploty** a **vlhkost** ve stejné struktuře:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Pokud použijeme tento model, bude možné snadněji pochopit, jak bude ve stejné sériové zprávě odesláno **teplotní** a **vlhkostní** . Však nemusí být zřejmé, proč to funguje tak předáním obou události datového **SERIALIZOVATELNOU** pomocí modelu 2.

Toto chování se snadněji pochopíte, pokud znáte předpoklady díky knihovně **serializer** . Abyste měli představu o to přejděte zpět do našeho modelu:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Přemýšlejte tento model řečeno objektově orientovaného. V tomto případě jsme jste modelování fyzické zařízení (termostatu) a toto zařízení zahrnuje atributy jako **teploty** a **vlhkosti**.

Pošleme celý stav našeho modelu s kódem třeba takto:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Za předpokladu, že hodnoty teplotu, vlhkost a čas je nastaveno najdete v článku jsme by události takto poslané na IoT centrální:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Někdy může jenom chcete odeslat *některé* vlastnosti modelu s cloudem (to platí zejména pokud model obsahuje velké množství dat událostí). Slouží k odeslání jen podmnožinu dat události, jako je třeba v našem příkladu starší:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Tím se vytvoří přesně stejné sériové událost jako kdybyste jsme měli tato pole definovány **TemperatureEvent** členem **teploty** a **času** , stejně jako jsme postupovala modelu 1. V tomto případě jsme našli generovat přesně stejné sériové událost pomocí různých modelu (model 2), protože jsme s názvem **SERIALIZOVATELNOU** jiným způsobem.

Důležité je, že pokud předáte více události datového **SERIALIZOVATELNOU,** pak předpokládá, že je vlastnost v na jeden objekt JSON každou událost.

Nejlepším řešením záleží na tom je a jak si myslíte o modelu. Pokud chcete odeslat "události" do cloudu a každou událost obsahuje definovaný sadu vlastností, první přístup je hodně smysl. V takovém případě použijete **DECLARE\_struktury** definovat strukturu každou událost a pak je zahrnout do modelu pomocí **s\_dat** makro. Můžete odešlete každou událost podle kroků uvedených v předchozím příkladu první. V tomto přístupu by pouze předáte událost nastavit jako jednu datovou **SERIALIZER**.

Pokud si myslíte o modelu způsobem objektově orientovaného, pak druhý přístup mohou vám vyhovoval. V tomto případě prvky definované pomocí **s\_dat** jsou "vlastnosti" objektu. Předáte jakéhokoliv podmnožinu události **SERIALIZOVATELNOU** , který se vám líbí, podle toho, kolik "objektu" státu, kterým chcete poslat do cloudu.

Nether přístup je doprava nebo nesprávné. Jenom Uvědomte si, jak funguje knihovnu **serializer** a vyberte přístup modelování, které nejlépe odpovídá vašim potřebám.

## <a name="message-handling"></a>Zpracování zpráv

Tento článek zatím obsahuje pouze popisované odesílání události IoT centrální a nebyla adresovaná přijímat zprávy. Důvodem je, potřebujeme vědět o přijímat zprávy má převážně byla podle [předchozích článek](iot-hub-device-sdk-c-intro.md). Odvolání od tohoto článku zpracování zpráv zaregistrováním funkci zpětné zprávy:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Zpětné funkci, která je vyvolat po přijetí zprávy napište:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Tato implementace **IoTHubMessage** hovorů funkce specifické pro každou akci ve vašem modelu. Pokud například modelu definuje tato akce:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Je třeba definovat funkce se tento podpis:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** pak nazývá při zprávy poslané na vaše zařízení.

Co jsme nebyly vysvětlení zatím je vypadá sériové verze zprávy. Jinými slovy Pokud chcete odeslat zprávu **SetAirResistance** do zařízení, jak takový, který vypadá?

Pokud chcete odeslat zprávu se zařízením, lze provést pomocí služby Azure IoT SDK. Potřebujete vědět, jaké řetězec odešlete vyvolat určitou akci. Pomocí formátu Obecný pro odeslání zprávy se zobrazí následující:

```
{"Name" : "", "Parameters" : "" }
```

Posíláte sériové JSON objekt s dvě vlastnosti: **název** je název akce (zprávy) a **Parametry** obsahuje parametry tuto akci.

Například vyvolat **SetAirResistance** můžete odeslat tuto zprávu na zařízení:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Název akce se musí přesně shodovat akce definované ve vašem modelu. Názvy parametrů musí odpovídat stejně. Všimněte si také, malá a velká písmena. **Název** a **parametrů** jsou vždy velká písmena. Zkontrolujte, že malá a velká písmena název akce a parametrů ve vašem modelu. V tomto příkladu je název akce "SetAirResistance" a ne "setairresistance".

V této části popsány všechno, co byste měli vědět, kdy událostí odesílání a přijímání zpráv s knihovnou **serializer** . Teprve pak přejděte na Podívejme vysvětlovat některé parametry, které můžete konfigurovat, kterými se řídí velikost je modelu.

## <a name="macro-configuration"></a>Konfigurace makra

Pokud používáte knihovnu **Serializer** důležitou součástí SDK nějaká nachází v knihovně azure-c sdílené – nástroj.
Pokud máte klonovat úložišti Azure iot SDK z GitHub pomocí možnosti – rekurzivní, zjistíte této sdílené nástroje knihovny:

```
.\\c\\azure-c-shared-utility
```

Pokud ještě klonovat knihovnu, najdete ji [zde](https://github.com/Azure/azure-c-shared-utility).

V knihovně Sdílené nástroj bude hledat následující složce:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Tato složka obsahuje Visual Studio řešení s názvem **makra\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Program v tomto řešení generuje **makra\_utils.h** soubor. Je výchozí makra\_utils.h soubor zahrnutý v sadě SDK. Toto řešení umožňuje změnit některé parametry a pak znovu vytvořte soubor záhlaví na základě těchto parametrů.

Jsou dva klíčové parametry se to týká se **nArithmetic** a **nMacroParameters** , které jsou definované v těchto dvou řádků najdete v makru\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Tyto hodnoty jsou výchozí hodnoty parametrů zahrnutý v sadě SDK. Každý parametr obsahuje následující význam:

-   nMacroParameters – Určuje, kolik parametry můžete definovat v jedné DECLARE\_definice makra modelu.

-   nArithmetic – mezery nastavuje celkový počet členů povolené v modelu.

Důvod, proč se tyto parametry důležité totiž budou určit, jak velký může být modelu. Například zvažte toto definice modelu:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Jak jsme zmínili dříve, **DECLARE\_modelu** je právě C makro. Název modelu a **s\_dat** údajů (ještě jiného makra) jsou parametry **DECLARE\_modelu**. **nMacroParameters** definuje, kolik parametry mohou být součástí **DECLARE\_modelu**. Efektivní definuje kolik data události a akce deklarace můžete mít. Jako takové s výchozí limit 124 to znamená, že můžete definovat pomocí kombinace asi 60 akce a události datového modelu. Pokud se pokusíte překročení omezení, dostanete kompilátoru chyby, které vypadají nějak takto:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

Parametr **nArithmetic** najdete další informace o vnitřní fungování makra jazyka než aplikace.  Určuje celkový počet členů, ke kterým máte ve vašem modelu, včetně **DECLARE_STRUCT** makra. Pokud začnete zobrazují chyby kompilátoru například to měli zkusit, zvýšení **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Pokud chcete změnit tyto parametry, změňte hodnoty v makru\_utils.tt souboru, překompilovat makro\_utils\_h\_generator.sln řešení a spustit zkompilovaný program. Pokud to chcete provést, vytvořte nové makro\_utils.h soubor je generováno a umístí do. \\běžné\\Inc. adresář.

Pokud chcete použít novou verzi makra\_utils.h, odeberte balíček NuGet **serializer** z vašeho řešení a místo něj obsahovat projekt aplikace Visual Studio **serializer** . Díky kódu sestavíte proti zdrojového kódu serializer knihovny. Jedná se o aktualizované makra\_utils.h. Pokud chcete udělat u **simplesample\_amqp**, začněte tím, že odeberete NuGet balíček pro knihovnu serializer řešení:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Potom přidejte tohoto projektu do vašeho řešení Visual Studio:

> . \\c\\serializer\\sestavit\\windows\\serializer.vcxproj

Až budete hotovi, řešení by měl vypadat takto:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Teď když kompilaci vašeho řešení aktualizované makra\_utils.h je součástí vašeho binární.

Všimněte si, že zvětšení tyto hodnoty dostatečně vysoké, můžete přesáhnout kompilátoru omezení. K tomuto bodu **nMacroParameters** je hlavní parametr, se kterým se to týká. Specifikace C99 Určuje, že aspoň 127 parametrů jsou povoleny definice makra. Microsoft kompilátoru následuje použití přesně (a může obsahovat maximálně 127), takže je nebudete moct zvětšit **nMacroParameters** za výchozí. Další kompilátoru by mohly umožnit posílání vám to (například kompilátoru GNU podporuje vyšší mezní hodnoty).

Pokud jsme jste nichž se uplatní prakticky všechno, co byste měli vědět o tom, jak kódu s knihovnou **serializer** . Před uzavírání návštěvě Řekněme pár témat z předchozí článků, které mohou být chcete vědět o.

## <a name="the-lower-level-apis"></a>Rozhraní API nižší úrovně

Ukázka aplikace dnem zaměření v tomto článku je **simplesample\_amqp**. V tomto příkladu vyšší úrovni (jiných-"l") rozhraní API pro události odesílání a přijímání zpráv. Pokud používáte těchto rozhraní API, podprocesem na pozadí spustí, který má na starosti jak události odesílání a přijímání zpráv. Rozhraní API nižší úrovně (l) však může použít k odstranění podprocesu pozadí a využít explicitní publikum nemůže ovládat při události odesílání a přijímání zpráv z cloudu.

Podle popisu v [Předchozí článek](iot-hub-device-sdk-c-iothubclient.md), je sada funkcí tvořená vyšší úrovni rozhraní API:

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_Destroy

Tato rozhraní API je znázorněn v **simplesample\_amqp**.

Je také podobnou stránce sadu rozhraní API nižší úrovně.

-   IoTHubClient\_l\_CreateFromConnectionString

-   IoTHubClient\_l\_SendEventAsync

-   IoTHubClient\_l\_SetMessageCallback

-   IoTHubClient\_l\_Destroy

Všimněte si, že rozhraní API nižší úrovně fungovat přesně stejným způsobem podle popisu v předchozí články. Pokud chcete pozadí vlákna zpracovávání událostí odesílání a přijímání zpráv můžete použít první sadě rozhraní API. Můžete použít druhý rozhraní API požadovaná explicitní publikum nemůže ovládat při odesílání a příjem dat z centrální IoT. Buď sadu rozhraní API práce s knihovnou **serializer** stejně.

Příklad použití rozhraní API nižší úrovně s knihovnou **serializer** naleznete v tématu **simplesample\_http** aplikace.

## <a name="additional-topics"></a>Další témata

Několik dalších tématech jmění zmínění znovu jsou vlastnost manipulace, pomocí přihlašovacích údajů alternativní zařízení a možnosti konfigurace. Toto jsou všechny témata v [Předchozí článek](iot-hub-device-sdk-c-iothubclient.md). Hlavní je všechny tyto funkce stejným způsobem jako s knihovnou **serializer** funguje jako s knihovnou **IoTHubClient** . Například, pokud chcete připojit vlastnosti na událost z modelu, používáte **IoTHubMessage\_vlastnosti** a **mapu**\_**AddorUpdate**, stejným způsobem, jak popsáno dříve:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Zda byla události generované knihovnu **serializer** nebo vytvořit ručně pomocí knihovnu **IoTHubClient** nezáleží.

Alternativní zařízení v části přihlašovací údaje pomocí **IoTHubClient\_l\_vytvořit** funguje jenom stejně jako **IoTHubClient\_CreateFromConnectionString** přidělování **IOTHUB\_klienta\_ZPRACOVAT**.

Nakonec, pokud používáte knihovnu **serializer** , můžete nastavit možnosti konfigurace s **IoTHubClient\_l\_SetOption** stejně jako při použití knihovnu **IoTHubClient** .

Funkce, které jsou jedinečné pro knihovnu **serializer** jsou inicializace rozhraní API. Aby mohli začít pracovat s knihovnou, musíte volat **serializer\_inicializace**:

```
serializer_init(NULL);
```

Důvodem je těsně před voláním **IoTHubClient\_CreateFromConnectionString**.

Podobně po dokončení práce s knihovnou, poslední volání usnadníte slouží k **serializer\_deinit**:

```
serializer_deinit();
```

V opačném všechny ostatní funkce výše uvedené funguje stejně, v knihovně **serializer** stejně jako v knihovně **IoTHubClient** . Další informace o některé z těchto témat najdete v tématu [Předchozí článek](iot-hub-device-sdk-c-iothubclient.md) v této řadě.

## <a name="next-steps"></a>Další kroky

Tento článek popisuje podrobně jedinečné aspekty knihovnu **serializer** obsažené v **Azure IoT zařízení SDK C**. S informacemi o za předpokladu, že byste měli mít dobré Principy používání modely události odesílat a přijímat zprávy od IoT rozbočovače.

Tímto taky dokončíte řady tří částí týkající se zabývají vývojem aplikací s **Azure IoT zařízení SDK C**. Je vhodné dost informací nejen vám usnadní zahájení práce, ale vám důkladné pochopení fungování rozhraní API. Další informace najdete pár ukázek v jsou SDK tady není uvedené. V opačném [si přečtěte následující dokumentaci SDK](https://github.com/Azure/azure-iot-sdks) je užitečný zdroj pro další informace.


Další informace o vytváření pro centrální IoT najdete v tématu [IoT centrální SDK][lnk-sdks].

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Napodobení zařízení s SDK IoT brány][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
