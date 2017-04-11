<properties
 pageTitle="Karta Vývojář Průvodce – Principy twins zařízení | Microsoft Azure"
 description="Azure IoT centrální vývojář Průvodce - twins zařízení použít k synchronizaci stav a konfigurace data mezi IoT centrální a zařízeních"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="understand-device-twins---preview"></a>Princip twins zařízení: zobrazení náhledu

## <a name="overview"></a>Základní informace

*Twins zařízení* jsou JSON dokumenty, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky). Rozbočovač IoT zůstává v dvojitých zařízení pro každé zařízení, které jste připojení k centrální IoT. Tento článek popisuje:

* Struktura dvojitých zařízení: *značky*, *požadované* a *vykázaného vlastnosti*, a
* operace, které zařízení aplikace a zpět skončí můžete provádět na twins zařízení.

> [AZURE.NOTE] V tomto okamžiku jsou dostupné pouze z zařízení připojujících se k rozbočovači IoT pomocí protokolu MQTT twins zařízení. Získáte [podporují MQTT] [ lnk-devguide-mqtt] článku najdete pokyny, jak chcete převést existující zařízení aplikaci používat MQTT.

### <a name="when-to-use"></a>Kdy použít

Použijte twins zařízení na:

* Obsahují zařízení konkrétní metadata v cloudu, například umístění nasazení prodejní počítače.
* Aktuální informace o stavu například dostupné možnosti a podmínky z aplikace zařízení, například připojení přes Wi-Fi nebo mobilní zařízení.
* Synchronizujte stav dlouhé spuštěné pracovní postupy mezi zařízení aplikaci a zpět konce, například back-end určující novou verzi firmwaru nainstalovat a zařízení aplikaci vykazování různé fáze procesu aktualizace.
* Dotaz zařízení metadata, konfigurace nebo stavu.

Pomocí [zařízení do cloudu zpráv] [ lnk-d2c] pro posloupnosti označen časovým razítkem událostí například časového úseku data snímače nebo upozornění. Použití [metod cloudu zařízení] [ lnk-methods] pro interaktivní ovládací prvek zařízení, třeba na ventilátory.

## <a name="device-twins"></a>Twins zařízení

Zařízení twins obsahují informace týkající se zařízení, která:

- Zařízení a zpět konce můžete použít k synchronizaci zařízení podmínky a konfigurace.
- Aplikace back-end slouží k dotazu a cílové dlouho probíhajících operace.

Životní cyklus dvojitých zařízení je propojená s odpovídajícím [zařízení identit][lnk-identity]. Twins je implicitně vytvořena a odstraňovat nové identity zařízení vytvořila se nebo odstraní v centrální IoT.

Dvojité zařízení je JSON dokument, který obsahuje:

* **Značky**. Čtení a který napsal back-end dokument JSON. Značky nejsou viditelné pro zařízení aplikace.
* **Požadovaná vlastnosti**. Použití s nahlášeného vlastnosti synchronizace konfigurace zařízení nebo podmínka. Požadované vlastnosti lze nastavit pouze aplikací zpět ukončit a můžete číst pomocí zařízení. Zařízení aplikaci můžete taky upozornění můžete v reálném čase změn na požadované vlastnosti.
* **Ohlášeno vlastnosti**. Použití s požadované vlastnosti synchronizace konfigurace zařízení nebo podmínka. Nahlášeného vlastnosti můžete nastavit jenom pomocí zařízení můžou být číst a zpracovávat pomocí aplikace back-end.

Navíc kořenovém dvojitých zařízení obsahuje vlastnosti jen pro čtení z odpovídající identitu jako obsažené v [zařízení identity registru][lnk-identity].

![][img-twin]

Tady je příklad dokumentu JSON dvojitých zařízení:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

V kořenovém objektu jsou vlastnosti systému a kontejneru objekty pro `tags` a `reported` a `desired properties`. `properties` Kontejner obsahuje některé prvky jen pro čtení (`$metadata`, `$etag`, a `$version`), které jsou popsané v tomto pořadí v [dvojitých metadat] [ lnk-twin-metadata] a [optimistické souběžné] [ lnk-concurrency] oddíly.

### <a name="reported-property-example"></a>Příklad nahlášeného vlastnost

Ve výše uvedeném příkladu obsahuje dvojitých zařízení `batteryLevel` vlastnost, kterou aplikaci zařízení. Tato vlastnost umožňuje dotazu a pracovat na zařízení na základě posledního nahlášeného battery úrovně. Jiný příklad budou mít na zařízení aplikaci sestavy zařízení funkce nebo možnosti připojení.

Všimněte si, jak nahlášeného vlastnosti zjednodušit scénáře, kde je back-end zájem o poslední známé hodnoty vlastnosti. Pomocí [zařízení do cloudu zpráv] [ lnk-d2c] Pokud back-end potřebuje zpracuje telemetrie zařízení ve formě sekvence označen časovým razítkem události, jako je třeba časovou řadu.

### <a name="desired-configuration-example"></a>Příklad požadovaná konfigurace

Ve výše uvedeném příkladu `telemetryConfig` vlastnosti potřeby a nahlášeného používají zpět konce a zařízení aplikaci pro synchronizaci konfigurace telemetrie zařízení. Příklad:

1. Aplikace back-end Nastaví požadovanou vlastnost s hodnotou požadovaná konfigurace. Tady je část dokumentu s požadované vlastnosti:

        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
        
2. Aplikaci zařízení informován o změně okamžitě, když se připojený nebo na první znovu připojit. Aplikaci zařízení pak sestavy aktualizovaná konfigurace (nebo stav chyby pomocí `status` vlastnost). Tady je část nahlášeného vlastnosti:

        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...

3. Můžete ponechat aplikace back-end sledování výsledků operaci konfigurace na mnoha zařízeních pomocí [dotazu] [ lnk-query] twins.

> [AZURE.NOTE] Výše uvedené zlomky jsou příklady optimalizované pro snazší čitelnost z možných způsobů, jak kódovat konfigurace zařízení a její stav. Rozbočovač IoT neukládá určité schéma požadované a nahlášeného vlastností v twins zařízení.

V mnoha případech twins slouží k synchronizaci dlouho probíhajících operací, jako je aktualizaci firmwaru. V nápovědě k [použití požadované vlastnosti konfigurace zařízení] [ lnk-twin-properties] Další informace o používání vlastnosti synchronizace a sledovat dlouho spuštění operace na zařízeních.

## <a name="back-end-operations"></a>Operace back-end

Back-end pracuje v dvojitých pomocí následující atomová operace vystaven prostřednictvím protokolu HTTP:

1. **Načtení dvojitých číslem**. Tuto operaci vrátí uveden obsah dvojitých dokumentu, včetně značek a potřeby a vlastnosti systému.
2. **Částečně aktualizovat dvojitých**. Tuto operaci umožňuje back-end částečně aktualizovat dvojitých značky nebo požadované vlastnosti. Částečné aktualizace vyjádřený JSON dokument, který slouží k přidání nebo aktualizace libovolného vlastnosti uvedené. Nastavenými tak, aby `null` se odeberou. Například následující vytvoří nové požadované vlastnosti s hodnotou `{"newProperty": "newValue"}`, přepíše existující hodnotu `existingProperty` s `"otherNewValue"`a úplně odebere `otherOldProperty`. Žádné změny stane jiné existující požadované vlastnosti nebo značky:

        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }

3. **Nahrazení požadované vlastnosti**. Tuto operaci umožňuje back-end úplně přepsat všechny existující požadované vlastnosti a nahradit nový dokument JSON `properties/desired`.
4. **Nahrazení značky**. Analogicky nahradit požadované vlastnosti, tento operace umožňuje back-end úplně přepsat všechny existující značky a nahradit nový dokument JSON `tags`.

Všechny výše uvedené operace podporují [optimistické souběžné] [ lnk-concurrency] a vyžadovat oprávnění **ServiceConnect** podle [zabezpečení] [ lnk-security] článek.

Kromě tyto operace back-end dotazu twins pomocí SQL profesionálové [dotazu jazyka][lnk-query]a provádět operace velkých sad twins pomocí [úloh][lnk-jobs].

## <a name="device-operations"></a>Operace zařízení

Aplikaci zařízení pracuje v dvojitých pomocí následující atomová operace:

1. **Načtení dvojitých**. Tuto operaci vrátí obsah dokumentu dvojitých (včetně značek a potřeby, vykázaného a vlastnosti) pro aktuálně připojený zařízení.
2. **Částečně aktualizace vykázaného vlastnosti**. Tuto operaci umožňuje částečné aktualizace nahlášeného vlastnosti aktuálně připojený zařízení. To stejné aktualizace formátu JSON používá jako back-end protilehlé částečné aktualizace požadované vlastnosti.
3. **Dodržovat požadované vlastnosti**. Můžete se rozhodnout oznámení o aktualizacích požadované vlastnosti hned, jak probíhají aktuálně připojený zařízení. Zaznamenání formuláři stejné aktualizace (úplné nebo částečné náhradní) prováděný back-end.

Všechny výše uvedené postupy vyžadují oprávnění **DeviceConnect** podle [zabezpečení] [ lnk-security] článek.

[Azure IoT zařízení SDK] [ lnk-sdks] usnadňují použití výše uvedených operace z mnoha jazyky a platformách. Další informace o podrobnosti Základní centrum IoT požadované vlastnosti synchronizace najdete v [toku opětovné připojení zařízení][lnk-reconnection].

> [AZURE.NOTE] Nyní jsou dostupné pouze z zařízení připojujících se k rozbočovači IoT pomocí protokolu MQTT twins zařízení.

## <a name="reference-topics"></a>Odkazy na témata:

V těchto tématech poskytují další informace o řízení přístupu k centrální IoT.

## <a name="tags-and-properties-format"></a>Značky a vlastnosti Formát

Značky, potřeby a nahlášeného vlastnosti jsou objekty JSON s následujícími omezeními:

* Všechny klíče v JSON objekty jsou malá a velká písmena 128 znak sady UNICODE řetězce. Povolené znaky vyloučit řídicí znaky UNICODE (segmenty C0 a C1), a `'.'`, `' '`, a `'$'`.
* Všechny hodnoty v objektu JSON může být z následujících typů JSON: logická hodnota, číslo, řetězec, objekt. Pole nejsou povolené.

## <a name="twin-size"></a>Velikost dvojitých

Rozbočovač IoT vynucuje omezení velikosti 8KB na hodnotách `tags`, `properties/desired`, a `properties/reported`, s výjimkou prvky určené jen pro čtení.
Velikost počítá počítání všech znaků s výjimkou UNICODE určit znaky (segmenty C0 a C1) a místa `' '` kdy zobrazí mimo řetězcová konstanta.
Rozbočovač IoT odmítne s chybou všechny operace, které by zvětšení tyto dokumenty nad limit.

## <a name="twin-metadata"></a>Dvojité metadat

Rozbočovač IoT udržuje časové razítko poslední aktualizace pro jednotlivé objekty JSON v dialogovém okně Vlastnosti potřeby a nahlášeného. Časová razítka jsou UTC a ve formátu [(ISO8601)] `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Příklad:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Tyto informace bude k dispozici na všech úrovních (nejen listy strukturu JSON) zachovat aktualizace, které odebrat klíče objektu.

## <a name="optimistic-concurrency"></a>Optimistická souběžné

Značky, potřeby a nahlášeného vlastnosti všechny podporují optimistické souběžné.
Značky mít etag, podle [RFC7232], představovaná znázornění JSON na značku. Můžete to při operacích podmíněné aktualizace z back-end zajistit soulad.

Vlastnosti požadované a nahlášeného nemají etags, ale máte `$version` hodnotu, která je zaručena přírůstková. Analogicky do etag verze se dá použít aktualizace osobou, například (zařízení aplikace nahlášeného vlastnosti) nebo back-end požadované vlastnosti k jejímu vynucení konzistence aktualizace.

Jsou také užitečné, pokud observing agent (například zařízení aplikace dodržování požadované vlastnosti) obsahuje odsouhlasit RAS mezi výsledek operace načíst a oznámení o aktualizaci. V části [zařízení obnovení toku] [ lnk-reconnection] s dalšími informacemi.

## <a name="device-reconnection-flow"></a>Opětovné připojení toku zařízení

Rozbočovač IoT nezachová oznamování aktualizací požadované vlastnosti pro odpojeno zařízení. Sleduje, že zařízení, které se připojuje musíte získat úplné požadované vlastnosti dokumentu, kromě přihlášení k odběru pro upozornění na aktualizace. Dostat možnost RAS mezi oznamování aktualizací a celou načítání, musí být zajištěno následující:

1. Zařízení aplikaci se připojí k rozbočovači IoT.
2. Zařízení aplikaci přihlásí pro požadované vlastnosti upozornění na aktualizace.
3. Zařízení aplikaci načte celý dokument pro požadované vlastnosti.

Aplikaci zařízení můžete ignorovat všechny oznámení s `$version` menší nebo rovna než verzi úplné načtená dokumentu. Je to možné, protože IoT centrální zaručuje, že verze vždy zvýšit.

> [AZURE.NOTE] Tato logika už implementovaná v [Azure IoT zařízení SDK][lnk-sdks]. Tento popis je užitečné jenom v případě zařízení aplikaci nelze použít Azure IoT zařízení SDK a musí program rozhraní MQTT přímo.

## <a name="additional-reference-material"></a>Další materiály

Další témata odkaz v příručce pro vývojáře patří:

- [Koncové body IoT centrální] [ lnk-endpoints] popisuje různé koncové body, které každého IoT rozbočovače zpřístupňuje pro operace runtime a správy.
- [Omezení a kvót] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT rozbočovači a omezení chování má čekat při používání služby.
- [Rozbočovač IoT zařízení a přihlašovacích údajů SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vytváření zařízení a služby aplikace, které spolupracují s IoT centrální.
- [Dotazy jazyka pro twins, metody a úlohy] [ lnk-query] popisuje dotazovací jazyk slouží k načtení informací z centrální IoT o zařízení twins, metody a úlohy.
- [Podpora IoT centrální MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře IoT centrální protokolu MQTT.

## <a name="next-steps"></a>Další kroky

Teď jste se naučili o zařízení twins se mohly zajímat v následujících tématech vývojář příručka:

- [Přímé metodu na zařízení][lnk-methods]
- [Plánování úlohy na několika zařízeních][lnk-jobs]

Pokud chcete vyzkoušet některé pojmy popisované v tomto článku, může být zajímavé v těchto kurzech IoT centrální:

- [Jak používat dvojitých zařízení][lnk-twin-tutorial]
- [Použití dvojitých vlastností][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md

[(ISO8601)]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png