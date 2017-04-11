
Kód pro všechny funkce v aplikaci dané funkce jsou umístěná v kořenové složky, která obsahuje soubor konfigurace hostitele a jeden nebo více podsložky, z nichž každá obsahuje kód samostatná funkce, jako v následujícím příkladu

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Soubor *host.json* obsahuje několik konfigurace specifické pro spuštěnou aplikaci a je umístěn v kořenové složce aplikace (funkce). Informace o nastavení, které jsou k dispozici najdete v článku [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) na wikiwebu WebJobs.Script úložiště.

Každá funkce má složku obsahující jeden nebo víc souborů kód, function.json konfigurace a jiné závislosti.