> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Systém Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Tento článek poskytuje podrobný návod [ukázkový kód Hello World] [ lnk-helloworld-sample] pro ilustraci základních součástí sady [Azure IoT Gateway SDK] [ lnk-gateway-sdk] architektury. Vzorku SDK IoT Gateway rozbočovač používá k vytvoření jednoduché brány, která zaznamenává zprávy "hello world" do souboru každých pět sekund.

Tento návod zahrnuje:

- **Základní pojmy**: Přehled komponent, které tvoří všechny brány vytvořit v sadě IoT Gateway SDK.  
- **Dobrý den ukázka architektury**: Popisuje, jak použít pojmy v příkladu Hello World a jak tyto součásti spolupracují.
- **Jak sestavit ukázku**: kroky potřebné k vytvoření vzorku.
- **Jak spustit vzorku**: kroky potřebné ke spuštění vzorku. 
- **Příklad typického výstupu**: příklad výstupu lze očekávat při spuštění vzorku.
- **Výstřižky kódu**: kolekce fragmentů kódu, chcete-li zobrazit, jak Hello World vzorku implementuje klíče brány komponenty.

## <a name="azure-iot-gateway-sdk-concepts"></a>Koncepty služby Azure IoT Gateway SDK

Před prozkoumejte ukázkový kód nebo vytvořit vlastní pole gateway pomocí IoT Gateway SDK, měli byste pochopit klíčové pojmy, které jsou základem architektura sady SDK.

### <a name="modules"></a>Moduly

Vytvořením a kompletace *modulů*vytvoříte bránu pomocí sady Azure IoT Gateway SDK. Moduly slouží pro výměnu dat mezi sebou *zprávy* . Modul obdrží zprávu, provede určitou akci, volitelně transformuje do nové zprávy a pak publikuje pro ostatní moduly pro zpracování. Některé moduly mohou vytvořit pouze nové zprávy a nikdy zpracování příchozích zpráv. Řetěz modulů kanálu zpracování dat vytvoří ke každému modulu provádění transformace dat na jednom místě v tomto kanálu.

![Řetěz moduly vytvořené pomocí sady Azure IoT Gateway SDK brána][1]
 
Sada SDK obsahuje následující:

- Předepsaný moduly, které provádějí běžné funkce Brána.
- Rozhraní, které může vývojář použít k zápisu vlastní moduly.
- Infrastruktury, které jsou nutné k nasazení a spuštění sady modulů.

Sada SDK obsahuje vrstvu abstrakce, umožňující sestavit brány v různých operačních systémech a platformách.

![Vrstva HAL Azure IoT Gateway SDK][2]

### <a name="messages"></a>Zprávy

Pohodlný způsob, jak conceptualize, jak funkce brány sice přemýšlení o modulech předáním zprávy navzájem nevyjadřuje přesně co se stane. Moduly vzájemně komunikují pomocí broker, jejich publikování zpráv broker (bus, pubsub nebo libovolný vzor pro zasílání zpráv) a nechte broker směrování zpráv do připojených modulů.

Moduly pomocí funkce **Broker_Publish** má zprostředkovatel publikovat zprávu. Broker doručuje zprávy na modul vyvoláním funkce zpětného volání. Zpráva obsahuje sadu vlastností klíč/hodnota a obsah předán jako blok paměti.

![Role zprostředkovatele v Azure IoT Gateway SDK][3]

### <a name="message-routing-and-filtering"></a>Směrování zpráv a filtrování

Existují dva způsoby směrování zpráv do správné moduly. Sada odkazů může být předán broker má zprostředkovatel zná zdroje a jímky pro každý modul nebo modul lze filtrovat podle vlastnosti zprávy. Modul by měl jednat pouze na základě zprávy je-li zpráva určena pro ni. Propojení a filtrování zpráv je co skutečně vytváří zprávy kanálu.

## <a name="hello-world-sample-architecture"></a>Hello World ukázka architektury

Dobrý den Ukázka předvádí Principy popsané v předchozí části. Vítáme vás vzorku implementuje brána se skládá ze dvou modulů kanálu:

-   Modul *Dobrý den* vytvoří zprávu každých pět sekund a předá jej do modulu protokolování.
-   Modul *protokolování* zpráv, které obdrží zapíše do souboru.

![Architektura, vytvořené pomocí sady Azure IoT Gateway SDK vzorku Hello World][4]

Jak je popsáno v předchozí části, modul Hello World nepředává zprávy přímo do modulu protokolování každých pět sekund. Místo toho jej publikuje zprávu agenta každých pět sekund.

Protokolovacího modulu obdrží zprávu od agenta a funguje po jeho zápisu do souboru obsah zprávy.

Modul protokolování pouze spotřebovává zprávy od agenta, nikdy ji má zprostředkovatel publikuje nové zprávy.

![Jak broker směruje zprávy mezi moduly v Azure IoT Gateway SDK][5]

Výše uvedený obrázek znázorňuje architekturu vzorku Hello World a relativní cesty ke zdrojovým souborům, které implementují různé části vzorku v [úložišti][lnk-gateway-sdk]. Prozkoumat kód nebo fragmenty kódu níže použijte jako vodítko.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk