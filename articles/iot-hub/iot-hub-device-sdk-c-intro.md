<properties
    pageTitle="Pomocí zařízení Azure IoT SDK pro C | Microsoft Azure"
    description="Další informace o a Začínáme pracovat s ukázkový kód v zařízení Azure IoT SDK pro C."
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

# <a name="introducing-the-azure-iot-device-sdk-for-c"></a>Úvodní informace o zařízení Azure IoT SDK c

**Azure IoT zařízení SDK** je sada knihoven navržený zjednodušit proces událostí odesílání a přijímání zpráv ze služby **Azure IoT centrální** . Existují různé varianty SDK každý zacílení konkrétní platformu, ale tento článek popisuje **Azure IoT zařízení SDK C**.

Azure IoT zařízení SDK c je napsané v jazyce C ANSI (C99) maximalizovat přenositelnost. To je vhodné pracovat na celou řadu platformy a zařízení, zejména pokud minimalizace disku a paměti stopu je prioritu.  

Existuje širokou škálu platformy, na kterých SDK otestování (podívejte se [Azure certifikované pro katalog zařízení IoT](https://catalog.azureiotsuite.com/) podrobnosti). I když tento článek obsahuje návody ukázek kódu spuštěných pro platformu Windows, mějte na paměti, že kód popisované v tomto článku se přes oblast podporované platformy přesně shodují.

V tomto článku se budete směřující k architektura zařízení Azure IoT SDK pro C. Budeme se ukazuje, jak lze inicializace knihovnu zařízení, odešlete události IoT centrální i přijímat zprávy z něho. Informace v tomto článku by měl být tak, abyste mohli začít používat v SDK, ale také poskytuje odkazy na další informace o knihovnách.

## <a name="sdk-architecture"></a>Architektura SDK

Můžete najít **Azure IoT zařízení SDK C** v úložišti [Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) GitHub a zobrazit podrobnosti o rozhraní API v [rozhraní API odkaz](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

Nejnovější verzi knihoven najdete v **hlavní** větev tohoto úložiště:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

Toto úložiště obsahuje celou rodinu Azure IoT zařízení SDK. Tento článek je však o zařízení Azure IoT SDK *c* kterou najdete ve složce **c** .

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* Provádění základních SDK najdete v **iothub\_klienta** složky, která obsahuje provádění nejnižší vrstvy rozhraní API v SDK: Knihovna **IoTHubClient** . Knihovna **IoTHubClient** obsahuje rozhraní API implementace jako nezpracovaná zasílání zpráv pro posílání zpráv do IoT centrální i přijímat zprávy od něj. Pokud chcete použít tuto knihovnu, jste zodpovědní za provádění serializace zprávy (nakonec pomocí výběru serializer píše níže), ale další podrobnosti o komunikaci s IoT centrální fungují za vás.
* Složka **serializer** obsahuje podporu funkcí a ukázky znázorňující, jak serializovat data před odesláním centrální IoT Azure pomocí klienta knihovny. Všimněte si, že využívání serializátoru není povinná a pouze k dispozici pro potřeby. Pokud používáte knihovnu **serializer** , začněte definování modelu, který určuje události, které chcete odeslat IoT centrální, jakož i zprávy, které můžete očekávat z něho. Po definování modelu SDK poskytuje rozhraní API povrchu, který umožňuje snadno pracovat s událostmi a zprávy nemusíte bát serializace podrobnosti.
Knihovnu závisí na jiných knihoven otevřít zdroj implementující přenos pomocí několika protokoly (MQTT, AMQP).
* Knihovna **IoTHubClient** závisí na jiných knihoven otevřít zdroj:
   * [Azure C sdílí nástroje](https://github.com/Azure/azure-c-shared-utility) knihovny, která obsahuje běžné funkce pro základní úlohy (jako jsou řetězce, seznam manipulace, vstupu a výstupu, atd....) potřeby napříč několika souvisejících s Azure C SDK
   * [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) knihovny, která je klienta straně provádění AMQP optimalizované pro zařízení omezení zdrojů.
   * [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) knihovny, která je obecný knihovna provádění protokolu MQTT a optimalizované pro zařízení omezení zdrojů.

To všechno je snadněji pochopitelný pohledem příklad. V následujících částech vás provede procesem několik ukázkové aplikace, které jsou součástí SDK. To vám měl dát dobré chování pro různé funkce architektonické vrstvy SDK, jakož i úvodní informace o fungování rozhraní API.

## <a name="before-running-the-samples"></a>Před spuštěním vzorky

Před spuštěním vzorky v zařízení Azure IoT SDK c je třeba pokud už nechcete mít jednu a dokončení úkolů, 2 vytvořit instanci služby Azure předplatné:
* Příprava vývojové prostředí
* Získejte oprávnění zařízení.

Pokud je potřeba vytvořit instance Azure IoT centrální předplatného Azure, postupujte podle pokynů [v tomto poli](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md).

[Soubor Readme pro](https://github.com/Azure/azure-iot-sdks/tree/master/c) zahrnutý v sadě SDK obsahuje pokyny pro přípravu vývojové prostředí a získejte oprávnění zařízení.
Následující části obsahují některé další komentářem podle těchto pokynů.

### <a name="preparing-your-development-environment"></a>Příprava vývojové prostředí

Během balíčků jsou k dispozici pro některé platformy (například NuGet pro Windows nebo apt_get Debian a systémem Ubuntu) a vzorků použít tyto balíčky-li k dispozici, pod pokyny k vytváření knihovnu podrobností a vzorků přímo formulář kód.

Musíte nejprve získat kopii v SDK z GitHub a pak vytvoříte zdroj. Z **hlavního** větve [GitHub úložiště](https://github.com/Azure/azure-iot-sdks)mají dostat kopii zdroji.

Po stažení kopie zdroje musíte nejdřív udělat podle kroků popsaných v článku SDK ["Připravit vývojové prostředí"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md).


Následuje několik tipů postupujte podle kroků popsaných v příručce pro přípravu:

-   Při instalaci nástroj **CMake** vyberte možnost Přidat **CMake** systému cesty pro **všechny uživatele** (přidání do **aktuálního uživatele** funguje taky):

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   Před otevřením **Vývojář příkazového řádku pro VS2015**nainstalujte nástroje libovolná příkazového řádku. Při instalaci těchto nástrojích, postupujte podle těchto kroků:

    1. Spusťte instalační program **Visual Studio 2015** (nebo zvolte **Microsoft Visual Studio 2015** z ovládací panel **programy a funkce** a vyberte **změnit**).
    
    2. Ujistěte se, funkci **Libovolná pro systém Windows** je vybraná v instalační program, ale můžete chtít taky zaškrtnout možnost **GitHub rozšíření Visual Studia** poskytují integrace integrovaném vývojovém prostředí:

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. Dokončete Průvodce nastavením instalace nástrojů.

    4. Přidáte adresáři libovolná nástroje **Koš** **prostředí systému Path** . Ve Windows to vypadá takto:

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


Po dokončení všech kroků popsaných v stránce ["Připravit vývojové prostředí"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) budete chtít kompilaci vzorku.

### <a name="obtaining-device-credentials"></a>Získání pověření zařízení

Vývojové prostředí nastavenou, dalším krokem je na sadu přihlašovacích údajů zařízení.  Pro zařízení pro přístup centrální IoT musíte napřed přidat zařízení registru IoT Centrum zařízení. Když přidáte zařízení dostanete sadu přihlašovacích údajů zařízení, které budete potřebovat, aby zařízení, aby se mohli připojit k rozbočovači IoT. Ukázkové aplikace, které podíváme na v následující části očekávají, že tyto údaje ve formuláři **zařízení připojovací řetězec**.

Existuje pár nástroje v úložišti SDK otevřít zdroj usnadní správu centru IoT. Reprodukujte aplikace s názvem zařízení Průzkumníku druhý je node.js na základě křížové platformy nástroj rozhraní příkazového řádku s názvem iothub Průzkumníka Windows. Další informace o těchto nástrojích [tady](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md).

Jak budeme prostřednictvím vzorky v systému Windows v tomto článku, používáme nástroji Explorer zařízení. Ale můžete také iothub explorer podle potřeby nástroje rozhraní příkazového řádku.

Nástroj [Explorer zařízení](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) používá knihovnu služby Azure IoT provádět různé funkce v centrální IoT, včetně přidání zařízení. Pokud používáte zařízení Explorer přidání zařízení, zobrazí se odpovídající připojovací řetězec. Je třeba tento připojovací řetězec, aby vzorku spuštění aplikace.

V případě, že nejste seznámit s procesem, následující postup popisuje pomocí Průzkumníka zařízení přidání zařízení a získání připojovacího řetězce zařízení.

O nástroji Explorer zařízení na [SDK uvolněte stránky](https://github.com/Azure/azure-iot-sdks/releases)najdete Instalační služby systému Windows. Ale můžete taky spustit nástroj přímo z jeho kódu otevřením **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** ve **Visual Studiu 2015** a vytváření řešení.

Při spuštění programu uvidíte rozhraní:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Zadejte **IoT centrální připojovacího řetězce** v prvním poli a klikněte na **Aktualizovat**. To tak, aby mohli komunikovat s IoT centrální nakonfiguruje nástroj.

Po konfiguraci připojovací řetězec IoT centrální klikněte na kartu **Správa** :

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Je to, kde budete spravovat zařízení registraci v centrální IoT.

Zařízení můžete vytvořit kliknutím na tlačítko **vytvořit** . Zobrazí se dialogové okno sadou předem vyplněné klíčů (primární a sekundární). Je potřeba udělat stačí zadat **ID zařízení** a pak klikněte na **vytvořit**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Po vytvoření zařízení seznam zařízení se aktualizuje se všemi registrovaných zařízeními, včetně databáze, kterou jste právě vytvořili. Pokud jste klikněte pravým tlačítkem na nové zařízení, uvidíte tuto nabídku:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Pokud vyberete možnost **zkopírujte připojovací řetězec pro vybrané zařízení** , připojovací řetězec pro zařízení s zkopírovali do schránky. Zkopírujte připojovací řetězec. Musíte to při spuštění aplikace ukázkové popsané v dalších částech.

Po provedení těchto kroků budete chtít spuštění některé kód. Obě vzorky musí konstanty v horní části hlavního zdrojového souboru, který umožňuje zadat připojovací řetězec. Například odpovídajících řádků z **iothub\_klienta\_vzorku\_amqp** aplikace vypadat takto.

```
static const char* connectionString = "[device connection string]";
```

Pokud chcete sledovat, zadat připojovací řetězec zařízení tady, překompilovat řešení a budete moct spouštět vzorku.

## <a name="iothubclient"></a>IoTHubClient

V rámci **iothub\_klienta** složek v úložišti azure iot SDK je složka **Ukázky** , která obsahuje aplikaci s názvem **iothub\_klienta\_vzorku\_amqp**.

Verze Windows **iothub\_klienta\_vzorku\_ampq** aplikace obsahuje následující řešení Visual Studio:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

Toto řešení obsahuje jeden projekt. Je vhodné poznamenat, že jsou čtyři NuGet balíčků nainstalovaný v tomto řešení:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

Vždy musíte balíček **Microsoft.Azure.C.SharedUtility** při práci s SDK. Protože v tomto příkladu závisí na AMQP, musí obsahovat také **Microsoft.Azure.uamqp** a **Microsoft.Azure.IoTHub.AmqpTransport** balíčků (neobsahuje odpovídající balíčků MQTT a HTTP). Protože vzorku používá knihovnu **IoTHubClient** , musí obsahovat balíček **Microsoft.Azure.IoTHub.IoTHubClient** také v řešení.

Můžete najít implementaci pro ukázkové aplikaci **iothub\_klienta\_vzorku\_amqp.c** zdrojový soubor.

Použijeme Tato ukázková aplikace vás provede co musí používat knihovnu **IoTHubClient** .

### <a name="initializing-the-library"></a>Inicializace knihovny

> [AZURE.NOTE] Než začnete pracovat s knihoven, budete muset provést některé konkrétní inicializace platformu. Pokud budete chtít použít AMQP na Linux například musí inicializace OpenSSL knihovny. Příklady v [úložišti GitHub](https://github.com/Azure/azure-iot-sdks) volání funkce nástroj **platform_init** klienta zahájení a volat **platform_deinit** před ukončením. Tyto funkce jsou deklarovány v souboru záhlaví "platform.h". Měli byste prozkoumat definice tyto funkce pro svoji platformu cílového [úložiště](https://github.com/Azure/azure-iot-sdks) a zjistit, jestli potřebujete zahrnout jakýkoli kód inicializace platformy ve vašem klientovi.

Začít pracovat s knihoven musíte nejdřív přidělit rutiny klienta IoT centrální:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Všimněte si, že jsme jste předávání kopii naše zařízení připojovací řetězec touto funkcí (jiný, který jsme získat z Průzkumníka zařízení). Jsme také určit protokol, který chcete použít. Tento příklad používá AMQP, ale MQTT a HTTP se taky jednu z možností.

Když máte platný **IOTHUB\_klienta\_ZPRACOVAT**, můžete začít volání rozhraní API pro události odesílání a přijímání zpráv od IoT rozbočovače. Podíváme na tuto další.

### <a name="sending-events"></a>Odeslání událostí

Odeslání události IoT centrální vyžaduje, proveďte následující kroky:

Nejprve vytvořte zprávu:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

Odešlete zprávu:

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Pokaždé, když odesíláte zprávy, zadejte odkaz na zpětné funkci, která je vyvolat při odesílání dat:

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Poznámka: volání **IoTHubMessage\_Destroy** fungovat až to budete mít zprávou. Musíte podat volání uvolnit zdroje přiřazené při vytvoření zprávy.

### <a name="receiving-messages"></a>Přijímání zpráv

Přijetí zprávy je asynchronní operace. Nejdřív se zaregistrujete zpětné chcete uplatnit zařízení přijaté zprávy:

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Poslední parametr je neplatný ukazatel na něco jiného, co chcete. Ve vzorku se ukazatel myši na celé číslo, ale může to být ukazatel složitější datová struktura. Díky funkci zpětné při práci s sdílených stavu s volajícím začnete této funkce.

Zprávy přijaté zařízení vyvolání funkce registrovaných zpětné:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Všimněte si, že používáte **IoTHubMessage\_GetByteArray** funkce vyhledat zprávy, která v tomto příkladu je řetězec.

### <a name="uninitializing-the-library"></a>Rušení inicializace knihovny

Až budete hotovi, událostí odesílání a přijímání zpráv, můžete inicializaci knihovnu IoT. Postup problému následující funkce hovoru:

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Tohle systémové prostředky dříve přidělené **IoTHubClient\_CreateFromConnectionString** (funkce).

Jak vidíte, není těžké si události odesílání a přijímání zpráv s knihovnou **IoTHubClient** . Knihovnu zpracovává informace komunikovat s IoT centrální, včetně protokol, který chcete použít (z hlediska vývojář, tady je možnost jednoduchý konfigurace).

Knihovna **IoTHubClient** také poskytuje přesné publikum nemůže ovládat postup serializovat události, které zařízení odešle IoT centrální. V některých případech je výhodu, ale v ostatních případech je ke které nechcete, aby se to týká podrobností implementace. Pokud to je případ, zvažte použití **serializer** knihovny, které jsme budete v následující části popisují.

## <a name="serializer"></a>Převodník do sériového tvaru

Entity knihovnu **serializer** nachází nad knihovnu **IoTHubClient** v SDK. Používá knihovnu **IoTHubClient** pro základní komunikace s IoT centrální, ale přidá možnosti modelování umožňující odebrání zatížení se zabývá serializace zprávy od vývojáře. Jak funguje to knihovně je nejlepší prokázat příklad.

V rámci **serializer** složka v úložišti azure iot SDK je **Ukázky** , která obsahuje aplikace s názvem **simplesample\_amqp**. Verze Windows v tomto příkladu obsahuje následující řešení Visual Studio:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

Stejně jako v předchozí ukázka tuto položku obsahuje několik balíčky NuGet:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

Jsme viděli většina z nich v předchozím příkladu, ale **Microsoft.Azure.IoTHub.Serializer** je nového. Při je to potřeba používáme **serializer** knihovny.

Můžete najít provádění ukázková aplikace v **simplesample\_amqp.c** soubor.

V následujících částech vás provede procesem klíčové části v tomto příkladu.

### <a name="initializing-the-library"></a>Inicializace knihovny

Pokud chcete začít pracovat s knihovnou **serializer** , musíte volat inicializace rozhraní API:

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

Volání **serializer\_inicializace** funkce jednorázové volání a slouží inicializace základní knihovny. Pak, zavolejte **IoTHubClient\_CreateFromConnectionString** funkci, která je se stejným rozhraním API jako **IoTHubClient** vzorku. Volání nastaví zařízení připojovací řetězec (Toto je také kde zvolte protokol chcete použít). Všimněte si, že tento příklad používá AMQP jako přenos, ale může HTTP.

Nakonec zavolejte **vytvořit\_modelu\_INSTANCE** (funkce). Upozorňujeme, že **WeatherStation** oboru modelu a **ContosoAnemometer** je název modelu. Po vytvoření instance modelu můžete začít odesílat události a přijímat zprávy. Je ale důležité porozumět tomu, jaký model.

### <a name="defining-the-model"></a>Definování modelu

Model v knihovně **serializer** definuje události, které zařízení můžete poslat IoT centrální a příslušné zprávy s názvem *Akce* v jazyce modelování, který může přijímat. Definování modelu pomocí sady C makra ve **simplesample\_amqp** vzorové aplikace:

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

**Začít\_obor názvů** a **END\_obor názvů** makra obou trvat názvů modelu jako argument. Očekává se, něco mezi tyto makra je definici vaše modely a datové struktury, které používají modely.

V tomto příkladu je jediný model s názvem **ContosoAnemometer**. Tento model definuje dvě události, které zařízení můžete poslat IoT centrální: **DeviceId** a **rychlost větru**. Definuje také tři akce (zprávy), které můžete zobrazit zařízení: **TurnFanOn** **TurnFanOff**a **SetAirResistance**. Každou událost s typem a každou akci má název (a volitelně sadu parametrů).

Události a akce definováno v modelu, definujte rozhraní API povrchu, který můžete poslat IoT centrální události, stejně jako odpovídání na zprávy poslané na zařízení. Rozumí se nejlíp v příkladu.

### <a name="sending-events"></a>Odeslání událostí

Model definuje události, které můžete poslat IoT centrální. V tomto příkladu, to znamená, jednu ze dvou událostí definované pomocí makra **WITH_DATA** . Například pokud chcete událost nastavit jako **rychlost větru** odešlete rozbočovači IoT, existuje několik kroků zahrnuté do vytváření to. Prvním je nastavte data, která nám chcete poslat:

```
myWeather->WindSpeed = 15;
```

Model, která byla definována dříve umožňuje abychom vám to provést nastavením členem **Struktura**. Dále jsme serializovat událost, kterou nám chcete poslat:

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

Tento kód jsou události vyrovnávací paměť (odkazováno **cíl**). Nakonec se pošleme událost k rozbočovači IoT s Tento kód:

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

Toto je pomocná funkce v ukázkové aplikace, která odešle naše sériové události IoT centrální:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

Tento kód je velmi podobné co jsme viděli v **iothub\_klienta\_vzorku\_amqp** aplikace, ve kterém jsme vytvořili zprávu z pole bajtů a pak použít **IoTHubClient\_SendEventAsync** odešlete IoT centrální. Až to jenom mít uvolnit úchyt zprávy jsme serializován vyrovnávací paměť jsme dříve rozdělení.

Druhé poslední parametru **IoTHubClient\_SendEventAsync** odkaz na zpětné funkci, která je místo toho, kdy byla úspěšně odeslána data. Tady je příklad zpětné funkci:

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

Druhý parametr je ukazatel kontext uživatele; stejný ukazatel jsme předaný **IoTHubClient\_SendEventAsync**. V tomto případě kontext je jednoduchý čítač, ale může být všechno, co, který chcete.

To je vše, existuje pro odeslání událostí. Jediné vlevo pokrýval se dozvíte, jak přijímat zprávy.

### <a name="receiving-messages"></a>Přijímání zpráv

Přijetí zprávy funguje podobně ke zprávám způsob, jak pracovat v knihovně **IoTHubClient** . Nejdřív registrace funkce zpětné zprávy:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Pak můžete napsat zpětné funkci, která vyvolání po přijetí zprávy:

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

Tento kód je často používaný – to je stejný pro žádné řešení. Tato funkce, zobrazí se zpráva a stará o směrování do odpovídající funkce prostřednictvím volání **spouštět\_příkaz**. Funkce jen v tomto okamžiku závisí na tom definici akcí v naší modelu.

Když definujete akce ve vašem modelu, budete vyzváni k provádění funkci, která se nazývá zařízení obdrží odpovídající zprávu. Pokud například modelu definuje tato akce:

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

Všimněte si, že název funkce shoduje s názvem akce v modelu a parametrů funkce splňují zadané parametry akce. První parametr požaduje vždy a obsahuje ukazatel instanci našeho modelu.

Když zařízení se zobrazuje zpráva, která odpovídá podpisu, je místo toho odpovídající funkce. Kromě museli zadat kód často používaný z **IoTHubMessage**, přijímat zprávy tedy jenom předmětem definování jednoduché funkce pro každou akci definované ve vašem modelu.

### <a name="uninitializing-the-library"></a>Rušení inicializace knihovny

Až budete hotovi odesílání dat a přijímání zpráv, můžete inicializaci IoT knihovny:

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Každá z těchto tří funkcí zarovnán tři inicializační funkce popsané výše. Volající tato rozhraní API zaručuje uvolnit dříve přiřazené zdroje.

## <a name="next-steps"></a>Další kroky

Tento článek se vztahuje základní informace o použití knihoven v **Azure IoT zařízení SDK C**. Je součástí dost informací chcete porozumět tomu, co je součástí SDK, jeho architektura a jak můžete začít pracovat s příklady Windows. Další článek pokračuje popis SDK tím, že vysvětlí [Další informace o knihovně IoTHubClient](iot-hub-device-sdk-c-iothubclient.md).

Další informace o vytváření pro centrální IoT najdete v tématu [IoT centrální SDK][lnk-sdks].

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Napodobení zařízení s SDK IoT brány][lnk-gateway]


[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
