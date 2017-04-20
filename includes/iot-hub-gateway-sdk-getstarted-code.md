## <a name="typical-output"></a>Příklad typického výstupu

Následuje příklad výstupu zapsána do souboru protokolu ve vzorku Hello World. Byly přidány znaky nového řádku a karty pro čitelnost:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Fragmenty kódu

Tato část popisuje některé klíčové části kód v ukázce Hello World.

### <a name="gateway-creation"></a>Vytvoření brány

Vývojář musí zapsat *proces brány*. Tento program vytvoří vnitřní infrastruktury (VP), načte moduly a nastaví vše fungovat správně. Sada SDK obsahuje funkce **Gateway_Create_From_JSON** povolit bránu z JSON soubor bootstrap. Použití funkce **Gateway_Create_From_JSON** musí předáte cestu souboru JSON, který určuje načíst moduly. 

Kód můžete najít bránu procesu v příkladu Hello World v [main.c] [ lnk-main-c] souboru. Pro čitelnost je fragment kódu zobrazí zkrácenou verzi kód procesu brány. Tento program vytvoří bránu a pak čeká uživatele dříve, než jej strhne brány, stiskněte klávesu **ENTER** . 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

Soubor nastavení JSON obsahuje seznam modulů, které chcete načíst. Každý modul je nutné zadat a:

- **module_name**: jedinečný název pro modul.
- **module_path**: cesta do knihovny obsahující modul. Linux soubor .so, v systému Windows je soubor DLL.
- **argumenty**: všechny informace o konfiguraci modulu potřebuje.

JSON soubor obsahuje také propojení mezi moduly, které budou předány broker. Odkaz má dvě vlastnosti:
- **zdroj**: název modulu z `modules` oddílu, nebo "\*".
- **umyvadlo**: název modulu z `modules` část

Každé propojení určuje směrování a směr. Zprávy z modulu `source` mají být doručeny do modulu `sink`. `source` Může být nastavena na "\*", označující zpráv z libovolného modulu budou přijímány `sink`.

Následující příklad ukazuje soubor JSON nastavení slouží ke konfiguraci ukázkové Vítáme na platformě Linux. Všechny zprávy vytvořené modul `hello_world` bude potřeba pomocí modulu `logger`. Zda je modul vyžaduje že argument závisí na návrhu modulu. V tomto příkladu protokolovacího modulu argument, což je cesta k výstupnímu souboru a modul Hello World nepřijímá žádné argumenty:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Hello World modul zpráva publikování

Můžete vyhledat kód používaný modulem "hello world" v ["hello_world.c"] publikovat zprávy[ lnk-helloworld-c] souboru. Následující fragment kódu ukazuje upravenou verzí s další komentáře a některé odebrána pro čitelnost kódu pro zpracování chyb:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Zpracování zpráv modulu Hello World

Modul Dobrý den nikdy musí zpracovat všechny zprávy, které má zprostředkovatel publikovat další moduly. Díky provádění zpětné volání zprávy Hello World modulu funkce č op.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Zpráva modulu protokolování publikování a zpracování

Protokolovacího modulu přijímá zprávy od agenta a zapíše je do souboru. Publikuje nikdy žádné zprávy. Kód modulu protokolování proto nikdy volá funkci **Broker_Publish** .

Funkce **Logger_Recieve** v [logger.c] [ lnk-logger-c] soubor je zpětné volání, vyvolá broker pro doručování zpráv do modulu protokolování. Následující fragment kódu ukazuje upravenou verzí s další komentáře a některé odebrána pro čitelnost kódu pro zpracování chyb:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Další kroky

Další informace o použití sady IoT Gateway SDK, naleznete v následujících tématech:

- [IoT Gateway SDK – odesílání zpráv zařízení cloud s simulované zařízení pomocí Linux][lnk-gateway-simulated].
- [Azure IoT Gateway SDK] [ lnk-gateway-sdk] na GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md