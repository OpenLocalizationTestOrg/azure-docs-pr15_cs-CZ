<properties
 pageTitle="Příručka pro vývojáře – dotazovací jazyk | Microsoft Azure"
 description="Azure IoT centrální vývojář Průvodce - Popis dotazovací jazyk použitý k načtení informací o twins, metody a úlohy z rozbočovače IoT"
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

# <a name="reference---query-language-for-twins-and-jobs"></a>Odkaz - je dotazovací jazyk pro úlohy a twins

## <a name="overview"></a>Základní informace

Rozbočovač IoT poskytuje výkonné jazyka SQL profesionálové načítat informace týkající se [zařízení twins] [ lnk-twins] [úlohy]a[lnk-jobs]. Tento článek uvádí:

* Úvod k hlavní funkce centrální IoT dotazovací jazyk a
* Podrobný popis požadovaný jazyk.

## <a name="getting-started-with-twin-queries"></a>Začínáme s dvojitých dotazů

[Zařízení twins] [ lnk-twins] mohou obsahovat objekty, libovolného JSON vlastnosti a značky. Rozbočovač IoT umožňuje twins zařízení dotazu jako jeden dokument JSON obsahující všechny informace o dvojitých.
Předpokládejme například, že vaše twins centrální IoT mít následující strukturu:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

Rozbočovač IoT zpřístupňuje twins zařízení jako kolekce dokument s názvem **zařízení**.
Následující dotaz načte tak celou sadu twins zařízení:

        SELECT * FROM devices

> [AZURE.NOTE] [IoT centrální SDK] [ lnk-hub-sdks] podporují stránkování velké výsledků.

Rozbočovač IoT umožňuje načítat twins filtrování pomocí libovolného podmínky. Například

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

načte twins slovem **location.region** nastavit **námi**.
Logické operátory a aritmetické porovnání podporovaných i, například

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

Obnoví všechna twins umístěný ve Spojených státech nakonfigurované tak, aby odeslat telemetrie méně často než každou minutu. Pro potřeby je také možné používání maticových konstant ve spojení s **v** a operátory **NV** (ne v). Například

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

Obnoví všechna twins, které vykázaného Wi-Fi nebo kabelové připojení. Podívejte se do [klauzule WHERE] [ lnk-query-where] oddíl pro celou odkaz Možnosti filtrování.

Seskupení a agregace jsou podporovány. Například

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Vrátí počet hodnot v zařízení v každém telemetrie konfigurace stavu.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

Výše uvedený příklad ukazuje situaci, kdy tři zařízení vykázaného úspěšné konfigurace, dvě pořád použití konfigurace a jednu k chybě.

### <a name="c-example"></a>Příklad C#

Funkci dotazu se zobrazí [C# služby SDK] [ lnk-hub-sdks] v **RegistryManager** předmětu.
Tady je příklad jednoduchého dotazu:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Všimněte si, jak je vytvořena objekt **dotazu** s velikostí stránky (až 1000) a pak můžete tak, že zavoláte metody **GetNextAsTwinAsync** tisknutím načíst více stránek.
Je důležité mít na paměti, že objekt dotazu zpřístupňuje více **Další\***, v závislosti na možnost deserializace vyžadované dotazu, tedy dvojitých nebo úlohy objektů nebo prostý Json se nemusí používat při použití prognózy.

### <a name="node-example"></a>Příklad uzel

Funkci dotazu se zobrazí [uzel služby SDK] [ lnk-hub-sdks] v objekt **registru** .
Tady je příklad jednoduchého dotazu:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Všimněte si, jak je vytvořena objekt **dotazu** s velikostí stránky (až 1000) a pak můžete tak, že zavoláte metody **nextAsTwin** tisknutím načíst více stránek.
Je důležité mít na paměti, že objekt dotazu zpřístupňuje více **Další\***, v závislosti na možnost deserializace vyžadované dotazu, tedy dvojitých nebo úlohy objektů nebo prostý Json se nemusí používat při použití prognózy.

### <a name="limitations"></a>Omezení

V současné době průzkumy jsou podporované jenom při použití agregace, tedy neagregované dotazů můžete použít jenom `SELECT *`. Navíc agregace podporují pouze ve spojení s seskupení.

## <a name="getting-started-with-jobs-queries"></a>Začínáme s úloh dotazy

[Úlohy] [ lnk-jobs] lze provádět operace s sady zařízení. Každý dvojitých zařízení s informacemi, úloh, která je součástí v kolekci s názvem **práce**.
Logicky,

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Tato kolekce je v současné době queriable jako **devices.jobs** v centrální IoT dotazovací jazyk.

Například, aby všechny úlohy (minulých a plánované), které ovlivňují jednoho zařízení, můžete použít následující dotaz:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Všimněte si, jak tento dotaz obsahuje stav specifické pro zařízení (a případně odpovědi přímá metoda) jednotlivé úlohy vrácena.
Je také možné filtrovat pomocí libovolného Boolean podmínek na všechny vlastnosti objektů v kolekci **devices.jobs** .
Například následující dotaz:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

Vyhledá všechny dokončené dvojitých úlohy aktualizace pro zařízení **myDeviceId** vytvořených po září 2016.

Je také možné k načítání výsledků na zařízení jednu úlohu.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Omezení
V současné době dotazy na **devices.jobs** nepodporují:

* Průzkumy, tedy pouze `SELECT *` je možné;
* Podmínky, které odkazují na zařízení dvojitých kromě vlastností úlohy uvedené výše;
* Provádění agregace, například počet, průměr, Seskupit podle.

## <a name="basics-of-an-iot-hub-query"></a>Základní informace o IoT centrální dotazu

Každý IoT centrální dotaz se skládá SELECT a od věty a volitelné WHERE a seskupit podle klauzulí. Každý dotaz se spustí na sbírky sledovaných dokumentů JSON, například twins zařízení. Klauzule FROM označuje kolekci dokumentů na vstupní na (**zařízení** nebo **devices.jobs**). Potom se použije filtr v klauzuli WHERE. V případě agregace, seskupování výsledků hledání v tomto kroku jako zadané v klauzuli GROUP BY a pro každou skupinu, řádek generováno jak je uvedeno v klauzuli SELECT.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>Klauzule FROM

Klauzule **FROM < from_specification >** může mít jenom dvě hodnoty: **od zařízení**twins zařízení dotazu, a **od devices.jobs**dotazu úlohy na zařízení podrobnosti.

## <a name="where-clause"></a>Klauzule WHERE

**Kde < filter_condition >** klauzule je nepovinný krok. Určuje podmínky, které JSON dokumentům v kolekci od musí splňovat, aby mohla být součástí výsledek. Všechny dokumenty JSON musí být zadané podmínky "true" zahrnuty ve výsledku.

Povolené podmínky jsou popsány v části [výrazů a podmínky][lnk-query-expressions].

## <a name="select-clause"></a>Klauzule SELECT

Klauzule SELECT (**Vybrat < select_list >**) je povinný a určuje, jaké budou hodnoty načtené z dotazu. Určuje JSON hodnoty, které mají být použita k vytvoření nových objektů JSON pro každý prvek filtrované (a volitelně seskupené) podmnožinu kolekci od fáze projekce vytvoří nový objekt JSON, vytvořena s hodnoty uvedené v klauzuli SELECT.

Toto je gramatiky klauzule SELECT:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

kde **attribute_name** odkazuje na libovolnou vlastnost JSON dokumentu v kolekci od. Pár příkladů klauzule FROM najdete v [Začínáme s dotazy dvojitých] [ lnk-query-getstarted] oddíl.

V současné době výběru klauzule liší od **Vyberte \* ** pouze podporuje agregační dotazy na twins.

## <a name="group-by-clause"></a>Klauzule GROUP BY

Klauzule **GROUP BY < group_specification >** je volitelný krok, který je lze provést po zadaný v klauzuli WHERE a před projekce uvedené v seznamu vyberte filtr. Seskupení dokumenty založené na hodnotě atributu. Tyto skupiny slouží ke generování agregované hodnoty uvedené v klauzuli SELECT.

Příklad dotazu pomocí GROUP BY je:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Formální syntaxe GROUP BY je:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

kde **attribute_name** odkazuje na libovolnou vlastnost JSON dokumentu v kolekci od. 

Klauzule GROUP BY v současné době je podporována pouze v případě dotazování twins.

## <a name="expressions-and-conditions"></a>Výrazy a podmínky

Na vysoké úrovni *výraz*:

* Vyhodnotí přidala instance tohoto typu JSON (tedy logická hodnota, číslo, řetězec, pole nebo objektu), a
* Je definován manipulaci s dat pocházejících z dokumentu JSON zařízení a konstant pomocí předdefinovaných operátory a funkcí.

*Podmínky* jsou výrazů, jejichž vyhodnocen jako logická hodnota, tedy všechny konstanta liší logickou **hodnotu true** je považován za **false** (včetně **null**, **Nedefinováno**, objektu nebo pole instance, jakýkoli řetězec a jasně logickou **hodnotu NEPRAVDA**).

Syntaxe pro výrazy je:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

kde je:

| Symbol | Definice |
| -------------- | -----------------|
| attribute_name | Všechny vlastnost JSON dokumentu v kolekci od. |
| unary_operator | Všechny unární operátor podle operátory bodu. |
| binary_operator | Binární operátor podle operátory bodu. |
| decimal_literal | Uvolnit vyjádřený v desítkové soustavě. |
| hexadecimal_literal | Číslo vyjádřené řetězcem "0 x" a pak řetězec šestnáctkové číslic. |
| string_literal | Řetězcových jsou řetězce Unicode představované posloupnost nula nebo více znaků kódu Unicode nebo řídicí sekvence. Řetězcových jsou uzavřeny do jednoduchých uvozovek (apostrof: ") nebo dvojité uvozovky (uvozovka:"). Povolené únik: `\'`, `\"`, `\\`, `\uXXXX` znaků Unicode definované šestnáctkové zaokrouhlené na 4 číslice. |

### <a name="operators"></a>Operátory

Jsou podporovány následující operátory:

| Řady | Operátory |
| -------------- | -----------------|
| Aritmetický | +,-,*,/,% |
| Logické | A, NEBO NE |
| Porovnání |  =,! =, <>;, <, > = <> |

## <a name="next-steps"></a>Další kroky

Zjistěte, jak provádět dotazy v aplikací pomocí [IoT centrální SDK][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md