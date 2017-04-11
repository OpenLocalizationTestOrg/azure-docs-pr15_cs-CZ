<properties
    pageTitle="Referenční informace pro vývojáře Azure funkce | Microsoft Azure"
    description="Principy funkce Azure koncepty a součásti, která jsou společná pro všechny jazyky a vazby."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcí, funkce, zpracování události, webhooks, dynamické výpočetním, bez serveru architektura"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Referenční informace pro vývojáře Azure funkcí

Azure funkce sdílení několik základních technické koncepty a součásti, bez ohledu na jazyk nebo vazbu, který používáte. Před přechodem do výukového informace specifické pro daný jazyk nebo vazbu, nezapomeňte si přečíst prostřednictvím přehled vztahující se ke všem poznámkám.

Tento článek předpokládá, že jste již přečetli [Přehled funkcí Azure](functions-overview.md) a znají [WebJobs SDK koncepty například aktivačních událostí, vazby a modulu runtime JobHost](../app-service-web/websites-dotnet-webjobs-sdk.md). Azure funkce vychází z WebJobs SDK. 


## <a name="function-code"></a>Kód (funkce)

*Funkce* je primární konceptu ve funkcích Azure. Napište kód pro funkce v jazyce podle svého výběru a ukládání souborů kód a konfiguračního souboru ve stejné složce. Konfigurace je ve formátu JSON a s názvem souboru `function.json`. Různé jazyky podporují a každý z nich má mírně odlišné setkat i v případě optimalizováno pro nejvhodnější pro daný jazyk. 

`function.json` Soubor obsahuje konfigurace specifické pro funkci, včetně vazby. Modul runtime přečte tento soubor k určení události aktivovat od, která data mají být při volání funkce a kam chcete odeslat data předaný podél z samotnou funkci. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Modul runtime můžete zabránit spuštění funkce nastavením `disabled` vlastnost `true`.

`bindings` Vlastnost je, kde můžete konfigurovat aktivačními událostmi a vazby. Každá vazba sdílí několik běžných nastavení a některá nastavení, které jsou specifické pro určitý typ vazby. Každá vazba vyžaduje následující nastavení:

|Vlastnost|Hodnoty a typy|Komentáře|
|---|-----|------|
|`type`|řetězec|Typ vazby. Například `queueTrigger`.
|`direction`|Kromě out| Označuje, zda vazby pro příjem data do funkce a odesílání dat z funkce.
| `name` | řetězec | Název, který se použije pro vazbu ve vázaných data ve funkci. U C# bude argument název. JavaScriptu bude klíč v seznamu klíč/hodnot.

## <a name="function-app"></a>Funkce aplikace

Funkce aplikace se skládá z jednotlivých funkcí zadány, které jsou spravovány společně službou Azure aplikace. Všechny funkce v aplikaci funkce Sdílení plánu stejného ceny, nepřetržitý nasazení a verze modulu runtime. Všechny funkce napsané v několika jazycích můžete sdílet aplikaci stejné (funkce). Funkce aplikace představit jako způsob, jak uspořádat a společně spravovat funkce. 

## <a name="runtime-script-host-and-web-host"></a>Modul runtime (script host a webového hostitele)

Runtime nebo script host je základní hostitel WebJobs SDK, která sleduje události shromažďuje a odešle data a nakonec spustí váš kód. 

Usnadnit aktivace HTTP se rovněž hostitele web, který slouží k sednout před hostiteli skript ve výrobním scénářích. Díky vyčlenění hostiteli skript z front-end přenosy spravuje hostiteli web.

## <a name="folder-structure"></a>Struktura složek

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Při vytváření projektu pro nasazení funkce z funkce aplikace v aplikaci služby Azure, můžete tento struktura složek považovat za kódu webu. Můžete použít existující nástroje jako průběžné integrace a nasazení nebo vlastního nasazení skripty ke konfiguraci nasazení Instalační balíček čas nebo kódu transpilation.

>[AZURE.NOTE] `wwwroot` Složka tady je místo, kam má získat soubory nasazené na. Však nesmí obsahovat této složky souborů nasazení, které by nakonec znamenat `wwwroot\wwwroot`. Místo toho vaše `host.json` složky souborů a funkce by měl být přímo na kořenové úrovni můžete nasadit.

## <a id="fileupdate"></a>Jak aktualizovat soubory aplikace (funkce)

Funkce editor integrované v portálu Azure umožňuje aktualizovat soubor *function.json* a soubor kódu pro funkci. Nahrávání nebo aktualizovat další soubory ATP *package.json* nebo *project.json* závislosti, musíte použít jiné metody nasazení.

Funkce aplikace jsou vytvořené v aplikaci služby tak všechny [Možnosti nasazení k dispozici standardní webové aplikace](../app-service-web/web-sites-deploy.md) jsou dostupné pro i aplikace (funkce). Tady jsou některé metody, pomocí kterých můžete nahrávání nebo aktualizace souborů aplikace (funkce). 

#### <a name="to-use-app-service-editor"></a>Použití editoru aplikace služby

1. Na portálu funkce Azure klikněte na **Nastavení aplikace (funkce)**.

2. V části **Upřesnit nastavení** klikněte na **Přejít na nastavení aplikace služeb**.

3. **Aplikace služby Editor** v navigačním podokně nabídky aplikace klikněte v části **Nástroje pro vývoj**.

4.  Klepněte na tlačítko **Přejít**.

    Po načtení aplikace služby Editor uvidíte *host.json* soubor funkce složky a v části *kořenových*. 

5. Otevření souborů, které chcete upravovat, nebo přetahovat z počítače vývoj nahrávání souborů.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Použití funkce aplikace koncový bod Správce služeb (Kudu)

1. Přejděte na: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klikněte na **ladění konzoly > CMD**.

3. Přejděte na `D:\home\site\wwwroot\` aktualizovat *host.json* nebo `D:\home\site\wwwroot\<function_name>` aktualizace souborů funkce.

4. Přetáhněte myší na soubor, který chcete nahrát do příslušné složky v mřížce soubor. Existují dvě oblasti v mřížce souborů, které můžete přetáhnout do souboru. *ZIP* soubory se zobrazí v poli s popiskem "Tažením tady nahrávání a rozbalit." Pro jiné typy souborů, a vložte v mřížce soubor, ale mimo pole "unzip".

#### <a name="to-use-ftp"></a>Použití FTP

1. Postupujte podle pokynů [sem](../app-service-web/web-sites-deploy.md#ftp) zobrazíte FTP nakonfigurované.

2. Pokud jste připojení k webu funkce aplikací, zkopírujte soubor aktualizované *host.json* k `/site/wwwroot` nebo kopírování funkce souborů do `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Použití nepřetržitý nasazení

Postupujte podle pokynů v tématu [Nepřetržitý nasazení Azure funkcí](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Paralelní spouštění

Když více spouštěcí událostem rychleji než jedním podprocesem funkce runtime zpracovat, modul runtime vyvolání funkce tisknutím paralelně.  Používáte-li funkce aplikace je [Dynamické plán služeb](functions-scale.md#dynamic-service-plan), funkce aplikace může rozšiřování automaticky.  Každý výskyt aplikaci funkce zda aplikace běží na dynamické plán služeb nebo běžná [Plán služeb aplikací](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), může zpracovat souběžné funkce vyvolání paralelně použití více vláken.  Maximální počet souběžné funkce vyvolání pokaždé aplikace funkce se liší v závislosti na velikosti paměti aplikaci (funkce). 

## <a name="azure-functions-pulse"></a>Azure funkce Pulse  

Pulse je živou události toku, který ukazuje, jak často vaši funkci získáte i úspěšné a neúspěšné. Můžete také sledovat průměr čas spuštění. Jsme budete přidávat další funkce a přizpůsobení k němu určitou dobu. Přístup ke stránce **Pulse** z karty **Sledování** .

## <a name="repositories"></a>Úložiště

Kód Azure funkce je otevřít zdroj a uložené v GitHub úložištích:

* [Azure za běhu funkce](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure funkce portálu](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure funkce šablony](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Rozšíření Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Vazby

Tady je tabulka všechny podporované vazby.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Hlášení problémů

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Další kroky

Další informace najdete v následujících zdrojích:

* [Azure funkce C# referenční informace pro vývojáře](functions-reference-csharp.md)
* [Azure funkce F # referenční informace pro vývojáře](functions-reference-fsharp.md)
* [Referenční informace pro vývojáře Azure NodeJS funkce](functions-reference-node.md)
* [Azure aktivace funkcí a vazby](functions-triggers-bindings.md)
* [Azure funkcí: cesty](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) na blogu týmu pro aplikaci služby Azure. Jak funkce Azure vyvinutý historie.





