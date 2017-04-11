<properties
    pageTitle="Azure aktivační událost funkce timer | Microsoft Azure"
    description="Pochopte, jak pomocí aktivačních událostí časovače ve funkcích Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funguje, funkce a zpracování události, dynamické výpočetním bez serveru architektura"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure aktivační událost funkce timer

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje postup při konfiguraci časovače aktivačních událostí v Azure funkcí. Timer spustí volání funkce podle plánu, jednorázové nebo opakování.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>Function.JSON časovače aktivační události

Soubor *function.json* obsahuje výrazu plánu. Například následující plán probíhá funkci každou minutu:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Aktivační událost časovače zpracovává více instancí škálování automaticky: jenom jedna instance funkce konkrétní timer bude spuštěn ve všech instancích.

## <a name="format-of-schedule-expression"></a>Formát výrazu expression plánu

Výraz plánu je [výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) , které obsahují 6 polí: `{second} {minute} {hour} {day} {month} {day of the week}`. 

Všimněte si spoustu cron výrazy, které můžete najít online vynechat pole {druhý}, pokud zkopírujete z jednoho z těchto budete muset nastavit další pole. 

Tady jsou některé další plán příklady výrazů:

Chcete-li aktivovat každých 5 minut:

```json
"schedule": "0 */5 * * * *"
```

V horní části každou hodinu, aktivovat jednou:

```json
"schedule": "0 0 * * * *",
```

Aktivovat jednou každých dvou hodin:

```json
"schedule": "0 0 */2 * * *",
```

Chcete-li aktivovat každou hodinu z 9: 00 do 17: 00:

```json
"schedule": "0 0 9-17 * * *",
```

Aktivovat v 9:30:00 každý den:

```json
"schedule": "0 30 9 * * *",
```

Chcete-li aktivovat v 9:30:00 každý den v týdnu:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Příklad kódu aktivační událost C# časovač

Tento příklad kódu C#: zapíše jeden protokol pokaždé, když aktivaci funkce.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
