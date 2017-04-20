> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [Systém Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

Tento návod [simulované zařízení Cloud uložit vzorek] ukazuje, jak používat [Azure IoT Gateway SDK] [ lnk-sdk] k odeslání telemetrické zařízení cloud k rozbočovači IoT ze simulovaných zařízení.

Tento návod zahrnuje:

1. **Architektura**: důležité architektonické informace o vzorku simulované odesílání cloudu zařízení.

2. **Sestavení a spuštění**: kroky potřebné k vytvoření a spuštění vzorku.

## <a name="architecture"></a>Architektura

Simulované zařízení Cloud uložit vzorek ukazuje, jak pomocí sady SDK můžete vytvořit bránu, která odešle telemetrické ze simulovaných zařízení k rozbočovači IoT. Simulované zařízení nemůže připojit přímo k rozbočovači IoT, protože:

- Zařízení nepoužívejte komunikační protokol posádka IoT rozbočovač.
- Zařízení nejsou dostatečně chytrý, aby zapamatování identity přiřazeny pomocí rozbočovače IoT.

Brána řeší tyto problémy pro simulované zařízení následujícími způsoby:

- Brána chápe protokol používaný simulované zařízení obdrží od zařízení telemetrické zařízení cloud a předává tyto zprávy k rozbočovači IoT pomocí protokolu srozumitelné rozbočovač.
- Brána ukládá jménem simulované zařízení IoT rozbočovač identit a funguje jako server proxy při simulovaném zařízení IoT rozbočovač odesílat zprávy.

Následující diagram znázorňuje hlavní součástí vzorku, včetně modulů brány:

![][1]


> [AZURE.NOTE] Moduly neprojde zprávy k sobě navzájem. Moduly publikovat zprávy vnitřní broker, který doručuje zprávy do jiných modulů pomocí předplatného mechanismus, jak je znázorněno na obrázku níže. Další informace naleznete v tématu [Začínáme s IoT Gateway SDK][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Protokol modulu požití

Tento modul je výchozím bodem pro získávání dat ze zařízení, prostřednictvím brány a do cloudu. Ve vzorku modul provádí čtyři úlohy:

1.  Vytvoří simulované teploty.
    
    Poznámka: Pokud jste používali skutečné zařízení, modul by číst data z těchto fyzických zařízení.

2.  Simulované teploty data umístí do obsahu zprávy.

3.  Přidá vlastnost s falešnou adresu MAC na zprávu, která obsahuje data o teplotách simulované.

4.  Je zpráva k dispozici další modul v řetězci.

> [AZURE.NOTE] Modul nazývá **protokol X požití** v diagram uvedený výše se nazývá **simulované zařízení** ve zdrojovém kódu.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt; - &gt; modul rozbočovače ID IoT

Tento modul slouží k vyhledání zprávy, které obsahují vlastnost, která obsahuje adresu MAC, přidal modulu protokolu požití v simulovaném zařízení. Zjistí-li modul takové vlastnosti, přidá další vlastnost s klíčem zařízení IoT rozbočovač ke zprávě a pak zpřístupní zprávy Další modul v řetězci. To je jak vzorek přidruží identit zařízení IoT rozbočovač simulovaném zařízení. Vývojář vytvoří mapování mezi rozbočovači IoT identity a adresy MAC ručně jako součást konfigurace modulu. 

> [AZURE.NOTE]  Tato ukázka používá adresu MAC jako identifikátor jedinečný zařízení a koreluje s identitou, zařízení IoT rozbočovač. Nicméně můžete napsat vlastní modul, který používá jiný jedinečný identifikátor. Například může mít zařízení s jednoznačná sériová čísla nebo data telemetrické, která má název UDN jej, který můžete použít k určení identity zařízení IoT rozbočovač.

### <a name="iot-hub-communication-module"></a>Modul komunikace rozbočovače IoT

Tento modul má zprávy s rozbočovači IoT zařízení identit přiřazováni předchozího modulu a odešle obsah zprávy IoT rozbočovači pomocí protokolu HTTP. HTTP je jedním ze tří protokolů posádka IoT rozbočovač.

Tento modul neotevře připojení k rozbočovači IoT pro každé simulované zařízení otevře jedno připojení HTTP z brány k rozbočovači IoT a multiplexes připojení z simulované zařízení prostřednictvím tohoto připojení. Díky tomu jedna brána připojit mnoho více zařízení, simulované nebo jinak, než by bylo možné, pokud je otevřen pro každé zařízení jedinečné připojení.

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Simulované vzorku odeslat zařízení Cloud]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md