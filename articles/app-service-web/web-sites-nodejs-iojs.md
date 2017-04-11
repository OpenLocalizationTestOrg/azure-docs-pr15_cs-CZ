<properties 
    pageTitle="Jak používat io.js s Azure aplikace služby Web Apps" 
    description="Zjistěte, jak pomocí web appu v aplikaci služby Azure io.js." 
    services="app-service\web" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016"
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Jak používat io.js s Azure aplikace služby Web Apps

Oblíbené vidlice uzel [io.js] funkcemi různých rozdílů, na které je Joyent Node.js projektu, včetně více otevřít modelu zásad správného řízení, rychlejší verze obrázku a rychlejší přijetí nových a pokusné JavaScript funkcí.

Během [Služba Azure aplikací](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps má mnoho verzí Node.js předinstalovaný, umožňuje rovněž binární Node.js uživatelem. Tento článek popisuje dva způsoby povolení použití io.js na aplikaci služby webové aplikace: použití rozšířených nasazení skript, který automaticky nakonfiguruje Azure použít nejnovější verzi dostupné io.js, jakož i ruční odeslání io.js binární. 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>Použití skript pro nasazení

Při nasazení aplikace Node.js spustí aplikace služby Web Apps počet malé příkazy zajistit, že prostředí správně nakonfigurované. Použití skript pro nasazení, tento proces můžete upravit, aby obsahoval stahování a konfigurace io.js.

[Io.js skript pro nasazení](https://github.com/felixrieseberg/iojs-azure) je dostupný na GitHub. Povolit io.js na svoji webovou aplikaci, jednoduše zkopírovat **.deployment**, **deploy.cmd** a **IISNode.yml** do kořenové složky aplikace a nasadit na Web Apps.  

První soubor **.deployment**pokyn Web Apps spuštění **deploy.cmd** na nasazení. Tento skript spustí všechny obvykle kroky pro aplikaci Node.js, ale taky stáhnou nejnovější verzi io.js. Nakonec **IISNode.yml** nakonfiguruje Web Apps můžete jenom stažené io.js binární místo předinstalovaná binární Node.js.

> [AZURE.NOTE] Aktualizovat binární použité io.js, stačí nasadit aplikaci - stáhne skript novou verzi io.js pokaždé jeden nasazení aplikace.

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>Použití ruční instalace

Ruční instalace verze vlastní io.js obsahuje pouze dva kroky. Nejdřív si stáhněte **win x64** binární přímo z [io.js rozdělení]. Povinné jsou dva soubory - **iojs.exe** a **iojs.lib**. Oba soubory uložte do složky uvnitř webovou aplikaci, třeba do **koše/iojs**.

Konfigurace webové aplikace místo předinstalovaná verze uzel použít **iojs.exe** , vytváření souboru **IISNode.yml** v kořenovém adresáři aplikace a přidejte následující řádek.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>Další kroky

V tomto článku se naučíte používat k dispozici io.js pomocí aplikace služby Web Apps, použitím skripty nasazení i ruční instalace. 

> [AZURE.NOTE] IO.js se nestahují vyvíjí a aktualizovat častěji Node.js. Počet Node.js moduly nemusí fungovat s io.js - prosím konzultacím [io.js na GitHub] návody na řešení.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

[IO.js]: https://iojs.org
[IO.js rozdělení]: https://iojs.org/dist/
[IO.js na GitHub]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 